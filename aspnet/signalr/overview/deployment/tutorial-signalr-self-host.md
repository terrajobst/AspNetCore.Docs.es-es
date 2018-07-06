---
uid: signalr/overview/deployment/tutorial-signalr-self-host
title: 'Tutorial: Interna de SignalR | Microsoft Docs'
author: pfletcher
description: Este tutorial muestra cómo crear un servidor de SignalR 2 alojado en sí mismos y cómo conectarse a ella con un cliente de JavaScript. Versiones de software que se usa en el tutorial V...
ms.author: aspnetcontent
ms.date: 06/10/2014
ms.assetid: 400db427-27af-4f2f-abf0-5486d5e024b5
msc.legacyurl: /signalr/overview/deployment/tutorial-signalr-self-host
msc.type: authoredcontent
ms.openlocfilehash: 71fb121377a49bb741ebff098ff20ec82e85c82a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37821790"
---
<a name="tutorial-signalr-self-host"></a><span data-ttu-id="d17e8-104">Tutorial: Interna de SignalR</span><span class="sxs-lookup"><span data-stu-id="d17e8-104">Tutorial: SignalR Self-Host</span></span>
====================
<span data-ttu-id="d17e8-105">por [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="d17e8-105">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[<span data-ttu-id="d17e8-106">Descargue el proyecto completado</span><span class="sxs-lookup"><span data-stu-id="d17e8-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/SignalR-Self-Host-Sample-6da0f383)

> <span data-ttu-id="d17e8-107">Este tutorial muestra cómo crear un servidor de SignalR 2 alojado en sí mismos y cómo conectarse a ella con un cliente de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d17e8-107">This tutorial shows how to create a self-hosted SignalR 2 server, and how to connect to it with a JavaScript client.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="d17e8-108">Versiones de software que se usa en el tutorial</span><span class="sxs-lookup"><span data-stu-id="d17e8-108">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="d17e8-109">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="d17e8-109">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="d17e8-110">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="d17e8-110">.NET 4.5</span></span>
> - <span data-ttu-id="d17e8-111">Versión 2 de SignalR</span><span class="sxs-lookup"><span data-stu-id="d17e8-111">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a><span data-ttu-id="d17e8-112">Uso de Visual Studio 2012 con este tutorial</span><span class="sxs-lookup"><span data-stu-id="d17e8-112">Using Visual Studio 2012 with this tutorial</span></span>
> 
> 
> <span data-ttu-id="d17e8-113">Para usar Visual Studio 2012 con este tutorial, realice lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="d17e8-113">To use Visual Studio 2012 with this tutorial, do the following:</span></span>
> 
> - <span data-ttu-id="d17e8-114">Actualización de su [Administrador de paquetes](http://docs.nuget.org/docs/start-here/installing-nuget) a la versión más reciente.</span><span class="sxs-lookup"><span data-stu-id="d17e8-114">Update your [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) to the latest version.</span></span>
> - <span data-ttu-id="d17e8-115">Instalar el [instalador de plataforma Web](https://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="d17e8-115">Install the [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> - <span data-ttu-id="d17e8-116">En el instalador de plataforma Web, buscar e instalar **ASP.NET y Web Tools 2013.1 para Visual Studio 2012**.</span><span class="sxs-lookup"><span data-stu-id="d17e8-116">In the Web Platform Installer, search for and install **ASP.NET and Web Tools 2013.1 for Visual Studio 2012**.</span></span> <span data-ttu-id="d17e8-117">Este modo instalará como plantillas de Visual Studio para las clases de SignalR **concentrador**.</span><span class="sxs-lookup"><span data-stu-id="d17e8-117">This will install Visual Studio templates for SignalR classes such as **Hub**.</span></span>
> - <span data-ttu-id="d17e8-118">Algunas plantillas (como **clase de inicio OWIN**) no estará disponible; para ello, use un archivo de clase en su lugar.</span><span class="sxs-lookup"><span data-stu-id="d17e8-118">Some templates (such as **OWIN Startup Class**) will not be available; for these, use a Class file instead.</span></span>
> 
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="d17e8-119">Preguntas y comentarios</span><span class="sxs-lookup"><span data-stu-id="d17e8-119">Questions and comments</span></span>
> 
> <span data-ttu-id="d17e8-120">Deje comentarios sobre cómo le gustó de este tutorial y que podíamos mejorar en los comentarios en la parte inferior de la página.</span><span class="sxs-lookup"><span data-stu-id="d17e8-120">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="d17e8-121">Si tiene preguntas que no están directamente relacionados con el tutorial, puede publicarlos en el [foro de ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="d17e8-121">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="d17e8-122">Información general</span><span class="sxs-lookup"><span data-stu-id="d17e8-122">Overview</span></span>

<span data-ttu-id="d17e8-123">Un servidor de SignalR normalmente se hospeda en una aplicación ASP.NET en IIS, pero también puede ser autohospedado (como en una aplicación de consola o un servicio de Windows) mediante la biblioteca de host propio.</span><span class="sxs-lookup"><span data-stu-id="d17e8-123">A SignalR server is usually hosted in an ASP.NET application in IIS, but it can also be self-hosted (such as in a console application or Windows service) using the self-host library.</span></span> <span data-ttu-id="d17e8-124">Esta biblioteca, al igual que todas SignalR 2, se basa en OWIN ([Open Web Interface para .NET](http://owin.org)).</span><span class="sxs-lookup"><span data-stu-id="d17e8-124">This library, like all of SignalR 2, is built on OWIN ([Open Web Interface for .NET](http://owin.org)).</span></span> <span data-ttu-id="d17e8-125">OWIN define una abstracción entre los servidores web de .NET y aplicaciones web.</span><span class="sxs-lookup"><span data-stu-id="d17e8-125">OWIN defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="d17e8-126">OWIN desacopla la aplicación web desde el servidor, lo que hace que OWIN ideal para el autohospedaje de una aplicación web en su propio proceso, fuera de IIS.</span><span class="sxs-lookup"><span data-stu-id="d17e8-126">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS.</span></span>

<span data-ttu-id="d17e8-127">No se hospeda en IIS razones:</span><span class="sxs-lookup"><span data-stu-id="d17e8-127">Reasons for not hosting in IIS include:</span></span>

- <span data-ttu-id="d17e8-128">Entornos donde IIS no está disponible o deseable, por ejemplo, una granja de servidores existente sin IIS.</span><span class="sxs-lookup"><span data-stu-id="d17e8-128">Environments where IIS is not available or desirable, such as an existing server farm without IIS.</span></span>
- <span data-ttu-id="d17e8-129">Debe evitarse la sobrecarga de rendimiento de IIS.</span><span class="sxs-lookup"><span data-stu-id="d17e8-129">The performance overhead of IIS needs to be avoided.</span></span>
- <span data-ttu-id="d17e8-130">Funcionalidad de SignalR es que se agregarán a una aplicación existente que se ejecuta en un servicio de Windows, el rol de trabajo de Azure u otro proceso.</span><span class="sxs-lookup"><span data-stu-id="d17e8-130">SignalR functionality is to be added to an exising application that runs in a Windows Service, Azure worker role, or other process.</span></span>

<span data-ttu-id="d17e8-131">Si se está desarrollando una solución como autohospedaje por motivos de rendimiento, se recomienda también prueba la aplicación hospedada en IIS para determinar la ventaja de rendimiento.</span><span class="sxs-lookup"><span data-stu-id="d17e8-131">If a solution is being developed as self-host for performance reasons, it's recommended to also test the application hosted in IIS to determine the performance benefit.</span></span>

<span data-ttu-id="d17e8-132">Este tutorial contiene las siguientes secciones:</span><span class="sxs-lookup"><span data-stu-id="d17e8-132">This tutorial contains the following sections:</span></span>

- [<span data-ttu-id="d17e8-133">Crear el servidor</span><span class="sxs-lookup"><span data-stu-id="d17e8-133">Creating the server</span></span>](#server)
- [<span data-ttu-id="d17e8-134">Acceso al servidor con un cliente de JavaScript</span><span class="sxs-lookup"><span data-stu-id="d17e8-134">Accessing the server with a JavaScript client</span></span>](#js)

<a id="server"></a>

## <a name="creating-the-server"></a><span data-ttu-id="d17e8-135">Crear el servidor</span><span class="sxs-lookup"><span data-stu-id="d17e8-135">Creating the server</span></span>

<span data-ttu-id="d17e8-136">En este tutorial, creará un servidor que se hospeda en una aplicación de consola, pero el servidor se puede hospedar en cualquier tipo de proceso, como un servicio de Windows o un rol de trabajo de Azure.</span><span class="sxs-lookup"><span data-stu-id="d17e8-136">In this tutorial, you'll create a server that's hosted in a console application, but the server can be hosted in any sort of process, such as a Windows service or Azure worker role.</span></span> <span data-ttu-id="d17e8-137">Para el código de ejemplo para hospedar un servidor de SignalR en un servicio de Windows, consulte [Self-Hosting SignalR en un servicio de Windows](https://code.msdn.microsoft.com/SignalR-self-hosted-in-6ff7e6c3).</span><span class="sxs-lookup"><span data-stu-id="d17e8-137">For sample code for hosting a SignalR server in a Windows Service, see [Self-Hosting SignalR in a Windows Service](https://code.msdn.microsoft.com/SignalR-self-hosted-in-6ff7e6c3).</span></span>

1. <span data-ttu-id="d17e8-138">Abra Visual Studio 2013 con privilegios de administrador.</span><span class="sxs-lookup"><span data-stu-id="d17e8-138">Open Visual Studio 2013 with administrator privileges.</span></span> <span data-ttu-id="d17e8-139">Seleccione **archivo**, **nuevo proyecto**.</span><span class="sxs-lookup"><span data-stu-id="d17e8-139">Select **File**, **New Project**.</span></span> <span data-ttu-id="d17e8-140">Seleccione **Windows** bajo el **Visual C#** nodo en el **plantillas** panel y seleccione el **aplicación de consola** plantilla.</span><span class="sxs-lookup"><span data-stu-id="d17e8-140">Select **Windows** under the **Visual C#** node in the **Templates** pane, and select the **Console Application** template.</span></span> <span data-ttu-id="d17e8-141">El nuevo proyecto el nombre "SignalRSelfHost" y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="d17e8-141">Name the new project "SignalRSelfHost" and click **OK**.</span></span>

    ![](tutorial-signalr-self-host/_static/image1.png)
2. <span data-ttu-id="d17e8-142">Abra la consola del Administrador de paquetes de biblioteca seleccionando **herramientas**, **Administrador de paquetes de biblioteca**, **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="d17e8-142">Open the library package manager console by selecting **Tools**, **Library Package Manager**, **Package Manager Console**.</span></span>
3. <span data-ttu-id="d17e8-143">En la consola de administrador de paquetes, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="d17e8-143">In the package manager console, enter the following command:</span></span>

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample1.ps1)]

    <span data-ttu-id="d17e8-144">Este comando agrega las bibliotecas de SignalR 2 Self al proyecto.</span><span class="sxs-lookup"><span data-stu-id="d17e8-144">This command adds the SignalR 2 Self-Host libraries to the project.</span></span>
4. <span data-ttu-id="d17e8-145">En la consola de administrador de paquetes, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="d17e8-145">In the package manager console, enter the following command:</span></span>

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample2.ps1)]

    <span data-ttu-id="d17e8-146">Este comando agrega la biblioteca Microsoft.Owin.Cors al proyecto.</span><span class="sxs-lookup"><span data-stu-id="d17e8-146">This command adds the Microsoft.Owin.Cors library to the project.</span></span> <span data-ttu-id="d17e8-147">Esta biblioteca se usará para la compatibilidad entre dominios, que es necesario para las aplicaciones que hospedan SignalR y un cliente de la página web en dominios diferentes.</span><span class="sxs-lookup"><span data-stu-id="d17e8-147">This library will be used for cross-domain support, which is required for applications that host SignalR and a web page client in different domains.</span></span> <span data-ttu-id="d17e8-148">Puesto que se va a hospedar el servidor de SignalR y el cliente web en puertos diferentes, esto significa que entre dominios deben estar habilitada para la comunicación entre estos componentes.</span><span class="sxs-lookup"><span data-stu-id="d17e8-148">Since you'll be hosting the SignalR server and the web client on different ports, this means that cross-domain must be enabled for communication between these components.</span></span>
5. <span data-ttu-id="d17e8-149">Reemplace el contenido de Program.cs por el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="d17e8-149">Replace the contents of Program.cs with the following code.</span></span>

    [!code-csharp[Main](tutorial-signalr-self-host/samples/sample3.cs)]

    <span data-ttu-id="d17e8-150">El código anterior incluye tres clases:</span><span class="sxs-lookup"><span data-stu-id="d17e8-150">The above code includes three classes:</span></span>

    - <span data-ttu-id="d17e8-151">**Programa**, incluido el **Main** definir la ruta de acceso principal de la ejecución del método.</span><span class="sxs-lookup"><span data-stu-id="d17e8-151">**Program**, including the **Main** method defining the primary path of execution.</span></span> <span data-ttu-id="d17e8-152">En este método, una aplicación web de tipo **inicio** se inicia en la dirección URL especificada (`http://localhost:8080`).</span><span class="sxs-lookup"><span data-stu-id="d17e8-152">In this method, a web application of type **Startup** is started at the specified URL (`http://localhost:8080`).</span></span> <span data-ttu-id="d17e8-153">Si se requiere seguridad en el punto de conexión, se puede implementar SSL.</span><span class="sxs-lookup"><span data-stu-id="d17e8-153">If security is required on the endpoint, SSL can be implemented.</span></span> <span data-ttu-id="d17e8-154">Consulte [Cómo: configurar un puerto con un certificado SSL](https://msdn.microsoft.com/library/ms733791.aspx) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="d17e8-154">See [How to: Configure a Port with an SSL Certificate](https://msdn.microsoft.com/library/ms733791.aspx) for more information.</span></span>
    - <span data-ttu-id="d17e8-155">**Inicio**, la clase que contiene la configuración para el servidor de SignalR (la única configuración de este tutorial se usa es la llamada a `UseCors`) y la llamada a `MapSignalR`, lo que crea rutas para los objetos del concentrador en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="d17e8-155">**Startup**, the class containing the configuration for the SignalR server (the only configuration this tutorial uses is the call to `UseCors`), and the call to `MapSignalR`, which creates routes for any Hub objects in the project.</span></span>
    - <span data-ttu-id="d17e8-156">**MyHub**, la clase de concentrador de SignalR que la aplicación proporciona a los clientes.</span><span class="sxs-lookup"><span data-stu-id="d17e8-156">**MyHub**, the SignalR Hub class that the application will provide to clients.</span></span> <span data-ttu-id="d17e8-157">Esta clase tiene un único método, **enviar**, que llamará los clientes para difundir un mensaje a todos los demás clientes conectados.</span><span class="sxs-lookup"><span data-stu-id="d17e8-157">This class has a single method, **Send**, that clients will call to broadcast a message to all other connected clients.</span></span>
6. <span data-ttu-id="d17e8-158">Compile y ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d17e8-158">Compile and run the application.</span></span> <span data-ttu-id="d17e8-159">La dirección que se está ejecutando el servidor debe mostrar en una ventana de consola.</span><span class="sxs-lookup"><span data-stu-id="d17e8-159">The address that the server is running should show in a console window.</span></span>

    ![](tutorial-signalr-self-host/_static/image2.png)
7. <span data-ttu-id="d17e8-160">Si se produce un error de ejecución con la excepción `System.Reflection.TargetInvocationException was unhandled`, deberá reiniciar Visual Studio con privilegios de administrador.</span><span class="sxs-lookup"><span data-stu-id="d17e8-160">If execution fails with the exception `System.Reflection.TargetInvocationException was unhandled`, you will need to restart Visual Studio with administrator privileges.</span></span>
8. <span data-ttu-id="d17e8-161">Detener la aplicación antes de continuar con la siguiente sección.</span><span class="sxs-lookup"><span data-stu-id="d17e8-161">Stop the application before proceeding to the next section.</span></span>

<a id="js"></a>

## <a name="accessing-the-server-with-a-javascript-client"></a><span data-ttu-id="d17e8-162">Acceso al servidor con un cliente de JavaScript</span><span class="sxs-lookup"><span data-stu-id="d17e8-162">Accessing the server with a JavaScript client</span></span>

<span data-ttu-id="d17e8-163">En esta sección, usará el mismo cliente de JavaScript desde el [tutorial de introducción](../getting-started/tutorial-getting-started-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="d17e8-163">In this section, you'll use the same JavaScript client from the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md).</span></span> <span data-ttu-id="d17e8-164">Solo se hará una modificación al cliente, que se usa para definir explícitamente la URL del centro.</span><span class="sxs-lookup"><span data-stu-id="d17e8-164">We'll only make one modification to the client, which is to explicitly define the hub URL.</span></span> <span data-ttu-id="d17e8-165">Con una aplicación autohospedada, el servidor no necesariamente esté en la misma dirección como la dirección URL de conexión (debido a los servidores proxy inversos y equilibradores de carga), por lo que la dirección URL se debe definir explícitamente.</span><span class="sxs-lookup"><span data-stu-id="d17e8-165">With a self-hosted application, the server may not necessarily be at the same address as the connection URL (due to reverse proxies and load balancers), so the URL needs to be defined explicitly.</span></span>

1. <span data-ttu-id="d17e8-166">En **el Explorador de soluciones**, haga doble clic en la solución y seleccione **agregar**, **nuevo proyecto**.</span><span class="sxs-lookup"><span data-stu-id="d17e8-166">In **Solution Explorer**, right-click on the solution and select **Add**, **New Project**.</span></span> <span data-ttu-id="d17e8-167">Seleccione el **Web** nodo y seleccione el **aplicación Web ASP.NET** plantilla.</span><span class="sxs-lookup"><span data-stu-id="d17e8-167">Select the **Web** node, and select the **ASP.NET Web Application** template.</span></span> <span data-ttu-id="d17e8-168">Denomine el proyecto "JavascriptClient" y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="d17e8-168">Name the project "JavascriptClient" and click **OK**.</span></span>

    ![](tutorial-signalr-self-host/_static/image3.png)
2. <span data-ttu-id="d17e8-169">Seleccione el **vacía** plantilla y deje sin selección las opciones restantes.</span><span class="sxs-lookup"><span data-stu-id="d17e8-169">Select the **Empty** template, and leave the remaining options unselected.</span></span> <span data-ttu-id="d17e8-170">Seleccione **crear proyecto**.</span><span class="sxs-lookup"><span data-stu-id="d17e8-170">Select **Create Project**.</span></span>

    ![](tutorial-signalr-self-host/_static/image4.png)
3. <span data-ttu-id="d17e8-171">En la consola de administrador de paquetes, seleccione el proyecto "JavascriptClient" en el **proyecto predeterminado** lista desplegable y ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="d17e8-171">In the package manager console, select the "JavascriptClient" project in the **Default project** drop-down, and execute the following command:</span></span>

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample4.ps1)]

    <span data-ttu-id="d17e8-172">Este comando instala las bibliotecas de SignalR y JQuery que necesitará en el cliente.</span><span class="sxs-lookup"><span data-stu-id="d17e8-172">This command installs the SignalR and JQuery libraries that you'll need in the client.</span></span>
4. <span data-ttu-id="d17e8-173">Haga doble clic en el proyecto y seleccione **agregar**, **nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="d17e8-173">Right-click on your project and select **Add**, **New Item**.</span></span> <span data-ttu-id="d17e8-174">Seleccione el **Web** nodo y seleccionar la página HTML.</span><span class="sxs-lookup"><span data-stu-id="d17e8-174">Select the **Web** node, and select HTML Page.</span></span> <span data-ttu-id="d17e8-175">Nombre de la página **Default.html**.</span><span class="sxs-lookup"><span data-stu-id="d17e8-175">Name the page **Default.html**.</span></span>

    ![](tutorial-signalr-self-host/_static/image5.png)
5. <span data-ttu-id="d17e8-176">Reemplace el contenido de la nueva página HTML con el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="d17e8-176">Replace the contents of the new HTML page with the following code.</span></span> <span data-ttu-id="d17e8-177">Compruebe que las referencias del script coinciden con las secuencias de comandos en la carpeta Scripts del proyecto.</span><span class="sxs-lookup"><span data-stu-id="d17e8-177">Verify that the script references here match the scripts in the Scripts folder of the project.</span></span>

    [!code-html[Main](tutorial-signalr-self-host/samples/sample5.html?highlight=31-32)]

    <span data-ttu-id="d17e8-178">El código siguiente (resaltado en el ejemplo de código anterior) es la suma que ha realizado en el cliente que se usa en el tutorial de introducción al uso (además de actualizar el código a SignalR 2 de la versión beta).</span><span class="sxs-lookup"><span data-stu-id="d17e8-178">The following code (highlighted in the code sample above) is the addition that you've made to the client used in the Getting Stared tutorial (in addition to upgrading the code to SignalR version 2 beta).</span></span> <span data-ttu-id="d17e8-179">Esta línea de código establece explícitamente la dirección URL de conexión de base de SignalR en el servidor.</span><span class="sxs-lookup"><span data-stu-id="d17e8-179">This line of code explicitly sets the base connection URL for SignalR on the server.</span></span>

    [!code-javascript[Main](tutorial-signalr-self-host/samples/sample6.js)]
6. <span data-ttu-id="d17e8-180">Haga doble clic en la solución y seleccione **Establecer proyectos de inicio...** . Seleccione el **varios proyectos de inicio** botón de radio y establezca ambos proyectos **acción** a **iniciar**.</span><span class="sxs-lookup"><span data-stu-id="d17e8-180">Right-click on the solution, and select **Set Startup Projects...**. Select the **Multiple startup projects** radio button, and set both projects' **Action** to **Start**.</span></span>

    ![](tutorial-signalr-self-host/_static/image6.png)
7. <span data-ttu-id="d17e8-181">Haga doble clic en "Default.html" y seleccione **establecer como página principal**.</span><span class="sxs-lookup"><span data-stu-id="d17e8-181">Right-click on "Default.html" and select **Set As Start Page**.</span></span>
8. <span data-ttu-id="d17e8-182">Ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d17e8-182">Run the application.</span></span> <span data-ttu-id="d17e8-183">Se iniciará el servidor y la página.</span><span class="sxs-lookup"><span data-stu-id="d17e8-183">The server and page will launch.</span></span> <span data-ttu-id="d17e8-184">Es posible que deba volver a cargar la página web (o seleccione **continuar** en el depurador) si la página se carga antes de inicia el servidor.</span><span class="sxs-lookup"><span data-stu-id="d17e8-184">You may need to reload the web page (or select **Continue** in the debugger) if the page loads before the server is started.</span></span>
9. <span data-ttu-id="d17e8-185">En el explorador, proporcione un nombre de usuario cuando se le solicite.</span><span class="sxs-lookup"><span data-stu-id="d17e8-185">In the browser, provide a username when prompted.</span></span> <span data-ttu-id="d17e8-186">Copie la dirección URL de la página en otra pestaña del explorador o la ventana y proporcione un nombre de usuario diferente.</span><span class="sxs-lookup"><span data-stu-id="d17e8-186">Copy the page's URL into another browser tab or window and provide a different username.</span></span> <span data-ttu-id="d17e8-187">Podrá enviar mensajes desde el panel de un explorador a otro, como se muestra en el tutorial de introducción.</span><span class="sxs-lookup"><span data-stu-id="d17e8-187">You will be able to send messages from one browser pane to the other, as in the Getting Started tutorial.</span></span>
