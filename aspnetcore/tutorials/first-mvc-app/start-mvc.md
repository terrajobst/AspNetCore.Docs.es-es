---
title: Introducción a ASP.NET Core MVC
author: rick-anderson
description: Obtenga información sobre cómo empezar a usar ASP.NET Core MVC.
ms.author: riande
ms.date: 08/05/2019
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 52d8fc631075408991d3a2e62cdaccf681c5927f
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082164"
---
# <a name="get-started-with-aspnet-core-mvc"></a><span data-ttu-id="68037-103">Introducción a ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="68037-103">Get started with ASP.NET Core MVC</span></span>

<span data-ttu-id="68037-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="68037-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="68037-105">En este tutorial se enseñan los conceptos básicos de la compilación de una aplicación web ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="68037-105">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="68037-106">La aplicación administra una base de datos de títulos de películas.</span><span class="sxs-lookup"><span data-stu-id="68037-106">The app manages a database of movie titles.</span></span> <span data-ttu-id="68037-107">Aprenderá a:</span><span class="sxs-lookup"><span data-stu-id="68037-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="68037-108">Crear una aplicación web.</span><span class="sxs-lookup"><span data-stu-id="68037-108">Create a web app.</span></span>
> * <span data-ttu-id="68037-109">Agregar un modelo y aplicarle scaffolding.</span><span class="sxs-lookup"><span data-stu-id="68037-109">Add and scaffold a model.</span></span>
> * <span data-ttu-id="68037-110">Trabajar con una base de datos.</span><span class="sxs-lookup"><span data-stu-id="68037-110">Work with a database.</span></span>
> * <span data-ttu-id="68037-111">Agregar búsqueda y validación.</span><span class="sxs-lookup"><span data-stu-id="68037-111">Add search and validation.</span></span>

<span data-ttu-id="68037-112">Al final, tendrá una aplicación que le permitirá administrar y mostrar datos de películas.</span><span class="sxs-lookup"><span data-stu-id="68037-112">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

## <a name="prerequisites"></a><span data-ttu-id="68037-113">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="68037-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="68037-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="68037-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="68037-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="68037-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="68037-116">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="68037-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-app"></a><span data-ttu-id="68037-117">Creación de una aplicación web</span><span class="sxs-lookup"><span data-stu-id="68037-117">Create a web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="68037-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="68037-118">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="68037-119">En Visual Studio, seleccione **Crear un proyecto**.</span><span class="sxs-lookup"><span data-stu-id="68037-119">From the Visual Studio select **Create a new project**.</span></span>

* <span data-ttu-id="68037-120">Seleccione **Aplicación web ASP.NET Core** y, después, **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="68037-120">Select **ASP.NET Core Web Application** and then select **Next**.</span></span>

![Nueva aplicación web de ASP.NET Core](start-mvc/_static/np_2.1.png)

* <span data-ttu-id="68037-122">Asigne el nombre **MvcMovie** al proyecto y seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="68037-122">Name the project **MvcMovie** and select **Create**.</span></span> <span data-ttu-id="68037-123">Es importante que el proyecto se llame **MvcMovie** para que, al copiar el código, coincida con el espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="68037-123">It's important to name the project **MvcMovie** so when you copy code, the namespace will match.</span></span>

  ![Nueva aplicación web de ASP.NET Core](start-mvc/_static/config.png)

* <span data-ttu-id="68037-125">Seleccione **Aplicación web (Modelo-Vista-Controlador)** y, luego, **Crear**.</span><span class="sxs-lookup"><span data-stu-id="68037-125">Select **Web Application(Model-View-Controller)**, and then select **Create**.</span></span>

![<span data-ttu-id="68037-126">Cuadro de diálogo Nuevo proyecto, .NET CORE en el panel izquierdo, Aplicación web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="68037-126">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="68037-127">Visual Studio ha usado la plantilla predeterminada para el proyecto de MVC que acaba de crear.</span><span class="sxs-lookup"><span data-stu-id="68037-127">Visual Studio used the default template for the MVC project you just created.</span></span> <span data-ttu-id="68037-128">Si escribe un nombre de proyecto y selecciona algunas opciones, dispondrá de inmediato de una aplicación operativa.</span><span class="sxs-lookup"><span data-stu-id="68037-128">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="68037-129">Se trata de un proyecto básico de inicio.</span><span class="sxs-lookup"><span data-stu-id="68037-129">This is a basic starter project.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="68037-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="68037-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="68037-131">Para realizar el tutorial debe estar familiarizado con VS Code.</span><span class="sxs-lookup"><span data-stu-id="68037-131">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="68037-132">Para más información, vea [Getting started with VS Code](https://code.visualstudio.com/docs) (Introducción a VS Code) y [Visual Studio Code help](#visual-studio-code-help) (Ayuda de Visual Studio Code).</span><span class="sxs-lookup"><span data-stu-id="68037-132">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="68037-133">Abra el [terminal integrado](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="68037-133">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="68037-134">Cambie los directorios (`cd`) a una carpeta que contenga el proyecto.</span><span class="sxs-lookup"><span data-stu-id="68037-134">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="68037-135">Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="68037-135">Run the following command:</span></span>

   ```dotnetcli
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="68037-136">Se muestra un cuadro de diálogo con el texto **Required assets to build and debug are missing from 'MvcMovie'. Add them?** (Faltan los activos necesarios para compilar y depurar en "RazorPagesMovie". ¿Desea agregarlos?).</span><span class="sxs-lookup"><span data-stu-id="68037-136">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="68037-137">Seleccione **Sí**.</span><span class="sxs-lookup"><span data-stu-id="68037-137">Select **Yes**</span></span>

  * <span data-ttu-id="68037-138">`dotnet new mvc -o MvcMovie`: crea un nuevo proyecto de ASP.NET Core MVC en la carpeta *MvcMovie*.</span><span class="sxs-lookup"><span data-stu-id="68037-138">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="68037-139">`code -r MvcMovie`: carga el archivo de proyecto *MvcMovie.csproj* en Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="68037-139">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="68037-140">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="68037-140">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="68037-141">Seleccione **Archivo** > **Nueva solución**.</span><span class="sxs-lookup"><span data-stu-id="68037-141">Select **File** > **New Solution**.</span></span>

  ![macOS: Nueva solución](./start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="68037-143">Seleccione **.NET Core** > **Aplicación** > **Aplicación web (Modelo-Vista-Controlador)** > **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="68037-143">Select **.NET Core** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span>

  ![Cuadro de diálogo de nuevo proyecto de macOS](./start-mvc/_static/new_project_mvc_vsmac.png)

* <span data-ttu-id="68037-145">En el cuadro de diálogo **Configurar la nueva API web de ASP.NET Core**, establezca el **Marco de trabajo de destino** de **.NET Core 3.0**.</span><span class="sxs-lookup"><span data-stu-id="68037-145">In the **Configure your new ASP.NET Core Web API** dialog, set the  **Target Framework** of **.NET Core 3.0**.</span></span>

<!-- 
  ![macOS .NET Core 2.2 selection](./start-mvc/_static/new_project_22_vsmac.png)
-->

* <span data-ttu-id="68037-146">Asigne el nombre **MvcMovie** al proyecto y, después, seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="68037-146">Name the project **MvcMovie**, and then select **Create**.</span></span>

---

### <a name="run-the-app"></a><span data-ttu-id="68037-147">Ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="68037-147">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="68037-148">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="68037-148">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="68037-149">Presione **Ctrl-F5** para ejecutar la aplicación en modo de no depuración.</span><span class="sxs-lookup"><span data-stu-id="68037-149">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="68037-150">Visual Studio inicia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) y ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="68037-150">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="68037-151">Tenga en cuenta que en la barra de direcciones aparece `localhost:port#` (y no algo como `example.com`).</span><span class="sxs-lookup"><span data-stu-id="68037-151">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="68037-152">Esto es así porque `localhost` es el nombre de host estándar del equipo local.</span><span class="sxs-lookup"><span data-stu-id="68037-152">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="68037-153">Cuando Visual Studio crea un proyecto web, se usa un puerto aleatorio para el servidor web.</span><span class="sxs-lookup"><span data-stu-id="68037-153">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="68037-154">El inicio de la aplicación con Ctrl+F5 (modo de no depuración) permite realizar cambios en el código, guardar el archivo, actualizar el explorador y ver los cambios de código.</span><span class="sxs-lookup"><span data-stu-id="68037-154">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="68037-155">Muchos desarrolladores prefieren usar el modo de no depuración para iniciar la aplicación rápidamente y ver los cambios.</span><span class="sxs-lookup"><span data-stu-id="68037-155">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="68037-156">Puede iniciar la aplicación en modo de depuración o en modo de no depuración desde el elemento de menú **Depurar**:</span><span class="sxs-lookup"><span data-stu-id="68037-156">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![Menú Depurar](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="68037-158">Puede depurar la aplicación seleccionando el botón **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="68037-158">You can debug the app by selecting the **IIS Express** button</span></span>

  ![IIS Express](start-mvc/_static/iis_express.png)

  <span data-ttu-id="68037-160">En la imagen siguiente se muestra la aplicación:</span><span class="sxs-lookup"><span data-stu-id="68037-160">The following image shows the app:</span></span>

  ![Página Inicio o Índice](start-mvc/_static/home2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="68037-162">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="68037-162">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="68037-163">Presione Ctrl+F5 para ejecutarla sin el depurador.</span><span class="sxs-lookup"><span data-stu-id="68037-163">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="68037-164">Visual Studio Code inicia [Kestrel](xref:fundamentals/servers/kestrel) y un explorador, y se desplaza hasta `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="68037-164">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="68037-165">En la barra de direcciones aparece `localhost:port:5001` (y no algo como `example.com`).</span><span class="sxs-lookup"><span data-stu-id="68037-165">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="68037-166">Esto es así porque `localhost` es el nombre de host estándar del equipo local.</span><span class="sxs-lookup"><span data-stu-id="68037-166">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="68037-167">Localhost solo sirve las solicitudes web del equipo local.</span><span class="sxs-lookup"><span data-stu-id="68037-167">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="68037-168">El inicio de la aplicación con Ctrl+F5 (modo de no depuración) permite realizar cambios en el código, guardar el archivo, actualizar el explorador y ver los cambios de código.</span><span class="sxs-lookup"><span data-stu-id="68037-168">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="68037-169">Muchos desarrolladores prefieren usar el modo de no depuración para actualizar la página y ver los cambios.</span><span class="sxs-lookup"><span data-stu-id="68037-169">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

  ![Página Inicio o Índice](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="68037-171">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="68037-171">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="68037-172">Seleccione **Ejecutar** > **Iniciar sin depurar** para iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="68037-172">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="68037-173">Visual Studio para Mac inicia el servidor [Kestrel](xref:fundamentals/servers/index#kestrel), inicia un explorador y navega a `http://localhost:port`, donde *port* es un número de puerto elegido aleatoriamente.</span><span class="sxs-lookup"><span data-stu-id="68037-173">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="68037-174">En la barra de direcciones aparece `localhost:port#` (y no algo como `example.com`).</span><span class="sxs-lookup"><span data-stu-id="68037-174">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="68037-175">Esto es así porque `localhost` es el nombre de host estándar del equipo local.</span><span class="sxs-lookup"><span data-stu-id="68037-175">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="68037-176">Cuando Visual Studio crea un proyecto web, se usa un puerto aleatorio para el servidor web.</span><span class="sxs-lookup"><span data-stu-id="68037-176">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="68037-177">Al ejecutar la aplicación verá otro puerto distinto.</span><span class="sxs-lookup"><span data-stu-id="68037-177">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="68037-178">Puede iniciar la aplicación en modo de depuración o en modo de no depuración desde el menú **Ejecutar**.</span><span class="sxs-lookup"><span data-stu-id="68037-178">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

  <span data-ttu-id="68037-179">En la imagen siguiente se muestra la aplicación:</span><span class="sxs-lookup"><span data-stu-id="68037-179">The following image shows the app:</span></span>

  ![Página Inicio o Índice](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="68037-181">En la siguiente sección de este tutorial conocerá MVC y empezará a escribir código.</span><span class="sxs-lookup"><span data-stu-id="68037-181">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="68037-182">Siguiente</span><span class="sxs-lookup"><span data-stu-id="68037-182">Next</span></span>](adding-controller.md)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="68037-183">En este tutorial se enseñan los conceptos básicos de la compilación de una aplicación web ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="68037-183">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="68037-184">La aplicación administra una base de datos de títulos de películas.</span><span class="sxs-lookup"><span data-stu-id="68037-184">The app manages a database of movie titles.</span></span> <span data-ttu-id="68037-185">Aprenderá a:</span><span class="sxs-lookup"><span data-stu-id="68037-185">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="68037-186">Crear una aplicación web.</span><span class="sxs-lookup"><span data-stu-id="68037-186">Create a web app.</span></span>
> * <span data-ttu-id="68037-187">Agregar un modelo y aplicarle scaffolding.</span><span class="sxs-lookup"><span data-stu-id="68037-187">Add and scaffold a model.</span></span>
> * <span data-ttu-id="68037-188">Trabajar con una base de datos.</span><span class="sxs-lookup"><span data-stu-id="68037-188">Work with a database.</span></span>
> * <span data-ttu-id="68037-189">Agregar búsqueda y validación.</span><span class="sxs-lookup"><span data-stu-id="68037-189">Add search and validation.</span></span>

<span data-ttu-id="68037-190">Al final, tendrá una aplicación que le permitirá administrar y mostrar datos de películas.</span><span class="sxs-lookup"><span data-stu-id="68037-190">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

## <a name="prerequisites"></a><span data-ttu-id="68037-191">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="68037-191">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="68037-192">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="68037-192">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="68037-193">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="68037-193">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="68037-194">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="68037-194">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---
## <a name="create-a-web-app"></a><span data-ttu-id="68037-195">Creación de una aplicación web</span><span class="sxs-lookup"><span data-stu-id="68037-195">Create a web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="68037-196">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="68037-196">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="68037-197">En Visual Studio, seleccione **Crear un proyecto**.</span><span class="sxs-lookup"><span data-stu-id="68037-197">From the Visual Studio select **Create a new project**.</span></span>

* <span data-ttu-id="68037-198">Seleccione **Aplicación web de ASP.NET Core** y, luego, **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="68037-198">Selecct **ASP.NET Core Web Application** and then select **Next**.</span></span>

![Nueva aplicación web de ASP.NET Core](start-mvc/_static/np_2.1.png)

* <span data-ttu-id="68037-200">Asigne el nombre **MvcMovie** al proyecto y seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="68037-200">Name the project **MvcMovie** and select **Create**.</span></span> <span data-ttu-id="68037-201">Es importante que el proyecto se llame **MvcMovie** para que, al copiar el código, coincida con el espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="68037-201">It's important to name the project **MvcMovie** so when you copy code, the namespace will match.</span></span>

  ![Nueva aplicación web de ASP.NET Core](start-mvc/_static/config.png)


* <span data-ttu-id="68037-203">Seleccione **Aplicación web (Modelo-Vista-Controlador)** y, luego, **Crear**.</span><span class="sxs-lookup"><span data-stu-id="68037-203">Select **Web Application(Model-View-Controller)**, and then select **Create**.</span></span>

![<span data-ttu-id="68037-204">Cuadro de diálogo Nuevo proyecto, .NET CORE en el panel izquierdo, Aplicación web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="68037-204">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="68037-205">Visual Studio ha usado la plantilla predeterminada para el proyecto de MVC que acaba de crear.</span><span class="sxs-lookup"><span data-stu-id="68037-205">Visual Studio used the default template for the MVC project you just created.</span></span> <span data-ttu-id="68037-206">Si escribe un nombre de proyecto y selecciona algunas opciones, dispondrá de inmediato de una aplicación operativa.</span><span class="sxs-lookup"><span data-stu-id="68037-206">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="68037-207">Se trata de un proyecto introductorio básico, pero es un buen punto de partida.</span><span class="sxs-lookup"><span data-stu-id="68037-207">This is a basic starter project, and it's a good place to start.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="68037-208">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="68037-208">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="68037-209">Para realizar el tutorial debe estar familiarizado con VS Code.</span><span class="sxs-lookup"><span data-stu-id="68037-209">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="68037-210">Para más información, vea [Getting started with VS Code](https://code.visualstudio.com/docs) (Introducción a VS Code) y [Visual Studio Code help](#visual-studio-code-help) (Ayuda de Visual Studio Code).</span><span class="sxs-lookup"><span data-stu-id="68037-210">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="68037-211">Abra el [terminal integrado](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="68037-211">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="68037-212">Cambie los directorios (`cd`) a una carpeta que contenga el proyecto.</span><span class="sxs-lookup"><span data-stu-id="68037-212">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="68037-213">Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="68037-213">Run the following command:</span></span>

   ```dotnetcli
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="68037-214">Se muestra un cuadro de diálogo con el texto **Required assets to build and debug are missing from 'MvcMovie'. Add them?** (Faltan los activos necesarios para compilar y depurar en "RazorPagesMovie". ¿Desea agregarlos?).</span><span class="sxs-lookup"><span data-stu-id="68037-214">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="68037-215">Seleccione **Sí**.</span><span class="sxs-lookup"><span data-stu-id="68037-215">Select **Yes**</span></span>

  * <span data-ttu-id="68037-216">`dotnet new mvc -o MvcMovie`: crea un nuevo proyecto de ASP.NET Core MVC en la carpeta *MvcMovie*.</span><span class="sxs-lookup"><span data-stu-id="68037-216">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="68037-217">`code -r MvcMovie`: carga el archivo de proyecto *MvcMovie.csproj* en Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="68037-217">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="68037-218">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="68037-218">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="68037-219">Seleccione **Archivo** > **Nueva solución**.</span><span class="sxs-lookup"><span data-stu-id="68037-219">Select **File** > **New Solution**.</span></span>

  ![macOS: Nueva solución](./start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="68037-221">Seleccione **.NET Core** > **Aplicación** > **Aplicación web (Modelo-Vista-Controlador)** > **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="68037-221">Select **.NET Core** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span>

  ![Cuadro de diálogo de nuevo proyecto de macOS](./start-mvc/_static/new_project_mvc_vsmac.png)

* <span data-ttu-id="68037-223">En el cuadro de diálogo **Configurar la nueva API web de ASP.NET Core**, acepte el **Marco de trabajo de destino** predeterminado de **.NET Core 2.2**.</span><span class="sxs-lookup"><span data-stu-id="68037-223">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of **.NET Core 2.2**.</span></span>

  ![Selección de .NET Core 2.2 de macOS](./start-mvc/_static/new_project_22_vsmac.png)

* <span data-ttu-id="68037-225">Asigne el nombre **MvcMovie** al proyecto y, después, seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="68037-225">Name the project **MvcMovie**, and then select **Create**.</span></span>

---

### <a name="run-the-app"></a><span data-ttu-id="68037-226">Ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="68037-226">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="68037-227">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="68037-227">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="68037-228">Presione **Ctrl-F5** para ejecutar la aplicación en modo de no depuración.</span><span class="sxs-lookup"><span data-stu-id="68037-228">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="68037-229">Visual Studio inicia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) y ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="68037-229">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="68037-230">Tenga en cuenta que en la barra de direcciones aparece `localhost:port#` (y no algo como `example.com`).</span><span class="sxs-lookup"><span data-stu-id="68037-230">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="68037-231">Esto es así porque `localhost` es el nombre de host estándar del equipo local.</span><span class="sxs-lookup"><span data-stu-id="68037-231">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="68037-232">Cuando Visual Studio crea un proyecto web, se usa un puerto aleatorio para el servidor web.</span><span class="sxs-lookup"><span data-stu-id="68037-232">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="68037-233">El inicio de la aplicación con Ctrl+F5 (modo de no depuración) permite realizar cambios en el código, guardar el archivo, actualizar el explorador y ver los cambios de código.</span><span class="sxs-lookup"><span data-stu-id="68037-233">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="68037-234">Muchos desarrolladores prefieren usar el modo de no depuración para iniciar la aplicación rápidamente y ver los cambios.</span><span class="sxs-lookup"><span data-stu-id="68037-234">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="68037-235">Puede iniciar la aplicación en modo de depuración o en modo de no depuración desde el elemento de menú **Depurar**:</span><span class="sxs-lookup"><span data-stu-id="68037-235">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![Menú Depurar](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="68037-237">Puede depurar la aplicación seleccionando el botón **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="68037-237">You can debug the app by selecting the **IIS Express** button</span></span>

  ![IIS Express](start-mvc/_static/iis_express.png)

* <span data-ttu-id="68037-239">Seleccione **Aceptar** para dar su consentimiento al seguimiento.</span><span class="sxs-lookup"><span data-stu-id="68037-239">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="68037-240">Esta aplicación no lleva un seguimiento de la información personal.</span><span class="sxs-lookup"><span data-stu-id="68037-240">This app doesn't track personal information.</span></span> <span data-ttu-id="68037-241">El código generado con plantilla incluye activos que sirven para cumplir el [Reglamento general de protección de datos (RGPD)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="68037-241">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Página Inicio o Índice](start-mvc/_static/privacy.png)

  <span data-ttu-id="68037-243">En la siguiente imagen se muestra la aplicación tras haber aceptado el seguimiento:</span><span class="sxs-lookup"><span data-stu-id="68037-243">The following image shows the app after accepting tracking:</span></span>

  ![Página Inicio o Índice](start-mvc/_static/home2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="68037-245">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="68037-245">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="68037-246">Presione Ctrl+F5 para ejecutarla sin el depurador.</span><span class="sxs-lookup"><span data-stu-id="68037-246">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="68037-247">Visual Studio Code inicia [Kestrel](xref:fundamentals/servers/kestrel) y un explorador, y se desplaza hasta `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="68037-247">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="68037-248">En la barra de direcciones aparece `localhost:port:5001` (y no algo como `example.com`).</span><span class="sxs-lookup"><span data-stu-id="68037-248">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="68037-249">Esto es así porque `localhost` es el nombre de host estándar del equipo local.</span><span class="sxs-lookup"><span data-stu-id="68037-249">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="68037-250">Localhost solo sirve las solicitudes web del equipo local.</span><span class="sxs-lookup"><span data-stu-id="68037-250">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="68037-251">El inicio de la aplicación con Ctrl+F5 (modo de no depuración) permite realizar cambios en el código, guardar el archivo, actualizar el explorador y ver los cambios de código.</span><span class="sxs-lookup"><span data-stu-id="68037-251">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="68037-252">Muchos desarrolladores prefieren usar el modo de no depuración para actualizar la página y ver los cambios.</span><span class="sxs-lookup"><span data-stu-id="68037-252">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

* <span data-ttu-id="68037-253">Seleccione **Aceptar** para dar su consentimiento al seguimiento.</span><span class="sxs-lookup"><span data-stu-id="68037-253">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="68037-254">Esta aplicación no lleva un seguimiento de la información personal.</span><span class="sxs-lookup"><span data-stu-id="68037-254">This app doesn't track personal information.</span></span> <span data-ttu-id="68037-255">El código generado con plantilla incluye activos que sirven para cumplir el [Reglamento general de protección de datos (RGPD)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="68037-255">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Página Inicio o Índice](start-mvc/_static/privacy.png)

  <span data-ttu-id="68037-257">En la siguiente imagen se muestra la aplicación tras haber aceptado el seguimiento:</span><span class="sxs-lookup"><span data-stu-id="68037-257">The following image shows the app after accepting tracking:</span></span>

  ![Página Inicio o Índice](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="68037-259">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="68037-259">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="68037-260">Seleccione **Ejecutar** > **Iniciar sin depurar** para iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="68037-260">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="68037-261">Visual Studio para Mac inicia el servidor [Kestrel](xref:fundamentals/servers/index#kestrel), inicia un explorador y navega a `http://localhost:port`, donde *port* es un número de puerto elegido aleatoriamente.</span><span class="sxs-lookup"><span data-stu-id="68037-261">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="68037-262">En la barra de direcciones aparece `localhost:port#` (y no algo como `example.com`).</span><span class="sxs-lookup"><span data-stu-id="68037-262">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="68037-263">Esto es así porque `localhost` es el nombre de host estándar del equipo local.</span><span class="sxs-lookup"><span data-stu-id="68037-263">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="68037-264">Cuando Visual Studio crea un proyecto web, se usa un puerto aleatorio para el servidor web.</span><span class="sxs-lookup"><span data-stu-id="68037-264">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="68037-265">Al ejecutar la aplicación verá otro puerto distinto.</span><span class="sxs-lookup"><span data-stu-id="68037-265">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="68037-266">Puede iniciar la aplicación en modo de depuración o en modo de no depuración desde el menú **Ejecutar**.</span><span class="sxs-lookup"><span data-stu-id="68037-266">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

* <span data-ttu-id="68037-267">Seleccione **Aceptar** para dar su consentimiento al seguimiento.</span><span class="sxs-lookup"><span data-stu-id="68037-267">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="68037-268">Esta aplicación no lleva un seguimiento de la información personal.</span><span class="sxs-lookup"><span data-stu-id="68037-268">This app doesn't track personal information.</span></span> <span data-ttu-id="68037-269">El código generado con plantilla incluye activos que sirven para cumplir el [Reglamento general de protección de datos (RGPD)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="68037-269">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Página Inicio o Índice](./start-mvc/_static/output_privacy_macos.png)

  <span data-ttu-id="68037-271">En la siguiente imagen se muestra la aplicación tras haber aceptado el seguimiento:</span><span class="sxs-lookup"><span data-stu-id="68037-271">The following image shows the app after accepting tracking:</span></span>

  ![Página Inicio o Índice](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="68037-273">En la siguiente sección de este tutorial conocerá MVC y empezará a escribir código.</span><span class="sxs-lookup"><span data-stu-id="68037-273">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="68037-274">Siguiente</span><span class="sxs-lookup"><span data-stu-id="68037-274">Next</span></span>](adding-controller.md)

::: moniker-end
