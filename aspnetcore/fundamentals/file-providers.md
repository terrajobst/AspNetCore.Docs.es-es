---
title: Proveedores de archivo en ASP.NET Core
author: guardrex
description: Obtenga información sobre cómo ASP.NET Core abstrae el acceso al sistema de archivos mediante el uso de proveedores de archivos.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/07/2019
uid: fundamentals/file-providers
ms.openlocfilehash: 3a92b44efc70d156596ee9fe80b4f6a65266e73d
ms.sourcegitcommit: 3d082bd46e9e00a3297ea0314582b1ed2abfa830
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/07/2019
ms.locfileid: "72007173"
---
# <a name="file-providers-in-aspnet-core"></a><span data-ttu-id="c8f72-103">Proveedores de archivo en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c8f72-103">File Providers in ASP.NET Core</span></span>

<span data-ttu-id="c8f72-104">Por [Steve Smith](https://ardalis.com/) y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="c8f72-104">By [Steve Smith](https://ardalis.com/) and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="c8f72-105">ASP.NET Core abstrae el acceso al sistema de archivos mediante el uso de proveedores de archivos.</span><span class="sxs-lookup"><span data-stu-id="c8f72-105">ASP.NET Core abstracts file system access through the use of File Providers.</span></span> <span data-ttu-id="c8f72-106">Los proveedores de archivos se usan en el marco de ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="c8f72-106">File Providers are used throughout the ASP.NET Core framework:</span></span>

* <span data-ttu-id="c8f72-107">`IWebHostEnvironment` expone la [raíz del contenido](xref:fundamentals/index#content-root) y la [raíz web](xref:fundamentals/index#web-root) de la aplicación como tipos `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="c8f72-107">`IWebHostEnvironment` exposes the app's [content root](xref:fundamentals/index#content-root) and [web root](xref:fundamentals/index#web-root) as `IFileProvider` types.</span></span>
* <span data-ttu-id="c8f72-108">El [middleware de archivos estáticos](xref:fundamentals/static-files) usa proveedores de archivos para buscar archivos estáticos.</span><span class="sxs-lookup"><span data-stu-id="c8f72-108">[Static File Middleware](xref:fundamentals/static-files) uses File Providers to locate static files.</span></span>
* <span data-ttu-id="c8f72-109">[Razor](xref:mvc/views/razor) usa proveedores de archivos para localizar páginas y vistas.</span><span class="sxs-lookup"><span data-stu-id="c8f72-109">[Razor](xref:mvc/views/razor) uses File Providers to locate pages and views.</span></span>
* <span data-ttu-id="c8f72-110">Las herramientas de .NET Core usan proveedores de archivos y patrones globales para especificar los archivos que deben publicarse.</span><span class="sxs-lookup"><span data-stu-id="c8f72-110">.NET Core tooling uses File Providers and glob patterns to specify which files should be published.</span></span>

<span data-ttu-id="c8f72-111">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c8f72-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="file-provider-interfaces"></a><span data-ttu-id="c8f72-112">Interfaces de proveedor de archivos</span><span class="sxs-lookup"><span data-stu-id="c8f72-112">File Provider interfaces</span></span>

<span data-ttu-id="c8f72-113">La interfaz principal es <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span><span class="sxs-lookup"><span data-stu-id="c8f72-113">The primary interface is <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span></span> <span data-ttu-id="c8f72-114">`IFileProvider` expone métodos para:</span><span class="sxs-lookup"><span data-stu-id="c8f72-114">`IFileProvider` exposes methods to:</span></span>

* <span data-ttu-id="c8f72-115">Obtenga la información del archivo (<xref:Microsoft.Extensions.FileProviders.IFileInfo>).</span><span class="sxs-lookup"><span data-stu-id="c8f72-115">Obtain file information (<xref:Microsoft.Extensions.FileProviders.IFileInfo>).</span></span>
* <span data-ttu-id="c8f72-116">Obtenga la información del directorio (<xref:Microsoft.Extensions.FileProviders.IDirectoryContents>).</span><span class="sxs-lookup"><span data-stu-id="c8f72-116">Obtain directory information (<xref:Microsoft.Extensions.FileProviders.IDirectoryContents>).</span></span>
* <span data-ttu-id="c8f72-117">Configure las notificaciones de cambio (mediante <xref:Microsoft.Extensions.Primitives.IChangeToken>).</span><span class="sxs-lookup"><span data-stu-id="c8f72-117">Set up change notifications (using an <xref:Microsoft.Extensions.Primitives.IChangeToken>).</span></span>

<span data-ttu-id="c8f72-118">`IFileInfo` proporciona métodos y propiedades para trabajar con archivos:</span><span class="sxs-lookup"><span data-stu-id="c8f72-118">`IFileInfo` provides methods and properties for working with files:</span></span>

* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Exists>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.IsDirectory>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Name>
* <span data-ttu-id="c8f72-119"><xref:Microsoft.Extensions.FileProviders.IFileInfo.Length> (en bytes)</span><span class="sxs-lookup"><span data-stu-id="c8f72-119"><xref:Microsoft.Extensions.FileProviders.IFileInfo.Length> (in bytes)</span></span>
* <span data-ttu-id="c8f72-120">Fecha <xref:Microsoft.Extensions.FileProviders.IFileInfo.LastModified></span><span class="sxs-lookup"><span data-stu-id="c8f72-120"><xref:Microsoft.Extensions.FileProviders.IFileInfo.LastModified> date</span></span>

<span data-ttu-id="c8f72-121">Puede leer del archivo mediante el método [IFileInfo.CreateReadStream](xref:Microsoft.Extensions.FileProviders.IFileInfo.CreateReadStream*).</span><span class="sxs-lookup"><span data-stu-id="c8f72-121">You can read from the file using the [IFileInfo.CreateReadStream](xref:Microsoft.Extensions.FileProviders.IFileInfo.CreateReadStream*) method.</span></span>

<span data-ttu-id="c8f72-122">La aplicación de ejemplo muestra cómo configurar un proveedor de archivos en `Startup.ConfigureServices` para su uso en toda la aplicación a través de la [inserción de dependencias](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="c8f72-122">The sample app demonstrates how to configure a File Provider in `Startup.ConfigureServices` for use throughout the app via [dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="file-provider-implementations"></a><span data-ttu-id="c8f72-123">Implementaciones del proveedor de archivos</span><span class="sxs-lookup"><span data-stu-id="c8f72-123">File Provider implementations</span></span>

<span data-ttu-id="c8f72-124">Hay tres implementaciones de `IFileProvider` disponibles.</span><span class="sxs-lookup"><span data-stu-id="c8f72-124">Three implementations of `IFileProvider` are available.</span></span>

| <span data-ttu-id="c8f72-125">Implementación</span><span class="sxs-lookup"><span data-stu-id="c8f72-125">Implementation</span></span> | <span data-ttu-id="c8f72-126">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="c8f72-126">Description</span></span> |
| -------------- | ----------- |
| [<span data-ttu-id="c8f72-127">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="c8f72-127">PhysicalFileProvider</span></span>](#physicalfileprovider) | <span data-ttu-id="c8f72-128">El proveedor físico se utiliza para acceder a los archivos físicos del sistema.</span><span class="sxs-lookup"><span data-stu-id="c8f72-128">The physical provider is used to access the system's physical files.</span></span> |
| [<span data-ttu-id="c8f72-129">ManifestEmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="c8f72-129">ManifestEmbeddedFileProvider</span></span>](#manifestembeddedfileprovider) | <span data-ttu-id="c8f72-130">El proveedor insertado de manifiestos se utiliza para tener acceder a archivos insertados en ensamblados.</span><span class="sxs-lookup"><span data-stu-id="c8f72-130">The manifest embedded provider is used to access files embedded in assemblies.</span></span> |
| [<span data-ttu-id="c8f72-131">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="c8f72-131">CompositeFileProvider</span></span>](#compositefileprovider) | <span data-ttu-id="c8f72-132">El proveedor compuesto se utiliza para proporcionar acceso combinado a archivos y directorios de uno o más proveedores.</span><span class="sxs-lookup"><span data-stu-id="c8f72-132">The composite provider is used to provide combined access to files and directories from one or more other providers.</span></span> |

### <a name="physicalfileprovider"></a><span data-ttu-id="c8f72-133">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="c8f72-133">PhysicalFileProvider</span></span>

<span data-ttu-id="c8f72-134"><xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> proporciona acceso al sistema de archivos físico.</span><span class="sxs-lookup"><span data-stu-id="c8f72-134">The <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> provides access to the physical file system.</span></span> <span data-ttu-id="c8f72-135">`PhysicalFileProvider` usa el tipo <xref:System.IO.File?displayProperty=fullName> (para el proveedor físico) y define el ámbito de todas las rutas de acceso a un directorio y sus elementos secundarios.</span><span class="sxs-lookup"><span data-stu-id="c8f72-135">`PhysicalFileProvider` uses the <xref:System.IO.File?displayProperty=fullName> type (for the physical provider) and scopes all paths to a directory and its children.</span></span> <span data-ttu-id="c8f72-136">Esta definición de ámbito impide el acceso al sistema de archivos fuera del directorio especificado y sus elementos secundarios.</span><span class="sxs-lookup"><span data-stu-id="c8f72-136">This scoping prevents access to the file system outside of the specified directory and its children.</span></span> <span data-ttu-id="c8f72-137">El escenario más común para crear y usar `PhysicalFileProvider` es solicitar `IFileProvider` en un constructor mediante la [inyección de dependencias](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="c8f72-137">The most common scenario for creating and using a `PhysicalFileProvider` is to request an `IFileProvider` in a constructor through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="c8f72-138">Al crear una instancia de este proveedor directamente, se requiere una ruta de acceso del directorio, que actúa como la ruta de acceso base para todas las solicitudes realizadas usando el proveedor.</span><span class="sxs-lookup"><span data-stu-id="c8f72-138">When instantiating this provider directly, a directory path is required and serves as the base path for all requests made using the provider.</span></span>

<span data-ttu-id="c8f72-139">El código siguiente muestra cómo crear `PhysicalFileProvider` y usarlo para obtener el contenido del directorio y la información del archivo:</span><span class="sxs-lookup"><span data-stu-id="c8f72-139">The following code shows how to create a `PhysicalFileProvider` and use it to obtain directory contents and file information:</span></span>

```csharp
var provider = new PhysicalFileProvider(applicationRoot);
var contents = provider.GetDirectoryContents(string.Empty);
var fileInfo = provider.GetFileInfo("wwwroot/js/site.js");
```

<span data-ttu-id="c8f72-140">Tipos del ejemplo anterior:</span><span class="sxs-lookup"><span data-stu-id="c8f72-140">Types in the preceding example:</span></span>

* <span data-ttu-id="c8f72-141">`provider` es `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="c8f72-141">`provider` is an `IFileProvider`.</span></span>
* <span data-ttu-id="c8f72-142">`contents` es `IDirectoryContents`.</span><span class="sxs-lookup"><span data-stu-id="c8f72-142">`contents` is an `IDirectoryContents`.</span></span>
* <span data-ttu-id="c8f72-143">`fileInfo` es `IFileInfo`.</span><span class="sxs-lookup"><span data-stu-id="c8f72-143">`fileInfo` is an `IFileInfo`.</span></span>

<span data-ttu-id="c8f72-144">El proveedor de archivos puede usarse para iterar por el directorio especificado por `applicationRoot` o llamar a `GetFileInfo` para obtener información de un archivo.</span><span class="sxs-lookup"><span data-stu-id="c8f72-144">The File Provider can be used to iterate through the directory specified by `applicationRoot` or call `GetFileInfo` to obtain a file's information.</span></span> <span data-ttu-id="c8f72-145">El proveedor de archivos no tiene acceso fuera del directorio `applicationRoot`.</span><span class="sxs-lookup"><span data-stu-id="c8f72-145">The File Provider has no access outside of the `applicationRoot` directory.</span></span>

<span data-ttu-id="c8f72-146">La aplicación de ejemplo crea el proveedor en la clase `Startup.ConfigureServices` de la aplicación mediante [IHostingEnvironment.ContentRootFileProvider](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootFileProvider):</span><span class="sxs-lookup"><span data-stu-id="c8f72-146">The sample app creates the provider in the app's `Startup.ConfigureServices` class using [IHostingEnvironment.ContentRootFileProvider](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootFileProvider):</span></span>

```csharp
var physicalProvider = _env.ContentRootFileProvider;
```

### <a name="manifestembeddedfileprovider"></a><span data-ttu-id="c8f72-147">ManifestEmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="c8f72-147">ManifestEmbeddedFileProvider</span></span>

<span data-ttu-id="c8f72-148"><xref:Microsoft.Extensions.FileProviders.ManifestEmbeddedFileProvider> se utiliza para acceder a archivos insertados dentro de ensamblados.</span><span class="sxs-lookup"><span data-stu-id="c8f72-148">The <xref:Microsoft.Extensions.FileProviders.ManifestEmbeddedFileProvider> is used to access files embedded within assemblies.</span></span> <span data-ttu-id="c8f72-149">`ManifestEmbeddedFileProvider` utiliza un manifiesto compilado en el ensamblado para reconstruir las rutas de acceso originales de los archivos incrustados.</span><span class="sxs-lookup"><span data-stu-id="c8f72-149">The `ManifestEmbeddedFileProvider` uses a manifest compiled into the assembly to reconstruct the original paths of the embedded files.</span></span>

<span data-ttu-id="c8f72-150">Agregue una referencia de paquete al proyecto para el paquete [Microsoft.Extensions.FileProviders.Embedded](https://www.nuget.org/packages/Microsoft.Extensions.FileProviders.Embedded).</span><span class="sxs-lookup"><span data-stu-id="c8f72-150">Add a package reference to the project for the [Microsoft.Extensions.FileProviders.Embedded](https://www.nuget.org/packages/Microsoft.Extensions.FileProviders.Embedded) package.</span></span>

<span data-ttu-id="c8f72-151">Para generar un manifiesto de los archivos incrustados, establezca la propiedad `<GenerateEmbeddedFilesManifest>` en `true`.</span><span class="sxs-lookup"><span data-stu-id="c8f72-151">To generate a manifest of the embedded files, set the `<GenerateEmbeddedFilesManifest>` property to `true`.</span></span> <span data-ttu-id="c8f72-152">Especifique los archivos que desea incrustar con [\<EmbeddedResource>](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):</span><span class="sxs-lookup"><span data-stu-id="c8f72-152">Specify the files to embed with [\<EmbeddedResource>](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):</span></span>

[!code-csharp[](file-providers/samples/3.x/FileProviderSample/FileProviderSample.csproj?highlight=6,14)]

<span data-ttu-id="c8f72-153">Use [patrones globales](#glob-patterns) para especificar uno o varios archivos para incrustar en el ensamblado.</span><span class="sxs-lookup"><span data-stu-id="c8f72-153">Use [glob patterns](#glob-patterns) to specify one or more files to embed into the assembly.</span></span>

<span data-ttu-id="c8f72-154">La aplicación de ejemplo crea `ManifestEmbeddedFileProvider` y pasa el ensamblado que se está ejecutando actualmente a su constructor.</span><span class="sxs-lookup"><span data-stu-id="c8f72-154">The sample app creates an `ManifestEmbeddedFileProvider` and passes the currently executing assembly to its constructor.</span></span>

<span data-ttu-id="c8f72-155">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="c8f72-155">*Startup.cs*:</span></span>

```csharp
var manifestEmbeddedProvider = 
    new ManifestEmbeddedFileProvider(Assembly.GetEntryAssembly());
```

<span data-ttu-id="c8f72-156">Las sobrecargas adicionales le permiten:</span><span class="sxs-lookup"><span data-stu-id="c8f72-156">Additional overloads allow you to:</span></span>

* <span data-ttu-id="c8f72-157">Especificar una ruta de acceso de archivo relativa.</span><span class="sxs-lookup"><span data-stu-id="c8f72-157">Specify a relative file path.</span></span>
* <span data-ttu-id="c8f72-158">Definir el ámbito de archivos a la fecha de la última modificación.</span><span class="sxs-lookup"><span data-stu-id="c8f72-158">Scope files to a last modified date.</span></span>
* <span data-ttu-id="c8f72-159">Asignar nombre al recurso incrustado que contiene el manifiesto del archivo incrustado.</span><span class="sxs-lookup"><span data-stu-id="c8f72-159">Name the embedded resource containing the embedded file manifest.</span></span>

| <span data-ttu-id="c8f72-160">Sobrecarga</span><span class="sxs-lookup"><span data-stu-id="c8f72-160">Overload</span></span> | <span data-ttu-id="c8f72-161">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="c8f72-161">Description</span></span> |
| -------- | ----------- |
| `ManifestEmbeddedFileProvider(Assembly, String)` | <span data-ttu-id="c8f72-162">Acepta parámetro de ruta de acceso relativa `root` opcional.</span><span class="sxs-lookup"><span data-stu-id="c8f72-162">Accepts an optional `root` relative path parameter.</span></span> <span data-ttu-id="c8f72-163">Especifique `root` para definir el ámbito de las llamadas en <xref:Microsoft.Extensions.FileProviders.IFileProvider.GetDirectoryContents*> en aquellos recursos que se encuentran bajo las rutas de acceso proporcionadas.</span><span class="sxs-lookup"><span data-stu-id="c8f72-163">Specify the `root` to scope calls to <xref:Microsoft.Extensions.FileProviders.IFileProvider.GetDirectoryContents*> to those resources under the provided path.</span></span> |
| `ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)` | <span data-ttu-id="c8f72-164">Acepta un parámetro de ruta de acceso relativa `root` opcional y un parámetro de fecha `lastModified` (<xref:System.DateTimeOffset>).</span><span class="sxs-lookup"><span data-stu-id="c8f72-164">Accepts an optional `root` relative path parameter and a `lastModified` date (<xref:System.DateTimeOffset>) parameter.</span></span> <span data-ttu-id="c8f72-165">La fecha `lastModified` define el ámbito de la última fecha de modificación para las instancias de <xref:Microsoft.Extensions.FileProviders.IFileInfo> que <xref:Microsoft.Extensions.FileProviders.IFileProvider> devuelve.</span><span class="sxs-lookup"><span data-stu-id="c8f72-165">The `lastModified` date scopes the last modification date for the <xref:Microsoft.Extensions.FileProviders.IFileInfo> instances returned by the <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span></span> |
| `ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)` | <span data-ttu-id="c8f72-166">Acepta una ruta de acceso relativa `root` opcional, una fecha `lastModified` y parámetros `manifestName`.</span><span class="sxs-lookup"><span data-stu-id="c8f72-166">Accepts an optional `root` relative path, `lastModified` date, and `manifestName` parameters.</span></span> <span data-ttu-id="c8f72-167">`manifestName` representa el nombre del recurso incrustado que contiene el manifiesto.</span><span class="sxs-lookup"><span data-stu-id="c8f72-167">The `manifestName` represents the name of the embedded resource containing the manifest.</span></span> |

### <a name="compositefileprovider"></a><span data-ttu-id="c8f72-168">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="c8f72-168">CompositeFileProvider</span></span>

<span data-ttu-id="c8f72-169"><xref:Microsoft.Extensions.FileProviders.CompositeFileProvider> combina instancias de `IFileProvider`, y expone una única interfaz para trabajar con archivos de varios proveedores.</span><span class="sxs-lookup"><span data-stu-id="c8f72-169">The <xref:Microsoft.Extensions.FileProviders.CompositeFileProvider> combines `IFileProvider` instances, exposing a single interface for working with files from multiple providers.</span></span> <span data-ttu-id="c8f72-170">Al crear `CompositeFileProvider`, se pasan una o varias instancias de `IFileProvider` a su constructor.</span><span class="sxs-lookup"><span data-stu-id="c8f72-170">When creating the `CompositeFileProvider`, pass one or more `IFileProvider` instances to its constructor.</span></span>

<span data-ttu-id="c8f72-171">En la aplicación de ejemplo, `PhysicalFileProvider` y `ManifestEmbeddedFileProvider` proporcionan archivos a un `CompositeFileProvider` registrado en el contenedor de servicios de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="c8f72-171">In the sample app, a `PhysicalFileProvider` and a `ManifestEmbeddedFileProvider` provide files to a `CompositeFileProvider` registered in the app's service container:</span></span>

[!code-csharp[](file-providers/samples/3.x/FileProviderSample/Startup.cs?name=snippet1)]

## <a name="watch-for-changes"></a><span data-ttu-id="c8f72-172">Observación de cambios</span><span class="sxs-lookup"><span data-stu-id="c8f72-172">Watch for changes</span></span>

<span data-ttu-id="c8f72-173">El método [IFileProvider.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) proporciona un escenario para ver uno o más archivos o directorios para detectar cambios.</span><span class="sxs-lookup"><span data-stu-id="c8f72-173">The [IFileProvider.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) method provides a scenario to watch one or more files or directories for changes.</span></span> <span data-ttu-id="c8f72-174">`Watch` acepta una cadena de ruta de acceso, que puede usar [patrones globales](#glob-patterns) para especificar varios archivos.</span><span class="sxs-lookup"><span data-stu-id="c8f72-174">`Watch` accepts a path string, which can use [glob patterns](#glob-patterns) to specify multiple files.</span></span> <span data-ttu-id="c8f72-175">`Watch` devuelve un valor de <xref:Microsoft.Extensions.Primitives.IChangeToken>.</span><span class="sxs-lookup"><span data-stu-id="c8f72-175">`Watch` returns an <xref:Microsoft.Extensions.Primitives.IChangeToken>.</span></span> <span data-ttu-id="c8f72-176">El token de cambio expone:</span><span class="sxs-lookup"><span data-stu-id="c8f72-176">The change token exposes:</span></span>

* <span data-ttu-id="c8f72-177"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> &ndash; una propiedad que se puede inspeccionar para determinar si se ha producido un cambio.</span><span class="sxs-lookup"><span data-stu-id="c8f72-177"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> &ndash; A property that can be inspected to determine if a change has occurred.</span></span>
* <span data-ttu-id="c8f72-178"><xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*> &ndash; se llama cuando se detectan cambios en la cadena de ruta de acceso especificada.</span><span class="sxs-lookup"><span data-stu-id="c8f72-178"><xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*> &ndash; Called when changes are detected to the specified path string.</span></span> <span data-ttu-id="c8f72-179">Cada token de cambio solo llama a su devolución de llamada asociada en respuesta a un único cambio.</span><span class="sxs-lookup"><span data-stu-id="c8f72-179">Each change token only calls its associated callback in response to a single change.</span></span> <span data-ttu-id="c8f72-180">Para habilitar la supervisión constante, puede usar <xref:System.Threading.Tasks.TaskCompletionSource`1> (como se muestra a continuación) o volver a crear instancias de `IChangeToken` en respuesta a los cambios.</span><span class="sxs-lookup"><span data-stu-id="c8f72-180">To enable constant monitoring, use a <xref:System.Threading.Tasks.TaskCompletionSource`1> (shown below) or recreate `IChangeToken` instances in response to changes.</span></span>

<span data-ttu-id="c8f72-181">En la aplicación de ejemplo, la aplicación de consola *WatchConsole* se configura para mostrar un mensaje cada vez que se modifica un archivo de texto:</span><span class="sxs-lookup"><span data-stu-id="c8f72-181">In the sample app, the *WatchConsole* console app is configured to display a message whenever a text file is modified:</span></span>

[!code-csharp[](file-providers/samples/3.x/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

<span data-ttu-id="c8f72-182">Algunos sistemas de archivos, como contenedores de Docker y recursos compartidos de red, no pueden enviar notificaciones de cambio de forma confiable.</span><span class="sxs-lookup"><span data-stu-id="c8f72-182">Some file systems, such as Docker containers and network shares, may not reliably send change notifications.</span></span> <span data-ttu-id="c8f72-183">Establezca la variable de entorno `DOTNET_USE_POLLING_FILE_WATCHER` en `1` o `true` para sondear el sistema de archivos en busca de cambios cada cuatro segundos (no configurable).</span><span class="sxs-lookup"><span data-stu-id="c8f72-183">Set the `DOTNET_USE_POLLING_FILE_WATCHER` environment variable to `1` or `true` to poll the file system for changes every four seconds (not configurable).</span></span>

## <a name="glob-patterns"></a><span data-ttu-id="c8f72-184">Patrones globales</span><span class="sxs-lookup"><span data-stu-id="c8f72-184">Glob patterns</span></span>

<span data-ttu-id="c8f72-185">Las rutas de acceso del sistema de archivos utilizan patrones de caracteres comodín denominados *patrones globales*.</span><span class="sxs-lookup"><span data-stu-id="c8f72-185">File system paths use wildcard patterns called *glob (or globbing) patterns*.</span></span> <span data-ttu-id="c8f72-186">Especifique grupos de archivos con estos patrones.</span><span class="sxs-lookup"><span data-stu-id="c8f72-186">Specify groups of files with these patterns.</span></span> <span data-ttu-id="c8f72-187">Los dos caracteres comodín son `*` y `**`:</span><span class="sxs-lookup"><span data-stu-id="c8f72-187">The two wildcard characters are `*` and `**`:</span></span>

**`*`**  
<span data-ttu-id="c8f72-188">Coincide con cualquier elemento del nivel de carpeta actual, con cualquier nombre de archivo o con cualquier extensión de archivo.</span><span class="sxs-lookup"><span data-stu-id="c8f72-188">Matches anything at the current folder level, any filename, or any file extension.</span></span> <span data-ttu-id="c8f72-189">Las coincidencias finalizan con los caracteres `/` y `.` en la ruta de acceso de archivo.</span><span class="sxs-lookup"><span data-stu-id="c8f72-189">Matches are terminated by `/` and `.` characters in the file path.</span></span>

**`**`**  
<span data-ttu-id="c8f72-190">Coincide con cualquier elemento de varios niveles de directorios.</span><span class="sxs-lookup"><span data-stu-id="c8f72-190">Matches anything across multiple directory levels.</span></span> <span data-ttu-id="c8f72-191">Puede usarse para coincidir de forma recursiva con muchos archivos dentro de una jerarquía de directorios.</span><span class="sxs-lookup"><span data-stu-id="c8f72-191">Can be used to recursively match many files within a directory hierarchy.</span></span>

<span data-ttu-id="c8f72-192">**Ejemplos de patrones globales**</span><span class="sxs-lookup"><span data-stu-id="c8f72-192">**Glob pattern examples**</span></span>

**`directory/file.txt`**  
<span data-ttu-id="c8f72-193">Coincide con un archivo concreto en un directorio específico.</span><span class="sxs-lookup"><span data-stu-id="c8f72-193">Matches a specific file in a specific directory.</span></span>

**`directory/*.txt`**  
<span data-ttu-id="c8f72-194">Coincide con todos los archivos que tengan la extensión *.txt* en un directorio específico.</span><span class="sxs-lookup"><span data-stu-id="c8f72-194">Matches all files with *.txt* extension in a specific directory.</span></span>

**`directory/*/appsettings.json`**  
<span data-ttu-id="c8f72-195">Coincide con todos los archivos `appsettings.json` que estén en directorios exactamente un nivel por debajo de la carpeta *directorio*.</span><span class="sxs-lookup"><span data-stu-id="c8f72-195">Matches all `appsettings.json` files in directories exactly one level below the *directory* folder.</span></span>

**`directory/**/*.txt`**  
<span data-ttu-id="c8f72-196">Coincide con todos los archivos que tengan la extensión *.txt* y se encuentren en cualquier lugar de la carpeta *directorio*.</span><span class="sxs-lookup"><span data-stu-id="c8f72-196">Matches all files with *.txt* extension found anywhere under the *directory* folder.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="c8f72-197">ASP.NET Core abstrae el acceso al sistema de archivos mediante el uso de proveedores de archivos.</span><span class="sxs-lookup"><span data-stu-id="c8f72-197">ASP.NET Core abstracts file system access through the use of File Providers.</span></span> <span data-ttu-id="c8f72-198">Los proveedores de archivos se usan en el marco de ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="c8f72-198">File Providers are used throughout the ASP.NET Core framework:</span></span>

* <span data-ttu-id="c8f72-199"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment> expone la [raíz del contenido](xref:fundamentals/index#content-root) y la [raíz web](xref:fundamentals/index#web-root) de la aplicación como tipos `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="c8f72-199"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment> exposes the app's [content root](xref:fundamentals/index#content-root) and [web root](xref:fundamentals/index#web-root) as `IFileProvider` types.</span></span>
* <span data-ttu-id="c8f72-200">El [middleware de archivos estáticos](xref:fundamentals/static-files) usa proveedores de archivos para buscar archivos estáticos.</span><span class="sxs-lookup"><span data-stu-id="c8f72-200">[Static File Middleware](xref:fundamentals/static-files) uses File Providers to locate static files.</span></span>
* <span data-ttu-id="c8f72-201">[Razor](xref:mvc/views/razor) usa proveedores de archivos para localizar páginas y vistas.</span><span class="sxs-lookup"><span data-stu-id="c8f72-201">[Razor](xref:mvc/views/razor) uses File Providers to locate pages and views.</span></span>
* <span data-ttu-id="c8f72-202">Las herramientas de .NET Core usan proveedores de archivos y patrones globales para especificar los archivos que deben publicarse.</span><span class="sxs-lookup"><span data-stu-id="c8f72-202">.NET Core tooling uses File Providers and glob patterns to specify which files should be published.</span></span>

<span data-ttu-id="c8f72-203">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c8f72-203">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="file-provider-interfaces"></a><span data-ttu-id="c8f72-204">Interfaces de proveedor de archivos</span><span class="sxs-lookup"><span data-stu-id="c8f72-204">File Provider interfaces</span></span>

<span data-ttu-id="c8f72-205">La interfaz principal es <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span><span class="sxs-lookup"><span data-stu-id="c8f72-205">The primary interface is <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span></span> <span data-ttu-id="c8f72-206">`IFileProvider` expone métodos para:</span><span class="sxs-lookup"><span data-stu-id="c8f72-206">`IFileProvider` exposes methods to:</span></span>

* <span data-ttu-id="c8f72-207">Obtenga la información del archivo (<xref:Microsoft.Extensions.FileProviders.IFileInfo>).</span><span class="sxs-lookup"><span data-stu-id="c8f72-207">Obtain file information (<xref:Microsoft.Extensions.FileProviders.IFileInfo>).</span></span>
* <span data-ttu-id="c8f72-208">Obtenga la información del directorio (<xref:Microsoft.Extensions.FileProviders.IDirectoryContents>).</span><span class="sxs-lookup"><span data-stu-id="c8f72-208">Obtain directory information (<xref:Microsoft.Extensions.FileProviders.IDirectoryContents>).</span></span>
* <span data-ttu-id="c8f72-209">Configure las notificaciones de cambio (mediante <xref:Microsoft.Extensions.Primitives.IChangeToken>).</span><span class="sxs-lookup"><span data-stu-id="c8f72-209">Set up change notifications (using an <xref:Microsoft.Extensions.Primitives.IChangeToken>).</span></span>

<span data-ttu-id="c8f72-210">`IFileInfo` proporciona métodos y propiedades para trabajar con archivos:</span><span class="sxs-lookup"><span data-stu-id="c8f72-210">`IFileInfo` provides methods and properties for working with files:</span></span>

* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Exists>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.IsDirectory>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Name>
* <span data-ttu-id="c8f72-211"><xref:Microsoft.Extensions.FileProviders.IFileInfo.Length> (en bytes)</span><span class="sxs-lookup"><span data-stu-id="c8f72-211"><xref:Microsoft.Extensions.FileProviders.IFileInfo.Length> (in bytes)</span></span>
* <span data-ttu-id="c8f72-212">Fecha <xref:Microsoft.Extensions.FileProviders.IFileInfo.LastModified></span><span class="sxs-lookup"><span data-stu-id="c8f72-212"><xref:Microsoft.Extensions.FileProviders.IFileInfo.LastModified> date</span></span>

<span data-ttu-id="c8f72-213">Puede leer del archivo mediante el método [IFileInfo.CreateReadStream](xref:Microsoft.Extensions.FileProviders.IFileInfo.CreateReadStream*).</span><span class="sxs-lookup"><span data-stu-id="c8f72-213">You can read from the file using the [IFileInfo.CreateReadStream](xref:Microsoft.Extensions.FileProviders.IFileInfo.CreateReadStream*) method.</span></span>

<span data-ttu-id="c8f72-214">La aplicación de ejemplo muestra cómo configurar un proveedor de archivos en `Startup.ConfigureServices` para su uso en toda la aplicación a través de la [inserción de dependencias](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="c8f72-214">The sample app demonstrates how to configure a File Provider in `Startup.ConfigureServices` for use throughout the app via [dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="file-provider-implementations"></a><span data-ttu-id="c8f72-215">Implementaciones del proveedor de archivos</span><span class="sxs-lookup"><span data-stu-id="c8f72-215">File Provider implementations</span></span>

<span data-ttu-id="c8f72-216">Hay tres implementaciones de `IFileProvider` disponibles.</span><span class="sxs-lookup"><span data-stu-id="c8f72-216">Three implementations of `IFileProvider` are available.</span></span>

| <span data-ttu-id="c8f72-217">Implementación</span><span class="sxs-lookup"><span data-stu-id="c8f72-217">Implementation</span></span> | <span data-ttu-id="c8f72-218">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="c8f72-218">Description</span></span> |
| -------------- | ----------- |
| [<span data-ttu-id="c8f72-219">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="c8f72-219">PhysicalFileProvider</span></span>](#physicalfileprovider) | <span data-ttu-id="c8f72-220">El proveedor físico se utiliza para acceder a los archivos físicos del sistema.</span><span class="sxs-lookup"><span data-stu-id="c8f72-220">The physical provider is used to access the system's physical files.</span></span> |
| [<span data-ttu-id="c8f72-221">ManifestEmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="c8f72-221">ManifestEmbeddedFileProvider</span></span>](#manifestembeddedfileprovider) | <span data-ttu-id="c8f72-222">El proveedor insertado de manifiestos se utiliza para tener acceder a archivos insertados en ensamblados.</span><span class="sxs-lookup"><span data-stu-id="c8f72-222">The manifest embedded provider is used to access files embedded in assemblies.</span></span> |
| [<span data-ttu-id="c8f72-223">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="c8f72-223">CompositeFileProvider</span></span>](#compositefileprovider) | <span data-ttu-id="c8f72-224">El proveedor compuesto se utiliza para proporcionar acceso combinado a archivos y directorios de uno o más proveedores.</span><span class="sxs-lookup"><span data-stu-id="c8f72-224">The composite provider is used to provide combined access to files and directories from one or more other providers.</span></span> |

### <a name="physicalfileprovider"></a><span data-ttu-id="c8f72-225">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="c8f72-225">PhysicalFileProvider</span></span>

<span data-ttu-id="c8f72-226"><xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> proporciona acceso al sistema de archivos físico.</span><span class="sxs-lookup"><span data-stu-id="c8f72-226">The <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> provides access to the physical file system.</span></span> <span data-ttu-id="c8f72-227">`PhysicalFileProvider` usa el tipo <xref:System.IO.File?displayProperty=fullName> (para el proveedor físico) y define el ámbito de todas las rutas de acceso a un directorio y sus elementos secundarios.</span><span class="sxs-lookup"><span data-stu-id="c8f72-227">`PhysicalFileProvider` uses the <xref:System.IO.File?displayProperty=fullName> type (for the physical provider) and scopes all paths to a directory and its children.</span></span> <span data-ttu-id="c8f72-228">Esta definición de ámbito impide el acceso al sistema de archivos fuera del directorio especificado y sus elementos secundarios.</span><span class="sxs-lookup"><span data-stu-id="c8f72-228">This scoping prevents access to the file system outside of the specified directory and its children.</span></span> <span data-ttu-id="c8f72-229">El escenario más común para crear y usar `PhysicalFileProvider` es solicitar `IFileProvider` en un constructor mediante la [inyección de dependencias](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="c8f72-229">The most common scenario for creating and using a `PhysicalFileProvider` is to request an `IFileProvider` in a constructor through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="c8f72-230">Al crear una instancia de este proveedor directamente, se requiere una ruta de acceso del directorio, que actúa como la ruta de acceso base para todas las solicitudes realizadas usando el proveedor.</span><span class="sxs-lookup"><span data-stu-id="c8f72-230">When instantiating this provider directly, a directory path is required and serves as the base path for all requests made using the provider.</span></span>

<span data-ttu-id="c8f72-231">El código siguiente muestra cómo crear `PhysicalFileProvider` y usarlo para obtener el contenido del directorio y la información del archivo:</span><span class="sxs-lookup"><span data-stu-id="c8f72-231">The following code shows how to create a `PhysicalFileProvider` and use it to obtain directory contents and file information:</span></span>

```csharp
var provider = new PhysicalFileProvider(applicationRoot);
var contents = provider.GetDirectoryContents(string.Empty);
var fileInfo = provider.GetFileInfo("wwwroot/js/site.js");
```

<span data-ttu-id="c8f72-232">Tipos del ejemplo anterior:</span><span class="sxs-lookup"><span data-stu-id="c8f72-232">Types in the preceding example:</span></span>

* <span data-ttu-id="c8f72-233">`provider` es `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="c8f72-233">`provider` is an `IFileProvider`.</span></span>
* <span data-ttu-id="c8f72-234">`contents` es `IDirectoryContents`.</span><span class="sxs-lookup"><span data-stu-id="c8f72-234">`contents` is an `IDirectoryContents`.</span></span>
* <span data-ttu-id="c8f72-235">`fileInfo` es `IFileInfo`.</span><span class="sxs-lookup"><span data-stu-id="c8f72-235">`fileInfo` is an `IFileInfo`.</span></span>

<span data-ttu-id="c8f72-236">El proveedor de archivos puede usarse para iterar por el directorio especificado por `applicationRoot` o llamar a `GetFileInfo` para obtener información de un archivo.</span><span class="sxs-lookup"><span data-stu-id="c8f72-236">The File Provider can be used to iterate through the directory specified by `applicationRoot` or call `GetFileInfo` to obtain a file's information.</span></span> <span data-ttu-id="c8f72-237">El proveedor de archivos no tiene acceso fuera del directorio `applicationRoot`.</span><span class="sxs-lookup"><span data-stu-id="c8f72-237">The File Provider has no access outside of the `applicationRoot` directory.</span></span>

<span data-ttu-id="c8f72-238">La aplicación de ejemplo crea el proveedor en la clase `Startup.ConfigureServices` de la aplicación mediante [IHostingEnvironment.ContentRootFileProvider](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootFileProvider):</span><span class="sxs-lookup"><span data-stu-id="c8f72-238">The sample app creates the provider in the app's `Startup.ConfigureServices` class using [IHostingEnvironment.ContentRootFileProvider](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootFileProvider):</span></span>

```csharp
var physicalProvider = _env.ContentRootFileProvider;
```

### <a name="manifestembeddedfileprovider"></a><span data-ttu-id="c8f72-239">ManifestEmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="c8f72-239">ManifestEmbeddedFileProvider</span></span>

<span data-ttu-id="c8f72-240"><xref:Microsoft.Extensions.FileProviders.ManifestEmbeddedFileProvider> se utiliza para acceder a archivos insertados dentro de ensamblados.</span><span class="sxs-lookup"><span data-stu-id="c8f72-240">The <xref:Microsoft.Extensions.FileProviders.ManifestEmbeddedFileProvider> is used to access files embedded within assemblies.</span></span> <span data-ttu-id="c8f72-241">`ManifestEmbeddedFileProvider` utiliza un manifiesto compilado en el ensamblado para reconstruir las rutas de acceso originales de los archivos incrustados.</span><span class="sxs-lookup"><span data-stu-id="c8f72-241">The `ManifestEmbeddedFileProvider` uses a manifest compiled into the assembly to reconstruct the original paths of the embedded files.</span></span>

<span data-ttu-id="c8f72-242">Para generar un manifiesto de los archivos incrustados, establezca la propiedad `<GenerateEmbeddedFilesManifest>` en `true`.</span><span class="sxs-lookup"><span data-stu-id="c8f72-242">To generate a manifest of the embedded files, set the `<GenerateEmbeddedFilesManifest>` property to `true`.</span></span> <span data-ttu-id="c8f72-243">Especifique los archivos que desea incrustar con [&lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):</span><span class="sxs-lookup"><span data-stu-id="c8f72-243">Specify the files to embed with [&lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/FileProviderSample.csproj?highlight=6,14)]

<span data-ttu-id="c8f72-244">Use [patrones globales](#glob-patterns) para especificar uno o varios archivos para incrustar en el ensamblado.</span><span class="sxs-lookup"><span data-stu-id="c8f72-244">Use [glob patterns](#glob-patterns) to specify one or more files to embed into the assembly.</span></span>

<span data-ttu-id="c8f72-245">La aplicación de ejemplo crea `ManifestEmbeddedFileProvider` y pasa el ensamblado que se está ejecutando actualmente a su constructor.</span><span class="sxs-lookup"><span data-stu-id="c8f72-245">The sample app creates an `ManifestEmbeddedFileProvider` and passes the currently executing assembly to its constructor.</span></span>

<span data-ttu-id="c8f72-246">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="c8f72-246">*Startup.cs*:</span></span>

```csharp
var manifestEmbeddedProvider = 
    new ManifestEmbeddedFileProvider(Assembly.GetEntryAssembly());
```

<span data-ttu-id="c8f72-247">Las sobrecargas adicionales le permiten:</span><span class="sxs-lookup"><span data-stu-id="c8f72-247">Additional overloads allow you to:</span></span>

* <span data-ttu-id="c8f72-248">Especificar una ruta de acceso de archivo relativa.</span><span class="sxs-lookup"><span data-stu-id="c8f72-248">Specify a relative file path.</span></span>
* <span data-ttu-id="c8f72-249">Definir el ámbito de archivos a la fecha de la última modificación.</span><span class="sxs-lookup"><span data-stu-id="c8f72-249">Scope files to a last modified date.</span></span>
* <span data-ttu-id="c8f72-250">Asignar nombre al recurso incrustado que contiene el manifiesto del archivo incrustado.</span><span class="sxs-lookup"><span data-stu-id="c8f72-250">Name the embedded resource containing the embedded file manifest.</span></span>

| <span data-ttu-id="c8f72-251">Sobrecarga</span><span class="sxs-lookup"><span data-stu-id="c8f72-251">Overload</span></span> | <span data-ttu-id="c8f72-252">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="c8f72-252">Description</span></span> |
| -------- | ----------- |
| `ManifestEmbeddedFileProvider(Assembly, String)` | <span data-ttu-id="c8f72-253">Acepta parámetro de ruta de acceso relativa `root` opcional.</span><span class="sxs-lookup"><span data-stu-id="c8f72-253">Accepts an optional `root` relative path parameter.</span></span> <span data-ttu-id="c8f72-254">Especifique `root` para definir el ámbito de las llamadas en <xref:Microsoft.Extensions.FileProviders.IFileProvider.GetDirectoryContents*> en aquellos recursos que se encuentran bajo las rutas de acceso proporcionadas.</span><span class="sxs-lookup"><span data-stu-id="c8f72-254">Specify the `root` to scope calls to <xref:Microsoft.Extensions.FileProviders.IFileProvider.GetDirectoryContents*> to those resources under the provided path.</span></span> |
| `ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)` | <span data-ttu-id="c8f72-255">Acepta un parámetro de ruta de acceso relativa `root` opcional y un parámetro de fecha `lastModified` (<xref:System.DateTimeOffset>).</span><span class="sxs-lookup"><span data-stu-id="c8f72-255">Accepts an optional `root` relative path parameter and a `lastModified` date (<xref:System.DateTimeOffset>) parameter.</span></span> <span data-ttu-id="c8f72-256">La fecha `lastModified` define el ámbito de la última fecha de modificación para las instancias de <xref:Microsoft.Extensions.FileProviders.IFileInfo> que <xref:Microsoft.Extensions.FileProviders.IFileProvider> devuelve.</span><span class="sxs-lookup"><span data-stu-id="c8f72-256">The `lastModified` date scopes the last modification date for the <xref:Microsoft.Extensions.FileProviders.IFileInfo> instances returned by the <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span></span> |
| `ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)` | <span data-ttu-id="c8f72-257">Acepta una ruta de acceso relativa `root` opcional, una fecha `lastModified` y parámetros `manifestName`.</span><span class="sxs-lookup"><span data-stu-id="c8f72-257">Accepts an optional `root` relative path, `lastModified` date, and `manifestName` parameters.</span></span> <span data-ttu-id="c8f72-258">`manifestName` representa el nombre del recurso incrustado que contiene el manifiesto.</span><span class="sxs-lookup"><span data-stu-id="c8f72-258">The `manifestName` represents the name of the embedded resource containing the manifest.</span></span> |

### <a name="compositefileprovider"></a><span data-ttu-id="c8f72-259">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="c8f72-259">CompositeFileProvider</span></span>

<span data-ttu-id="c8f72-260"><xref:Microsoft.Extensions.FileProviders.CompositeFileProvider> combina instancias de `IFileProvider`, y expone una única interfaz para trabajar con archivos de varios proveedores.</span><span class="sxs-lookup"><span data-stu-id="c8f72-260">The <xref:Microsoft.Extensions.FileProviders.CompositeFileProvider> combines `IFileProvider` instances, exposing a single interface for working with files from multiple providers.</span></span> <span data-ttu-id="c8f72-261">Al crear `CompositeFileProvider`, se pasan una o varias instancias de `IFileProvider` a su constructor.</span><span class="sxs-lookup"><span data-stu-id="c8f72-261">When creating the `CompositeFileProvider`, pass one or more `IFileProvider` instances to its constructor.</span></span>

<span data-ttu-id="c8f72-262">En la aplicación de ejemplo, `PhysicalFileProvider` y `ManifestEmbeddedFileProvider` proporcionan archivos a un `CompositeFileProvider` registrado en el contenedor de servicios de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="c8f72-262">In the sample app, a `PhysicalFileProvider` and a `ManifestEmbeddedFileProvider` provide files to a `CompositeFileProvider` registered in the app's service container:</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/Startup.cs?name=snippet1)]

## <a name="watch-for-changes"></a><span data-ttu-id="c8f72-263">Observación de cambios</span><span class="sxs-lookup"><span data-stu-id="c8f72-263">Watch for changes</span></span>

<span data-ttu-id="c8f72-264">El método [IFileProvider.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) proporciona un escenario para ver uno o más archivos o directorios para detectar cambios.</span><span class="sxs-lookup"><span data-stu-id="c8f72-264">The [IFileProvider.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) method provides a scenario to watch one or more files or directories for changes.</span></span> <span data-ttu-id="c8f72-265">`Watch` acepta una cadena de ruta de acceso, que puede usar [patrones globales](#glob-patterns) para especificar varios archivos.</span><span class="sxs-lookup"><span data-stu-id="c8f72-265">`Watch` accepts a path string, which can use [glob patterns](#glob-patterns) to specify multiple files.</span></span> <span data-ttu-id="c8f72-266">`Watch` devuelve un valor de <xref:Microsoft.Extensions.Primitives.IChangeToken>.</span><span class="sxs-lookup"><span data-stu-id="c8f72-266">`Watch` returns an <xref:Microsoft.Extensions.Primitives.IChangeToken>.</span></span> <span data-ttu-id="c8f72-267">El token de cambio expone:</span><span class="sxs-lookup"><span data-stu-id="c8f72-267">The change token exposes:</span></span>

* <span data-ttu-id="c8f72-268"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> &ndash; una propiedad que se puede inspeccionar para determinar si se ha producido un cambio.</span><span class="sxs-lookup"><span data-stu-id="c8f72-268"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> &ndash; A property that can be inspected to determine if a change has occurred.</span></span>
* <span data-ttu-id="c8f72-269"><xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*> &ndash; se llama cuando se detectan cambios en la cadena de ruta de acceso especificada.</span><span class="sxs-lookup"><span data-stu-id="c8f72-269"><xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*> &ndash; Called when changes are detected to the specified path string.</span></span> <span data-ttu-id="c8f72-270">Cada token de cambio solo llama a su devolución de llamada asociada en respuesta a un único cambio.</span><span class="sxs-lookup"><span data-stu-id="c8f72-270">Each change token only calls its associated callback in response to a single change.</span></span> <span data-ttu-id="c8f72-271">Para habilitar la supervisión constante, puede usar <xref:System.Threading.Tasks.TaskCompletionSource`1> (como se muestra a continuación) o volver a crear instancias de `IChangeToken` en respuesta a los cambios.</span><span class="sxs-lookup"><span data-stu-id="c8f72-271">To enable constant monitoring, use a <xref:System.Threading.Tasks.TaskCompletionSource`1> (shown below) or recreate `IChangeToken` instances in response to changes.</span></span>

<span data-ttu-id="c8f72-272">En la aplicación de ejemplo, la aplicación de consola *WatchConsole* se configura para mostrar un mensaje cada vez que se modifica un archivo de texto:</span><span class="sxs-lookup"><span data-stu-id="c8f72-272">In the sample app, the *WatchConsole* console app is configured to display a message whenever a text file is modified:</span></span>

[!code-csharp[](file-providers/samples/2.x/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

<span data-ttu-id="c8f72-273">Algunos sistemas de archivos, como contenedores de Docker y recursos compartidos de red, no pueden enviar notificaciones de cambio de forma confiable.</span><span class="sxs-lookup"><span data-stu-id="c8f72-273">Some file systems, such as Docker containers and network shares, may not reliably send change notifications.</span></span> <span data-ttu-id="c8f72-274">Establezca la variable de entorno `DOTNET_USE_POLLING_FILE_WATCHER` en `1` o `true` para sondear el sistema de archivos en busca de cambios cada cuatro segundos (no configurable).</span><span class="sxs-lookup"><span data-stu-id="c8f72-274">Set the `DOTNET_USE_POLLING_FILE_WATCHER` environment variable to `1` or `true` to poll the file system for changes every four seconds (not configurable).</span></span>

## <a name="glob-patterns"></a><span data-ttu-id="c8f72-275">Patrones globales</span><span class="sxs-lookup"><span data-stu-id="c8f72-275">Glob patterns</span></span>

<span data-ttu-id="c8f72-276">Las rutas de acceso del sistema de archivos utilizan patrones de caracteres comodín denominados *patrones globales*.</span><span class="sxs-lookup"><span data-stu-id="c8f72-276">File system paths use wildcard patterns called *glob (or globbing) patterns*.</span></span> <span data-ttu-id="c8f72-277">Especifique grupos de archivos con estos patrones.</span><span class="sxs-lookup"><span data-stu-id="c8f72-277">Specify groups of files with these patterns.</span></span> <span data-ttu-id="c8f72-278">Los dos caracteres comodín son `*` y `**`:</span><span class="sxs-lookup"><span data-stu-id="c8f72-278">The two wildcard characters are `*` and `**`:</span></span>

**`*`**  
<span data-ttu-id="c8f72-279">Coincide con cualquier elemento del nivel de carpeta actual, con cualquier nombre de archivo o con cualquier extensión de archivo.</span><span class="sxs-lookup"><span data-stu-id="c8f72-279">Matches anything at the current folder level, any filename, or any file extension.</span></span> <span data-ttu-id="c8f72-280">Las coincidencias finalizan con los caracteres `/` y `.` en la ruta de acceso de archivo.</span><span class="sxs-lookup"><span data-stu-id="c8f72-280">Matches are terminated by `/` and `.` characters in the file path.</span></span>

**`**`**  
<span data-ttu-id="c8f72-281">Coincide con cualquier elemento de varios niveles de directorios.</span><span class="sxs-lookup"><span data-stu-id="c8f72-281">Matches anything across multiple directory levels.</span></span> <span data-ttu-id="c8f72-282">Puede usarse para coincidir de forma recursiva con muchos archivos dentro de una jerarquía de directorios.</span><span class="sxs-lookup"><span data-stu-id="c8f72-282">Can be used to recursively match many files within a directory hierarchy.</span></span>

<span data-ttu-id="c8f72-283">**Ejemplos de patrones globales**</span><span class="sxs-lookup"><span data-stu-id="c8f72-283">**Glob pattern examples**</span></span>

**`directory/file.txt`**  
<span data-ttu-id="c8f72-284">Coincide con un archivo concreto en un directorio específico.</span><span class="sxs-lookup"><span data-stu-id="c8f72-284">Matches a specific file in a specific directory.</span></span>

**`directory/*.txt`**  
<span data-ttu-id="c8f72-285">Coincide con todos los archivos que tengan la extensión *.txt* en un directorio específico.</span><span class="sxs-lookup"><span data-stu-id="c8f72-285">Matches all files with *.txt* extension in a specific directory.</span></span>

**`directory/*/appsettings.json`**  
<span data-ttu-id="c8f72-286">Coincide con todos los archivos `appsettings.json` que estén en directorios exactamente un nivel por debajo de la carpeta *directorio*.</span><span class="sxs-lookup"><span data-stu-id="c8f72-286">Matches all `appsettings.json` files in directories exactly one level below the *directory* folder.</span></span>

**`directory/**/*.txt`**  
<span data-ttu-id="c8f72-287">Coincide con todos los archivos que tengan la extensión *.txt* y se encuentren en cualquier lugar de la carpeta *directorio*.</span><span class="sxs-lookup"><span data-stu-id="c8f72-287">Matches all files with *.txt* extension found anywhere under the *directory* folder.</span></span>

::: moniker-end
