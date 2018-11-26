---
title: Elección entre ASP.NET 4.x y ASP.NET Core
author: rick-anderson
description: Se explican las diferencias entre ASP.NET Core. y ASP.NET 4.x, y cómo elegir entre ellos.
ms.author: riande
ms.date: 09/11/2018
uid: fundamentals/choose-between-aspnet-and-aspnetcore
ms.openlocfilehash: 79e56e10b756677431ceff289300c251e54bf632
ms.sourcegitcommit: bdfba5e7575b2a786ef27c0edf688c7dbd09ee95
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/22/2018
ms.locfileid: "52288673"
---
# <a name="choose-between-aspnet-4x-and-aspnet-core"></a><span data-ttu-id="b2986-103">Elección entre ASP.NET 4.x y ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b2986-103">Choose between ASP.NET 4.x and ASP.NET Core</span></span>

<span data-ttu-id="b2986-104">ASP.NET Core es un rediseño de ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="b2986-104">ASP.NET Core is a redesign of ASP.NET 4.x.</span></span> <span data-ttu-id="b2986-105">En este artículo se enumeran las diferencias entre ellos.</span><span class="sxs-lookup"><span data-stu-id="b2986-105">This article lists the differences between them.</span></span>

## <a name="aspnet-core"></a><span data-ttu-id="b2986-106">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b2986-106">ASP.NET Core</span></span>

<span data-ttu-id="b2986-107">ASP.NET Core es un marco multiplataforma de código abierto que tiene como finalidad compilar modernas aplicaciones web basadas en la nube en Windows, macOS o Linux.</span><span class="sxs-lookup"><span data-stu-id="b2986-107">ASP.NET Core is an open-source, cross-platform framework for building modern, cloud-based web apps on Windows, macOS, or Linux.</span></span>

[!INCLUDE[](~/includes/benefits.md)]

## <a name="aspnet-4x"></a><span data-ttu-id="b2986-108">ASP.NET 4.x</span><span class="sxs-lookup"><span data-stu-id="b2986-108">ASP.NET 4.x</span></span>

<span data-ttu-id="b2986-109">ASP.NET 4.x es un marco consolidado que proporciona los servicios necesarios para compilar aplicaciones web de nivel empresarial basadas en servidor en Windows.</span><span class="sxs-lookup"><span data-stu-id="b2986-109">ASP.NET 4.x is a mature framework that provides the services needed to build enterprise-grade, server-based web apps on Windows.</span></span>

## <a name="framework-selection"></a><span data-ttu-id="b2986-110">Selección del marco</span><span class="sxs-lookup"><span data-stu-id="b2986-110">Framework selection</span></span>

<span data-ttu-id="b2986-111">En la tabla siguiente se compara ASP.NET Core en ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="b2986-111">The following table compares ASP.NET Core to ASP.NET 4.x.</span></span>

| <span data-ttu-id="b2986-112">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b2986-112">ASP.NET Core</span></span> | <span data-ttu-id="b2986-113">ASP.NET 4.x</span><span class="sxs-lookup"><span data-stu-id="b2986-113">ASP.NET 4.x</span></span> |
|---|---|
|<span data-ttu-id="b2986-114">Compilación para Windows, macOS o Linux</span><span class="sxs-lookup"><span data-stu-id="b2986-114">Build for Windows, macOS, or Linux</span></span>|<span data-ttu-id="b2986-115">Compilación para Windows</span><span class="sxs-lookup"><span data-stu-id="b2986-115">Build for Windows</span></span>|
|<span data-ttu-id="b2986-116">[Las páginas de Razor](xref:razor-pages/index) son el método recomendado para crear una interfaz de usuario web desde la aparición de ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="b2986-116">[Razor Pages](xref:razor-pages/index) is the recommended approach to create a Web UI as of ASP.NET Core 2.x.</span></span> <span data-ttu-id="b2986-117">Vea también [MVC](xref:mvc/overview), [Web API](xref:tutorials/first-web-api) y [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="b2986-117">See also [MVC](xref:mvc/overview), [Web API](xref:tutorials/first-web-api), and [SignalR](xref:signalr/introduction).</span></span>|<span data-ttu-id="b2986-118">Use [formularios Web Forms](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), [Web API](/aspnet/web-api/), [WebHooks](/aspnet/webhooks/) o [Web Pages](/aspnet/web-pages)</span><span class="sxs-lookup"><span data-stu-id="b2986-118">Use [Web Forms](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), [Web API](/aspnet/web-api/), [WebHooks](/aspnet/webhooks/), or [Web Pages](/aspnet/web-pages)</span></span>|
|<span data-ttu-id="b2986-119">Varias versiones por equipo</span><span class="sxs-lookup"><span data-stu-id="b2986-119">Multiple versions per machine</span></span>|<span data-ttu-id="b2986-120">Una versión por equipo</span><span class="sxs-lookup"><span data-stu-id="b2986-120">One version per machine</span></span>|
|<span data-ttu-id="b2986-121">Desarrollo con Visual Studio, [Visual Studio para Mac](https://www.visualstudio.com/vs/visual-studio-mac/) o [Visual Studio Code](https://code.visualstudio.com/) con C# o F#</span><span class="sxs-lookup"><span data-stu-id="b2986-121">Develop with Visual Studio, [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/), or [Visual Studio Code](https://code.visualstudio.com/) using C# or F#</span></span>|<span data-ttu-id="b2986-122">Desarrollo con Visual Studio con C#, VB o F#</span><span class="sxs-lookup"><span data-stu-id="b2986-122">Develop with Visual Studio using C#, VB, or F#</span></span>|
|<span data-ttu-id="b2986-123">Mayor rendimiento que ASP.NET 4.x</span><span class="sxs-lookup"><span data-stu-id="b2986-123">Higher performance than ASP.NET 4.x</span></span>|<span data-ttu-id="b2986-124">Buen rendimiento</span><span class="sxs-lookup"><span data-stu-id="b2986-124">Good performance</span></span>|
|[<span data-ttu-id="b2986-125">Elegir .NET Framework o .NET Core</span><span class="sxs-lookup"><span data-stu-id="b2986-125">Choose .NET Framework or .NET Core runtime</span></span>](/dotnet/articles/standard/choosing-core-framework-server)|<span data-ttu-id="b2986-126">Usar el tiempo de ejecución de .NET Framework</span><span class="sxs-lookup"><span data-stu-id="b2986-126">Use .NET Framework runtime</span></span>|

<span data-ttu-id="b2986-127">Vea [ASP.NET Core con .NET Framework como destino](xref:index#target-framework) para obtener información sobre la compatibilidad de ASP.NET Core 2.x en .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="b2986-127">See [ASP.NET Core targeting .NET Framework](xref:index#target-framework) for information on ASP.NET Core 2.x support on .NET Framework.</span></span>

## <a name="aspnet-core-scenarios"></a><span data-ttu-id="b2986-128">Escenarios de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b2986-128">ASP.NET Core scenarios</span></span>

* <span data-ttu-id="b2986-129">[Las páginas de Razor](xref:razor-pages/index) son el método recomendado para crear una interfaz de usuario web desde la aparición de ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="b2986-129">[Razor Pages](xref:razor-pages/index) is the recommended approach to create a Web UI as of ASP.NET Core 2.x.</span></span>
* [<span data-ttu-id="b2986-130">Sitios web</span><span class="sxs-lookup"><span data-stu-id="b2986-130">Websites</span></span>](xref:tutorials/first-mvc-app/index)
* [<span data-ttu-id="b2986-131">API</span><span class="sxs-lookup"><span data-stu-id="b2986-131">APIs</span></span>](xref:tutorials/first-web-api)
* [<span data-ttu-id="b2986-132">En tiempo real</span><span class="sxs-lookup"><span data-stu-id="b2986-132">Real-time</span></span>](xref:signalr/index)
* [<span data-ttu-id="b2986-133">Implementación de una aplicación ASP.NET Core en Azure</span><span class="sxs-lookup"><span data-stu-id="b2986-133">Deploy an ASP.NET Core app to Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet)

## <a name="aspnet-4x-scenarios"></a><span data-ttu-id="b2986-134">Escenarios de ASP.NET 4.x</span><span class="sxs-lookup"><span data-stu-id="b2986-134">ASP.NET 4.x scenarios</span></span>

* [<span data-ttu-id="b2986-135">Sitios web</span><span class="sxs-lookup"><span data-stu-id="b2986-135">Websites</span></span>](/aspnet/mvc)
* [<span data-ttu-id="b2986-136">API</span><span class="sxs-lookup"><span data-stu-id="b2986-136">APIs</span></span>](/aspnet/web-api)
* [<span data-ttu-id="b2986-137">En tiempo real</span><span class="sxs-lookup"><span data-stu-id="b2986-137">Real-time</span></span>](/aspnet/signalr)
* [<span data-ttu-id="b2986-138">Creación de una aplicación web ASP.NET 4.x en Azure</span><span class="sxs-lookup"><span data-stu-id="b2986-138">Create an ASP.NET 4.x web app in Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet-framework)

## <a name="additional-resources"></a><span data-ttu-id="b2986-139">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="b2986-139">Additional resources</span></span>

* [<span data-ttu-id="b2986-140">Introducción a ASP.NET</span><span class="sxs-lookup"><span data-stu-id="b2986-140">Introduction to ASP.NET</span></span>](/aspnet/overview)
* [<span data-ttu-id="b2986-141">Introducción a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b2986-141">Introduction to ASP.NET Core</span></span>](xref:index)
* <xref:host-and-deploy/azure-apps/index>

> [!NOTE]
> <span data-ttu-id="b2986-142">Vamos a probar la facilidad de uso de una nueva estructura propuesta para la tabla de contenido de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b2986-142">We’re testing the usability of a proposed new structure for the ASP.NET Core table of contents.</span></span>  <span data-ttu-id="b2986-143">Si tiene unos minutos para realizar un ejercicio de búsqueda de 7 temas diferentes en la tabla actual o propuesta de contenido, [haga clic aquí para participar en el estudio](https://dpk4xbh5.optimalworkshop.com/treejack/aa11wn82).</span><span class="sxs-lookup"><span data-stu-id="b2986-143">If you have a few minutes to try an exercise of finding 7 different topics in the current or proposed table of contents, please [click here to participate in the study](https://dpk4xbh5.optimalworkshop.com/treejack/aa11wn82).</span></span>