---
uid: signalr/overview/performance/scaleout-with-windows-azure-service-bus
title: Ampliación de SignalR con el Bus de servicio de Azure | Documentos de Microsoft
author: MikeWasson
description: Versiones de software usan en esta versión de Visual Studio 2013 .NET 4.5 SignalR tema 2 versiones anteriores de esta versión de 1.x para SignalR el tema de este tema...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: ce1305f9-30fd-49e3-bf38-d0a78dfb06c3
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: e6d9e4e6ba2040aa2c6e453aacf0ddca38c4a1a9
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/03/2018
---
<a name="signalr-scaleout-with-azure-service-bus"></a><span data-ttu-id="08d16-103">Ampliación de SignalR con el Bus de servicio de Azure</span><span class="sxs-lookup"><span data-stu-id="08d16-103">SignalR Scaleout with Azure Service Bus</span></span>
====================
<span data-ttu-id="08d16-104">por [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="08d16-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

<span data-ttu-id="08d16-105">En este tutorial, implementará una aplicación de SignalR para un rol Web de Windows Azure mediante el backplane de Bus de servicio para distribuir mensajes a cada instancia de rol.</span><span class="sxs-lookup"><span data-stu-id="08d16-105">In this tutorial, you will deploy a SignalR application to a Windows Azure Web Role, using the Service Bus backplane to distribute messages to each role instance.</span></span> <span data-ttu-id="08d16-106">(También puede usar el backplane de Bus de servicio con [aplicaciones de servicio de aplicaciones de Azure web](https://docs.microsoft.com/azure/app-service-web/).)</span><span class="sxs-lookup"><span data-stu-id="08d16-106">(You can also use the Service Bus backplane with [web apps in Azure App Service](https://docs.microsoft.com/azure/app-service-web/).)</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

<span data-ttu-id="08d16-107">Requisitos previos:</span><span class="sxs-lookup"><span data-stu-id="08d16-107">Prerequisites:</span></span>

- <span data-ttu-id="08d16-108">Una cuenta de Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="08d16-108">A Windows Azure account.</span></span>
- <span data-ttu-id="08d16-109">El [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="08d16-109">The [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span></span>
- <span data-ttu-id="08d16-110">Visual Studio 2012 o 2013.</span><span class="sxs-lookup"><span data-stu-id="08d16-110">Visual Studio 2012 or 2013.</span></span>

<span data-ttu-id="08d16-111">El backplane de bus de servicio también es compatible con [Service Bus para Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), versión 1.1.</span><span class="sxs-lookup"><span data-stu-id="08d16-111">The service bus backplane is also compatible with [Service Bus for Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), version 1.1.</span></span> <span data-ttu-id="08d16-112">Sin embargo, no es compatible con la versión 1.0 de Service Bus para Windows Server.</span><span class="sxs-lookup"><span data-stu-id="08d16-112">However, it is not compatible with version 1.0 of Service Bus for Windows Server.</span></span>

## <a name="pricing"></a><span data-ttu-id="08d16-113">Precio</span><span class="sxs-lookup"><span data-stu-id="08d16-113">Pricing</span></span>

<span data-ttu-id="08d16-114">El backplane de Bus de servicio utiliza temas para enviar mensajes.</span><span class="sxs-lookup"><span data-stu-id="08d16-114">The Service Bus backplane uses topics to send messages.</span></span> <span data-ttu-id="08d16-115">Para la información más reciente sobre los precios, consulte [Bus de servicio](https://azure.microsoft.com/pricing/details/service-bus/).</span><span class="sxs-lookup"><span data-stu-id="08d16-115">For the latest pricing information, see [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).</span></span> <span data-ttu-id="08d16-116">En el momento de redactar este artículo, puede enviar 1.000.000 mensajes al mes de menos de 1 $.</span><span class="sxs-lookup"><span data-stu-id="08d16-116">At the time of this writing, you can send 1,000,000 messages per month for less than $1.</span></span> <span data-ttu-id="08d16-117">El backplane envía un mensaje de bus de servicio para cada invocación de un método de concentrador de SignalR.</span><span class="sxs-lookup"><span data-stu-id="08d16-117">The backplane sends a service bus message for each invocation of a SignalR hub method.</span></span> <span data-ttu-id="08d16-118">También hay algunos mensajes de control para las conexiones, cuando se producen desconexiones, dejando o unirse a grupos y así sucesivamente.</span><span class="sxs-lookup"><span data-stu-id="08d16-118">There are also some control messages for connections, disconnections, joining or leaving groups, and so forth.</span></span> <span data-ttu-id="08d16-119">En la mayoría de las aplicaciones, la mayoría del tráfico de mensajes serán las invocaciones de método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="08d16-119">In most applications, the majority of the message traffic will be hub method invocations.</span></span>

## <a name="overview"></a><span data-ttu-id="08d16-120">Información general</span><span class="sxs-lookup"><span data-stu-id="08d16-120">Overview</span></span>

<span data-ttu-id="08d16-121">Antes de entrar en el tutorial detallado, presentamos una introducción rápida de lo va a hacer.</span><span class="sxs-lookup"><span data-stu-id="08d16-121">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="08d16-122">Usar el portal de Windows Azure para crear un nuevo espacio de nombres de Bus de servicio.</span><span class="sxs-lookup"><span data-stu-id="08d16-122">Use the Windows Azure portal to create a new Service Bus namespace.</span></span>
2. <span data-ttu-id="08d16-123">Agregue estos paquetes de NuGet para la aplicación:</span><span class="sxs-lookup"><span data-stu-id="08d16-123">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="08d16-124">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="08d16-124">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - <span data-ttu-id="08d16-125">[Microsoft.AspNet.SignalR.ServiceBus3](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus3) o [Microsoft.AspNet.SignalR.ServiceBus](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus)</span><span class="sxs-lookup"><span data-stu-id="08d16-125">[Microsoft.AspNet.SignalR.ServiceBus3](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus3) or [Microsoft.AspNet.SignalR.ServiceBus](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus)</span></span>
3. <span data-ttu-id="08d16-126">Cree una aplicación de SignalR.</span><span class="sxs-lookup"><span data-stu-id="08d16-126">Create a SignalR application.</span></span>
4. <span data-ttu-id="08d16-127">Agregue el código siguiente a Startup.cs para configurar el plano posterior:</span><span class="sxs-lookup"><span data-stu-id="08d16-127">Add the following code to Startup.cs to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

<span data-ttu-id="08d16-128">Este código configura el plano posterior con los valores predeterminados de [TopicCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.servicebusscaleoutconfiguration.topiccount(v=vs.118).aspx) y [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span><span class="sxs-lookup"><span data-stu-id="08d16-128">This code configures the backplane with the default values for [TopicCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.servicebusscaleoutconfiguration.topiccount(v=vs.118).aspx) and [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span></span> <span data-ttu-id="08d16-129">Para obtener información acerca de cómo cambiar estos valores, consulte [SignalR rendimiento: ampliación métricas](signalr-performance.md#scaleout_metrics).</span><span class="sxs-lookup"><span data-stu-id="08d16-129">For information on changing these values, see [SignalR Performance: Scaleout Metrics](signalr-performance.md#scaleout_metrics).</span></span>

<span data-ttu-id="08d16-130">Para cada aplicación, elegir un valor diferente para "YourAppName".</span><span class="sxs-lookup"><span data-stu-id="08d16-130">For each application, pick a different value for "YourAppName".</span></span> <span data-ttu-id="08d16-131">No utilice el mismo valor en varias aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="08d16-131">Do not use the same value across multiple applications.</span></span>

## <a name="create-the-azure-services"></a><span data-ttu-id="08d16-132">Crear los servicios de Azure</span><span class="sxs-lookup"><span data-stu-id="08d16-132">Create the Azure Services</span></span>

<span data-ttu-id="08d16-133">Crear un servicio en la nube, como se describe en [cómo crear e implementar un servicio de nube](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span><span class="sxs-lookup"><span data-stu-id="08d16-133">Create a Cloud Service, as described in [How to Create and Deploy a Cloud Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span></span> <span data-ttu-id="08d16-134">Siga los pasos descritos en la sección "Cómo: crear un servicio de nube mediante Creación rápida".</span><span class="sxs-lookup"><span data-stu-id="08d16-134">Follow the steps in the section "How to: Create a cloud service using Quick Create".</span></span> <span data-ttu-id="08d16-135">Para este tutorial, no es necesario cargar un certificado.</span><span class="sxs-lookup"><span data-stu-id="08d16-135">For this tutorial, you do not need to upload a certificate.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image2.png)

<span data-ttu-id="08d16-136">Crear un nuevo espacio de nombres de Bus de servicio, como se describe en [cómo para el Bus de servicio de uso temas/suscripciones](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span><span class="sxs-lookup"><span data-stu-id="08d16-136">Create a new Service Bus namespace, as described in [How to Use Service Bus Topics/Subscriptions](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span></span> <span data-ttu-id="08d16-137">Siga los pasos descritos en la sección "Crear un Namespace de servicio".</span><span class="sxs-lookup"><span data-stu-id="08d16-137">Follow the steps in the section "Create a Service Namespace".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="08d16-138">Asegúrese de seleccionar la misma región para el servicio de nube y el espacio de nombres de Bus de servicio.</span><span class="sxs-lookup"><span data-stu-id="08d16-138">Make sure to select the same region for the cloud service and the Service Bus namespace.</span></span>


## <a name="create-the-visual-studio-project"></a><span data-ttu-id="08d16-139">Crear el proyecto de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="08d16-139">Create the Visual Studio Project</span></span>

<span data-ttu-id="08d16-140">Inicie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="08d16-140">Start Visual Studio.</span></span> <span data-ttu-id="08d16-141">Desde el **archivo** menú, haga clic en **nuevo proyecto**.</span><span class="sxs-lookup"><span data-stu-id="08d16-141">From the **File** menu, click **New Project**.</span></span>

<span data-ttu-id="08d16-142">En el **nuevo proyecto** cuadro de diálogo, expanda **Visual C#**.</span><span class="sxs-lookup"><span data-stu-id="08d16-142">In the **New Project** dialog box, expand **Visual C#**.</span></span> <span data-ttu-id="08d16-143">En **plantillas instaladas**, seleccione **nube** y, a continuación, seleccione **servicio de nube de Windows Azure**.</span><span class="sxs-lookup"><span data-stu-id="08d16-143">Under **Installed Templates**, select **Cloud** and then select **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="08d16-144">Mantenga el valor predeterminado de .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="08d16-144">Keep the default .NET Framework 4.5.</span></span> <span data-ttu-id="08d16-145">Nombre de la aplicación ChatChannel y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="08d16-145">Name the application ChatService and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image4.png)

<span data-ttu-id="08d16-146">En el **nuevo servicio de nube de Windows Azure** cuadro de diálogo, seleccione rol Web de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="08d16-146">In the **New Windows Azure Cloud Service** dialog, select ASP.NET Web Role.</span></span> <span data-ttu-id="08d16-147">Haga clic en el botón de flecha derecha (**&gt;**) para agregar el rol a la solución.</span><span class="sxs-lookup"><span data-stu-id="08d16-147">Click the right-arrow button (**&gt;**) to add the role to your solution.</span></span>

<span data-ttu-id="08d16-148">Mantenga el mouse sobre el nuevo rol, por lo que el icono de lápiz visible.</span><span class="sxs-lookup"><span data-stu-id="08d16-148">Hover the mouse over the new role, so the pencil icon visible.</span></span> <span data-ttu-id="08d16-149">Haga clic en este icono para cambiar el nombre de la función.</span><span class="sxs-lookup"><span data-stu-id="08d16-149">Click this icon to rename the role.</span></span> <span data-ttu-id="08d16-150">Nombre de la función "SignalRChat" y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="08d16-150">Name the role "SignalRChat" and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

<span data-ttu-id="08d16-151">En el **nuevo proyecto ASP.NET** cuadro de diálogo, seleccione **MVC**y haga clic en Aceptar.</span><span class="sxs-lookup"><span data-stu-id="08d16-151">In the **New ASP.NET Project** dialog, select **MVC**, and click OK.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

<span data-ttu-id="08d16-152">El Asistente para proyecto crea dos proyectos:</span><span class="sxs-lookup"><span data-stu-id="08d16-152">The project wizard creates two projects:</span></span>

- <span data-ttu-id="08d16-153">ChatChannel: Este proyecto es la aplicación de Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="08d16-153">ChatService: This project is the Windows Azure application.</span></span> <span data-ttu-id="08d16-154">Define las funciones de Azure y otras opciones de configuración.</span><span class="sxs-lookup"><span data-stu-id="08d16-154">It defines the Azure roles and other configuration options.</span></span>
- <span data-ttu-id="08d16-155">SignalRChat: Este proyecto es el proyecto de MVC de ASP.NET 5.</span><span class="sxs-lookup"><span data-stu-id="08d16-155">SignalRChat: This project is your ASP.NET MVC 5 project.</span></span>

## <a name="create-the-signalr-chat-application"></a><span data-ttu-id="08d16-156">Crear la aplicación de Chat de SignalR</span><span class="sxs-lookup"><span data-stu-id="08d16-156">Create the SignalR Chat Application</span></span>

<span data-ttu-id="08d16-157">Para crear la aplicación de chat, siga los pasos del tutorial [Getting Started with SignalR y MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span><span class="sxs-lookup"><span data-stu-id="08d16-157">To create the chat application, follow the steps in the tutorial [Getting Started with SignalR and MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span></span>

<span data-ttu-id="08d16-158">Use NuGet para instalar las bibliotecas necesarias.</span><span class="sxs-lookup"><span data-stu-id="08d16-158">Use NuGet to install the required libraries.</span></span> <span data-ttu-id="08d16-159">Desde el **herramientas** menú, seleccione **Administrador de paquetes de biblioteca**, a continuación, seleccione **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="08d16-159">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="08d16-160">En el **Package Manager Console** ventana, escriba los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="08d16-160">In the **Package Manager Console** window, enter the following commands:</span></span>

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

<span data-ttu-id="08d16-161">Use la `-ProjectName` opción para instalar los paquetes en el proyecto de ASP.NET MVC, en lugar de en el proyecto de Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="08d16-161">Use the `-ProjectName` option to install the packages to the ASP.NET MVC project, rather than the Windows Azure project.</span></span>

## <a name="configure-the-backplane"></a><span data-ttu-id="08d16-162">Configurar el Backplane</span><span class="sxs-lookup"><span data-stu-id="08d16-162">Configure the Backplane</span></span>

<span data-ttu-id="08d16-163">En el archivo de Startup.cs de la aplicación, agregue el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="08d16-163">In your application's Startup.cs file, add the following code:</span></span>

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

<span data-ttu-id="08d16-164">Ahora debe obtener la cadena de conexión de bus de servicio.</span><span class="sxs-lookup"><span data-stu-id="08d16-164">Now you need to get your service bus connection string.</span></span> <span data-ttu-id="08d16-165">En el portal de Azure, seleccione el espacio de nombres de bus de servicio que creó y haga clic en el icono de la clave de acceso.</span><span class="sxs-lookup"><span data-stu-id="08d16-165">In the Azure portal, select the service bus namespace that you created and click the Access Key icon.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

<span data-ttu-id="08d16-166">Copiar la cadena de conexión en el Portapapeles, a continuación, péguelo en el *connectionString* variable.</span><span class="sxs-lookup"><span data-stu-id="08d16-166">Copy the connection string to the clipboard, then paste it into the *connectionString* variable.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a><span data-ttu-id="08d16-167">Implementar en Azure</span><span class="sxs-lookup"><span data-stu-id="08d16-167">Deploy to Azure</span></span>

<span data-ttu-id="08d16-168">En el Explorador de soluciones, expanda la **Roles** carpeta dentro del proyecto ChatChannel.</span><span class="sxs-lookup"><span data-stu-id="08d16-168">In Solution Explorer, expand the **Roles** folder inside the ChatService project.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

<span data-ttu-id="08d16-169">Haga clic en el rol de SignalRChat y seleccione **propiedades**.</span><span class="sxs-lookup"><span data-stu-id="08d16-169">Right-click the SignalRChat role and select **Properties**.</span></span> <span data-ttu-id="08d16-170">Seleccione el **configuración** ficha. En **instancias** seleccione 2.</span><span class="sxs-lookup"><span data-stu-id="08d16-170">Select the **Configuration** tab. Under **Instances** select 2.</span></span> <span data-ttu-id="08d16-171">También puede establecer el tamaño de VM en **Extra pequeño**.</span><span class="sxs-lookup"><span data-stu-id="08d16-171">You can also set the VM size to **Extra Small**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

<span data-ttu-id="08d16-172">Guarde los cambios.</span><span class="sxs-lookup"><span data-stu-id="08d16-172">Save the changes.</span></span>

<span data-ttu-id="08d16-173">En el Explorador de soluciones, haga clic en el proyecto ChatChannel.</span><span class="sxs-lookup"><span data-stu-id="08d16-173">In Solution Explorer, right-click the ChatService project.</span></span> <span data-ttu-id="08d16-174">Seleccione **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="08d16-174">Select **Publish**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

<span data-ttu-id="08d16-175">Si se trata de la primera publicación de tiempo en Windows Azure, debe descargar sus credenciales.</span><span class="sxs-lookup"><span data-stu-id="08d16-175">If this is your first time publishing to Windows Azure, you must download your credentials.</span></span> <span data-ttu-id="08d16-176">En el **publicar** asistente, haga clic en "Iniciar sesión para descargar credenciales".</span><span class="sxs-lookup"><span data-stu-id="08d16-176">In the **Publish** wizard, click "Sign in to download credentials".</span></span> <span data-ttu-id="08d16-177">Esto le pedirá que inicie sesión en el portal de Windows Azure y descargar un archivo de configuración de publicación.</span><span class="sxs-lookup"><span data-stu-id="08d16-177">This will prompt you to sign into the Windows Azure portal and download a publish settings file.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

<span data-ttu-id="08d16-178">Haga clic en **importación** y seleccione el archivo de configuración de publicación que descargó.</span><span class="sxs-lookup"><span data-stu-id="08d16-178">Click **Import** and select the publish settings file that you downloaded.</span></span>

<span data-ttu-id="08d16-179">Haga clic en **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="08d16-179">Click **Next**.</span></span> <span data-ttu-id="08d16-180">En el **configuración de publicación** cuadro de diálogo, en **servicio de nube**, seleccione el servicio de nube que creó anteriormente.</span><span class="sxs-lookup"><span data-stu-id="08d16-180">In the **Publish Settings** dialog, under **Cloud Service**, select the cloud service that you created earlier.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

<span data-ttu-id="08d16-181">Haga clic en **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="08d16-181">Click **Publish**.</span></span> <span data-ttu-id="08d16-182">Puede tardar unos minutos para implementar la aplicación e iniciar las máquinas virtuales.</span><span class="sxs-lookup"><span data-stu-id="08d16-182">It can take a few minutes to deploy the application and start the VMs.</span></span>

<span data-ttu-id="08d16-183">Ahora al ejecutar la aplicación de chat, las instancias de rol se comunican a través del Bus de servicio de Azure, con un tema de Bus de servicio.</span><span class="sxs-lookup"><span data-stu-id="08d16-183">Now when you run the chat application, the role instances communicate through Azure Service Bus, using a Service Bus topic.</span></span> <span data-ttu-id="08d16-184">Un tema es una cola de mensajes que permite a varios suscriptores.</span><span class="sxs-lookup"><span data-stu-id="08d16-184">A topic is a message queue that allows multiple subscribers.</span></span>

<span data-ttu-id="08d16-185">El backplane crea automáticamente el tema y las suscripciones.</span><span class="sxs-lookup"><span data-stu-id="08d16-185">The backplane automatically creates the topic and the subscriptions.</span></span> <span data-ttu-id="08d16-186">Para ver las suscripciones y la actividad de un mensaje, abra el portal de Azure, seleccione el espacio de nombres de Bus de servicio y haga clic en "Temas".</span><span class="sxs-lookup"><span data-stu-id="08d16-186">To see the subscriptions and message activity, open the Azure portal, select the Service Bus namespace, and click on "Topics".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

<span data-ttu-id="08d16-187">Puede durar unos minutos para la actividad de mensaje que aparezcan en el panel.</span><span class="sxs-lookup"><span data-stu-id="08d16-187">It make take a few minutes for the message activity to show up in the dashboard.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image15.png)

<span data-ttu-id="08d16-188">SignalR administra la duración de tema.</span><span class="sxs-lookup"><span data-stu-id="08d16-188">SignalR manages the topic lifetime.</span></span> <span data-ttu-id="08d16-189">Siempre y cuando se implementa la aplicación, no intente eliminar temas manualmente o cambiar la configuración en el tema.</span><span class="sxs-lookup"><span data-stu-id="08d16-189">As long as your application is deployed, don't try to manually delete topics or change settings on the topic.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="08d16-190">Solución de problemas</span><span class="sxs-lookup"><span data-stu-id="08d16-190">Troubleshooting</span></span>

<span data-ttu-id="08d16-191">**System.InvalidOperationException "El solo IsolationLevel compatible es 'IsolationLevel.Serializable'".**</span><span class="sxs-lookup"><span data-stu-id="08d16-191">**System.InvalidOperationException "The only supported IsolationLevel is 'IsolationLevel.Serializable'."**</span></span>

<span data-ttu-id="08d16-192">Este error puede producirse si el nivel de transacción para una operación se establece a algo distinto de `Serializable`.</span><span class="sxs-lookup"><span data-stu-id="08d16-192">This error can occur if the transaction level for an operation is set to something other than `Serializable`.</span></span> <span data-ttu-id="08d16-193">Compruebe que no se se realiza ninguna operación con otros niveles de transacción.</span><span class="sxs-lookup"><span data-stu-id="08d16-193">Verify that no operations are being performed with other transaction levels.</span></span>
