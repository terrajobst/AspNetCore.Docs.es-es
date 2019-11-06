---
title: Introducción a ASP.NET Core
author: rick-anderson
description: Consulte una introducción a ASP.NET Core, un marco multiplataforma de código abierto y de alto rendimiento que tiene como finalidad compilar modernas aplicaciones conectadas a Internet y basadas en la nube.
ms.author: riande
ms.custom: mvc
ms.date: 11/03/2019
uid: index
ms.openlocfilehash: edbdce19656af64d7c2c0ee554bc5213a0d0c50e
ms.sourcegitcommit: 09f4a5ded39cc8204576fe801d760bd8b611f3aa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2019
ms.locfileid: "73611407"
---
# <a name="introduction-to-aspnet-core"></a><span data-ttu-id="f1024-103">Introducción a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f1024-103">Introduction to ASP.NET Core</span></span>

<span data-ttu-id="f1024-104">Por [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT) y [Shaun Luttin](https://twitter.com/dicshaunary)</span><span class="sxs-lookup"><span data-stu-id="f1024-104">By [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Shaun Luttin](https://twitter.com/dicshaunary)</span></span>

<span data-ttu-id="f1024-105">ASP.NET Core es un marco multiplataforma de [código abierto](https://github.com/aspnet/home) y de alto rendimiento que tiene como finalidad compilar modernas aplicaciones conectadas a Internet y basadas en la nube.</span><span class="sxs-lookup"><span data-stu-id="f1024-105">ASP.NET Core is a cross-platform, high-performance, [open-source](https://github.com/aspnet/home) framework for building modern, cloud-based, Internet-connected applications.</span></span> <span data-ttu-id="f1024-106">Con ASP.NET Core puede hacer lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="f1024-106">With ASP.NET Core, you can:</span></span>

* <span data-ttu-id="f1024-107">Compilar servicios y aplicaciones web, aplicaciones de [IoT](https://www.microsoft.com/internet-of-things/) y back-ends móviles.</span><span class="sxs-lookup"><span data-stu-id="f1024-107">Build web apps and services, [IoT](https://www.microsoft.com/internet-of-things/) apps, and mobile backends.</span></span>
* <span data-ttu-id="f1024-108">Usar sus herramientas de desarrollo favoritas en Windows, macOS y Linux.</span><span class="sxs-lookup"><span data-stu-id="f1024-108">Use your favorite development tools on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="f1024-109">Efectuar implementaciones locales y en la nube.</span><span class="sxs-lookup"><span data-stu-id="f1024-109">Deploy to the cloud or on-premises.</span></span>
* <span data-ttu-id="f1024-110">Ejecutarlo en [.NET Core o en .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="f1024-110">Run on [.NET Core or .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="why-choose-aspnet-core"></a><span data-ttu-id="f1024-111">¿Por qué elegir ASP.NET Core?</span><span class="sxs-lookup"><span data-stu-id="f1024-111">Why choose ASP.NET Core?</span></span>

<span data-ttu-id="f1024-112">Millones de desarrolladores han usado [ASP.NET 4.x](/aspnet/overview) (y siguen usándolo) para crear aplicaciones web.</span><span class="sxs-lookup"><span data-stu-id="f1024-112">Millions of developers have used (and continue to use) [ASP.NET 4.x](/aspnet/overview) to create web apps.</span></span> <span data-ttu-id="f1024-113">ASP.NET Core es un nuevo diseño de ASP.NET 4.x que cuenta con cambios en la arquitectura que dan como resultado un marco más sencillo y modular.</span><span class="sxs-lookup"><span data-stu-id="f1024-113">ASP.NET Core is a redesign of ASP.NET 4.x, with architectural changes that result in a leaner, more modular framework.</span></span>

[!INCLUDE[](~/includes/benefits.md)]

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a><span data-ttu-id="f1024-114">Creación de API web e interfaces de usuario web mediante ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="f1024-114">Build web APIs and web UI using ASP.NET Core MVC</span></span>

<span data-ttu-id="f1024-115">ASP.NET Core MVC proporciona características para crear [API web](xref:tutorials/first-web-api) y [aplicaciones web](xref:tutorials/razor-pages/index):</span><span class="sxs-lookup"><span data-stu-id="f1024-115">ASP.NET Core MVC provides features to build [web APIs](xref:tutorials/first-web-api) and [web apps](xref:tutorials/razor-pages/index):</span></span>

* <span data-ttu-id="f1024-116">El [patrón Modelo-Vista-Controlador (MVC)](xref:mvc/overview) permite que se puedan hacer pruebas en las API web y en las aplicaciones web.</span><span class="sxs-lookup"><span data-stu-id="f1024-116">The [Model-View-Controller (MVC) pattern](xref:mvc/overview) helps make your web APIs and web apps testable.</span></span>
* <span data-ttu-id="f1024-117">[Razor Pages](xref:razor-pages/index) es un modelo de programación basado en páginas que facilita la compilación de interfaces de usuario web y hace que sea más productiva.</span><span class="sxs-lookup"><span data-stu-id="f1024-117">[Razor Pages](xref:razor-pages/index) is a page-based programming model that makes building web UI easier and more productive.</span></span>
* <span data-ttu-id="f1024-118">El [marcado de Razor](xref:mvc/views/razor) proporciona una sintaxis productiva para las [páginas de Razor](xref:razor-pages/index) y las [vistas de MVC](xref:mvc/views/overview).</span><span class="sxs-lookup"><span data-stu-id="f1024-118">[Razor markup](xref:mvc/views/razor) provides a productive syntax for [Razor Pages](xref:razor-pages/index) and [MVC views](xref:mvc/views/overview).</span></span>
* <span data-ttu-id="f1024-119">Los [asistentes de etiquetas](xref:mvc/views/tag-helpers/intro) permiten que el código de servidor participe en la creación y la representación de elementos HTML en archivos de Razor.</span><span class="sxs-lookup"><span data-stu-id="f1024-119">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span>
* <span data-ttu-id="f1024-120">La compatibilidad integrada para [varios formatos de datos y la negociación de contenidos](xref:web-api/advanced/formatting) permite que las API web lleguen a una amplia gama de clientes, como los exploradores y los dispositivos móviles.</span><span class="sxs-lookup"><span data-stu-id="f1024-120">Built-in support for [multiple data formats and content negotiation](xref:web-api/advanced/formatting) lets your web APIs reach a broad range of clients, including browsers and mobile devices.</span></span>
* <span data-ttu-id="f1024-121">El [enlace de modelo](xref:mvc/models/model-binding) asigna automáticamente datos de solicitudes HTTP a parámetros de método de acción.</span><span class="sxs-lookup"><span data-stu-id="f1024-121">[Model binding](xref:mvc/models/model-binding) automatically maps data from HTTP requests to action method parameters.</span></span>
* <span data-ttu-id="f1024-122">La [validación de modelos](xref:mvc/models/validation) efectúa una validación del lado cliente y del lado servidor de forma automática.</span><span class="sxs-lookup"><span data-stu-id="f1024-122">[Model validation](xref:mvc/models/validation) automatically performs client-side and server-side validation.</span></span>

## <a name="client-side-development"></a><span data-ttu-id="f1024-123">Desarrollo del lado del cliente</span><span class="sxs-lookup"><span data-stu-id="f1024-123">Client-side development</span></span>

<span data-ttu-id="f1024-124">ASP.NET Core se integra perfectamente con bibliotecas y marcos populares del lado cliente, que incluyen [Blazor](xref:blazor/index), [Angular](xref:spa/angular), [React](xref:spa/react) y [Bootstrap](https://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="f1024-124">ASP.NET Core integrates seamlessly with popular client-side frameworks and libraries, including [Blazor](xref:blazor/index), [Angular](xref:spa/angular), [React](xref:spa/react), and [Bootstrap](https://getbootstrap.com/).</span></span> <span data-ttu-id="f1024-125">Para más información, consulte <xref:blazor/index> y los temas relacionados en *Client-side development* (Desarrollo del lado cliente).</span><span class="sxs-lookup"><span data-stu-id="f1024-125">For more information, see <xref:blazor/index> and related topics under *Client-side development*.</span></span>

<a name="target-framework"></a>

## <a name="aspnet-core-targeting-net-framework"></a><span data-ttu-id="f1024-126">ASP.NET Core con .NET Framework como destino</span><span class="sxs-lookup"><span data-stu-id="f1024-126">ASP.NET Core targeting .NET Framework</span></span>

<span data-ttu-id="f1024-127">ASP.NET Core 2.x puede tener como destino .NET Core o .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="f1024-127">ASP.NET Core 2.x can target .NET Core or .NET Framework.</span></span> <span data-ttu-id="f1024-128">Las aplicaciones de ASP.NET Core que tienen como destino .NET Framework no son multiplataforma, sino que solo se ejecutan en Windows.</span><span class="sxs-lookup"><span data-stu-id="f1024-128">ASP.NET Core apps targeting .NET Framework aren't cross-platform&mdash;they run on Windows only.</span></span> <span data-ttu-id="f1024-129">Por lo general, ASP.NET Core 2.x está formado por bibliotecas de [.NET Standard](/dotnet/standard/net-standard).</span><span class="sxs-lookup"><span data-stu-id="f1024-129">Generally, ASP.NET Core 2.x is made up of [.NET Standard](/dotnet/standard/net-standard) libraries.</span></span> <span data-ttu-id="f1024-130">Las bibliotecas escritas con .NET Standard 2.0 se ejecutan en [cualquier plataforma .NET que implementa .NET Standard 2.0](/dotnet/standard/net-standard#net-implementation-support).</span><span class="sxs-lookup"><span data-stu-id="f1024-130">Libraries written with .NET Standard 2.0 run on any [.NET platform that implements .NET Standard 2.0](/dotnet/standard/net-standard#net-implementation-support).</span></span>

<span data-ttu-id="f1024-131">ASP.NET Core 2.x se admite en las versiones de .NET Framework que implementan .NET Standard 2.0:</span><span class="sxs-lookup"><span data-stu-id="f1024-131">ASP.NET Core 2.x is supported on .NET Framework versions that implement .NET Standard 2.0:</span></span>

* <span data-ttu-id="f1024-132">Se recomienda la versión más reciente de .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="f1024-132">.NET Framework latest version is strongly recommended.</span></span>
* <span data-ttu-id="f1024-133">.NET Framework 4.6.1 y posterior.</span><span class="sxs-lookup"><span data-stu-id="f1024-133">.NET Framework 4.6.1 and later.</span></span>

<span data-ttu-id="f1024-134">ASP.NET Core 3.0 y versiones posteriores solo se ejecutan en .NET Core.</span><span class="sxs-lookup"><span data-stu-id="f1024-134">ASP.NET Core 3.0 and later will only run on .NET Core.</span></span> <span data-ttu-id="f1024-135">Para obtener más información sobre este cambio, vea [A first look at changes coming in ASP.NET Core 3.0](https://blogs.msdn.microsoft.com/webdev/2018/10/29/a-first-look-at-changes-coming-in-asp-net-core-3-0/) (Descripción general de los cambios que se aplicarán a ASP.NET Core 3.0).</span><span class="sxs-lookup"><span data-stu-id="f1024-135">For more details regarding this change, see [A first look at changes coming in ASP.NET Core 3.0](https://blogs.msdn.microsoft.com/webdev/2018/10/29/a-first-look-at-changes-coming-in-asp-net-core-3-0/).</span></span>

<span data-ttu-id="f1024-136">El uso de .NET Core como destino cuenta con varias ventajas que van en aumento con cada versión.</span><span class="sxs-lookup"><span data-stu-id="f1024-136">There are several advantages to targeting .NET Core, and these advantages increase with each release.</span></span> <span data-ttu-id="f1024-137">Entre las ventajas del uso de .NET Core en vez de .NET Framework se incluyen las siguientes:</span><span class="sxs-lookup"><span data-stu-id="f1024-137">Some advantages of .NET Core over .NET Framework include:</span></span>

* <span data-ttu-id="f1024-138">Multiplataforma.</span><span class="sxs-lookup"><span data-stu-id="f1024-138">Cross-platform.</span></span> <span data-ttu-id="f1024-139">Ejecución en macOS, Linux y Windows.</span><span class="sxs-lookup"><span data-stu-id="f1024-139">Runs on macOS, Linux, and Windows.</span></span>
* <span data-ttu-id="f1024-140">Rendimiento mejorado</span><span class="sxs-lookup"><span data-stu-id="f1024-140">Improved performance</span></span>
* [<span data-ttu-id="f1024-141">Control de versiones en paralelo</span><span class="sxs-lookup"><span data-stu-id="f1024-141">Side-by-side versioning</span></span>](/dotnet/standard/choosing-core-framework-server#a-need-for-side-by-side-of-net-versions-per-application-level)
* <span data-ttu-id="f1024-142">Nuevas API.</span><span class="sxs-lookup"><span data-stu-id="f1024-142">New APIs</span></span>
* <span data-ttu-id="f1024-143">Código Abierto</span><span class="sxs-lookup"><span data-stu-id="f1024-143">Open source</span></span>

<span data-ttu-id="f1024-144">Estamos trabajando intensamente para cerrar la brecha de API entre .NET Framework y .NET Core.</span><span class="sxs-lookup"><span data-stu-id="f1024-144">We're working hard to close the API gap from .NET Framework to .NET Core.</span></span> <span data-ttu-id="f1024-145">El [paquete de compatibilidad de Windows](/dotnet/core/porting/windows-compat-pack) ha permitido que miles de API solo de Windows estén disponibles en .NET Core.</span><span class="sxs-lookup"><span data-stu-id="f1024-145">The [Windows Compatibility Pack](/dotnet/core/porting/windows-compat-pack) made thousands of Windows-only APIs available in .NET Core.</span></span> <span data-ttu-id="f1024-146">Estas API no estaban disponibles en .NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="f1024-146">These APIs weren't available in .NET Core 1.x.</span></span>

## <a name="recommended-learning-path"></a><span data-ttu-id="f1024-147">Ruta de aprendizaje recomendada</span><span class="sxs-lookup"><span data-stu-id="f1024-147">Recommended learning path</span></span>

<span data-ttu-id="f1024-148">Se recomienda la siguiente secuencia de tutoriales y artículos para obtener una introducción para desarrollar aplicaciones de ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="f1024-148">We recommend the following sequence of tutorials and articles for an introduction to developing ASP.NET Core apps:</span></span>

1. <span data-ttu-id="f1024-149">Siga un tutorial para el tipo de aplicación que quiere desarrollar o mantener:</span><span class="sxs-lookup"><span data-stu-id="f1024-149">Follow a tutorial for the type of app you want to develop or maintain:</span></span>

   |<span data-ttu-id="f1024-150">Tipo de aplicación</span><span class="sxs-lookup"><span data-stu-id="f1024-150">App type</span></span>  |<span data-ttu-id="f1024-151">Escenario</span><span class="sxs-lookup"><span data-stu-id="f1024-151">Scenario</span></span>  |<span data-ttu-id="f1024-152">Tutorial</span><span class="sxs-lookup"><span data-stu-id="f1024-152">Tutorial</span></span>  |
   |----------|----------|----------|
   |<span data-ttu-id="f1024-153">Aplicación web</span><span class="sxs-lookup"><span data-stu-id="f1024-153">Web app</span></span>                   | <span data-ttu-id="f1024-154">Para un nuevo desarrollo</span><span class="sxs-lookup"><span data-stu-id="f1024-154">For new development</span></span>        |[<span data-ttu-id="f1024-155">Introducción a las páginas de Razor</span><span class="sxs-lookup"><span data-stu-id="f1024-155">Get started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start) |
   |<span data-ttu-id="f1024-156">Aplicación web</span><span class="sxs-lookup"><span data-stu-id="f1024-156">Web app</span></span>                   | <span data-ttu-id="f1024-157">Para mantener una aplicación MVC</span><span class="sxs-lookup"><span data-stu-id="f1024-157">For maintaining an MVC app</span></span> |[<span data-ttu-id="f1024-158">Introducción a MVC</span><span class="sxs-lookup"><span data-stu-id="f1024-158">Get started with MVC</span></span>](xref:tutorials/first-mvc-app/start-mvc)|
   |<span data-ttu-id="f1024-159">Web API</span><span class="sxs-lookup"><span data-stu-id="f1024-159">Web API</span></span>                   |                            |<span data-ttu-id="f1024-160">[Creación de una API web](xref:tutorials/first-web-api)\*</span><span class="sxs-lookup"><span data-stu-id="f1024-160">[Create a web API](xref:tutorials/first-web-api)\*</span></span>  |
   |<span data-ttu-id="f1024-161">Aplicación en tiempo real</span><span class="sxs-lookup"><span data-stu-id="f1024-161">Real-time app</span></span>             |                            |[<span data-ttu-id="f1024-162">Introducción a SignalR</span><span class="sxs-lookup"><span data-stu-id="f1024-162">Get started with SignalR</span></span>](xref:tutorials/signalr) |
   |<span data-ttu-id="f1024-163">Aplicación de Blazor</span><span class="sxs-lookup"><span data-stu-id="f1024-163">Blazor app</span></span>                |                            |[<span data-ttu-id="f1024-164">Introducción a Blazor</span><span class="sxs-lookup"><span data-stu-id="f1024-164">Get started with Blazor</span></span>](xref:blazor/get-started) |
   |<span data-ttu-id="f1024-165">Aplicación de llamada a procedimiento remoto</span><span class="sxs-lookup"><span data-stu-id="f1024-165">Remote Procedure Call app</span></span> |                            |[<span data-ttu-id="f1024-166">Introducción a un servicio gRPC</span><span class="sxs-lookup"><span data-stu-id="f1024-166">Get started with a gRPC service</span></span>](xref:tutorials/grpc/grpc-start) |

1. <span data-ttu-id="f1024-167">Siga un tutorial que muestra cómo realizar el acceso a datos básicos:</span><span class="sxs-lookup"><span data-stu-id="f1024-167">Follow a tutorial that shows how to do basic data access:</span></span>

   |<span data-ttu-id="f1024-168">Escenario</span><span class="sxs-lookup"><span data-stu-id="f1024-168">Scenario</span></span>  |<span data-ttu-id="f1024-169">Tutorial</span><span class="sxs-lookup"><span data-stu-id="f1024-169">Tutorial</span></span>  |
   |----------|----------|
   | <span data-ttu-id="f1024-170">Para un nuevo desarrollo</span><span class="sxs-lookup"><span data-stu-id="f1024-170">For new development</span></span>        |[<span data-ttu-id="f1024-171"> Razor Pages con Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="f1024-171">Razor Pages with Entity Framework Core</span></span>](xref:data/ef-rp/intro) |
   | <span data-ttu-id="f1024-172">Para mantener una aplicación MVC</span><span class="sxs-lookup"><span data-stu-id="f1024-172">For maintaining an MVC app</span></span> |[<span data-ttu-id="f1024-173">MVC con Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="f1024-173">MVC with Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

1. <span data-ttu-id="f1024-174">Lea una introducción a las características de ASP.NET Core que se aplican a todos los tipos de aplicaciones:</span><span class="sxs-lookup"><span data-stu-id="f1024-174">Read an overview of ASP.NET Core features that apply to all app types:</span></span>

   * [<span data-ttu-id="f1024-175">Aspectos básicos</span><span class="sxs-lookup"><span data-stu-id="f1024-175">Fundamentals</span></span>](xref:fundamentals/index)

1. <span data-ttu-id="f1024-176">Examine la tabla de contenido para ver otros temas de interés.</span><span class="sxs-lookup"><span data-stu-id="f1024-176">Browse the Table of Contents for other topics of interest.</span></span>

<span data-ttu-id="f1024-177">\* Hay un nuevo [tutorial de API web que sigue completamente en el explorador](https://docs.microsoft.com/learn/modules/build-web-api-net-core), no es necesaria una instalación del IDE local.</span><span class="sxs-lookup"><span data-stu-id="f1024-177">\* There is a new [web API tutorial that you follow entirely in the browser](https://docs.microsoft.com/learn/modules/build-web-api-net-core), no local IDE installation required.</span></span>  <span data-ttu-id="f1024-178">El código se ejecuta en un [Azure Cloud Shell](https://azure.microsoft.com/features/cloud-shell/) y se usa [curl](https://curl.haxx.se/) para realizar pruebas.</span><span class="sxs-lookup"><span data-stu-id="f1024-178">The code runs in an [Azure Cloud Shell](https://azure.microsoft.com/features/cloud-shell/), and [curl](https://curl.haxx.se/) is used for testing.</span></span>

## <a name="migration-from-the-net-framework"></a><span data-ttu-id="f1024-179">Migración desde .NET Framework</span><span class="sxs-lookup"><span data-stu-id="f1024-179">Migration from the .NET Framework</span></span>

<span data-ttu-id="f1024-180">Para obtener una guía de referencia para migrar aplicaciones de ASP.NET a ASP.NET Core, vea <migration/proper-to-2x/index>.</span><span class="sxs-lookup"><span data-stu-id="f1024-180">For a reference guide to migrating ASP.NET apps to ASP.NET Core, see <migration/proper-to-2x/index>.</span></span>

## <a name="how-to-download-a-sample"></a><span data-ttu-id="f1024-181">Cómo descargar un ejemplo</span><span class="sxs-lookup"><span data-stu-id="f1024-181">How to download a sample</span></span>

<span data-ttu-id="f1024-182">En muchos de los artículos y tutoriales se incluyen vínculos a código de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="f1024-182">Many of the articles and tutorials include links to sample code.</span></span>

1. <span data-ttu-id="f1024-183">[Descargue el archivo ZIP del repositorio de ASP.NET](https://codeload.github.com/aspnet/AspNetCore.Docs/zip/master).</span><span class="sxs-lookup"><span data-stu-id="f1024-183">[Download the ASP.NET repository zip file](https://codeload.github.com/aspnet/AspNetCore.Docs/zip/master).</span></span>
1. <span data-ttu-id="f1024-184">Descomprima el archivo *Docs-master.zip*.</span><span class="sxs-lookup"><span data-stu-id="f1024-184">Unzip the *Docs-master.zip* file.</span></span>
1. <span data-ttu-id="f1024-185">Use la dirección URL del vínculo de ejemplo para ir al directorio de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="f1024-185">Use the URL in the sample link to help you navigate to the sample directory.</span></span>

### <a name="preprocessor-directives-in-sample-code"></a><span data-ttu-id="f1024-186">Directivas de preprocesador en código de ejemplo</span><span class="sxs-lookup"><span data-stu-id="f1024-186">Preprocessor directives in sample code</span></span>

<span data-ttu-id="f1024-187">Para mostrar varios escenarios, las aplicaciones de ejemplo usan las directivas de preprocesador `#define` y `#if-#else/#elif-#endif` para compilar de forma selectiva y ejecutar secciones distintas de código de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="f1024-187">To demonstrate multiple scenarios, sample apps use the `#define` and `#if-#else/#elif-#endif` preprocessor directives to selectively compile and run different sections of sample code.</span></span> <span data-ttu-id="f1024-188">Para los ejemplos que usan este enfoque, establezca la directiva `#define` en la parte superior de los archivos C# para definir el símbolo asociado con el escenario que quiera ejecutar.</span><span class="sxs-lookup"><span data-stu-id="f1024-188">For those samples that make use of this approach, set the `#define` directive at the top of the C# files to define the symbol associated with the scenario that you want to run.</span></span> <span data-ttu-id="f1024-189">Algunos ejemplos requieren la definición del símbolo en la parte superior de varios archivos para ejecutar un escenario.</span><span class="sxs-lookup"><span data-stu-id="f1024-189">Some samples require defining the symbol at the top of multiple files in order to run a scenario.</span></span>

<span data-ttu-id="f1024-190">Por ejemplo, la siguiente lista de símbolos de `#define` indica que hay cuatro escenarios disponibles (un escenario por símbolo).</span><span class="sxs-lookup"><span data-stu-id="f1024-190">For example, the following `#define` symbol list indicates that four scenarios are available (one scenario per symbol).</span></span> <span data-ttu-id="f1024-191">La configuración de ejemplo actual ejecuta el escenario `TemplateCode`:</span><span class="sxs-lookup"><span data-stu-id="f1024-191">The current sample configuration runs the `TemplateCode` scenario:</span></span>

```csharp
#define TemplateCode // or LogFromMain or ExpandDefault or FilterInCode
```

<span data-ttu-id="f1024-192">Para cambiar el ejemplo el escenario `ExpandDefault`, defina el símbolo `ExpandDefault` y deje los símbolos restantes comentados:</span><span class="sxs-lookup"><span data-stu-id="f1024-192">To change the sample to run the `ExpandDefault` scenario, define the `ExpandDefault` symbol and leave the remaining symbols commented-out:</span></span>

```csharp
#define ExpandDefault // TemplateCode or LogFromMain or FilterInCode
```

<span data-ttu-id="f1024-193">Para obtener información sobre cómo usar [directivas de preprocesador de C#](/dotnet/csharp/language-reference/preprocessor-directives/) para compilar selectivamente secciones de código, vea [#define (Referencia de C#)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-define) e [#if (Referencia de C#)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-if).</span><span class="sxs-lookup"><span data-stu-id="f1024-193">For more information on using [C# preprocessor directives](/dotnet/csharp/language-reference/preprocessor-directives/) to selectively compile sections of code, see [#define (C# Reference)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-define) and [#if (C# Reference)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-if).</span></span>

### <a name="regions-in-sample-code"></a><span data-ttu-id="f1024-194">Regiones en código de ejemplo</span><span class="sxs-lookup"><span data-stu-id="f1024-194">Regions in sample code</span></span>

<span data-ttu-id="f1024-195">Algunas aplicaciones de ejemplo contienen secciones de código rodeadas de las directivas [#region](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-region) y [#end-region](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-endregion) de C#.</span><span class="sxs-lookup"><span data-stu-id="f1024-195">Some sample apps contain sections of code surrounded by [#region](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-region) and [#endregion](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-endregion) C# directives.</span></span> <span data-ttu-id="f1024-196">El sistema de creación de documentación inserta estas regiones en los temas de documentación representados.</span><span class="sxs-lookup"><span data-stu-id="f1024-196">The documentation build system injects these regions into the rendered documentation topics.</span></span>  

<span data-ttu-id="f1024-197">Normalmente, los nombres de región contienen la palabra "snippet".</span><span class="sxs-lookup"><span data-stu-id="f1024-197">Region names usually contain the word "snippet."</span></span> <span data-ttu-id="f1024-198">En el ejemplo siguiente se muestra una región denominada `snippet_WebHostDefaults`:</span><span class="sxs-lookup"><span data-stu-id="f1024-198">The following example shows a region named `snippet_WebHostDefaults`:</span></span>

```csharp
#region snippet_WebHostDefaults
Host.CreateDefaultBuilder(args)
    .ConfigureWebHostDefaults(webBuilder =>
    {
        webBuilder.UseStartup<Startup>();
    });
#endregion
```

<span data-ttu-id="f1024-199">En el archivo Markdown del tema se hace referencia al fragmento de código de C# anterior con la siguiente línea:</span><span class="sxs-lookup"><span data-stu-id="f1024-199">The preceding C# code snippet is referenced in the topic's markdown file with the following line:</span></span>

```md
[!code-csharp[](sample/SampleApp/Program.cs?name=snippet_WebHostDefaults)]
```

<span data-ttu-id="f1024-200">Puede ignorar sin problemas (o incluso quitar) las directivas `#region` y `#endregion` que rodean el código.</span><span class="sxs-lookup"><span data-stu-id="f1024-200">You may safely ignore (or remove) the `#region` and `#endregion` directives that surround the code.</span></span> <span data-ttu-id="f1024-201">No altere el código de estas directivas si tiene planeado ejecutar los escenarios de ejemplo descritos en el tema.</span><span class="sxs-lookup"><span data-stu-id="f1024-201">Don't alter the code within these directives if you plan to run the sample scenarios described in the topic.</span></span> <span data-ttu-id="f1024-202">Puede alterarlo si quiere experimentar con otros escenarios.</span><span class="sxs-lookup"><span data-stu-id="f1024-202">Feel free to alter the code when experimenting with other scenarios.</span></span>

<span data-ttu-id="f1024-203">Para obtener más información, consulte [Contribute to the ASP.NET documentation: Code snippets](https://github.com/aspnet/AspNetCore.Docs/blob/master/CONTRIBUTING.md#code-snippets) (Contribución a la documentación de ASP.NET: fragmentos de código).</span><span class="sxs-lookup"><span data-stu-id="f1024-203">For more information, see [Contribute to the ASP.NET documentation: Code snippets](https://github.com/aspnet/AspNetCore.Docs/blob/master/CONTRIBUTING.md#code-snippets).</span></span>

## <a name="next-steps"></a><span data-ttu-id="f1024-204">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="f1024-204">Next steps</span></span>

<span data-ttu-id="f1024-205">Para obtener más información, vea los siguientes recursos:</span><span class="sxs-lookup"><span data-stu-id="f1024-205">For more information, see the following resources:</span></span>

* <xref:getting-started>
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [<span data-ttu-id="f1024-206">Conceptos básicos de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f1024-206">ASP.NET Core fundamentals</span></span>](xref:fundamentals/index)
* <span data-ttu-id="f1024-207">[La reunión semanal de la comunidad de ASP.NET](https://live.asp.net/) trata el progreso y los planes del equipo.</span><span class="sxs-lookup"><span data-stu-id="f1024-207">[The weekly ASP.NET community standup](https://live.asp.net/) covers the team's progress and plans.</span></span> <span data-ttu-id="f1024-208">Incluye nuevos blogs y nuevo software de terceros.</span><span class="sxs-lookup"><span data-stu-id="f1024-208">It features new blogs and third-party software.</span></span>
