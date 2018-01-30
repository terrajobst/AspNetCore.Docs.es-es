---
title: "Agregar características de la aplicación desde un ensamblado externo con IHostingStartup en ASP.NET Core"
author: guardrex
description: "Descubra cómo agregar características a una aplicación de ASP.NET Core desde un ensamblado externo con una implementación de IHostingStartup."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 12/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/ihostingstartup
ms.openlocfilehash: bd2446d6133e0c06dc14509271c2d17be4c95b63
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
# <a name="add-app-features-from-an-external-assembly-using-ihostingstartup-in-aspnet-core"></a><span data-ttu-id="637cd-103">Agregar características de la aplicación desde un ensamblado externo con IHostingStartup en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="637cd-103">Add app features from an external assembly using IHostingStartup in ASP.NET Core</span></span>

<span data-ttu-id="637cd-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="637cd-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="637cd-105">Un [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) implementación permite agregar características a una aplicación durante el inicio desde fuera de la aplicación `Startup` clase.</span><span class="sxs-lookup"><span data-stu-id="637cd-105">An [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) implementation allows adding features to an app at startup from outside of the app's `Startup` class.</span></span> <span data-ttu-id="637cd-106">Por ejemplo, puede utilizar una biblioteca de herramientas externas un `IHostingStartup` implementación para proporcionar servicios a una aplicación o proveedores de configuración adicionales.</span><span class="sxs-lookup"><span data-stu-id="637cd-106">For example, an external tooling library can use an `IHostingStartup` implementation to provide additional configuration providers or services to an app.</span></span> <span data-ttu-id="637cd-107">`IHostingStartup`*está disponible en el núcleo de ASP.NET 2.0 y versiones posteriores.*</span><span class="sxs-lookup"><span data-stu-id="637cd-107">`IHostingStartup` *is available in ASP.NET Core 2.0 and later.*</span></span>

<span data-ttu-id="637cd-108">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/ihostingstartup/sample/) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="637cd-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/ihostingstartup/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="discover-loaded-hosting-startup-assemblies"></a><span data-ttu-id="637cd-109">Detectar los ensamblados cargados de hospedaje inicio</span><span class="sxs-lookup"><span data-stu-id="637cd-109">Discover loaded hosting startup assemblies</span></span>

<span data-ttu-id="637cd-110">Para detectar hospedaje ensamblados inicio cargados por la aplicación o bibliotecas, habilitar el registro y compruebe los registros de aplicación.</span><span class="sxs-lookup"><span data-stu-id="637cd-110">To discover hosting startup assemblies loaded by the app or by libraries, enable logging and check the application logs.</span></span> <span data-ttu-id="637cd-111">Se registran los errores que se producen al cargar los ensamblados.</span><span class="sxs-lookup"><span data-stu-id="637cd-111">Errors that occur when loading assemblies are logged.</span></span> <span data-ttu-id="637cd-112">Cargar ensamblados de inicio de hospedaje se registran en el nivel de depuración y se registran todos los errores.</span><span class="sxs-lookup"><span data-stu-id="637cd-112">Loaded hosting startup assemblies are logged at the Debug level, and all errors are logged.</span></span>

<span data-ttu-id="637cd-113">Las lecturas de la aplicación de ejemplo la la [HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey) en un `string` de matriz y se muestra el resultado en la página de índice de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="637cd-113">The sample app reads the the [HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey) into a `string` array and displays the result in the app's Index page:</span></span>

[!code-csharp[Main](ihostingstartup/sample/HostingStartupSample/Pages/Index.cshtml.cs?name=snippet1&highlight=14-16)]

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a><span data-ttu-id="637cd-114">Deshabilitar la carga automática de hospedaje de ensamblados de inicio</span><span class="sxs-lookup"><span data-stu-id="637cd-114">Disable automatic loading of hosting startup assemblies</span></span>

<span data-ttu-id="637cd-115">Hay dos formas de deshabilitar la carga automática de hospedaje de ensamblados de inicio:</span><span class="sxs-lookup"><span data-stu-id="637cd-115">There are two ways to disable the automatic loading of hosting startup assemblies:</span></span>

* <span data-ttu-id="637cd-116">Establecer el [evitar el inicio de hospedaje](xref:fundamentals/hosting#prevent-hosting-startup) de configuración de host.</span><span class="sxs-lookup"><span data-stu-id="637cd-116">Set the [Prevent Hosting Startup](xref:fundamentals/hosting#prevent-hosting-startup) host configuration setting.</span></span>
* <span data-ttu-id="637cd-117">Establecer el `ASPNETCORE_preventHostingStartup` variable de entorno.</span><span class="sxs-lookup"><span data-stu-id="637cd-117">Set the `ASPNETCORE_preventHostingStartup` environment variable.</span></span>

<span data-ttu-id="637cd-118">Cuando la configuración del host o la variable de entorno se establece en `true` o `1`, hospedaje ensamblados de inicio no se carguen automáticamente.</span><span class="sxs-lookup"><span data-stu-id="637cd-118">When either the host setting or the environment variable is set to `true` or `1`, hosting startup assemblies aren't automatically loaded.</span></span> <span data-ttu-id="637cd-119">Si ambas están establecidas, la configuración del host controla el comportamiento.</span><span class="sxs-lookup"><span data-stu-id="637cd-119">If both are set, the host setting controls the behavior.</span></span>

<span data-ttu-id="637cd-120">Deshabilitar hospedaje ensamblados de inicio mediante la variable de entorno o configuración de host deshabilita globalmente y puede deshabilitar varias características de una aplicación.</span><span class="sxs-lookup"><span data-stu-id="637cd-120">Disabling hosting startup assemblies using the host setting or environment variable disables them globally and may disable several features of an app.</span></span> <span data-ttu-id="637cd-121">No es posible actualmente deshabilitar de forma selectiva un ensamblado de inicio hospedado agregado una biblioteca a menos que la biblioteca ofrece su propia opción de configuración.</span><span class="sxs-lookup"><span data-stu-id="637cd-121">It isn't currently possible to selectively disable a hosting startup assembly added by a library unless the library offers its own configuration option.</span></span> <span data-ttu-id="637cd-122">Una versión futura le ofrece la capacidad de deshabilitar de forma selectiva hospedaje ensamblados de inicio (consulte [GitHub emitir aspnet/hospedaje #1243](https://github.com/aspnet/Hosting/pull/1243)).</span><span class="sxs-lookup"><span data-stu-id="637cd-122">A future release will offer the ability to selectively disable hosting startup assemblies (see [GitHub issue aspnet/Hosting #1243](https://github.com/aspnet/Hosting/pull/1243)).</span></span>

## <a name="implement-ihostingstartup-features"></a><span data-ttu-id="637cd-123">Implementar características IHostingStartup</span><span class="sxs-lookup"><span data-stu-id="637cd-123">Implement IHostingStartup features</span></span>

### <a name="create-the-assembly"></a><span data-ttu-id="637cd-124">Crear el ensamblado</span><span class="sxs-lookup"><span data-stu-id="637cd-124">Create the assembly</span></span>

<span data-ttu-id="637cd-125">Un `IHostingStartup` característica se implementa como un ensamblado basado en una aplicación de consola sin un punto de entrada.</span><span class="sxs-lookup"><span data-stu-id="637cd-125">An `IHostingStartup` feature is deployed as an assembly based on a console app without an entry point.</span></span> <span data-ttu-id="637cd-126">Las referencias de ensamblado del [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) paquete:</span><span class="sxs-lookup"><span data-stu-id="637cd-126">The assembly references the [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) package:</span></span>

[!code-xml[Main](ihostingstartup/snapshot_sample/StartupFeature.csproj)]

<span data-ttu-id="637cd-127">A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) atributo identifica una clase como una implementación de `IHostingStartup` de carga y ejecución al compilar el [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost).</span><span class="sxs-lookup"><span data-stu-id="637cd-127">A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) attribute identifies a class as an implementation of `IHostingStartup` for loading and execution when building the [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost).</span></span> <span data-ttu-id="637cd-128">En el ejemplo siguiente, el espacio de nombres es `StartupFeature`, y es la clase `StartupFeatureHostingStartup`:</span><span class="sxs-lookup"><span data-stu-id="637cd-128">In the following example, the namespace is `StartupFeature`, and the class is `StartupFeatureHostingStartup`:</span></span>

[!code-csharp[Main](ihostingstartup/snapshot_sample/StartupFeature.cs?name=snippet1)]

<span data-ttu-id="637cd-129">Una clase que implementa `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="637cd-129">A class implements `IHostingStartup`.</span></span> <span data-ttu-id="637cd-130">La clase [configurar](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) método usa un [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) para agregar características a una aplicación:</span><span class="sxs-lookup"><span data-stu-id="637cd-130">The class's [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) method uses an [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) to add features to an app:</span></span>

[!code-csharp[Main](ihostingstartup/snapshot_sample/StartupFeature.cs?name=snippet2&highlight=3,5)]

<span data-ttu-id="637cd-131">Cuando se crea un `IHostingStartup` del proyecto, el archivo de dependencias (*\*. deps.json*) establece el `runtime` ubicación del ensamblado en el *bin* carpeta:</span><span class="sxs-lookup"><span data-stu-id="637cd-131">When building an `IHostingStartup` project, the dependencies file (*\*.deps.json*) sets the `runtime` location of the assembly to the *bin* folder:</span></span>

[!code-json[Main](ihostingstartup/snapshot_sample/StartupFeature1.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="637cd-132">Se muestra solo una parte del archivo.</span><span class="sxs-lookup"><span data-stu-id="637cd-132">Only part of the file is shown.</span></span> <span data-ttu-id="637cd-133">Es el nombre del ensamblado en el ejemplo `StartupFeature`.</span><span class="sxs-lookup"><span data-stu-id="637cd-133">The assembly name in the example is `StartupFeature`.</span></span>

### <a name="update-the-dependencies-file"></a><span data-ttu-id="637cd-134">Actualizar el archivo de dependencias</span><span class="sxs-lookup"><span data-stu-id="637cd-134">Update the dependencies file</span></span>

<span data-ttu-id="637cd-135">La ubicación en tiempo de ejecución se especifica en el  *\*. deps.json* archivo.</span><span class="sxs-lookup"><span data-stu-id="637cd-135">The runtime location is specified in the *\*.deps.json* file.</span></span> <span data-ttu-id="637cd-136">Activa la característica, la `runtime` elemento debe especificar la ubicación del ensamblado en tiempo de ejecución de la característica.</span><span class="sxs-lookup"><span data-stu-id="637cd-136">To active the feature, the `runtime` element must specify the location of the feature's runtime assembly.</span></span> <span data-ttu-id="637cd-137">Prefijo del `runtime` ubicación con `lib/netcoreapp2.0/`:</span><span class="sxs-lookup"><span data-stu-id="637cd-137">Prefix the `runtime` location with `lib/netcoreapp2.0/`:</span></span>

[!code-json[Main](ihostingstartup/snapshot_sample/StartupFeature2.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="637cd-138">En la aplicación de ejemplo, la modificación de la  *\*. deps.json* archivo se realiza mediante un [PowerShell](/powershell/scripting/powershell-scripting) secuencia de comandos.</span><span class="sxs-lookup"><span data-stu-id="637cd-138">In the sample app, modification of the *\*.deps.json* file is performed by a [PowerShell](/powershell/scripting/powershell-scripting) script.</span></span> <span data-ttu-id="637cd-139">El script de PowerShell se activa automáticamente mediante un destino de compilación en el archivo de proyecto.</span><span class="sxs-lookup"><span data-stu-id="637cd-139">The PowerShell script is automatically triggered by a build target in the project file.</span></span>

### <a name="feature-activation"></a><span data-ttu-id="637cd-140">Activación de características</span><span class="sxs-lookup"><span data-stu-id="637cd-140">Feature activation</span></span>

<span data-ttu-id="637cd-141">**Coloque el archivo de ensamblado**</span><span class="sxs-lookup"><span data-stu-id="637cd-141">**Place the assembly file**</span></span>

<span data-ttu-id="637cd-142">El `IHostingStartup` debe ser el archivo de ensamblado de la implementación *bin*-implementados en la aplicación o se colocan en el [en tiempo de ejecución almacén](/dotnet/core/deploying/runtime-store):</span><span class="sxs-lookup"><span data-stu-id="637cd-142">The `IHostingStartup` implementation's assembly file must be *bin*-deployed in the app or placed in the [runtime store](/dotnet/core/deploying/runtime-store):</span></span>

<span data-ttu-id="637cd-143">Para su uso por usuario, coloque el ensamblado en el almacén de tiempo de ejecución del perfil de usuario en:</span><span class="sxs-lookup"><span data-stu-id="637cd-143">For per-user use, place the assembly in the user profile's runtime store at:</span></span>

```
<DRIVE>\Users\<USER>\.dotnet\store\x64\netcoreapp2.0\<FEATURE_ASSEMBLY_NAME>\<FEATURE_VERSION>\lib\netcoreapp2.0\
```

<span data-ttu-id="637cd-144">Para su uso global, coloque el ensamblado en el almacén de tiempo de ejecución de la instalación de .NET Core:</span><span class="sxs-lookup"><span data-stu-id="637cd-144">For global use, place the assembly in the .NET Core installation's runtime store:</span></span>

```
<DRIVE>\Program Files\dotnet\store\x64\netcoreapp2.0\<FEATURE_ASSEMBLY_NAME>\<FEATURE_VERSION>\lib\netcoreapp2.0\
```

<span data-ttu-id="637cd-145">Al implementar el ensamblado en el almacén de tiempo de ejecución, el archivo de símbolos también se puede implementar, pero no es necesario para que funcione la característica.</span><span class="sxs-lookup"><span data-stu-id="637cd-145">When deploying the assembly to the runtime store, the symbols file may be deployed as well but isn't required for the feature to work.</span></span>

<span data-ttu-id="637cd-146">**Coloque el archivo de dependencias**</span><span class="sxs-lookup"><span data-stu-id="637cd-146">**Place the dependencies file**</span></span>

<span data-ttu-id="637cd-147">La implementación  *\*. deps.json* archivo debe estar en una ubicación accesible.</span><span class="sxs-lookup"><span data-stu-id="637cd-147">The implementation's *\*.deps.json* file must be in an accessible location.</span></span>

<span data-ttu-id="637cd-148">Para su uso por usuario, coloque el archivo en el `additonalDeps` carpeta del perfil de usuario `.dotnet` configuración:</span><span class="sxs-lookup"><span data-stu-id="637cd-148">For per-user use, place the file in the `additonalDeps` folder of the user profile's `.dotnet` settings:</span></span> 

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\<FEATURE_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\
```

<span data-ttu-id="637cd-149">Para su uso global, coloque el archivo en el `additonalDeps` carpeta de la instalación de .NET Core:</span><span class="sxs-lookup"><span data-stu-id="637cd-149">For global use, place the file in the `additonalDeps` folder of the .NET Core installation:</span></span>

```
<DRIVE>\Program Files\dotnet\additionalDeps\<FEATURE_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\
```

<span data-ttu-id="637cd-150">Tenga en cuenta la versión `2.0.0`, refleja la versión del runtime compartida que utiliza la aplicación de destino.</span><span class="sxs-lookup"><span data-stu-id="637cd-150">Note the version, `2.0.0`, reflects the version of the shared runtime that the target app uses.</span></span> <span data-ttu-id="637cd-151">El tiempo de ejecución compartido se muestra en el  *\*. runtimeconfig.json* archivo.</span><span class="sxs-lookup"><span data-stu-id="637cd-151">The shared runtime is shown in the *\*.runtimeconfig.json* file.</span></span> <span data-ttu-id="637cd-152">En la aplicación de ejemplo, el tiempo de ejecución compartido se especifica en el *HostingStartupSample.runtimeconfig.json* archivo.</span><span class="sxs-lookup"><span data-stu-id="637cd-152">In the sample app, the shared runtime is specified in the *HostingStartupSample.runtimeconfig.json* file.</span></span>

<span data-ttu-id="637cd-153">**Establecer variables de entorno**</span><span class="sxs-lookup"><span data-stu-id="637cd-153">**Set environment variables**</span></span>

<span data-ttu-id="637cd-154">Establecer las siguientes variables de entorno en el contexto de la aplicación que utiliza la característica.</span><span class="sxs-lookup"><span data-stu-id="637cd-154">Set the following environment variables in the context of the app that uses the feature.</span></span>

<span data-ttu-id="637cd-155">ASPNETCORE\_HOSTINGSTARTUPASSEMBLIES</span><span class="sxs-lookup"><span data-stu-id="637cd-155">ASPNETCORE\_HOSTINGSTARTUPASSEMBLIES</span></span>

<span data-ttu-id="637cd-156">Ensamblados de inicio de hospedaje solo se examinan la `HostingStartupAttribute`.</span><span class="sxs-lookup"><span data-stu-id="637cd-156">Only hosting startup assemblies are scanned for the `HostingStartupAttribute`.</span></span> <span data-ttu-id="637cd-157">El nombre del ensamblado de la implementación se proporciona en esta variable de entorno.</span><span class="sxs-lookup"><span data-stu-id="637cd-157">The assembly name of the implementation is provided in this environment variable.</span></span> <span data-ttu-id="637cd-158">La aplicación de ejemplo establece este valor en `StartupDiagnostics`.</span><span class="sxs-lookup"><span data-stu-id="637cd-158">The sample app sets this value to `StartupDiagnostics`.</span></span>

<span data-ttu-id="637cd-159">El valor también puede establecerse utilizando la [hospedaje inicio ensamblados](xref:fundamentals/hosting#hosting-startup-assemblies) de configuración de host.</span><span class="sxs-lookup"><span data-stu-id="637cd-159">The value can also be set using the [Hosting Startup Assemblies](xref:fundamentals/hosting#hosting-startup-assemblies) host configuration setting.</span></span>

<span data-ttu-id="637cd-160">DOTNET\_ADICIONALES\_DEPÓS</span><span class="sxs-lookup"><span data-stu-id="637cd-160">DOTNET\_ADDITIONAL\_DEPS</span></span>

<span data-ttu-id="637cd-161">La ubicación de la implementación  *\*. deps.json* archivo.</span><span class="sxs-lookup"><span data-stu-id="637cd-161">The location of the implementation's *\*.deps.json* file.</span></span>

<span data-ttu-id="637cd-162">Si el archivo se encuentra en el perfil de usuario *.dotnet* carpeta para su uso por usuario:</span><span class="sxs-lookup"><span data-stu-id="637cd-162">If the file is placed in the user profile's *.dotnet* folder for per-user use:</span></span>

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\
```

<span data-ttu-id="637cd-163">Si el archivo se encuentra en la instalación de .NET Core para su uso global, proporcione la ruta de acceso completa al archivo:</span><span class="sxs-lookup"><span data-stu-id="637cd-163">If the file is placed in the .NET Core installation for global use, provide the full path to the file:</span></span>

```
<DRIVE>\Program Files\dotnet\additionalDeps\<FEATURE_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\<FEATURE_ASSEMBLY_NAME>.deps.json
```

<span data-ttu-id="637cd-164">La aplicación de ejemplo, establece este valor en:</span><span class="sxs-lookup"><span data-stu-id="637cd-164">The sample app sets this value to:</span></span>

```
%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\
```

<span data-ttu-id="637cd-165">Para obtener ejemplos de cómo establecer variables de entorno para varios sistemas operativos, consulte [trabajar con varios entornos](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="637cd-165">For examples of how to set environment variables for various operating systems, see [Working with multiple environments](xref:fundamentals/environments).</span></span>

## <a name="sample-app"></a><span data-ttu-id="637cd-166">Aplicación de ejemplo</span><span class="sxs-lookup"><span data-stu-id="637cd-166">Sample app</span></span>

<span data-ttu-id="637cd-167">El [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/ihostingstartup/sample/) ([cómo descargar](xref:tutorials/index#how-to-download-a-sample)) usa `IHostingStartup` para crear una herramienta de diagnóstico.</span><span class="sxs-lookup"><span data-stu-id="637cd-167">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/ihostingstartup/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)) uses `IHostingStartup` to create a diagnostics tool.</span></span> <span data-ttu-id="637cd-168">La herramienta agrega dos middlewares a la aplicación en el inicio que proporcionan información de diagnóstico:</span><span class="sxs-lookup"><span data-stu-id="637cd-168">The tool adds two middlewares to the app at startup that provide diagnostic information:</span></span>

* <span data-ttu-id="637cd-169">Servicios registrados</span><span class="sxs-lookup"><span data-stu-id="637cd-169">Registered services</span></span>
* <span data-ttu-id="637cd-170">Dirección: esquema, host, ruta de acceso base, ruta de acceso, cadena de consulta</span><span class="sxs-lookup"><span data-stu-id="637cd-170">Address: scheme, host, path base, path, query string</span></span>
* <span data-ttu-id="637cd-171">Conexión: IP remotas, puerto remoto, IP local, puerto local, el certificado de cliente</span><span class="sxs-lookup"><span data-stu-id="637cd-171">Connection: remote IP, remote port, local IP, local port, client certificate</span></span>
* <span data-ttu-id="637cd-172">Encabezados de solicitud</span><span class="sxs-lookup"><span data-stu-id="637cd-172">Request headers</span></span>
* <span data-ttu-id="637cd-173">Variables de entorno</span><span class="sxs-lookup"><span data-stu-id="637cd-173">Environment variables</span></span>

<span data-ttu-id="637cd-174">Para ejecutar el ejemplo:</span><span class="sxs-lookup"><span data-stu-id="637cd-174">To run the sample:</span></span>

1. <span data-ttu-id="637cd-175">El proyecto de inicio diagnóstico utiliza [PowerShell](/powershell/scripting/powershell-scripting) para modificar su *StartupDiagnostics.deps.json* archivo.</span><span class="sxs-lookup"><span data-stu-id="637cd-175">The Startup Diagnostic project uses [PowerShell](/powershell/scripting/powershell-scripting) to modify its *StartupDiagnostics.deps.json* file.</span></span> <span data-ttu-id="637cd-176">PowerShell se instala de forma predeterminada en el sistema operativo de Windows a partir de Windows 7 SP1 y Windows Server 2008 R2 SP1.</span><span class="sxs-lookup"><span data-stu-id="637cd-176">PowerShell is installed by default on Windows OS starting with Windows 7 SP1 and Windows Server 2008 R2 SP1.</span></span> <span data-ttu-id="637cd-177">Para obtener PowerShell en otras plataformas, vea [instalar Windows PowerShell](/powershell/scripting/setup/installing-windows-powershell).</span><span class="sxs-lookup"><span data-stu-id="637cd-177">To obtain PowerShell on other platforms, see [Installing Windows PowerShell](/powershell/scripting/setup/installing-windows-powershell).</span></span>
2. <span data-ttu-id="637cd-178">Compile el proyecto de inicio diagnóstico.</span><span class="sxs-lookup"><span data-stu-id="637cd-178">Build the Startup Diagnostic project.</span></span> <span data-ttu-id="637cd-179">Un destino de compilación en el archivo de proyecto:</span><span class="sxs-lookup"><span data-stu-id="637cd-179">A build target in the project file:</span></span>
   * <span data-ttu-id="637cd-180">Mueve el ensamblado y los archivos al almacén de tiempo de ejecución del perfil de usuario de símbolos.</span><span class="sxs-lookup"><span data-stu-id="637cd-180">Moves the assembly and symbols files to the user profile's runtime store.</span></span>
   * <span data-ttu-id="637cd-181">Desencadena la secuencia de comandos de PowerShell para modificar el *StartupDiagnostics.deps.json* archivo.</span><span class="sxs-lookup"><span data-stu-id="637cd-181">Triggers the PowerShell script to modify the *StartupDiagnostics.deps.json* file.</span></span>
   * <span data-ttu-id="637cd-182">Mueve el *StartupDiagnostics.deps.json* archivo para el perfil de usuario `additionalDeps` carpeta.</span><span class="sxs-lookup"><span data-stu-id="637cd-182">Moves the *StartupDiagnostics.deps.json* file to the user profile's `additionalDeps` folder.</span></span>
3. <span data-ttu-id="637cd-183">Establecer las variables de entorno:</span><span class="sxs-lookup"><span data-stu-id="637cd-183">Set the environment variables:</span></span>
    * <span data-ttu-id="637cd-184">`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`: `StartupDiagnostics`</span><span class="sxs-lookup"><span data-stu-id="637cd-184">`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`: `StartupDiagnostics`</span></span>
    * <span data-ttu-id="637cd-185">`DOTNET_ADDITIONAL_DEPS`: `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`</span><span class="sxs-lookup"><span data-stu-id="637cd-185">`DOTNET_ADDITIONAL_DEPS`: `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`</span></span>
4. <span data-ttu-id="637cd-186">Ejecute la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="637cd-186">Run the sample app.</span></span>
5. <span data-ttu-id="637cd-187">Solicitar la `/services` extremo para ver la aplicación registra los servicios.</span><span class="sxs-lookup"><span data-stu-id="637cd-187">Request the `/services` endpoint to see the app's registered services.</span></span> <span data-ttu-id="637cd-188">Solicitar la `/diag` punto de conexión para ver la información de diagnóstico.</span><span class="sxs-lookup"><span data-stu-id="637cd-188">Request the `/diag` endpoint to see the diagnostic information.</span></span>
