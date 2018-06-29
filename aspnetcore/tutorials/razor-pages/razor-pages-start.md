---
title: Introducción a las páginas de Razor en ASP.NET Core
author: rick-anderson
description: Conozca los conceptos básicos de la compilación de una aplicación web de páginas de Razor de ASP.NET Core. Se recomienda usar páginas de Razor en las cargas de trabajo web en ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 5/30/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: d7cdf7c8fac3b2ac1e526c6eeee8205068964ec9
ms.sourcegitcommit: 545ff5a632e2281035c1becec1f99137298e4f5c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2018
ms.locfileid: "34582822"
---
# <a name="get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="fa0a4-104">Introducción a las páginas de Razor en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fa0a4-104">Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="fa0a4-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="fa0a4-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="fa0a4-106">Se recomienda seguir la versión de ASP.NET Core 2.1 de este tutorial,</span><span class="sxs-lookup"><span data-stu-id="fa0a4-106">We recommend you follow the ASP.NET Core 2.1 version of this tutorial.</span></span> <span data-ttu-id="fa0a4-107">ya que es **mucho** más fácil de seguir y abarca más características.</span><span class="sxs-lookup"><span data-stu-id="fa0a4-107">It's **much** easier to follow and covers more features.</span></span> <span data-ttu-id="fa0a4-108">Seleccione **ASP.NET Core 2.1** en el selector de versión.</span><span class="sxs-lookup"><span data-stu-id="fa0a4-108">Select **ASP.NET Core 2.1** in the version selector.</span></span>

![Selector de versión en la TDC](razor-pages-start/_static/v21.png)

::: moniker-end

<span data-ttu-id="fa0a4-110">En este tutorial se enseñan los conceptos básicos de la compilación de una aplicación web de páginas de Razor de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="fa0a4-110">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="fa0a4-111">Las páginas de Razor son el método recomendado para crear la interfaz de usuario de aplicaciones web en ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="fa0a4-111">Razor Pages is the recommended way to build UI for web apps in ASP.NET Core.</span></span>

<span data-ttu-id="fa0a4-112">Hay tres versiones de este tutorial:</span><span class="sxs-lookup"><span data-stu-id="fa0a4-112">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="fa0a4-113">Windows: este tutorial</span><span class="sxs-lookup"><span data-stu-id="fa0a4-113">Windows: This tutorial</span></span>
* <span data-ttu-id="fa0a4-114">MacOS: [Introducción a las páginas de Razor en ASP.NET Core con Visual Studio para Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="fa0a4-114">MacOS: [Get started with Razor Pages with Visual Studio for Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span></span>
* <span data-ttu-id="fa0a4-115">macOS, Linux y Windows: [Introducción a las páginas de Razor en ASP.NET Core con Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="fa0a4-115">macOS, Linux, and Windows: [Get started with ASP.NET Core Razor Pages in Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)</span></span>

<span data-ttu-id="fa0a4-116">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="fa0a4-116">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="prerequisites"></a><span data-ttu-id="fa0a4-117">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="fa0a4-117">Prerequisites</span></span>

<span data-ttu-id="fa0a4-118">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span><span class="sxs-lookup"><span data-stu-id="fa0a4-118">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span></span>

## <a name="create-a-razor-web-app"></a><span data-ttu-id="fa0a4-119">Creación de una aplicación web de Razor</span><span class="sxs-lookup"><span data-stu-id="fa0a4-119">Create a Razor web app</span></span>

* <span data-ttu-id="fa0a4-120">En el menú **Archivo** de Visual Studio, seleccione **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="fa0a4-120">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="fa0a4-121">Cree una aplicación web de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="fa0a4-121">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="fa0a4-122">Asigne al proyecto el nombre **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="fa0a4-122">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="fa0a4-123">Es importante asignarle el nombre *RazorPagesMovie* para que los espacios de nombres coincidan al copiar y pegar el código.</span><span class="sxs-lookup"><span data-stu-id="fa0a4-123">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
 <span data-ttu-id="fa0a4-124">![Nueva aplicación web de ASP.NET Core](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="fa0a4-124">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>
* <span data-ttu-id="fa0a4-125">Seleccione **ASP.NET Core 2.1** en la lista desplegable y, luego, **Aplicación web**.</span><span class="sxs-lookup"><span data-stu-id="fa0a4-125">Select **ASP.NET Core 2.1** in the dropdown, and then select **Web Application**.</span></span>

 ![Nueva aplicación web de ASP.NET Core](razor-pages-start/_static/np_2_2.1.png)

<span data-ttu-id="fa0a4-127">La plantilla de Visual Studio crea un proyecto de inicio:</span><span class="sxs-lookup"><span data-stu-id="fa0a4-127">The Visual Studio template creates a starter project:</span></span>

![Explorador de soluciones](razor-pages-start/_static/se2.1.png)

<span data-ttu-id="fa0a4-129">Presione **F5** para ejecutar la aplicación en modo de depuración o **Ctrl+F5** para que se ejecute sin adjuntar el depurador.</span><span class="sxs-lookup"><span data-stu-id="fa0a4-129">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger.</span></span> <span data-ttu-id="fa0a4-130">Seleccione **Aceptar** para dar su consentimiento al seguimiento.</span><span class="sxs-lookup"><span data-stu-id="fa0a4-130">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="fa0a4-131">Esta aplicación no lleva un seguimiento de la información personal.</span><span class="sxs-lookup"><span data-stu-id="fa0a4-131">This app doesn't track personal information.</span></span> <span data-ttu-id="fa0a4-132">El código generado con plantilla incluye activos que sirven para cumplir el [Reglamento general de protección de datos (RGPD)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="fa0a4-132">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

![Página Inicio o Índice](razor-pages-start/_static/homeGDPR.png)

<span data-ttu-id="fa0a4-134">En la siguiente imagen se muestra la aplicación tras haber aceptado el seguimiento:</span><span class="sxs-lookup"><span data-stu-id="fa0a4-134">The following image shows the app after accepting tracking:</span></span>

![Página Inicio o Índice](razor-pages-start/_static/home2.1.png)

* <span data-ttu-id="fa0a4-136">Visual Studio inicia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) y ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="fa0a4-136">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="fa0a4-137">En la barra de direcciones aparece `localhost:port#` (y no algo como `example.com`).</span><span class="sxs-lookup"><span data-stu-id="fa0a4-137">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="fa0a4-138">Esto es así porque `localhost` es el nombre de host estándar del equipo local.</span><span class="sxs-lookup"><span data-stu-id="fa0a4-138">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="fa0a4-139">Localhost solo sirve las solicitudes web del equipo local.</span><span class="sxs-lookup"><span data-stu-id="fa0a4-139">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="fa0a4-140">Cuando Visual Studio crea un proyecto web, se usa un puerto aleatorio para el servidor web.</span><span class="sxs-lookup"><span data-stu-id="fa0a4-140">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="fa0a4-141">En la imagen anterior, el número de puerto es 5000.</span><span class="sxs-lookup"><span data-stu-id="fa0a4-141">In the preceding image, the port number is 5000.</span></span> <span data-ttu-id="fa0a4-142">Al ejecutar la aplicación verá otro puerto distinto.</span><span class="sxs-lookup"><span data-stu-id="fa0a4-142">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="fa0a4-143">Iniciar la aplicación con **CTRL+F5** (modo de no depuración) le permite efectuar cambios en el código, guardar el archivo, actualizar el explorador y ver los cambios de código.</span><span class="sxs-lookup"><span data-stu-id="fa0a4-143">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="fa0a4-144">Muchos desarrolladores prefieren usar el modo de no depuración para iniciar la aplicación rápidamente y ver los cambios.</span><span class="sxs-lookup"><span data-stu-id="fa0a4-144">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE [razor-pages-start](~/includes/RP/2.1/razor-pages-start.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

## <a name="prerequisites"></a><span data-ttu-id="fa0a4-145">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="fa0a4-145">Prerequisites</span></span>

<span data-ttu-id="fa0a4-146">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span><span class="sxs-lookup"><span data-stu-id="fa0a4-146">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span></span>

## <a name="create-a-razor-web-app"></a><span data-ttu-id="fa0a4-147">Creación de una aplicación web de Razor</span><span class="sxs-lookup"><span data-stu-id="fa0a4-147">Create a Razor web app</span></span>

* <span data-ttu-id="fa0a4-148">En el menú **Archivo** de Visual Studio, seleccione **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="fa0a4-148">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="fa0a4-149">Cree una aplicación web de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="fa0a4-149">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="fa0a4-150">Asigne al proyecto el nombre **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="fa0a4-150">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="fa0a4-151">Es importante asignarle el nombre *RazorPagesMovie* para que los espacios de nombres coincidan al copiar y pegar el código.</span><span class="sxs-lookup"><span data-stu-id="fa0a4-151">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
  <span data-ttu-id="fa0a4-152">![Nueva aplicación web de ASP.NET Core](../../mvc/razor-pages/index/_static/np.png)</span><span class="sxs-lookup"><span data-stu-id="fa0a4-152">![new ASP.NET Core Web Application](../../mvc/razor-pages/index/_static/np.png)</span></span>
* <span data-ttu-id="fa0a4-153">Seleccione **ASP.NET Core 2.0** en la lista desplegable y, luego, seleccione **Aplicación web**.</span><span class="sxs-lookup"><span data-stu-id="fa0a4-153">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span>

  [!INCLUDE [install 2.0](~/includes/dotnetcore-on-dotnetfx-vs.md)]

<span data-ttu-id="fa0a4-154">La plantilla de Visual Studio crea un proyecto de inicio:</span><span class="sxs-lookup"><span data-stu-id="fa0a4-154">The Visual Studio template creates a starter project:</span></span>

![Explorador de soluciones](razor-pages-start/_static/se.png)

<span data-ttu-id="fa0a4-156">Presione **F5** para ejecutar la aplicación en modo de depuración o **Ctrl-F5** para que se ejecute sin adjuntar el depurador.</span><span class="sxs-lookup"><span data-stu-id="fa0a4-156">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger</span></span>

![Página Inicio o Índice](razor-pages-start/_static/home.png)

* <span data-ttu-id="fa0a4-158">Visual Studio inicia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) y ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="fa0a4-158">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="fa0a4-159">En la barra de direcciones aparece `localhost:port#` (y no algo como `example.com`).</span><span class="sxs-lookup"><span data-stu-id="fa0a4-159">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="fa0a4-160">Esto es así porque `localhost` es el nombre de host estándar del equipo local.</span><span class="sxs-lookup"><span data-stu-id="fa0a4-160">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="fa0a4-161">Localhost solo sirve las solicitudes web del equipo local.</span><span class="sxs-lookup"><span data-stu-id="fa0a4-161">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="fa0a4-162">Cuando Visual Studio crea un proyecto web, se usa un puerto aleatorio para el servidor web.</span><span class="sxs-lookup"><span data-stu-id="fa0a4-162">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="fa0a4-163">En la imagen anterior, el número de puerto es 5000.</span><span class="sxs-lookup"><span data-stu-id="fa0a4-163">In the preceding image, the port number is 5000.</span></span> <span data-ttu-id="fa0a4-164">Al ejecutar la aplicación verá otro puerto distinto.</span><span class="sxs-lookup"><span data-stu-id="fa0a4-164">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="fa0a4-165">Iniciar la aplicación con **CTRL+F5** (modo de no depuración) le permite efectuar cambios en el código, guardar el archivo, actualizar el explorador y ver los cambios de código.</span><span class="sxs-lookup"><span data-stu-id="fa0a4-165">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="fa0a4-166">Muchos desarrolladores prefieren usar el modo de no depuración para iniciar la aplicación rápidamente y ver los cambios.</span><span class="sxs-lookup"><span data-stu-id="fa0a4-166">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE [razor-pages-start](~/includes/RP/2.1/razor-pages-start.md)]

::: moniker-end

> [!div class="step-by-step"]
> [<span data-ttu-id="fa0a4-167">Siguiente: Adición de un modelo</span><span class="sxs-lookup"><span data-stu-id="fa0a4-167">Next: Adding a model</span></span>](xref:tutorials/razor-pages/model)
