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
ms.openlocfilehash: 7a845cec23f662dd6fe48044b819099f2c20ecb3
ms.sourcegitcommit: f8f6b5934bd071a349f5bc1e389365c52b1c00fa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/14/2017
---
# <a name="migrating-from-aspnet-core-1x-to-aspnet-core-20"></a><span data-ttu-id="fbdc3-104">Migración de ASP.NET Core 1.x a ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="fbdc3-104">Migrating from ASP.NET Core 1.x to ASP.NET Core 2.0</span></span>

<span data-ttu-id="fbdc3-105">Por [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="fbdc3-105">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="fbdc3-106">En este artículo se le guía a lo largo del proceso de actualización de un proyecto existente de ASP.NET Core 1.x a ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="fbdc3-106">In this article, we'll walk you through updating an existing ASP.NET Core 1.x project to ASP.NET Core 2.0.</span></span> <span data-ttu-id="fbdc3-107">La migración de la aplicación a ASP.NET Core 2.0 permite aprovechar [muchas características nuevas y mejoras de rendimiento](https://docs.microsoft.com/aspnet/core/aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="fbdc3-107">Migrating your application to ASP.NET Core 2.0 enables you to take advantage of [many new features and performance improvements](https://docs.microsoft.com/aspnet/core/aspnetcore-2.0).</span></span> 

<span data-ttu-id="fbdc3-108">Las aplicaciones existentes de ASP.NET Core 1.x se basan en plantillas de proyecto específicas de la versión.</span><span class="sxs-lookup"><span data-stu-id="fbdc3-108">Existing ASP.NET Core 1.x applications are based off of version-specific project templates.</span></span> <span data-ttu-id="fbdc3-109">A medida que el marco de trabajo de ASP.NET Core evoluciona, también lo hacen las plantillas de proyecto y el código de inicio incluido en ellas.</span><span class="sxs-lookup"><span data-stu-id="fbdc3-109">As the ASP.NET Core framework evolves, so do the project templates and the starter code contained within them.</span></span> <span data-ttu-id="fbdc3-110">Además de actualizar el marco de trabajo de ASP.NET Core, debe actualizar el código de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="fbdc3-110">In addition to updating the ASP.NET Core framework, you need to update the code for your application.</span></span>

<a name="prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="fbdc3-111">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="fbdc3-111">Prerequisites</span></span>
<span data-ttu-id="fbdc3-112">Vea [Introducción a ASP.NET Core](xref:getting-started).</span><span class="sxs-lookup"><span data-stu-id="fbdc3-112">Please see [Getting Started with ASP.NET Core](xref:getting-started).</span></span>

<a name="tfm"></a>

## <a name="update-target-framework-moniker-tfm"></a><span data-ttu-id="fbdc3-113">Actualización del moniker de la plataforma de destino (TFM)</span><span class="sxs-lookup"><span data-stu-id="fbdc3-113">Update Target Framework Moniker (TFM)</span></span>
<span data-ttu-id="fbdc3-114">Los proyectos para .NET Core deben usar el [TFM](/dotnet/standard/frameworks#referring-to-frameworks) de una versión mayor o igual que .NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="fbdc3-114">Projects targeting .NET Core should use the [TFM](/dotnet/standard/frameworks#referring-to-frameworks) of a version greater than or equal to .NET Core 2.0.</span></span> <span data-ttu-id="fbdc3-115">Busque el nodo `<TargetFramework>` del archivo *.csproj* y reemplace su texto interno por `netcoreapp2.0`:</span><span class="sxs-lookup"><span data-stu-id="fbdc3-115">Search for the `<TargetFramework>` node in the *.csproj* file, and replace its inner text with `netcoreapp2.0`:</span></span>

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=3)]

<span data-ttu-id="fbdc3-116">Los proyectos para .NET Framework deben usar el TFM de una versión mayor o igual que .NET Framework 4.6.1.</span><span class="sxs-lookup"><span data-stu-id="fbdc3-116">Projects targeting .NET Framework should use the TFM of a version greater than or equal to .NET Framework 4.6.1.</span></span> <span data-ttu-id="fbdc3-117">Busque el nodo `<TargetFramework>` del archivo *.csproj* y reemplace su texto interno por `net461`:</span><span class="sxs-lookup"><span data-stu-id="fbdc3-117">Search for the `<TargetFramework>` node in the *.csproj* file, and replace its inner text with `net461`:</span></span>

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=4)]

> [!NOTE]
> <span data-ttu-id="fbdc3-118">.NET Core 2.0 ofrece un área expuesta mucho mayor que .NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="fbdc3-118">.NET Core 2.0 offers a much larger surface area than .NET Core 1.x.</span></span> <span data-ttu-id="fbdc3-119">Si el destino es .NET Framework solo porque faltan API en .NET Core 1.x, el uso de .NET Core 2.0 como destino es probable que dé resultado.</span><span class="sxs-lookup"><span data-stu-id="fbdc3-119">If you're targeting .NET Framework solely because of missing APIs in .NET Core 1.x, targeting .NET Core 2.0 is likely to work.</span></span>

<a name="global-json"></a>

## <a name="update-net-core-sdk-version-in-globaljson"></a><span data-ttu-id="fbdc3-120">Actualización de la versión del SDK de .NET Core en global.json</span><span class="sxs-lookup"><span data-stu-id="fbdc3-120">Update .NET Core SDK version in global.json</span></span>
<span data-ttu-id="fbdc3-121">Si la solución se basa en un archivo [*global.json*](https://docs.microsoft.com/dotnet/core/tools/global-json) para que el destino sea una versión específica del SDK de .NET Core, actualice su propiedad `version` para usar la versión 2.0 instalada en el equipo:</span><span class="sxs-lookup"><span data-stu-id="fbdc3-121">If your solution relies upon a [*global.json*](https://docs.microsoft.com/dotnet/core/tools/global-json) file to target a specific .NET Core SDK version, update its `version` property to use the 2.0 version installed on your machine:</span></span>

[!code-json[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/global.json?highlight=3)]

<a name="package-reference"></a>

## <a name="update-package-references"></a><span data-ttu-id="fbdc3-122">Actualización de las referencias del paquete</span><span class="sxs-lookup"><span data-stu-id="fbdc3-122">Update package references</span></span>
<span data-ttu-id="fbdc3-123">El archivo *.csproj* de un proyecto de 1.x enumera cada paquete NuGet usado por el proyecto.</span><span class="sxs-lookup"><span data-stu-id="fbdc3-123">The *.csproj* file in a 1.x project lists each NuGet package used by the project.</span></span>

<span data-ttu-id="fbdc3-124">En un proyecto de ASP.NET Core 2.0 para .NET Core 2.0, una sola referencia de [metapaquete](xref:fundamentals/metapackage) del archivo *.csproj* reemplaza a la colección de paquetes:</span><span class="sxs-lookup"><span data-stu-id="fbdc3-124">In an ASP.NET Core 2.0 project targeting .NET Core 2.0, a single [metapackage](xref:fundamentals/metapackage) reference in the *.csproj* file replaces the collection of packages:</span></span>

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=8-10)]

<span data-ttu-id="fbdc3-125">Todas las características de ASP.NET Core 2.0 y Entity Framework Core 2.0 están incluidas en el metapaquete.</span><span class="sxs-lookup"><span data-stu-id="fbdc3-125">All the features of ASP.NET Core 2.0 and Entity Framework Core 2.0 are included in the metapackage.</span></span>

<span data-ttu-id="fbdc3-126">Los proyectos de ASP.NET Core 2.0 para .NET Framework deben seguir haciendo referencia a paquetes NuGet individuales.</span><span class="sxs-lookup"><span data-stu-id="fbdc3-126">ASP.NET Core 2.0 projects targeting .NET Framework should continue to reference individual NuGet packages.</span></span> <span data-ttu-id="fbdc3-127">Actualización del atributo `Version` de cada nodo `<PackageReference />` a 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="fbdc3-127">Update the `Version` attribute of each `<PackageReference />` node to 2.0.0.</span></span>

<span data-ttu-id="fbdc3-128">Por ejemplo, esta es la lista de nodos `<PackageReference />` usados en un proyecto típico de ASP.NET Core 2.0 para .NET Framework:</span><span class="sxs-lookup"><span data-stu-id="fbdc3-128">For example, here's the list of `<PackageReference />` nodes used in a typical ASP.NET Core 2.0 project targeting .NET Framework:</span></span>

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=9-22)]

<a name="dot-net-cli-tool-reference"></a>

## <a name="update-net-core-cli-tools"></a><span data-ttu-id="fbdc3-129">Actualización de las herramientas de la CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="fbdc3-129">Update .NET Core CLI tools</span></span>
<span data-ttu-id="fbdc3-130">En el archivo *.csproj*, actualice el atributo `Version` de cada nodo `<DotNetCliToolReference />` a 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="fbdc3-130">In the *.csproj* file, update the `Version` attribute of each `<DotNetCliToolReference />` node to 2.0.0.</span></span>

<span data-ttu-id="fbdc3-131">Por ejemplo, esta es la lista de herramientas de la CLI usadas en un proyecto típico de ASP.NET Core 2.0 para .NET Core 2.0:</span><span class="sxs-lookup"><span data-stu-id="fbdc3-131">For example, here's the list of CLI tools used in a typical ASP.NET Core 2.0 project targeting .NET Core 2.0:</span></span>

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=12-16)]

<a name="package-target-fallback"></a>

## <a name="rename-package-target-fallback-property"></a><span data-ttu-id="fbdc3-132">Cambio de nombre de la propiedad Package Target Fallback</span><span class="sxs-lookup"><span data-stu-id="fbdc3-132">Rename Package Target Fallback property</span></span>
<span data-ttu-id="fbdc3-133">El archivo *.csproj* de un proyecto de 1.x usa un nodo `PackageTargetFallback` y una variable:</span><span class="sxs-lookup"><span data-stu-id="fbdc3-133">The *.csproj* file of a 1.x project used a `PackageTargetFallback` node and variable:</span></span>

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App.csproj?range=5)]

<span data-ttu-id="fbdc3-134">Cambie el nombre del nodo y la variable a `AssetTargetFallback`:</span><span class="sxs-lookup"><span data-stu-id="fbdc3-134">Rename both the node and variable to `AssetTargetFallback`:</span></span>

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=4)]

<a name="program-cs"></a>

## <a name="update-main-method-in-programcs"></a><span data-ttu-id="fbdc3-135">Actualización del método Main de Program.cs</span><span class="sxs-lookup"><span data-stu-id="fbdc3-135">Update Main method in Program.cs</span></span>
<span data-ttu-id="fbdc3-136">En los proyectos de 1.x, el método `Main` de *Program.cs* tenía este aspecto:</span><span class="sxs-lookup"><span data-stu-id="fbdc3-136">In 1.x projects, the `Main` method of *Program.cs* looked like this:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Program.cs?name=snippet_ProgramCs&highlight=8-19)]

<span data-ttu-id="fbdc3-137">En los proyectos de 2.0, el método `Main` de *Program.cs* se ha simplificado:</span><span class="sxs-lookup"><span data-stu-id="fbdc3-137">In 2.0 projects, the `Main` method of *Program.cs* has been simplified:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Program.cs?highlight=8-11)]

<span data-ttu-id="fbdc3-138">La adopción de este nuevo patrón 2.0 es muy recomendable y necesaria para que funcionen características de producto como las [migraciones de Entity Framework (EF) Core](xref:data/ef-mvc/migrations).</span><span class="sxs-lookup"><span data-stu-id="fbdc3-138">The adoption of this new 2.0 pattern is highly recommended and is required for product features like [Entity Framework (EF) Core Migrations](xref:data/ef-mvc/migrations) to work.</span></span> <span data-ttu-id="fbdc3-139">Por ejemplo, la ejecución de `Update-Database` desde la ventana Consola del Administrador de paquetes o de `dotnet ef database update` desde la línea de comandos (en proyectos convertidos a ASP.NET Core 2.0) genera el error siguiente:</span><span class="sxs-lookup"><span data-stu-id="fbdc3-139">For example, running `Update-Database` from the Package Manager Console window or `dotnet ef database update` from the command line (on projects converted to ASP.NET Core 2.0) generates the following error:</span></span>

```
Unable to create an object of type '<Context>'. Add an implementation of 'IDesignTimeDbContextFactory<Context>' to the project, or see https://go.microsoft.com/fwlink/?linkid=851728 for additional patterns supported at design time.
```

<a name="db-init-code"></a>

## <a name="move-database-initialization-code"></a><span data-ttu-id="fbdc3-140">Mover el código de inicialización de la base de datos</span><span class="sxs-lookup"><span data-stu-id="fbdc3-140">Move database initialization code</span></span>
<span data-ttu-id="fbdc3-141">En proyectos de 1.x que usen EF Core 1.x, un comando como `dotnet ef migrations add` hace lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="fbdc3-141">In 1.x projects using EF Core 1.x, a command such as `dotnet ef migrations add` does the following:</span></span>
1. <span data-ttu-id="fbdc3-142">Crea una instancia de `Startup`.</span><span class="sxs-lookup"><span data-stu-id="fbdc3-142">Instantiates a `Startup` instance</span></span>
2. <span data-ttu-id="fbdc3-143">Invoca el método `ConfigureServices` para registrar todos los servicios de la inserción de dependencias (como los tipos `DbContext`).</span><span class="sxs-lookup"><span data-stu-id="fbdc3-143">Invokes the `ConfigureServices` method to register all services with dependency injection (including `DbContext` types)</span></span>
3. <span data-ttu-id="fbdc3-144">Realiza las tareas necesarias.</span><span class="sxs-lookup"><span data-stu-id="fbdc3-144">Performs its requisite tasks</span></span>

<span data-ttu-id="fbdc3-145">En proyectos 2.0 que usen EF Core 2.0, se invoca `Program.BuildWebHost` para obtener los servicios de aplicación.</span><span class="sxs-lookup"><span data-stu-id="fbdc3-145">In 2.0 projects using EF Core 2.0, `Program.BuildWebHost` is invoked to obtain the application services.</span></span> <span data-ttu-id="fbdc3-146">A diferencia de 1.x, se invoca `Startup.Configure` como efecto secundario adicional.</span><span class="sxs-lookup"><span data-stu-id="fbdc3-146">Unlike 1.x, this has the additional side effect of invoking `Startup.Configure`.</span></span> <span data-ttu-id="fbdc3-147">Si la aplicación 1.x ha invocado el código de inicialización de la aplicación en el método `Configure`, pueden producirse errores inesperados.</span><span class="sxs-lookup"><span data-stu-id="fbdc3-147">If your 1.x app invoked database initialization code in its `Configure` method, unexpected problems can occur.</span></span> <span data-ttu-id="fbdc3-148">Por ejemplo, si todavía no existe la base de datos, el código de propagación se ejecuta antes que el comando EF Core Migrations.</span><span class="sxs-lookup"><span data-stu-id="fbdc3-148">For example, if the database doesn't yet exist, the seeding code runs before the EF Core Migrations command execution.</span></span> <span data-ttu-id="fbdc3-149">Este problema provocará un error en un comando `dotnet ef migrations list` si la base de datos no existe.</span><span class="sxs-lookup"><span data-stu-id="fbdc3-149">This problem causes a `dotnet ef migrations list` command to fail if the database doesn't yet exist.</span></span>

<span data-ttu-id="fbdc3-150">Tenga en cuenta el código de propagación 1.x siguiente en el método `Configure` de *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="fbdc3-150">Consider the following 1.x seed initialization code in the `Configure` method of *Startup.cs*:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Startup.cs?name=snippet_ConfigureSeedData&highlight=8)]

<span data-ttu-id="fbdc3-151">En los proyectos 2.0, mueva la llamada `SeedData.Initialize` al método `Main` de *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="fbdc3-151">In 2.0 projects, move the `SeedData.Initialize` call to the `Main` method of *Program.cs*:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Program2.cs?name=snippet_Main2Code&highlight=10)]

<span data-ttu-id="fbdc3-152">A partir de 2.0 se desaconseja cualquier acción en `BuildWebHost`, excepto la compilación y la configuración del host web.</span><span class="sxs-lookup"><span data-stu-id="fbdc3-152">As of 2.0, it's bad practice to do anything in `BuildWebHost` except build and configure the web host.</span></span> <span data-ttu-id="fbdc3-153">Todo lo relacionado con la ejecución de la aplicación deberá gestionarse fuera de `BuildWebHost` &mdash;, normalmente en el método `Main` de *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="fbdc3-153">Anything that is about running the application should be handled outside of `BuildWebHost` &mdash; typically in the `Main` method of *Program.cs*.</span></span>

<a name="view-compilation"></a>

## <a name="review-your-razor-view-compilation-setting"></a><span data-ttu-id="fbdc3-154">Revisión de la configuración de compilación de la vista Razor</span><span class="sxs-lookup"><span data-stu-id="fbdc3-154">Review your Razor View Compilation setting</span></span>
<span data-ttu-id="fbdc3-155">Un tiempo de inicio de aplicación más rápido y unos lotes publicados más pequeños son de la máxima importancia para el usuario.</span><span class="sxs-lookup"><span data-stu-id="fbdc3-155">Faster application startup time and smaller published bundles are of utmost importance to you.</span></span> <span data-ttu-id="fbdc3-156">Por ello, la [compilación de la vista Razor](xref:mvc/views/view-compilation) está habilitada de forma predeterminada en ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="fbdc3-156">For these reasons, [Razor view compilation](xref:mvc/views/view-compilation) is enabled by default in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="fbdc3-157">Ya no es necesario establecer la propiedad `MvcRazorCompileOnPublish` en true.</span><span class="sxs-lookup"><span data-stu-id="fbdc3-157">Setting the `MvcRazorCompileOnPublish` property to true is no longer required.</span></span> <span data-ttu-id="fbdc3-158">A menos que se esté deshabilitando la compilación de la vista, se puede quitar la propiedad del archivo *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="fbdc3-158">Unless you're disabling view compilation, the property may be removed from the *.csproj* file.</span></span>

<span data-ttu-id="fbdc3-159">Cuando el destino es .NET Framework, se debe hacer referencia de forma explícita al paquete NuGet [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation) en el archivo *.csproj*:</span><span class="sxs-lookup"><span data-stu-id="fbdc3-159">When targeting .NET Framework, you still need to explicitly reference the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation) NuGet package in your *.csproj* file:</span></span>

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=15)]

<a name="app-insights"></a>

## <a name="rely-on-application-insights-light-up-features"></a><span data-ttu-id="fbdc3-160">Características "Light-Up" de Application Insights como base</span><span class="sxs-lookup"><span data-stu-id="fbdc3-160">Rely on Application Insights "Light-Up" features</span></span>
<span data-ttu-id="fbdc3-161">Es importante configurar sin esfuerzo la instrumentación de rendimiento de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="fbdc3-161">Effortless setup of application performance instrumentation is important.</span></span> <span data-ttu-id="fbdc3-162">Ahora puede basarse en las nuevas características "light-up" de [Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) disponibles en las herramientas de Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="fbdc3-162">You can now rely on the new [Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) "light-up" features available in the Visual Studio 2017 tooling.</span></span>

<span data-ttu-id="fbdc3-163">Los proyectos de ASP.NET Core 1.1 creados en Visual Studio 2017 han agregado Application Insights de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="fbdc3-163">ASP.NET Core 1.1 projects created in Visual Studio 2017 added Application Insights by default.</span></span> <span data-ttu-id="fbdc3-164">Si no usa el SDK de Application Insights directamente, fuera de *Program.cs* y *Startup.cs*, siga estos pasos:</span><span class="sxs-lookup"><span data-stu-id="fbdc3-164">If you're not using the Application Insights SDK directly, outside of *Program.cs* and *Startup.cs*, follow these steps:</span></span>

1. <span data-ttu-id="fbdc3-165">Quite el siguiente nodo `<PackageReference />` del archivo *.csproj*:</span><span class="sxs-lookup"><span data-stu-id="fbdc3-165">Remove the following `<PackageReference />` node from the *.csproj* file:</span></span>
    
    [!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App.csproj?range=10)]

2. <span data-ttu-id="fbdc3-166">Quite la invocación del método de extensión `UseApplicationInsights` de *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="fbdc3-166">Remove the `UseApplicationInsights` extension method invocation from *Program.cs*:</span></span>

    [!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Program.cs?name=snippet_ProgramCsMain&highlight=8)]

3. <span data-ttu-id="fbdc3-167">Quite la llamada API del lado cliente de Application Insights de *_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="fbdc3-167">Remove the Application Insights client-side API call from *_Layout.cshtml*.</span></span> <span data-ttu-id="fbdc3-168">Comprende las dos líneas de código siguientes:</span><span class="sxs-lookup"><span data-stu-id="fbdc3-168">It comprises the following two lines of code:</span></span>

    [!code-cshtml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Shared/_Layout.cshtml?range=1,19)]

<span data-ttu-id="fbdc3-169">Si usa el SDK de Application Insights directamente, siga haciéndolo.</span><span class="sxs-lookup"><span data-stu-id="fbdc3-169">If you are using the Application Insights SDK directly, continue to do so.</span></span> <span data-ttu-id="fbdc3-170">El [metapaquete](xref:fundamentals/metapackage) 2.0 incluye la versión más reciente de Application Insights, por lo que si se hace referencia a una versión anterior, aparece un error de degradación de paquete.</span><span class="sxs-lookup"><span data-stu-id="fbdc3-170">The 2.0 [metapackage](xref:fundamentals/metapackage) includes the latest version of Application Insights, so a package downgrade error appears if you're referencing an older version.</span></span>

<a name="auth-and-identity"></a>

## <a name="adopt-authentication--identity-improvements"></a><span data-ttu-id="fbdc3-171">Adopción de mejoras de autenticación o Identity</span><span class="sxs-lookup"><span data-stu-id="fbdc3-171">Adopt Authentication / Identity Improvements</span></span>
<span data-ttu-id="fbdc3-172">ASP.NET Core 2.0 tiene un nuevo modelo de autenticación y una serie de cambios significativos en ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="fbdc3-172">ASP.NET Core 2.0 has a new authentication model and a number of significant changes to ASP.NET Core Identity.</span></span> <span data-ttu-id="fbdc3-173">Si ha creado el proyecto con la opción Cuentas de usuario individuales habilitada o si ha agregado manualmente la autenticación o Identity, vea [Migrating Authentication and Identity to ASP.NET Core 2.0](xref:migration/1x-to-2x/identity-2x) (Migración de la autenticación e Identity a ASP.NET Core 2.0).</span><span class="sxs-lookup"><span data-stu-id="fbdc3-173">If you created your project with Individual User Accounts enabled, or if you have manually added authentication or Identity, see [Migrating Authentication and Identity to ASP.NET Core 2.0](xref:migration/1x-to-2x/identity-2x).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fbdc3-174">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="fbdc3-174">Additional Resources</span></span>
- [<span data-ttu-id="fbdc3-175">Cambios importantes en ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="fbdc3-175">Breaking Changes in ASP.NET Core 2.0</span></span>](https://github.com/aspnet/announcements/issues?page=1&q=is%3Aissue+is%3Aopen+label%3A2.0.0+label%3A%22Breaking+change%22&utf8=%E2%9C%93)
