---
title: "Introducción a ASP.NET Core 2.0"
author: rick-anderson
description: "Tutorial rápido que crea y ejecuta una aplicación Hola mundo sencilla mediante ASP.NET Core."
keywords: "ASP.NET Core,tutorial,introducción"
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.assetid: 73543e9d-d9d5-47d6-9664-17a9beea6cd3
ms.technology: aspnet
ms.prod: asp.net-core
uid: getting-started
ms.openlocfilehash: 3399df3958093da9117b013736b1cb370fd6beb2
ms.sourcegitcommit: 297ee5d2f3b3b24eb8a2c4a25195c9e2973cb91b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/14/2017
---
# <a name="getting-started-with-aspnet-core"></a><span data-ttu-id="d6bd1-104">Introducción a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d6bd1-104">Getting Started with ASP.NET Core</span></span>

> [!NOTE]
> <span data-ttu-id="d6bd1-105">Estas instrucciones corresponden a la versión más reciente de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d6bd1-105">These instructions are for the latest version of ASP.NET Core.</span></span> <span data-ttu-id="d6bd1-106">Si busca la introducción de una versión anterior,</span><span class="sxs-lookup"><span data-stu-id="d6bd1-106">Looking to get started with an earlier version?</span></span> <span data-ttu-id="d6bd1-107">vea [la versión 1.1 de este tutorial](xref:getting-started-1.1).</span><span class="sxs-lookup"><span data-stu-id="d6bd1-107">See [the 1.1 version of this tutorial](xref:getting-started-1.1).</span></span>

1. <span data-ttu-id="d6bd1-108">Instale [.NET Core](https://microsoft.com/net/core/).</span><span class="sxs-lookup"><span data-stu-id="d6bd1-108">Install [.NET Core](https://microsoft.com/net/core/).</span></span>

2. <span data-ttu-id="d6bd1-109">Cree un nuevo proyecto de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="d6bd1-109">Create a new .NET Core project.</span></span>

   <span data-ttu-id="d6bd1-110">En macOS y Linux, abra una ventana de terminal.</span><span class="sxs-lookup"><span data-stu-id="d6bd1-110">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="d6bd1-111">En Windows, abra un símbolo del sistema.</span><span class="sxs-lookup"><span data-stu-id="d6bd1-111">On Windows, open a command prompt.</span></span>

   ```terminal
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   dotnet new web
   ```
    
4. <span data-ttu-id="d6bd1-112">Ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d6bd1-112">Run the app.</span></span>

   <span data-ttu-id="d6bd1-113">El comando `dotnet run` primero compila la aplicación en caso necesario.</span><span class="sxs-lookup"><span data-stu-id="d6bd1-113">The `dotnet run` command builds the app first if needed.</span></span>

   ```terminal
   dotnet run
   ```

5. <span data-ttu-id="d6bd1-114">Vaya a `http://localhost:5000`</span><span class="sxs-lookup"><span data-stu-id="d6bd1-114">Browse to `http://localhost:5000`</span></span>

### <a name="next-steps"></a><span data-ttu-id="d6bd1-115">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="d6bd1-115">Next steps</span></span>

<span data-ttu-id="d6bd1-116">Para obtener tutoriales de introducción, vea [Tutoriales de ASP.NET Core](tutorials/index.md)</span><span class="sxs-lookup"><span data-stu-id="d6bd1-116">For getting-started tutorials, see [ASP.NET Core Tutorials](tutorials/index.md)</span></span>

<span data-ttu-id="d6bd1-117">Para obtener una introducción a la arquitectura y los conceptos de ASP.NET Core, vea [ASP.NET Core Introduction (Introducción a ASP.NET Core)](index.md) y [ASP.NET Core Fundamentals (Conceptos básicos de ASP.NET Core)](fundamentals/index.md).</span><span class="sxs-lookup"><span data-stu-id="d6bd1-117">For an introduction to ASP.NET Core concepts and architecture, see [ASP.NET Core Introduction](index.md) and [ASP.NET Core Fundamentals](fundamentals/index.md).</span></span>

<span data-ttu-id="d6bd1-118">Una aplicación de ASP.NET Core puede usar la biblioteca de clases base y el runtime de .NET Core o .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="d6bd1-118">An ASP.NET Core app can use the .NET Core or .NET Framework Base Class Library and runtime.</span></span> <span data-ttu-id="d6bd1-119">Para más información, vea [Selección entre .NET Core y .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="d6bd1-119">For more information, see [Choosing between .NET Core and .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>
