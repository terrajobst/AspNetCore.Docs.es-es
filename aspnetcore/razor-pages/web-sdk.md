---
title: SDK Web de ASP.NET Core
author: Rick-Anderson
description: Información general de Microsoft. NET. SDK. Web.
ms.author: riande
ms.date: 01/25/2020
no-loc:
- Blazor
uid: razor-pages/web-sdk
ms.openlocfilehash: 6a9d531efd2188aed525c949bb124914c31119db
ms.sourcegitcommit: fe41cff0b99f3920b727286944e5b652ca301640
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2020
ms.locfileid: "76869771"
---
# <a name="aspnet-core-web-sdk"></a><span data-ttu-id="28858-103">SDK Web de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="28858-103">ASP.NET Core Web SDK</span></span>

### <a name="overview"></a><span data-ttu-id="28858-104">Información general del</span><span class="sxs-lookup"><span data-stu-id="28858-104">Overview</span></span>

<span data-ttu-id="28858-105">`Microsoft.NET.Sdk.Web` es un [SDK de proyecto de MSBuild](https://docs.microsoft.com/visualstudio/msbuild/how-to-use-project-sdk) para compilar aplicaciones de ASP.net Core.</span><span class="sxs-lookup"><span data-stu-id="28858-105">`Microsoft.NET.Sdk.Web` is an [MSBuild project SDK](https://docs.microsoft.com/visualstudio/msbuild/how-to-use-project-sdk) for building ASP.NET Core apps.</span></span> <span data-ttu-id="28858-106">No obstante, es posible compilar una aplicación ASP.NET Core sin este SDK:</span><span class="sxs-lookup"><span data-stu-id="28858-106">It's possible to build an ASP.NET Core app without this SDK, however, the Web SDK is:</span></span>

* <span data-ttu-id="28858-107">Adaptado para proporcionar una experiencia de primera clase.</span><span class="sxs-lookup"><span data-stu-id="28858-107">Tailored towards providing a first-class experience.</span></span>
* <span data-ttu-id="28858-108">El destino recomendado para la mayoría de los usuarios.</span><span class="sxs-lookup"><span data-stu-id="28858-108">The recommended target for most users.</span></span>

<span data-ttu-id="28858-109">Usar Web. SDK en un proyecto:</span><span class="sxs-lookup"><span data-stu-id="28858-109">Use the Web.SDK in a project:</span></span>

  ```xml
  <Project Sdk="Microsoft.NET.Sdk.Web">
    <!-- omitted for brevity -->
  </Project>
  ```

<span data-ttu-id="28858-110">Características habilitadas mediante el SDK Web:</span><span class="sxs-lookup"><span data-stu-id="28858-110">Features enabled by using the Web SDK:</span></span>

* <span data-ttu-id="28858-111">Los proyectos que tienen como destino .NET Core 3,0 o posterior hacen referencia implícitamente:</span><span class="sxs-lookup"><span data-stu-id="28858-111">Projects targeting .NET Core 3.0 or later implicitly reference:</span></span>

  * <span data-ttu-id="28858-112">[ASP.net Core marco de trabajo compartido](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="28858-112">The [ASP.NET Core shared framework](xref:fundamentals/metapackage-app).</span></span>
  * <span data-ttu-id="28858-113">[Analizadores](/visualstudio/extensibility/getting-started-with-roslyn-analyzers) diseñados para compilar ASP.net Core aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="28858-113">[Analyzers](/visualstudio/extensibility/getting-started-with-roslyn-analyzers) designed for building ASP.NET Core apps.</span></span>
* <span data-ttu-id="28858-114">El SDK web importa los destinos de MSBuild que habilitan el uso de perfiles de publicación y publicación con WebDeploy.</span><span class="sxs-lookup"><span data-stu-id="28858-114">The Web SDK imports MSBuild targets that enable the use of publish profiles and publishing using WebDeploy.</span></span>

### <a name="properties"></a><span data-ttu-id="28858-115">Propiedades</span><span class="sxs-lookup"><span data-stu-id="28858-115">Properties</span></span>

| <span data-ttu-id="28858-116">La propiedad</span><span class="sxs-lookup"><span data-stu-id="28858-116">Property</span></span> | <span data-ttu-id="28858-117">Descripción</span><span class="sxs-lookup"><span data-stu-id="28858-117">Description</span></span> |
| -------- | ----------- |
| `DisableImplicitFrameworkReferences` | <span data-ttu-id="28858-118">Deshabilita la referencia implícita a la `Microsoft.AspNetCore.App` marco de trabajo compartido.</span><span class="sxs-lookup"><span data-stu-id="28858-118">Disables implicit reference to the `Microsoft.AspNetCore.App` shared framework.</span></span> |
| `DisableImplicitAspNetCoreAnalyzers` | <span data-ttu-id="28858-119">Deshabilita la referencia implícita a los analizadores de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="28858-119">Disables implicit reference to ASP.NET Core analyzers.</span></span> |
| `DisableImplicitComponentsAnalyzers` | <span data-ttu-id="28858-120">Deshabilita la referencia implícita a los analizadores de componentes de Razor al compilar aplicaciones Blazor (servidor).</span><span class="sxs-lookup"><span data-stu-id="28858-120">Disables implicit reference to Razor Components analyzers when building Blazor (server) applications.</span></span> |
