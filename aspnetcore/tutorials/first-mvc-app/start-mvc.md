---
title: Introducción a ASP.NET Core MVC
author: rick-anderson
description: Obtenga información sobre cómo empezar a usar ASP.NET Core MVC.
ms.author: riande
ms.date: 10/16/2019
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 901257efdfbc7b36249233745175f5ed253da2c7
ms.sourcegitcommit: da2fb2d78ce70accdba903ccbfdcfffdd0112123
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/07/2020
ms.locfileid: "75722904"
---
# <a name="get-started-with-aspnet-core-mvc"></a><span data-ttu-id="411bd-103">Introducción a ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="411bd-103">Get started with ASP.NET Core MVC</span></span>

<span data-ttu-id="411bd-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="411bd-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="411bd-105">En este tutorial se enseñan los conceptos básicos de la compilación de una aplicación web ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="411bd-105">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="411bd-106">La aplicación administra una base de datos de títulos de películas.</span><span class="sxs-lookup"><span data-stu-id="411bd-106">The app manages a database of movie titles.</span></span> <span data-ttu-id="411bd-107">Aprenderá a:</span><span class="sxs-lookup"><span data-stu-id="411bd-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="411bd-108">Crear una aplicación web.</span><span class="sxs-lookup"><span data-stu-id="411bd-108">Create a web app.</span></span>
> * <span data-ttu-id="411bd-109">Agregar un modelo y aplicarle scaffolding.</span><span class="sxs-lookup"><span data-stu-id="411bd-109">Add and scaffold a model.</span></span>
> * <span data-ttu-id="411bd-110">Trabajar con una base de datos.</span><span class="sxs-lookup"><span data-stu-id="411bd-110">Work with a database.</span></span>
> * <span data-ttu-id="411bd-111">Agregar búsqueda y validación.</span><span class="sxs-lookup"><span data-stu-id="411bd-111">Add search and validation.</span></span>

<span data-ttu-id="411bd-112">Al final, tendrá una aplicación que le permitirá administrar y mostrar datos de películas.</span><span class="sxs-lookup"><span data-stu-id="411bd-112">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

## <a name="prerequisites"></a><span data-ttu-id="411bd-113">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="411bd-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="411bd-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="411bd-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.1.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="411bd-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="411bd-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.1.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="411bd-116">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="411bd-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.1.md)]

---

## <a name="create-a-web-app"></a><span data-ttu-id="411bd-117">Creación de una aplicación web</span><span class="sxs-lookup"><span data-stu-id="411bd-117">Create a web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="411bd-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="411bd-118">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="411bd-119">En Visual Studio, seleccione **Crear un proyecto**.</span><span class="sxs-lookup"><span data-stu-id="411bd-119">From the Visual Studio select **Create a new project**.</span></span>

* <span data-ttu-id="411bd-120">Seleccione **Aplicación web ASP.NET Core** y, después, **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="411bd-120">Select **ASP.NET Core Web Application** and then select **Next**.</span></span>

![Nueva aplicación web de ASP.NET Core](start-mvc/_static/np_2.1.png)

* <span data-ttu-id="411bd-122">Asigne el nombre **MvcMovie** al proyecto y seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="411bd-122">Name the project **MvcMovie** and select **Create**.</span></span> <span data-ttu-id="411bd-123">Es importante que el proyecto se llame **MvcMovie** para que, al copiar el código, coincida con el espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="411bd-123">It's important to name the project **MvcMovie** so when you copy code, the namespace will match.</span></span>

  ![Nueva aplicación web de ASP.NET Core](start-mvc/_static/config.png)

* <span data-ttu-id="411bd-125">Seleccione **Aplicación web (Modelo-Vista-Controlador)** y, luego, **Crear**.</span><span class="sxs-lookup"><span data-stu-id="411bd-125">Select **Web Application(Model-View-Controller)**, and then select **Create**.</span></span>

![<span data-ttu-id="411bd-126">Cuadro de diálogo Nuevo proyecto, .NET CORE en el panel izquierdo, Aplicación web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="411bd-126">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project30.png)

<span data-ttu-id="411bd-127">Visual Studio ha usado la plantilla predeterminada para el proyecto de MVC que acaba de crear.</span><span class="sxs-lookup"><span data-stu-id="411bd-127">Visual Studio used the default template for the MVC project you just created.</span></span> <span data-ttu-id="411bd-128">Si escribe un nombre de proyecto y selecciona algunas opciones, dispondrá de inmediato de una aplicación operativa.</span><span class="sxs-lookup"><span data-stu-id="411bd-128">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="411bd-129">Se trata de un proyecto básico de inicio.</span><span class="sxs-lookup"><span data-stu-id="411bd-129">This is a basic starter project.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="411bd-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="411bd-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="411bd-131">Para realizar el tutorial debe estar familiarizado con VS Code.</span><span class="sxs-lookup"><span data-stu-id="411bd-131">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="411bd-132">Para más información, vea [Getting started with VS Code](https://code.visualstudio.com/docs) (Introducción a VS Code) y [Visual Studio Code help](#visual-studio-code-help) (Ayuda de Visual Studio Code).</span><span class="sxs-lookup"><span data-stu-id="411bd-132">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="411bd-133">Abra el [terminal integrado](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="411bd-133">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="411bd-134">Cambie los directorios (`cd`) a una carpeta que contenga el proyecto.</span><span class="sxs-lookup"><span data-stu-id="411bd-134">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="411bd-135">Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="411bd-135">Run the following command:</span></span>

   ```dotnetcli
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="411bd-136">Se muestra un cuadro de diálogo con el texto **Required assets to build and debug are missing from 'MvcMovie'. Add them?** (Faltan los activos necesarios para compilar y depurar en "RazorPagesMovie". ¿Desea agregarlos?).</span><span class="sxs-lookup"><span data-stu-id="411bd-136">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="411bd-137">Seleccione **Sí**.</span><span class="sxs-lookup"><span data-stu-id="411bd-137">Select **Yes**</span></span>

  * <span data-ttu-id="411bd-138">`dotnet new mvc -o MvcMovie`: crea un nuevo proyecto de ASP.NET Core MVC en la carpeta *MvcMovie*.</span><span class="sxs-lookup"><span data-stu-id="411bd-138">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="411bd-139">`code -r MvcMovie`: carga el archivo de proyecto *MvcMovie.csproj* en Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="411bd-139">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="411bd-140">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="411bd-140">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="411bd-141">Seleccione **Archivo** > **Nueva solución**.</span><span class="sxs-lookup"><span data-stu-id="411bd-141">Select **File** > **New Solution**.</span></span>

  ![macOS: Nueva solución](./start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="411bd-143">Seleccione **.NET Core** > **Aplicación** > **Aplicación web (Modelo-Vista-Controlador)** > **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="411bd-143">Select **.NET Core** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span>

  ![Cuadro de diálogo de nuevo proyecto de macOS](./start-mvc/_static/new_project_mvc_vsmac.png)

* <span data-ttu-id="411bd-145">En el cuadro de diálogo **Configurar la nueva API web de ASP.NET Core**, establezca el **Marco de trabajo de destino** de **.NET Core 3.1**.</span><span class="sxs-lookup"><span data-stu-id="411bd-145">In the **Configure your new ASP.NET Core Web API** dialog, set the  **Target Framework** of **.NET Core 3.1**.</span></span>

  ![Selección de .NET Core 3.1 de macOS](./start-mvc/_static/new_project_31_vsmac.png)

* <span data-ttu-id="411bd-147">Asigne el nombre **MvcMovie** al proyecto y, después, seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="411bd-147">Name the project **MvcMovie**, and then select **Create**.</span></span>

---

### <a name="run-the-app"></a><span data-ttu-id="411bd-148">Ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="411bd-148">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="411bd-149">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="411bd-149">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="411bd-150">Presione **Ctrl-F5** para ejecutar la aplicación en modo de no depuración.</span><span class="sxs-lookup"><span data-stu-id="411bd-150">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="411bd-151">Visual Studio inicia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) y ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="411bd-151">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="411bd-152">Tenga en cuenta que en la barra de direcciones aparece `localhost:port#` (y no algo como `example.com`).</span><span class="sxs-lookup"><span data-stu-id="411bd-152">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="411bd-153">Esto es así porque `localhost` es el nombre de host estándar del equipo local.</span><span class="sxs-lookup"><span data-stu-id="411bd-153">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="411bd-154">Cuando Visual Studio crea un proyecto web, se usa un puerto aleatorio para el servidor web.</span><span class="sxs-lookup"><span data-stu-id="411bd-154">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="411bd-155">El inicio de la aplicación con Ctrl+F5 (modo de no depuración) permite realizar cambios en el código, guardar el archivo, actualizar el explorador y ver los cambios de código.</span><span class="sxs-lookup"><span data-stu-id="411bd-155">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="411bd-156">Muchos desarrolladores prefieren usar el modo de no depuración para iniciar la aplicación rápidamente y ver los cambios.</span><span class="sxs-lookup"><span data-stu-id="411bd-156">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="411bd-157">Puede iniciar la aplicación en modo de depuración o en modo de no depuración desde el elemento de menú **Depurar**:</span><span class="sxs-lookup"><span data-stu-id="411bd-157">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![Menú Depurar](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="411bd-159">Puede depurar la aplicación seleccionando el botón **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="411bd-159">You can debug the app by selecting the **IIS Express** button</span></span>

  ![IIS Express](start-mvc/_static/iis_express.png)

  <span data-ttu-id="411bd-161">En la imagen siguiente se muestra la aplicación:</span><span class="sxs-lookup"><span data-stu-id="411bd-161">The following image shows the app:</span></span>

  ![Página Inicio o Índice](start-mvc/_static/home2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="411bd-163">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="411bd-163">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="411bd-164">Presione Ctrl+F5 para ejecutarla sin el depurador.</span><span class="sxs-lookup"><span data-stu-id="411bd-164">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="411bd-165">Visual Studio Code inicia [Kestrel](xref:fundamentals/servers/kestrel) y un explorador, y se desplaza hasta `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="411bd-165">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="411bd-166">En la barra de direcciones aparece `localhost:port:5001` (y no algo como `example.com`).</span><span class="sxs-lookup"><span data-stu-id="411bd-166">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="411bd-167">Esto es así porque `localhost` es el nombre de host estándar del equipo local.</span><span class="sxs-lookup"><span data-stu-id="411bd-167">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="411bd-168">Localhost solo sirve las solicitudes web del equipo local.</span><span class="sxs-lookup"><span data-stu-id="411bd-168">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="411bd-169">El inicio de la aplicación con Ctrl+F5 (modo de no depuración) permite realizar cambios en el código, guardar el archivo, actualizar el explorador y ver los cambios de código.</span><span class="sxs-lookup"><span data-stu-id="411bd-169">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="411bd-170">Muchos desarrolladores prefieren usar el modo de no depuración para actualizar la página y ver los cambios.</span><span class="sxs-lookup"><span data-stu-id="411bd-170">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

  ![Página Inicio o Índice](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="411bd-172">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="411bd-172">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="411bd-173">Seleccione **Ejecutar** > **Iniciar sin depurar** para iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="411bd-173">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="411bd-174">Visual Studio para Mac inicia el servidor [Kestrel](xref:fundamentals/servers/index#kestrel), inicia un explorador y navega a `http://localhost:port`, donde *port* es un número de puerto elegido aleatoriamente.</span><span class="sxs-lookup"><span data-stu-id="411bd-174">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="411bd-175">En la barra de direcciones aparece `localhost:port#` (y no algo como `example.com`).</span><span class="sxs-lookup"><span data-stu-id="411bd-175">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="411bd-176">Esto es así porque `localhost` es el nombre de host estándar del equipo local.</span><span class="sxs-lookup"><span data-stu-id="411bd-176">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="411bd-177">Cuando Visual Studio crea un proyecto web, se usa un puerto aleatorio para el servidor web.</span><span class="sxs-lookup"><span data-stu-id="411bd-177">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="411bd-178">Al ejecutar la aplicación verá otro puerto distinto.</span><span class="sxs-lookup"><span data-stu-id="411bd-178">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="411bd-179">Puede iniciar la aplicación en modo de depuración o en modo de no depuración desde el menú **Ejecutar**.</span><span class="sxs-lookup"><span data-stu-id="411bd-179">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

  <span data-ttu-id="411bd-180">En la imagen siguiente se muestra la aplicación:</span><span class="sxs-lookup"><span data-stu-id="411bd-180">The following image shows the app:</span></span>

  ![Página Inicio o Índice](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="411bd-182">En la siguiente sección de este tutorial conocerá MVC y empezará a escribir código.</span><span class="sxs-lookup"><span data-stu-id="411bd-182">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="411bd-183">Siguiente</span><span class="sxs-lookup"><span data-stu-id="411bd-183">Next</span></span>](adding-controller.md)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="411bd-184">En este tutorial se enseñan los conceptos básicos de la compilación de una aplicación web ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="411bd-184">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="411bd-185">La aplicación administra una base de datos de títulos de películas.</span><span class="sxs-lookup"><span data-stu-id="411bd-185">The app manages a database of movie titles.</span></span> <span data-ttu-id="411bd-186">Aprenderá a:</span><span class="sxs-lookup"><span data-stu-id="411bd-186">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="411bd-187">Crear una aplicación web.</span><span class="sxs-lookup"><span data-stu-id="411bd-187">Create a web app.</span></span>
> * <span data-ttu-id="411bd-188">Agregar un modelo y aplicarle scaffolding.</span><span class="sxs-lookup"><span data-stu-id="411bd-188">Add and scaffold a model.</span></span>
> * <span data-ttu-id="411bd-189">Trabajar con una base de datos.</span><span class="sxs-lookup"><span data-stu-id="411bd-189">Work with a database.</span></span>
> * <span data-ttu-id="411bd-190">Agregar búsqueda y validación.</span><span class="sxs-lookup"><span data-stu-id="411bd-190">Add search and validation.</span></span>

<span data-ttu-id="411bd-191">Al final, tendrá una aplicación que le permitirá administrar y mostrar datos de películas.</span><span class="sxs-lookup"><span data-stu-id="411bd-191">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

## <a name="prerequisites"></a><span data-ttu-id="411bd-192">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="411bd-192">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="411bd-193">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="411bd-193">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="411bd-194">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="411bd-194">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="411bd-195">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="411bd-195">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---
## <a name="create-a-web-app"></a><span data-ttu-id="411bd-196">Creación de una aplicación web</span><span class="sxs-lookup"><span data-stu-id="411bd-196">Create a web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="411bd-197">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="411bd-197">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="411bd-198">En Visual Studio, seleccione **Crear un proyecto**.</span><span class="sxs-lookup"><span data-stu-id="411bd-198">From the Visual Studio select **Create a new project**.</span></span>

* <span data-ttu-id="411bd-199">Seleccione **Aplicación web ASP.NET Core** y, después, **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="411bd-199">Select **ASP.NET Core Web Application** and then select **Next**.</span></span>

![Nueva aplicación web de ASP.NET Core](start-mvc/_static/np_2.1.png)

* <span data-ttu-id="411bd-201">Asigne el nombre **MvcMovie** al proyecto y seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="411bd-201">Name the project **MvcMovie** and select **Create**.</span></span> <span data-ttu-id="411bd-202">Es importante que el proyecto se llame **MvcMovie** para que, al copiar el código, coincida con el espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="411bd-202">It's important to name the project **MvcMovie** so when you copy code, the namespace will match.</span></span>

  ![Nueva aplicación web de ASP.NET Core](start-mvc/_static/config.png)


* <span data-ttu-id="411bd-204">Seleccione **Aplicación web (Modelo-Vista-Controlador)** y, luego, **Crear**.</span><span class="sxs-lookup"><span data-stu-id="411bd-204">Select **Web Application(Model-View-Controller)**, and then select **Create**.</span></span>

![<span data-ttu-id="411bd-205">Cuadro de diálogo Nuevo proyecto, .NET CORE en el panel izquierdo, Aplicación web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="411bd-205">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="411bd-206">Visual Studio ha usado la plantilla predeterminada para el proyecto de MVC que acaba de crear.</span><span class="sxs-lookup"><span data-stu-id="411bd-206">Visual Studio used the default template for the MVC project you just created.</span></span> <span data-ttu-id="411bd-207">Si escribe un nombre de proyecto y selecciona algunas opciones, dispondrá de inmediato de una aplicación operativa.</span><span class="sxs-lookup"><span data-stu-id="411bd-207">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="411bd-208">Se trata de un proyecto introductorio básico, pero es un buen punto de partida.</span><span class="sxs-lookup"><span data-stu-id="411bd-208">This is a basic starter project, and it's a good place to start.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="411bd-209">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="411bd-209">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="411bd-210">Para realizar el tutorial debe estar familiarizado con VS Code.</span><span class="sxs-lookup"><span data-stu-id="411bd-210">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="411bd-211">Para más información, vea [Getting started with VS Code](https://code.visualstudio.com/docs) (Introducción a VS Code) y [Visual Studio Code help](#visual-studio-code-help) (Ayuda de Visual Studio Code).</span><span class="sxs-lookup"><span data-stu-id="411bd-211">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="411bd-212">Abra el [terminal integrado](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="411bd-212">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="411bd-213">Cambie los directorios (`cd`) a una carpeta que contenga el proyecto.</span><span class="sxs-lookup"><span data-stu-id="411bd-213">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="411bd-214">Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="411bd-214">Run the following command:</span></span>

   ```dotnetcli
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="411bd-215">Se muestra un cuadro de diálogo con el texto **Required assets to build and debug are missing from 'MvcMovie'. Add them?** (Faltan los activos necesarios para compilar y depurar en "RazorPagesMovie". ¿Desea agregarlos?).</span><span class="sxs-lookup"><span data-stu-id="411bd-215">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="411bd-216">Seleccione **Sí**.</span><span class="sxs-lookup"><span data-stu-id="411bd-216">Select **Yes**</span></span>

  * <span data-ttu-id="411bd-217">`dotnet new mvc -o MvcMovie`: crea un nuevo proyecto de ASP.NET Core MVC en la carpeta *MvcMovie*.</span><span class="sxs-lookup"><span data-stu-id="411bd-217">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="411bd-218">`code -r MvcMovie`: carga el archivo de proyecto *MvcMovie.csproj* en Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="411bd-218">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="411bd-219">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="411bd-219">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="411bd-220">Seleccione **Archivo** > **Nueva solución**.</span><span class="sxs-lookup"><span data-stu-id="411bd-220">Select **File** > **New Solution**.</span></span>

  ![macOS: Nueva solución](./start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="411bd-222">Seleccione **.NET Core** > **Aplicación** > **Aplicación web (Modelo-Vista-Controlador)** > **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="411bd-222">Select **.NET Core** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span>

  ![Cuadro de diálogo de nuevo proyecto de macOS](./start-mvc/_static/new_project_mvc_vsmac.png)

* <span data-ttu-id="411bd-224">En el cuadro de diálogo **Configurar la nueva API web de ASP.NET Core**, acepte el **Marco de trabajo de destino** predeterminado de **.NET Core 2.2**.</span><span class="sxs-lookup"><span data-stu-id="411bd-224">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of **.NET Core 2.2**.</span></span>

  ![Selección de .NET Core 2.2 de macOS](./start-mvc/_static/new_project_22_vsmac.png)

* <span data-ttu-id="411bd-226">Asigne el nombre **MvcMovie** al proyecto y, después, seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="411bd-226">Name the project **MvcMovie**, and then select **Create**.</span></span>

---

### <a name="run-the-app"></a><span data-ttu-id="411bd-227">Ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="411bd-227">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="411bd-228">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="411bd-228">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="411bd-229">Presione **Ctrl-F5** para ejecutar la aplicación en modo de no depuración.</span><span class="sxs-lookup"><span data-stu-id="411bd-229">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="411bd-230">Visual Studio inicia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) y ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="411bd-230">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="411bd-231">Tenga en cuenta que en la barra de direcciones aparece `localhost:port#` (y no algo como `example.com`).</span><span class="sxs-lookup"><span data-stu-id="411bd-231">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="411bd-232">Esto es así porque `localhost` es el nombre de host estándar del equipo local.</span><span class="sxs-lookup"><span data-stu-id="411bd-232">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="411bd-233">Cuando Visual Studio crea un proyecto web, se usa un puerto aleatorio para el servidor web.</span><span class="sxs-lookup"><span data-stu-id="411bd-233">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="411bd-234">El inicio de la aplicación con Ctrl+F5 (modo de no depuración) permite realizar cambios en el código, guardar el archivo, actualizar el explorador y ver los cambios de código.</span><span class="sxs-lookup"><span data-stu-id="411bd-234">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="411bd-235">Muchos desarrolladores prefieren usar el modo de no depuración para iniciar la aplicación rápidamente y ver los cambios.</span><span class="sxs-lookup"><span data-stu-id="411bd-235">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="411bd-236">Puede iniciar la aplicación en modo de depuración o en modo de no depuración desde el elemento de menú **Depurar**:</span><span class="sxs-lookup"><span data-stu-id="411bd-236">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![Menú Depurar](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="411bd-238">Puede depurar la aplicación seleccionando el botón **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="411bd-238">You can debug the app by selecting the **IIS Express** button</span></span>

  ![IIS Express](start-mvc/_static/iis_express.png)

* <span data-ttu-id="411bd-240">Seleccione **Aceptar** para dar su consentimiento al seguimiento.</span><span class="sxs-lookup"><span data-stu-id="411bd-240">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="411bd-241">Esta aplicación no lleva un seguimiento de la información personal.</span><span class="sxs-lookup"><span data-stu-id="411bd-241">This app doesn't track personal information.</span></span> <span data-ttu-id="411bd-242">El código generado con plantilla incluye activos que sirven para cumplir el [Reglamento general de protección de datos (RGPD)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="411bd-242">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Página Inicio o Índice](start-mvc/_static/privacy.png)

  <span data-ttu-id="411bd-244">En la siguiente imagen se muestra la aplicación tras haber aceptado el seguimiento:</span><span class="sxs-lookup"><span data-stu-id="411bd-244">The following image shows the app after accepting tracking:</span></span>

  ![Página Inicio o Índice](start-mvc/_static/home2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="411bd-246">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="411bd-246">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="411bd-247">Presione Ctrl+F5 para ejecutarla sin el depurador.</span><span class="sxs-lookup"><span data-stu-id="411bd-247">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="411bd-248">Visual Studio Code inicia [Kestrel](xref:fundamentals/servers/kestrel) y un explorador, y se desplaza hasta `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="411bd-248">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="411bd-249">En la barra de direcciones aparece `localhost:port:5001` (y no algo como `example.com`).</span><span class="sxs-lookup"><span data-stu-id="411bd-249">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="411bd-250">Esto es así porque `localhost` es el nombre de host estándar del equipo local.</span><span class="sxs-lookup"><span data-stu-id="411bd-250">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="411bd-251">Localhost solo sirve las solicitudes web del equipo local.</span><span class="sxs-lookup"><span data-stu-id="411bd-251">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="411bd-252">El inicio de la aplicación con Ctrl+F5 (modo de no depuración) permite realizar cambios en el código, guardar el archivo, actualizar el explorador y ver los cambios de código.</span><span class="sxs-lookup"><span data-stu-id="411bd-252">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="411bd-253">Muchos desarrolladores prefieren usar el modo de no depuración para actualizar la página y ver los cambios.</span><span class="sxs-lookup"><span data-stu-id="411bd-253">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

* <span data-ttu-id="411bd-254">Seleccione **Aceptar** para dar su consentimiento al seguimiento.</span><span class="sxs-lookup"><span data-stu-id="411bd-254">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="411bd-255">Esta aplicación no lleva un seguimiento de la información personal.</span><span class="sxs-lookup"><span data-stu-id="411bd-255">This app doesn't track personal information.</span></span> <span data-ttu-id="411bd-256">El código generado con plantilla incluye activos que sirven para cumplir el [Reglamento general de protección de datos (RGPD)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="411bd-256">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Página Inicio o Índice](start-mvc/_static/privacy.png)

  <span data-ttu-id="411bd-258">En la siguiente imagen se muestra la aplicación tras haber aceptado el seguimiento:</span><span class="sxs-lookup"><span data-stu-id="411bd-258">The following image shows the app after accepting tracking:</span></span>

  ![Página Inicio o Índice](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="411bd-260">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="411bd-260">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="411bd-261">Seleccione **Ejecutar** > **Iniciar sin depurar** para iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="411bd-261">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="411bd-262">Visual Studio para Mac inicia el servidor [Kestrel](xref:fundamentals/servers/index#kestrel), inicia un explorador y navega a `http://localhost:port`, donde *port* es un número de puerto elegido aleatoriamente.</span><span class="sxs-lookup"><span data-stu-id="411bd-262">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="411bd-263">En la barra de direcciones aparece `localhost:port#` (y no algo como `example.com`).</span><span class="sxs-lookup"><span data-stu-id="411bd-263">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="411bd-264">Esto es así porque `localhost` es el nombre de host estándar del equipo local.</span><span class="sxs-lookup"><span data-stu-id="411bd-264">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="411bd-265">Cuando Visual Studio crea un proyecto web, se usa un puerto aleatorio para el servidor web.</span><span class="sxs-lookup"><span data-stu-id="411bd-265">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="411bd-266">Al ejecutar la aplicación verá otro puerto distinto.</span><span class="sxs-lookup"><span data-stu-id="411bd-266">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="411bd-267">Puede iniciar la aplicación en modo de depuración o en modo de no depuración desde el menú **Ejecutar**.</span><span class="sxs-lookup"><span data-stu-id="411bd-267">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

* <span data-ttu-id="411bd-268">Seleccione **Aceptar** para dar su consentimiento al seguimiento.</span><span class="sxs-lookup"><span data-stu-id="411bd-268">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="411bd-269">Esta aplicación no lleva un seguimiento de la información personal.</span><span class="sxs-lookup"><span data-stu-id="411bd-269">This app doesn't track personal information.</span></span> <span data-ttu-id="411bd-270">El código generado con plantilla incluye activos que sirven para cumplir el [Reglamento general de protección de datos (RGPD)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="411bd-270">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Página Inicio o Índice](./start-mvc/_static/output_privacy_macos.png)

  <span data-ttu-id="411bd-272">En la siguiente imagen se muestra la aplicación tras haber aceptado el seguimiento:</span><span class="sxs-lookup"><span data-stu-id="411bd-272">The following image shows the app after accepting tracking:</span></span>

  ![Página Inicio o Índice](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="411bd-274">En la siguiente sección de este tutorial conocerá MVC y empezará a escribir código.</span><span class="sxs-lookup"><span data-stu-id="411bd-274">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="411bd-275">Siguiente</span><span class="sxs-lookup"><span data-stu-id="411bd-275">Next</span></span>](adding-controller.md)

::: moniker-end
