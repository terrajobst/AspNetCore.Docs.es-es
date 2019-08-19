---
title: ASP.NET Core las plataformas compatibles con el increíble
author: guardrex
description: Obtenga información sobre las plataformas admitidas para ASP.NET Core increíblemente.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/01/2019
uid: blazor/supported-platforms
ms.openlocfilehash: 01f3a55a8536feedf713e07ea3724a0bc51e7c63
ms.sourcegitcommit: 7a40c56bf6a6aaa63a7ee83a2cac9b3a1d77555e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/12/2019
ms.locfileid: "68948255"
---
# <a name="aspnet-core-blazor-supported-platforms"></a><span data-ttu-id="3e285-103">ASP.NET Core las plataformas compatibles con el increíble</span><span class="sxs-lookup"><span data-stu-id="3e285-103">ASP.NET Core Blazor supported platforms</span></span>

<span data-ttu-id="3e285-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="3e285-104">By [Luke Latham](https://github.com/guardrex)</span></span>

## <a name="browser-requirements"></a><span data-ttu-id="3e285-105">Requisitos del explorador</span><span class="sxs-lookup"><span data-stu-id="3e285-105">Browser requirements</span></span>

### <a name="blazor-client-side"></a><span data-ttu-id="3e285-106">Lado cliente de Blazor</span><span class="sxs-lookup"><span data-stu-id="3e285-106">Blazor client-side</span></span>

| <span data-ttu-id="3e285-107">Browser</span><span class="sxs-lookup"><span data-stu-id="3e285-107">Browser</span></span>                          | <span data-ttu-id="3e285-108">`Version`</span><span class="sxs-lookup"><span data-stu-id="3e285-108">Version</span></span>               |
| -------------------------------- | :-------------------: |
| <span data-ttu-id="3e285-109">Microsoft Edge</span><span class="sxs-lookup"><span data-stu-id="3e285-109">Microsoft Edge</span></span>                   | <span data-ttu-id="3e285-110">Current</span><span class="sxs-lookup"><span data-stu-id="3e285-110">Current</span></span>               |
| <span data-ttu-id="3e285-111">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="3e285-111">Mozilla Firefox</span></span>                  | <span data-ttu-id="3e285-112">Current</span><span class="sxs-lookup"><span data-stu-id="3e285-112">Current</span></span>               |
| <span data-ttu-id="3e285-113">Google Chrome, incluido Android</span><span class="sxs-lookup"><span data-stu-id="3e285-113">Google Chrome, including Android</span></span> | <span data-ttu-id="3e285-114">Current</span><span class="sxs-lookup"><span data-stu-id="3e285-114">Current</span></span>               |
| <span data-ttu-id="3e285-115">Safari, incluido iOS</span><span class="sxs-lookup"><span data-stu-id="3e285-115">Safari, including iOS</span></span>            | <span data-ttu-id="3e285-116">Current</span><span class="sxs-lookup"><span data-stu-id="3e285-116">Current</span></span>               |
| <span data-ttu-id="3e285-117">Microsoft Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="3e285-117">Microsoft Internet Explorer</span></span>      | <span data-ttu-id="3e285-118">No compatible&dagger;</span><span class="sxs-lookup"><span data-stu-id="3e285-118">Not Supported&dagger;</span></span> |

<span data-ttu-id="3e285-119">&dagger;Microsoft Internet Explorer no es compatible con [WebAssembly](https://webassembly.org).</span><span class="sxs-lookup"><span data-stu-id="3e285-119">&dagger;Microsoft Internet Explorer doesn't support [WebAssembly](https://webassembly.org).</span></span>

### <a name="blazor-server-side"></a><span data-ttu-id="3e285-120">Lado servidor de Blazor</span><span class="sxs-lookup"><span data-stu-id="3e285-120">Blazor server-side</span></span>

| <span data-ttu-id="3e285-121">Browser</span><span class="sxs-lookup"><span data-stu-id="3e285-121">Browser</span></span>                          | <span data-ttu-id="3e285-122">`Version`</span><span class="sxs-lookup"><span data-stu-id="3e285-122">Version</span></span>    |
| -------------------------------- | :--------: |
| <span data-ttu-id="3e285-123">Microsoft Edge</span><span class="sxs-lookup"><span data-stu-id="3e285-123">Microsoft Edge</span></span>                   | <span data-ttu-id="3e285-124">Current</span><span class="sxs-lookup"><span data-stu-id="3e285-124">Current</span></span>    |
| <span data-ttu-id="3e285-125">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="3e285-125">Mozilla Firefox</span></span>                  | <span data-ttu-id="3e285-126">Current</span><span class="sxs-lookup"><span data-stu-id="3e285-126">Current</span></span>    |
| <span data-ttu-id="3e285-127">Google Chrome, incluido Android</span><span class="sxs-lookup"><span data-stu-id="3e285-127">Google Chrome, including Android</span></span> | <span data-ttu-id="3e285-128">Current</span><span class="sxs-lookup"><span data-stu-id="3e285-128">Current</span></span>    |
| <span data-ttu-id="3e285-129">Safari, incluido iOS</span><span class="sxs-lookup"><span data-stu-id="3e285-129">Safari, including iOS</span></span>            | <span data-ttu-id="3e285-130">Current</span><span class="sxs-lookup"><span data-stu-id="3e285-130">Current</span></span>    |
| <span data-ttu-id="3e285-131">Microsoft Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="3e285-131">Microsoft Internet Explorer</span></span>      | <span data-ttu-id="3e285-132">279&dagger;</span><span class="sxs-lookup"><span data-stu-id="3e285-132">11&dagger;</span></span> |

<span data-ttu-id="3e285-133">&dagger;Se requieren polyfill adicionales (por ejemplo, se pueden agregar promesas a través de un paquete de [polyfill.IO](https://polyfill.io/v3/) ).</span><span class="sxs-lookup"><span data-stu-id="3e285-133">&dagger;Additional polyfills are required (for example, promises can be added via a [Polyfill.io](https://polyfill.io/v3/) bundle).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3e285-134">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="3e285-134">Additional resources</span></span>

* <xref:blazor/hosting-models>
