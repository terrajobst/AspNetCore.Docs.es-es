---
title: Elegir entre ASP.NET y ASP.NET Core
author: rick-anderson
description: Descubra cómo elegir entre ASP.NET y ASP.NET Core.
ms.author: riande
ms.date: 05/11/2018
uid: fundamentals/choose-between-aspnet-and-aspnetcore
ms.openlocfilehash: 6d759c0bc5e5c7d32d6c14786db6ba9fe7a2f1e8
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36297234"
---
# <a name="choose-between-aspnet-and-aspnet-core"></a><span data-ttu-id="d58a7-103">Elegir entre ASP.NET y ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d58a7-103">Choose between ASP.NET and ASP.NET Core</span></span>

<span data-ttu-id="d58a7-104">Independientemente de la aplicación web que vaya a crear, ASP.NET tiene una solución para usted: desde aplicaciones web empresariales para Windows Server hasta pequeños microservicios para contenedores de Linux, y mucho más.</span><span class="sxs-lookup"><span data-stu-id="d58a7-104">No matter the web app you're creating, ASP.NET has a solution for you: from enterprise web apps targeting Windows Server, to small microservices targeting Linux containers, and everything in between.</span></span>

## <a name="aspnet-core"></a><span data-ttu-id="d58a7-105">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d58a7-105">ASP.NET Core</span></span>

<span data-ttu-id="d58a7-106">ASP.NET Core es un marco multiplataforma de código abierto que tiene como finalidad compilar modernas aplicaciones web basadas en la nube en Windows, macOS o Linux.</span><span class="sxs-lookup"><span data-stu-id="d58a7-106">ASP.NET Core is an open-source, cross-platform framework for building modern, cloud-based web apps on Windows, macOS, or Linux.</span></span>

## <a name="aspnet"></a><span data-ttu-id="d58a7-107">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="d58a7-107">ASP.NET</span></span>

<span data-ttu-id="d58a7-108">ASP.NET es un marco consolidado que proporciona todos los servicios necesarios para compilar aplicaciones web empresariales basadas en servidor en Windows.</span><span class="sxs-lookup"><span data-stu-id="d58a7-108">ASP.NET is a mature framework that provides all the services needed to build enterprise-grade, server-based web apps on Windows.</span></span>

## <a name="framework-selection"></a><span data-ttu-id="d58a7-109">Selección del marco</span><span class="sxs-lookup"><span data-stu-id="d58a7-109">Framework selection</span></span>

<span data-ttu-id="d58a7-110">Revise la siguiente tabla para averiguar qué marco es el más adecuado según sus necesidades.</span><span class="sxs-lookup"><span data-stu-id="d58a7-110">Review the table below to determine which framework is most appropriate for your needs.</span></span>

| <span data-ttu-id="d58a7-111">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d58a7-111">ASP.NET Core</span></span> | <span data-ttu-id="d58a7-112">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="d58a7-112">ASP.NET</span></span> |
|---|---|
|<span data-ttu-id="d58a7-113">Compilación para Windows, macOS o Linux</span><span class="sxs-lookup"><span data-stu-id="d58a7-113">Build for Windows, macOS, or Linux</span></span>|<span data-ttu-id="d58a7-114">Compilación para Windows</span><span class="sxs-lookup"><span data-stu-id="d58a7-114">Build for Windows</span></span>|
|<span data-ttu-id="d58a7-115">[Las páginas de Razor](xref:razor-pages/index) son el método recomendado para crear una interfaz de usuario web desde la aparición de ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="d58a7-115">[Razor Pages](xref:razor-pages/index) is the recommended approach to create a Web UI as of ASP.NET Core 2.x.</span></span> <span data-ttu-id="d58a7-116">Vea también [MVC](xref:mvc/overview), [Web API](xref:tutorials/first-web-api) y [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="d58a7-116">See also [MVC](xref:mvc/overview), [Web API](xref:tutorials/first-web-api), and [SignalR](xref:signalr/introduction).</span></span>|<span data-ttu-id="d58a7-117">Use [formularios Web Forms](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), [Web API](/aspnet/web-api/), [WebHooks](/aspnet/webhooks/) o [Web Pages](/aspnet/web-pages)</span><span class="sxs-lookup"><span data-stu-id="d58a7-117">Use [Web Forms](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), [Web API](/aspnet/web-api/), [WebHooks](/aspnet/webhooks/), or [Web Pages](/aspnet/web-pages)</span></span>|
|<span data-ttu-id="d58a7-118">Varias versiones por equipo</span><span class="sxs-lookup"><span data-stu-id="d58a7-118">Multiple versions per machine</span></span>|<span data-ttu-id="d58a7-119">Una versión por equipo</span><span class="sxs-lookup"><span data-stu-id="d58a7-119">One version per machine</span></span>|
|<span data-ttu-id="d58a7-120">Desarrollo con Visual Studio, [Visual Studio para Mac](https://www.visualstudio.com/vs/visual-studio-mac/) o [Visual Studio Code](https://code.visualstudio.com/) con C# o F#</span><span class="sxs-lookup"><span data-stu-id="d58a7-120">Develop with Visual Studio, [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/), or [Visual Studio Code](https://code.visualstudio.com/) using C# or F#</span></span>|<span data-ttu-id="d58a7-121">Desarrollo con Visual Studio con C#, VB o F#</span><span class="sxs-lookup"><span data-stu-id="d58a7-121">Develop with Visual Studio using C#, VB, or F#</span></span>|
|<span data-ttu-id="d58a7-122">Mayor rendimiento que ASP.NET</span><span class="sxs-lookup"><span data-stu-id="d58a7-122">Higher performance than ASP.NET</span></span>|<span data-ttu-id="d58a7-123">Buen rendimiento</span><span class="sxs-lookup"><span data-stu-id="d58a7-123">Good performance</span></span>|
|[<span data-ttu-id="d58a7-124">Elegir .NET Framework o .NET Core</span><span class="sxs-lookup"><span data-stu-id="d58a7-124">Choose .NET Framework or .NET Core runtime</span></span>](/dotnet/articles/standard/choosing-core-framework-server)|<span data-ttu-id="d58a7-125">Usar el tiempo de ejecución de .NET Framework</span><span class="sxs-lookup"><span data-stu-id="d58a7-125">Use .NET Framework runtime</span></span>|

## <a name="aspnet-core-scenarios"></a><span data-ttu-id="d58a7-126">Escenarios de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d58a7-126">ASP.NET Core scenarios</span></span>

* <span data-ttu-id="d58a7-127">[Las páginas de Razor](xref:razor-pages/index) son el método recomendado para crear una interfaz de usuario web desde la aparición de ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="d58a7-127">[Razor Pages](xref:razor-pages/index) is the recommended approach to create a Web UI as of ASP.NET Core 2.x.</span></span>
* [<span data-ttu-id="d58a7-128">Sitios web</span><span class="sxs-lookup"><span data-stu-id="d58a7-128">Websites</span></span>](xref:tutorials/first-mvc-app/index)
* [<span data-ttu-id="d58a7-129">API</span><span class="sxs-lookup"><span data-stu-id="d58a7-129">APIs</span></span>](xref:tutorials/first-web-api)
* [<span data-ttu-id="d58a7-130">En tiempo real</span><span class="sxs-lookup"><span data-stu-id="d58a7-130">Real-time</span></span>](xref:signalr/index)

## <a name="aspnet-scenarios"></a><span data-ttu-id="d58a7-131">Escenarios de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="d58a7-131">ASP.NET scenarios</span></span>

* [<span data-ttu-id="d58a7-132">Sitios web</span><span class="sxs-lookup"><span data-stu-id="d58a7-132">Websites</span></span>](/aspnet/mvc)
* [<span data-ttu-id="d58a7-133">API</span><span class="sxs-lookup"><span data-stu-id="d58a7-133">APIs</span></span>](/aspnet/web-api)
* [<span data-ttu-id="d58a7-134">En tiempo real</span><span class="sxs-lookup"><span data-stu-id="d58a7-134">Real-time</span></span>](/aspnet/signalr)

## <a name="resources"></a><span data-ttu-id="d58a7-135">Recursos</span><span class="sxs-lookup"><span data-stu-id="d58a7-135">Resources</span></span>

* [<span data-ttu-id="d58a7-136">Introducción a ASP.NET</span><span class="sxs-lookup"><span data-stu-id="d58a7-136">Introduction to ASP.NET</span></span>](/aspnet/overview)
* [<span data-ttu-id="d58a7-137">Introducción a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d58a7-137">Introduction to ASP.NET Core</span></span>](xref:index)
