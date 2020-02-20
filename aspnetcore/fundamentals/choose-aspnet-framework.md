---
title: Elección entre ASP.NET 4.x y ASP.NET Core
author: rick-anderson
description: Se explican las diferencias entre ASP.NET Core. y ASP.NET 4.x, y cómo elegir entre ellos.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 02/12/2020
no-loc:
- SignalR
uid: fundamentals/choose-between-aspnet-and-aspnetcore
ms.openlocfilehash: a7280b59578ee1d96edeeccf9c9df0b0e4eb4eb8
ms.sourcegitcommit: 6645435fc8f5092fc7e923742e85592b56e37ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2020
ms.locfileid: "77447300"
---
# <a name="choose-between-aspnet-4x-and-aspnet-core"></a><span data-ttu-id="4cd15-103">Elección entre ASP.NET 4.x y ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4cd15-103">Choose between ASP.NET 4.x and ASP.NET Core</span></span>

<span data-ttu-id="4cd15-104">ASP.NET Core es un rediseño de ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="4cd15-104">ASP.NET Core is a redesign of ASP.NET 4.x.</span></span> <span data-ttu-id="4cd15-105">En este artículo se enumeran las diferencias entre ellos.</span><span class="sxs-lookup"><span data-stu-id="4cd15-105">This article lists the differences between them.</span></span>

## <a name="aspnet-core"></a><span data-ttu-id="4cd15-106">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4cd15-106">ASP.NET Core</span></span>

<span data-ttu-id="4cd15-107">ASP.NET Core es un marco multiplataforma de código abierto que tiene como finalidad compilar modernas aplicaciones web basadas en la nube en Windows, macOS o Linux.</span><span class="sxs-lookup"><span data-stu-id="4cd15-107">ASP.NET Core is an open-source, cross-platform framework for building modern, cloud-based web apps on Windows, macOS, or Linux.</span></span>

[!INCLUDE[](~/includes/benefits.md)]

## <a name="aspnet-4x"></a><span data-ttu-id="4cd15-108">ASP.NET 4.x</span><span class="sxs-lookup"><span data-stu-id="4cd15-108">ASP.NET 4.x</span></span>

<span data-ttu-id="4cd15-109">ASP.NET 4.x es un marco consolidado que proporciona los servicios necesarios para compilar aplicaciones web de nivel empresarial basadas en servidor en Windows.</span><span class="sxs-lookup"><span data-stu-id="4cd15-109">ASP.NET 4.x is a mature framework that provides the services needed to build enterprise-grade, server-based web apps on Windows.</span></span>

## <a name="framework-selection"></a><span data-ttu-id="4cd15-110">Selección del marco</span><span class="sxs-lookup"><span data-stu-id="4cd15-110">Framework selection</span></span>

<span data-ttu-id="4cd15-111">En la tabla siguiente se compara ASP.NET Core en ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="4cd15-111">The following table compares ASP.NET Core to ASP.NET 4.x.</span></span>

| <span data-ttu-id="4cd15-112">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4cd15-112">ASP.NET Core</span></span> | <span data-ttu-id="4cd15-113">ASP.NET 4.x</span><span class="sxs-lookup"><span data-stu-id="4cd15-113">ASP.NET 4.x</span></span> |
|---|---|
|<span data-ttu-id="4cd15-114">Compilación para Windows, macOS o Linux</span><span class="sxs-lookup"><span data-stu-id="4cd15-114">Build for Windows, macOS, or Linux</span></span>|<span data-ttu-id="4cd15-115">Compilación para Windows</span><span class="sxs-lookup"><span data-stu-id="4cd15-115">Build for Windows</span></span>|
|<span data-ttu-id="4cd15-116">[Las páginas de Razor](xref:razor-pages/index) son el método recomendado para crear una interfaz de usuario web desde la aparición de ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="4cd15-116">[Razor Pages](xref:razor-pages/index) is the recommended approach to create a Web UI as of ASP.NET Core 2.x.</span></span> <span data-ttu-id="4cd15-117">Consulte también [MVC](xref:mvc/overview), [Web API](xref:tutorials/first-web-api) y [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="4cd15-117">See also [MVC](xref:mvc/overview), [Web API](xref:tutorials/first-web-api), and [SignalR](xref:signalr/introduction).</span></span>|<span data-ttu-id="4cd15-118">Use [Web Forms](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), [Web API](/aspnet/web-api/), [WebHooks](/aspnet/webhooks/) o [Web Pages](/aspnet/web-pages)</span><span class="sxs-lookup"><span data-stu-id="4cd15-118">Use [Web Forms](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), [Web API](/aspnet/web-api/), [WebHooks](/aspnet/webhooks/), or [Web Pages](/aspnet/web-pages)</span></span>|
|<span data-ttu-id="4cd15-119">Varias versiones por equipo</span><span class="sxs-lookup"><span data-stu-id="4cd15-119">Multiple versions per machine</span></span>|<span data-ttu-id="4cd15-120">Una versión por equipo</span><span class="sxs-lookup"><span data-stu-id="4cd15-120">One version per machine</span></span>|
|<span data-ttu-id="4cd15-121">Desarrollo con [Visual Studio](https://visualstudio.microsoft.com/vs/), [Visual Studio para Mac](https://visualstudio.microsoft.com/vs/mac/) o [Visual Studio Code](https://code.visualstudio.com/) con C# o F#</span><span class="sxs-lookup"><span data-stu-id="4cd15-121">Develop with [Visual Studio](https://visualstudio.microsoft.com/vs/), [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/), or [Visual Studio Code](https://code.visualstudio.com/) using C# or F#</span></span>|<span data-ttu-id="4cd15-122">Desarrollo con [Visual Studio](https://visualstudio.microsoft.com/vs/) con C#, VB o F#</span><span class="sxs-lookup"><span data-stu-id="4cd15-122">Develop with [Visual Studio](https://visualstudio.microsoft.com/vs/) using C#, VB, or F#</span></span>|
|<span data-ttu-id="4cd15-123">Mayor rendimiento que ASP.NET 4.x</span><span class="sxs-lookup"><span data-stu-id="4cd15-123">Higher performance than ASP.NET 4.x</span></span>|<span data-ttu-id="4cd15-124">Buen rendimiento</span><span class="sxs-lookup"><span data-stu-id="4cd15-124">Good performance</span></span>|
|[<span data-ttu-id="4cd15-125">Uso del entorno de ejecución de .NET Core</span><span class="sxs-lookup"><span data-stu-id="4cd15-125">Use .NET Core runtime</span></span>](/dotnet/standard/choosing-core-framework-server)|<span data-ttu-id="4cd15-126">Usar el tiempo de ejecución de .NET Framework</span><span class="sxs-lookup"><span data-stu-id="4cd15-126">Use .NET Framework runtime</span></span>|

<span data-ttu-id="4cd15-127">Vea [ASP.NET Core con .NET Framework como destino](xref:index#target-framework) para obtener información sobre la compatibilidad de ASP.NET Core 2.x en .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="4cd15-127">See [ASP.NET Core targeting .NET Framework](xref:index#target-framework) for information on ASP.NET Core 2.x support on .NET Framework.</span></span>

## <a name="aspnet-core-scenarios"></a><span data-ttu-id="4cd15-128">Escenarios de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4cd15-128">ASP.NET Core scenarios</span></span>

* [<span data-ttu-id="4cd15-129">Sitios web</span><span class="sxs-lookup"><span data-stu-id="4cd15-129">Websites</span></span>](xref:tutorials/first-mvc-app/index)
* [<span data-ttu-id="4cd15-130">API</span><span class="sxs-lookup"><span data-stu-id="4cd15-130">APIs</span></span>](xref:tutorials/first-web-api)
* [<span data-ttu-id="4cd15-131">En tiempo real</span><span class="sxs-lookup"><span data-stu-id="4cd15-131">Real-time</span></span>](xref:signalr/introduction)
* [<span data-ttu-id="4cd15-132">Implementación de una aplicación ASP.NET Core en Azure</span><span class="sxs-lookup"><span data-stu-id="4cd15-132">Deploy an ASP.NET Core app to Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet)

## <a name="aspnet-4x-scenarios"></a><span data-ttu-id="4cd15-133">Escenarios de ASP.NET 4.x</span><span class="sxs-lookup"><span data-stu-id="4cd15-133">ASP.NET 4.x scenarios</span></span>

* [<span data-ttu-id="4cd15-134">Sitios web</span><span class="sxs-lookup"><span data-stu-id="4cd15-134">Websites</span></span>](/aspnet/mvc)
* [<span data-ttu-id="4cd15-135">API</span><span class="sxs-lookup"><span data-stu-id="4cd15-135">APIs</span></span>](/aspnet/web-api)
* [<span data-ttu-id="4cd15-136">En tiempo real</span><span class="sxs-lookup"><span data-stu-id="4cd15-136">Real-time</span></span>](/aspnet/signalr)
* [<span data-ttu-id="4cd15-137">Creación de una aplicación web ASP.NET 4.x en Azure</span><span class="sxs-lookup"><span data-stu-id="4cd15-137">Create an ASP.NET 4.x web app in Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet-framework)

## <a name="additional-resources"></a><span data-ttu-id="4cd15-138">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="4cd15-138">Additional resources</span></span>

* [<span data-ttu-id="4cd15-139">Introducción a ASP.NET</span><span class="sxs-lookup"><span data-stu-id="4cd15-139">Introduction to ASP.NET</span></span>](/aspnet/overview)
* [<span data-ttu-id="4cd15-140">Introducción a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4cd15-140">Introduction to ASP.NET Core</span></span>](xref:index)
* <xref:host-and-deploy/azure-apps/index>
