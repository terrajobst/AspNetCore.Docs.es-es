---
uid: signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
title: Ampliación de SignalR con el Bus de servicio de Azure (SignalR 1.x) | Documentos de Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/01/2013
ms.topic: article
ms.assetid: 501db899-e68c-49ff-81b2-1dc561bfe908
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: b48a7b04701b69f68a492c0f7e08da4a37a92a48
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="signalr-scaleout-with-azure-service-bus-signalr-1x"></a><span data-ttu-id="27c6b-102">Ampliación de SignalR con el Bus de servicio de Azure (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="27c6b-102">SignalR Scaleout with Azure Service Bus (SignalR 1.x)</span></span>
====================
<span data-ttu-id="27c6b-103">por [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="27c6b-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

<span data-ttu-id="27c6b-104">En este tutorial, implementará una aplicación de SignalR para un rol Web de Windows Azure mediante el backplane de Bus de servicio para distribuir mensajes a cada instancia de rol.</span><span class="sxs-lookup"><span data-stu-id="27c6b-104">In this tutorial, you will deploy a SignalR application to a Windows Azure Web Role, using the Service Bus backplane to distribute messages to each role instance.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

<span data-ttu-id="27c6b-105">Requisitos previos:</span><span class="sxs-lookup"><span data-stu-id="27c6b-105">Prerequisites:</span></span>

- <span data-ttu-id="27c6b-106">Una cuenta de Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="27c6b-106">A Windows Azure account.</span></span>
- <span data-ttu-id="27c6b-107">El [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="27c6b-107">The [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span></span>
- <span data-ttu-id="27c6b-108">Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="27c6b-108">Visual Studio 2012.</span></span>

<span data-ttu-id="27c6b-109">El backplane de bus de servicio también es compatible con [Service Bus para Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), versión 1.1.</span><span class="sxs-lookup"><span data-stu-id="27c6b-109">The service bus backplane is also compatible with [Service Bus for Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), version 1.1.</span></span> <span data-ttu-id="27c6b-110">Sin embargo, no es compatible con la versión 1.0 de Service Bus para Windows Server.</span><span class="sxs-lookup"><span data-stu-id="27c6b-110">However, it is not compatible with version 1.0 of Service Bus for Windows Server.</span></span>

## <a name="pricing"></a><span data-ttu-id="27c6b-111">Precio</span><span class="sxs-lookup"><span data-stu-id="27c6b-111">Pricing</span></span>

<span data-ttu-id="27c6b-112">El backplane de Bus de servicio utiliza temas para enviar mensajes.</span><span class="sxs-lookup"><span data-stu-id="27c6b-112">The Service Bus backplane uses topics to send messages.</span></span> <span data-ttu-id="27c6b-113">Para la información más reciente sobre los precios, consulte [Bus de servicio](https://azure.microsoft.com/pricing/details/service-bus/).</span><span class="sxs-lookup"><span data-stu-id="27c6b-113">For the latest pricing information, see [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).</span></span> <span data-ttu-id="27c6b-114">En el momento de redactar este artículo, puede enviar 1.000.000 mensajes al mes de menos de 1 $.</span><span class="sxs-lookup"><span data-stu-id="27c6b-114">At the time of this writing, you can send 1,000,000 messages per month for less than $1.</span></span> <span data-ttu-id="27c6b-115">El backplane envía un mensaje de bus de servicio para cada invocación de un método de concentrador de SignalR.</span><span class="sxs-lookup"><span data-stu-id="27c6b-115">The backplane sends a service bus message for each invocation of a SignalR hub method.</span></span> <span data-ttu-id="27c6b-116">También hay algunos mensajes de control para las conexiones, cuando se producen desconexiones, dejando o unirse a grupos y así sucesivamente.</span><span class="sxs-lookup"><span data-stu-id="27c6b-116">There are also some control messages for connections, disconnections, joining or leaving groups, and so forth.</span></span> <span data-ttu-id="27c6b-117">En la mayoría de las aplicaciones, la mayoría del tráfico de mensajes serán las invocaciones de método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="27c6b-117">In most applications, the majority of the message traffic will be hub method invocations.</span></span>

## <a name="overview"></a><span data-ttu-id="27c6b-118">Información general</span><span class="sxs-lookup"><span data-stu-id="27c6b-118">Overview</span></span>

<span data-ttu-id="27c6b-119">Antes de entrar en el tutorial detallado, presentamos una introducción rápida de lo va a hacer.</span><span class="sxs-lookup"><span data-stu-id="27c6b-119">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="27c6b-120">Usar el portal de Windows Azure para crear un nuevo espacio de nombres de Bus de servicio.</span><span class="sxs-lookup"><span data-stu-id="27c6b-120">Use the Windows Azure portal to create a new Service Bus namespace.</span></span>
2. <span data-ttu-id="27c6b-121">Agregue estos paquetes de NuGet para la aplicación:</span><span class="sxs-lookup"><span data-stu-id="27c6b-121">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="27c6b-122">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="27c6b-122">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="27c6b-123">Microsoft.AspNet.SignalR.ServiceBus</span><span class="sxs-lookup"><span data-stu-id="27c6b-123">Microsoft.AspNet.SignalR.ServiceBus</span></span>](http://www.nuget.org/packages/SignalR.WindowsAzureServiceBus)
3. <span data-ttu-id="27c6b-124">Cree una aplicación de SignalR.</span><span class="sxs-lookup"><span data-stu-id="27c6b-124">Create a SignalR application.</span></span>
4. <span data-ttu-id="27c6b-125">Agregue el código siguiente a Global.asax para configurar el plano posterior:</span><span class="sxs-lookup"><span data-stu-id="27c6b-125">Add the following code to Global.asax to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

<span data-ttu-id="27c6b-126">Para cada aplicación, elegir un valor diferente para "YourAppName".</span><span class="sxs-lookup"><span data-stu-id="27c6b-126">For each application, pick a different value for "YourAppName".</span></span> <span data-ttu-id="27c6b-127">No utilice el mismo valor en varias aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="27c6b-127">Do not use the same value across multiple applications.</span></span>

## <a name="create-the-azure-services"></a><span data-ttu-id="27c6b-128">Crear los servicios de Azure</span><span class="sxs-lookup"><span data-stu-id="27c6b-128">Create the Azure Services</span></span>

<span data-ttu-id="27c6b-129">Crear un servicio en la nube, como se describe en [cómo crear e implementar un servicio de nube](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span><span class="sxs-lookup"><span data-stu-id="27c6b-129">Create a Cloud Service, as described in [How to Create and Deploy a Cloud Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span></span> <span data-ttu-id="27c6b-130">Siga los pasos descritos en la sección "Cómo: crear un servicio de nube mediante Creación rápida".</span><span class="sxs-lookup"><span data-stu-id="27c6b-130">Follow the steps in the section "How to: Create a cloud service using Quick Create".</span></span> <span data-ttu-id="27c6b-131">Para este tutorial, no es necesario cargar un certificado.</span><span class="sxs-lookup"><span data-stu-id="27c6b-131">For this tutorial, you do not need to upload a certificate.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image2.png)

<span data-ttu-id="27c6b-132">Crear un nuevo espacio de nombres de Bus de servicio, como se describe en [cómo para el Bus de servicio de uso temas/suscripciones](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span><span class="sxs-lookup"><span data-stu-id="27c6b-132">Create a new Service Bus namespace, as described in [How to Use Service Bus Topics/Subscriptions](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span></span> <span data-ttu-id="27c6b-133">Siga los pasos descritos en la sección "Crear un Namespace de servicio".</span><span class="sxs-lookup"><span data-stu-id="27c6b-133">Follow the steps in the section "Create a Service Namespace".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="27c6b-134">Asegúrese de seleccionar la misma región para el servicio de nube y el espacio de nombres de Bus de servicio.</span><span class="sxs-lookup"><span data-stu-id="27c6b-134">Make sure to select the same region for the cloud service and the Service Bus namespace.</span></span>


## <a name="create-the-visual-studio-project"></a><span data-ttu-id="27c6b-135">Crear el proyecto de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="27c6b-135">Create the Visual Studio Project</span></span>

<span data-ttu-id="27c6b-136">Inicie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="27c6b-136">Start Visual Studio.</span></span> <span data-ttu-id="27c6b-137">Desde el **archivo** menú, haga clic en **nuevo proyecto**.</span><span class="sxs-lookup"><span data-stu-id="27c6b-137">From the **File** menu, click **New Project**.</span></span>

<span data-ttu-id="27c6b-138">En el **nuevo proyecto** cuadro de diálogo, expanda **Visual C#**.</span><span class="sxs-lookup"><span data-stu-id="27c6b-138">In the **New Project** dialog box, expand **Visual C#**.</span></span> <span data-ttu-id="27c6b-139">En **plantillas instaladas**, seleccione **nube** y, a continuación, seleccione **servicio de nube de Windows Azure**.</span><span class="sxs-lookup"><span data-stu-id="27c6b-139">Under **Installed Templates**, select **Cloud** and then select **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="27c6b-140">Mantenga el valor predeterminado de .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="27c6b-140">Keep the default .NET Framework 4.5.</span></span> <span data-ttu-id="27c6b-141">Nombre de la aplicación ChatChannel y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="27c6b-141">Name the application ChatService and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image4.png)

<span data-ttu-id="27c6b-142">En el **nuevo servicio de nube de Windows Azure** cuadro de diálogo, seleccione un rol de Web de 4 de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="27c6b-142">In the **New Windows Azure Cloud Service** dialog, select ASP.NET MVC 4 Web Role.</span></span> <span data-ttu-id="27c6b-143">Haga clic en el botón de flecha derecha (**&gt;**) para agregar el rol a la solución.</span><span class="sxs-lookup"><span data-stu-id="27c6b-143">Click the right-arrow button (**&gt;**) to add the role to your solution.</span></span>

<span data-ttu-id="27c6b-144">Mantenga el mouse sobre el nuevo rol, por lo que el icono de lápiz visible.</span><span class="sxs-lookup"><span data-stu-id="27c6b-144">Hover the mouse over the new role, so the pencil icon visible.</span></span> <span data-ttu-id="27c6b-145">Haga clic en este icono para cambiar el nombre de la función.</span><span class="sxs-lookup"><span data-stu-id="27c6b-145">Click this icon to rename the role.</span></span> <span data-ttu-id="27c6b-146">Nombre de la función "SignalRChat" y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="27c6b-146">Name the role "SignalRChat" and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

<span data-ttu-id="27c6b-147">En el **nuevo proyecto de ASP.NET MVC 4** asistente, seleccione **aplicación de Internet**.</span><span class="sxs-lookup"><span data-stu-id="27c6b-147">In the **New ASP.NET MVC 4 Project** wizard, select **Internet Application**.</span></span> <span data-ttu-id="27c6b-148">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="27c6b-148">Click **OK**.</span></span> <span data-ttu-id="27c6b-149">El Asistente para proyecto crea dos proyectos:</span><span class="sxs-lookup"><span data-stu-id="27c6b-149">The project wizard creates two projects:</span></span>

- <span data-ttu-id="27c6b-150">ChatChannel: Este proyecto es la aplicación de Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="27c6b-150">ChatService: This project is the Windows Azure application.</span></span> <span data-ttu-id="27c6b-151">Define las funciones de Azure y otras opciones de configuración.</span><span class="sxs-lookup"><span data-stu-id="27c6b-151">It defines the Azure roles and other configuration options.</span></span>
- <span data-ttu-id="27c6b-152">SignalRChat: Este proyecto es el proyecto de ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="27c6b-152">SignalRChat: This project is your ASP.NET MVC 4 project.</span></span>

## <a name="create-the-signalr-chat-application"></a><span data-ttu-id="27c6b-153">Crear la aplicación de Chat de SignalR</span><span class="sxs-lookup"><span data-stu-id="27c6b-153">Create the SignalR Chat Application</span></span>

<span data-ttu-id="27c6b-154">Para crear la aplicación de chat, siga los pasos del tutorial [Getting Started with SignalR y MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).</span><span class="sxs-lookup"><span data-stu-id="27c6b-154">To create the chat application, follow the steps in the tutorial [Getting Started with SignalR and MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).</span></span>

<span data-ttu-id="27c6b-155">Use NuGet para instalar las bibliotecas necesarias.</span><span class="sxs-lookup"><span data-stu-id="27c6b-155">Use NuGet to install the required libraries.</span></span> <span data-ttu-id="27c6b-156">Desde el **herramientas** menú, seleccione **Administrador de paquetes de biblioteca**, a continuación, seleccione **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="27c6b-156">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="27c6b-157">En el **Package Manager Console** ventana, escriba los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="27c6b-157">In the **Package Manager Console** window, enter the following commands:</span></span>

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

<span data-ttu-id="27c6b-158">Use la `-ProjectName` opción para instalar los paquetes en el proyecto de ASP.NET MVC, en lugar de en el proyecto de Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="27c6b-158">Use the `-ProjectName` option to install the packages to the ASP.NET MVC project, rather than the Windows Azure project.</span></span>

## <a name="configure-the-backplane"></a><span data-ttu-id="27c6b-159">Configurar el Backplane</span><span class="sxs-lookup"><span data-stu-id="27c6b-159">Configure the Backplane</span></span>

<span data-ttu-id="27c6b-160">En el archivo Global.asax de la aplicación, agregue el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="27c6b-160">In your application's Global.asax file, add the following code:</span></span>

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

<span data-ttu-id="27c6b-161">Ahora debe obtener la cadena de conexión de bus de servicio.</span><span class="sxs-lookup"><span data-stu-id="27c6b-161">Now you need to get your service bus connection string.</span></span> <span data-ttu-id="27c6b-162">En el portal de Azure, seleccione el espacio de nombres de bus de servicio que creó y haga clic en el icono de la clave de acceso.</span><span class="sxs-lookup"><span data-stu-id="27c6b-162">In the Azure portal, select the service bus namespace that you created and click the Access Key icon.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

<span data-ttu-id="27c6b-163">Copiar la cadena de conexión en el Portapapeles, a continuación, péguelo en el *connectionString* variable.</span><span class="sxs-lookup"><span data-stu-id="27c6b-163">Copy the connection string to the clipboard, then paste it into the *connectionString* variable.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a><span data-ttu-id="27c6b-164">Implementar en Azure</span><span class="sxs-lookup"><span data-stu-id="27c6b-164">Deploy to Azure</span></span>

<span data-ttu-id="27c6b-165">En el Explorador de soluciones, expanda la **Roles** carpeta dentro del proyecto ChatChannel.</span><span class="sxs-lookup"><span data-stu-id="27c6b-165">In Solution Explorer, expand the **Roles** folder inside the ChatService project.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

<span data-ttu-id="27c6b-166">Haga clic en el rol de SignalRChat y seleccione **propiedades**.</span><span class="sxs-lookup"><span data-stu-id="27c6b-166">Right-click the SignalRChat role and select **Properties**.</span></span> <span data-ttu-id="27c6b-167">Seleccione el **configuración** ficha. En **instancias** seleccione 2.</span><span class="sxs-lookup"><span data-stu-id="27c6b-167">Select the **Configuration** tab. Under **Instances** select 2.</span></span> <span data-ttu-id="27c6b-168">También puede establecer el tamaño de VM en **Extra pequeño**.</span><span class="sxs-lookup"><span data-stu-id="27c6b-168">You can also set the VM size to **Extra Small**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

<span data-ttu-id="27c6b-169">Guarde los cambios.</span><span class="sxs-lookup"><span data-stu-id="27c6b-169">Save the changes.</span></span>

<span data-ttu-id="27c6b-170">En el Explorador de soluciones, haga clic en el proyecto ChatChannel.</span><span class="sxs-lookup"><span data-stu-id="27c6b-170">In Solution Explorer, right-click the ChatService project.</span></span> <span data-ttu-id="27c6b-171">Seleccione **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="27c6b-171">Select **Publish**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

<span data-ttu-id="27c6b-172">Si se trata de la primera publicación de tiempo en Windows Azure, debe descargar sus credenciales.</span><span class="sxs-lookup"><span data-stu-id="27c6b-172">If this is your first time publishing to Windows Azure, you must download your credentials.</span></span> <span data-ttu-id="27c6b-173">En el **publicar** asistente, haga clic en "Iniciar sesión para descargar credenciales".</span><span class="sxs-lookup"><span data-stu-id="27c6b-173">In the **Publish** wizard, click "Sign in to download credentials".</span></span> <span data-ttu-id="27c6b-174">Esto le pedirá que inicie sesión en el portal de Windows Azure y descargar un archivo de configuración de publicación.</span><span class="sxs-lookup"><span data-stu-id="27c6b-174">This will prompt you to sign into the Windows Azure portal and download a publish settings file.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

<span data-ttu-id="27c6b-175">Haga clic en **importación** y seleccione el archivo de configuración de publicación que descargó.</span><span class="sxs-lookup"><span data-stu-id="27c6b-175">Click **Import** and select the publish settings file that you downloaded.</span></span>

<span data-ttu-id="27c6b-176">Haga clic en **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="27c6b-176">Click **Next**.</span></span> <span data-ttu-id="27c6b-177">En el **configuración de publicación** cuadro de diálogo, en **servicio de nube**, seleccione el servicio de nube que creó anteriormente.</span><span class="sxs-lookup"><span data-stu-id="27c6b-177">In the **Publish Settings** dialog, under **Cloud Service**, select the cloud service that you created earlier.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

<span data-ttu-id="27c6b-178">Haga clic en **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="27c6b-178">Click **Publish**.</span></span> <span data-ttu-id="27c6b-179">Puede tardar unos minutos para implementar la aplicación e iniciar las máquinas virtuales.</span><span class="sxs-lookup"><span data-stu-id="27c6b-179">It can take a few minutes to deploy the application and start the VMs.</span></span>

<span data-ttu-id="27c6b-180">Ahora al ejecutar la aplicación de chat, las instancias de rol se comunican a través del Bus de servicio de Azure, con un tema de Bus de servicio.</span><span class="sxs-lookup"><span data-stu-id="27c6b-180">Now when you run the chat application, the role instances communicate through Azure Service Bus, using a Service Bus topic.</span></span> <span data-ttu-id="27c6b-181">Un tema es una cola de mensajes que permite a varios suscriptores.</span><span class="sxs-lookup"><span data-stu-id="27c6b-181">A topic is a message queue that allows multiple subscribers.</span></span>

<span data-ttu-id="27c6b-182">El backplane crea automáticamente el tema y las suscripciones.</span><span class="sxs-lookup"><span data-stu-id="27c6b-182">The backplane automatically creates the topic and the subscriptions.</span></span> <span data-ttu-id="27c6b-183">Para ver las suscripciones y la actividad de un mensaje, abra el portal de Azure, seleccione el espacio de nombres de Bus de servicio y haga clic en "Temas".</span><span class="sxs-lookup"><span data-stu-id="27c6b-183">To see the subscriptions and message activity, open the Azure portal, select the Service Bus namespace, and click on "Topics".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

<span data-ttu-id="27c6b-184">Puede durar unos minutos para la actividad de mensaje que aparezcan en el panel.</span><span class="sxs-lookup"><span data-stu-id="27c6b-184">It make take a few minutes for the message activity to show up in the dashboard.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

<span data-ttu-id="27c6b-185">SignalR administra la duración de tema.</span><span class="sxs-lookup"><span data-stu-id="27c6b-185">SignalR manages the topic lifetime.</span></span> <span data-ttu-id="27c6b-186">Siempre y cuando se implementa la aplicación, no intente eliminar temas manualmente o cambiar la configuración en el tema.</span><span class="sxs-lookup"><span data-stu-id="27c6b-186">As long as your application is deployed, don't try to manually delete topics or change settings on the topic.</span></span>
