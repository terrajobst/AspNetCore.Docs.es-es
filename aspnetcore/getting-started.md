---
title: "Introducción a ASP.NET Core 2.0"
author: rick-anderson
description: "Tutorial rápido que crea y ejecuta una aplicación Hola mundo sencilla mediante ASP.NET Core."
keywords: "ASP.NET Core,tutorial,introducción"
ms.author: riande
manager: wpickett
ms.date: 10/18/2017
ms.topic: get-started-article
ms.assetid: 73543e9d-d9d5-47d6-9664-17a9beea6cd3
ms.technology: aspnet
ms.prod: asp.net-core
uid: getting-started
ms.openlocfilehash: 5b8c9f770e749c13bc562f157b4ebfd25a88a4e0
ms.sourcegitcommit: 019e5a0342fd49a94056d14fc7a1a1d0f81d2a39
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/22/2017
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="e671e-104">Introducción a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e671e-104">Get Started with ASP.NET Core</span></span>

> [!NOTE]
> <span data-ttu-id="e671e-105">Estas instrucciones corresponden a la versión más reciente de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e671e-105">These instructions are for the latest version of ASP.NET Core.</span></span> <span data-ttu-id="e671e-106">Si busca la introducción de una versión anterior,</span><span class="sxs-lookup"><span data-stu-id="e671e-106">Looking to get started with an earlier version?</span></span> <span data-ttu-id="e671e-107">vea [la versión 1.1 de este tutorial](xref:getting-started-1.1).</span><span class="sxs-lookup"><span data-stu-id="e671e-107">See [the 1.1 version of this tutorial](xref:getting-started-1.1).</span></span>

1. <span data-ttu-id="e671e-108">Instale [.NET Core](https://www.microsoft.com/net/core/).</span><span class="sxs-lookup"><span data-stu-id="e671e-108">Install [.NET Core](https://www.microsoft.com/net/core/).</span></span>

2. <span data-ttu-id="e671e-109">Cree un nuevo proyecto de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="e671e-109">Create a new .NET Core project.</span></span>

   <span data-ttu-id="e671e-110">En macOS y Linux, abra una ventana de terminal.</span><span class="sxs-lookup"><span data-stu-id="e671e-110">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="e671e-111">En Windows, abra un símbolo del sistema.</span><span class="sxs-lookup"><span data-stu-id="e671e-111">On Windows, open a command prompt.</span></span> <span data-ttu-id="e671e-112">Escriba el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="e671e-112">Enter the following command:</span></span>

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```
    
4. <span data-ttu-id="e671e-113">Ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="e671e-113">Run the app.</span></span>

    <span data-ttu-id="e671e-114">Use los comandos siguientes para ejecutar la aplicación:</span><span class="sxs-lookup"><span data-stu-id="e671e-114">Use the following commands to run the app:</span></span>

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

5. <span data-ttu-id="e671e-115">Vaya a [http://localhost:5000](http://localhost:5000).</span><span class="sxs-lookup"><span data-stu-id="e671e-115">Browse to [http://localhost:5000](http://localhost:5000)</span></span>

6. <span data-ttu-id="e671e-116">Abra *Pages/About.cshtml* y modifique la página para que muestre el mensaje "¡Hola, mundo!".</span><span class="sxs-lookup"><span data-stu-id="e671e-116">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="e671e-117">La hora del servidor es "@DateTime.Now":</span><span class="sxs-lookup"><span data-stu-id="e671e-117">The time on the server is @DateTime.Now ":</span></span>

    [!code-html[Main](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]

7. <span data-ttu-id="e671e-118">Vaya a [http://localhost:5000/About](http://localhost:5000/About) y compruebe los cambios.</span><span class="sxs-lookup"><span data-stu-id="e671e-118">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

### <a name="next-steps"></a><span data-ttu-id="e671e-119">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="e671e-119">Next steps</span></span>

<span data-ttu-id="e671e-120">Para obtener tutoriales de introducción, vea [Tutoriales de ASP.NET Core](tutorials/index.md)</span><span class="sxs-lookup"><span data-stu-id="e671e-120">For getting-started tutorials, see [ASP.NET Core Tutorials](tutorials/index.md)</span></span>

<span data-ttu-id="e671e-121">Para obtener una introducción a la arquitectura y los conceptos de ASP.NET Core, vea [ASP.NET Core Introduction (Introducción a ASP.NET Core)](index.md) y [ASP.NET Core Fundamentals (Conceptos básicos de ASP.NET Core)](fundamentals/index.md).</span><span class="sxs-lookup"><span data-stu-id="e671e-121">For an introduction to ASP.NET Core concepts and architecture, see [ASP.NET Core Introduction](index.md) and [ASP.NET Core Fundamentals](fundamentals/index.md).</span></span>

<span data-ttu-id="e671e-122">Una aplicación de ASP.NET Core puede usar la biblioteca de clases base y el runtime de .NET Core o .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="e671e-122">An ASP.NET Core app can use the .NET Core or .NET Framework Base Class Library and runtime.</span></span> <span data-ttu-id="e671e-123">Para más información, vea [Selección entre .NET Core y .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="e671e-123">For more information, see [Choosing between .NET Core and .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>
