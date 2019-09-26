---
title: Características de cliente de SignalR
author: bradygaster
description: Obtenga información sobre qué características son compatibles con los distintos clientes de Signalr ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: bradyg
ms.custom: mvc
ms.date: 09/18/2019
uid: signalr/client-features
ms.openlocfilehash: 2d6759a5484c37aee6db3d22b3127414231605ae
ms.sourcegitcommit: 14b25156e34c82ed0495b4aff5776ac5b1950b5e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2019
ms.locfileid: "71301186"
---
# <a name="aspnet-core-signalr-client-features"></a><span data-ttu-id="5e31e-103">Características de cliente de Signalr ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5e31e-103">ASP.NET Core SignalR client features</span></span>

## <a name="feature-distribution"></a><span data-ttu-id="5e31e-104">Distribución de características</span><span class="sxs-lookup"><span data-stu-id="5e31e-104">Feature distribution</span></span>

<span data-ttu-id="5e31e-105">En la tabla siguiente se muestran las características y la compatibilidad de los clientes que ofrecen compatibilidad en tiempo real.</span><span class="sxs-lookup"><span data-stu-id="5e31e-105">The table below shows the features and support for the clients that offer real-time support.</span></span>

| <span data-ttu-id="5e31e-106">Característica</span><span class="sxs-lookup"><span data-stu-id="5e31e-106">Feature</span></span> | <span data-ttu-id="5e31e-107">.NET</span><span class="sxs-lookup"><span data-stu-id="5e31e-107">.NET</span></span> | <span data-ttu-id="5e31e-108">JavaScript</span><span class="sxs-lookup"><span data-stu-id="5e31e-108">JavaScript</span></span> | <span data-ttu-id="5e31e-109">Java</span><span class="sxs-lookup"><span data-stu-id="5e31e-109">Java</span></span> |
| ---- | :-: | :-: | :-: |
| <span data-ttu-id="5e31e-110">Compatibilidad con Azure Signalr Service</span><span class="sxs-lookup"><span data-stu-id="5e31e-110">Azure SignalR Service Support</span></span> |<span data-ttu-id="5e31e-111">✔</span><span class="sxs-lookup"><span data-stu-id="5e31e-111">✔</span></span>|<span data-ttu-id="5e31e-112">✔</span><span class="sxs-lookup"><span data-stu-id="5e31e-112">✔</span></span>|<span data-ttu-id="5e31e-113">✔</span><span class="sxs-lookup"><span data-stu-id="5e31e-113">✔</span></span>|
| [<span data-ttu-id="5e31e-114">Streaming de servidor a cliente</span><span class="sxs-lookup"><span data-stu-id="5e31e-114">Server-to-client Streaming</span></span>](xref:signalr/streaming)          |<span data-ttu-id="5e31e-115">✔</span><span class="sxs-lookup"><span data-stu-id="5e31e-115">✔</span></span>|<span data-ttu-id="5e31e-116">✔</span><span class="sxs-lookup"><span data-stu-id="5e31e-116">✔</span></span>|<span data-ttu-id="5e31e-117">✔</span><span class="sxs-lookup"><span data-stu-id="5e31e-117">✔</span></span>|
| [<span data-ttu-id="5e31e-118">Streaming de cliente a servidor</span><span class="sxs-lookup"><span data-stu-id="5e31e-118">Client-to-server Streaming</span></span>](xref:signalr/streaming)          |<span data-ttu-id="5e31e-119">✔</span><span class="sxs-lookup"><span data-stu-id="5e31e-119">✔</span></span>|<span data-ttu-id="5e31e-120">✔</span><span class="sxs-lookup"><span data-stu-id="5e31e-120">✔</span></span>|<span data-ttu-id="5e31e-121">✔</span><span class="sxs-lookup"><span data-stu-id="5e31e-121">✔</span></span>|
| <span data-ttu-id="5e31e-122">Reconexión automática ([.net](/aspnet/core/signalr/dotnet-client?view=aspnetcore-3.0&tabs=visual-studio#handle-lost-connection), [JavaScript](/aspnet/core/signalr/javascript-client?view=aspnetcore-3.0#reconnect-clients))</span><span class="sxs-lookup"><span data-stu-id="5e31e-122">Automatic Reconnection ([.NET](/aspnet/core/signalr/dotnet-client?view=aspnetcore-3.0&tabs=visual-studio#handle-lost-connection), [JavaScript](/aspnet/core/signalr/javascript-client?view=aspnetcore-3.0#reconnect-clients))</span></span>          |<span data-ttu-id="5e31e-123">✔</span><span class="sxs-lookup"><span data-stu-id="5e31e-123">✔</span></span>|<span data-ttu-id="5e31e-124">✔</span><span class="sxs-lookup"><span data-stu-id="5e31e-124">✔</span></span>| |
| <span data-ttu-id="5e31e-125">Transporte de WebSockets</span><span class="sxs-lookup"><span data-stu-id="5e31e-125">WebSockets Transport</span></span> |<span data-ttu-id="5e31e-126">✔</span><span class="sxs-lookup"><span data-stu-id="5e31e-126">✔</span></span>|<span data-ttu-id="5e31e-127">✔</span><span class="sxs-lookup"><span data-stu-id="5e31e-127">✔</span></span>|<span data-ttu-id="5e31e-128">✔</span><span class="sxs-lookup"><span data-stu-id="5e31e-128">✔</span></span>|
| <span data-ttu-id="5e31e-129">Transporte de eventos enviados por servidor</span><span class="sxs-lookup"><span data-stu-id="5e31e-129">Server-Sent Events Transport</span></span> |<span data-ttu-id="5e31e-130">✔</span><span class="sxs-lookup"><span data-stu-id="5e31e-130">✔</span></span>|<span data-ttu-id="5e31e-131">✔</span><span class="sxs-lookup"><span data-stu-id="5e31e-131">✔</span></span>| |
| <span data-ttu-id="5e31e-132">Transporte de sondeo prolongado</span><span class="sxs-lookup"><span data-stu-id="5e31e-132">Long Polling Transport</span></span> |<span data-ttu-id="5e31e-133">✔</span><span class="sxs-lookup"><span data-stu-id="5e31e-133">✔</span></span>|<span data-ttu-id="5e31e-134">✔</span><span class="sxs-lookup"><span data-stu-id="5e31e-134">✔</span></span>|<span data-ttu-id="5e31e-135">✔</span><span class="sxs-lookup"><span data-stu-id="5e31e-135">✔</span></span>|
| <span data-ttu-id="5e31e-136">Protocolo de concentrador de JSON</span><span class="sxs-lookup"><span data-stu-id="5e31e-136">JSON Hub Protocol</span></span> |<span data-ttu-id="5e31e-137">✔</span><span class="sxs-lookup"><span data-stu-id="5e31e-137">✔</span></span>|<span data-ttu-id="5e31e-138">✔</span><span class="sxs-lookup"><span data-stu-id="5e31e-138">✔</span></span>|<span data-ttu-id="5e31e-139">✔</span><span class="sxs-lookup"><span data-stu-id="5e31e-139">✔</span></span>|
| <span data-ttu-id="5e31e-140">Protocolo de concentrador MessagePack</span><span class="sxs-lookup"><span data-stu-id="5e31e-140">MessagePack Hub Protocol</span></span> |<span data-ttu-id="5e31e-141">✔</span><span class="sxs-lookup"><span data-stu-id="5e31e-141">✔</span></span>|<span data-ttu-id="5e31e-142">✔</span><span class="sxs-lookup"><span data-stu-id="5e31e-142">✔</span></span>| |

<span data-ttu-id="5e31e-143">Se realiza un seguimiento de la compatibilidad con la reconexión automática en el cliente de Java en nuestro seguimiento de [problemas](https://github.com/aspnet/AspNetCore/issues/8711).</span><span class="sxs-lookup"><span data-stu-id="5e31e-143">Support for automatic reconnect in the Java client is tracked in [our issue tracker](https://github.com/aspnet/AspNetCore/issues/8711).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5e31e-144">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="5e31e-144">Additional resources</span></span>

* [<span data-ttu-id="5e31e-145">Introducción a Signalr para ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5e31e-145">Get started with SignalR for ASP.NET Core</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="5e31e-146">Plataformas compatibles</span><span class="sxs-lookup"><span data-stu-id="5e31e-146">Supported platforms</span></span>](xref:signalr/supported-platforms)
* [<span data-ttu-id="5e31e-147">Concentradores</span><span class="sxs-lookup"><span data-stu-id="5e31e-147">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="5e31e-148">Cliente de JavaScript</span><span class="sxs-lookup"><span data-stu-id="5e31e-148">JavaScript client</span></span>](xref:signalr/javascript-client)
