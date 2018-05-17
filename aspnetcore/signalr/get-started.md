---
title: Empezar a trabajar con SignalR en ASP.NET Core
author: rachelappel
description: En este tutorial, creará una aplicación con SignalR para ASP.NET Core.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/09/2018
ms.prod: aspnet-core
ms.topic: tutorial
ms.technology: aspnet
uid: signalr/get-started
ms.openlocfilehash: 5929ee44aa58088614f910560eafbf5f5ab82ded
ms.sourcegitcommit: 9bc34b8269d2a150b844c3b8646dcb30278a95ea
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/12/2018
---
# <a name="get-started-with-signalr-on-aspnet-core"></a><span data-ttu-id="8d3f0-103">Empezar a trabajar con SignalR en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8d3f0-103">Get started with SignalR on ASP.NET Core</span></span>

<span data-ttu-id="8d3f0-104">Por [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="8d3f0-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="8d3f0-105">Este tutorial le enseña los fundamentos de la creación de una aplicación en tiempo real con SignalR para ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8d3f0-105">This tutorial teaches the basics of building a real-time app using SignalR for ASP.NET Core.</span></span>

   ![Soluciones](get-started/_static/signalr-get-started-finished.png)

<span data-ttu-id="8d3f0-107">Este tutorial muestra las tareas de desarrollo de SignalR siguientes:</span><span class="sxs-lookup"><span data-stu-id="8d3f0-107">This tutorial demonstrates the following SignalR development tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="8d3f0-108">Crear un SignalR en la aplicación web de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8d3f0-108">Create a SignalR on ASP.NET Core web app.</span></span>
> * <span data-ttu-id="8d3f0-109">Cree un concentrador de SignalR para insertar contenido en los clientes.</span><span class="sxs-lookup"><span data-stu-id="8d3f0-109">Create a SignalR hub to push content to clients.</span></span>
> * <span data-ttu-id="8d3f0-110">Modificar el `Startup` clase y configurar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="8d3f0-110">Modify the `Startup` class and configure the app.</span></span>

<span data-ttu-id="8d3f0-111">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started/sample/) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8d3f0-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

# <a name="prerequisites"></a><span data-ttu-id="8d3f0-112">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="8d3f0-112">Prerequisites</span></span>

<span data-ttu-id="8d3f0-113">Instalar el software siguiente:</span><span class="sxs-lookup"><span data-stu-id="8d3f0-113">Install the following software:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8d3f0-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8d3f0-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="8d3f0-115">[SDK de .NET core 2.1.0 RC 1](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-rc1) o posterior</span><span class="sxs-lookup"><span data-stu-id="8d3f0-115">[.NET Core 2.1.0 RC 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-rc1) or later</span></span>
* <span data-ttu-id="8d3f0-116">[Visual Studio de 2017](https://www.visualstudio.com/downloads/) versión 15.7 o posterior con el **ASP.NET y desarrollo web** carga de trabajo</span><span class="sxs-lookup"><span data-stu-id="8d3f0-116">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.7 or later with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="8d3f0-117">NPM</span><span class="sxs-lookup"><span data-stu-id="8d3f0-117">npm</span></span>](https://www.npmjs.com/get-npm)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="8d3f0-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8d3f0-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="8d3f0-119">[SDK de .NET core 2.1.0 RC 1](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-rc1) o posterior</span><span class="sxs-lookup"><span data-stu-id="8d3f0-119">[.NET Core 2.1.0 RC 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-rc1) or later</span></span>
* [<span data-ttu-id="8d3f0-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8d3f0-120">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="8d3f0-121">C# para el código de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8d3f0-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [<span data-ttu-id="8d3f0-122">NPM</span><span class="sxs-lookup"><span data-stu-id="8d3f0-122">npm</span></span>](https://www.npmjs.com/get-npm)

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a><span data-ttu-id="8d3f0-123">Cree un proyecto de ASP.NET Core que hospeda el servidor y cliente de SignalR</span><span class="sxs-lookup"><span data-stu-id="8d3f0-123">Create an ASP.NET Core project that hosts SignalR client and server</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8d3f0-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8d3f0-124">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="8d3f0-125">Use la **archivo** > **nuevo proyecto** menú opción y elija **aplicación Web de ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="8d3f0-125">Use the **File** > **New Project** menu option and choose **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="8d3f0-126">Denomine el proyecto *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="8d3f0-126">Name the project *SignalRChat*.</span></span>

   ![Cuadro de diálogo nuevo proyecto en Visual Studio](get-started/_static/signalr-new-project-dialog.png)

2. <span data-ttu-id="8d3f0-128">Seleccione **aplicación Web** para crear un proyecto con páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="8d3f0-128">Select **Web Application** to create a project using Razor Pages.</span></span> <span data-ttu-id="8d3f0-129">A continuación, seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="8d3f0-129">Then select **OK**.</span></span> <span data-ttu-id="8d3f0-130">Asegúrese de que **ASP.NET Core 2.1** está seleccionada en el selector de framework, aunque se ejecute SignalR en versiones anteriores de .NET.</span><span class="sxs-lookup"><span data-stu-id="8d3f0-130">Be sure that **ASP.NET Core 2.1** is selected from the framework selector, though SignalR runs on older versions of .NET.</span></span>

   ![Cuadro de diálogo nuevo proyecto en Visual Studio](get-started/_static/signalr-new-project-choose-type.png)

<span data-ttu-id="8d3f0-132">Visual Studio incluye la `Microsoft.AspNetCore.SignalR` paquete que contiene sus bibliotecas de servidor como parte de su **aplicación Web de ASP.NET Core** plantilla.</span><span class="sxs-lookup"><span data-stu-id="8d3f0-132">Visual Studio includes the `Microsoft.AspNetCore.SignalR` package containing its server libraries as part of its **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="8d3f0-133">Sin embargo, debe instalarse la biblioteca de cliente de JavaScript para SignalR con *npm*.</span><span class="sxs-lookup"><span data-stu-id="8d3f0-133">However, the JavaScript client library for SignalR must be installed using *npm*.</span></span>

3. <span data-ttu-id="8d3f0-134">Ejecute los siguientes comandos el **Package Manager Console** ventana, desde la raíz del proyecto:</span><span class="sxs-lookup"><span data-stu-id="8d3f0-134">Run the following commands in the **Package Manager Console** window, from the project root:</span></span>

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```     

4. <span data-ttu-id="8d3f0-135">Copia la *signalr.js* de archivos de *node_modules\\ @aspnet\signalr\dist\browser*  a la *lib* carpeta del proyecto.</span><span class="sxs-lookup"><span data-stu-id="8d3f0-135">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to the *lib* folder in your project.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="8d3f0-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8d3f0-136">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="8d3f0-137">Desde el **Terminal integrado**, ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="8d3f0-137">From the **Integrated Terminal**, run the following command:</span></span>

    ```console
    dotnet new razor -o SignalRChat
    ```

2. <span data-ttu-id="8d3f0-138">Instalar la biblioteca de cliente de JavaScript con *npm*.</span><span class="sxs-lookup"><span data-stu-id="8d3f0-138">Install the JavaScript client library using *npm*.</span></span>

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

3. <span data-ttu-id="8d3f0-139">Copia la *signalr.js* de archivos de *node_modules\\ @aspnet\signalr\dist\browser*  a la *lib* carpeta del proyecto.</span><span class="sxs-lookup"><span data-stu-id="8d3f0-139">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to the *lib* folder in your project.</span></span>

-----

## <a name="create-the-signalr-hub"></a><span data-ttu-id="8d3f0-140">Crear el concentrador de SignalR</span><span class="sxs-lookup"><span data-stu-id="8d3f0-140">Create the SignalR Hub</span></span>

<span data-ttu-id="8d3f0-141">Un concentrador es una clase que actúa como una canalización de alto nivel que permite al cliente y servidor llamar a métodos en entre sí.</span><span class="sxs-lookup"><span data-stu-id="8d3f0-141">A hub is a class that serves as a high-level pipeline that allows the client and server to call methods on each other.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8d3f0-142">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8d3f0-142">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="8d3f0-143">Agregar una clase al proyecto eligiendo **archivo** > **New** > **archivo** y seleccionando **clase de Visual C#**.</span><span class="sxs-lookup"><span data-stu-id="8d3f0-143">Add a class to the project by choosing **File** > **New** > **File** and selecting **Visual C# Class**.</span></span>

2. <span data-ttu-id="8d3f0-144">Heredar de `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="8d3f0-144">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="8d3f0-145">La `Hub` clase contiene propiedades y eventos para administrar las conexiones y grupos, así como enviar y recibir datos.</span><span class="sxs-lookup"><span data-stu-id="8d3f0-145">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data.</span></span>

3. <span data-ttu-id="8d3f0-146">Crear el `SendMessage` método que envía un mensaje a todos los clientes de chat conectado.</span><span class="sxs-lookup"><span data-stu-id="8d3f0-146">Create the `SendMessage` method that sends a message to all connected chat clients.</span></span> <span data-ttu-id="8d3f0-147">Tenga en cuenta devuelve un [tarea](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), porque SignalR es asincrónica.</span><span class="sxs-lookup"><span data-stu-id="8d3f0-147">Notice it returns a [Task](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), because SignalR is asynchronous.</span></span> <span data-ttu-id="8d3f0-148">Código asincrónico se escala mejor.</span><span class="sxs-lookup"><span data-stu-id="8d3f0-148">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](get-started/sample/Hubs/ChatHub.cs)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="8d3f0-149">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8d3f0-149">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="8d3f0-150">Abra la *SignalRChat* carpeta en el código de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8d3f0-150">Open the *SignalRChat* folder in Visual Studio Code.</span></span>

2. <span data-ttu-id="8d3f0-151">Agregar una clase al proyecto seleccionando **archivo** > **nuevo archivo** en el menú.</span><span class="sxs-lookup"><span data-stu-id="8d3f0-151">Add a class to the project by selecting **File** > **New File** from the menu.</span></span>

3. <span data-ttu-id="8d3f0-152">Heredar de `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="8d3f0-152">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="8d3f0-153">La `Hub` clase contiene propiedades y eventos para administrar las conexiones y grupos, así como enviar y recibir datos a los clientes.</span><span class="sxs-lookup"><span data-stu-id="8d3f0-153">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data to clients.</span></span>

4. <span data-ttu-id="8d3f0-154">Agregue un método `SendMessage` a la clase.</span><span class="sxs-lookup"><span data-stu-id="8d3f0-154">Add a `SendMessage` method to the class.</span></span> <span data-ttu-id="8d3f0-155">El `SendMessage` método envía un mensaje a todos los clientes de chat conectado.</span><span class="sxs-lookup"><span data-stu-id="8d3f0-155">The `SendMessage` method sends a message to all connected chat clients.</span></span> <span data-ttu-id="8d3f0-156">Tenga en cuenta devuelve un [tarea](/dotnet/api/system.threading.tasks.task), porque SignalR es asincrónica.</span><span class="sxs-lookup"><span data-stu-id="8d3f0-156">Notice it returns a [Task](/dotnet/api/system.threading.tasks.task), because SignalR is asynchronous.</span></span> <span data-ttu-id="8d3f0-157">Código asincrónico se escala mejor.</span><span class="sxs-lookup"><span data-stu-id="8d3f0-157">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](get-started/sample/Hubs/ChatHub.cs?range=6-12)]

-----

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="8d3f0-158">Configurar el proyecto para que use SignalR</span><span class="sxs-lookup"><span data-stu-id="8d3f0-158">Configure the project to use SignalR</span></span>

<span data-ttu-id="8d3f0-159">El servidor de SignalR debe configurarse para que sepa para pasar las solicitudes para SignalR.</span><span class="sxs-lookup"><span data-stu-id="8d3f0-159">The SignalR server must be configured so that it knows to pass requests to SignalR.</span></span>

1. <span data-ttu-id="8d3f0-160">Para configurar un proyecto de SignalR, modifique el proyecto `Startup.ConfigureServices` método.</span><span class="sxs-lookup"><span data-stu-id="8d3f0-160">To configure a SignalR project, modify the project's `Startup.ConfigureServices` method.</span></span>

   <span data-ttu-id="8d3f0-161">`services.AddSignalR` Agrega SignalR como parte de la [middleware](xref:fundamentals/middleware/index) canalización.</span><span class="sxs-lookup"><span data-stu-id="8d3f0-161">`services.AddSignalR` adds SignalR as part of the [middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

2. <span data-ttu-id="8d3f0-162">Configurar las rutas a los concentradores con `UseSignalR`.</span><span class="sxs-lookup"><span data-stu-id="8d3f0-162">Configure routes to your hubs using `UseSignalR`.</span></span>


   [!code-csharp[Startup](get-started/sample/Startup.cs?highlight=37,57-60)]


## <a name="create-the-signalr-client-code"></a><span data-ttu-id="8d3f0-163">Crear el código de cliente de SignalR</span><span class="sxs-lookup"><span data-stu-id="8d3f0-163">Create the SignalR client code</span></span>

1. <span data-ttu-id="8d3f0-164">Reemplace el contenido de *Pages\Index.cshtml* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="8d3f0-164">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

   [!code-cshtml[Index](get-started/sample/Pages/Index.cshtml)]

   <span data-ttu-id="8d3f0-165">El código HTML anterior muestra el nombre y campos del mensaje y un botón de envío.</span><span class="sxs-lookup"><span data-stu-id="8d3f0-165">The preceding HTML displays name and message fields, and a submit button.</span></span> <span data-ttu-id="8d3f0-166">Tenga en cuenta las referencias de script en la parte inferior: una referencia a SignalR y *chat.js*.</span><span class="sxs-lookup"><span data-stu-id="8d3f0-166">Notice the script references at the bottom: a reference to SignalR and *chat.js*.</span></span>

2. <span data-ttu-id="8d3f0-167">Agregue un archivo de JavaScript denominado *chat.js*, a la *wwwroot\js* carpeta.</span><span class="sxs-lookup"><span data-stu-id="8d3f0-167">Add a JavaScript file, named *chat.js*, to the *wwwroot\js* folder.</span></span> <span data-ttu-id="8d3f0-168">Agregue el código siguiente a él:</span><span class="sxs-lookup"><span data-stu-id="8d3f0-168">Add the following code to it:</span></span>

   [!code-javascript[Index](get-started/sample/wwwroot/js/chat.js)]

## <a name="run-the-app"></a><span data-ttu-id="8d3f0-169">Ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="8d3f0-169">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8d3f0-170">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8d3f0-170">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="8d3f0-171">Seleccione **depurar** > **iniciar sin depurar** para iniciar un explorador y cargar el sitio Web de forma local.</span><span class="sxs-lookup"><span data-stu-id="8d3f0-171">Select **Debug** > **Start without debugging** to launch a browser and load the website locally.</span></span> <span data-ttu-id="8d3f0-172">Copie la dirección URL de la barra de direcciones.</span><span class="sxs-lookup"><span data-stu-id="8d3f0-172">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="8d3f0-173">Abra otra instancia del explorador (cualquier explorador) y pegue la dirección URL en la barra de direcciones.</span><span class="sxs-lookup"><span data-stu-id="8d3f0-173">Open another browser instance (any browser) and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="8d3f0-174">Elija cualquiera de estos exploradores, escriba un nombre y un mensaje y haga clic en el **enviar** botón.</span><span class="sxs-lookup"><span data-stu-id="8d3f0-174">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="8d3f0-175">El nombre y el mensaje se muestran en ambas páginas al instante.</span><span class="sxs-lookup"><span data-stu-id="8d3f0-175">The name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="8d3f0-176">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8d3f0-176">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="8d3f0-177">Presione **Depurar** (F5) para compilar y ejecutar el programa.</span><span class="sxs-lookup"><span data-stu-id="8d3f0-177">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="8d3f0-178">Ejecutar el programa, abre una ventana del explorador.</span><span class="sxs-lookup"><span data-stu-id="8d3f0-178">Running the program opens a browser window.</span></span>

1. <span data-ttu-id="8d3f0-179">Abrir otra ventana del explorador y cargar el sitio Web de forma local en ella.</span><span class="sxs-lookup"><span data-stu-id="8d3f0-179">Open another browser window and load the website locally in it.</span></span>

1. <span data-ttu-id="8d3f0-180">Elija cualquiera de estos exploradores, escriba un nombre y un mensaje y haga clic en el **enviar** botón.</span><span class="sxs-lookup"><span data-stu-id="8d3f0-180">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="8d3f0-181">El nombre y el mensaje se muestran en ambas páginas al instante.</span><span class="sxs-lookup"><span data-stu-id="8d3f0-181">The name and message are displayed on both pages instantly.</span></span>

---

  ![Soluciones](get-started/_static/signalr-get-started-finished.png)

## <a name="related-resources"></a><span data-ttu-id="8d3f0-183">Recursos relacionados</span><span class="sxs-lookup"><span data-stu-id="8d3f0-183">Related resources</span></span>

[<span data-ttu-id="8d3f0-184">Introducción a ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="8d3f0-184">Introduction to ASP.NET Core SignalR</span></span>](introduction.md)
