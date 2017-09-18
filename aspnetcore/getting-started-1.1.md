---
title: "Introducción a ASP.NET Core 1.1"
author: rick-anderson
description: "Tutorial rápido que crea y ejecuta una aplicación Hola mundo sencilla mediante ASP.NET Core 1.1."
keywords: "ASP.NET Core,tutorial,introducción"
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.assetid: 73543e9d-d9d5-47d6-9664-17a9beea6cd3
ms.technology: aspnet
ms.prod: asp.net-core
uid: getting-started-1.1
ms.openlocfilehash: e8fd9ef60ebc1cff6ca0e03000ea50eebff0a9f9
ms.sourcegitcommit: 297ee5d2f3b3b24eb8a2c4a25195c9e2973cb91b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/14/2017
---
# <a name="getting-started-with-aspnet-core-11"></a><span data-ttu-id="17b56-104">Introducción a ASP.NET Core 1.1</span><span class="sxs-lookup"><span data-stu-id="17b56-104">Getting Started with ASP.NET Core 1.1</span></span>

> [!NOTE]
> <span data-ttu-id="17b56-105">Estas instrucciones corresponden a ASP.NET Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="17b56-105">These instructions are for ASP.NET Core 1.1.</span></span> <span data-ttu-id="17b56-106">Si busca la versión más reciente,</span><span class="sxs-lookup"><span data-stu-id="17b56-106">Looking for the latest version?</span></span> <span data-ttu-id="17b56-107">vea [la versión actual de este tutorial](xref:getting-started).</span><span class="sxs-lookup"><span data-stu-id="17b56-107">See [the current version of this tutorial](xref:getting-started).</span></span>

1. <span data-ttu-id="17b56-108">Instale el **instalador del SDK** de .NET Core para SDK 1.0.4 desde la [página de descargas de .NET Core 1.0.5 & 1.1.2 SDK 1.0.4](https://github.com/dotnet/core/blob/master/release-notes/download-archives/1.0.5-download.md).</span><span class="sxs-lookup"><span data-stu-id="17b56-108">Install the .NET Core **SDK Installer** for SDK 1.0.4 from the [.NET Core 1.0.5 & 1.1.2 SDK 1.0.4 downloads page](https://github.com/dotnet/core/blob/master/release-notes/download-archives/1.0.5-download.md).</span></span>

2. <span data-ttu-id="17b56-109">Cree una carpeta para un nuevo proyecto de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="17b56-109">Create a folder for a new .NET Core project.</span></span>

   <span data-ttu-id="17b56-110">En macOS y Linux, abra una ventana de terminal.</span><span class="sxs-lookup"><span data-stu-id="17b56-110">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="17b56-111">En Windows, abra un símbolo del sistema.</span><span class="sxs-lookup"><span data-stu-id="17b56-111">On Windows, open a command prompt.</span></span>

   ```terminal
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

2. <span data-ttu-id="17b56-112">Si ha instalado una versión posterior del SDK en el equipo, cree un archivo *global.json* para seleccionar la versión 1.0.4 del SDK.</span><span class="sxs-lookup"><span data-stu-id="17b56-112">If you have installed a later SDK version on your machine, create a *global.json* file to select the 1.0.4 SDK.</span></span>

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

2. <span data-ttu-id="17b56-113">Cree un nuevo proyecto de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="17b56-113">Create a new .NET Core project.</span></span>

   ```terminal
   dotnet new web
   ```
   
3.  <span data-ttu-id="17b56-114">Restaure los paquetes.</span><span class="sxs-lookup"><span data-stu-id="17b56-114">Restore the packages.</span></span>

    ```terminal
    dotnet restore
    ```

4. <span data-ttu-id="17b56-115">Ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="17b56-115">Run the app.</span></span>

   <span data-ttu-id="17b56-116">El comando `dotnet run` primero compila la aplicación en caso necesario.</span><span class="sxs-lookup"><span data-stu-id="17b56-116">The `dotnet run` command builds the app first if needed.</span></span>

   ```terminal
   dotnet run
   ```

5. <span data-ttu-id="17b56-117">Vaya a `http://localhost:5000`</span><span class="sxs-lookup"><span data-stu-id="17b56-117">Browse to `http://localhost:5000`</span></span>

<!-- H3 to avoid a single-entry internal TOC -->
### <a name="next-steps"></a><span data-ttu-id="17b56-118">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="17b56-118">Next steps</span></span>

<span data-ttu-id="17b56-119">Para obtener tutoriales de introducción, vea [Tutoriales de ASP.NET Core](tutorials/index.md)</span><span class="sxs-lookup"><span data-stu-id="17b56-119">For getting-started tutorials, see [ASP.NET Core Tutorials](tutorials/index.md)</span></span>

<span data-ttu-id="17b56-120">Para obtener una introducción a la arquitectura y los conceptos de ASP.NET Core, vea [ASP.NET Core Introduction (Introducción a ASP.NET Core)](index.md) y [ASP.NET Core Fundamentals (Conceptos básicos de ASP.NET Core)](fundamentals/index.md).</span><span class="sxs-lookup"><span data-stu-id="17b56-120">For an introduction to ASP.NET Core concepts and architecture, see [ASP.NET Core Introduction](index.md) and [ASP.NET Core Fundamentals](fundamentals/index.md).</span></span>

<span data-ttu-id="17b56-121">Una aplicación de ASP.NET Core puede usar la biblioteca de clases base y el runtime de .NET Core o .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="17b56-121">An ASP.NET Core app can use the .NET Core or .NET Framework Base Class Library and runtime.</span></span> <span data-ttu-id="17b56-122">Para más información, vea [Selección entre .NET Core y .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="17b56-122">For more information, see [Choosing between .NET Core and .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>
