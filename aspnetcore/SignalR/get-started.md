---
title: Empezar a trabajar con SignalR en ASP.NET Core
author: rachelappel
description: En este tutorial, creará una aplicación con SignalR para ASP.NET Core.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 03/16/2018
ms.prod: aspnet-core
ms.topic: tutorial
ms.technology: aspnet
uid: signalr/get-started
ms.openlocfilehash: 03735359bb22cc3085ddc7b34372ecfc9501a940
ms.sourcegitcommit: 07903a1be39a99dcf538d57981161592d0e658b8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/20/2018
---
# <a name="get-started-with-signalr-on-aspnet-core"></a><span data-ttu-id="aba03-103">Empezar a trabajar con SignalR en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="aba03-103">Get started with SignalR on ASP.NET Core</span></span>

<span data-ttu-id="aba03-104">Por [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="aba03-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

[!INCLUDE [2.1 preview notice](~/includes/2.1.md)]

<span data-ttu-id="aba03-105">Este tutorial le enseña los fundamentos de la creación de una aplicación en tiempo real con SignalR para ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="aba03-105">This tutorial teaches the basics of building a real-time app using SignalR for ASP.NET Core.</span></span>

   ![Soluciones](get-started/_static/signalr-get-started-finished.png)

<span data-ttu-id="aba03-107">Este tutorial muestra las tareas de desarrollo de SignalR siguientes:</span><span class="sxs-lookup"><span data-stu-id="aba03-107">This tutorial demonstrates the following SignalR development tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="aba03-108">Crear un SignalR en la aplicación web de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="aba03-108">Create a SignalR on ASP.NET Core web app.</span></span>
> * <span data-ttu-id="aba03-109">Cree un concentrador de SignalR para insertar contenido en los clientes.</span><span class="sxs-lookup"><span data-stu-id="aba03-109">Create a SignalR hub to push content to clients.</span></span>
> * <span data-ttu-id="aba03-110">Modificar el `Startup` clase y configurar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="aba03-110">Modify the `Startup` class and configure the app.</span></span>

<span data-ttu-id="aba03-111">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started/sample/) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="aba03-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

# <a name="prerequisites"></a><span data-ttu-id="aba03-112">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="aba03-112">Prerequisites</span></span>

<span data-ttu-id="aba03-113">Instalar el software siguiente:</span><span class="sxs-lookup"><span data-stu-id="aba03-113">Install the following software:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="aba03-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="aba03-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="aba03-115">[Obtener una vista previa en .NET core 2.1.0 SDK 2](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview2) o posterior</span><span class="sxs-lookup"><span data-stu-id="aba03-115">[.NET Core 2.1.0 Preview 2 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview2) or later</span></span>
* <span data-ttu-id="aba03-116">[Visual Studio de 2017](https://www.visualstudio.com/downloads/) versión 15.7 o posterior con el **ASP.NET y desarrollo web** carga de trabajo</span><span class="sxs-lookup"><span data-stu-id="aba03-116">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.7 or later with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="aba03-117">NPM</span><span class="sxs-lookup"><span data-stu-id="aba03-117">npm</span></span>](https://www.npmjs.com/get-npm)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="aba03-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="aba03-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="aba03-119">[Obtener una vista previa en .NET core 2.1.0 SDK 2](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview2) o posterior</span><span class="sxs-lookup"><span data-stu-id="aba03-119">[.NET Core 2.1.0 Preview 2 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview2) or later</span></span>
* [<span data-ttu-id="aba03-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="aba03-120">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="aba03-121">C# para el código de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="aba03-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [<span data-ttu-id="aba03-122">NPM</span><span class="sxs-lookup"><span data-stu-id="aba03-122">npm</span></span>](https://www.npmjs.com/get-npm)

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a><span data-ttu-id="aba03-123">Cree un proyecto de ASP.NET Core que hospeda el servidor y cliente de SignalR</span><span class="sxs-lookup"><span data-stu-id="aba03-123">Create an ASP.NET Core project that hosts SignalR client and server</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="aba03-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="aba03-124">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="aba03-125">Use la **archivo** > **nuevo proyecto** menú opción y elija **aplicación Web de ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="aba03-125">Use the **File** > **New Project** menu option and choose **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="aba03-126">Denomine el proyecto *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="aba03-126">Name the project *SignalRChat*.</span></span>

   ![Cuadro de diálogo nuevo proyecto en Visual Studio](get-started/_static/signalr-new-project-dialog.png)

2. <span data-ttu-id="aba03-128">Seleccione **aplicación Web** para crear un proyecto con páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="aba03-128">Select **Web Application** to create a project using Razor Pages.</span></span> <span data-ttu-id="aba03-129">A continuación, seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="aba03-129">Then select **OK**.</span></span> <span data-ttu-id="aba03-130">Asegúrese de que **ASP.NET Core 2.1** está seleccionada en el selector de framework, aunque se ejecute SignalR en versiones anteriores de .NET.</span><span class="sxs-lookup"><span data-stu-id="aba03-130">Be sure that **ASP.NET Core 2.1** is selected from the framework selector, though SignalR runs on older versions of .NET.</span></span>

   ![Cuadro de diálogo nuevo proyecto en Visual Studio](get-started/_static/signalr-new-project-choose-type.png)

<span data-ttu-id="aba03-132">Visual Studio incluye la `Microsoft.AspNetCore.SignalR` paquete que contiene sus bibliotecas de servidor como parte de su **aplicación Web de ASP.NET Core** plantilla.</span><span class="sxs-lookup"><span data-stu-id="aba03-132">Visual Studio includes the `Microsoft.AspNetCore.SignalR` package containing its server libraries as part of its **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="aba03-133">Sin embargo, debe instalarse la biblioteca de cliente de JavaScript para SignalR con *npm*.</span><span class="sxs-lookup"><span data-stu-id="aba03-133">However, the JavaScript client library for SignalR must be installed using *npm*.</span></span>

3. <span data-ttu-id="aba03-134">Ejecute los siguientes comandos el **Package Manager Console** ventana, desde la raíz del proyecto:</span><span class="sxs-lookup"><span data-stu-id="aba03-134">Run the following commands in the **Package Manager Console** window, from the project root:</span></span>

    ```console
      npm init -y
      npm install @aspnet/signalr
    ```     

4. <span data-ttu-id="aba03-135">Copia la *signalr.js* de archivos de *node_modules\\ @aspnet\signalr\dist\browser*  a la *lib* carpeta del proyecto.</span><span class="sxs-lookup"><span data-stu-id="aba03-135">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to the *lib* folder in your project.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="aba03-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="aba03-136">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="aba03-137">Desde el **Terminal integrado**, ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="aba03-137">From the **Integrated Terminal**, run the following command:</span></span>

    ```console
      dotnet new razor -o SignalRChat
    ```

2. <span data-ttu-id="aba03-138">Instalar la biblioteca de cliente de JavaScript con *npm*.</span><span class="sxs-lookup"><span data-stu-id="aba03-138">Install the JavaScript client library using *npm*.</span></span>

    ```
      npm init -y
      npm install @aspnet/signalr
    ```

-----

## <a name="create-the-signalr-hub"></a><span data-ttu-id="aba03-139">Crear el concentrador de SignalR</span><span class="sxs-lookup"><span data-stu-id="aba03-139">Create the SignalR Hub</span></span>

<span data-ttu-id="aba03-140">Un concentrador es una clase que actúa como una canalización de alto nivel que permite al cliente y servidor llamar a métodos en entre sí.</span><span class="sxs-lookup"><span data-stu-id="aba03-140">A hub is a class that serves as a high-level pipeline that allows the client and server to call methods on each other.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="aba03-141">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="aba03-141">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="aba03-142">Agregar una clase al proyecto eligiendo **archivo** > **New** > **archivo** y seleccionando **clase de Visual C#**.</span><span class="sxs-lookup"><span data-stu-id="aba03-142">Add a class to the project by choosing **File** > **New** > **File** and selecting **Visual C# Class**.</span></span>

2. <span data-ttu-id="aba03-143">Heredar de `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="aba03-143">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="aba03-144">La `Hub` clase contiene propiedades y eventos para administrar las conexiones y grupos, así como enviar y recibir datos.</span><span class="sxs-lookup"><span data-stu-id="aba03-144">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data.</span></span>

3. <span data-ttu-id="aba03-145">Crear el `SendMessage` método que envía un mensaje a todos los clientes de chat conectado.</span><span class="sxs-lookup"><span data-stu-id="aba03-145">Create the `SendMessage` method that sends a message to all connected chat clients.</span></span> <span data-ttu-id="aba03-146">Tenga en cuenta devuelve un [tarea](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), porque SignalR es asincrónica.</span><span class="sxs-lookup"><span data-stu-id="aba03-146">Notice it returns a [Task](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), because SignalR is asynchronous.</span></span> <span data-ttu-id="aba03-147">Código asincrónico se escala mejor.</span><span class="sxs-lookup"><span data-stu-id="aba03-147">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](get-started/sample/Hubs/ChatHub.cs)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="aba03-148">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="aba03-148">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="aba03-149">Abra la *SignalRChat* carpeta en el código de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="aba03-149">Open the *SignalRChat* folder in Visual Studio Code.</span></span>

2. <span data-ttu-id="aba03-150">Agregar una clase al proyecto seleccionando **archivo** > **nuevo archivo** en el menú.</span><span class="sxs-lookup"><span data-stu-id="aba03-150">Add a class to the project by selecting **File** > **New File** from the menu.</span></span>

3. <span data-ttu-id="aba03-151">Heredar de `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="aba03-151">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="aba03-152">La `Hub` clase contiene propiedades y eventos para administrar las conexiones y grupos, así como enviar y recibir datos a los clientes.</span><span class="sxs-lookup"><span data-stu-id="aba03-152">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data to clients.</span></span>

4. <span data-ttu-id="aba03-153">Agregue un método `SendMessage` a la clase.</span><span class="sxs-lookup"><span data-stu-id="aba03-153">Add a `SendMessage` method to the class.</span></span> <span data-ttu-id="aba03-154">El `SendMessage` método envía un mensaje a todos los clientes de chat conectado.</span><span class="sxs-lookup"><span data-stu-id="aba03-154">The `SendMessage` method sends a message to all connected chat clients.</span></span> <span data-ttu-id="aba03-155">Tenga en cuenta devuelve un [tarea](/dotnet/api/system.threading.tasks.task), porque SignalR es asincrónica.</span><span class="sxs-lookup"><span data-stu-id="aba03-155">Notice it returns a [Task](/dotnet/api/system.threading.tasks.task), because SignalR is asynchronous.</span></span> <span data-ttu-id="aba03-156">Código asincrónico se escala mejor.</span><span class="sxs-lookup"><span data-stu-id="aba03-156">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](get-started/sample/Hubs/ChatHub.cs?range=6-12)]

-----

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="aba03-157">Configurar el proyecto para que use SignalR</span><span class="sxs-lookup"><span data-stu-id="aba03-157">Configure the project to use SignalR</span></span>

<span data-ttu-id="aba03-158">El servidor de SignalR debe configurarse para que sepa para pasar las solicitudes para SignalR.</span><span class="sxs-lookup"><span data-stu-id="aba03-158">The SignalR server must be configured so that it knows to pass requests to SignalR.</span></span>

1. <span data-ttu-id="aba03-159">Para configurar un proyecto de SignalR, modifique el proyecto `Startup.ConfigureServices` método.</span><span class="sxs-lookup"><span data-stu-id="aba03-159">To configure a SignalR project, modify the project's `Startup.ConfigureServices` method.</span></span>

   <span data-ttu-id="aba03-160">`services.AddSignalR` Agrega SignalR como parte de la [middleware](xref:fundamentals/middleware/index) canalización.</span><span class="sxs-lookup"><span data-stu-id="aba03-160">`services.AddSignalR` adds SignalR as part of the [middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

2. <span data-ttu-id="aba03-161">Configurar las rutas a los concentradores con `UseSignalR`.</span><span class="sxs-lookup"><span data-stu-id="aba03-161">Configure routes to your hubs using `UseSignalR`.</span></span>


   [!code-csharp[Startup](get-started/sample/Startup.cs?highlight=36,56-59)]


## <a name="create-the-signalr-client-code"></a><span data-ttu-id="aba03-162">Crear el código de cliente de SignalR</span><span class="sxs-lookup"><span data-stu-id="aba03-162">Create the SignalR client code</span></span>

1. <span data-ttu-id="aba03-163">Reemplace el contenido de *Pages\Index.cshtml* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="aba03-163">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

   [!code-cshtml[Index](get-started/sample/Pages/Index.cshtml)]

   <span data-ttu-id="aba03-164">El código HTML anterior muestra el nombre y campos del mensaje y un botón de envío.</span><span class="sxs-lookup"><span data-stu-id="aba03-164">The preceding HTML displays name and message fields, and a submit button.</span></span> <span data-ttu-id="aba03-165">Tenga en cuenta las referencias de script en la parte inferior: una referencia a SignalR y *chat.js*.</span><span class="sxs-lookup"><span data-stu-id="aba03-165">Notice the script references at the bottom: a reference to SignalR and *chat.js*.</span></span>

2. <span data-ttu-id="aba03-166">Agregue un archivo de JavaScript denominado *chat.js*, a la *wwwroot\js* carpeta.</span><span class="sxs-lookup"><span data-stu-id="aba03-166">Add a JavaScript file, named *chat.js*, to the *wwwroot\js* folder.</span></span> <span data-ttu-id="aba03-167">Agregue el código siguiente a él:</span><span class="sxs-lookup"><span data-stu-id="aba03-167">Add the following code to it:</span></span>

   [!code-javascript[Index](get-started/sample/wwwroot/js/chat.js)]

## <a name="run-the-app"></a><span data-ttu-id="aba03-168">Ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="aba03-168">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="aba03-169">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="aba03-169">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="aba03-170">Seleccione **depurar** > **iniciar sin depurar** para iniciar un explorador y cargar el sitio Web de forma local.</span><span class="sxs-lookup"><span data-stu-id="aba03-170">Select **Debug** > **Start without debugging** to launch a browser and load the website locally.</span></span> <span data-ttu-id="aba03-171">Copie la dirección URL de la barra de direcciones.</span><span class="sxs-lookup"><span data-stu-id="aba03-171">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="aba03-172">Abra otra instancia del explorador (cualquier explorador) y pegue la dirección URL en la barra de direcciones.</span><span class="sxs-lookup"><span data-stu-id="aba03-172">Open another browser instance (any browser) and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="aba03-173">Elija cualquiera de estos exploradores, escriba un nombre y un mensaje y haga clic en el **enviar** botón.</span><span class="sxs-lookup"><span data-stu-id="aba03-173">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="aba03-174">El nombre y el mensaje se muestran en ambas páginas al instante.</span><span class="sxs-lookup"><span data-stu-id="aba03-174">The name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="aba03-175">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="aba03-175">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="aba03-176">Presione **Depurar** (F5) para compilar y ejecutar el programa.</span><span class="sxs-lookup"><span data-stu-id="aba03-176">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="aba03-177">Ejecutar el programa, abre una ventana del explorador.</span><span class="sxs-lookup"><span data-stu-id="aba03-177">Running the program opens a browser window.</span></span>

1. <span data-ttu-id="aba03-178">Abrir otra ventana del explorador y cargar el sitio Web de forma local en ella.</span><span class="sxs-lookup"><span data-stu-id="aba03-178">Open another browser window and load the website locally in it.</span></span>

1. <span data-ttu-id="aba03-179">Elija cualquiera de estos exploradores, escriba un nombre y un mensaje y haga clic en el **enviar** botón.</span><span class="sxs-lookup"><span data-stu-id="aba03-179">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="aba03-180">El nombre y el mensaje se muestran en ambas páginas al instante.</span><span class="sxs-lookup"><span data-stu-id="aba03-180">The name and message are displayed on both pages instantly.</span></span>

-----

  ![Soluciones](get-started/_static/signalr-get-started-finished.png)

## <a name="related-resources"></a><span data-ttu-id="aba03-182">Recursos relacionados</span><span class="sxs-lookup"><span data-stu-id="aba03-182">Related resources</span></span>

[<span data-ttu-id="aba03-183">Introducción a ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="aba03-183">Introduction to ASP.NET Core SignalR</span></span>](introduction.md)