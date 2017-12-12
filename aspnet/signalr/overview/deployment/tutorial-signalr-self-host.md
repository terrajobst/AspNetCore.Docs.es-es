---
uid: signalr/overview/deployment/tutorial-signalr-self-host
title: 'Tutorial: SignalR autohospedaje | Documentos de Microsoft'
author: pfletcher
description: "Este tutorial muestra cómo crear un servidor de SignalR 2 hospedado por sí mismo y cómo conectarse a él con un cliente de JavaScript. Versiones de software que se usa en el tutorial V..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 400db427-27af-4f2f-abf0-5486d5e024b5
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/deployment/tutorial-signalr-self-host
msc.type: authoredcontent
ms.openlocfilehash: 997756ff8d48e41da981491d6154f3107ec7a051
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="tutorial-signalr-self-host"></a><span data-ttu-id="bafbd-104">Tutorial: SignalR autohospedaje</span><span class="sxs-lookup"><span data-stu-id="bafbd-104">Tutorial: SignalR Self-Host</span></span>
====================
<span data-ttu-id="bafbd-105">por [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="bafbd-105">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[<span data-ttu-id="bafbd-106">Descargar el proyecto completado</span><span class="sxs-lookup"><span data-stu-id="bafbd-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/SignalR-Self-Host-Sample-6da0f383)

> <span data-ttu-id="bafbd-107">Este tutorial muestra cómo crear un servidor de SignalR 2 hospedado por sí mismo y cómo conectarse a él con un cliente de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="bafbd-107">This tutorial shows how to create a self-hosted SignalR 2 server, and how to connect to it with a JavaScript client.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="bafbd-108">Versiones de software que se usa en el tutorial</span><span class="sxs-lookup"><span data-stu-id="bafbd-108">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="bafbd-109">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="bafbd-109">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="bafbd-110">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="bafbd-110">.NET 4.5</span></span>
> - <span data-ttu-id="bafbd-111">SignalR versión 2</span><span class="sxs-lookup"><span data-stu-id="bafbd-111">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a><span data-ttu-id="bafbd-112">Uso de Visual Studio 2012 con este tutorial</span><span class="sxs-lookup"><span data-stu-id="bafbd-112">Using Visual Studio 2012 with this tutorial</span></span>
> 
> 
> <span data-ttu-id="bafbd-113">Para usar Visual Studio 2012 con este tutorial, realice lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="bafbd-113">To use Visual Studio 2012 with this tutorial, do the following:</span></span>
> 
> - <span data-ttu-id="bafbd-114">Actualización de su [el Administrador de paquetes](http://docs.nuget.org/docs/start-here/installing-nuget) a la versión más reciente.</span><span class="sxs-lookup"><span data-stu-id="bafbd-114">Update your [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) to the latest version.</span></span>
> - <span data-ttu-id="bafbd-115">Instalar el [instalador de plataforma Web](https://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="bafbd-115">Install the [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> - <span data-ttu-id="bafbd-116">El instalador de plataforma Web, buscar e instalar **2013.1 de herramientas Web para Visual Studio 2012 y ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="bafbd-116">In the Web Platform Installer, search for and install **ASP.NET and Web Tools 2013.1 for Visual Studio 2012**.</span></span> <span data-ttu-id="bafbd-117">Esto instalará como plantillas de Visual Studio para las clases de SignalR **concentrador**.</span><span class="sxs-lookup"><span data-stu-id="bafbd-117">This will install Visual Studio templates for SignalR classes such as **Hub**.</span></span>
> - <span data-ttu-id="bafbd-118">Algunas plantillas (como **clase de inicio de OWIN**) no estará disponible; para ello, utilice un archivo de clase en su lugar.</span><span class="sxs-lookup"><span data-stu-id="bafbd-118">Some templates (such as **OWIN Startup Class**) will not be available; for these, use a Class file instead.</span></span>
> 
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="bafbd-119">Preguntas y comentarios</span><span class="sxs-lookup"><span data-stu-id="bafbd-119">Questions and comments</span></span>
> 
> <span data-ttu-id="bafbd-120">Vota sobre cómo le gustó este tutorial y lo que podemos mejorar en los comentarios en la parte inferior de la página.</span><span class="sxs-lookup"><span data-stu-id="bafbd-120">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="bafbd-121">Si tiene preguntas que no están directamente relacionados con el tutorial, puede publicar para la [foro de ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="bafbd-121">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="bafbd-122">Información general</span><span class="sxs-lookup"><span data-stu-id="bafbd-122">Overview</span></span>

<span data-ttu-id="bafbd-123">Un servidor de SignalR normalmente se hospeda en una aplicación ASP.NET en IIS, pero también puede ser hospedado por sí mismo (como en una aplicación de consola o el servicio de Windows) mediante la biblioteca de host propio.</span><span class="sxs-lookup"><span data-stu-id="bafbd-123">A SignalR server is usually hosted in an ASP.NET application in IIS, but it can also be self-hosted (such as in a console application or Windows service) using the self-host library.</span></span> <span data-ttu-id="bafbd-124">Esta biblioteca, como ocurre con todas SignalR 2, se basa en OWIN ([interfaz Web abierta para .NET](http://owin.org)).</span><span class="sxs-lookup"><span data-stu-id="bafbd-124">This library, like all of SignalR 2, is built on OWIN ([Open Web Interface for .NET](http://owin.org)).</span></span> <span data-ttu-id="bafbd-125">OWIN define una abstracción entre servidores web de .NET y las aplicaciones web.</span><span class="sxs-lookup"><span data-stu-id="bafbd-125">OWIN defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="bafbd-126">OWIN separa la aplicación web desde el servidor, lo que hace OWIN ideal para el autohospedaje una aplicación web en su propio proceso, fuera de IIS.</span><span class="sxs-lookup"><span data-stu-id="bafbd-126">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS.</span></span>

<span data-ttu-id="bafbd-127">Motivos para no se hospeda en IIS:</span><span class="sxs-lookup"><span data-stu-id="bafbd-127">Reasons for not hosting in IIS include:</span></span>

- <span data-ttu-id="bafbd-128">Entornos donde IIS no está disponible o deseable, por ejemplo, una granja de servidores existente sin IIS.</span><span class="sxs-lookup"><span data-stu-id="bafbd-128">Environments where IIS is not available or desirable, such as an existing server farm without IIS.</span></span>
- <span data-ttu-id="bafbd-129">La sobrecarga de rendimiento de IIS debe evitarse.</span><span class="sxs-lookup"><span data-stu-id="bafbd-129">The performance overhead of IIS needs to be avoided.</span></span>
- <span data-ttu-id="bafbd-130">Funcionalidad de SignalR consiste en Agregar a una aplicación existente que se ejecuta en un servicio de Windows, el rol de trabajador de Azure u otro proceso.</span><span class="sxs-lookup"><span data-stu-id="bafbd-130">SignalR functionality is to be added to an exising application that runs in a Windows Service, Azure worker role, or other process.</span></span>

<span data-ttu-id="bafbd-131">Si se está desarrollando una solución como autohospedaje por motivos de rendimiento, se recomienda también prueba la aplicación hospedada en IIS para determinar la ventaja de rendimiento.</span><span class="sxs-lookup"><span data-stu-id="bafbd-131">If a solution is being developed as self-host for performance reasons, it's recommended to also test the application hosted in IIS to determine the performance benefit.</span></span>

<span data-ttu-id="bafbd-132">Este tutorial contiene las siguientes secciones:</span><span class="sxs-lookup"><span data-stu-id="bafbd-132">This tutorial contains the following sections:</span></span>

- [<span data-ttu-id="bafbd-133">Crear el servidor</span><span class="sxs-lookup"><span data-stu-id="bafbd-133">Creating the server</span></span>](#server)
- [<span data-ttu-id="bafbd-134">Tener acceso al servidor con un cliente de JavaScript</span><span class="sxs-lookup"><span data-stu-id="bafbd-134">Accessing the server with a JavaScript client</span></span>](#js)

<a id="server"></a>

## <a name="creating-the-server"></a><span data-ttu-id="bafbd-135">Crear el servidor</span><span class="sxs-lookup"><span data-stu-id="bafbd-135">Creating the server</span></span>

<span data-ttu-id="bafbd-136">En este tutorial, creará un servidor que se hospeda en una aplicación de consola, pero el servidor puede estar hospedado en algún tipo de proceso, como un servicio de Windows o un rol de trabajador de Azure.</span><span class="sxs-lookup"><span data-stu-id="bafbd-136">In this tutorial, you'll create a server that's hosted in a console application, but the server can be hosted in any sort of process, such as a Windows service or Azure worker role.</span></span> <span data-ttu-id="bafbd-137">Para obtener código de ejemplo para hospedar un servidor de SignalR en un servicio de Windows, vea [Self-Hosting SignalR en un servicio de Windows](https://code.msdn.microsoft.com/SignalR-self-hosted-in-6ff7e6c3).</span><span class="sxs-lookup"><span data-stu-id="bafbd-137">For sample code for hosting a SignalR server in a Windows Service, see [Self-Hosting SignalR in a Windows Service](https://code.msdn.microsoft.com/SignalR-self-hosted-in-6ff7e6c3).</span></span>

1. <span data-ttu-id="bafbd-138">Abra Visual Studio 2013 con privilegios de administrador.</span><span class="sxs-lookup"><span data-stu-id="bafbd-138">Open Visual Studio 2013 with administrator privileges.</span></span> <span data-ttu-id="bafbd-139">Seleccione **archivo**, **nuevo proyecto**.</span><span class="sxs-lookup"><span data-stu-id="bafbd-139">Select **File**, **New Project**.</span></span> <span data-ttu-id="bafbd-140">Seleccione **Windows** en el **Visual C#** nodo en el **plantillas** panel y seleccione el **aplicación de consola** plantilla.</span><span class="sxs-lookup"><span data-stu-id="bafbd-140">Select **Windows** under the **Visual C#** node in the **Templates** pane, and select the **Console Application** template.</span></span> <span data-ttu-id="bafbd-141">Asigne al nuevo proyecto "SignalRSelfHost" y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="bafbd-141">Name the new project "SignalRSelfHost" and click **OK**.</span></span>

    ![](tutorial-signalr-self-host/_static/image1.png)
2. <span data-ttu-id="bafbd-142">Abra la consola del Administrador de paquetes de biblioteca seleccionando **herramientas**, **Administrador de paquetes de biblioteca**, **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="bafbd-142">Open the library package manager console by selecting **Tools**, **Library Package Manager**, **Package Manager Console**.</span></span>
3. <span data-ttu-id="bafbd-143">En la consola de administrador de paquetes, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="bafbd-143">In the package manager console, enter the following command:</span></span>

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample1.ps1)]

    <span data-ttu-id="bafbd-144">Este comando agrega las bibliotecas de SignalR 2 Self-Host al proyecto.</span><span class="sxs-lookup"><span data-stu-id="bafbd-144">This command adds the SignalR 2 Self-Host libraries to the project.</span></span>
4. <span data-ttu-id="bafbd-145">En la consola de administrador de paquetes, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="bafbd-145">In the package manager console, enter the following command:</span></span>

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample2.ps1)]

    <span data-ttu-id="bafbd-146">Este comando agrega la biblioteca Microsoft.Owin.Cors al proyecto.</span><span class="sxs-lookup"><span data-stu-id="bafbd-146">This command adds the Microsoft.Owin.Cors library to the project.</span></span> <span data-ttu-id="bafbd-147">Esta biblioteca se usará para la compatibilidad entre dominios, que es necesario para las aplicaciones que hospedan SignalR y un cliente de la página web en dominios diferentes.</span><span class="sxs-lookup"><span data-stu-id="bafbd-147">This library will be used for cross-domain support, which is required for applications that host SignalR and a web page client in different domains.</span></span> <span data-ttu-id="bafbd-148">Puesto que va a hospedar el servidor de SignalR y el cliente web en puertos diferentes, esto significa que entre dominios deben estar habilitada para la comunicación entre estos componentes.</span><span class="sxs-lookup"><span data-stu-id="bafbd-148">Since you'll be hosting the SignalR server and the web client on different ports, this means that cross-domain must be enabled for communication between these components.</span></span>
5. <span data-ttu-id="bafbd-149">Reemplace el contenido de Program.cs por el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="bafbd-149">Replace the contents of Program.cs with the following code.</span></span>

    [!code-csharp[Main](tutorial-signalr-self-host/samples/sample3.cs)]

    <span data-ttu-id="bafbd-150">El código anterior incluye tres clases:</span><span class="sxs-lookup"><span data-stu-id="bafbd-150">The above code includes three classes:</span></span>

    - <span data-ttu-id="bafbd-151">**Programa**, incluido el **Main** definir la ruta de acceso principal de la ejecución del método.</span><span class="sxs-lookup"><span data-stu-id="bafbd-151">**Program**, including the **Main** method defining the primary path of execution.</span></span> <span data-ttu-id="bafbd-152">En este método, una aplicación web de tipo **inicio** se inicia en la dirección URL especificada (`http://localhost:8080`).</span><span class="sxs-lookup"><span data-stu-id="bafbd-152">In this method, a web application of type **Startup** is started at the specified URL (`http://localhost:8080`).</span></span> <span data-ttu-id="bafbd-153">Si es necesaria la seguridad en el punto de conexión, se puede implementar SSL.</span><span class="sxs-lookup"><span data-stu-id="bafbd-153">If security is required on the endpoint, SSL can be implemented.</span></span> <span data-ttu-id="bafbd-154">Vea [Cómo: configurar un puerto con un certificado SSL](https://msdn.microsoft.com/en-us/library/ms733791.aspx) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="bafbd-154">See [How to: Configure a Port with an SSL Certificate](https://msdn.microsoft.com/en-us/library/ms733791.aspx) for more information.</span></span>
    - <span data-ttu-id="bafbd-155">**Inicio**, la clase que contiene la configuración para el servidor de SignalR (la única configuración que utiliza este tutorial es la llamada a `UseCors`) y la llamada a `MapSignalR`, que crea rutas para los objetos de base de datos central en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="bafbd-155">**Startup**, the class containing the configuration for the SignalR server (the only configuration this tutorial uses is the call to `UseCors`), and the call to `MapSignalR`, which creates routes for any Hub objects in the project.</span></span>
    - <span data-ttu-id="bafbd-156">**MyHub**, la clase de concentrador de SignalR que la aplicación proporcionará a los clientes.</span><span class="sxs-lookup"><span data-stu-id="bafbd-156">**MyHub**, the SignalR Hub class that the application will provide to clients.</span></span> <span data-ttu-id="bafbd-157">Esta clase tiene un método único, **enviar**, que los clientes llamarán para difundir un mensaje a todos los demás clientes conectados.</span><span class="sxs-lookup"><span data-stu-id="bafbd-157">This class has a single method, **Send**, that clients will call to broadcast a message to all other connected clients.</span></span>
6. <span data-ttu-id="bafbd-158">Compile y ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="bafbd-158">Compile and run the application.</span></span> <span data-ttu-id="bafbd-159">La dirección que se ejecuta el servidor debe mostrar en una ventana de consola.</span><span class="sxs-lookup"><span data-stu-id="bafbd-159">The address that the server is running should show in a console window.</span></span>

    ![](tutorial-signalr-self-host/_static/image2.png)
7. <span data-ttu-id="bafbd-160">Si se produce un error en la ejecución con la excepción `System.Reflection.TargetInvocationException was unhandled`, debe reiniciar Visual Studio con privilegios de administrador.</span><span class="sxs-lookup"><span data-stu-id="bafbd-160">If execution fails with the exception `System.Reflection.TargetInvocationException was unhandled`, you will need to restart Visual Studio with administrator privileges.</span></span>
8. <span data-ttu-id="bafbd-161">Detener la aplicación antes de continuar con la siguiente sección.</span><span class="sxs-lookup"><span data-stu-id="bafbd-161">Stop the application before proceeding to the next section.</span></span>

<a id="js"></a>

## <a name="accessing-the-server-with-a-javascript-client"></a><span data-ttu-id="bafbd-162">Tener acceso al servidor con un cliente de JavaScript</span><span class="sxs-lookup"><span data-stu-id="bafbd-162">Accessing the server with a JavaScript client</span></span>

<span data-ttu-id="bafbd-163">En esta sección, se usará el mismo cliente de JavaScript desde el [tutorial de introducción](../getting-started/tutorial-getting-started-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="bafbd-163">In this section, you'll use the same JavaScript client from the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md).</span></span> <span data-ttu-id="bafbd-164">Sólo se realizará una modificación en el cliente, que se usa para definir explícitamente la dirección URL del concentrador.</span><span class="sxs-lookup"><span data-stu-id="bafbd-164">We'll only make one modification to the client, which is to explicitly define the hub URL.</span></span> <span data-ttu-id="bafbd-165">Con una aplicación hospedada por sí mismo, el servidor no necesariamente esté en la misma dirección que la dirección URL de conexión (debido a los servidores proxy inversos y equilibradores de carga), por lo que las necesidades de la dirección URL que se defina explícitamente.</span><span class="sxs-lookup"><span data-stu-id="bafbd-165">With a self-hosted application, the server may not necessarily be at the same address as the connection URL (due to reverse proxies and load balancers), so the URL needs to be defined explicitly.</span></span>

1. <span data-ttu-id="bafbd-166">En **el Explorador de soluciones**, haga doble clic en la solución y seleccione **agregar**, **nuevo proyecto**.</span><span class="sxs-lookup"><span data-stu-id="bafbd-166">In **Solution Explorer**, right-click on the solution and select **Add**, **New Project**.</span></span> <span data-ttu-id="bafbd-167">Seleccione el **Web** y seleccione el **aplicación Web ASP.NET** plantilla.</span><span class="sxs-lookup"><span data-stu-id="bafbd-167">Select the **Web** node, and select the **ASP.NET Web Application** template.</span></span> <span data-ttu-id="bafbd-168">Denomine el proyecto "JavascriptClient" y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="bafbd-168">Name the project "JavascriptClient" and click **OK**.</span></span>

    ![](tutorial-signalr-self-host/_static/image3.png)
2. <span data-ttu-id="bafbd-169">Seleccione el **vacía** plantilla y deje que se anula la selección de las opciones restantes.</span><span class="sxs-lookup"><span data-stu-id="bafbd-169">Select the **Empty** template, and leave the remaining options unselected.</span></span> <span data-ttu-id="bafbd-170">Seleccione **crear proyecto**.</span><span class="sxs-lookup"><span data-stu-id="bafbd-170">Select **Create Project**.</span></span>

    ![](tutorial-signalr-self-host/_static/image4.png)
3. <span data-ttu-id="bafbd-171">En la consola de administrador de paquetes, seleccione el proyecto "JavascriptClient" en la **proyecto predeterminado** lista desplegable y ejecute el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="bafbd-171">In the package manager console, select the "JavascriptClient" project in the **Default project** drop-down, and execute the following command:</span></span>

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample4.ps1)]

    <span data-ttu-id="bafbd-172">Este comando instala las bibliotecas de SignalR y JQuery que necesitará en el cliente.</span><span class="sxs-lookup"><span data-stu-id="bafbd-172">This command installs the SignalR and JQuery libraries that you'll need in the client.</span></span>
4. <span data-ttu-id="bafbd-173">Haga doble clic en el proyecto y seleccione **agregar**, **nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="bafbd-173">Right-click on your project and select **Add**, **New Item**.</span></span> <span data-ttu-id="bafbd-174">Seleccione el **Web** nodo y seleccione página HTML.</span><span class="sxs-lookup"><span data-stu-id="bafbd-174">Select the **Web** node, and select HTML Page.</span></span> <span data-ttu-id="bafbd-175">Nombre de la página **Default.html**.</span><span class="sxs-lookup"><span data-stu-id="bafbd-175">Name the page **Default.html**.</span></span>

    ![](tutorial-signalr-self-host/_static/image5.png)
5. <span data-ttu-id="bafbd-176">Reemplace el contenido de la nueva página HTML con el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="bafbd-176">Replace the contents of the new HTML page with the following code.</span></span> <span data-ttu-id="bafbd-177">Compruebe que las referencias de script aquí coinciden con las secuencias de comandos en la carpeta Scripts del proyecto.</span><span class="sxs-lookup"><span data-stu-id="bafbd-177">Verify that the script references here match the scripts in the Scripts folder of the project.</span></span>

    [!code-html[Main](tutorial-signalr-self-host/samples/sample5.html?highlight=31-32)]

    <span data-ttu-id="bafbd-178">El código siguiente (resaltado en el ejemplo de código anterior) es la suma que haya realizado al cliente usado en el tutorial de introducción quedé (además de actualizar el código a la versión beta de SignalR versión 2).</span><span class="sxs-lookup"><span data-stu-id="bafbd-178">The following code (highlighted in the code sample above) is the addition that you've made to the client used in the Getting Stared tutorial (in addition to upgrading the code to SignalR version 2 beta).</span></span> <span data-ttu-id="bafbd-179">Esta línea de código establece explícitamente la dirección URL de conexión de la base de SignalR en el servidor.</span><span class="sxs-lookup"><span data-stu-id="bafbd-179">This line of code explicitly sets the base connection URL for SignalR on the server.</span></span>

    [!code-javascript[Main](tutorial-signalr-self-host/samples/sample6.js)]
6. <span data-ttu-id="bafbd-180">Haga doble clic en la solución y seleccione **Establecer proyectos de inicio...** . Seleccione el **proyectos de inicio múltiples** botón de radio y establezca ambos proyectos **acción** a **iniciar**.</span><span class="sxs-lookup"><span data-stu-id="bafbd-180">Right-click on the solution, and select **Set Startup Projects...**. Select the **Multiple startup projects** radio button, and set both projects' **Action** to **Start**.</span></span>

    ![](tutorial-signalr-self-host/_static/image6.png)
7. <span data-ttu-id="bafbd-181">Haga doble clic en "Default.html" y seleccione **establecer como página principal**.</span><span class="sxs-lookup"><span data-stu-id="bafbd-181">Right-click on "Default.html" and select **Set As Start Page**.</span></span>
8. <span data-ttu-id="bafbd-182">Ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="bafbd-182">Run the application.</span></span> <span data-ttu-id="bafbd-183">Se iniciarán la página y el servidor.</span><span class="sxs-lookup"><span data-stu-id="bafbd-183">The server and page will launch.</span></span> <span data-ttu-id="bafbd-184">Puede que necesite volver a cargar la página web (o seleccione **continuar** en el depurador) si la página se cargue antes de que se inicia el servidor.</span><span class="sxs-lookup"><span data-stu-id="bafbd-184">You may need to reload the web page (or select **Continue** in the debugger) if the page loads before the server is started.</span></span>
9. <span data-ttu-id="bafbd-185">En el explorador, proporcione un nombre de usuario cuando se le solicite.</span><span class="sxs-lookup"><span data-stu-id="bafbd-185">In the browser, provide a username when prompted.</span></span> <span data-ttu-id="bafbd-186">Copiar dirección URL de la página en otra ficha del explorador o ventana y proporcione un nombre de usuario diferente.</span><span class="sxs-lookup"><span data-stu-id="bafbd-186">Copy the page's URL into another browser tab or window and provide a different username.</span></span> <span data-ttu-id="bafbd-187">Podrá enviar mensajes desde el panel del explorador de uno a otro, como se muestra en el tutorial de introducción.</span><span class="sxs-lookup"><span data-stu-id="bafbd-187">You will be able to send messages from one browser pane to the other, as in the Getting Started tutorial.</span></span>
