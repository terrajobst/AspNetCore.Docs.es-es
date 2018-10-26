---
title: Introducción a ASP.NET Core MVC y Visual Studio
author: rick-anderson
description: Obtenga información sobre cómo empezar a usar ASP.NET Core MVC y Visual Studio.
ms.author: riande
ms.date: 10/07/2017
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: fe555e4cfcaec5d4bb8ccee00b06d1bbcaae9dcd
ms.sourcegitcommit: f43f430a166a7ec137fcad12ded0372747227498
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2018
ms.locfileid: "49391211"
---
# <a name="get-started-with-aspnet-core-mvc-and-visual-studio"></a><span data-ttu-id="e7d2f-103">Introducción a ASP.NET Core MVC y Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e7d2f-103">Get started with ASP.NET Core MVC and Visual Studio</span></span>

<span data-ttu-id="e7d2f-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e7d2f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="e7d2f-105">Hay tres versiones de este tutorial:</span><span class="sxs-lookup"><span data-stu-id="e7d2f-105">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="e7d2f-106">macOS: [Creación de una aplicación de ASP.NET Core MVC con Visual Studio para Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="e7d2f-106">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="e7d2f-107">Windows: [Creación de una aplicación de ASP.NET Core MVC con Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="e7d2f-107">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="e7d2f-108">macOS, Linux y Windows: [Creación de una aplicación de ASP.NET Core MVC con Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="e7d2f-108">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

## <a name="install-visual-studio-and-net-core"></a><span data-ttu-id="e7d2f-109">Instalar Visual Studio y .NET Core</span><span class="sxs-lookup"><span data-stu-id="e7d2f-109">Install Visual Studio and .NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-web-app"></a><span data-ttu-id="e7d2f-110">Creación de una aplicación web</span><span class="sxs-lookup"><span data-stu-id="e7d2f-110">Create a web app</span></span>

<span data-ttu-id="e7d2f-111">En Visual Studio, seleccione **Archivo > Nuevo > Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-111">From Visual Studio, select  **File > New > Project**.</span></span>

![Archivo > Nuevo > Proyecto](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="e7d2f-113">Complete el cuadro de diálogo **Nuevo proyecto**:</span><span class="sxs-lookup"><span data-stu-id="e7d2f-113">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="e7d2f-114">En el panel izquierdo, pulse **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-114">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="e7d2f-115">En el panel central, pulse **Aplicación web ASP.NET Core (.NET Core)**.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-115">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="e7d2f-116">Asigne al proyecto el nombre "MvcMovie" (es importante asignarle este nombre para que, al copiar el código, coincida con el espacio de nombres).</span><span class="sxs-lookup"><span data-stu-id="e7d2f-116">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="e7d2f-117">Pulse **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-117">Tap **OK**</span></span>

![<span data-ttu-id="e7d2f-118">Cuadro de diálogo Nuevo proyecto, .NET CORE en el panel izquierdo, Aplicación web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e7d2f-118">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2-21.png)

<span data-ttu-id="e7d2f-119">Complete el cuadro de diálogo **Nueva aplicación web ASP.NET Core (.NET Core) - MvcMovie**:</span><span class="sxs-lookup"><span data-stu-id="e7d2f-119">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="e7d2f-120">En el cuadro desplegable del selector de versión, seleccione **ASP.NET Core 2.1**.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-120">In the version selector drop-down box select **ASP.NET Core 2.1**</span></span>
* <span data-ttu-id="e7d2f-121">Seleccione **Aplicación web (Modelo-Vista-Controlador)**.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-121">Select **Web Application (Model-View-Controller)**</span></span>
* <span data-ttu-id="e7d2f-122">Pulse **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-122">Tap **OK**.</span></span>

![<span data-ttu-id="e7d2f-123">Cuadro de diálogo Nuevo proyecto, .NET CORE en el panel izquierdo, Aplicación web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e7d2f-123">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="e7d2f-124">Visual Studio ha usado una plantilla predeterminada para el proyecto de MVC que acaba de crear.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-124">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="e7d2f-125">Si escribe un nombre de proyecto y selecciona algunas opciones, dispondrá de inmediato de una aplicación operativa.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-125">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="e7d2f-126">Se trata de un proyecto introductorio básico, pero es un buen punto de partida.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-126">This is a basic starter project, and it's a good place to start.</span></span>

<span data-ttu-id="e7d2f-127">Pulse **F5** para ejecutar la aplicación en modo de depuración o **Ctrl-F5** para ejecutarla en modo de no depuración.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-127">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<span data-ttu-id="e7d2f-128"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![aplicación en ejecución](start-mvc/_static/1.png)</span><span class="sxs-lookup"><span data-stu-id="e7d2f-128"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![running app](start-mvc/_static/1.png)</span></span>

* <span data-ttu-id="e7d2f-129">Visual Studio inicia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) y ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-129">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="e7d2f-130">Tenga en cuenta que en la barra de direcciones aparece `localhost:port#` (y no algo como `example.com`).</span><span class="sxs-lookup"><span data-stu-id="e7d2f-130">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="e7d2f-131">Esto es así porque `localhost` es el nombre de host estándar del equipo local.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-131">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="e7d2f-132">Cuando Visual Studio crea un proyecto web, se usa un puerto aleatorio para el servidor web.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-132">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="e7d2f-133">En la imagen anterior, el número de puerto es el 5000.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-133">In the image above, the port number is 5000.</span></span> <span data-ttu-id="e7d2f-134">La dirección URL del explorador es `localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-134">The URL in the browser shows `localhost:5000`.</span></span> <span data-ttu-id="e7d2f-135">Al ejecutar la aplicación verá otro puerto distinto.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-135">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="e7d2f-136">Iniciar la aplicación con **CTRL+F5** (modo de no depuración) le permite efectuar cambios en el código, guardar el archivo, actualizar el explorador y ver los cambios de código.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-136">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="e7d2f-137">Muchos desarrolladores prefieren usar el modo de no depuración para iniciar la aplicación rápidamente y ver los cambios.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-137">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="e7d2f-138">Puede iniciar la aplicación en modo de depuración o en modo de no depuración desde el elemento de menú **Depurar**:</span><span class="sxs-lookup"><span data-stu-id="e7d2f-138">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![Menú Depurar](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="e7d2f-140">Puede depurar la aplicación pulsando el botón **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-140">You can debug the app by tapping the **IIS Express** button</span></span>

![IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="e7d2f-142">La plantilla predeterminada proporciona los vínculos **Inicio, Acerca de** y **Contacto**, totalmente funcionales.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-142">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="e7d2f-143">En la imagen del explorador anterior no se muestran estos vínculos.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-143">The browser image above doesn't show these links.</span></span> <span data-ttu-id="e7d2f-144">Según el tamaño del explorador, tendrá que hacer clic en el icono de navegación para que se muestren.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-144">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![icono de navegación en la esquina superior derecha](start-mvc/_static/2.png)

<span data-ttu-id="e7d2f-146">Si iba a efectuar una ejecución en modo de depuración, pulse **MAYÚS-F5** para detener la depuración.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-146">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="e7d2f-147">En la siguiente sección de este tutorial obtendrá información sobre MVC y empezará a escribir código.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-147">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e7d2f-148">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e7d2f-148">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!INCLUDE [](~/includes/net-core-prereqs.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e7d2f-149">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e7d2f-149">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="e7d2f-150">Instale Visual Studio Community 2017.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-150">Install Visual Studio Community 2017.</span></span> <span data-ttu-id="e7d2f-151">Seleccione la descarga de Community.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-151">Select the Community download.</span></span> <span data-ttu-id="e7d2f-152">Omita este paso si tiene instalado Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-152">Skip this step if you have Visual Studio 2017 installed.</span></span>

* [<span data-ttu-id="e7d2f-153">Página principal del instalador de Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="e7d2f-153">Visual Studio 2017 Home page installer</span></span>](https://www.visualstudio.com/)

<span data-ttu-id="e7d2f-154">Ejecute el instalador y seleccione las siguientes cargas de trabajo:</span><span class="sxs-lookup"><span data-stu-id="e7d2f-154">Run the installer and select the following workloads:</span></span>

* <span data-ttu-id="e7d2f-155">**Desarrollo de ASP.NET y web** (en **Web y nube**)</span><span class="sxs-lookup"><span data-stu-id="e7d2f-155">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
* <span data-ttu-id="e7d2f-156">**Desarrollo multiplataforma de .NET Core** (en **Otros conjuntos de herramientas**)</span><span class="sxs-lookup"><span data-stu-id="e7d2f-156">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>

![**Desarrollo de ASP.NET y web** (en **Web y nube**)](start-mvc/_static/web_workload.png)

![**Desarrollo multiplataforma de .NET Core** (en **Otros conjuntos de herramientas**)](start-mvc/_static/x_plat_wl.png)

---

## <a name="create-a-web-app"></a><span data-ttu-id="e7d2f-159">Crear una aplicación web</span><span class="sxs-lookup"><span data-stu-id="e7d2f-159">Create a web app</span></span>

<span data-ttu-id="e7d2f-160">En Visual Studio, seleccione **Archivo > Nuevo > Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-160">From Visual Studio, select  **File > New > Project**.</span></span>

![Archivo > Nuevo > Proyecto](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="e7d2f-162">Complete el cuadro de diálogo **Nuevo proyecto**:</span><span class="sxs-lookup"><span data-stu-id="e7d2f-162">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="e7d2f-163">En el panel izquierdo, pulse **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-163">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="e7d2f-164">En el panel central, pulse **Aplicación web ASP.NET Core (.NET Core)**.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-164">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="e7d2f-165">Asigne al proyecto el nombre "MvcMovie" (es importante asignarle este nombre para que, al copiar el código, coincida con el espacio de nombres).</span><span class="sxs-lookup"><span data-stu-id="e7d2f-165">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="e7d2f-166">Pulse **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-166">Tap **OK**</span></span>

![<span data-ttu-id="e7d2f-167">Cuadro de diálogo Nuevo proyecto, .NET CORE en el panel izquierdo, Aplicación web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e7d2f-167">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e7d2f-168">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e7d2f-168">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="e7d2f-169">Complete el cuadro de diálogo **Nueva aplicación web ASP.NET Core (.NET Core) - MvcMovie**:</span><span class="sxs-lookup"><span data-stu-id="e7d2f-169">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="e7d2f-170">En el cuadro desplegable del selector de versión, seleccione **ASP.NET Core 2.-**.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-170">In the version selector drop-down box select **ASP.NET Core 2.-**</span></span>
* <span data-ttu-id="e7d2f-171">Seleccione **Aplicación web (controlador de vista de modelos)**.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-171">Select **Web Application(Model-View-Controller)**</span></span>
* <span data-ttu-id="e7d2f-172">Pulse **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-172">Tap **OK**.</span></span>

![<span data-ttu-id="e7d2f-173">Cuadro de diálogo Nuevo proyecto, .NET CORE en el panel izquierdo, Aplicación web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e7d2f-173">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e7d2f-174">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e7d2f-174">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="e7d2f-175">Complete el cuadro de diálogo **Nueva aplicación web ASP.NET Core (.NET Core) - MvcMovie**:</span><span class="sxs-lookup"><span data-stu-id="e7d2f-175">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="e7d2f-176">En el cuadro desplegable del selector de versión, pulse **ASP.NET Core 1.1**.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-176">In the version selector drop-down box tap **ASP.NET Core 1.1**</span></span>
* <span data-ttu-id="e7d2f-177">Pulse **Aplicación web**.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-177">Tap **Web Application**</span></span>
* <span data-ttu-id="e7d2f-178">Mantenga la opción predeterminado **Sin autenticación**.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-178">Keep the default **No Authentication**</span></span>
* <span data-ttu-id="e7d2f-179">Pulse **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-179">Tap **OK**.</span></span>

![Nueva aplicación web de ASP.NET Core](start-mvc/_static/p3.png)

---

<span data-ttu-id="e7d2f-181">Visual Studio ha usado una plantilla predeterminada para el proyecto de MVC que acaba de crear.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-181">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="e7d2f-182">Si escribe un nombre de proyecto y selecciona algunas opciones, dispondrá de inmediato de una aplicación operativa.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-182">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="e7d2f-183">Se trata de un proyecto introductorio básico, pero es un buen punto de partida.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-183">This is a basic starter project, and it's a good place to start,</span></span>

<span data-ttu-id="e7d2f-184">Pulse **F5** para ejecutar la aplicación en modo de depuración o **Ctrl-F5** para ejecutarla en modo de no depuración.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-184">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<span data-ttu-id="e7d2f-185"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![aplicación en ejecución](start-mvc/_static/1.png)</span><span class="sxs-lookup"><span data-stu-id="e7d2f-185"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![running app](start-mvc/_static/1.png)</span></span>

* <span data-ttu-id="e7d2f-186">Visual Studio inicia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) y ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-186">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="e7d2f-187">Tenga en cuenta que en la barra de direcciones aparece `localhost:port#` (y no algo como `example.com`).</span><span class="sxs-lookup"><span data-stu-id="e7d2f-187">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="e7d2f-188">Esto es así porque `localhost` es el nombre de host estándar del equipo local.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-188">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="e7d2f-189">Cuando Visual Studio crea un proyecto web, se usa un puerto aleatorio para el servidor web.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-189">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="e7d2f-190">En la imagen anterior, el número de puerto es el 5000.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-190">In the image above, the port number is 5000.</span></span> <span data-ttu-id="e7d2f-191">La dirección URL del explorador es `localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-191">The URL in the browser shows `localhost:5000`.</span></span> <span data-ttu-id="e7d2f-192">Al ejecutar la aplicación verá otro puerto distinto.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-192">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="e7d2f-193">Iniciar la aplicación con **CTRL+F5** (modo de no depuración) le permite efectuar cambios en el código, guardar el archivo, actualizar el explorador y ver los cambios de código.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-193">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="e7d2f-194">Muchos desarrolladores prefieren usar el modo de no depuración para iniciar la aplicación rápidamente y ver los cambios.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-194">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="e7d2f-195">Puede iniciar la aplicación en modo de depuración o en modo de no depuración desde el elemento de menú **Depurar**:</span><span class="sxs-lookup"><span data-stu-id="e7d2f-195">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![Menú Depurar](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="e7d2f-197">Puede depurar la aplicación pulsando el botón **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-197">You can debug the app by tapping the **IIS Express** button</span></span>

![IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="e7d2f-199">La plantilla predeterminada proporciona los vínculos **Inicio, Acerca de** y **Contacto**, totalmente funcionales.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-199">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="e7d2f-200">En la imagen del explorador anterior no se muestran estos vínculos.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-200">The browser image above doesn't show these links.</span></span> <span data-ttu-id="e7d2f-201">Según el tamaño del explorador, tendrá que hacer clic en el icono de navegación para que se muestren.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-201">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![icono de navegación en la esquina superior derecha](start-mvc/_static/2.png)

<span data-ttu-id="e7d2f-203">Si iba a efectuar una ejecución en modo de depuración, pulse **MAYÚS-F5** para detener la depuración.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-203">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="e7d2f-204">En la siguiente sección de este tutorial obtendrá información sobre MVC y empezará a escribir código.</span><span class="sxs-lookup"><span data-stu-id="e7d2f-204">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

::: moniker-end

> [!div class="step-by-step"]
> [<span data-ttu-id="e7d2f-205">Siguiente</span><span class="sxs-lookup"><span data-stu-id="e7d2f-205">Next</span></span>](adding-controller.md)  
