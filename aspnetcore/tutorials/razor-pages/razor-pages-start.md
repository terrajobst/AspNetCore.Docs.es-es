---
title: Introducción a las páginas de Razor en ASP.NET Core
author: rick-anderson
description: Conozca los conceptos básicos de la compilación de una aplicación web de páginas de Razor de ASP.NET Core. Se recomienda usar páginas de Razor en las cargas de trabajo web en ASP.NET Core.
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: e317b49f2ad33c392de33bc32a87f67bb8cb72a0
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278049"
---
# <a name="get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="75f44-104">Introducción a las páginas de Razor en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="75f44-104">Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="75f44-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="75f44-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="75f44-106">Se recomienda seguir la versión de ASP.NET Core 2.1 de este tutorial,</span><span class="sxs-lookup"><span data-stu-id="75f44-106">We recommend you follow the ASP.NET Core 2.1 version of this tutorial.</span></span> <span data-ttu-id="75f44-107">ya que es **mucho** más fácil de seguir y abarca más características.</span><span class="sxs-lookup"><span data-stu-id="75f44-107">It's **much** easier to follow and covers more features.</span></span> <span data-ttu-id="75f44-108">Seleccione **ASP.NET Core 2.1** en el selector de versión.</span><span class="sxs-lookup"><span data-stu-id="75f44-108">Select **ASP.NET Core 2.1** in the version selector.</span></span>

![Selector de versión en la TDC](razor-pages-start/_static/v21.png)

::: moniker-end

<span data-ttu-id="75f44-110">En este tutorial se enseñan los conceptos básicos de la compilación de una aplicación web de páginas de Razor de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="75f44-110">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="75f44-111">Las páginas de Razor son el método recomendado para crear la interfaz de usuario de aplicaciones web en ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="75f44-111">Razor Pages is the recommended way to build UI for web apps in ASP.NET Core.</span></span>

<span data-ttu-id="75f44-112">Hay tres versiones de este tutorial:</span><span class="sxs-lookup"><span data-stu-id="75f44-112">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="75f44-113">Windows: este tutorial</span><span class="sxs-lookup"><span data-stu-id="75f44-113">Windows: This tutorial</span></span>
* <span data-ttu-id="75f44-114">MacOS: [Introducción a las páginas de Razor en ASP.NET Core con Visual Studio para Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="75f44-114">MacOS: [Get started with Razor Pages with Visual Studio for Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span></span>
* <span data-ttu-id="75f44-115">macOS, Linux y Windows: [Introducción a las páginas de Razor en ASP.NET Core con Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="75f44-115">macOS, Linux, and Windows: [Get started with ASP.NET Core Razor Pages in Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)</span></span>

<span data-ttu-id="75f44-116">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="75f44-116">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="prerequisites"></a><span data-ttu-id="75f44-117">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="75f44-117">Prerequisites</span></span>

<span data-ttu-id="75f44-118">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span><span class="sxs-lookup"><span data-stu-id="75f44-118">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span></span>

## <a name="create-a-razor-web-app"></a><span data-ttu-id="75f44-119">Creación de una aplicación web de Razor</span><span class="sxs-lookup"><span data-stu-id="75f44-119">Create a Razor web app</span></span>

* <span data-ttu-id="75f44-120">En el menú **Archivo** de Visual Studio, seleccione **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="75f44-120">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="75f44-121">Cree una aplicación web de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="75f44-121">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="75f44-122">Asigne al proyecto el nombre **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="75f44-122">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="75f44-123">Es importante asignarle el nombre *RazorPagesMovie* para que los espacios de nombres coincidan al copiar y pegar el código.</span><span class="sxs-lookup"><span data-stu-id="75f44-123">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
 <span data-ttu-id="75f44-124">![Nueva aplicación web de ASP.NET Core](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="75f44-124">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>
* <span data-ttu-id="75f44-125">Seleccione **ASP.NET Core 2.1** en la lista desplegable y, luego, **Aplicación web**.</span><span class="sxs-lookup"><span data-stu-id="75f44-125">Select **ASP.NET Core 2.1** in the dropdown, and then select **Web Application**.</span></span>

 ![Nueva aplicación web de ASP.NET Core](razor-pages-start/_static/np_2_2.1.png)

<span data-ttu-id="75f44-127">La plantilla de Visual Studio crea un proyecto de inicio:</span><span class="sxs-lookup"><span data-stu-id="75f44-127">The Visual Studio template creates a starter project:</span></span>

![Explorador de soluciones](razor-pages-start/_static/se2.1.png)

<span data-ttu-id="75f44-129">Presione **F5** para ejecutar la aplicación en modo de depuración o **Ctrl+F5** para que se ejecute sin adjuntar el depurador.</span><span class="sxs-lookup"><span data-stu-id="75f44-129">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger.</span></span> <span data-ttu-id="75f44-130">Seleccione **Aceptar** para dar su consentimiento al seguimiento.</span><span class="sxs-lookup"><span data-stu-id="75f44-130">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="75f44-131">Esta aplicación no lleva un seguimiento de la información personal.</span><span class="sxs-lookup"><span data-stu-id="75f44-131">This app doesn't track personal information.</span></span> <span data-ttu-id="75f44-132">El código generado con plantilla incluye activos que sirven para cumplir el [Reglamento general de protección de datos (RGPD)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="75f44-132">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

![Página Inicio o Índice](razor-pages-start/_static/homeGDPR.png)

<span data-ttu-id="75f44-134">En la siguiente imagen se muestra la aplicación tras haber aceptado el seguimiento:</span><span class="sxs-lookup"><span data-stu-id="75f44-134">The following image shows the app after accepting tracking:</span></span>

![Página Inicio o Índice](razor-pages-start/_static/home2.1.png)

* <span data-ttu-id="75f44-136">Visual Studio inicia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) y ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="75f44-136">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="75f44-137">En la barra de direcciones aparece `localhost:port#` (y no algo como `example.com`).</span><span class="sxs-lookup"><span data-stu-id="75f44-137">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="75f44-138">Esto es así porque `localhost` es el nombre de host estándar del equipo local.</span><span class="sxs-lookup"><span data-stu-id="75f44-138">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="75f44-139">Localhost solo sirve las solicitudes web del equipo local.</span><span class="sxs-lookup"><span data-stu-id="75f44-139">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="75f44-140">Cuando Visual Studio crea un proyecto web, se usa un puerto aleatorio para el servidor web.</span><span class="sxs-lookup"><span data-stu-id="75f44-140">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="75f44-141">En la imagen anterior, el número de puerto es 5000.</span><span class="sxs-lookup"><span data-stu-id="75f44-141">In the preceding image, the port number is 5000.</span></span> <span data-ttu-id="75f44-142">Al ejecutar la aplicación verá otro puerto distinto.</span><span class="sxs-lookup"><span data-stu-id="75f44-142">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="75f44-143">Iniciar la aplicación con **CTRL+F5** (modo de no depuración) le permite efectuar cambios en el código, guardar el archivo, actualizar el explorador y ver los cambios de código.</span><span class="sxs-lookup"><span data-stu-id="75f44-143">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="75f44-144">Muchos desarrolladores prefieren usar el modo de no depuración para iniciar la aplicación rápidamente y ver los cambios.</span><span class="sxs-lookup"><span data-stu-id="75f44-144">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE [razor-pages-start](~/includes/RP/2.1/razor-pages-start.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

## <a name="prerequisites"></a><span data-ttu-id="75f44-145">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="75f44-145">Prerequisites</span></span>

<span data-ttu-id="75f44-146">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span><span class="sxs-lookup"><span data-stu-id="75f44-146">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span></span>

## <a name="create-a-razor-web-app"></a><span data-ttu-id="75f44-147">Creación de una aplicación web de Razor</span><span class="sxs-lookup"><span data-stu-id="75f44-147">Create a Razor web app</span></span>

* <span data-ttu-id="75f44-148">En el menú **Archivo** de Visual Studio, seleccione **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="75f44-148">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="75f44-149">Cree una aplicación web de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="75f44-149">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="75f44-150">Asigne al proyecto el nombre **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="75f44-150">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="75f44-151">Es importante asignarle el nombre *RazorPagesMovie* para que los espacios de nombres coincidan al copiar y pegar el código.</span><span class="sxs-lookup"><span data-stu-id="75f44-151">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
  <span data-ttu-id="75f44-152">![Nueva aplicación web de ASP.NET Core](../../razor-pages/index/_static/np.png)</span><span class="sxs-lookup"><span data-stu-id="75f44-152">![new ASP.NET Core Web Application](../../razor-pages/index/_static/np.png)</span></span>
* <span data-ttu-id="75f44-153">Seleccione **ASP.NET Core 2.0** en la lista desplegable y, luego, seleccione **Aplicación web**.</span><span class="sxs-lookup"><span data-stu-id="75f44-153">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span>

  [!INCLUDE [install 2.0](~/includes/dotnetcore-on-dotnetfx-vs.md)]

<span data-ttu-id="75f44-154">La plantilla de Visual Studio crea un proyecto de inicio:</span><span class="sxs-lookup"><span data-stu-id="75f44-154">The Visual Studio template creates a starter project:</span></span>

![Explorador de soluciones](razor-pages-start/_static/se.png)

<span data-ttu-id="75f44-156">Presione **F5** para ejecutar la aplicación en modo de depuración o **Ctrl-F5** para que se ejecute sin adjuntar el depurador.</span><span class="sxs-lookup"><span data-stu-id="75f44-156">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger</span></span>

![Página Inicio o Índice](razor-pages-start/_static/home.png)

* <span data-ttu-id="75f44-158">Visual Studio inicia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) y ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="75f44-158">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="75f44-159">En la barra de direcciones aparece `localhost:port#` (y no algo como `example.com`).</span><span class="sxs-lookup"><span data-stu-id="75f44-159">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="75f44-160">Esto es así porque `localhost` es el nombre de host estándar del equipo local.</span><span class="sxs-lookup"><span data-stu-id="75f44-160">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="75f44-161">Localhost solo sirve las solicitudes web del equipo local.</span><span class="sxs-lookup"><span data-stu-id="75f44-161">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="75f44-162">Cuando Visual Studio crea un proyecto web, se usa un puerto aleatorio para el servidor web.</span><span class="sxs-lookup"><span data-stu-id="75f44-162">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="75f44-163">En la imagen anterior, el número de puerto es 5000.</span><span class="sxs-lookup"><span data-stu-id="75f44-163">In the preceding image, the port number is 5000.</span></span> <span data-ttu-id="75f44-164">Al ejecutar la aplicación verá otro puerto distinto.</span><span class="sxs-lookup"><span data-stu-id="75f44-164">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="75f44-165">Iniciar la aplicación con **CTRL+F5** (modo de no depuración) le permite efectuar cambios en el código, guardar el archivo, actualizar el explorador y ver los cambios de código.</span><span class="sxs-lookup"><span data-stu-id="75f44-165">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="75f44-166">Muchos desarrolladores prefieren usar el modo de no depuración para iniciar la aplicación rápidamente y ver los cambios.</span><span class="sxs-lookup"><span data-stu-id="75f44-166">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE [razor-pages-start](~/includes/RP/2.1/razor-pages-start.md)]

::: moniker-end

> [!div class="step-by-step"]
> [<span data-ttu-id="75f44-167">Siguiente: Adición de un modelo</span><span class="sxs-lookup"><span data-stu-id="75f44-167">Next: Adding a model</span></span>](xref:tutorials/razor-pages/model)
