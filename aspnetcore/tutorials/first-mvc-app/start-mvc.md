---
title: Introducción a ASP.NET Core MVC
author: rick-anderson
description: Obtenga información sobre cómo empezar a usar ASP.NET Core MVC.
ms.author: riande
ms.date: 12/12/2018
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: f0c2351de017de7f4c62021b8f9478603055e9bc
ms.sourcegitcommit: d75d8eb26c2cce19876c8d5b65ac8a4b21f625ef
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2019
ms.locfileid: "56410552"
---
# <a name="get-started-with-aspnet-core-mvc"></a><span data-ttu-id="62e11-103">Introducción a ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="62e11-103">Get started with ASP.NET Core MVC</span></span>

<span data-ttu-id="62e11-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="62e11-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [consider RP](~/includes/razor.md)]

https://docs.microsoft.com/en-us/visualstudio/ide/visual-studio-ide?view=vs-2017

<span data-ttu-id="62e11-105">En este tutorial se enseñan los conceptos básicos de la compilación de una aplicación web ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="62e11-105">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="62e11-106">La aplicación administra una base de datos de títulos de películas.</span><span class="sxs-lookup"><span data-stu-id="62e11-106">The app manages a database of movie titles.</span></span> <span data-ttu-id="62e11-107">Aprenderá a:</span><span class="sxs-lookup"><span data-stu-id="62e11-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="62e11-108">Crear una aplicación web.</span><span class="sxs-lookup"><span data-stu-id="62e11-108">Create a web app.</span></span>
> * <span data-ttu-id="62e11-109">Agregar un modelo y aplicarle scaffolding.</span><span class="sxs-lookup"><span data-stu-id="62e11-109">Add and scaffold a model.</span></span>
> * <span data-ttu-id="62e11-110">Trabajar con una base de datos.</span><span class="sxs-lookup"><span data-stu-id="62e11-110">Work with a database.</span></span>
> * <span data-ttu-id="62e11-111">Agregar búsqueda y validación.</span><span class="sxs-lookup"><span data-stu-id="62e11-111">Add search and validation.</span></span>

<span data-ttu-id="62e11-112">Al final, tendrá una aplicación que le permitirá administrar y mostrar datos de películas.</span><span class="sxs-lookup"><span data-stu-id="62e11-112">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

> [!NOTE]
> <span data-ttu-id="62e11-113">Vamos a probar la facilidad de uso de una nueva estructura propuesta para la tabla de contenido de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="62e11-113">We’re testing the usability of a proposed new structure for the ASP.NET Core table of contents.</span></span>  <span data-ttu-id="62e11-114">Si tiene unos minutos para realizar un ejercicio de búsqueda de 7 temas diferentes en la tabla actual o propuesta de contenido, [haga clic aquí para participar en el estudio](https://dpk4xbh5.optimalworkshop.com/treejack/aa11wn82).</span><span class="sxs-lookup"><span data-stu-id="62e11-114">If you have a few minutes to try an exercise of finding 7 different topics in the current or proposed table of contents, please [click here to participate in the study](https://dpk4xbh5.optimalworkshop.com/treejack/aa11wn82).</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-web-app"></a><span data-ttu-id="62e11-115">Creación de una aplicación web</span><span class="sxs-lookup"><span data-stu-id="62e11-115">Create a web app</span></span>

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="62e11-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="62e11-116">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="62e11-117">En Visual Studio, seleccione **Archivo > Nuevo > Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="62e11-117">From Visual Studio, select  **File > New > Project**.</span></span>

![Archivo > Nuevo > Proyecto](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="62e11-119">Complete el cuadro de diálogo **Nuevo proyecto**:</span><span class="sxs-lookup"><span data-stu-id="62e11-119">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="62e11-120">En el panel izquierdo, seleccione **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="62e11-120">In the left pane, select **.NET Core**</span></span>
* <span data-ttu-id="62e11-121">En el panel central, seleccione **Aplicación web ASP.NET Core (.NET Core)**</span><span class="sxs-lookup"><span data-stu-id="62e11-121">In the center pane, select **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="62e11-122">Asigne al proyecto el nombre "MvcMovie" (es importante asignarle este nombre para que, al copiar el código, coincida con el espacio de nombres).</span><span class="sxs-lookup"><span data-stu-id="62e11-122">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="62e11-123">Seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="62e11-123">select **OK**</span></span>

![<span data-ttu-id="62e11-124">Cuadro de diálogo Nuevo proyecto, .NET CORE en el panel izquierdo, Aplicación web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="62e11-124">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2-21.png)

<span data-ttu-id="62e11-125">Complete el cuadro de diálogo **Nueva aplicación web ASP.NET Core (.NET Core) - MvcMovie**:</span><span class="sxs-lookup"><span data-stu-id="62e11-125">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="62e11-126">En el cuadro desplegable del selector de versión, seleccione **ASP.NET Core 2.2**.</span><span class="sxs-lookup"><span data-stu-id="62e11-126">In the version selector drop-down box select **ASP.NET Core 2.2**</span></span>
* <span data-ttu-id="62e11-127">Seleccione **Aplicación web (Modelo-Vista-Controlador)**.</span><span class="sxs-lookup"><span data-stu-id="62e11-127">Select **Web Application (Model-View-Controller)**</span></span>
* <span data-ttu-id="62e11-128">Seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="62e11-128">select **OK**.</span></span>

![<span data-ttu-id="62e11-129">Cuadro de diálogo Nuevo proyecto, .NET CORE en el panel izquierdo, Aplicación web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="62e11-129">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="62e11-130">Visual Studio ha usado una plantilla predeterminada para el proyecto de MVC que acaba de crear.</span><span class="sxs-lookup"><span data-stu-id="62e11-130">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="62e11-131">Si escribe un nombre de proyecto y selecciona algunas opciones, dispondrá de inmediato de una aplicación operativa.</span><span class="sxs-lookup"><span data-stu-id="62e11-131">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="62e11-132">Se trata de un proyecto introductorio básico, pero es un buen punto de partida.</span><span class="sxs-lookup"><span data-stu-id="62e11-132">This is a basic starter project, and it's a good place to start.</span></span>

<span data-ttu-id="62e11-133">Presione **Ctrl-F5** para ejecutar la aplicación en modo de no depuración.</span><span class="sxs-lookup"><span data-stu-id="62e11-133">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

* <span data-ttu-id="62e11-134">Visual Studio inicia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) y ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="62e11-134">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="62e11-135">Tenga en cuenta que en la barra de direcciones aparece `localhost:port#` (y no algo como `example.com`).</span><span class="sxs-lookup"><span data-stu-id="62e11-135">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="62e11-136">Esto es así porque `localhost` es el nombre de host estándar del equipo local.</span><span class="sxs-lookup"><span data-stu-id="62e11-136">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="62e11-137">Cuando Visual Studio crea un proyecto web, se usa un puerto aleatorio para el servidor web.</span><span class="sxs-lookup"><span data-stu-id="62e11-137">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="62e11-138">En la imagen anterior, el número de puerto es el 5000.</span><span class="sxs-lookup"><span data-stu-id="62e11-138">In the image above, the port number is 5000.</span></span> <span data-ttu-id="62e11-139">La dirección URL del explorador es `localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="62e11-139">The URL in the browser shows `localhost:5000`.</span></span> <span data-ttu-id="62e11-140">Al ejecutar la aplicación verá otro puerto distinto.</span><span class="sxs-lookup"><span data-stu-id="62e11-140">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="62e11-141">Iniciar la aplicación con **CTRL+F5** (modo de no depuración) le permite efectuar cambios en el código, guardar el archivo, actualizar el explorador y ver los cambios de código.</span><span class="sxs-lookup"><span data-stu-id="62e11-141">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="62e11-142">Muchos desarrolladores prefieren usar el modo de no depuración para iniciar la aplicación rápidamente y ver los cambios.</span><span class="sxs-lookup"><span data-stu-id="62e11-142">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="62e11-143">Puede iniciar la aplicación en modo de depuración o en modo de no depuración desde el elemento de menú **Depurar**:</span><span class="sxs-lookup"><span data-stu-id="62e11-143">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![Menú Depurar](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="62e11-145">Puede depurar la aplicación seleccionando el botón **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="62e11-145">You can debug the app by selecting the **IIS Express** button</span></span>

![IIS Express](start-mvc/_static/iis_express.png)

<!-- Code -------------------------->
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="62e11-147">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="62e11-147">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="62e11-148">Para realizar el tutorial debe estar familiarizado con VS Code.</span><span class="sxs-lookup"><span data-stu-id="62e11-148">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="62e11-149">Para más información, vea [Getting started with VS Code](https://code.visualstudio.com/docs) (Introducción a VS Code) y [Visual Studio Code help](#visual-studio-code-help) (Ayuda de Visual Studio Code).</span><span class="sxs-lookup"><span data-stu-id="62e11-149">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="62e11-150">Abra el [terminal integrado](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="62e11-150">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="62e11-151">Cambie los directorios (`cd`) a una carpeta que contenga el proyecto.</span><span class="sxs-lookup"><span data-stu-id="62e11-151">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="62e11-152">Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="62e11-152">Run the following command:</span></span>

   ```console
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="62e11-153">Se muestra un cuadro de diálogo con el texto **Required assets to build and debug are missing from 'MvcMovie'. Add them?** (Faltan los activos necesarios para compilar y depurar en "RazorPagesMovie". ¿Desea agregarlos?).</span><span class="sxs-lookup"><span data-stu-id="62e11-153">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="62e11-154">Seleccione **Sí**.</span><span class="sxs-lookup"><span data-stu-id="62e11-154">Select **Yes**</span></span>

  * <span data-ttu-id="62e11-155">`dotnet new mvc -o MvcMovie`: crea un nuevo proyecto de ASP.NET Core MVC en la carpeta *MvcMovie*.</span><span class="sxs-lookup"><span data-stu-id="62e11-155">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="62e11-156">`code -r MvcMovie`: carga el archivo de proyecto *MvcMovie.csproj* en Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="62e11-156">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="62e11-157">Iniciar la aplicación</span><span class="sxs-lookup"><span data-stu-id="62e11-157">Launch the app</span></span>

* <span data-ttu-id="62e11-158">Presione **Ctrl+F5** para ejecutar sin el depurador.</span><span class="sxs-lookup"><span data-stu-id="62e11-158">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="62e11-159">Visual Studio Code inicia [Kestrel](xref:fundamentals/servers/kestrel) y un explorador, y se desplaza hasta `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="62e11-159">Visual Studio Code starts starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="62e11-160">En la barra de direcciones aparece `localhost:port:5001` (y no algo como `example.com`).</span><span class="sxs-lookup"><span data-stu-id="62e11-160">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="62e11-161">Esto es así porque `localhost` es el nombre de host estándar del equipo local.</span><span class="sxs-lookup"><span data-stu-id="62e11-161">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="62e11-162">Localhost solo sirve las solicitudes web del equipo local.</span><span class="sxs-lookup"><span data-stu-id="62e11-162">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="62e11-163">Iniciar la aplicación con **CTRL+F5** (modo de no depuración) le permite efectuar cambios en el código, guardar el archivo, actualizar el explorador y ver los cambios de código.</span><span class="sxs-lookup"><span data-stu-id="62e11-163">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="62e11-164">Muchos desarrolladores prefieren usar el modo de no depuración para actualizar la página y ver los cambios.</span><span class="sxs-lookup"><span data-stu-id="62e11-164">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="62e11-165">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="62e11-165">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="62e11-166">Seleccione **Archivo** > **Nueva solución**.</span><span class="sxs-lookup"><span data-stu-id="62e11-166">Select **File** > **New Solution**.</span></span>

  ![macOS: Nueva solución](~/tutorials/first-web-api-mac/_static/sln.png)

* <span data-ttu-id="62e11-168">Seleccione **Aplicación .NET Core** > **ASP.NET Core** > **Aplicación web ASP.NET Core (MVC)** > **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="62e11-168">Select **.NET Core App** > **ASP.NET Core** > **ASP.NET Core Web App (MVC)** > **Next**.</span></span>

  ![Cuadro de diálogo de nuevo proyecto de macOS](~/tutorials/first-mvc-app-mac/start-mvc/1.png)

* <span data-ttu-id="62e11-170">En el cuadro de diálogo **Configurar la nueva API web de ASP.NET Core**, acepte la **plataforma de destino** predeterminada de \**.NET Core 2.2*.</span><span class="sxs-lookup"><span data-stu-id="62e11-170">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of \**.NET Core 2.2*.</span></span>

* <span data-ttu-id="62e11-171">Asigne el nombre **MvcMovie** al proyecto y, después, seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="62e11-171">Name the project **MvcMovie**, and then select **Create**.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="62e11-172">Iniciar la aplicación</span><span class="sxs-lookup"><span data-stu-id="62e11-172">Launch the app</span></span>

<span data-ttu-id="62e11-173">Seleccione **Ejecutar** > **Iniciar sin depurar** para iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="62e11-173">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="62e11-174">Visual Studio para Mac inicia el servidor [Kestrel](xref:fundamentals/servers/index#kestrel), inicia un explorador y navega a `http://localhost:port`, donde *port* es un número de puerto elegido aleatoriamente.</span><span class="sxs-lookup"><span data-stu-id="62e11-174">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

* <span data-ttu-id="62e11-175">En la barra de direcciones aparece `localhost:port#` (y no algo como `example.com`).</span><span class="sxs-lookup"><span data-stu-id="62e11-175">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="62e11-176">Esto es así porque `localhost` es el nombre de host estándar del equipo local.</span><span class="sxs-lookup"><span data-stu-id="62e11-176">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="62e11-177">Cuando Visual Studio crea un proyecto web, se usa un puerto aleatorio para el servidor web.</span><span class="sxs-lookup"><span data-stu-id="62e11-177">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="62e11-178">Al ejecutar la aplicación verá otro puerto distinto.</span><span class="sxs-lookup"><span data-stu-id="62e11-178">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="62e11-179">Puede iniciar la aplicación en modo de depuración o en modo de no depuración desde el menú **Ejecutar**.</span><span class="sxs-lookup"><span data-stu-id="62e11-179">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

---  
<!-- End of VS tabs -->

* <span data-ttu-id="62e11-180">Seleccione **Aceptar** para dar su consentimiento al seguimiento.</span><span class="sxs-lookup"><span data-stu-id="62e11-180">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="62e11-181">Esta aplicación no lleva un seguimiento de la información personal.</span><span class="sxs-lookup"><span data-stu-id="62e11-181">This app doesn't track personal information.</span></span> <span data-ttu-id="62e11-182">El código generado con plantilla incluye activos que sirven para cumplir el [Reglamento general de protección de datos (RGPD)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="62e11-182">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Página Inicio o Índice](start-mvc/_static/privacy.png)

  <span data-ttu-id="62e11-184">En la siguiente imagen se muestra la aplicación tras haber aceptado el seguimiento:</span><span class="sxs-lookup"><span data-stu-id="62e11-184">The following image shows the app after accepting tracking:</span></span>

  ![Página Inicio o Índice](start-mvc/_static/home2.2.png)

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="62e11-186">En la siguiente sección de este tutorial conocerá MVC y empezará a escribir código.</span><span class="sxs-lookup"><span data-stu-id="62e11-186">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="62e11-187">Siguiente</span><span class="sxs-lookup"><span data-stu-id="62e11-187">Next</span></span>](adding-controller.md)  
