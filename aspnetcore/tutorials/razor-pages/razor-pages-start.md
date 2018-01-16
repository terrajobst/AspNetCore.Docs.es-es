---
title: "Introducción a las páginas de Razor en ASP.NET Core"
author: rick-anderson
description: "Introducción a las páginas de Razor en ASP.NET Core"
keywords: "ASP.NET Core, páginas de Razor, Razor, MVC"
ms.author: riande
manager: wpickett
ms.date: 12/22/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 4643665d48ca07ff43ce52064291fc106bd5c8ac
ms.sourcegitcommit: df2157ae9aeea0075772719c29784425c783e82a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/10/2018
---
# <a name="get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="c7703-104">Introducción a las páginas de Razor en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c7703-104">Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="c7703-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c7703-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c7703-106">En este tutorial se enseñan los conceptos básicos de la compilación de una aplicación web de páginas de Razor de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c7703-106">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="c7703-107">Las páginas de Razor son el método recomendado para crear la interfaz de usuario de aplicaciones web en ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c7703-107">Razor Pages is the recommended way to build UI for web apps in ASP.NET Core.</span></span>

<span data-ttu-id="c7703-108">Hay tres versiones de este tutorial:</span><span class="sxs-lookup"><span data-stu-id="c7703-108">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="c7703-109">Windows: este tutorial</span><span class="sxs-lookup"><span data-stu-id="c7703-109">Windows: This tutorial</span></span>
* <span data-ttu-id="c7703-110">MacOS: [Introducción a las páginas de Razor en ASP.NET Core con Visual Studio para Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="c7703-110">MacOS: [Getting started with Razor Pages with Visual Studio for Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span></span>
* <span data-ttu-id="c7703-111">macOS, Linux y Windows: [Introducción a las páginas de Razor en ASP.NET Core con Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="c7703-111">macOS, Linux, and Windows: [Getting started with Razor Pages in ASP.NET Core with Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)</span></span>

<span data-ttu-id="c7703-112">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c7703-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c7703-113">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="c7703-113">Prerequisites</span></span>

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="c7703-114">Creación de una aplicación web de Razor</span><span class="sxs-lookup"><span data-stu-id="c7703-114">Create a Razor web app</span></span>

* <span data-ttu-id="c7703-115">En el menú **Archivo** de Visual Studio, seleccione **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="c7703-115">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="c7703-116">Cree una aplicación web de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c7703-116">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="c7703-117">Asigne al proyecto el nombre **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="c7703-117">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="c7703-118">Es importante asignarle el nombre *RazorPagesMovie* para que los espacios de nombres coincidan al copiar y pegar el código.</span><span class="sxs-lookup"><span data-stu-id="c7703-118">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
  <span data-ttu-id="c7703-119">![Nueva aplicación web de ASP.NET Core](../../mvc/razor-pages/index/_static/np.png)</span><span class="sxs-lookup"><span data-stu-id="c7703-119">![new ASP.NET Core Web Application](../../mvc/razor-pages/index/_static/np.png)</span></span>
* <span data-ttu-id="c7703-120">Seleccione **ASP.NET Core 2.0** en la lista desplegable y, luego, seleccione **Aplicación web**.</span><span class="sxs-lookup"><span data-stu-id="c7703-120">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span>

  [!INCLUDE[install 2.0](../../includes/dotnetcore-on-dotnetfx-vs.md)]

<span data-ttu-id="c7703-121">La plantilla de Visual Studio crea un proyecto de inicio:</span><span class="sxs-lookup"><span data-stu-id="c7703-121">The Visual Studio template creates a starter project:</span></span>

![Explorador de soluciones](razor-pages-start/_static/se.png)

<span data-ttu-id="c7703-123">Presione **F5** para ejecutar la aplicación en modo de depuración o **Ctrl-F5** para que se ejecute sin adjuntar el depurador.</span><span class="sxs-lookup"><span data-stu-id="c7703-123">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger</span></span>

![Página Inicio o Índice](razor-pages-start/_static/home.png)

* <span data-ttu-id="c7703-125">Visual Studio inicia [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) y ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c7703-125">Visual Studio starts [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="c7703-126">En la barra de direcciones aparece `localhost:port#` (y no algo como `example.com`).</span><span class="sxs-lookup"><span data-stu-id="c7703-126">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="c7703-127">Esto es así porque `localhost` es el nombre de host estándar del equipo local.</span><span class="sxs-lookup"><span data-stu-id="c7703-127">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="c7703-128">Localhost solo sirve las solicitudes web del equipo local.</span><span class="sxs-lookup"><span data-stu-id="c7703-128">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="c7703-129">Cuando Visual Studio crea un proyecto web, se usa un puerto aleatorio para el servidor web.</span><span class="sxs-lookup"><span data-stu-id="c7703-129">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="c7703-130">En la imagen anterior, el número de puerto es 5000.</span><span class="sxs-lookup"><span data-stu-id="c7703-130">In the preceding image, the port number is 5000.</span></span> <span data-ttu-id="c7703-131">Al ejecutar la aplicación verá otro puerto distinto.</span><span class="sxs-lookup"><span data-stu-id="c7703-131">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="c7703-132">Iniciar la aplicación con **CTRL+F5** (modo de no depuración) le permite efectuar cambios en el código, guardar el archivo, actualizar el explorador y ver los cambios de código.</span><span class="sxs-lookup"><span data-stu-id="c7703-132">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="c7703-133">Muchos desarrolladores prefieren usar el modo de no depuración para iniciar la aplicación rápidamente y ver los cambios.</span><span class="sxs-lookup"><span data-stu-id="c7703-133">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

>[!div class="step-by-step"]
[<span data-ttu-id="c7703-134">Siguiente: Adición de un modelo</span><span class="sxs-lookup"><span data-stu-id="c7703-134">Next: Adding a model</span></span>](xref:tutorials/razor-pages/model)

>[!div class="step-by-step"]
[<span data-ttu-id="c7703-135">Siguiente: Adición de un modelo</span><span class="sxs-lookup"><span data-stu-id="c7703-135">Next: Adding a model</span></span>](xref:tutorials/razor-pages/model)
