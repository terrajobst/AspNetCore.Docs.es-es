---
title: Introducción a ASP.NET Core MVC y Visual Studio
author: rick-anderson
description: Obtenga información sobre cómo empezar a usar ASP.NET Core MVC y Visual Studio.
ms.author: riande
ms.date: 10/07/2017
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 738c49272c2ae2b075866001f06ad09fe73969f9
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2018
ms.locfileid: "52862205"
---
# <a name="get-started-with-aspnet-core-mvc-and-visual-studio"></a><span data-ttu-id="eda12-103">Introducción a ASP.NET Core MVC y Visual Studio</span><span class="sxs-lookup"><span data-stu-id="eda12-103">Get started with ASP.NET Core MVC and Visual Studio</span></span>

<span data-ttu-id="eda12-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="eda12-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="eda12-105">Hay tres versiones de este tutorial:</span><span class="sxs-lookup"><span data-stu-id="eda12-105">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="eda12-106">macOS: [Creación de una aplicación de ASP.NET Core MVC con Visual Studio para Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="eda12-106">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="eda12-107">Windows: [Creación de una aplicación de ASP.NET Core MVC con Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="eda12-107">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="eda12-108">macOS, Linux y Windows: [Creación de una aplicación de ASP.NET Core MVC con Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="eda12-108">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

> [!NOTE]
> <span data-ttu-id="eda12-109">Vamos a probar la facilidad de uso de una nueva estructura propuesta para la tabla de contenido de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="eda12-109">We’re testing the usability of a proposed new structure for the ASP.NET Core table of contents.</span></span>  <span data-ttu-id="eda12-110">Si tiene unos minutos para realizar un ejercicio de búsqueda de 7 temas diferentes en la tabla actual o propuesta de contenido, [haga clic aquí para participar en el estudio](https://dpk4xbh5.optimalworkshop.com/treejack/aa11wn82).</span><span class="sxs-lookup"><span data-stu-id="eda12-110">If you have a few minutes to try an exercise of finding 7 different topics in the current or proposed table of contents, please [click here to participate in the study](https://dpk4xbh5.optimalworkshop.com/treejack/aa11wn82).</span></span>

## <a name="install-visual-studio-and-net-core"></a><span data-ttu-id="eda12-111">Instalar Visual Studio y .NET Core</span><span class="sxs-lookup"><span data-stu-id="eda12-111">Install Visual Studio and .NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-web-app"></a><span data-ttu-id="eda12-112">Creación de una aplicación web</span><span class="sxs-lookup"><span data-stu-id="eda12-112">Create a web app</span></span>

<span data-ttu-id="eda12-113">En Visual Studio, seleccione **Archivo > Nuevo > Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="eda12-113">From Visual Studio, select  **File > New > Project**.</span></span>

![Archivo > Nuevo > Proyecto](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="eda12-115">Complete el cuadro de diálogo **Nuevo proyecto**:</span><span class="sxs-lookup"><span data-stu-id="eda12-115">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="eda12-116">En el panel izquierdo, pulse **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="eda12-116">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="eda12-117">En el panel central, pulse **Aplicación web ASP.NET Core (.NET Core)**.</span><span class="sxs-lookup"><span data-stu-id="eda12-117">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="eda12-118">Asigne al proyecto el nombre "MvcMovie" (es importante asignarle este nombre para que, al copiar el código, coincida con el espacio de nombres).</span><span class="sxs-lookup"><span data-stu-id="eda12-118">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="eda12-119">Pulse **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="eda12-119">Tap **OK**</span></span>

![<span data-ttu-id="eda12-120">Cuadro de diálogo Nuevo proyecto, .NET CORE en el panel izquierdo, Aplicación web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="eda12-120">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2-21.png)

<span data-ttu-id="eda12-121">Complete el cuadro de diálogo **Nueva aplicación web ASP.NET Core (.NET Core) - MvcMovie**:</span><span class="sxs-lookup"><span data-stu-id="eda12-121">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="eda12-122">En el cuadro desplegable del selector de versión, seleccione **ASP.NET Core 2.1**.</span><span class="sxs-lookup"><span data-stu-id="eda12-122">In the version selector drop-down box select **ASP.NET Core 2.1**</span></span>
* <span data-ttu-id="eda12-123">Seleccione **Aplicación web (Modelo-Vista-Controlador)**.</span><span class="sxs-lookup"><span data-stu-id="eda12-123">Select **Web Application (Model-View-Controller)**</span></span>
* <span data-ttu-id="eda12-124">Pulse **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="eda12-124">Tap **OK**.</span></span>

![<span data-ttu-id="eda12-125">Cuadro de diálogo Nuevo proyecto, .NET CORE en el panel izquierdo, Aplicación web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="eda12-125">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="eda12-126">Visual Studio ha usado una plantilla predeterminada para el proyecto de MVC que acaba de crear.</span><span class="sxs-lookup"><span data-stu-id="eda12-126">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="eda12-127">Si escribe un nombre de proyecto y selecciona algunas opciones, dispondrá de inmediato de una aplicación operativa.</span><span class="sxs-lookup"><span data-stu-id="eda12-127">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="eda12-128">Se trata de un proyecto introductorio básico, pero es un buen punto de partida.</span><span class="sxs-lookup"><span data-stu-id="eda12-128">This is a basic starter project, and it's a good place to start.</span></span>

<span data-ttu-id="eda12-129">Pulse **F5** para ejecutar la aplicación en modo de depuración o **Ctrl-F5** para ejecutarla en modo de no depuración.</span><span class="sxs-lookup"><span data-stu-id="eda12-129">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<span data-ttu-id="eda12-130"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![aplicación en ejecución](start-mvc/_static/1.png)</span><span class="sxs-lookup"><span data-stu-id="eda12-130"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![running app](start-mvc/_static/1.png)</span></span>

* <span data-ttu-id="eda12-131">Visual Studio inicia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) y ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="eda12-131">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="eda12-132">Tenga en cuenta que en la barra de direcciones aparece `localhost:port#` (y no algo como `example.com`).</span><span class="sxs-lookup"><span data-stu-id="eda12-132">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="eda12-133">Esto es así porque `localhost` es el nombre de host estándar del equipo local.</span><span class="sxs-lookup"><span data-stu-id="eda12-133">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="eda12-134">Cuando Visual Studio crea un proyecto web, se usa un puerto aleatorio para el servidor web.</span><span class="sxs-lookup"><span data-stu-id="eda12-134">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="eda12-135">En la imagen anterior, el número de puerto es el 5000.</span><span class="sxs-lookup"><span data-stu-id="eda12-135">In the image above, the port number is 5000.</span></span> <span data-ttu-id="eda12-136">La dirección URL del explorador es `localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="eda12-136">The URL in the browser shows `localhost:5000`.</span></span> <span data-ttu-id="eda12-137">Al ejecutar la aplicación verá otro puerto distinto.</span><span class="sxs-lookup"><span data-stu-id="eda12-137">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="eda12-138">Iniciar la aplicación con **CTRL+F5** (modo de no depuración) le permite efectuar cambios en el código, guardar el archivo, actualizar el explorador y ver los cambios de código.</span><span class="sxs-lookup"><span data-stu-id="eda12-138">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="eda12-139">Muchos desarrolladores prefieren usar el modo de no depuración para iniciar la aplicación rápidamente y ver los cambios.</span><span class="sxs-lookup"><span data-stu-id="eda12-139">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="eda12-140">Puede iniciar la aplicación en modo de depuración o en modo de no depuración desde el elemento de menú **Depurar**:</span><span class="sxs-lookup"><span data-stu-id="eda12-140">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![Menú Depurar](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="eda12-142">Puede depurar la aplicación pulsando el botón **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="eda12-142">You can debug the app by tapping the **IIS Express** button</span></span>

![IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="eda12-144">La plantilla predeterminada proporciona los vínculos **Inicio, Acerca de** y **Contacto**, totalmente funcionales.</span><span class="sxs-lookup"><span data-stu-id="eda12-144">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="eda12-145">En la imagen del explorador anterior no se muestran estos vínculos.</span><span class="sxs-lookup"><span data-stu-id="eda12-145">The browser image above doesn't show these links.</span></span> <span data-ttu-id="eda12-146">Según el tamaño del explorador, tendrá que hacer clic en el icono de navegación para que se muestren.</span><span class="sxs-lookup"><span data-stu-id="eda12-146">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![icono de navegación en la esquina superior derecha](start-mvc/_static/2.png)

<span data-ttu-id="eda12-148">Si iba a efectuar una ejecución en modo de depuración, pulse **MAYÚS-F5** para detener la depuración.</span><span class="sxs-lookup"><span data-stu-id="eda12-148">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="eda12-149">En la siguiente sección de este tutorial obtendrá información sobre MVC y empezará a escribir código.</span><span class="sxs-lookup"><span data-stu-id="eda12-149">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!INCLUDE [](~/includes/net-core-prereqs.md)]

## <a name="create-a-web-app"></a><span data-ttu-id="eda12-150">Creación de una aplicación web</span><span class="sxs-lookup"><span data-stu-id="eda12-150">Create a web app</span></span>

<span data-ttu-id="eda12-151">En Visual Studio, seleccione **Archivo > Nuevo > Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="eda12-151">From Visual Studio, select  **File > New > Project**.</span></span>

![Archivo > Nuevo > Proyecto](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="eda12-153">Complete el cuadro de diálogo **Nuevo proyecto**:</span><span class="sxs-lookup"><span data-stu-id="eda12-153">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="eda12-154">En el panel izquierdo, pulse **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="eda12-154">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="eda12-155">En el panel central, pulse **Aplicación web ASP.NET Core (.NET Core)**.</span><span class="sxs-lookup"><span data-stu-id="eda12-155">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="eda12-156">Asigne al proyecto el nombre "MvcMovie" (es importante asignarle este nombre para que, al copiar el código, coincida con el espacio de nombres).</span><span class="sxs-lookup"><span data-stu-id="eda12-156">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="eda12-157">Pulse **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="eda12-157">Tap **OK**</span></span>

![<span data-ttu-id="eda12-158">Cuadro de diálogo Nuevo proyecto, .NET CORE en el panel izquierdo, Aplicación web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="eda12-158">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2.png)

<span data-ttu-id="eda12-159">Complete el cuadro de diálogo **Nueva aplicación web ASP.NET Core (.NET Core) - MvcMovie**:</span><span class="sxs-lookup"><span data-stu-id="eda12-159">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="eda12-160">En el cuadro desplegable del selector de versión, seleccione **ASP.NET Core 2.-**.</span><span class="sxs-lookup"><span data-stu-id="eda12-160">In the version selector drop-down box select **ASP.NET Core 2.-**</span></span>
* <span data-ttu-id="eda12-161">Seleccione **Aplicación web (controlador de vista de modelos)**.</span><span class="sxs-lookup"><span data-stu-id="eda12-161">Select **Web Application(Model-View-Controller)**</span></span>
* <span data-ttu-id="eda12-162">Pulse **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="eda12-162">Tap **OK**.</span></span>

![<span data-ttu-id="eda12-163">Cuadro de diálogo Nuevo proyecto, .NET CORE en el panel izquierdo, Aplicación web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="eda12-163">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22.png)

<span data-ttu-id="eda12-164">Visual Studio ha usado una plantilla predeterminada para el proyecto de MVC que acaba de crear.</span><span class="sxs-lookup"><span data-stu-id="eda12-164">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="eda12-165">Si escribe un nombre de proyecto y selecciona algunas opciones, dispondrá de inmediato de una aplicación operativa.</span><span class="sxs-lookup"><span data-stu-id="eda12-165">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="eda12-166">Se trata de un proyecto introductorio básico, pero es un buen punto de partida.</span><span class="sxs-lookup"><span data-stu-id="eda12-166">This is a basic starter project, and it's a good place to start,</span></span>

<span data-ttu-id="eda12-167">Pulse **F5** para ejecutar la aplicación en modo de depuración o **Ctrl-F5** para ejecutarla en modo de no depuración.</span><span class="sxs-lookup"><span data-stu-id="eda12-167">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<span data-ttu-id="eda12-168"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![aplicación en ejecución](start-mvc/_static/1.png)</span><span class="sxs-lookup"><span data-stu-id="eda12-168"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![running app](start-mvc/_static/1.png)</span></span>

* <span data-ttu-id="eda12-169">Visual Studio inicia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) y ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="eda12-169">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="eda12-170">Tenga en cuenta que en la barra de direcciones aparece `localhost:port#` (y no algo como `example.com`).</span><span class="sxs-lookup"><span data-stu-id="eda12-170">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="eda12-171">Esto es así porque `localhost` es el nombre de host estándar del equipo local.</span><span class="sxs-lookup"><span data-stu-id="eda12-171">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="eda12-172">Cuando Visual Studio crea un proyecto web, se usa un puerto aleatorio para el servidor web.</span><span class="sxs-lookup"><span data-stu-id="eda12-172">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="eda12-173">En la imagen anterior, el número de puerto es el 5000.</span><span class="sxs-lookup"><span data-stu-id="eda12-173">In the image above, the port number is 5000.</span></span> <span data-ttu-id="eda12-174">La dirección URL del explorador es `localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="eda12-174">The URL in the browser shows `localhost:5000`.</span></span> <span data-ttu-id="eda12-175">Al ejecutar la aplicación verá otro puerto distinto.</span><span class="sxs-lookup"><span data-stu-id="eda12-175">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="eda12-176">Iniciar la aplicación con **CTRL+F5** (modo de no depuración) le permite efectuar cambios en el código, guardar el archivo, actualizar el explorador y ver los cambios de código.</span><span class="sxs-lookup"><span data-stu-id="eda12-176">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="eda12-177">Muchos desarrolladores prefieren usar el modo de no depuración para iniciar la aplicación rápidamente y ver los cambios.</span><span class="sxs-lookup"><span data-stu-id="eda12-177">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="eda12-178">Puede iniciar la aplicación en modo de depuración o en modo de no depuración desde el elemento de menú **Depurar**:</span><span class="sxs-lookup"><span data-stu-id="eda12-178">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![Menú Depurar](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="eda12-180">Puede depurar la aplicación pulsando el botón **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="eda12-180">You can debug the app by tapping the **IIS Express** button</span></span>

![IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="eda12-182">La plantilla predeterminada proporciona los vínculos **Inicio, Acerca de** y **Contacto**, totalmente funcionales.</span><span class="sxs-lookup"><span data-stu-id="eda12-182">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="eda12-183">En la imagen del explorador anterior no se muestran estos vínculos.</span><span class="sxs-lookup"><span data-stu-id="eda12-183">The browser image above doesn't show these links.</span></span> <span data-ttu-id="eda12-184">Según el tamaño del explorador, tendrá que hacer clic en el icono de navegación para que se muestren.</span><span class="sxs-lookup"><span data-stu-id="eda12-184">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![icono de navegación en la esquina superior derecha](start-mvc/_static/2.png)

<span data-ttu-id="eda12-186">Si iba a efectuar una ejecución en modo de depuración, pulse **MAYÚS-F5** para detener la depuración.</span><span class="sxs-lookup"><span data-stu-id="eda12-186">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="eda12-187">En la siguiente sección de este tutorial obtendrá información sobre MVC y empezará a escribir código.</span><span class="sxs-lookup"><span data-stu-id="eda12-187">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

::: moniker-end

> [!div class="step-by-step"]
> [<span data-ttu-id="eda12-188">Siguiente</span><span class="sxs-lookup"><span data-stu-id="eda12-188">Next</span></span>](adding-controller.md)  
