---
title: Hospedaje en ASP.NET Core
author: guardrex
description: Obtenga más información sobre el host web de ASP.NET Core y el genérico de .NET, los elementos responsables del inicio de las aplicaciones y la administración de la vigencia.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/host/index
ms.openlocfilehash: 7ad059e39866f59040c12b7ac15e9fa3405a9aad
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/17/2018
---
# <a name="host-in-aspnet-core"></a><span data-ttu-id="286de-103">Hospedaje en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="286de-103">Host in ASP.NET Core</span></span>

<span data-ttu-id="286de-104">Las aplicaciones .NET configuran e inician un *host*.</span><span class="sxs-lookup"><span data-stu-id="286de-104">.NET apps configure and launch a *host*.</span></span> <span data-ttu-id="286de-105">El host es responsable de la administración del inicio y la duración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="286de-105">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="286de-106">Hay dos API host disponibles para su uso:</span><span class="sxs-lookup"><span data-stu-id="286de-106">Two host APIs are available for use:</span></span>

* <span data-ttu-id="286de-107">[Host web](xref:fundamentals/host/web-host): indicado para el hospedaje de aplicaciones web.</span><span class="sxs-lookup"><span data-stu-id="286de-107">[Web Host](xref:fundamentals/host/web-host) &ndash; Suitable for hosting web apps.</span></span>
* <span data-ttu-id="286de-108">[Host genérico](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 y versiones posteriores): indicado para el hospedaje de aplicaciones que no sean web, por ejemplo, las que ejecutan tareas en segundo plano.</span><span class="sxs-lookup"><span data-stu-id="286de-108">[Generic Host](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 or later) &ndash; Suitable for hosting non-web apps (for example, apps that run background tasks).</span></span> <span data-ttu-id="286de-109">En una versión posterior, el host genérico será adecuado para hospedar aplicaciones de cualquier tipo, incluidas las web.</span><span class="sxs-lookup"><span data-stu-id="286de-109">In a future release, the Generic Host will be suitable for hosting any kind of app, including web apps.</span></span> <span data-ttu-id="286de-110">Llegado el momento, el host genérico reemplazará el host web.</span><span class="sxs-lookup"><span data-stu-id="286de-110">The Generic Host will eventually replace the Web Host.</span></span>

<span data-ttu-id="286de-111">En este momento, los desarrolladores deben usar el [host web](xref:fundamentals/host/web-host) basado en [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) para hospedar aplicaciones ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="286de-111">At this time, developers should use the [Web Host](xref:fundamentals/host/web-host) based on [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) for hosting ASP.NET Core apps.</span></span>
