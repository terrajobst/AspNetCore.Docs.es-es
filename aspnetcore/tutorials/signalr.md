---
title: Introducción a SignalR en ASP.NET Core
author: rachelappel
description: En este tutorial, creará una aplicación con SignalR para ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/22/2018
uid: tutorials/signalr
ms.openlocfilehash: 6b8222ee04573ca7157b4e1125ed5a4453b2b9a9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830560"
---
# <a name="get-started-with-signalr-on-aspnet-core"></a><span data-ttu-id="b76d6-103">Introducción a SignalR en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b76d6-103">Get started with SignalR on ASP.NET Core</span></span>

<span data-ttu-id="b76d6-104">Por [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="b76d6-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="b76d6-105">Este tutorial le enseña los conceptos básicos de la creación de una aplicación en tiempo real con SignalR para ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b76d6-105">This tutorial teaches the basics of building a real-time app using SignalR for ASP.NET Core.</span></span>

   ![Soluciones](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="b76d6-107">En este tutorial se muestran las siguientes tareas de desarrollo de SignalR:</span><span class="sxs-lookup"><span data-stu-id="b76d6-107">This tutorial demonstrates the following SignalR development tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b76d6-108">Crear una aplicación web de SignalR en ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b76d6-108">Create a SignalR on ASP.NET Core web app.</span></span>
> * <span data-ttu-id="b76d6-109">Cree un concentrador de SignalR para insertar contenido en los clientes.</span><span class="sxs-lookup"><span data-stu-id="b76d6-109">Create a SignalR hub to push content to clients.</span></span>
> * <span data-ttu-id="b76d6-110">Modificar la clase `Startup` y configurar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b76d6-110">Modify the `Startup` class and configure the app.</span></span>

<span data-ttu-id="b76d6-111">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b76d6-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b76d6-112">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="b76d6-112">Prerequisites</span></span>

<span data-ttu-id="b76d6-113">Instale el software siguiente:</span><span class="sxs-lookup"><span data-stu-id="b76d6-113">Install the following software:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b76d6-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b76d6-114">Visual Studio</span></span>](#tab/visual-studio)

* [<span data-ttu-id="b76d6-115">.NET Core SDK 2.1 o posterior</span><span class="sxs-lookup"><span data-stu-id="b76d6-115">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="b76d6-116">[Visual Studio 2017](https://www.visualstudio.com/downloads/) versión 15.7.3 o posterior con la carga de trabajo **ASP.NET y desarrollo web**</span><span class="sxs-lookup"><span data-stu-id="b76d6-116">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.7.3 or later with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="b76d6-117">npm</span><span class="sxs-lookup"><span data-stu-id="b76d6-117">npm</span></span>](https://www.npmjs.com/get-npm)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b76d6-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b76d6-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="b76d6-119">.NET Core SDK 2.1 o posterior</span><span class="sxs-lookup"><span data-stu-id="b76d6-119">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="b76d6-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b76d6-120">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="b76d6-121">C# para Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b76d6-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [<span data-ttu-id="b76d6-122">npm</span><span class="sxs-lookup"><span data-stu-id="b76d6-122">npm</span></span>](https://www.npmjs.com/get-npm)

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a><span data-ttu-id="b76d6-123">Crear un proyecto de ASP.NET Core que hospede el servidor y cliente de SignalR</span><span class="sxs-lookup"><span data-stu-id="b76d6-123">Create an ASP.NET Core project that hosts SignalR client and server</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b76d6-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b76d6-124">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="b76d6-125">Use la opción de menú **Archivo** > **Nuevo proyecto** y elija **Aplicación web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="b76d6-125">Use the **File** > **New Project** menu option and choose **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="b76d6-126">Asigne al proyecto el nombre *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="b76d6-126">Name the project *SignalRChat*.</span></span>

   ![Cuadro de diálogo Nuevo proyecto en Visual Studio](signalr/_static/signalr-new-project-dialog.png)

2. <span data-ttu-id="b76d6-128">Seleccione **Aplicación web** para crear un proyecto con Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="b76d6-128">Select **Web Application** to create a project using Razor Pages.</span></span> <span data-ttu-id="b76d6-129">Después, haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="b76d6-129">Then select **OK**.</span></span> <span data-ttu-id="b76d6-130">Asegúrese de que **ASP.NET Core 2.1** está seleccionada en el selector de framework, aunque se ejecute SignalR en versiones anteriores de .NET.</span><span class="sxs-lookup"><span data-stu-id="b76d6-130">Be sure that **ASP.NET Core 2.1** is selected from the framework selector, though SignalR runs on older versions of .NET.</span></span>

   ![Cuadro de diálogo Nuevo proyecto en Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

<span data-ttu-id="b76d6-132">Visual Studio incluye el paquete `Microsoft.AspNetCore.SignalR` que contiene sus bibliotecas de servidor como parte de la plantilla **Aplicación web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="b76d6-132">Visual Studio includes the `Microsoft.AspNetCore.SignalR` package containing its server libraries as part of its **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="b76d6-133">Pero debe instalarse la biblioteca de cliente de JavaScript para SignalR con *npm*.</span><span class="sxs-lookup"><span data-stu-id="b76d6-133">However, the JavaScript client library for SignalR must be installed using *npm*.</span></span>

3. <span data-ttu-id="b76d6-134">Ejecute los comandos siguientes en la ventana **Consola del Administrador de paquetes**, desde la raíz del proyecto:</span><span class="sxs-lookup"><span data-stu-id="b76d6-134">Run the following commands in the **Package Manager Console** window, from the project root:</span></span>

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

4. <span data-ttu-id="b76d6-135">Cree una carpeta denominada "signalr" dentro de la carpeta *lib* del proyecto.</span><span class="sxs-lookup"><span data-stu-id="b76d6-135">Create a new folder named "signalr" inside the  *lib* folder in your project.</span></span> <span data-ttu-id="b76d6-136">Copie el archivo *signalr.js* de *node_modules\\@aspnet\signalr\dist\browser*  a esta carpeta.</span><span class="sxs-lookup"><span data-stu-id="b76d6-136">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to this folder.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b76d6-137">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b76d6-137">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="b76d6-138">En el **terminal integrado**, ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="b76d6-138">From the **Integrated Terminal**, run the following command:</span></span>

    ```console
    dotnet new webapp -o SignalRChat
    ```

    [!INCLUDE[](~/includes/webapp-alias-notice.md)]

2. <span data-ttu-id="b76d6-139">Instale la biblioteca de cliente de JavaScript con *npm*.</span><span class="sxs-lookup"><span data-stu-id="b76d6-139">Install the JavaScript client library using *npm*.</span></span>

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

3. <span data-ttu-id="b76d6-140">Cree una carpeta denominada "signalr" dentro de la carpeta *lib* del proyecto.</span><span class="sxs-lookup"><span data-stu-id="b76d6-140">Create a new folder named "signalr" inside the  *lib* folder in your project.</span></span> <span data-ttu-id="b76d6-141">Copie el archivo *signalr.js* de *node_modules\\@aspnet\signalr\dist\browser*  a esta carpeta.</span><span class="sxs-lookup"><span data-stu-id="b76d6-141">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to this folder.</span></span>

---

## <a name="create-the-signalr-hub"></a><span data-ttu-id="b76d6-142">Crear el concentrador SignalR</span><span class="sxs-lookup"><span data-stu-id="b76d6-142">Create the SignalR Hub</span></span>

<span data-ttu-id="b76d6-143">Un concentrador es una clase que actúa como una canalización de alto nivel que permite al cliente y el servidor llamar a métodos entre sí.</span><span class="sxs-lookup"><span data-stu-id="b76d6-143">A hub is a class that serves as a high-level pipeline that allows the client and server to call methods on each other.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b76d6-144">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b76d6-144">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="b76d6-145">Agregue una clase al proyecto eligiendo **Archivo** > **Nuevo** > **Archivo** y seleccionando **Clase de Visual C#**.</span><span class="sxs-lookup"><span data-stu-id="b76d6-145">Add a class to the project by choosing **File** > **New** > **File** and selecting **Visual C# Class**.</span></span> <span data-ttu-id="b76d6-146">Asigne el nombre `ChatHub` a la clase y *ChatHub.cs* al archivo.</span><span class="sxs-lookup"><span data-stu-id="b76d6-146">Name the class `ChatHub` and the file *ChatHub.cs*.</span></span>

2. <span data-ttu-id="b76d6-147">Heredan de `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="b76d6-147">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="b76d6-148">La clase `Hub` contiene propiedades y eventos para administrar las conexiones y grupos, así como enviar y recibir datos.</span><span class="sxs-lookup"><span data-stu-id="b76d6-148">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data.</span></span>

3. <span data-ttu-id="b76d6-149">Cree el método `SendMessage` que envía un mensaje a todos los clientes de chat conectados.</span><span class="sxs-lookup"><span data-stu-id="b76d6-149">Create the `SendMessage` method that sends a message to all connected chat clients.</span></span> <span data-ttu-id="b76d6-150">Tenga en cuenta que devuelve una [Tarea](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), porque SignalR es asincrónica.</span><span class="sxs-lookup"><span data-stu-id="b76d6-150">Notice it returns a [Task](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), because SignalR is asynchronous.</span></span> <span data-ttu-id="b76d6-151">El código asincrónico se escala mejor.</span><span class="sxs-lookup"><span data-stu-id="b76d6-151">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b76d6-152">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b76d6-152">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="b76d6-153">Abra la carpeta *SignalRChat* en Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="b76d6-153">Open the *SignalRChat* folder in Visual Studio Code.</span></span>

2. <span data-ttu-id="b76d6-154">Agregue una clase al proyecto seleccionando **Archivo** > **Nuevo archivo** en el menú.</span><span class="sxs-lookup"><span data-stu-id="b76d6-154">Add a class to the project by selecting **File** > **New File** from the menu.</span></span> <span data-ttu-id="b76d6-155">Asigne el nombre `ChatHub` a la clase y *ChatHub.cs* al archivo.</span><span class="sxs-lookup"><span data-stu-id="b76d6-155">Name the class `ChatHub` and the file *ChatHub.cs*.</span></span>

3. <span data-ttu-id="b76d6-156">Heredan de `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="b76d6-156">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="b76d6-157">La clase `Hub` contiene propiedades y eventos para administrar las conexiones y grupos, así como enviar y recibir datos de los clientes.</span><span class="sxs-lookup"><span data-stu-id="b76d6-157">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data to clients.</span></span>

4. <span data-ttu-id="b76d6-158">Agregue un método `SendMessage` a la clase.</span><span class="sxs-lookup"><span data-stu-id="b76d6-158">Add a `SendMessage` method to the class.</span></span> <span data-ttu-id="b76d6-159">El método `SendMessage` envía un mensaje a todos los clientes de chat conectados.</span><span class="sxs-lookup"><span data-stu-id="b76d6-159">The `SendMessage` method sends a message to all connected chat clients.</span></span> <span data-ttu-id="b76d6-160">Tenga en cuenta que devuelve una [Tarea](/dotnet/api/system.threading.tasks.task), porque SignalR es asincrónica.</span><span class="sxs-lookup"><span data-stu-id="b76d6-160">Notice it returns a [Task](/dotnet/api/system.threading.tasks.task), because SignalR is asynchronous.</span></span> <span data-ttu-id="b76d6-161">El código asincrónico se escala mejor.</span><span class="sxs-lookup"><span data-stu-id="b76d6-161">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

-----

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="b76d6-162">Configurar el proyecto para que use SignalR</span><span class="sxs-lookup"><span data-stu-id="b76d6-162">Configure the project to use SignalR</span></span>

<span data-ttu-id="b76d6-163">El servidor de SignalR debe configurarse para que sepa pasar las solicitudes a SignalR.</span><span class="sxs-lookup"><span data-stu-id="b76d6-163">The SignalR server must be configured so that it knows to pass requests to SignalR.</span></span>

1. <span data-ttu-id="b76d6-164">Para configurar un proyecto de SignalR, modifique el método `Startup.ConfigureServices` del proyecto.</span><span class="sxs-lookup"><span data-stu-id="b76d6-164">To configure a SignalR project, modify the project's `Startup.ConfigureServices` method.</span></span>

   <span data-ttu-id="b76d6-165">`services.AddSignalR` hace que los servicios SignalR estén disponibles para el sistema de [inserción de dependencias](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="b76d6-165">`services.AddSignalR` makes the SignalR services available to the [dependency injection](xref:fundamentals/dependency-injection) system.</span></span>

1. <span data-ttu-id="b76d6-166">Configure rutas a las centrales con `UseSignalR` en el método `Configure`.</span><span class="sxs-lookup"><span data-stu-id="b76d6-166">Configure routes to your hubs with `UseSignalR` in the `Configure` method.</span></span> <span data-ttu-id="b76d6-167">`app.UseSignalR` agrega SignalR a la canalización de [middleware](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="b76d6-167">`app.UseSignalR` adds SignalR to the [middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

   [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=37,57-60)]

## <a name="create-the-signalr-client-code"></a><span data-ttu-id="b76d6-168">Crear el código de cliente de SignalR</span><span class="sxs-lookup"><span data-stu-id="b76d6-168">Create the SignalR client code</span></span>

1. <span data-ttu-id="b76d6-169">Agregue un archivo de JavaScript denominado *chat.js*, a la carpeta *wwwroot\js*.</span><span class="sxs-lookup"><span data-stu-id="b76d6-169">Add a JavaScript file, named *chat.js*, to the *wwwroot\js* folder.</span></span> <span data-ttu-id="b76d6-170">Agregue el código siguiente a él:</span><span class="sxs-lookup"><span data-stu-id="b76d6-170">Add the following code to it:</span></span>

   [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

2. <span data-ttu-id="b76d6-171">Reemplace el contenido de *Pages/Index.cshtml* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="b76d6-171">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

   [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

   <span data-ttu-id="b76d6-172">El código HTML anterior muestra los campos de nombre y mensaje, y un botón de envío.</span><span class="sxs-lookup"><span data-stu-id="b76d6-172">The preceding HTML displays name and message fields, and a submit button.</span></span> <span data-ttu-id="b76d6-173">Tenga en cuenta las referencias de script en la parte inferior: una referencia a SignalR y *chat.js*.</span><span class="sxs-lookup"><span data-stu-id="b76d6-173">Notice the script references at the bottom: a reference to SignalR and *chat.js*.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="b76d6-174">Ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="b76d6-174">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b76d6-175">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b76d6-175">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="b76d6-176">Seleccione **Depurar** > **Iniciar sin depurar** para iniciar un explorador y cargar el sitio web de forma local.</span><span class="sxs-lookup"><span data-stu-id="b76d6-176">Select **Debug** > **Start without debugging** to launch a browser and load the website locally.</span></span> <span data-ttu-id="b76d6-177">Copie la dirección URL de la barra de direcciones.</span><span class="sxs-lookup"><span data-stu-id="b76d6-177">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="b76d6-178">Abra otra instancia del explorador (cualquier explorador) y pegue la dirección URL en la barra de direcciones.</span><span class="sxs-lookup"><span data-stu-id="b76d6-178">Open another browser instance (any browser) and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="b76d6-179">Elija cualquier explorador, escriba un nombre y un mensaje, y haga clic en el botón **Enviar**.</span><span class="sxs-lookup"><span data-stu-id="b76d6-179">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="b76d6-180">El nombre y el mensaje se muestran en ambas páginas al instante.</span><span class="sxs-lookup"><span data-stu-id="b76d6-180">The name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b76d6-181">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b76d6-181">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="b76d6-182">Presione **Depurar** (F5) para compilar y ejecutar el programa.</span><span class="sxs-lookup"><span data-stu-id="b76d6-182">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="b76d6-183">Al ejecutar el programa se abre una ventana del explorador.</span><span class="sxs-lookup"><span data-stu-id="b76d6-183">Running the program opens a browser window.</span></span>

1. <span data-ttu-id="b76d6-184">Abra otra ventana del explorador y cargue el sitio web de forma local en ella.</span><span class="sxs-lookup"><span data-stu-id="b76d6-184">Open another browser window and load the website locally in it.</span></span>

1. <span data-ttu-id="b76d6-185">Elija cualquier explorador, escriba un nombre y un mensaje, y haga clic en el botón **Enviar**.</span><span class="sxs-lookup"><span data-stu-id="b76d6-185">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="b76d6-186">El nombre y el mensaje se muestran en ambas páginas al instante.</span><span class="sxs-lookup"><span data-stu-id="b76d6-186">The name and message are displayed on both pages instantly.</span></span>

---

  ![Soluciones](signalr/_static/signalr-get-started-finished.png)

## <a name="related-resources"></a><span data-ttu-id="b76d6-188">Recursos relacionados</span><span class="sxs-lookup"><span data-stu-id="b76d6-188">Related resources</span></span>

[<span data-ttu-id="b76d6-189">Introducción a SignalR de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b76d6-189">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
