---
title: ASP.NET Core las plataformas compatibles con el increíble
author: guardrex
description: Obtenga información sobre las plataformas admitidas para ASP.NET Core increíblemente.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
uid: blazor/supported-platforms
ms.openlocfilehash: 4e86bd6967a747a59c99a515c1c838cc2c21770f
ms.sourcegitcommit: 35a86ce48041caaf6396b1e88b0472578ba24483
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2019
ms.locfileid: "72391216"
---
# <a name="aspnet-core-blazor-supported-platforms"></a><span data-ttu-id="16232-103">ASP.NET Core las plataformas compatibles con el increíble</span><span class="sxs-lookup"><span data-stu-id="16232-103">ASP.NET Core Blazor supported platforms</span></span>

<span data-ttu-id="16232-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="16232-104">By [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

## <a name="browser-requirements"></a><span data-ttu-id="16232-105">Requisitos del explorador</span><span class="sxs-lookup"><span data-stu-id="16232-105">Browser requirements</span></span>

### <a name="blazor-webassembly"></a><span data-ttu-id="16232-106">WebAssembly de Blazor</span><span class="sxs-lookup"><span data-stu-id="16232-106">Blazor WebAssembly</span></span>

| <span data-ttu-id="16232-107">Explorador</span><span class="sxs-lookup"><span data-stu-id="16232-107">Browser</span></span>                          | <span data-ttu-id="16232-108">Versión</span><span class="sxs-lookup"><span data-stu-id="16232-108">Version</span></span>               |
| -------------------------------- | :-------------------: |
| <span data-ttu-id="16232-109">Microsoft Edge</span><span class="sxs-lookup"><span data-stu-id="16232-109">Microsoft Edge</span></span>                   | <span data-ttu-id="16232-110">Current</span><span class="sxs-lookup"><span data-stu-id="16232-110">Current</span></span>               |
| <span data-ttu-id="16232-111">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="16232-111">Mozilla Firefox</span></span>                  | <span data-ttu-id="16232-112">Current</span><span class="sxs-lookup"><span data-stu-id="16232-112">Current</span></span>               |
| <span data-ttu-id="16232-113">Google Chrome, incluido Android</span><span class="sxs-lookup"><span data-stu-id="16232-113">Google Chrome, including Android</span></span> | <span data-ttu-id="16232-114">Current</span><span class="sxs-lookup"><span data-stu-id="16232-114">Current</span></span>               |
| <span data-ttu-id="16232-115">Safari, incluido iOS</span><span class="sxs-lookup"><span data-stu-id="16232-115">Safari, including iOS</span></span>            | <span data-ttu-id="16232-116">Current</span><span class="sxs-lookup"><span data-stu-id="16232-116">Current</span></span>               |
| <span data-ttu-id="16232-117">Microsoft Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="16232-117">Microsoft Internet Explorer</span></span>      | <span data-ttu-id="16232-118">No se admite @ no__t-0</span><span class="sxs-lookup"><span data-stu-id="16232-118">Not Supported&dagger;</span></span> |

<span data-ttu-id="16232-119">@no__t 0Microsoft Internet Explorer no es compatible con [Webassembly](https://webassembly.org).</span><span class="sxs-lookup"><span data-stu-id="16232-119">&dagger;Microsoft Internet Explorer doesn't support [WebAssembly](https://webassembly.org).</span></span>

### <a name="blazor-server"></a><span data-ttu-id="16232-120">Servidor Blazor</span><span class="sxs-lookup"><span data-stu-id="16232-120">Blazor Server</span></span>

| <span data-ttu-id="16232-121">Explorador</span><span class="sxs-lookup"><span data-stu-id="16232-121">Browser</span></span>                          | <span data-ttu-id="16232-122">Versión</span><span class="sxs-lookup"><span data-stu-id="16232-122">Version</span></span>    |
| -------------------------------- | :--------: |
| <span data-ttu-id="16232-123">Microsoft Edge</span><span class="sxs-lookup"><span data-stu-id="16232-123">Microsoft Edge</span></span>                   | <span data-ttu-id="16232-124">Current</span><span class="sxs-lookup"><span data-stu-id="16232-124">Current</span></span>    |
| <span data-ttu-id="16232-125">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="16232-125">Mozilla Firefox</span></span>                  | <span data-ttu-id="16232-126">Current</span><span class="sxs-lookup"><span data-stu-id="16232-126">Current</span></span>    |
| <span data-ttu-id="16232-127">Google Chrome, incluido Android</span><span class="sxs-lookup"><span data-stu-id="16232-127">Google Chrome, including Android</span></span> | <span data-ttu-id="16232-128">Current</span><span class="sxs-lookup"><span data-stu-id="16232-128">Current</span></span>    |
| <span data-ttu-id="16232-129">Safari, incluido iOS</span><span class="sxs-lookup"><span data-stu-id="16232-129">Safari, including iOS</span></span>            | <span data-ttu-id="16232-130">Current</span><span class="sxs-lookup"><span data-stu-id="16232-130">Current</span></span>    |
| <span data-ttu-id="16232-131">Microsoft Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="16232-131">Microsoft Internet Explorer</span></span>      | <span data-ttu-id="16232-132">11 @ no__t-0</span><span class="sxs-lookup"><span data-stu-id="16232-132">11&dagger;</span></span> |

<span data-ttu-id="16232-133">@no__t se requieren poli0Additionals (por ejemplo, se pueden agregar promesas a través de un lote de [polyfill.IO](https://polyfill.io/v3/) ).</span><span class="sxs-lookup"><span data-stu-id="16232-133">&dagger;Additional polyfills are required (for example, promises can be added via a [Polyfill.io](https://polyfill.io/v3/) bundle).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="16232-134">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="16232-134">Additional resources</span></span>

* <xref:blazor/hosting-models>
