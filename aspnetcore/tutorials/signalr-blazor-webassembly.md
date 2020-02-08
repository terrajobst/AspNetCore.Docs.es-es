---
title: Uso de ASP.NET Core SignalR con Blazor WebAssembly
author: guardrex
description: Cree una aplicación de chat que use ASP.NET Core SignalR con Blazor WebAssembly.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/31/2020
no-loc:
- Blazor
- SignalR
uid: tutorials/signalr-blazor-webassembly
ms.openlocfilehash: d3605c0823e9ec3ce34fb781da66a7470aa00622
ms.sourcegitcommit: 0e21d4f8111743bcb205a2ae0f8e57910c3e8c25
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2020
ms.locfileid: "77034148"
---
# <a name="use-aspnet-core-signalr-with-blazor-webassembly"></a><span data-ttu-id="ca7cb-103">Uso de ASP.NET Core SignalR con Blazor WebAssembly</span><span class="sxs-lookup"><span data-stu-id="ca7cb-103">Use ASP.NET Core SignalR with Blazor WebAssembly</span></span>

<span data-ttu-id="ca7cb-104">Por [Daniel Roth](https://github.com/danroth27) y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="ca7cb-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="ca7cb-105">Este tutorial le enseña los conceptos básicos de la creación de una aplicación en tiempo real mediante SignalR con Blazor WebAssembly.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-105">This tutorial teaches the basics of building a real-time app using SignalR with Blazor WebAssembly.</span></span> <span data-ttu-id="ca7cb-106">Aprenderá a:</span><span class="sxs-lookup"><span data-stu-id="ca7cb-106">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ca7cb-107">Crear un proyecto de aplicación hospedado de Blazor WebAssembly</span><span class="sxs-lookup"><span data-stu-id="ca7cb-107">Create a Blazor WebAssembly Hosted app project</span></span>
> * <span data-ttu-id="ca7cb-108">Agregar la biblioteca cliente de SignalR</span><span class="sxs-lookup"><span data-stu-id="ca7cb-108">Add the SignalR client library</span></span>
> * <span data-ttu-id="ca7cb-109">Agregar un concentrador de SignalR</span><span class="sxs-lookup"><span data-stu-id="ca7cb-109">Add a SignalR hub</span></span>
> * <span data-ttu-id="ca7cb-110">Agregar servicios de SignalR y un punto de conexión para el concentrador de SignalR</span><span class="sxs-lookup"><span data-stu-id="ca7cb-110">Add SignalR services and an endpoint for the SignalR hub</span></span>
> * <span data-ttu-id="ca7cb-111">Agregar código de componente de Razor para el chat</span><span class="sxs-lookup"><span data-stu-id="ca7cb-111">Add Razor component code for chat</span></span>

<span data-ttu-id="ca7cb-112">Al final de este tutorial, tendrá una aplicación de chat funcional.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-112">At the end of this tutorial, you'll have a working chat app.</span></span>

<span data-ttu-id="ca7cb-113">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/signalr-blazor-webassembly/samples/) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ca7cb-113">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/signalr-blazor-webassembly/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ca7cb-114">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="ca7cb-114">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ca7cb-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ca7cb-115">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.1.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ca7cb-116">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ca7cb-116">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.1.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="ca7cb-117">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="ca7cb-117">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.1.md)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="ca7cb-118">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="ca7cb-118">.NET Core CLI</span></span>](#tab/netcore-cli/)

[!INCLUDE[](~/includes/3.1-SDK.md)]

---

## <a name="create-a-hosted-blazor-webassembly-app-project"></a><span data-ttu-id="ca7cb-119">Creación de un proyecto de aplicación de Blazor WebAssembly hospedado</span><span class="sxs-lookup"><span data-stu-id="ca7cb-119">Create a hosted Blazor WebAssembly app project</span></span>

<span data-ttu-id="ca7cb-120">Instale la plantilla de [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly).</span><span class="sxs-lookup"><span data-stu-id="ca7cb-120">Install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template.</span></span> <span data-ttu-id="ca7cb-121">El paquete [Microsoft.AspNetCore.Blazor.Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) tiene una versión preliminar, mientras que Blazor WebAssembly se encuentra en versión preliminar.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-121">The [Microsoft.AspNetCore.Blazor.Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) package has a preview version while Blazor WebAssembly is in preview.</span></span> <span data-ttu-id="ca7cb-122">En un shell de comandos, ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="ca7cb-122">In a command shell, execute the following command:</span></span>

```dotnetcli
dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.2.0-preview1.20073.1
```

<span data-ttu-id="ca7cb-123">Siga las instrucciones para su elección de herramientas:</span><span class="sxs-lookup"><span data-stu-id="ca7cb-123">Follow the guidance for your choice of tooling:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ca7cb-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ca7cb-124">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="ca7cb-125">Cree un nuevo proyecto.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-125">Create a new project.</span></span>

1. <span data-ttu-id="ca7cb-126">Seleccione **Aplicación Blazor** y luego **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-126">Select **Blazor App** and select **Next**.</span></span>

1. <span data-ttu-id="ca7cb-127">Escriba "BlazorSignalRApp" en el campo **Nombre del proyecto**.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-127">Type "BlazorSignalRApp" in the **Project name** field.</span></span> <span data-ttu-id="ca7cb-128">Confirme que la entrada de **Ubicación** es correcta o proporcione una ubicación para el proyecto.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-128">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="ca7cb-129">Seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-129">Select **Create**.</span></span>

1. <span data-ttu-id="ca7cb-130">Elija la plantilla **Aplicación WebAssembly de Blazor**.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-130">Choose the **Blazor WebAssembly App** template.</span></span>

1. <span data-ttu-id="ca7cb-131">En **Avanzado**, active la casilla **ASP.NET Core hospedado**.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-131">Under **Advanced**, select the **ASP.NET Core hosted** check box.</span></span>

1. <span data-ttu-id="ca7cb-132">Seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-132">Select **Create**.</span></span>

> [!NOTE]
> <span data-ttu-id="ca7cb-133">Si ha actualizado o instalado una nueva versión de Visual Studio y la plantilla de Blazor WebAssembly no aparece en la interfaz de usuario de dicha aplciación, vuelva a instalar la plantilla con el comando de `dotnet new` que mostrado anteriormente.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-133">If you upgraded or installed a new version of Visual Studio and the Blazor WebAssembly template doesn't appear in the VS UI, reinstall the template using the `dotnet new` command shown previously.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ca7cb-134">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ca7cb-134">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="ca7cb-135">En un shell de comandos, ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="ca7cb-135">In a command shell, execute the following command:</span></span>

   ```dotnetcli
   dotnet new blazorwasm --hosted --output BlazorSignalRApp
   ```

1. <span data-ttu-id="ca7cb-136">En Visual Studio Code, abra la carpeta de proyecto de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-136">In Visual Studio Code, open the app's project folder.</span></span>

1. <span data-ttu-id="ca7cb-137">Cuando aparezca el cuadro de diálogo para agregar recursos para compilar y depurar la aplicación, seleccione **Sí**.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-137">When the dialog appears to add assets to build and debug the app, select **Yes**.</span></span> <span data-ttu-id="ca7cb-138">Visual Studio Code agrega automáticamente la carpeta *.vscode* con los archivos *launch.json* y *tasks.json* generados.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-138">Visual Studio Code automatically adds the *.vscode* folder with generated *launch.json* and *tasks.json* files.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="ca7cb-139">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="ca7cb-139">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="ca7cb-140">En un shell de comandos, ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="ca7cb-140">In a command shell, execute the following command:</span></span>

   ```dotnetcli
   dotnet new blazorwasm --hosted --output BlazorSignalRApp
   ```

1. <span data-ttu-id="ca7cb-141">En Visual Studio para Mac, abra el proyecto; para ello, vaya a la carpeta del proyecto y abra el archivo de solución del proyecto ( *.sln*).</span><span class="sxs-lookup"><span data-stu-id="ca7cb-141">In Visual Studio for Mac, open the project by navigating to the project folder and opening the project's solution file (*.sln*).</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="ca7cb-142">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="ca7cb-142">.NET Core CLI</span></span>](#tab/netcore-cli/)

<span data-ttu-id="ca7cb-143">En un shell de comandos, ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="ca7cb-143">In a command shell, execute the following command:</span></span>

```dotnetcli
dotnet new blazorwasm --hosted --output BlazorSignalRApp
```

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="ca7cb-144">Agregar la biblioteca cliente de SignalR</span><span class="sxs-lookup"><span data-stu-id="ca7cb-144">Add the SignalR client library</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ca7cb-145">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ca7cb-145">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="ca7cb-146">En el **Explorador de soluciones**, haga clic con el botón derecho en el proyecto **BlazorSignalRApp.Client** y seleccione **Administrar paquetes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-146">In **Solution Explorer**, right-click the **BlazorSignalRApp.Client** project and select **Manage NuGet Packages**.</span></span>

1. <span data-ttu-id="ca7cb-147">En el cuadro de diálogo **Administrar paquetes NuGet**, confirme que **Origen del paquete** se ha establecido en *nuget.org*.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-147">In the **Manage NuGet Packages** dialog, confirm that the **Package source** is set to *nuget.org*.</span></span>

1. <span data-ttu-id="ca7cb-148">Con **Examinar** seleccionado, escriba "Microsoft.AspNetCore.SignalR.Client" en el cuadro de búsqueda.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-148">With **Browse** selected, type "Microsoft.AspNetCore.SignalR.Client" in the search box.</span></span>

1. <span data-ttu-id="ca7cb-149">En los resultados de la búsqueda, seleccione el paquete `Microsoft.AspNetCore.SignalR.Client` y luego **Instalar**.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-149">In the search results, select the `Microsoft.AspNetCore.SignalR.Client` package and select **Install**.</span></span>

1. <span data-ttu-id="ca7cb-150">Si aparece el cuadro de diálogo **Vista previa de los cambios**, seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-150">If the **Preview Changes** dialog appears, select **OK**.</span></span>

1. <span data-ttu-id="ca7cb-151">Si aparece el cuadro de diálogo **Aceptación de la licencia**, seleccione **Acepto** si está de acuerdo con los términos de la licencia.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-151">If the **License Acceptance** dialog appears, select **I Accept** if you agree with the license terms.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ca7cb-152">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ca7cb-152">Visual Studio Code</span></span>](#tab/visual-studio-code/)

<span data-ttu-id="ca7cb-153">En **Terminal integrado** (**Ver** > **Terminal** en la barra de herramientas), ejecute los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="ca7cb-153">In the **Integrated Terminal** (**View** > **Terminal** from the toolbar), execute the following commands:</span></span>

```dotnetcli
dotnet add Client package Microsoft.AspNetCore.SignalR.Client
```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="ca7cb-154">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="ca7cb-154">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="ca7cb-155">En la barra lateral **Solución**, haga clic con el botón derecho en el proyecto **BlazorSignalRApp.Client** y seleccione **Administrar paquetes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-155">In the **Solution** sidebar, right-click the **BlazorSignalRApp.Client** project and select **Manage NuGet Packages**.</span></span>

1. <span data-ttu-id="ca7cb-156">En el cuadro de diálogo **Administrar paquetes NuGet**, confirme que la lista desplegable de origen se ha establecido en *nuget.org*.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-156">In the **Manage NuGet Packages** dialog, confirm that the source drop-down is set to *nuget.org*.</span></span>

1. <span data-ttu-id="ca7cb-157">Con **Examinar** seleccionado, escriba "Microsoft.AspNetCore.SignalR.Client" en el cuadro de búsqueda.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-157">With **Browse** selected, type "Microsoft.AspNetCore.SignalR.Client" in the search box.</span></span>

1. <span data-ttu-id="ca7cb-158">En los resultados de la búsqueda, active la casilla situada junto al paquete `Microsoft.AspNetCore.SignalR.Client` y seleccione **Agregar paquete**.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-158">In the search results, select the check box next to the `Microsoft.AspNetCore.SignalR.Client` package and select **Add Package**.</span></span>

1. <span data-ttu-id="ca7cb-159">Si aparece el cuadro de diálogo **Aceptación de la licencia**, seleccione **Acepto** si está de acuerdo con los términos de la licencia.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-159">If the **License Acceptance** dialog appears, select **Accept** if you agree with the license terms.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="ca7cb-160">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="ca7cb-160">.NET Core CLI</span></span>](#tab/netcore-cli/)

<span data-ttu-id="ca7cb-161">En un shell de comandos, ejecute los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="ca7cb-161">In a command shell, execute the following commands:</span></span>

```dotnetcli
cd BlazorSignalRApp
dotnet add Client package Microsoft.AspNetCore.SignalR.Client
```

---

## <a name="add-a-signalr-hub"></a><span data-ttu-id="ca7cb-162">Agregar un concentrador de SignalR</span><span class="sxs-lookup"><span data-stu-id="ca7cb-162">Add a SignalR hub</span></span>

<span data-ttu-id="ca7cb-163">En el proyecto **BlazorSignalRApp.Server**, cree una carpeta de *Hubs* (plural) y agregue la siguiente clase `ChatHub` (*Hubs/ChatHub.cs*):</span><span class="sxs-lookup"><span data-stu-id="ca7cb-163">In the **BlazorSignalRApp.Server** project, create a *Hubs* (plural) folder and add the following `ChatHub` class (*Hubs/ChatHub.cs*):</span></span>

[!code-csharp[](signalr-blazor-webassembly/samples/3.x/BlazorSignalRApp/Server/Hubs/ChatHub.cs)]

## <a name="add-signalr-services-and-an-endpoint-for-the-signalr-hub"></a><span data-ttu-id="ca7cb-164">Agregar servicios de SignalR y un punto de conexión para el concentrador de SignalR</span><span class="sxs-lookup"><span data-stu-id="ca7cb-164">Add SignalR services and an endpoint for the SignalR hub</span></span>

1. <span data-ttu-id="ca7cb-165">En el proyecto **BlazorSignalRApp.Server**, abra el archivo *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-165">In the **BlazorSignalRApp.Server** project, open the *Startup.cs* file.</span></span>

1. <span data-ttu-id="ca7cb-166">Agregue el espacio de nombres para la clase `ChatHub` en la parte superior del archivo:</span><span class="sxs-lookup"><span data-stu-id="ca7cb-166">Add the namespace for the `ChatHub` class to the top of the file:</span></span>

   ```csharp
   using BlazorSignalRApp.Server.Hubs;
   ```

1. <span data-ttu-id="ca7cb-167">Agregue los servicios de SignalR a `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="ca7cb-167">Add the SignalR services to `Startup.ConfigureServices`:</span></span>

   ```csharp
   services.AddSignalR();
   ```

1. <span data-ttu-id="ca7cb-168">En `Startup.Configure` entre los puntos de conexión de la ruta del controlador predeterminada y la reserva del lado cliente, agregue un punto de conexión para el concentrador:</span><span class="sxs-lookup"><span data-stu-id="ca7cb-168">In `Startup.Configure` between the endpoints for the default controller route and the client-side fallback, add an endpoint for the hub:</span></span>

   [!code-csharp[](signalr-blazor-webassembly/samples/3.x/BlazorSignalRApp/Server/Startup.cs?name=snippet&highlight=4)]

## <a name="add-razor-component-code-for-chat"></a><span data-ttu-id="ca7cb-169">Agregar código de componente de Razor para el chat</span><span class="sxs-lookup"><span data-stu-id="ca7cb-169">Add Razor component code for chat</span></span>

1. <span data-ttu-id="ca7cb-170">En el proyecto **BlazorSignalRApp.Client**, abra el archivo *Pages/Index.razor*.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-170">In the **BlazorSignalRApp.Client** project, open the *Pages/Index.razor* file.</span></span>

1. <span data-ttu-id="ca7cb-171">Reemplace el marcado con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="ca7cb-171">Replace the markup with the following code:</span></span>

[!code-razor[](signalr-blazor-webassembly/samples/3.x/BlazorSignalRApp/Client/Pages/Index.razor)]

## <a name="run-the-app"></a><span data-ttu-id="ca7cb-172">Ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="ca7cb-172">Run the app</span></span>

1. <span data-ttu-id="ca7cb-173">Siga las instrucciones para las herramientas:</span><span class="sxs-lookup"><span data-stu-id="ca7cb-173">Follow the guidance for your tooling:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ca7cb-174">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ca7cb-174">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="ca7cb-175">En **Explorador de soluciones**, seleccione el proyecto **BlazorSignalRApp.Server**.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-175">In **Solution Explorer**, select the **BlazorSignalRApp.Server** project.</span></span> <span data-ttu-id="ca7cb-176">Presione **CTRL+F5** para ejecutar la aplicación sin depurar.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-176">Press **Ctrl+F5** to run the app without debugging.</span></span>

1. <span data-ttu-id="ca7cb-177">Copie la dirección URL de la barra de direcciones, abra otra instancia o pestaña del explorador, y pegue la dirección URL en la barra de direcciones.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-177">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="ca7cb-178">Elija cualquier explorador, escriba un nombre y un mensaje, y haga clic en el botón **Enviar**.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-178">Choose either browser, enter a name and message, and select the **Send** button.</span></span> <span data-ttu-id="ca7cb-179">El nombre y el mensaje se muestran en ambas páginas al instante:</span><span class="sxs-lookup"><span data-stu-id="ca7cb-179">The name and message are displayed on both pages instantly:</span></span>

   ![Aplicación de ejemplo de SignalR Blazor WebAssembly abierta en dos ventanas del explorador que muestran mensajes intercambiados.](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)

   <span data-ttu-id="ca7cb-181">Citas: *Star Trek VI: The Undiscovered Country* &copy;1991 [Paramount](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span><span class="sxs-lookup"><span data-stu-id="ca7cb-181">Quotes: *Star Trek VI: The Undiscovered Country* &copy;1991 [Paramount](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ca7cb-182">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ca7cb-182">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="ca7cb-183">Seleccione **Depurar** > **Ejecutar sin depuración** en la barra de herramientas.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-183">Select **Debug** > **Run Without Debugging** from the toolbar.</span></span>

1. <span data-ttu-id="ca7cb-184">Copie la dirección URL de la barra de direcciones, abra otra instancia o pestaña del explorador, y pegue la dirección URL en la barra de direcciones.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-184">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="ca7cb-185">Elija cualquier explorador, escriba un nombre y un mensaje, y haga clic en el botón **Enviar**.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-185">Choose either browser, enter a name and message, and select the **Send** button.</span></span> <span data-ttu-id="ca7cb-186">El nombre y el mensaje se muestran en ambas páginas al instante:</span><span class="sxs-lookup"><span data-stu-id="ca7cb-186">The name and message are displayed on both pages instantly:</span></span>

   ![Aplicación de ejemplo de SignalR Blazor WebAssembly abierta en dos ventanas del explorador que muestran mensajes intercambiados.](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)

   <span data-ttu-id="ca7cb-188">Citas: *Star Trek VI: The Undiscovered Country* &copy;1991 [Paramount](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span><span class="sxs-lookup"><span data-stu-id="ca7cb-188">Quotes: *Star Trek VI: The Undiscovered Country* &copy;1991 [Paramount](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="ca7cb-189">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="ca7cb-189">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="ca7cb-190">En la barra lateral **Solución**, seleccione el proyecto **BlazorSignalRApp.Server**.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-190">In the **Solution** sidebar, select the **BlazorSignalRApp.Server** project.</span></span> <span data-ttu-id="ca7cb-191">En el menú, seleccione **Ejecutar** > **Iniciar sin depuración**.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-191">From the menu, select **Run** > **Start Without Debugging**.</span></span>

1. <span data-ttu-id="ca7cb-192">Copie la dirección URL de la barra de direcciones, abra otra instancia o pestaña del explorador, y pegue la dirección URL en la barra de direcciones.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-192">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="ca7cb-193">Elija cualquier explorador, escriba un nombre y un mensaje, y haga clic en el botón **Enviar**.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-193">Choose either browser, enter a name and message, and select the **Send** button.</span></span> <span data-ttu-id="ca7cb-194">El nombre y el mensaje se muestran en ambas páginas al instante:</span><span class="sxs-lookup"><span data-stu-id="ca7cb-194">The name and message are displayed on both pages instantly:</span></span>

   ![Aplicación de ejemplo de SignalR Blazor WebAssembly abierta en dos ventanas del explorador que muestran mensajes intercambiados.](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)

   <span data-ttu-id="ca7cb-196">Citas: *Star Trek VI: The Undiscovered Country* &copy;1991 [Paramount](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span><span class="sxs-lookup"><span data-stu-id="ca7cb-196">Quotes: *Star Trek VI: The Undiscovered Country* &copy;1991 [Paramount](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="ca7cb-197">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="ca7cb-197">.NET Core CLI</span></span>](#tab/netcore-cli/)

1. <span data-ttu-id="ca7cb-198">En un shell de comandos, ejecute los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="ca7cb-198">In a command shell, execute the following commands:</span></span>

   ```dotnetcli
   cd Server
   dotnet run
   ```

1. <span data-ttu-id="ca7cb-199">Copie la dirección URL de la barra de direcciones, abra otra instancia o pestaña del explorador, y pegue la dirección URL en la barra de direcciones.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-199">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="ca7cb-200">Elija cualquier explorador, escriba un nombre y un mensaje, y haga clic en el botón **Enviar**.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-200">Choose either browser, enter a name and message, and select the **Send** button.</span></span> <span data-ttu-id="ca7cb-201">El nombre y el mensaje se muestran en ambas páginas al instante:</span><span class="sxs-lookup"><span data-stu-id="ca7cb-201">The name and message are displayed on both pages instantly:</span></span>

   ![Aplicación de ejemplo de SignalR Blazor WebAssembly abierta en dos ventanas del explorador que muestran mensajes intercambiados.](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)

   <span data-ttu-id="ca7cb-203">Citas: *Star Trek VI: The Undiscovered Country* &copy;1991 [Paramount](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span><span class="sxs-lookup"><span data-stu-id="ca7cb-203">Quotes: *Star Trek VI: The Undiscovered Country* &copy;1991 [Paramount](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span></span>

---

## <a name="next-steps"></a><span data-ttu-id="ca7cb-204">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="ca7cb-204">Next steps</span></span>

<span data-ttu-id="ca7cb-205">En este tutorial ha aprendido a:</span><span class="sxs-lookup"><span data-stu-id="ca7cb-205">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ca7cb-206">Crear un proyecto de aplicación hospedado de Blazor WebAssembly</span><span class="sxs-lookup"><span data-stu-id="ca7cb-206">Create a Blazor WebAssembly Hosted app project</span></span>
> * <span data-ttu-id="ca7cb-207">Adición de la biblioteca cliente de SignalR</span><span class="sxs-lookup"><span data-stu-id="ca7cb-207">Add the SignalR client library</span></span>
> * <span data-ttu-id="ca7cb-208">Agregar un concentrador de SignalR</span><span class="sxs-lookup"><span data-stu-id="ca7cb-208">Add a SignalR hub</span></span>
> * <span data-ttu-id="ca7cb-209">Agregar servicios de SignalR y un punto de conexión para el concentrador de SignalR</span><span class="sxs-lookup"><span data-stu-id="ca7cb-209">Add SignalR services and an endpoint for the SignalR hub</span></span>
> * <span data-ttu-id="ca7cb-210">Agregar código de componente de Razor para el chat</span><span class="sxs-lookup"><span data-stu-id="ca7cb-210">Add Razor component code for chat</span></span>

<span data-ttu-id="ca7cb-211">Para más información sobre la creación de aplicaciones de Blazor, consulte la documentación de Blazor:</span><span class="sxs-lookup"><span data-stu-id="ca7cb-211">To learn more about building Blazor apps, see the Blazor documentation:</span></span>

> [!div class="nextstepaction"]
> <xref:blazor/index>

## <a name="additional-resources"></a><span data-ttu-id="ca7cb-212">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="ca7cb-212">Additional resources</span></span>

* <xref:signalr/introduction>
