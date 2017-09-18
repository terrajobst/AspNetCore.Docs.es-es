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
ms.openlocfilehash: 56ff18589d189b0d2760c761c58b5b030d02940b
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2017
---
# <a name="getting-started-with-razor-pages-in-aspnet-core-with-visual-studio-for-mac"></a><span data-ttu-id="165bd-104">Introducción a las páginas de Razor en ASP.NET Core con Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="165bd-104">Getting started with Razor Pages in ASP.NET Core with Visual Studio for Mac</span></span>

<span data-ttu-id="165bd-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="165bd-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="165bd-106">En este tutorial se enseñan los conceptos básicos de la compilación de una aplicación web de páginas de Razor de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="165bd-106">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="165bd-107">Se recomienda leer [Introduction to Razor Pages](xref:mvc/razor-pages/index) (Introducción a las páginas de Razor) antes de empezar este tutorial.</span><span class="sxs-lookup"><span data-stu-id="165bd-107">We recommend you complete [Introduction to Razor Pages](xref:mvc/razor-pages/index) before starting this tutorial.</span></span> <span data-ttu-id="165bd-108">Las páginas de Razor son el método recomendado para crear la interfaz de usuario de aplicaciones web en ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="165bd-108">Razor Pages is the recommended way to build UI for web applications in ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="165bd-109">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="165bd-109">Prerequisites</span></span>

<span data-ttu-id="165bd-110">Instale el software siguiente:</span><span class="sxs-lookup"><span data-stu-id="165bd-110">Install the following:</span></span>

* <span data-ttu-id="165bd-111">[SDK de .NET Core 2.0.0](https://www.microsoft.com/net/core) o versiones posteriores</span><span class="sxs-lookup"><span data-stu-id="165bd-111">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later</span></span>
* [<span data-ttu-id="165bd-112">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="165bd-112">Visual Studio for Mac</span></span>](https://www.visualstudio.com/vs/visual-studio-mac/)

## <a name="create-a-razor-web-app"></a><span data-ttu-id="165bd-113">Creación de una aplicación web de Razor</span><span class="sxs-lookup"><span data-stu-id="165bd-113">Create a Razor web app</span></span>

<span data-ttu-id="165bd-114">Desde un terminal, ejecute estos comandos:</span><span class="sxs-lookup"><span data-stu-id="165bd-114">From a terminal, run the following commands:</span></span>

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

<span data-ttu-id="165bd-115">Los comandos anteriores usan el [CLI de .NET Core](https://docs.microsoft.com/dotnet/core/tools/dotnet) para crear y ejecutar un proyecto de páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="165bd-115">The preceding commands use the [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) to create and run a Razor Pages project.</span></span> <span data-ttu-id="165bd-116">Abra http://localhost:5000 en un explorador para ver la aplicación.</span><span class="sxs-lookup"><span data-stu-id="165bd-116">Open a browser to http://localhost:5000 to view the application.</span></span>

![Página principal o de índice](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a><span data-ttu-id="165bd-118">Abrir el proyecto</span><span class="sxs-lookup"><span data-stu-id="165bd-118">Open the project</span></span>

<span data-ttu-id="165bd-119">Presione CTRL+C para cerrar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="165bd-119">Press Ctrl+C to shut down the application.</span></span>

<span data-ttu-id="165bd-120">En Visual Studio, seleccione **Archivo > Abrir** y elija el archivo *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="165bd-120">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="165bd-121">Iniciar la aplicación</span><span class="sxs-lookup"><span data-stu-id="165bd-121">Launch the app</span></span>

<span data-ttu-id="165bd-122">En Visual Studio, seleccione **Ejecutar > Iniciar sin depurar** para iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="165bd-122">In Visual Studio, select **Run > Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="165bd-123">Visual Studio iniciará [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) y un explorador y se desplazará a `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="165bd-123">Visual Studio starts [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview), launches a browser, and navigates to `http://localhost:5000`.</span></span>

<span data-ttu-id="165bd-124">En el tutorial siguiente, agregaremos un modelo al proyecto.</span><span class="sxs-lookup"><span data-stu-id="165bd-124">In the next tutorial, we add a model to the project.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="165bd-125">Siguiente: Adición de un modelo</span><span class="sxs-lookup"><span data-stu-id="165bd-125">Next: Adding a model</span></span>](xref:tutorials/razor-pages-mac/model)
