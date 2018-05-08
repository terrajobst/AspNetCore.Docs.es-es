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
---
# <a name="file-providers-in-aspnet-core"></a>Proveedores de archivo en ASP.NET Core

Por [Steve Smith](https://ardalis.com/)

ASP.NET Core abstrae el acceso al sistema de archivos mediante el uso de proveedores de archivos.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

## <a name="file-provider-abstractions"></a>Abstracciones de proveedores de archivos

Los proveedores de archivos son una abstracción respecto a los sistemas de archivos. La interfaz principal es `IFileProvider`. `IFileProvider` expone métodos para obtener información de archivos (`IFileInfo`) y de directorios (`IDirectoryContents`), y para configurar las notificaciones de cambio (con `IChangeToken`).

`IFileInfo` proporciona métodos y propiedades de archivos o directorios individuales. Tiene dos propiedades booleanas, `Exists` y `IsDirectory`, así como propiedades que describen el archivo, como `Name`, `Length` (en bytes) y la fecha `LastModified`. Se puede leer en el archivo mediante el método `CreateReadStream`.

## <a name="file-provider-implementations"></a>Implementaciones del proveedor de archivos

Hay disponibles tres implementaciones de `IFileProvider`: física, insertada y compuesta. El proveedor físico se utiliza para tener acceso a los archivos del sistema real. El proveedor insertado se utiliza para tener acceso a archivos insertados en ensamblados. El proveedor compuesto se utiliza para proporcionar acceso combinado a archivos y directorios de uno o más proveedores.

### <a name="physicalfileprovider"></a>PhysicalFileProvider

`PhysicalFileProvider` proporciona acceso al sistema de archivos físico. Encapsula el tipo `System.IO.File` (para el proveedor físico), definiendo el ámbito de todas las rutas de acceso a un directorio y sus elementos secundarios. Este ámbito limita el acceso a un directorio determinado y sus elementos secundarios, impidiendo el acceso al sistema de archivos fuera de este límite. Al crear una instancia de este proveedor, debe proporcionarle una ruta de acceso de directorio, que actúa como la ruta de acceso base para todas las solicitudes realizadas a este proveedor (y que restringe el acceso fuera de esta ruta de acceso). En una aplicación ASP.NET Core, puede crear instancias de un proveedor `PhysicalFileProvider` directamente o puede solicitar un proveedor `IFileProvider` de un controlador o constructor del servicio a través de [inserción de dependencias](dependency-injection.md). Por lo general, el último enfoque brindará una solución más flexible y más fácil de probar.

En el ejemplo siguiente se muestra cómo crear `PhysicalFileProvider`.


```csharp
IFileProvider provider = new PhysicalFileProvider(applicationRoot);
IDirectoryContents contents = provider.GetDirectoryContents(""); // the applicationRoot contents
IFileInfo fileInfo = provider.GetFileInfo("wwwroot/js/site.js"); // a file under applicationRoot
```

Para recorrer en iteración el contenido del directorio u obtener información de un archivo específico, puede proporcionar una subruta de acceso.

Para solicitar un proveedor de un controlador, especifíquelo en el constructor del controlador y asígnelo a un campo local. Utilice la instancia local de los métodos de acción:

[!code-csharp[](file-providers/sample/src/FileProviderSample/Controllers/HomeController.cs?highlight=5,7,12&range=6-19)]

A continuación, cree el proveedor en la clase `Startup` de la aplicación:

[!code-csharp[](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=35,40&range=1-43)]

En la vista *Index.cshtml*, recorrer en iteración el `IDirectoryContents` proporcionado:

[!code-html[](file-providers/sample/src/FileProviderSample/Views/Home/Index.cshtml?highlight=2,7,9,11,15)]

Resultado:

![Aplicación de ejemplo de proveedor de archivos donde se enumeran las carpetas y los archivos físicos](file-providers/_static/physical-directory-listing.png)

### <a name="embeddedfileprovider"></a>EmbeddedFileProvider

`EmbeddedFileProvider` se utiliza para tener acceso a archivos insertados en ensamblados. En .NET Core, se insertan archivos en un ensamblado con el elemento `<EmbeddedResource>` en el archivo *.csproj*:

[!code-json[](file-providers/sample/src/FileProviderSample/FileProviderSample.csproj?range=13-18)]

Puede usar [patrones de comodines](#globbing-patterns) para especificar archivos que se insertarán en el ensamblado. Estos patrones se pueden usar para que coincidan con uno o más archivos.

> [!NOTE]
> Es improbable que alguna vez quiera insertar cada archivo .js del proyecto en su ensamblado; el ejemplo anterior se incluye solo con fines de demostración.

Al crear `EmbeddedFileProvider`, pase el ensamblado que va a leer a su constructor.

```csharp
var embeddedProvider = new EmbeddedFileProvider(Assembly.GetEntryAssembly());
```

El fragmento de código anterior muestra cómo crear `EmbeddedFileProvider` con acceso al ensamblado que se está ejecutando actualmente.

La actualización de la aplicación de ejemplo para usar `EmbeddedFileProvider` genera la siguiente salida:

![Aplicación de ejemplo de proveedor de archivos donde se enumeran los archivos insertados](file-providers/_static/embedded-directory-listing.png)

> [!NOTE]
> Los recursos insertados no exponen directorios. En su lugar, la ruta de acceso al recurso (a través de su espacio de nombres) se inserta en su nombre de archivo con separadores `.`.

> [!TIP]
> El constructor `EmbeddedFileProvider` acepta un parámetro `baseNamespace` opcional. Si se especifica este parámetro, las llamadas a `GetDirectoryContents` tendrán como ámbito esos recursos en el espacio de nombres proporcionado.

### <a name="compositefileprovider"></a>CompositeFileProvider

`CompositeFileProvider` combina instancias de `IFileProvider`, y expone una única interfaz para trabajar con archivos de varios proveedores. Al crear `CompositeFileProvider`, se pasan una o varias instancias de `IFileProvider` a su constructor:

[!code-csharp[](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=3&range=35-37)]

Actualizar la aplicación de ejemplo para que use un proveedor `CompositeFileProvider` que incluya los proveedores físicos y los insertados que se configuraron anteriormente, produce la siguiente salida:

![Aplicación de ejemplo de proveedor de archivos donde se enumeran las carpetas y los archivos físicos, y los archivos insertados](file-providers/_static/composite-directory-listing.png)

## <a name="watching-for-changes"></a>Observación de cambios

El método `IFileProvider` `Watch` proporciona una manera de ver uno o más archivos o directorios para detectar cambios. Este método acepta una cadena de ruta de acceso, que puede usar [patrones de comodines](#globbing-patterns) para especificar varios archivos y devuelve un token `IChangeToken`. Este token expone una propiedad `HasChanged` que se puede inspeccionar, así como un método `RegisterChangeCallback` al que se llama cuando se detectan cambios en la cadena de ruta de acceso especificada. Tenga en cuenta que cada token de cambio solo llama a su devolución de llamada asociada en respuesta a un único cambio. Para habilitar la supervisión constante, puede usar `TaskCompletionSource` tal y como se muestra a continuación, o volver a crear instancias de `IChangeToken` en respuesta a los cambios.

En el ejemplo de este artículo, se configura una aplicación de consola para mostrar un mensaje cada vez que se modifica un archivo de texto:

[!code-csharp[](file-providers/sample/src/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

El resultado, después de guardar el archivo varias veces:

![Ventana Comandos después de ejecutar dotnet run muestra la aplicación supervisando el archivo quotes.txt para detectar cambios. El archivo ha cambiado cinco veces.](file-providers/_static/watch-console.png)

> [!NOTE]
> Algunos sistemas de archivos, como contenedores de Docker y recursos compartidos de red, no pueden enviar notificaciones de cambio de forma confiable. Establezca la variable de entorno `DOTNET_USE_POLLINGFILEWATCHER` en `1` o `true` para sondear el sistema de archivos en busca de cambios cada 4 segundos.

## <a name="globbing-patterns"></a>Patrones de comodines

Las rutas de acceso del sistema de archivos utilizan patrones de caracteres comodín denominados *patrones de comodines*. Estos sencillos modelos se pueden usar para especificar grupos de archivos. Los dos caracteres comodín son `*` y `**`.

**`*`**

   Coincide con cualquier elemento del nivel de carpeta actual, con cualquier nombre de archivo o con cualquier extensión de archivo. Las coincidencias finalizan con los caracteres `/` y `.` en la ruta de acceso de archivo.

<strong><code>**</code></strong>

   Coincide con cualquier elemento de varios niveles de directorios. Puede usarse para coincidir de forma recursiva con muchos archivos dentro de una jerarquía de directorios.

### <a name="globbing-pattern-examples"></a>Ejemplos de patrones de uso de comodines

**`directory/file.txt`**

   Coincide con un archivo concreto en un directorio específico.

**<code>directory/*.txt</code>**

   Coincide con todos los archivos que tengan la extensión `.txt` en un directorio específico.

**`directory/*/bower.json`**

   Coincide con todos los archivos `bower.json` que estén en directorios exactamente un nivel por debajo del directorio `directory`.

**<code>directory/&#42;&#42;/&#42;.txt</code>**

   Coincide con todos los archivos cuya extensión sea `.txt` y se encuentren en cualquier lugar del directorio `directory`.

## <a name="file-provider-usage-in-aspnet-core"></a>Uso de proveedor de archivos en ASP.NET Core

Varias partes de ASP.NET Core usan proveedores de archivos. `IHostingEnvironment` expone la raíz del contenido de la aplicación y la raíz web como tipos `IFileProvider`. El middleware de archivos estáticos usa proveedores de archivos para buscar archivos estáticos. Razor hace un uso intensivo de `IFileProvider` en la localización de vistas. La funcionalidad de publicación de Dotnet usa proveedores de archivos y patrones de comodines para especificar los archivos que deben publicarse.

## <a name="recommendations-for-use-in-apps"></a>Recomendaciones para su uso en aplicaciones

Si su aplicación de ASP.NET Core requiere acceso al sistema de archivos, puede solicitar una instancia de `IFileProvider` a través de la inserción de dependencias y, a continuación, utilizar sus métodos para realizar el acceso, tal como se muestra en este ejemplo. Esto le permite configurar el proveedor una sola vez cuando se inicia la aplicación, y reduce el número de tipos de implementación de los que la aplicación crea instancias.
