---
title: "Introducción a ASP.NET Core MVC y Visual Studio"
author: rick-anderson
description: "Introducción a ASP.NET Core MVC y Visual Studio"
keywords: ASP.NET Core, MVC
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 9636e0e51e506d294ffb50a21165195c9d04fe20
ms.sourcegitcommit: bd05f7ea8f87ad076ef6e8b704698ebcba5ca80c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/25/2017
---
# <a name="getting-started-with-aspnet-core-mvc-and-visual-studio"></a><span data-ttu-id="8db65-104">Introducción a ASP.NET Core MVC y Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8db65-104">Getting started with ASP.NET Core MVC and Visual Studio</span></span>

<span data-ttu-id="8db65-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8db65-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="8db65-106">En este tutorial aprenderá los aspectos básicos de la creación de una aplicación web de ASP.NET Core MVC con [Visual Studio 2017](https://www.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="8db65-106">This tutorial will teach you the basics of building an ASP.NET Core MVC web app using [Visual Studio 2017](https://www.visualstudio.com/).</span></span> [!INCLUDE[consider RP](../../includes/razor.md)]

<span data-ttu-id="8db65-107">Hay tres versiones de este tutorial:</span><span class="sxs-lookup"><span data-stu-id="8db65-107">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="8db65-108">macOS: [Crear una aplicación de ASP.NET Core MVC con Visual Studio para Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="8db65-108">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="8db65-109">Windows: [Crear una aplicación de ASP.NET Core MVC con Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="8db65-109">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="8db65-110">macOS, Linux y Windows: [Crear una aplicación de ASP.NET Core MVC con Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="8db65-110">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

<span data-ttu-id="8db65-111">Para la versión de Visual Studio 2015 de este tutorial, vea la [versión de VS 2015 de la documentación de ASP.NET Core en formato PDF](https://github.com/aspnet/Docs/blob/master/aspnetcore/common/_static/aspnet-core-project-json.pdf).</span><span class="sxs-lookup"><span data-stu-id="8db65-111">For the Visual Studio 2015 version of this tutorial, see the [VS 2015 version of ASP.NET Core documentation in PDF format](https://github.com/aspnet/Docs/blob/master/aspnetcore/common/_static/aspnet-core-project-json.pdf).</span></span>

## <a name="install-visual-studio-and-net-core"></a><span data-ttu-id="8db65-112">Instalar Visual Studio y .NET Core</span><span class="sxs-lookup"><span data-stu-id="8db65-112">Install Visual Studio and .NET Core</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8db65-113">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8db65-113">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8db65-114">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8db65-114">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="8db65-115">Instale Visual Studio Community 2017.</span><span class="sxs-lookup"><span data-stu-id="8db65-115">Install Visual Studio Community 2017.</span></span> <span data-ttu-id="8db65-116">Seleccione la descarga de Community.</span><span class="sxs-lookup"><span data-stu-id="8db65-116">Select the Community download.</span></span> <span data-ttu-id="8db65-117">Omita este paso si tiene instalado Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="8db65-117">Skip this step if you have Visual Studio 2017 installed.</span></span>

* [<span data-ttu-id="8db65-118">Página principal del instalador de Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="8db65-118">Visual Studio 2017 Home page installer</span></span>](https://www.visualstudio.com/visual-studio-homepage-vs.aspx)

<span data-ttu-id="8db65-119">Ejecute el instalador y seleccione las siguientes cargas de trabajo:</span><span class="sxs-lookup"><span data-stu-id="8db65-119">Run the installer and select the following workloads:</span></span>

* <span data-ttu-id="8db65-120">**Desarrollo de ASP.NET y web** (en **Web y nube**)</span><span class="sxs-lookup"><span data-stu-id="8db65-120">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
* <span data-ttu-id="8db65-121">**Desarrollo multiplataforma de .NET Core** (en **Otros conjuntos de herramientas**)</span><span class="sxs-lookup"><span data-stu-id="8db65-121">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>

![**Desarrollo de ASP.NET y web** (en **Web y nube**)](start-mvc/_static/web_workload.png)

![**Desarrollo multiplataforma de .NET Core** (en **Otros conjuntos de herramientas**)](start-mvc/_static/x_plat_wl.png)

---

## <a name="create-a-web-app"></a><span data-ttu-id="8db65-124">Crear una aplicación web</span><span class="sxs-lookup"><span data-stu-id="8db65-124">Create a web app</span></span>

<span data-ttu-id="8db65-125">En Visual Studio, seleccione **Archivo > Nuevo > Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="8db65-125">From Visual Studio, select  **File > New > Project**.</span></span>

![Archivo > Nuevo > Proyecto](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="8db65-127">Complete el cuadro de diálogo **Nuevo proyecto**:</span><span class="sxs-lookup"><span data-stu-id="8db65-127">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="8db65-128">En el panel izquierdo, pulse **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="8db65-128">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="8db65-129">En el panel central, pulse **Aplicación web ASP.NET Core (.NET Core)**.</span><span class="sxs-lookup"><span data-stu-id="8db65-129">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="8db65-130">Asigne al proyecto el nombre "MvcMovie" (es importante asignarle este nombre para que, al copiar el código, coincida con el espacio de nombres).</span><span class="sxs-lookup"><span data-stu-id="8db65-130">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="8db65-131">Pulse **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="8db65-131">Tap **OK**</span></span>

![<span data-ttu-id="8db65-132">Cuadro de diálogo Nuevo proyecto, .NET CORE en el panel izquierdo, Aplicación web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8db65-132">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2.png)


# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8db65-133">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8db65-133">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="8db65-134">Complete el cuadro de diálogo **Nueva aplicación web ASP.NET Core (.NET Core) - MvcMovie**:</span><span class="sxs-lookup"><span data-stu-id="8db65-134">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="8db65-135">En el cuadro desplegable del selector de versión, seleccione **ASP.NET Core 2.-**.</span><span class="sxs-lookup"><span data-stu-id="8db65-135">In the version selector drop-down box select **ASP.NET Core 2.-**</span></span>
* <span data-ttu-id="8db65-136">Seleccione **Aplicación web (controlador de vista de modelos)**.</span><span class="sxs-lookup"><span data-stu-id="8db65-136">Select **Web Application(Model-View-Controller)**</span></span>
* <span data-ttu-id="8db65-137">Pulse **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="8db65-137">Tap **OK**.</span></span>

![<span data-ttu-id="8db65-138">Cuadro de diálogo Nuevo proyecto, .NET CORE en el panel izquierdo, Aplicación web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8db65-138">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8db65-139">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8db65-139">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="8db65-140">Complete el cuadro de diálogo **Nueva aplicación web ASP.NET Core (.NET Core) - MvcMovie**:</span><span class="sxs-lookup"><span data-stu-id="8db65-140">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="8db65-141">En el cuadro desplegable del selector de versión, pulse **ASP.NET Core 1.1**.</span><span class="sxs-lookup"><span data-stu-id="8db65-141">In the version selector drop-down box tap **ASP.NET Core 1.1**</span></span>
* <span data-ttu-id="8db65-142">Pulse **Aplicación web**.</span><span class="sxs-lookup"><span data-stu-id="8db65-142">Tap **Web Application**</span></span>
* <span data-ttu-id="8db65-143">Mantenga la opción predeterminado **Sin autenticación**.</span><span class="sxs-lookup"><span data-stu-id="8db65-143">Keep the default **No Authentication**</span></span>
* <span data-ttu-id="8db65-144">Pulse **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="8db65-144">Tap **OK**.</span></span>

![Nueva aplicación web de ASP.NET Core](start-mvc/_static/p3.png)

---

<span data-ttu-id="8db65-146">Visual Studio ha usado una plantilla predeterminada para el proyecto de MVC que acaba de crear.</span><span class="sxs-lookup"><span data-stu-id="8db65-146">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="8db65-147">Si escribe un nombre de proyecto y selecciona algunas opciones, dispondrá de inmediato de una aplicación operativa.</span><span class="sxs-lookup"><span data-stu-id="8db65-147">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="8db65-148">Se trata de un proyecto introductorio sencillo, pero es un buen punto de partida.</span><span class="sxs-lookup"><span data-stu-id="8db65-148">This is a simple starter project, and it's a good place to start,</span></span>

<span data-ttu-id="8db65-149">Pulse **F5** para ejecutar la aplicación en modo de depuración o **Ctrl-F5** para ejecutarla en modo de no depuración.</span><span class="sxs-lookup"><span data-stu-id="8db65-149">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![aplicación en ejecución](start-mvc/_static/1.png)

* <span data-ttu-id="8db65-151">Visual Studio inicia [IIS Express](http://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) y ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="8db65-151">Visual Studio starts [IIS Express](http://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="8db65-152">Tenga en cuenta que en la barra de direcciones aparece `localhost:port#` (y no algo como `example.com`).</span><span class="sxs-lookup"><span data-stu-id="8db65-152">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="8db65-153">Esto es así porque `localhost` es el nombre de host estándar del equipo local.</span><span class="sxs-lookup"><span data-stu-id="8db65-153">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="8db65-154">Cuando Visual Studio crea un proyecto web, se usa un puerto aleatorio para el servidor web.</span><span class="sxs-lookup"><span data-stu-id="8db65-154">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="8db65-155">En la imagen anterior, el número de puerto es el 5000.</span><span class="sxs-lookup"><span data-stu-id="8db65-155">In the image above, the port number is 5000.</span></span> <span data-ttu-id="8db65-156">Al ejecutar la aplicación verá otro número de puerto.</span><span class="sxs-lookup"><span data-stu-id="8db65-156">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="8db65-157">Iniciar la aplicación con **CTRL+F5** (modo de no depuración) le permite efectuar cambios en el código, guardar el archivo, actualizar el explorador y ver los cambios de código.</span><span class="sxs-lookup"><span data-stu-id="8db65-157">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="8db65-158">Muchos desarrolladores prefieren usar el modo de no depuración para iniciar la aplicación rápidamente y ver los cambios.</span><span class="sxs-lookup"><span data-stu-id="8db65-158">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="8db65-159">Puede iniciar la aplicación en modo de depuración o en modo de no depuración desde el elemento de menú **Depurar**:</span><span class="sxs-lookup"><span data-stu-id="8db65-159">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![Menú Depurar](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="8db65-161">Puede depurar la aplicación pulsando el botón **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="8db65-161">You can debug the app by tapping the **IIS Express** button</span></span>

![IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="8db65-163">La plantilla predeterminada le proporciona los vínculos **Inicio, Acerca de** y **Contacto**.</span><span class="sxs-lookup"><span data-stu-id="8db65-163">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="8db65-164">En la imagen del explorador anterior no se muestran estos vínculos.</span><span class="sxs-lookup"><span data-stu-id="8db65-164">The browser image above doesn't show these links.</span></span> <span data-ttu-id="8db65-165">Según el tamaño del explorador, tendrá que hacer clic en el icono de navegación para que se muestren.</span><span class="sxs-lookup"><span data-stu-id="8db65-165">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![icono de navegación en la esquina superior derecha](start-mvc/_static/2.png)

<span data-ttu-id="8db65-167">Si iba a efectuar una ejecución en modo de depuración, pulse **MAYÚS-F5** para detener la depuración.</span><span class="sxs-lookup"><span data-stu-id="8db65-167">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="8db65-168">En la siguiente sección de este tutorial obtendrá información sobre MVC y aprenderá a escribir código.</span><span class="sxs-lookup"><span data-stu-id="8db65-168">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="8db65-169">Siguiente</span><span class="sxs-lookup"><span data-stu-id="8db65-169">Next</span></span>](adding-controller.md)  
