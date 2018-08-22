---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
title: 'Tutorial: Introducción a SignalR 2 y MVC 5 | Microsoft Docs'
author: pfletcher
description: Este tutorial muestra cómo usar ASP.NET SignalR 2 para crear una aplicación de chat en tiempo real. Agregará SignalR a una aplicación de MVC 5 y crear una vista de chat...
ms.author: riande
ms.date: 06/10/2014
ms.assetid: 80bfe5fb-bdfc-41fe-ac43-2132e5d69fac
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
msc.type: authoredcontent
ms.openlocfilehash: 3fca46ac1e73905063afec9fc1eb9cf8df3aee24
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835403"
---
<a name="tutorial-getting-started-with-signalr-2-and-mvc-5"></a><span data-ttu-id="542e1-104">Tutorial: Introducción a SignalR 2 y MVC 5</span><span class="sxs-lookup"><span data-stu-id="542e1-104">Tutorial: Getting Started with SignalR 2 and MVC 5</span></span>
====================
<span data-ttu-id="542e1-105">por [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span><span class="sxs-lookup"><span data-stu-id="542e1-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span></span>

[<span data-ttu-id="542e1-106">Descargue el proyecto completado</span><span class="sxs-lookup"><span data-stu-id="542e1-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Getting-Started-with-c366b2f3)

> <span data-ttu-id="542e1-107">Este tutorial muestra cómo usar ASP.NET SignalR 2 para crear una aplicación de chat en tiempo real.</span><span class="sxs-lookup"><span data-stu-id="542e1-107">This tutorial shows how to use ASP.NET SignalR 2 to create a real-time chat application.</span></span> <span data-ttu-id="542e1-108">Agregará SignalR a una aplicación de MVC 5 y crear una vista de conversación para enviar y mostrar mensajes.</span><span class="sxs-lookup"><span data-stu-id="542e1-108">You will add SignalR to an MVC 5 application and create a chat view to send and display messages.</span></span> 
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="542e1-109">Versiones de software que se usa en el tutorial</span><span class="sxs-lookup"><span data-stu-id="542e1-109">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="542e1-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="542e1-110">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="542e1-111">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="542e1-111">.NET 4.5</span></span>
> - <span data-ttu-id="542e1-112">MVC 5</span><span class="sxs-lookup"><span data-stu-id="542e1-112">MVC 5</span></span>
> - <span data-ttu-id="542e1-113">Versión 2 de SignalR</span><span class="sxs-lookup"><span data-stu-id="542e1-113">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a><span data-ttu-id="542e1-114">Uso de Visual Studio 2012 con este tutorial</span><span class="sxs-lookup"><span data-stu-id="542e1-114">Using Visual Studio 2012 with this tutorial</span></span>
> 
> 
> <span data-ttu-id="542e1-115">Para usar Visual Studio 2012 con este tutorial, realice lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="542e1-115">To use Visual Studio 2012 with this tutorial, do the following:</span></span>
> 
> - <span data-ttu-id="542e1-116">Actualización de su [Administrador de paquetes](http://docs.nuget.org/docs/start-here/installing-nuget) a la versión más reciente.</span><span class="sxs-lookup"><span data-stu-id="542e1-116">Update your [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) to the latest version.</span></span>
> - <span data-ttu-id="542e1-117">Instalar el [instalador de plataforma Web](https://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="542e1-117">Install the [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> - <span data-ttu-id="542e1-118">En el instalador de plataforma Web, buscar e instalar **ASP.NET y Web Tools 2013.1 para Visual Studio 2012**.</span><span class="sxs-lookup"><span data-stu-id="542e1-118">In the Web Platform Installer, search for and install **ASP.NET and Web Tools 2013.1 for Visual Studio 2012**.</span></span> <span data-ttu-id="542e1-119">Este modo instalará como plantillas de Visual Studio para las clases de SignalR **concentrador**.</span><span class="sxs-lookup"><span data-stu-id="542e1-119">This will install Visual Studio templates for SignalR classes such as **Hub**.</span></span>
> - <span data-ttu-id="542e1-120">Algunas plantillas (como **clase de inicio OWIN**) no estará disponible; para ello, use un archivo de clase en su lugar.</span><span class="sxs-lookup"><span data-stu-id="542e1-120">Some templates (such as **OWIN Startup Class**) will not be available; for these, use a Class file instead.</span></span>
> 
> 
> ## <a name="tutorial-versions"></a><span data-ttu-id="542e1-121">Versiones de tutoriales</span><span class="sxs-lookup"><span data-stu-id="542e1-121">Tutorial Versions</span></span>
> 
> <span data-ttu-id="542e1-122">Para obtener información acerca de las versiones anteriores de SignalR, consulte [versiones anteriores de SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="542e1-122">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="542e1-123">Preguntas y comentarios</span><span class="sxs-lookup"><span data-stu-id="542e1-123">Questions and comments</span></span>
> 
> <span data-ttu-id="542e1-124">Deje comentarios sobre cómo le gustó de este tutorial y que podíamos mejorar en los comentarios en la parte inferior de la página.</span><span class="sxs-lookup"><span data-stu-id="542e1-124">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="542e1-125">Si tiene preguntas que no están directamente relacionados con el tutorial, puede publicarlos en el [foro de ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="542e1-125">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="542e1-126">Información general</span><span class="sxs-lookup"><span data-stu-id="542e1-126">Overview</span></span>

<span data-ttu-id="542e1-127">Este tutorial presentan al desarrollo de aplicaciones web en tiempo real con SignalR 2 de ASP.NET y ASP.NET MVC 5.</span><span class="sxs-lookup"><span data-stu-id="542e1-127">This tutorial introduces you to real-time web application development with ASP.NET SignalR 2 and ASP.NET MVC 5.</span></span> <span data-ttu-id="542e1-128">El tutorial utiliza el mismo código de aplicación de chat como el [tutorial de introducción a SignalR](tutorial-getting-started-with-signalr.md), pero se muestra cómo agregar a una aplicación MVC 5.</span><span class="sxs-lookup"><span data-stu-id="542e1-128">The tutorial uses the same chat application code as the [SignalR Getting Started tutorial](tutorial-getting-started-with-signalr.md), but shows how to add it to an MVC 5 application.</span></span>

<span data-ttu-id="542e1-129">En este tema obtendrá información sobre las siguientes tareas de desarrollo de SignalR:</span><span class="sxs-lookup"><span data-stu-id="542e1-129">In this topic you will learn the following SignalR development tasks:</span></span>

- <span data-ttu-id="542e1-130">Agregar la biblioteca de SignalR a una aplicación de MVC 5.</span><span class="sxs-lookup"><span data-stu-id="542e1-130">Adding the SignalR library to an MVC 5 application.</span></span>
- <span data-ttu-id="542e1-131">Crear clases de inicio OWIN para insertar contenido en los clientes y concentrador.</span><span class="sxs-lookup"><span data-stu-id="542e1-131">Creating hub and OWIN startup classes to push content to clients.</span></span>
- <span data-ttu-id="542e1-132">Uso de la biblioteca de jQuery SignalR en una página web para enviar mensajes y mostrar las actualizaciones desde el centro.</span><span class="sxs-lookup"><span data-stu-id="542e1-132">Using the SignalR jQuery library in a web page to send messages and display updates from the hub.</span></span>

<span data-ttu-id="542e1-133">Captura de pantalla siguiente muestra la aplicación de chat completada que se ejecuta en un explorador.</span><span class="sxs-lookup"><span data-stu-id="542e1-133">The following screen shot shows the completed chat application running in a browser.</span></span>

![Instancias del chat](tutorial-getting-started-with-signalr-and-mvc/_static/image1.png)

<span data-ttu-id="542e1-135">Secciones:</span><span class="sxs-lookup"><span data-stu-id="542e1-135">Sections:</span></span>

- [<span data-ttu-id="542e1-136">Configurar el proyecto</span><span class="sxs-lookup"><span data-stu-id="542e1-136">Set up the Project</span></span>](#setup)
- [<span data-ttu-id="542e1-137">Ejecutar el ejemplo</span><span class="sxs-lookup"><span data-stu-id="542e1-137">Run the Sample</span></span>](#run)
- [<span data-ttu-id="542e1-138">Examine el código</span><span class="sxs-lookup"><span data-stu-id="542e1-138">Examine the Code</span></span>](#code)
- [<span data-ttu-id="542e1-139">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="542e1-139">Next Steps</span></span>](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a><span data-ttu-id="542e1-140">Configurar el proyecto</span><span class="sxs-lookup"><span data-stu-id="542e1-140">Set up the Project</span></span>

<span data-ttu-id="542e1-141">Requisitos previos:</span><span class="sxs-lookup"><span data-stu-id="542e1-141">Prerequisites:</span></span>

- <span data-ttu-id="542e1-142">Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="542e1-142">Visual Studio 2013.</span></span> <span data-ttu-id="542e1-143">Si no tiene Visual Studio, consulte [descargas de ASP.NET](https://www.asp.net/downloads) para obtener el Visual Studio 2013 Express herramienta de desarrollo gratuita.</span><span class="sxs-lookup"><span data-stu-id="542e1-143">If you do not have Visual Studio, see [ASP.NET Downloads](https://www.asp.net/downloads) to get the free Visual Studio 2013 Express Development Tool.</span></span>

<span data-ttu-id="542e1-144">En esta sección se muestra cómo crear una aplicación ASP.NET MVC 5, agregue la biblioteca SignalR y crear la aplicación de chat.</span><span class="sxs-lookup"><span data-stu-id="542e1-144">This section shows how to create an ASP.NET MVC 5 application, add the SignalR library, and create the chat application.</span></span>

1. <span data-ttu-id="542e1-145">En Visual Studio, cree una aplicación ASP.NET de C# que tiene como destino .NET Framework 4.5, asígnele el nombre SignalRChat y haga clic en Aceptar.</span><span class="sxs-lookup"><span data-stu-id="542e1-145">In Visual Studio, create a C# ASP.NET application that targets .NET Framework 4.5, name it SignalRChat, and click OK.</span></span>

    ![Creación de web](tutorial-getting-started-with-signalr-and-mvc/_static/image2.png)
2. <span data-ttu-id="542e1-147">En el `New ASP.NET Project` cuadro de diálogo y seleccione **MVC**y haga clic en **Cambiar autenticación**.</span><span class="sxs-lookup"><span data-stu-id="542e1-147">In the `New ASP.NET Project` dialog, and select **MVC**, and click **Change Authentication**.</span></span>

    ![Creación de web](tutorial-getting-started-with-signalr-and-mvc/_static/image3.png)
3. <span data-ttu-id="542e1-149">Seleccione **sin autenticación** en el **Cambiar autenticación** cuadro de diálogo y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="542e1-149">Select **No Authentication** in the **Change Authentication** dialog, and click **OK**.</span></span>

    ![No seleccione autenticación](tutorial-getting-started-with-signalr-and-mvc/_static/image4.png)

    > [!NOTE]
    > <span data-ttu-id="542e1-151">Si selecciona un proveedor de autenticación diferente para la aplicación, un `Startup.cs` clase se crearán automáticamente; no lo necesitará crear sus propios `Startup.cs` clase en el paso 10 más abajo.</span><span class="sxs-lookup"><span data-stu-id="542e1-151">If you select a different authentication provider for your application, a `Startup.cs` class will be created for you; you will not need to create your own `Startup.cs` class in step 10 below.</span></span>
4. <span data-ttu-id="542e1-152">Haga clic en **Aceptar** en el **nuevo proyecto ASP.NET** cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="542e1-152">Click **OK** in the **New ASP.NET Project** dialog.</span></span>
5. <span data-ttu-id="542e1-153">Abra el **herramientas | Administrador de paquetes de biblioteca | Consola de administrador de paquetes** y ejecute el siguiente comando.</span><span class="sxs-lookup"><span data-stu-id="542e1-153">Open the **Tools | Library Package Manager | Package Manager Console** and run the following command.</span></span> <span data-ttu-id="542e1-154">Este paso agrega al proyecto un conjunto de archivos de script y las referencias de ensamblado que habilitan la funcionalidad de SignalR.</span><span class="sxs-lookup"><span data-stu-id="542e1-154">This step adds to the project a set of script files and assembly references that enable SignalR functionality.</span></span>

    `install-package Microsoft.AspNet.SignalR`
6. <span data-ttu-id="542e1-155">En **el Explorador de soluciones**, expanda la carpeta Scripts.</span><span class="sxs-lookup"><span data-stu-id="542e1-155">In **Solution Explorer**, expand the Scripts folder.</span></span> <span data-ttu-id="542e1-156">Tenga en cuenta que se han agregado al proyecto de bibliotecas de scripts para SignalR.</span><span class="sxs-lookup"><span data-stu-id="542e1-156">Note that script libraries for SignalR have been added to the project.</span></span>

    ![Carpeta de scripts](tutorial-getting-started-with-signalr-and-mvc/_static/image5.png)
7. <span data-ttu-id="542e1-158">En **el Explorador de soluciones**, haga clic en el proyecto, seleccione **Add | Nueva carpeta**, y agregue una nueva carpeta denominada **Hubs**.</span><span class="sxs-lookup"><span data-stu-id="542e1-158">In **Solution Explorer**, right-click the project, select **Add | New Folder**, and add a new folder named **Hubs**.</span></span>
8. <span data-ttu-id="542e1-159">Haga clic en el **Hubs** carpeta, haga clic en **Add | Nuevo elemento**, seleccione el **Visual C# | Web | SignalR** nodo en el **instalado** panel, seleccione **clase de concentrador de SignalR (v2)** desde el panel central y crear un nuevo centro denominado **ChatHub.cs**.</span><span class="sxs-lookup"><span data-stu-id="542e1-159">Right-click the **Hubs** folder, click **Add | New Item**, select the **Visual C# | Web | SignalR** node in the **Installed** pane, select **SignalR Hub Class (v2)** from the center pane, and create a new hub named **ChatHub.cs**.</span></span> <span data-ttu-id="542e1-160">Usará esta clase como un concentrador de servidor de SignalR que envía mensajes a todos los clientes.</span><span class="sxs-lookup"><span data-stu-id="542e1-160">You will use this class as a SignalR server hub that sends messages to all clients.</span></span>

    ![Crear nuevo centro](tutorial-getting-started-with-signalr-and-mvc/_static/image6.png)
9. <span data-ttu-id="542e1-162">Reemplace el código en el **ChatHub** con el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="542e1-162">Replace the code in the **ChatHub** class with the following code.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample1.cs)]
10. <span data-ttu-id="542e1-163">Cree una nueva clase denominada Startup.cs.</span><span class="sxs-lookup"><span data-stu-id="542e1-163">Create a new class called Startup.cs.</span></span> <span data-ttu-id="542e1-164">Cambiar el contenido del archivo a la siguiente.</span><span class="sxs-lookup"><span data-stu-id="542e1-164">Change the contents of the file to the following.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample2.cs)]
11. <span data-ttu-id="542e1-165">Editar el `HomeController` clase se encuentra en **Controllers/HomeController.cs** y agregue el siguiente método a la clase.</span><span class="sxs-lookup"><span data-stu-id="542e1-165">Edit the `HomeController` class found in **Controllers/HomeController.cs** and add the following method to the class.</span></span> <span data-ttu-id="542e1-166">Este método devuelve el **Chat** vista que va a crear en un paso posterior.</span><span class="sxs-lookup"><span data-stu-id="542e1-166">This method returns the **Chat** view that you will create in a later step.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample3.cs)]
12. <span data-ttu-id="542e1-167">Haga clic en el **vistas/inicio** carpeta y seleccione **Add... | Vista**.</span><span class="sxs-lookup"><span data-stu-id="542e1-167">Right-click the **Views/Home** folder, and select **Add... | View**.</span></span>
13. <span data-ttu-id="542e1-168">En el **agregar vista** cuadro de diálogo, nombre de la nueva vista **Chat**.</span><span class="sxs-lookup"><span data-stu-id="542e1-168">In the **Add View** dialog, name the new view **Chat**.</span></span>

    ![Agregar una vista](tutorial-getting-started-with-signalr-and-mvc/_static/image7.png)
14. <span data-ttu-id="542e1-170">Reemplace el contenido de **Chat.cshtml** con el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="542e1-170">Replace the contents of **Chat.cshtml** with the following code.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="542e1-171">Al agregar SignalR y otras bibliotecas de script al proyecto de Visual Studio, el Administrador de paquetes puede instalar una versión del archivo de script de SignalR que es más reciente que la versión que se muestra en este tema.</span><span class="sxs-lookup"><span data-stu-id="542e1-171">When you add SignalR and other script libraries to your Visual Studio project, the Package Manager might install a version of the SignalR script file that is more recent than the version shown in this topic.</span></span> <span data-ttu-id="542e1-172">Asegúrese de que la referencia de script en el código coincide con la versión de la biblioteca de scripts instalada en su proyecto.</span><span class="sxs-lookup"><span data-stu-id="542e1-172">Make sure that the script reference in your code matches the version of the script library installed in your project.</span></span>

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample4.cshtml)]
15. <span data-ttu-id="542e1-173">**Guardar todo** para el proyecto.</span><span class="sxs-lookup"><span data-stu-id="542e1-173">**Save All** for the project.</span></span>

<a id="run"></a>

## <a name="run-the-sample"></a><span data-ttu-id="542e1-174">Ejecutar el ejemplo</span><span class="sxs-lookup"><span data-stu-id="542e1-174">Run the Sample</span></span>

1. <span data-ttu-id="542e1-175">Presione F5 para ejecutar el proyecto en modo de depuración.</span><span class="sxs-lookup"><span data-stu-id="542e1-175">Press F5 to run the project in debug mode.</span></span>
2. <span data-ttu-id="542e1-176">En la línea de dirección del explorador, anexe **/home/chat** a la dirección URL de la página predeterminada para el proyecto.</span><span class="sxs-lookup"><span data-stu-id="542e1-176">In the browser address line, append **/home/chat** to the URL of the default page for the project.</span></span> <span data-ttu-id="542e1-177">La página de Chat se carga en una instancia del explorador y solicitará un nombre de usuario.</span><span class="sxs-lookup"><span data-stu-id="542e1-177">The Chat page loads in a browser instance and prompts for a user name.</span></span>

    ![Escribir nombre de usuario](tutorial-getting-started-with-signalr-and-mvc/_static/image8.png)
3. <span data-ttu-id="542e1-179">Escriba un nombre de usuario.</span><span class="sxs-lookup"><span data-stu-id="542e1-179">Enter a user name.</span></span>
4. <span data-ttu-id="542e1-180">Copie la dirección URL desde la línea de dirección del explorador y usarlo para abrir las instancias del explorador más dos.</span><span class="sxs-lookup"><span data-stu-id="542e1-180">Copy the URL from the address line of the browser and use it to open two more browser instances.</span></span> <span data-ttu-id="542e1-181">En cada instancia del explorador, escriba un nombre de usuario único.</span><span class="sxs-lookup"><span data-stu-id="542e1-181">In each browser instance, enter a unique user name.</span></span>
5. <span data-ttu-id="542e1-182">En cada instancia del explorador, agregue un comentario y haga clic en **enviar**.</span><span class="sxs-lookup"><span data-stu-id="542e1-182">In each browser instance, add a comment and click **Send**.</span></span> <span data-ttu-id="542e1-183">Los comentarios deben aparecer en todas las instancias del explorador.</span><span class="sxs-lookup"><span data-stu-id="542e1-183">The comments should display in all browser instances.</span></span>

    > [!NOTE]
    > <span data-ttu-id="542e1-184">Esta aplicación de chat simple no mantiene el contexto de discusión en el servidor.</span><span class="sxs-lookup"><span data-stu-id="542e1-184">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="542e1-185">El concentrador difunde comentarios a todos los usuarios actuales.</span><span class="sxs-lookup"><span data-stu-id="542e1-185">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="542e1-186">Los usuarios que se unan a la charla más adelante verá mensajes agregados desde el momento en que se unen a.</span><span class="sxs-lookup"><span data-stu-id="542e1-186">Users who join the chat later will see messages added from the time they join.</span></span>
6. <span data-ttu-id="542e1-187">Captura de pantalla siguiente muestra la aplicación de chat que se ejecuta en un explorador.</span><span class="sxs-lookup"><span data-stu-id="542e1-187">The following screen shot shows the chat application running in a browser.</span></span>

    ![Exploradores de chat](tutorial-getting-started-with-signalr-and-mvc/_static/image9.png)
7. <span data-ttu-id="542e1-189">En **el Explorador de soluciones**, inspeccionar la **documentos de Script** nodo para la aplicación en ejecución.</span><span class="sxs-lookup"><span data-stu-id="542e1-189">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="542e1-190">Este nodo está visible en modo de depuración si está usando Internet Explorer como explorador.</span><span class="sxs-lookup"><span data-stu-id="542e1-190">This node is visible in debug mode if you are using Internet Explorer as your browser.</span></span> <span data-ttu-id="542e1-191">Hay un archivo de script denominado **hubs** que la biblioteca SignalR genera dinámicamente en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="542e1-191">There is a script file named **hubs** that the SignalR library dynamically generates at runtime.</span></span> <span data-ttu-id="542e1-192">Este archivo administra la comunicación entre el script de jQuery y el código de servidor.</span><span class="sxs-lookup"><span data-stu-id="542e1-192">This file manages the communication between jQuery script and server-side code.</span></span> <span data-ttu-id="542e1-193">Si usa un explorador que no sea Internet Explorer, también puede acceder a la dinámica **hubs** archivo yendo a él directamente, por ejemplo http://mywebsite/signalr/hubs.</span><span class="sxs-lookup"><span data-stu-id="542e1-193">If you use a browser other than Internet Explorer, you can also access the dynamic **hubs** file by browsing to it directly, for example http://mywebsite/signalr/hubs.</span></span>

<a id="code"></a>

## <a name="examine-the-code"></a><span data-ttu-id="542e1-194">Examine el código</span><span class="sxs-lookup"><span data-stu-id="542e1-194">Examine the Code</span></span>

<span data-ttu-id="542e1-195">La aplicación de chat de SignalR muestra dos tareas básicas de desarrollo de SignalR: crear un centro de que el objeto principal de coordinación en el servidor y mediante la biblioteca de jQuery de SignalR para enviar y recibir mensajes.</span><span class="sxs-lookup"><span data-stu-id="542e1-195">The SignalR chat application demonstrates two basic SignalR development tasks: creating a hub as the main coordination object on the server, and using the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs"></a><span data-ttu-id="542e1-196">Concentradores de SignalR</span><span class="sxs-lookup"><span data-stu-id="542e1-196">SignalR Hubs</span></span>

<span data-ttu-id="542e1-197">En el ejemplo de código la **ChatHub** clase se deriva de la **Microsoft.AspNet.SignalR.Hub** clase.</span><span class="sxs-lookup"><span data-stu-id="542e1-197">In the code sample the **ChatHub** class derives from the **Microsoft.AspNet.SignalR.Hub** class.</span></span> <span data-ttu-id="542e1-198">Derivar de la **concentrador** clase es una forma útil de compilar una aplicación de SignalR.</span><span class="sxs-lookup"><span data-stu-id="542e1-198">Deriving from the **Hub** class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="542e1-199">Puede crear métodos públicos en la clase de concentrador y, a continuación, obtener acceso a esos métodos al llamarlas desde secuencias de comandos en una página web.</span><span class="sxs-lookup"><span data-stu-id="542e1-199">You can create public methods on your hub class and then access those methods by calling them from scripts in a web page.</span></span>

<span data-ttu-id="542e1-200">En el código de chat, llame los clientes la **ChatHub.Send** método para enviar un mensaje nuevo.</span><span class="sxs-lookup"><span data-stu-id="542e1-200">In the chat code, clients call the **ChatHub.Send** method to send a new message.</span></span> <span data-ttu-id="542e1-201">El concentrador a su vez envía el mensaje a todos los clientes mediante una llamada a **Clients.All.addNewMessageToPage**.</span><span class="sxs-lookup"><span data-stu-id="542e1-201">The hub in turn sends the message to all clients by calling **Clients.All.addNewMessageToPage**.</span></span>

<span data-ttu-id="542e1-202">El **enviar** método muestra varios conceptos de hub:</span><span class="sxs-lookup"><span data-stu-id="542e1-202">The **Send** method demonstrates several hub concepts :</span></span>

- <span data-ttu-id="542e1-203">Declarar métodos públicos en un centro de modo que los clientes se les pueden llamar.</span><span class="sxs-lookup"><span data-stu-id="542e1-203">Declare public methods on a hub so that clients can call them.</span></span>
- <span data-ttu-id="542e1-204">Use la **Microsoft.AspNet.SignalR.Hub.Clients** propiedad para tener acceso a todos los clientes conectados a este centro.</span><span class="sxs-lookup"><span data-stu-id="542e1-204">Use the **Microsoft.AspNet.SignalR.Hub.Clients** property to access all clients connected to this hub.</span></span>
- <span data-ttu-id="542e1-205">Llamar a una función en el cliente (como el `addNewMessageToPage` función) para actualizar los clientes.</span><span class="sxs-lookup"><span data-stu-id="542e1-205">Call a function on the client (such as the `addNewMessageToPage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a><span data-ttu-id="542e1-206">SignalR y jQuery</span><span class="sxs-lookup"><span data-stu-id="542e1-206">SignalR and jQuery</span></span>

<span data-ttu-id="542e1-207">El **Chat.cshtml** Ver archivo en el ejemplo de código muestra cómo usar la biblioteca de jQuery SignalR para comunicarse con un concentrador SignalR.</span><span class="sxs-lookup"><span data-stu-id="542e1-207">The **Chat.cshtml** view file in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="542e1-208">Las tareas esenciales en el código están creando una referencia para el proxy generado automáticamente para el centro, declarar una función que el servidor puede llamar para insertar contenido a los clientes y, a partir de una conexión para enviar mensajes al centro.</span><span class="sxs-lookup"><span data-stu-id="542e1-208">The essential tasks in the code are creating a reference to the auto-generated proxy for the hub, declaring a function that the server can call to push content to clients, and starting a connection to send messages to the hub.</span></span>

<span data-ttu-id="542e1-209">El código siguiente declara una referencia a un proxy de concentrador.</span><span class="sxs-lookup"><span data-stu-id="542e1-209">The following code declares a reference to a hub proxy.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample6.js)]

> [!NOTE]
> <span data-ttu-id="542e1-210">En JavaScript, la referencia a la clase de servidor y sus miembros está en mayúsculas y minúsculas.</span><span class="sxs-lookup"><span data-stu-id="542e1-210">In JavaScript the reference to the server class and its members is in camel case.</span></span> <span data-ttu-id="542e1-211">El ejemplo de código hace referencia a C# **ChatHub** clases en JavaScript como **chatHub**.</span><span class="sxs-lookup"><span data-stu-id="542e1-211">The code sample references the C# **ChatHub** class in JavaScript as **chatHub**.</span></span> <span data-ttu-id="542e1-212">Si desea hacer referencia a la `ChatHub` clase en jQuery con grafía Pascal convencional las mayúsculas y minúsculas como lo haría en C#, edite el archivo de clase ChatHub.cs.</span><span class="sxs-lookup"><span data-stu-id="542e1-212">If you want to reference the `ChatHub` class in jQuery with conventional Pascal casing as you would in C#, edit the ChatHub.cs class file.</span></span> <span data-ttu-id="542e1-213">Agregar un `using` instrucción para hacer referencia a la `Microsoft.AspNet.SignalR.Hubs` espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="542e1-213">Add a `using` statement to reference the `Microsoft.AspNet.SignalR.Hubs` namespace.</span></span> <span data-ttu-id="542e1-214">A continuación, agregue el `HubName` atributo a la `ChatHub` clase, por ejemplo `[HubName("ChatHub")]`.</span><span class="sxs-lookup"><span data-stu-id="542e1-214">Then add the `HubName` attribute to the `ChatHub` class, for example `[HubName("ChatHub")]`.</span></span> <span data-ttu-id="542e1-215">Por último, actualice la referencia de jQuery a la `ChatHub` clase.</span><span class="sxs-lookup"><span data-stu-id="542e1-215">Finally, update your jQuery reference to the `ChatHub` class.</span></span>


<span data-ttu-id="542e1-216">El código siguiente muestra cómo crear una función de devolución de llamada en la secuencia de comandos.</span><span class="sxs-lookup"><span data-stu-id="542e1-216">The following code shows how to create a callback function in the script.</span></span> <span data-ttu-id="542e1-217">La clase de hub en el servidor llama a esta función para insertar las actualizaciones de contenido en cada cliente.</span><span class="sxs-lookup"><span data-stu-id="542e1-217">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="542e1-218">La llamada opcional para el `htmlEncode` función se muestra un camino hacia HTML codifica el contenido del mensaje antes de mostrarla en la página, como una manera de evitar la inyección de script.</span><span class="sxs-lookup"><span data-stu-id="542e1-218">The optional call to the `htmlEncode` function shows a way to HTML encode the message content before displaying it in the page, as a way to prevent script injection.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample7.html)]

<span data-ttu-id="542e1-219">El código siguiente muestra cómo abrir una conexión con el centro.</span><span class="sxs-lookup"><span data-stu-id="542e1-219">The following code shows how to open a connection with the hub.</span></span> <span data-ttu-id="542e1-220">El código inicia la conexión y, a continuación, pasa una función para controlar el evento click en la **enviar** botón en la página de Chat.</span><span class="sxs-lookup"><span data-stu-id="542e1-220">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the Chat page.</span></span>

> [!NOTE]
> <span data-ttu-id="542e1-221">Este enfoque garantiza que la conexión se establece antes de que se ejecuta el controlador de eventos.</span><span class="sxs-lookup"><span data-stu-id="542e1-221">This approach ensures that the connection is established before the event handler executes.</span></span>


[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a><span data-ttu-id="542e1-222">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="542e1-222">Next Steps</span></span>

<span data-ttu-id="542e1-223">Ha aprendido que SignalR es un marco para crear aplicaciones web en tiempo real.</span><span class="sxs-lookup"><span data-stu-id="542e1-223">You learned that SignalR is a framework for building real-time web applications.</span></span> <span data-ttu-id="542e1-224">También ha aprendido varias tareas de desarrollo de SignalR: cómo agregar SignalR a una aplicación ASP.NET, cómo crear una clase de hub y cómo enviar y recibir mensajes desde el centro.</span><span class="sxs-lookup"><span data-stu-id="542e1-224">You also learned several SignalR development tasks: how to add SignalR to an ASP.NET application, how to create a hub class, and how to send and receive messages from the hub.</span></span>

<span data-ttu-id="542e1-225">Para ver un tutorial sobre cómo implementar la aplicación de ejemplo SignalR en Azure, consulte [usando SignalR con Web Apps en Azure App Service](../deployment/using-signalr-with-azure-web-sites.md).</span><span class="sxs-lookup"><span data-stu-id="542e1-225">For a walkthrough on how to deploy the sample SignalR application to Azure, see [Using SignalR with Web Apps in Azure App Service](../deployment/using-signalr-with-azure-web-sites.md).</span></span> <span data-ttu-id="542e1-226">Para obtener información detallada sobre cómo implementar un proyecto web de Visual Studio a un sitio Web de Windows Azure, consulte [crear una aplicación web ASP.NET en Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span><span class="sxs-lookup"><span data-stu-id="542e1-226">For detailed information about how to deploy a Visual Studio web project to a Windows Azure Web Site, see [Create an ASP.NET web app in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span></span>

<span data-ttu-id="542e1-227">Para obtener información sobre los conceptos más avanzados de los desarrollos de SignalR, visite los sitios siguientes para recursos y código fuente de SignalR:</span><span class="sxs-lookup"><span data-stu-id="542e1-227">To learn more advanced SignalR developments concepts, visit the following sites for SignalR source code and resources :</span></span>

- [<span data-ttu-id="542e1-228">Proyecto de SignalR</span><span class="sxs-lookup"><span data-stu-id="542e1-228">SignalR Project</span></span>](http://signalr.net)
- [<span data-ttu-id="542e1-229">SignalR Github y ejemplos</span><span class="sxs-lookup"><span data-stu-id="542e1-229">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="542e1-230">SignalR Wiki</span><span class="sxs-lookup"><span data-stu-id="542e1-230">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)
