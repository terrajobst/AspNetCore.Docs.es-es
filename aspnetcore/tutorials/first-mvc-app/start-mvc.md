---
title: Introducción a ASP.NET Core MVC
author: rick-anderson
description: Obtenga información sobre cómo empezar a usar ASP.NET Core MVC.
ms.author: riande
ms.date: 12/12/2018
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: dbc07558d7d7672e60e8834dc3e4e9d8aab437e3
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2019
ms.locfileid: "58265287"
---
# <a name="get-started-with-aspnet-core-mvc"></a><span data-ttu-id="6e236-103">Introducción a ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="6e236-103">Get started with ASP.NET Core MVC</span></span>

<span data-ttu-id="6e236-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="6e236-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="6e236-105">En este tutorial se enseñan los conceptos básicos de la compilación de una aplicación web ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="6e236-105">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="6e236-106">La aplicación administra una base de datos de títulos de películas.</span><span class="sxs-lookup"><span data-stu-id="6e236-106">The app manages a database of movie titles.</span></span> <span data-ttu-id="6e236-107">Aprenderá a:</span><span class="sxs-lookup"><span data-stu-id="6e236-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="6e236-108">Crear una aplicación web.</span><span class="sxs-lookup"><span data-stu-id="6e236-108">Create a web app.</span></span>
> * <span data-ttu-id="6e236-109">Agregar un modelo y aplicarle scaffolding.</span><span class="sxs-lookup"><span data-stu-id="6e236-109">Add and scaffold a model.</span></span>
> * <span data-ttu-id="6e236-110">Trabajar con una base de datos.</span><span class="sxs-lookup"><span data-stu-id="6e236-110">Work with a database.</span></span>
> * <span data-ttu-id="6e236-111">Agregar búsqueda y validación.</span><span class="sxs-lookup"><span data-stu-id="6e236-111">Add search and validation.</span></span>

<span data-ttu-id="6e236-112">Al final, tendrá una aplicación que le permitirá administrar y mostrar datos de películas.</span><span class="sxs-lookup"><span data-stu-id="6e236-112">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-web-app"></a><span data-ttu-id="6e236-113">Creación de una aplicación web</span><span class="sxs-lookup"><span data-stu-id="6e236-113">Create a web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6e236-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6e236-114">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="6e236-115">En Visual Studio, seleccione **Archivo > Nuevo > Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="6e236-115">From Visual Studio, select  **File > New > Project**.</span></span>

![Archivo > Nuevo > Proyecto](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="6e236-117">Complete el cuadro de diálogo **Nuevo proyecto**:</span><span class="sxs-lookup"><span data-stu-id="6e236-117">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="6e236-118">En el panel izquierdo, seleccione **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="6e236-118">In the left pane, select **.NET Core**</span></span>
* <span data-ttu-id="6e236-119">En el panel central, seleccione **Aplicación web ASP.NET Core (.NET Core)**</span><span class="sxs-lookup"><span data-stu-id="6e236-119">In the center pane, select **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="6e236-120">Asigne al proyecto el nombre "MvcMovie" (es importante asignarle este nombre para que, al copiar el código, coincida con el espacio de nombres).</span><span class="sxs-lookup"><span data-stu-id="6e236-120">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="6e236-121">Seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="6e236-121">select **OK**</span></span>

![<span data-ttu-id="6e236-122">Cuadro de diálogo Nuevo proyecto, .NET CORE en el panel izquierdo, Aplicación web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6e236-122">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2-21.png)

<span data-ttu-id="6e236-123">Complete el cuadro de diálogo **Nueva aplicación web ASP.NET Core (.NET Core) - MvcMovie**:</span><span class="sxs-lookup"><span data-stu-id="6e236-123">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="6e236-124">En el cuadro desplegable del selector de versión, seleccione **ASP.NET Core 2.2**.</span><span class="sxs-lookup"><span data-stu-id="6e236-124">In the version selector drop-down box select **ASP.NET Core 2.2**</span></span>
* <span data-ttu-id="6e236-125">Seleccione **Aplicación web (Modelo-Vista-Controlador)**.</span><span class="sxs-lookup"><span data-stu-id="6e236-125">Select **Web Application (Model-View-Controller)**</span></span>
* <span data-ttu-id="6e236-126">Seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="6e236-126">select **OK**.</span></span>

![<span data-ttu-id="6e236-127">Cuadro de diálogo Nuevo proyecto, .NET CORE en el panel izquierdo, Aplicación web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6e236-127">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="6e236-128">Visual Studio ha usado una plantilla predeterminada para el proyecto de MVC que acaba de crear.</span><span class="sxs-lookup"><span data-stu-id="6e236-128">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="6e236-129">Si escribe un nombre de proyecto y selecciona algunas opciones, dispondrá de inmediato de una aplicación operativa.</span><span class="sxs-lookup"><span data-stu-id="6e236-129">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="6e236-130">Se trata de un proyecto introductorio básico, pero es un buen punto de partida.</span><span class="sxs-lookup"><span data-stu-id="6e236-130">This is a basic starter project, and it's a good place to start.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6e236-131">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6e236-131">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="6e236-132">Para realizar el tutorial debe estar familiarizado con VS Code.</span><span class="sxs-lookup"><span data-stu-id="6e236-132">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="6e236-133">Para más información, vea [Getting started with VS Code](https://code.visualstudio.com/docs) (Introducción a VS Code) y [Visual Studio Code help](#visual-studio-code-help) (Ayuda de Visual Studio Code).</span><span class="sxs-lookup"><span data-stu-id="6e236-133">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="6e236-134">Abra el [terminal integrado](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="6e236-134">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="6e236-135">Cambie los directorios (`cd`) a una carpeta que contenga el proyecto.</span><span class="sxs-lookup"><span data-stu-id="6e236-135">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="6e236-136">Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="6e236-136">Run the following command:</span></span>

   ```console
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="6e236-137">Se muestra un cuadro de diálogo con el texto **Required assets to build and debug are missing from 'MvcMovie'. Add them?** (Faltan los activos necesarios para compilar y depurar en "RazorPagesMovie". ¿Desea agregarlos?).</span><span class="sxs-lookup"><span data-stu-id="6e236-137">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="6e236-138">Seleccione **Sí**.</span><span class="sxs-lookup"><span data-stu-id="6e236-138">Select **Yes**</span></span>

  * <span data-ttu-id="6e236-139">`dotnet new mvc -o MvcMovie`: crea un nuevo proyecto de ASP.NET Core MVC en la carpeta *MvcMovie*.</span><span class="sxs-lookup"><span data-stu-id="6e236-139">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="6e236-140">`code -r MvcMovie`: carga el archivo de proyecto *MvcMovie.csproj* en Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="6e236-140">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6e236-141">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="6e236-141">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="6e236-142">Seleccione **Archivo** > **Nueva solución**.</span><span class="sxs-lookup"><span data-stu-id="6e236-142">Select **File** > **New Solution**.</span></span>

  ![macOS: Nueva solución](~/tutorials/first-web-api-mac/_static/sln.png)

* <span data-ttu-id="6e236-144">Seleccione **Aplicación .NET Core** > **ASP.NET Core** > **Aplicación web ASP.NET Core (MVC)** > **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="6e236-144">Select **.NET Core App** > **ASP.NET Core** > **ASP.NET Core Web App (MVC)** > **Next**.</span></span>

  ![Cuadro de diálogo de nuevo proyecto de macOS](~/tutorials/first-mvc-app-mac/start-mvc/1.png)

* <span data-ttu-id="6e236-146">En el cuadro de diálogo **Configurar la nueva API web de ASP.NET Core**, acepte la **plataforma de destino** predeterminada de \**.NET Core 2.2*.</span><span class="sxs-lookup"><span data-stu-id="6e236-146">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of \**.NET Core 2.2*.</span></span>

* <span data-ttu-id="6e236-147">Asigne el nombre **MvcMovie** al proyecto y, después, seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="6e236-147">Name the project **MvcMovie**, and then select **Create**.</span></span>

---

### <a name="run-the-app"></a><span data-ttu-id="6e236-148">Ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="6e236-148">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6e236-149">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6e236-149">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="6e236-150">Presione **Ctrl-F5** para ejecutar la aplicación en modo de no depuración.</span><span class="sxs-lookup"><span data-stu-id="6e236-150">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="6e236-151">Visual Studio inicia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) y ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="6e236-151">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="6e236-152">Tenga en cuenta que en la barra de direcciones aparece `localhost:port#` (y no algo como `example.com`).</span><span class="sxs-lookup"><span data-stu-id="6e236-152">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="6e236-153">Esto es así porque `localhost` es el nombre de host estándar del equipo local.</span><span class="sxs-lookup"><span data-stu-id="6e236-153">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="6e236-154">Cuando Visual Studio crea un proyecto web, se usa un puerto aleatorio para el servidor web.</span><span class="sxs-lookup"><span data-stu-id="6e236-154">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="6e236-155">El inicio de la aplicación con Ctrl+F5 (modo de no depuración) permite realizar cambios en el código, guardar el archivo, actualizar el explorador y ver los cambios de código.</span><span class="sxs-lookup"><span data-stu-id="6e236-155">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="6e236-156">Muchos desarrolladores prefieren usar el modo de no depuración para iniciar la aplicación rápidamente y ver los cambios.</span><span class="sxs-lookup"><span data-stu-id="6e236-156">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="6e236-157">Puede iniciar la aplicación en modo de depuración o en modo de no depuración desde el elemento de menú **Depurar**:</span><span class="sxs-lookup"><span data-stu-id="6e236-157">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![Menú Depurar](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="6e236-159">Puede depurar la aplicación seleccionando el botón **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="6e236-159">You can debug the app by selecting the **IIS Express** button</span></span>

  ![IIS Express](start-mvc/_static/iis_express.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6e236-161">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6e236-161">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="6e236-162">Presione Ctrl+F5 para ejecutarla sin el depurador.</span><span class="sxs-lookup"><span data-stu-id="6e236-162">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="6e236-163">Visual Studio Code inicia [Kestrel](xref:fundamentals/servers/kestrel) y un explorador, y se desplaza hasta `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="6e236-163">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="6e236-164">En la barra de direcciones aparece `localhost:port:5001` (y no algo como `example.com`).</span><span class="sxs-lookup"><span data-stu-id="6e236-164">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="6e236-165">Esto es así porque `localhost` es el nombre de host estándar del equipo local.</span><span class="sxs-lookup"><span data-stu-id="6e236-165">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="6e236-166">Localhost solo sirve las solicitudes web del equipo local.</span><span class="sxs-lookup"><span data-stu-id="6e236-166">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="6e236-167">El inicio de la aplicación con Ctrl+F5 (modo de no depuración) permite realizar cambios en el código, guardar el archivo, actualizar el explorador y ver los cambios de código.</span><span class="sxs-lookup"><span data-stu-id="6e236-167">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="6e236-168">Muchos desarrolladores prefieren usar el modo de no depuración para actualizar la página y ver los cambios.</span><span class="sxs-lookup"><span data-stu-id="6e236-168">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6e236-169">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="6e236-169">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="6e236-170">Seleccione **Ejecutar** > **Iniciar sin depurar** para iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="6e236-170">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="6e236-171">Visual Studio para Mac inicia el servidor [Kestrel](xref:fundamentals/servers/index#kestrel), inicia un explorador y navega a `http://localhost:port`, donde *port* es un número de puerto elegido aleatoriamente.</span><span class="sxs-lookup"><span data-stu-id="6e236-171">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="6e236-172">En la barra de direcciones aparece `localhost:port#` (y no algo como `example.com`).</span><span class="sxs-lookup"><span data-stu-id="6e236-172">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="6e236-173">Esto es así porque `localhost` es el nombre de host estándar del equipo local.</span><span class="sxs-lookup"><span data-stu-id="6e236-173">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="6e236-174">Cuando Visual Studio crea un proyecto web, se usa un puerto aleatorio para el servidor web.</span><span class="sxs-lookup"><span data-stu-id="6e236-174">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="6e236-175">Al ejecutar la aplicación verá otro puerto distinto.</span><span class="sxs-lookup"><span data-stu-id="6e236-175">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="6e236-176">Puede iniciar la aplicación en modo de depuración o en modo de no depuración desde el menú **Ejecutar**.</span><span class="sxs-lookup"><span data-stu-id="6e236-176">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

---

* <span data-ttu-id="6e236-177">Seleccione **Aceptar** para dar su consentimiento al seguimiento.</span><span class="sxs-lookup"><span data-stu-id="6e236-177">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="6e236-178">Esta aplicación no lleva un seguimiento de la información personal.</span><span class="sxs-lookup"><span data-stu-id="6e236-178">This app doesn't track personal information.</span></span> <span data-ttu-id="6e236-179">El código generado con plantilla incluye activos que sirven para cumplir el [Reglamento general de protección de datos (RGPD)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="6e236-179">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Página Inicio o Índice](start-mvc/_static/privacy.png)

  <span data-ttu-id="6e236-181">En la siguiente imagen se muestra la aplicación tras haber aceptado el seguimiento:</span><span class="sxs-lookup"><span data-stu-id="6e236-181">The following image shows the app after accepting tracking:</span></span>

  ![Página Inicio o Índice](start-mvc/_static/home2.2.png)

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="6e236-183">En la siguiente sección de este tutorial conocerá MVC y empezará a escribir código.</span><span class="sxs-lookup"><span data-stu-id="6e236-183">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="6e236-184">Siguiente</span><span class="sxs-lookup"><span data-stu-id="6e236-184">Next</span></span>](adding-controller.md)
