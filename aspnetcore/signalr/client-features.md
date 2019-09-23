---
title: Características de cliente de signalr
author: bradygaster
description: Obtenga información sobre qué características son compatibles con los distintos clientes de Signalr ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: bradyg
ms.custom: mvc
ms.date: 09/18/2019
uid: signalr/client-features
ms.openlocfilehash: 55086673e0c9f9b73f07730ea25c3fa322f7fd98
ms.sourcegitcommit: d34b2627a69bc8940b76a949de830335db9701d3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/23/2019
ms.locfileid: "71187393"
---
# <a name="aspnet-core-signalr-client-features"></a><span data-ttu-id="e14e1-103">Características de cliente de Signalr ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e14e1-103">ASP.NET Core SignalR client features</span></span>

## <a name="feature-distribution"></a><span data-ttu-id="e14e1-104">Distribución de características</span><span class="sxs-lookup"><span data-stu-id="e14e1-104">Feature distribution</span></span>

<span data-ttu-id="e14e1-105">En la tabla siguiente se muestran las características y la compatibilidad de los clientes que ofrecen compatibilidad en tiempo real.</span><span class="sxs-lookup"><span data-stu-id="e14e1-105">The table below shows the features and support for the clients that offer real-time support.</span></span>

| <span data-ttu-id="e14e1-106">Característica</span><span class="sxs-lookup"><span data-stu-id="e14e1-106">Feature</span></span> | <span data-ttu-id="e14e1-107">Núcleo de .NET</span><span class="sxs-lookup"><span data-stu-id="e14e1-107">.NET Core</span></span> | <span data-ttu-id="e14e1-108">JavaScript</span><span class="sxs-lookup"><span data-stu-id="e14e1-108">JavaScript</span></span> | <span data-ttu-id="e14e1-109">Java</span><span class="sxs-lookup"><span data-stu-id="e14e1-109">Java</span></span> |
| ---- | :-: | :-: | :-: |
| [<span data-ttu-id="e14e1-110">Streaming de servidor a cliente</span><span class="sxs-lookup"><span data-stu-id="e14e1-110">Server-to-client Streaming</span></span>](xref:signalr/streaming)          |<span data-ttu-id="e14e1-111">✔</span><span class="sxs-lookup"><span data-stu-id="e14e1-111">✔</span></span>|<span data-ttu-id="e14e1-112">✔</span><span class="sxs-lookup"><span data-stu-id="e14e1-112">✔</span></span>|<span data-ttu-id="e14e1-113">✔</span><span class="sxs-lookup"><span data-stu-id="e14e1-113">✔</span></span>|
| [<span data-ttu-id="e14e1-114">Streaming de cliente a servidor</span><span class="sxs-lookup"><span data-stu-id="e14e1-114">Client-to-server Streaming</span></span>](xref:signalr/streaming)          |<span data-ttu-id="e14e1-115">✔</span><span class="sxs-lookup"><span data-stu-id="e14e1-115">✔</span></span>|<span data-ttu-id="e14e1-116">✔</span><span class="sxs-lookup"><span data-stu-id="e14e1-116">✔</span></span>|<span data-ttu-id="e14e1-117">✔</span><span class="sxs-lookup"><span data-stu-id="e14e1-117">✔</span></span>|
| <span data-ttu-id="e14e1-118">Reconexión automática ([.net](/aspnet/core/signalr/dotnet-client?view=aspnetcore-3.0&tabs=visual-studio#handle-lost-connection), [JavaScript](/aspnet/core/signalr/javascript-client?view=aspnetcore-3.0#reconnect-clients))</span><span class="sxs-lookup"><span data-stu-id="e14e1-118">Automatic Reconnection ([.NET](/aspnet/core/signalr/dotnet-client?view=aspnetcore-3.0&tabs=visual-studio#handle-lost-connection), [JavaScript](/aspnet/core/signalr/javascript-client?view=aspnetcore-3.0#reconnect-clients))</span></span>          |<span data-ttu-id="e14e1-119">✔</span><span class="sxs-lookup"><span data-stu-id="e14e1-119">✔</span></span>|<span data-ttu-id="e14e1-120">✔</span><span class="sxs-lookup"><span data-stu-id="e14e1-120">✔</span></span>| |

## <a name="additional-resources"></a><span data-ttu-id="e14e1-121">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="e14e1-121">Additional resources</span></span>

* [<span data-ttu-id="e14e1-122">Introducción a Signalr para ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e14e1-122">Get started with SignalR for ASP.NET Core</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="e14e1-123">Plataformas compatibles</span><span class="sxs-lookup"><span data-stu-id="e14e1-123">Supported platforms</span></span>](xref:signalr/supported-platforms)
* [<span data-ttu-id="e14e1-124">Concentradores</span><span class="sxs-lookup"><span data-stu-id="e14e1-124">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="e14e1-125">Cliente de JavaScript</span><span class="sxs-lookup"><span data-stu-id="e14e1-125">JavaScript client</span></span>](xref:signalr/javascript-client)
