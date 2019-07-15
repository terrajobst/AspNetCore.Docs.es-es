---
title: Introducción a SignalR de ASP.NET Core
author: bradygaster
description: En este tutorial, creará una aplicación de chat en la que se usa SignalR de ASP.NET Core.
ms.author: bradyg
ms.custom: mvc
ms.date: 07/08/2019
uid: tutorials/signalr
ms.openlocfilehash: fd3324dfa0731ae16747178d83bd93ed95dd15ce
ms.sourcegitcommit: bee530454ae2b3c25dc7ffebf93536f479a14460
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2019
ms.locfileid: "67724479"
---
# <a name="tutorial-get-started-with-aspnet-core-signalr"></a><span data-ttu-id="7623c-103">Tutorial: Introducción a SignalR de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7623c-103">Tutorial: Get started with ASP.NET Core SignalR</span></span>

<span data-ttu-id="7623c-104">En este tutorial se describen los conceptos básicos de la creación de una aplicación en tiempo real con SignalR.</span><span class="sxs-lookup"><span data-stu-id="7623c-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="7623c-105">Aprenderá a:</span><span class="sxs-lookup"><span data-stu-id="7623c-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="7623c-106">Cree un proyecto web.</span><span class="sxs-lookup"><span data-stu-id="7623c-106">Create a web project.</span></span>
> * <span data-ttu-id="7623c-107">Agregar la biblioteca cliente de SignalR.</span><span class="sxs-lookup"><span data-stu-id="7623c-107">Add the SignalR client library.</span></span>
> * <span data-ttu-id="7623c-108">Crear un concentrador de SignalR.</span><span class="sxs-lookup"><span data-stu-id="7623c-108">Create a SignalR hub.</span></span>
> * <span data-ttu-id="7623c-109">Configurar el proyecto para usar SignalR.</span><span class="sxs-lookup"><span data-stu-id="7623c-109">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="7623c-110">Agregar código que envía mensajes desde cualquier cliente a todos los clientes conectados.</span><span class="sxs-lookup"><span data-stu-id="7623c-110">Add code that sends messages from any client to all connected clients.</span></span>

<span data-ttu-id="7623c-111">Al final, tendrá una aplicación de chat funcional:</span><span class="sxs-lookup"><span data-stu-id="7623c-111">At the end, you'll have a working chat app:</span></span>

![Aplicación de ejemplo de SignalR](signalr/_static/signalr-get-started-finished.png)

## <a name="prerequisites"></a><span data-ttu-id="7623c-113">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="7623c-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7623c-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7623c-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="7623c-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7623c-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="7623c-116">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="7623c-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-app-project"></a><span data-ttu-id="7623c-117">Crear un proyecto de aplicación web</span><span class="sxs-lookup"><span data-stu-id="7623c-117">Create a web app project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7623c-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7623c-118">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="7623c-119">En el menú, seleccione **Archivo > Nuevo proyecto**.</span><span class="sxs-lookup"><span data-stu-id="7623c-119">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="7623c-120">En el cuadro de diálogo **Crear un proyecto nuevo**, seleccione **Aplicación web ASP.NET Core** y, a continuación, seleccione **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="7623c-120">In the **Create a new project** dialog, select **ASP.NET Core Web Application**, and then select **Next**.</span></span>

* <span data-ttu-id="7623c-121">En el cuadro de diálogo **Configurar el nuevo proyecto**, asigne al proyecto el nombre *SignalRChat* y, a continuación, seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="7623c-121">In the **Configure your new project** dialog, name the project *SignalRChat*, and then select **Create**.</span></span>

* <span data-ttu-id="7623c-122">En el cuadro de diálogo **Crear una aplicación web ASP.NET Core**, seleccione **.NET Core** y **ASP.NET Core 3.0**.</span><span class="sxs-lookup"><span data-stu-id="7623c-122">In the **Create a new ASP.NET Core Web Application** dialog, select **.NET Core** and **ASP.NET Core 3.0**.</span></span> 

* <span data-ttu-id="7623c-123">Seleccione **Aplicación web** para crear un proyecto en el que se use Razor Pages, y, a continuación, seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="7623c-123">Select **Web Application** to create a project that uses Razor Pages, and then select **Create**.</span></span>

  ![Cuadro de diálogo Nuevo proyecto en Visual Studio](signalr/_static/signalr-new-project-dialog.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="7623c-125">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7623c-125">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="7623c-126">Abra el [terminal integrado](https://code.visualstudio.com/docs/editor/integrated-terminal) en la carpeta en la que vaya a crear la del proyecto nuevo.</span><span class="sxs-lookup"><span data-stu-id="7623c-126">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) to the folder in which the new project folder will be created.</span></span>

* <span data-ttu-id="7623c-127">Ejecute los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="7623c-127">Run the following commands:</span></span>

   ```console
   dotnet new webapp -o SignalRChat
   code -r SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="7623c-128">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="7623c-128">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="7623c-129">En el menú, seleccione **Archivo > Nueva solución**.</span><span class="sxs-lookup"><span data-stu-id="7623c-129">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="7623c-130">Seleccione **.NET Core > Aplicación > Aplicación web** (no **Aplicación web [Modelo-Vista-Controlador]** ) y, a continuación, **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="7623c-130">Select **.NET Core > App > Web Application** (Don't select **Web Application (Model-View-Controller)**), and then select **Next**.</span></span>

* <span data-ttu-id="7623c-131">Asegúrese de que la **Plataforma de destino** esté establecida en **.NET Core 3.0** y, a continuación, seleccione **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="7623c-131">Make sure the **Target Framework** is set to **.NET Core 3.0**, and then select **Next**.</span></span>

* <span data-ttu-id="7623c-132">Asigne el nombre *SignalRChat* al proyecto y, después, haga clic en **Crear**.</span><span class="sxs-lookup"><span data-stu-id="7623c-132">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="7623c-133">Agregar la biblioteca cliente de SignalR</span><span class="sxs-lookup"><span data-stu-id="7623c-133">Add the SignalR client library</span></span>

<span data-ttu-id="7623c-134">La biblioteca de servidor de SignalR se incluye en la plataforma de trabajo compartida de ASP.NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="7623c-134">The SignalR server library is included in the ASP.NET Core 3.0 shared framework.</span></span> <span data-ttu-id="7623c-135">La biblioteca cliente de JavaScript no se incluye automáticamente en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="7623c-135">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="7623c-136">En este tutorial, usará el Administrador de bibliotecas (LibMan) para obtener la biblioteca cliente de *unpkg*.</span><span class="sxs-lookup"><span data-stu-id="7623c-136">For this tutorial, you use Library Manager (LibMan) to get the client library from *unpkg*.</span></span> <span data-ttu-id="7623c-137">unpkg es una red de entrega de contenido (CDN) que puede entregar todo lo que encuentre en npm, el administrador de paquetes de Node.js.</span><span class="sxs-lookup"><span data-stu-id="7623c-137">unpkg is a content delivery network (CDN)) that can deliver anything found in npm, the Node.js package manager.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7623c-138">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7623c-138">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="7623c-139">En el **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y seleccione **Agregar** > **Client-Side Library** (Biblioteca del lado cliente).</span><span class="sxs-lookup"><span data-stu-id="7623c-139">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>

* <span data-ttu-id="7623c-140">En el cuadro de diálogo **Add Client-Side Library** (Agregar biblioteca del lado cliente), en **Proveedor**, seleccione **unpkg**.</span><span class="sxs-lookup"><span data-stu-id="7623c-140">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span>

* <span data-ttu-id="7623c-141">Para **Biblioteca**, indique `@aspnet/signalr@next`.</span><span class="sxs-lookup"><span data-stu-id="7623c-141">For **Library**, enter `@aspnet/signalr@next`.</span></span>
<!-- when 3.0 is released, change @next to @latest -->

* <span data-ttu-id="7623c-142">Seleccione **Choose specific files** (Elegir archivos específicos), expanda la carpeta *dist/browser* y seleccione *signalr.js* y *signalr.min.js*.</span><span class="sxs-lookup"><span data-stu-id="7623c-142">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span>

* <span data-ttu-id="7623c-143">Establezca **Ubicación de destino** en *wwwroot/lib/signalr/* y seleccione **Instalar**.</span><span class="sxs-lookup"><span data-stu-id="7623c-143">Set **Target Location** to *wwwroot/lib/signalr/*, and select **Install**.</span></span>

  ![Cuadro de diálogo Add Client-Side Library (Agregar biblioteca del lado cliente): selección de la biblioteca](signalr/_static/libman1.png)

  <span data-ttu-id="7623c-145">LibMan crea una carpeta *wwwroot/lib/signalr* y copia en ella los archivos seleccionados.</span><span class="sxs-lookup"><span data-stu-id="7623c-145">LibMan creates a *wwwroot/lib/signalr* folder and copies the selected files to it.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="7623c-146">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7623c-146">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="7623c-147">En el terminal integrado, ejecute el comando siguiente para instalar LibMan.</span><span class="sxs-lookup"><span data-stu-id="7623c-147">In the integrated terminal, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="7623c-148">Ejecute el comando siguiente para obtener la biblioteca cliente de SignalR con LibMan.</span><span class="sxs-lookup"><span data-stu-id="7623c-148">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="7623c-149">Puede que deba esperar unos segundos antes de ver la salida.</span><span class="sxs-lookup"><span data-stu-id="7623c-149">You might have to wait a few seconds before seeing output.</span></span>

  ```console
  libman install @aspnet/signalr@next -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="7623c-150">Los parámetros especifican las opciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="7623c-150">The parameters specify the following options:</span></span>
  * <span data-ttu-id="7623c-151">Use el proveedor unpkg.</span><span class="sxs-lookup"><span data-stu-id="7623c-151">Use the unpkg provider.</span></span>
  * <span data-ttu-id="7623c-152">Copie los archivos en el destino *wwwroot/lib/signalr*.</span><span class="sxs-lookup"><span data-stu-id="7623c-152">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="7623c-153">Copie solo los archivos especificados.</span><span class="sxs-lookup"><span data-stu-id="7623c-153">Copy only the specified files.</span></span>

  <span data-ttu-id="7623c-154">La salida se parece al ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="7623c-154">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@next" to "wwwroot/lib/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="7623c-155">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="7623c-155">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="7623c-156">En el **terminal**, ejecute el siguiente comando para instalar LibMan.</span><span class="sxs-lookup"><span data-stu-id="7623c-156">In the **Terminal**, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="7623c-157">Vaya a la carpeta de proyecto (la que contiene el archivo *SignalRChat.csproj*).</span><span class="sxs-lookup"><span data-stu-id="7623c-157">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="7623c-158">Ejecute el comando siguiente para obtener la biblioteca cliente de SignalR con LibMan.</span><span class="sxs-lookup"><span data-stu-id="7623c-158">Run the following command to get the SignalR client library by using LibMan.</span></span>

  ```console
  libman install @aspnet/signalr@next -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="7623c-159">Los parámetros especifican las opciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="7623c-159">The parameters specify the following options:</span></span>
  * <span data-ttu-id="7623c-160">Use el proveedor unpkg.</span><span class="sxs-lookup"><span data-stu-id="7623c-160">Use the unpkg provider.</span></span>
  * <span data-ttu-id="7623c-161">Copie los archivos en el destino *wwwroot/lib/signalr*.</span><span class="sxs-lookup"><span data-stu-id="7623c-161">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="7623c-162">Copie solo los archivos especificados.</span><span class="sxs-lookup"><span data-stu-id="7623c-162">Copy only the specified files.</span></span>

  <span data-ttu-id="7623c-163">La salida se parece al ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="7623c-163">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@next" to "wwwroot/lib/signalr"
  ```

---

## <a name="create-a-signalr-hub"></a><span data-ttu-id="7623c-164">Creación de un concentrador de SignalR</span><span class="sxs-lookup"><span data-stu-id="7623c-164">Create a SignalR hub</span></span>

<span data-ttu-id="7623c-165">Un *concentrador* es una clase que actúa como una canalización general que controla la comunicación entre el cliente y el servidor.</span><span class="sxs-lookup"><span data-stu-id="7623c-165">A *hub* is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="7623c-166">En la carpeta del proyecto SignalRChat, cree una carpeta *Hubs*.</span><span class="sxs-lookup"><span data-stu-id="7623c-166">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="7623c-167">En la carpeta *Hubs*, cree un archivo *ChatHub.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="7623c-167">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample-snapshot/ChatHub.cs)]

  <span data-ttu-id="7623c-168">La clase `ChatHub` hereda de la clase `Hub` de SignalR.</span><span class="sxs-lookup"><span data-stu-id="7623c-168">The `ChatHub` class inherits from the SignalR `Hub` class.</span></span> <span data-ttu-id="7623c-169">La clase `Hub` administra las conexiones, los grupos y la mensajería.</span><span class="sxs-lookup"><span data-stu-id="7623c-169">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="7623c-170">Puede llamarse al método `SendMessage` mediante un cliente conectado para enviar un mensaje a todos los clientes.</span><span class="sxs-lookup"><span data-stu-id="7623c-170">The `SendMessage` method can be called by a connected client to send a message to all clients.</span></span> <span data-ttu-id="7623c-171">El código de cliente de JavaScript que llama al método se muestra más adelante en el tutorial.</span><span class="sxs-lookup"><span data-stu-id="7623c-171">JavaScript client code that calls the method is shown later in the tutorial.</span></span> <span data-ttu-id="7623c-172">El código de SignalR es asincrónico para proporcionar la máxima escalabilidad.</span><span class="sxs-lookup"><span data-stu-id="7623c-172">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-signalr"></a><span data-ttu-id="7623c-173">Configuración de SignalR</span><span class="sxs-lookup"><span data-stu-id="7623c-173">Configure SignalR</span></span>

<span data-ttu-id="7623c-174">El servidor de SignalR se debe configurar para que pase las solicitudes de SignalR a SignalR.</span><span class="sxs-lookup"><span data-stu-id="7623c-174">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="7623c-175">Agregue el código resaltado siguiente al archivo *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="7623c-175">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample-snapshot/Startup.cs?highlight=6,30,58)]

  <span data-ttu-id="7623c-176">Estos cambios agregan SignalR al sistema de inserción de dependencias de ASP.NET Core y a los sistemas de enrutamiento.</span><span class="sxs-lookup"><span data-stu-id="7623c-176">These changes add SignalR to the ASP.NET Core dependency injection and routing systems.</span></span>

## <a name="add-signalr-client-code"></a><span data-ttu-id="7623c-177">Adición del código de cliente de SignalR</span><span class="sxs-lookup"><span data-stu-id="7623c-177">Add SignalR client code</span></span>

* <span data-ttu-id="7623c-178">Reemplace el contenido de *Pages/Index.cshtml* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="7623c-178">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](signalr/sample-snapshot/Index.cshtml)]

  <span data-ttu-id="7623c-179">El código anterior:</span><span class="sxs-lookup"><span data-stu-id="7623c-179">The preceding code:</span></span>

  * <span data-ttu-id="7623c-180">Crea cuadros de texto para el nombre y el mensaje de texto, y un botón de envío.</span><span class="sxs-lookup"><span data-stu-id="7623c-180">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="7623c-181">Crea una lista con `id="messagesList"` para mostrar los mensajes que se reciben desde el concentrador SignalR.</span><span class="sxs-lookup"><span data-stu-id="7623c-181">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="7623c-182">Incluye las referencias de script en SignalR y el código de aplicación de *chat.js* que se va a crear en el paso siguiente.</span><span class="sxs-lookup"><span data-stu-id="7623c-182">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="7623c-183">En la carpeta *wwwroot/js*, cree un archivo *chat.js* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="7623c-183">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample-snapshot/chat.js)]

  <span data-ttu-id="7623c-184">El código anterior:</span><span class="sxs-lookup"><span data-stu-id="7623c-184">The preceding code:</span></span>

  * <span data-ttu-id="7623c-185">Crea e inicia una conexión.</span><span class="sxs-lookup"><span data-stu-id="7623c-185">Creates and starts a connection.</span></span>
  * <span data-ttu-id="7623c-186">Agrega al botón de envío un controlador que envía mensajes al concentrador.</span><span class="sxs-lookup"><span data-stu-id="7623c-186">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="7623c-187">Agrega al objeto de conexión un controlador que recibe mensajes desde el concentrador y los agrega a la lista.</span><span class="sxs-lookup"><span data-stu-id="7623c-187">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="7623c-188">Ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="7623c-188">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7623c-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7623c-189">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="7623c-190">Presione **CTRL+F5** para ejecutar la aplicación sin depurar.</span><span class="sxs-lookup"><span data-stu-id="7623c-190">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="7623c-191">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7623c-191">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="7623c-192">En el terminal integrado, ejecute el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="7623c-192">In the integrated terminal, run the following command:</span></span>

  ```console
  dotnet run -p SignalRChat.csproj
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="7623c-193">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="7623c-193">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="7623c-194">En el menú, seleccione **Ejecutar > Iniciar sin depurar**.</span><span class="sxs-lookup"><span data-stu-id="7623c-194">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="7623c-195">Copie la dirección URL de la barra de direcciones, abra otra instancia o pestaña del explorador, y pegue la dirección URL en la barra de direcciones.</span><span class="sxs-lookup"><span data-stu-id="7623c-195">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="7623c-196">Elija cualquier explorador, escriba un nombre y un mensaje, y haga clic en el botón **Enviar mensaje**.</span><span class="sxs-lookup"><span data-stu-id="7623c-196">Choose either browser, enter a name and message, and select the **Send Message** button.</span></span>

  <span data-ttu-id="7623c-197">El nombre y el mensaje se muestran en ambas páginas al instante.</span><span class="sxs-lookup"><span data-stu-id="7623c-197">The name and message are displayed on both pages instantly.</span></span>

  ![Aplicación de ejemplo de SignalR](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> * <span data-ttu-id="7623c-199">Si la aplicación no funciona, abra las herramientas para desarrolladores del explorador (F12) y vaya a la consola.</span><span class="sxs-lookup"><span data-stu-id="7623c-199">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="7623c-200">Es posible que vea errores relacionados con el código HTML y JavaScript.</span><span class="sxs-lookup"><span data-stu-id="7623c-200">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="7623c-201">Por ejemplo, suponga que coloca *signalr.js* en una carpeta distinta a la indicada.</span><span class="sxs-lookup"><span data-stu-id="7623c-201">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="7623c-202">En ese caso, la referencia a ese archivo no funcionará y verá un error 404 en la consola.</span><span class="sxs-lookup"><span data-stu-id="7623c-202">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
>   <span data-ttu-id="7623c-203">![Error: signalr.js no encontrado](signalr/_static/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="7623c-203">![signalr.js not found error](signalr/_static/f12-console.png)</span></span>
> * <span data-ttu-id="7623c-204">Si se produce el error ERR_SPDY_INADEQUATE_TRANSPORT_SECURITY en Chrome o NS_ERROR_NET_INADEQUATE_SECURITY en Firefox, ejecute estos comandos para actualizar el certificado de desarrollo:</span><span class="sxs-lookup"><span data-stu-id="7623c-204">If you get the error ERR_SPDY_INADEQUATE_TRANSPORT_SECURITY in Chrome or NS_ERROR_NET_INADEQUATE_SECURITY in Firefox, run these commands to update your development certificate:</span></span>
>   ```
>   dotnet dev-certs https --clean
>   dotnet dev-certs https --trust
>   ```

## <a name="next-steps"></a><span data-ttu-id="7623c-205">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="7623c-205">Next steps</span></span>

<span data-ttu-id="7623c-206">Para obtener más información sobre SignalR, vea la introducción:</span><span class="sxs-lookup"><span data-stu-id="7623c-206">To learn more about SignalR, see the introduction:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="7623c-207">Introducción a SignalR de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7623c-207">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
