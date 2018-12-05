---
title: Plataformas compatibles con ASP.NET Core SignalR
author: tdykstra
description: Obtenga información acerca de las plataformas admitidas para ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/12/2018
uid: signalr/supported-platforms
ms.openlocfilehash: be3d4d0049395fb2499bd0b4aac126e953ce7910
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861724"
---
# <a name="aspnet-core-signalr-supported-platforms"></a><span data-ttu-id="ceefa-103">Plataformas compatibles con ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="ceefa-103">ASP.NET Core SignalR supported platforms</span></span>

## <a name="server-system-requirements"></a><span data-ttu-id="ceefa-104">Requisitos del sistema de servidor</span><span class="sxs-lookup"><span data-stu-id="ceefa-104">Server system requirements</span></span>

<span data-ttu-id="ceefa-105">SignalR para ASP.NET Core es compatible con cualquier plataforma de servidor ASP.NET Core admite.</span><span class="sxs-lookup"><span data-stu-id="ceefa-105">SignalR for ASP.NET Core supports any server platform that ASP.NET Core supports.</span></span>

## <a name="javascript-client"></a><span data-ttu-id="ceefa-106">Cliente de JavaScript</span><span class="sxs-lookup"><span data-stu-id="ceefa-106">JavaScript client</span></span>

<span data-ttu-id="ceefa-107">El [cliente JavaScript](https://www.npmjs.com/package/@aspnet/signalr) se ejecuta en NodeJS 8 y versiones posteriores y los siguientes exploradores:</span><span class="sxs-lookup"><span data-stu-id="ceefa-107">The [JavaScript client](https://www.npmjs.com/package/@aspnet/signalr) runs on NodeJS 8 and later versions and the following browsers:</span></span>

| <span data-ttu-id="ceefa-108">Explorador</span><span class="sxs-lookup"><span data-stu-id="ceefa-108">Browser</span></span>                         | <span data-ttu-id="ceefa-109">Versión</span><span class="sxs-lookup"><span data-stu-id="ceefa-109">Version</span></span> |
| ------------------------------- | ------- |
| <span data-ttu-id="ceefa-110">Microsoft Edge</span><span class="sxs-lookup"><span data-stu-id="ceefa-110">Microsoft Edge</span></span>                  | <span data-ttu-id="ceefa-111">actuales</span><span class="sxs-lookup"><span data-stu-id="ceefa-111">current</span></span> |
| <span data-ttu-id="ceefa-112">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="ceefa-112">Mozilla Firefox</span></span>                 | <span data-ttu-id="ceefa-113">actuales</span><span class="sxs-lookup"><span data-stu-id="ceefa-113">current</span></span> |
| <span data-ttu-id="ceefa-114">Google Chrome; incluye Android</span><span class="sxs-lookup"><span data-stu-id="ceefa-114">Google Chrome; includes Android</span></span> | <span data-ttu-id="ceefa-115">actuales</span><span class="sxs-lookup"><span data-stu-id="ceefa-115">current</span></span> |
| <span data-ttu-id="ceefa-116">Safari; incluye iOS</span><span class="sxs-lookup"><span data-stu-id="ceefa-116">Safari; includes iOS</span></span>            | <span data-ttu-id="ceefa-117">actuales</span><span class="sxs-lookup"><span data-stu-id="ceefa-117">current</span></span> |
| <span data-ttu-id="ceefa-118">Microsoft Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="ceefa-118">Microsoft Internet Explorer</span></span>     | <span data-ttu-id="ceefa-119">11</span><span class="sxs-lookup"><span data-stu-id="ceefa-119">11</span></span>      |
 
## <a name="net-client"></a><span data-ttu-id="ceefa-120">Cliente .NET</span><span class="sxs-lookup"><span data-stu-id="ceefa-120">.NET client</span></span>

<span data-ttu-id="ceefa-121">El [cliente.NET](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) se ejecuta en cualquier plataforma compatible con ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ceefa-121">The [.NET client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) runs on any platform supported by ASP.NET Core.</span></span> <span data-ttu-id="ceefa-122">Por ejemplo, [los desarrolladores de Xamarin pueden usar SignalR](https://github.com/aspnet/Announcements/issues/305) para la creación de aplicaciones de Android con Xamarin.Android 8.4.0.1 y versiones posteriores y en aplicaciones de iOS con Xamarin.iOS 11.14.0.4 y versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="ceefa-122">For example, [Xamarin developers can use SignalR](https://github.com/aspnet/Announcements/issues/305) for building Android apps using Xamarin.Android 8.4.0.1 and later and iOS apps using Xamarin.iOS 11.14.0.4 and later.</span></span>

<span data-ttu-id="ceefa-123">Si el servidor ejecuta IIS, el transporte de WebSockets requiere IIS 8.0 o posterior en Windows Server 2012 o posterior.</span><span class="sxs-lookup"><span data-stu-id="ceefa-123">If the server runs IIS, the WebSockets transport requires IIS 8.0 or higher on Windows Server 2012 or higher.</span></span> <span data-ttu-id="ceefa-124">Otros transportes se admiten en todas las plataformas.</span><span class="sxs-lookup"><span data-stu-id="ceefa-124">Other transports are supported on all platforms.</span></span>

## <a name="java-client"></a><span data-ttu-id="ceefa-125">Cliente de Java</span><span class="sxs-lookup"><span data-stu-id="ceefa-125">Java client</span></span>

<span data-ttu-id="ceefa-126">El [cliente Java](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) admite Java 8 y versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="ceefa-126">The [Java client](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) supports Java 8 and later versions.</span></span>

## <a name="unsupported-clients"></a><span data-ttu-id="ceefa-127">Clientes no compatibles</span><span class="sxs-lookup"><span data-stu-id="ceefa-127">Unsupported clients</span></span>

<span data-ttu-id="ceefa-128">Los clientes siguientes están disponibles, pero son experimentales o no oficiales.</span><span class="sxs-lookup"><span data-stu-id="ceefa-128">The following clients are available but are experimental or unofficial.</span></span> <span data-ttu-id="ceefa-129">Que no se admiten actualmente y nunca pueden ser.</span><span class="sxs-lookup"><span data-stu-id="ceefa-129">They aren't currently supported and may never be.</span></span>

* [<span data-ttu-id="ceefa-130">Cliente de C++</span><span class="sxs-lookup"><span data-stu-id="ceefa-130">C++ client</span></span>](https://github.com/aspnet/SignalR/tree/master/clients/cpp)

* [<span data-ttu-id="ceefa-131">Cliente de SWIFT</span><span class="sxs-lookup"><span data-stu-id="ceefa-131">Swift client</span></span>](https://github.com/moozzyk/SignalR-Client-Swift)
