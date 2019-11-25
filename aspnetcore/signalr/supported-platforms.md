---
title: ASP.NET Core SignalR plataformas admitidas
author: bradygaster
description: Obtenga información acerca de las plataformas admitidas para ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/21/2019
no-loc:
- SignalR
uid: signalr/supported-platforms
ms.openlocfilehash: 9b9cf1d57d61c333c485f23b7ab952c66814d2aa
ms.sourcegitcommit: 3e503ef510008e77be6dd82ee79213c9f7b97607
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/22/2019
ms.locfileid: "74317465"
---
# <a name="aspnet-core-opno-locsignalr-supported-platforms"></a><span data-ttu-id="250b1-103">ASP.NET Core SignalR plataformas admitidas</span><span class="sxs-lookup"><span data-stu-id="250b1-103">ASP.NET Core SignalR supported platforms</span></span>

## <a name="server-system-requirements"></a><span data-ttu-id="250b1-104">Requisitos del sistema de servidor</span><span class="sxs-lookup"><span data-stu-id="250b1-104">Server system requirements</span></span>

SignalR<span data-ttu-id="250b1-105"> para ASP.NET Core admite cualquier plataforma de servidor que ASP.NET Core admita.</span><span class="sxs-lookup"><span data-stu-id="250b1-105"> for ASP.NET Core supports any server platform that ASP.NET Core supports.</span></span>

## <a name="javascript-client"></a><span data-ttu-id="250b1-106">Cliente de JavaScript</span><span class="sxs-lookup"><span data-stu-id="250b1-106">JavaScript client</span></span>

<span data-ttu-id="250b1-107">El [cliente de JavaScript](xref:signalr/javascript-client) se ejecuta en NodeJS 8 y versiones posteriores, y en los siguientes exploradores:</span><span class="sxs-lookup"><span data-stu-id="250b1-107">The [JavaScript client](xref:signalr/javascript-client) runs on NodeJS 8 and later versions and the following browsers:</span></span>

| <span data-ttu-id="250b1-108">Explorador</span><span class="sxs-lookup"><span data-stu-id="250b1-108">Browser</span></span>                         | <span data-ttu-id="250b1-109">Versión</span><span class="sxs-lookup"><span data-stu-id="250b1-109">Version</span></span>         |
| ------------------------------- | --------------- |
| <span data-ttu-id="250b1-110">Microsoft Edge</span><span class="sxs-lookup"><span data-stu-id="250b1-110">Microsoft Edge</span></span>                  | <span data-ttu-id="250b1-111">&dagger; actual</span><span class="sxs-lookup"><span data-stu-id="250b1-111">Current&dagger;</span></span> |
| <span data-ttu-id="250b1-112">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="250b1-112">Mozilla Firefox</span></span>                 | <span data-ttu-id="250b1-113">&dagger; actual</span><span class="sxs-lookup"><span data-stu-id="250b1-113">Current&dagger;</span></span> |
| <span data-ttu-id="250b1-114">Google Chrome; incluye Android</span><span class="sxs-lookup"><span data-stu-id="250b1-114">Google Chrome; includes Android</span></span> | <span data-ttu-id="250b1-115">&dagger; actual</span><span class="sxs-lookup"><span data-stu-id="250b1-115">Current&dagger;</span></span> |
| <span data-ttu-id="250b1-116">Safari incluye iOS</span><span class="sxs-lookup"><span data-stu-id="250b1-116">Safari; includes iOS</span></span>            | <span data-ttu-id="250b1-117">&dagger; actual</span><span class="sxs-lookup"><span data-stu-id="250b1-117">Current&dagger;</span></span> |
| <span data-ttu-id="250b1-118">Microsoft Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="250b1-118">Microsoft Internet Explorer</span></span>     | <span data-ttu-id="250b1-119">11</span><span class="sxs-lookup"><span data-stu-id="250b1-119">11</span></span>              |

<span data-ttu-id="250b1-120">&dagger;*actual* hace referencia a la versión más reciente del explorador.</span><span class="sxs-lookup"><span data-stu-id="250b1-120">&dagger;*Current* refers to the latest version of the browser.</span></span>

## <a name="net-client"></a><span data-ttu-id="250b1-121">Cliente .NET</span><span class="sxs-lookup"><span data-stu-id="250b1-121">.NET client</span></span>

<span data-ttu-id="250b1-122">El [cliente .net](xref:signalr/dotnet-client) se ejecuta en cualquier plataforma compatible con ASP.net Core.</span><span class="sxs-lookup"><span data-stu-id="250b1-122">The [.NET client](xref:signalr/dotnet-client) runs on any platform supported by ASP.NET Core.</span></span> <span data-ttu-id="250b1-123">Por ejemplo, [los desarrolladores de Xamarin pueden usar SignalR](https://github.com/aspnet/Announcements/issues/305) para compilar aplicaciones de Android con Xamarin. Android 8.4.0.1 y versiones posteriores y aplicaciones de iOS con Xamarin. iOS 11.14.0.4 y versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="250b1-123">For example, [Xamarin developers can use SignalR](https://github.com/aspnet/Announcements/issues/305) for building Android apps using Xamarin.Android 8.4.0.1 and later and iOS apps using Xamarin.iOS 11.14.0.4 and later.</span></span>

<span data-ttu-id="250b1-124">Si el servidor ejecuta IIS, el transporte de WebSockets requiere IIS 8,0 o posterior en Windows Server 2012 o posterior.</span><span class="sxs-lookup"><span data-stu-id="250b1-124">If the server runs IIS, the WebSockets transport requires IIS 8.0 or later on Windows Server 2012 or later.</span></span> <span data-ttu-id="250b1-125">Se admiten otros transportes en todas las plataformas.</span><span class="sxs-lookup"><span data-stu-id="250b1-125">Other transports are supported on all platforms.</span></span>

## <a name="java-client"></a><span data-ttu-id="250b1-126">Cliente de Java</span><span class="sxs-lookup"><span data-stu-id="250b1-126">Java client</span></span>

<span data-ttu-id="250b1-127">El [cliente de Java](xref:signalr/java-client) es compatible con Java 8 y versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="250b1-127">The [Java client](xref:signalr/java-client) supports Java 8 and later versions.</span></span>

## <a name="unsupported-clients"></a><span data-ttu-id="250b1-128">Clientes no compatibles</span><span class="sxs-lookup"><span data-stu-id="250b1-128">Unsupported clients</span></span>

<span data-ttu-id="250b1-129">Los siguientes clientes están disponibles pero son experimentales o no oficiales.</span><span class="sxs-lookup"><span data-stu-id="250b1-129">The following clients are available but are experimental or unofficial.</span></span> <span data-ttu-id="250b1-130">No se admiten actualmente y nunca pueden ser.</span><span class="sxs-lookup"><span data-stu-id="250b1-130">They aren't currently supported and may never be.</span></span>

* <span data-ttu-id="250b1-131">[C++Nº](https://github.com/aspnet/SignalR/tree/master/clients/cpp)</span><span class="sxs-lookup"><span data-stu-id="250b1-131">[C++ client](https://github.com/aspnet/SignalR/tree/master/clients/cpp)</span></span>

* <span data-ttu-id="250b1-132">[Cliente SWIFT](https://github.com/moozzyk/SignalR-Client-Swift)</span><span class="sxs-lookup"><span data-stu-id="250b1-132">[Swift client](https://github.com/moozzyk/SignalR-Client-Swift)</span></span>
