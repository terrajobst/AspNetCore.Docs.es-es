---
uid: signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
title: Escalabilidad horizontal de SignalR con Azure Service Bus (SignalR 1.x) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 05/01/2013
ms.assetid: 501db899-e68c-49ff-81b2-1dc561bfe908
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: 751de591ec62942c2c9d8b26af40101eb2e922d5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820945"
---
<a name="signalr-scaleout-with-azure-service-bus-signalr-1x"></a><span data-ttu-id="63c39-102">Escalabilidad horizontal de SignalR con Azure Service Bus (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="63c39-102">SignalR Scaleout with Azure Service Bus (SignalR 1.x)</span></span>
====================
<span data-ttu-id="63c39-103">por [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="63c39-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

<span data-ttu-id="63c39-104">En este tutorial, implementará una aplicación de SignalR para un rol Web de Windows Azure mediante el backplane de Service Bus para distribuir mensajes a cada instancia de rol.</span><span class="sxs-lookup"><span data-stu-id="63c39-104">In this tutorial, you will deploy a SignalR application to a Windows Azure Web Role, using the Service Bus backplane to distribute messages to each role instance.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

<span data-ttu-id="63c39-105">Requisitos previos:</span><span class="sxs-lookup"><span data-stu-id="63c39-105">Prerequisites:</span></span>

- <span data-ttu-id="63c39-106">Una cuenta de Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="63c39-106">A Windows Azure account.</span></span>
- <span data-ttu-id="63c39-107">El [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="63c39-107">The [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span></span>
- <span data-ttu-id="63c39-108">Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="63c39-108">Visual Studio 2012.</span></span>

<span data-ttu-id="63c39-109">También es compatible con el backplane de bus de servicio [Service Bus para Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), versión 1.1.</span><span class="sxs-lookup"><span data-stu-id="63c39-109">The service bus backplane is also compatible with [Service Bus for Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), version 1.1.</span></span> <span data-ttu-id="63c39-110">Sin embargo, no es compatible con la versión 1.0 de Service Bus para Windows Server.</span><span class="sxs-lookup"><span data-stu-id="63c39-110">However, it is not compatible with version 1.0 of Service Bus for Windows Server.</span></span>

## <a name="pricing"></a><span data-ttu-id="63c39-111">Precio</span><span class="sxs-lookup"><span data-stu-id="63c39-111">Pricing</span></span>

<span data-ttu-id="63c39-112">El backplane de Service Bus usa temas para enviar mensajes.</span><span class="sxs-lookup"><span data-stu-id="63c39-112">The Service Bus backplane uses topics to send messages.</span></span> <span data-ttu-id="63c39-113">Para la información de precio más reciente, consulte [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).</span><span class="sxs-lookup"><span data-stu-id="63c39-113">For the latest pricing information, see [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).</span></span> <span data-ttu-id="63c39-114">En el momento de redactar este artículo, puede enviar 1.000.000 mensajes al mes por menos de $1.</span><span class="sxs-lookup"><span data-stu-id="63c39-114">At the time of this writing, you can send 1,000,000 messages per month for less than $1.</span></span> <span data-ttu-id="63c39-115">El backplane envía un mensaje de bus de servicio para cada invocación de un método de concentrador SignalR.</span><span class="sxs-lookup"><span data-stu-id="63c39-115">The backplane sends a service bus message for each invocation of a SignalR hub method.</span></span> <span data-ttu-id="63c39-116">También hay algunos mensajes de control para las conexiones, desconexiones, dejando o unirse a grupos y así sucesivamente.</span><span class="sxs-lookup"><span data-stu-id="63c39-116">There are also some control messages for connections, disconnections, joining or leaving groups, and so forth.</span></span> <span data-ttu-id="63c39-117">En la mayoría de las aplicaciones, la mayoría del tráfico de mensajes serán las invocaciones de método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="63c39-117">In most applications, the majority of the message traffic will be hub method invocations.</span></span>

## <a name="overview"></a><span data-ttu-id="63c39-118">Información general</span><span class="sxs-lookup"><span data-stu-id="63c39-118">Overview</span></span>

<span data-ttu-id="63c39-119">Antes de entrar en el tutorial detallado, le presentamos una introducción rápida de lo que hará.</span><span class="sxs-lookup"><span data-stu-id="63c39-119">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="63c39-120">Usar el portal de Windows Azure para crear un nuevo espacio de nombres de Service Bus.</span><span class="sxs-lookup"><span data-stu-id="63c39-120">Use the Windows Azure portal to create a new Service Bus namespace.</span></span>
2. <span data-ttu-id="63c39-121">Agregue estos paquetes de NuGet para la aplicación:</span><span class="sxs-lookup"><span data-stu-id="63c39-121">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="63c39-122">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="63c39-122">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="63c39-123">Microsoft.AspNet.SignalR.ServiceBus</span><span class="sxs-lookup"><span data-stu-id="63c39-123">Microsoft.AspNet.SignalR.ServiceBus</span></span>](http://www.nuget.org/packages/SignalR.WindowsAzureServiceBus)
3. <span data-ttu-id="63c39-124">Cree una aplicación de SignalR.</span><span class="sxs-lookup"><span data-stu-id="63c39-124">Create a SignalR application.</span></span>
4. <span data-ttu-id="63c39-125">Agregue el código siguiente en Global.asax para configurar la placa posterior:</span><span class="sxs-lookup"><span data-stu-id="63c39-125">Add the following code to Global.asax to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

<span data-ttu-id="63c39-126">Para cada aplicación, elija un valor diferente para "YourAppName".</span><span class="sxs-lookup"><span data-stu-id="63c39-126">For each application, pick a different value for "YourAppName".</span></span> <span data-ttu-id="63c39-127">No use el mismo valor en varias aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="63c39-127">Do not use the same value across multiple applications.</span></span>

## <a name="create-the-azure-services"></a><span data-ttu-id="63c39-128">Crear los servicios de Azure</span><span class="sxs-lookup"><span data-stu-id="63c39-128">Create the Azure Services</span></span>

<span data-ttu-id="63c39-129">Crear un servicio en la nube, como se describe en [cómo crear e implementar un servicio de nube](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span><span class="sxs-lookup"><span data-stu-id="63c39-129">Create a Cloud Service, as described in [How to Create and Deploy a Cloud Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span></span> <span data-ttu-id="63c39-130">Siga los pasos descritos en la sección "Cómo: crear un servicio de nube mediante Creación rápida".</span><span class="sxs-lookup"><span data-stu-id="63c39-130">Follow the steps in the section "How to: Create a cloud service using Quick Create".</span></span> <span data-ttu-id="63c39-131">Para este tutorial, no es necesario cargar un certificado.</span><span class="sxs-lookup"><span data-stu-id="63c39-131">For this tutorial, you do not need to upload a certificate.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image2.png)

<span data-ttu-id="63c39-132">Crear un nuevo espacio de nombres de Service Bus, como se describe en [cómo a usar temas/suscripciones de Service Bus](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span><span class="sxs-lookup"><span data-stu-id="63c39-132">Create a new Service Bus namespace, as described in [How to Use Service Bus Topics/Subscriptions](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span></span> <span data-ttu-id="63c39-133">Siga los pasos descritos en la sección "Crear unas Namespace de servicio".</span><span class="sxs-lookup"><span data-stu-id="63c39-133">Follow the steps in the section "Create a Service Namespace".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="63c39-134">Asegúrese de seleccionar la misma región para el servicio en la nube y el espacio de nombres de Service Bus.</span><span class="sxs-lookup"><span data-stu-id="63c39-134">Make sure to select the same region for the cloud service and the Service Bus namespace.</span></span>


## <a name="create-the-visual-studio-project"></a><span data-ttu-id="63c39-135">Crear el proyecto de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="63c39-135">Create the Visual Studio Project</span></span>

<span data-ttu-id="63c39-136">Inicie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="63c39-136">Start Visual Studio.</span></span> <span data-ttu-id="63c39-137">Desde el **archivo** menú, haga clic en **nuevo proyecto**.</span><span class="sxs-lookup"><span data-stu-id="63c39-137">From the **File** menu, click **New Project**.</span></span>

<span data-ttu-id="63c39-138">En el **nuevo proyecto** cuadro de diálogo, expanda **Visual C#**.</span><span class="sxs-lookup"><span data-stu-id="63c39-138">In the **New Project** dialog box, expand **Visual C#**.</span></span> <span data-ttu-id="63c39-139">En **plantillas instaladas**, seleccione **en la nube** y, a continuación, seleccione **Windows Azure Cloud Service**.</span><span class="sxs-lookup"><span data-stu-id="63c39-139">Under **Installed Templates**, select **Cloud** and then select **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="63c39-140">Mantenga el valor predeterminado de .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="63c39-140">Keep the default .NET Framework 4.5.</span></span> <span data-ttu-id="63c39-141">Nombre de la aplicación ChatService y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="63c39-141">Name the application ChatService and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image4.png)

<span data-ttu-id="63c39-142">En el **nuevo servicio de nube de Windows Azure** cuadro de diálogo, seleccione ASP.NET MVC 4 Web Role.</span><span class="sxs-lookup"><span data-stu-id="63c39-142">In the **New Windows Azure Cloud Service** dialog, select ASP.NET MVC 4 Web Role.</span></span> <span data-ttu-id="63c39-143">Haga clic en el botón de flecha derecha (**&gt;**) para agregar el rol a la solución.</span><span class="sxs-lookup"><span data-stu-id="63c39-143">Click the right-arrow button (**&gt;**) to add the role to your solution.</span></span>

<span data-ttu-id="63c39-144">Mantenga el mouse sobre el nuevo rol, por lo que el icono de lápiz visible.</span><span class="sxs-lookup"><span data-stu-id="63c39-144">Hover the mouse over the new role, so the pencil icon visible.</span></span> <span data-ttu-id="63c39-145">Haga clic en este icono para cambiar el nombre de la función.</span><span class="sxs-lookup"><span data-stu-id="63c39-145">Click this icon to rename the role.</span></span> <span data-ttu-id="63c39-146">Nombre de la función "SignalRChat" y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="63c39-146">Name the role "SignalRChat" and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

<span data-ttu-id="63c39-147">En el **nuevo proyecto de ASP.NET MVC 4** asistente, seleccione **aplicación de Internet**.</span><span class="sxs-lookup"><span data-stu-id="63c39-147">In the **New ASP.NET MVC 4 Project** wizard, select **Internet Application**.</span></span> <span data-ttu-id="63c39-148">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="63c39-148">Click **OK**.</span></span> <span data-ttu-id="63c39-149">El Asistente para proyecto crea dos proyectos:</span><span class="sxs-lookup"><span data-stu-id="63c39-149">The project wizard creates two projects:</span></span>

- <span data-ttu-id="63c39-150">ChatService: Este proyecto es la aplicación de Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="63c39-150">ChatService: This project is the Windows Azure application.</span></span> <span data-ttu-id="63c39-151">Define los roles de Azure y otras opciones de configuración.</span><span class="sxs-lookup"><span data-stu-id="63c39-151">It defines the Azure roles and other configuration options.</span></span>
- <span data-ttu-id="63c39-152">SignalRChat: Este proyecto es el proyecto de ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="63c39-152">SignalRChat: This project is your ASP.NET MVC 4 project.</span></span>

## <a name="create-the-signalr-chat-application"></a><span data-ttu-id="63c39-153">Crear la aplicación de Chat de SignalR</span><span class="sxs-lookup"><span data-stu-id="63c39-153">Create the SignalR Chat Application</span></span>

<span data-ttu-id="63c39-154">Para crear la aplicación de chat, siga los pasos del tutorial [Introducción a SignalR y MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).</span><span class="sxs-lookup"><span data-stu-id="63c39-154">To create the chat application, follow the steps in the tutorial [Getting Started with SignalR and MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).</span></span>

<span data-ttu-id="63c39-155">Use NuGet para instalar las bibliotecas necesarias.</span><span class="sxs-lookup"><span data-stu-id="63c39-155">Use NuGet to install the required libraries.</span></span> <span data-ttu-id="63c39-156">Desde el **herramientas** menú, seleccione **Administrador de paquetes de biblioteca**, a continuación, seleccione **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="63c39-156">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="63c39-157">En el **Package Manager Console** ventana, escriba los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="63c39-157">In the **Package Manager Console** window, enter the following commands:</span></span>

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

<span data-ttu-id="63c39-158">Use el `-ProjectName` opción para instalar los paquetes en el proyecto de ASP.NET MVC, en lugar de en el proyecto de Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="63c39-158">Use the `-ProjectName` option to install the packages to the ASP.NET MVC project, rather than the Windows Azure project.</span></span>

## <a name="configure-the-backplane"></a><span data-ttu-id="63c39-159">Configurar la placa posterior</span><span class="sxs-lookup"><span data-stu-id="63c39-159">Configure the Backplane</span></span>

<span data-ttu-id="63c39-160">En el archivo Global.asax de la aplicación, agregue el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="63c39-160">In your application's Global.asax file, add the following code:</span></span>

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

<span data-ttu-id="63c39-161">Ahora deberá obtener la cadena de conexión del bus de servicio.</span><span class="sxs-lookup"><span data-stu-id="63c39-161">Now you need to get your service bus connection string.</span></span> <span data-ttu-id="63c39-162">En el portal de Azure, seleccione el espacio de nombres del bus de servicio que ha creado y haga clic en el icono de la clave de acceso.</span><span class="sxs-lookup"><span data-stu-id="63c39-162">In the Azure portal, select the service bus namespace that you created and click the Access Key icon.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

<span data-ttu-id="63c39-163">Copie la cadena de conexión en el Portapapeles y, después, péguelo en el *connectionString* variable.</span><span class="sxs-lookup"><span data-stu-id="63c39-163">Copy the connection string to the clipboard, then paste it into the *connectionString* variable.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a><span data-ttu-id="63c39-164">Implementar en Azure</span><span class="sxs-lookup"><span data-stu-id="63c39-164">Deploy to Azure</span></span>

<span data-ttu-id="63c39-165">En el Explorador de soluciones, expanda el **Roles** carpeta dentro del proyecto ChatService.</span><span class="sxs-lookup"><span data-stu-id="63c39-165">In Solution Explorer, expand the **Roles** folder inside the ChatService project.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

<span data-ttu-id="63c39-166">Haga clic en el rol SignalRChat y seleccione **propiedades**.</span><span class="sxs-lookup"><span data-stu-id="63c39-166">Right-click the SignalRChat role and select **Properties**.</span></span> <span data-ttu-id="63c39-167">Seleccione el **configuración** ficha. En **instancias** seleccione 2.</span><span class="sxs-lookup"><span data-stu-id="63c39-167">Select the **Configuration** tab. Under **Instances** select 2.</span></span> <span data-ttu-id="63c39-168">También se puede establecer el tamaño de máquina virtual como **Extra pequeño**.</span><span class="sxs-lookup"><span data-stu-id="63c39-168">You can also set the VM size to **Extra Small**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

<span data-ttu-id="63c39-169">Guarde los cambios.</span><span class="sxs-lookup"><span data-stu-id="63c39-169">Save the changes.</span></span>

<span data-ttu-id="63c39-170">En el Explorador de soluciones, haga clic en el proyecto ChatService.</span><span class="sxs-lookup"><span data-stu-id="63c39-170">In Solution Explorer, right-click the ChatService project.</span></span> <span data-ttu-id="63c39-171">Seleccione **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="63c39-171">Select **Publish**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

<span data-ttu-id="63c39-172">Si se trata de su primera publicación de tiempo en Windows Azure, debe descargar las credenciales.</span><span class="sxs-lookup"><span data-stu-id="63c39-172">If this is your first time publishing to Windows Azure, you must download your credentials.</span></span> <span data-ttu-id="63c39-173">En el **publicar** asistente, haga clic en "Iniciar sesión descargar credenciales".</span><span class="sxs-lookup"><span data-stu-id="63c39-173">In the **Publish** wizard, click "Sign in to download credentials".</span></span> <span data-ttu-id="63c39-174">Esto le pedirá que inicie sesión en el portal de Windows Azure y descargar un archivo de configuración de publicación.</span><span class="sxs-lookup"><span data-stu-id="63c39-174">This will prompt you to sign into the Windows Azure portal and download a publish settings file.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

<span data-ttu-id="63c39-175">Haga clic en **importación** y seleccione el archivo de configuración de publicación que descargó.</span><span class="sxs-lookup"><span data-stu-id="63c39-175">Click **Import** and select the publish settings file that you downloaded.</span></span>

<span data-ttu-id="63c39-176">Haga clic en **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="63c39-176">Click **Next**.</span></span> <span data-ttu-id="63c39-177">En el **configuración de publicación** cuadro de diálogo, en **servicio en la nube**, seleccione el servicio en la nube que creó anteriormente.</span><span class="sxs-lookup"><span data-stu-id="63c39-177">In the **Publish Settings** dialog, under **Cloud Service**, select the cloud service that you created earlier.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

<span data-ttu-id="63c39-178">Haga clic en **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="63c39-178">Click **Publish**.</span></span> <span data-ttu-id="63c39-179">Puede tardar unos minutos en implementar la aplicación e iniciar las máquinas virtuales.</span><span class="sxs-lookup"><span data-stu-id="63c39-179">It can take a few minutes to deploy the application and start the VMs.</span></span>

<span data-ttu-id="63c39-180">Ahora al ejecutar la aplicación de chat, las instancias de rol se comunican a través de Azure Service Bus, con un tema de Service Bus.</span><span class="sxs-lookup"><span data-stu-id="63c39-180">Now when you run the chat application, the role instances communicate through Azure Service Bus, using a Service Bus topic.</span></span> <span data-ttu-id="63c39-181">Un tema es una cola de mensajes que permite que varios suscriptores.</span><span class="sxs-lookup"><span data-stu-id="63c39-181">A topic is a message queue that allows multiple subscribers.</span></span>

<span data-ttu-id="63c39-182">El backplane crea automáticamente el tema y las suscripciones.</span><span class="sxs-lookup"><span data-stu-id="63c39-182">The backplane automatically creates the topic and the subscriptions.</span></span> <span data-ttu-id="63c39-183">Para ver las suscripciones y la actividad de mensaje, abra Azure portal, seleccione el espacio de nombres de Service Bus y haga clic en "Temas".</span><span class="sxs-lookup"><span data-stu-id="63c39-183">To see the subscriptions and message activity, open the Azure portal, select the Service Bus namespace, and click on "Topics".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

<span data-ttu-id="63c39-184">Pueden pasar unos minutos para que se muestran en el panel de la actividad de mensaje.</span><span class="sxs-lookup"><span data-stu-id="63c39-184">It make take a few minutes for the message activity to show up in the dashboard.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

<span data-ttu-id="63c39-185">SignalR administra la duración de tema.</span><span class="sxs-lookup"><span data-stu-id="63c39-185">SignalR manages the topic lifetime.</span></span> <span data-ttu-id="63c39-186">Siempre y cuando se implementa la aplicación, no intente eliminar temas manualmente o cambiar la configuración en el tema.</span><span class="sxs-lookup"><span data-stu-id="63c39-186">As long as your application is deployed, don't try to manually delete topics or change settings on the topic.</span></span>
