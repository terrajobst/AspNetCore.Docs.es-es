---
title: Mejorar una aplicación desde un ensamblado externo en ASP.NET Core con IHostingStartup
author: guardrex
description: Sepa cómo mejorar una aplicación ASP.NET Core desde un ensamblado externo con una implementación de IHostingStartup.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/configuration/platform-specific-configuration
ms.openlocfilehash: 793169b491596cd7326d747a3f19d7fdaf7e2b65
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/17/2018
---
# <a name="enhance-an-app-from-an-external-assembly-in-aspnet-core-with-ihostingstartup"></a><span data-ttu-id="7b90e-103">Mejorar una aplicación desde un ensamblado externo en ASP.NET Core con IHostingStartup</span><span class="sxs-lookup"><span data-stu-id="7b90e-103">Enhance an app from an external assembly in ASP.NET Core with IHostingStartup</span></span>

<span data-ttu-id="7b90e-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="7b90e-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="7b90e-105">Una implementación de [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) permite agregar mejoras a una aplicación al iniciarla a partir de un ensamblado externo fuera de la clase `Startup` de esta.</span><span class="sxs-lookup"><span data-stu-id="7b90e-105">An [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="7b90e-106">Por ejemplo, una biblioteca de herramientas externas puede usar una implementación de `IHostingStartup` para suministrar más servicios o proveedores de configuración a una aplicación.</span><span class="sxs-lookup"><span data-stu-id="7b90e-106">For example, an external tooling library can use an `IHostingStartup` implementation to provide additional configuration providers or services to an app.</span></span> <span data-ttu-id="7b90e-107">`IHostingStartup`  *está disponible en ASP.NET 2.0 Core y versiones posteriores.*</span><span class="sxs-lookup"><span data-stu-id="7b90e-107">`IHostingStartup` *is available in ASP.NET Core 2.0 and later.*</span></span>

<span data-ttu-id="7b90e-108">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/platform-specific-configuration/sample/) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7b90e-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/platform-specific-configuration/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="discover-loaded-hosting-startup-assemblies"></a><span data-ttu-id="7b90e-109">Detectar los ensamblados de inicio de hospedaje cargados</span><span class="sxs-lookup"><span data-stu-id="7b90e-109">Discover loaded hosting startup assemblies</span></span>

<span data-ttu-id="7b90e-110">Para detectar los ensamblados de inicio de hospedaje que la aplicación o las bibliotecas han cargado, habilite el registro y consulte los registros de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7b90e-110">To discover hosting startup assemblies loaded by the app or by libraries, enable logging and check the application logs.</span></span> <span data-ttu-id="7b90e-111">En ellos se registran los errores que se producen al cargar ensamblados.</span><span class="sxs-lookup"><span data-stu-id="7b90e-111">Errors that occur when loading assemblies are logged.</span></span> <span data-ttu-id="7b90e-112">Los ensamblados de inicio de hospedaje se registran en el nivel de depuración y se registran todos los errores.</span><span class="sxs-lookup"><span data-stu-id="7b90e-112">Loaded hosting startup assemblies are logged at the Debug level, and all errors are logged.</span></span>

<span data-ttu-id="7b90e-113">La aplicación de ejemplo lee el valor de [HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey) en una matriz `string` y muestra el resultado en la página de índice de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="7b90e-113">The sample app reads the [HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey) into a `string` array and displays the result in the app's Index page:</span></span>

[!code-csharp[](platform-specific-configuration/sample/HostingStartupSample/Pages/Index.cshtml.cs?name=snippet1&highlight=14-16)]

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a><span data-ttu-id="7b90e-114">Deshabilitar la carga automática de ensamblados de inicio de hospedaje</span><span class="sxs-lookup"><span data-stu-id="7b90e-114">Disable automatic loading of hosting startup assemblies</span></span>

<span data-ttu-id="7b90e-115">Existen dos formas de deshabilitar la carga automática de ensamblados de inicio de hospedaje:</span><span class="sxs-lookup"><span data-stu-id="7b90e-115">There are two ways to disable the automatic loading of hosting startup assemblies:</span></span>

* <span data-ttu-id="7b90e-116">Establecer la opción de configuración de host para [impedir el inicio de hospedaje](xref:fundamentals/host/web-host#prevent-hosting-startup).</span><span class="sxs-lookup"><span data-stu-id="7b90e-116">Set the [Prevent Hosting Startup](xref:fundamentals/host/web-host#prevent-hosting-startup) host configuration setting.</span></span>
* <span data-ttu-id="7b90e-117">Establecer la variable de entorno `ASPNETCORE_PREVENTHOSTINGSTARTUP`.</span><span class="sxs-lookup"><span data-stu-id="7b90e-117">Set the `ASPNETCORE_PREVENTHOSTINGSTARTUP` environment variable.</span></span>

<span data-ttu-id="7b90e-118">Cuando la configuración del host o la variable de entorno está establecida en `true` o `1`, los ensamblados de inicio de hospedaje no se cargan automáticamente.</span><span class="sxs-lookup"><span data-stu-id="7b90e-118">When either the host setting or the environment variable is set to `true` or `1`, hosting startup assemblies aren't automatically loaded.</span></span> <span data-ttu-id="7b90e-119">Si ambas se han definido, será la configuración del host la que controle el comportamiento.</span><span class="sxs-lookup"><span data-stu-id="7b90e-119">If both are set, the host setting controls the behavior.</span></span>

<span data-ttu-id="7b90e-120">Deshabilitar los ensamblados de inicio de hospedaje a través de la variable de entorno o de la configuración de host hace que estos se deshabiliten globalmente y, asimismo, puede deshabilitar también otras características de una aplicación.</span><span class="sxs-lookup"><span data-stu-id="7b90e-120">Disabling hosting startup assemblies using the host setting or environment variable disables them globally and may disable several characteristics of an app.</span></span> <span data-ttu-id="7b90e-121">Actualmente no se puede deshabilitar de manera selectiva un ensamblado de inicio de hospedaje que esté agregado a una biblioteca, a menos que esa biblioteca disponga de su propia opción de configuración.</span><span class="sxs-lookup"><span data-stu-id="7b90e-121">It isn't currently possible to selectively disable a hosting startup assembly added by a library unless the library offers its own configuration option.</span></span> <span data-ttu-id="7b90e-122">En una próxima versión existirá la posibilidad de deshabilitar ensamblados de inicio de hospedaje de forma selectiva (vea el [problema de GitHub aspnet/Hosting n.º 1243](https://github.com/aspnet/Hosting/pull/1243)).</span><span class="sxs-lookup"><span data-stu-id="7b90e-122">A future release will offer the ability to selectively disable hosting startup assemblies (see [GitHub issue aspnet/Hosting #1243](https://github.com/aspnet/Hosting/pull/1243)).</span></span>

## <a name="implement-ihostingstartup"></a><span data-ttu-id="7b90e-123">Implementar IHostingStartup</span><span class="sxs-lookup"><span data-stu-id="7b90e-123">Implement IHostingStartup</span></span>

### <a name="create-the-assembly"></a><span data-ttu-id="7b90e-124">Crear el ensamblado</span><span class="sxs-lookup"><span data-stu-id="7b90e-124">Create the assembly</span></span>

<span data-ttu-id="7b90e-125">Una mejora de `IHostingStartup` se implementa como un ensamblado basado en una aplicación de consola sin un punto de entrada.</span><span class="sxs-lookup"><span data-stu-id="7b90e-125">An `IHostingStartup` enhancement is deployed as an assembly based on a console app without an entry point.</span></span> <span data-ttu-id="7b90e-126">El ensamblado hace referencia al paquete [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/):</span><span class="sxs-lookup"><span data-stu-id="7b90e-126">The assembly references the [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) package:</span></span>

[!code-xml[](platform-specific-configuration/snapshot_sample/StartupEnhancement.csproj)]

<span data-ttu-id="7b90e-127">Un atributo [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) identifica una clase como una implementación de `IHostingStartup` para la carga y ejecución al compilar [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost).</span><span class="sxs-lookup"><span data-stu-id="7b90e-127">A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) attribute identifies a class as an implementation of `IHostingStartup` for loading and execution when building the [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost).</span></span> <span data-ttu-id="7b90e-128">En el siguiente ejemplo, el espacio de nombres es `StartupEnhancement` y la clase, `StartupEnhancementHostingStartup`:</span><span class="sxs-lookup"><span data-stu-id="7b90e-128">In the following example, the namespace is `StartupEnhancement`, and the class is `StartupEnhancementHostingStartup`:</span></span>

[!code-csharp[](platform-specific-configuration/snapshot_sample/StartupEnhancement.cs?name=snippet1)]

<span data-ttu-id="7b90e-129">Una clase implementa `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="7b90e-129">A class implements `IHostingStartup`.</span></span> <span data-ttu-id="7b90e-130">El método [Configurar](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) de la clase usa un [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) para agregar mejoras a una aplicación:</span><span class="sxs-lookup"><span data-stu-id="7b90e-130">The class's [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) method uses an [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) to add enhancements to an app:</span></span>

[!code-csharp[](platform-specific-configuration/snapshot_sample/StartupEnhancement.cs?name=snippet2&highlight=3,5)]

<span data-ttu-id="7b90e-131">Al crear un proyecto de `IHostingStartup`, el archivo de dependencias (*\*.deps.json*) establece la ubicación de `runtime` del ensamblado en la carpeta *bin*:</span><span class="sxs-lookup"><span data-stu-id="7b90e-131">When building an `IHostingStartup` project, the dependencies file (*\*.deps.json*) sets the `runtime` location of the assembly to the *bin* folder:</span></span>

[!code-json[](platform-specific-configuration/snapshot_sample/StartupEnhancement1.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="7b90e-132">Solo se muestra parte del archivo.</span><span class="sxs-lookup"><span data-stu-id="7b90e-132">Only part of the file is shown.</span></span> <span data-ttu-id="7b90e-133">El nombre del ensamblado en el ejemplo es `StartupEnhancement`.</span><span class="sxs-lookup"><span data-stu-id="7b90e-133">The assembly name in the example is `StartupEnhancement`.</span></span>

### <a name="update-the-dependencies-file"></a><span data-ttu-id="7b90e-134">Actualizar el archivo de dependencias</span><span class="sxs-lookup"><span data-stu-id="7b90e-134">Update the dependencies file</span></span>

<span data-ttu-id="7b90e-135">La ubicación del tiempo de ejecución se especifica en el archivo *\*.deps.json*.</span><span class="sxs-lookup"><span data-stu-id="7b90e-135">The runtime location is specified in the *\*.deps.json* file.</span></span> <span data-ttu-id="7b90e-136">Para activar la mejora, el elemento `runtime` debe especificar la ubicación del ensamblado de tiempo de ejecución de la mejora.</span><span class="sxs-lookup"><span data-stu-id="7b90e-136">To active the enhancement, the `runtime` element must specify the location of the enhancement's runtime assembly.</span></span> <span data-ttu-id="7b90e-137">Anteponga `lib/netcoreapp2.0/` a la ubicación de `runtime`:</span><span class="sxs-lookup"><span data-stu-id="7b90e-137">Prefix the `runtime` location with `lib/netcoreapp2.0/`:</span></span>

[!code-json[](platform-specific-configuration/snapshot_sample/StartupEnhancement2.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="7b90e-138">En la aplicación de ejemplo, el archivo *\*.deps.json* se modifica por medio de un script de [PowerShell](/powershell/scripting/powershell-scripting)</span><span class="sxs-lookup"><span data-stu-id="7b90e-138">In the sample app, modification of the *\*.deps.json* file is performed by a [PowerShell](/powershell/scripting/powershell-scripting) script.</span></span> <span data-ttu-id="7b90e-139">que se activa automáticamente a través de un destino de compilación en el archivo de proyecto.</span><span class="sxs-lookup"><span data-stu-id="7b90e-139">The PowerShell script is automatically triggered by a build target in the project file.</span></span>

### <a name="enhancement-activation"></a><span data-ttu-id="7b90e-140">Activación de la mejora</span><span class="sxs-lookup"><span data-stu-id="7b90e-140">Enhancement activation</span></span>

<span data-ttu-id="7b90e-141">**Colocar el archivo de ensamblado**</span><span class="sxs-lookup"><span data-stu-id="7b90e-141">**Place the assembly file**</span></span>

<span data-ttu-id="7b90e-142">El archivo de ensamblado de la implementación `IHostingStartup` debe estar implementado en *bin* en la aplicación o colocarse en el [almacenamiento en tiempo de ejecución](/dotnet/core/deploying/runtime-store):</span><span class="sxs-lookup"><span data-stu-id="7b90e-142">The `IHostingStartup` implementation's assembly file must be *bin*-deployed in the app or placed in the [runtime store](/dotnet/core/deploying/runtime-store):</span></span>

<span data-ttu-id="7b90e-143">Para usarlo individualmente con cada usuario, coloque el ensamblado en el almacenamiento en tiempo de ejecución del perfil del usuario en cuestión, en:</span><span class="sxs-lookup"><span data-stu-id="7b90e-143">For per-user use, place the assembly in the user profile's runtime store at:</span></span>

```
<DRIVE>\Users\<USER>\.dotnet\store\x64\netcoreapp2.0\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\netcoreapp2.0\
```

<span data-ttu-id="7b90e-144">Para usarlo de manera global, colóquelo en el almacenamiento en tiempo de ejecución de la instalación de .NET Core:</span><span class="sxs-lookup"><span data-stu-id="7b90e-144">For global use, place the assembly in the .NET Core installation's runtime store:</span></span>

```
<DRIVE>\Program Files\dotnet\store\x64\netcoreapp2.0\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\netcoreapp2.0\
```

<span data-ttu-id="7b90e-145">Si el ensamblado se implementa en el almacenamiento en tiempo de ejecución, puede que también se implemente el archivo de símbolos, si bien esto no es imprescindible para que la mejora funcione.</span><span class="sxs-lookup"><span data-stu-id="7b90e-145">When deploying the assembly to the runtime store, the symbols file may be deployed as well but isn't required for the enhancement to work.</span></span>

<span data-ttu-id="7b90e-146">**Colocar el archivo de dependencias**</span><span class="sxs-lookup"><span data-stu-id="7b90e-146">**Place the dependencies file**</span></span>

<span data-ttu-id="7b90e-147">El archivo *\*.deps.json* de la implementación debe estar en una ubicación accesible.</span><span class="sxs-lookup"><span data-stu-id="7b90e-147">The implementation's *\*.deps.json* file must be in an accessible location.</span></span>

<span data-ttu-id="7b90e-148">Para usarlo individualmente con cada usuario, coloque el archivo en la carpeta `additonalDeps` de la configuración `.dotnet` del perfil de usuario:</span><span class="sxs-lookup"><span data-stu-id="7b90e-148">For per-user use, place the file in the `additonalDeps` folder of the user profile's `.dotnet` settings:</span></span> 

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\
```

<span data-ttu-id="7b90e-149">Para usarlo de manera global, colóquelo en la carpeta `additonalDeps` de la instalación de .NET Core:</span><span class="sxs-lookup"><span data-stu-id="7b90e-149">For global use, place the file in the `additonalDeps` folder of the .NET Core installation:</span></span>

```
<DRIVE>\Program Files\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\
```

<span data-ttu-id="7b90e-150">La versión `2.0.0` es la versión del tiempo de ejecución compartido que la aplicación de destino usa.</span><span class="sxs-lookup"><span data-stu-id="7b90e-150">Note the version, `2.0.0`, reflects the version of the shared runtime that the target app uses.</span></span> <span data-ttu-id="7b90e-151">El tiempo de ejecución compartido se muestra en el archivo *\*.runtimeconfig.json*.</span><span class="sxs-lookup"><span data-stu-id="7b90e-151">The shared runtime is shown in the *\*.runtimeconfig.json* file.</span></span> <span data-ttu-id="7b90e-152">En la aplicación de ejemplo, el tiempo de ejecución compartido se especifica en el archivo *HostingStartupSample.runtimeconfig.json*.</span><span class="sxs-lookup"><span data-stu-id="7b90e-152">In the sample app, the shared runtime is specified in the *HostingStartupSample.runtimeconfig.json* file.</span></span>

<span data-ttu-id="7b90e-153">**Establecer las variables de entorno**</span><span class="sxs-lookup"><span data-stu-id="7b90e-153">**Set environment variables**</span></span>

<span data-ttu-id="7b90e-154">Establezca las siguientes variables de entorno en el contexto de la aplicación que usa la mejora.</span><span class="sxs-lookup"><span data-stu-id="7b90e-154">Set the following environment variables in the context of the app that uses the enhancement.</span></span>

<span data-ttu-id="7b90e-155">ASPNETCORE\_HOSTINGSTARTUPASSEMBLIES</span><span class="sxs-lookup"><span data-stu-id="7b90e-155">ASPNETCORE\_HOSTINGSTARTUPASSEMBLIES</span></span>

<span data-ttu-id="7b90e-156">Solo se examinan los ensamblados de inicio de hospedaje en busca de `HostingStartupAttribute`.</span><span class="sxs-lookup"><span data-stu-id="7b90e-156">Only hosting startup assemblies are scanned for the `HostingStartupAttribute`.</span></span> <span data-ttu-id="7b90e-157">El nombre del ensamblado de la implementación se proporciona en esta variable de entorno.</span><span class="sxs-lookup"><span data-stu-id="7b90e-157">The assembly name of the implementation is provided in this environment variable.</span></span> <span data-ttu-id="7b90e-158">En la aplicación de ejemplo, este valor se establece en `StartupDiagnostics`.</span><span class="sxs-lookup"><span data-stu-id="7b90e-158">The sample app sets this value to `StartupDiagnostics`.</span></span>

<span data-ttu-id="7b90e-159">Dicho valor también se puede establecer usando la configuración de host de [ensamblados de inicio de hospedaje](xref:fundamentals/host/web-host#hosting-startup-assemblies).</span><span class="sxs-lookup"><span data-stu-id="7b90e-159">The value can also be set using the [Hosting Startup Assemblies](xref:fundamentals/host/web-host#hosting-startup-assemblies) host configuration setting.</span></span>

<span data-ttu-id="7b90e-160">DOTNET\_ADDITIONAL\_DEPS</span><span class="sxs-lookup"><span data-stu-id="7b90e-160">DOTNET\_ADDITIONAL\_DEPS</span></span>

<span data-ttu-id="7b90e-161">Esta es la ubicación del archivo *\*.deps.json* de la implementación.</span><span class="sxs-lookup"><span data-stu-id="7b90e-161">The location of the implementation's *\*.deps.json* file.</span></span>

<span data-ttu-id="7b90e-162">Si el archivo está en la carpeta *.dotnet* del perfil de usuario para usarlo individualmente con cada usuario:</span><span class="sxs-lookup"><span data-stu-id="7b90e-162">If the file is placed in the user profile's *.dotnet* folder for per-user use:</span></span>

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\
```

<span data-ttu-id="7b90e-163">Si el archivo está en la instalación de .NET Core para usarlo de manera global, indique la ruta de acceso completa al archivo:</span><span class="sxs-lookup"><span data-stu-id="7b90e-163">If the file is placed in the .NET Core installation for global use, provide the full path to the file:</span></span>

```
<DRIVE>\Program Files\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\<ENHANCEMENT_ASSEMBLY_NAME>.deps.json
```

<span data-ttu-id="7b90e-164">En la aplicación de ejemplo, este valor se establece en:</span><span class="sxs-lookup"><span data-stu-id="7b90e-164">The sample app sets this value to:</span></span>

```
%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\
```

<span data-ttu-id="7b90e-165">Para obtener ejemplos de cómo establecer variables de entorno en distintos sistemas operativos, vea [Uso de varios entornos](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="7b90e-165">For examples of how to set environment variables for various operating systems, see [Use multiple environments](xref:fundamentals/environments).</span></span>

## <a name="sample-app"></a><span data-ttu-id="7b90e-166">Aplicación de ejemplo</span><span class="sxs-lookup"><span data-stu-id="7b90e-166">Sample app</span></span>

<span data-ttu-id="7b90e-167">La [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/platform-specific-configuration/sample/) ([cómo descargar](xref:tutorials/index#how-to-download-a-sample)) usa `IHostingStartup` para crear una herramienta de diagnóstico.</span><span class="sxs-lookup"><span data-stu-id="7b90e-167">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/platform-specific-configuration/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)) uses `IHostingStartup` to create a diagnostics tool.</span></span> <span data-ttu-id="7b90e-168">Esta herramienta agrega dos middlewares a la aplicación (mientras se inicia) que proporcionan información de diagnóstico:</span><span class="sxs-lookup"><span data-stu-id="7b90e-168">The tool adds two middlewares to the app at startup that provide diagnostic information:</span></span>

* <span data-ttu-id="7b90e-169">Servicios registrados</span><span class="sxs-lookup"><span data-stu-id="7b90e-169">Registered services</span></span>
* <span data-ttu-id="7b90e-170">Dirección: esquema, host, ruta de acceso base, ruta de acceso, cadena de consulta</span><span class="sxs-lookup"><span data-stu-id="7b90e-170">Address: scheme, host, path base, path, query string</span></span>
* <span data-ttu-id="7b90e-171">Conexión: dirección IP remota, puerto remoto, dirección IP local, puerto local, certificado de cliente</span><span class="sxs-lookup"><span data-stu-id="7b90e-171">Connection: remote IP, remote port, local IP, local port, client certificate</span></span>
* <span data-ttu-id="7b90e-172">Encabezados de solicitud</span><span class="sxs-lookup"><span data-stu-id="7b90e-172">Request headers</span></span>
* <span data-ttu-id="7b90e-173">Variables de entorno</span><span class="sxs-lookup"><span data-stu-id="7b90e-173">Environment variables</span></span>

<span data-ttu-id="7b90e-174">Para ejecutar el ejemplo:</span><span class="sxs-lookup"><span data-stu-id="7b90e-174">To run the sample:</span></span>

1. <span data-ttu-id="7b90e-175">El proyecto de diagnóstico de inicio usa [PowerShell](/powershell/scripting/powershell-scripting) para modificar el archivo *StartupDiagnostics.deps.json*.</span><span class="sxs-lookup"><span data-stu-id="7b90e-175">The Startup Diagnostic project uses [PowerShell](/powershell/scripting/powershell-scripting) to modify its *StartupDiagnostics.deps.json* file.</span></span> <span data-ttu-id="7b90e-176">PowerShell se instala de forma predeterminada en cualquier sistema operativo Windows a partir de Windows 7 SP1 y Windows Server 2008 R2 SP1.</span><span class="sxs-lookup"><span data-stu-id="7b90e-176">PowerShell is installed by default on Windows OS starting with Windows 7 SP1 and Windows Server 2008 R2 SP1.</span></span> <span data-ttu-id="7b90e-177">Para obtener PowerShell en otras plataformas, vea [Instalación de Windows PowerShell](/powershell/scripting/setup/installing-windows-powershell).</span><span class="sxs-lookup"><span data-stu-id="7b90e-177">To obtain PowerShell on other platforms, see [Installing Windows PowerShell](/powershell/scripting/setup/installing-windows-powershell).</span></span>
2. <span data-ttu-id="7b90e-178">Compile el proyecto de diagnóstico de inicio.</span><span class="sxs-lookup"><span data-stu-id="7b90e-178">Build the Startup Diagnostic project.</span></span> <span data-ttu-id="7b90e-179">Un destino de compilación en el archivo de proyecto hace lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="7b90e-179">A build target in the project file:</span></span>
   * <span data-ttu-id="7b90e-180">Mueve el ensamblado y los archivos de símbolos al almacenamiento en tiempo de ejecución del perfil de usuario.</span><span class="sxs-lookup"><span data-stu-id="7b90e-180">Moves the assembly and symbols files to the user profile's runtime store.</span></span>
   * <span data-ttu-id="7b90e-181">Activa el script de PowerShell para modificar el archivo *StartupDiagnostics.deps.json*.</span><span class="sxs-lookup"><span data-stu-id="7b90e-181">Triggers the PowerShell script to modify the *StartupDiagnostics.deps.json* file.</span></span>
   * <span data-ttu-id="7b90e-182">Mueve el archivo *StartupDiagnostics.deps.json* a la carpeta `additionalDeps` del perfil de usuario.</span><span class="sxs-lookup"><span data-stu-id="7b90e-182">Moves the *StartupDiagnostics.deps.json* file to the user profile's `additionalDeps` folder.</span></span>
3. <span data-ttu-id="7b90e-183">Establezca las variables de entorno:</span><span class="sxs-lookup"><span data-stu-id="7b90e-183">Set the environment variables:</span></span>
    * <span data-ttu-id="7b90e-184">`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`: `StartupDiagnostics`</span><span class="sxs-lookup"><span data-stu-id="7b90e-184">`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`: `StartupDiagnostics`</span></span>
    * <span data-ttu-id="7b90e-185">`DOTNET_ADDITIONAL_DEPS`: `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`</span><span class="sxs-lookup"><span data-stu-id="7b90e-185">`DOTNET_ADDITIONAL_DEPS`: `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`</span></span>
4. <span data-ttu-id="7b90e-186">Ejecute la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="7b90e-186">Run the sample app.</span></span>
5. <span data-ttu-id="7b90e-187">Solicite al punto de conexión `/services` ver los servicios registrados de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7b90e-187">Request the `/services` endpoint to see the app's registered services.</span></span> <span data-ttu-id="7b90e-188">Solicite al punto de conexión `/diag` ver la información de diagnóstico.</span><span class="sxs-lookup"><span data-stu-id="7b90e-188">Request the `/diag` endpoint to see the diagnostic information.</span></span>
