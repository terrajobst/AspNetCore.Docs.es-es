---
title: ASP.NET Core Blazor plataformas admitidas
author: guardrex
description: Obtenga información acerca de las plataformas admitidas para ASP.NET Core Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
no-loc:
- Blazor
uid: blazor/supported-platforms
ms.openlocfilehash: de51296cc8785474e1c1406cfd5d4e5bd4050172
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2019
ms.locfileid: "73962737"
---
# <a name="aspnet-core-opno-locblazor-supported-platforms"></a><span data-ttu-id="94430-103">ASP.NET Core Blazor plataformas admitidas</span><span class="sxs-lookup"><span data-stu-id="94430-103">ASP.NET Core Blazor supported platforms</span></span>

<span data-ttu-id="94430-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="94430-104">By [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

## <a name="browser-requirements"></a><span data-ttu-id="94430-105">Requisitos del explorador</span><span class="sxs-lookup"><span data-stu-id="94430-105">Browser requirements</span></span>

### <a name="opno-locblazor-webassembly"></a>Blazor<span data-ttu-id="94430-106"> webassembly</span><span class="sxs-lookup"><span data-stu-id="94430-106"> WebAssembly</span></span>

| <span data-ttu-id="94430-107">Explorador</span><span class="sxs-lookup"><span data-stu-id="94430-107">Browser</span></span>                          | <span data-ttu-id="94430-108">Versión</span><span class="sxs-lookup"><span data-stu-id="94430-108">Version</span></span>               |
| -------------------------------- | :-------------------: |
| <span data-ttu-id="94430-109">Microsoft Edge</span><span class="sxs-lookup"><span data-stu-id="94430-109">Microsoft Edge</span></span>                   | <span data-ttu-id="94430-110">Current</span><span class="sxs-lookup"><span data-stu-id="94430-110">Current</span></span>               |
| <span data-ttu-id="94430-111">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="94430-111">Mozilla Firefox</span></span>                  | <span data-ttu-id="94430-112">Current</span><span class="sxs-lookup"><span data-stu-id="94430-112">Current</span></span>               |
| <span data-ttu-id="94430-113">Google Chrome, incluido Android</span><span class="sxs-lookup"><span data-stu-id="94430-113">Google Chrome, including Android</span></span> | <span data-ttu-id="94430-114">Current</span><span class="sxs-lookup"><span data-stu-id="94430-114">Current</span></span>               |
| <span data-ttu-id="94430-115">Safari, incluido iOS</span><span class="sxs-lookup"><span data-stu-id="94430-115">Safari, including iOS</span></span>            | <span data-ttu-id="94430-116">Current</span><span class="sxs-lookup"><span data-stu-id="94430-116">Current</span></span>               |
| <span data-ttu-id="94430-117">Microsoft Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="94430-117">Microsoft Internet Explorer</span></span>      | <span data-ttu-id="94430-118">No compatible&dagger;</span><span class="sxs-lookup"><span data-stu-id="94430-118">Not Supported&dagger;</span></span> |

<span data-ttu-id="94430-119">&dagger;Microsoft Internet Explorer no es compatible con [Webassembly](https://webassembly.org).</span><span class="sxs-lookup"><span data-stu-id="94430-119">&dagger;Microsoft Internet Explorer doesn't support [WebAssembly](https://webassembly.org).</span></span>

### <a name="opno-locblazor-server"></a><span data-ttu-id="94430-120">Servidor de Blazor</span><span class="sxs-lookup"><span data-stu-id="94430-120">Blazor Server</span></span>

| <span data-ttu-id="94430-121">Explorador</span><span class="sxs-lookup"><span data-stu-id="94430-121">Browser</span></span>                          | <span data-ttu-id="94430-122">Versión</span><span class="sxs-lookup"><span data-stu-id="94430-122">Version</span></span>    |
| -------------------------------- | :--------: |
| <span data-ttu-id="94430-123">Microsoft Edge</span><span class="sxs-lookup"><span data-stu-id="94430-123">Microsoft Edge</span></span>                   | <span data-ttu-id="94430-124">Current</span><span class="sxs-lookup"><span data-stu-id="94430-124">Current</span></span>    |
| <span data-ttu-id="94430-125">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="94430-125">Mozilla Firefox</span></span>                  | <span data-ttu-id="94430-126">Current</span><span class="sxs-lookup"><span data-stu-id="94430-126">Current</span></span>    |
| <span data-ttu-id="94430-127">Google Chrome, incluido Android</span><span class="sxs-lookup"><span data-stu-id="94430-127">Google Chrome, including Android</span></span> | <span data-ttu-id="94430-128">Current</span><span class="sxs-lookup"><span data-stu-id="94430-128">Current</span></span>    |
| <span data-ttu-id="94430-129">Safari, incluido iOS</span><span class="sxs-lookup"><span data-stu-id="94430-129">Safari, including iOS</span></span>            | <span data-ttu-id="94430-130">Current</span><span class="sxs-lookup"><span data-stu-id="94430-130">Current</span></span>    |
| <span data-ttu-id="94430-131">Microsoft Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="94430-131">Microsoft Internet Explorer</span></span>      | <span data-ttu-id="94430-132">11&dagger;</span><span class="sxs-lookup"><span data-stu-id="94430-132">11&dagger;</span></span> |

<span data-ttu-id="94430-133">se requieren &dagger;polyfill adicionales (por ejemplo, se pueden agregar promesas a través de un paquete de [polyfill.IO](https://polyfill.io/v3/) ).</span><span class="sxs-lookup"><span data-stu-id="94430-133">&dagger;Additional polyfills are required (for example, promises can be added via a [Polyfill.io](https://polyfill.io/v3/) bundle).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="94430-134">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="94430-134">Additional resources</span></span>

* <xref:blazor/hosting-models>
