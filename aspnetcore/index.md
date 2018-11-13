---
title: Introducción a ASP.NET Core
author: rick-anderson
description: Consulte una introducción a ASP.NET Core, un marco multiplataforma de código abierto y de alto rendimiento que tiene como finalidad compilar modernas aplicaciones conectadas a Internet y basadas en la nube.
ms.author: riande
ms.custom: mvc
ms.date: 11/10/2018
uid: index
ms.openlocfilehash: 1699acc0086dfd50c573afc239bc8f37eb9e7af9
ms.sourcegitcommit: 408921a932448f66cb46fd53c307a864f5323fe5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2018
ms.locfileid: "51569993"
---
# <a name="introduction-to-aspnet-core"></a><span data-ttu-id="a92e4-103">Introducción a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a92e4-103">Introduction to ASP.NET Core</span></span>

<span data-ttu-id="a92e4-104">Por [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT) y [Shaun Luttin](https://twitter.com/dicshaunary)</span><span class="sxs-lookup"><span data-stu-id="a92e4-104">By [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Shaun Luttin](https://twitter.com/dicshaunary)</span></span>

<span data-ttu-id="a92e4-105">ASP.NET Core es un marco multiplataforma de [código abierto](https://github.com/aspnet/home) y de alto rendimiento que tiene como finalidad compilar modernas aplicaciones conectadas a Internet y basadas en la nube.</span><span class="sxs-lookup"><span data-stu-id="a92e4-105">ASP.NET Core is a cross-platform, high-performance, [open-source](https://github.com/aspnet/home) framework for building modern, cloud-based, Internet-connected applications.</span></span> <span data-ttu-id="a92e4-106">Con ASP.NET Core puede hacer lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="a92e4-106">With ASP.NET Core, you can:</span></span>

* <span data-ttu-id="a92e4-107">Compilar servicios y aplicaciones web, aplicaciones de [IoT](https://www.microsoft.com/internet-of-things/) y back-ends móviles.</span><span class="sxs-lookup"><span data-stu-id="a92e4-107">Build web apps and services, [IoT](https://www.microsoft.com/internet-of-things/) apps, and mobile backends.</span></span>
* <span data-ttu-id="a92e4-108">Usar sus herramientas de desarrollo favoritas en Windows, macOS y Linux.</span><span class="sxs-lookup"><span data-stu-id="a92e4-108">Use your favorite development tools on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="a92e4-109">Efectuar implementaciones locales y en la nube.</span><span class="sxs-lookup"><span data-stu-id="a92e4-109">Deploy to the cloud or on-premises.</span></span>
* <span data-ttu-id="a92e4-110">Ejecutarlo en [.NET Core o en .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="a92e4-110">Run on [.NET Core or .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="why-use-aspnet-core"></a><span data-ttu-id="a92e4-111">¿Por qué debería usar ASP.NET Core?</span><span class="sxs-lookup"><span data-stu-id="a92e4-111">Why use ASP.NET Core?</span></span>

<span data-ttu-id="a92e4-112">Millones de desarrolladores han usado [ASP.NET 4.x](/aspnet/overview) (y siguen usándolo) para crear aplicaciones web.</span><span class="sxs-lookup"><span data-stu-id="a92e4-112">Millions of developers have used (and continue to use) [ASP.NET 4.x](/aspnet/overview) to create web apps.</span></span> <span data-ttu-id="a92e4-113">ASP.NET Core es un nuevo diseño de ASP.NET 4.x que cuenta con cambios en la arquitectura que dan como resultado un marco más sencillo y modular.</span><span class="sxs-lookup"><span data-stu-id="a92e4-113">ASP.NET Core is a redesign of ASP.NET 4.x, with architectural changes that result in a leaner, more modular framework.</span></span>

[!INCLUDE[](~/includes/benefits.md)]

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a><span data-ttu-id="a92e4-114">Creación de API web e interfaces de usuario web mediante ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="a92e4-114">Build web APIs and web UI using ASP.NET Core MVC</span></span>

<span data-ttu-id="a92e4-115">ASP.NET Core MVC proporciona características para crear [API web](xref:tutorials/first-web-api) y [aplicaciones web](xref:tutorials/razor-pages/index):</span><span class="sxs-lookup"><span data-stu-id="a92e4-115">ASP.NET Core MVC provides features to build [web APIs](xref:tutorials/first-web-api) and [web apps](xref:tutorials/razor-pages/index):</span></span>

* <span data-ttu-id="a92e4-116">El [patrón Modelo-Vista-Controlador (MVC)](xref:mvc/overview) permite que se puedan hacer pruebas en las API web y en las aplicaciones web.</span><span class="sxs-lookup"><span data-stu-id="a92e4-116">The [Model-View-Controller (MVC) pattern](xref:mvc/overview) helps make your web APIs and web apps testable.</span></span>
* <span data-ttu-id="a92e4-117">[Páginas de Razor](xref:razor-pages/index) (novedad de ASP.NET Core 2.0) es un modelo de programación basado en páginas que facilita la compilación de interfaces de usuario web y hace que sea más productiva.</span><span class="sxs-lookup"><span data-stu-id="a92e4-117">[Razor Pages](xref:razor-pages/index) (new in ASP.NET Core 2.0) is a page-based programming model that makes building web UI easier and more productive.</span></span>
* <span data-ttu-id="a92e4-118">El [marcado de Razor](xref:mvc/views/razor) proporciona una sintaxis productiva para las [páginas de Razor](xref:razor-pages/index) y las [vistas de MVC](xref:mvc/views/overview).</span><span class="sxs-lookup"><span data-stu-id="a92e4-118">[Razor markup](xref:mvc/views/razor) provides a productive syntax for [Razor Pages](xref:razor-pages/index) and [MVC views](xref:mvc/views/overview).</span></span>
* <span data-ttu-id="a92e4-119">Los [asistentes de etiquetas](xref:mvc/views/tag-helpers/intro) permiten que el código de servidor participe en la creación y la representación de elementos HTML en archivos de Razor.</span><span class="sxs-lookup"><span data-stu-id="a92e4-119">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span>
* <span data-ttu-id="a92e4-120">La compatibilidad integrada para [varios formatos de datos y la negociación de contenidos](xref:web-api/advanced/formatting) permite que las API web lleguen a una amplia gama de clientes, como los exploradores y los dispositivos móviles.</span><span class="sxs-lookup"><span data-stu-id="a92e4-120">Built-in support for [multiple data formats and content negotiation](xref:web-api/advanced/formatting) lets your web APIs reach a broad range of clients, including browsers and mobile devices.</span></span>
* <span data-ttu-id="a92e4-121">El [enlace de modelo](xref:mvc/models/model-binding) asigna automáticamente datos de solicitudes HTTP a parámetros de método de acción.</span><span class="sxs-lookup"><span data-stu-id="a92e4-121">[Model binding](xref:mvc/models/model-binding) automatically maps data from HTTP requests to action method parameters.</span></span>
* <span data-ttu-id="a92e4-122">La [validación de modelos](xref:mvc/models/validation) efectúa una validación del lado cliente y del lado servidor de forma automática.</span><span class="sxs-lookup"><span data-stu-id="a92e4-122">[Model validation](xref:mvc/models/validation) automatically performs client-side and server-side validation.</span></span>

## <a name="client-side-development"></a><span data-ttu-id="a92e4-123">Desarrollo del lado cliente</span><span class="sxs-lookup"><span data-stu-id="a92e4-123">Client-side development</span></span>

<span data-ttu-id="a92e4-124">ASP.NET Core se integra perfectamente con bibliotecas y marcos de trabajo populares del lado cliente, que incluyen [Angular](xref:spa/angular), [React](xref:spa/react) y [Bootstrap](https://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="a92e4-124">ASP.NET Core integrates seamlessly with popular client-side frameworks and libraries, including [Angular](xref:spa/angular), [React](xref:spa/react), and [Bootstrap](https://getbootstrap.com/).</span></span> <span data-ttu-id="a92e4-125">Para obtener más información, vea [Desarrollo del lado cliente](xref:client-side/index).</span><span class="sxs-lookup"><span data-stu-id="a92e4-125">For more information, see [Client-side development](xref:client-side/index).</span></span>

<a name="target-framework"></a>

## <a name="aspnet-core-targeting-net-framework"></a><span data-ttu-id="a92e4-126">ASP.NET Core con .NET Framework como destino</span><span class="sxs-lookup"><span data-stu-id="a92e4-126">ASP.NET Core targeting .NET Framework</span></span>

<span data-ttu-id="a92e4-127">ASP.NET Core 2.x puede tener como destino .NET Core o .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="a92e4-127">ASP.NET Core 2.x can target .NET Core or .NET Framework.</span></span> <span data-ttu-id="a92e4-128">Las aplicaciones de ASP.NET Core que tienen como destino .NET Framework no son multiplataforma, sino que solo se ejecutan en Windows.</span><span class="sxs-lookup"><span data-stu-id="a92e4-128">ASP.NET Core apps targeting .NET Framework aren't cross-platform&mdash;they run on Windows only.</span></span> <span data-ttu-id="a92e4-129">Por lo general, ASP.NET Core 2.x está formado por bibliotecas de [.NET Standard](/dotnet/standard/net-standard).</span><span class="sxs-lookup"><span data-stu-id="a92e4-129">Generally, ASP.NET Core 2.x is made up of [.NET Standard](/dotnet/standard/net-standard) libraries.</span></span> <span data-ttu-id="a92e4-130">Las aplicaciones escritas con .NET Standard 2.0 se ejecutan en cualquier lugar en el que se admita .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="a92e4-130">Apps written with .NET Standard 2.0 run anywhere that .NET Standard 2.0 is supported.</span></span>

<span data-ttu-id="a92e4-131">ASP.NET Core 2.x se admite en las versiones de .NET Framework compatibles con .NET Standard 2.0:</span><span class="sxs-lookup"><span data-stu-id="a92e4-131">ASP.NET Core 2.x is supported on .NET Framework versions compatible with .NET Standard 2.0:</span></span>

* <span data-ttu-id="a92e4-132">Se recomienda encarecidamente .NET Framework 4.7.1 y posterior.</span><span class="sxs-lookup"><span data-stu-id="a92e4-132">.NET Framework 4.7.1 and later is strongly recommended.</span></span>
* <span data-ttu-id="a92e4-133">.NET Framework 4.6.1 y posterior.</span><span class="sxs-lookup"><span data-stu-id="a92e4-133">.NET Framework 4.6.1 and later.</span></span>

<span data-ttu-id="a92e4-134">ASP.NET Core 3.0 y versiones posteriores solo se ejecutan en .NET Core.</span><span class="sxs-lookup"><span data-stu-id="a92e4-134">ASP.NET Core 3.0 and later will only run on .NET Core.</span></span> <span data-ttu-id="a92e4-135">Para obtener más información sobre este cambio, vea [A first look at changes coming in ASP.NET Core 3.0](https://blogs.msdn.microsoft.com/webdev/2018/10/29/a-first-look-at-changes-coming-in-asp-net-core-3-0/) (Descripción general de los cambios que se aplicarán a ASP.NET Core 3.0).</span><span class="sxs-lookup"><span data-stu-id="a92e4-135">For more details regarding this change, see [A first look at changes coming in ASP.NET Core 3.0](https://blogs.msdn.microsoft.com/webdev/2018/10/29/a-first-look-at-changes-coming-in-asp-net-core-3-0/).</span></span>

<span data-ttu-id="a92e4-136">El uso de .NET Core como destino cuenta con varias ventajas que van en aumento con cada versión.</span><span class="sxs-lookup"><span data-stu-id="a92e4-136">There are several advantages to targeting .NET Core, and these advantages increase with each release.</span></span> <span data-ttu-id="a92e4-137">Entre las ventajas del uso de .NET Core en vez de .NET Framework se incluyen las siguientes:</span><span class="sxs-lookup"><span data-stu-id="a92e4-137">Some advantages of .NET Core over .NET Framework include:</span></span>

* <span data-ttu-id="a92e4-138">Multiplataforma.</span><span class="sxs-lookup"><span data-stu-id="a92e4-138">Cross-platform.</span></span> <span data-ttu-id="a92e4-139">Ejecución en macOS, Linux y Windows.</span><span class="sxs-lookup"><span data-stu-id="a92e4-139">Runs on macOS, Linux, and Windows.</span></span>
* <span data-ttu-id="a92e4-140">Rendimiento mejorado</span><span class="sxs-lookup"><span data-stu-id="a92e4-140">Improved performance</span></span>
* <span data-ttu-id="a92e4-141">Control de versiones en paralelo.</span><span class="sxs-lookup"><span data-stu-id="a92e4-141">Side-by-side versioning</span></span>
* <span data-ttu-id="a92e4-142">Nuevas API.</span><span class="sxs-lookup"><span data-stu-id="a92e4-142">New APIs</span></span>
* <span data-ttu-id="a92e4-143">Abrir origen</span><span class="sxs-lookup"><span data-stu-id="a92e4-143">Open source</span></span>

<span data-ttu-id="a92e4-144">Estamos trabajando intensamente para cerrar la brecha de API entre .NET Framework y .NET Core.</span><span class="sxs-lookup"><span data-stu-id="a92e4-144">We're working hard to close the API gap from .NET Framework to .NET Core.</span></span> <span data-ttu-id="a92e4-145">El [paquete de compatibilidad de Windows](/dotnet/core/porting/windows-compat-pack) ha permitido que miles de API solo de Windows estén disponibles en .NET Core.</span><span class="sxs-lookup"><span data-stu-id="a92e4-145">The [Windows Compatibility Pack](/dotnet/core/porting/windows-compat-pack) made thousands of Windows-only APIs available in .NET Core.</span></span> <span data-ttu-id="a92e4-146">Estas API no estaban disponibles en .NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="a92e4-146">These APIs weren't available in .NET Core 1.x.</span></span>

## <a name="how-to-download-a-sample"></a><span data-ttu-id="a92e4-147">Cómo descargar un ejemplo</span><span class="sxs-lookup"><span data-stu-id="a92e4-147">How to download a sample</span></span>

<span data-ttu-id="a92e4-148">En muchos de los artículos y tutoriales se incluyen vínculos a código de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="a92e4-148">Many of the articles and tutorials include links to sample code.</span></span>

1. <span data-ttu-id="a92e4-149">[Descargue el archivo ZIP del repositorio de ASP.NET](https://codeload.github.com/aspnet/Docs/zip/master).</span><span class="sxs-lookup"><span data-stu-id="a92e4-149">[Download the ASP.NET repository zip file](https://codeload.github.com/aspnet/Docs/zip/master).</span></span>
1. <span data-ttu-id="a92e4-150">Descomprima el archivo *Docs-master.zip*.</span><span class="sxs-lookup"><span data-stu-id="a92e4-150">Unzip the *Docs-master.zip* file.</span></span>
1. <span data-ttu-id="a92e4-151">Use la dirección URL del vínculo de ejemplo para ir al directorio de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="a92e4-151">Use the URL in the sample link to help you navigate to the sample directory.</span></span>

<span data-ttu-id="a92e4-152">Para mostrar varios escenarios, las aplicaciones de ejemplo usan las instrucciones `#define` y `#if-#else/#elif-#endif` de C# para compilar de forma selectiva y ejecutar secciones distintas de código de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="a92e4-152">To demonstrate multiple scenarios, sample apps make use of the `#define` and `#if-#else/#elif-#endif` C# statements to selectively compile and run different sections of sample code.</span></span> <span data-ttu-id="a92e4-153">Para los ejemplos que usan este enfoque, establezca la instrucción `#define` en la parte superior de los archivos C# con el símbolo asociado con el escenario que quiera ejecutar.</span><span class="sxs-lookup"><span data-stu-id="a92e4-153">For those samples that make use of this approach, set the `#define` statement at the top of the C# files to the symbol associated with the scenario that you want to run.</span></span> <span data-ttu-id="a92e4-154">Es posible que un ejemplo requiere establecer el símbolo en la parte superior de varios archivos para ejecutar un escenario.</span><span class="sxs-lookup"><span data-stu-id="a92e4-154">A sample may require you to set the symbol at the top of multiple files in order to run a scenario.</span></span>

<span data-ttu-id="a92e4-155">Por ejemplo, la siguiente lista de símbolos de `#define` indica que hay cuatro escenarios disponibles (un escenario por símbolo).</span><span class="sxs-lookup"><span data-stu-id="a92e4-155">For example, the following `#define` symbol list indicates that four scenarios are available (one scenario per symbol).</span></span> <span data-ttu-id="a92e4-156">La configuración de ejemplo actual ejecuta el escenario `TemplateCode`:</span><span class="sxs-lookup"><span data-stu-id="a92e4-156">The current sample configuration runs the `TemplateCode` scenario:</span></span>

```csharp
#define TemplateCode // or LogFromMain or ExpandDefault or FilterInCode
```

<span data-ttu-id="a92e4-157">Para cambiar el ejemplo el escenario `ExpandDefault`, defina el símbolo `ExpandDefault` y deje los símbolos restantes comentados:</span><span class="sxs-lookup"><span data-stu-id="a92e4-157">To change the sample to run the `ExpandDefault` scenario, define the `ExpandDefault` symbol and leave the remaining symbols commented-out:</span></span>

```csharp
#define ExpandDefault // TemplateCode or LogFromMain or FilterInCode
```

<span data-ttu-id="a92e4-158">Para obtener información sobre cómo usar [directivas de preprocesador de C#](/dotnet/csharp/language-reference/preprocessor-directives/) para compilar selectivamente secciones de código, vea [#define (Referencia de C#)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-define) e [#if (Referencia de C#)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-if).</span><span class="sxs-lookup"><span data-stu-id="a92e4-158">For more information on using [C# preprocessor directives](/dotnet/csharp/language-reference/preprocessor-directives/) to selectively compile sections of code, see [#define (C# Reference)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-define) and [#if (C# Reference)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-if).</span></span>

## <a name="next-steps"></a><span data-ttu-id="a92e4-159">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="a92e4-159">Next steps</span></span>

<span data-ttu-id="a92e4-160">Para obtener más información, vea los siguientes recursos:</span><span class="sxs-lookup"><span data-stu-id="a92e4-160">For more information, see the following resources:</span></span>

* [<span data-ttu-id="a92e4-161">Introducción a las páginas de Razor</span><span class="sxs-lookup"><span data-stu-id="a92e4-161">Get started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [<span data-ttu-id="a92e4-162">Conceptos básicos de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a92e4-162">ASP.NET Core fundamentals</span></span>](xref:fundamentals/index)
* <span data-ttu-id="a92e4-163">[La reunión semanal de la comunidad de ASP.NET](https://live.asp.net/) trata el progreso y los planes del equipo.</span><span class="sxs-lookup"><span data-stu-id="a92e4-163">[The weekly ASP.NET community standup](https://live.asp.net/) covers the team's progress and plans.</span></span> <span data-ttu-id="a92e4-164">Incluye nuevos blogs y nuevo software de terceros.</span><span class="sxs-lookup"><span data-stu-id="a92e4-164">It features new blogs and third-party software.</span></span>
