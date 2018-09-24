---
title: Introducción a las páginas de Razor en ASP.NET Core en macOS con Visual Studio para Mac
author: rick-anderson
description: Obtenga información sobre cómo empezar a usar las páginas de Razor de ASP.NET Core con Visual Studio para Mac.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 07/27/2017
uid: tutorials/razor-pages-mac/razor-pages-start
ms.openlocfilehash: 0e1c2a9ab436968e2c24aa5f306ec69674fb025b
ms.sourcegitcommit: c12ebdab65853f27fbb418204646baf6ce69515e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/21/2018
ms.locfileid: "46523277"
---
# <a name="get-started-with-razor-pages-in-aspnet-core-on-macos-with-visual-studio-for-mac"></a><span data-ttu-id="0497f-103">Introducción a las páginas de Razor en ASP.NET Core en macOS con Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="0497f-103">Get started with Razor Pages in ASP.NET Core on macOS with Visual Studio for Mac</span></span>

<span data-ttu-id="0497f-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0497f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="0497f-105">En este tutorial se enseñan los conceptos básicos de la compilación de una aplicación web de páginas de Razor de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0497f-105">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="0497f-106">Se recomienda leer [Introduction to Razor Pages](xref:razor-pages/index) (Introducción a las páginas de Razor) antes de empezar este tutorial.</span><span class="sxs-lookup"><span data-stu-id="0497f-106">We recommend you review [Introduction to Razor Pages](xref:razor-pages/index) before starting this tutorial.</span></span> <span data-ttu-id="0497f-107">Las páginas de Razor son el método recomendado para crear la interfaz de usuario de aplicaciones web en ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0497f-107">Razor Pages is the recommended way to build UI for web applications in ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0497f-108">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="0497f-108">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-macos.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="0497f-109">Creación de una aplicación web de Razor</span><span class="sxs-lookup"><span data-stu-id="0497f-109">Create a Razor web app</span></span>

<span data-ttu-id="0497f-110">Desde un terminal, ejecute estos comandos:</span><span class="sxs-lookup"><span data-stu-id="0497f-110">From a terminal, run the following commands:</span></span>

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new webapp -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

::: moniker-end

<span data-ttu-id="0497f-111">Los comandos anteriores usan el [CLI de .NET Core](https://docs.microsoft.com/dotnet/core/tools/dotnet) para crear y ejecutar un proyecto de páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="0497f-111">The preceding commands use the [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) to create and run a Razor Pages project.</span></span> <span data-ttu-id="0497f-112">Abra http://localhost:5000 en un explorador para ver la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0497f-112">Open a browser to http://localhost:5000 to view the application.</span></span>

![Página Inicio o Índice](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE [razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a><span data-ttu-id="0497f-114">Abrir el proyecto</span><span class="sxs-lookup"><span data-stu-id="0497f-114">Open the project</span></span>

<span data-ttu-id="0497f-115">Presione CTRL+C para cerrar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0497f-115">Press Ctrl+C to shut down the application.</span></span>

<span data-ttu-id="0497f-116">En Visual Studio, seleccione **Archivo > Abrir** y elija el archivo *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="0497f-116">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="0497f-117">Iniciar la aplicación</span><span class="sxs-lookup"><span data-stu-id="0497f-117">Launch the app</span></span>

<span data-ttu-id="0497f-118">En Visual Studio, seleccione **Ejecutar > Iniciar sin depurar** para iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0497f-118">In Visual Studio, select **Run > Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="0497f-119">Visual Studio iniciará [Kestrel](xref:fundamentals/servers/kestrel) y un explorador, y se desplazará a `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="0497f-119">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5000`.</span></span>

<span data-ttu-id="0497f-120">En el tutorial siguiente, agregaremos un modelo al proyecto.</span><span class="sxs-lookup"><span data-stu-id="0497f-120">In the next tutorial, we add a model to the project.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="0497f-121">Siguiente: Adición de un modelo</span><span class="sxs-lookup"><span data-stu-id="0497f-121">Next: Adding a model</span></span>](xref:tutorials/razor-pages-mac/model)
