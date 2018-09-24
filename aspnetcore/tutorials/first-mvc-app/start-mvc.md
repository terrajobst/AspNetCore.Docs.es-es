---
title: Introducción a ASP.NET Core MVC y Visual Studio
author: rick-anderson
description: Obtenga información sobre cómo empezar a usar ASP.NET Core MVC y Visual Studio.
ms.author: riande
ms.date: 10/07/2017
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 41f986a06ec46dc025c4e8218745b4a513e8ee2a
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011711"
---
# <a name="get-started-with-aspnet-core-mvc-and-visual-studio"></a><span data-ttu-id="c5628-103">Introducción a ASP.NET Core MVC y Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c5628-103">Get started with ASP.NET Core MVC and Visual Studio</span></span>

<span data-ttu-id="c5628-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c5628-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="c5628-105">Hay tres versiones de este tutorial:</span><span class="sxs-lookup"><span data-stu-id="c5628-105">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="c5628-106">macOS: [Creación de una aplicación de ASP.NET Core MVC con Visual Studio para Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="c5628-106">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="c5628-107">Windows: [Creación de una aplicación de ASP.NET Core MVC con Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="c5628-107">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="c5628-108">macOS, Linux y Windows: [Creación de una aplicación de ASP.NET Core MVC con Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="c5628-108">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

## <a name="install-visual-studio-and-net-core"></a><span data-ttu-id="c5628-109">Instalar Visual Studio y .NET Core</span><span class="sxs-lookup"><span data-stu-id="c5628-109">Install Visual Studio and .NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-web-app"></a><span data-ttu-id="c5628-110">Creación de una aplicación web</span><span class="sxs-lookup"><span data-stu-id="c5628-110">Create a web app</span></span>

<span data-ttu-id="c5628-111">En Visual Studio, seleccione **Archivo > Nuevo > Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="c5628-111">From Visual Studio, select  **File > New > Project**.</span></span>

![Archivo > Nuevo > Proyecto](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="c5628-113">Complete el cuadro de diálogo **Nuevo proyecto**:</span><span class="sxs-lookup"><span data-stu-id="c5628-113">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="c5628-114">En el panel izquierdo, pulse **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="c5628-114">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="c5628-115">En el panel central, pulse **Aplicación web ASP.NET Core (.NET Core)**.</span><span class="sxs-lookup"><span data-stu-id="c5628-115">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="c5628-116">Asigne al proyecto el nombre "MvcMovie" (es importante asignarle este nombre para que, al copiar el código, coincida con el espacio de nombres).</span><span class="sxs-lookup"><span data-stu-id="c5628-116">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="c5628-117">Pulse **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="c5628-117">Tap **OK**</span></span>

![<span data-ttu-id="c5628-118">Cuadro de diálogo Nuevo proyecto, .NET CORE en el panel izquierdo, Aplicación web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c5628-118">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2-21.png)

<span data-ttu-id="c5628-119">Complete el cuadro de diálogo **Nueva aplicación web ASP.NET Core (.NET Core) - MvcMovie**:</span><span class="sxs-lookup"><span data-stu-id="c5628-119">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="c5628-120">En el cuadro desplegable del selector de versión, seleccione **ASP.NET Core 2.1**.</span><span class="sxs-lookup"><span data-stu-id="c5628-120">In the version selector drop-down box select **ASP.NET Core 2.1**</span></span>
* <span data-ttu-id="c5628-121">Seleccione **Aplicación web (controlador de vista de modelos)**.</span><span class="sxs-lookup"><span data-stu-id="c5628-121">Select **Web Application(Model-View-Controller)**</span></span>
* <span data-ttu-id="c5628-122">Pulse **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="c5628-122">Tap **OK**.</span></span>

![<span data-ttu-id="c5628-123">Cuadro de diálogo Nuevo proyecto, .NET CORE en el panel izquierdo, Aplicación web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c5628-123">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="c5628-124">Visual Studio ha usado una plantilla predeterminada para el proyecto de MVC que acaba de crear.</span><span class="sxs-lookup"><span data-stu-id="c5628-124">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="c5628-125">Si escribe un nombre de proyecto y selecciona algunas opciones, dispondrá de inmediato de una aplicación operativa.</span><span class="sxs-lookup"><span data-stu-id="c5628-125">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="c5628-126">Se trata de un proyecto introductorio básico, pero es un buen punto de partida.</span><span class="sxs-lookup"><span data-stu-id="c5628-126">This is a basic starter project, and it's a good place to start,</span></span>

<span data-ttu-id="c5628-127">Pulse **F5** para ejecutar la aplicación en modo de depuración o **Ctrl-F5** para ejecutarla en modo de no depuración.</span><span class="sxs-lookup"><span data-stu-id="c5628-127">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<span data-ttu-id="c5628-128"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![aplicación en ejecución](start-mvc/_static/1.png)</span><span class="sxs-lookup"><span data-stu-id="c5628-128"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![running app](start-mvc/_static/1.png)</span></span>

* <span data-ttu-id="c5628-129">Visual Studio inicia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) y ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c5628-129">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="c5628-130">Tenga en cuenta que en la barra de direcciones aparece `localhost:port#` (y no algo como `example.com`).</span><span class="sxs-lookup"><span data-stu-id="c5628-130">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="c5628-131">Esto es así porque `localhost` es el nombre de host estándar del equipo local.</span><span class="sxs-lookup"><span data-stu-id="c5628-131">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="c5628-132">Cuando Visual Studio crea un proyecto web, se usa un puerto aleatorio para el servidor web.</span><span class="sxs-lookup"><span data-stu-id="c5628-132">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="c5628-133">En la imagen anterior, el número de puerto es el 5000.</span><span class="sxs-lookup"><span data-stu-id="c5628-133">In the image above, the port number is 5000.</span></span> <span data-ttu-id="c5628-134">La dirección URL del explorador es `localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="c5628-134">The URL in the browser shows `localhost:5000`.</span></span> <span data-ttu-id="c5628-135">Al ejecutar la aplicación verá otro puerto distinto.</span><span class="sxs-lookup"><span data-stu-id="c5628-135">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="c5628-136">Iniciar la aplicación con **CTRL+F5** (modo de no depuración) le permite efectuar cambios en el código, guardar el archivo, actualizar el explorador y ver los cambios de código.</span><span class="sxs-lookup"><span data-stu-id="c5628-136">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="c5628-137">Muchos desarrolladores prefieren usar el modo de no depuración para iniciar la aplicación rápidamente y ver los cambios.</span><span class="sxs-lookup"><span data-stu-id="c5628-137">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="c5628-138">Puede iniciar la aplicación en modo de depuración o en modo de no depuración desde el elemento de menú **Depurar**:</span><span class="sxs-lookup"><span data-stu-id="c5628-138">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![Menú Depurar](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="c5628-140">Puede depurar la aplicación pulsando el botón **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="c5628-140">You can debug the app by tapping the **IIS Express** button</span></span>

![IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="c5628-142">La plantilla predeterminada proporciona los vínculos **Inicio, Acerca de** y **Contacto**, totalmente funcionales.</span><span class="sxs-lookup"><span data-stu-id="c5628-142">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="c5628-143">En la imagen del explorador anterior no se muestran estos vínculos.</span><span class="sxs-lookup"><span data-stu-id="c5628-143">The browser image above doesn't show these links.</span></span> <span data-ttu-id="c5628-144">Según el tamaño del explorador, tendrá que hacer clic en el icono de navegación para que se muestren.</span><span class="sxs-lookup"><span data-stu-id="c5628-144">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![icono de navegación en la esquina superior derecha](start-mvc/_static/2.png)

<span data-ttu-id="c5628-146">Si iba a efectuar una ejecución en modo de depuración, pulse **MAYÚS-F5** para detener la depuración.</span><span class="sxs-lookup"><span data-stu-id="c5628-146">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="c5628-147">En la siguiente sección de este tutorial obtendrá información sobre MVC y empezará a escribir código.</span><span class="sxs-lookup"><span data-stu-id="c5628-147">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c5628-148">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c5628-148">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!INCLUDE [](~/includes/net-core-prereqs.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c5628-149">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c5628-149">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="c5628-150">Instale Visual Studio Community 2017.</span><span class="sxs-lookup"><span data-stu-id="c5628-150">Install Visual Studio Community 2017.</span></span> <span data-ttu-id="c5628-151">Seleccione la descarga de Community.</span><span class="sxs-lookup"><span data-stu-id="c5628-151">Select the Community download.</span></span> <span data-ttu-id="c5628-152">Omita este paso si tiene instalado Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="c5628-152">Skip this step if you have Visual Studio 2017 installed.</span></span>

* [<span data-ttu-id="c5628-153">Página principal del instalador de Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="c5628-153">Visual Studio 2017 Home page installer</span></span>](https://www.visualstudio.com/)

<span data-ttu-id="c5628-154">Ejecute el instalador y seleccione las siguientes cargas de trabajo:</span><span class="sxs-lookup"><span data-stu-id="c5628-154">Run the installer and select the following workloads:</span></span>

* <span data-ttu-id="c5628-155">**Desarrollo de ASP.NET y web** (en **Web y nube**)</span><span class="sxs-lookup"><span data-stu-id="c5628-155">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
* <span data-ttu-id="c5628-156">**Desarrollo multiplataforma de .NET Core** (en **Otros conjuntos de herramientas**)</span><span class="sxs-lookup"><span data-stu-id="c5628-156">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>

![**Desarrollo de ASP.NET y web** (en **Web y nube**)](start-mvc/_static/web_workload.png)

![**Desarrollo multiplataforma de .NET Core** (en **Otros conjuntos de herramientas**)](start-mvc/_static/x_plat_wl.png)

---

## <a name="create-a-web-app"></a><span data-ttu-id="c5628-159">Crear una aplicación web</span><span class="sxs-lookup"><span data-stu-id="c5628-159">Create a web app</span></span>

<span data-ttu-id="c5628-160">En Visual Studio, seleccione **Archivo > Nuevo > Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="c5628-160">From Visual Studio, select  **File > New > Project**.</span></span>

![Archivo > Nuevo > Proyecto](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="c5628-162">Complete el cuadro de diálogo **Nuevo proyecto**:</span><span class="sxs-lookup"><span data-stu-id="c5628-162">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="c5628-163">En el panel izquierdo, pulse **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="c5628-163">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="c5628-164">En el panel central, pulse **Aplicación web ASP.NET Core (.NET Core)**.</span><span class="sxs-lookup"><span data-stu-id="c5628-164">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="c5628-165">Asigne al proyecto el nombre "MvcMovie" (es importante asignarle este nombre para que, al copiar el código, coincida con el espacio de nombres).</span><span class="sxs-lookup"><span data-stu-id="c5628-165">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="c5628-166">Pulse **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="c5628-166">Tap **OK**</span></span>

![<span data-ttu-id="c5628-167">Cuadro de diálogo Nuevo proyecto, .NET CORE en el panel izquierdo, Aplicación web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c5628-167">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c5628-168">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c5628-168">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="c5628-169">Complete el cuadro de diálogo **Nueva aplicación web ASP.NET Core (.NET Core) - MvcMovie**:</span><span class="sxs-lookup"><span data-stu-id="c5628-169">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="c5628-170">En el cuadro desplegable del selector de versión, seleccione **ASP.NET Core 2.-**.</span><span class="sxs-lookup"><span data-stu-id="c5628-170">In the version selector drop-down box select **ASP.NET Core 2.-**</span></span>
* <span data-ttu-id="c5628-171">Seleccione **Aplicación web (controlador de vista de modelos)**.</span><span class="sxs-lookup"><span data-stu-id="c5628-171">Select **Web Application(Model-View-Controller)**</span></span>
* <span data-ttu-id="c5628-172">Pulse **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="c5628-172">Tap **OK**.</span></span>

![<span data-ttu-id="c5628-173">Cuadro de diálogo Nuevo proyecto, .NET CORE en el panel izquierdo, Aplicación web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c5628-173">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c5628-174">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c5628-174">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="c5628-175">Complete el cuadro de diálogo **Nueva aplicación web ASP.NET Core (.NET Core) - MvcMovie**:</span><span class="sxs-lookup"><span data-stu-id="c5628-175">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="c5628-176">En el cuadro desplegable del selector de versión, pulse **ASP.NET Core 1.1**.</span><span class="sxs-lookup"><span data-stu-id="c5628-176">In the version selector drop-down box tap **ASP.NET Core 1.1**</span></span>
* <span data-ttu-id="c5628-177">Pulse **Aplicación web**.</span><span class="sxs-lookup"><span data-stu-id="c5628-177">Tap **Web Application**</span></span>
* <span data-ttu-id="c5628-178">Mantenga la opción predeterminado **Sin autenticación**.</span><span class="sxs-lookup"><span data-stu-id="c5628-178">Keep the default **No Authentication**</span></span>
* <span data-ttu-id="c5628-179">Pulse **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="c5628-179">Tap **OK**.</span></span>

![Nueva aplicación web de ASP.NET Core](start-mvc/_static/p3.png)

---

<span data-ttu-id="c5628-181">Visual Studio ha usado una plantilla predeterminada para el proyecto de MVC que acaba de crear.</span><span class="sxs-lookup"><span data-stu-id="c5628-181">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="c5628-182">Si escribe un nombre de proyecto y selecciona algunas opciones, dispondrá de inmediato de una aplicación operativa.</span><span class="sxs-lookup"><span data-stu-id="c5628-182">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="c5628-183">Se trata de un proyecto introductorio básico, pero es un buen punto de partida.</span><span class="sxs-lookup"><span data-stu-id="c5628-183">This is a basic starter project, and it's a good place to start,</span></span>

<span data-ttu-id="c5628-184">Pulse **F5** para ejecutar la aplicación en modo de depuración o **Ctrl-F5** para ejecutarla en modo de no depuración.</span><span class="sxs-lookup"><span data-stu-id="c5628-184">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<span data-ttu-id="c5628-185"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![aplicación en ejecución](start-mvc/_static/1.png)</span><span class="sxs-lookup"><span data-stu-id="c5628-185"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![running app](start-mvc/_static/1.png)</span></span>

* <span data-ttu-id="c5628-186">Visual Studio inicia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) y ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c5628-186">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="c5628-187">Tenga en cuenta que en la barra de direcciones aparece `localhost:port#` (y no algo como `example.com`).</span><span class="sxs-lookup"><span data-stu-id="c5628-187">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="c5628-188">Esto es así porque `localhost` es el nombre de host estándar del equipo local.</span><span class="sxs-lookup"><span data-stu-id="c5628-188">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="c5628-189">Cuando Visual Studio crea un proyecto web, se usa un puerto aleatorio para el servidor web.</span><span class="sxs-lookup"><span data-stu-id="c5628-189">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="c5628-190">En la imagen anterior, el número de puerto es el 5000.</span><span class="sxs-lookup"><span data-stu-id="c5628-190">In the image above, the port number is 5000.</span></span> <span data-ttu-id="c5628-191">La dirección URL del explorador es `localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="c5628-191">The URL in the browser shows `localhost:5000`.</span></span> <span data-ttu-id="c5628-192">Al ejecutar la aplicación verá otro puerto distinto.</span><span class="sxs-lookup"><span data-stu-id="c5628-192">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="c5628-193">Iniciar la aplicación con **CTRL+F5** (modo de no depuración) le permite efectuar cambios en el código, guardar el archivo, actualizar el explorador y ver los cambios de código.</span><span class="sxs-lookup"><span data-stu-id="c5628-193">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="c5628-194">Muchos desarrolladores prefieren usar el modo de no depuración para iniciar la aplicación rápidamente y ver los cambios.</span><span class="sxs-lookup"><span data-stu-id="c5628-194">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="c5628-195">Puede iniciar la aplicación en modo de depuración o en modo de no depuración desde el elemento de menú **Depurar**:</span><span class="sxs-lookup"><span data-stu-id="c5628-195">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![Menú Depurar](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="c5628-197">Puede depurar la aplicación pulsando el botón **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="c5628-197">You can debug the app by tapping the **IIS Express** button</span></span>

![IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="c5628-199">La plantilla predeterminada proporciona los vínculos **Inicio, Acerca de** y **Contacto**, totalmente funcionales.</span><span class="sxs-lookup"><span data-stu-id="c5628-199">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="c5628-200">En la imagen del explorador anterior no se muestran estos vínculos.</span><span class="sxs-lookup"><span data-stu-id="c5628-200">The browser image above doesn't show these links.</span></span> <span data-ttu-id="c5628-201">Según el tamaño del explorador, tendrá que hacer clic en el icono de navegación para que se muestren.</span><span class="sxs-lookup"><span data-stu-id="c5628-201">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![icono de navegación en la esquina superior derecha](start-mvc/_static/2.png)

<span data-ttu-id="c5628-203">Si iba a efectuar una ejecución en modo de depuración, pulse **MAYÚS-F5** para detener la depuración.</span><span class="sxs-lookup"><span data-stu-id="c5628-203">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="c5628-204">En la siguiente sección de este tutorial obtendrá información sobre MVC y empezará a escribir código.</span><span class="sxs-lookup"><span data-stu-id="c5628-204">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

::: moniker-end

> [!div class="step-by-step"]
> [<span data-ttu-id="c5628-205">Siguiente</span><span class="sxs-lookup"><span data-stu-id="c5628-205">Next</span></span>](adding-controller.md)  
