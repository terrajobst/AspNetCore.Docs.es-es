---
title: Proveedores de archivo en ASP.NET Core
author: guardrex
description: Obtenga información sobre cómo ASP.NET Core abstrae el acceso al sistema de archivos mediante el uso de proveedores de archivos.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/26/2019
uid: fundamentals/file-providers
ms.openlocfilehash: 44c439dce893d486668bf8ac3f20cdf7952c5186
ms.sourcegitcommit: 0774a61a3a6c1412a7da0e7d932dc60c506441fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2019
ms.locfileid: "70059093"
---
# <a name="file-providers-in-aspnet-core"></a><span data-ttu-id="7e4f2-103">Proveedores de archivo en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7e4f2-103">File Providers in ASP.NET Core</span></span>

<span data-ttu-id="7e4f2-104">Por [Steve Smith](https://ardalis.com/) y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="7e4f2-104">By [Steve Smith](https://ardalis.com/) and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="7e4f2-105">ASP.NET Core abstrae el acceso al sistema de archivos mediante el uso de proveedores de archivos.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-105">ASP.NET Core abstracts file system access through the use of File Providers.</span></span> <span data-ttu-id="7e4f2-106">Los proveedores de archivos se usan en el marco de ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="7e4f2-106">File Providers are used throughout the ASP.NET Core framework:</span></span>

* <span data-ttu-id="7e4f2-107">`IWebHostEnvironment` expone la raíz del contenido de la aplicación y la raíz web como tipos `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-107">`IWebHostEnvironment` exposes the app's content root and web root as `IFileProvider` types.</span></span>
* <span data-ttu-id="7e4f2-108">El [middleware de archivos estáticos](xref:fundamentals/static-files) usa proveedores de archivos para buscar archivos estáticos.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-108">[Static File Middleware](xref:fundamentals/static-files) uses File Providers to locate static files.</span></span>
* <span data-ttu-id="7e4f2-109">[Razor](xref:mvc/views/razor) usa proveedores de archivos para localizar páginas y vistas.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-109">[Razor](xref:mvc/views/razor) uses File Providers to locate pages and views.</span></span>
* <span data-ttu-id="7e4f2-110">Las herramientas de .NET Core usan proveedores de archivos y patrones globales para especificar los archivos que deben publicarse.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-110">.NET Core tooling uses File Providers and glob patterns to specify which files should be published.</span></span>

<span data-ttu-id="7e4f2-111">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7e4f2-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="file-provider-interfaces"></a><span data-ttu-id="7e4f2-112">Interfaces de proveedor de archivos</span><span class="sxs-lookup"><span data-stu-id="7e4f2-112">File Provider interfaces</span></span>

<span data-ttu-id="7e4f2-113">La interfaz principal es <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-113">The primary interface is <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span></span> <span data-ttu-id="7e4f2-114">`IFileProvider` expone métodos para:</span><span class="sxs-lookup"><span data-stu-id="7e4f2-114">`IFileProvider` exposes methods to:</span></span>

* <span data-ttu-id="7e4f2-115">Obtenga la información del archivo (<xref:Microsoft.Extensions.FileProviders.IFileInfo>).</span><span class="sxs-lookup"><span data-stu-id="7e4f2-115">Obtain file information (<xref:Microsoft.Extensions.FileProviders.IFileInfo>).</span></span>
* <span data-ttu-id="7e4f2-116">Obtenga la información del directorio (<xref:Microsoft.Extensions.FileProviders.IDirectoryContents>).</span><span class="sxs-lookup"><span data-stu-id="7e4f2-116">Obtain directory information (<xref:Microsoft.Extensions.FileProviders.IDirectoryContents>).</span></span>
* <span data-ttu-id="7e4f2-117">Configure las notificaciones de cambio (mediante <xref:Microsoft.Extensions.Primitives.IChangeToken>).</span><span class="sxs-lookup"><span data-stu-id="7e4f2-117">Set up change notifications (using an <xref:Microsoft.Extensions.Primitives.IChangeToken>).</span></span>

<span data-ttu-id="7e4f2-118">`IFileInfo` proporciona métodos y propiedades para trabajar con archivos:</span><span class="sxs-lookup"><span data-stu-id="7e4f2-118">`IFileInfo` provides methods and properties for working with files:</span></span>

* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Exists>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.IsDirectory>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Name>
* <span data-ttu-id="7e4f2-119"><xref:Microsoft.Extensions.FileProviders.IFileInfo.Length> (en bytes)</span><span class="sxs-lookup"><span data-stu-id="7e4f2-119"><xref:Microsoft.Extensions.FileProviders.IFileInfo.Length> (in bytes)</span></span>
* <span data-ttu-id="7e4f2-120">Fecha <xref:Microsoft.Extensions.FileProviders.IFileInfo.LastModified></span><span class="sxs-lookup"><span data-stu-id="7e4f2-120"><xref:Microsoft.Extensions.FileProviders.IFileInfo.LastModified> date</span></span>

<span data-ttu-id="7e4f2-121">Puede leer del archivo mediante el método [IFileInfo.CreateReadStream](xref:Microsoft.Extensions.FileProviders.IFileInfo.CreateReadStream*).</span><span class="sxs-lookup"><span data-stu-id="7e4f2-121">You can read from the file using the [IFileInfo.CreateReadStream](xref:Microsoft.Extensions.FileProviders.IFileInfo.CreateReadStream*) method.</span></span>

<span data-ttu-id="7e4f2-122">La aplicación de ejemplo muestra cómo configurar un proveedor de archivos en `Startup.ConfigureServices` para su uso en toda la aplicación a través de la [inserción de dependencias](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="7e4f2-122">The sample app demonstrates how to configure a File Provider in `Startup.ConfigureServices` for use throughout the app via [dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="file-provider-implementations"></a><span data-ttu-id="7e4f2-123">Implementaciones del proveedor de archivos</span><span class="sxs-lookup"><span data-stu-id="7e4f2-123">File Provider implementations</span></span>

<span data-ttu-id="7e4f2-124">Hay tres implementaciones de `IFileProvider` disponibles.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-124">Three implementations of `IFileProvider` are available.</span></span>

| <span data-ttu-id="7e4f2-125">Implementación</span><span class="sxs-lookup"><span data-stu-id="7e4f2-125">Implementation</span></span> | <span data-ttu-id="7e4f2-126">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="7e4f2-126">Description</span></span> |
| -------------- | ----------- |
| [<span data-ttu-id="7e4f2-127">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="7e4f2-127">PhysicalFileProvider</span></span>](#physicalfileprovider) | <span data-ttu-id="7e4f2-128">El proveedor físico se utiliza para acceder a los archivos físicos del sistema.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-128">The physical provider is used to access the system's physical files.</span></span> |
| [<span data-ttu-id="7e4f2-129">ManifestEmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="7e4f2-129">ManifestEmbeddedFileProvider</span></span>](#manifestembeddedfileprovider) | <span data-ttu-id="7e4f2-130">El proveedor insertado de manifiestos se utiliza para tener acceder a archivos insertados en ensamblados.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-130">The manifest embedded provider is used to access files embedded in assemblies.</span></span> |
| [<span data-ttu-id="7e4f2-131">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="7e4f2-131">CompositeFileProvider</span></span>](#compositefileprovider) | <span data-ttu-id="7e4f2-132">El proveedor compuesto se utiliza para proporcionar acceso combinado a archivos y directorios de uno o más proveedores.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-132">The composite provider is used to provide combined access to files and directories from one or more other providers.</span></span> |

### <a name="physicalfileprovider"></a><span data-ttu-id="7e4f2-133">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="7e4f2-133">PhysicalFileProvider</span></span>

<span data-ttu-id="7e4f2-134"><xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> proporciona acceso al sistema de archivos físico.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-134">The <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> provides access to the physical file system.</span></span> <span data-ttu-id="7e4f2-135">`PhysicalFileProvider` usa el tipo <xref:System.IO.File?displayProperty=fullName> (para el proveedor físico) y define el ámbito de todas las rutas de acceso a un directorio y sus elementos secundarios.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-135">`PhysicalFileProvider` uses the <xref:System.IO.File?displayProperty=fullName> type (for the physical provider) and scopes all paths to a directory and its children.</span></span> <span data-ttu-id="7e4f2-136">Esta definición de ámbito impide el acceso al sistema de archivos fuera del directorio especificado y sus elementos secundarios.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-136">This scoping prevents access to the file system outside of the specified directory and its children.</span></span> <span data-ttu-id="7e4f2-137">El escenario más común para crear y usar `PhysicalFileProvider` es solicitar `IFileProvider` en un constructor mediante la [inyección de dependencias](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="7e4f2-137">The most common scenario for creating and using a `PhysicalFileProvider` is to request an `IFileProvider` in a constructor through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="7e4f2-138">Al crear una instancia de este proveedor directamente, se requiere una ruta de acceso del directorio, que actúa como la ruta de acceso base para todas las solicitudes realizadas usando el proveedor.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-138">When instantiating this provider directly, a directory path is required and serves as the base path for all requests made using the provider.</span></span>

<span data-ttu-id="7e4f2-139">El código siguiente muestra cómo crear `PhysicalFileProvider` y usarlo para obtener el contenido del directorio y la información del archivo:</span><span class="sxs-lookup"><span data-stu-id="7e4f2-139">The following code shows how to create a `PhysicalFileProvider` and use it to obtain directory contents and file information:</span></span>

```csharp
var provider = new PhysicalFileProvider(applicationRoot);
var contents = provider.GetDirectoryContents(string.Empty);
var fileInfo = provider.GetFileInfo("wwwroot/js/site.js");
```

<span data-ttu-id="7e4f2-140">Tipos del ejemplo anterior:</span><span class="sxs-lookup"><span data-stu-id="7e4f2-140">Types in the preceding example:</span></span>

* <span data-ttu-id="7e4f2-141">`provider` es `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-141">`provider` is an `IFileProvider`.</span></span>
* <span data-ttu-id="7e4f2-142">`contents` es `IDirectoryContents`.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-142">`contents` is an `IDirectoryContents`.</span></span>
* <span data-ttu-id="7e4f2-143">`fileInfo` es `IFileInfo`.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-143">`fileInfo` is an `IFileInfo`.</span></span>

<span data-ttu-id="7e4f2-144">El proveedor de archivos puede usarse para iterar por el directorio especificado por `applicationRoot` o llamar a `GetFileInfo` para obtener información de un archivo.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-144">The File Provider can be used to iterate through the directory specified by `applicationRoot` or call `GetFileInfo` to obtain a file's information.</span></span> <span data-ttu-id="7e4f2-145">El proveedor de archivos no tiene acceso fuera del directorio `applicationRoot`.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-145">The File Provider has no access outside of the `applicationRoot` directory.</span></span>

<span data-ttu-id="7e4f2-146">La aplicación de ejemplo crea el proveedor en la clase `Startup.ConfigureServices` de la aplicación mediante [IHostingEnvironment.ContentRootFileProvider](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootFileProvider):</span><span class="sxs-lookup"><span data-stu-id="7e4f2-146">The sample app creates the provider in the app's `Startup.ConfigureServices` class using [IHostingEnvironment.ContentRootFileProvider](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootFileProvider):</span></span>

```csharp
var physicalProvider = _env.ContentRootFileProvider;
```

### <a name="manifestembeddedfileprovider"></a><span data-ttu-id="7e4f2-147">ManifestEmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="7e4f2-147">ManifestEmbeddedFileProvider</span></span>

<span data-ttu-id="7e4f2-148"><xref:Microsoft.Extensions.FileProviders.ManifestEmbeddedFileProvider> se utiliza para acceder a archivos insertados dentro de ensamblados.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-148">The <xref:Microsoft.Extensions.FileProviders.ManifestEmbeddedFileProvider> is used to access files embedded within assemblies.</span></span> <span data-ttu-id="7e4f2-149">`ManifestEmbeddedFileProvider` utiliza un manifiesto compilado en el ensamblado para reconstruir las rutas de acceso originales de los archivos incrustados.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-149">The `ManifestEmbeddedFileProvider` uses a manifest compiled into the assembly to reconstruct the original paths of the embedded files.</span></span>

<span data-ttu-id="7e4f2-150">Agregue una referencia de paquete al proyecto para el paquete [Microsoft.Extensions.FileProviders.Embedded](https://www.nuget.org/packages/Microsoft.Extensions.FileProviders.Embedded).</span><span class="sxs-lookup"><span data-stu-id="7e4f2-150">Add a package reference to the project for the [Microsoft.Extensions.FileProviders.Embedded](https://www.nuget.org/packages/Microsoft.Extensions.FileProviders.Embedded) package.</span></span>

<span data-ttu-id="7e4f2-151">Para generar un manifiesto de los archivos incrustados, establezca la propiedad `<GenerateEmbeddedFilesManifest>` en `true`.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-151">To generate a manifest of the embedded files, set the `<GenerateEmbeddedFilesManifest>` property to `true`.</span></span> <span data-ttu-id="7e4f2-152">Especifique los archivos que desea incrustar con [\<EmbeddedResource>](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):</span><span class="sxs-lookup"><span data-stu-id="7e4f2-152">Specify the files to embed with [\<EmbeddedResource>](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):</span></span>

[!code-csharp[](file-providers/samples/3.x/FileProviderSample/FileProviderSample.csproj?highlight=6,14)]

<span data-ttu-id="7e4f2-153">Use [patrones globales](#glob-patterns) para especificar uno o varios archivos para incrustar en el ensamblado.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-153">Use [glob patterns](#glob-patterns) to specify one or more files to embed into the assembly.</span></span>

<span data-ttu-id="7e4f2-154">La aplicación de ejemplo crea `ManifestEmbeddedFileProvider` y pasa el ensamblado que se está ejecutando actualmente a su constructor.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-154">The sample app creates an `ManifestEmbeddedFileProvider` and passes the currently executing assembly to its constructor.</span></span>

<span data-ttu-id="7e4f2-155">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="7e4f2-155">*Startup.cs*:</span></span>

```csharp
var manifestEmbeddedProvider = 
    new ManifestEmbeddedFileProvider(Assembly.GetEntryAssembly());
```

<span data-ttu-id="7e4f2-156">Las sobrecargas adicionales le permiten:</span><span class="sxs-lookup"><span data-stu-id="7e4f2-156">Additional overloads allow you to:</span></span>

* <span data-ttu-id="7e4f2-157">Especificar una ruta de acceso de archivo relativa.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-157">Specify a relative file path.</span></span>
* <span data-ttu-id="7e4f2-158">Definir el ámbito de archivos a la fecha de la última modificación.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-158">Scope files to a last modified date.</span></span>
* <span data-ttu-id="7e4f2-159">Asignar nombre al recurso incrustado que contiene el manifiesto del archivo incrustado.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-159">Name the embedded resource containing the embedded file manifest.</span></span>

| <span data-ttu-id="7e4f2-160">Sobrecarga</span><span class="sxs-lookup"><span data-stu-id="7e4f2-160">Overload</span></span> | <span data-ttu-id="7e4f2-161">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="7e4f2-161">Description</span></span> |
| -------- | ----------- |
| `ManifestEmbeddedFileProvider(Assembly, String)` | <span data-ttu-id="7e4f2-162">Acepta parámetro de ruta de acceso relativa `root` opcional.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-162">Accepts an optional `root` relative path parameter.</span></span> <span data-ttu-id="7e4f2-163">Especifique `root` para definir el ámbito de las llamadas en <xref:Microsoft.Extensions.FileProviders.IFileProvider.GetDirectoryContents*> en aquellos recursos que se encuentran bajo las rutas de acceso proporcionadas.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-163">Specify the `root` to scope calls to <xref:Microsoft.Extensions.FileProviders.IFileProvider.GetDirectoryContents*> to those resources under the provided path.</span></span> |
| `ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)` | <span data-ttu-id="7e4f2-164">Acepta un parámetro de ruta de acceso relativa `root` opcional y un parámetro de fecha `lastModified` (<xref:System.DateTimeOffset>).</span><span class="sxs-lookup"><span data-stu-id="7e4f2-164">Accepts an optional `root` relative path parameter and a `lastModified` date (<xref:System.DateTimeOffset>) parameter.</span></span> <span data-ttu-id="7e4f2-165">La fecha `lastModified` define el ámbito de la última fecha de modificación para las instancias de <xref:Microsoft.Extensions.FileProviders.IFileInfo> que <xref:Microsoft.Extensions.FileProviders.IFileProvider> devuelve.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-165">The `lastModified` date scopes the last modification date for the <xref:Microsoft.Extensions.FileProviders.IFileInfo> instances returned by the <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span></span> |
| `ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)` | <span data-ttu-id="7e4f2-166">Acepta una ruta de acceso relativa `root` opcional, una fecha `lastModified` y parámetros `manifestName`.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-166">Accepts an optional `root` relative path, `lastModified` date, and `manifestName` parameters.</span></span> <span data-ttu-id="7e4f2-167">`manifestName` representa el nombre del recurso incrustado que contiene el manifiesto.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-167">The `manifestName` represents the name of the embedded resource containing the manifest.</span></span> |

### <a name="compositefileprovider"></a><span data-ttu-id="7e4f2-168">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="7e4f2-168">CompositeFileProvider</span></span>

<span data-ttu-id="7e4f2-169"><xref:Microsoft.Extensions.FileProviders.CompositeFileProvider> combina instancias de `IFileProvider`, y expone una única interfaz para trabajar con archivos de varios proveedores.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-169">The <xref:Microsoft.Extensions.FileProviders.CompositeFileProvider> combines `IFileProvider` instances, exposing a single interface for working with files from multiple providers.</span></span> <span data-ttu-id="7e4f2-170">Al crear `CompositeFileProvider`, se pasan una o varias instancias de `IFileProvider` a su constructor.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-170">When creating the `CompositeFileProvider`, pass one or more `IFileProvider` instances to its constructor.</span></span>

<span data-ttu-id="7e4f2-171">En la aplicación de ejemplo, `PhysicalFileProvider` y `ManifestEmbeddedFileProvider` proporcionan archivos a un `CompositeFileProvider` registrado en el contenedor de servicios de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="7e4f2-171">In the sample app, a `PhysicalFileProvider` and a `ManifestEmbeddedFileProvider` provide files to a `CompositeFileProvider` registered in the app's service container:</span></span>

[!code-csharp[](file-providers/samples/3.x/FileProviderSample/Startup.cs?name=snippet1)]

## <a name="watch-for-changes"></a><span data-ttu-id="7e4f2-172">Observación de cambios</span><span class="sxs-lookup"><span data-stu-id="7e4f2-172">Watch for changes</span></span>

<span data-ttu-id="7e4f2-173">El método [IFileProvider.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) proporciona un escenario para ver uno o más archivos o directorios para detectar cambios.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-173">The [IFileProvider.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) method provides a scenario to watch one or more files or directories for changes.</span></span> <span data-ttu-id="7e4f2-174">`Watch` acepta una cadena de ruta de acceso, que puede usar [patrones globales](#glob-patterns) para especificar varios archivos.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-174">`Watch` accepts a path string, which can use [glob patterns](#glob-patterns) to specify multiple files.</span></span> <span data-ttu-id="7e4f2-175">`Watch` devuelve un valor de <xref:Microsoft.Extensions.Primitives.IChangeToken>.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-175">`Watch` returns an <xref:Microsoft.Extensions.Primitives.IChangeToken>.</span></span> <span data-ttu-id="7e4f2-176">El token de cambio expone:</span><span class="sxs-lookup"><span data-stu-id="7e4f2-176">The change token exposes:</span></span>

* <span data-ttu-id="7e4f2-177"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> &ndash; una propiedad que se puede inspeccionar para determinar si se ha producido un cambio.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-177"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> &ndash; A property that can be inspected to determine if a change has occurred.</span></span>
* <span data-ttu-id="7e4f2-178"><xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*> &ndash; se llama cuando se detectan cambios en la cadena de ruta de acceso especificada.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-178"><xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*> &ndash; Called when changes are detected to the specified path string.</span></span> <span data-ttu-id="7e4f2-179">Cada token de cambio solo llama a su devolución de llamada asociada en respuesta a un único cambio.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-179">Each change token only calls its associated callback in response to a single change.</span></span> <span data-ttu-id="7e4f2-180">Para habilitar la supervisión constante, puede usar <xref:System.Threading.Tasks.TaskCompletionSource`1> (como se muestra a continuación) o volver a crear instancias de `IChangeToken` en respuesta a los cambios.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-180">To enable constant monitoring, use a <xref:System.Threading.Tasks.TaskCompletionSource`1> (shown below) or recreate `IChangeToken` instances in response to changes.</span></span>

<span data-ttu-id="7e4f2-181">En la aplicación de ejemplo, la aplicación de consola *WatchConsole* se configura para mostrar un mensaje cada vez que se modifica un archivo de texto:</span><span class="sxs-lookup"><span data-stu-id="7e4f2-181">In the sample app, the *WatchConsole* console app is configured to display a message whenever a text file is modified:</span></span>

[!code-csharp[](file-providers/samples/3.x/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

<span data-ttu-id="7e4f2-182">Algunos sistemas de archivos, como contenedores de Docker y recursos compartidos de red, no pueden enviar notificaciones de cambio de forma confiable.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-182">Some file systems, such as Docker containers and network shares, may not reliably send change notifications.</span></span> <span data-ttu-id="7e4f2-183">Establezca la variable de entorno `DOTNET_USE_POLLING_FILE_WATCHER` en `1` o `true` para sondear el sistema de archivos en busca de cambios cada cuatro segundos (no configurable).</span><span class="sxs-lookup"><span data-stu-id="7e4f2-183">Set the `DOTNET_USE_POLLING_FILE_WATCHER` environment variable to `1` or `true` to poll the file system for changes every four seconds (not configurable).</span></span>

## <a name="glob-patterns"></a><span data-ttu-id="7e4f2-184">Patrones globales</span><span class="sxs-lookup"><span data-stu-id="7e4f2-184">Glob patterns</span></span>

<span data-ttu-id="7e4f2-185">Las rutas de acceso del sistema de archivos utilizan patrones de caracteres comodín denominados *patrones globales*.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-185">File system paths use wildcard patterns called *glob (or globbing) patterns*.</span></span> <span data-ttu-id="7e4f2-186">Especifique grupos de archivos con estos patrones.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-186">Specify groups of files with these patterns.</span></span> <span data-ttu-id="7e4f2-187">Los dos caracteres comodín son `*` y `**`:</span><span class="sxs-lookup"><span data-stu-id="7e4f2-187">The two wildcard characters are `*` and `**`:</span></span>

**`*`**  
<span data-ttu-id="7e4f2-188">Coincide con cualquier elemento del nivel de carpeta actual, con cualquier nombre de archivo o con cualquier extensión de archivo.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-188">Matches anything at the current folder level, any filename, or any file extension.</span></span> <span data-ttu-id="7e4f2-189">Las coincidencias finalizan con los caracteres `/` y `.` en la ruta de acceso de archivo.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-189">Matches are terminated by `/` and `.` characters in the file path.</span></span>

**`**`**  
<span data-ttu-id="7e4f2-190">Coincide con cualquier elemento de varios niveles de directorios.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-190">Matches anything across multiple directory levels.</span></span> <span data-ttu-id="7e4f2-191">Puede usarse para coincidir de forma recursiva con muchos archivos dentro de una jerarquía de directorios.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-191">Can be used to recursively match many files within a directory hierarchy.</span></span>

<span data-ttu-id="7e4f2-192">**Ejemplos de patrones globales**</span><span class="sxs-lookup"><span data-stu-id="7e4f2-192">**Glob pattern examples**</span></span>

**`directory/file.txt`**  
<span data-ttu-id="7e4f2-193">Coincide con un archivo concreto en un directorio específico.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-193">Matches a specific file in a specific directory.</span></span>

**`directory/*.txt`**  
<span data-ttu-id="7e4f2-194">Coincide con todos los archivos que tengan la extensión *.txt* en un directorio específico.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-194">Matches all files with *.txt* extension in a specific directory.</span></span>

**`directory/*/appsettings.json`**  
<span data-ttu-id="7e4f2-195">Coincide con todos los archivos `appsettings.json` que estén en directorios exactamente un nivel por debajo de la carpeta *directorio*.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-195">Matches all `appsettings.json` files in directories exactly one level below the *directory* folder.</span></span>

**`directory/**/*.txt`**  
<span data-ttu-id="7e4f2-196">Coincide con todos los archivos que tengan la extensión *.txt* y se encuentren en cualquier lugar de la carpeta *directorio*.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-196">Matches all files with *.txt* extension found anywhere under the *directory* folder.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="7e4f2-197">ASP.NET Core abstrae el acceso al sistema de archivos mediante el uso de proveedores de archivos.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-197">ASP.NET Core abstracts file system access through the use of File Providers.</span></span> <span data-ttu-id="7e4f2-198">Los proveedores de archivos se usan en el marco de ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="7e4f2-198">File Providers are used throughout the ASP.NET Core framework:</span></span>

* <span data-ttu-id="7e4f2-199"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment> expone la raíz del contenido de la aplicación y la raíz web como tipos `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-199"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment> exposes the app's content root and web root as `IFileProvider` types.</span></span>
* <span data-ttu-id="7e4f2-200">El [middleware de archivos estáticos](xref:fundamentals/static-files) usa proveedores de archivos para buscar archivos estáticos.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-200">[Static File Middleware](xref:fundamentals/static-files) uses File Providers to locate static files.</span></span>
* <span data-ttu-id="7e4f2-201">[Razor](xref:mvc/views/razor) usa proveedores de archivos para localizar páginas y vistas.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-201">[Razor](xref:mvc/views/razor) uses File Providers to locate pages and views.</span></span>
* <span data-ttu-id="7e4f2-202">Las herramientas de .NET Core usan proveedores de archivos y patrones globales para especificar los archivos que deben publicarse.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-202">.NET Core tooling uses File Providers and glob patterns to specify which files should be published.</span></span>

<span data-ttu-id="7e4f2-203">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7e4f2-203">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="file-provider-interfaces"></a><span data-ttu-id="7e4f2-204">Interfaces de proveedor de archivos</span><span class="sxs-lookup"><span data-stu-id="7e4f2-204">File Provider interfaces</span></span>

<span data-ttu-id="7e4f2-205">La interfaz principal es <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-205">The primary interface is <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span></span> <span data-ttu-id="7e4f2-206">`IFileProvider` expone métodos para:</span><span class="sxs-lookup"><span data-stu-id="7e4f2-206">`IFileProvider` exposes methods to:</span></span>

* <span data-ttu-id="7e4f2-207">Obtenga la información del archivo (<xref:Microsoft.Extensions.FileProviders.IFileInfo>).</span><span class="sxs-lookup"><span data-stu-id="7e4f2-207">Obtain file information (<xref:Microsoft.Extensions.FileProviders.IFileInfo>).</span></span>
* <span data-ttu-id="7e4f2-208">Obtenga la información del directorio (<xref:Microsoft.Extensions.FileProviders.IDirectoryContents>).</span><span class="sxs-lookup"><span data-stu-id="7e4f2-208">Obtain directory information (<xref:Microsoft.Extensions.FileProviders.IDirectoryContents>).</span></span>
* <span data-ttu-id="7e4f2-209">Configure las notificaciones de cambio (mediante <xref:Microsoft.Extensions.Primitives.IChangeToken>).</span><span class="sxs-lookup"><span data-stu-id="7e4f2-209">Set up change notifications (using an <xref:Microsoft.Extensions.Primitives.IChangeToken>).</span></span>

<span data-ttu-id="7e4f2-210">`IFileInfo` proporciona métodos y propiedades para trabajar con archivos:</span><span class="sxs-lookup"><span data-stu-id="7e4f2-210">`IFileInfo` provides methods and properties for working with files:</span></span>

* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Exists>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.IsDirectory>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Name>
* <span data-ttu-id="7e4f2-211"><xref:Microsoft.Extensions.FileProviders.IFileInfo.Length> (en bytes)</span><span class="sxs-lookup"><span data-stu-id="7e4f2-211"><xref:Microsoft.Extensions.FileProviders.IFileInfo.Length> (in bytes)</span></span>
* <span data-ttu-id="7e4f2-212">Fecha <xref:Microsoft.Extensions.FileProviders.IFileInfo.LastModified></span><span class="sxs-lookup"><span data-stu-id="7e4f2-212"><xref:Microsoft.Extensions.FileProviders.IFileInfo.LastModified> date</span></span>

<span data-ttu-id="7e4f2-213">Puede leer del archivo mediante el método [IFileInfo.CreateReadStream](xref:Microsoft.Extensions.FileProviders.IFileInfo.CreateReadStream*).</span><span class="sxs-lookup"><span data-stu-id="7e4f2-213">You can read from the file using the [IFileInfo.CreateReadStream](xref:Microsoft.Extensions.FileProviders.IFileInfo.CreateReadStream*) method.</span></span>

<span data-ttu-id="7e4f2-214">La aplicación de ejemplo muestra cómo configurar un proveedor de archivos en `Startup.ConfigureServices` para su uso en toda la aplicación a través de la [inserción de dependencias](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="7e4f2-214">The sample app demonstrates how to configure a File Provider in `Startup.ConfigureServices` for use throughout the app via [dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="file-provider-implementations"></a><span data-ttu-id="7e4f2-215">Implementaciones del proveedor de archivos</span><span class="sxs-lookup"><span data-stu-id="7e4f2-215">File Provider implementations</span></span>

<span data-ttu-id="7e4f2-216">Hay tres implementaciones de `IFileProvider` disponibles.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-216">Three implementations of `IFileProvider` are available.</span></span>

| <span data-ttu-id="7e4f2-217">Implementación</span><span class="sxs-lookup"><span data-stu-id="7e4f2-217">Implementation</span></span> | <span data-ttu-id="7e4f2-218">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="7e4f2-218">Description</span></span> |
| -------------- | ----------- |
| [<span data-ttu-id="7e4f2-219">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="7e4f2-219">PhysicalFileProvider</span></span>](#physicalfileprovider) | <span data-ttu-id="7e4f2-220">El proveedor físico se utiliza para acceder a los archivos físicos del sistema.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-220">The physical provider is used to access the system's physical files.</span></span> |
| [<span data-ttu-id="7e4f2-221">ManifestEmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="7e4f2-221">ManifestEmbeddedFileProvider</span></span>](#manifestembeddedfileprovider) | <span data-ttu-id="7e4f2-222">El proveedor insertado de manifiestos se utiliza para tener acceder a archivos insertados en ensamblados.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-222">The manifest embedded provider is used to access files embedded in assemblies.</span></span> |
| [<span data-ttu-id="7e4f2-223">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="7e4f2-223">CompositeFileProvider</span></span>](#compositefileprovider) | <span data-ttu-id="7e4f2-224">El proveedor compuesto se utiliza para proporcionar acceso combinado a archivos y directorios de uno o más proveedores.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-224">The composite provider is used to provide combined access to files and directories from one or more other providers.</span></span> |

### <a name="physicalfileprovider"></a><span data-ttu-id="7e4f2-225">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="7e4f2-225">PhysicalFileProvider</span></span>

<span data-ttu-id="7e4f2-226"><xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> proporciona acceso al sistema de archivos físico.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-226">The <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> provides access to the physical file system.</span></span> <span data-ttu-id="7e4f2-227">`PhysicalFileProvider` usa el tipo <xref:System.IO.File?displayProperty=fullName> (para el proveedor físico) y define el ámbito de todas las rutas de acceso a un directorio y sus elementos secundarios.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-227">`PhysicalFileProvider` uses the <xref:System.IO.File?displayProperty=fullName> type (for the physical provider) and scopes all paths to a directory and its children.</span></span> <span data-ttu-id="7e4f2-228">Esta definición de ámbito impide el acceso al sistema de archivos fuera del directorio especificado y sus elementos secundarios.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-228">This scoping prevents access to the file system outside of the specified directory and its children.</span></span> <span data-ttu-id="7e4f2-229">El escenario más común para crear y usar `PhysicalFileProvider` es solicitar `IFileProvider` en un constructor mediante la [inyección de dependencias](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="7e4f2-229">The most common scenario for creating and using a `PhysicalFileProvider` is to request an `IFileProvider` in a constructor through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="7e4f2-230">Al crear una instancia de este proveedor directamente, se requiere una ruta de acceso del directorio, que actúa como la ruta de acceso base para todas las solicitudes realizadas usando el proveedor.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-230">When instantiating this provider directly, a directory path is required and serves as the base path for all requests made using the provider.</span></span>

<span data-ttu-id="7e4f2-231">El código siguiente muestra cómo crear `PhysicalFileProvider` y usarlo para obtener el contenido del directorio y la información del archivo:</span><span class="sxs-lookup"><span data-stu-id="7e4f2-231">The following code shows how to create a `PhysicalFileProvider` and use it to obtain directory contents and file information:</span></span>

```csharp
var provider = new PhysicalFileProvider(applicationRoot);
var contents = provider.GetDirectoryContents(string.Empty);
var fileInfo = provider.GetFileInfo("wwwroot/js/site.js");
```

<span data-ttu-id="7e4f2-232">Tipos del ejemplo anterior:</span><span class="sxs-lookup"><span data-stu-id="7e4f2-232">Types in the preceding example:</span></span>

* <span data-ttu-id="7e4f2-233">`provider` es `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-233">`provider` is an `IFileProvider`.</span></span>
* <span data-ttu-id="7e4f2-234">`contents` es `IDirectoryContents`.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-234">`contents` is an `IDirectoryContents`.</span></span>
* <span data-ttu-id="7e4f2-235">`fileInfo` es `IFileInfo`.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-235">`fileInfo` is an `IFileInfo`.</span></span>

<span data-ttu-id="7e4f2-236">El proveedor de archivos puede usarse para iterar por el directorio especificado por `applicationRoot` o llamar a `GetFileInfo` para obtener información de un archivo.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-236">The File Provider can be used to iterate through the directory specified by `applicationRoot` or call `GetFileInfo` to obtain a file's information.</span></span> <span data-ttu-id="7e4f2-237">El proveedor de archivos no tiene acceso fuera del directorio `applicationRoot`.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-237">The File Provider has no access outside of the `applicationRoot` directory.</span></span>

<span data-ttu-id="7e4f2-238">La aplicación de ejemplo crea el proveedor en la clase `Startup.ConfigureServices` de la aplicación mediante [IHostingEnvironment.ContentRootFileProvider](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootFileProvider):</span><span class="sxs-lookup"><span data-stu-id="7e4f2-238">The sample app creates the provider in the app's `Startup.ConfigureServices` class using [IHostingEnvironment.ContentRootFileProvider](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootFileProvider):</span></span>

```csharp
var physicalProvider = _env.ContentRootFileProvider;
```

### <a name="manifestembeddedfileprovider"></a><span data-ttu-id="7e4f2-239">ManifestEmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="7e4f2-239">ManifestEmbeddedFileProvider</span></span>

<span data-ttu-id="7e4f2-240"><xref:Microsoft.Extensions.FileProviders.ManifestEmbeddedFileProvider> se utiliza para acceder a archivos insertados dentro de ensamblados.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-240">The <xref:Microsoft.Extensions.FileProviders.ManifestEmbeddedFileProvider> is used to access files embedded within assemblies.</span></span> <span data-ttu-id="7e4f2-241">`ManifestEmbeddedFileProvider` utiliza un manifiesto compilado en el ensamblado para reconstruir las rutas de acceso originales de los archivos incrustados.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-241">The `ManifestEmbeddedFileProvider` uses a manifest compiled into the assembly to reconstruct the original paths of the embedded files.</span></span>

<span data-ttu-id="7e4f2-242">Para generar un manifiesto de los archivos incrustados, establezca la propiedad `<GenerateEmbeddedFilesManifest>` en `true`.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-242">To generate a manifest of the embedded files, set the `<GenerateEmbeddedFilesManifest>` property to `true`.</span></span> <span data-ttu-id="7e4f2-243">Especifique los archivos que desea incrustar con [&lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):</span><span class="sxs-lookup"><span data-stu-id="7e4f2-243">Specify the files to embed with [&lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/FileProviderSample.csproj?highlight=6,14)]

<span data-ttu-id="7e4f2-244">Use [patrones globales](#glob-patterns) para especificar uno o varios archivos para incrustar en el ensamblado.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-244">Use [glob patterns](#glob-patterns) to specify one or more files to embed into the assembly.</span></span>

<span data-ttu-id="7e4f2-245">La aplicación de ejemplo crea `ManifestEmbeddedFileProvider` y pasa el ensamblado que se está ejecutando actualmente a su constructor.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-245">The sample app creates an `ManifestEmbeddedFileProvider` and passes the currently executing assembly to its constructor.</span></span>

<span data-ttu-id="7e4f2-246">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="7e4f2-246">*Startup.cs*:</span></span>

```csharp
var manifestEmbeddedProvider = 
    new ManifestEmbeddedFileProvider(Assembly.GetEntryAssembly());
```

<span data-ttu-id="7e4f2-247">Las sobrecargas adicionales le permiten:</span><span class="sxs-lookup"><span data-stu-id="7e4f2-247">Additional overloads allow you to:</span></span>

* <span data-ttu-id="7e4f2-248">Especificar una ruta de acceso de archivo relativa.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-248">Specify a relative file path.</span></span>
* <span data-ttu-id="7e4f2-249">Definir el ámbito de archivos a la fecha de la última modificación.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-249">Scope files to a last modified date.</span></span>
* <span data-ttu-id="7e4f2-250">Asignar nombre al recurso incrustado que contiene el manifiesto del archivo incrustado.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-250">Name the embedded resource containing the embedded file manifest.</span></span>

| <span data-ttu-id="7e4f2-251">Sobrecarga</span><span class="sxs-lookup"><span data-stu-id="7e4f2-251">Overload</span></span> | <span data-ttu-id="7e4f2-252">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="7e4f2-252">Description</span></span> |
| -------- | ----------- |
| `ManifestEmbeddedFileProvider(Assembly, String)` | <span data-ttu-id="7e4f2-253">Acepta parámetro de ruta de acceso relativa `root` opcional.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-253">Accepts an optional `root` relative path parameter.</span></span> <span data-ttu-id="7e4f2-254">Especifique `root` para definir el ámbito de las llamadas en <xref:Microsoft.Extensions.FileProviders.IFileProvider.GetDirectoryContents*> en aquellos recursos que se encuentran bajo las rutas de acceso proporcionadas.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-254">Specify the `root` to scope calls to <xref:Microsoft.Extensions.FileProviders.IFileProvider.GetDirectoryContents*> to those resources under the provided path.</span></span> |
| `ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)` | <span data-ttu-id="7e4f2-255">Acepta un parámetro de ruta de acceso relativa `root` opcional y un parámetro de fecha `lastModified` (<xref:System.DateTimeOffset>).</span><span class="sxs-lookup"><span data-stu-id="7e4f2-255">Accepts an optional `root` relative path parameter and a `lastModified` date (<xref:System.DateTimeOffset>) parameter.</span></span> <span data-ttu-id="7e4f2-256">La fecha `lastModified` define el ámbito de la última fecha de modificación para las instancias de <xref:Microsoft.Extensions.FileProviders.IFileInfo> que <xref:Microsoft.Extensions.FileProviders.IFileProvider> devuelve.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-256">The `lastModified` date scopes the last modification date for the <xref:Microsoft.Extensions.FileProviders.IFileInfo> instances returned by the <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span></span> |
| `ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)` | <span data-ttu-id="7e4f2-257">Acepta una ruta de acceso relativa `root` opcional, una fecha `lastModified` y parámetros `manifestName`.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-257">Accepts an optional `root` relative path, `lastModified` date, and `manifestName` parameters.</span></span> <span data-ttu-id="7e4f2-258">`manifestName` representa el nombre del recurso incrustado que contiene el manifiesto.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-258">The `manifestName` represents the name of the embedded resource containing the manifest.</span></span> |

### <a name="compositefileprovider"></a><span data-ttu-id="7e4f2-259">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="7e4f2-259">CompositeFileProvider</span></span>

<span data-ttu-id="7e4f2-260"><xref:Microsoft.Extensions.FileProviders.CompositeFileProvider> combina instancias de `IFileProvider`, y expone una única interfaz para trabajar con archivos de varios proveedores.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-260">The <xref:Microsoft.Extensions.FileProviders.CompositeFileProvider> combines `IFileProvider` instances, exposing a single interface for working with files from multiple providers.</span></span> <span data-ttu-id="7e4f2-261">Al crear `CompositeFileProvider`, se pasan una o varias instancias de `IFileProvider` a su constructor.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-261">When creating the `CompositeFileProvider`, pass one or more `IFileProvider` instances to its constructor.</span></span>

<span data-ttu-id="7e4f2-262">En la aplicación de ejemplo, `PhysicalFileProvider` y `ManifestEmbeddedFileProvider` proporcionan archivos a un `CompositeFileProvider` registrado en el contenedor de servicios de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="7e4f2-262">In the sample app, a `PhysicalFileProvider` and a `ManifestEmbeddedFileProvider` provide files to a `CompositeFileProvider` registered in the app's service container:</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/Startup.cs?name=snippet1)]

## <a name="watch-for-changes"></a><span data-ttu-id="7e4f2-263">Observación de cambios</span><span class="sxs-lookup"><span data-stu-id="7e4f2-263">Watch for changes</span></span>

<span data-ttu-id="7e4f2-264">El método [IFileProvider.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) proporciona un escenario para ver uno o más archivos o directorios para detectar cambios.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-264">The [IFileProvider.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) method provides a scenario to watch one or more files or directories for changes.</span></span> <span data-ttu-id="7e4f2-265">`Watch` acepta una cadena de ruta de acceso, que puede usar [patrones globales](#glob-patterns) para especificar varios archivos.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-265">`Watch` accepts a path string, which can use [glob patterns](#glob-patterns) to specify multiple files.</span></span> <span data-ttu-id="7e4f2-266">`Watch` devuelve un valor de <xref:Microsoft.Extensions.Primitives.IChangeToken>.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-266">`Watch` returns an <xref:Microsoft.Extensions.Primitives.IChangeToken>.</span></span> <span data-ttu-id="7e4f2-267">El token de cambio expone:</span><span class="sxs-lookup"><span data-stu-id="7e4f2-267">The change token exposes:</span></span>

* <span data-ttu-id="7e4f2-268"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> &ndash; una propiedad que se puede inspeccionar para determinar si se ha producido un cambio.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-268"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> &ndash; A property that can be inspected to determine if a change has occurred.</span></span>
* <span data-ttu-id="7e4f2-269"><xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*> &ndash; se llama cuando se detectan cambios en la cadena de ruta de acceso especificada.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-269"><xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*> &ndash; Called when changes are detected to the specified path string.</span></span> <span data-ttu-id="7e4f2-270">Cada token de cambio solo llama a su devolución de llamada asociada en respuesta a un único cambio.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-270">Each change token only calls its associated callback in response to a single change.</span></span> <span data-ttu-id="7e4f2-271">Para habilitar la supervisión constante, puede usar <xref:System.Threading.Tasks.TaskCompletionSource`1> (como se muestra a continuación) o volver a crear instancias de `IChangeToken` en respuesta a los cambios.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-271">To enable constant monitoring, use a <xref:System.Threading.Tasks.TaskCompletionSource`1> (shown below) or recreate `IChangeToken` instances in response to changes.</span></span>

<span data-ttu-id="7e4f2-272">En la aplicación de ejemplo, la aplicación de consola *WatchConsole* se configura para mostrar un mensaje cada vez que se modifica un archivo de texto:</span><span class="sxs-lookup"><span data-stu-id="7e4f2-272">In the sample app, the *WatchConsole* console app is configured to display a message whenever a text file is modified:</span></span>

[!code-csharp[](file-providers/samples/2.x/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

<span data-ttu-id="7e4f2-273">Algunos sistemas de archivos, como contenedores de Docker y recursos compartidos de red, no pueden enviar notificaciones de cambio de forma confiable.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-273">Some file systems, such as Docker containers and network shares, may not reliably send change notifications.</span></span> <span data-ttu-id="7e4f2-274">Establezca la variable de entorno `DOTNET_USE_POLLING_FILE_WATCHER` en `1` o `true` para sondear el sistema de archivos en busca de cambios cada cuatro segundos (no configurable).</span><span class="sxs-lookup"><span data-stu-id="7e4f2-274">Set the `DOTNET_USE_POLLING_FILE_WATCHER` environment variable to `1` or `true` to poll the file system for changes every four seconds (not configurable).</span></span>

## <a name="glob-patterns"></a><span data-ttu-id="7e4f2-275">Patrones globales</span><span class="sxs-lookup"><span data-stu-id="7e4f2-275">Glob patterns</span></span>

<span data-ttu-id="7e4f2-276">Las rutas de acceso del sistema de archivos utilizan patrones de caracteres comodín denominados *patrones globales*.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-276">File system paths use wildcard patterns called *glob (or globbing) patterns*.</span></span> <span data-ttu-id="7e4f2-277">Especifique grupos de archivos con estos patrones.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-277">Specify groups of files with these patterns.</span></span> <span data-ttu-id="7e4f2-278">Los dos caracteres comodín son `*` y `**`:</span><span class="sxs-lookup"><span data-stu-id="7e4f2-278">The two wildcard characters are `*` and `**`:</span></span>

**`*`**  
<span data-ttu-id="7e4f2-279">Coincide con cualquier elemento del nivel de carpeta actual, con cualquier nombre de archivo o con cualquier extensión de archivo.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-279">Matches anything at the current folder level, any filename, or any file extension.</span></span> <span data-ttu-id="7e4f2-280">Las coincidencias finalizan con los caracteres `/` y `.` en la ruta de acceso de archivo.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-280">Matches are terminated by `/` and `.` characters in the file path.</span></span>

**`**`**  
<span data-ttu-id="7e4f2-281">Coincide con cualquier elemento de varios niveles de directorios.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-281">Matches anything across multiple directory levels.</span></span> <span data-ttu-id="7e4f2-282">Puede usarse para coincidir de forma recursiva con muchos archivos dentro de una jerarquía de directorios.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-282">Can be used to recursively match many files within a directory hierarchy.</span></span>

<span data-ttu-id="7e4f2-283">**Ejemplos de patrones globales**</span><span class="sxs-lookup"><span data-stu-id="7e4f2-283">**Glob pattern examples**</span></span>

**`directory/file.txt`**  
<span data-ttu-id="7e4f2-284">Coincide con un archivo concreto en un directorio específico.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-284">Matches a specific file in a specific directory.</span></span>

**`directory/*.txt`**  
<span data-ttu-id="7e4f2-285">Coincide con todos los archivos que tengan la extensión *.txt* en un directorio específico.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-285">Matches all files with *.txt* extension in a specific directory.</span></span>

**`directory/*/appsettings.json`**  
<span data-ttu-id="7e4f2-286">Coincide con todos los archivos `appsettings.json` que estén en directorios exactamente un nivel por debajo de la carpeta *directorio*.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-286">Matches all `appsettings.json` files in directories exactly one level below the *directory* folder.</span></span>

**`directory/**/*.txt`**  
<span data-ttu-id="7e4f2-287">Coincide con todos los archivos que tengan la extensión *.txt* y se encuentren en cualquier lugar de la carpeta *directorio*.</span><span class="sxs-lookup"><span data-stu-id="7e4f2-287">Matches all files with *.txt* extension found anywhere under the *directory* folder.</span></span>

::: moniker-end
