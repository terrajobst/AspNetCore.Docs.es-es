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
ms.openlocfilehash: 0cb61315916d1b38f7cab3339e150c446fb69d98
ms.sourcegitcommit: 74a8ad9c1ba5c155d7c4303e67632a0922c38e86
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/20/2017
---
# <a name="razor-view-compilation-and-precompilation-in-aspnet-core"></a><span data-ttu-id="406e7-104">Compilación de vista Razor y precompilación en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="406e7-104">Razor view compilation and precompilation in ASP.NET Core</span></span>

<span data-ttu-id="406e7-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="406e7-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="406e7-106">Vistas de Razor se compilan en tiempo de ejecución cuando se invoca la vista.</span><span class="sxs-lookup"><span data-stu-id="406e7-106">Razor views are compiled at runtime when the view is invoked.</span></span> <span data-ttu-id="406e7-107">ASP.NET principales 1.1.0 y versiones posteriores puede opcionalmente compilar vistas Razor e implementarlas con la aplicación &mdash; un proceso conocido como precompilación.</span><span class="sxs-lookup"><span data-stu-id="406e7-107">ASP.NET Core 1.1.0 and higher can optionally compile Razor views and deploy them with the app &mdash; a process known as precompilation.</span></span> <span data-ttu-id="406e7-108">Las plantillas de proyecto de ASP.NET Core 2.x habilitan precompilación de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="406e7-108">The ASP.NET Core 2.x project templates enable precompilation by default.</span></span>

> [!NOTE]
> <span data-ttu-id="406e7-109">Precompilación de vista Razor no está disponible cuando se realiza una [Self-Contained implementación](https://docs.microsoft.com/dotnet/core/deploying/#self-contained-deployments-scd) en versiones de ASP.NET Core 2.0.0 y versiones anteriores.</span><span class="sxs-lookup"><span data-stu-id="406e7-109">Razor view precompilation is unavailable when doing a [Self-Contained Deployment](https://docs.microsoft.com/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core versions 2.0.0 and earlier.</span></span>

<span data-ttu-id="406e7-110">Consideraciones para la precompilación:</span><span class="sxs-lookup"><span data-stu-id="406e7-110">Precompilation considerations:</span></span>

* <span data-ttu-id="406e7-111">Precompilar vistas da como resultado un menor publicado agrupación y el tiempo de inicio.</span><span class="sxs-lookup"><span data-stu-id="406e7-111">Precompiling views results in a smaller published bundle and faster startup time.</span></span>
* <span data-ttu-id="406e7-112">No se puede editar archivos de Razor después de precompilar vistas.</span><span class="sxs-lookup"><span data-stu-id="406e7-112">You can't edit Razor files after you precompile views.</span></span> <span data-ttu-id="406e7-113">Las vistas editadas no estará presentes en el paquete publicado.</span><span class="sxs-lookup"><span data-stu-id="406e7-113">The edited views won't be present in the published bundle.</span></span> 

<span data-ttu-id="406e7-114">Para implementar vistas precompiladas:</span><span class="sxs-lookup"><span data-stu-id="406e7-114">To deploy precompiled views:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="406e7-115">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="406e7-115">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="406e7-116">Si el proyecto tiene como destino .NET Framework, incluir una referencia de paquete a `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`:</span><span class="sxs-lookup"><span data-stu-id="406e7-116">If your project targets .NET Framework, include a package reference to `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`:</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

<span data-ttu-id="406e7-117">Si el proyecto tiene como destino .NET Core, no son necesarios cambios.</span><span class="sxs-lookup"><span data-stu-id="406e7-117">If your project targets .NET Core, no changes are necessary.</span></span>

<span data-ttu-id="406e7-118">Las plantillas de proyecto de ASP.NET Core 2.x establece implícitamente `MvcRazorCompileOnPublish` a `true` de forma predeterminada, lo que significa que este nodo se puede quitar de forma segura desde el *.csproj* archivo.</span><span class="sxs-lookup"><span data-stu-id="406e7-118">The ASP.NET Core 2.x project templates implicitly set `MvcRazorCompileOnPublish` to `true` by default, which means this node can be safely removed from the *.csproj* file.</span></span> <span data-ttu-id="406e7-119">Si prefiere sea explícito, no hay peligro en configuración de la `MvcRazorCompileOnPublish` propiedad `true`.</span><span class="sxs-lookup"><span data-stu-id="406e7-119">If you prefer to be explicit, there's no harm in setting the `MvcRazorCompileOnPublish` property to `true`.</span></span> <span data-ttu-id="406e7-120">El siguiente *.csproj* ejemplo resalta esta configuración:</span><span class="sxs-lookup"><span data-stu-id="406e7-120">The following *.csproj* sample highlights this setting:</span></span>

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=5)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="406e7-121">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="406e7-121">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="406e7-122">Establecer `MvcRazorCompileOnPublish` a `true`e incluir una referencia de paquete para `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`.</span><span class="sxs-lookup"><span data-stu-id="406e7-122">Set `MvcRazorCompileOnPublish` to `true`, and include a package reference to `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`.</span></span> <span data-ttu-id="406e7-123">El siguiente *.csproj* ejemplo resalta estas opciones:</span><span class="sxs-lookup"><span data-stu-id="406e7-123">The following *.csproj* sample highlights these settings:</span></span>

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish.csproj?highlight=5,12)]

---
