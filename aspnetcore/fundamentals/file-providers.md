---
title: "Proveedores de archivo en el núcleo de ASP.NET"
author: ardalis
description: "Obtenga información acerca de cómo ASP.NET Core abstrae el acceso al sistema de archivos mediante el uso de proveedores de archivos."
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: 1e35d362-0005-4f84-a187-274ca203a787
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/file-providers
ms.openlocfilehash: fd847db992b20ab096b54378418d2b9bccff67be
ms.sourcegitcommit: 2b263e87217658caa42eedc4f9d2d21ef0ab5d59
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/12/2018
---
# <a name="file-providers-in-aspnet-core"></a><span data-ttu-id="7494f-104">Proveedores de archivo en el núcleo de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="7494f-104">File Providers in ASP.NET Core</span></span>

<span data-ttu-id="7494f-105">Por [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="7494f-105">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="7494f-106">ASP.NET Core abstrae el acceso al sistema de archivos mediante el uso de proveedores de archivos.</span><span class="sxs-lookup"><span data-stu-id="7494f-106">ASP.NET Core abstracts file system access through the use of File Providers.</span></span>

<span data-ttu-id="7494f-107">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7494f-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="file-provider-abstractions"></a><span data-ttu-id="7494f-108">Abstracciones de proveedor de archivo</span><span class="sxs-lookup"><span data-stu-id="7494f-108">File Provider abstractions</span></span>

<span data-ttu-id="7494f-109">Los proveedores de archivo son una abstracción sobre los sistemas de archivos.</span><span class="sxs-lookup"><span data-stu-id="7494f-109">File Providers are an abstraction over file systems.</span></span> <span data-ttu-id="7494f-110">La interfaz principal es `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="7494f-110">The main interface is `IFileProvider`.</span></span> <span data-ttu-id="7494f-111">`IFileProvider`expone métodos para obtener información de archivo (`IFileInfo`), información de directorio (`IDirectoryContents`) y para configurar las notificaciones de cambio (con un `IChangeToken`).</span><span class="sxs-lookup"><span data-stu-id="7494f-111">`IFileProvider` exposes methods to get file information (`IFileInfo`), directory information (`IDirectoryContents`), and to set up change notifications (using an `IChangeToken`).</span></span>

<span data-ttu-id="7494f-112">`IFileInfo`Proporciona métodos y propiedades de los archivos o directorios.</span><span class="sxs-lookup"><span data-stu-id="7494f-112">`IFileInfo` provides methods and properties about individual files or directories.</span></span> <span data-ttu-id="7494f-113">Tiene dos propiedades booleanas, `Exists` y `IsDirectory`, así como las propiedades que describa el archivo `Name`, `Length` (en bytes), y `LastModified` fecha.</span><span class="sxs-lookup"><span data-stu-id="7494f-113">It has two boolean properties, `Exists` and `IsDirectory`, as well as properties describing the file's `Name`, `Length` (in bytes), and `LastModified` date.</span></span> <span data-ttu-id="7494f-114">Puede leer en el archivo mediante su `CreateReadStream` método.</span><span class="sxs-lookup"><span data-stu-id="7494f-114">You can read from the file using its `CreateReadStream` method.</span></span>

## <a name="file-provider-implementations"></a><span data-ttu-id="7494f-115">Implementaciones del proveedor de archivo</span><span class="sxs-lookup"><span data-stu-id="7494f-115">File Provider implementations</span></span>

<span data-ttu-id="7494f-116">Las tres implementaciones de `IFileProvider` están disponibles: física, incrustada y compuesto.</span><span class="sxs-lookup"><span data-stu-id="7494f-116">Three implementations of `IFileProvider` are available: Physical, Embedded, and Composite.</span></span> <span data-ttu-id="7494f-117">El proveedor físico se utiliza para tener acceso a archivos del sistema real.</span><span class="sxs-lookup"><span data-stu-id="7494f-117">The physical provider is used to access the actual system's files.</span></span> <span data-ttu-id="7494f-118">El proveedor incrustado se utiliza para tener acceso a archivos incrustados en ensamblados.</span><span class="sxs-lookup"><span data-stu-id="7494f-118">The embedded provider is used to access files embedded in assemblies.</span></span> <span data-ttu-id="7494f-119">El proveedor compuesto se utiliza para proporcionar acceso combinado a archivos y directorios de uno o varios proveedores.</span><span class="sxs-lookup"><span data-stu-id="7494f-119">The composite provider is used to provide combined access to files and directories from one or more other providers.</span></span>

### <a name="physicalfileprovider"></a><span data-ttu-id="7494f-120">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="7494f-120">PhysicalFileProvider</span></span>

<span data-ttu-id="7494f-121">El `PhysicalFileProvider` proporciona acceso al sistema de archivos físico.</span><span class="sxs-lookup"><span data-stu-id="7494f-121">The `PhysicalFileProvider` provides access to the physical file system.</span></span> <span data-ttu-id="7494f-122">Encapsula el `System.IO.File` tipo (para el proveedor físico), definir el ámbito de todas las rutas de acceso a un directorio y sus elementos secundarios.</span><span class="sxs-lookup"><span data-stu-id="7494f-122">It wraps the `System.IO.File` type (for the physical provider), scoping all paths to a directory and its children.</span></span> <span data-ttu-id="7494f-123">Este ámbito limita el acceso a un directorio determinado y sus elementos secundarios, impidiendo el acceso al sistema de archivos fuera de este límite.</span><span class="sxs-lookup"><span data-stu-id="7494f-123">This scoping limits access to a certain directory and its children, preventing access to the file system outside of this boundary.</span></span> <span data-ttu-id="7494f-124">Al crear una instancia de este proveedor, debe proporcionar con una ruta de acceso de directorio, que actúa como la ruta de acceso base para todas las solicitudes realizadas a este proveedor (y que restringe el acceso fuera de esta ruta de acceso).</span><span class="sxs-lookup"><span data-stu-id="7494f-124">When instantiating this provider, you must provide it with a directory path, which serves as the base path for all requests made to this provider (and which restricts access outside of this path).</span></span> <span data-ttu-id="7494f-125">En una aplicación ASP.NET Core, puede crear instancias de un `PhysicalFileProvider` proveedor directamente o se puede solicitar un `IFileProvider` en un controlador o el constructor del servicio a través de [inyección de dependencia](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="7494f-125">In an ASP.NET Core app, you can instantiate a `PhysicalFileProvider` provider directly, or you can request an `IFileProvider` in a Controller or service's constructor through [dependency injection](dependency-injection.md).</span></span> <span data-ttu-id="7494f-126">El último enfoque normalmente producirá una solución más flexible y pueden someterse a prueba.</span><span class="sxs-lookup"><span data-stu-id="7494f-126">The latter approach will typically yield a more flexible and testable solution.</span></span>

<span data-ttu-id="7494f-127">El ejemplo siguiente muestra cómo crear un `PhysicalFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="7494f-127">The sample below shows how to create a `PhysicalFileProvider`.</span></span>


```csharp
IFileProvider provider = new PhysicalFileProvider(applicationRoot);
IDirectoryContents contents = provider.GetDirectoryContents(""); // the applicationRoot contents
IFileInfo fileInfo = provider.GetFileInfo("wwwroot/js/site.js"); // a file under applicationRoot
```

<span data-ttu-id="7494f-128">Puede recorrer en iteración su contenido del directorio u obtener información de un archivo específico, proporcionando una subruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="7494f-128">You can iterate through its directory contents or get a specific file's information by providing a subpath.</span></span>

<span data-ttu-id="7494f-129">Para solicitar un proveedor de un controlador, especifíquela en el constructor del controlador y asignarlo a un campo local.</span><span class="sxs-lookup"><span data-stu-id="7494f-129">To request a provider from a controller, specify it in the controller's constructor and assign it to a local field.</span></span> <span data-ttu-id="7494f-130">Utilice la instancia local de los métodos de acción:</span><span class="sxs-lookup"><span data-stu-id="7494f-130">Use the local instance from your action methods:</span></span>

[!code-csharp[Main](file-providers/sample/src/FileProviderSample/Controllers/HomeController.cs?highlight=5,7,12&range=6-19)]

<span data-ttu-id="7494f-131">A continuación, cree el proveedor en la aplicación `Startup` clase:</span><span class="sxs-lookup"><span data-stu-id="7494f-131">Then, create the provider in the app's `Startup` class:</span></span>

[!code-csharp[Main](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=35,40&range=1-43)]

<span data-ttu-id="7494f-132">En el *Index.cshtml* ver, recorrer en iteración el `IDirectoryContents` proporciona:</span><span class="sxs-lookup"><span data-stu-id="7494f-132">In the *Index.cshtml* view, iterate through the `IDirectoryContents` provided:</span></span>

[!code-html[Main](file-providers/sample/src/FileProviderSample/Views/Home/Index.cshtml?highlight=2,7,9,11,15)]

<span data-ttu-id="7494f-133">El resultado:</span><span class="sxs-lookup"><span data-stu-id="7494f-133">The result:</span></span>

![Archivos de lista de aplicación de ejemplo de proveedor físicos y carpetas de archivos](file-providers/_static/physical-directory-listing.png)

### <a name="embeddedfileprovider"></a><span data-ttu-id="7494f-135">EmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="7494f-135">EmbeddedFileProvider</span></span>

<span data-ttu-id="7494f-136">El `EmbeddedFileProvider` se utiliza para tener acceso a archivos incrustados en ensamblados.</span><span class="sxs-lookup"><span data-stu-id="7494f-136">The `EmbeddedFileProvider` is used to access files embedded in assemblies.</span></span> <span data-ttu-id="7494f-137">En .NET Core, incrustar archivos en un ensamblado con el `<EmbeddedResource>` elemento en el *.csproj* archivo:</span><span class="sxs-lookup"><span data-stu-id="7494f-137">In .NET Core, you embed files in an assembly with the `<EmbeddedResource>` element in the *.csproj* file:</span></span>

[!code-json[Main](file-providers/sample/src/FileProviderSample/FileProviderSample.csproj?range=13-18)]

<span data-ttu-id="7494f-138">Puede usar [patrones de uso de comodines](#globbing-patterns) al especificar archivos para incrustar en el ensamblado.</span><span class="sxs-lookup"><span data-stu-id="7494f-138">You can use [globbing patterns](#globbing-patterns) when specifying files to embed in the assembly.</span></span> <span data-ttu-id="7494f-139">Estos modelos se pueden utilizar para que coincida con uno o varios archivos.</span><span class="sxs-lookup"><span data-stu-id="7494f-139">These patterns can be used to match one or more files.</span></span>

> [!NOTE]
> <span data-ttu-id="7494f-140">Es improbable que alguna vez desearía realmente incrustar cada archivo .js en el proyecto en el ensamblado; el ejemplo anterior es solo con fines de demostración.</span><span class="sxs-lookup"><span data-stu-id="7494f-140">It's unlikely you would ever want to actually embed every .js file in your project in its assembly; the above sample is for demo purposes only.</span></span>

<span data-ttu-id="7494f-141">Al crear un `EmbeddedFileProvider`, pase el ensamblado que va a leer a su constructor.</span><span class="sxs-lookup"><span data-stu-id="7494f-141">When creating an `EmbeddedFileProvider`, pass the assembly it will read to its constructor.</span></span>

```csharp
var embeddedProvider = new EmbeddedFileProvider(Assembly.GetEntryAssembly());
```

<span data-ttu-id="7494f-142">El fragmento de código anterior muestra cómo crear un `EmbeddedFileProvider` con acceso al ensamblado que se está ejecutando actualmente.</span><span class="sxs-lookup"><span data-stu-id="7494f-142">The snippet above demonstrates how to create an `EmbeddedFileProvider` with access to the currently executing assembly.</span></span>

<span data-ttu-id="7494f-143">Actualizando la aplicación de ejemplo para usar un `EmbeddedFileProvider` da como resultado el siguiente resultado:</span><span class="sxs-lookup"><span data-stu-id="7494f-143">Updating the sample app to use an `EmbeddedFileProvider` results in the following output:</span></span>

![Aplicación de ejemplo de proveedor de archivo enumerar archivos incrustados](file-providers/_static/embedded-directory-listing.png)

> [!NOTE]
> <span data-ttu-id="7494f-145">Los recursos incrustados no exponen directorios.</span><span class="sxs-lookup"><span data-stu-id="7494f-145">Embedded resources do not expose directories.</span></span> <span data-ttu-id="7494f-146">En su lugar, se incrusta la ruta de acceso al recurso (a través de su espacio de nombres) en su nombre de archivo con `.` separadores.</span><span class="sxs-lookup"><span data-stu-id="7494f-146">Rather, the path to the resource (via its namespace) is embedded in its filename using `.` separators.</span></span>

> [!TIP]
> <span data-ttu-id="7494f-147">El `EmbeddedFileProvider` constructor acepta opcional `baseNamespace` parámetro.</span><span class="sxs-lookup"><span data-stu-id="7494f-147">The `EmbeddedFileProvider` constructor accepts an optional `baseNamespace` parameter.</span></span> <span data-ttu-id="7494f-148">Especificar este ámbito aplicará las llamadas a `GetDirectoryContents` a esos recursos en el espacio de nombres proporcionado.</span><span class="sxs-lookup"><span data-stu-id="7494f-148">Specifying this will scope calls to `GetDirectoryContents` to those resources under the provided namespace.</span></span>

### <a name="compositefileprovider"></a><span data-ttu-id="7494f-149">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="7494f-149">CompositeFileProvider</span></span>

<span data-ttu-id="7494f-150">El `CompositeFileProvider` combina `IFileProvider` instancias, exponer una única interfaz para trabajar con archivos de varios proveedores.</span><span class="sxs-lookup"><span data-stu-id="7494f-150">The `CompositeFileProvider` combines `IFileProvider` instances, exposing a single interface for working with files from multiple providers.</span></span> <span data-ttu-id="7494f-151">Al crear el `CompositeFileProvider`, pasar uno o varios `IFileProvider` instancias a su constructor:</span><span class="sxs-lookup"><span data-stu-id="7494f-151">When creating the `CompositeFileProvider`, you pass one or more `IFileProvider` instances to its constructor:</span></span>

[!code-csharp[Main](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=3&range=35-37)]

<span data-ttu-id="7494f-152">Actualizando la aplicación de ejemplo para usar un `CompositeFileProvider` que incluye los proveedores físicos e incrustados configurados anteriormente, aparecerá el siguiente resultado:</span><span class="sxs-lookup"><span data-stu-id="7494f-152">Updating the sample app to use a `CompositeFileProvider` that includes both the physical and embedded providers configured previously, results in the following output:</span></span>

![Aplicación de ejemplo de proveedor de archivo enumerar archivos físicos y las carpetas y archivos incrustados](file-providers/_static/composite-directory-listing.png)

## <a name="watching-for-changes"></a><span data-ttu-id="7494f-154">Ver cambios</span><span class="sxs-lookup"><span data-stu-id="7494f-154">Watching for changes</span></span>

<span data-ttu-id="7494f-155">El `IFileProvider` `Watch` método proporciona una manera de ver uno o más archivos o directorios para detectar cambios.</span><span class="sxs-lookup"><span data-stu-id="7494f-155">The `IFileProvider` `Watch` method provides a way to watch one or more files or directories for changes.</span></span> <span data-ttu-id="7494f-156">Este método acepta una cadena de ruta de acceso, que puede usar [patrones de uso de comodines en](#globbing-patterns) para especificar varios archivos y devuelve un `IChangeToken`.</span><span class="sxs-lookup"><span data-stu-id="7494f-156">This method accepts a path string, which can use [globbing patterns](#globbing-patterns) to specify multiple files, and returns an `IChangeToken`.</span></span> <span data-ttu-id="7494f-157">Este token expone un `HasChanged` propiedad que se puede inspeccionar, y un `RegisterChangeCallback` método al que se llama cuando se detectan cambios en la cadena de ruta de acceso especificada.</span><span class="sxs-lookup"><span data-stu-id="7494f-157">This token exposes a `HasChanged` property that can be inspected, and a `RegisterChangeCallback` method that is called when changes are detected to the specified path string.</span></span> <span data-ttu-id="7494f-158">Tenga en cuenta que cada token de cambio sólo llama a su devolución de llamada asociada en respuesta a un único cambio.</span><span class="sxs-lookup"><span data-stu-id="7494f-158">Note that each change token only calls its associated callback in response to a single change.</span></span> <span data-ttu-id="7494f-159">Para habilitar la supervisión constante, puede usar un `TaskCompletionSource` tal y como se muestra a continuación, o volver a crear `IChangeToken` instancias en respuesta a los cambios.</span><span class="sxs-lookup"><span data-stu-id="7494f-159">To enable constant monitoring, you can use a `TaskCompletionSource` as shown below, or re-create `IChangeToken` instances in response to changes.</span></span>

<span data-ttu-id="7494f-160">En el ejemplo de este artículo, se configura una aplicación de consola para mostrar un mensaje cada vez que se modifica un archivo de texto:</span><span class="sxs-lookup"><span data-stu-id="7494f-160">In this article's sample, a console application is configured to display a message whenever a text file is modified:</span></span>

[!code-csharp[Main](file-providers/sample/src/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

<span data-ttu-id="7494f-161">El resultado, después de guardar el archivo varias veces:</span><span class="sxs-lookup"><span data-stu-id="7494f-161">The result, after saving the file several times:</span></span>

![Ventana de comandos después de ejecutar dotnet ejecutar muestra el archivo quotes.txt para cambios de supervisión de aplicaciones y el archivo ha cambiado cinco veces.](file-providers/_static/watch-console.png)

> [!NOTE]
> <span data-ttu-id="7494f-163">Algunos sistemas de archivos, como contenedores de Docker y recursos compartidos de red, no pueden enviar notificaciones de cambio de forma confiable.</span><span class="sxs-lookup"><span data-stu-id="7494f-163">Some file systems, such as Docker containers and network shares, may not reliably send change notifications.</span></span> <span data-ttu-id="7494f-164">Establecer el `DOTNET_USE_POLLINGFILEWATCHER` variable de entorno `1` o `true` para sondear el sistema de archivos para los cambios cada 4 segundos.</span><span class="sxs-lookup"><span data-stu-id="7494f-164">Set the `DOTNET_USE_POLLINGFILEWATCHER` environment variable to `1` or `true` to poll the file system for changes every 4 seconds.</span></span>

## <a name="globbing-patterns"></a><span data-ttu-id="7494f-165">Patrones de uso de comodines</span><span class="sxs-lookup"><span data-stu-id="7494f-165">Globbing patterns</span></span>

<span data-ttu-id="7494f-166">Las rutas de acceso del sistema de archivos utilizan patrones de caracteres comodín denominados *patrones de uso de comodines en*.</span><span class="sxs-lookup"><span data-stu-id="7494f-166">File system paths use wildcard patterns called *globbing patterns*.</span></span> <span data-ttu-id="7494f-167">Estos modelos simples se pueden utilizar para especificar los grupos de archivos.</span><span class="sxs-lookup"><span data-stu-id="7494f-167">These simple patterns can be used to specify groups of files.</span></span> <span data-ttu-id="7494f-168">Los dos caracteres comodín son `*` y `**`.</span><span class="sxs-lookup"><span data-stu-id="7494f-168">The two wildcard characters are `*` and `**`.</span></span>

**`*`**

   <span data-ttu-id="7494f-169">Coincide con nada en el nivel de carpeta actual, o cualquier nombre de archivo o cualquier extensión de archivo.</span><span class="sxs-lookup"><span data-stu-id="7494f-169">Matches anything at the current folder level, or any filename, or any file extension.</span></span> <span data-ttu-id="7494f-170">Se finalizan coincidencias `/` y `.` caracteres en la ruta de acceso de archivo.</span><span class="sxs-lookup"><span data-stu-id="7494f-170">Matches are terminated by `/` and `.` characters in the file path.</span></span>

<strong><code>**</code></strong>

   <span data-ttu-id="7494f-171">Coincide con ninguna cadena a través de varios niveles de directorios.</span><span class="sxs-lookup"><span data-stu-id="7494f-171">Matches anything across multiple directory levels.</span></span> <span data-ttu-id="7494f-172">Puede usarse para recursivamente coincide con muchos archivos dentro de una jerarquía de directorios.</span><span class="sxs-lookup"><span data-stu-id="7494f-172">Can be used to recursively match many files within a directory hierarchy.</span></span>

### <a name="globbing-pattern-examples"></a><span data-ttu-id="7494f-173">Ejemplos de patrones de uso de comodines</span><span class="sxs-lookup"><span data-stu-id="7494f-173">Globbing pattern examples</span></span>

**`directory/file.txt`**

   <span data-ttu-id="7494f-174">Coincide con un archivo concreto en un directorio específico.</span><span class="sxs-lookup"><span data-stu-id="7494f-174">Matches a specific file in a specific directory.</span></span>

**<code>directory/*.txt</code>**

   <span data-ttu-id="7494f-175">Coincide con todos los archivos con `.txt` extensión en un directorio específico.</span><span class="sxs-lookup"><span data-stu-id="7494f-175">Matches all files with `.txt` extension in a specific directory.</span></span>

**`directory/*/bower.json`**

   <span data-ttu-id="7494f-176">Coincide con todos los `bower.json` archivos en directorios exactamente un nivel por debajo del `directory` directory.</span><span class="sxs-lookup"><span data-stu-id="7494f-176">Matches all `bower.json` files in directories exactly one level below the `directory` directory.</span></span>

**<code>directory/&#42;&#42;/&#42;.txt</code>**

   <span data-ttu-id="7494f-177">Coincide con todos los archivos con `.txt` extensión se encuentra en cualquier lugar en el `directory` directory.</span><span class="sxs-lookup"><span data-stu-id="7494f-177">Matches all files with `.txt` extension found anywhere under the `directory` directory.</span></span>

## <a name="file-provider-usage-in-aspnet-core"></a><span data-ttu-id="7494f-178">Uso de proveedor de los archivos en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7494f-178">File Provider usage in ASP.NET Core</span></span>

<span data-ttu-id="7494f-179">Varias partes de ASP.NET Core usan proveedores de archivos.</span><span class="sxs-lookup"><span data-stu-id="7494f-179">Several parts of ASP.NET Core utilize file providers.</span></span> <span data-ttu-id="7494f-180">`IHostingEnvironment`expone la aplicación raíz del contenido y la raíz web como `IFileProvider` tipos.</span><span class="sxs-lookup"><span data-stu-id="7494f-180">`IHostingEnvironment` exposes the app's content root and web root as `IFileProvider` types.</span></span> <span data-ttu-id="7494f-181">El middleware de archivos estáticos usa proveedores de archivos para buscar archivos estáticos.</span><span class="sxs-lookup"><span data-stu-id="7494f-181">The static files middleware uses file providers to locate static files.</span></span> <span data-ttu-id="7494f-182">Razor hace un uso intensivo de `IFileProvider` en la localización de vistas.</span><span class="sxs-lookup"><span data-stu-id="7494f-182">Razor makes heavy use of `IFileProvider` in locating views.</span></span> <span data-ttu-id="7494f-183">Dotnet publica funcionalidad usa archivo proveedores y patrones de uso de comodines para especificar qué archivos deben publicarse.</span><span class="sxs-lookup"><span data-stu-id="7494f-183">Dotnet's publish functionality uses file providers and globbing patterns to specify which files should be published.</span></span>

## <a name="recommendations-for-use-in-apps"></a><span data-ttu-id="7494f-184">Recomendaciones para su uso en aplicaciones</span><span class="sxs-lookup"><span data-stu-id="7494f-184">Recommendations for use in apps</span></span>

<span data-ttu-id="7494f-185">Si su aplicación de ASP.NET Core requiere acceso al sistema de archivos, puede solicitar una instancia de `IFileProvider` a través de la inyección de dependencia y, a continuación, utilizar sus métodos para realizar el acceso, tal como se muestra en este ejemplo.</span><span class="sxs-lookup"><span data-stu-id="7494f-185">If your ASP.NET Core app requires file system access, you can request an instance of `IFileProvider` through dependency injection, and then use its methods to perform the access, as shown in this sample.</span></span> <span data-ttu-id="7494f-186">Esto le permite configurar el proveedor de una vez, cuando se inicia la aplicación y reduce el número de tipos de implementación que crea una instancia de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7494f-186">This allows you to configure the provider once, when the app starts up, and reduces the number of implementation types your app instantiates.</span></span>
