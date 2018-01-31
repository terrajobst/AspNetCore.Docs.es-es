---
title: "Introducción a las páginas de Razor en ASP.NET Core"
author: rick-anderson
description: "Introducción a las páginas de Razor en ASP.NET Core"
manager: wpickett
ms.author: riande
ms.date: 12/22/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 54fa970d136de3903ae08b710b55f15f96f9a012
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
# <a name="get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="66bec-103">Introducción a las páginas de Razor en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="66bec-103">Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="66bec-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="66bec-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="66bec-105">En este tutorial se enseñan los conceptos básicos de la compilación de una aplicación web de páginas de Razor de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="66bec-105">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="66bec-106">Las páginas de Razor son el método recomendado para crear la interfaz de usuario de aplicaciones web en ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="66bec-106">Razor Pages is the recommended way to build UI for web apps in ASP.NET Core.</span></span>

<span data-ttu-id="66bec-107">Hay tres versiones de este tutorial:</span><span class="sxs-lookup"><span data-stu-id="66bec-107">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="66bec-108">Windows: este tutorial</span><span class="sxs-lookup"><span data-stu-id="66bec-108">Windows: This tutorial</span></span>
* <span data-ttu-id="66bec-109">MacOS: [Introducción a las páginas de Razor en ASP.NET Core con Visual Studio para Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="66bec-109">MacOS: [Getting started with Razor Pages with Visual Studio for Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span></span>
* <span data-ttu-id="66bec-110">macOS, Linux y Windows: [Introducción a las páginas de Razor en ASP.NET Core con Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="66bec-110">macOS, Linux, and Windows: [Getting started with Razor Pages in ASP.NET Core with Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)</span></span>

<span data-ttu-id="66bec-111">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="66bec-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="66bec-112">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="66bec-112">Prerequisites</span></span>

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="66bec-113">Creación de una aplicación web de Razor</span><span class="sxs-lookup"><span data-stu-id="66bec-113">Create a Razor web app</span></span>

* <span data-ttu-id="66bec-114">En el menú **Archivo** de Visual Studio, seleccione **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="66bec-114">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="66bec-115">Cree una aplicación web de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="66bec-115">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="66bec-116">Asigne al proyecto el nombre **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="66bec-116">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="66bec-117">Es importante asignarle el nombre *RazorPagesMovie* para que los espacios de nombres coincidan al copiar y pegar el código.</span><span class="sxs-lookup"><span data-stu-id="66bec-117">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
  <span data-ttu-id="66bec-118">![Nueva aplicación web de ASP.NET Core](../../mvc/razor-pages/index/_static/np.png)</span><span class="sxs-lookup"><span data-stu-id="66bec-118">![new ASP.NET Core Web Application](../../mvc/razor-pages/index/_static/np.png)</span></span>
* <span data-ttu-id="66bec-119">Seleccione **ASP.NET Core 2.0** en la lista desplegable y, luego, seleccione **Aplicación web**.</span><span class="sxs-lookup"><span data-stu-id="66bec-119">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span>

  [!INCLUDE[install 2.0](../../includes/dotnetcore-on-dotnetfx-vs.md)]

<span data-ttu-id="66bec-120">La plantilla de Visual Studio crea un proyecto de inicio:</span><span class="sxs-lookup"><span data-stu-id="66bec-120">The Visual Studio template creates a starter project:</span></span>

![Explorador de soluciones](razor-pages-start/_static/se.png)

<span data-ttu-id="66bec-122">Presione **F5** para ejecutar la aplicación en modo de depuración o **Ctrl-F5** para que se ejecute sin adjuntar el depurador.</span><span class="sxs-lookup"><span data-stu-id="66bec-122">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger</span></span>

![Página Inicio o Índice](razor-pages-start/_static/home.png)

* <span data-ttu-id="66bec-124">Visual Studio inicia [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) y ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="66bec-124">Visual Studio starts [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="66bec-125">En la barra de direcciones aparece `localhost:port#` (y no algo como `example.com`).</span><span class="sxs-lookup"><span data-stu-id="66bec-125">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="66bec-126">Esto es así porque `localhost` es el nombre de host estándar del equipo local.</span><span class="sxs-lookup"><span data-stu-id="66bec-126">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="66bec-127">Localhost solo sirve las solicitudes web del equipo local.</span><span class="sxs-lookup"><span data-stu-id="66bec-127">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="66bec-128">Cuando Visual Studio crea un proyecto web, se usa un puerto aleatorio para el servidor web.</span><span class="sxs-lookup"><span data-stu-id="66bec-128">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="66bec-129">En la imagen anterior, el número de puerto es 5000.</span><span class="sxs-lookup"><span data-stu-id="66bec-129">In the preceding image, the port number is 5000.</span></span> <span data-ttu-id="66bec-130">Al ejecutar la aplicación verá otro puerto distinto.</span><span class="sxs-lookup"><span data-stu-id="66bec-130">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="66bec-131">Iniciar la aplicación con **CTRL+F5** (modo de no depuración) le permite efectuar cambios en el código, guardar el archivo, actualizar el explorador y ver los cambios de código.</span><span class="sxs-lookup"><span data-stu-id="66bec-131">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="66bec-132">Muchos desarrolladores prefieren usar el modo de no depuración para iniciar la aplicación rápidamente y ver los cambios.</span><span class="sxs-lookup"><span data-stu-id="66bec-132">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

>[!div class="step-by-step"]
[<span data-ttu-id="66bec-133">Siguiente: Adición de un modelo</span><span class="sxs-lookup"><span data-stu-id="66bec-133">Next: Adding a model</span></span>](xref:tutorials/razor-pages/model)

>[!div class="step-by-step"]
[<span data-ttu-id="66bec-134">Siguiente: Adición de un modelo</span><span class="sxs-lookup"><span data-stu-id="66bec-134">Next: Adding a model</span></span>](xref:tutorials/razor-pages/model)
