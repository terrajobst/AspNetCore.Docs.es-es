---
title: Empezar a trabajar con SignalR en ASP.NET Core
author: rachelappel
description: "Obtenga información acerca de los conceptos básicos de la creación de una aplicación en tiempo real con SignalR para ASP.NET Core."
manager: wpickett
ms.author: rachelap
ms.custom: mvc
ms.date: 03/06/2018
ms.prod: aspnet-core
ms.technology: dotnet-signalr
ms.topic: tutorial
uid: signalr/get-started-signalr-core
ms.openlocfilehash: 79af59fc8c2ada71d764ada95a431e10f4f00f27
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/15/2018
---
# <a name="tutorial-get-started-with-signalr-for-aspnet-core"></a><span data-ttu-id="bd5b3-103">Tutorial: Introducción a SignalR para ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bd5b3-103">Tutorial: Get started with SignalR for ASP.NET Core</span></span>

<span data-ttu-id="bd5b3-104">Por [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="bd5b3-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="bd5b3-105">Este tutorial le enseña los fundamentos de la creación de una aplicación en tiempo real con SignalR para ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bd5b3-105">This tutorial teaches the basics of building a real-time app using SignalR for ASP.NET Core.</span></span>

   ![Soluciones](get-started-signalr-core/_static/signalr-get-started-finished.png)

<span data-ttu-id="bd5b3-107">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started-signalr-core/sample/) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="bd5b3-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started-signalr-core/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="bd5b3-108">Este tutorial muestra las tareas de desarrollo de SignalR siguientes:</span><span class="sxs-lookup"><span data-stu-id="bd5b3-108">This tutorial demonstrates the following SignalR development tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="bd5b3-109">Crear una aplicación web de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bd5b3-109">Create an ASP.NET Core web app.</span></span>
> * <span data-ttu-id="bd5b3-110">Cree un concentrador de SignalR para insertar contenido en los clientes.</span><span class="sxs-lookup"><span data-stu-id="bd5b3-110">Create a SignalR hub to push content to clients.</span></span>
> * <span data-ttu-id="bd5b3-111">Utilice la biblioteca de SignalR JavaScript para enviar mensajes y muestra las actualizaciones desde el concentrador.</span><span class="sxs-lookup"><span data-stu-id="bd5b3-111">Use the SignalR JavaScript library to send messages and display updates from the hub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bd5b3-112">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="bd5b3-112">Prerequisites</span></span>

<span data-ttu-id="bd5b3-113">Instalar el software siguiente:</span><span class="sxs-lookup"><span data-stu-id="bd5b3-113">Install the following software:</span></span>

* <span data-ttu-id="bd5b3-114">[Obtener una vista previa en .NET core 2.1.0 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1) o posterior</span><span class="sxs-lookup"><span data-stu-id="bd5b3-114">[.NET Core 2.1.0 Preview 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1) or later</span></span>
* <span data-ttu-id="bd5b3-115">[Visual Studio de 2017](https://www.visualstudio.com/downloads/) versión 15.6 o posterior con la carga de trabajo de desarrollo web y ASP.NET</span><span class="sxs-lookup"><span data-stu-id="bd5b3-115">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.6 or later with the ASP.NET and web development workload</span></span>
* [<span data-ttu-id="bd5b3-116">npm</span><span class="sxs-lookup"><span data-stu-id="bd5b3-116">npm</span></span>](https://www.npmjs.com/get-npm)

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a><span data-ttu-id="bd5b3-117">Cree un proyecto de ASP.NET Core que hospeda el servidor y cliente de SignalR</span><span class="sxs-lookup"><span data-stu-id="bd5b3-117">Create an ASP.NET Core project that hosts SignalR client and server</span></span>

1. <span data-ttu-id="bd5b3-118">Use la **archivo** > **nuevo proyecto** menú opción y elija **aplicación Web de ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="bd5b3-118">Use the **File** > **New Project** menu option and choose **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="bd5b3-119">Dé un nombre al proyecto `SignalRChat`.</span><span class="sxs-lookup"><span data-stu-id="bd5b3-119">Name the project `SignalRChat`.</span></span>

  ![Cuadro de diálogo nuevo proyecto en Visual Studio](get-started-signalr-core/_static/signalr-new-project-dialog.png)

2. <span data-ttu-id="bd5b3-121">Seleccione **aplicación Web** para crear un proyecto con páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="bd5b3-121">Select **Web Application** to create a project using Razor Pages.</span></span> <span data-ttu-id="bd5b3-122">A continuación, seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="bd5b3-122">Then select **Ok**.</span></span> <span data-ttu-id="bd5b3-123">Asegúrese de que **ASP.NET Core 2.1** está seleccionada en el selector de framework, aunque se ejecute SignalR en versiones anteriores de .NET.</span><span class="sxs-lookup"><span data-stu-id="bd5b3-123">Be sure that **ASP.NET Core 2.1** is selected from the framework selector, though SignalR runs on older versions of .NET.</span></span>

  ![Cuadro de diálogo nuevo proyecto en Visual Studio](get-started-signalr-core/_static/signalr-new-project-choose-type.png)

  <span data-ttu-id="bd5b3-125">Las bibliotecas que hospedan SignalR código del lado servidor se incluyen en la plantilla de proyecto.</span><span class="sxs-lookup"><span data-stu-id="bd5b3-125">The libraries that host SignalR server-side code are included in the project template.</span></span> <span data-ttu-id="bd5b3-126">Instalar el código de JavaScript de cliente por separado con [npm](https://www.npmjs.com/).</span><span class="sxs-lookup"><span data-stu-id="bd5b3-126">Install the client-side JavaScript separately with [npm](https://www.npmjs.com/).</span></span>

  ```console
   npm install @aspnet/signalr
  ```

3. <span data-ttu-id="bd5b3-127">Copia la *signalr.js* de *node_modules\\ @aspnet\signalr\dist\browser*  a la *wwwroot\lib* carpeta del proyecto.</span><span class="sxs-lookup"><span data-stu-id="bd5b3-127">Copy the *signalr.js* from *node_modules\\@aspnet\signalr\dist\browser* to the *wwwroot\lib* folder in your project.</span></span>

## <a name="create-the-signalr-hub"></a><span data-ttu-id="bd5b3-128">Crear el concentrador de SignalR</span><span class="sxs-lookup"><span data-stu-id="bd5b3-128">Create the SignalR Hub</span></span>

<span data-ttu-id="bd5b3-129">Un concentrador es una clase que actúa como una canalización de alto nivel que permite al cliente y servidor llamar a métodos en entre sí.</span><span class="sxs-lookup"><span data-stu-id="bd5b3-129">A hub is a class that serves as a high-level pipeline that allows the client and server to call methods on each other.</span></span>

1. <span data-ttu-id="bd5b3-130">Agregar una clase al proyecto eligiendo **archivo** > **New** > **archivo** y seleccionando **clase de Visual C#**.</span><span class="sxs-lookup"><span data-stu-id="bd5b3-130">Add a class to the project by choosing **File** > **New** > **File** and selecting **Visual C# Class**.</span></span> 

1. <span data-ttu-id="bd5b3-131">Heredar de `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="bd5b3-131">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="bd5b3-132">La `Hub` clase contiene propiedades y eventos para administrar las conexiones y grupos, así como enviar y recibir datos.</span><span class="sxs-lookup"><span data-stu-id="bd5b3-132">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data.</span></span>

1. <span data-ttu-id="bd5b3-133">Crear el `Send` método que envía un mensaje a todos los clientes de chat conectado.</span><span class="sxs-lookup"><span data-stu-id="bd5b3-133">Create the `Send` method that sends a message to all connected chat clients.</span></span> <span data-ttu-id="bd5b3-134">Tenga en cuenta devuelve un `Task`, porque SignalR es asincrónica.</span><span class="sxs-lookup"><span data-stu-id="bd5b3-134">Notice it returns a `Task`, because SignalR is asynchronous.</span></span> <span data-ttu-id="bd5b3-135">Código asincrónico se escala mejor.</span><span class="sxs-lookup"><span data-stu-id="bd5b3-135">Asynchronous code scales better.</span></span>

  [!code-csharp[Startup](get-started-signalr-core/sample/Hubs/ChatHub.cs?range=7-14)]

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="bd5b3-136">Configurar el proyecto para que use SignalR</span><span class="sxs-lookup"><span data-stu-id="bd5b3-136">Configure the project to use SignalR</span></span>

<span data-ttu-id="bd5b3-137">El servidor de SignalR debe configurarse para que sepa para pasar las solicitudes para SignalR.</span><span class="sxs-lookup"><span data-stu-id="bd5b3-137">The SignalR server must be configured so that it knows to pass requests to SignalR.</span></span>

1. <span data-ttu-id="bd5b3-138">Para configurar un proyecto de SignalR, modifique la `ConfigureServices` método de la aplicación `Startup` clase mediante la inserción de una llamada a `services.AddSignalR`.</span><span class="sxs-lookup"><span data-stu-id="bd5b3-138">To configure a SignalR project, modify the `ConfigureServices` method of the application's `Startup` class by inserting a call to `services.AddSignalR`.</span></span>

  <span data-ttu-id="bd5b3-139">`services.AddSignalR` Agrega SignalR como parte de la [middleware de ASP.NET Core](xref:fundamentals/middleware/index) canalización.</span><span class="sxs-lookup"><span data-stu-id="bd5b3-139">`services.AddSignalR` adds SignalR as part of the [ASP.NET Core middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

1. <span data-ttu-id="bd5b3-140">Configurar las rutas a los concentradores con `UseSignalR`.</span><span class="sxs-lookup"><span data-stu-id="bd5b3-140">Configure routes to your hubs using `UseSignalR`.</span></span>

  [!code-csharp[Startup](get-started-signalr-core/sample/Startup.cs?highlight=22,40-43)]

## <a name="create-the-signalr-client-code"></a><span data-ttu-id="bd5b3-141">Crear el código de cliente de SignalR</span><span class="sxs-lookup"><span data-stu-id="bd5b3-141">Create the SignalR client code</span></span>

1. <span data-ttu-id="bd5b3-142">Reemplace el contenido de *Pages\Index.cshtml* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="bd5b3-142">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](get-started-signalr-core/sample/Pages/Index.cshtml)]

  <span data-ttu-id="bd5b3-143">El código HTML anterior muestra el nombre y campos del mensaje y un botón de envío.</span><span class="sxs-lookup"><span data-stu-id="bd5b3-143">The preceding HTML displays name and message fields, and a submit button.</span></span> <span data-ttu-id="bd5b3-144">Tenga en cuenta las referencias de script en la parte inferior: una referencia a SignalR y *chat.js*.</span><span class="sxs-lookup"><span data-stu-id="bd5b3-144">Notice the script references at the bottom: a reference to SignalR and *chat.js*.</span></span>

1. <span data-ttu-id="bd5b3-145">Agregue un archivo de JavaScript a la *wwwroot\js* carpeta denominada *chat.js* y agregue el siguiente código en él:</span><span class="sxs-lookup"><span data-stu-id="bd5b3-145">Add a JavaScript file to the *wwwroot\js* folder named *chat.js* and add the following code to it:</span></span>

  [!code-javascript[Index](get-started-signalr-core/sample/wwwroot/js/chat.js)]

## <a name="run-the-app"></a><span data-ttu-id="bd5b3-146">Ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="bd5b3-146">Run the app</span></span>

1. <span data-ttu-id="bd5b3-147">Seleccione **depurar** > **iniciar sin depurar** para iniciar un explorador y cargar el sitio Web de forma local.</span><span class="sxs-lookup"><span data-stu-id="bd5b3-147">Select **Debug** > **Start without debugging** to launch a browser and load the website locally.</span></span> <span data-ttu-id="bd5b3-148">Copie la dirección URL de la barra de direcciones.</span><span class="sxs-lookup"><span data-stu-id="bd5b3-148">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="bd5b3-149">Abra otra instancia del explorador (cualquier explorador) y pegue la dirección URL en la barra de direcciones.</span><span class="sxs-lookup"><span data-stu-id="bd5b3-149">Open another browser instance (any browser) and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="bd5b3-150">Elija cualquiera de estos exploradores, escriba un nombre y un mensaje y haga clic en el **enviar** botón.</span><span class="sxs-lookup"><span data-stu-id="bd5b3-150">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="bd5b3-151">El nombre y el mensaje se muestran en ambas páginas al instante.</span><span class="sxs-lookup"><span data-stu-id="bd5b3-151">The name and message are displayed on both pages instantly.</span></span>

  ![Soluciones](get-started-signalr-core/_static/signalr-get-started-finished.png)
