---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
title: 'Tutorial: Introducción con SignalR 1.x y MVC 4 | Documentos de Microsoft'
author: pfletcher
description: Usar ASP.NET SignalR y 4 de ASP.NET MVC para compilar una aplicación de chat en tiempo real.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/29/2013
ms.topic: article
ms.assetid: eeef9f73-6de3-49f9-b50b-9af22108f2ce
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 1ae330be5caf00c3cac7451f326398c0958538af
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="tutorial-getting-started-with-signalr-1x-and-mvc-4"></a><span data-ttu-id="fe379-103">Tutorial: Introducción con SignalR 1.x y MVC 4</span><span class="sxs-lookup"><span data-stu-id="fe379-103">Tutorial: Getting Started with SignalR 1.x and MVC 4</span></span>
====================
<span data-ttu-id="fe379-104">por [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span><span class="sxs-lookup"><span data-stu-id="fe379-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span></span>

> <span data-ttu-id="fe379-105">Este tutorial muestra cómo usar ASP.NET SignalR para crear una aplicación de chat en tiempo real.</span><span class="sxs-lookup"><span data-stu-id="fe379-105">This tutorial shows how to use ASP.NET SignalR to create a real-time chat application.</span></span> <span data-ttu-id="fe379-106">Agregará SignalR a una aplicación de MVC 4 y crear una vista de chat para enviar y mostrar mensajes.</span><span class="sxs-lookup"><span data-stu-id="fe379-106">You will add SignalR to an MVC 4 application and create a chat view to send and display messages.</span></span>


## <a name="overview"></a><span data-ttu-id="fe379-107">Información general</span><span class="sxs-lookup"><span data-stu-id="fe379-107">Overview</span></span>

<span data-ttu-id="fe379-108">Este tutorial presentan al desarrollo de aplicaciones web en tiempo real con ASP.NET SignalR y ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="fe379-108">This tutorial introduces you to real-time web application development with ASP.NET SignalR and ASP.NET MVC 4.</span></span> <span data-ttu-id="fe379-109">El tutorial utiliza el mismo código de aplicación de chat que la [tutorial de introducción SignalR](tutorial-getting-started-with-signalr.md), pero muestra cómo agregar a una aplicación de MVC 4 basada en la plantilla de Internet.</span><span class="sxs-lookup"><span data-stu-id="fe379-109">The tutorial uses the same chat application code as the [SignalR Getting Started tutorial](tutorial-getting-started-with-signalr.md), but shows how to add it to an MVC 4 application based on the Internet template.</span></span>

<span data-ttu-id="fe379-110">En este tema aprenderá las tareas de desarrollo de SignalR siguientes:</span><span class="sxs-lookup"><span data-stu-id="fe379-110">In this topic you will learn the following SignalR development tasks:</span></span>

- <span data-ttu-id="fe379-111">Agregar la biblioteca de SignalR a una aplicación de MVC 4.</span><span class="sxs-lookup"><span data-stu-id="fe379-111">Adding the SignalR library to an MVC 4 application.</span></span>
- <span data-ttu-id="fe379-112">Crear una clase de base de datos central para insertar contenido en los clientes.</span><span class="sxs-lookup"><span data-stu-id="fe379-112">Creating a hub class to push content to clients.</span></span>
- <span data-ttu-id="fe379-113">Usa la biblioteca de jQuery de SignalR en una página web para enviar mensajes y muestra las actualizaciones desde el concentrador.</span><span class="sxs-lookup"><span data-stu-id="fe379-113">Using the SignalR jQuery library in a web page to send messages and display updates from the hub.</span></span>

<span data-ttu-id="fe379-114">La captura de pantalla siguiente muestra la aplicación de chat completada que se ejecuta en un explorador.</span><span class="sxs-lookup"><span data-stu-id="fe379-114">The following screen shot shows the completed chat application running in a browser.</span></span>

![Instancias de chat](tutorial-getting-started-with-signalr-and-mvc-4/_static/image2.png)

<span data-ttu-id="fe379-116">Secciones:</span><span class="sxs-lookup"><span data-stu-id="fe379-116">Sections:</span></span>

- [<span data-ttu-id="fe379-117">Configurar el proyecto</span><span class="sxs-lookup"><span data-stu-id="fe379-117">Set up the Project</span></span>](#setup)
- [<span data-ttu-id="fe379-118">Ejecutar el ejemplo</span><span class="sxs-lookup"><span data-stu-id="fe379-118">Run the Sample</span></span>](#run)
- [<span data-ttu-id="fe379-119">Examine el código</span><span class="sxs-lookup"><span data-stu-id="fe379-119">Examine the Code</span></span>](#code)
- [<span data-ttu-id="fe379-120">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="fe379-120">Next Steps</span></span>](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a><span data-ttu-id="fe379-121">Configurar el proyecto</span><span class="sxs-lookup"><span data-stu-id="fe379-121">Set up the Project</span></span>

<span data-ttu-id="fe379-122">Requisitos previos:</span><span class="sxs-lookup"><span data-stu-id="fe379-122">Prerequisites:</span></span>

- <span data-ttu-id="fe379-123">Visual Studio 2010 SP1, Visual Studio 2012 o Visual Studio 2012 Express.</span><span class="sxs-lookup"><span data-stu-id="fe379-123">Visual Studio 2010 SP1, Visual Studio 2012, or Visual Studio 2012 Express.</span></span> <span data-ttu-id="fe379-124">Si no tiene Visual Studio, consulte [descarga ASP.NET](https://www.asp.net/downloads) para obtener la libre Visual Studio 2012 Express herramienta de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="fe379-124">If you do not have Visual Studio, see [ASP.NET Downloads](https://www.asp.net/downloads) to get the free Visual Studio 2012 Express Development Tool.</span></span>
- <span data-ttu-id="fe379-125">Para Visual Studio 2010, instale [ASP.NET MVC 4](https://www.microsoft.com/download/details.aspx?id=30683).</span><span class="sxs-lookup"><span data-stu-id="fe379-125">For Visual Studio 2010, install [ASP.NET MVC 4](https://www.microsoft.com/download/details.aspx?id=30683).</span></span>

<span data-ttu-id="fe379-126">Esta sección muestra cómo crear una aplicación de ASP.NET MVC 4, agregue la biblioteca de SignalR y crear la aplicación de chat.</span><span class="sxs-lookup"><span data-stu-id="fe379-126">This section shows how to create an ASP.NET MVC 4 application, add the SignalR library, and create the chat application.</span></span>

1. 1. <span data-ttu-id="fe379-127">En Visual Studio crea una aplicación de ASP.NET MVC 4, asígnele el nombre SignalRChat y haga clic en Aceptar.</span><span class="sxs-lookup"><span data-stu-id="fe379-127">In Visual Studio create an ASP.NET MVC 4 application, name it SignalRChat, and click OK.</span></span>

        > [!NOTE]
        > <span data-ttu-id="fe379-128">En VS 2010, seleccione **.NET Framework 4** en el control de lista desplegable de versión de Framework.</span><span class="sxs-lookup"><span data-stu-id="fe379-128">In VS 2010, select **.NET Framework 4** in the Framework version dropdown control.</span></span> <span data-ttu-id="fe379-129">Código de SignalR se ejecuta en las versiones de .NET Framework 4 y 4.5.</span><span class="sxs-lookup"><span data-stu-id="fe379-129">SignalR code runs on .NET Framework versions 4 and 4.5.</span></span>

        ![Crear sitio web de mvc](tutorial-getting-started-with-signalr-and-mvc-4/_static/image3.png)
      2. <span data-ttu-id="fe379-131">Seleccione la plantilla de aplicación de Internet, desactive la opción de **crear un proyecto de prueba unitaria**y haga clic en Aceptar.</span><span class="sxs-lookup"><span data-stu-id="fe379-131">Select the Internet Application template, clear the option to **Create a unit test project**, and click OK.</span></span>

         ![Crear el sitio de internet de mvc](tutorial-getting-started-with-signalr-and-mvc-4/_static/image4.png)
      3. <span data-ttu-id="fe379-133">Abra el **herramientas | Administrador de paquetes de biblioteca | Consola de administrador de paquetes** y ejecute el siguiente comando.</span><span class="sxs-lookup"><span data-stu-id="fe379-133">Open the **Tools | Library Package Manager | Package Manager Console** and run the following command.</span></span> <span data-ttu-id="fe379-134">Este paso agrega al proyecto un conjunto de archivos de script y las referencias de ensamblado que habilitar la funcionalidad de SignalR.</span><span class="sxs-lookup"><span data-stu-id="fe379-134">This step adds to the project a set of script files and assembly references that enable SignalR functionality.</span></span>

         `install-package Microsoft.AspNet.SignalR -Version 1.1.3`
      4. <span data-ttu-id="fe379-135">En **el Explorador de soluciones** expanda la carpeta Scripts.</span><span class="sxs-lookup"><span data-stu-id="fe379-135">In **Solution Explorer** expand the Scripts folder.</span></span> <span data-ttu-id="fe379-136">Tenga en cuenta que las bibliotecas de scripts para SignalR se han agregado al proyecto.</span><span class="sxs-lookup"><span data-stu-id="fe379-136">Note that script libraries for SignalR have been added to the project.</span></span>

         ![Referencias de biblioteca](tutorial-getting-started-with-signalr-and-mvc-4/_static/image6.png)
      5. <span data-ttu-id="fe379-138">En **el Explorador de soluciones**, haga clic en el proyecto, seleccione **agregar | Nueva carpeta**, y agrega una carpeta nueva denominada **concentradores**.</span><span class="sxs-lookup"><span data-stu-id="fe379-138">In **Solution Explorer**, right-click the project, select **Add | New Folder**, and add a new folder named **Hubs**.</span></span>
      6. <span data-ttu-id="fe379-139">Haga clic en el **concentradores** carpeta, haga clic en **agregar | Clase**y cree una nueva clase de C# denominada **ChatHub.cs**.</span><span class="sxs-lookup"><span data-stu-id="fe379-139">Right-click the **Hubs** folder, click **Add | Class**, and create a new C# class named **ChatHub.cs**.</span></span> <span data-ttu-id="fe379-140">Usará esta clase como un concentrador de servidor de SignalR que envía mensajes a todos los clientes.</span><span class="sxs-lookup"><span data-stu-id="fe379-140">You will use this class as a SignalR server hub that sends messages to all clients.</span></span>

> [!NOTE]
> <span data-ttu-id="fe379-141">Si usa Visual Studio 2012 y ha instalado el [actualización ASP.NET y Web Tools 2012.2](../../../visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw.md#_Installation), puede usar la nueva plantilla de elemento de SignalR para crear la clase de base de datos central.</span><span class="sxs-lookup"><span data-stu-id="fe379-141">If you use Visual Studio 2012 and have installed the [ASP.NET and Web Tools 2012.2 update](../../../visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw.md#_Installation), you can use the new SignalR item template to create the hub class.</span></span> <span data-ttu-id="fe379-142">Para ello, haga clic en el **concentradores** carpeta, haga clic en **agregar | Nuevo elemento**, seleccione **clase de concentrador de SignalR (v1)**y el nombre de la clase **ChatHub.cs**.</span><span class="sxs-lookup"><span data-stu-id="fe379-142">To do that, right-click the **Hubs** folder, click **Add | New Item**, select **SignalR Hub Class (v1)**, and name the class **ChatHub.cs**.</span></span>


1. <span data-ttu-id="fe379-143">Reemplace el código de la **ChatHub** clase con el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="fe379-143">Replace the code in the **ChatHub** class with the following code.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample1.cs)]
2. <span data-ttu-id="fe379-144">Abra la **Global.asax** de archivos para el proyecto y agregue una llamada al método `RouteTable.Routes.MapHubs();` como la primera línea de código en el `Application_Start` método.</span><span class="sxs-lookup"><span data-stu-id="fe379-144">Open the **Global.asax** file for the project, and add a call to the method `RouteTable.Routes.MapHubs();` as the first line of code in the `Application_Start` method.</span></span> <span data-ttu-id="fe379-145">Este código registra la ruta predeterminada para los concentradores SignalR y se debe llamar antes de registrar las otras rutas.</span><span class="sxs-lookup"><span data-stu-id="fe379-145">This code registers the default route for SignalR hubs and must be called before you register any other routes.</span></span> <span data-ttu-id="fe379-146">Completado `Application_Start` método es similar al ejemplo siguiente.</span><span class="sxs-lookup"><span data-stu-id="fe379-146">The completed `Application_Start` method looks like the following example.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample2.cs)]
3. <span data-ttu-id="fe379-147">Editar la `HomeController` encontrar en la clase **Controllers/HomeController.cs** y agregue el método siguiente a la clase.</span><span class="sxs-lookup"><span data-stu-id="fe379-147">Edit the `HomeController` class found in **Controllers/HomeController.cs** and add the following method to the class.</span></span> <span data-ttu-id="fe379-148">Este método devuelve el **Chat** vista que va a crear en un paso posterior.</span><span class="sxs-lookup"><span data-stu-id="fe379-148">This method returns the **Chat** view that you will create in a later step.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample3.cs)]
4. <span data-ttu-id="fe379-149">Pulse el botón derecho en el `Chat` método que acaba de crea y haga clic en **agregar vista** para crear un nuevo archivo de vista.</span><span class="sxs-lookup"><span data-stu-id="fe379-149">Right-click within the `Chat` method you just created, and click **Add View** to create a new view file.</span></span>
5. <span data-ttu-id="fe379-150">En el **agregar vista** cuadro de diálogo, asegúrese de que está seleccionada la casilla de verificación para **usar una página de diseño o maestra** (desactive las demás casillas de verificación) y, a continuación, haga clic en **agregar**.</span><span class="sxs-lookup"><span data-stu-id="fe379-150">In the **Add View** dialog, make sure the check box is selected to **Use a layout or master page** (clear the other check boxes), and then click **Add**.</span></span>

    ![Agregar una vista](tutorial-getting-started-with-signalr-and-mvc-4/_static/image8.png)
6. <span data-ttu-id="fe379-152">Edite el nuevo archivo de vista denominado **Chat.cshtml**.</span><span class="sxs-lookup"><span data-stu-id="fe379-152">Edit the new view file named **Chat.cshtml**.</span></span> <span data-ttu-id="fe379-153">Después de la &lt;h2&gt; etiqueta, pegue el siguiente &lt;div&gt; sección y `@section scripts` bloque de código en la página.</span><span class="sxs-lookup"><span data-stu-id="fe379-153">After the &lt;h2&gt; tag, paste the following &lt;div&gt; section and `@section scripts` code block into the page.</span></span> <span data-ttu-id="fe379-154">Esta secuencia de comandos permite a la página enviar mensajes de chat y mostrar mensajes desde el servidor.</span><span class="sxs-lookup"><span data-stu-id="fe379-154">This script enables the page to send chat messages and display messages from the server.</span></span> <span data-ttu-id="fe379-155">El código completo de la vista de chat aparece en el siguiente bloque de código.</span><span class="sxs-lookup"><span data-stu-id="fe379-155">The complete code for the chat view appears in the following code block.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="fe379-156">Cuando se agrega SignalR y otras bibliotecas de script a su proyecto de Visual Studio, el Administrador de paquetes puede instalar las versiones de las secuencias de comandos que son más recientes que las versiones que se muestra en este tema.</span><span class="sxs-lookup"><span data-stu-id="fe379-156">When you add SignalR and other script libraries to your Visual Studio project, the Package Manager might install versions of the scripts that are more recent than the versions shown in this topic.</span></span> <span data-ttu-id="fe379-157">Asegúrese de que las referencias de script en el código coinciden con las versiones de las bibliotecas de scripts instaladas en su proyecto.</span><span class="sxs-lookup"><span data-stu-id="fe379-157">Make sure that the script references in your code match the versions of the script libraries installed in your project.</span></span>

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample4.cshtml)]
7. <span data-ttu-id="fe379-158">**Guardar todo** para el proyecto.</span><span class="sxs-lookup"><span data-stu-id="fe379-158">**Save All** for the project.</span></span>

<a id="run"></a>

## <a name="run-the-sample"></a><span data-ttu-id="fe379-159">Ejecutar el ejemplo</span><span class="sxs-lookup"><span data-stu-id="fe379-159">Run the Sample</span></span>

1. <span data-ttu-id="fe379-160">Presione F5 para ejecutar el proyecto en modo de depuración.</span><span class="sxs-lookup"><span data-stu-id="fe379-160">Press F5 to run the project in debug mode.</span></span>
2. <span data-ttu-id="fe379-161">En la línea de dirección del explorador, anexar **/principal/chat** a la dirección URL de la página predeterminada para el proyecto.</span><span class="sxs-lookup"><span data-stu-id="fe379-161">In the browser address line, append **/home/chat** to the URL of the default page for the project.</span></span> <span data-ttu-id="fe379-162">La página de Chat se cargue en una instancia del explorador y solicita un nombre de usuario.</span><span class="sxs-lookup"><span data-stu-id="fe379-162">The Chat page loads in a browser instance and prompts for a user name.</span></span>

    ![Escribir nombre de usuario](tutorial-getting-started-with-signalr-and-mvc-4/_static/image9.png)
3. <span data-ttu-id="fe379-164">Escriba un nombre de usuario.</span><span class="sxs-lookup"><span data-stu-id="fe379-164">Enter a user name.</span></span>
4. <span data-ttu-id="fe379-165">Copie la dirección URL desde la línea de dirección del explorador y usarlo para abrir dos instancias del explorador más.</span><span class="sxs-lookup"><span data-stu-id="fe379-165">Copy the URL from the address line of the browser and use it to open two more browser instances.</span></span> <span data-ttu-id="fe379-166">En cada instancia del explorador, escriba un nombre de usuario único.</span><span class="sxs-lookup"><span data-stu-id="fe379-166">In each browser instance, enter a unique user name.</span></span>
5. <span data-ttu-id="fe379-167">En cada instancia del explorador, agregue un comentario y haga clic en **enviar**.</span><span class="sxs-lookup"><span data-stu-id="fe379-167">In each browser instance, add a comment and click **Send**.</span></span> <span data-ttu-id="fe379-168">Los comentarios deben aparecer en todas las instancias del explorador.</span><span class="sxs-lookup"><span data-stu-id="fe379-168">The comments should display in all browser instances.</span></span>

    > [!NOTE]
    > <span data-ttu-id="fe379-169">Esta aplicación de chat simple no mantiene el contexto de discusión en el servidor.</span><span class="sxs-lookup"><span data-stu-id="fe379-169">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="fe379-170">El concentrador difunde los comentarios de todos los usuarios actuales.</span><span class="sxs-lookup"><span data-stu-id="fe379-170">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="fe379-171">Los usuarios que se unan a la conversación más adelante verá mensajes agregados desde el momento en que se unan.</span><span class="sxs-lookup"><span data-stu-id="fe379-171">Users who join the chat later will see messages added from the time they join.</span></span>
6. <span data-ttu-id="fe379-172">La captura de pantalla siguiente muestra la aplicación de chat que se ejecuta en un explorador.</span><span class="sxs-lookup"><span data-stu-id="fe379-172">The following screen shot shows the chat application running in a browser.</span></span>

    ![Exploradores de chat](tutorial-getting-started-with-signalr-and-mvc-4/_static/image11.png)
7. <span data-ttu-id="fe379-174">En **el Explorador de soluciones**, inspeccionar la **documentos de Script** nodo para la aplicación en ejecución.</span><span class="sxs-lookup"><span data-stu-id="fe379-174">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="fe379-175">Este nodo está visible en modo de depuración si está usando Internet Explorer como explorador.</span><span class="sxs-lookup"><span data-stu-id="fe379-175">This node is visible in debug mode if you are using Internet Explorer as your browser.</span></span> <span data-ttu-id="fe379-176">Hay un archivo de script denominado **concentradores** que la biblioteca de SignalR genera dinámicamente en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="fe379-176">There is a script file named **hubs** that the SignalR library dynamically generates at runtime.</span></span> <span data-ttu-id="fe379-177">Este archivo administra la comunicación entre el código de servidor y los scripts de jQuery.</span><span class="sxs-lookup"><span data-stu-id="fe379-177">This file manages the communication between jQuery script and server-side code.</span></span> <span data-ttu-id="fe379-178">Si utiliza un explorador que no sea Internet Explorer, también puede tener acceso a los dinámicos **concentradores** desplazándose hasta ella directamente, por ejemplo el archivo http://mywebsite/signalr/hubs.</span><span class="sxs-lookup"><span data-stu-id="fe379-178">If you use a browser other than Internet Explorer, you can also access the dynamic **hubs** file by browsing to it directly, for example http://mywebsite/signalr/hubs.</span></span>

    ![Script de concentrador generado](tutorial-getting-started-with-signalr-and-mvc-4/_static/image13.png)

<a id="code"></a>

## <a name="examine-the-code"></a><span data-ttu-id="fe379-180">Examine el código</span><span class="sxs-lookup"><span data-stu-id="fe379-180">Examine the Code</span></span>

<span data-ttu-id="fe379-181">La aplicación de chat de SignalR muestra dos tareas básicas de desarrollo de SignalR: creando un centro de que el objeto de coordinación principal en el servidor y utiliza la biblioteca de jQuery de SignalR para enviar y recibir mensajes.</span><span class="sxs-lookup"><span data-stu-id="fe379-181">The SignalR chat application demonstrates two basic SignalR development tasks: creating a hub as the main coordination object on the server, and using the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs"></a><span data-ttu-id="fe379-182">Concentradores de SignalR</span><span class="sxs-lookup"><span data-stu-id="fe379-182">SignalR Hubs</span></span>

<span data-ttu-id="fe379-183">En el ejemplo de código la **ChatHub** clase se deriva de la **Microsoft.AspNet.SignalR.Hub** clase.</span><span class="sxs-lookup"><span data-stu-id="fe379-183">In the code sample the **ChatHub** class derives from the **Microsoft.AspNet.SignalR.Hub** class.</span></span> <span data-ttu-id="fe379-184">Derivar de la **concentrador** clase es una forma útil de compilar una aplicación de SignalR.</span><span class="sxs-lookup"><span data-stu-id="fe379-184">Deriving from the **Hub** class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="fe379-185">Puede crear métodos públicos en la clase de base de datos central y, a continuación, obtener acceso a estos métodos al llamarlas desde scripts de jQuery en una página web.</span><span class="sxs-lookup"><span data-stu-id="fe379-185">You can create public methods on your hub class and then access those methods by calling them from jQuery scripts in a web page.</span></span>

<span data-ttu-id="fe379-186">En el código de chat, llame los clientes la **ChatHub.Send** método para enviar un mensaje nuevo.</span><span class="sxs-lookup"><span data-stu-id="fe379-186">In the chat code, clients call the **ChatHub.Send** method to send a new message.</span></span> <span data-ttu-id="fe379-187">El concentrador a su vez envía el mensaje a todos los clientes mediante una llamada a **Clients.All.addNewMessageToPage**.</span><span class="sxs-lookup"><span data-stu-id="fe379-187">The hub in turn sends the message to all clients by calling **Clients.All.addNewMessageToPage**.</span></span>

<span data-ttu-id="fe379-188">El **enviar** método muestra varios conceptos de base de datos central:</span><span class="sxs-lookup"><span data-stu-id="fe379-188">The **Send** method demonstrates several hub concepts :</span></span>

- <span data-ttu-id="fe379-189">Declarar métodos públicos en un centro de modo que los clientes se les pueden llamar.</span><span class="sxs-lookup"><span data-stu-id="fe379-189">Declare public methods on a hub so that clients can call them.</span></span>
- <span data-ttu-id="fe379-190">Use la **Microsoft.AspNet.SignalR.Hub.Clients** propiedad para tener acceso a todos los clientes conectados a este concentrador.</span><span class="sxs-lookup"><span data-stu-id="fe379-190">Use the **Microsoft.AspNet.SignalR.Hub.Clients** property to access all clients connected to this hub.</span></span>
- <span data-ttu-id="fe379-191">Llamar a una función de jQuery en el cliente (como el `addNewMessageToPage` función) para actualizar los clientes.</span><span class="sxs-lookup"><span data-stu-id="fe379-191">Call a jQuery function on the client (such as the `addNewMessageToPage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a><span data-ttu-id="fe379-192">SignalR y jQuery</span><span class="sxs-lookup"><span data-stu-id="fe379-192">SignalR and jQuery</span></span>

<span data-ttu-id="fe379-193">El **Chat.cshtml** archivo de vista en el ejemplo de código muestra cómo utilizar la biblioteca de jQuery SignalR para comunicarse con un concentrador de SignalR.</span><span class="sxs-lookup"><span data-stu-id="fe379-193">The **Chat.cshtml** view file in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="fe379-194">Las tareas esenciales en el código va a crear una referencia para el proxy generado automáticamente para el concentrador, declarar una función que el servidor puede llamar para contenido de inserción a los clientes y, a partir de una conexión para enviar mensajes al concentrador.</span><span class="sxs-lookup"><span data-stu-id="fe379-194">The essential tasks in the code are creating a reference to the auto-generated proxy for the hub, declaring a function that the server can call to push content to clients, and starting a connection to send messages to the hub.</span></span>

<span data-ttu-id="fe379-195">El código siguiente declara a un proxy para un concentrador.</span><span class="sxs-lookup"><span data-stu-id="fe379-195">The following code declares a proxy for a hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample6.js)]

> [!NOTE]
> <span data-ttu-id="fe379-196">En jQuery la referencia a la clase de servidor y sus miembros está en mayúsculas y minúsculas camel.</span><span class="sxs-lookup"><span data-stu-id="fe379-196">In jQuery the reference to the server class and its members is in camel case.</span></span> <span data-ttu-id="fe379-197">El ejemplo de código hace referencia a C# **ChatHub** clase jQuery como **chatHub**.</span><span class="sxs-lookup"><span data-stu-id="fe379-197">The code sample references the C# **ChatHub** class in jQuery as **chatHub**.</span></span> <span data-ttu-id="fe379-198">Si desea hacer referencia a la `ChatHub` clase en jQuery con Pascal convencional mayúsculas y minúsculas como lo haría en C#, modifique el archivo de clase ChatHub.cs.</span><span class="sxs-lookup"><span data-stu-id="fe379-198">If you want to reference the `ChatHub` class in jQuery with conventional Pascal casing as you would in C#, edit the ChatHub.cs class file.</span></span> <span data-ttu-id="fe379-199">Agregar un `using` instrucción para hacer referencia a la `Microsoft.AspNet.SignalR.Hubs` espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="fe379-199">Add a `using` statement to reference the `Microsoft.AspNet.SignalR.Hubs` namespace.</span></span> <span data-ttu-id="fe379-200">A continuación, agregue el `HubName` atribuir a la `ChatHub` de la clase, por ejemplo `[HubName("ChatHub")]`.</span><span class="sxs-lookup"><span data-stu-id="fe379-200">Then add the `HubName` attribute to the `ChatHub` class, for example `[HubName("ChatHub")]`.</span></span> <span data-ttu-id="fe379-201">Por último, actualizar la referencia de jQuery para la `ChatHub` clase.</span><span class="sxs-lookup"><span data-stu-id="fe379-201">Finally, update your jQuery reference to the `ChatHub` class.</span></span>


<span data-ttu-id="fe379-202">El código siguiente muestra cómo crear una función de devolución de llamada en la secuencia de comandos.</span><span class="sxs-lookup"><span data-stu-id="fe379-202">The following code shows how to create a callback function in the script.</span></span> <span data-ttu-id="fe379-203">La clase de base de datos central en el servidor llama a esta función para insertar las actualizaciones de contenido en cada cliente.</span><span class="sxs-lookup"><span data-stu-id="fe379-203">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="fe379-204">La llamada opcional a la `htmlEncode` función muestra una manera de HTML codifica el contenido del mensaje antes de mostrarlo en la página, como una manera de evitar la inyección de script.</span><span class="sxs-lookup"><span data-stu-id="fe379-204">The optional call to the `htmlEncode` function shows a way to HTML encode the message content before displaying it in the page, as a way to prevent script injection.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample7.html)]

<span data-ttu-id="fe379-205">El código siguiente muestra cómo abrir una conexión con el centro.</span><span class="sxs-lookup"><span data-stu-id="fe379-205">The following code shows how to open a connection with the hub.</span></span> <span data-ttu-id="fe379-206">El código inicia la conexión y, a continuación, pasa una función para controlar el evento click en la **enviar** botón en la página de Chat.</span><span class="sxs-lookup"><span data-stu-id="fe379-206">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the Chat page.</span></span>

> [!NOTE]
> <span data-ttu-id="fe379-207">Este enfoque garantiza que la conexión se establece antes de que se ejecuta el controlador de eventos.</span><span class="sxs-lookup"><span data-stu-id="fe379-207">This approach ensures that the connection is established before the event handler executes.</span></span>


[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a><span data-ttu-id="fe379-208">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="fe379-208">Next Steps</span></span>

<span data-ttu-id="fe379-209">Ha aprendido que SignalR es un marco para generar aplicaciones web en tiempo real.</span><span class="sxs-lookup"><span data-stu-id="fe379-209">You learned that SignalR is a framework for building real-time web applications.</span></span> <span data-ttu-id="fe379-210">También ha aprendido a varias tareas de desarrollo de SignalR: cómo agregar SignalR a una aplicación de ASP.NET, cómo crear una clase de base de datos central y cómo enviar y recibir mensajes desde el concentrador.</span><span class="sxs-lookup"><span data-stu-id="fe379-210">You also learned several SignalR development tasks: how to add SignalR to an ASP.NET application, how to create a hub class, and how to send and receive messages from the hub.</span></span>

<span data-ttu-id="fe379-211">Para obtener información sobre conceptos de los desarrollos de SignalR más avanzados, visite los siguientes sitios de código fuente de SignalR y recursos:</span><span class="sxs-lookup"><span data-stu-id="fe379-211">To learn more advanced SignalR developments concepts, visit the following sites for SignalR source code and resources :</span></span>

- [<span data-ttu-id="fe379-212">Proyecto de SignalR</span><span class="sxs-lookup"><span data-stu-id="fe379-212">SignalR Project</span></span>](http://signalr.net)
- [<span data-ttu-id="fe379-213">SignalR Github y ejemplos</span><span class="sxs-lookup"><span data-stu-id="fe379-213">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="fe379-214">SignalR Wiki</span><span class="sxs-lookup"><span data-stu-id="fe379-214">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)
