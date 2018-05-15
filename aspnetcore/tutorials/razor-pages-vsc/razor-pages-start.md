---
title: Introducción a las páginas de Razor en ASP.NET Core con Visual Studio Code
author: rick-anderson
description: Obtenga información sobre los conceptos básicos para compilar una aplicación web de páginas de Razor de ASP.NET Core con Visual Studio Code.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/27/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages-vsc/razor-pages-start
ms.openlocfilehash: 0ad008b4f2b2e74dcf7f3d6c83798d5f03d1d315
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2018
---
# <a name="get-started-with-aspnet-core-razor-pages-in-visual-studio-code"></a><span data-ttu-id="fdc30-103">Introducción a las páginas de Razor en ASP.NET Core con Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="fdc30-103">Get started with ASP.NET Core Razor Pages in Visual Studio Code</span></span>

<span data-ttu-id="fdc30-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="fdc30-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="fdc30-105">En este tutorial se enseñan los conceptos básicos de la compilación de una aplicación web de páginas de Razor de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="fdc30-105">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="fdc30-106">Se recomienda leer [Introduction to Razor Pages](xref:mvc/razor-pages/index) (Introducción a las páginas de Razor) antes de empezar este tutorial.</span><span class="sxs-lookup"><span data-stu-id="fdc30-106">We recommend you complete [Introduction to Razor Pages](xref:mvc/razor-pages/index) before starting this tutorial.</span></span> <span data-ttu-id="fdc30-107">Las páginas de Razor son el método recomendado para crear la interfaz de usuario de aplicaciones web en ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="fdc30-107">Razor Pages is the recommended way to build UI for web applications in ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fdc30-108">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="fdc30-108">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="fdc30-109">Creación de una aplicación web de Razor</span><span class="sxs-lookup"><span data-stu-id="fdc30-109">Create a Razor web app</span></span>

<span data-ttu-id="fdc30-110">Desde un terminal, ejecute estos comandos:</span><span class="sxs-lookup"><span data-stu-id="fdc30-110">From a terminal, run the following commands:</span></span>

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

<span data-ttu-id="fdc30-111">Los comandos anteriores usan el [CLI de .NET Core](https://docs.microsoft.com/dotnet/core/tools/dotnet) para crear y ejecutar un proyecto de páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="fdc30-111">The preceding commands use the [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) to create and run a Razor Pages project.</span></span> <span data-ttu-id="fdc30-112">Abra http://localhost:5000 en un explorador para ver la aplicación.</span><span class="sxs-lookup"><span data-stu-id="fdc30-112">Open a browser to http://localhost:5000 to view the application.</span></span>

![Página Inicio o Índice](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE [razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a><span data-ttu-id="fdc30-114">Abrir el proyecto</span><span class="sxs-lookup"><span data-stu-id="fdc30-114">Open the project</span></span>

<span data-ttu-id="fdc30-115">Presione CTRL+C para cerrar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="fdc30-115">Press Ctrl+C to shut down the application.</span></span>

<span data-ttu-id="fdc30-116">En Visual Studio Code (VS Code), seleccione **Archivo > Abrir carpeta** y elija la carpeta *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="fdc30-116">From Visual Studio Code (VS Code), select **File > Open Folder**, and then select the *RazorPagesMovie* folder.</span></span>

- <span data-ttu-id="fdc30-117">Seleccione **Sí** en el mensaje de **advertencia** "Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?" (Faltan los activos necesarios para compilar y depurar en 'RazorPagesMovie'.</span><span class="sxs-lookup"><span data-stu-id="fdc30-117">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'RazorPagesMovie'.</span></span> <span data-ttu-id="fdc30-118">Add them?" (Faltan los activos necesarios para compilar y depurar en 'TodoApi'. ¿Quiere agregarlos?).</span><span class="sxs-lookup"><span data-stu-id="fdc30-118">Add them?"</span></span>
- <span data-ttu-id="fdc30-119">Seleccione **Restaurar** en el mensaje de **información** "Hay dependencias no resueltas".</span><span class="sxs-lookup"><span data-stu-id="fdc30-119">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="fdc30-120">Iniciar la aplicación</span><span class="sxs-lookup"><span data-stu-id="fdc30-120">Launch the app</span></span>

<span data-ttu-id="fdc30-121">Presione Ctrl+F5 para iniciar la aplicación sin depurar.</span><span class="sxs-lookup"><span data-stu-id="fdc30-121">Press Ctrl+F5 to start the app without debugging.</span></span> <span data-ttu-id="fdc30-122">Si lo prefiere, puede ir al menú **Depurar** y seleccionar **Iniciar sin depurar**.</span><span class="sxs-lookup"><span data-stu-id="fdc30-122">Alternatively, from the **Debug** menu, select **Start Without Debugging**.</span></span>

<span data-ttu-id="fdc30-123">En el tutorial siguiente, agregaremos un modelo al proyecto.</span><span class="sxs-lookup"><span data-stu-id="fdc30-123">In the next tutorial, we add a model to the project.</span></span> 

> [!div class="step-by-step"]
> [<span data-ttu-id="fdc30-124">Siguiente: Adición de un modelo</span><span class="sxs-lookup"><span data-stu-id="fdc30-124">Next: Adding a model</span></span>](xref:tutorials/razor-pages-vsc/model)  
