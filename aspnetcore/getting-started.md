---
title: "Introducción a ASP.NET Core 2.0"
author: rick-anderson
description: "Tutorial rápido que crea y ejecuta una aplicación Hola mundo sencilla mediante ASP.NET Core."
keywords: "ASP.NET Core,tutorial,introducción"
ms.author: riande
manager: wpickett
ms.date: 08/30/2017
ms.topic: get-started-article
ms.assetid: 73543e9d-d9d5-47d6-9664-17a9beea6cd3
ms.technology: aspnet
ms.prod: asp.net-core
uid: getting-started
ms.openlocfilehash: 5cba57c23ab66475648b19cba3f9aaa02bea3dba
ms.sourcegitcommit: d56f36805ce70470775da58db63c0965d65660d3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2017
---
# <a name="getting-started-with-aspnet-core"></a><span data-ttu-id="f8880-104">Introducción a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f8880-104">Getting Started with ASP.NET Core</span></span>

> [!NOTE]
> <span data-ttu-id="f8880-105">Estas instrucciones corresponden a la versión más reciente de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f8880-105">These instructions are for the latest version of ASP.NET Core.</span></span> <span data-ttu-id="f8880-106">Si busca la introducción de una versión anterior,</span><span class="sxs-lookup"><span data-stu-id="f8880-106">Looking to get started with an earlier version?</span></span> <span data-ttu-id="f8880-107">vea [la versión 1.1 de este tutorial](xref:getting-started-1.1).</span><span class="sxs-lookup"><span data-stu-id="f8880-107">See [the 1.1 version of this tutorial](xref:getting-started-1.1).</span></span>

1. <span data-ttu-id="f8880-108">Instale [.NET Core](https://www.microsoft.com/net/core/).</span><span class="sxs-lookup"><span data-stu-id="f8880-108">Install [.NET Core](https://www.microsoft.com/net/core/).</span></span>

2. <span data-ttu-id="f8880-109">Cree un nuevo proyecto de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="f8880-109">Create a new .NET Core project.</span></span>

   <span data-ttu-id="f8880-110">En macOS y Linux, abra una ventana de terminal.</span><span class="sxs-lookup"><span data-stu-id="f8880-110">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="f8880-111">En Windows, abra un símbolo del sistema.</span><span class="sxs-lookup"><span data-stu-id="f8880-111">On Windows, open a command prompt.</span></span>

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```
    
4. <span data-ttu-id="f8880-112">Ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f8880-112">Run the app.</span></span>

    <span data-ttu-id="f8880-113">Use los comandos siguientes para ejecutar la aplicación:</span><span class="sxs-lookup"><span data-stu-id="f8880-113">Use the following commands to run the app:</span></span>

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

5. <span data-ttu-id="f8880-114">Vaya a [http://localhost:5000](http://localhost:5000).</span><span class="sxs-lookup"><span data-stu-id="f8880-114">Browse to [http://localhost:5000](http://localhost:5000)</span></span>

6. <span data-ttu-id="f8880-115">Abra *Pages/About.cshtml* y modifique la página para que muestre el mensaje "¡Hola, mundo!".</span><span class="sxs-lookup"><span data-stu-id="f8880-115">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="f8880-116">La hora del servidor es "@DateTime.Now":</span><span class="sxs-lookup"><span data-stu-id="f8880-116">The time on the server is @DateTime.Now ":</span></span>

    [!code-html[Main](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]

7. <span data-ttu-id="f8880-117">Vaya a [http://localhost:5000/About](http://localhost:5000/About) y compruebe los cambios.</span><span class="sxs-lookup"><span data-stu-id="f8880-117">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

### <a name="next-steps"></a><span data-ttu-id="f8880-118">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="f8880-118">Next steps</span></span>

<span data-ttu-id="f8880-119">Para obtener tutoriales de introducción, vea [Tutoriales de ASP.NET Core](tutorials/index.md)</span><span class="sxs-lookup"><span data-stu-id="f8880-119">For getting-started tutorials, see [ASP.NET Core Tutorials](tutorials/index.md)</span></span>

<span data-ttu-id="f8880-120">Para obtener una introducción a la arquitectura y los conceptos de ASP.NET Core, vea [ASP.NET Core Introduction (Introducción a ASP.NET Core)](index.md) y [ASP.NET Core Fundamentals (Conceptos básicos de ASP.NET Core)](fundamentals/index.md).</span><span class="sxs-lookup"><span data-stu-id="f8880-120">For an introduction to ASP.NET Core concepts and architecture, see [ASP.NET Core Introduction](index.md) and [ASP.NET Core Fundamentals](fundamentals/index.md).</span></span>

<span data-ttu-id="f8880-121">Una aplicación de ASP.NET Core puede usar la biblioteca de clases base y el runtime de .NET Core o .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="f8880-121">An ASP.NET Core app can use the .NET Core or .NET Framework Base Class Library and runtime.</span></span> <span data-ttu-id="f8880-122">Para más información, vea [Selección entre .NET Core y .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="f8880-122">For more information, see [Choosing between .NET Core and .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>
