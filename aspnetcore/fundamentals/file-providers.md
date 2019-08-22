---
title: Proveedores de archivo en ASP.NET Core
author: guardrex
description: Obtenga información sobre cómo ASP.NET Core abstrae el acceso al sistema de archivos mediante el uso de proveedores de archivos.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/30/2019
uid: fundamentals/file-providers
ms.openlocfilehash: b93b2df7fad7c173f43ad69aec865f09de6c9c34
ms.sourcegitcommit: 7a46973998623aead757ad386fe33602b1658793
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/15/2019
ms.locfileid: "69487576"
---
# <a name="file-providers-in-aspnet-core"></a>Proveedores de archivo en ASP.NET Core

Por [Steve Smith](https://ardalis.com/) y [Luke Latham](https://github.com/guardrex)

ASP.NET Core abstrae el acceso al sistema de archivos mediante el uso de proveedores de archivos. Los proveedores de archivos se usan en el marco de ASP.NET Core:

* [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) expone la raíz del contenido de la aplicación y la raíz web como tipos `IFileProvider`.
* El [middleware de archivos estáticos](xref:fundamentals/static-files) usa proveedores de archivos para buscar archivos estáticos.
* [Razor](xref:mvc/views/razor) usa proveedores de archivos para localizar páginas y vistas.
* Las herramientas de .NET Core usan proveedores de archivos y patrones globales para especificar los archivos que deben publicarse.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))

## <a name="file-provider-interfaces"></a>Interfaces de proveedor de archivos

La interfaz principal es [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider). `IFileProvider` expone métodos para:

* Obtener información de archivo ([IFileInfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo)).
* Obtener información de directorio ([IDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.idirectorycontents)).
* Configurar las notificaciones de cambio (mediante [IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken)).

`IFileInfo` proporciona métodos y propiedades para trabajar con archivos:

* [Exists](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.exists)
* [IsDirectory](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.isdirectory)
* [Name](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.name)
* [Length](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.length) (en bytes)
* Fecha [LastModified](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.lastmodified)

Puede leer del archivo mediante el método [IFileInfo.CreateReadStream](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.createreadstream).

La aplicación de ejemplo muestra cómo configurar un proveedor de archivos en `Startup.ConfigureServices` para su uso en toda la aplicación a través de la [inserción de dependencias](xref:fundamentals/dependency-injection).

## <a name="file-provider-implementations"></a>Implementaciones del proveedor de archivos

Hay tres implementaciones de `IFileProvider` disponibles.

| Implementación | DESCRIPCIÓN |
| -------------- | ----------- |
| [PhysicalFileProvider](#physicalfileprovider) | El proveedor físico se utiliza para acceder a los archivos físicos del sistema. |
| [ManifestEmbeddedFileProvider](#manifestembeddedfileprovider) | El proveedor insertado de manifiestos se utiliza para tener acceder a archivos insertados en ensamblados. |
| [CompositeFileProvider](#compositefileprovider) | El proveedor compuesto se utiliza para proporcionar acceso combinado a archivos y directorios de uno o más proveedores. |

### <a name="physicalfileprovider"></a>PhysicalFileProvider

[PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider) proporciona acceso al sistema de archivos físico. `PhysicalFileProvider` usa el tipo [System.IO.File](/dotnet/api/system.io.file) (para el proveedor físico) y define el ámbito de todas las rutas de acceso a un directorio y sus elementos secundarios. Esta definición de ámbito impide el acceso al sistema de archivos fuera del directorio especificado y sus elementos secundarios. Al crear una instancia de este proveedor, se requiere una ruta de acceso del directorio, que actúa como la ruta de acceso base para todas las solicitudes realizadas usando el proveedor. Puede crear instancias de un proveedor `PhysicalFileProvider` directamente o puede solicitar `IFileProvider` en un constructor a través de [inserción de dependencias](xref:fundamentals/dependency-injection).

**Tipos estáticos**

El código siguiente muestra cómo crear `PhysicalFileProvider` y usarlo para obtener el contenido del directorio y la información del archivo:

```csharp
var provider = new PhysicalFileProvider(applicationRoot);
var contents = provider.GetDirectoryContents(string.Empty);
var fileInfo = provider.GetFileInfo("wwwroot/js/site.js");
```

Tipos del ejemplo anterior:

* `provider` es `IFileProvider`.
* `contents` es `IDirectoryContents`.
* `fileInfo` es `IFileInfo`.

El proveedor de archivos puede usarse para iterar por el directorio especificado por `applicationRoot` o llamar a `GetFileInfo` para obtener información de un archivo. El proveedor de archivos no tiene acceso fuera del directorio `applicationRoot`.

La aplicación de ejemplo crea el proveedor en la clase `Startup.ConfigureServices` de la aplicación mediante [IHostingEnvironment.ContentRootFileProvider](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.contentrootfileprovider):

```csharp
var physicalProvider = _env.ContentRootFileProvider;
```

**Obtención de tipos de proveedor de archivos con inserción de dependencias**

Inserte el proveedor en cualquier constructor de clase y asígnelo a un campo local. Utilice el campo en todos los métodos de la clase para acceder a archivos.

En la aplicación de ejemplo, la clase `IndexModel` recibe una instancia `IFileProvider` para obtener el contenido del directorio para la ruta de acceso base de la aplicación.

*Páginas/Index.cshtml.cs*:

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/Pages/Index.cshtml.cs?name=snippet1)]

`IDirectoryContents` se iteran en la página.

*Páginas/Index.cshtml*:

[!code-cshtml[](file-providers/samples/2.x/FileProviderSample/Pages/Index.cshtml?name=snippet1)]

### <a name="manifestembeddedfileprovider"></a>ManifestEmbeddedFileProvider

[ManifestEmbeddedFileProvider](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider) se utiliza para acceder a archivos incrustados dentro de ensamblados. `ManifestEmbeddedFileProvider` utiliza un manifiesto compilado en el ensamblado para reconstruir las rutas de acceso originales de los archivos incrustados.

Para generar un manifiesto de los archivos incrustados, establezca la propiedad `<GenerateEmbeddedFilesManifest>` en `true`. Especifique los archivos que desea incrustar con [&lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/FileProviderSample.csproj?highlight=6,14)]

Use [patrones globales](#glob-patterns) para especificar uno o varios archivos para incrustar en el ensamblado.

La aplicación de ejemplo crea `ManifestEmbeddedFileProvider` y pasa el ensamblado que se está ejecutando actualmente a su constructor.

*Startup.cs*:

```csharp
var manifestEmbeddedProvider = 
    new ManifestEmbeddedFileProvider(Assembly.GetEntryAssembly());
```

Las sobrecargas adicionales le permiten:

* Especificar una ruta de acceso de archivo relativa.
* Definir el ámbito de archivos a la fecha de la última modificación.
* Asignar nombre al recurso incrustado que contiene el manifiesto del archivo incrustado.

| Sobrecarga | DESCRIPCIÓN |
| -------- | ----------- |
| [ManifestEmbeddedFileProvider(Assembly, String)](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_ManifestEmbeddedFileProvider__ctor_System_Reflection_Assembly_System_String_) | Acepta parámetro de ruta de acceso relativa `root` opcional. Especifique `root` para definir el ámbito de las llamadas en [GetDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.getdirectorycontents) en aquellos recursos que se encuentran bajo las rutas de acceso proporcionadas. |
| [ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_ManifestEmbeddedFileProvider__ctor_System_Reflection_Assembly_System_String_System_DateTimeOffset_) | Acepta un parámetro de ruta de acceso relativa `root` opcional y un parámetro de fecha `lastModified` ([DateTimeOffset](/dotnet/api/system.datetimeoffset)). La fecha `lastModified` define el ámbito de la última fecha de modificación para las instancias de [IFileInfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo) que [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider) devuelve. |
| [ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_ManifestEmbeddedFileProvider__ctor_System_Reflection_Assembly_System_String_System_String_System_DateTimeOffset_) | Acepta una ruta de acceso relativa `root` opcional, una fecha `lastModified` y parámetros `manifestName`. `manifestName` representa el nombre del recurso incrustado que contiene el manifiesto. |

### <a name="compositefileprovider"></a>CompositeFileProvider

[CompositeFileProvider](/dotnet/api/microsoft.extensions.fileproviders.compositefileprovider) combina instancias de `IFileProvider` y expone una única interfaz para trabajar con archivos de varios proveedores. Al crear `CompositeFileProvider`, se pasan una o varias instancias de `IFileProvider` a su constructor.

En la aplicación de ejemplo, `PhysicalFileProvider` y `ManifestEmbeddedFileProvider` proporcionan archivos a un `CompositeFileProvider` registrado en el contenedor de servicios de la aplicación:

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/Startup.cs?name=snippet1)]

## <a name="watch-for-changes"></a>Observación de cambios

El método [IFileProvider.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) proporciona un escenario para ver uno o más archivos o directorios para detectar cambios. `Watch` acepta una cadena de ruta de acceso, que puede usar [patrones globales](#glob-patterns) para especificar varios archivos. `Watch` devuelve [IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken). El token de cambio expone:

* [HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged): una propiedad que se puede inspeccionar para determinar si se ha producido un cambio.
* [RegisterChangeCallback](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback): se llama cuando se detectan cambios en la cadena de ruta de acceso especificada. Cada token de cambio solo llama a su devolución de llamada asociada en respuesta a un único cambio. Para habilitar la supervisión constante, puede usar [TaskCompletionSource](/dotnet/api/system.threading.tasks.taskcompletionsource-1) (como se muestra a continuación) o volver a crear instancias de `IChangeToken` en respuesta a los cambios.

En la aplicación de ejemplo, la aplicación de consola *WatchConsole* se configura para mostrar un mensaje cada vez que se modifica un archivo de texto:

[!code-csharp[](file-providers/samples/2.x/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

Algunos sistemas de archivos, como contenedores de Docker y recursos compartidos de red, no pueden enviar notificaciones de cambio de forma confiable. Establezca la variable de entorno `DOTNET_USE_POLLING_FILE_WATCHER` en `1` o `true` para sondear el sistema de archivos en busca de cambios cada cuatro segundos (no configurable).

## <a name="glob-patterns"></a>Patrones globales

Las rutas de acceso del sistema de archivos utilizan patrones de caracteres comodín denominados *patrones globales*. Especifique grupos de archivos con estos patrones. Los dos caracteres comodín son `*` y `**`:

**`*`**  
Coincide con cualquier elemento del nivel de carpeta actual, con cualquier nombre de archivo o con cualquier extensión de archivo. Las coincidencias finalizan con los caracteres `/` y `.` en la ruta de acceso de archivo.

**`**`**  
Coincide con cualquier elemento de varios niveles de directorios. Puede usarse para coincidir de forma recursiva con muchos archivos dentro de una jerarquía de directorios.

**Ejemplos de patrones globales**

**`directory/file.txt`**  
Coincide con un archivo concreto en un directorio específico.

**`directory/*.txt`**  
Coincide con todos los archivos que tengan la extensión *.txt* en un directorio específico.

**`directory/*/appsettings.json`**  
Coincide con todos los archivos `appsettings.json` que estén en directorios exactamente un nivel por debajo de la carpeta *directorio*.

**`directory/**/*.txt`**  
Coincide con todos los archivos que tengan la extensión *.txt* y se encuentren en cualquier lugar de la carpeta *directorio*.
