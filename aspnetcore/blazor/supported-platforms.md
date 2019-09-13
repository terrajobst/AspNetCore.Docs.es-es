---
title: ASP.NET Core las plataformas compatibles con el increíble
author: guardrex
description: Obtenga información sobre las plataformas admitidas para ASP.NET Core increíblemente.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/01/2019
uid: blazor/supported-platforms
ms.openlocfilehash: 042fbb1b2c7f92b7dc6443319f3f195a12a55adc
ms.sourcegitcommit: 092061c4f6ef46ed2165fa84de6273d3786fb97e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2019
ms.locfileid: "70963879"
---
# <a name="aspnet-core-blazor-supported-platforms"></a><span data-ttu-id="c008e-103">ASP.NET Core las plataformas compatibles con el increíble</span><span class="sxs-lookup"><span data-stu-id="c008e-103">ASP.NET Core Blazor supported platforms</span></span>

<span data-ttu-id="c008e-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="c008e-104">By [Luke Latham](https://github.com/guardrex)</span></span>

## <a name="browser-requirements"></a><span data-ttu-id="c008e-105">Requisitos del explorador</span><span class="sxs-lookup"><span data-stu-id="c008e-105">Browser requirements</span></span>

### <a name="blazor-webassembly"></a><span data-ttu-id="c008e-106">Webassembly increíblemente</span><span class="sxs-lookup"><span data-stu-id="c008e-106">Blazor WebAssembly</span></span>

| <span data-ttu-id="c008e-107">Browser</span><span class="sxs-lookup"><span data-stu-id="c008e-107">Browser</span></span>                          | <span data-ttu-id="c008e-108">`Version`</span><span class="sxs-lookup"><span data-stu-id="c008e-108">Version</span></span>               |
| -------------------------------- | :-------------------: |
| <span data-ttu-id="c008e-109">Microsoft Edge</span><span class="sxs-lookup"><span data-stu-id="c008e-109">Microsoft Edge</span></span>                   | <span data-ttu-id="c008e-110">Current</span><span class="sxs-lookup"><span data-stu-id="c008e-110">Current</span></span>               |
| <span data-ttu-id="c008e-111">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="c008e-111">Mozilla Firefox</span></span>                  | <span data-ttu-id="c008e-112">Current</span><span class="sxs-lookup"><span data-stu-id="c008e-112">Current</span></span>               |
| <span data-ttu-id="c008e-113">Google Chrome, incluido Android</span><span class="sxs-lookup"><span data-stu-id="c008e-113">Google Chrome, including Android</span></span> | <span data-ttu-id="c008e-114">Current</span><span class="sxs-lookup"><span data-stu-id="c008e-114">Current</span></span>               |
| <span data-ttu-id="c008e-115">Safari, incluido iOS</span><span class="sxs-lookup"><span data-stu-id="c008e-115">Safari, including iOS</span></span>            | <span data-ttu-id="c008e-116">Current</span><span class="sxs-lookup"><span data-stu-id="c008e-116">Current</span></span>               |
| <span data-ttu-id="c008e-117">Microsoft Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="c008e-117">Microsoft Internet Explorer</span></span>      | <span data-ttu-id="c008e-118">No compatible&dagger;</span><span class="sxs-lookup"><span data-stu-id="c008e-118">Not Supported&dagger;</span></span> |

<span data-ttu-id="c008e-119">&dagger;Microsoft Internet Explorer no es compatible con [WebAssembly](https://webassembly.org).</span><span class="sxs-lookup"><span data-stu-id="c008e-119">&dagger;Microsoft Internet Explorer doesn't support [WebAssembly](https://webassembly.org).</span></span>

### <a name="blazor-server"></a><span data-ttu-id="c008e-120">Servidor increíble</span><span class="sxs-lookup"><span data-stu-id="c008e-120">Blazor Server</span></span>

| <span data-ttu-id="c008e-121">Browser</span><span class="sxs-lookup"><span data-stu-id="c008e-121">Browser</span></span>                          | <span data-ttu-id="c008e-122">`Version`</span><span class="sxs-lookup"><span data-stu-id="c008e-122">Version</span></span>    |
| -------------------------------- | :--------: |
| <span data-ttu-id="c008e-123">Microsoft Edge</span><span class="sxs-lookup"><span data-stu-id="c008e-123">Microsoft Edge</span></span>                   | <span data-ttu-id="c008e-124">Current</span><span class="sxs-lookup"><span data-stu-id="c008e-124">Current</span></span>    |
| <span data-ttu-id="c008e-125">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="c008e-125">Mozilla Firefox</span></span>                  | <span data-ttu-id="c008e-126">Current</span><span class="sxs-lookup"><span data-stu-id="c008e-126">Current</span></span>    |
| <span data-ttu-id="c008e-127">Google Chrome, incluido Android</span><span class="sxs-lookup"><span data-stu-id="c008e-127">Google Chrome, including Android</span></span> | <span data-ttu-id="c008e-128">Current</span><span class="sxs-lookup"><span data-stu-id="c008e-128">Current</span></span>    |
| <span data-ttu-id="c008e-129">Safari, incluido iOS</span><span class="sxs-lookup"><span data-stu-id="c008e-129">Safari, including iOS</span></span>            | <span data-ttu-id="c008e-130">Current</span><span class="sxs-lookup"><span data-stu-id="c008e-130">Current</span></span>    |
| <span data-ttu-id="c008e-131">Microsoft Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="c008e-131">Microsoft Internet Explorer</span></span>      | <span data-ttu-id="c008e-132">279&dagger;</span><span class="sxs-lookup"><span data-stu-id="c008e-132">11&dagger;</span></span> |

<span data-ttu-id="c008e-133">&dagger;Se requieren polyfill adicionales (por ejemplo, se pueden agregar promesas a través de un paquete de [polyfill.IO](https://polyfill.io/v3/) ).</span><span class="sxs-lookup"><span data-stu-id="c008e-133">&dagger;Additional polyfills are required (for example, promises can be added via a [Polyfill.io](https://polyfill.io/v3/) bundle).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c008e-134">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="c008e-134">Additional resources</span></span>

* <xref:blazor/hosting-models>
