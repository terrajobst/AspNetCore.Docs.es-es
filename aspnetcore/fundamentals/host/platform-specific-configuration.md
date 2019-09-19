---
title: Uso de ensamblados de inicio de hospedaje en ASP.NET Core
author: guardrex
description: Sepa cómo mejorar una aplicación ASP.NET Core desde un ensamblado externo con una implementación de IHostingStartup.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 08/02/2019
uid: fundamentals/configuration/platform-specific-configuration
ms.openlocfilehash: f20f8c4e9b1089acca6689d14cdf84bef648d461
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2019
ms.locfileid: "71081754"
---
# <a name="use-hosting-startup-assemblies-in-aspnet-core"></a><span data-ttu-id="74e1a-103">Uso de ensamblados de inicio de hospedaje en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="74e1a-103">Use hosting startup assemblies in ASP.NET Core</span></span>

<span data-ttu-id="74e1a-104">Por [Luke Latham](https://github.com/guardrex) y [Pavel Krymets](https://github.com/pakrym)</span><span class="sxs-lookup"><span data-stu-id="74e1a-104">By [Luke Latham](https://github.com/guardrex) and [Pavel Krymets](https://github.com/pakrym)</span></span>

<span data-ttu-id="74e1a-105">Una implementación de [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) (inicio de hospedaje) permite agregar mejoras a una aplicación al iniciarla a partir de un ensamblado externo.</span><span class="sxs-lookup"><span data-stu-id="74e1a-105">An [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) (hosting startup) implementation adds enhancements to an app at startup from an external assembly.</span></span> <span data-ttu-id="74e1a-106">Por ejemplo, una biblioteca externa puede usar una implementación de inicio de hospedaje para suministrar más servicios o proveedores de configuración a una aplicación.</span><span class="sxs-lookup"><span data-stu-id="74e1a-106">For example, an external library can use a hosting startup implementation to provide additional configuration providers or services to an app.</span></span>

<span data-ttu-id="74e1a-107">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="74e1a-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="hostingstartup-attribute"></a><span data-ttu-id="74e1a-108">Atributo HostingStartup</span><span class="sxs-lookup"><span data-stu-id="74e1a-108">HostingStartup attribute</span></span>

<span data-ttu-id="74e1a-109">Un atributo [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) indica la presencia de un ensamblado de inicio de hospedaje para activar en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="74e1a-109">A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) attribute indicates the presence of a hosting startup assembly to activate at runtime.</span></span>

<span data-ttu-id="74e1a-110">El ensamblado de entrada o el ensamblado que contiene la clase `Startup` se analiza automáticamente para detectar el atributo `HostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="74e1a-110">The entry assembly or the assembly containing the `Startup` class is automatically scanned for the `HostingStartup` attribute.</span></span> <span data-ttu-id="74e1a-111">La lista de ensamblados para buscar atributos `HostingStartup` se carga en tiempo de ejecución desde la configuración de [WebHostDefaults.HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey).</span><span class="sxs-lookup"><span data-stu-id="74e1a-111">The list of assemblies to search for `HostingStartup` attributes is loaded at runtime from configuration in the [WebHostDefaults.HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey).</span></span> <span data-ttu-id="74e1a-112">La lista de ensamblados que se excluirán de la detección se carga desde [WebHostDefaults.HostingStartupExcludeAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupexcludeassemblieskey).</span><span class="sxs-lookup"><span data-stu-id="74e1a-112">The list of assemblies to exclude from discovery is loaded from the [WebHostDefaults.HostingStartupExcludeAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupexcludeassemblieskey).</span></span> <span data-ttu-id="74e1a-113">Para más información, consulte [Host web: Ensamblados de inicio de hospedaje](xref:fundamentals/host/web-host#hosting-startup-assemblies) y [Host web: ensamblados de exclusión de inicio de hospedaje](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies).</span><span class="sxs-lookup"><span data-stu-id="74e1a-113">For more information, see [Web Host: Hosting Startup Assemblies](xref:fundamentals/host/web-host#hosting-startup-assemblies) and [Web Host: Hosting Startup Exclude Assemblies](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies).</span></span>

<span data-ttu-id="74e1a-114">En el ejemplo siguiente, el espacio de nombres del ensamblado de inicio de hospedaje es `StartupEnhancement`.</span><span class="sxs-lookup"><span data-stu-id="74e1a-114">In the following example, the namespace of the hosting startup assembly is `StartupEnhancement`.</span></span> <span data-ttu-id="74e1a-115">La clase que contiene el código de inicio de hospedaje es `StartupEnhancementHostingStartup`:</span><span class="sxs-lookup"><span data-stu-id="74e1a-115">The class containing the hosting startup code is `StartupEnhancementHostingStartup`:</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet1)]

<span data-ttu-id="74e1a-116">Normalmente el atributo `HostingStartup` se encuentra en el archivo de clase de implementación `IHostingStartup` del ensamblado de inicio de hospedaje.</span><span class="sxs-lookup"><span data-stu-id="74e1a-116">The `HostingStartup` attribute is typically located in the hosting startup assembly's `IHostingStartup` implementation class file.</span></span>

## <a name="discover-loaded-hosting-startup-assemblies"></a><span data-ttu-id="74e1a-117">Detectar los ensamblados de inicio de hospedaje cargados</span><span class="sxs-lookup"><span data-stu-id="74e1a-117">Discover loaded hosting startup assemblies</span></span>

<span data-ttu-id="74e1a-118">Para detectar los ensamblados de inicio de hospedaje cargados, habilite el registro y consulte los registros de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="74e1a-118">To discover loaded hosting startup assemblies, enable logging and check the app's logs.</span></span> <span data-ttu-id="74e1a-119">En ellos se registran los errores que se producen al cargar ensamblados.</span><span class="sxs-lookup"><span data-stu-id="74e1a-119">Errors that occur when loading assemblies are logged.</span></span> <span data-ttu-id="74e1a-120">Los ensamblados de inicio de hospedaje se registran en el nivel de depuración y se registran todos los errores.</span><span class="sxs-lookup"><span data-stu-id="74e1a-120">Loaded hosting startup assemblies are logged at the Debug level, and all errors are logged.</span></span>

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a><span data-ttu-id="74e1a-121">Deshabilitar la carga automática de ensamblados de inicio de hospedaje</span><span class="sxs-lookup"><span data-stu-id="74e1a-121">Disable automatic loading of hosting startup assemblies</span></span>

<span data-ttu-id="74e1a-122">Para deshabilitar la carga automática de los ensamblados de inicio de hospedaje, use uno de los enfoques siguientes:</span><span class="sxs-lookup"><span data-stu-id="74e1a-122">To disable automatic loading of hosting startup assemblies, use one of the following approaches:</span></span>

* <span data-ttu-id="74e1a-123">Para evitar que se carguen todos los ensamblados de inicio de hospedaje, establezca uno de los procedimientos siguientes en `true` o `1`:</span><span class="sxs-lookup"><span data-stu-id="74e1a-123">To prevent all hosting startup assemblies from loading, set one of the following to `true` or `1`:</span></span>
  * <span data-ttu-id="74e1a-124">La opción de configuración de hospedaje para [Evitar el inicio de hospedaje](xref:fundamentals/host/web-host#prevent-hosting-startup).</span><span class="sxs-lookup"><span data-stu-id="74e1a-124">[Prevent Hosting Startup](xref:fundamentals/host/web-host#prevent-hosting-startup) host configuration setting.</span></span>
  * <span data-ttu-id="74e1a-125">La variable de entorno `ASPNETCORE_PREVENTHOSTINGSTARTUP`.</span><span class="sxs-lookup"><span data-stu-id="74e1a-125">`ASPNETCORE_PREVENTHOSTINGSTARTUP` environment variable.</span></span>
* <span data-ttu-id="74e1a-126">Para evitar la carga de ensamblados de inicio de hospedaje específicos, establezca una de las opciones siguientes en una cadena delimitada por punto y coma de ensamblados de inicio de hospedaje para excluir durante el inicio:</span><span class="sxs-lookup"><span data-stu-id="74e1a-126">To prevent specific hosting startup assemblies from loading, set one of the following to a semicolon-delimited string of hosting startup assemblies to exclude at startup:</span></span>
  * <span data-ttu-id="74e1a-127">La opción de configuración de host para [Ensamblados de exclusión de inicio de hospedaje](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies).</span><span class="sxs-lookup"><span data-stu-id="74e1a-127">[Hosting Startup Exclude Assemblies](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies) host configuration setting.</span></span>
  * <span data-ttu-id="74e1a-128">La variable de entorno `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`.</span><span class="sxs-lookup"><span data-stu-id="74e1a-128">`ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES` environment variable.</span></span>

<span data-ttu-id="74e1a-129">Si se establecen la opción de configuración de host y la variable de entorno, la configuración de host controla el comportamiento.</span><span class="sxs-lookup"><span data-stu-id="74e1a-129">If both the host configuration setting and the environment variable are set, the host setting controls the behavior.</span></span>

<span data-ttu-id="74e1a-130">Deshabilitar los ensamblados de inicio de hospedaje a través de la variable de entorno o de la configuración de host hace que el ensamblado se deshabilite globalmente y, asimismo, puede deshabilitar también otras características de una aplicación.</span><span class="sxs-lookup"><span data-stu-id="74e1a-130">Disabling hosting startup assemblies using the host setting or environment variable disables the assembly globally and may disable several characteristics of an app.</span></span>

## <a name="project"></a><span data-ttu-id="74e1a-131">Proyecto</span><span class="sxs-lookup"><span data-stu-id="74e1a-131">Project</span></span>

<span data-ttu-id="74e1a-132">Cree un inicio de hospedaje con cualquiera de los siguientes tipos de proyecto:</span><span class="sxs-lookup"><span data-stu-id="74e1a-132">Create a hosting startup with either of the following project types:</span></span>

* [<span data-ttu-id="74e1a-133">Biblioteca de clases</span><span class="sxs-lookup"><span data-stu-id="74e1a-133">Class library</span></span>](#class-library)
* [<span data-ttu-id="74e1a-134">Aplicación de consola sin un punto de entrada</span><span class="sxs-lookup"><span data-stu-id="74e1a-134">Console app without an entry point</span></span>](#console-app-without-an-entry-point)

### <a name="class-library"></a><span data-ttu-id="74e1a-135">Biblioteca de clases</span><span class="sxs-lookup"><span data-stu-id="74e1a-135">Class library</span></span>

<span data-ttu-id="74e1a-136">Una mejora de inicio de hospedaje se puede proporcionar en una biblioteca de clases.</span><span class="sxs-lookup"><span data-stu-id="74e1a-136">A hosting startup enhancement can be provided in a class library.</span></span> <span data-ttu-id="74e1a-137">La biblioteca contiene un atributo `HostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="74e1a-137">The library contains a `HostingStartup` attribute.</span></span>

<span data-ttu-id="74e1a-138">El [código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) incluye una aplicación Razor Pages, *HostingStartupApp* y una biblioteca de clases, *HostingStartupLibrary*.</span><span class="sxs-lookup"><span data-stu-id="74e1a-138">The [sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) includes a Razor Pages app, *HostingStartupApp*, and a class library, *HostingStartupLibrary*.</span></span> <span data-ttu-id="74e1a-139">La biblioteca de clases:</span><span class="sxs-lookup"><span data-stu-id="74e1a-139">The class library:</span></span>

* <span data-ttu-id="74e1a-140">Contiene una clase de inicio de hospedaje, `ServiceKeyInjection`, que implementa `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="74e1a-140">Contains a hosting startup class, `ServiceKeyInjection`, which implements `IHostingStartup`.</span></span> <span data-ttu-id="74e1a-141">`ServiceKeyInjection` agrega un par de cadenas de servicio a la configuración de la aplicación mediante el proveedor de configuración en memoria ([AddInMemoryCollection](/dotnet/api/microsoft.extensions.configuration.memoryconfigurationbuilderextensions.addinmemorycollection)).</span><span class="sxs-lookup"><span data-stu-id="74e1a-141">`ServiceKeyInjection` adds a pair of service strings to the app's configuration using the in-memory configuration provider ([AddInMemoryCollection](/dotnet/api/microsoft.extensions.configuration.memoryconfigurationbuilderextensions.addinmemorycollection)).</span></span>
* <span data-ttu-id="74e1a-142">Incluye un atributo `HostingStartup` que identifica espacio de nombres y la clase del inicio de hospedaje.</span><span class="sxs-lookup"><span data-stu-id="74e1a-142">Includes a `HostingStartup` attribute that identifies the hosting startup's namespace and class.</span></span>

<span data-ttu-id="74e1a-143">El método [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) de la clase `ServiceKeyInjection` usa un [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) para agregar mejoras a una aplicación.</span><span class="sxs-lookup"><span data-stu-id="74e1a-143">The `ServiceKeyInjection` class's [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) method uses an [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) to add enhancements to an app.</span></span>

<span data-ttu-id="74e1a-144">*HostingStartupLibrary/ServiceKeyInjection.cs*:</span><span class="sxs-lookup"><span data-stu-id="74e1a-144">*HostingStartupLibrary/ServiceKeyInjection.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupLibrary/ServiceKeyInjection.cs?name=snippet1)]

<span data-ttu-id="74e1a-145">La página de índice de la aplicación lee y procesa los valores de configuración para las dos claves establecidas por el ensamblado de inicio de hospedaje de la biblioteca de clases:</span><span class="sxs-lookup"><span data-stu-id="74e1a-145">The app's Index page reads and renders the configuration values for the two keys set by the class library's hosting startup assembly:</span></span>

<span data-ttu-id="74e1a-146">*HostingStartupApp/Pages/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="74e1a-146">*HostingStartupApp/Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=5-6,11-12)]

<span data-ttu-id="74e1a-147">El [código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) también incluye un proyecto de paquete NuGet que proporciona un inicio de hospedaje independiente, *HostingStartupPackage*.</span><span class="sxs-lookup"><span data-stu-id="74e1a-147">The [sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) also includes a NuGet package project that provides a separate hosting startup, *HostingStartupPackage*.</span></span> <span data-ttu-id="74e1a-148">El paquete tiene las mismas características que la biblioteca de clases que se ha descrito anteriormente.</span><span class="sxs-lookup"><span data-stu-id="74e1a-148">The package has the same characteristics of the class library described earlier.</span></span> <span data-ttu-id="74e1a-149">El paquete:</span><span class="sxs-lookup"><span data-stu-id="74e1a-149">The package:</span></span>

* <span data-ttu-id="74e1a-150">Contiene una clase de inicio de hospedaje, `ServiceKeyInjection`, que implementa `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="74e1a-150">Contains a hosting startup class, `ServiceKeyInjection`, which implements `IHostingStartup`.</span></span> <span data-ttu-id="74e1a-151">`ServiceKeyInjection` agrega un par de cadenas de servicio a la configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="74e1a-151">`ServiceKeyInjection` adds a pair of service strings to the app's configuration.</span></span>
* <span data-ttu-id="74e1a-152">Incluye un atributo `HostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="74e1a-152">Includes a `HostingStartup` attribute.</span></span>

<span data-ttu-id="74e1a-153">*HostingStartupPackage/ServiceKeyInjection.cs*:</span><span class="sxs-lookup"><span data-stu-id="74e1a-153">*HostingStartupPackage/ServiceKeyInjection.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupPackage/ServiceKeyInjection.cs?name=snippet1)]

<span data-ttu-id="74e1a-154">La página de índice de la aplicación lee y procesa los valores de configuración para las dos claves establecidas por el ensamblado de inicio de hospedaje del paquete:</span><span class="sxs-lookup"><span data-stu-id="74e1a-154">The app's Index page reads and renders the configuration values for the two keys set by the package's hosting startup assembly:</span></span>

<span data-ttu-id="74e1a-155">*HostingStartupApp/Pages/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="74e1a-155">*HostingStartupApp/Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=7-8,13-14)]

### <a name="console-app-without-an-entry-point"></a><span data-ttu-id="74e1a-156">Aplicación de consola sin un punto de entrada</span><span class="sxs-lookup"><span data-stu-id="74e1a-156">Console app without an entry point</span></span>

<span data-ttu-id="74e1a-157">*Este enfoque solo está disponible para las aplicaciones de .NET Core, no de .NET Framework.*</span><span class="sxs-lookup"><span data-stu-id="74e1a-157">*This approach is only available for .NET Core apps, not .NET Framework.*</span></span>

<span data-ttu-id="74e1a-158">Se puede proporcionar una mejora del inicio de hospedaje dinámico que no requiere una referencia de tiempo de compilación para la activación en una aplicación de consola sin un punto de entrada que contiene un atributo `HostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="74e1a-158">A dynamic hosting startup enhancement that doesn't require a compile-time reference for activation can be provided in a console app without an entry point that contains a `HostingStartup` attribute.</span></span> <span data-ttu-id="74e1a-159">Publicar la aplicación de consola genera un ensamblado de inicio de hospedaje que se puede consumir desde el almacén en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="74e1a-159">Publishing the console app produces a hosting startup assembly that can be consumed from the runtime store.</span></span>

<span data-ttu-id="74e1a-160">En este proceso se usa una aplicación de consola sin un punto de entrada porque:</span><span class="sxs-lookup"><span data-stu-id="74e1a-160">A console app without an entry point is used in this process because:</span></span>

* <span data-ttu-id="74e1a-161">Se requiere un archivo de dependencias para consumir el inicio de hospedaje en el ensamblado de inicio de hospedaje.</span><span class="sxs-lookup"><span data-stu-id="74e1a-161">A dependencies file is required to consume the hosting startup in the hosting startup assembly.</span></span> <span data-ttu-id="74e1a-162">Un archivo de dependencias es un recurso de aplicación ejecutable que se genera mediante la publicación de una aplicación, no una biblioteca.</span><span class="sxs-lookup"><span data-stu-id="74e1a-162">A dependencies file is a runnable app asset that's produced by publishing an app, not a library.</span></span>
* <span data-ttu-id="74e1a-163">Una biblioteca no se puede agregar directamente al [almacén de paquetes en tiempo de ejecución](/dotnet/core/deploying/runtime-store), lo que requiere un proyecto ejecutable que tenga como destino el tiempo de ejecución compartido.</span><span class="sxs-lookup"><span data-stu-id="74e1a-163">A library can't be added directly to the [runtime package store](/dotnet/core/deploying/runtime-store), which requires a runnable project that targets the shared runtime.</span></span>

<span data-ttu-id="74e1a-164">En la creación de un inicio de hospedaje dinámico:</span><span class="sxs-lookup"><span data-stu-id="74e1a-164">In the creation of a dynamic hosting startup:</span></span>

* <span data-ttu-id="74e1a-165">Se crea un ensamblado de inicio de hospedaje desde la aplicación de consola sin un punto de entrada que:</span><span class="sxs-lookup"><span data-stu-id="74e1a-165">A hosting startup assembly is created from the console app without an entry point that:</span></span>
  * <span data-ttu-id="74e1a-166">Incluye una clase que contiene la implementación de `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="74e1a-166">Includes a class that contains the `IHostingStartup` implementation.</span></span>
  * <span data-ttu-id="74e1a-167">Incluye un atributo [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) para identificar la clase de implementación `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="74e1a-167">Includes a [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) attribute to identify the `IHostingStartup` implementation class.</span></span>
* <span data-ttu-id="74e1a-168">La aplicación de consola se publica para obtener las dependencias del inicio de hospedaje.</span><span class="sxs-lookup"><span data-stu-id="74e1a-168">The console app is published to obtain the hosting startup's dependencies.</span></span> <span data-ttu-id="74e1a-169">Una consecuencia de la publicación de la aplicación de consola es que las dependencias no usadas se cortan en el archivo de dependencias.</span><span class="sxs-lookup"><span data-stu-id="74e1a-169">A consequence of publishing the console app is that unused dependencies are trimmed from the dependencies file.</span></span>
* <span data-ttu-id="74e1a-170">El archivo de dependencias se modifica para establecer la ubicación en tiempo de ejecución del ensamblado de inicio de hospedaje.</span><span class="sxs-lookup"><span data-stu-id="74e1a-170">The dependencies file is modified to set the runtime location of the hosting startup assembly.</span></span>
* <span data-ttu-id="74e1a-171">El ensamblado de inicio de hospedaje y su archivo de dependencias se colocan en el almacén de paquetes en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="74e1a-171">The hosting startup assembly and its dependencies file is placed into the runtime package store.</span></span> <span data-ttu-id="74e1a-172">Para detectar el ensamblado de inicio de hospedaje y su archivo de dependencias, se enumeran en un par de variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="74e1a-172">To discover the hosting startup assembly and its dependencies file, they're listed in a pair of environment variables.</span></span>

<span data-ttu-id="74e1a-173">La aplicación de consola hace referencia al paquete [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/):</span><span class="sxs-lookup"><span data-stu-id="74e1a-173">The console app references the [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) package:</span></span>

[!code-xml[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.csproj)]

<span data-ttu-id="74e1a-174">Un atributo [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) identifica una clase como una implementación de `IHostingStartup` para la carga y ejecución al compilar [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost).</span><span class="sxs-lookup"><span data-stu-id="74e1a-174">A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) attribute identifies a class as an implementation of `IHostingStartup` for loading and execution when building the [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost).</span></span> <span data-ttu-id="74e1a-175">En el siguiente ejemplo, el espacio de nombres es `StartupEnhancement` y la clase, `StartupEnhancementHostingStartup`:</span><span class="sxs-lookup"><span data-stu-id="74e1a-175">In the following example, the namespace is `StartupEnhancement`, and the class is `StartupEnhancementHostingStartup`:</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet1)]

<span data-ttu-id="74e1a-176">Una clase implementa `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="74e1a-176">A class implements `IHostingStartup`.</span></span> <span data-ttu-id="74e1a-177">El método [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) de la clase usa un [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) para agregar mejoras a una aplicación.</span><span class="sxs-lookup"><span data-stu-id="74e1a-177">The class's [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) method uses an [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) to add enhancements to an app.</span></span> <span data-ttu-id="74e1a-178">El tiempo de ejecución llama a `IHostingStartup.Configure` en el ensamblado de inicio del hospedaje antes de `Startup.Configure` en el código de usuario, lo que permite que el código de usuario sobrescriba cualquier configuración facilitada por el ensamblado de inicio del hospedaje.</span><span class="sxs-lookup"><span data-stu-id="74e1a-178">`IHostingStartup.Configure` in the hosting startup assembly is called by the runtime before `Startup.Configure` in user code, which allows user code to overwrite any configuration provided by the hosting startup assembly.</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet2&highlight=3,5)]

<span data-ttu-id="74e1a-179">Al crear un proyecto de `IHostingStartup`, el archivo de dependencias ( *.deps.json*) establece la ubicación de `runtime` del ensamblado en la carpeta *bin*:</span><span class="sxs-lookup"><span data-stu-id="74e1a-179">When building an `IHostingStartup` project, the dependencies file (*.deps.json*) sets the `runtime` location of the assembly to the *bin* folder:</span></span>

[!code-json[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement1.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="74e1a-180">Solo se muestra parte del archivo.</span><span class="sxs-lookup"><span data-stu-id="74e1a-180">Only part of the file is shown.</span></span> <span data-ttu-id="74e1a-181">El nombre del ensamblado en el ejemplo es `StartupEnhancement`.</span><span class="sxs-lookup"><span data-stu-id="74e1a-181">The assembly name in the example is `StartupEnhancement`.</span></span>

## <a name="configuration-provided-by-the-hosting-startup"></a><span data-ttu-id="74e1a-182">Configuración proporcionada por el inicio de hospedaje</span><span class="sxs-lookup"><span data-stu-id="74e1a-182">Configuration provided by the hosting startup</span></span>

<span data-ttu-id="74e1a-183">Existen dos enfoques para controlar la configuración según si quiere que la configuración del inicio de hospedaje tenga prioridad o si la configuración de la aplicación tenga prioridad:</span><span class="sxs-lookup"><span data-stu-id="74e1a-183">There are two approaches to handling configuration depending on whether you want the hosting startup's configuration to take precedence or the app's configuration to take precedence:</span></span>

1. <span data-ttu-id="74e1a-184">Proporcione la configuración a la aplicación utilizando <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> para cargar la configuración después de que se ejecuten los delegados <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="74e1a-184">Provide configuration to the app using <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> to load the configuration after the app's <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> delegates execute.</span></span> <span data-ttu-id="74e1a-185">La configuración del inicio de hospedaje tiene prioridad sobre la configuración de la aplicación con este enfoque.</span><span class="sxs-lookup"><span data-stu-id="74e1a-185">Hosting startup configuration takes priority over the app's configuration using this approach.</span></span>
1. <span data-ttu-id="74e1a-186">Proporcione la configuración a la aplicación utilizando <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> para cargar la configuración antes de que se ejecuten los delegados <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="74e1a-186">Provide configuration to the app using <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> to load the configuration before the app's <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> delegates execute.</span></span> <span data-ttu-id="74e1a-187">Los valores de configuración de la aplicación tienen prioridad a sobre los proporcionados por el inicio de hospedaje con este enfoque.</span><span class="sxs-lookup"><span data-stu-id="74e1a-187">The app's configuration values take priority over those provided by the hosting startup using this approach.</span></span>

```csharp
public class ConfigurationInjection : IHostingStartup
{
    public void Configure(IWebHostBuilder builder)
    {
        Dictionary<string, string> dict;

        builder.ConfigureAppConfiguration(config =>
        {
            dict = new Dictionary<string, string>
            {
                {"ConfigurationKey1", 
                    "From IHostingStartup: Higher priority than the app's configuration."},
            };

            config.AddInMemoryCollection(dict);
        });

        dict = new Dictionary<string, string>
        {
            {"ConfigurationKey2", 
                "From IHostingStartup: Lower priority than the app's configuration."},
        };

        var builtConfig = new ConfigurationBuilder()
            .AddInMemoryCollection(dict)
            .Build();

        builder.UseConfiguration(builtConfig);
    }
}
```

## <a name="specify-the-hosting-startup-assembly"></a><span data-ttu-id="74e1a-188">Especificar el ensamblado de inicio de hospedaje</span><span class="sxs-lookup"><span data-stu-id="74e1a-188">Specify the hosting startup assembly</span></span>

<span data-ttu-id="74e1a-189">En el caso de un inicio de hospedaje proporcionado por una biblioteca de clase o una aplicación de consola, especifique el nombre del ensamblado de inicio de hospedaje en la variable de entorno `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`.</span><span class="sxs-lookup"><span data-stu-id="74e1a-189">For either a class library- or console app-supplied hosting startup, specify the hosting startup assembly's name in the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span> <span data-ttu-id="74e1a-190">La variable de entorno es una lista de ensamblados delimitada por punto y coma.</span><span class="sxs-lookup"><span data-stu-id="74e1a-190">The environment variable is a semicolon-delimited list of assemblies.</span></span>

<span data-ttu-id="74e1a-191">Solo se examinan los ensamblados de inicio de hospedaje en busca del atributo `HostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="74e1a-191">Only hosting startup assemblies are scanned for the `HostingStartup` attribute.</span></span> <span data-ttu-id="74e1a-192">En la aplicación de ejemplo, *HostingStartupApp*, para descubrir los nuevos inicios de hospedaje descritos anteriormente, la variable de entorno se establece en el siguiente valor:</span><span class="sxs-lookup"><span data-stu-id="74e1a-192">For the sample app, *HostingStartupApp*, to discover the hosting startups described earlier, the environment variable is set to the following value:</span></span>

```
HostingStartupLibrary;HostingStartupPackage;StartupDiagnostics
```

<span data-ttu-id="74e1a-193">Un ensamblado de inicio de hospedaje también se puede establecer mediante la configuración de host [Ensamblados de inicio de hospedaje](xref:fundamentals/host/web-host#hosting-startup-assemblies).</span><span class="sxs-lookup"><span data-stu-id="74e1a-193">A hosting startup assembly can also be set using the [Hosting Startup Assemblies](xref:fundamentals/host/web-host#hosting-startup-assemblies) host configuration setting.</span></span>

<span data-ttu-id="74e1a-194">Cuando existen varios ensamblados de inicio del hospedaje, sus métodos [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) se ejecutan en el orden en el que aparecen los ensamblados.</span><span class="sxs-lookup"><span data-stu-id="74e1a-194">When multiple hosting startup assembles are present, their [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) methods are executed in the order that the assemblies are listed.</span></span>

## <a name="activation"></a><span data-ttu-id="74e1a-195">Activación</span><span class="sxs-lookup"><span data-stu-id="74e1a-195">Activation</span></span>

<span data-ttu-id="74e1a-196">Las opciones de activación del inicio de hospedaje son:</span><span class="sxs-lookup"><span data-stu-id="74e1a-196">Options for hosting startup activation are:</span></span>

* <span data-ttu-id="74e1a-197">[Almacén en tiempo de ejecución](#runtime-store) &ndash; la activación no requiere una referencia de tiempo de compilación para la activación.</span><span class="sxs-lookup"><span data-stu-id="74e1a-197">[Runtime store](#runtime-store) &ndash; Activation doesn't require a compile-time reference for activation.</span></span> <span data-ttu-id="74e1a-198">La aplicación de ejemplo coloca el ensamblado de inicio de hospedaje y los archivos de dependencias en una carpeta, *implementación*, para facilitar la implementación del inicio del hospedaje en un entorno de varios equipos.</span><span class="sxs-lookup"><span data-stu-id="74e1a-198">The sample app places the hosting startup assembly and dependencies files into a folder, *deployment*, to facilitate deployment of the hosting startup in a multimachine environment.</span></span> <span data-ttu-id="74e1a-199">La carpeta *implementación* también incluye un script de PowerShell que crea o modifica las variables de entorno en el sistema de implementación para habilitar el inicio de hospedaje.</span><span class="sxs-lookup"><span data-stu-id="74e1a-199">The *deployment* folder also includes a PowerShell script that creates or modifies environment variables on the deployment system to enable the hosting startup.</span></span>
* <span data-ttu-id="74e1a-200">Se requiere una referencia al tiempo de compilación para la activación</span><span class="sxs-lookup"><span data-stu-id="74e1a-200">Compile-time reference required for activation</span></span>
  * [<span data-ttu-id="74e1a-201">Paquete NuGet</span><span class="sxs-lookup"><span data-stu-id="74e1a-201">NuGet package</span></span>](#nuget-package)
  * [<span data-ttu-id="74e1a-202">Carpeta bin del proyecto</span><span class="sxs-lookup"><span data-stu-id="74e1a-202">Project bin folder</span></span>](#project-bin-folder)

### <a name="runtime-store"></a><span data-ttu-id="74e1a-203">Almacén en tiempo de ejecución</span><span class="sxs-lookup"><span data-stu-id="74e1a-203">Runtime store</span></span>

<span data-ttu-id="74e1a-204">La implementación de inicio de hospedaje se coloca en el [almacén en tiempo de ejecución](/dotnet/core/deploying/runtime-store).</span><span class="sxs-lookup"><span data-stu-id="74e1a-204">The hosting startup implementation is placed in the [runtime store](/dotnet/core/deploying/runtime-store).</span></span> <span data-ttu-id="74e1a-205">No es necesario que la aplicación mejorada haga ninguna referencia de tiempo de compilación al ensamblado.</span><span class="sxs-lookup"><span data-stu-id="74e1a-205">A compile-time reference to the assembly isn't required by the enhanced app.</span></span>

<span data-ttu-id="74e1a-206">Una vez compilado el inicio de hospedaje, se genera un almacén en tiempo de ejecución mediante el archivo del proyecto de manifiesto y el comando [dotnet store](/dotnet/core/tools/dotnet-store).</span><span class="sxs-lookup"><span data-stu-id="74e1a-206">After the hosting startup is built, a runtime store is generated using the manifest project file and the [dotnet store](/dotnet/core/tools/dotnet-store) command.</span></span>

```dotnetcli
dotnet store --manifest {MANIFEST FILE} --runtime {RUNTIME IDENTIFIER} --output {OUTPUT LOCATION} --skip-optimization
```

<span data-ttu-id="74e1a-207">En la aplicación de ejemplo (proyecto *RuntimeStore*), se usa el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="74e1a-207">In the sample app (*RuntimeStore* project) the following command is used:</span></span>

```dotnetcli
dotnet store --manifest store.manifest.csproj --runtime win7-x64 --output ./deployment/store --skip-optimization
```

<span data-ttu-id="74e1a-208">Para que el tiempo de ejecución detecte el almacén en tiempo de ejecución, la ubicación del almacén en tiempo de ejecución se agrega a la variable de entorno `DOTNET_SHARED_STORE`.</span><span class="sxs-lookup"><span data-stu-id="74e1a-208">For the runtime to discover the runtime store, the runtime store's location is added to the `DOTNET_SHARED_STORE` environment variable.</span></span>

<span data-ttu-id="74e1a-209">**Modificar y colocar el archivo de dependencias del inicio de hospedaje**</span><span class="sxs-lookup"><span data-stu-id="74e1a-209">**Modify and place the hosting startup's dependencies file**</span></span>

<span data-ttu-id="74e1a-210">Para activar la mejora sin una referencia de paquete a la mejora, especifique dependencias adicionales del tiempo de ejecución en `additionalDeps`.</span><span class="sxs-lookup"><span data-stu-id="74e1a-210">To activate the enhancement without a package reference to the enhancement, specify additional dependencies to the runtime with `additionalDeps`.</span></span> <span data-ttu-id="74e1a-211">`additionalDeps` permite:</span><span class="sxs-lookup"><span data-stu-id="74e1a-211">`additionalDeps` allows you to:</span></span>

* <span data-ttu-id="74e1a-212">Ampliar el gráfico de la biblioteca de la aplicación al proporcionar un conjunto de archivos *.deps.json* adicionales para combinarlos con el propio archivo *.deps.json* de la aplicación durante el inicio.</span><span class="sxs-lookup"><span data-stu-id="74e1a-212">Extend the app's library graph by providing a set of additional *.deps.json* files to merge with the app's own *.deps.json* file on startup.</span></span>
* <span data-ttu-id="74e1a-213">Permitir la detección y la carga del ensamblado de inicio de hospedaje.</span><span class="sxs-lookup"><span data-stu-id="74e1a-213">Make the hosting startup assembly discoverable and loadable.</span></span>

<span data-ttu-id="74e1a-214">El enfoque recomendado para generar el archivo de dependencias adicionales es:</span><span class="sxs-lookup"><span data-stu-id="74e1a-214">The recommended approach for generating the additional dependencies file is to:</span></span>

 1. <span data-ttu-id="74e1a-215">Ejecutar `dotnet publish` en el archivo de manifiesto del almacén en tiempo de ejecución al que se hace referencia en la sección anterior.</span><span class="sxs-lookup"><span data-stu-id="74e1a-215">Execute `dotnet publish` on the runtime store manifest file referenced in the previous section.</span></span>
 1. <span data-ttu-id="74e1a-216">Quitar la referencia del manifiesto de las bibliotecas y de la sección `runtime` del archivo *deps.json* resultante.</span><span class="sxs-lookup"><span data-stu-id="74e1a-216">Remove the manifest reference from libraries and the `runtime` section of the resulting *.deps.json* file.</span></span>

<span data-ttu-id="74e1a-217">En el proyecto de ejemplo, la propiedad `store.manifest/1.0.0` se quita de las secciones `targets` y `libraries`:</span><span class="sxs-lookup"><span data-stu-id="74e1a-217">In the example project, the `store.manifest/1.0.0` property is removed from the `targets` and `libraries` section:</span></span>

```json
{
  "runtimeTarget": {
    "name": ".NETCoreApp,Version=v2.1",
    "signature": "4ea77c7b75ad1895ae1ea65e6ba2399010514f99"
  },
  "compilationOptions": {},
  "targets": {
    ".NETCoreApp,Version=v2.1": {
      "store.manifest/1.0.0": {
        "dependencies": {
          "StartupDiagnostics": "1.0.0"
        },
        "runtime": {
          "store.manifest.dll": {}
        }
      },
      "StartupDiagnostics/1.0.0": {
        "runtime": {
          "lib/netcoreapp2.1/StartupDiagnostics.dll": {
            "assemblyVersion": "1.0.0.0",
            "fileVersion": "1.0.0.0"
          }
        }
      }
    }
  },
  "libraries": {
    "store.manifest/1.0.0": {
      "type": "project",
      "serviceable": false,
      "sha512": ""
    },
    "StartupDiagnostics/1.0.0": {
      "type": "package",
      "serviceable": true,
      "sha512": "sha512-oiQr60vBQW7+nBTmgKLSldj06WNLRTdhOZpAdEbCuapoZ+M2DJH2uQbRLvFT8EGAAv4TAKzNtcztpx5YOgBXQQ==",
      "path": "startupdiagnostics/1.0.0",
      "hashPath": "startupdiagnostics.1.0.0.nupkg.sha512"
    }
  }
}
```

<span data-ttu-id="74e1a-218">Coloque el archivo *.deps.json* en la siguiente ubicación:</span><span class="sxs-lookup"><span data-stu-id="74e1a-218">Place the *.deps.json* file into the following location:</span></span>

```
{ADDITIONAL DEPENDENCIES PATH}/shared/{SHARED FRAMEWORK NAME}/{SHARED FRAMEWORK VERSION}/{ENHANCEMENT ASSEMBLY NAME}.deps.json
```

* <span data-ttu-id="74e1a-219">`{ADDITIONAL DEPENDENCIES PATH}`: ubicación agregada a la variable de entorno `DOTNET_ADDITIONAL_DEPS`.</span><span class="sxs-lookup"><span data-stu-id="74e1a-219">`{ADDITIONAL DEPENDENCIES PATH}` &ndash; Location added to the `DOTNET_ADDITIONAL_DEPS` environment variable.</span></span>
* <span data-ttu-id="74e1a-220">`{SHARED FRAMEWORK NAME}`: marco compartido necesario para este archivo de dependencias adicionales.</span><span class="sxs-lookup"><span data-stu-id="74e1a-220">`{SHARED FRAMEWORK NAME}` &ndash; Shared framework required for this additional dependencies file.</span></span>
* <span data-ttu-id="74e1a-221">`{SHARED FRAMEWORK VERSION}`: versión mínima del marco compartido.</span><span class="sxs-lookup"><span data-stu-id="74e1a-221">`{SHARED FRAMEWORK VERSION}` &ndash; Minimum shared framework version.</span></span>
* <span data-ttu-id="74e1a-222">`{ENHANCEMENT ASSEMBLY NAME}`: nombre del ensamblado de la mejora.</span><span class="sxs-lookup"><span data-stu-id="74e1a-222">`{ENHANCEMENT ASSEMBLY NAME}` &ndash; The enhancement's assembly name.</span></span>

<span data-ttu-id="74e1a-223">En la aplicación de ejemplo (proyecto *RuntimeStore*), el archivo de dependencias adicionales se coloca en la siguiente ubicación:</span><span class="sxs-lookup"><span data-stu-id="74e1a-223">In the sample app (*RuntimeStore* project), the additional dependencies file is placed into the following location:</span></span>

```
additionalDeps/shared/Microsoft.AspNetCore.App/2.1.0/StartupDiagnostics.deps.json
```

<span data-ttu-id="74e1a-224">Para que el tiempo de ejecución detecte la ubicación del almacén en tiempo de ejecución, la ubicación del archivo de dependencias adicionales se agrega a la variable de entorno `DOTNET_ADDITIONAL_DEPS`.</span><span class="sxs-lookup"><span data-stu-id="74e1a-224">For runtime to discover the runtime store location, the additional dependencies file location is added to the `DOTNET_ADDITIONAL_DEPS` environment variable.</span></span>

<span data-ttu-id="74e1a-225">En la aplicación de ejemplo (proyecto *RuntimeStore*), la creación del almacén en tiempo de ejecución y la generación del archivo de dependencias adicionales se realizan con un script de [PowerShell](/powershell/scripting/powershell-scripting).</span><span class="sxs-lookup"><span data-stu-id="74e1a-225">In the sample app (*RuntimeStore* project), building the runtime store and generating the additional dependencies file is accomplished using a [PowerShell](/powershell/scripting/powershell-scripting) script.</span></span>

<span data-ttu-id="74e1a-226">Para obtener ejemplos de cómo establecer variables de entorno en distintos sistemas operativos, vea [Uso de varios entornos](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="74e1a-226">For examples of how to set environment variables for various operating systems, see [Use multiple environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="74e1a-227">**Implementación**</span><span class="sxs-lookup"><span data-stu-id="74e1a-227">**Deployment**</span></span>

<span data-ttu-id="74e1a-228">Para facilitar la implementación de un inicio de hospedaje en un entorno de varios equipos, la aplicación de ejemplo crea una carpeta de *implementación* en el resultado publicado que contiene:</span><span class="sxs-lookup"><span data-stu-id="74e1a-228">To facilitate the deployment of a hosting startup in a multimachine environment, the sample app creates a *deployment* folder in published output that contains:</span></span>

* <span data-ttu-id="74e1a-229">El almacén en tiempo de ejecución de inicio de hospedaje.</span><span class="sxs-lookup"><span data-stu-id="74e1a-229">The hosting startup runtime store.</span></span>
* <span data-ttu-id="74e1a-230">El archivo de dependencias del inicio de hospedaje.</span><span class="sxs-lookup"><span data-stu-id="74e1a-230">The hosting startup dependencies file.</span></span>
* <span data-ttu-id="74e1a-231">Un script de PowerShell que crea o modifica `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`, `DOTNET_SHARED_STORE` y `DOTNET_ADDITIONAL_DEPS` para admitir la activación del inicio del hospedaje.</span><span class="sxs-lookup"><span data-stu-id="74e1a-231">A PowerShell script that creates or modifies the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`, `DOTNET_SHARED_STORE`, and `DOTNET_ADDITIONAL_DEPS` to support the activation of the hosting startup.</span></span> <span data-ttu-id="74e1a-232">Ejecute el script desde un símbolo del sistema de PowerShell administrativo en el sistema de implementación.</span><span class="sxs-lookup"><span data-stu-id="74e1a-232">Run the script from an administrative PowerShell command prompt on the deployment system.</span></span>

### <a name="nuget-package"></a><span data-ttu-id="74e1a-233">Detección de</span><span class="sxs-lookup"><span data-stu-id="74e1a-233">NuGet package</span></span>

<span data-ttu-id="74e1a-234">Una mejora de inicio de hospedaje se puede proporcionar en un paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="74e1a-234">A hosting startup enhancement can be provided in a NuGet package.</span></span> <span data-ttu-id="74e1a-235">El paquete tiene un atributo `HostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="74e1a-235">The package has a `HostingStartup` attribute.</span></span> <span data-ttu-id="74e1a-236">Los tipos de inicio de hospedaje proporcionados por el paquete están disponibles en la aplicación mediante cualquiera de los métodos siguientes:</span><span class="sxs-lookup"><span data-stu-id="74e1a-236">The hosting startup types provided by the package are made available to the app using either of the following approaches:</span></span>

* <span data-ttu-id="74e1a-237">El archivo de proyecto de la aplicación mejorada hace una referencia al paquete para el inicio de hospedaje en el archivo de proyecto de la aplicación (una referencia de tiempo de compilación).</span><span class="sxs-lookup"><span data-stu-id="74e1a-237">The enhanced app's project file makes a package reference for the hosting startup in the app's project file (a compile-time reference).</span></span> <span data-ttu-id="74e1a-238">Con la referencia de tiempo de compilación realizada, el ensamblado de inicio de hospedaje y todas sus dependencias se incorporan en el archivo de dependencia de la aplicación ( *.deps.json*).</span><span class="sxs-lookup"><span data-stu-id="74e1a-238">With the compile-time reference in place, the hosting startup assembly and all of its dependencies are incorporated into the app's dependency file (*.deps.json*).</span></span> <span data-ttu-id="74e1a-239">Este enfoque se aplica a un paquete de ensamblado de inicio de hospedaje publicado en [nuget.org](https://www.nuget.org/).</span><span class="sxs-lookup"><span data-stu-id="74e1a-239">This approach applies to a hosting startup assembly package published to [nuget.org](https://www.nuget.org/).</span></span>
* <span data-ttu-id="74e1a-240">El archivo de dependencias del inicio de hospedaje está disponible para la aplicación mejorada como se describe en la sección [Almacén en tiempo de ejecución](#runtime-store) (sin una referencia de tiempo de compilación).</span><span class="sxs-lookup"><span data-stu-id="74e1a-240">The hosting startup's dependencies file is made available to the enhanced app as described in the [Runtime store](#runtime-store) section (without a compile-time reference).</span></span>

<span data-ttu-id="74e1a-241">Para obtener más información sobre los paquetes NuGet y el almacén en tiempo de ejecución, vea los temas siguientes:</span><span class="sxs-lookup"><span data-stu-id="74e1a-241">For more information on NuGet packages and the runtime store, see the following topics:</span></span>

* [<span data-ttu-id="74e1a-242">Cómo crear un paquete NuGet con herramientas multiplataforma</span><span class="sxs-lookup"><span data-stu-id="74e1a-242">How to Create a NuGet Package with Cross Platform Tools</span></span>](/dotnet/core/deploying/creating-nuget-packages)
* [<span data-ttu-id="74e1a-243">Publicar paquetes</span><span class="sxs-lookup"><span data-stu-id="74e1a-243">Publishing packages</span></span>](/nuget/create-packages/publish-a-package)
* <span data-ttu-id="74e1a-244">[Runtime package store](/dotnet/core/deploying/runtime-store) (Almacenamiento de paquetes en tiempo de ejecución)</span><span class="sxs-lookup"><span data-stu-id="74e1a-244">[Runtime package store](/dotnet/core/deploying/runtime-store)</span></span>

### <a name="project-bin-folder"></a><span data-ttu-id="74e1a-245">Carpeta bin del proyecto</span><span class="sxs-lookup"><span data-stu-id="74e1a-245">Project bin folder</span></span>

<span data-ttu-id="74e1a-246">Un ensamblado implementado por *bin* puede proporcionar una mejora del inicio de hospedaje en la aplicación mejorada.</span><span class="sxs-lookup"><span data-stu-id="74e1a-246">A hosting startup enhancement can be provided by a *bin*-deployed assembly in the enhanced app.</span></span> <span data-ttu-id="74e1a-247">Los tipos de inicio de hospedaje que proporciona el ensamblado están disponibles en la aplicación mediante uno de los métodos siguientes:</span><span class="sxs-lookup"><span data-stu-id="74e1a-247">The hosting startup types provided by the assembly are made available to the app using one of the following approaches:</span></span>

* <span data-ttu-id="74e1a-248">El archivo de proyecto de la aplicación mejorada hace referencia de ensamblado al inicio de hospedaje (una referencia de tiempo de compilación).</span><span class="sxs-lookup"><span data-stu-id="74e1a-248">The enhanced app's project file makes an assembly reference to the hosting startup (a compile-time reference).</span></span> <span data-ttu-id="74e1a-249">Con la referencia de tiempo de compilación realizada, el ensamblado de inicio de hospedaje y todas sus dependencias se incorporan en el archivo de dependencia de la aplicación ( *.deps.json*).</span><span class="sxs-lookup"><span data-stu-id="74e1a-249">With the compile-time reference in place, the hosting startup assembly and all of its dependencies are incorporated into the app's dependency file (*.deps.json*).</span></span> <span data-ttu-id="74e1a-250">Este método se aplica cuando el escenario de implementación llama para realizar una referencia en tiempo de compilación al ensamblado del inicio del hospedaje (archivo *.dll*) y mover el ensamblado a una de las siguientes ubicaciones:</span><span class="sxs-lookup"><span data-stu-id="74e1a-250">This approach applies when the deployment scenario calls for making a compile-time reference to the hosting startup's assembly (*.dll* file) and moving the assembly to either:</span></span>
  * <span data-ttu-id="74e1a-251">Proyecto utilizado.</span><span class="sxs-lookup"><span data-stu-id="74e1a-251">The consuming project.</span></span>
  * <span data-ttu-id="74e1a-252">Ubicación a la que puede acceder el proyecto utilizado.</span><span class="sxs-lookup"><span data-stu-id="74e1a-252">A location accessible by the consuming project.</span></span>
* <span data-ttu-id="74e1a-253">El archivo de dependencias del inicio de hospedaje está disponible para la aplicación mejorada como se describe en la sección [Almacén en tiempo de ejecución](#runtime-store) (sin una referencia de tiempo de compilación).</span><span class="sxs-lookup"><span data-stu-id="74e1a-253">The hosting startup's dependencies file is made available to the enhanced app as described in the [Runtime store](#runtime-store) section (without a compile-time reference).</span></span>
* <span data-ttu-id="74e1a-254">Si el destino es .NET Framework, el ensamblado se puede cargar en el contexto de carga predeterminado, lo que en .NET Framework significa que el ensamblado se encuentra en cualquiera de las siguientes ubicaciones:</span><span class="sxs-lookup"><span data-stu-id="74e1a-254">When targeting the .NET Framework, the assembly is loadable in the default load context, which on .NET Framework means that the assembly is located at either of the following locations:</span></span>
  * <span data-ttu-id="74e1a-255">Ruta de acceso base de la aplicación: carpeta *bin* en la que se encuentra el archivo ejecutable ( *.exe*) de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="74e1a-255">Application base path &ndash; The *bin* folder where the app's executable (*.exe*) is located.</span></span>
  * <span data-ttu-id="74e1a-256">Caché global de ensamblados (GAC): almacena los ensamblados que comparten varias aplicaciones .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="74e1a-256">Global Assembly Cache (GAC) &ndash; The GAC stores assemblies that several .NET Framework apps share.</span></span> <span data-ttu-id="74e1a-257">Para obtener más información, vea [Cómo: Instalar un ensamblado en la caché global de ensamblados](/dotnet/framework/app-domains/how-to-install-an-assembly-into-the-gac) en la documentación de .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="74e1a-257">For more information, see [How to: Install an assembly into the global assembly cache](/dotnet/framework/app-domains/how-to-install-an-assembly-into-the-gac) in the .NET Framework documentation.</span></span>

## <a name="sample-code"></a><span data-ttu-id="74e1a-258">Código de ejemplo</span><span class="sxs-lookup"><span data-stu-id="74e1a-258">Sample code</span></span>

<span data-ttu-id="74e1a-259">En el [código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([cómo descargar](xref:index#how-to-download-a-sample)) se muestran escenarios de implementación de inicio de hospedaje:</span><span class="sxs-lookup"><span data-stu-id="74e1a-259">The [sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([how to download](xref:index#how-to-download-a-sample)) demonstrates hosting startup implementation scenarios:</span></span>

* <span data-ttu-id="74e1a-260">Cada uno de los dos ensamblados de inicio de hospedaje (bibliotecas de clases) establece un par clave-valor de configuración en memoria:</span><span class="sxs-lookup"><span data-stu-id="74e1a-260">Two hosting startup assemblies (class libraries) set a pair of in-memory configuration key-value pairs each:</span></span>
  * <span data-ttu-id="74e1a-261">Paquete NuGet (*HostingStartupPackage*)</span><span class="sxs-lookup"><span data-stu-id="74e1a-261">NuGet package (*HostingStartupPackage*)</span></span>
  * <span data-ttu-id="74e1a-262">Biblioteca de clases (*HostingStartupLibrary*)</span><span class="sxs-lookup"><span data-stu-id="74e1a-262">Class library (*HostingStartupLibrary*)</span></span>
* <span data-ttu-id="74e1a-263">Se activa un inicio de hospedaje desde un ensamblado implementado por el almacén de tiempo de ejecución (*StartupDiagnostics*).</span><span class="sxs-lookup"><span data-stu-id="74e1a-263">A hosting startup is activated from a runtime store-deployed assembly (*StartupDiagnostics*).</span></span> <span data-ttu-id="74e1a-264">Este ensamblado agrega dos middleware a la aplicación (mientras se inicia) que proporcionan información de diagnóstico en:</span><span class="sxs-lookup"><span data-stu-id="74e1a-264">The assembly adds two middlewares to the app at startup that provide diagnostic information on:</span></span>
  * <span data-ttu-id="74e1a-265">Servicios registrados</span><span class="sxs-lookup"><span data-stu-id="74e1a-265">Registered services</span></span>
  * <span data-ttu-id="74e1a-266">Dirección (esquema, host, ruta de acceso base, ruta de acceso, cadena de consulta)</span><span class="sxs-lookup"><span data-stu-id="74e1a-266">Address (scheme, host, path base, path, query string)</span></span>
  * <span data-ttu-id="74e1a-267">Conexión (dirección IP remota, puerto remoto, dirección IP local, puerto local, certificado de cliente)</span><span class="sxs-lookup"><span data-stu-id="74e1a-267">Connection (remote IP, remote port, local IP, local port, client certificate)</span></span>
  * <span data-ttu-id="74e1a-268">Encabezados de solicitud</span><span class="sxs-lookup"><span data-stu-id="74e1a-268">Request headers</span></span>
  * <span data-ttu-id="74e1a-269">Variables de entorno</span><span class="sxs-lookup"><span data-stu-id="74e1a-269">Environment variables</span></span>

<span data-ttu-id="74e1a-270">Para ejecutar el ejemplo:</span><span class="sxs-lookup"><span data-stu-id="74e1a-270">To run the sample:</span></span>

<span data-ttu-id="74e1a-271">**Activación desde un paquete NuGet**</span><span class="sxs-lookup"><span data-stu-id="74e1a-271">**Activation from a NuGet package**</span></span>

1. <span data-ttu-id="74e1a-272">Compile el paquete *HostingStartupPackage* con el comando [dotnet pack](/dotnet/core/tools/dotnet-pack).</span><span class="sxs-lookup"><span data-stu-id="74e1a-272">Compile the *HostingStartupPackage* package with the [dotnet pack](/dotnet/core/tools/dotnet-pack) command.</span></span>
1. <span data-ttu-id="74e1a-273">Agregue el nombre de ensamblado del paquete de *HostingStartupPackage* a la variable de entorno `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`.</span><span class="sxs-lookup"><span data-stu-id="74e1a-273">Add the package's assembly name of the *HostingStartupPackage* to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
1. <span data-ttu-id="74e1a-274">Compile y ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="74e1a-274">Compile and run the app.</span></span> <span data-ttu-id="74e1a-275">Una referencia de paquete está presente en la aplicación mejorada (una referencia de tiempo de compilación).</span><span class="sxs-lookup"><span data-stu-id="74e1a-275">A package reference is present in the enhanced app (a compile-time reference).</span></span> <span data-ttu-id="74e1a-276">El parámetro `<PropertyGroup>` del archivo del proyecto de la aplicación especifica el resultado del proyecto de paquete ( *../HostingStartupPackage/bin/Debug*) como origen del paquete.</span><span class="sxs-lookup"><span data-stu-id="74e1a-276">A `<PropertyGroup>` in the app's project file specifies the package project's output (*../HostingStartupPackage/bin/Debug*) as a package source.</span></span> <span data-ttu-id="74e1a-277">Esto permite que la aplicación utilice el paquete sin cargar el paquete a [nuget.org](https://www.nuget.org/). Para obtener más información, vea las notas que encontrará en el archivo del proyecto de HostingStartupApp.</span><span class="sxs-lookup"><span data-stu-id="74e1a-277">This allows the app to use the package without uploading the package to [nuget.org](https://www.nuget.org/). For more information, see the notes in the HostingStartupApp's project file.</span></span>

   ```xml
   <PropertyGroup>
     <RestoreSources>$(RestoreSources);https://api.nuget.org/v3/index.json;../HostingStartupPackage/bin/Debug</RestoreSources>
   </PropertyGroup>
   ```

1. <span data-ttu-id="74e1a-278">Observe que los valores de la clave de configuración del servicio representados por la página de índice coinciden con los valores establecidos por método `ServiceKeyInjection.Configure` del paquete.</span><span class="sxs-lookup"><span data-stu-id="74e1a-278">Observe that the service configuration key values rendered by the Index page match the values set by the package's `ServiceKeyInjection.Configure` method.</span></span>

<span data-ttu-id="74e1a-279">Si realiza cambios en el proyecto *HostingStartupPackage* y lo vuelve a compilar, borre las cachés del paquete NuGet local para asegurarse de que *HostingStartupApp* recibe el paquete actualizado y no un paquete en desuso de la caché local.</span><span class="sxs-lookup"><span data-stu-id="74e1a-279">If you make changes to the *HostingStartupPackage* project and recompile it, clear the local NuGet package caches to ensure that the *HostingStartupApp* receives the updated package and not a stale package from the local cache.</span></span> <span data-ttu-id="74e1a-280">Para borrar las cachés de NuGet locales, ejecute el siguiente comando [dotnet nuget locals](/dotnet/core/tools/dotnet-nuget-locals):</span><span class="sxs-lookup"><span data-stu-id="74e1a-280">To clear the local NuGet caches, execute the following [dotnet nuget locals](/dotnet/core/tools/dotnet-nuget-locals) command:</span></span>

```dotnetcli
dotnet nuget locals all --clear
```

<span data-ttu-id="74e1a-281">**Activación desde una biblioteca de clases**</span><span class="sxs-lookup"><span data-stu-id="74e1a-281">**Activation from a class library**</span></span>

1. <span data-ttu-id="74e1a-282">Compile la biblioteca de clases *HostingStartupLibrary* con el comando [dotnet build](/dotnet/core/tools/dotnet-build).</span><span class="sxs-lookup"><span data-stu-id="74e1a-282">Compile the *HostingStartupLibrary* class library with the [dotnet build](/dotnet/core/tools/dotnet-build) command.</span></span>
1. <span data-ttu-id="74e1a-283">Agregue el nombre del ensamblado de la biblioteca de clases *HostingStartupLibrary* a la variable de entorno `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`.</span><span class="sxs-lookup"><span data-stu-id="74e1a-283">Add the class library's assembly name of *HostingStartupLibrary* to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
1. <span data-ttu-id="74e1a-284">Implemente con *bin*-el ensamblado de la biblioteca de clases en la aplicación copiando el archivo *HostingStartupLibrary.dll* desde el resultado compilado de la biblioteca de aplicaciones en la carpeta *bin/Debug* de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="74e1a-284">*bin*-deploy the class library's assembly to the app by copying the *HostingStartupLibrary.dll* file from the class library's compiled output to the app's *bin/Debug* folder.</span></span>
1. <span data-ttu-id="74e1a-285">Compile y ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="74e1a-285">Compile and run the app.</span></span> <span data-ttu-id="74e1a-286">Un parámetro `<ItemGroup>` del archivo del proyecto de la aplicación hace referencia al ensamblado de la biblioteca de clases ( *.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll*) (una referencia de tiempo de compilación).</span><span class="sxs-lookup"><span data-stu-id="74e1a-286">An `<ItemGroup>` in the app's project file references the class library's assembly (*.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll*) (a compile-time reference).</span></span> <span data-ttu-id="74e1a-287">Para obtener más información, vea las notas que encontrará en el archivo del proyecto de HostingStartupApp.</span><span class="sxs-lookup"><span data-stu-id="74e1a-287">For more information, see the notes in the HostingStartupApp's project file.</span></span>

   ```xml
   <ItemGroup>
     <Reference Include=".\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll">
       <HintPath>.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll</HintPath>
       <SpecificVersion>False</SpecificVersion>
     </Reference>
   </ItemGroup>
   ```

1. <span data-ttu-id="74e1a-288">Observe que los valores de la clave de configuración del servicio representados por la página de índice coinciden con los valores establecidos por el método `ServiceKeyInjection.Configure` de la biblioteca de clases.</span><span class="sxs-lookup"><span data-stu-id="74e1a-288">Observe that the service configuration key values rendered by the Index page match the values set by the class library's `ServiceKeyInjection.Configure` method.</span></span>

<span data-ttu-id="74e1a-289">**Activación desde un ensamblado implementado por el almacén de tiempo de ejecución**</span><span class="sxs-lookup"><span data-stu-id="74e1a-289">**Activation from a runtime store-deployed assembly**</span></span>

1. <span data-ttu-id="74e1a-290">El proyecto *StartupDiagnostics* usa [PowerShell](/powershell/scripting/powershell-scripting) para modificar el archivo *StartupDiagnostics.deps.json*.</span><span class="sxs-lookup"><span data-stu-id="74e1a-290">The *StartupDiagnostics* project uses [PowerShell](/powershell/scripting/powershell-scripting) to modify its *StartupDiagnostics.deps.json* file.</span></span> <span data-ttu-id="74e1a-291">PowerShell se instala de forma predeterminada en Windows a partir de Windows 7 SP1 y Windows Server 2008 R2 SP1.</span><span class="sxs-lookup"><span data-stu-id="74e1a-291">PowerShell is installed by default on Windows starting with Windows 7 SP1 and Windows Server 2008 R2 SP1.</span></span> <span data-ttu-id="74e1a-292">Para obtener PowerShell en otras plataformas, vea [Instalación de Windows PowerShell](/powershell/scripting/setup/installing-powershell#powershell-core).</span><span class="sxs-lookup"><span data-stu-id="74e1a-292">To obtain PowerShell on other platforms, see [Installing Windows PowerShell](/powershell/scripting/setup/installing-powershell#powershell-core).</span></span>
1. <span data-ttu-id="74e1a-293">Ejecute el script *build.ps1* en la carpeta *RuntimeStore*.</span><span class="sxs-lookup"><span data-stu-id="74e1a-293">Execute the *build.ps1* script in the *RuntimeStore* folder.</span></span> <span data-ttu-id="74e1a-294">El script:</span><span class="sxs-lookup"><span data-stu-id="74e1a-294">The script:</span></span>
   * <span data-ttu-id="74e1a-295">Genera el paquete `StartupDiagnostics`.</span><span class="sxs-lookup"><span data-stu-id="74e1a-295">Generates the `StartupDiagnostics` package.</span></span>
   * <span data-ttu-id="74e1a-296">Genera el almacén en tiempo de ejecución para `StartupDiagnostics` en la carpeta *store*.</span><span class="sxs-lookup"><span data-stu-id="74e1a-296">Generates the runtime store for `StartupDiagnostics` in the *store* folder.</span></span> <span data-ttu-id="74e1a-297">El comando `dotnet store` del script usa el [identificador de runtime (RID)](/dotnet/core/rid-catalog) `win7-x64` para un inicio de hospedaje implementado en Windows.</span><span class="sxs-lookup"><span data-stu-id="74e1a-297">The `dotnet store` command in the script uses the `win7-x64` [runtime identifier (RID)](/dotnet/core/rid-catalog) for a hosting startup deployed to Windows.</span></span> <span data-ttu-id="74e1a-298">Para proporcionar el inicio de hospedaje para otro tiempo de ejecución, sustituya el RID correcto en la línea 37 del script.</span><span class="sxs-lookup"><span data-stu-id="74e1a-298">When providing the hosting startup for a different runtime, substitute the correct RID on line 37 of the script.</span></span>
   * <span data-ttu-id="74e1a-299">Genera `additionalDeps` para `StartupDiagnostics` en la carpeta *additionalDeps/shared/Microsoft.AspNetCore.App/{Shared Framework Version}/* .</span><span class="sxs-lookup"><span data-stu-id="74e1a-299">Generates the `additionalDeps` for `StartupDiagnostics` in the *additionalDeps/shared/Microsoft.AspNetCore.App/{Shared Framework Version}/* folder.</span></span>
   * <span data-ttu-id="74e1a-300">Coloca el archivo *deploy.ps1* en la carpeta *deployment*.</span><span class="sxs-lookup"><span data-stu-id="74e1a-300">Places the *deploy.ps1* file in the *deployment* folder.</span></span>
1. <span data-ttu-id="74e1a-301">Ejecute el script *deploy.ps1* en la carpeta *deployment*.</span><span class="sxs-lookup"><span data-stu-id="74e1a-301">Run the *deploy.ps1* script in the *deployment* folder.</span></span> <span data-ttu-id="74e1a-302">El script anexa:</span><span class="sxs-lookup"><span data-stu-id="74e1a-302">The script appends:</span></span>
   * <span data-ttu-id="74e1a-303">`StartupDiagnostics` a la variable de entorno `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`.</span><span class="sxs-lookup"><span data-stu-id="74e1a-303">`StartupDiagnostics` to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
   * <span data-ttu-id="74e1a-304">La ruta de acceso de las dependencias del inicio de hospedaje a la variable de entorno `DOTNET_ADDITIONAL_DEPS`.</span><span class="sxs-lookup"><span data-stu-id="74e1a-304">The hosting startup dependencies path to the `DOTNET_ADDITIONAL_DEPS` environment variable.</span></span>
   * <span data-ttu-id="74e1a-305">La ruta de acceso del almacén en tiempo de ejecución a la variable de entorno `DOTNET_SHARED_STORE`.</span><span class="sxs-lookup"><span data-stu-id="74e1a-305">The runtime store path to the `DOTNET_SHARED_STORE` environment variable.</span></span>
1. <span data-ttu-id="74e1a-306">Ejecute la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="74e1a-306">Run the sample app.</span></span>
1. <span data-ttu-id="74e1a-307">Solicite al punto de conexión `/services` ver los servicios registrados de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="74e1a-307">Request the `/services` endpoint to see the app's registered services.</span></span> <span data-ttu-id="74e1a-308">Solicite al punto de conexión `/diag` ver la información de diagnóstico.</span><span class="sxs-lookup"><span data-stu-id="74e1a-308">Request the `/diag` endpoint to see the diagnostic information.</span></span>
