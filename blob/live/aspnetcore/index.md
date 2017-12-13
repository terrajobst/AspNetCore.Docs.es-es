---
title: "Introducción a ASP.NET Core"
author: rick-anderson
description: "Proporciona una introducción a ASP.NET Core."
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 09/03/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: index
ms.openlocfilehash: a075c63fddb9e28a1da37d4ef6647808a0dcb583
ms.sourcegitcommit: 8f42ab93402c1b8044815e1e48d0bb84c81f8b59
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/29/2017
---
# <a name="introduction-to-aspnet-core"></a><span data-ttu-id="5b8e0-104">Introducción a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5b8e0-104">Introduction to ASP.NET Core</span></span>

<span data-ttu-id="5b8e0-105">Por [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT) y [Shaun Luttin](https://twitter.com/dicshaunary)</span><span class="sxs-lookup"><span data-stu-id="5b8e0-105">By [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Shaun Luttin](https://twitter.com/dicshaunary)</span></span>

<span data-ttu-id="5b8e0-106">ASP.NET Core es un marco multiplataforma de [código abierto](https://github.com/aspnet/home) y de alto rendimiento que tiene como finalidad compilar modernas aplicaciones conectadas a Internet y basadas en la nube.</span><span class="sxs-lookup"><span data-stu-id="5b8e0-106">ASP.NET Core is a cross-platform, high-performance, [open-source](https://github.com/aspnet/home) framework for building modern, cloud-based, Internet-connected applications.</span></span> <span data-ttu-id="5b8e0-107">Con ASP.NET Core puede hacer lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="5b8e0-107">With ASP.NET Core, you can:</span></span>

* <span data-ttu-id="5b8e0-108">Compilar servicios y aplicaciones web, aplicaciones de [IoT](https://www.microsoft.com/en-us/internet-of-things/) y back-ends móviles.</span><span class="sxs-lookup"><span data-stu-id="5b8e0-108">Build web apps and services, [IoT](https://www.microsoft.com/en-us/internet-of-things/) apps, and mobile backends.</span></span>
* <span data-ttu-id="5b8e0-109">Usar sus herramientas de desarrollo favoritas en Windows, macOS y Linux.</span><span class="sxs-lookup"><span data-stu-id="5b8e0-109">Use your favorite development tools on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="5b8e0-110">Efectuar implementaciones locales y en la nube.</span><span class="sxs-lookup"><span data-stu-id="5b8e0-110">Deploy to the cloud or on-premises</span></span>
* <span data-ttu-id="5b8e0-111">Ejecutarlo en [.NET Core o en .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="5b8e0-111">Run on [.NET Core or .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="why-use-aspnet-core"></a><span data-ttu-id="5b8e0-112">¿Por qué debería usar ASP.NET Core?</span><span class="sxs-lookup"><span data-stu-id="5b8e0-112">Why use ASP.NET Core?</span></span>

<span data-ttu-id="5b8e0-113">Millones de desarrolladores han usado ASP.NET (y siguen usándolo) para crear aplicaciones web.</span><span class="sxs-lookup"><span data-stu-id="5b8e0-113">Millions of developers have used (and continue to use) ASP.NET to create web apps.</span></span> <span data-ttu-id="5b8e0-114">ASP.NET Core es un nuevo diseño de ASP.NET que cuenta con cambios en la arquitectura que dan como resultado un marco más sencillo y modular.</span><span class="sxs-lookup"><span data-stu-id="5b8e0-114">ASP.NET Core is a redesign of ASP.NET, with architectural changes that result in a leaner and modular framework.</span></span>

<span data-ttu-id="5b8e0-115">ASP.NET Core ofrece las siguientes ventajas:</span><span class="sxs-lookup"><span data-stu-id="5b8e0-115">ASP.NET Core provides the following benefits:</span></span>

* <span data-ttu-id="5b8e0-116">Un caso unificado para crear API web y una interfaz de usuario web.</span><span class="sxs-lookup"><span data-stu-id="5b8e0-116">A unified story for building web UI and web APIs.</span></span>
* <span data-ttu-id="5b8e0-117">Integración de [marcos del lado cliente modernos](xref:client-side/index) y flujos de trabajo de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="5b8e0-117">Integration of [modern client-side frameworks](xref:client-side/index) and development workflows.</span></span>
* <span data-ttu-id="5b8e0-118">Un [sistema de configuración](xref:fundamentals/configuration/index) basado en el entorno y preparado para la nube.</span><span class="sxs-lookup"><span data-stu-id="5b8e0-118">A cloud-ready, environment-based [configuration system](xref:fundamentals/configuration/index).</span></span>
* <span data-ttu-id="5b8e0-119">[Inserción de dependencias](xref:fundamentals/dependency-injection) integrada.</span><span class="sxs-lookup"><span data-stu-id="5b8e0-119">Built-in [dependency injection](xref:fundamentals/dependency-injection).</span></span>
* <span data-ttu-id="5b8e0-120">Una canalización de solicitudes HTTP ligera, modular y de alto rendimiento.</span><span class="sxs-lookup"><span data-stu-id="5b8e0-120">A lightweight, high-performance, and modular HTTP request pipeline.</span></span>
* <span data-ttu-id="5b8e0-121">Capacidad de hospedarse en IIS o de autohospedarse en su propio proceso.</span><span class="sxs-lookup"><span data-stu-id="5b8e0-121">Ability to host on IIS or self-host in your own process.</span></span>
* <span data-ttu-id="5b8e0-122">Se puede ejecutar en [.NET Core](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server), que es compatible con un auténtico control de versiones de aplicaciones en paralelo.</span><span class="sxs-lookup"><span data-stu-id="5b8e0-122">Can run on [.NET Core](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server), which supports true side-by-side app versioning.</span></span>
* <span data-ttu-id="5b8e0-123">Herramientas que simplifican el desarrollo web moderno.</span><span class="sxs-lookup"><span data-stu-id="5b8e0-123">Tooling that simplifies modern web development.</span></span>
* <span data-ttu-id="5b8e0-124">Capacidad para compilarse y ejecutarse en Windows, macOS y Linux.</span><span class="sxs-lookup"><span data-stu-id="5b8e0-124">Ability to build and run on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="5b8e0-125">De código abierto y centrado en la comunidad.</span><span class="sxs-lookup"><span data-stu-id="5b8e0-125">Open-source and community-focused.</span></span>

<span data-ttu-id="5b8e0-126">ASP.NET Core se distribuye en su totalidad como paquetes [NuGet](https://www.nuget.org/).</span><span class="sxs-lookup"><span data-stu-id="5b8e0-126">ASP.NET Core ships entirely as [NuGet](https://www.nuget.org/) packages.</span></span> <span data-ttu-id="5b8e0-127">Esto le permite optimizar la aplicación para incluir únicamente los paquetes NuGet que necesita.</span><span class="sxs-lookup"><span data-stu-id="5b8e0-127">This allows you to optimize your app to include just the NuGet packages you need.</span></span> <span data-ttu-id="5b8e0-128">Entre las ventajas de una menor superficie de aplicación se incluyen una mayor seguridad, un mantenimiento reducido y un rendimiento mejorado.</span><span class="sxs-lookup"><span data-stu-id="5b8e0-128">The benefits of a smaller app surface area include tighter security, reduced servicing, and improved performance.</span></span>

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a><span data-ttu-id="5b8e0-129">Creación de API web e interfaces de usuario web mediante ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="5b8e0-129">Build web APIs and web UI using ASP.NET Core MVC</span></span>

<span data-ttu-id="5b8e0-130">ASP.NET Core MVC proporciona funciones que le ayudarán a crear [API web](xref:tutorials/index#building-web-apis) y [aplicaciones web](xref:tutorials/index#building-web-applications):</span><span class="sxs-lookup"><span data-stu-id="5b8e0-130">ASP.NET Core MVC provides features that help you build [web APIs](xref:tutorials/index#building-web-apis) and [web apps](xref:tutorials/index#building-web-applications):</span></span>

* <span data-ttu-id="5b8e0-131">El [patrón Modelo-Vista-Controlador (MVC)](xref:mvc/overview) permite que se [puedan hacer pruebas](testing/index.md) en las API web y en las aplicaciones web.</span><span class="sxs-lookup"><span data-stu-id="5b8e0-131">The [Model-View-Controller (MVC) pattern](xref:mvc/overview) helps make your web APIs and web apps [testable](testing/index.md).</span></span>
* <span data-ttu-id="5b8e0-132">[Páginas de Razor](xref:mvc/razor-pages/index) (novedad de la versión 2.0) es un modelo de programación basado en páginas que facilita la compilación de interfaces de usuario web y hace que sea más productiva.</span><span class="sxs-lookup"><span data-stu-id="5b8e0-132">[Razor Pages](xref:mvc/razor-pages/index) (new in 2.0) is a page-based programming model that makes building web UI easier and more productive.</span></span>
* <span data-ttu-id="5b8e0-133">La [sintaxis de Razor](xref:mvc/views/razor) proporciona un lenguaje productivo para las [páginas de Razor](xref:mvc/razor-pages/index) y las [vistas de MVC](xref:mvc/views/overview).</span><span class="sxs-lookup"><span data-stu-id="5b8e0-133">[Razor syntax](xref:mvc/views/razor) provides a productive language for [Razor Pages](xref:mvc/razor-pages/index) and [MVC Views](xref:mvc/views/overview).</span></span>
* <span data-ttu-id="5b8e0-134">Las [aplicaciones auxiliares de etiquetas](xref:mvc/views/tag-helpers/intro) permiten al código de servidor participar en la creación y representación de elementos HTML en archivos de Razor.</span><span class="sxs-lookup"><span data-stu-id="5b8e0-134">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span>
* <span data-ttu-id="5b8e0-135">La compatibilidad integrada para [varios formatos de datos y la negociación de contenidos](mvc/models/formatting.md) permite que las API web lleguen a una amplia gama de clientes, como los exploradores y los dispositivos móviles.</span><span class="sxs-lookup"><span data-stu-id="5b8e0-135">Built-in support for [multiple data formats and content negotiation](mvc/models/formatting.md) lets your web APIs reach a broad range of clients, including browsers and mobile devices.</span></span>
* <span data-ttu-id="5b8e0-136">El [enlace de modelo](xref:mvc/models/model-binding) asigna automáticamente datos de solicitudes HTTP a parámetros de método de acción.</span><span class="sxs-lookup"><span data-stu-id="5b8e0-136">[Model Binding](xref:mvc/models/model-binding) automatically maps data from HTTP requests to action method parameters.</span></span>
* <span data-ttu-id="5b8e0-137">La [validación de modelos](xref:mvc/models/validation) efectúa una validación del lado cliente y del lado servidor de forma automática.</span><span class="sxs-lookup"><span data-stu-id="5b8e0-137">[Model Validation](xref:mvc/models/validation) automatically performs client and server-side validation.</span></span>

## <a name="client-side-development"></a><span data-ttu-id="5b8e0-138">Desarrollo del lado cliente</span><span class="sxs-lookup"><span data-stu-id="5b8e0-138">Client-side development</span></span>

<span data-ttu-id="5b8e0-139">ASP.NET Core se ha diseñado para integrarse a la perfección con una serie de marcos del lado cliente, como [AngularJS](xref:client-side/angular), [KnockoutJS](xref:client-side/knockout) y [Bootstrap](xref:client-side/bootstrap).</span><span class="sxs-lookup"><span data-stu-id="5b8e0-139">ASP.NET Core is designed to integrate seamlessly with a variety of client-side frameworks, including [AngularJS](xref:client-side/angular), [KnockoutJS](xref:client-side/knockout), and [Bootstrap](xref:client-side/bootstrap).</span></span> <span data-ttu-id="5b8e0-140">Vea [Client-side development](client-side/index.md) (Desarrollo del lado cliente) para más información.</span><span class="sxs-lookup"><span data-stu-id="5b8e0-140">See [Client-side development](client-side/index.md) for more details.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5b8e0-141">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="5b8e0-141">Next steps</span></span>

<span data-ttu-id="5b8e0-142">Para obtener más información, vea los siguientes recursos:</span><span class="sxs-lookup"><span data-stu-id="5b8e0-142">For more information, see the following resources:</span></span>

* [<span data-ttu-id="5b8e0-143">Tutoriales de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5b8e0-143">ASP.NET Core tutorials</span></span>](xref:tutorials/index)
* [<span data-ttu-id="5b8e0-144">Conceptos básicos de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5b8e0-144">ASP.NET Core fundamentals</span></span>](xref:fundamentals/index)
* <span data-ttu-id="5b8e0-145">[La reunión semanal de la comunidad de ASP.NET](https://live.asp.net/) trata el progreso del equipo y planea y presenta nuevos blogs y fabricantes de software de terceros.</span><span class="sxs-lookup"><span data-stu-id="5b8e0-145">[The weekly ASP.NET community standup](https://live.asp.net/) covers the team's progress and plans and features new blogs and third-party software.</span></span>
