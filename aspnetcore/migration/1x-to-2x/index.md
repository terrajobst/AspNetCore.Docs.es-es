---
title: Migración de ASP.NET Core 1.x a 2.0
author: scottaddie
description: En este artículo se indican los requisitos previos y los pasos más comunes para migrar un proyecto de ASP.NET Core 1.x a ASP.NET Core 2.0.
ms.author: scaddie
ms.custom: mvc
ms.date: 10/24/2018
uid: migration/1x-to-2x/index
ms.openlocfilehash: f5bd2bc9862a7487658125e14837798886efad11
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/27/2019
ms.locfileid: "64882800"
---
# <a name="migrate-from-aspnet-core-1x-to-20"></a><span data-ttu-id="5b747-103">Migración de ASP.NET Core 1.x a 2.0</span><span class="sxs-lookup"><span data-stu-id="5b747-103">Migrate from ASP.NET Core 1.x to 2.0</span></span>

<span data-ttu-id="5b747-104">Por [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="5b747-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="5b747-105">En este artículo se le guía a lo largo del proceso de actualización de un proyecto existente de ASP.NET Core 1.x a ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="5b747-105">In this article, we walk you through updating an existing ASP.NET Core 1.x project to ASP.NET Core 2.0.</span></span> <span data-ttu-id="5b747-106">La migración de la aplicación a ASP.NET Core 2.0 permite aprovechar [muchas características nuevas y mejoras de rendimiento](xref:aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="5b747-106">Migrating your application to ASP.NET Core 2.0 enables you to take advantage of [many new features and performance improvements](xref:aspnetcore-2.0).</span></span>

<span data-ttu-id="5b747-107">Las aplicaciones existentes de ASP.NET Core 1.x se basan en plantillas de proyecto específicas de la versión.</span><span class="sxs-lookup"><span data-stu-id="5b747-107">Existing ASP.NET Core 1.x applications are based off of version-specific project templates.</span></span> <span data-ttu-id="5b747-108">A medida que el marco de trabajo de ASP.NET Core evoluciona, también lo hacen las plantillas de proyecto y el código de inicio incluido en ellas.</span><span class="sxs-lookup"><span data-stu-id="5b747-108">As the ASP.NET Core framework evolves, so do the project templates and the starter code contained within them.</span></span> <span data-ttu-id="5b747-109">Además de actualizar el marco de trabajo de ASP.NET Core, debe actualizar el código de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5b747-109">In addition to updating the ASP.NET Core framework, you need to update the code for your application.</span></span>

<a name="prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="5b747-110">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="5b747-110">Prerequisites</span></span>

<span data-ttu-id="5b747-111">Consulte [Introducción a ASP.NET Core](xref:getting-started).</span><span class="sxs-lookup"><span data-stu-id="5b747-111">See [Get Started with ASP.NET Core](xref:getting-started).</span></span>

<a name="tfm"></a>

## <a name="update-target-framework-moniker-tfm"></a><span data-ttu-id="5b747-112">Actualización del moniker de la plataforma de destino (TFM)</span><span class="sxs-lookup"><span data-stu-id="5b747-112">Update Target Framework Moniker (TFM)</span></span>

<span data-ttu-id="5b747-113">Los proyectos para .NET Core deben usar el [TFM](/dotnet/standard/frameworks#referring-to-frameworks) de una versión mayor o igual que .NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="5b747-113">Projects targeting .NET Core should use the [TFM](/dotnet/standard/frameworks#referring-to-frameworks) of a version greater than or equal to .NET Core 2.0.</span></span> <span data-ttu-id="5b747-114">Busque el nodo `<TargetFramework>` del archivo *.csproj* y reemplace su texto interno por `netcoreapp2.0`:</span><span class="sxs-lookup"><span data-stu-id="5b747-114">Search for the `<TargetFramework>` node in the *.csproj* file, and replace its inner text with `netcoreapp2.0`:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=3)]

<span data-ttu-id="5b747-115">Los proyectos para .NET Framework deben usar el TFM de una versión mayor o igual que .NET Framework 4.6.1.</span><span class="sxs-lookup"><span data-stu-id="5b747-115">Projects targeting .NET Framework should use the TFM of a version greater than or equal to .NET Framework 4.6.1.</span></span> <span data-ttu-id="5b747-116">Busque el nodo `<TargetFramework>` del archivo *.csproj* y reemplace su texto interno por `net461`:</span><span class="sxs-lookup"><span data-stu-id="5b747-116">Search for the `<TargetFramework>` node in the *.csproj* file, and replace its inner text with `net461`:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=4)]

> [!NOTE]
> <span data-ttu-id="5b747-117">.NET Core 2.0 ofrece un área expuesta mucho mayor que .NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="5b747-117">.NET Core 2.0 offers a much larger surface area than .NET Core 1.x.</span></span> <span data-ttu-id="5b747-118">Si el destino es .NET Framework solo porque faltan API en .NET Core 1.x, el uso de .NET Core 2.0 como destino es probable que dé resultado.</span><span class="sxs-lookup"><span data-stu-id="5b747-118">If you're targeting .NET Framework solely because of missing APIs in .NET Core 1.x, targeting .NET Core 2.0 is likely to work.</span></span>

<a name="global-json"></a>

## <a name="update-net-core-sdk-version-in-globaljson"></a><span data-ttu-id="5b747-119">Actualización de la versión del SDK de .NET Core en global.json</span><span class="sxs-lookup"><span data-stu-id="5b747-119">Update .NET Core SDK version in global.json</span></span>

<span data-ttu-id="5b747-120">Si la solución se basa en un archivo [*global.json*](/dotnet/core/tools/global-json) para que el destino sea una versión específica del SDK de .NET Core, actualice su propiedad `version` para usar la versión 2.0 instalada en el equipo:</span><span class="sxs-lookup"><span data-stu-id="5b747-120">If your solution relies upon a [*global.json*](/dotnet/core/tools/global-json) file to target a specific .NET Core SDK version, update its `version` property to use the 2.0 version installed on your machine:</span></span>

[!code-json[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/global.json?highlight=3)]

<a name="package-reference"></a>

## <a name="update-package-references"></a><span data-ttu-id="5b747-121">Actualización de las referencias del paquete</span><span class="sxs-lookup"><span data-stu-id="5b747-121">Update package references</span></span>

<span data-ttu-id="5b747-122">El archivo *.csproj* de un proyecto de 1.x enumera cada paquete NuGet usado por el proyecto.</span><span class="sxs-lookup"><span data-stu-id="5b747-122">The *.csproj* file in a 1.x project lists each NuGet package used by the project.</span></span>

<span data-ttu-id="5b747-123">En un proyecto de ASP.NET Core 2.0 para .NET Core 2.0, una sola referencia de [metapaquete](xref:fundamentals/metapackage) del archivo *.csproj* reemplaza a la colección de paquetes:</span><span class="sxs-lookup"><span data-stu-id="5b747-123">In an ASP.NET Core 2.0 project targeting .NET Core 2.0, a single [metapackage](xref:fundamentals/metapackage) reference in the *.csproj* file replaces the collection of packages:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=8-10)]

<span data-ttu-id="5b747-124">Todas las características de ASP.NET Core 2.0 y Entity Framework Core 2.0 están incluidas en el metapaquete.</span><span class="sxs-lookup"><span data-stu-id="5b747-124">All the features of ASP.NET Core 2.0 and Entity Framework Core 2.0 are included in the metapackage.</span></span>

<span data-ttu-id="5b747-125">Los proyectos de ASP.NET Core 2.0 para .NET Framework deben seguir haciendo referencia a paquetes NuGet individuales.</span><span class="sxs-lookup"><span data-stu-id="5b747-125">ASP.NET Core 2.0 projects targeting .NET Framework should continue to reference individual NuGet packages.</span></span> <span data-ttu-id="5b747-126">Actualización del atributo `Version` de cada nodo `<PackageReference />` a 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="5b747-126">Update the `Version` attribute of each `<PackageReference />` node to 2.0.0.</span></span>

<span data-ttu-id="5b747-127">Por ejemplo, esta es la lista de nodos `<PackageReference />` usados en un proyecto típico de ASP.NET Core 2.0 para .NET Framework:</span><span class="sxs-lookup"><span data-stu-id="5b747-127">For example, here's the list of `<PackageReference />` nodes used in a typical ASP.NET Core 2.0 project targeting .NET Framework:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=9-22)]

<a name="dot-net-cli-tool-reference"></a>

## <a name="update-net-core-cli-tools"></a><span data-ttu-id="5b747-128">Actualización de las herramientas de la CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="5b747-128">Update .NET Core CLI tools</span></span>

<span data-ttu-id="5b747-129">En el archivo *.csproj*, actualice el atributo `Version` de cada nodo `<DotNetCliToolReference />` a 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="5b747-129">In the *.csproj* file, update the `Version` attribute of each `<DotNetCliToolReference />` node to 2.0.0.</span></span>

<span data-ttu-id="5b747-130">Por ejemplo, esta es la lista de herramientas de la CLI usadas en un proyecto típico de ASP.NET Core 2.0 para .NET Core 2.0:</span><span class="sxs-lookup"><span data-stu-id="5b747-130">For example, here's the list of CLI tools used in a typical ASP.NET Core 2.0 project targeting .NET Core 2.0:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=12-16)]

<a name="package-target-fallback"></a>

## <a name="rename-package-target-fallback-property"></a><span data-ttu-id="5b747-131">Cambio de nombre de la propiedad Package Target Fallback</span><span class="sxs-lookup"><span data-stu-id="5b747-131">Rename Package Target Fallback property</span></span>

<span data-ttu-id="5b747-132">El archivo *.csproj* de un proyecto de 1.x usa un nodo `PackageTargetFallback` y una variable:</span><span class="sxs-lookup"><span data-stu-id="5b747-132">The *.csproj* file of a 1.x project used a `PackageTargetFallback` node and variable:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App.csproj?range=5)]

<span data-ttu-id="5b747-133">Cambie el nombre del nodo y la variable a `AssetTargetFallback`:</span><span class="sxs-lookup"><span data-stu-id="5b747-133">Rename both the node and variable to `AssetTargetFallback`:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=4)]

<a name="program-cs"></a>

## <a name="update-main-method-in-programcs"></a><span data-ttu-id="5b747-134">Actualización del método Main de Program.cs</span><span class="sxs-lookup"><span data-stu-id="5b747-134">Update Main method in Program.cs</span></span>

<span data-ttu-id="5b747-135">En los proyectos de 1.x, el método `Main` de *Program.cs* tenía este aspecto:</span><span class="sxs-lookup"><span data-stu-id="5b747-135">In 1.x projects, the `Main` method of *Program.cs* looked like this:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Program.cs?name=snippet_ProgramCs&highlight=8-19)]

<span data-ttu-id="5b747-136">En los proyectos de 2.0, el método `Main` de *Program.cs* se ha simplificado:</span><span class="sxs-lookup"><span data-stu-id="5b747-136">In 2.0 projects, the `Main` method of *Program.cs* has been simplified:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Program.cs?highlight=8-11)]

<span data-ttu-id="5b747-137">La adopción de este nuevo patrón 2.0 es muy recomendable y necesaria para que funcionen características de producto como las [migraciones de Entity Framework (EF) Core](xref:data/ef-mvc/migrations).</span><span class="sxs-lookup"><span data-stu-id="5b747-137">The adoption of this new 2.0 pattern is highly recommended and is required for product features like [Entity Framework (EF) Core Migrations](xref:data/ef-mvc/migrations) to work.</span></span> <span data-ttu-id="5b747-138">Por ejemplo, la ejecución de `Update-Database` desde la ventana Consola del Administrador de paquetes o de `dotnet ef database update` desde la línea de comandos (en proyectos convertidos a ASP.NET Core 2.0) genera el error siguiente:</span><span class="sxs-lookup"><span data-stu-id="5b747-138">For example, running `Update-Database` from the Package Manager Console window or `dotnet ef database update` from the command line (on projects converted to ASP.NET Core 2.0) generates the following error:</span></span>

```
Unable to create an object of type '<Context>'. Add an implementation of 'IDesignTimeDbContextFactory<Context>' to the project, or see https://go.microsoft.com/fwlink/?linkid=851728 for additional patterns supported at design time.
```

<a name="add-modify-configuration"></a>

## <a name="add-configuration-providers"></a><span data-ttu-id="5b747-139">Incorporación de proveedores de configuración</span><span class="sxs-lookup"><span data-stu-id="5b747-139">Add configuration providers</span></span>

<span data-ttu-id="5b747-140">En los proyectos de 1.x, la incorporación de proveedores de configuración a una aplicación se lograba a través del constructor `Startup`.</span><span class="sxs-lookup"><span data-stu-id="5b747-140">In 1.x projects, adding configuration providers to an app was accomplished via the `Startup` constructor.</span></span> <span data-ttu-id="5b747-141">Para ello, era necesario crear una instancia de `ConfigurationBuilder`, cargar los proveedores aplicables (variables de entorno, configuración de la aplicación, etc.) e inicializar un miembro de `IConfigurationRoot`.</span><span class="sxs-lookup"><span data-stu-id="5b747-141">The steps involved creating an instance of `ConfigurationBuilder`, loading applicable providers (environment variables, app settings, etc.), and initializing a member of `IConfigurationRoot`.</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Startup.cs?name=snippet_1xStartup)]

<span data-ttu-id="5b747-142">El ejemplo anterior carga el miembro `Configuration` con opciones de configuración de *appsettings.json*, así como cualquier archivo *appsettings.\<EnvironmentName\>.json* que coincida con la propiedad `IHostingEnvironment.EnvironmentName`.</span><span class="sxs-lookup"><span data-stu-id="5b747-142">The preceding example loads the `Configuration` member with configuration settings from *appsettings.json* as well as any *appsettings.\<EnvironmentName\>.json* file matching the `IHostingEnvironment.EnvironmentName` property.</span></span> <span data-ttu-id="5b747-143">La ubicación de estos archivos está en la misma ruta de acceso que *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="5b747-143">The location of these files is at the same path as *Startup.cs*.</span></span>

<span data-ttu-id="5b747-144">En los proyectos de 2.0, el código de configuración reutilizable inherente a los proyectos de 1.x se ejecuta en segundo plano.</span><span class="sxs-lookup"><span data-stu-id="5b747-144">In 2.0 projects, the boilerplate configuration code inherent to 1.x projects runs behind-the-scenes.</span></span> <span data-ttu-id="5b747-145">Por ejemplo, las variables de entorno y la configuración de la aplicación se cargan durante el inicio.</span><span class="sxs-lookup"><span data-stu-id="5b747-145">For example, environment variables and app settings are loaded at startup.</span></span> <span data-ttu-id="5b747-146">El código de *Startup.cs* equivalente se reduce a la inicialización de `IConfiguration` con la instancia insertada:</span><span class="sxs-lookup"><span data-stu-id="5b747-146">The equivalent *Startup.cs* code is reduced to `IConfiguration` initialization with the injected instance:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/Startup.cs?name=snippet_2xStartup)]

<span data-ttu-id="5b747-147">Para quitar los proveedores predeterminados que `WebHostBuilder.CreateDefaultBuilder` agrega, invoque el método `Clear` en la propiedad `IConfigurationBuilder.Sources` dentro de `ConfigureAppConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="5b747-147">To remove the default providers added by `WebHostBuilder.CreateDefaultBuilder`, invoke the `Clear` method on the `IConfigurationBuilder.Sources` property inside of `ConfigureAppConfiguration`.</span></span> <span data-ttu-id="5b747-148">Para volver a agregar proveedores, use el método `ConfigureAppConfiguration` en *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="5b747-148">To add providers back, utilize the `ConfigureAppConfiguration` method in *Program.cs*:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/Program.cs?name=snippet_ProgramMainConfigProviders&highlight=9-14)]

<span data-ttu-id="5b747-149">La configuración que el método `CreateDefaultBuilder` utiliza en el fragmento de código anterior puede verse [aquí](https://github.com/aspnet/MetaPackages/blob/rel/2.0.0/src/Microsoft.AspNetCore/WebHost.cs#L152).</span><span class="sxs-lookup"><span data-stu-id="5b747-149">The configuration used by the `CreateDefaultBuilder` method in the preceding code snippet can be seen [here](https://github.com/aspnet/MetaPackages/blob/rel/2.0.0/src/Microsoft.AspNetCore/WebHost.cs#L152).</span></span>

<span data-ttu-id="5b747-150">Para más información, consulte [Configuración en ASP.NET Core](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="5b747-150">For more information, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index).</span></span>

<a name="db-init-code"></a>

## <a name="move-database-initialization-code"></a><span data-ttu-id="5b747-151">Mover el código de inicialización de la base de datos</span><span class="sxs-lookup"><span data-stu-id="5b747-151">Move database initialization code</span></span>

<span data-ttu-id="5b747-152">En proyectos de 1.x que usen EF Core 1.x, un comando como `dotnet ef migrations add` hace lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="5b747-152">In 1.x projects using EF Core 1.x, a command such as `dotnet ef migrations add` does the following:</span></span>

1. <span data-ttu-id="5b747-153">Crea una instancia de `Startup`.</span><span class="sxs-lookup"><span data-stu-id="5b747-153">Instantiates a `Startup` instance</span></span>
1. <span data-ttu-id="5b747-154">Invoca el método `ConfigureServices` para registrar todos los servicios de la inserción de dependencias (como los tipos `DbContext`).</span><span class="sxs-lookup"><span data-stu-id="5b747-154">Invokes the `ConfigureServices` method to register all services with dependency injection (including `DbContext` types)</span></span>
1. <span data-ttu-id="5b747-155">Realiza las tareas necesarias.</span><span class="sxs-lookup"><span data-stu-id="5b747-155">Performs its requisite tasks</span></span>

<span data-ttu-id="5b747-156">En proyectos 2.0 que usen EF Core 2.0, se invoca `Program.BuildWebHost` para obtener los servicios de aplicación.</span><span class="sxs-lookup"><span data-stu-id="5b747-156">In 2.0 projects using EF Core 2.0, `Program.BuildWebHost` is invoked to obtain the application services.</span></span> <span data-ttu-id="5b747-157">A diferencia de 1.x, se invoca `Startup.Configure` como efecto secundario adicional.</span><span class="sxs-lookup"><span data-stu-id="5b747-157">Unlike 1.x, this has the additional side effect of invoking `Startup.Configure`.</span></span> <span data-ttu-id="5b747-158">Si la aplicación 1.x ha invocado el código de inicialización de la aplicación en el método `Configure`, pueden producirse errores inesperados.</span><span class="sxs-lookup"><span data-stu-id="5b747-158">If your 1.x app invoked database initialization code in its `Configure` method, unexpected problems can occur.</span></span> <span data-ttu-id="5b747-159">Por ejemplo, si todavía no existe la base de datos, el código de propagación se ejecuta antes que el comando EF Core Migrations.</span><span class="sxs-lookup"><span data-stu-id="5b747-159">For example, if the database doesn't yet exist, the seeding code runs before the EF Core Migrations command execution.</span></span> <span data-ttu-id="5b747-160">Este problema provocará un error en un comando `dotnet ef migrations list` si la base de datos no existe.</span><span class="sxs-lookup"><span data-stu-id="5b747-160">This problem causes a `dotnet ef migrations list` command to fail if the database doesn't yet exist.</span></span>

<span data-ttu-id="5b747-161">Tenga en cuenta el código de propagación 1.x siguiente en el método `Configure` de *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="5b747-161">Consider the following 1.x seed initialization code in the `Configure` method of *Startup.cs*:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Startup.cs?name=snippet_ConfigureSeedData&highlight=8)]

<span data-ttu-id="5b747-162">En los proyectos 2.0, mueva la llamada `SeedData.Initialize` al método `Main` de *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="5b747-162">In 2.0 projects, move the `SeedData.Initialize` call to the `Main` method of *Program.cs*:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Program2.cs?name=snippet_Main2Code&highlight=10)]

<span data-ttu-id="5b747-163">A partir de 2.0 se desaconseja cualquier acción en `BuildWebHost`, excepto la compilación y la configuración del host web.</span><span class="sxs-lookup"><span data-stu-id="5b747-163">As of 2.0, it's bad practice to do anything in `BuildWebHost` except build and configure the web host.</span></span> <span data-ttu-id="5b747-164">Todo lo relacionado con la ejecución de la aplicación deberá gestionarse fuera de `BuildWebHost` &mdash;, normalmente en el método `Main` de *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="5b747-164">Anything that's about running the application should be handled outside of `BuildWebHost` &mdash; typically in the `Main` method of *Program.cs*.</span></span>

<a name="view-compilation"></a>

## <a name="review-razor-view-compilation-setting"></a><span data-ttu-id="5b747-165">Revisión de la configuración de compilación de la vista Razor</span><span class="sxs-lookup"><span data-stu-id="5b747-165">Review Razor view compilation setting</span></span>

<span data-ttu-id="5b747-166">Un tiempo de inicio de aplicación más rápido y unos lotes publicados más pequeños son de la máxima importancia para el usuario.</span><span class="sxs-lookup"><span data-stu-id="5b747-166">Faster application startup time and smaller published bundles are of utmost importance to you.</span></span> <span data-ttu-id="5b747-167">Por ello, la [compilación de la vista Razor](xref:mvc/views/view-compilation) está habilitada de forma predeterminada en ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="5b747-167">For these reasons, [Razor view compilation](xref:mvc/views/view-compilation) is enabled by default in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="5b747-168">Ya no es necesario establecer la propiedad `MvcRazorCompileOnPublish` en true.</span><span class="sxs-lookup"><span data-stu-id="5b747-168">Setting the `MvcRazorCompileOnPublish` property to true is no longer required.</span></span> <span data-ttu-id="5b747-169">A menos que se esté deshabilitando la compilación de la vista, se puede quitar la propiedad del archivo *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="5b747-169">Unless you're disabling view compilation, the property may be removed from the *.csproj* file.</span></span>

<span data-ttu-id="5b747-170">Cuando el destino es .NET Framework, se debe hacer referencia de forma explícita al paquete NuGet [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation) en el archivo *.csproj*:</span><span class="sxs-lookup"><span data-stu-id="5b747-170">When targeting .NET Framework, you still need to explicitly reference the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation) NuGet package in your *.csproj* file:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=15)]

<a name="app-insights"></a>

## <a name="rely-on-application-insights-light-up-features"></a><span data-ttu-id="5b747-171">Características "light-up" de Application Insights como base</span><span class="sxs-lookup"><span data-stu-id="5b747-171">Rely on Application Insights "light-up" features</span></span>

<span data-ttu-id="5b747-172">Es importante configurar sin esfuerzo la instrumentación de rendimiento de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5b747-172">Effortless setup of application performance instrumentation is important.</span></span> <span data-ttu-id="5b747-173">Ahora puede basarse en las nuevas características "light-up" de [Application Insights](/azure/application-insights/app-insights-overview) disponibles en las herramientas de Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="5b747-173">You can now rely on the new [Application Insights](/azure/application-insights/app-insights-overview) "light-up" features available in the Visual Studio 2017 tooling.</span></span>

<span data-ttu-id="5b747-174">Los proyectos de ASP.NET Core 1.1 creados en Visual Studio 2017 han agregado Application Insights de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="5b747-174">ASP.NET Core 1.1 projects created in Visual Studio 2017 added Application Insights by default.</span></span> <span data-ttu-id="5b747-175">Si no usa el SDK de Application Insights directamente, fuera de *Program.cs* y *Startup.cs*, siga estos pasos:</span><span class="sxs-lookup"><span data-stu-id="5b747-175">If you're not using the Application Insights SDK directly, outside of *Program.cs* and *Startup.cs*, follow these steps:</span></span>

1. <span data-ttu-id="5b747-176">Si el destino es .NET Core, elimine el siguiente nodo `<PackageReference />` del archivo *.csproj*:</span><span class="sxs-lookup"><span data-stu-id="5b747-176">If targeting .NET Core, remove the following `<PackageReference />` node from the *.csproj* file:</span></span>

    [!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App.csproj?range=10)]

2. <span data-ttu-id="5b747-177">Si el destino es .NET Core, elimine la invocación del método de extensión `UseApplicationInsights` de *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="5b747-177">If targeting .NET Core, remove the `UseApplicationInsights` extension method invocation from *Program.cs*:</span></span>

    [!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Program.cs?name=snippet_ProgramCsMain&highlight=8)]

3. <span data-ttu-id="5b747-178">Quite la llamada API del lado cliente de Application Insights de *_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="5b747-178">Remove the Application Insights client-side API call from *_Layout.cshtml*.</span></span> <span data-ttu-id="5b747-179">Comprende las dos líneas de código siguientes:</span><span class="sxs-lookup"><span data-stu-id="5b747-179">It comprises the following two lines of code:</span></span>

    [!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Shared/_Layout.cshtml?range=1,19&dedent=4)]

<span data-ttu-id="5b747-180">Si usa el SDK de Application Insights directamente, siga haciéndolo.</span><span class="sxs-lookup"><span data-stu-id="5b747-180">If you are using the Application Insights SDK directly, continue to do so.</span></span> <span data-ttu-id="5b747-181">El [metapaquete](xref:fundamentals/metapackage) 2.0 incluye la versión más reciente de Application Insights, por lo que si se hace referencia a una versión anterior, aparece un error de degradación de paquete.</span><span class="sxs-lookup"><span data-stu-id="5b747-181">The 2.0 [metapackage](xref:fundamentals/metapackage) includes the latest version of Application Insights, so a package downgrade error appears if you're referencing an older version.</span></span>

<a name="auth-and-identity"></a>

## <a name="adopt-authenticationidentity-improvements"></a><span data-ttu-id="5b747-182">Adopción de mejoras de autenticación o identidad</span><span class="sxs-lookup"><span data-stu-id="5b747-182">Adopt authentication/Identity improvements</span></span>

<span data-ttu-id="5b747-183">ASP.NET Core 2.0 tiene un nuevo modelo de autenticación y una serie de cambios significativos en ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="5b747-183">ASP.NET Core 2.0 has a new authentication model and a number of significant changes to ASP.NET Core Identity.</span></span> <span data-ttu-id="5b747-184">Si ha creado el proyecto con la opción Cuentas de usuario individuales habilitada o si ha agregado manualmente la autenticación o Identity, vea [Migración de la autenticación e Identity a ASP.NET Core 2.0](xref:migration/1x-to-2x/identity-2x).</span><span class="sxs-lookup"><span data-stu-id="5b747-184">If you created your project with Individual User Accounts enabled, or if you have manually added authentication or Identity, see [Migrate Authentication and Identity to ASP.NET Core 2.0](xref:migration/1x-to-2x/identity-2x).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5b747-185">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="5b747-185">Additional resources</span></span>

* [<span data-ttu-id="5b747-186">Cambios importantes en ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="5b747-186">Breaking Changes in ASP.NET Core 2.0</span></span>](https://github.com/aspnet/announcements/issues?page=1&q=is%3Aissue+is%3Aopen+label%3A2.0.0+label%3A%22Breaking+change%22&utf8=%E2%9C%93)
