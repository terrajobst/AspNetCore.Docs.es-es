---
title: "Introducción a las páginas de Razor en ASP.NET Core en Mac"
author: rick-anderson
description: "Introducción a las páginas de Razor en ASP.NET Core con Visual Studio para Mac"
keywords: "ASP.NET Core,páginas Razor,scaffolding,Entity Framework Core,EF,EF Core,base de datos,mac,macOS,Visual Studio para Mac"
ms.author: riande
manager: wpickett
ms.date: 7/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages-mac/razor-pages-start
ms.openlocfilehash: caadc3fcb3bb71abe0773aed4f6ff60a043e3a02
ms.sourcegitcommit: d9ec19e5452af83648074db5d96c0a0f4f9e7f9a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/29/2017
---
# <a name="getting-started-with-razor-pages-in-aspnet-core-with-visual-studio-for-mac"></a><span data-ttu-id="d5999-104">Introducción a las páginas de Razor en ASP.NET Core con Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="d5999-104">Getting started with Razor Pages in ASP.NET Core with Visual Studio for Mac</span></span>

<span data-ttu-id="d5999-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d5999-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="d5999-106">En este tutorial se enseñan los conceptos básicos de la creación de una aplicación web de las páginas Razor de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d5999-106">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="d5999-107">Se recomienda completar [Introducción a las páginas Razor](xref:mvc/razor-pages/index) antes de iniciar este tutorial.</span><span class="sxs-lookup"><span data-stu-id="d5999-107">We recommend you complete [Introduction to Razor Pages](xref:mvc/razor-pages/index) before starting this tutorial.</span></span> <span data-ttu-id="d5999-108">Las páginas de Razor son el método recomendado para crear la interfaz de usuario de aplicaciones web en ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d5999-108">Razor Pages is the recommended way to build UI for web applications in ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d5999-109">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="d5999-109">Prerequisites</span></span>

<span data-ttu-id="d5999-110">Instale los elementos siguientes:</span><span class="sxs-lookup"><span data-stu-id="d5999-110">Install the following:</span></span>

* <span data-ttu-id="d5999-111">[SDK de .NET Core 2.0.0](https://dot.net/core) o versiones posteriores</span><span class="sxs-lookup"><span data-stu-id="d5999-111">[.NET Core 2.0.0 SDK](https://dot.net/core) or later</span></span>
* [<span data-ttu-id="d5999-112">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="d5999-112">Visual Studio for Mac</span></span>](https://www.visualstudio.com/vs/visual-studio-mac/)

## <a name="create-a-razor-web-app"></a><span data-ttu-id="d5999-113">Crear una aplicación web de Razor</span><span class="sxs-lookup"><span data-stu-id="d5999-113">Create a Razor web app</span></span>

<span data-ttu-id="d5999-114">Desde un terminal, ejecute los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="d5999-114">From a terminal, run the following commands:</span></span>

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

<span data-ttu-id="d5999-115">Los comandos anteriores usan el [CLI de .NET Core](https://docs.microsoft.com/dotnet/core/tools/dotnet) para crear y ejecutar un proyecto de páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="d5999-115">The preceding commands use the [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) to create and run a Razor Pages project.</span></span> <span data-ttu-id="d5999-116">Abra http://localhost:5000 en un explorador para ver la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d5999-116">Open a browser to http://localhost:5000 to view the application.</span></span>

![Página principal o de índice](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a><span data-ttu-id="d5999-118">Abrir el proyecto</span><span class="sxs-lookup"><span data-stu-id="d5999-118">Open the project</span></span>

<span data-ttu-id="d5999-119">Presione CTRL+C para cerrar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d5999-119">Press Ctrl+C to shut down the application.</span></span>

<span data-ttu-id="d5999-120">En Visual Studio, seleccione **Archivo > Abrir** y elija el archivo *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="d5999-120">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="d5999-121">Iniciar la aplicación</span><span class="sxs-lookup"><span data-stu-id="d5999-121">Launch the app</span></span>

<span data-ttu-id="d5999-122">En Visual Studio, seleccione **Ejecutar > Iniciar sin depurar** para iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d5999-122">In Visual Studio, select **Run > Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="d5999-123">Visual Studio iniciará [IIS Express](http://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) y un explorador y se desplazará a `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="d5999-123">Visual Studio starts [IIS Express](http://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview), launches a browser, and navigates to `http://localhost:5000`.</span></span>

<span data-ttu-id="d5999-124">En el tutorial siguiente, agregaremos un modelo al proyecto.</span><span class="sxs-lookup"><span data-stu-id="d5999-124">In the next tutorial, we add a model to the project.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="d5999-125">Siguiente: Agregar un modelo</span><span class="sxs-lookup"><span data-stu-id="d5999-125">Next: Adding a model</span></span>](xref:tutorials/razor-pages-mac/model)
