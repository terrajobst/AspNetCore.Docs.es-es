---
title: Introducción a SignalR de ASP.NET Core
author: tdykstra
description: En este tutorial, creará una aplicación de chat en la que se usa SignalR de ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/31/2018
uid: tutorials/signalr
ms.openlocfilehash: fcfe2fa6cc88b9eee1389e171fa5eb7711b4f14f
ms.sourcegitcommit: fc2486ddbeb15ab4969168d99b3fe0fbe91e8661
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/01/2018
ms.locfileid: "50758133"
---
# <a name="tutorial-get-started-with-aspnet-core-signalr"></a><span data-ttu-id="2fcf1-103">Tutorial: Introducción a SignalR de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2fcf1-103">Tutorial: Get started with ASP.NET Core SignalR</span></span>

<span data-ttu-id="2fcf1-104">En este tutorial se describen los conceptos básicos de la creación de una aplicación en tiempo real con SignalR.</span><span class="sxs-lookup"><span data-stu-id="2fcf1-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="2fcf1-105">Aprenderá a:</span><span class="sxs-lookup"><span data-stu-id="2fcf1-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="2fcf1-106">Cree un proyecto web.</span><span class="sxs-lookup"><span data-stu-id="2fcf1-106">Create a web project.</span></span>
> * <span data-ttu-id="2fcf1-107">Agregar la biblioteca cliente de SignalR.</span><span class="sxs-lookup"><span data-stu-id="2fcf1-107">Add the SignalR client library.</span></span>
> * <span data-ttu-id="2fcf1-108">Crear un concentrador de SignalR.</span><span class="sxs-lookup"><span data-stu-id="2fcf1-108">Create a SignalR hub.</span></span>
> * <span data-ttu-id="2fcf1-109">Configurar el proyecto para usar SignalR.</span><span class="sxs-lookup"><span data-stu-id="2fcf1-109">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="2fcf1-110">Agregar código que envía mensajes desde cualquier cliente a todos los clientes conectados.</span><span class="sxs-lookup"><span data-stu-id="2fcf1-110">Add code that sends messages from any client to all connected clients.</span></span>

<span data-ttu-id="2fcf1-111">Al final, tendrá una aplicación de chat funcional:</span><span class="sxs-lookup"><span data-stu-id="2fcf1-111">At the end, you'll have a working chat app:</span></span>

![Aplicación de ejemplo de SignalR](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="2fcf1-113">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="2fcf1-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2fcf1-114">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="2fcf1-114">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2fcf1-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2fcf1-115">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="2fcf1-116">[Visual Studio 2017 versión 15.8 o posterior](https://www.visualstudio.com/downloads/) con la carga de trabajo **ASP.NET y desarrollo web**</span><span class="sxs-lookup"><span data-stu-id="2fcf1-116">[Visual Studio 2017 version 15.8 or later](https://www.visualstudio.com/downloads/) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="2fcf1-117">.NET Core SDK 2.1 o posterior</span><span class="sxs-lookup"><span data-stu-id="2fcf1-117">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2fcf1-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2fcf1-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="2fcf1-119">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2fcf1-119">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="2fcf1-120">.NET Core SDK 2.1 o posterior</span><span class="sxs-lookup"><span data-stu-id="2fcf1-120">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="2fcf1-121">C# para Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2fcf1-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="2fcf1-122">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="2fcf1-122">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* [<span data-ttu-id="2fcf1-123">Visual Studio para Mac, versión 7.5.4 o posterior</span><span class="sxs-lookup"><span data-stu-id="2fcf1-123">Visual Studio for Mac version 7.5.4 or later</span></span>](https://www.visualstudio.com/downloads/)
* <span data-ttu-id="2fcf1-124">[SDK de .NET Core 2.1 o posterior](https://www.microsoft.com/net/download/all) (incluido en la instalación de Visual Studio)</span><span class="sxs-lookup"><span data-stu-id="2fcf1-124">[.NET Core SDK 2.1 or later](https://www.microsoft.com/net/download/all) (included in the Visual Studio install)</span></span>

---

## <a name="create-a-web-project"></a><span data-ttu-id="2fcf1-125">Creación de un proyecto web</span><span class="sxs-lookup"><span data-stu-id="2fcf1-125">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2fcf1-126">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2fcf1-126">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="2fcf1-127">En el menú, seleccione **Archivo > Nuevo proyecto**.</span><span class="sxs-lookup"><span data-stu-id="2fcf1-127">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="2fcf1-128">En el cuadro de diálogo **Nuevo proyecto**, seleccione **Instalado > Visual C# > Web > Aplicación web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="2fcf1-128">In the **New Project** dialog, select **Installed > Visual C# > Web > ASP.NET Core Web Application**.</span></span> <span data-ttu-id="2fcf1-129">Asigne al proyecto el nombre *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="2fcf1-129">Name the project *SignalRChat*.</span></span>

  ![Cuadro de diálogo Nuevo proyecto en Visual Studio](signalr/_static/signalr-new-project-dialog.png)

* <span data-ttu-id="2fcf1-131">Seleccione **Aplicación web** para crear un proyecto en el que se use Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="2fcf1-131">Select **Web Application** to create a project that uses Razor Pages.</span></span>

* <span data-ttu-id="2fcf1-132">Seleccione una plataforma de destino de **.NET Core**, seleccione **ASP.NET Core 2.1** y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="2fcf1-132">Select a target framework of **.NET Core**, select **ASP.NET Core 2.1**, and click **OK**.</span></span>

  ![Cuadro de diálogo Nuevo proyecto en Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2fcf1-134">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2fcf1-134">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="2fcf1-135">Abra una carpeta que pueda usar para un proyecto nuevo.</span><span class="sxs-lookup"><span data-stu-id="2fcf1-135">Open a folder that you can use for a new project.</span></span>

* <span data-ttu-id="2fcf1-136">En el [terminal integrado](https://code.visualstudio.com/docs/editor/integrated-terminal), ejecute el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="2fcf1-136">In the [Integrated Terminal](https://code.visualstudio.com/docs/editor/integrated-terminal), run the following command:</span></span>

   ```console
   dotnet new webapp -o SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="2fcf1-137">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="2fcf1-137">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="2fcf1-138">En el menú, seleccione **Archivo > Nueva solución**.</span><span class="sxs-lookup"><span data-stu-id="2fcf1-138">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="2fcf1-139">Seleccione **.NET Core > Aplicación > Aplicación web ASP.NET Core** (no seleccione **Aplicación web de ASP.NET Core (MVC)**).</span><span class="sxs-lookup"><span data-stu-id="2fcf1-139">Select **.NET Core > App > ASP.NET Core Web App** (Don't select **ASP.NET Core Web App (MVC)**).</span></span>

* <span data-ttu-id="2fcf1-140">Seleccione **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="2fcf1-140">Select **Next**.</span></span>

* <span data-ttu-id="2fcf1-141">Asigne el nombre *SignalRChat* al proyecto y, después, haga clic en **Crear**.</span><span class="sxs-lookup"><span data-stu-id="2fcf1-141">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="2fcf1-142">Agregar la biblioteca cliente de SignalR</span><span class="sxs-lookup"><span data-stu-id="2fcf1-142">Add the SignalR client library</span></span>

<span data-ttu-id="2fcf1-143">La biblioteca de servidor de SignalR se incluye en el metapaquete `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="2fcf1-143">The SignalR server library is included in the `Microsoft.AspNetCore.App` metapackage.</span></span> <span data-ttu-id="2fcf1-144">La biblioteca cliente de JavaScript no se incluye automáticamente en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="2fcf1-144">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="2fcf1-145">En este tutorial, usará el Administrador de bibliotecas (LibMan) para obtener la biblioteca cliente de *unpkg*.</span><span class="sxs-lookup"><span data-stu-id="2fcf1-145">For this tutorial, you use Library Manager (LibMan) to get the client library from *unpkg*.</span></span> <span data-ttu-id="2fcf1-146">unpkg es una red de entrega de contenido (CDN) que puede entregar todo lo que encuentre en npm, el administrador de paquetes de Node.js.</span><span class="sxs-lookup"><span data-stu-id="2fcf1-146">unpkg is a content delivery network (CDN)) that can deliver anything found in npm, the Node.js package manager.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2fcf1-147">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2fcf1-147">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="2fcf1-148">En el **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y seleccione **Agregar** > **Client-Side Library** (Biblioteca del lado cliente).</span><span class="sxs-lookup"><span data-stu-id="2fcf1-148">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>

* <span data-ttu-id="2fcf1-149">En el cuadro de diálogo **Add Client-Side Library** (Agregar biblioteca del lado cliente), en **Proveedor**, seleccione **unpkg**.</span><span class="sxs-lookup"><span data-stu-id="2fcf1-149">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span> 

* <span data-ttu-id="2fcf1-150">En **Biblioteca**, escriba `@aspnet/signalr@1` y seleccione la versión más reciente que no sea una versión preliminar.</span><span class="sxs-lookup"><span data-stu-id="2fcf1-150">For **Library**, enter `@aspnet/signalr@1`, and select the latest version that isn't preview.</span></span>

  ![Cuadro de diálogo Add Client-Side Library (Agregar biblioteca del lado cliente): selección de la biblioteca](signalr/_static/libman1.png)

* <span data-ttu-id="2fcf1-152">Seleccione **Choose specific files** (Elegir archivos específicos), expanda la carpeta *dist/browser* y seleccione *signalr.js* y *signalr.min.js*.</span><span class="sxs-lookup"><span data-stu-id="2fcf1-152">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span>

* <span data-ttu-id="2fcf1-153">Establezca **Ubicación de destino** en *wwwroot/lib/signalr/* y seleccione **Instalar**.</span><span class="sxs-lookup"><span data-stu-id="2fcf1-153">Set **Target Location** to *wwwroot/lib/signalr/*, and select **Install**.</span></span>

  ![Cuadro de diálogo Add Client-Side Library (Agregar biblioteca del lado cliente): selección de archivos y destino](signalr/_static/libman2.png)

  <span data-ttu-id="2fcf1-155">LibMan crea una carpeta *wwwroot/lib/signalr* y copia en ella los archivos seleccionados.</span><span class="sxs-lookup"><span data-stu-id="2fcf1-155">LibMan creates a *wwwroot/lib/signalr* folder and copies the selected files to it.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2fcf1-156">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2fcf1-156">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="2fcf1-157">En el **terminal integrado**, ejecute el comando siguiente para instalar LibMan.</span><span class="sxs-lookup"><span data-stu-id="2fcf1-157">In the **Integrated Terminal**, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="2fcf1-158">Vaya a la carpeta de proyecto (la que contiene el archivo *SignalRChat.csproj*).</span><span class="sxs-lookup"><span data-stu-id="2fcf1-158">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="2fcf1-159">Ejecute el comando siguiente para obtener la biblioteca cliente de SignalR con LibMan.</span><span class="sxs-lookup"><span data-stu-id="2fcf1-159">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="2fcf1-160">Puede que deba esperar unos segundos antes de ver la salida.</span><span class="sxs-lookup"><span data-stu-id="2fcf1-160">You might have to wait a few seconds before seeing output.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="2fcf1-161">Los parámetros especifican las opciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="2fcf1-161">The parameters specify the following options:</span></span>
  * <span data-ttu-id="2fcf1-162">Use el proveedor unpkg.</span><span class="sxs-lookup"><span data-stu-id="2fcf1-162">Use the unpkg provider.</span></span>
  * <span data-ttu-id="2fcf1-163">Copie los archivos en el destino *wwwroot/lib/signalr*.</span><span class="sxs-lookup"><span data-stu-id="2fcf1-163">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="2fcf1-164">Copie solo los archivos especificados.</span><span class="sxs-lookup"><span data-stu-id="2fcf1-164">Copy only the specified files.</span></span>

  <span data-ttu-id="2fcf1-165">La salida se parece al ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="2fcf1-165">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="2fcf1-166">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="2fcf1-166">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="2fcf1-167">En el **terminal**, ejecute el siguiente comando para instalar LibMan.</span><span class="sxs-lookup"><span data-stu-id="2fcf1-167">In the **Terminal**, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="2fcf1-168">Vaya a la carpeta de proyecto (la que contiene el archivo *SignalRChat.csproj*).</span><span class="sxs-lookup"><span data-stu-id="2fcf1-168">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="2fcf1-169">Ejecute el comando siguiente para obtener la biblioteca cliente de SignalR con LibMan.</span><span class="sxs-lookup"><span data-stu-id="2fcf1-169">Run the following command to get the SignalR client library by using LibMan.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="2fcf1-170">Los parámetros especifican las opciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="2fcf1-170">The parameters specify the following options:</span></span>
  * <span data-ttu-id="2fcf1-171">Use el proveedor unpkg.</span><span class="sxs-lookup"><span data-stu-id="2fcf1-171">Use the unpkg provider.</span></span>
  * <span data-ttu-id="2fcf1-172">Copie los archivos en el destino *wwwroot/lib/signalr*.</span><span class="sxs-lookup"><span data-stu-id="2fcf1-172">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="2fcf1-173">Copie solo los archivos especificados.</span><span class="sxs-lookup"><span data-stu-id="2fcf1-173">Copy only the specified files.</span></span>

  <span data-ttu-id="2fcf1-174">La salida se parece al ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="2fcf1-174">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

---

## <a name="create-a-signalr-hub"></a><span data-ttu-id="2fcf1-175">Creación de un concentrador de SignalR</span><span class="sxs-lookup"><span data-stu-id="2fcf1-175">Create a SignalR hub</span></span>

<span data-ttu-id="2fcf1-176">Un *concentrador* es una clase que actúa como una canalización general que controla la comunicación entre el cliente y el servidor.</span><span class="sxs-lookup"><span data-stu-id="2fcf1-176">A *hub* is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="2fcf1-177">En la carpeta del proyecto SignalRChat, cree una carpeta *Hubs*.</span><span class="sxs-lookup"><span data-stu-id="2fcf1-177">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="2fcf1-178">En la carpeta *Hubs*, cree un archivo *ChatHub.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="2fcf1-178">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

  <span data-ttu-id="2fcf1-179">La clase `ChatHub` hereda de la clase `Hub` de SignalR.</span><span class="sxs-lookup"><span data-stu-id="2fcf1-179">The `ChatHub` class inherits from the SignalR `Hub` class.</span></span> <span data-ttu-id="2fcf1-180">La clase `Hub` administra las conexiones, los grupos y la mensajería.</span><span class="sxs-lookup"><span data-stu-id="2fcf1-180">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="2fcf1-181">Cualquier cliente conectado puede llamar al método `SendMessage`.</span><span class="sxs-lookup"><span data-stu-id="2fcf1-181">The `SendMessage` method can be called by any connected client.</span></span> <span data-ttu-id="2fcf1-182">Envía el mensaje recibido a todos los clientes.</span><span class="sxs-lookup"><span data-stu-id="2fcf1-182">It sends the received message to all clients.</span></span> <span data-ttu-id="2fcf1-183">El código de SignalR es asincrónico para proporcionar la máxima escalabilidad.</span><span class="sxs-lookup"><span data-stu-id="2fcf1-183">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-signalr"></a><span data-ttu-id="2fcf1-184">Configuración de SignalR</span><span class="sxs-lookup"><span data-stu-id="2fcf1-184">Configure SignalR</span></span>

<span data-ttu-id="2fcf1-185">El servidor de SignalR se debe configurar para que pase las solicitudes de SignalR a SignalR.</span><span class="sxs-lookup"><span data-stu-id="2fcf1-185">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="2fcf1-186">Agregue el código resaltado siguiente al archivo *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="2fcf1-186">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=7,33,52-55)]

  <span data-ttu-id="2fcf1-187">Estos cambios agregan SignalR al sistema de inserción de dependencias de ASP.NET Core y a la canalización de software intermedio.</span><span class="sxs-lookup"><span data-stu-id="2fcf1-187">These changes add SignalR to the ASP.NET Core dependency injection system and the middleware pipeline.</span></span>

## <a name="add-signalr-client-code"></a><span data-ttu-id="2fcf1-188">Adición del código de cliente de SignalR</span><span class="sxs-lookup"><span data-stu-id="2fcf1-188">Add SignalR client code</span></span>

* <span data-ttu-id="2fcf1-189">Reemplace el contenido de *Pages/Index.cshtml* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="2fcf1-189">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

  <span data-ttu-id="2fcf1-190">El código anterior:</span><span class="sxs-lookup"><span data-stu-id="2fcf1-190">The preceding code:</span></span>

  * <span data-ttu-id="2fcf1-191">Crea cuadros de texto para el nombre y el mensaje de texto, y un botón de envío.</span><span class="sxs-lookup"><span data-stu-id="2fcf1-191">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="2fcf1-192">Crea una lista con `id="messagesList"` para mostrar los mensajes que se reciben desde el concentrador SignalR.</span><span class="sxs-lookup"><span data-stu-id="2fcf1-192">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="2fcf1-193">Incluye las referencias de script en SignalR y el código de aplicación de *chat.js* que se va a crear en el paso siguiente.</span><span class="sxs-lookup"><span data-stu-id="2fcf1-193">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="2fcf1-194">En la carpeta *wwwroot/js*, cree un archivo *chat.js* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="2fcf1-194">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

  <span data-ttu-id="2fcf1-195">El código anterior:</span><span class="sxs-lookup"><span data-stu-id="2fcf1-195">The preceding code:</span></span>

  * <span data-ttu-id="2fcf1-196">Crea e inicia una conexión.</span><span class="sxs-lookup"><span data-stu-id="2fcf1-196">Creates and starts a connection.</span></span>
  * <span data-ttu-id="2fcf1-197">Agrega al botón de envío un controlador que envía mensajes al concentrador.</span><span class="sxs-lookup"><span data-stu-id="2fcf1-197">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="2fcf1-198">Agrega al objeto de conexión un controlador que recibe mensajes desde el concentrador y los agrega a la lista.</span><span class="sxs-lookup"><span data-stu-id="2fcf1-198">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="2fcf1-199">Ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="2fcf1-199">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2fcf1-200">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2fcf1-200">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="2fcf1-201">Presione **CTRL+F5** para ejecutar la aplicación sin depurar.</span><span class="sxs-lookup"><span data-stu-id="2fcf1-201">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2fcf1-202">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2fcf1-202">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="2fcf1-203">Presione **CTRL+F5** para ejecutar la aplicación sin depurar.</span><span class="sxs-lookup"><span data-stu-id="2fcf1-203">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="2fcf1-204">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="2fcf1-204">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="2fcf1-205">En el menú, seleccione **Ejecutar > Iniciar sin depurar**.</span><span class="sxs-lookup"><span data-stu-id="2fcf1-205">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="2fcf1-206">Copie la dirección URL de la barra de direcciones, abra otra instancia o pestaña del explorador, y pegue la dirección URL en la barra de direcciones.</span><span class="sxs-lookup"><span data-stu-id="2fcf1-206">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="2fcf1-207">Elija cualquier explorador, escriba un nombre y un mensaje, y haga clic en el botón **Enviar**.</span><span class="sxs-lookup"><span data-stu-id="2fcf1-207">Choose either browser, enter a name and message, and select the **Send** button.</span></span>

  <span data-ttu-id="2fcf1-208">El nombre y el mensaje se muestran en ambas páginas al instante.</span><span class="sxs-lookup"><span data-stu-id="2fcf1-208">The name and message are displayed on both pages instantly.</span></span>

  ![Aplicación de ejemplo de SignalR](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> <span data-ttu-id="2fcf1-210">Si la aplicación no funciona, abra las herramientas para desarrolladores del explorador (F12) y vaya a la consola.</span><span class="sxs-lookup"><span data-stu-id="2fcf1-210">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="2fcf1-211">Es posible que vea errores relacionados con el código HTML y JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2fcf1-211">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="2fcf1-212">Por ejemplo, suponga que coloca *signalr.js* en una carpeta distinta a la indicada.</span><span class="sxs-lookup"><span data-stu-id="2fcf1-212">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="2fcf1-213">En ese caso, la referencia a ese archivo no funcionará y verá un error 404 en la consola.</span><span class="sxs-lookup"><span data-stu-id="2fcf1-213">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
> <span data-ttu-id="2fcf1-214">![Error: signalr.js no encontrado](signalr/_static/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="2fcf1-214">![signalr.js not found error](signalr/_static/f12-console.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="2fcf1-215">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="2fcf1-215">Next steps</span></span>

<span data-ttu-id="2fcf1-216">En este tutorial ha aprendido a:</span><span class="sxs-lookup"><span data-stu-id="2fcf1-216">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="2fcf1-217">Crear un proyecto de aplicación web.</span><span class="sxs-lookup"><span data-stu-id="2fcf1-217">Create a web app project.</span></span>
> * <span data-ttu-id="2fcf1-218">Agregar la biblioteca cliente de SignalR.</span><span class="sxs-lookup"><span data-stu-id="2fcf1-218">Add the SignalR client library.</span></span>
> * <span data-ttu-id="2fcf1-219">Crear un concentrador de SignalR.</span><span class="sxs-lookup"><span data-stu-id="2fcf1-219">Create a SignalR hub.</span></span>
> * <span data-ttu-id="2fcf1-220">Configurar el proyecto para usar SignalR.</span><span class="sxs-lookup"><span data-stu-id="2fcf1-220">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="2fcf1-221">Agregar código que usa el concentrador para enviar mensajes desde cualquier cliente a todos los clientes conectados.</span><span class="sxs-lookup"><span data-stu-id="2fcf1-221">Add code that uses the hub to send messages from any client to all connected clients.</span></span>

<span data-ttu-id="2fcf1-222">Para obtener más información sobre SignalR, vea la introducción:</span><span class="sxs-lookup"><span data-stu-id="2fcf1-222">To learn more about SignalR, see the introduction:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="2fcf1-223">Introducción a SignalR de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2fcf1-223">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
