---
title: "Introducción a ASP.NET Core MVC y Visual Studio"
author: rick-anderson
description: "Introducción a ASP.NET Core MVC y Visual Studio"
keywords: ASP.NET Core, MVC
ms.author: riande
manager: wpickett
ms.date: 10/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 4b86eb22e367d2305b7995421aec6f37008c5637
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
# <a name="getting-started-with-aspnet-core-mvc-and-visual-studio"></a><span data-ttu-id="23128-104">Introducción a ASP.NET Core MVC y Visual Studio</span><span class="sxs-lookup"><span data-stu-id="23128-104">Getting started with ASP.NET Core MVC and Visual Studio</span></span>

<span data-ttu-id="23128-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="23128-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[consider RP](../../includes/razor.md)]

<span data-ttu-id="23128-106">Hay tres versiones de este tutorial:</span><span class="sxs-lookup"><span data-stu-id="23128-106">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="23128-107">macOS: [Creación de una aplicación de ASP.NET Core MVC con Visual Studio para Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="23128-107">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="23128-108">Windows: [Creación de una aplicación de ASP.NET Core MVC con Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="23128-108">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="23128-109">macOS, Linux y Windows: [Creación de una aplicación de ASP.NET Core MVC con Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="23128-109">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

## <a name="install-visual-studio-and-net-core"></a><span data-ttu-id="23128-110">Instalar Visual Studio y .NET Core</span><span class="sxs-lookup"><span data-stu-id="23128-110">Install Visual Studio and .NET Core</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="23128-111">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="23128-111">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="23128-112">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="23128-112">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="23128-113">Instale Visual Studio Community 2017.</span><span class="sxs-lookup"><span data-stu-id="23128-113">Install Visual Studio Community 2017.</span></span> <span data-ttu-id="23128-114">Seleccione la descarga de Community.</span><span class="sxs-lookup"><span data-stu-id="23128-114">Select the Community download.</span></span> <span data-ttu-id="23128-115">Omita este paso si tiene instalado Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="23128-115">Skip this step if you have Visual Studio 2017 installed.</span></span>

* [<span data-ttu-id="23128-116">Página principal del instalador de Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="23128-116">Visual Studio 2017 Home page installer</span></span>](https://www.visualstudio.com/)

<span data-ttu-id="23128-117">Ejecute el instalador y seleccione las siguientes cargas de trabajo:</span><span class="sxs-lookup"><span data-stu-id="23128-117">Run the installer and select the following workloads:</span></span>

* <span data-ttu-id="23128-118">**Desarrollo de ASP.NET y web** (en **Web y nube**)</span><span class="sxs-lookup"><span data-stu-id="23128-118">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
* <span data-ttu-id="23128-119">**Desarrollo multiplataforma de .NET Core** (en **Otros conjuntos de herramientas**)</span><span class="sxs-lookup"><span data-stu-id="23128-119">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>

![**Desarrollo de ASP.NET y web** (en **Web y nube**)](start-mvc/_static/web_workload.png)

![**Desarrollo multiplataforma de .NET Core** (en **Otros conjuntos de herramientas**)](start-mvc/_static/x_plat_wl.png)

---

## <a name="create-a-web-app"></a><span data-ttu-id="23128-122">Crear una aplicación web</span><span class="sxs-lookup"><span data-stu-id="23128-122">Create a web app</span></span>

<span data-ttu-id="23128-123">En Visual Studio, seleccione **Archivo > Nuevo > Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="23128-123">From Visual Studio, select  **File > New > Project**.</span></span>

![Archivo > Nuevo > Proyecto](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="23128-125">Complete el cuadro de diálogo **Nuevo proyecto**:</span><span class="sxs-lookup"><span data-stu-id="23128-125">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="23128-126">En el panel izquierdo, pulse **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="23128-126">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="23128-127">En el panel central, pulse **Aplicación web ASP.NET Core (.NET Core)**.</span><span class="sxs-lookup"><span data-stu-id="23128-127">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="23128-128">Asigne al proyecto el nombre "MvcMovie" (es importante asignarle este nombre para que, al copiar el código, coincida con el espacio de nombres).</span><span class="sxs-lookup"><span data-stu-id="23128-128">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="23128-129">Pulse **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="23128-129">Tap **OK**</span></span>

![<span data-ttu-id="23128-130">Cuadro de diálogo Nuevo proyecto, .NET CORE en el panel izquierdo, Aplicación web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="23128-130">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2.png)


# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="23128-131">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="23128-131">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="23128-132">Complete el cuadro de diálogo **Nueva aplicación web ASP.NET Core (.NET Core) - MvcMovie**:</span><span class="sxs-lookup"><span data-stu-id="23128-132">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="23128-133">En el cuadro desplegable del selector de versión, seleccione **ASP.NET Core 2.-**.</span><span class="sxs-lookup"><span data-stu-id="23128-133">In the version selector drop-down box select **ASP.NET Core 2.-**</span></span>
* <span data-ttu-id="23128-134">Seleccione **Aplicación web (controlador de vista de modelos)**.</span><span class="sxs-lookup"><span data-stu-id="23128-134">Select **Web Application(Model-View-Controller)**</span></span>
* <span data-ttu-id="23128-135">Pulse **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="23128-135">Tap **OK**.</span></span>

![<span data-ttu-id="23128-136">Cuadro de diálogo Nuevo proyecto, .NET CORE en el panel izquierdo, Aplicación web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="23128-136">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="23128-137">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="23128-137">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="23128-138">Complete el cuadro de diálogo **Nueva aplicación web ASP.NET Core (.NET Core) - MvcMovie**:</span><span class="sxs-lookup"><span data-stu-id="23128-138">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="23128-139">En el cuadro desplegable del selector de versión, pulse **ASP.NET Core 1.1**.</span><span class="sxs-lookup"><span data-stu-id="23128-139">In the version selector drop-down box tap **ASP.NET Core 1.1**</span></span>
* <span data-ttu-id="23128-140">Pulse **Aplicación web**.</span><span class="sxs-lookup"><span data-stu-id="23128-140">Tap **Web Application**</span></span>
* <span data-ttu-id="23128-141">Mantenga la opción predeterminado **Sin autenticación**.</span><span class="sxs-lookup"><span data-stu-id="23128-141">Keep the default **No Authentication**</span></span>
* <span data-ttu-id="23128-142">Pulse **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="23128-142">Tap **OK**.</span></span>

![Nueva aplicación web de ASP.NET Core](start-mvc/_static/p3.png)

---

<span data-ttu-id="23128-144">Visual Studio ha usado una plantilla predeterminada para el proyecto de MVC que acaba de crear.</span><span class="sxs-lookup"><span data-stu-id="23128-144">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="23128-145">Si escribe un nombre de proyecto y selecciona algunas opciones, dispondrá de inmediato de una aplicación operativa.</span><span class="sxs-lookup"><span data-stu-id="23128-145">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="23128-146">Se trata de un proyecto introductorio sencillo, pero es un buen punto de partida.</span><span class="sxs-lookup"><span data-stu-id="23128-146">This is a simple starter project, and it's a good place to start,</span></span>

<span data-ttu-id="23128-147">Pulse **F5** para ejecutar la aplicación en modo de depuración o **Ctrl-F5** para ejecutarla en modo de no depuración.</span><span class="sxs-lookup"><span data-stu-id="23128-147">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![aplicación en ejecución](start-mvc/_static/1.png)

* <span data-ttu-id="23128-149">Visual Studio inicia [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) y ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="23128-149">Visual Studio starts [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="23128-150">Tenga en cuenta que en la barra de direcciones aparece `localhost:port#` (y no algo como `example.com`).</span><span class="sxs-lookup"><span data-stu-id="23128-150">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="23128-151">Esto es así porque `localhost` es el nombre de host estándar del equipo local.</span><span class="sxs-lookup"><span data-stu-id="23128-151">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="23128-152">Cuando Visual Studio crea un proyecto web, se usa un puerto aleatorio para el servidor web.</span><span class="sxs-lookup"><span data-stu-id="23128-152">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="23128-153">En la imagen anterior, el número de puerto es el 5000.</span><span class="sxs-lookup"><span data-stu-id="23128-153">In the image above, the port number is 5000.</span></span> <span data-ttu-id="23128-154">Al ejecutar la aplicación verá otro número de puerto.</span><span class="sxs-lookup"><span data-stu-id="23128-154">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="23128-155">Iniciar la aplicación con **CTRL+F5** (modo de no depuración) le permite efectuar cambios en el código, guardar el archivo, actualizar el explorador y ver los cambios de código.</span><span class="sxs-lookup"><span data-stu-id="23128-155">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="23128-156">Muchos desarrolladores prefieren usar el modo de no depuración para iniciar la aplicación rápidamente y ver los cambios.</span><span class="sxs-lookup"><span data-stu-id="23128-156">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="23128-157">Puede iniciar la aplicación en modo de depuración o en modo de no depuración desde el elemento de menú **Depurar**:</span><span class="sxs-lookup"><span data-stu-id="23128-157">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![Menú Depurar](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="23128-159">Puede depurar la aplicación pulsando el botón **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="23128-159">You can debug the app by tapping the **IIS Express** button</span></span>

![IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="23128-161">La plantilla predeterminada proporciona los vínculos **Inicio, Acerca de** y **Contacto**, totalmente funcionales.</span><span class="sxs-lookup"><span data-stu-id="23128-161">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="23128-162">En la imagen del explorador anterior no se muestran estos vínculos.</span><span class="sxs-lookup"><span data-stu-id="23128-162">The browser image above doesn't show these links.</span></span> <span data-ttu-id="23128-163">Según el tamaño del explorador, tendrá que hacer clic en el icono de navegación para que se muestren.</span><span class="sxs-lookup"><span data-stu-id="23128-163">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![icono de navegación en la esquina superior derecha](start-mvc/_static/2.png)

<span data-ttu-id="23128-165">Si iba a efectuar una ejecución en modo de depuración, pulse **MAYÚS-F5** para detener la depuración.</span><span class="sxs-lookup"><span data-stu-id="23128-165">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="23128-166">En la siguiente sección de este tutorial obtendrá información sobre MVC y empezará a escribir código.</span><span class="sxs-lookup"><span data-stu-id="23128-166">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="23128-167">Siguiente</span><span class="sxs-lookup"><span data-stu-id="23128-167">Next</span></span>](adding-controller.md)  
