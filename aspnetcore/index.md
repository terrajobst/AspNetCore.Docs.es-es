---
title: Introducción a ASP.NET Core
author: rick-anderson
description: Consulte una introducción a ASP.NET Core, un marco multiplataforma de código abierto y de alto rendimiento que tiene como finalidad compilar modernas aplicaciones conectadas a Internet y basadas en la nube.
ms.author: riande
ms.date: 9/28/2018
uid: index
ms.openlocfilehash: 3bb86fa255548ff66592ac14c1020e0c6b47959c
ms.sourcegitcommit: f43f430a166a7ec137fcad12ded0372747227498
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2018
ms.locfileid: "49391162"
---
# <a name="introduction-to-aspnet-core"></a><span data-ttu-id="24e89-103">Introducción a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="24e89-103">Introduction to ASP.NET Core</span></span>

<span data-ttu-id="24e89-104">Por [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT) y [Shaun Luttin](https://twitter.com/dicshaunary)</span><span class="sxs-lookup"><span data-stu-id="24e89-104">By [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Shaun Luttin](https://twitter.com/dicshaunary)</span></span>

<span data-ttu-id="24e89-105">ASP.NET Core es un marco multiplataforma de [código abierto](https://github.com/aspnet/home) y de alto rendimiento que tiene como finalidad compilar modernas aplicaciones conectadas a Internet y basadas en la nube.</span><span class="sxs-lookup"><span data-stu-id="24e89-105">ASP.NET Core is a cross-platform, high-performance, [open-source](https://github.com/aspnet/home) framework for building modern, cloud-based, Internet-connected applications.</span></span> <span data-ttu-id="24e89-106">Con ASP.NET Core puede hacer lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="24e89-106">With ASP.NET Core, you can:</span></span>

* <span data-ttu-id="24e89-107">Compilar servicios y aplicaciones web, aplicaciones de [IoT](https://www.microsoft.com/internet-of-things/) y back-ends móviles.</span><span class="sxs-lookup"><span data-stu-id="24e89-107">Build web apps and services, [IoT](https://www.microsoft.com/internet-of-things/) apps, and mobile backends.</span></span>
* <span data-ttu-id="24e89-108">Usar sus herramientas de desarrollo favoritas en Windows, macOS y Linux.</span><span class="sxs-lookup"><span data-stu-id="24e89-108">Use your favorite development tools on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="24e89-109">Efectuar implementaciones locales y en la nube.</span><span class="sxs-lookup"><span data-stu-id="24e89-109">Deploy to the cloud or on-premises.</span></span>
* <span data-ttu-id="24e89-110">Ejecutarlo en [.NET Core o en .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="24e89-110">Run on [.NET Core or .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="why-use-aspnet-core"></a><span data-ttu-id="24e89-111">¿Por qué debería usar ASP.NET Core?</span><span class="sxs-lookup"><span data-stu-id="24e89-111">Why use ASP.NET Core?</span></span>

<span data-ttu-id="24e89-112">Millones de desarrolladores han usado [ASP.NET 4.x](https://docs.microsoft.com/aspnet/overview) (y siguen usándolo) para crear aplicaciones web.</span><span class="sxs-lookup"><span data-stu-id="24e89-112">Millions of developers have used (and continue to use) [ASP.NET 4.x](https://docs.microsoft.com/aspnet/overview) to create web apps.</span></span> <span data-ttu-id="24e89-113">ASP.NET Core es un nuevo diseño de ASP.NET 4.x que cuenta con cambios en la arquitectura que dan como resultado un marco más sencillo y modular.</span><span class="sxs-lookup"><span data-stu-id="24e89-113">ASP.NET Core is a redesign of ASP.NET 4.x, with architectural changes that result in a leaner, more modular framework.</span></span>

[!INCLUDE[](~/includes/benefits.md)]

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a><span data-ttu-id="24e89-114">Creación de API web e interfaces de usuario web mediante ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="24e89-114">Build web APIs and web UI using ASP.NET Core MVC</span></span>

<span data-ttu-id="24e89-115">ASP.NET Core MVC proporciona características para crear [API web](xref:tutorials/index#build-web-apis) y [aplicaciones web](xref:tutorials/index#build-web-apps):</span><span class="sxs-lookup"><span data-stu-id="24e89-115">ASP.NET Core MVC provides features to build [web APIs](xref:tutorials/index#build-web-apis) and [web apps](xref:tutorials/index#build-web-apps):</span></span>

* <span data-ttu-id="24e89-116">El [patrón Modelo-Vista-Controlador (MVC)](xref:mvc/overview) permite que se [puedan hacer pruebas](xref:test/index) en las API web y en las aplicaciones web.</span><span class="sxs-lookup"><span data-stu-id="24e89-116">The [Model-View-Controller (MVC) pattern](xref:mvc/overview) helps make your web APIs and web apps [testable](xref:test/index).</span></span>
* <span data-ttu-id="24e89-117">[Páginas de Razor](xref:razor-pages/index) (novedad de ASP.NET Core 2.0) es un modelo de programación basado en páginas que facilita la compilación de interfaces de usuario web y hace que sea más productiva.</span><span class="sxs-lookup"><span data-stu-id="24e89-117">[Razor Pages](xref:razor-pages/index) (new in ASP.NET Core 2.0) is a page-based programming model that makes building web UI easier and more productive.</span></span>
* <span data-ttu-id="24e89-118">El [marcado de Razor](xref:mvc/views/razor) proporciona una sintaxis productiva para las [páginas de Razor](xref:razor-pages/index) y las [vistas de MVC](xref:mvc/views/overview).</span><span class="sxs-lookup"><span data-stu-id="24e89-118">[Razor markup](xref:mvc/views/razor) provides a productive syntax for [Razor Pages](xref:razor-pages/index) and [MVC views](xref:mvc/views/overview).</span></span>
* <span data-ttu-id="24e89-119">Los [asistentes de etiquetas](xref:mvc/views/tag-helpers/intro) permiten que el código de servidor participe en la creación y la representación de elementos HTML en archivos de Razor.</span><span class="sxs-lookup"><span data-stu-id="24e89-119">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span>
* <span data-ttu-id="24e89-120">La compatibilidad integrada para [varios formatos de datos y la negociación de contenidos](xref:web-api/advanced/formatting) permite que las API web lleguen a una amplia gama de clientes, como los exploradores y los dispositivos móviles.</span><span class="sxs-lookup"><span data-stu-id="24e89-120">Built-in support for [multiple data formats and content negotiation](xref:web-api/advanced/formatting) lets your web APIs reach a broad range of clients, including browsers and mobile devices.</span></span>
* <span data-ttu-id="24e89-121">El [enlace de modelo](xref:mvc/models/model-binding) asigna automáticamente datos de solicitudes HTTP a parámetros de método de acción.</span><span class="sxs-lookup"><span data-stu-id="24e89-121">[Model binding](xref:mvc/models/model-binding) automatically maps data from HTTP requests to action method parameters.</span></span>
* <span data-ttu-id="24e89-122">La [validación de modelos](xref:mvc/models/validation) efectúa una validación del lado cliente y del lado servidor de forma automática.</span><span class="sxs-lookup"><span data-stu-id="24e89-122">[Model validation](xref:mvc/models/validation) automatically performs client-side and server-side validation.</span></span>

## <a name="client-side-development"></a><span data-ttu-id="24e89-123">Desarrollo del lado cliente</span><span class="sxs-lookup"><span data-stu-id="24e89-123">Client-side development</span></span>

<span data-ttu-id="24e89-124">ASP.NET Core se integra perfectamente con bibliotecas y marcos de trabajo populares del lado cliente, que incluyen [Angular](xref:spa/angular), [React](xref:spa/react) y [Bootstrap](https://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="24e89-124">ASP.NET Core integrates seamlessly with popular client-side frameworks and libraries, including [Angular](xref:spa/angular), [React](xref:spa/react), and [Bootstrap](https://getbootstrap.com/).</span></span> <span data-ttu-id="24e89-125">Para obtener más información, vea [Desarrollo del lado cliente](xref:client-side/index).</span><span class="sxs-lookup"><span data-stu-id="24e89-125">For more information, see [Client-side development](xref:client-side/index).</span></span>

<a name="target-framework"></a>

## <a name="aspnet-core-targeting-net-framework"></a><span data-ttu-id="24e89-126">ASP.NET Core con .NET Framework como destino</span><span class="sxs-lookup"><span data-stu-id="24e89-126">ASP.NET Core targeting .NET Framework</span></span>

<span data-ttu-id="24e89-127">ASP.NET Core puede tener como destino .NET Core o .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="24e89-127">ASP.NET Core can target .NET Core or .NET Framework.</span></span> <span data-ttu-id="24e89-128">Las aplicaciones de ASP.NET Core que tienen como destino .NET Framework no son multiplataforma, sino que solo se ejecutan en Windows.</span><span class="sxs-lookup"><span data-stu-id="24e89-128">ASP.NET Core apps targeting .NET Framework aren't cross-platform&mdash;they run on Windows only.</span></span> <span data-ttu-id="24e89-129">No está previsto eliminar la compatibilidad con .NET Framework como destino en ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="24e89-129">There are no plans to remove support for targeting .NET Framework in ASP.NET Core.</span></span> <span data-ttu-id="24e89-130">Por lo general, ASP.NET Core está formado por bibliotecas de [.NET Standard](/dotnet/standard/net-standard).</span><span class="sxs-lookup"><span data-stu-id="24e89-130">Generally, ASP.NET Core is made up of [.NET Standard](/dotnet/standard/net-standard) libraries.</span></span> <span data-ttu-id="24e89-131">Las aplicaciones escritas con .NET Standard 2.0 se ejecutan en cualquier lugar en el que se admita .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="24e89-131">Apps written with .NET Standard 2.0 run anywhere that .NET Standard 2.0 is supported.</span></span>

<span data-ttu-id="24e89-132">ASP.NET Core 2.x se admite en las versiones de .NET Framework compatibles con .NET Standard 2.0:</span><span class="sxs-lookup"><span data-stu-id="24e89-132">ASP.NET Core 2.x is supported on .NET Framework versions compatible with .NET Standard 2.0:</span></span>

* <span data-ttu-id="24e89-133">Se recomienda encarecidamente .NET Framework 4.7.1 y posterior.</span><span class="sxs-lookup"><span data-stu-id="24e89-133">.NET Framework 4.7.1 and later is strongly recommended.</span></span>
* <span data-ttu-id="24e89-134">.NET Framework 4.6.1 y posterior.</span><span class="sxs-lookup"><span data-stu-id="24e89-134">.NET Framework 4.6.1 and later.</span></span>

<span data-ttu-id="24e89-135">El uso de .NET Core como destino cuenta con varias ventajas que van en aumento con cada versión.</span><span class="sxs-lookup"><span data-stu-id="24e89-135">There are several advantages to targeting .NET Core, and these advantages increase with each release.</span></span> <span data-ttu-id="24e89-136">Entre las ventajas del uso de .NET Core en vez de .NET Framework se incluyen las siguientes:</span><span class="sxs-lookup"><span data-stu-id="24e89-136">Some advantages of .NET Core over .NET Framework include:</span></span>

* <span data-ttu-id="24e89-137">Multiplataforma.</span><span class="sxs-lookup"><span data-stu-id="24e89-137">Cross-platform.</span></span> <span data-ttu-id="24e89-138">Ejecución en macOS, Linux y Windows.</span><span class="sxs-lookup"><span data-stu-id="24e89-138">Runs on macOS, Linux, and Windows.</span></span>
* <span data-ttu-id="24e89-139">Rendimiento mejorado</span><span class="sxs-lookup"><span data-stu-id="24e89-139">Improved performance</span></span>
* <span data-ttu-id="24e89-140">Control de versiones en paralelo.</span><span class="sxs-lookup"><span data-stu-id="24e89-140">Side-by-side versioning</span></span>
* <span data-ttu-id="24e89-141">Nuevas API.</span><span class="sxs-lookup"><span data-stu-id="24e89-141">New APIs</span></span>
* <span data-ttu-id="24e89-142">Abrir origen</span><span class="sxs-lookup"><span data-stu-id="24e89-142">Open source</span></span>

<span data-ttu-id="24e89-143">Estamos trabajando intensamente para cerrar la brecha de API entre .NET Framework y .NET Core.</span><span class="sxs-lookup"><span data-stu-id="24e89-143">We're working hard to close the API gap from .NET Framework to .NET Core.</span></span> <span data-ttu-id="24e89-144">El [paquete de compatibilidad de Windows](/dotnet/core/porting/windows-compat-pack) ha permitido que miles de API solo de Windows estén disponibles en .NET Core.</span><span class="sxs-lookup"><span data-stu-id="24e89-144">The [Windows Compatibility Pack](/dotnet/core/porting/windows-compat-pack) made thousands of Windows-only APIs available in .NET Core.</span></span> <span data-ttu-id="24e89-145">Estas API no estaban disponibles en .NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="24e89-145">These APIs weren't available in .NET Core 1.x.</span></span>

## <a name="next-steps"></a><span data-ttu-id="24e89-146">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="24e89-146">Next steps</span></span>

<span data-ttu-id="24e89-147">Para obtener más información, vea los siguientes recursos:</span><span class="sxs-lookup"><span data-stu-id="24e89-147">For more information, see the following resources:</span></span>

* [<span data-ttu-id="24e89-148">Introducción a las páginas de Razor</span><span class="sxs-lookup"><span data-stu-id="24e89-148">Get started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="24e89-149">Tutoriales de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="24e89-149">ASP.NET Core tutorials</span></span>](xref:tutorials/index)
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [<span data-ttu-id="24e89-150">Conceptos básicos de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="24e89-150">ASP.NET Core fundamentals</span></span>](xref:fundamentals/index)
* <span data-ttu-id="24e89-151">[La reunión semanal de la comunidad de ASP.NET](https://live.asp.net/) trata el progreso y los planes del equipo.</span><span class="sxs-lookup"><span data-stu-id="24e89-151">[The weekly ASP.NET community standup](https://live.asp.net/) covers the team's progress and plans.</span></span> <span data-ttu-id="24e89-152">Incluye nuevos blogs y nuevo software de terceros.</span><span class="sxs-lookup"><span data-stu-id="24e89-152">It features new blogs and third-party software.</span></span>
