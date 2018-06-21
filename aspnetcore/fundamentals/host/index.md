---
title: Hospedaje en ASP.NET Core
author: guardrex
description: Obtenga más información sobre el host web de ASP.NET Core y el genérico de .NET, los elementos responsables del inicio de las aplicaciones y la administración de la vigencia.
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
uid: fundamentals/host/index
ms.openlocfilehash: 365c679e789c07818c6eb007f40f6aef43b82c44
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276622"
---
# <a name="host-in-aspnet-core"></a><span data-ttu-id="f791f-103">Hospedaje en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f791f-103">Host in ASP.NET Core</span></span>

<span data-ttu-id="f791f-104">Las aplicaciones .NET configuran e inician un *host*.</span><span class="sxs-lookup"><span data-stu-id="f791f-104">.NET apps configure and launch a *host*.</span></span> <span data-ttu-id="f791f-105">El host es responsable de la administración del inicio y la duración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f791f-105">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="f791f-106">Hay dos API host disponibles para su uso:</span><span class="sxs-lookup"><span data-stu-id="f791f-106">Two host APIs are available for use:</span></span>

* <span data-ttu-id="f791f-107">[Host web](xref:fundamentals/host/web-host): indicado para el hospedaje de aplicaciones web.</span><span class="sxs-lookup"><span data-stu-id="f791f-107">[Web Host](xref:fundamentals/host/web-host) &ndash; Suitable for hosting web apps.</span></span>
* <span data-ttu-id="f791f-108">[Host genérico](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 y versiones posteriores): indicado para el hospedaje de aplicaciones que no sean web, por ejemplo, las que ejecutan tareas en segundo plano.</span><span class="sxs-lookup"><span data-stu-id="f791f-108">[Generic Host](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 or later) &ndash; Suitable for hosting non-web apps (for example, apps that run background tasks).</span></span> <span data-ttu-id="f791f-109">En una versión posterior, el host genérico será adecuado para hospedar aplicaciones de cualquier tipo, incluidas las web.</span><span class="sxs-lookup"><span data-stu-id="f791f-109">In a future release, the Generic Host will be suitable for hosting any kind of app, including web apps.</span></span> <span data-ttu-id="f791f-110">Llegado el momento, el host genérico reemplazará el host web.</span><span class="sxs-lookup"><span data-stu-id="f791f-110">The Generic Host will eventually replace the Web Host.</span></span>

<span data-ttu-id="f791f-111">Para hospedar *aplicaciones web* ASP.NET Core, los desarrolladores deben usar el host web basado en [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="f791f-111">For hosting ASP.NET Core *web apps*, developers should use the Web Host based on [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="f791f-112">Para hospedar *aplicaciones que no sean web*, los desarrolladores deben usar el host genérico basado en [HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder).</span><span class="sxs-lookup"><span data-stu-id="f791f-112">For hosting *non-web apps*, developers should use the Generic Host based on [HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder).</span></span>
