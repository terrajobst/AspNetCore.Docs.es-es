---
title: "Precompilación y compilación de vista razor"
author: rick-anderson
description: "Un documento de referencia que se explica cómo habilitar la compilación de la vista de MVC Razor y precompilación en aplicaciones de ASP.NET Core."
keywords: "Núcleo de ASP.NET, la compilación de vista Razor, Razor anterior a la compilación, precompilación de Razor"
ms.author: riande
manager: wpickett
ms.date: 08/16/2017
ms.topic: article
ms.assetid: ab4705b7-1638-1638-bc97-ea7f292fe92a
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/view-compilation
ms.openlocfilehash: bfee2e5e8f71c99465be79589a77f0e173097b23
ms.sourcegitcommit: 8005eb4051e568d88ee58d48424f39916052e6e2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2017
---
# <a name="razor-view-compilation-and-precompilation-in-aspnet-core"></a><span data-ttu-id="280c9-104">Compilación de vista Razor y precompilación en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="280c9-104">Razor view compilation and precompilation in ASP.NET Core</span></span>

<span data-ttu-id="280c9-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="280c9-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="280c9-106">Vistas de Razor se compilan en tiempo de ejecución cuando se invoca la vista.</span><span class="sxs-lookup"><span data-stu-id="280c9-106">Razor views are compiled at runtime when the view is invoked.</span></span> <span data-ttu-id="280c9-107">ASP.NET principales 1.1.0 y versiones posteriores puede opcionalmente compilar vistas Razor e implementarlas con la aplicación &mdash; un proceso conocido como precompilación.</span><span class="sxs-lookup"><span data-stu-id="280c9-107">ASP.NET Core 1.1.0 and higher can optionally compile Razor views and deploy them with the app &mdash; a process known as precompilation.</span></span> <span data-ttu-id="280c9-108">Las plantillas de proyecto de ASP.NET Core 2.x habilitan precompilación de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="280c9-108">The ASP.NET Core 2.x project templates enable precompilation by default.</span></span>

> [!NOTE]
> <span data-ttu-id="280c9-109">Precompilación de vista Razor no está disponible actualmente al realizar una [implementación independiente (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) de núcleo de ASP.NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="280c9-109">Razor view precompilation is currently unavailable when performing a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0.</span></span> <span data-ttu-id="280c9-110">La característica estará disponible para DVL cuando vaya a versiones 2.1.</span><span class="sxs-lookup"><span data-stu-id="280c9-110">The feature will be available for SCDs when 2.1 releases.</span></span> <span data-ttu-id="280c9-111">Para obtener más información, consulte [se produce un error en la compilación de la vista cuando se compila entre para Linux en Windows](https://github.com/aspnet/MvcPrecompilation/issues/102).</span><span class="sxs-lookup"><span data-stu-id="280c9-111">For more information, see [View compilation fails when cross-compiling for Linux on Windows](https://github.com/aspnet/MvcPrecompilation/issues/102).</span></span>

<span data-ttu-id="280c9-112">Consideraciones para la precompilación:</span><span class="sxs-lookup"><span data-stu-id="280c9-112">Precompilation considerations:</span></span>

* <span data-ttu-id="280c9-113">Precompilar vistas da como resultado un menor publicado agrupación y el tiempo de inicio.</span><span class="sxs-lookup"><span data-stu-id="280c9-113">Precompiling views results in a smaller published bundle and faster startup time.</span></span>
* <span data-ttu-id="280c9-114">No se puede editar archivos de Razor después de precompilar vistas.</span><span class="sxs-lookup"><span data-stu-id="280c9-114">You can't edit Razor files after you precompile views.</span></span> <span data-ttu-id="280c9-115">Las vistas editadas no estará presentes en el paquete publicado.</span><span class="sxs-lookup"><span data-stu-id="280c9-115">The edited views won't be present in the published bundle.</span></span> 

<span data-ttu-id="280c9-116">Para implementar vistas precompiladas:</span><span class="sxs-lookup"><span data-stu-id="280c9-116">To deploy precompiled views:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="280c9-117">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="280c9-117">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="280c9-118">Si el proyecto tiene como destino .NET Framework, incluir una referencia de paquete a `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`:</span><span class="sxs-lookup"><span data-stu-id="280c9-118">If your project targets .NET Framework, include a package reference to `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`:</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

<span data-ttu-id="280c9-119">Si el proyecto tiene como destino .NET Core, no son necesarios cambios.</span><span class="sxs-lookup"><span data-stu-id="280c9-119">If your project targets .NET Core, no changes are necessary.</span></span>

<span data-ttu-id="280c9-120">Las plantillas de proyecto de ASP.NET Core 2.x establece implícitamente `MvcRazorCompileOnPublish` a `true` de forma predeterminada, lo que significa que este nodo se puede quitar de forma segura desde el *.csproj* archivo.</span><span class="sxs-lookup"><span data-stu-id="280c9-120">The ASP.NET Core 2.x project templates implicitly set `MvcRazorCompileOnPublish` to `true` by default, which means this node can be safely removed from the *.csproj* file.</span></span> <span data-ttu-id="280c9-121">Si prefiere sea explícito, no hay peligro en configuración de la `MvcRazorCompileOnPublish` propiedad `true`.</span><span class="sxs-lookup"><span data-stu-id="280c9-121">If you prefer to be explicit, there's no harm in setting the `MvcRazorCompileOnPublish` property to `true`.</span></span> <span data-ttu-id="280c9-122">El siguiente *.csproj* ejemplo resalta esta configuración:</span><span class="sxs-lookup"><span data-stu-id="280c9-122">The following *.csproj* sample highlights this setting:</span></span>

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=5)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="280c9-123">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="280c9-123">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="280c9-124">Establecer `MvcRazorCompileOnPublish` a `true`e incluir una referencia de paquete para `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`.</span><span class="sxs-lookup"><span data-stu-id="280c9-124">Set `MvcRazorCompileOnPublish` to `true`, and include a package reference to `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`.</span></span> <span data-ttu-id="280c9-125">El siguiente *.csproj* ejemplo resalta estas opciones:</span><span class="sxs-lookup"><span data-stu-id="280c9-125">The following *.csproj* sample highlights these settings:</span></span>

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish.csproj?highlight=5,12)]

---
