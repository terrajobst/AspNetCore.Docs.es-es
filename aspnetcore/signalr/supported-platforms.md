---
title: Plataformas compatibles con Signalr ASP.NET Core
author: bradygaster
description: Obtenga información sobre las plataformas admitidas para ASP.NET Core Signalr.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/01/2019
uid: signalr/supported-platforms
ms.openlocfilehash: 1be7a307710e6e522c0088fd1ca01da11a13eda1
ms.sourcegitcommit: 77c8be22d5e88dd710f42c739748869f198865dd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/01/2019
ms.locfileid: "73426973"
---
# <a name="aspnet-core-signalr-supported-platforms"></a><span data-ttu-id="40e39-103">Plataformas compatibles con Signalr ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="40e39-103">ASP.NET Core SignalR supported platforms</span></span>

## <a name="server-system-requirements"></a><span data-ttu-id="40e39-104">Requisitos del sistema de servidor</span><span class="sxs-lookup"><span data-stu-id="40e39-104">Server system requirements</span></span>

<span data-ttu-id="40e39-105">Signalr para ASP.NET Core admite cualquier plataforma de servidor que ASP.NET Core admita.</span><span class="sxs-lookup"><span data-stu-id="40e39-105">SignalR for ASP.NET Core supports any server platform that ASP.NET Core supports.</span></span>

## <a name="javascript-client"></a><span data-ttu-id="40e39-106">Cliente de JavaScript</span><span class="sxs-lookup"><span data-stu-id="40e39-106">JavaScript client</span></span>

<span data-ttu-id="40e39-107">El [cliente de JavaScript](https://www.npmjs.com/package/@aspnet/signalr) se ejecuta en NodeJS 8 y versiones posteriores, y en los siguientes exploradores:</span><span class="sxs-lookup"><span data-stu-id="40e39-107">The [JavaScript client](https://www.npmjs.com/package/@aspnet/signalr) runs on NodeJS 8 and later versions and the following browsers:</span></span>

| <span data-ttu-id="40e39-108">Explorador</span><span class="sxs-lookup"><span data-stu-id="40e39-108">Browser</span></span>                         | <span data-ttu-id="40e39-109">Versión</span><span class="sxs-lookup"><span data-stu-id="40e39-109">Version</span></span>         |
| ------------------------------- | --------------- |
| <span data-ttu-id="40e39-110">Microsoft Edge</span><span class="sxs-lookup"><span data-stu-id="40e39-110">Microsoft Edge</span></span>                  | <span data-ttu-id="40e39-111">&dagger; actual</span><span class="sxs-lookup"><span data-stu-id="40e39-111">Current&dagger;</span></span> |
| <span data-ttu-id="40e39-112">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="40e39-112">Mozilla Firefox</span></span>                 | <span data-ttu-id="40e39-113">&dagger; actual</span><span class="sxs-lookup"><span data-stu-id="40e39-113">Current&dagger;</span></span> |
| <span data-ttu-id="40e39-114">Google Chrome; incluye Android</span><span class="sxs-lookup"><span data-stu-id="40e39-114">Google Chrome; includes Android</span></span> | <span data-ttu-id="40e39-115">&dagger; actual</span><span class="sxs-lookup"><span data-stu-id="40e39-115">Current&dagger;</span></span> |
| <span data-ttu-id="40e39-116">Safari incluye iOS</span><span class="sxs-lookup"><span data-stu-id="40e39-116">Safari; includes iOS</span></span>            | <span data-ttu-id="40e39-117">&dagger; actual</span><span class="sxs-lookup"><span data-stu-id="40e39-117">Current&dagger;</span></span> |
| <span data-ttu-id="40e39-118">Microsoft Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="40e39-118">Microsoft Internet Explorer</span></span>     | <span data-ttu-id="40e39-119">11</span><span class="sxs-lookup"><span data-stu-id="40e39-119">11</span></span>              |

<span data-ttu-id="40e39-120">&dagger;*actual* hace referencia a la versión más reciente del explorador.</span><span class="sxs-lookup"><span data-stu-id="40e39-120">&dagger;*Current* refers to the latest version of the browser.</span></span>

## <a name="net-client"></a><span data-ttu-id="40e39-121">Cliente .NET</span><span class="sxs-lookup"><span data-stu-id="40e39-121">.NET client</span></span>

<span data-ttu-id="40e39-122">El [cliente .net](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) se ejecuta en cualquier plataforma compatible con ASP.net Core.</span><span class="sxs-lookup"><span data-stu-id="40e39-122">The [.NET client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) runs on any platform supported by ASP.NET Core.</span></span> <span data-ttu-id="40e39-123">Por ejemplo, [los desarrolladores de Xamarin pueden usar signalr](https://github.com/aspnet/Announcements/issues/305) para compilar aplicaciones de Android con Xamarin. Android 8.4.0.1 y las aplicaciones iOS y posteriores con Xamarin. iOS 11.14.0.4 y versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="40e39-123">For example, [Xamarin developers can use SignalR](https://github.com/aspnet/Announcements/issues/305) for building Android apps using Xamarin.Android 8.4.0.1 and later and iOS apps using Xamarin.iOS 11.14.0.4 and later.</span></span>

<span data-ttu-id="40e39-124">Si el servidor ejecuta IIS, el transporte de WebSockets requiere IIS 8,0 o posterior en Windows Server 2012 o posterior.</span><span class="sxs-lookup"><span data-stu-id="40e39-124">If the server runs IIS, the WebSockets transport requires IIS 8.0 or later on Windows Server 2012 or later.</span></span> <span data-ttu-id="40e39-125">Se admiten otros transportes en todas las plataformas.</span><span class="sxs-lookup"><span data-stu-id="40e39-125">Other transports are supported on all platforms.</span></span>

## <a name="java-client"></a><span data-ttu-id="40e39-126">Cliente de Java</span><span class="sxs-lookup"><span data-stu-id="40e39-126">Java client</span></span>

<span data-ttu-id="40e39-127">El [cliente de Java](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) es compatible con Java 8 y versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="40e39-127">The [Java client](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) supports Java 8 and later versions.</span></span>

## <a name="unsupported-clients"></a><span data-ttu-id="40e39-128">Clientes no compatibles</span><span class="sxs-lookup"><span data-stu-id="40e39-128">Unsupported clients</span></span>

<span data-ttu-id="40e39-129">Los siguientes clientes están disponibles pero son experimentales o no oficiales.</span><span class="sxs-lookup"><span data-stu-id="40e39-129">The following clients are available but are experimental or unofficial.</span></span> <span data-ttu-id="40e39-130">No se admiten actualmente y nunca pueden ser.</span><span class="sxs-lookup"><span data-stu-id="40e39-130">They aren't currently supported and may never be.</span></span>

* [<span data-ttu-id="40e39-131">C++Nº</span><span class="sxs-lookup"><span data-stu-id="40e39-131">C++ client</span></span>](https://github.com/aspnet/SignalR/tree/master/clients/cpp)

* [<span data-ttu-id="40e39-132">Cliente SWIFT</span><span class="sxs-lookup"><span data-stu-id="40e39-132">Swift client</span></span>](https://github.com/moozzyk/SignalR-Client-Swift)
