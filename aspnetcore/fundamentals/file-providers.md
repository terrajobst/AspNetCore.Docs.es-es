---
title: Proveedores de archivo en ASP.NET Core
author: ardalis
description: Obtenga información sobre cómo ASP.NET Core abstrae el acceso al sistema de archivos mediante el uso de proveedores de archivos.
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/file-providers
ms.openlocfilehash: cdbffdadd9616fe941809d67dc2c0bbd52149561
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2018
ms.locfileid: "29724576"
---
# <a name="file-providers-in-aspnet-core"></a><span data-ttu-id="2ebdd-103">Proveedores de archivo en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2ebdd-103">File Providers in ASP.NET Core</span></span>

<span data-ttu-id="2ebdd-104">Por [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="2ebdd-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="2ebdd-105">ASP.NET Core abstrae el acceso al sistema de archivos mediante el uso de proveedores de archivos.</span><span class="sxs-lookup"><span data-stu-id="2ebdd-105">ASP.NET Core abstracts file system access through the use of File Providers.</span></span>

<span data-ttu-id="2ebdd-106">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2ebdd-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="file-provider-abstractions"></a><span data-ttu-id="2ebdd-107">Abstracciones de proveedores de archivos</span><span class="sxs-lookup"><span data-stu-id="2ebdd-107">File Provider abstractions</span></span>

<span data-ttu-id="2ebdd-108">Los proveedores de archivos son una abstracción respecto a los sistemas de archivos.</span><span class="sxs-lookup"><span data-stu-id="2ebdd-108">File Providers are an abstraction over file systems.</span></span> <span data-ttu-id="2ebdd-109">La interfaz principal es `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="2ebdd-109">The main interface is `IFileProvider`.</span></span> <span data-ttu-id="2ebdd-110">`IFileProvider` expone métodos para obtener información de archivos (`IFileInfo`) y de directorios (`IDirectoryContents`), y para configurar las notificaciones de cambio (con `IChangeToken`).</span><span class="sxs-lookup"><span data-stu-id="2ebdd-110">`IFileProvider` exposes methods to get file information (`IFileInfo`), directory information (`IDirectoryContents`), and to set up change notifications (using an `IChangeToken`).</span></span>

<span data-ttu-id="2ebdd-111">`IFileInfo` proporciona métodos y propiedades de archivos o directorios individuales.</span><span class="sxs-lookup"><span data-stu-id="2ebdd-111">`IFileInfo` provides methods and properties about individual files or directories.</span></span> <span data-ttu-id="2ebdd-112">Tiene dos propiedades booleanas, `Exists` y `IsDirectory`, así como propiedades que describen el archivo, como `Name`, `Length` (en bytes) y la fecha `LastModified`.</span><span class="sxs-lookup"><span data-stu-id="2ebdd-112">It has two boolean properties, `Exists` and `IsDirectory`, as well as properties describing the file's `Name`, `Length` (in bytes), and `LastModified` date.</span></span> <span data-ttu-id="2ebdd-113">Se puede leer en el archivo mediante el método `CreateReadStream`.</span><span class="sxs-lookup"><span data-stu-id="2ebdd-113">You can read from the file using its `CreateReadStream` method.</span></span>

## <a name="file-provider-implementations"></a><span data-ttu-id="2ebdd-114">Implementaciones del proveedor de archivos</span><span class="sxs-lookup"><span data-stu-id="2ebdd-114">File Provider implementations</span></span>

<span data-ttu-id="2ebdd-115">Hay disponibles tres implementaciones de `IFileProvider`: física, insertada y compuesta.</span><span class="sxs-lookup"><span data-stu-id="2ebdd-115">Three implementations of `IFileProvider` are available: Physical, Embedded, and Composite.</span></span> <span data-ttu-id="2ebdd-116">El proveedor físico se utiliza para tener acceso a los archivos del sistema real.</span><span class="sxs-lookup"><span data-stu-id="2ebdd-116">The physical provider is used to access the actual system's files.</span></span> <span data-ttu-id="2ebdd-117">El proveedor insertado se utiliza para tener acceso a archivos insertados en ensamblados.</span><span class="sxs-lookup"><span data-stu-id="2ebdd-117">The embedded provider is used to access files embedded in assemblies.</span></span> <span data-ttu-id="2ebdd-118">El proveedor compuesto se utiliza para proporcionar acceso combinado a archivos y directorios de uno o más proveedores.</span><span class="sxs-lookup"><span data-stu-id="2ebdd-118">The composite provider is used to provide combined access to files and directories from one or more other providers.</span></span>

### <a name="physicalfileprovider"></a><span data-ttu-id="2ebdd-119">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="2ebdd-119">PhysicalFileProvider</span></span>

<span data-ttu-id="2ebdd-120">`PhysicalFileProvider` proporciona acceso al sistema de archivos físico.</span><span class="sxs-lookup"><span data-stu-id="2ebdd-120">The `PhysicalFileProvider` provides access to the physical file system.</span></span> <span data-ttu-id="2ebdd-121">Encapsula el tipo `System.IO.File` (para el proveedor físico), definiendo el ámbito de todas las rutas de acceso a un directorio y sus elementos secundarios.</span><span class="sxs-lookup"><span data-stu-id="2ebdd-121">It wraps the `System.IO.File` type (for the physical provider), scoping all paths to a directory and its children.</span></span> <span data-ttu-id="2ebdd-122">Este ámbito limita el acceso a un directorio determinado y sus elementos secundarios, impidiendo el acceso al sistema de archivos fuera de este límite.</span><span class="sxs-lookup"><span data-stu-id="2ebdd-122">This scoping limits access to a certain directory and its children, preventing access to the file system outside of this boundary.</span></span> <span data-ttu-id="2ebdd-123">Al crear una instancia de este proveedor, debe proporcionarle una ruta de acceso de directorio, que actúa como la ruta de acceso base para todas las solicitudes realizadas a este proveedor (y que restringe el acceso fuera de esta ruta de acceso).</span><span class="sxs-lookup"><span data-stu-id="2ebdd-123">When instantiating this provider, you must provide it with a directory path, which serves as the base path for all requests made to this provider (and which restricts access outside of this path).</span></span> <span data-ttu-id="2ebdd-124">En una aplicación ASP.NET Core, puede crear instancias de un proveedor `PhysicalFileProvider` directamente o puede solicitar un proveedor `IFileProvider` de un controlador o constructor del servicio a través de [inserción de dependencias](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="2ebdd-124">In an ASP.NET Core app, you can instantiate a `PhysicalFileProvider` provider directly, or you can request an `IFileProvider` in a Controller or service's constructor through [dependency injection](dependency-injection.md).</span></span> <span data-ttu-id="2ebdd-125">Por lo general, el último enfoque brindará una solución más flexible y más fácil de probar.</span><span class="sxs-lookup"><span data-stu-id="2ebdd-125">The latter approach will typically yield a more flexible and testable solution.</span></span>

<span data-ttu-id="2ebdd-126">En el ejemplo siguiente se muestra cómo crear `PhysicalFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="2ebdd-126">The sample below shows how to create a `PhysicalFileProvider`.</span></span>


```csharp
IFileProvider provider = new PhysicalFileProvider(applicationRoot);
IDirectoryContents contents = provider.GetDirectoryContents(""); // the applicationRoot contents
IFileInfo fileInfo = provider.GetFileInfo("wwwroot/js/site.js"); // a file under applicationRoot
```

<span data-ttu-id="2ebdd-127">Para recorrer en iteración el contenido del directorio u obtener información de un archivo específico, puede proporcionar una subruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="2ebdd-127">You can iterate through its directory contents or get a specific file's information by providing a subpath.</span></span>

<span data-ttu-id="2ebdd-128">Para solicitar un proveedor de un controlador, especifíquelo en el constructor del controlador y asígnelo a un campo local.</span><span class="sxs-lookup"><span data-stu-id="2ebdd-128">To request a provider from a controller, specify it in the controller's constructor and assign it to a local field.</span></span> <span data-ttu-id="2ebdd-129">Utilice la instancia local de los métodos de acción:</span><span class="sxs-lookup"><span data-stu-id="2ebdd-129">Use the local instance from your action methods:</span></span>

[!code-csharp[](file-providers/sample/src/FileProviderSample/Controllers/HomeController.cs?highlight=5,7,12&range=6-19)]

<span data-ttu-id="2ebdd-130">A continuación, cree el proveedor en la clase `Startup` de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="2ebdd-130">Then, create the provider in the app's `Startup` class:</span></span>

[!code-csharp[](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=35,40&range=1-43)]

<span data-ttu-id="2ebdd-131">En la vista *Index.cshtml*, recorrer en iteración el `IDirectoryContents` proporcionado:</span><span class="sxs-lookup"><span data-stu-id="2ebdd-131">In the *Index.cshtml* view, iterate through the `IDirectoryContents` provided:</span></span>

[!code-html[](file-providers/sample/src/FileProviderSample/Views/Home/Index.cshtml?highlight=2,7,9,11,15)]

<span data-ttu-id="2ebdd-132">Resultado:</span><span class="sxs-lookup"><span data-stu-id="2ebdd-132">The result:</span></span>

![Aplicación de ejemplo de proveedor de archivos donde se enumeran las carpetas y los archivos físicos](file-providers/_static/physical-directory-listing.png)

### <a name="embeddedfileprovider"></a><span data-ttu-id="2ebdd-134">EmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="2ebdd-134">EmbeddedFileProvider</span></span>

<span data-ttu-id="2ebdd-135">`EmbeddedFileProvider` se utiliza para tener acceso a archivos insertados en ensamblados.</span><span class="sxs-lookup"><span data-stu-id="2ebdd-135">The `EmbeddedFileProvider` is used to access files embedded in assemblies.</span></span> <span data-ttu-id="2ebdd-136">En .NET Core, se insertan archivos en un ensamblado con el elemento `<EmbeddedResource>` en el archivo *.csproj*:</span><span class="sxs-lookup"><span data-stu-id="2ebdd-136">In .NET Core, you embed files in an assembly with the `<EmbeddedResource>` element in the *.csproj* file:</span></span>

[!code-json[](file-providers/sample/src/FileProviderSample/FileProviderSample.csproj?range=13-18)]

<span data-ttu-id="2ebdd-137">Puede usar [patrones de comodines](#globbing-patterns) para especificar archivos que se insertarán en el ensamblado.</span><span class="sxs-lookup"><span data-stu-id="2ebdd-137">You can use [globbing patterns](#globbing-patterns) when specifying files to embed in the assembly.</span></span> <span data-ttu-id="2ebdd-138">Estos patrones se pueden usar para que coincidan con uno o más archivos.</span><span class="sxs-lookup"><span data-stu-id="2ebdd-138">These patterns can be used to match one or more files.</span></span>

> [!NOTE]
> <span data-ttu-id="2ebdd-139">Es improbable que alguna vez quiera insertar cada archivo .js del proyecto en su ensamblado; el ejemplo anterior se incluye solo con fines de demostración.</span><span class="sxs-lookup"><span data-stu-id="2ebdd-139">It's unlikely you would ever want to actually embed every .js file in your project in its assembly; the above sample is for demo purposes only.</span></span>

<span data-ttu-id="2ebdd-140">Al crear `EmbeddedFileProvider`, pase el ensamblado que va a leer a su constructor.</span><span class="sxs-lookup"><span data-stu-id="2ebdd-140">When creating an `EmbeddedFileProvider`, pass the assembly it will read to its constructor.</span></span>

```csharp
var embeddedProvider = new EmbeddedFileProvider(Assembly.GetEntryAssembly());
```

<span data-ttu-id="2ebdd-141">El fragmento de código anterior muestra cómo crear `EmbeddedFileProvider` con acceso al ensamblado que se está ejecutando actualmente.</span><span class="sxs-lookup"><span data-stu-id="2ebdd-141">The snippet above demonstrates how to create an `EmbeddedFileProvider` with access to the currently executing assembly.</span></span>

<span data-ttu-id="2ebdd-142">La actualización de la aplicación de ejemplo para usar `EmbeddedFileProvider` genera la siguiente salida:</span><span class="sxs-lookup"><span data-stu-id="2ebdd-142">Updating the sample app to use an `EmbeddedFileProvider` results in the following output:</span></span>

![Aplicación de ejemplo de proveedor de archivos donde se enumeran los archivos insertados](file-providers/_static/embedded-directory-listing.png)

> [!NOTE]
> <span data-ttu-id="2ebdd-144">Los recursos insertados no exponen directorios.</span><span class="sxs-lookup"><span data-stu-id="2ebdd-144">Embedded resources don't expose directories.</span></span> <span data-ttu-id="2ebdd-145">En su lugar, la ruta de acceso al recurso (a través de su espacio de nombres) se inserta en su nombre de archivo con separadores `.`.</span><span class="sxs-lookup"><span data-stu-id="2ebdd-145">Rather, the path to the resource (via its namespace) is embedded in its filename using `.` separators.</span></span>

> [!TIP]
> <span data-ttu-id="2ebdd-146">El constructor `EmbeddedFileProvider` acepta un parámetro `baseNamespace` opcional.</span><span class="sxs-lookup"><span data-stu-id="2ebdd-146">The `EmbeddedFileProvider` constructor accepts an optional `baseNamespace` parameter.</span></span> <span data-ttu-id="2ebdd-147">Si se especifica este parámetro, las llamadas a `GetDirectoryContents` tendrán como ámbito esos recursos en el espacio de nombres proporcionado.</span><span class="sxs-lookup"><span data-stu-id="2ebdd-147">Specifying this will scope calls to `GetDirectoryContents` to those resources under the provided namespace.</span></span>

### <a name="compositefileprovider"></a><span data-ttu-id="2ebdd-148">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="2ebdd-148">CompositeFileProvider</span></span>

<span data-ttu-id="2ebdd-149">`CompositeFileProvider` combina instancias de `IFileProvider`, y expone una única interfaz para trabajar con archivos de varios proveedores.</span><span class="sxs-lookup"><span data-stu-id="2ebdd-149">The `CompositeFileProvider` combines `IFileProvider` instances, exposing a single interface for working with files from multiple providers.</span></span> <span data-ttu-id="2ebdd-150">Al crear `CompositeFileProvider`, se pasan una o varias instancias de `IFileProvider` a su constructor:</span><span class="sxs-lookup"><span data-stu-id="2ebdd-150">When creating the `CompositeFileProvider`, you pass one or more `IFileProvider` instances to its constructor:</span></span>

[!code-csharp[](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=3&range=35-37)]

<span data-ttu-id="2ebdd-151">Actualizar la aplicación de ejemplo para que use un proveedor `CompositeFileProvider` que incluya los proveedores físicos y los insertados que se configuraron anteriormente, produce la siguiente salida:</span><span class="sxs-lookup"><span data-stu-id="2ebdd-151">Updating the sample app to use a `CompositeFileProvider` that includes both the physical and embedded providers configured previously, results in the following output:</span></span>

![Aplicación de ejemplo de proveedor de archivos donde se enumeran las carpetas y los archivos físicos, y los archivos insertados](file-providers/_static/composite-directory-listing.png)

## <a name="watching-for-changes"></a><span data-ttu-id="2ebdd-153">Observación de cambios</span><span class="sxs-lookup"><span data-stu-id="2ebdd-153">Watching for changes</span></span>

<span data-ttu-id="2ebdd-154">El método `IFileProvider` `Watch` proporciona una manera de ver uno o más archivos o directorios para detectar cambios.</span><span class="sxs-lookup"><span data-stu-id="2ebdd-154">The `IFileProvider` `Watch` method provides a way to watch one or more files or directories for changes.</span></span> <span data-ttu-id="2ebdd-155">Este método acepta una cadena de ruta de acceso, que puede usar [patrones de comodines](#globbing-patterns) para especificar varios archivos y devuelve un token `IChangeToken`.</span><span class="sxs-lookup"><span data-stu-id="2ebdd-155">This method accepts a path string, which can use [globbing patterns](#globbing-patterns) to specify multiple files, and returns an `IChangeToken`.</span></span> <span data-ttu-id="2ebdd-156">Este token expone una propiedad `HasChanged` que se puede inspeccionar, así como un método `RegisterChangeCallback` al que se llama cuando se detectan cambios en la cadena de ruta de acceso especificada.</span><span class="sxs-lookup"><span data-stu-id="2ebdd-156">This token exposes a `HasChanged` property that can be inspected, and a `RegisterChangeCallback` method that's called when changes are detected to the specified path string.</span></span> <span data-ttu-id="2ebdd-157">Tenga en cuenta que cada token de cambio solo llama a su devolución de llamada asociada en respuesta a un único cambio.</span><span class="sxs-lookup"><span data-stu-id="2ebdd-157">Note that each change token only calls its associated callback in response to a single change.</span></span> <span data-ttu-id="2ebdd-158">Para habilitar la supervisión constante, puede usar `TaskCompletionSource` tal y como se muestra a continuación, o volver a crear instancias de `IChangeToken` en respuesta a los cambios.</span><span class="sxs-lookup"><span data-stu-id="2ebdd-158">To enable constant monitoring, you can use a `TaskCompletionSource` as shown below, or re-create `IChangeToken` instances in response to changes.</span></span>

<span data-ttu-id="2ebdd-159">En el ejemplo de este artículo, se configura una aplicación de consola para mostrar un mensaje cada vez que se modifica un archivo de texto:</span><span class="sxs-lookup"><span data-stu-id="2ebdd-159">In this article's sample, a console application is configured to display a message whenever a text file is modified:</span></span>

[!code-csharp[](file-providers/sample/src/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

<span data-ttu-id="2ebdd-160">El resultado, después de guardar el archivo varias veces:</span><span class="sxs-lookup"><span data-stu-id="2ebdd-160">The result, after saving the file several times:</span></span>

![Ventana Comandos después de ejecutar dotnet run muestra la aplicación supervisando el archivo quotes.txt para detectar cambios. El archivo ha cambiado cinco veces.](file-providers/_static/watch-console.png)

> [!NOTE]
> <span data-ttu-id="2ebdd-162">Algunos sistemas de archivos, como contenedores de Docker y recursos compartidos de red, no pueden enviar notificaciones de cambio de forma confiable.</span><span class="sxs-lookup"><span data-stu-id="2ebdd-162">Some file systems, such as Docker containers and network shares, may not reliably send change notifications.</span></span> <span data-ttu-id="2ebdd-163">Establezca la variable de entorno `DOTNET_USE_POLLINGFILEWATCHER` en `1` o `true` para sondear el sistema de archivos en busca de cambios cada 4 segundos.</span><span class="sxs-lookup"><span data-stu-id="2ebdd-163">Set the `DOTNET_USE_POLLINGFILEWATCHER` environment variable to `1` or `true` to poll the file system for changes every 4 seconds.</span></span>

## <a name="globbing-patterns"></a><span data-ttu-id="2ebdd-164">Patrones de comodines</span><span class="sxs-lookup"><span data-stu-id="2ebdd-164">Globbing patterns</span></span>

<span data-ttu-id="2ebdd-165">Las rutas de acceso del sistema de archivos utilizan patrones de caracteres comodín denominados *patrones de comodines*.</span><span class="sxs-lookup"><span data-stu-id="2ebdd-165">File system paths use wildcard patterns called *globbing patterns*.</span></span> <span data-ttu-id="2ebdd-166">Estos sencillos modelos se pueden usar para especificar grupos de archivos.</span><span class="sxs-lookup"><span data-stu-id="2ebdd-166">These simple patterns can be used to specify groups of files.</span></span> <span data-ttu-id="2ebdd-167">Los dos caracteres comodín son `*` y `**`.</span><span class="sxs-lookup"><span data-stu-id="2ebdd-167">The two wildcard characters are `*` and `**`.</span></span>

**`*`**

   <span data-ttu-id="2ebdd-168">Coincide con cualquier elemento del nivel de carpeta actual, con cualquier nombre de archivo o con cualquier extensión de archivo.</span><span class="sxs-lookup"><span data-stu-id="2ebdd-168">Matches anything at the current folder level, or any filename, or any file extension.</span></span> <span data-ttu-id="2ebdd-169">Las coincidencias finalizan con los caracteres `/` y `.` en la ruta de acceso de archivo.</span><span class="sxs-lookup"><span data-stu-id="2ebdd-169">Matches are terminated by `/` and `.` characters in the file path.</span></span>

<strong><code>**</code></strong>

   <span data-ttu-id="2ebdd-170">Coincide con cualquier elemento de varios niveles de directorios.</span><span class="sxs-lookup"><span data-stu-id="2ebdd-170">Matches anything across multiple directory levels.</span></span> <span data-ttu-id="2ebdd-171">Puede usarse para coincidir de forma recursiva con muchos archivos dentro de una jerarquía de directorios.</span><span class="sxs-lookup"><span data-stu-id="2ebdd-171">Can be used to recursively match many files within a directory hierarchy.</span></span>

### <a name="globbing-pattern-examples"></a><span data-ttu-id="2ebdd-172">Ejemplos de patrones de uso de comodines</span><span class="sxs-lookup"><span data-stu-id="2ebdd-172">Globbing pattern examples</span></span>

**`directory/file.txt`**

   <span data-ttu-id="2ebdd-173">Coincide con un archivo concreto en un directorio específico.</span><span class="sxs-lookup"><span data-stu-id="2ebdd-173">Matches a specific file in a specific directory.</span></span>

**<code>directory/*.txt</code>**

   <span data-ttu-id="2ebdd-174">Coincide con todos los archivos que tengan la extensión `.txt` en un directorio específico.</span><span class="sxs-lookup"><span data-stu-id="2ebdd-174">Matches all files with `.txt` extension in a specific directory.</span></span>

**`directory/*/bower.json`**

   <span data-ttu-id="2ebdd-175">Coincide con todos los archivos `bower.json` que estén en directorios exactamente un nivel por debajo del directorio `directory`.</span><span class="sxs-lookup"><span data-stu-id="2ebdd-175">Matches all `bower.json` files in directories exactly one level below the `directory` directory.</span></span>

**<code>directory/&#42;&#42;/&#42;.txt</code>**

   <span data-ttu-id="2ebdd-176">Coincide con todos los archivos cuya extensión sea `.txt` y se encuentren en cualquier lugar del directorio `directory`.</span><span class="sxs-lookup"><span data-stu-id="2ebdd-176">Matches all files with `.txt` extension found anywhere under the `directory` directory.</span></span>

## <a name="file-provider-usage-in-aspnet-core"></a><span data-ttu-id="2ebdd-177">Uso de proveedor de archivos en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2ebdd-177">File Provider usage in ASP.NET Core</span></span>

<span data-ttu-id="2ebdd-178">Varias partes de ASP.NET Core usan proveedores de archivos.</span><span class="sxs-lookup"><span data-stu-id="2ebdd-178">Several parts of ASP.NET Core utilize file providers.</span></span> <span data-ttu-id="2ebdd-179">`IHostingEnvironment` expone la raíz del contenido de la aplicación y la raíz web como tipos `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="2ebdd-179">`IHostingEnvironment` exposes the app's content root and web root as `IFileProvider` types.</span></span> <span data-ttu-id="2ebdd-180">El middleware de archivos estáticos usa proveedores de archivos para buscar archivos estáticos.</span><span class="sxs-lookup"><span data-stu-id="2ebdd-180">The static files middleware uses file providers to locate static files.</span></span> <span data-ttu-id="2ebdd-181">Razor hace un uso intensivo de `IFileProvider` en la localización de vistas.</span><span class="sxs-lookup"><span data-stu-id="2ebdd-181">Razor makes heavy use of `IFileProvider` in locating views.</span></span> <span data-ttu-id="2ebdd-182">La funcionalidad de publicación de Dotnet usa proveedores de archivos y patrones de comodines para especificar los archivos que deben publicarse.</span><span class="sxs-lookup"><span data-stu-id="2ebdd-182">Dotnet's publish functionality uses file providers and globbing patterns to specify which files should be published.</span></span>

## <a name="recommendations-for-use-in-apps"></a><span data-ttu-id="2ebdd-183">Recomendaciones para su uso en aplicaciones</span><span class="sxs-lookup"><span data-stu-id="2ebdd-183">Recommendations for use in apps</span></span>

<span data-ttu-id="2ebdd-184">Si su aplicación de ASP.NET Core requiere acceso al sistema de archivos, puede solicitar una instancia de `IFileProvider` a través de la inserción de dependencias y, a continuación, utilizar sus métodos para realizar el acceso, tal como se muestra en este ejemplo.</span><span class="sxs-lookup"><span data-stu-id="2ebdd-184">If your ASP.NET Core app requires file system access, you can request an instance of `IFileProvider` through dependency injection, and then use its methods to perform the access, as shown in this sample.</span></span> <span data-ttu-id="2ebdd-185">Esto le permite configurar el proveedor una sola vez cuando se inicia la aplicación, y reduce el número de tipos de implementación de los que la aplicación crea instancias.</span><span class="sxs-lookup"><span data-stu-id="2ebdd-185">This allows you to configure the provider once, when the app starts up, and reduces the number of implementation types your app instantiates.</span></span>
