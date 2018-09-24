---
title: 'Tutorial: Introducción a SignalR en ASP.NET Core'
author: tdykstra
description: En este tutorial, creará una aplicación de chat en la que se usa SignalR para ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/31/2018
uid: tutorials/signalr
ms.openlocfilehash: 8581628b3f0cf878b8cc1d0684046d22a8374af9
ms.sourcegitcommit: c12ebdab65853f27fbb418204646baf6ce69515e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/21/2018
ms.locfileid: "46523225"
---
# <a name="tutorial-get-started-with-signalr-on-aspnet-core"></a><span data-ttu-id="5e609-103">Tutorial: Introducción a SignalR en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5e609-103">Tutorial: Get started with SignalR on ASP.NET Core</span></span>

<span data-ttu-id="5e609-104">En este tutorial se describen los conceptos básicos de la creación de una aplicación en tiempo real con SignalR.</span><span class="sxs-lookup"><span data-stu-id="5e609-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="5e609-105">Aprenderá a:</span><span class="sxs-lookup"><span data-stu-id="5e609-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="5e609-106">Crear una aplicación web en la que se usa SignalR en ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5e609-106">Create a web app that uses SignalR on ASP.NET Core.</span></span>
> * <span data-ttu-id="5e609-107">Crear un concentrador SignalR en el servidor.</span><span class="sxs-lookup"><span data-stu-id="5e609-107">Create a SignalR hub on the server.</span></span>
> * <span data-ttu-id="5e609-108">Conectarse con el concentrador SignalR desde clientes de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="5e609-108">Connect to the SignalR hub from JavaScript clients.</span></span>
> * <span data-ttu-id="5e609-109">Usar el concentrador para enviar mensajes desde cualquier cliente a todos los clientes conectados.</span><span class="sxs-lookup"><span data-stu-id="5e609-109">Use the hub to send messages from any client to all connected clients.</span></span>

<span data-ttu-id="5e609-110">Al final, tendrá una aplicación de chat funcional:</span><span class="sxs-lookup"><span data-stu-id="5e609-110">At the end, you'll have a working chat app:</span></span>

![Aplicación de ejemplo de SignalR](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="5e609-112">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="5e609-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5e609-113">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="5e609-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5e609-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5e609-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="5e609-115">[Visual Studio 2017 versión 15.8 o posterior](https://www.visualstudio.com/downloads/) con la carga de trabajo **ASP.NET y desarrollo web**</span><span class="sxs-lookup"><span data-stu-id="5e609-115">[Visual Studio 2017 version 15.8 or later](https://www.visualstudio.com/downloads/) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="5e609-116">.NET Core SDK 2.1 o posterior</span><span class="sxs-lookup"><span data-stu-id="5e609-116">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5e609-117">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5e609-117">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="5e609-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5e609-118">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="5e609-119">.NET Core SDK 2.1 o posterior</span><span class="sxs-lookup"><span data-stu-id="5e609-119">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="5e609-120">C# para Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5e609-120">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="5e609-121">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="5e609-121">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* [<span data-ttu-id="5e609-122">Visual Studio para Mac, versión 7.5.4 o posterior</span><span class="sxs-lookup"><span data-stu-id="5e609-122">Visual Studio for Mac version 7.5.4 or later</span></span>](https://www.visualstudio.com/downloads/)
* <span data-ttu-id="5e609-123">[SDK de .NET Core 2.1 o posterior](https://www.microsoft.com/net/download/all) (incluido en la instalación de Visual Studio)</span><span class="sxs-lookup"><span data-stu-id="5e609-123">[.NET Core SDK 2.1 or later](https://www.microsoft.com/net/download/all) (included in the Visual Studio install)</span></span>

---

## <a name="create-the-project"></a><span data-ttu-id="5e609-124">Crear el proyecto</span><span class="sxs-lookup"><span data-stu-id="5e609-124">Create the project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5e609-125">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5e609-125">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="5e609-126">En el menú, seleccione **Archivo > Nuevo proyecto**.</span><span class="sxs-lookup"><span data-stu-id="5e609-126">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="5e609-127">En el cuadro de diálogo **Nuevo proyecto**, seleccione **Instalado > Visual C# > Web > Aplicación web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="5e609-127">In the **New Project** dialog, select **Installed > Visual C# > Web > ASP.NET Core Web Application**.</span></span> <span data-ttu-id="5e609-128">Asigne al proyecto el nombre *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="5e609-128">Name the project *SignalRChat*.</span></span>

  ![Cuadro de diálogo Nuevo proyecto en Visual Studio](signalr/_static/signalr-new-project-dialog.png)

* <span data-ttu-id="5e609-130">Seleccione **Aplicación web** para crear un proyecto en el que se use Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="5e609-130">Select **Web Application** to create a project that uses Razor Pages.</span></span>

* <span data-ttu-id="5e609-131">Seleccione una plataforma de destino de **.NET Core**, seleccione **ASP.NET Core 2.1** y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="5e609-131">Select a target framework of **.NET Core**, select **ASP.NET Core 2.1**, and click **OK**.</span></span>

  ![Cuadro de diálogo Nuevo proyecto en Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5e609-133">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5e609-133">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="5e609-134">Abra una carpeta que pueda usar para un proyecto nuevo.</span><span class="sxs-lookup"><span data-stu-id="5e609-134">Open a folder that you can use for a new project.</span></span>

* <span data-ttu-id="5e609-135">En el **terminal integrado**, ejecute el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="5e609-135">In the **Integrated Terminal**, run the following command:</span></span>

   ```console
   dotnet new webapp -o SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="5e609-136">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="5e609-136">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="5e609-137">En el menú, seleccione **Archivo > Nueva solución**.</span><span class="sxs-lookup"><span data-stu-id="5e609-137">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="5e609-138">Seleccione **.NET Core > Aplicación > Aplicación web ASP.NET Core** (no seleccione **Aplicación web de ASP.NET Core (MVC)**).</span><span class="sxs-lookup"><span data-stu-id="5e609-138">Select **.NET Core > App > ASP.NET Core Web App** (Don't select **ASP.NET Core Web App (MVC)**).</span></span>

* <span data-ttu-id="5e609-139">Seleccione **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="5e609-139">Select **Next**.</span></span>

* <span data-ttu-id="5e609-140">Asigne el nombre *SignalRChat* al proyecto y, después, haga clic en **Crear**.</span><span class="sxs-lookup"><span data-stu-id="5e609-140">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="5e609-141">Agregar la biblioteca cliente de SignalR</span><span class="sxs-lookup"><span data-stu-id="5e609-141">Add the SignalR client library</span></span>

<span data-ttu-id="5e609-142">La biblioteca de servidor de SignalR se incluye en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="5e609-142">The SignalR server library is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="5e609-143">La biblioteca cliente de JavaScript no se incluye automáticamente en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="5e609-143">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="5e609-144">En este tutorial, usará el [Administrador de bibliotecas (LibMan)](xref:client-side/libman/index) para obtener la biblioteca cliente de *unpkg*.</span><span class="sxs-lookup"><span data-stu-id="5e609-144">For this tutorial, you use [Library Manager (LibMan)](xref:client-side/libman/index) to get the client library from *unpkg*.</span></span> <span data-ttu-id="5e609-145">[unpkg](https://unpkg.com/#/) es una [red de entrega de contenido](https://wikipedia.org/wiki/Content_delivery_network) que puede entregar todo lo que encuentre en [npm, el Administrador de paquetes Node.js](https://www.npmjs.com/get-npm).</span><span class="sxs-lookup"><span data-stu-id="5e609-145">[unpkg](https://unpkg.com/#/) is a [content delivery network](https://wikipedia.org/wiki/Content_delivery_network) that can deliver anything found in [npm, the Node.js package manager](https://www.npmjs.com/get-npm).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5e609-146">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5e609-146">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="5e609-147">En el **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y seleccione **Agregar** > **Client-Side Library** (Biblioteca del lado cliente).</span><span class="sxs-lookup"><span data-stu-id="5e609-147">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>

* <span data-ttu-id="5e609-148">En el cuadro de diálogo **Add Client-Side Library** (Agregar biblioteca del lado cliente), en **Proveedor**, seleccione **unpkg**.</span><span class="sxs-lookup"><span data-stu-id="5e609-148">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span> 

* <span data-ttu-id="5e609-149">En **Biblioteca**, escriba _@aspnet/signalr@1_ y seleccione la versión más reciente que no sea una versión preliminar.</span><span class="sxs-lookup"><span data-stu-id="5e609-149">For **Library**, enter _@aspnet/signalr@1_, and select the latest version that isn't preview.</span></span>

  ![Cuadro de diálogo Add Client-Side Library (Agregar biblioteca del lado cliente): selección de la biblioteca](signalr/_static/libman1.png)

* <span data-ttu-id="5e609-151">Seleccione **Choose specific files** (Elegir archivos específicos), expanda la carpeta *dist/browser* y seleccione *signalr.js* y *signalr.min.js*.</span><span class="sxs-lookup"><span data-stu-id="5e609-151">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span>

* <span data-ttu-id="5e609-152">Establezca **Ubicación de destino** en *wwwroot/lib/signalr/* y seleccione **Instalar**.</span><span class="sxs-lookup"><span data-stu-id="5e609-152">Set **Target Location** to *wwwroot/lib/signalr/*, and select **Install**.</span></span>

  ![Cuadro de diálogo Add Client-Side Library (Agregar biblioteca del lado cliente): selección de archivos y destino](signalr/_static/libman2.png)

  <span data-ttu-id="5e609-154">[LibMan](xref:client-side/libman/index) crea una carpeta *wwwroot/lib/signalr* y copia en ella los archivos seleccionados.</span><span class="sxs-lookup"><span data-stu-id="5e609-154">[LibMan](xref:client-side/libman/index) creates a *wwwroot/lib/signalr* folder and copies the selected files to it.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5e609-155">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5e609-155">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="5e609-156">En el **terminal integrado**, ejecute el comando siguiente para instalar LibMan.</span><span class="sxs-lookup"><span data-stu-id="5e609-156">In the **Integrated Terminal**, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="5e609-157">Vaya a la carpeta de proyecto (la que contiene el archivo *SignalRChat.csproj*).</span><span class="sxs-lookup"><span data-stu-id="5e609-157">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="5e609-158">Ejecute el comando siguiente para obtener la biblioteca cliente de SignalR con LibMan.</span><span class="sxs-lookup"><span data-stu-id="5e609-158">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="5e609-159">Puede que deba esperar unos segundos antes de ver la salida.</span><span class="sxs-lookup"><span data-stu-id="5e609-159">You might have to wait a few seconds before seeing output.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot\lib\signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="5e609-160">Los parámetros especifican las opciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="5e609-160">The parameters specify the following options:</span></span>
  * <span data-ttu-id="5e609-161">Use el proveedor unpkg.</span><span class="sxs-lookup"><span data-stu-id="5e609-161">Use the unpkg provider.</span></span>
  * <span data-ttu-id="5e609-162">Copie los archivos en el destino *wwwroot/lib/signalr*.</span><span class="sxs-lookup"><span data-stu-id="5e609-162">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="5e609-163">Copie solo los archivos especificados.</span><span class="sxs-lookup"><span data-stu-id="5e609-163">Copy only the specified files.</span></span>

  <span data-ttu-id="5e609-164">La salida se parece al ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="5e609-164">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot\lib\signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="5e609-165">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="5e609-165">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="5e609-166">En el **terminal**, ejecute el siguiente comando para instalar LibMan.</span><span class="sxs-lookup"><span data-stu-id="5e609-166">In the **Terminal**, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="5e609-167">Vaya a la carpeta de proyecto (la que contiene el archivo *SignalRChat.csproj*).</span><span class="sxs-lookup"><span data-stu-id="5e609-167">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="5e609-168">Ejecute el comando siguiente para obtener la biblioteca cliente de SignalR con LibMan.</span><span class="sxs-lookup"><span data-stu-id="5e609-168">Run the following command to get the SignalR client library by using LibMan.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot\lib\signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="5e609-169">Los parámetros especifican las opciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="5e609-169">The parameters specify the following options:</span></span>
  * <span data-ttu-id="5e609-170">Use el proveedor unpkg.</span><span class="sxs-lookup"><span data-stu-id="5e609-170">Use the unpkg provider.</span></span>
  * <span data-ttu-id="5e609-171">Copie los archivos en el destino *wwwroot/lib/signalr*.</span><span class="sxs-lookup"><span data-stu-id="5e609-171">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="5e609-172">Copie solo los archivos especificados.</span><span class="sxs-lookup"><span data-stu-id="5e609-172">Copy only the specified files.</span></span>

  <span data-ttu-id="5e609-173">La salida se parece al ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="5e609-173">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot\lib\signalr"
  ```

---

## <a name="create-the-signalr-hub"></a><span data-ttu-id="5e609-174">Crear el concentrador SignalR</span><span class="sxs-lookup"><span data-stu-id="5e609-174">Create the SignalR hub</span></span>

<span data-ttu-id="5e609-175">Un [concentrador](xref:signalr/hubs) es una clase que actúa como una canalización general que controla la comunicación entre el cliente y el servidor.</span><span class="sxs-lookup"><span data-stu-id="5e609-175">A [hub](xref:signalr/hubs) is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="5e609-176">En la carpeta del proyecto SignalRChat, cree una carpeta *Hubs*.</span><span class="sxs-lookup"><span data-stu-id="5e609-176">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="5e609-177">En la carpeta *Hubs*, cree un archivo *ChatHub.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="5e609-177">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

  <span data-ttu-id="5e609-178">La clase `ChatHub` hereda de la clase [Hub](/dotnet/api/microsoft.aspnetcore.signalr.hub) de SignalR.</span><span class="sxs-lookup"><span data-stu-id="5e609-178">The `ChatHub` class inherits from the SignalR [Hub](/dotnet/api/microsoft.aspnetcore.signalr.hub) class.</span></span> <span data-ttu-id="5e609-179">La clase `Hub` administra las conexiones, los grupos y la mensajería.</span><span class="sxs-lookup"><span data-stu-id="5e609-179">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="5e609-180">Cualquier cliente conectado puede llamar al método `SendMessage`.</span><span class="sxs-lookup"><span data-stu-id="5e609-180">The `SendMessage` method can be called by any connected client.</span></span> <span data-ttu-id="5e609-181">Envía el mensaje recibido a todos los clientes.</span><span class="sxs-lookup"><span data-stu-id="5e609-181">It sends the received message to all clients.</span></span> <span data-ttu-id="5e609-182">El código de SignalR es asincrónico para proporcionar la máxima escalabilidad.</span><span class="sxs-lookup"><span data-stu-id="5e609-182">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="5e609-183">Configurar el proyecto para que use SignalR</span><span class="sxs-lookup"><span data-stu-id="5e609-183">Configure the project to use SignalR</span></span>

<span data-ttu-id="5e609-184">El servidor de SignalR se debe configurar para que pase las solicitudes de SignalR a SignalR.</span><span class="sxs-lookup"><span data-stu-id="5e609-184">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="5e609-185">Agregue el código resaltado siguiente al archivo *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="5e609-185">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=7,33,52-55)]

  <span data-ttu-id="5e609-186">Estos cambios agregan SignalR al sistema de [inserción de dependencias](xref:fundamentals/dependency-injection) y a la canalización de [software intermedio](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="5e609-186">These changes add SignalR to the [dependency injection](xref:fundamentals/dependency-injection) system and the [middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

## <a name="create-the-signalr-client-code"></a><span data-ttu-id="5e609-187">Crear el código de cliente de SignalR</span><span class="sxs-lookup"><span data-stu-id="5e609-187">Create the SignalR client code</span></span>

* <span data-ttu-id="5e609-188">Reemplace el contenido de *Pages/Index.cshtml* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="5e609-188">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

  <span data-ttu-id="5e609-189">El código anterior:</span><span class="sxs-lookup"><span data-stu-id="5e609-189">The preceding code:</span></span>

  * <span data-ttu-id="5e609-190">Crea cuadros de texto para el nombre y el mensaje de texto, y un botón de envío.</span><span class="sxs-lookup"><span data-stu-id="5e609-190">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="5e609-191">Crea una lista con `id="messagesList"` para mostrar los mensajes que se reciben desde el concentrador SignalR.</span><span class="sxs-lookup"><span data-stu-id="5e609-191">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="5e609-192">Incluye las referencias de script en SignalR y el código de aplicación de *chat.js* que se va a crear en el paso siguiente.</span><span class="sxs-lookup"><span data-stu-id="5e609-192">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="5e609-193">En la carpeta *wwwroot/js*, cree un archivo *chat.js* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="5e609-193">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

  <span data-ttu-id="5e609-194">El código anterior:</span><span class="sxs-lookup"><span data-stu-id="5e609-194">The preceding code:</span></span>

  * <span data-ttu-id="5e609-195">Crea e inicia una conexión.</span><span class="sxs-lookup"><span data-stu-id="5e609-195">Creates and starts a connection.</span></span>
  * <span data-ttu-id="5e609-196">Agrega al botón de envío un controlador que envía mensajes al concentrador.</span><span class="sxs-lookup"><span data-stu-id="5e609-196">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="5e609-197">Agrega al objeto de conexión un controlador que recibe mensajes desde el concentrador y los agrega a la lista.</span><span class="sxs-lookup"><span data-stu-id="5e609-197">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="5e609-198">Ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="5e609-198">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5e609-199">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5e609-199">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="5e609-200">Presione **CTRL+F5** para ejecutar la aplicación sin depurar.</span><span class="sxs-lookup"><span data-stu-id="5e609-200">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5e609-201">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5e609-201">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="5e609-202">Presione **CTRL+F5** para ejecutar la aplicación sin depurar.</span><span class="sxs-lookup"><span data-stu-id="5e609-202">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="5e609-203">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="5e609-203">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="5e609-204">En el menú, seleccione **Ejecutar > Iniciar sin depurar**.</span><span class="sxs-lookup"><span data-stu-id="5e609-204">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="5e609-205">Copie la dirección URL de la barra de direcciones, abra otra instancia o pestaña del explorador, y pegue la dirección URL en la barra de direcciones.</span><span class="sxs-lookup"><span data-stu-id="5e609-205">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="5e609-206">Elija cualquier explorador, escriba un nombre y un mensaje, y haga clic en el botón **Enviar**.</span><span class="sxs-lookup"><span data-stu-id="5e609-206">Choose either browser, enter a name and message, and select the **Send** button.</span></span>

  <span data-ttu-id="5e609-207">El nombre y el mensaje se muestran en ambas páginas al instante.</span><span class="sxs-lookup"><span data-stu-id="5e609-207">The name and message are displayed on both pages instantly.</span></span>

  ![Aplicación de ejemplo de SignalR](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> <span data-ttu-id="5e609-209">Si la aplicación no funciona, abra las herramientas para desarrolladores del explorador (F12) y vaya a la consola.</span><span class="sxs-lookup"><span data-stu-id="5e609-209">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="5e609-210">Es posible que vea errores relacionados con el código HTML y JavaScript.</span><span class="sxs-lookup"><span data-stu-id="5e609-210">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="5e609-211">Por ejemplo, suponga que coloca *signalr.js* en una carpeta distinta a la indicada.</span><span class="sxs-lookup"><span data-stu-id="5e609-211">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="5e609-212">En ese caso, la referencia a ese archivo no funcionará y verá un error 404 en la consola.</span><span class="sxs-lookup"><span data-stu-id="5e609-212">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
> <span data-ttu-id="5e609-213">![Error: signalr.js no encontrado](signalr/_static/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="5e609-213">![signalr.js not found error](signalr/_static/f12-console.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="5e609-214">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="5e609-214">Next steps</span></span>

<span data-ttu-id="5e609-215">Si quiere que los clientes se conecten a una aplicación de SignalR desde otros dominios, tendrá que habilitar el uso compartido de recursos entre orígenes (CORS).</span><span class="sxs-lookup"><span data-stu-id="5e609-215">If you want clients to connect to a SignalR app from different domains, you have to enable Cross-Origin Resource Sharing (CORS).</span></span> <span data-ttu-id="5e609-216">Para obtener más información, vea [Uso compartido de recursos entre orígenes](xref:signalr/security?view=aspnetcore-2.1#cross-origin-resource-sharing).</span><span class="sxs-lookup"><span data-stu-id="5e609-216">For more information, see [Cross-origin resource sharing](xref:signalr/security?view=aspnetcore-2.1#cross-origin-resource-sharing).</span></span>

<span data-ttu-id="5e609-217">Para obtener más información sobre SignalR, los concentradores y los clientes de JavaScript, vea estos recursos:</span><span class="sxs-lookup"><span data-stu-id="5e609-217">To learn more about SignalR, hubs, and JavaScript clients, see these resources:</span></span>

* [<span data-ttu-id="5e609-218">Introducción a SignalR para ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5e609-218">Introduction to SignalR for ASP.NET Core</span></span>](xref:signalr/introduction)
* [<span data-ttu-id="5e609-219">Uso de concentradores de SignalR para ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5e609-219">Use hubs in SignalR for ASP.NET Core</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="5e609-220">Cliente de JavaScript de SignalR para ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5e609-220">ASP.NET Core SignalR JavaScript client</span></span>](xref:signalr/javascript-client)
