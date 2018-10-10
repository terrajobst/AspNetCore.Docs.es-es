---
uid: signalr/overview/performance/scaleout-with-windows-azure-service-bus
title: Escalabilidad horizontal de SignalR con Azure Service Bus | Microsoft Docs
author: MikeWasson
description: Las versiones de software usan en esta versión de Visual Studio 2013 .NET 4.5 SignalR tema 2 versiones anteriores de esta versión de tema para el SignalR 1.x de este tema...
ms.author: riande
ms.date: 06/10/2014
ms.assetid: ce1305f9-30fd-49e3-bf38-d0a78dfb06c3
msc.legacyurl: /signalr/overview/performance/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: 3adc8768eb7271de32180ba98f67864b22283510
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/10/2018
ms.locfileid: "48910803"
---
<a name="signalr-scaleout-with-azure-service-bus"></a><span data-ttu-id="c08b4-103">Escalabilidad horizontal de SignalR con Azure Service Bus</span><span class="sxs-lookup"><span data-stu-id="c08b4-103">SignalR Scaleout with Azure Service Bus</span></span>
====================
<span data-ttu-id="c08b4-104">por [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="c08b4-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

<span data-ttu-id="c08b4-105">En este tutorial, implementará una aplicación de SignalR para un rol Web de Windows Azure mediante el backplane de Service Bus para distribuir mensajes a cada instancia de rol.</span><span class="sxs-lookup"><span data-stu-id="c08b4-105">In this tutorial, you will deploy a SignalR application to a Windows Azure Web Role, using the Service Bus backplane to distribute messages to each role instance.</span></span> <span data-ttu-id="c08b4-106">(También puede usar el backplane de Service Bus con [web apps en Azure App Service](https://docs.microsoft.com/azure/app-service-web/).)</span><span class="sxs-lookup"><span data-stu-id="c08b4-106">(You can also use the Service Bus backplane with [web apps in Azure App Service](https://docs.microsoft.com/azure/app-service-web/).)</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

<span data-ttu-id="c08b4-107">Requisitos previos:</span><span class="sxs-lookup"><span data-stu-id="c08b4-107">Prerequisites:</span></span>

- <span data-ttu-id="c08b4-108">Una cuenta de Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="c08b4-108">A Windows Azure account.</span></span>
- <span data-ttu-id="c08b4-109">El [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="c08b4-109">The [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span></span>
- <span data-ttu-id="c08b4-110">Visual Studio 2012 o 2013.</span><span class="sxs-lookup"><span data-stu-id="c08b4-110">Visual Studio 2012 or 2013.</span></span>

<span data-ttu-id="c08b4-111">También es compatible con el backplane de bus de servicio [Service Bus para Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), versión 1.1.</span><span class="sxs-lookup"><span data-stu-id="c08b4-111">The service bus backplane is also compatible with [Service Bus for Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), version 1.1.</span></span> <span data-ttu-id="c08b4-112">Sin embargo, no es compatible con la versión 1.0 de Service Bus para Windows Server.</span><span class="sxs-lookup"><span data-stu-id="c08b4-112">However, it is not compatible with version 1.0 of Service Bus for Windows Server.</span></span>

## <a name="pricing"></a><span data-ttu-id="c08b4-113">Precio</span><span class="sxs-lookup"><span data-stu-id="c08b4-113">Pricing</span></span>

<span data-ttu-id="c08b4-114">El backplane de Service Bus usa temas para enviar mensajes.</span><span class="sxs-lookup"><span data-stu-id="c08b4-114">The Service Bus backplane uses topics to send messages.</span></span> <span data-ttu-id="c08b4-115">Para la información de precio más reciente, consulte [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).</span><span class="sxs-lookup"><span data-stu-id="c08b4-115">For the latest pricing information, see [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).</span></span> <span data-ttu-id="c08b4-116">En el momento de redactar este artículo, puede enviar 1.000.000 mensajes al mes por menos de $1.</span><span class="sxs-lookup"><span data-stu-id="c08b4-116">At the time of this writing, you can send 1,000,000 messages per month for less than $1.</span></span> <span data-ttu-id="c08b4-117">El backplane envía un mensaje de bus de servicio para cada invocación de un método de concentrador SignalR.</span><span class="sxs-lookup"><span data-stu-id="c08b4-117">The backplane sends a service bus message for each invocation of a SignalR hub method.</span></span> <span data-ttu-id="c08b4-118">También hay algunos mensajes de control para las conexiones, desconexiones, dejando o unirse a grupos y así sucesivamente.</span><span class="sxs-lookup"><span data-stu-id="c08b4-118">There are also some control messages for connections, disconnections, joining or leaving groups, and so forth.</span></span> <span data-ttu-id="c08b4-119">En la mayoría de las aplicaciones, la mayoría del tráfico de mensajes serán las invocaciones de método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="c08b4-119">In most applications, the majority of the message traffic will be hub method invocations.</span></span>

## <a name="overview"></a><span data-ttu-id="c08b4-120">Información general</span><span class="sxs-lookup"><span data-stu-id="c08b4-120">Overview</span></span>

<span data-ttu-id="c08b4-121">Antes de entrar en el tutorial detallado, le presentamos una introducción rápida de lo que hará.</span><span class="sxs-lookup"><span data-stu-id="c08b4-121">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="c08b4-122">Usar el portal de Windows Azure para crear un nuevo espacio de nombres de Service Bus.</span><span class="sxs-lookup"><span data-stu-id="c08b4-122">Use the Windows Azure portal to create a new Service Bus namespace.</span></span>
2. <span data-ttu-id="c08b4-123">Agregue estos paquetes de NuGet para la aplicación:</span><span class="sxs-lookup"><span data-stu-id="c08b4-123">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="c08b4-124">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="c08b4-124">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - <span data-ttu-id="c08b4-125">[Microsoft.AspNet.SignalR.ServiceBus3](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus3) o [Microsoft.AspNet.SignalR.ServiceBus](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus)</span><span class="sxs-lookup"><span data-stu-id="c08b4-125">[Microsoft.AspNet.SignalR.ServiceBus3](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus3) or [Microsoft.AspNet.SignalR.ServiceBus](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus)</span></span>
3. <span data-ttu-id="c08b4-126">Cree una aplicación de SignalR.</span><span class="sxs-lookup"><span data-stu-id="c08b4-126">Create a SignalR application.</span></span>
4. <span data-ttu-id="c08b4-127">Agregue el siguiente código en Startup.cs, para configurar la placa posterior:</span><span class="sxs-lookup"><span data-stu-id="c08b4-127">Add the following code to Startup.cs to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

<span data-ttu-id="c08b4-128">Este código configura el backplane con los valores predeterminados de [TopicCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.servicebusscaleoutconfiguration.topiccount(v=vs.118).aspx) y [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span><span class="sxs-lookup"><span data-stu-id="c08b4-128">This code configures the backplane with the default values for [TopicCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.servicebusscaleoutconfiguration.topiccount(v=vs.118).aspx) and [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span></span> <span data-ttu-id="c08b4-129">Para obtener información acerca de cómo cambiar estos valores, vea [rendimiento de SignalR: métricas de escalado horizontal](signalr-performance.md#scaleout_metrics).</span><span class="sxs-lookup"><span data-stu-id="c08b4-129">For information on changing these values, see [SignalR Performance: Scaleout Metrics](signalr-performance.md#scaleout_metrics).</span></span>

<span data-ttu-id="c08b4-130">Para cada aplicación, elija un valor diferente para "YourAppName".</span><span class="sxs-lookup"><span data-stu-id="c08b4-130">For each application, pick a different value for "YourAppName".</span></span> <span data-ttu-id="c08b4-131">No use el mismo valor en varias aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="c08b4-131">Do not use the same value across multiple applications.</span></span>

## <a name="create-the-azure-services"></a><span data-ttu-id="c08b4-132">Crear los servicios de Azure</span><span class="sxs-lookup"><span data-stu-id="c08b4-132">Create the Azure Services</span></span>

<span data-ttu-id="c08b4-133">Crear un servicio en la nube, como se describe en [cómo crear e implementar un servicio de nube](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span><span class="sxs-lookup"><span data-stu-id="c08b4-133">Create a Cloud Service, as described in [How to Create and Deploy a Cloud Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span></span> <span data-ttu-id="c08b4-134">Siga los pasos descritos en la sección "Cómo: crear un servicio de nube mediante Creación rápida".</span><span class="sxs-lookup"><span data-stu-id="c08b4-134">Follow the steps in the section "How to: Create a cloud service using Quick Create".</span></span> <span data-ttu-id="c08b4-135">Para este tutorial, no es necesario cargar un certificado.</span><span class="sxs-lookup"><span data-stu-id="c08b4-135">For this tutorial, you do not need to upload a certificate.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image2.png)

<span data-ttu-id="c08b4-136">Crear un nuevo espacio de nombres de Service Bus, como se describe en [cómo a usar temas/suscripciones de Service Bus](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span><span class="sxs-lookup"><span data-stu-id="c08b4-136">Create a new Service Bus namespace, as described in [How to Use Service Bus Topics/Subscriptions](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span></span> <span data-ttu-id="c08b4-137">Siga los pasos descritos en la sección "Crear unas Namespace de servicio".</span><span class="sxs-lookup"><span data-stu-id="c08b4-137">Follow the steps in the section "Create a Service Namespace".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="c08b4-138">Asegúrese de seleccionar la misma región para el servicio en la nube y el espacio de nombres de Service Bus.</span><span class="sxs-lookup"><span data-stu-id="c08b4-138">Make sure to select the same region for the cloud service and the Service Bus namespace.</span></span>


## <a name="create-the-visual-studio-project"></a><span data-ttu-id="c08b4-139">Crear el proyecto de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c08b4-139">Create the Visual Studio Project</span></span>

<span data-ttu-id="c08b4-140">Inicie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c08b4-140">Start Visual Studio.</span></span> <span data-ttu-id="c08b4-141">Desde el **archivo** menú, haga clic en **nuevo proyecto**.</span><span class="sxs-lookup"><span data-stu-id="c08b4-141">From the **File** menu, click **New Project**.</span></span>

<span data-ttu-id="c08b4-142">En el **nuevo proyecto** cuadro de diálogo, expanda **Visual C#**.</span><span class="sxs-lookup"><span data-stu-id="c08b4-142">In the **New Project** dialog box, expand **Visual C#**.</span></span> <span data-ttu-id="c08b4-143">En **plantillas instaladas**, seleccione **en la nube** y, a continuación, seleccione **Windows Azure Cloud Service**.</span><span class="sxs-lookup"><span data-stu-id="c08b4-143">Under **Installed Templates**, select **Cloud** and then select **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="c08b4-144">Mantenga el valor predeterminado de .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="c08b4-144">Keep the default .NET Framework 4.5.</span></span> <span data-ttu-id="c08b4-145">Nombre de la aplicación ChatService y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="c08b4-145">Name the application ChatService and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image4.png)

<span data-ttu-id="c08b4-146">En el **nuevo servicio de nube de Windows Azure** cuadro de diálogo, seleccione rol Web de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c08b4-146">In the **New Windows Azure Cloud Service** dialog, select ASP.NET Web Role.</span></span> <span data-ttu-id="c08b4-147">Haga clic en el botón de flecha derecha (**&gt;**) para agregar el rol a la solución.</span><span class="sxs-lookup"><span data-stu-id="c08b4-147">Click the right-arrow button (**&gt;**) to add the role to your solution.</span></span>

<span data-ttu-id="c08b4-148">Mantenga el mouse sobre el nuevo rol, por lo que el icono de lápiz visible.</span><span class="sxs-lookup"><span data-stu-id="c08b4-148">Hover the mouse over the new role, so the pencil icon visible.</span></span> <span data-ttu-id="c08b4-149">Haga clic en este icono para cambiar el nombre de la función.</span><span class="sxs-lookup"><span data-stu-id="c08b4-149">Click this icon to rename the role.</span></span> <span data-ttu-id="c08b4-150">Nombre de la función "SignalRChat" y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="c08b4-150">Name the role "SignalRChat" and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

<span data-ttu-id="c08b4-151">En el **nuevo proyecto ASP.NET** cuadro de diálogo, seleccione **MVC**y haga clic en Aceptar.</span><span class="sxs-lookup"><span data-stu-id="c08b4-151">In the **New ASP.NET Project** dialog, select **MVC**, and click OK.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

<span data-ttu-id="c08b4-152">El Asistente para proyecto crea dos proyectos:</span><span class="sxs-lookup"><span data-stu-id="c08b4-152">The project wizard creates two projects:</span></span>

- <span data-ttu-id="c08b4-153">ChatService: Este proyecto es la aplicación de Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="c08b4-153">ChatService: This project is the Windows Azure application.</span></span> <span data-ttu-id="c08b4-154">Define los roles de Azure y otras opciones de configuración.</span><span class="sxs-lookup"><span data-stu-id="c08b4-154">It defines the Azure roles and other configuration options.</span></span>
- <span data-ttu-id="c08b4-155">SignalRChat: Este proyecto es el proyecto de ASP.NET MVC 5.</span><span class="sxs-lookup"><span data-stu-id="c08b4-155">SignalRChat: This project is your ASP.NET MVC 5 project.</span></span>

## <a name="create-the-signalr-chat-application"></a><span data-ttu-id="c08b4-156">Crear la aplicación de Chat de SignalR</span><span class="sxs-lookup"><span data-stu-id="c08b4-156">Create the SignalR Chat Application</span></span>

<span data-ttu-id="c08b4-157">Para crear la aplicación de chat, siga los pasos del tutorial [Introducción a SignalR y MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span><span class="sxs-lookup"><span data-stu-id="c08b4-157">To create the chat application, follow the steps in the tutorial [Getting Started with SignalR and MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span></span>

<span data-ttu-id="c08b4-158">Use NuGet para instalar las bibliotecas necesarias.</span><span class="sxs-lookup"><span data-stu-id="c08b4-158">Use NuGet to install the required libraries.</span></span> <span data-ttu-id="c08b4-159">Desde el **herramientas** menú, seleccione **Administrador de paquetes de NuGet**, a continuación, seleccione **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="c08b4-159">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="c08b4-160">En el **Package Manager Console** ventana, escriba los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="c08b4-160">In the **Package Manager Console** window, enter the following commands:</span></span>

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

<span data-ttu-id="c08b4-161">Use el `-ProjectName` opción para instalar los paquetes en el proyecto de ASP.NET MVC, en lugar de en el proyecto de Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="c08b4-161">Use the `-ProjectName` option to install the packages to the ASP.NET MVC project, rather than the Windows Azure project.</span></span>

## <a name="configure-the-backplane"></a><span data-ttu-id="c08b4-162">Configurar la placa posterior</span><span class="sxs-lookup"><span data-stu-id="c08b4-162">Configure the Backplane</span></span>

<span data-ttu-id="c08b4-163">En el archivo de la aplicación Startup.cs, agregue el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="c08b4-163">In your application's Startup.cs file, add the following code:</span></span>

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

<span data-ttu-id="c08b4-164">Ahora deberá obtener la cadena de conexión del bus de servicio.</span><span class="sxs-lookup"><span data-stu-id="c08b4-164">Now you need to get your service bus connection string.</span></span> <span data-ttu-id="c08b4-165">En el portal de Azure, seleccione el espacio de nombres del bus de servicio que ha creado y haga clic en el icono de la clave de acceso.</span><span class="sxs-lookup"><span data-stu-id="c08b4-165">In the Azure portal, select the service bus namespace that you created and click the Access Key icon.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

<span data-ttu-id="c08b4-166">Copie la cadena de conexión en el Portapapeles y, después, péguelo en el *connectionString* variable.</span><span class="sxs-lookup"><span data-stu-id="c08b4-166">Copy the connection string to the clipboard, then paste it into the *connectionString* variable.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a><span data-ttu-id="c08b4-167">Implementar en Azure</span><span class="sxs-lookup"><span data-stu-id="c08b4-167">Deploy to Azure</span></span>

<span data-ttu-id="c08b4-168">En el Explorador de soluciones, expanda el **Roles** carpeta dentro del proyecto ChatService.</span><span class="sxs-lookup"><span data-stu-id="c08b4-168">In Solution Explorer, expand the **Roles** folder inside the ChatService project.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

<span data-ttu-id="c08b4-169">Haga clic en el rol SignalRChat y seleccione **propiedades**.</span><span class="sxs-lookup"><span data-stu-id="c08b4-169">Right-click the SignalRChat role and select **Properties**.</span></span> <span data-ttu-id="c08b4-170">Seleccione el **configuración** ficha. En **instancias** seleccione 2.</span><span class="sxs-lookup"><span data-stu-id="c08b4-170">Select the **Configuration** tab. Under **Instances** select 2.</span></span> <span data-ttu-id="c08b4-171">También se puede establecer el tamaño de máquina virtual como **Extra pequeño**.</span><span class="sxs-lookup"><span data-stu-id="c08b4-171">You can also set the VM size to **Extra Small**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

<span data-ttu-id="c08b4-172">Guarde los cambios.</span><span class="sxs-lookup"><span data-stu-id="c08b4-172">Save the changes.</span></span>

<span data-ttu-id="c08b4-173">En el Explorador de soluciones, haga clic en el proyecto ChatService.</span><span class="sxs-lookup"><span data-stu-id="c08b4-173">In Solution Explorer, right-click the ChatService project.</span></span> <span data-ttu-id="c08b4-174">Seleccione **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="c08b4-174">Select **Publish**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

<span data-ttu-id="c08b4-175">Si se trata de su primera publicación de tiempo en Windows Azure, debe descargar las credenciales.</span><span class="sxs-lookup"><span data-stu-id="c08b4-175">If this is your first time publishing to Windows Azure, you must download your credentials.</span></span> <span data-ttu-id="c08b4-176">En el **publicar** asistente, haga clic en "Iniciar sesión descargar credenciales".</span><span class="sxs-lookup"><span data-stu-id="c08b4-176">In the **Publish** wizard, click "Sign in to download credentials".</span></span> <span data-ttu-id="c08b4-177">Esto le pedirá que inicie sesión en el portal de Windows Azure y descargar un archivo de configuración de publicación.</span><span class="sxs-lookup"><span data-stu-id="c08b4-177">This will prompt you to sign into the Windows Azure portal and download a publish settings file.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

<span data-ttu-id="c08b4-178">Haga clic en **importación** y seleccione el archivo de configuración de publicación que descargó.</span><span class="sxs-lookup"><span data-stu-id="c08b4-178">Click **Import** and select the publish settings file that you downloaded.</span></span>

<span data-ttu-id="c08b4-179">Haga clic en **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="c08b4-179">Click **Next**.</span></span> <span data-ttu-id="c08b4-180">En el **configuración de publicación** cuadro de diálogo, en **servicio en la nube**, seleccione el servicio en la nube que creó anteriormente.</span><span class="sxs-lookup"><span data-stu-id="c08b4-180">In the **Publish Settings** dialog, under **Cloud Service**, select the cloud service that you created earlier.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

<span data-ttu-id="c08b4-181">Haga clic en **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="c08b4-181">Click **Publish**.</span></span> <span data-ttu-id="c08b4-182">Puede tardar unos minutos en implementar la aplicación e iniciar las máquinas virtuales.</span><span class="sxs-lookup"><span data-stu-id="c08b4-182">It can take a few minutes to deploy the application and start the VMs.</span></span>

<span data-ttu-id="c08b4-183">Ahora al ejecutar la aplicación de chat, las instancias de rol se comunican a través de Azure Service Bus, con un tema de Service Bus.</span><span class="sxs-lookup"><span data-stu-id="c08b4-183">Now when you run the chat application, the role instances communicate through Azure Service Bus, using a Service Bus topic.</span></span> <span data-ttu-id="c08b4-184">Un tema es una cola de mensajes que permite que varios suscriptores.</span><span class="sxs-lookup"><span data-stu-id="c08b4-184">A topic is a message queue that allows multiple subscribers.</span></span>

<span data-ttu-id="c08b4-185">El backplane crea automáticamente el tema y las suscripciones.</span><span class="sxs-lookup"><span data-stu-id="c08b4-185">The backplane automatically creates the topic and the subscriptions.</span></span> <span data-ttu-id="c08b4-186">Para ver las suscripciones y la actividad de mensaje, abra Azure portal, seleccione el espacio de nombres de Service Bus y haga clic en "Temas".</span><span class="sxs-lookup"><span data-stu-id="c08b4-186">To see the subscriptions and message activity, open the Azure portal, select the Service Bus namespace, and click on "Topics".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

<span data-ttu-id="c08b4-187">Pueden pasar unos minutos para que se muestran en el panel de la actividad de mensaje.</span><span class="sxs-lookup"><span data-stu-id="c08b4-187">It make take a few minutes for the message activity to show up in the dashboard.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image15.png)

<span data-ttu-id="c08b4-188">SignalR administra la duración de tema.</span><span class="sxs-lookup"><span data-stu-id="c08b4-188">SignalR manages the topic lifetime.</span></span> <span data-ttu-id="c08b4-189">Siempre y cuando se implementa la aplicación, no intente eliminar temas manualmente o cambiar la configuración en el tema.</span><span class="sxs-lookup"><span data-stu-id="c08b4-189">As long as your application is deployed, don't try to manually delete topics or change settings on the topic.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="c08b4-190">Solución de problemas</span><span class="sxs-lookup"><span data-stu-id="c08b4-190">Troubleshooting</span></span>

<span data-ttu-id="c08b4-191">**System.InvalidOperationException "La única IsolationLevel admitida es 'IsolationLevel.Serializable'".**</span><span class="sxs-lookup"><span data-stu-id="c08b4-191">**System.InvalidOperationException "The only supported IsolationLevel is 'IsolationLevel.Serializable'."**</span></span>

<span data-ttu-id="c08b4-192">Este error puede producirse si se establece el nivel de transacción para una operación a algo distinto `Serializable`.</span><span class="sxs-lookup"><span data-stu-id="c08b4-192">This error can occur if the transaction level for an operation is set to something other than `Serializable`.</span></span> <span data-ttu-id="c08b4-193">Compruebe que ninguna de las operaciones se realizan con otros niveles de transacción.</span><span class="sxs-lookup"><span data-stu-id="c08b4-193">Verify that no operations are being performed with other transaction levels.</span></span>
