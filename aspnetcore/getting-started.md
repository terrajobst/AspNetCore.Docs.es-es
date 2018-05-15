---
title: Introducción a ASP.NET Core
author: rick-anderson
description: Tutorial rápido que crea y ejecuta una aplicación Hola mundo sencilla mediante ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/18/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: getting-started
ms.openlocfilehash: c2f18c69901a5a6503314d508a776e6985872681
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="67fa9-103">Introducción a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="67fa9-103">Get Started with ASP.NET Core</span></span>

> [!NOTE]
> <span data-ttu-id="67fa9-104">Estas instrucciones corresponden a la versión más reciente de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="67fa9-104">These instructions are for the latest version of ASP.NET Core.</span></span> <span data-ttu-id="67fa9-105">Vea [Introducción a ASP.NET Core 1.1](xref:getting-started-1.1) para obtener este documento, pero sobre la versión 1.1.</span><span class="sxs-lookup"><span data-stu-id="67fa9-105">See [Getting Started with ASP.NET Core 1.1](xref:getting-started-1.1) for the 1.1 version of this document.</span></span>

1. <span data-ttu-id="67fa9-106">Instale el [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span><span class="sxs-lookup"><span data-stu-id="67fa9-106">Install the [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span></span>

2. <span data-ttu-id="67fa9-107">Cree un nuevo proyecto de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="67fa9-107">Create a new .NET Core project.</span></span>

   <span data-ttu-id="67fa9-108">En macOS y Linux, abra una ventana de terminal.</span><span class="sxs-lookup"><span data-stu-id="67fa9-108">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="67fa9-109">En Windows, abra un símbolo del sistema.</span><span class="sxs-lookup"><span data-stu-id="67fa9-109">On Windows, open a command prompt.</span></span> <span data-ttu-id="67fa9-110">Escriba el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="67fa9-110">Enter the following command:</span></span>

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```
    
3. <span data-ttu-id="67fa9-111">Ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="67fa9-111">Run the app.</span></span>

    <span data-ttu-id="67fa9-112">Use los comandos siguientes para ejecutar la aplicación:</span><span class="sxs-lookup"><span data-stu-id="67fa9-112">Use the following commands to run the app:</span></span>

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

4. <span data-ttu-id="67fa9-113">Vaya a [http://localhost:5000](http://localhost:5000).</span><span class="sxs-lookup"><span data-stu-id="67fa9-113">Browse to [http://localhost:5000](http://localhost:5000)</span></span>

5. <span data-ttu-id="67fa9-114">Abra <em>Pages/About.cshtml</em> y modifique la página para que muestre el mensaje "¡Hola, mundo!".</span><span class="sxs-lookup"><span data-stu-id="67fa9-114">Open <em>Pages/About.cshtml</em> and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="67fa9-115">La hora del servidor es "@DateTime.Now":</span><span class="sxs-lookup"><span data-stu-id="67fa9-115">The time on the server is @DateTime.Now ":</span></span>

    [!code-html[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. <span data-ttu-id="67fa9-116">Vaya a [http://localhost:5000/About](http://localhost:5000/About) y compruebe los cambios.</span><span class="sxs-lookup"><span data-stu-id="67fa9-116">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

### <a name="next-steps"></a><span data-ttu-id="67fa9-117">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="67fa9-117">Next steps</span></span>

<span data-ttu-id="67fa9-118">Para obtener tutoriales de introducción, vea [Tutoriales de ASP.NET Core](tutorials/index.md)</span><span class="sxs-lookup"><span data-stu-id="67fa9-118">For getting-started tutorials, see [ASP.NET Core Tutorials](tutorials/index.md)</span></span>

<span data-ttu-id="67fa9-119">Para obtener una introducción a la arquitectura y los conceptos de ASP.NET Core, vea [ASP.NET Core Introduction (Introducción a ASP.NET Core)](index.md) y [ASP.NET Core Fundamentals (Conceptos básicos de ASP.NET Core)](fundamentals/index.md).</span><span class="sxs-lookup"><span data-stu-id="67fa9-119">For an introduction to ASP.NET Core concepts and architecture, see [ASP.NET Core Introduction](index.md) and [ASP.NET Core Fundamentals](fundamentals/index.md).</span></span>

<span data-ttu-id="67fa9-120">Una aplicación de ASP.NET Core puede usar la biblioteca de clases base y el runtime de .NET Core o .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="67fa9-120">An ASP.NET Core app can use the .NET Core or .NET Framework Base Class Library and runtime.</span></span> <span data-ttu-id="67fa9-121">Para más información, vea [Selección entre .NET Core y .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="67fa9-121">For more information, see [Choosing between .NET Core and .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>
