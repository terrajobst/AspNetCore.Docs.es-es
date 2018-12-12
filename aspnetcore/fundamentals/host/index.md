---
title: Hosts web y hosts genéricos de ASP.NET Core
author: guardrex
description: Obtenga más información sobre el host web de ASP.NET Core y el genérico de .NET, los elementos responsables del inicio de las aplicaciones y la administración de la vigencia.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 08/28/2018
uid: fundamentals/host/index
ms.openlocfilehash: 43a68f67368ce37a1fb4032579c6c4e4c05d2719
ms.sourcegitcommit: b34b25da2ab68e6495b2460ff570468f16a9bf0d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2018
ms.locfileid: "53284583"
---
# <a name="web-host-and-generic-host-in-aspnet-core"></a><span data-ttu-id="177ef-103">Hosts web y hosts genéricos de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="177ef-103">Web Host and Generic Host in ASP.NET Core</span></span>

<span data-ttu-id="177ef-104">Las aplicaciones .NET configuran e inician un *host*.</span><span class="sxs-lookup"><span data-stu-id="177ef-104">.NET apps configure and launch a *host*.</span></span> <span data-ttu-id="177ef-105">El host es responsable de la administración del inicio y la duración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="177ef-105">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="177ef-106">Hay dos API host disponibles para su uso:</span><span class="sxs-lookup"><span data-stu-id="177ef-106">Two host APIs are available for use:</span></span>

* <span data-ttu-id="177ef-107">[Host web](xref:fundamentals/host/web-host): indicado para el hospedaje de aplicaciones web.</span><span class="sxs-lookup"><span data-stu-id="177ef-107">[Web Host](xref:fundamentals/host/web-host) &ndash; Suitable for hosting web apps.</span></span>
* <span data-ttu-id="177ef-108">[Host genérico](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 y versiones posteriores): indicado para el hospedaje de aplicaciones que no sean web, por ejemplo, las que ejecutan tareas en segundo plano.</span><span class="sxs-lookup"><span data-stu-id="177ef-108">[Generic Host](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 or later) &ndash; Suitable for hosting non-web apps (for example, apps that run background tasks).</span></span> <span data-ttu-id="177ef-109">En una versión posterior, el host genérico será adecuado para hospedar aplicaciones de cualquier tipo, incluidas las web.</span><span class="sxs-lookup"><span data-stu-id="177ef-109">In a future release, the Generic Host will be suitable for hosting any kind of app, including web apps.</span></span> <span data-ttu-id="177ef-110">Llegado el momento, el host genérico reemplazará el host web.</span><span class="sxs-lookup"><span data-stu-id="177ef-110">The Generic Host will eventually replace the Web Host.</span></span>

<span data-ttu-id="177ef-111">Para hospedar *aplicaciones web* ASP.NET Core, los desarrolladores deben usar el host web basado en <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>.</span><span class="sxs-lookup"><span data-stu-id="177ef-111">For hosting ASP.NET Core *web apps*, developers should use the Web Host based on <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>.</span></span> <span data-ttu-id="177ef-112">Para hospedar *aplicaciones que no sean web*, los desarrolladores deben usar el host genérico basado en <xref:Microsoft.Extensions.Hosting.HostBuilder>.</span><span class="sxs-lookup"><span data-stu-id="177ef-112">For hosting *non-web apps*, developers should use the Generic Host based on <xref:Microsoft.Extensions.Hosting.HostBuilder>.</span></span>

<xref:fundamentals/host/hosted-services>  
<span data-ttu-id="177ef-113">Obtenga información sobre cómo implementar tareas en segundo plano con servicios hospedados en ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="177ef-113">Learn how to implement background tasks with hosted services in ASP.NET Core.</span></span>

<xref:fundamentals/configuration/platform-specific-configuration>  
<span data-ttu-id="177ef-114">Descubra cómo mejorar una aplicación ASP.NET Core desde un ensamblado sin referencia o al que se hace referencia mediante una implementación de <xref:Microsoft.AspNetCore.Hosting.IHostingStartup>.</span><span class="sxs-lookup"><span data-stu-id="177ef-114">Discover how to enhance an ASP.NET Core app from a referenced or unreferenced assembly using an <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation.</span></span>
