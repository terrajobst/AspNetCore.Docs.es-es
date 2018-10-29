---
title: Mejorar una aplicación desde un ensamblado externo en ASP.NET Core con IHostingStartup
author: guardrex
description: Sepa cómo mejorar una aplicación ASP.NET Core desde un ensamblado externo con una implementación de IHostingStartup.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/13/2018
uid: fundamentals/configuration/platform-specific-configuration
ms.openlocfilehash: a06c2da04c1631f5811a535c891ca5190b0d8864
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207568"
---
# <a name="enhance-an-app-from-an-external-assembly-in-aspnet-core-with-ihostingstartup"></a><span data-ttu-id="ee6c4-103">Mejorar una aplicación desde un ensamblado externo en ASP.NET Core con IHostingStartup</span><span class="sxs-lookup"><span data-stu-id="ee6c4-103">Enhance an app from an external assembly in ASP.NET Core with IHostingStartup</span></span>

<span data-ttu-id="ee6c4-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="ee6c4-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="ee6c4-105">Una implementación de [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) (inicio de hospedaje) permite agregar mejoras a una aplicación al iniciarla a partir de un ensamblado externo.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-105">An [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) (hosting startup) implementation adds enhancements to an app at startup from an external assembly.</span></span> <span data-ttu-id="ee6c4-106">Por ejemplo, una biblioteca externa puede usar una implementación de inicio de hospedaje para suministrar más servicios o proveedores de configuración a una aplicación.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-106">For example, an external library can use a hosting startup implementation to provide additional configuration providers or services to an app.</span></span> <span data-ttu-id="ee6c4-107">`IHostingStartup` *está disponible en ASP.NET Core 2.0 o versiones posteriores.*</span><span class="sxs-lookup"><span data-stu-id="ee6c4-107">`IHostingStartup` *is available in ASP.NET Core 2.0 or later.*</span></span>

<span data-ttu-id="ee6c4-108">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ee6c4-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="hostingstartup-attribute"></a><span data-ttu-id="ee6c4-109">Atributo HostingStartup</span><span class="sxs-lookup"><span data-stu-id="ee6c4-109">HostingStartup attribute</span></span>

<span data-ttu-id="ee6c4-110">Un atributo [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) indica la presencia de un ensamblado de inicio de hospedaje para activar en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-110">A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) attribute indicates the presence of a hosting startup assembly to activate at runtime.</span></span>

<span data-ttu-id="ee6c4-111">El ensamblado de entrada o el ensamblado que contiene la clase `Startup` se analiza automáticamente para detectar el atributo `HostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-111">The entry assembly or the assembly containing the `Startup` class is automatically scanned for the `HostingStartup` attribute.</span></span> <span data-ttu-id="ee6c4-112">La lista de ensamblados para buscar atributos `HostingStartup` se carga en tiempo de ejecución desde la configuración de [WebHostDefaults.HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey).</span><span class="sxs-lookup"><span data-stu-id="ee6c4-112">The list of assemblies to search for `HostingStartup` attributes is loaded at runtime from configuration in the [WebHostDefaults.HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey).</span></span> <span data-ttu-id="ee6c4-113">La lista de ensamblados que se excluirán de la detección se carga desde [WebHostDefaults.HostingStartupExcludeAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupexcludeassemblieskey).</span><span class="sxs-lookup"><span data-stu-id="ee6c4-113">The list of assemblies to exclude from discovery is loaded from the [WebHostDefaults.HostingStartupExcludeAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupexcludeassemblieskey).</span></span> <span data-ttu-id="ee6c4-114">Para obtener más información, vea [Host web: ensamblados de inicio de hospedaje](xref:fundamentals/host/web-host#hosting-startup-assemblies) y [Web Host: Hosting Startup Exclude Assemblies](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies) (Host web: ensamblados de exclusión de inicio de hospedaje).</span><span class="sxs-lookup"><span data-stu-id="ee6c4-114">For more information, see [Web Host: Hosting Startup Assemblies](xref:fundamentals/host/web-host#hosting-startup-assemblies) and [Web Host: Hosting Startup Exclude Assemblies](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies).</span></span>

<span data-ttu-id="ee6c4-115">En el ejemplo siguiente, el espacio de nombres del ensamblado de inicio de hospedaje es `StartupEnhancement`.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-115">In the following example, the namespace of the hosting startup assembly is `StartupEnhancement`.</span></span> <span data-ttu-id="ee6c4-116">La clase que contiene el código de inicio de hospedaje es `StartupEnhancementHostingStartup`:</span><span class="sxs-lookup"><span data-stu-id="ee6c4-116">The class containing the hosting startup code is `StartupEnhancementHostingStartup`:</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet1)]

<span data-ttu-id="ee6c4-117">Normalmente el atributo `HostingStartup` se encuentra en el archivo de clase de implementación `IHostingStartup` del ensamblado de inicio de hospedaje.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-117">The `HostingStartup` attribute is typically located in the hosting startup assembly's `IHostingStartup` implementation class file.</span></span>

## <a name="discover-loaded-hosting-startup-assemblies"></a><span data-ttu-id="ee6c4-118">Detectar los ensamblados de inicio de hospedaje cargados</span><span class="sxs-lookup"><span data-stu-id="ee6c4-118">Discover loaded hosting startup assemblies</span></span>

<span data-ttu-id="ee6c4-119">Para detectar los ensamblados de inicio de hospedaje cargados, habilite el registro y consulte los registros de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-119">To discover loaded hosting startup assemblies, enable logging and check the app's logs.</span></span> <span data-ttu-id="ee6c4-120">En ellos se registran los errores que se producen al cargar ensamblados.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-120">Errors that occur when loading assemblies are logged.</span></span> <span data-ttu-id="ee6c4-121">Los ensamblados de inicio de hospedaje se registran en el nivel de depuración y se registran todos los errores.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-121">Loaded hosting startup assemblies are logged at the Debug level, and all errors are logged.</span></span>

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a><span data-ttu-id="ee6c4-122">Deshabilitar la carga automática de ensamblados de inicio de hospedaje</span><span class="sxs-lookup"><span data-stu-id="ee6c4-122">Disable automatic loading of hosting startup assemblies</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="ee6c4-123">Para deshabilitar la carga automática de los ensamblados de inicio de hospedaje, use uno de los enfoques siguientes:</span><span class="sxs-lookup"><span data-stu-id="ee6c4-123">To disable automatic loading of hosting startup assemblies, use one of the following approaches:</span></span>

* <span data-ttu-id="ee6c4-124">Para evitar que se carguen todos los ensamblados de inicio de hospedaje, establezca uno de los procedimientos siguientes en `true` o `1`:</span><span class="sxs-lookup"><span data-stu-id="ee6c4-124">To prevent all hosting startup assemblies from loading, set one of the following to `true` or `1`:</span></span>
  * <span data-ttu-id="ee6c4-125">La opción de configuración de hospedaje para [Evitar el inicio de hospedaje](xref:fundamentals/host/web-host#prevent-hosting-startup).</span><span class="sxs-lookup"><span data-stu-id="ee6c4-125">[Prevent Hosting Startup](xref:fundamentals/host/web-host#prevent-hosting-startup) host configuration setting.</span></span>
  * <span data-ttu-id="ee6c4-126">La variable de entorno `ASPNETCORE_PREVENTHOSTINGSTARTUP`.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-126">`ASPNETCORE_PREVENTHOSTINGSTARTUP` environment variable.</span></span>
* <span data-ttu-id="ee6c4-127">Para evitar la carga de ensamblados de inicio de hospedaje específicos, establezca una de las opciones siguientes en una cadena delimitada por punto y coma de ensamblados de inicio de hospedaje para excluir durante el inicio:</span><span class="sxs-lookup"><span data-stu-id="ee6c4-127">To prevent specific hosting startup assemblies from loading, set one of the following to a semicolon-delimited string of hosting startup assemblies to exclude at startup:</span></span>
  * <span data-ttu-id="ee6c4-128">La opción de configuración de host para [Ensamblados de exclusión de inicio de hospedaje](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies).</span><span class="sxs-lookup"><span data-stu-id="ee6c4-128">[Hosting Startup Exclude Assemblies](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies) host configuration setting.</span></span>
  * <span data-ttu-id="ee6c4-129">La variable de entorno `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-129">`ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES` environment variable.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="ee6c4-130">Para deshabilitar la carga automática de los ensamblados de inicio de hospedaje, establezca una de las opciones siguientes en `true` o `1`:</span><span class="sxs-lookup"><span data-stu-id="ee6c4-130">To disable automatic loading of hosting startup assemblies, set one of the following to `true` or `1`:</span></span>

* <span data-ttu-id="ee6c4-131">La opción de configuración de hospedaje para [Evitar el inicio de hospedaje](xref:fundamentals/host/web-host#prevent-hosting-startup).</span><span class="sxs-lookup"><span data-stu-id="ee6c4-131">[Prevent Hosting Startup](xref:fundamentals/host/web-host#prevent-hosting-startup) host configuration setting.</span></span>
* <span data-ttu-id="ee6c4-132">La variable de entorno `ASPNETCORE_PREVENTHOSTINGSTARTUP`.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-132">`ASPNETCORE_PREVENTHOSTINGSTARTUP` environment variable.</span></span>

::: moniker-end

<span data-ttu-id="ee6c4-133">Si se establecen la opción de configuración de host y la variable de entorno, la configuración de host controla el comportamiento.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-133">If both the host configuration setting and the environment variable are set, the host setting controls the behavior.</span></span>

<span data-ttu-id="ee6c4-134">Deshabilitar los ensamblados de inicio de hospedaje a través de la variable de entorno o de la configuración de host hace que el ensamblado se deshabilite globalmente y, asimismo, puede deshabilitar también otras características de una aplicación.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-134">Disabling hosting startup assemblies using the host setting or environment variable disables the assembly globally and may disable several characteristics of an app.</span></span>

## <a name="project"></a><span data-ttu-id="ee6c4-135">Proyecto</span><span class="sxs-lookup"><span data-stu-id="ee6c4-135">Project</span></span>

<span data-ttu-id="ee6c4-136">Cree un inicio de hospedaje con cualquiera de los siguientes tipos de proyecto:</span><span class="sxs-lookup"><span data-stu-id="ee6c4-136">Create a hosting startup with either of the following project types:</span></span>

* [<span data-ttu-id="ee6c4-137">Biblioteca de clases</span><span class="sxs-lookup"><span data-stu-id="ee6c4-137">Class library</span></span>](#class-library)
* [<span data-ttu-id="ee6c4-138">Aplicación de consola sin un punto de entrada</span><span class="sxs-lookup"><span data-stu-id="ee6c4-138">Console app without an entry point</span></span>](#console-app-without-an-entry-point)

### <a name="class-library"></a><span data-ttu-id="ee6c4-139">Biblioteca de clases</span><span class="sxs-lookup"><span data-stu-id="ee6c4-139">Class library</span></span>

<span data-ttu-id="ee6c4-140">Una mejora de inicio de hospedaje se puede proporcionar en una biblioteca de clases.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-140">A hosting startup enhancement can be provided in a class library.</span></span> <span data-ttu-id="ee6c4-141">La biblioteca contiene un atributo `HostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-141">The library contains a `HostingStartup` attribute.</span></span>

<span data-ttu-id="ee6c4-142">El [código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) incluye una aplicación Razor Pages, *HostingStartupApp* y una biblioteca de clases, *HostingStartupLibrary*.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-142">The [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) includes a Razor Pages app, *HostingStartupApp*, and a class library, *HostingStartupLibrary*.</span></span> <span data-ttu-id="ee6c4-143">La biblioteca de clases:</span><span class="sxs-lookup"><span data-stu-id="ee6c4-143">The class library:</span></span>

* <span data-ttu-id="ee6c4-144">Contiene una clase de inicio de hospedaje, `ServiceKeyInjection`, que implementa `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-144">Contains a hosting startup class, `ServiceKeyInjection`, which implements `IHostingStartup`.</span></span> <span data-ttu-id="ee6c4-145">`ServiceKeyInjection` agrega un par de cadenas de servicio a la configuración de la aplicación mediante el proveedor de configuración en memoria ([AddInMemoryCollection](/dotnet/api/microsoft.extensions.configuration.memoryconfigurationbuilderextensions.addinmemorycollection)).</span><span class="sxs-lookup"><span data-stu-id="ee6c4-145">`ServiceKeyInjection` adds a pair of service strings to the app's configuration using the in-memory configuration provider ([AddInMemoryCollection](/dotnet/api/microsoft.extensions.configuration.memoryconfigurationbuilderextensions.addinmemorycollection)).</span></span>
* <span data-ttu-id="ee6c4-146">Incluye un atributo `HostingStartup` que identifica espacio de nombres y la clase del inicio de hospedaje.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-146">Includes a `HostingStartup` attribute that identifies the hosting startup's namespace and class.</span></span>

<span data-ttu-id="ee6c4-147">El método [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) de la clase `ServiceKeyInjection` usa un [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) para agregar mejoras a una aplicación.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-147">The `ServiceKeyInjection` class's [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) method uses an [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) to add enhancements to an app.</span></span> <span data-ttu-id="ee6c4-148">El tiempo de ejecución llama a `IHostingStartup.Configure` en el ensamblado de inicio del hospedaje antes de `Startup.Configure` en el código de usuario, lo que permite que el código de usuario sobrescriba cualquier configuración facilitada por el ensamblado de inicio del hospedaje.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-148">`IHostingStartup.Configure` in the hosting startup assembly is called by the runtime before `Startup.Configure` in user code, which allows user code to overwrite any configuration provided by the hosting startup assembly.</span></span>

<span data-ttu-id="ee6c4-149">*HostingStartupLibrary/ServiceKeyInjection.cs*:</span><span class="sxs-lookup"><span data-stu-id="ee6c4-149">*HostingStartupLibrary/ServiceKeyInjection.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupLibrary/ServiceKeyInjection.cs?name=snippet1)]

<span data-ttu-id="ee6c4-150">La página de índice de la aplicación lee y procesa los valores de configuración para las dos claves establecidas por el ensamblado de inicio de hospedaje de la biblioteca de clases:</span><span class="sxs-lookup"><span data-stu-id="ee6c4-150">The app's Index page reads and renders the configuration values for the two keys set by the class library's hosting startup assembly:</span></span>

<span data-ttu-id="ee6c4-151">*HostingStartupApp/Pages/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="ee6c4-151">*HostingStartupApp/Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=5-6,11-12)]

<span data-ttu-id="ee6c4-152">El [código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) también incluye un proyecto de paquete NuGet que proporciona un inicio de hospedaje independiente, *HostingStartupPackage*.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-152">The [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) also includes a NuGet package project that provides a separate hosting startup, *HostingStartupPackage*.</span></span> <span data-ttu-id="ee6c4-153">El paquete tiene las mismas características que la biblioteca de clases que se ha descrito anteriormente.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-153">The package has the same characteristics of the class library described earlier.</span></span> <span data-ttu-id="ee6c4-154">El paquete:</span><span class="sxs-lookup"><span data-stu-id="ee6c4-154">The package:</span></span>

* <span data-ttu-id="ee6c4-155">Contiene una clase de inicio de hospedaje, `ServiceKeyInjection`, que implementa `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-155">Contains a hosting startup class, `ServiceKeyInjection`, which implements `IHostingStartup`.</span></span> <span data-ttu-id="ee6c4-156">`ServiceKeyInjection` agrega un par de cadenas de servicio a la configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-156">`ServiceKeyInjection` adds a pair of service strings to the app's configuration.</span></span>
* <span data-ttu-id="ee6c4-157">Incluye un atributo `HostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-157">Includes a `HostingStartup` attribute.</span></span>

<span data-ttu-id="ee6c4-158">*HostingStartupPackage/ServiceKeyInjection.cs*:</span><span class="sxs-lookup"><span data-stu-id="ee6c4-158">*HostingStartupPackage/ServiceKeyInjection.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupPackage/ServiceKeyInjection.cs?name=snippet1)]

<span data-ttu-id="ee6c4-159">La página de índice de la aplicación lee y procesa los valores de configuración para las dos claves establecidas por el ensamblado de inicio de hospedaje del paquete:</span><span class="sxs-lookup"><span data-stu-id="ee6c4-159">The app's Index page reads and renders the configuration values for the two keys set by the package's hosting startup assembly:</span></span>

<span data-ttu-id="ee6c4-160">*HostingStartupApp/Pages/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="ee6c4-160">*HostingStartupApp/Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=7-8,13-14)]

### <a name="console-app-without-an-entry-point"></a><span data-ttu-id="ee6c4-161">Aplicación de consola sin un punto de entrada</span><span class="sxs-lookup"><span data-stu-id="ee6c4-161">Console app without an entry point</span></span>

<span data-ttu-id="ee6c4-162">*Este enfoque solo está disponible para las aplicaciones de .NET Core, no de .NET Framework.*</span><span class="sxs-lookup"><span data-stu-id="ee6c4-162">*This approach is only available for .NET Core apps, not .NET Framework.*</span></span>

<span data-ttu-id="ee6c4-163">Se puede proporcionar una mejora del inicio de hospedaje dinámico que no requiere una referencia de tiempo de compilación para la activación en una aplicación de consola sin un punto de entrada.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-163">A dynamic hosting startup enhancement that doesn't require a compile-time reference for activation can be provided in a console app without an entry point.</span></span> <span data-ttu-id="ee6c4-164">La aplicación contiene un atributo `HostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-164">The app contains a `HostingStartup` attribute.</span></span> <span data-ttu-id="ee6c4-165">Para crear un inicio de hospedaje dinámico, siga estos pasos:</span><span class="sxs-lookup"><span data-stu-id="ee6c4-165">To create a dynamic hosting startup:</span></span>

1. <span data-ttu-id="ee6c4-166">La biblioteca de implementación se crea a partir de la clase que contiene la implementación `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-166">An implementation library is created from the class that contains the `IHostingStartup` implementation.</span></span> <span data-ttu-id="ee6c4-167">La biblioteca de implementación se trata como un paquete normal.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-167">The implementation library is treated as a normal package.</span></span>
1. <span data-ttu-id="ee6c4-168">Una aplicación de consola sin un punto de entrada hace referencia al paquete de la biblioteca de implementación.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-168">A console app without an entry point references the implementation library package.</span></span> <span data-ttu-id="ee6c4-169">Una aplicación de consola se usa porque:</span><span class="sxs-lookup"><span data-stu-id="ee6c4-169">A console app is used because:</span></span>
   * <span data-ttu-id="ee6c4-170">Un archivo de dependencias es un recurso de aplicación ejecutable, por lo que una biblioteca no puede proporcionar un archivo de dependencias.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-170">A dependencies file is a runnable app asset, so a library can't furnish a dependencies file.</span></span>
   * <span data-ttu-id="ee6c4-171">Una biblioteca no se puede agregar directamente al [almacén de paquetes en tiempo de ejecución](/dotnet/core/deploying/runtime-store), lo que requiere un proyecto ejecutable que tenga como destino el tiempo de ejecución compartido.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-171">A library can't be added directly to the [runtime package store](/dotnet/core/deploying/runtime-store), which requires a runnable project that targets the shared runtime.</span></span>
1. <span data-ttu-id="ee6c4-172">La aplicación de consola se publica para obtener las dependencias del inicio de hospedaje.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-172">The console app is published to obtain the hosting startup's dependencies.</span></span> <span data-ttu-id="ee6c4-173">Una consecuencia de la publicación de la aplicación de consola es que las dependencias no usadas se cortan en el archivo de dependencias.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-173">A consequence of publishing the console app is that unused dependencies are trimmed from the dependencies file.</span></span>
1. <span data-ttu-id="ee6c4-174">La aplicación y su archivo de dependencias se colocan en el almacén de paquetes en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-174">The app and its dependencies file is placed into the runtime package store.</span></span> <span data-ttu-id="ee6c4-175">Para detectar el ensamblado de inicio de hospedaje y su archivo de dependencias, se hace referencia a ellos en un par de variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-175">To discover the hosting startup assembly and its dependencies file, they're referenced in a pair of environment variables.</span></span>

<span data-ttu-id="ee6c4-176">La aplicación de consola hace referencia al paquete [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/):</span><span class="sxs-lookup"><span data-stu-id="ee6c4-176">The console app references the [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) package:</span></span>

[!code-xml[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.csproj)]

<span data-ttu-id="ee6c4-177">Un atributo [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) identifica una clase como una implementación de `IHostingStartup` para la carga y ejecución al compilar [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost).</span><span class="sxs-lookup"><span data-stu-id="ee6c4-177">A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) attribute identifies a class as an implementation of `IHostingStartup` for loading and execution when building the [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost).</span></span> <span data-ttu-id="ee6c4-178">En el siguiente ejemplo, el espacio de nombres es `StartupEnhancement` y la clase, `StartupEnhancementHostingStartup`:</span><span class="sxs-lookup"><span data-stu-id="ee6c4-178">In the following example, the namespace is `StartupEnhancement`, and the class is `StartupEnhancementHostingStartup`:</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet1)]

<span data-ttu-id="ee6c4-179">Una clase implementa `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-179">A class implements `IHostingStartup`.</span></span> <span data-ttu-id="ee6c4-180">El método [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) de la clase usa un [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) para agregar mejoras a una aplicación.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-180">The class's [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) method uses an [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) to add enhancements to an app.</span></span> <span data-ttu-id="ee6c4-181">El tiempo de ejecución llama a `IHostingStartup.Configure` en el ensamblado de inicio del hospedaje antes de `Startup.Configure` en el código de usuario, lo que permite que el código de usuario sobrescriba cualquier configuración facilitada por el ensamblado de inicio del hospedaje.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-181">`IHostingStartup.Configure` in the hosting startup assembly is called by the runtime before `Startup.Configure` in user code, which allows user code to overwrite any configuration provided by the hosting startup assembly.</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet2&highlight=3,5)]

<span data-ttu-id="ee6c4-182">Al crear un proyecto de `IHostingStartup`, el archivo de dependencias (*\*.deps.json*) establece la ubicación de `runtime` del ensamblado en la carpeta *bin*:</span><span class="sxs-lookup"><span data-stu-id="ee6c4-182">When building an `IHostingStartup` project, the dependencies file (*\*.deps.json*) sets the `runtime` location of the assembly to the *bin* folder:</span></span>

[!code-json[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement1.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="ee6c4-183">Solo se muestra parte del archivo.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-183">Only part of the file is shown.</span></span> <span data-ttu-id="ee6c4-184">El nombre del ensamblado en el ejemplo es `StartupEnhancement`.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-184">The assembly name in the example is `StartupEnhancement`.</span></span>

## <a name="specify-the-hosting-startup-assembly"></a><span data-ttu-id="ee6c4-185">Especificar el ensamblado de inicio de hospedaje</span><span class="sxs-lookup"><span data-stu-id="ee6c4-185">Specify the hosting startup assembly</span></span>

<span data-ttu-id="ee6c4-186">En el caso de un inicio de hospedaje proporcionado por una biblioteca de clase o una aplicación de consola, especifique el nombre del ensamblado de inicio de hospedaje en la variable de entorno `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-186">For either a class library- or console app-supplied hosting startup, specify the hosting startup assembly's name in the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span> <span data-ttu-id="ee6c4-187">La variable de entorno es una lista de ensamblados delimitada por punto y coma.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-187">The environment variable is a semicolon-delimited list of assemblies.</span></span>

<span data-ttu-id="ee6c4-188">Solo se examinan los ensamblados de inicio de hospedaje en busca del atributo `HostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-188">Only hosting startup assemblies are scanned for the `HostingStartup` attribute.</span></span> <span data-ttu-id="ee6c4-189">En la aplicación de ejemplo, *HostingStartupApp*, para descubrir los nuevos inicios de hospedaje descritos anteriormente, la variable de entorno se establece en el siguiente valor:</span><span class="sxs-lookup"><span data-stu-id="ee6c4-189">For the sample app, *HostingStartupApp*, to discover the hosting startups described earlier, the environment variable is set to the following value:</span></span>

```
HostingStartupLibrary;HostingStartupPackage;StartupDiagnostics
```

<span data-ttu-id="ee6c4-190">Un ensamblado de inicio de hospedaje también se puede establecer mediante la configuración de host [Ensamblados de inicio de hospedaje](xref:fundamentals/host/web-host#hosting-startup-assemblies).</span><span class="sxs-lookup"><span data-stu-id="ee6c4-190">A hosting startup assembly can also be set using the [Hosting Startup Assemblies](xref:fundamentals/host/web-host#hosting-startup-assemblies) host configuration setting.</span></span>

<span data-ttu-id="ee6c4-191">Cuando existen varios ensamblados de inicio del hospedaje, sus métodos [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) se ejecutan en el orden en el que aparecen los ensamblados.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-191">When multiple hosting startup assembles are present, their [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) methods are executed in the order that the assemblies are listed.</span></span>

## <a name="activation"></a><span data-ttu-id="ee6c4-192">Activación</span><span class="sxs-lookup"><span data-stu-id="ee6c4-192">Activation</span></span>

<span data-ttu-id="ee6c4-193">Las opciones de activación del inicio de hospedaje son:</span><span class="sxs-lookup"><span data-stu-id="ee6c4-193">Options for hosting startup activation are:</span></span>

* <span data-ttu-id="ee6c4-194">[Almacén en tiempo de ejecución](#runtime-store) &ndash; la activación no requiere una referencia de tiempo de compilación para la activación.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-194">[Runtime store](#runtime-store) &ndash; Activation doesn't require a compile-time reference for activation.</span></span> <span data-ttu-id="ee6c4-195">La aplicación de ejemplo coloca el ensamblado de inicio de hospedaje y los archivos de dependencias en una carpeta, *implementación*, para facilitar la implementación del inicio del hospedaje en un entorno de varios equipos.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-195">The sample app places the hosting startup assembly and dependencies files into a folder, *deployment*, to facilitate deployment of the hosting startup in a multimachine environment.</span></span> <span data-ttu-id="ee6c4-196">La carpeta *implementación* también incluye un script de PowerShell que crea o modifica las variables de entorno en el sistema de implementación para habilitar el inicio de hospedaje.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-196">The *deployment* folder also includes a PowerShell script that creates or modifies environment variables on the deployment system to enable the hosting startup.</span></span>
* <span data-ttu-id="ee6c4-197">Se requiere una referencia al tiempo de compilación para la activación</span><span class="sxs-lookup"><span data-stu-id="ee6c4-197">Compile-time reference required for activation</span></span>
  * [<span data-ttu-id="ee6c4-198">Paquete NuGet</span><span class="sxs-lookup"><span data-stu-id="ee6c4-198">NuGet package</span></span>](#nuget-package)
  * [<span data-ttu-id="ee6c4-199">Carpeta bin del proyecto</span><span class="sxs-lookup"><span data-stu-id="ee6c4-199">Project bin folder</span></span>](#project-bin-folder)

### <a name="runtime-store"></a><span data-ttu-id="ee6c4-200">Almacén en tiempo de ejecución</span><span class="sxs-lookup"><span data-stu-id="ee6c4-200">Runtime store</span></span>

<span data-ttu-id="ee6c4-201">La implementación de inicio de hospedaje se coloca en el [almacén en tiempo de ejecución](/dotnet/core/deploying/runtime-store).</span><span class="sxs-lookup"><span data-stu-id="ee6c4-201">The hosting startup implementation is placed in the [runtime store](/dotnet/core/deploying/runtime-store).</span></span> <span data-ttu-id="ee6c4-202">No es necesario que la aplicación mejorada haga ninguna referencia de tiempo de compilación al ensamblado.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-202">A compile-time reference to the assembly isn't required by the enhanced app.</span></span>

<span data-ttu-id="ee6c4-203">Una vez compilado el inicio de hospedaje, el archivo de proyecto de inicio de hospedaje actúa como el archivo de manifiesto para el comando [dotnet store](/dotnet/core/tools/dotnet-store).</span><span class="sxs-lookup"><span data-stu-id="ee6c4-203">After the hosting startup is built, the hosting startup's project file serves as the manifest file for the [dotnet store](/dotnet/core/tools/dotnet-store) command.</span></span>

```console
dotnet store --manifest <PROJECT_FILE> --runtime <RUNTIME_IDENTIFIER>
```

<span data-ttu-id="ee6c4-204">Este comando coloca el ensamblado de inicio de hospedaje y otras dependencias que no forman parte del marco compartido en el almacén de tiempo de ejecución del perfil de usuario en:</span><span class="sxs-lookup"><span data-stu-id="ee6c4-204">This command places the hosting startup assembly and other dependencies that aren't part of the shared framework in the user profile's runtime store at:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="ee6c4-205">Windows</span><span class="sxs-lookup"><span data-stu-id="ee6c4-205">Windows</span></span>](#tab/windows)

```
%USERPROFILE%\.dotnet\store\x64\<TARGET_FRAMEWORK_MONIKER>\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\<TARGET_FRAMEWORK_MONIKER>\
```

# <a name="macostabmacos"></a>[<span data-ttu-id="ee6c4-206">macOS</span><span class="sxs-lookup"><span data-stu-id="ee6c4-206">macOS</span></span>](#tab/macos)

```
/Users/<USER>/.dotnet/store/x64/<TARGET_FRAMEWORK_MONIKER>/<ENHANCEMENT_ASSEMBLY_NAME>/<ENHANCEMENT_VERSION>/lib/<TARGET_FRAMEWORK_MONIKER>/
```

# <a name="linuxtablinux"></a>[<span data-ttu-id="ee6c4-207">Linux</span><span class="sxs-lookup"><span data-stu-id="ee6c4-207">Linux</span></span>](#tab/linux)

```
/Users/<USER>/.dotnet/store/x64/<TARGET_FRAMEWORK_MONIKER>/<ENHANCEMENT_ASSEMBLY_NAME>/<ENHANCEMENT_VERSION>/lib/<TARGET_FRAMEWORK_MONIKER>/
```

---

<span data-ttu-id="ee6c4-208">Si desea colocar el ensamblado y las dependencias para su uso global, agregue la opción `-o|--output` al comando `dotnet store` con la ruta de acceso siguiente:</span><span class="sxs-lookup"><span data-stu-id="ee6c4-208">If you desire to place the assembly and dependencies for global use, add the `-o|--output` option to the `dotnet store` command with the following path:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="ee6c4-209">Windows</span><span class="sxs-lookup"><span data-stu-id="ee6c4-209">Windows</span></span>](#tab/windows)

```
%PROGRAMFILES%\dotnet\store\x64\<TARGET_FRAMEWORK_MONIKER>\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\<TARGET_FRAMEWORK_MONIKER>\
```

# <a name="macostabmacos"></a>[<span data-ttu-id="ee6c4-210">macOS</span><span class="sxs-lookup"><span data-stu-id="ee6c4-210">macOS</span></span>](#tab/macos)

```
/usr/local/share/dotnet/store/x64/<TARGET_FRAMEWORK_MONIKER>/<ENHANCEMENT_ASSEMBLY_NAME>/<ENHANCEMENT_VERSION>/lib/<TARGET_FRAMEWORK_MONIKER>/
```

# <a name="linuxtablinux"></a>[<span data-ttu-id="ee6c4-211">Linux</span><span class="sxs-lookup"><span data-stu-id="ee6c4-211">Linux</span></span>](#tab/linux)

```
/usr/local/share/dotnet/store/x64/<TARGET_FRAMEWORK_MONIKER>/<ENHANCEMENT_ASSEMBLY_NAME>/<ENHANCEMENT_VERSION>/lib/<TARGET_FRAMEWORK_MONIKER>/
```

---

<span data-ttu-id="ee6c4-212">**Modificar y colocar el archivo de dependencias del inicio de hospedaje**</span><span class="sxs-lookup"><span data-stu-id="ee6c4-212">**Modify and place the hosting startup's dependencies file**</span></span>

<span data-ttu-id="ee6c4-213">La ubicación del tiempo de ejecución se especifica en el archivo *\*.deps.json*.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-213">The runtime location is specified in the *\*.deps.json* file.</span></span> <span data-ttu-id="ee6c4-214">Para activar la mejora, el elemento `runtime` debe especificar la ubicación del ensamblado de tiempo de ejecución de la mejora.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-214">To activate the enhancement, the `runtime` element must specify the location of the enhancement's runtime assembly.</span></span> <span data-ttu-id="ee6c4-215">Anteponga `lib/<TARGET_FRAMEWORK_MONIKER>/` a la ubicación de `runtime`:</span><span class="sxs-lookup"><span data-stu-id="ee6c4-215">Prefix the `runtime` location with `lib/<TARGET_FRAMEWORK_MONIKER>/`:</span></span>

[!code-json[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement2.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="ee6c4-216">En la aplicación de ejemplo (proyecto *StartupDiagnostics*), el archivo *\*.deps.json* se modifica por medio de un script de [PowerShell](/powershell/scripting/powershell-scripting).</span><span class="sxs-lookup"><span data-stu-id="ee6c4-216">In the sample code (*StartupDiagnostics* project), modification of the *\*.deps.json* file is performed by a [PowerShell](/powershell/scripting/powershell-scripting) script.</span></span> <span data-ttu-id="ee6c4-217">que se activa automáticamente a través de un destino de compilación en el archivo de proyecto.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-217">The PowerShell script is automatically triggered by a build target in the project file.</span></span>

<span data-ttu-id="ee6c4-218">El archivo *\*.deps.json* de la implementación debe estar en una ubicación accesible.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-218">The implementation's *\*.deps.json* file must be in an accessible location.</span></span>

<span data-ttu-id="ee6c4-219">Para usarlo individualmente con cada usuario, coloque el archivo en la carpeta *additonalDeps* de la configuración de `.dotnet` del perfil de usuario:</span><span class="sxs-lookup"><span data-stu-id="ee6c4-219">For per-user use, place the file in the *additonalDeps* folder of the user profile's `.dotnet` settings:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="ee6c4-220">Windows</span><span class="sxs-lookup"><span data-stu-id="ee6c4-220">Windows</span></span>](#tab/windows)

```
%USERPROFILE%\.dotnet\x64\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\
```

# <a name="macostabmacos"></a>[<span data-ttu-id="ee6c4-221">macOS</span><span class="sxs-lookup"><span data-stu-id="ee6c4-221">macOS</span></span>](#tab/macos)

```
/Users/<USER>/.dotnet/x64/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/
```

# <a name="linuxtablinux"></a>[<span data-ttu-id="ee6c4-222">Linux</span><span class="sxs-lookup"><span data-stu-id="ee6c4-222">Linux</span></span>](#tab/linux)

```
/Users/<USER>/.dotnet/x64/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/
```

---

<span data-ttu-id="ee6c4-223">Para usarlo de manera global, colóquelo en la carpeta *additonalDeps* de la instalación de .NET Core:</span><span class="sxs-lookup"><span data-stu-id="ee6c4-223">For global use, place the file in the *additonalDeps* folder of the .NET Core installation:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="ee6c4-224">Windows</span><span class="sxs-lookup"><span data-stu-id="ee6c4-224">Windows</span></span>](#tab/windows)

```
%PROGRAMFILES%\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\
```

# <a name="macostabmacos"></a>[<span data-ttu-id="ee6c4-225">macOS</span><span class="sxs-lookup"><span data-stu-id="ee6c4-225">macOS</span></span>](#tab/macos)

```
/usr/local/share/dotnet/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/
```

# <a name="linuxtablinux"></a>[<span data-ttu-id="ee6c4-226">Linux</span><span class="sxs-lookup"><span data-stu-id="ee6c4-226">Linux</span></span>](#tab/linux)

```
/usr/local/share/dotnet/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/
```

---

<span data-ttu-id="ee6c4-227">La versión "Shared Framework" es la versión del tiempo de ejecución compartido que la aplicación de destino usa.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-227">The shared framework version reflects the version of the shared runtime that the target app uses.</span></span> <span data-ttu-id="ee6c4-228">El tiempo de ejecución compartido se muestra en el archivo *\*.runtimeconfig.json*.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-228">The shared runtime is shown in the *\*.runtimeconfig.json* file.</span></span> <span data-ttu-id="ee6c4-229">En la aplicación de ejemplo (*HostingStartupApp*), el tiempo de ejecución compartido se especifica en el archivo *HostingStartupApp.runtimeconfig.json*.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-229">In the sample app (*HostingStartupApp*), the shared runtime is specified in the *HostingStartupApp.runtimeconfig.json* file.</span></span>

<span data-ttu-id="ee6c4-230">**Indicar el archivo de dependencias del inicio de hospedaje**</span><span class="sxs-lookup"><span data-stu-id="ee6c4-230">**List the hosting startup's dependencies file**</span></span>

<span data-ttu-id="ee6c4-231">La ubicación del archivo *\*.deps.json* de implementación aparece en la variable de entorno `DOTNET_ADDITIONAL_DEPS`.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-231">The location of the implementation's *\*.deps.json* file is listed in the `DOTNET_ADDITIONAL_DEPS` environment variable.</span></span>

<span data-ttu-id="ee6c4-232">Si el archivo se coloca en la carpeta *.dotnet* del perfil de usuario, establezca el valor de la variable de entorno en:</span><span class="sxs-lookup"><span data-stu-id="ee6c4-232">If the file is placed in the user profile's *.dotnet* folder, set the environment variable's value to:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="ee6c4-233">Windows</span><span class="sxs-lookup"><span data-stu-id="ee6c4-233">Windows</span></span>](#tab/windows)

```
%USERPROFILE%\.dotnet\x64\additionalDeps\
```

# <a name="macostabmacos"></a>[<span data-ttu-id="ee6c4-234">macOS</span><span class="sxs-lookup"><span data-stu-id="ee6c4-234">macOS</span></span>](#tab/macos)

```
/Users/<USER>/.dotnet/x64/additionalDeps/
```

# <a name="linuxtablinux"></a>[<span data-ttu-id="ee6c4-235">Linux</span><span class="sxs-lookup"><span data-stu-id="ee6c4-235">Linux</span></span>](#tab/linux)

```
/Users/<USER>/.dotnet/x64/additionalDeps/
```

---

<span data-ttu-id="ee6c4-236">Si el archivo está en la instalación de .NET Core para usarlo de manera global, indique la ruta de acceso completa al archivo:</span><span class="sxs-lookup"><span data-stu-id="ee6c4-236">If the file is placed in the .NET Core installation for global use, provide the full path to the file:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="ee6c4-237">Windows</span><span class="sxs-lookup"><span data-stu-id="ee6c4-237">Windows</span></span>](#tab/windows)

```
%PROGRAMFILES%\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\<ENHANCEMENT_ASSEMBLY_NAME>.deps.json
```

# <a name="macostabmacos"></a>[<span data-ttu-id="ee6c4-238">macOS</span><span class="sxs-lookup"><span data-stu-id="ee6c4-238">macOS</span></span>](#tab/macos)

```
/usr/local/share/dotnet/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/<ENHANCEMENT_ASSEMBLY_NAME>.deps.json
```

# <a name="linuxtablinux"></a>[<span data-ttu-id="ee6c4-239">Linux</span><span class="sxs-lookup"><span data-stu-id="ee6c4-239">Linux</span></span>](#tab/linux)

```
/usr/local/share/dotnet/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/<ENHANCEMENT_ASSEMBLY_NAME>.deps.json
```

---

<span data-ttu-id="ee6c4-240">Para que la aplicación de ejemplo (*HostingStartupApp*) encuentre el archivo de dependencias (*HostingStartupApp.runtimeconfig.json*), el archivo de dependencias se coloca en el perfil del usuario.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-240">For the sample app (*HostingStartupApp*) to find the dependencies file (*HostingStartupApp.runtimeconfig.json*), the dependencies file is placed in the user's profile.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="ee6c4-241">Windows</span><span class="sxs-lookup"><span data-stu-id="ee6c4-241">Windows</span></span>](#tab/windows)

<span data-ttu-id="ee6c4-242">Use la sintaxis siguiente para establecer la variable de entorno `DOTNET_ADDITIONAL_DEPS`:</span><span class="sxs-lookup"><span data-stu-id="ee6c4-242">Set the `DOTNET_ADDITIONAL_DEPS` environment variable to the following value:</span></span>

```
%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\
```

# <a name="macostabmacos"></a>[<span data-ttu-id="ee6c4-243">macOS</span><span class="sxs-lookup"><span data-stu-id="ee6c4-243">macOS</span></span>](#tab/macos)

<span data-ttu-id="ee6c4-244">Use la sintaxis siguiente para establecer la variable de entorno `DOTNET_ADDITIONAL_DEPS`:</span><span class="sxs-lookup"><span data-stu-id="ee6c4-244">Set the `DOTNET_ADDITIONAL_DEPS` environment variable to the following value:</span></span>

```
/Users/<USER>/.dotnet/x64/additionalDeps/StartupDiagnostics/
```

# <a name="linuxtablinux"></a>[<span data-ttu-id="ee6c4-245">Linux</span><span class="sxs-lookup"><span data-stu-id="ee6c4-245">Linux</span></span>](#tab/linux)

<span data-ttu-id="ee6c4-246">Use la sintaxis siguiente para establecer la variable de entorno `DOTNET_ADDITIONAL_DEPS`:</span><span class="sxs-lookup"><span data-stu-id="ee6c4-246">Set the `DOTNET_ADDITIONAL_DEPS` environment variable to the following value:</span></span>

```
/Users/<USER>/.dotnet/x64/additionalDeps/StartupDiagnostics/
```

---

<span data-ttu-id="ee6c4-247">Para obtener ejemplos de cómo establecer variables de entorno en distintos sistemas operativos, vea [Uso de varios entornos](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="ee6c4-247">For examples of how to set environment variables for various operating systems, see [Use multiple environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="ee6c4-248">**Implementación**</span><span class="sxs-lookup"><span data-stu-id="ee6c4-248">**Deployment**</span></span>

<span data-ttu-id="ee6c4-249">Para facilitar la implementación de un inicio de hospedaje en un entorno de varios equipos, la aplicación de ejemplo crea una carpeta de *implementación* en el resultado publicado que contiene:</span><span class="sxs-lookup"><span data-stu-id="ee6c4-249">To facilitate the deployment of a hosting startup in a multimachine environment, the sample app creates a *deployment* folder in published output that contains:</span></span>

* <span data-ttu-id="ee6c4-250">El ensamblado de inicio de hospedaje.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-250">The hosting startup assembly.</span></span>
* <span data-ttu-id="ee6c4-251">El archivo de dependencias del inicio de hospedaje.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-251">The hosting startup dependencies file.</span></span>
* <span data-ttu-id="ee6c4-252">Un script de PowerShell que crea o modifica `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` y `DOTNET_ADDITIONAL_DEPS` para admitir la activación del inicio del hospedaje.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-252">A PowerShell script that creates or modifies the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` and `DOTNET_ADDITIONAL_DEPS` to support the activation of the hosting startup.</span></span> <span data-ttu-id="ee6c4-253">Ejecute el script desde un símbolo del sistema de PowerShell administrativo en el sistema de implementación.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-253">Run the script from an administrative PowerShell command prompt on the deployment system.</span></span>

### <a name="nuget-package"></a><span data-ttu-id="ee6c4-254">Detección de</span><span class="sxs-lookup"><span data-stu-id="ee6c4-254">NuGet package</span></span>

<span data-ttu-id="ee6c4-255">Una mejora de inicio de hospedaje se puede proporcionar en un paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-255">A hosting startup enhancement can be provided in a NuGet package.</span></span> <span data-ttu-id="ee6c4-256">El paquete tiene un atributo `HostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-256">The package has a `HostingStartup` attribute.</span></span> <span data-ttu-id="ee6c4-257">Los tipos de inicio de hospedaje proporcionados por el paquete están disponibles en la aplicación mediante cualquiera de los métodos siguientes:</span><span class="sxs-lookup"><span data-stu-id="ee6c4-257">The hosting startup types provided by the package are made available to the app using either of the following approaches:</span></span>

* <span data-ttu-id="ee6c4-258">El archivo de proyecto de la aplicación mejorada hace una referencia al paquete para el inicio de hospedaje en el archivo de proyecto de la aplicación (una referencia de tiempo de compilación).</span><span class="sxs-lookup"><span data-stu-id="ee6c4-258">The enhanced app's project file makes a package reference for the hosting startup in the app's project file (a compile-time reference).</span></span> <span data-ttu-id="ee6c4-259">Con la referencia de tiempo de compilación realizada, el ensamblado de inicio de hospedaje y todas sus dependencias se incorporan en el archivo de dependencia de la aplicación (*\*. deps.json*).</span><span class="sxs-lookup"><span data-stu-id="ee6c4-259">With the compile-time reference in place, the hosting startup assembly and all of its dependencies are incorporated into the app's dependency file (*\*.deps.json*).</span></span> <span data-ttu-id="ee6c4-260">Este enfoque se aplica a un paquete de ensamblado de inicio de hospedaje publicado en [nuget.org](https://www.nuget.org/).</span><span class="sxs-lookup"><span data-stu-id="ee6c4-260">This approach applies to a hosting startup assembly package published to [nuget.org](https://www.nuget.org/).</span></span>
* <span data-ttu-id="ee6c4-261">El archivo de dependencias del inicio de hospedaje está disponible para la aplicación mejorada como se describe en la sección [Almacén en tiempo de ejecución](#runtime-store) (sin una referencia de tiempo de compilación).</span><span class="sxs-lookup"><span data-stu-id="ee6c4-261">The hosting startup's dependencies file is made available to the enhanced app as described in the [Runtime store](#runtime-store) section (without a compile-time reference).</span></span>

<span data-ttu-id="ee6c4-262">Para obtener más información sobre los paquetes NuGet y el almacén en tiempo de ejecución, vea los temas siguientes:</span><span class="sxs-lookup"><span data-stu-id="ee6c4-262">For more information on NuGet packages and the runtime store, see the following topics:</span></span>

* [<span data-ttu-id="ee6c4-263">Cómo crear un paquete NuGet con herramientas multiplataforma</span><span class="sxs-lookup"><span data-stu-id="ee6c4-263">How to Create a NuGet Package with Cross Platform Tools</span></span>](/dotnet/core/deploying/creating-nuget-packages)
* [<span data-ttu-id="ee6c4-264">Publicar paquetes</span><span class="sxs-lookup"><span data-stu-id="ee6c4-264">Publishing packages</span></span>](/nuget/create-packages/publish-a-package)
* <span data-ttu-id="ee6c4-265">[Runtime package store](/dotnet/core/deploying/runtime-store) (Almacenamiento de paquetes en tiempo de ejecución)</span><span class="sxs-lookup"><span data-stu-id="ee6c4-265">[Runtime package store](/dotnet/core/deploying/runtime-store)</span></span>

### <a name="project-bin-folder"></a><span data-ttu-id="ee6c4-266">Carpeta bin del proyecto</span><span class="sxs-lookup"><span data-stu-id="ee6c4-266">Project bin folder</span></span>

<span data-ttu-id="ee6c4-267">Un ensamblado implementado por *bin* puede proporcionar una mejora del inicio de hospedaje en la aplicación mejorada.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-267">A hosting startup enhancement can be provided by a *bin*-deployed assembly in the enhanced app.</span></span> <span data-ttu-id="ee6c4-268">Los tipos de inicio de hospedaje proporcionados por el ensamblado están disponibles en la aplicación mediante cualquiera de los métodos siguientes:</span><span class="sxs-lookup"><span data-stu-id="ee6c4-268">The hosting startup types provided by the assembly are made available to the app using either of the following approaches:</span></span>

* <span data-ttu-id="ee6c4-269">El archivo de proyecto de la aplicación mejorada hace referencia de ensamblado al inicio de hospedaje (una referencia de tiempo de compilación).</span><span class="sxs-lookup"><span data-stu-id="ee6c4-269">The enhanced app's project file makes an assembly reference to the hosting startup (a compile-time reference).</span></span> <span data-ttu-id="ee6c4-270">Con la referencia de tiempo de compilación realizada, el ensamblado de inicio de hospedaje y todas sus dependencias se incorporan en el archivo de dependencia de la aplicación (*\*. deps.json*).</span><span class="sxs-lookup"><span data-stu-id="ee6c4-270">With the compile-time reference in place, the hosting startup assembly and all of its dependencies are incorporated into the app's dependency file (*\*.deps.json*).</span></span> <span data-ttu-id="ee6c4-271">Este enfoque se aplica cuando el escenario de implementación llama para mover el ensamblado de la biblioteca de inicio de hospedaje compilado (archivo DLL) al proyecto de consumo o a una ubicación a la que el proyecto de consumo pueda acceder y se realiza una referencia de tiempo de compilación en el ensamblado del inicio de hospedaje.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-271">This approach applies when the deployment scenario calls for moving the compiled hosting startup library's assembly (DLL file) to the consuming project or to a location accessible by the consuming project and a compile-time reference is made to the hosting startup's assembly.</span></span>
* <span data-ttu-id="ee6c4-272">El archivo de dependencias del inicio de hospedaje está disponible para la aplicación mejorada como se describe en la sección [Almacén en tiempo de ejecución](#runtime-store) (sin una referencia de tiempo de compilación).</span><span class="sxs-lookup"><span data-stu-id="ee6c4-272">The hosting startup's dependencies file is made available to the enhanced app as described in the [Runtime store](#runtime-store) section (without a compile-time reference).</span></span>

## <a name="sample-code"></a><span data-ttu-id="ee6c4-273">Código de ejemplo</span><span class="sxs-lookup"><span data-stu-id="ee6c4-273">Sample code</span></span>

<span data-ttu-id="ee6c4-274">En el [código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([cómo descargar](xref:index#how-to-download-a-sample)) se muestran escenarios de implementación de inicio de hospedaje:</span><span class="sxs-lookup"><span data-stu-id="ee6c4-274">The [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([how to download](xref:index#how-to-download-a-sample)) demonstrates hosting startup implementation scenarios:</span></span>

* <span data-ttu-id="ee6c4-275">Cada uno de los dos ensamblados de inicio de hospedaje (bibliotecas de clases) establece un par clave-valor de configuración en memoria:</span><span class="sxs-lookup"><span data-stu-id="ee6c4-275">Two hosting startup assemblies (class libraries) set a pair of in-memory configuration key-value pairs each:</span></span>
  * <span data-ttu-id="ee6c4-276">Paquete NuGet (*HostingStartupPackage*)</span><span class="sxs-lookup"><span data-stu-id="ee6c4-276">NuGet package (*HostingStartupPackage*)</span></span>
  * <span data-ttu-id="ee6c4-277">Biblioteca de clases (*HostingStartupLibrary*)</span><span class="sxs-lookup"><span data-stu-id="ee6c4-277">Class library (*HostingStartupLibrary*)</span></span>
* <span data-ttu-id="ee6c4-278">Se activa un inicio de hospedaje desde un ensamblado implementado por el almacén de tiempo de ejecución (*StartupDiagnostics*).</span><span class="sxs-lookup"><span data-stu-id="ee6c4-278">A hosting startup is activated from a runtime store-deployed assembly (*StartupDiagnostics*).</span></span> <span data-ttu-id="ee6c4-279">Este ensamblado agrega dos middleware a la aplicación (mientras se inicia) que proporcionan información de diagnóstico en:</span><span class="sxs-lookup"><span data-stu-id="ee6c4-279">The assembly adds two middlewares to the app at startup that provide diagnostic information on:</span></span>
  * <span data-ttu-id="ee6c4-280">Servicios registrados</span><span class="sxs-lookup"><span data-stu-id="ee6c4-280">Registered services</span></span>
  * <span data-ttu-id="ee6c4-281">Dirección (esquema, host, ruta de acceso base, ruta de acceso, cadena de consulta)</span><span class="sxs-lookup"><span data-stu-id="ee6c4-281">Address (scheme, host, path base, path, query string)</span></span>
  * <span data-ttu-id="ee6c4-282">Conexión (dirección IP remota, puerto remoto, dirección IP local, puerto local, certificado de cliente)</span><span class="sxs-lookup"><span data-stu-id="ee6c4-282">Connection (remote IP, remote port, local IP, local port, client certificate)</span></span>
  * <span data-ttu-id="ee6c4-283">Encabezados de solicitud</span><span class="sxs-lookup"><span data-stu-id="ee6c4-283">Request headers</span></span>
  * <span data-ttu-id="ee6c4-284">Variables de entorno</span><span class="sxs-lookup"><span data-stu-id="ee6c4-284">Environment variables</span></span>

<span data-ttu-id="ee6c4-285">Para ejecutar el ejemplo:</span><span class="sxs-lookup"><span data-stu-id="ee6c4-285">To run the sample:</span></span>

<span data-ttu-id="ee6c4-286">**Activación desde un paquete NuGet**</span><span class="sxs-lookup"><span data-stu-id="ee6c4-286">**Activation from a NuGet package**</span></span>

1. <span data-ttu-id="ee6c4-287">Compile el paquete *HostingStartupPackage* con el comando [dotnet pack](/dotnet/core/tools/dotnet-pack).</span><span class="sxs-lookup"><span data-stu-id="ee6c4-287">Compile the *HostingStartupPackage* package with the [dotnet pack](/dotnet/core/tools/dotnet-pack) command.</span></span>
1. <span data-ttu-id="ee6c4-288">Agregue el nombre de ensamblado del paquete de *HostingStartupPackage* a la variable de entorno `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-288">Add the package's assembly name of the *HostingStartupPackage* to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
1. <span data-ttu-id="ee6c4-289">Compile y ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-289">Compile and run the app.</span></span> <span data-ttu-id="ee6c4-290">Una referencia de paquete está presente en la aplicación mejorada (una referencia de tiempo de compilación).</span><span class="sxs-lookup"><span data-stu-id="ee6c4-290">A package reference is present in the enhanced app (a compile-time reference).</span></span> <span data-ttu-id="ee6c4-291">El parámetro `<PropertyGroup>` del archivo del proyecto de la aplicación especifica el resultado del proyecto de paquete (*../HostingStartupPackage/bin/Debug*) como origen del paquete.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-291">A `<PropertyGroup>` in the app's project file specifies the package project's output (*../HostingStartupPackage/bin/Debug*) as a package source.</span></span> <span data-ttu-id="ee6c4-292">Esto permite que la aplicación utilice el paquete sin cargar el paquete a [nuget.org](https://www.nuget.org/). Para obtener más información, vea las notas que encontrará en el archivo del proyecto de HostingStartupApp.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-292">This allows the app to use the package without uploading the package to [nuget.org](https://www.nuget.org/). For more information, see the notes in the HostingStartupApp's project file.</span></span>

   ```xml
   <PropertyGroup>
     <RestoreSources>$(RestoreSources);https://api.nuget.org/v3/index.json;../HostingStartupPackage/bin/Debug</RestoreSources>
   </PropertyGroup>
   ```
1. <span data-ttu-id="ee6c4-293">Observe que los valores de la clave de configuración del servicio representados por la página de índice coinciden con los valores establecidos por método `ServiceKeyInjection.Configure` del paquete.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-293">Observe that the service configuration key values rendered by the Index page match the values set by the package's `ServiceKeyInjection.Configure` method.</span></span>

<span data-ttu-id="ee6c4-294">Si realiza cambios en el proyecto *HostingStartupPackage* y lo vuelve a compilar, borre las cachés del paquete NuGet local para asegurarse de que *HostingStartupApp* recibe el paquete actualizado y no un paquete en desuso de la caché local.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-294">If you make changes to the *HostingStartupPackage* project and recompile it, clear the local NuGet package caches to ensure that the *HostingStartupApp* receives the updated package and not a stale package from the local cache.</span></span> <span data-ttu-id="ee6c4-295">Para borrar las cachés de NuGet locales, ejecute el siguiente comando [dotnet nuget locals](/dotnet/core/tools/dotnet-nuget-locals):</span><span class="sxs-lookup"><span data-stu-id="ee6c4-295">To clear the local NuGet caches, execute the following [dotnet nuget locals](/dotnet/core/tools/dotnet-nuget-locals) command:</span></span>

```console
dotnet nuget locals all --clear
```

<span data-ttu-id="ee6c4-296">**Activación desde una biblioteca de clases**</span><span class="sxs-lookup"><span data-stu-id="ee6c4-296">**Activation from a class library**</span></span>

1. <span data-ttu-id="ee6c4-297">Compile la biblioteca de clases *HostingStartupLibrary* con el comando [dotnet build](/dotnet/core/tools/dotnet-build).</span><span class="sxs-lookup"><span data-stu-id="ee6c4-297">Compile the *HostingStartupLibrary* class library with the [dotnet build](/dotnet/core/tools/dotnet-build) command.</span></span>
1. <span data-ttu-id="ee6c4-298">Agregue el nombre del ensamblado de la biblioteca de clases *HostingStartupLibrary* a la variable de entorno `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-298">Add the class library's assembly name of *HostingStartupLibrary* to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
1. <span data-ttu-id="ee6c4-299">Implemente con *bin*-el ensamblado de la biblioteca de clases en la aplicación copiando el archivo *HostingStartupLibrary.dll* desde el resultado compilado de la biblioteca de aplicaciones en la carpeta *bin/Debug* de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-299">*bin*-deploy the class library's assembly to the app by copying the *HostingStartupLibrary.dll* file from the class library's compiled output to the app's *bin/Debug* folder.</span></span>
1. <span data-ttu-id="ee6c4-300">Compile y ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-300">Compile and run the app.</span></span> <span data-ttu-id="ee6c4-301">Un parámetro `<ItemGroup>` del archivo del proyecto de la aplicación hace referencia al ensamblado de la biblioteca de clases (*.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll*) (una referencia de tiempo de compilación).</span><span class="sxs-lookup"><span data-stu-id="ee6c4-301">An `<ItemGroup>` in the app's project file references the class library's assembly (*.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll*) (a compile-time reference).</span></span> <span data-ttu-id="ee6c4-302">Para obtener más información, vea las notas que encontrará en el archivo del proyecto de HostingStartupApp.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-302">For more information, see the notes in the HostingStartupApp's project file.</span></span>

   ```xml
   <ItemGroup>
     <Reference Include=".\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll">
       <HintPath>.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll</HintPath>
       <SpecificVersion>False</SpecificVersion>
     </Reference>
   </ItemGroup>
   ```
1. <span data-ttu-id="ee6c4-303">Observe que los valores de la clave de configuración del servicio representados por la página de índice coinciden con los valores establecidos por el método `ServiceKeyInjection.Configure` de la biblioteca de clases.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-303">Observe that the service configuration key values rendered by the Index page match the values set by the class library's `ServiceKeyInjection.Configure` method.</span></span>

<span data-ttu-id="ee6c4-304">**Activación desde un ensamblado implementado por el almacén de tiempo de ejecución**</span><span class="sxs-lookup"><span data-stu-id="ee6c4-304">**Activation from a runtime store-deployed assembly**</span></span>

1. <span data-ttu-id="ee6c4-305">El proyecto *StartupDiagnostics* usa [PowerShell](/powershell/scripting/powershell-scripting) para modificar el archivo *StartupDiagnostics.deps.json*.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-305">The *StartupDiagnostics* project uses [PowerShell](/powershell/scripting/powershell-scripting) to modify its *StartupDiagnostics.deps.json* file.</span></span> <span data-ttu-id="ee6c4-306">PowerShell se instala de forma predeterminada en Windows a partir de Windows 7 SP1 y Windows Server 2008 R2 SP1.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-306">PowerShell is installed by default on Windows starting with Windows 7 SP1 and Windows Server 2008 R2 SP1.</span></span> <span data-ttu-id="ee6c4-307">Para obtener PowerShell en otras plataformas, vea [Instalación de Windows PowerShell](/powershell/scripting/setup/installing-powershell#powershell-core).</span><span class="sxs-lookup"><span data-stu-id="ee6c4-307">To obtain PowerShell on other platforms, see [Installing Windows PowerShell](/powershell/scripting/setup/installing-powershell#powershell-core).</span></span>
1. <span data-ttu-id="ee6c4-308">Compile el proyecto *StartupDiagnostics*.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-308">Build the *StartupDiagnostics* project.</span></span> <span data-ttu-id="ee6c4-309">Después de compilar el proyecto, un destino de compilación en el archivo de proyecto realiza automáticamente las acciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="ee6c4-309">After the project is built, a build target in the project file automatically:</span></span>
   * <span data-ttu-id="ee6c4-310">Activa el script de PowerShell para modificar el archivo *StartupDiagnostics.deps.json*.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-310">Triggers the PowerShell script to modify the *StartupDiagnostics.deps.json* file.</span></span>
   * <span data-ttu-id="ee6c4-311">Mueve el archivo *StartupDiagnostics.deps.json* a la carpeta *additionalDeps* del perfil de usuario.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-311">Moves the *StartupDiagnostics.deps.json* file to the user profile's *additionalDeps* folder.</span></span>
1. <span data-ttu-id="ee6c4-312">Ejecute el comando `dotnet store` en un símbolo del sistema en el directorio de inicio de hospedaje para almacenar el ensamblado y sus dependencias en el almacén de tiempo de ejecución del perfil de usuario:</span><span class="sxs-lookup"><span data-stu-id="ee6c4-312">Execute the `dotnet store` command at a command prompt in the hosting startup's directory to store the assembly and its dependencies in the user profile's runtime store:</span></span>

   ```console
   dotnet store --manifest StartupDiagnostics.csproj --runtime <RID>
   ```
   <span data-ttu-id="ee6c4-313">Para Windows, el comando usa el [identificador en tiempo de ejecución (RID)](/dotnet/core/rid-catalog) `win7-x64`.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-313">For Windows, the command uses the `win7-x64` [runtime identifier (RID)](/dotnet/core/rid-catalog).</span></span> <span data-ttu-id="ee6c4-314">Para proporcionar el inicio de hospedaje para otro tiempo de ejecución, sustituya el RID correcto.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-314">When providing the hosting startup for a different runtime, substitute the correct RID.</span></span>
1. <span data-ttu-id="ee6c4-315">Establezca las variables de entorno:</span><span class="sxs-lookup"><span data-stu-id="ee6c4-315">Set the environment variables:</span></span>
   * <span data-ttu-id="ee6c4-316">Agregue el nombre de ensamblado de *StartupDiagnostics* a la variable de entorno `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-316">Add the assembly name of *StartupDiagnostics* to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
   * <span data-ttu-id="ee6c4-317">En Windows, establezca la variable de entorno `DOTNET_ADDITIONAL_DEPS` en `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-317">On Windows, set the `DOTNET_ADDITIONAL_DEPS` environment variable to `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`.</span></span> <span data-ttu-id="ee6c4-318">En macOS o Linux, establezca la variable de entorno `DOTNET_ADDITIONAL_DEPS` en `/Users/<USER>/.dotnet/x64/additionalDeps/StartupDiagnostics/`, donde `<USER>` es el perfil de usuario que contiene el inicio de hospedaje.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-318">On macOS/Linux, set the `DOTNET_ADDITIONAL_DEPS` environment variable to `/Users/<USER>/.dotnet/x64/additionalDeps/StartupDiagnostics/`, where `<USER>` is the user profile that contains the hosting startup.</span></span>
1. <span data-ttu-id="ee6c4-319">Ejecute la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-319">Run the sample app.</span></span>
1. <span data-ttu-id="ee6c4-320">Solicite al punto de conexión `/services` ver los servicios registrados de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-320">Request the `/services` endpoint to see the app's registered services.</span></span> <span data-ttu-id="ee6c4-321">Solicite al punto de conexión `/diag` ver la información de diagnóstico.</span><span class="sxs-lookup"><span data-stu-id="ee6c4-321">Request the `/diag` endpoint to see the diagnostic information.</span></span>
