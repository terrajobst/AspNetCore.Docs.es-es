---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr
title: "Tutorial: Introducción con SignalR 1.x | Documentos de Microsoft"
author: pfletcher
description: "Use ASP.NET SignalR para compilar una aplicación de chat en tiempo real en una página HTML."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: fdc3599a-5217-44c1-951f-0eec9812dce7
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: c61be6f7a64c000c8d9489f35eea520fd0bb32dd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="tutorial-getting-started-with-signalr-1x"></a><span data-ttu-id="b8560-103">Tutorial: Introducción con SignalR 1.x</span><span class="sxs-lookup"><span data-stu-id="b8560-103">Tutorial: Getting Started with SignalR 1.x</span></span>
====================
<span data-ttu-id="b8560-104">por [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span><span class="sxs-lookup"><span data-stu-id="b8560-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span></span>

> <span data-ttu-id="b8560-105">En este tutorial se muestra cómo usar SignalR para crear una aplicación de chat en tiempo real.</span><span class="sxs-lookup"><span data-stu-id="b8560-105">This tutorial shows how to use SignalR to create a real-time chat application.</span></span> <span data-ttu-id="b8560-106">Agregará SignalR a una aplicación web ASP.NET vacía y cree una página HTML para enviar y mostrar mensajes.</span><span class="sxs-lookup"><span data-stu-id="b8560-106">You will add SignalR to an empty ASP.NET web application and create an HTML page to send and display messages.</span></span>


## <a name="overview"></a><span data-ttu-id="b8560-107">Información general</span><span class="sxs-lookup"><span data-stu-id="b8560-107">Overview</span></span>

<span data-ttu-id="b8560-108">Este tutorial presentan SignalR desarrollo mostrándole cómo crear una aplicación de chat simple basada en explorador.</span><span class="sxs-lookup"><span data-stu-id="b8560-108">This tutorial introduces SignalR development by showing how to build a simple browser-based chat application.</span></span> <span data-ttu-id="b8560-109">Agregará la biblioteca de SignalR a una aplicación web ASP.NET vacía, cree una clase de base de datos central para enviar mensajes a los clientes y crear una página HTML que permite a los usuarios enviar y recibir mensajes de chat.</span><span class="sxs-lookup"><span data-stu-id="b8560-109">You will add the SignalR library to an empty ASP.NET web application, create a hub class for sending messages to clients, and create an HTML page that lets users send and receive chat messages.</span></span> <span data-ttu-id="b8560-110">Para obtener un tutorial similar que muestra cómo crear una aplicación de chat en MVC 4 mediante una vista de MVC, vea [Getting Started with SignalR y MVC 4](index.md).</span><span class="sxs-lookup"><span data-stu-id="b8560-110">For a similar tutorial that shows how to create a chat application in MVC 4 using an MVC view, see [Getting Started with SignalR and MVC 4](index.md).</span></span>

> [!NOTE]
> <span data-ttu-id="b8560-111">Este tutorial usa la versión de lanzamiento (1.x) de SignalR.</span><span class="sxs-lookup"><span data-stu-id="b8560-111">This tutorial uses the release (1.x) version of SignalR.</span></span> <span data-ttu-id="b8560-112">Para obtener más información sobre los cambios entre SignalR 1.x y 2.0, consulte [SignalR actualizar 1.x proyectos](../releases/upgrading-signalr-1x-projects-to-20.md).</span><span class="sxs-lookup"><span data-stu-id="b8560-112">For details on changes between SignalR 1.x and 2.0, see [Upgrading SignalR 1.x Projects](../releases/upgrading-signalr-1x-projects-to-20.md).</span></span>

<span data-ttu-id="b8560-113">SignalR es una biblioteca de .NET de código abierto para la creación de aplicaciones web que requieren la interacción del usuario en vivo o las actualizaciones de datos en tiempo real.</span><span class="sxs-lookup"><span data-stu-id="b8560-113">SignalR is an open-source .NET library for building web applications that require live user interaction or real-time data updates.</span></span> <span data-ttu-id="b8560-114">Algunos ejemplos son aplicaciones sociales, juegos multiusuario, el tiempo de colaboración y de noticias, business o aplicaciones de actualización financiera.</span><span class="sxs-lookup"><span data-stu-id="b8560-114">Examples include social applications, multiuser games, business collaboration, and news, weather, or financial update applications.</span></span> <span data-ttu-id="b8560-115">A menudo se denominan aplicaciones en tiempo real.</span><span class="sxs-lookup"><span data-stu-id="b8560-115">These are often called real-time applications.</span></span>

<span data-ttu-id="b8560-116">SignalR simplifica el proceso de creación de aplicaciones en tiempo real.</span><span class="sxs-lookup"><span data-stu-id="b8560-116">SignalR simplifies the process of building real-time applications.</span></span> <span data-ttu-id="b8560-117">Incluye una biblioteca de servidor ASP.NET y una biblioteca de cliente de JavaScript para que resulten más fáciles de administrar las conexiones de cliente y servidor e insertar las actualizaciones de contenido a los clientes.</span><span class="sxs-lookup"><span data-stu-id="b8560-117">It includes an ASP.NET server library and a JavaScript client library to make it easier to manage client-server connections and push content updates to clients.</span></span> <span data-ttu-id="b8560-118">Puede agregar la biblioteca de SignalR a una aplicación ASP.NET existente para obtener una funcionalidad en tiempo real.</span><span class="sxs-lookup"><span data-stu-id="b8560-118">You can add the SignalR library to an existing ASP.NET application to gain real-time functionality.</span></span>

<span data-ttu-id="b8560-119">El tutorial muestra las tareas de desarrollo de SignalR siguientes:</span><span class="sxs-lookup"><span data-stu-id="b8560-119">The tutorial demonstrates the following SignalR development tasks:</span></span>

- <span data-ttu-id="b8560-120">Agregar la biblioteca de SignalR a una aplicación web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="b8560-120">Adding the SignalR library to an ASP.NET web application.</span></span>
- <span data-ttu-id="b8560-121">Crear una clase de base de datos central para insertar contenido en los clientes.</span><span class="sxs-lookup"><span data-stu-id="b8560-121">Creating a hub class to push content to clients.</span></span>
- <span data-ttu-id="b8560-122">Usa la biblioteca de jQuery de SignalR en una página web para enviar mensajes y muestra las actualizaciones desde el concentrador.</span><span class="sxs-lookup"><span data-stu-id="b8560-122">Using the SignalR jQuery library in a web page to send messages and display updates from the hub.</span></span>

<span data-ttu-id="b8560-123">La captura de pantalla siguiente muestra la aplicación de chat que se ejecuta en un explorador.</span><span class="sxs-lookup"><span data-stu-id="b8560-123">The following screen shot shows the chat application running in a browser.</span></span> <span data-ttu-id="b8560-124">Cada nuevo usuario puede registrar comentarios y ver comentarios agregados después de que el usuario une a la conversación.</span><span class="sxs-lookup"><span data-stu-id="b8560-124">Each new user can post comments and see comments added after the user joins the chat.</span></span>

![Instancias de chat](tutorial-getting-started-with-signalr/_static/image1.png)

<span data-ttu-id="b8560-126">Secciones:</span><span class="sxs-lookup"><span data-stu-id="b8560-126">Sections:</span></span>

- [<span data-ttu-id="b8560-127">Configurar el proyecto</span><span class="sxs-lookup"><span data-stu-id="b8560-127">Set up the Project</span></span>](#setup)
- [<span data-ttu-id="b8560-128">Ejecutar el ejemplo</span><span class="sxs-lookup"><span data-stu-id="b8560-128">Run the Sample</span></span>](#run)
- [<span data-ttu-id="b8560-129">Examine el código</span><span class="sxs-lookup"><span data-stu-id="b8560-129">Examine the Code</span></span>](#code)
- [<span data-ttu-id="b8560-130">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="b8560-130">Next Steps</span></span>](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a><span data-ttu-id="b8560-131">Configurar el proyecto</span><span class="sxs-lookup"><span data-stu-id="b8560-131">Set up the Project</span></span>

<span data-ttu-id="b8560-132">Esta sección muestra cómo crear una aplicación web ASP.NET vacía, agregue SignalR y crear la aplicación de chat.</span><span class="sxs-lookup"><span data-stu-id="b8560-132">This section shows how to create an empty ASP.NET web application, add SignalR, and create the chat application.</span></span>

<span data-ttu-id="b8560-133">Requisitos previos:</span><span class="sxs-lookup"><span data-stu-id="b8560-133">Prerequisites:</span></span>

- <span data-ttu-id="b8560-134">Visual Studio 2010 SP1 o 2012.</span><span class="sxs-lookup"><span data-stu-id="b8560-134">Visual Studio 2010 SP1 or 2012.</span></span> <span data-ttu-id="b8560-135">Si no tiene Visual Studio, consulte [descarga ASP.NET](https://www.asp.net/downloads) para obtener la libre Visual Studio 2012 Express herramienta de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="b8560-135">If you do not have Visual Studio, see [ASP.NET Downloads](https://www.asp.net/downloads) to get the free Visual Studio 2012 Express Development Tool.</span></span>
- <span data-ttu-id="b8560-136">[Microsoft Web ASP.NET y herramientas 2012.2](https://go.microsoft.com/fwlink/?LinkId=279941).</span><span class="sxs-lookup"><span data-stu-id="b8560-136">[Microsoft ASP.NET and Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=279941).</span></span> <span data-ttu-id="b8560-137">Para Visual Studio 2012, este instalador agrega nuevas características ASP.NET, incluidas las plantillas de SignalR a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b8560-137">For Visual Studio 2012, this installer adds new ASP.NET features including SignalR templates to Visual Studio.</span></span> <span data-ttu-id="b8560-138">Para Visual Studio 2010 SP1, un instalador no está disponible, pero puede completar el tutorial instalando el paquete SignalR NuGet tal y como se describe en los pasos de configuración.</span><span class="sxs-lookup"><span data-stu-id="b8560-138">For Visual Studio 2010 SP1, an installer is not available but you can complete the tutorial by installing the SignalR NuGet package as described in the setup steps.</span></span>

<span data-ttu-id="b8560-139">Los pasos siguientes utilizan Visual Studio 2012 para crear una aplicación Web ASP.NET vacía y agregar la biblioteca de SignalR:</span><span class="sxs-lookup"><span data-stu-id="b8560-139">The following steps use Visual Studio 2012 to create an ASP.NET Empty Web Application and add the SignalR library:</span></span>

1. <span data-ttu-id="b8560-140">En Visual Studio, cree una aplicación Web ASP.NET vacía.</span><span class="sxs-lookup"><span data-stu-id="b8560-140">In Visual Studio create an ASP.NET Empty Web Application.</span></span>

    ![Crear sitio web vacío](tutorial-getting-started-with-signalr/_static/image2.png)
2. <span data-ttu-id="b8560-142">Abra la **Package Manager Console** seleccionando **herramientas | Administrador de paquetes de biblioteca | Consola de administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="b8560-142">Open the **Package Manager Console** by selecting **Tools | Library Package Manager | Package Manager Console**.</span></span> <span data-ttu-id="b8560-143">Escriba el siguiente comando en la ventana de consola:</span><span class="sxs-lookup"><span data-stu-id="b8560-143">Enter the following command into the console window:</span></span>

    `Install-Package Microsoft.AspNet.SignalR -Version 1.1.3`

    <span data-ttu-id="b8560-144">Este comando instala la versión más reciente de SignalR 1.x.</span><span class="sxs-lookup"><span data-stu-id="b8560-144">This command installs the latest version of SignalR 1.x.</span></span>
3. <span data-ttu-id="b8560-145">En **el Explorador de soluciones**, haga clic en el proyecto, seleccione **agregar | Clase**.</span><span class="sxs-lookup"><span data-stu-id="b8560-145">In **Solution Explorer**, right-click the project, select **Add | Class**.</span></span> <span data-ttu-id="b8560-146">La nueva clase el nombre **ChatHub**.</span><span class="sxs-lookup"><span data-stu-id="b8560-146">Name the new class **ChatHub**.</span></span>
4. <span data-ttu-id="b8560-147">En **el Explorador de soluciones** expanda el nodo de secuencias de comandos.</span><span class="sxs-lookup"><span data-stu-id="b8560-147">In **Solution Explorer** expand the Scripts node.</span></span> <span data-ttu-id="b8560-148">Bibliotecas de scripts de jQuery y SignalR están visibles en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="b8560-148">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    ![Referencias de biblioteca](tutorial-getting-started-with-signalr/_static/image3.png)
5. <span data-ttu-id="b8560-150">Reemplace el código de la **ChatHub** clase con el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="b8560-150">Replace the code in the **ChatHub** class with the following code.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. <span data-ttu-id="b8560-151">En **el Explorador de soluciones**, haga clic en el proyecto, a continuación, haga clic en **agregar | Nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="b8560-151">In **Solution Explorer**, right-click the project, then click **Add | New Item**.</span></span> <span data-ttu-id="b8560-152">En el **Agregar nuevo elemento** cuadro de diálogo, seleccione **clase de aplicación Global** y haga clic en **agregar**.</span><span class="sxs-lookup"><span data-stu-id="b8560-152">In the **Add New Item** dialog, select **Global Application Class** and click **Add**.</span></span>

    ![Agregar global](tutorial-getting-started-with-signalr/_static/image4.png)
7. <span data-ttu-id="b8560-154">Agregue el siguiente `using` instrucciones después proporcionado `using` las instrucciones de la clase Global.asax.cs.</span><span class="sxs-lookup"><span data-stu-id="b8560-154">Add the following `using` statements after the provided `using` statements in the Global.asax.cs class.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. <span data-ttu-id="b8560-155">Agregue la siguiente línea de código en el `Application_Start` método de la clase Global para registrar la ruta predeterminada para los concentradores de SignalR.</span><span class="sxs-lookup"><span data-stu-id="b8560-155">Add the following line of code in the `Application_Start` method of the Global class to register the default route for SignalR hubs.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample3.cs)]
9. <span data-ttu-id="b8560-156">En **el Explorador de soluciones**, haga clic en el proyecto, a continuación, haga clic en **agregar | Nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="b8560-156">In **Solution Explorer**, right-click the project, then click **Add | New Item**.</span></span> <span data-ttu-id="b8560-157">En el **Agregar nuevo elemento** cuadro de diálogo, seleccione la página Html y haga clic en **agregar**.</span><span class="sxs-lookup"><span data-stu-id="b8560-157">In the **Add New Item** dialog, select Html Page and click **Add**.</span></span>
10. <span data-ttu-id="b8560-158">En **el Explorador de soluciones**, haga clic en la página HTML que acaba de crear y haga clic en **establecer como página de inicio**.</span><span class="sxs-lookup"><span data-stu-id="b8560-158">In **Solution Explorer**, right-click the HTML page you just created and click **Set as Start Page**.</span></span>
11. <span data-ttu-id="b8560-159">Reemplace el código predeterminado en la página HTML con el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="b8560-159">Replace the default code in the HTML page with the following code.</span></span>

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample4.html)]
12. <span data-ttu-id="b8560-160">**Guardar todo** para el proyecto.</span><span class="sxs-lookup"><span data-stu-id="b8560-160">**Save All** for the project.</span></span>

<a id="run"></a>

## <a name="run-the-sample"></a><span data-ttu-id="b8560-161">Ejecutar el ejemplo</span><span class="sxs-lookup"><span data-stu-id="b8560-161">Run the Sample</span></span>

1. <span data-ttu-id="b8560-162">Presione F5 para ejecutar el proyecto en modo de depuración.</span><span class="sxs-lookup"><span data-stu-id="b8560-162">Press F5 to run the project in debug mode.</span></span> <span data-ttu-id="b8560-163">La página HTML se carga en una instancia del explorador y solicita un nombre de usuario.</span><span class="sxs-lookup"><span data-stu-id="b8560-163">The HTML page loads in a browser instance and prompts for a user name.</span></span>

    ![Escribir nombre de usuario](tutorial-getting-started-with-signalr/_static/image5.png)
2. <span data-ttu-id="b8560-165">Escriba un nombre de usuario.</span><span class="sxs-lookup"><span data-stu-id="b8560-165">Enter a user name.</span></span>
3. <span data-ttu-id="b8560-166">Copie la dirección URL desde la línea de dirección del explorador y usarlo para abrir dos instancias del explorador más.</span><span class="sxs-lookup"><span data-stu-id="b8560-166">Copy the URL from the address line of the browser and use it to open two more browser instances.</span></span> <span data-ttu-id="b8560-167">En cada instancia del explorador, escriba un nombre de usuario único.</span><span class="sxs-lookup"><span data-stu-id="b8560-167">In each browser instance, enter a unique user name.</span></span>
4. <span data-ttu-id="b8560-168">En cada instancia del explorador, agregue un comentario y haga clic en **enviar**.</span><span class="sxs-lookup"><span data-stu-id="b8560-168">In each browser instance, add a comment and click **Send**.</span></span> <span data-ttu-id="b8560-169">Los comentarios deben aparecer en todas las instancias del explorador.</span><span class="sxs-lookup"><span data-stu-id="b8560-169">The comments should display in all browser instances.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b8560-170">Esta aplicación de chat simple no mantiene el contexto de discusión en el servidor.</span><span class="sxs-lookup"><span data-stu-id="b8560-170">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="b8560-171">El concentrador difunde los comentarios de todos los usuarios actuales.</span><span class="sxs-lookup"><span data-stu-id="b8560-171">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="b8560-172">Los usuarios que se unan a la conversación más adelante verá mensajes agregados desde el momento en que se unan.</span><span class="sxs-lookup"><span data-stu-id="b8560-172">Users who join the chat later will see messages added from the time they join.</span></span>

    <span data-ttu-id="b8560-173">La captura de pantalla siguiente muestra la aplicación de chat que se ejecuta en tres instancias del explorador, todos ellos se actualizan cuando una instancia envía un mensaje:</span><span class="sxs-lookup"><span data-stu-id="b8560-173">The following screen shot shows the chat application running in three browser instances, all of which are updated when one instance sends a message:</span></span>

    ![Exploradores de chat](tutorial-getting-started-with-signalr/_static/image6.png)
5. <span data-ttu-id="b8560-175">En **el Explorador de soluciones**, inspeccionar la **documentos de Script** nodo para la aplicación en ejecución.</span><span class="sxs-lookup"><span data-stu-id="b8560-175">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="b8560-176">Hay un archivo de script denominado **concentradores** que la biblioteca de SignalR genera dinámicamente en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="b8560-176">There is a script file named **hubs** that the SignalR library dynamically generates at runtime.</span></span> <span data-ttu-id="b8560-177">Este archivo administra la comunicación entre el código de servidor y los scripts de jQuery.</span><span class="sxs-lookup"><span data-stu-id="b8560-177">This file manages the communication between jQuery script and server-side code.</span></span>

    ![Script de concentrador generado](tutorial-getting-started-with-signalr/_static/image7.png)

<a id="code"></a>

## <a name="examine-the-code"></a><span data-ttu-id="b8560-179">Examine el código</span><span class="sxs-lookup"><span data-stu-id="b8560-179">Examine the Code</span></span>

<span data-ttu-id="b8560-180">La aplicación de chat de SignalR muestra dos tareas básicas de desarrollo de SignalR: creando un centro de que el objeto de coordinación principal en el servidor y utiliza la biblioteca de jQuery de SignalR para enviar y recibir mensajes.</span><span class="sxs-lookup"><span data-stu-id="b8560-180">The SignalR chat application demonstrates two basic SignalR development tasks: creating a hub as the main coordination object on the server, and using the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs"></a><span data-ttu-id="b8560-181">Concentradores de SignalR</span><span class="sxs-lookup"><span data-stu-id="b8560-181">SignalR Hubs</span></span>

<span data-ttu-id="b8560-182">En el ejemplo de código la **ChatHub** clase se deriva de la **Microsoft.AspNet.SignalR.Hub** clase.</span><span class="sxs-lookup"><span data-stu-id="b8560-182">In the code sample the **ChatHub** class derives from the **Microsoft.AspNet.SignalR.Hub** class.</span></span> <span data-ttu-id="b8560-183">Derivar de la **concentrador** clase es una forma útil de compilar una aplicación de SignalR.</span><span class="sxs-lookup"><span data-stu-id="b8560-183">Deriving from the **Hub** class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="b8560-184">Puede crear métodos públicos en la clase de base de datos central y, a continuación, obtener acceso a estos métodos al llamarlas desde scripts de jQuery en una página web.</span><span class="sxs-lookup"><span data-stu-id="b8560-184">You can create public methods on your hub class and then access those methods by calling them from jQuery scripts in a web page.</span></span>

<span data-ttu-id="b8560-185">En el código de chat, llame los clientes la **ChatHub.Send** método para enviar un mensaje nuevo.</span><span class="sxs-lookup"><span data-stu-id="b8560-185">In the chat code, clients call the **ChatHub.Send** method to send a new message.</span></span> <span data-ttu-id="b8560-186">El concentrador a su vez envía el mensaje a todos los clientes mediante una llamada a **Clients.All.broadcastMessage**.</span><span class="sxs-lookup"><span data-stu-id="b8560-186">The hub in turn sends the message to all clients by calling **Clients.All.broadcastMessage**.</span></span>

<span data-ttu-id="b8560-187">El **enviar** método muestra varios conceptos de base de datos central:</span><span class="sxs-lookup"><span data-stu-id="b8560-187">The **Send** method demonstrates several hub concepts :</span></span>

- <span data-ttu-id="b8560-188">Declarar métodos públicos en un centro de modo que los clientes se les pueden llamar.</span><span class="sxs-lookup"><span data-stu-id="b8560-188">Declare public methods on a hub so that clients can call them.</span></span>
- <span data-ttu-id="b8560-189">Use la **Microsoft.AspNet.SignalR.Hub.Clients** propiedades dinámicas para tener acceso a todos los clientes conectan a este concentrador.</span><span class="sxs-lookup"><span data-stu-id="b8560-189">Use the **Microsoft.AspNet.SignalR.Hub.Clients** dynamic property to access all clients connected to this hub.</span></span>
- <span data-ttu-id="b8560-190">Llamar a una función de jQuery en el cliente (como el `broadcastMessage` función) para actualizar los clientes.</span><span class="sxs-lookup"><span data-stu-id="b8560-190">Call a jQuery function on the client (such as the `broadcastMessage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a><span data-ttu-id="b8560-191">SignalR y jQuery</span><span class="sxs-lookup"><span data-stu-id="b8560-191">SignalR and jQuery</span></span>

<span data-ttu-id="b8560-192">La página HTML en el ejemplo de código muestra cómo utilizar la biblioteca de jQuery SignalR para comunicarse con un concentrador de SignalR.</span><span class="sxs-lookup"><span data-stu-id="b8560-192">The HTML page in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="b8560-193">Las tareas esenciales en el código declara un proxy para hacer referencia al concentrador, declarar una función que el servidor puede llamar para contenido de inserción a los clientes y, a partir de una conexión para enviar mensajes al concentrador.</span><span class="sxs-lookup"><span data-stu-id="b8560-193">The essential tasks in the code are declaring a proxy to reference the hub, declaring a function that the server can call to push content to clients, and starting a connection to send messages to the hub.</span></span>

<span data-ttu-id="b8560-194">El código siguiente declara a un proxy para un concentrador.</span><span class="sxs-lookup"><span data-stu-id="b8560-194">The following code declares a proxy for a hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample6.js)]

> [!NOTE]
> <span data-ttu-id="b8560-195">En jQuery la referencia a la clase de servidor y sus miembros está en mayúsculas y minúsculas camel.</span><span class="sxs-lookup"><span data-stu-id="b8560-195">In jQuery the reference to the server class and its members is in camel case.</span></span> <span data-ttu-id="b8560-196">El ejemplo de código hace referencia a C# **ChatHub** clase jQuery como **chatHub**.</span><span class="sxs-lookup"><span data-stu-id="b8560-196">The code sample references the C# **ChatHub** class in jQuery as **chatHub**.</span></span>


<span data-ttu-id="b8560-197">El código siguiente es cómo crear una función de devolución de llamada en la secuencia de comandos.</span><span class="sxs-lookup"><span data-stu-id="b8560-197">The following code is how you create a callback function in the script.</span></span> <span data-ttu-id="b8560-198">La clase de base de datos central en el servidor llama a esta función para insertar las actualizaciones de contenido en cada cliente.</span><span class="sxs-lookup"><span data-stu-id="b8560-198">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="b8560-199">Las dos líneas que HTML codificar el contenido antes de mostrarlo son opcionales y mostrar una manera sencilla de evitar la inyección de script.</span><span class="sxs-lookup"><span data-stu-id="b8560-199">The two lines that HTML encode the content before displaying it are optional and show a simple way to prevent script injection.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample7.html)]

<span data-ttu-id="b8560-200">El código siguiente muestra cómo abrir una conexión con el centro.</span><span class="sxs-lookup"><span data-stu-id="b8560-200">The following code shows how to open a connection with the hub.</span></span> <span data-ttu-id="b8560-201">El código inicia la conexión y, a continuación, pasa una función para controlar el evento click en la **enviar** botón en la página HTML.</span><span class="sxs-lookup"><span data-stu-id="b8560-201">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the HTML page.</span></span>

> [!NOTE]
> <span data-ttu-id="b8560-202">Este enfoque garantiza que la conexión se establece antes de que se ejecuta el controlador de eventos.</span><span class="sxs-lookup"><span data-stu-id="b8560-202">This approach insures that the connection is established before the event handler executes.</span></span>


[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a><span data-ttu-id="b8560-203">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="b8560-203">Next Steps</span></span>

<span data-ttu-id="b8560-204">Ha aprendido que SignalR es un marco para generar aplicaciones web en tiempo real.</span><span class="sxs-lookup"><span data-stu-id="b8560-204">You learned that SignalR is a framework for building real-time web applications.</span></span> <span data-ttu-id="b8560-205">También ha aprendido a varias tareas de desarrollo de SignalR: cómo agregar SignalR a una aplicación de ASP.NET, cómo crear una clase de base de datos central y cómo enviar y recibir mensajes desde el concentrador.</span><span class="sxs-lookup"><span data-stu-id="b8560-205">You also learned several SignalR development tasks: how to add SignalR to an ASP.NET application, how to create a hub class, and how to send and receive messages from the hub.</span></span>

<span data-ttu-id="b8560-206">Hacer que la aplicación de ejemplo en este tutorial u otras aplicaciones SignalR disponible a través de Internet mediante su implementación en un proveedor de hospedaje.</span><span class="sxs-lookup"><span data-stu-id="b8560-206">You can make the sample application in this tutorial or other SignalR applications available over the Internet by deploying them to a hosting provider.</span></span> <span data-ttu-id="b8560-207">Microsoft ofrece el hospedaje web gratuito para hasta 10 sitios web en una segunda [cuenta de prueba de Windows Azure](https://www.windowsazure.com/en-us/pricing/free-trial/?WT.mc_id=A443DD604).</span><span class="sxs-lookup"><span data-stu-id="b8560-207">Microsoft offers free web hosting for up to 10 web sites in a free [Windows Azure trial account](https://www.windowsazure.com/en-us/pricing/free-trial/?WT.mc_id=A443DD604).</span></span> <span data-ttu-id="b8560-208">Para ver un tutorial sobre cómo implementar la aplicación de ejemplo SignalR, consulte [publicar el ejemplo de introducción a SignalR como un sitio Web de Windows Azure](https://blogs.msdn.com/b/timlee/archive/2013/02/27/deploy-the-signalr-getting-started-sample-as-a-windows-azure-web-site.aspx).</span><span class="sxs-lookup"><span data-stu-id="b8560-208">For a walkthrough on how to deploy the sample SignalR application, see [Publish the SignalR Getting Started Sample as a Windows Azure Web Site](https://blogs.msdn.com/b/timlee/archive/2013/02/27/deploy-the-signalr-getting-started-sample-as-a-windows-azure-web-site.aspx).</span></span> <span data-ttu-id="b8560-209">Para obtener información detallada sobre cómo implementar un proyecto web de Visual Studio en un sitio Web de Windows Azure, consulte [implementar una aplicación de ASP.NET en un sitio Web de Windows Azure](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).</span><span class="sxs-lookup"><span data-stu-id="b8560-209">For detailed information about how to deploy a Visual Studio web project to a Windows Azure Web Site, see [Deploying an ASP.NET Application to a Windows Azure Web Site](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).</span></span> <span data-ttu-id="b8560-210">(Nota: el transporte de WebSocket no se admite actualmente para sitios Web de Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="b8560-210">(Note: The WebSocket transport is not currently supported for Windows Azure Web Sites.</span></span> <span data-ttu-id="b8560-211">Transporte de WebSocket cuando no está disponible, SignalR usa los otros transportes disponibles, tal como se describe en la sección de transportes de la [Introducción al tema de SignalR](index.md).)</span><span class="sxs-lookup"><span data-stu-id="b8560-211">When WebSocket transport is not available, SignalR uses the other available transports as described in the Transports section of the [Introduction to SignalR topic](index.md).)</span></span>

<span data-ttu-id="b8560-212">Para obtener información sobre conceptos de los desarrollos de SignalR más avanzados, visite los siguientes sitios de código fuente de SignalR y recursos:</span><span class="sxs-lookup"><span data-stu-id="b8560-212">To learn more advanced SignalR developments concepts, visit the following sites for SignalR source code and resources:</span></span>

- [<span data-ttu-id="b8560-213">Proyecto de SignalR</span><span class="sxs-lookup"><span data-stu-id="b8560-213">SignalR Project</span></span>](http://signalr.net)
- [<span data-ttu-id="b8560-214">SignalR Github y ejemplos</span><span class="sxs-lookup"><span data-stu-id="b8560-214">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="b8560-215">SignalR Wiki</span><span class="sxs-lookup"><span data-stu-id="b8560-215">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)
