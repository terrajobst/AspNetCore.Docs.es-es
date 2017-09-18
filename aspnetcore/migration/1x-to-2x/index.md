---
title: "Migración de ASP.NET Core 1.x a 2.0"
author: scottaddie
description: "En este artículo se indican los requisitos previos y los pasos más comunes para migrar un proyecto de ASP.NET Core 1.x a ASP.NET Core 2.0."
keywords: "ASP.NET Core,migración"
ms.author: scaddie
manager: wpickett
ms.date: 08/01/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/1x-to-2x/index
ms.openlocfilehash: c14e7d61e8b353c18fc4a4f2bf3658069982bad5
ms.sourcegitcommit: e832a9b9f41a8b26a8c88edfd8fc35b8bfd97d5d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/18/2017
---
# <a name="migrating-from-aspnet-core-1x-to-aspnet-core-20"></a><span data-ttu-id="46f2e-104">Migración de ASP.NET Core 1.x a ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="46f2e-104">Migrating from ASP.NET Core 1.x to ASP.NET Core 2.0</span></span>

<span data-ttu-id="46f2e-105">Por [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="46f2e-105">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="46f2e-106">En este artículo se le guía a lo largo del proceso de actualización de un proyecto existente de ASP.NET Core 1.x a ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="46f2e-106">In this article, we'll walk you through updating an existing ASP.NET Core 1.x project to ASP.NET Core 2.0.</span></span> <span data-ttu-id="46f2e-107">La migración de la aplicación a ASP.NET Core 2.0 permite aprovechar [muchas características nuevas y mejoras de rendimiento](https://go.microsoft.com/fwlink/?linkid=854094).</span><span class="sxs-lookup"><span data-stu-id="46f2e-107">Migrating your application to ASP.NET Core 2.0 enables you to take advantage of [many new features and performance improvements](https://go.microsoft.com/fwlink/?linkid=854094).</span></span> 

<span data-ttu-id="46f2e-108">Las aplicaciones existentes de ASP.NET Core 1.x se basan en plantillas de proyecto específicas de la versión.</span><span class="sxs-lookup"><span data-stu-id="46f2e-108">Existing ASP.NET Core 1.x applications are based off of version-specific project templates.</span></span> <span data-ttu-id="46f2e-109">A medida que el marco de trabajo de ASP.NET Core evoluciona, también lo hacen las plantillas de proyecto y el código de inicio incluido en ellas.</span><span class="sxs-lookup"><span data-stu-id="46f2e-109">As the ASP.NET Core framework evolves, so do the project templates and the starter code contained within them.</span></span> <span data-ttu-id="46f2e-110">Además de actualizar el marco de trabajo de ASP.NET Core, debe actualizar el código de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="46f2e-110">In addition to updating the ASP.NET Core framework, you need to update the code for your application.</span></span>

<a name="prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="46f2e-111">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="46f2e-111">Prerequisites</span></span>
<span data-ttu-id="46f2e-112">Vea [Introducción a ASP.NET Core](xref:getting-started).</span><span class="sxs-lookup"><span data-stu-id="46f2e-112">Please see [Getting Started with ASP.NET Core](xref:getting-started).</span></span>

<a name="tfm"></a>

## <a name="update-target-framework-moniker-tfm"></a><span data-ttu-id="46f2e-113">Actualización del moniker de la plataforma de destino (TFM)</span><span class="sxs-lookup"><span data-stu-id="46f2e-113">Update Target Framework Moniker (TFM)</span></span>
<span data-ttu-id="46f2e-114">Los proyectos para .NET Core deben usar el [TFM](/dotnet/standard/frameworks#referring-to-frameworks) de una versión mayor o igual que .NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="46f2e-114">Projects targeting .NET Core should use the [TFM](/dotnet/standard/frameworks#referring-to-frameworks) of a version greater than or equal to .NET Core 2.0.</span></span> <span data-ttu-id="46f2e-115">Busque el nodo `<TargetFramework>` del archivo *.csproj* y reemplace su texto interno por `netcoreapp2.0`:</span><span class="sxs-lookup"><span data-stu-id="46f2e-115">Search for the `<TargetFramework>` node in the *.csproj* file, and replace its inner text with `netcoreapp2.0`:</span></span>

<span data-ttu-id="46f2e-116">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App.csproj?range=3)]</span><span class="sxs-lookup"><span data-stu-id="46f2e-116">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App.csproj?range=3)]</span></span>

<span data-ttu-id="46f2e-117">Los proyectos para .NET Framework deben usar el TFM de una versión mayor o igual que .NET Framework 4.6.1.</span><span class="sxs-lookup"><span data-stu-id="46f2e-117">Projects targeting .NET Framework should use the TFM of a version greater than or equal to .NET Framework 4.6.1.</span></span> <span data-ttu-id="46f2e-118">Busque el nodo `<TargetFramework>` del archivo *.csproj* y reemplace su texto interno por `net461`:</span><span class="sxs-lookup"><span data-stu-id="46f2e-118">Search for the `<TargetFramework>` node in the *.csproj* file, and replace its inner text with `net461`:</span></span>

<span data-ttu-id="46f2e-119">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=4)]</span><span class="sxs-lookup"><span data-stu-id="46f2e-119">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=4)]</span></span>

> [!NOTE]
> <span data-ttu-id="46f2e-120">.NET Core 2.0 ofrece un área expuesta mucho mayor que .NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="46f2e-120">.NET Core 2.0 offers a much larger surface area than .NET Core 1.x.</span></span> <span data-ttu-id="46f2e-121">Si el destino es .NET Framework solo porque faltan API en .NET Core 1.x, el uso de .NET Core 2.0 como destino es probable que dé resultado.</span><span class="sxs-lookup"><span data-stu-id="46f2e-121">If you're targeting .NET Framework solely because of missing APIs in .NET Core 1.x, targeting .NET Core 2.0 is likely to work.</span></span>

<a name="global-json"></a>

## <a name="update-net-core-sdk-version-in-globaljson"></a><span data-ttu-id="46f2e-122">Actualización de la versión del SDK de .NET Core en global.json</span><span class="sxs-lookup"><span data-stu-id="46f2e-122">Update .NET Core SDK version in global.json</span></span>
<span data-ttu-id="46f2e-123">Si la solución se basa en un archivo [*global.json*](https://docs.microsoft.com/dotnet/core/tools/global-json) para que el destino sea una versión específica del SDK de .NET Core, actualice su propiedad `version` para usar la versión 2.0 instalada en el equipo:</span><span class="sxs-lookup"><span data-stu-id="46f2e-123">If your solution relies upon a [*global.json*](https://docs.microsoft.com/dotnet/core/tools/global-json) file to target a specific .NET Core SDK version, update its `version` property to use the 2.0 version installed on your machine:</span></span>

<span data-ttu-id="46f2e-124">[!code-json[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/global.json?highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="46f2e-124">[!code-json[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/global.json?highlight=3)]</span></span>

<a name="package-reference"></a>

## <a name="update-package-references"></a><span data-ttu-id="46f2e-125">Actualización de las referencias del paquete</span><span class="sxs-lookup"><span data-stu-id="46f2e-125">Update package references</span></span>
<span data-ttu-id="46f2e-126">El archivo *.csproj* de un proyecto de 1.x enumera cada paquete NuGet usado por el proyecto.</span><span class="sxs-lookup"><span data-stu-id="46f2e-126">The *.csproj* file in a 1.x project lists each NuGet package used by the project.</span></span>

<span data-ttu-id="46f2e-127">En un proyecto de ASP.NET Core 2.0 para .NET Core 2.0, una sola referencia de [metapaquete](xref:fundamentals/metapackage) del archivo *.csproj* reemplaza a la colección de paquetes:</span><span class="sxs-lookup"><span data-stu-id="46f2e-127">In an ASP.NET Core 2.0 project targeting .NET Core 2.0, a single [metapackage](xref:fundamentals/metapackage) reference in the *.csproj* file replaces the collection of packages:</span></span>

<span data-ttu-id="46f2e-128">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App.csproj?range=9-11)]</span><span class="sxs-lookup"><span data-stu-id="46f2e-128">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App.csproj?range=9-11)]</span></span>

<span data-ttu-id="46f2e-129">Todas las características de ASP.NET Core 2.0 y Entity Framework Core 2.0 están incluidas en el metapaquete.</span><span class="sxs-lookup"><span data-stu-id="46f2e-129">All the features of ASP.NET Core 2.0 and Entity Framework Core 2.0 are included in the metapackage.</span></span>

<span data-ttu-id="46f2e-130">Los proyectos de ASP.NET Core 2.0 para .NET Framework deben seguir haciendo referencia a paquetes NuGet individuales.</span><span class="sxs-lookup"><span data-stu-id="46f2e-130">ASP.NET Core 2.0 projects targeting .NET Framework should continue to reference individual NuGet packages.</span></span> <span data-ttu-id="46f2e-131">Actualización del atributo `Version` de cada nodo `<PackageReference />` a 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="46f2e-131">Update the `Version` attribute of each `<PackageReference />` node to 2.0.0.</span></span>

<span data-ttu-id="46f2e-132">Por ejemplo, esta es la lista de nodos `<PackageReference />` usados en un proyecto típico de ASP.NET Core 2.0 para .NET Framework:</span><span class="sxs-lookup"><span data-stu-id="46f2e-132">For example, here's the list of `<PackageReference />` nodes used in a typical ASP.NET Core 2.0 project targeting .NET Framework:</span></span>

<span data-ttu-id="46f2e-133">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=9-22)]</span><span class="sxs-lookup"><span data-stu-id="46f2e-133">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=9-22)]</span></span>

<a name="dot-net-cli-tool-reference"></a>

## <a name="update-net-core-cli-tools"></a><span data-ttu-id="46f2e-134">Actualización de las herramientas de la CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="46f2e-134">Update .NET Core CLI tools</span></span>
<span data-ttu-id="46f2e-135">En el archivo *.csproj*, actualice el atributo `Version` de cada nodo `<DotNetCliToolReference />` a 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="46f2e-135">In the *.csproj* file, update the `Version` attribute of each `<DotNetCliToolReference />` node to 2.0.0.</span></span>

<span data-ttu-id="46f2e-136">Por ejemplo, esta es la lista de herramientas de la CLI usadas en un proyecto típico de ASP.NET Core 2.0 para .NET Core 2.0:</span><span class="sxs-lookup"><span data-stu-id="46f2e-136">For example, here's the list of CLI tools used in a typical ASP.NET Core 2.0 project targeting .NET Core 2.0:</span></span>

<span data-ttu-id="46f2e-137">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App.csproj?range=13-17)]</span><span class="sxs-lookup"><span data-stu-id="46f2e-137">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App.csproj?range=13-17)]</span></span>

<a name="package-target-fallback"></a>

## <a name="rename-package-target-fallback-property"></a><span data-ttu-id="46f2e-138">Cambio de nombre de la propiedad Package Target Fallback</span><span class="sxs-lookup"><span data-stu-id="46f2e-138">Rename Package Target Fallback property</span></span>
<span data-ttu-id="46f2e-139">El archivo *.csproj* de un proyecto de 1.x usa un nodo `PackageTargetFallback` y una variable:</span><span class="sxs-lookup"><span data-stu-id="46f2e-139">The *.csproj* file of a 1.x project used a `PackageTargetFallback` node and variable:</span></span>

<span data-ttu-id="46f2e-140">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App.csproj?range=5)]</span><span class="sxs-lookup"><span data-stu-id="46f2e-140">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App.csproj?range=5)]</span></span>

<span data-ttu-id="46f2e-141">Cambie el nombre del nodo y la variable a `AssetTargetFallback`:</span><span class="sxs-lookup"><span data-stu-id="46f2e-141">Rename both the node and variable to `AssetTargetFallback`:</span></span>

<span data-ttu-id="46f2e-142">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App.csproj?range=5)]</span><span class="sxs-lookup"><span data-stu-id="46f2e-142">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App.csproj?range=5)]</span></span>

<a name="program-cs"></a>

## <a name="update-main-method-in-programcs"></a><span data-ttu-id="46f2e-143">Actualización del método Main de Program.cs</span><span class="sxs-lookup"><span data-stu-id="46f2e-143">Update Main method in Program.cs</span></span>
<span data-ttu-id="46f2e-144">En los proyectos de 1.x, el método `Main` de *Program.cs* tenía este aspecto:</span><span class="sxs-lookup"><span data-stu-id="46f2e-144">In 1.x projects, the `Main` method of *Program.cs* looked like this:</span></span>

<span data-ttu-id="46f2e-145">[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/Program.cs?name=snippet_ProgramCs&highlight=8-19)]</span><span class="sxs-lookup"><span data-stu-id="46f2e-145">[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/Program.cs?name=snippet_ProgramCs&highlight=8-19)]</span></span>

<span data-ttu-id="46f2e-146">En los proyectos de 2.0, el método `Main` de *Program.cs* se ha simplificado:</span><span class="sxs-lookup"><span data-stu-id="46f2e-146">In 2.0 projects, the `Main` method of *Program.cs* has been simplified:</span></span>

<span data-ttu-id="46f2e-147">[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/Program.cs?highlight=8-11)]</span><span class="sxs-lookup"><span data-stu-id="46f2e-147">[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/Program.cs?highlight=8-11)]</span></span>

<span data-ttu-id="46f2e-148">La adopción de este nuevo patrón 2.0 es muy recomendable y necesaria para que funcionen características de producto como las [migraciones de Entity Framework Core](xref:data/ef-mvc/migrations).</span><span class="sxs-lookup"><span data-stu-id="46f2e-148">The adoption of this new 2.0 pattern is highly recommended and is required for product features like [Entity Framework Core Migrations](xref:data/ef-mvc/migrations) to work.</span></span> <span data-ttu-id="46f2e-149">Por ejemplo, la ejecución de `Update-Database` desde la ventana Consola del Administrador de paquetes o de `dotnet ef database update` desde la línea de comandos (en proyectos convertidos a ASP.NET Core 2.0) genera el error siguiente:</span><span class="sxs-lookup"><span data-stu-id="46f2e-149">For example, running `Update-Database` from the Package Manager Console window or `dotnet ef database update` from the command line (on projects converted to ASP.NET Core 2.0) generates the following error:</span></span>

```
Unable to create an object of type '<Context>'. Add an implementation of 'IDesignTimeDbContextFactory<Context>' to the project, or see https://go.microsoft.com/fwlink/?linkid=851728 for additional patterns supported at design time.
```

<a name="view-compilation"></a>

## <a name="review-your-razor-view-compilation-setting"></a><span data-ttu-id="46f2e-150">Revisión de la configuración de compilación de la vista Razor</span><span class="sxs-lookup"><span data-stu-id="46f2e-150">Review your Razor View Compilation setting</span></span>
<span data-ttu-id="46f2e-151">Un tiempo de inicio de aplicación más rápido y unos lotes publicados más pequeños son de la máxima importancia para el usuario.</span><span class="sxs-lookup"><span data-stu-id="46f2e-151">Faster application startup time and smaller published bundles are of utmost importance to you.</span></span> <span data-ttu-id="46f2e-152">Por ello, la [compilación de la vista Razor](xref:mvc/views/view-compilation) está habilitada de forma predeterminada en ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="46f2e-152">For these reasons, [Razor view compilation](xref:mvc/views/view-compilation) is enabled by default in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="46f2e-153">Ya no es necesario establecer la propiedad `MvcRazorCompileOnPublish` en true.</span><span class="sxs-lookup"><span data-stu-id="46f2e-153">Setting the `MvcRazorCompileOnPublish` property to true is no longer required.</span></span> <span data-ttu-id="46f2e-154">A menos que se esté deshabilitando la compilación de la vista, se puede quitar la propiedad del archivo *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="46f2e-154">Unless you're disabling view compilation, the property may be removed from the *.csproj* file.</span></span>

<span data-ttu-id="46f2e-155">Cuando el destino es .NET Framework, se debe hacer referencia de forma explícita al paquete NuGet [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation) en el archivo *.csproj*:</span><span class="sxs-lookup"><span data-stu-id="46f2e-155">When targeting .NET Framework, you still need to explicitly reference the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation) NuGet package in your *.csproj* file:</span></span>

<span data-ttu-id="46f2e-156">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=15)]</span><span class="sxs-lookup"><span data-stu-id="46f2e-156">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=15)]</span></span>

<a name="app-insights"></a>

## <a name="rely-on-application-insights-light-up-features"></a><span data-ttu-id="46f2e-157">Características "Light-Up" de Application Insights como base</span><span class="sxs-lookup"><span data-stu-id="46f2e-157">Rely on Application Insights "Light-Up" features</span></span>
<span data-ttu-id="46f2e-158">Es importante configurar sin esfuerzo la instrumentación de rendimiento de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="46f2e-158">Effortless setup of application performance instrumentation is important.</span></span> <span data-ttu-id="46f2e-159">Ahora puede basarse en las nuevas características "light-up" de [Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) disponibles en las herramientas de Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="46f2e-159">You can now rely on the new [Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) "light-up" features available in the Visual Studio 2017 tooling.</span></span>

<span data-ttu-id="46f2e-160">Los proyectos de ASP.NET Core 1.1 creados en Visual Studio 2017 han agregado Application Insights de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="46f2e-160">ASP.NET Core 1.1 projects created in Visual Studio 2017 added Application Insights by default.</span></span> <span data-ttu-id="46f2e-161">Si no usa el SDK de Application Insights directamente, fuera de *Program.cs* y *Startup.cs*, siga estos pasos:</span><span class="sxs-lookup"><span data-stu-id="46f2e-161">If you're not using the Application Insights SDK directly, outside of *Program.cs* and *Startup.cs*, follow these steps:</span></span>

1. <span data-ttu-id="46f2e-162">Quite el siguiente nodo `<PackageReference />` del archivo *.csproj*:</span><span class="sxs-lookup"><span data-stu-id="46f2e-162">Remove the following `<PackageReference />` node from the *.csproj* file:</span></span>
    
    <span data-ttu-id="46f2e-163">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App.csproj?range=10)]</span><span class="sxs-lookup"><span data-stu-id="46f2e-163">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App.csproj?range=10)]</span></span>

2. <span data-ttu-id="46f2e-164">Quite la invocación del método de extensión `UseApplicationInsights` de *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="46f2e-164">Remove the `UseApplicationInsights` extension method invocation from *Program.cs*:</span></span>

    <span data-ttu-id="46f2e-165">[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/Program.cs?name=snippet_ProgramCsMain&highlight=8)]</span><span class="sxs-lookup"><span data-stu-id="46f2e-165">[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/Program.cs?name=snippet_ProgramCsMain&highlight=8)]</span></span>

3. <span data-ttu-id="46f2e-166">Quite la llamada API del lado cliente de Application Insights de *_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="46f2e-166">Remove the Application Insights client-side API call from *_Layout.cshtml*.</span></span> <span data-ttu-id="46f2e-167">Comprende las dos líneas de código siguientes:</span><span class="sxs-lookup"><span data-stu-id="46f2e-167">It comprises the following two lines of code:</span></span>

    <span data-ttu-id="46f2e-168">[!code-cshtml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/Views/Shared/_Layout.cshtml?range=1,19)]</span><span class="sxs-lookup"><span data-stu-id="46f2e-168">[!code-cshtml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/Views/Shared/_Layout.cshtml?range=1,19)]</span></span>

<span data-ttu-id="46f2e-169">Si usa el SDK de Application Insights directamente, siga haciéndolo.</span><span class="sxs-lookup"><span data-stu-id="46f2e-169">If you are using the Application Insights SDK directly, continue to do so.</span></span> <span data-ttu-id="46f2e-170">El [metapaquete](xref:fundamentals/metapackage) 2.0 incluye la versión más reciente de Application Insights, por lo que si se hace referencia a una versión anterior, aparece un error de degradación de paquete.</span><span class="sxs-lookup"><span data-stu-id="46f2e-170">The 2.0 [metapackage](xref:fundamentals/metapackage) includes the latest version of Application Insights, so a package downgrade error appears if you're referencing an older version.</span></span>

<a name="auth-and-identity"></a>

## <a name="adopt-authentication--identity-improvements"></a><span data-ttu-id="46f2e-171">Adopción de mejoras de autenticación o Identity</span><span class="sxs-lookup"><span data-stu-id="46f2e-171">Adopt Authentication / Identity Improvements</span></span>
<span data-ttu-id="46f2e-172">ASP.NET Core 2.0 tiene un nuevo modelo de autenticación y una serie de cambios significativos en ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="46f2e-172">ASP.NET Core 2.0 has a new authentication model and a number of significant changes to ASP.NET Core Identity.</span></span> <span data-ttu-id="46f2e-173">Si ha creado el proyecto con la opción Cuentas de usuario individuales habilitada o si ha agregado manualmente la autenticación o Identity, vea [Migrating Authentication and Identity to ASP.NET Core 2.0](xref:migration/1x-to-2x/identity-2x) (Migración de la autenticación e Identity a ASP.NET Core 2.0).</span><span class="sxs-lookup"><span data-stu-id="46f2e-173">If you created your project with Individual User Accounts enabled, or if you have manually added authentication or Identity, see [Migrating Authentication and Identity to ASP.NET Core 2.0](xref:migration/1x-to-2x/identity-2x).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="46f2e-174">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="46f2e-174">Additional Resources</span></span>
- [<span data-ttu-id="46f2e-175">Cambios importantes en ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="46f2e-175">Breaking Changes in ASP.NET Core 2.0</span></span>](https://github.com/aspnet/announcements/issues?page=1&q=is%3Aissue+is%3Aopen+label%3A2.0.0+label%3A%22Breaking+change%22&utf8=%E2%9C%93)
