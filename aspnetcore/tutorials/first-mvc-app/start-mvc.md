---
title: Introducción a ASP.NET Core MVC
author: rick-anderson
description: Obtenga información sobre cómo empezar a usar ASP.NET Core MVC.
ms.author: riande
ms.date: 04/24/2019
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 91564bac02df77632a3b56b6dbddeb3b622f6438
ms.sourcegitcommit: d6e51c60439f03a8992bda70cc982ddb15d3f100
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2019
ms.locfileid: "67555852"
---
# <a name="get-started-with-aspnet-core-mvc"></a><span data-ttu-id="a84eb-103">Introducción a ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="a84eb-103">Get started with ASP.NET Core MVC</span></span>

<span data-ttu-id="a84eb-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a84eb-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="a84eb-105">En este tutorial se enseñan los conceptos básicos de la compilación de una aplicación web ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="a84eb-105">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="a84eb-106">La aplicación administra una base de datos de títulos de películas.</span><span class="sxs-lookup"><span data-stu-id="a84eb-106">The app manages a database of movie titles.</span></span> <span data-ttu-id="a84eb-107">Aprenderá a:</span><span class="sxs-lookup"><span data-stu-id="a84eb-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a84eb-108">Crear una aplicación web.</span><span class="sxs-lookup"><span data-stu-id="a84eb-108">Create a web app.</span></span>
> * <span data-ttu-id="a84eb-109">Agregar un modelo y aplicarle scaffolding.</span><span class="sxs-lookup"><span data-stu-id="a84eb-109">Add and scaffold a model.</span></span>
> * <span data-ttu-id="a84eb-110">Trabajar con una base de datos.</span><span class="sxs-lookup"><span data-stu-id="a84eb-110">Work with a database.</span></span>
> * <span data-ttu-id="a84eb-111">Agregar búsqueda y validación.</span><span class="sxs-lookup"><span data-stu-id="a84eb-111">Add search and validation.</span></span>

<span data-ttu-id="a84eb-112">Al final, tendrá una aplicación que le permitirá administrar y mostrar datos de películas.</span><span class="sxs-lookup"><span data-stu-id="a84eb-112">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

## <a name="prerequisites"></a><span data-ttu-id="a84eb-113">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="a84eb-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a84eb-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a84eb-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a84eb-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a84eb-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a84eb-116">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="a84eb-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---
## <a name="create-a-web-app"></a><span data-ttu-id="a84eb-117">Creación de una aplicación web</span><span class="sxs-lookup"><span data-stu-id="a84eb-117">Create a web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a84eb-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a84eb-118">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a84eb-119">En Visual Studio, seleccione **Crear un proyecto**.</span><span class="sxs-lookup"><span data-stu-id="a84eb-119">From the Visual Studio select **Create a new project**.</span></span>

* <span data-ttu-id="a84eb-120">Seleccione **Aplicación web de ASP.NET Core** y, luego, **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="a84eb-120">Selecct **ASP.NET Core Web Application** and then select **Next**.</span></span>

![Nueva aplicación web de ASP.NET Core](start-mvc/_static/np_2.1.png)

* <span data-ttu-id="a84eb-122">Asigne el nombre **MvcMovie** al proyecto y seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="a84eb-122">Name the project **MvcMovie** and select **Create**.</span></span> <span data-ttu-id="a84eb-123">Es importante que el proyecto se llame **MvcMovie** para que, al copiar el código, coincida con el espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="a84eb-123">It's important to name the project **MvcMovie** so when you copy code, the namespace will match.</span></span>

  ![Nueva aplicación web de ASP.NET Core](start-mvc/_static/config.png)


* <span data-ttu-id="a84eb-125">Seleccione **Aplicación web (Modelo-Vista-Controlador)** y, luego, **Crear**.</span><span class="sxs-lookup"><span data-stu-id="a84eb-125">Select **Web Application(Model-View-Controller)**, and then select **Create**.</span></span>

![<span data-ttu-id="a84eb-126">Cuadro de diálogo Nuevo proyecto, .NET CORE en el panel izquierdo, Aplicación web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a84eb-126">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="a84eb-127">Visual Studio ha usado la plantilla predeterminada para el proyecto de MVC que acaba de crear.</span><span class="sxs-lookup"><span data-stu-id="a84eb-127">Visual Studio used the default template for the MVC project you just created.</span></span> <span data-ttu-id="a84eb-128">Si escribe un nombre de proyecto y selecciona algunas opciones, dispondrá de inmediato de una aplicación operativa.</span><span class="sxs-lookup"><span data-stu-id="a84eb-128">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="a84eb-129">Se trata de un proyecto introductorio básico, pero es un buen punto de partida.</span><span class="sxs-lookup"><span data-stu-id="a84eb-129">This is a basic starter project, and it's a good place to start.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a84eb-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a84eb-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="a84eb-131">Para realizar el tutorial debe estar familiarizado con VS Code.</span><span class="sxs-lookup"><span data-stu-id="a84eb-131">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="a84eb-132">Para más información, vea [Getting started with VS Code](https://code.visualstudio.com/docs) (Introducción a VS Code) y [Visual Studio Code help](#visual-studio-code-help) (Ayuda de Visual Studio Code).</span><span class="sxs-lookup"><span data-stu-id="a84eb-132">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="a84eb-133">Abra el [terminal integrado](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="a84eb-133">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="a84eb-134">Cambie los directorios (`cd`) a una carpeta que contenga el proyecto.</span><span class="sxs-lookup"><span data-stu-id="a84eb-134">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="a84eb-135">Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="a84eb-135">Run the following command:</span></span>

   ```console
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="a84eb-136">Se muestra un cuadro de diálogo con el texto **Required assets to build and debug are missing from 'MvcMovie'. Add them?** (Faltan los activos necesarios para compilar y depurar en "RazorPagesMovie". ¿Desea agregarlos?).</span><span class="sxs-lookup"><span data-stu-id="a84eb-136">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="a84eb-137">Seleccione **Sí**.</span><span class="sxs-lookup"><span data-stu-id="a84eb-137">Select **Yes**</span></span>

  * <span data-ttu-id="a84eb-138">`dotnet new mvc -o MvcMovie`: crea un nuevo proyecto de ASP.NET Core MVC en la carpeta *MvcMovie*.</span><span class="sxs-lookup"><span data-stu-id="a84eb-138">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="a84eb-139">`code -r MvcMovie`: carga el archivo de proyecto *MvcMovie.csproj* en Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="a84eb-139">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a84eb-140">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="a84eb-140">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="a84eb-141">Seleccione **Archivo** > **Nueva solución**.</span><span class="sxs-lookup"><span data-stu-id="a84eb-141">Select **File** > **New Solution**.</span></span>

  ![macOS: Nueva solución](./start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="a84eb-143">Seleccione **.NET Core** > **Aplicación** > **Aplicación web (Modelo-Vista-Controlador)**  > **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="a84eb-143">Select **.NET Core** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span>

  ![Cuadro de diálogo de nuevo proyecto de macOS](./start-mvc/_static/new_project_mvc_vsmac.png)

* <span data-ttu-id="a84eb-145">En el cuadro de diálogo **Configurar la nueva API web de ASP.NET Core**, acepte el **Marco de trabajo de destino** predeterminado de **.NET Core 2.2**.</span><span class="sxs-lookup"><span data-stu-id="a84eb-145">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of **.NET Core 2.2**.</span></span>

  ![Selección de .NET Core 2.2 de macOS](./start-mvc/_static/new_project_22_vsmac.png)

* <span data-ttu-id="a84eb-147">Asigne el nombre **MvcMovie** al proyecto y, después, seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="a84eb-147">Name the project **MvcMovie**, and then select **Create**.</span></span>

---

### <a name="run-the-app"></a><span data-ttu-id="a84eb-148">Ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="a84eb-148">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a84eb-149">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a84eb-149">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a84eb-150">Presione **Ctrl-F5** para ejecutar la aplicación en modo de no depuración.</span><span class="sxs-lookup"><span data-stu-id="a84eb-150">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="a84eb-151">Visual Studio inicia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) y ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a84eb-151">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="a84eb-152">Tenga en cuenta que en la barra de direcciones aparece `localhost:port#` (y no algo como `example.com`).</span><span class="sxs-lookup"><span data-stu-id="a84eb-152">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="a84eb-153">Esto es así porque `localhost` es el nombre de host estándar del equipo local.</span><span class="sxs-lookup"><span data-stu-id="a84eb-153">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="a84eb-154">Cuando Visual Studio crea un proyecto web, se usa un puerto aleatorio para el servidor web.</span><span class="sxs-lookup"><span data-stu-id="a84eb-154">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="a84eb-155">El inicio de la aplicación con Ctrl+F5 (modo de no depuración) permite realizar cambios en el código, guardar el archivo, actualizar el explorador y ver los cambios de código.</span><span class="sxs-lookup"><span data-stu-id="a84eb-155">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="a84eb-156">Muchos desarrolladores prefieren usar el modo de no depuración para iniciar la aplicación rápidamente y ver los cambios.</span><span class="sxs-lookup"><span data-stu-id="a84eb-156">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="a84eb-157">Puede iniciar la aplicación en modo de depuración o en modo de no depuración desde el elemento de menú **Depurar**:</span><span class="sxs-lookup"><span data-stu-id="a84eb-157">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![Menú Depurar](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="a84eb-159">Puede depurar la aplicación seleccionando el botón **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="a84eb-159">You can debug the app by selecting the **IIS Express** button</span></span>

  ![IIS Express](start-mvc/_static/iis_express.png)

* <span data-ttu-id="a84eb-161">Seleccione **Aceptar** para dar su consentimiento al seguimiento.</span><span class="sxs-lookup"><span data-stu-id="a84eb-161">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="a84eb-162">Esta aplicación no lleva un seguimiento de la información personal.</span><span class="sxs-lookup"><span data-stu-id="a84eb-162">This app doesn't track personal information.</span></span> <span data-ttu-id="a84eb-163">El código generado con plantilla incluye activos que sirven para cumplir el [Reglamento general de protección de datos (RGPD)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="a84eb-163">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Página Inicio o Índice](start-mvc/_static/privacy.png)

  <span data-ttu-id="a84eb-165">En la siguiente imagen se muestra la aplicación tras haber aceptado el seguimiento:</span><span class="sxs-lookup"><span data-stu-id="a84eb-165">The following image shows the app after accepting tracking:</span></span>

  ![Página Inicio o Índice](start-mvc/_static/home2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a84eb-167">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a84eb-167">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="a84eb-168">Presione Ctrl+F5 para ejecutarla sin el depurador.</span><span class="sxs-lookup"><span data-stu-id="a84eb-168">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="a84eb-169">Visual Studio Code inicia [Kestrel](xref:fundamentals/servers/kestrel) y un explorador, y se desplaza hasta `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="a84eb-169">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="a84eb-170">En la barra de direcciones aparece `localhost:port:5001` (y no algo como `example.com`).</span><span class="sxs-lookup"><span data-stu-id="a84eb-170">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="a84eb-171">Esto es así porque `localhost` es el nombre de host estándar del equipo local.</span><span class="sxs-lookup"><span data-stu-id="a84eb-171">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="a84eb-172">Localhost solo sirve las solicitudes web del equipo local.</span><span class="sxs-lookup"><span data-stu-id="a84eb-172">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="a84eb-173">El inicio de la aplicación con Ctrl+F5 (modo de no depuración) permite realizar cambios en el código, guardar el archivo, actualizar el explorador y ver los cambios de código.</span><span class="sxs-lookup"><span data-stu-id="a84eb-173">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="a84eb-174">Muchos desarrolladores prefieren usar el modo de no depuración para actualizar la página y ver los cambios.</span><span class="sxs-lookup"><span data-stu-id="a84eb-174">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

* <span data-ttu-id="a84eb-175">Seleccione **Aceptar** para dar su consentimiento al seguimiento.</span><span class="sxs-lookup"><span data-stu-id="a84eb-175">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="a84eb-176">Esta aplicación no lleva un seguimiento de la información personal.</span><span class="sxs-lookup"><span data-stu-id="a84eb-176">This app doesn't track personal information.</span></span> <span data-ttu-id="a84eb-177">El código generado con plantilla incluye activos que sirven para cumplir el [Reglamento general de protección de datos (RGPD)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="a84eb-177">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Página Inicio o Índice](start-mvc/_static/privacy.png)

  <span data-ttu-id="a84eb-179">En la siguiente imagen se muestra la aplicación tras haber aceptado el seguimiento:</span><span class="sxs-lookup"><span data-stu-id="a84eb-179">The following image shows the app after accepting tracking:</span></span>

  ![Página Inicio o Índice](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a84eb-181">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="a84eb-181">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="a84eb-182">Seleccione **Ejecutar** > **Iniciar sin depurar** para iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a84eb-182">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="a84eb-183">Visual Studio para Mac inicia el servidor [Kestrel](xref:fundamentals/servers/index#kestrel), inicia un explorador y navega a `http://localhost:port`, donde *port* es un número de puerto elegido aleatoriamente.</span><span class="sxs-lookup"><span data-stu-id="a84eb-183">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="a84eb-184">En la barra de direcciones aparece `localhost:port#` (y no algo como `example.com`).</span><span class="sxs-lookup"><span data-stu-id="a84eb-184">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="a84eb-185">Esto es así porque `localhost` es el nombre de host estándar del equipo local.</span><span class="sxs-lookup"><span data-stu-id="a84eb-185">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="a84eb-186">Cuando Visual Studio crea un proyecto web, se usa un puerto aleatorio para el servidor web.</span><span class="sxs-lookup"><span data-stu-id="a84eb-186">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="a84eb-187">Al ejecutar la aplicación verá otro puerto distinto.</span><span class="sxs-lookup"><span data-stu-id="a84eb-187">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="a84eb-188">Puede iniciar la aplicación en modo de depuración o en modo de no depuración desde el menú **Ejecutar**.</span><span class="sxs-lookup"><span data-stu-id="a84eb-188">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

* <span data-ttu-id="a84eb-189">Seleccione **Aceptar** para dar su consentimiento al seguimiento.</span><span class="sxs-lookup"><span data-stu-id="a84eb-189">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="a84eb-190">Esta aplicación no lleva un seguimiento de la información personal.</span><span class="sxs-lookup"><span data-stu-id="a84eb-190">This app doesn't track personal information.</span></span> <span data-ttu-id="a84eb-191">El código generado con plantilla incluye activos que sirven para cumplir el [Reglamento general de protección de datos (RGPD)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="a84eb-191">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Página Inicio o Índice](./start-mvc/_static/output_privacy_macos.png)

  <span data-ttu-id="a84eb-193">En la siguiente imagen se muestra la aplicación tras haber aceptado el seguimiento:</span><span class="sxs-lookup"><span data-stu-id="a84eb-193">The following image shows the app after accepting tracking:</span></span>

  ![Página Inicio o Índice](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="a84eb-195">En la siguiente sección de este tutorial conocerá MVC y empezará a escribir código.</span><span class="sxs-lookup"><span data-stu-id="a84eb-195">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="a84eb-196">Siguiente</span><span class="sxs-lookup"><span data-stu-id="a84eb-196">Next</span></span>](adding-controller.md)
