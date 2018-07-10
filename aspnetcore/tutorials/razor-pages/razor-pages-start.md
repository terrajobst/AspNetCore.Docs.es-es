---
title: Introducción a las páginas de Razor en ASP.NET Core
author: rick-anderson
description: Conozca los conceptos básicos de la compilación de una aplicación web de páginas de Razor de ASP.NET Core. Se recomienda usar páginas de Razor en las cargas de trabajo web en ASP.NET Core.
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 10e9174b0bf094f7a4ea820a5afcf2705f900327
ms.sourcegitcommit: 18339e3cb5a891a3ca36d8146fa83cf91c32e707
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37433911"
---
# <a name="get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="d8a56-104">Introducción a las páginas de Razor en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d8a56-104">Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="d8a56-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d8a56-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="d8a56-106">Se recomienda seguir la versión de ASP.NET Core 2.1 de este tutorial,</span><span class="sxs-lookup"><span data-stu-id="d8a56-106">We recommend you follow the ASP.NET Core 2.1 version of this tutorial.</span></span> <span data-ttu-id="d8a56-107">ya que es **mucho** más fácil de seguir y abarca más características.</span><span class="sxs-lookup"><span data-stu-id="d8a56-107">It's **much** easier to follow and covers more features.</span></span> <span data-ttu-id="d8a56-108">Seleccione **ASP.NET Core 2.1** en el selector de versión.</span><span class="sxs-lookup"><span data-stu-id="d8a56-108">Select **ASP.NET Core 2.1** in the version selector.</span></span>

![Selector de versión en la TDC](razor-pages-start/_static/v21.png)

::: moniker-end

<span data-ttu-id="d8a56-110">En este tutorial se enseñan los conceptos básicos de la compilación de una aplicación web de páginas de Razor de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d8a56-110">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="d8a56-111">Las páginas de Razor son el método recomendado para crear la interfaz de usuario de aplicaciones web en ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d8a56-111">Razor Pages is the recommended way to build UI for web apps in ASP.NET Core.</span></span>

<span data-ttu-id="d8a56-112">Hay tres versiones de este tutorial:</span><span class="sxs-lookup"><span data-stu-id="d8a56-112">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="d8a56-113">Windows: este tutorial</span><span class="sxs-lookup"><span data-stu-id="d8a56-113">Windows: This tutorial</span></span>
* <span data-ttu-id="d8a56-114">MacOS: [Introducción a las páginas de Razor en ASP.NET Core con Visual Studio para Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="d8a56-114">MacOS: [Get started with Razor Pages with Visual Studio for Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span></span>
* <span data-ttu-id="d8a56-115">macOS, Linux y Windows: [Introducción a las páginas de Razor en ASP.NET Core con Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="d8a56-115">macOS, Linux, and Windows: [Get started with ASP.NET Core Razor Pages in Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)</span></span>

<span data-ttu-id="d8a56-116">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d8a56-116">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="prerequisites"></a><span data-ttu-id="d8a56-117">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="d8a56-117">Prerequisites</span></span>

[!INCLUDE [Prerequisites](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="d8a56-118">Creación de una aplicación web de Razor</span><span class="sxs-lookup"><span data-stu-id="d8a56-118">Create a Razor web app</span></span>

* <span data-ttu-id="d8a56-119">En el menú **Archivo** de Visual Studio, seleccione **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="d8a56-119">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="d8a56-120">Cree una aplicación web de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d8a56-120">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="d8a56-121">Asigne al proyecto el nombre **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="d8a56-121">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="d8a56-122">Es importante asignarle el nombre *RazorPagesMovie* para que los espacios de nombres coincidan al copiar y pegar el código.</span><span class="sxs-lookup"><span data-stu-id="d8a56-122">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
 <span data-ttu-id="d8a56-123">![Nueva aplicación web de ASP.NET Core](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="d8a56-123">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>
* <span data-ttu-id="d8a56-124">Seleccione **ASP.NET Core 2.1** en la lista desplegable y, luego, **Aplicación web**.</span><span class="sxs-lookup"><span data-stu-id="d8a56-124">Select **ASP.NET Core 2.1** in the dropdown, and then select **Web Application**.</span></span>

 ![Nueva aplicación web de ASP.NET Core](razor-pages-start/_static/np_2_2.1.png)

<span data-ttu-id="d8a56-126">La plantilla de Visual Studio crea un proyecto de inicio:</span><span class="sxs-lookup"><span data-stu-id="d8a56-126">The Visual Studio template creates a starter project:</span></span>

![Explorador de soluciones](razor-pages-start/_static/se2.1.png)

<span data-ttu-id="d8a56-128">Presione **F5** para ejecutar la aplicación en modo de depuración o **Ctrl+F5** para que se ejecute sin adjuntar el depurador.</span><span class="sxs-lookup"><span data-stu-id="d8a56-128">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger.</span></span> <span data-ttu-id="d8a56-129">Seleccione **Aceptar** para dar su consentimiento al seguimiento.</span><span class="sxs-lookup"><span data-stu-id="d8a56-129">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="d8a56-130">Esta aplicación no lleva un seguimiento de la información personal.</span><span class="sxs-lookup"><span data-stu-id="d8a56-130">This app doesn't track personal information.</span></span> <span data-ttu-id="d8a56-131">El código generado con plantilla incluye activos que sirven para cumplir el [Reglamento general de protección de datos (RGPD)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="d8a56-131">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

![Página Inicio o Índice](razor-pages-start/_static/homeGDPR.png)

<span data-ttu-id="d8a56-133">En la siguiente imagen se muestra la aplicación tras haber aceptado el seguimiento:</span><span class="sxs-lookup"><span data-stu-id="d8a56-133">The following image shows the app after accepting tracking:</span></span>

![Página Inicio o Índice](razor-pages-start/_static/home2.1.png)

* <span data-ttu-id="d8a56-135">Visual Studio inicia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) y ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d8a56-135">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="d8a56-136">En la barra de direcciones aparece `localhost:port#` (y no algo como `example.com`).</span><span class="sxs-lookup"><span data-stu-id="d8a56-136">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="d8a56-137">Esto es así porque `localhost` es el nombre de host estándar del equipo local.</span><span class="sxs-lookup"><span data-stu-id="d8a56-137">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="d8a56-138">Localhost solo sirve las solicitudes web del equipo local.</span><span class="sxs-lookup"><span data-stu-id="d8a56-138">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="d8a56-139">Cuando Visual Studio crea un proyecto web, se usa un puerto aleatorio para el servidor web.</span><span class="sxs-lookup"><span data-stu-id="d8a56-139">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="d8a56-140">En la imagen anterior, el número de puerto es 5000.</span><span class="sxs-lookup"><span data-stu-id="d8a56-140">In the preceding image, the port number is 5000.</span></span> <span data-ttu-id="d8a56-141">Al ejecutar la aplicación verá otro puerto distinto.</span><span class="sxs-lookup"><span data-stu-id="d8a56-141">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="d8a56-142">Iniciar la aplicación con **CTRL+F5** (modo de no depuración) le permite efectuar cambios en el código, guardar el archivo, actualizar el explorador y ver los cambios de código.</span><span class="sxs-lookup"><span data-stu-id="d8a56-142">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="d8a56-143">Muchos desarrolladores prefieren usar el modo de no depuración para iniciar la aplicación rápidamente y ver los cambios.</span><span class="sxs-lookup"><span data-stu-id="d8a56-143">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE [razor-pages-start](~/includes/RP/2.1/razor-pages-start.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

## <a name="prerequisites"></a><span data-ttu-id="d8a56-144">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="d8a56-144">Prerequisites</span></span>

[!INCLUDE [Prerequisites](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="d8a56-145">Creación de una aplicación web de Razor</span><span class="sxs-lookup"><span data-stu-id="d8a56-145">Create a Razor web app</span></span>

* <span data-ttu-id="d8a56-146">En el menú **Archivo** de Visual Studio, seleccione **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="d8a56-146">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="d8a56-147">Cree una aplicación web de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d8a56-147">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="d8a56-148">Asigne al proyecto el nombre **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="d8a56-148">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="d8a56-149">Es importante asignarle el nombre *RazorPagesMovie* para que los espacios de nombres coincidan al copiar y pegar el código.</span><span class="sxs-lookup"><span data-stu-id="d8a56-149">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
  <span data-ttu-id="d8a56-150">![Nueva aplicación web de ASP.NET Core](../../razor-pages/index/_static/np.png)</span><span class="sxs-lookup"><span data-stu-id="d8a56-150">![new ASP.NET Core Web Application](../../razor-pages/index/_static/np.png)</span></span>
* <span data-ttu-id="d8a56-151">Seleccione **ASP.NET Core 2.0** en la lista desplegable y, luego, seleccione **Aplicación web**.</span><span class="sxs-lookup"><span data-stu-id="d8a56-151">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span>

  [!INCLUDE [install 2.0](~/includes/dotnetcore-on-dotnetfx-vs.md)]

<span data-ttu-id="d8a56-152">La plantilla de Visual Studio crea un proyecto de inicio:</span><span class="sxs-lookup"><span data-stu-id="d8a56-152">The Visual Studio template creates a starter project:</span></span>

![Explorador de soluciones](razor-pages-start/_static/se.png)

<span data-ttu-id="d8a56-154">Presione **F5** para ejecutar la aplicación en modo de depuración o **Ctrl-F5** para que se ejecute sin adjuntar el depurador.</span><span class="sxs-lookup"><span data-stu-id="d8a56-154">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger</span></span>

![Página Inicio o Índice](razor-pages-start/_static/home.png)

* <span data-ttu-id="d8a56-156">Visual Studio inicia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) y ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d8a56-156">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="d8a56-157">En la barra de direcciones aparece `localhost:port#` (y no algo como `example.com`).</span><span class="sxs-lookup"><span data-stu-id="d8a56-157">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="d8a56-158">Esto es así porque `localhost` es el nombre de host estándar del equipo local.</span><span class="sxs-lookup"><span data-stu-id="d8a56-158">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="d8a56-159">Localhost solo sirve las solicitudes web del equipo local.</span><span class="sxs-lookup"><span data-stu-id="d8a56-159">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="d8a56-160">Cuando Visual Studio crea un proyecto web, se usa un puerto aleatorio para el servidor web.</span><span class="sxs-lookup"><span data-stu-id="d8a56-160">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="d8a56-161">En la imagen anterior, el número de puerto es 5000.</span><span class="sxs-lookup"><span data-stu-id="d8a56-161">In the preceding image, the port number is 5000.</span></span> <span data-ttu-id="d8a56-162">Al ejecutar la aplicación verá otro puerto distinto.</span><span class="sxs-lookup"><span data-stu-id="d8a56-162">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="d8a56-163">Iniciar la aplicación con **CTRL+F5** (modo de no depuración) le permite efectuar cambios en el código, guardar el archivo, actualizar el explorador y ver los cambios de código.</span><span class="sxs-lookup"><span data-stu-id="d8a56-163">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="d8a56-164">Muchos desarrolladores prefieren usar el modo de no depuración para iniciar la aplicación rápidamente y ver los cambios.</span><span class="sxs-lookup"><span data-stu-id="d8a56-164">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE [razor-pages-start](~/includes/RP/razor-pages-start.md)]

::: moniker-end

> [!div class="step-by-step"]
> [<span data-ttu-id="d8a56-165">Siguiente: Adición de un modelo</span><span class="sxs-lookup"><span data-stu-id="d8a56-165">Next: Adding a model</span></span>](xref:tutorials/razor-pages/model)
