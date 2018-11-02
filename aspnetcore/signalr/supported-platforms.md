---
title: Plataformas compatibles con ASP.NET Core SignalR
author: tdykstra
description: Obtenga información acerca de las plataformas admitidas para ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 10/31/2018
uid: signalr/supported-platforms
ms.openlocfilehash: 773f6c020dbb2982911e177b55855473c750d52a
ms.sourcegitcommit: fc2486ddbeb15ab4969168d99b3fe0fbe91e8661
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/01/2018
ms.locfileid: "50758185"
---
# <a name="aspnet-core-signalr-supported-platforms"></a><span data-ttu-id="d7acb-103">Plataformas compatibles con ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="d7acb-103">ASP.NET Core SignalR supported platforms</span></span>

## <a name="server-system-requirements"></a><span data-ttu-id="d7acb-104">Requisitos del sistema de servidor</span><span class="sxs-lookup"><span data-stu-id="d7acb-104">Server system requirements</span></span>

<span data-ttu-id="d7acb-105">SignalR para ASP.NET Core es compatible con cualquier plataforma de servidor ASP.NET Core admite.</span><span class="sxs-lookup"><span data-stu-id="d7acb-105">SignalR for ASP.NET Core supports any server platform that ASP.NET Core supports.</span></span>

## <a name="javascript-client"></a><span data-ttu-id="d7acb-106">Cliente de JavaScript</span><span class="sxs-lookup"><span data-stu-id="d7acb-106">JavaScript client</span></span>

<span data-ttu-id="d7acb-107">El [cliente JavaScript](https://www.npmjs.com/package/@aspnet/signalr) se ejecuta en NodeJS 8 y versiones posteriores y los siguientes exploradores:</span><span class="sxs-lookup"><span data-stu-id="d7acb-107">The [JavaScript client](https://www.npmjs.com/package/@aspnet/signalr) runs on NodeJS 8 and later versions and the following browsers:</span></span>

| <span data-ttu-id="d7acb-108">Explorador</span><span class="sxs-lookup"><span data-stu-id="d7acb-108">Browser</span></span>                         | <span data-ttu-id="d7acb-109">Versión</span><span class="sxs-lookup"><span data-stu-id="d7acb-109">Version</span></span> |
| ------------------------------- | ------- |
| <span data-ttu-id="d7acb-110">Microsoft Edge</span><span class="sxs-lookup"><span data-stu-id="d7acb-110">Microsoft Edge</span></span>                  | <span data-ttu-id="d7acb-111">actuales</span><span class="sxs-lookup"><span data-stu-id="d7acb-111">current</span></span> |
| <span data-ttu-id="d7acb-112">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="d7acb-112">Mozilla Firefox</span></span>                 | <span data-ttu-id="d7acb-113">actuales</span><span class="sxs-lookup"><span data-stu-id="d7acb-113">current</span></span> |
| <span data-ttu-id="d7acb-114">Google Chrome; incluye Android</span><span class="sxs-lookup"><span data-stu-id="d7acb-114">Google Chrome; includes Android</span></span> | <span data-ttu-id="d7acb-115">actuales</span><span class="sxs-lookup"><span data-stu-id="d7acb-115">current</span></span> |
| <span data-ttu-id="d7acb-116">Safari; incluye iOS</span><span class="sxs-lookup"><span data-stu-id="d7acb-116">Safari; includes iOS</span></span>            | <span data-ttu-id="d7acb-117">actuales</span><span class="sxs-lookup"><span data-stu-id="d7acb-117">current</span></span> |
| <span data-ttu-id="d7acb-118">Microsoft Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="d7acb-118">Microsoft Internet Explorer</span></span>     | <span data-ttu-id="d7acb-119">11</span><span class="sxs-lookup"><span data-stu-id="d7acb-119">11</span></span>      |
 
## <a name="net-client"></a><span data-ttu-id="d7acb-120">Cliente .NET</span><span class="sxs-lookup"><span data-stu-id="d7acb-120">.NET client</span></span>

<span data-ttu-id="d7acb-121">El [cliente.NET](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) se ejecuta en cualquier plataforma de servidor compatible con ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d7acb-121">The [.NET client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) runs on any server platform supported by ASP.NET Core.</span></span>

<span data-ttu-id="d7acb-122">Si el servidor ejecuta IIS, el transporte de WebSockets requiere IIS 8.0 o posterior en Windows Server 2012 o posterior.</span><span class="sxs-lookup"><span data-stu-id="d7acb-122">If the server runs IIS, the WebSockets transport requires IIS 8.0 or higher on Windows Server 2012 or higher.</span></span> <span data-ttu-id="d7acb-123">Otros transportes se admiten en todas las plataformas.</span><span class="sxs-lookup"><span data-stu-id="d7acb-123">Other transports are supported on all platforms.</span></span>

## <a name="java-client"></a><span data-ttu-id="d7acb-124">Cliente de Java</span><span class="sxs-lookup"><span data-stu-id="d7acb-124">Java client</span></span>

<span data-ttu-id="d7acb-125">El [cliente Java](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) admite Java 8 y versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="d7acb-125">The [Java client](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) supports Java 8 and later versions.</span></span>

## <a name="unsupported-clients"></a><span data-ttu-id="d7acb-126">Clientes no compatibles</span><span class="sxs-lookup"><span data-stu-id="d7acb-126">Unsupported clients</span></span>

<span data-ttu-id="d7acb-127">Los clientes siguientes están disponibles, pero son experimentales o no oficiales.</span><span class="sxs-lookup"><span data-stu-id="d7acb-127">The following clients are available but are experimental or unofficial.</span></span> <span data-ttu-id="d7acb-128">Que no se admiten actualmente y nunca pueden ser.</span><span class="sxs-lookup"><span data-stu-id="d7acb-128">They aren't currently supported and may never be.</span></span>

* [<span data-ttu-id="d7acb-129">Cliente de C++</span><span class="sxs-lookup"><span data-stu-id="d7acb-129">C++ client</span></span>](https://github.com/aspnet/SignalR/tree/master/clients/cpp)

* [<span data-ttu-id="d7acb-130">Cliente de SWIFT</span><span class="sxs-lookup"><span data-stu-id="d7acb-130">Swift client</span></span>](https://github.com/moozzyk/SignalR-Client-Swift)
