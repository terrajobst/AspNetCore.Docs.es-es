---
title: "Proveedores de archivo en el núcleo de ASP.NET"
author: ardalis
description: "Obtenga información acerca de cómo ASP.NET Core abstrae el acceso al sistema de archivos mediante el uso de proveedores de archivos."
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/file-providers
ms.openlocfilehash: 10f3276d3e71e8a29b452d4c62865cbb82298513
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
# <a name="file-providers-in-aspnet-core"></a>Proveedores de archivo en el núcleo de ASP.NET

Por [Steve Smith](https://ardalis.com/)

ASP.NET Core abstrae el acceso al sistema de archivos mediante el uso de proveedores de archivos.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

## <a name="file-provider-abstractions"></a>Abstracciones de proveedor de archivo

Los proveedores de archivo son una abstracción sobre los sistemas de archivos. La interfaz principal es `IFileProvider`. `IFileProvider`expone métodos para obtener información de archivo (`IFileInfo`), información de directorio (`IDirectoryContents`) y para configurar las notificaciones de cambio (con un `IChangeToken`).

`IFileInfo`Proporciona métodos y propiedades de los archivos o directorios. Tiene dos propiedades booleanas, `Exists` y `IsDirectory`, así como las propiedades que describa el archivo `Name`, `Length` (en bytes), y `LastModified` fecha. Puede leer en el archivo mediante su `CreateReadStream` método.

## <a name="file-provider-implementations"></a>Implementaciones del proveedor de archivo

Las tres implementaciones de `IFileProvider` están disponibles: física, incrustada y compuesto. El proveedor físico se utiliza para tener acceso a archivos del sistema real. El proveedor incrustado se utiliza para tener acceso a archivos incrustados en ensamblados. El proveedor compuesto se utiliza para proporcionar acceso combinado a archivos y directorios de uno o varios proveedores.

### <a name="physicalfileprovider"></a>PhysicalFileProvider

El `PhysicalFileProvider` proporciona acceso al sistema de archivos físico. Encapsula el `System.IO.File` tipo (para el proveedor físico), definir el ámbito de todas las rutas de acceso a un directorio y sus elementos secundarios. Este ámbito limita el acceso a un directorio determinado y sus elementos secundarios, impidiendo el acceso al sistema de archivos fuera de este límite. Al crear una instancia de este proveedor, debe proporcionar con una ruta de acceso de directorio, que actúa como la ruta de acceso base para todas las solicitudes realizadas a este proveedor (y que restringe el acceso fuera de esta ruta de acceso). En una aplicación ASP.NET Core, puede crear instancias de un `PhysicalFileProvider` proveedor directamente o se puede solicitar un `IFileProvider` en un controlador o el constructor del servicio a través de [inyección de dependencia](dependency-injection.md). El último enfoque normalmente producirá una solución más flexible y pueden someterse a prueba.

El ejemplo siguiente muestra cómo crear un `PhysicalFileProvider`.


```csharp
IFileProvider provider = new PhysicalFileProvider(applicationRoot);
IDirectoryContents contents = provider.GetDirectoryContents(""); // the applicationRoot contents
IFileInfo fileInfo = provider.GetFileInfo("wwwroot/js/site.js"); // a file under applicationRoot
```

Puede recorrer en iteración su contenido del directorio u obtener información de un archivo específico, proporcionando una subruta de acceso.

Para solicitar un proveedor de un controlador, especifíquela en el constructor del controlador y asignarlo a un campo local. Utilice la instancia local de los métodos de acción:

[!code-csharp[Main](file-providers/sample/src/FileProviderSample/Controllers/HomeController.cs?highlight=5,7,12&range=6-19)]

A continuación, cree el proveedor en la aplicación `Startup` clase:

[!code-csharp[Main](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=35,40&range=1-43)]

En el *Index.cshtml* ver, recorrer en iteración el `IDirectoryContents` proporciona:

[!code-html[Main](file-providers/sample/src/FileProviderSample/Views/Home/Index.cshtml?highlight=2,7,9,11,15)]

El resultado:

![Archivos de lista de aplicación de ejemplo de proveedor físicos y carpetas de archivos](file-providers/_static/physical-directory-listing.png)

### <a name="embeddedfileprovider"></a>EmbeddedFileProvider

El `EmbeddedFileProvider` se utiliza para tener acceso a archivos incrustados en ensamblados. En .NET Core, incrustar archivos en un ensamblado con el `<EmbeddedResource>` elemento en el *.csproj* archivo:

[!code-json[Main](file-providers/sample/src/FileProviderSample/FileProviderSample.csproj?range=13-18)]

Puede usar [patrones de uso de comodines](#globbing-patterns) al especificar archivos para incrustar en el ensamblado. Estos modelos se pueden utilizar para que coincida con uno o varios archivos.

> [!NOTE]
> Es improbable que alguna vez desearía realmente incrustar cada archivo .js en el proyecto en el ensamblado; el ejemplo anterior es solo con fines de demostración.

Al crear un `EmbeddedFileProvider`, pase el ensamblado que va a leer a su constructor.

```csharp
var embeddedProvider = new EmbeddedFileProvider(Assembly.GetEntryAssembly());
```

El fragmento de código anterior muestra cómo crear un `EmbeddedFileProvider` con acceso al ensamblado que se está ejecutando actualmente.

Actualizando la aplicación de ejemplo para usar un `EmbeddedFileProvider` da como resultado el siguiente resultado:

![Aplicación de ejemplo de proveedor de archivo enumerar archivos incrustados](file-providers/_static/embedded-directory-listing.png)

> [!NOTE]
> Los recursos incrustados no exponen directorios. En su lugar, se incrusta la ruta de acceso al recurso (a través de su espacio de nombres) en su nombre de archivo con `.` separadores.

> [!TIP]
> El `EmbeddedFileProvider` constructor acepta opcional `baseNamespace` parámetro. Especificar este ámbito aplicará las llamadas a `GetDirectoryContents` a esos recursos en el espacio de nombres proporcionado.

### <a name="compositefileprovider"></a>CompositeFileProvider

El `CompositeFileProvider` combina `IFileProvider` instancias, exponer una única interfaz para trabajar con archivos de varios proveedores. Al crear el `CompositeFileProvider`, pasar uno o varios `IFileProvider` instancias a su constructor:

[!code-csharp[Main](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=3&range=35-37)]

Actualizando la aplicación de ejemplo para usar un `CompositeFileProvider` que incluye los proveedores físicos e incrustados configurados anteriormente, aparecerá el siguiente resultado:

![Aplicación de ejemplo de proveedor de archivo enumerar archivos físicos y las carpetas y archivos incrustados](file-providers/_static/composite-directory-listing.png)

## <a name="watching-for-changes"></a>Ver cambios

El `IFileProvider` `Watch` método proporciona una manera de ver uno o más archivos o directorios para detectar cambios. Este método acepta una cadena de ruta de acceso, que puede usar [patrones de uso de comodines en](#globbing-patterns) para especificar varios archivos y devuelve un `IChangeToken`. Este token expone un `HasChanged` propiedad que se puede inspeccionar, y un `RegisterChangeCallback` método al que se llama cuando se detectan cambios en la cadena de ruta de acceso especificada. Tenga en cuenta que cada token de cambio sólo llama a su devolución de llamada asociada en respuesta a un único cambio. Para habilitar la supervisión constante, puede usar un `TaskCompletionSource` tal y como se muestra a continuación, o volver a crear `IChangeToken` instancias en respuesta a los cambios.

En el ejemplo de este artículo, se configura una aplicación de consola para mostrar un mensaje cada vez que se modifica un archivo de texto:

[!code-csharp[Main](file-providers/sample/src/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

El resultado, después de guardar el archivo varias veces:

![Ventana de comandos después de ejecutar dotnet ejecutar muestra el archivo quotes.txt para cambios de supervisión de aplicaciones y el archivo ha cambiado cinco veces.](file-providers/_static/watch-console.png)

> [!NOTE]
> Algunos sistemas de archivos, como contenedores de Docker y recursos compartidos de red, no pueden enviar notificaciones de cambio de forma confiable. Establecer el `DOTNET_USE_POLLINGFILEWATCHER` variable de entorno `1` o `true` para sondear el sistema de archivos para los cambios cada 4 segundos.

## <a name="globbing-patterns"></a>Patrones de uso de comodines

Las rutas de acceso del sistema de archivos utilizan patrones de caracteres comodín denominados *patrones de uso de comodines en*. Estos modelos simples se pueden utilizar para especificar los grupos de archivos. Los dos caracteres comodín son `*` y `**`.

**`*`**

   Coincide con nada en el nivel de carpeta actual, o cualquier nombre de archivo o cualquier extensión de archivo. Se finalizan coincidencias `/` y `.` caracteres en la ruta de acceso de archivo.

<strong><code>**</code></strong>

   Coincide con ninguna cadena a través de varios niveles de directorios. Puede usarse para recursivamente coincide con muchos archivos dentro de una jerarquía de directorios.

### <a name="globbing-pattern-examples"></a>Ejemplos de patrones de uso de comodines

**`directory/file.txt`**

   Coincide con un archivo concreto en un directorio específico.

**<code>directory/*.txt</code>**

   Coincide con todos los archivos con `.txt` extensión en un directorio específico.

**`directory/*/bower.json`**

   Coincide con todos los `bower.json` archivos en directorios exactamente un nivel por debajo del `directory` directory.

**<code>directory/&#42;&#42;/&#42;.txt</code>**

   Coincide con todos los archivos con `.txt` extensión se encuentra en cualquier lugar en el `directory` directory.

## <a name="file-provider-usage-in-aspnet-core"></a>Uso de proveedor de los archivos en ASP.NET Core

Varias partes de ASP.NET Core usan proveedores de archivos. `IHostingEnvironment`expone la aplicación raíz del contenido y la raíz web como `IFileProvider` tipos. El middleware de archivos estáticos usa proveedores de archivos para buscar archivos estáticos. Razor hace un uso intensivo de `IFileProvider` en la localización de vistas. Dotnet publica funcionalidad usa archivo proveedores y patrones de uso de comodines para especificar qué archivos deben publicarse.

## <a name="recommendations-for-use-in-apps"></a>Recomendaciones para su uso en aplicaciones

Si su aplicación de ASP.NET Core requiere acceso al sistema de archivos, puede solicitar una instancia de `IFileProvider` a través de la inyección de dependencia y, a continuación, utilizar sus métodos para realizar el acceso, tal como se muestra en este ejemplo. Esto le permite configurar el proveedor de una vez, cuando se inicia la aplicación y reduce el número de tipos de implementación que crea una instancia de la aplicación.
