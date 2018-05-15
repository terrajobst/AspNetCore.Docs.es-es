---
title: Introducción a ASP.NET Core MVC y Visual Studio
author: rick-anderson
description: Obtenga información sobre cómo empezar a usar ASP.NET Core MVC y Visual Studio.
manager: wpickett
ms.author: riande
ms.date: 10/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: b7036b92e8f25613917cfc4112389be81c12f614
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/03/2018
---
# <a name="get-started-with-aspnet-core-mvc-and-visual-studio"></a><span data-ttu-id="fbc16-103">Introducción a ASP.NET Core MVC y Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fbc16-103">Get started with ASP.NET Core MVC and Visual Studio</span></span>

<span data-ttu-id="fbc16-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="fbc16-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [consider RP](../../includes/razor.md)]

<span data-ttu-id="fbc16-105">Hay tres versiones de este tutorial:</span><span class="sxs-lookup"><span data-stu-id="fbc16-105">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="fbc16-106">macOS: [Creación de una aplicación de ASP.NET Core MVC con Visual Studio para Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="fbc16-106">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="fbc16-107">Windows: [Creación de una aplicación de ASP.NET Core MVC con Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="fbc16-107">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="fbc16-108">macOS, Linux y Windows: [Creación de una aplicación de ASP.NET Core MVC con Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="fbc16-108">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

## <a name="install-visual-studio-and-net-core"></a><span data-ttu-id="fbc16-109">Instalar Visual Studio y .NET Core</span><span class="sxs-lookup"><span data-stu-id="fbc16-109">Install Visual Studio and .NET Core</span></span>

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="fbc16-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="fbc16-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
[!INCLUDE [](~/includes/net-core-prereqs.md)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="fbc16-111">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="fbc16-111">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
<span data-ttu-id="fbc16-112">Instale Visual Studio Community 2017.</span><span class="sxs-lookup"><span data-stu-id="fbc16-112">Install Visual Studio Community 2017.</span></span> <span data-ttu-id="fbc16-113">Seleccione la descarga de Community.</span><span class="sxs-lookup"><span data-stu-id="fbc16-113">Select the Community download.</span></span> <span data-ttu-id="fbc16-114">Omita este paso si tiene instalado Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="fbc16-114">Skip this step if you have Visual Studio 2017 installed.</span></span>

* [<span data-ttu-id="fbc16-115">Página principal del instalador de Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="fbc16-115">Visual Studio 2017 Home page installer</span></span>](https://www.visualstudio.com/)

<span data-ttu-id="fbc16-116">Ejecute el instalador y seleccione las siguientes cargas de trabajo:</span><span class="sxs-lookup"><span data-stu-id="fbc16-116">Run the installer and select the following workloads:</span></span>

* <span data-ttu-id="fbc16-117">**Desarrollo de ASP.NET y web** (en **Web y nube**)</span><span class="sxs-lookup"><span data-stu-id="fbc16-117">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
* <span data-ttu-id="fbc16-118">**Desarrollo multiplataforma de .NET Core** (en **Otros conjuntos de herramientas**)</span><span class="sxs-lookup"><span data-stu-id="fbc16-118">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>

![**Desarrollo de ASP.NET y web** (en **Web y nube**)](start-mvc/_static/web_workload.png)

![**Desarrollo multiplataforma de .NET Core** (en **Otros conjuntos de herramientas**)](start-mvc/_static/x_plat_wl.png)

* * *
## <a name="create-a-web-app"></a><span data-ttu-id="fbc16-121">Crear una aplicación web</span><span class="sxs-lookup"><span data-stu-id="fbc16-121">Create a web app</span></span>

<span data-ttu-id="fbc16-122">En Visual Studio, seleccione **Archivo > Nuevo > Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="fbc16-122">From Visual Studio, select  **File > New > Project**.</span></span>

![Archivo > Nuevo > Proyecto](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="fbc16-124">Complete el cuadro de diálogo **Nuevo proyecto**:</span><span class="sxs-lookup"><span data-stu-id="fbc16-124">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="fbc16-125">En el panel izquierdo, pulse **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="fbc16-125">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="fbc16-126">En el panel central, pulse **Aplicación web ASP.NET Core (.NET Core)**.</span><span class="sxs-lookup"><span data-stu-id="fbc16-126">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="fbc16-127">Asigne al proyecto el nombre "MvcMovie" (es importante asignarle este nombre para que, al copiar el código, coincida con el espacio de nombres).</span><span class="sxs-lookup"><span data-stu-id="fbc16-127">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="fbc16-128">Pulse **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="fbc16-128">Tap **OK**</span></span>

![<span data-ttu-id="fbc16-129">Cuadro de diálogo Nuevo proyecto, .NET CORE en el panel izquierdo, Aplicación web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fbc16-129">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2.png)


# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="fbc16-130">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="fbc16-130">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="fbc16-131">Complete el cuadro de diálogo **Nueva aplicación web ASP.NET Core (.NET Core) - MvcMovie**:</span><span class="sxs-lookup"><span data-stu-id="fbc16-131">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="fbc16-132">En el cuadro desplegable del selector de versión, seleccione **ASP.NET Core 2.-**.</span><span class="sxs-lookup"><span data-stu-id="fbc16-132">In the version selector drop-down box select **ASP.NET Core 2.-**</span></span>
* <span data-ttu-id="fbc16-133">Seleccione **Aplicación web (controlador de vista de modelos)**.</span><span class="sxs-lookup"><span data-stu-id="fbc16-133">Select **Web Application(Model-View-Controller)**</span></span>
* <span data-ttu-id="fbc16-134">Pulse **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="fbc16-134">Tap **OK**.</span></span>

![<span data-ttu-id="fbc16-135">Cuadro de diálogo Nuevo proyecto, .NET CORE en el panel izquierdo, Aplicación web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fbc16-135">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="fbc16-136">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="fbc16-136">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="fbc16-137">Complete el cuadro de diálogo **Nueva aplicación web ASP.NET Core (.NET Core) - MvcMovie**:</span><span class="sxs-lookup"><span data-stu-id="fbc16-137">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="fbc16-138">En el cuadro desplegable del selector de versión, pulse **ASP.NET Core 1.1**.</span><span class="sxs-lookup"><span data-stu-id="fbc16-138">In the version selector drop-down box tap **ASP.NET Core 1.1**</span></span>
* <span data-ttu-id="fbc16-139">Pulse **Aplicación web**.</span><span class="sxs-lookup"><span data-stu-id="fbc16-139">Tap **Web Application**</span></span>
* <span data-ttu-id="fbc16-140">Mantenga la opción predeterminado **Sin autenticación**.</span><span class="sxs-lookup"><span data-stu-id="fbc16-140">Keep the default **No Authentication**</span></span>
* <span data-ttu-id="fbc16-141">Pulse **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="fbc16-141">Tap **OK**.</span></span>

![Nueva aplicación web de ASP.NET Core](start-mvc/_static/p3.png)

---

<span data-ttu-id="fbc16-143">Visual Studio ha usado una plantilla predeterminada para el proyecto de MVC que acaba de crear.</span><span class="sxs-lookup"><span data-stu-id="fbc16-143">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="fbc16-144">Si escribe un nombre de proyecto y selecciona algunas opciones, dispondrá de inmediato de una aplicación operativa.</span><span class="sxs-lookup"><span data-stu-id="fbc16-144">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="fbc16-145">Se trata de un proyecto introductorio sencillo, pero es un buen punto de partida.</span><span class="sxs-lookup"><span data-stu-id="fbc16-145">This is a simple starter project, and it's a good place to start,</span></span>

<span data-ttu-id="fbc16-146">Pulse **F5** para ejecutar la aplicación en modo de depuración o **Ctrl-F5** para ejecutarla en modo de no depuración.</span><span class="sxs-lookup"><span data-stu-id="fbc16-146">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![aplicación en ejecución](start-mvc/_static/1.png)

* <span data-ttu-id="fbc16-148">Visual Studio inicia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) y ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="fbc16-148">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="fbc16-149">Tenga en cuenta que en la barra de direcciones aparece `localhost:port#` (y no algo como `example.com`).</span><span class="sxs-lookup"><span data-stu-id="fbc16-149">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="fbc16-150">Esto es así porque `localhost` es el nombre de host estándar del equipo local.</span><span class="sxs-lookup"><span data-stu-id="fbc16-150">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="fbc16-151">Cuando Visual Studio crea un proyecto web, se usa un puerto aleatorio para el servidor web.</span><span class="sxs-lookup"><span data-stu-id="fbc16-151">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="fbc16-152">En la imagen anterior, el número de puerto es el 5000.</span><span class="sxs-lookup"><span data-stu-id="fbc16-152">In the image above, the port number is 5000.</span></span> <span data-ttu-id="fbc16-153">La dirección URL del explorador es `localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="fbc16-153">The URL in the browser shows `localhost:5000`.</span></span> <span data-ttu-id="fbc16-154">Al ejecutar la aplicación verá otro puerto distinto.</span><span class="sxs-lookup"><span data-stu-id="fbc16-154">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="fbc16-155">Iniciar la aplicación con **CTRL+F5** (modo de no depuración) le permite efectuar cambios en el código, guardar el archivo, actualizar el explorador y ver los cambios de código.</span><span class="sxs-lookup"><span data-stu-id="fbc16-155">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="fbc16-156">Muchos desarrolladores prefieren usar el modo de no depuración para iniciar la aplicación rápidamente y ver los cambios.</span><span class="sxs-lookup"><span data-stu-id="fbc16-156">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="fbc16-157">Puede iniciar la aplicación en modo de depuración o en modo de no depuración desde el elemento de menú **Depurar**:</span><span class="sxs-lookup"><span data-stu-id="fbc16-157">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![Menú Depurar](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="fbc16-159">Puede depurar la aplicación pulsando el botón **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="fbc16-159">You can debug the app by tapping the **IIS Express** button</span></span>

![IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="fbc16-161">La plantilla predeterminada proporciona los vínculos **Inicio, Acerca de** y **Contacto**, totalmente funcionales.</span><span class="sxs-lookup"><span data-stu-id="fbc16-161">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="fbc16-162">En la imagen del explorador anterior no se muestran estos vínculos.</span><span class="sxs-lookup"><span data-stu-id="fbc16-162">The browser image above doesn't show these links.</span></span> <span data-ttu-id="fbc16-163">Según el tamaño del explorador, tendrá que hacer clic en el icono de navegación para que se muestren.</span><span class="sxs-lookup"><span data-stu-id="fbc16-163">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![icono de navegación en la esquina superior derecha](start-mvc/_static/2.png)

<span data-ttu-id="fbc16-165">Si iba a efectuar una ejecución en modo de depuración, pulse **MAYÚS-F5** para detener la depuración.</span><span class="sxs-lookup"><span data-stu-id="fbc16-165">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="fbc16-166">En la siguiente sección de este tutorial obtendrá información sobre MVC y empezará a escribir código.</span><span class="sxs-lookup"><span data-stu-id="fbc16-166">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="fbc16-167">Siguiente</span><span class="sxs-lookup"><span data-stu-id="fbc16-167">Next</span></span>](adding-controller.md)  
