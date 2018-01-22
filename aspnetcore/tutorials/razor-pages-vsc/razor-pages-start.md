---
title: "Introducción a las páginas de Razor en ASP.NET Core con Visual Studio Code"
author: rick-anderson
description: "Introducción a las páginas de Razor en ASP.NET Core con Visual Studio Code"
ms.author: riande
manager: wpickett
ms.date: 08/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages-vsc/razor-pages-start
ms.openlocfilehash: 727c3d5c8bed0aef3094816b7e5f0a940aea654c
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2018
---
# <a name="getting-started-with-razor-pages-in-aspnet-core-with-visual-studio-code"></a><span data-ttu-id="84400-103">Introducción a las páginas de Razor en ASP.NET Core con Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="84400-103">Getting started with Razor Pages in ASP.NET Core with Visual Studio Code</span></span>

<span data-ttu-id="84400-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="84400-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="84400-105">En este tutorial se enseñan los conceptos básicos de la compilación de una aplicación web de páginas de Razor de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="84400-105">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="84400-106">Se recomienda leer [Introduction to Razor Pages](xref:mvc/razor-pages/index) (Introducción a las páginas de Razor) antes de empezar este tutorial.</span><span class="sxs-lookup"><span data-stu-id="84400-106">We recommend you complete [Introduction to Razor Pages](xref:mvc/razor-pages/index) before starting this tutorial.</span></span> <span data-ttu-id="84400-107">Las páginas de Razor son el método recomendado para crear la interfaz de usuario de aplicaciones web en ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="84400-107">Razor Pages is the recommended way to build UI for web applications in ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="84400-108">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="84400-108">Prerequisites</span></span>

<span data-ttu-id="84400-109">Instale el software siguiente:</span><span class="sxs-lookup"><span data-stu-id="84400-109">Install the following:</span></span>

* <span data-ttu-id="84400-110">[SDK de .NET Core 2.0.0](https://www.microsoft.com/net/core) o versiones posteriores</span><span class="sxs-lookup"><span data-stu-id="84400-110">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later</span></span>
* [<span data-ttu-id="84400-111">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="84400-111">Visual Studio Code</span></span>](https://code.visualstudio.com)
* <span data-ttu-id="84400-112">[Extensión de C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) de VS Code</span><span class="sxs-lookup"><span data-stu-id="84400-112">VS Code [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span></span> 

## <a name="create-a-razor-web-app"></a><span data-ttu-id="84400-113">Creación de una aplicación web de Razor</span><span class="sxs-lookup"><span data-stu-id="84400-113">Create a Razor web app</span></span>

<span data-ttu-id="84400-114">Desde un terminal, ejecute estos comandos:</span><span class="sxs-lookup"><span data-stu-id="84400-114">From a terminal, run the following commands:</span></span>

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

<span data-ttu-id="84400-115">Los comandos anteriores usan el [CLI de .NET Core](https://docs.microsoft.com/dotnet/core/tools/dotnet) para crear y ejecutar un proyecto de páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="84400-115">The preceding commands use the [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) to create and run a Razor Pages project.</span></span> <span data-ttu-id="84400-116">Abra http://localhost:5000 en un explorador para ver la aplicación.</span><span class="sxs-lookup"><span data-stu-id="84400-116">Open a browser to http://localhost:5000 to view the application.</span></span>

![Página Inicio o Índice](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a><span data-ttu-id="84400-118">Abrir el proyecto</span><span class="sxs-lookup"><span data-stu-id="84400-118">Open the project</span></span>

<span data-ttu-id="84400-119">Presione CTRL+C para cerrar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="84400-119">Press Ctrl+C to shut down the application.</span></span>

<span data-ttu-id="84400-120">En Visual Studio Code (VS Code), seleccione **Archivo > Abrir carpeta** y elija la carpeta *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="84400-120">From Visual Studio Code (VS Code), select **File > Open Folder**, and then select the *RazorPagesMovie* folder.</span></span>

- <span data-ttu-id="84400-121">Seleccione **Sí** en el mensaje de **advertencia** "Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?" (Faltan los activos necesarios para compilar y depurar en 'RazorPagesMovie'.</span><span class="sxs-lookup"><span data-stu-id="84400-121">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'RazorPagesMovie'.</span></span> <span data-ttu-id="84400-122">Add them?" (Faltan los activos necesarios para compilar y depurar en 'TodoApi'. ¿Quiere agregarlos?).</span><span class="sxs-lookup"><span data-stu-id="84400-122">Add them?"</span></span>
- <span data-ttu-id="84400-123">Seleccione **Restaurar** en el mensaje de **información** "Hay dependencias no resueltas".</span><span class="sxs-lookup"><span data-stu-id="84400-123">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="84400-124">Iniciar la aplicación</span><span class="sxs-lookup"><span data-stu-id="84400-124">Launch the app</span></span>

<span data-ttu-id="84400-125">Presione Ctrl+F5 para iniciar la aplicación sin depurar.</span><span class="sxs-lookup"><span data-stu-id="84400-125">Press Ctrl+F5 to start the app without debugging.</span></span> <span data-ttu-id="84400-126">Si lo prefiere, puede ir al menú **Depurar** y seleccionar **Iniciar sin depurar**.</span><span class="sxs-lookup"><span data-stu-id="84400-126">Alternatively, from the **Debug** menu, select **Start Without Debugging**.</span></span>

<span data-ttu-id="84400-127">En el tutorial siguiente, agregaremos un modelo al proyecto.</span><span class="sxs-lookup"><span data-stu-id="84400-127">In the next tutorial, we add a model to the project.</span></span> 

>[!div class="step-by-step"]
[<span data-ttu-id="84400-128">Siguiente: Adición de un modelo</span><span class="sxs-lookup"><span data-stu-id="84400-128">Next: Adding a model</span></span>](xref:tutorials/razor-pages-vsc/model)  
