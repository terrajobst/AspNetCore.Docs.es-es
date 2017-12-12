---
title: "Trabajar con archivos estáticos en ASP.NET Core"
author: rick-anderson
description: "Obtenga información acerca de cómo trabajar con archivos estáticos en ASP.NET Core."
keywords: "Núcleo de ASP.NET, archivos estáticos, activos estáticos, HTML, CSS, JavaScript"
ms.author: riande
manager: wpickett
ms.date: 4/07/2017
ms.topic: article
ms.assetid: e32245c7-4eee-4831-bd2e-915dbf9f5f70
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/static-files
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c0751576a1391f26f045c3f8c42ea39c0ff6e5d9
ms.sourcegitcommit: e4fb6b13be56a0fb2f2778623740a047d6489227
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/16/2017
---
# <a name="working-with-static-files-in-aspnet-core"></a>Trabajar con archivos estáticos en ASP.NET Core

<a name="fundamentals-static-files"></a>

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

Los archivos estáticos, como HTML, CSS, imágenes y JavaScript, están activos que una aplicación de ASP.NET Core puede actuar directamente a los clientes.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/static-files/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

## <a name="serving-static-files"></a>Enviar archivos estáticos

Archivos estáticos suelen encontrarse en la `web root` (*\<contenido raíz > / wwwroot*) carpeta. Vea [raíz del contenido](xref:fundamentals/index#content-root) y [raíz Web](xref:fundamentals/index#web-root) para obtener más información. En general establecida la raíz del contenido es el directorio actual para que el proyecto `web root` se encontrarán en el desarrollo.

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs?highlight=5&start=12&end=22)]

Archivos estáticos pueden almacenarse en cualquier carpeta bajo la `web root` y obtener acceso a una ruta de acceso relativa a esa raíz. Por ejemplo, cuando se crea un proyecto de aplicación Web predeterminada con Visual Studio, hay varias carpetas que creó en el *wwwroot* carpeta - *css*, *imágenes*, y *js*. El URI para tener acceso a una imagen en el *imágenes* subcarpeta:

* `http://<app>/images/<imageFileName>`
* `http://localhost:9189/images/banner3.svg`

Para los archivos estáticos que se sirvan, debe configurar el [Middleware](middleware.md) para agregar archivos estáticos a la canalización. El middleware de archivos estáticos puede configurarse mediante la adición de una dependencia en el *Microsoft.AspNetCore.StaticFiles* paquete a su proyecto y, a continuación, llamar a la `UseStaticFiles` método de extensión de `Startup.Configure`:

[!code-csharp[Main](../fundamentals/static-files/sample/StartupStaticFiles.cs?highlight=3&name=snippet1)]

`app.UseStaticFiles();`hace que los archivos en `web root` (*wwwroot* de forma predeterminada) servable. Más adelante voy a explicar cómo hacer servable con otro contenido del directorio `UseStaticFiles`.

Debe incluir el paquete NuGet "Microsoft.AspNetCore.StaticFiles".

> [!NOTE]
> `web root`tiene como valor predeterminado el *wwwroot* directorio, pero puede establecer el `web root` directorio con `UseWebRoot`.

Suponga que tiene una jerarquía de proyectos donde están los archivos estáticos que se va a actuar fuera de la `web root`. Por ejemplo:

* wwwroot
  * css
  * imágenes
  * ...
* MyStaticFiles
  * Test.png

Para una solicitud obtener acceso a *test.png*, configurar el middleware de archivos estáticos como sigue:

[!code-csharp[Main](../fundamentals/static-files/sample/StartupTwoStaticFiles.cs?highlight=5,6,7,8,9,10&name=snippet1)]

Una solicitud para `http://<app>/StaticFiles/test.png` dará servicio a la *test.png* archivo.

`StaticFileOptions()`puede establecer encabezados de respuesta. Por ejemplo, el código siguiente configura estático file servers de la *wwwroot* carpeta y establece la `Cache-Control` encabezado para hacerlos públicamente almacenable en caché durante 10 minutos (600 segundos):

[!code-csharp[Main](../fundamentals/static-files/sample/StartupAddHeader.cs?name=snippet1)]

El [HeaderDictionaryExtensions.Append](/dotnet/api/microsoft.aspnetcore.http.headerdictionaryextensions.append) método está disponible desde el [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) paquete. Agregar `using Microsoft.AspNetCore.Http;` a su *csharp* archivo si el método no está disponible.

![Encabezados de respuesta que muestra el encabezado Cache-Control se ha agregado](static-files/_static/add-header.png)

## <a name="static-file-authorization"></a>Autorización de archivos estáticos

El módulo de archivo estático proporciona **sin** comprobaciones de autorización. Los archivos atendido por él, las de incluidas *wwwroot* estén disponibles públicamente. Para servir archivos según su autorización:

* Almacenarlos fuera de *wwwroot* y cualquier directorio accesible para el middleware de archivos estáticos **y**

* Atender a ellos a través de una acción de controlador, devolver un `FileResult` donde se aplica la autorización

## <a name="enabling-directory-browsing"></a>Habilitar examen de directorios

Examen de directorios permite al usuario de la aplicación web ver una lista de directorios y archivos contenidos en un directorio especificado. Examen de directorios está deshabilitado de forma predeterminada por motivos de seguridad (consulte [consideraciones](#considerations)). Para habilitar el examen de directorios, llame a la `UseDirectoryBrowser` método de extensión de `Startup.Configure`:

[!code-csharp[Main](static-files/sample/StartupBrowse.cs?name=snippet1)]

Y agregue los servicios necesarios mediante una llamada a `AddDirectoryBrowser` método de extensión de `Startup.ConfigureServices`:

[!code-csharp[Main](static-files/sample/StartupBrowse.cs?name=snippet2)]

El código anterior permite la exploración de directorios de la *wwwroot/imágenes* carpeta mediante la dirección URL http://\<aplicación > / MyImages, con vínculos a cada archivo y carpeta:

![examen de directorios](static-files/_static/dir-browse.png)

Vea [consideraciones](#considerations) sobre los riesgos de seguridad al habilitar la exploración.

Tenga en cuenta los dos `app.UseStaticFiles` llamadas. La primera de ellas es requerida para atender la CSS, imágenes y JavaScript en el *wwwroot* carpeta y la segunda llamada de examen de directorios de la *wwwroot/imágenes* carpeta mediante la dirección URL http://\<aplicación > / MyImages:

[!code-csharp[Main](static-files/sample/StartupBrowse.cs?highlight=3,5&name=snippet1)]

## <a name="serving-a-default-document"></a>Servir un documento predeterminado

Establecer una página principal predeterminada, proporciona a los visitantes del sitio un punto de partida cuando se visita el sitio. En el orden de la aplicación Web servir una página predeterminada sin que el usuario tener que calificar totalmente el URI, llame a la `UseDefaultFiles` método de extensión de `Startup.Configure` como se indica a continuación.

[!code-csharp[Main](../fundamentals/static-files/sample/StartupEmpty.cs?highlight=3&name=snippet1)]

> [!NOTE]
> `UseDefaultFiles`debe llamarse antes `UseStaticFiles` para servir el archivo predeterminado. `UseDefaultFiles`es un escritor de nuevo de dirección URL que no sirva realmente el archivo. Debe habilitar el middleware de archivos estáticos (`UseStaticFiles`) para servir el archivo.

Con `UseDefaultFiles`, buscará las solicitudes a una carpeta:

* default.htm
* default.html
* index.htm
* index.html

El primer archivo que se encuentra en la lista se servirá como si la solicitud no era el URI completo (aunque se seguirá la dirección URL del explorador mostrar el identificador URI solicitado).

El código siguiente muestra cómo cambiar el nombre de archivo predeterminado que *mydefault.html*.

[!code-csharp[Main](static-files/sample/StartupDefault.cs?name=snippet1)]

## <a name="usefileserver"></a>UseFileServer

`UseFileServer`combina la funcionalidad de `UseStaticFiles`, `UseDefaultFiles`, y `UseDirectoryBrowser`.

El código siguiente permite archivos estáticos y el archivo predeterminado que se sirvan, pero no permite la exploración de directorios:

```csharp
app.UseFileServer();
   ```

El código siguiente permite archivos estáticos, los archivos de forma predeterminada y examen de directorios:

```csharp
app.UseFileServer(enableDirectoryBrowsing: true);
   ```

Vea [consideraciones](#considerations) sobre los riesgos de seguridad al habilitar la exploración. Al igual que con `UseStaticFiles`, `UseDefaultFiles`, y `UseDirectoryBrowser`, si desea servir archivos que existen fuera de la `web root`, crear una instancia y configurar un `FileServerOptions` objeto que se pasa como un parámetro a `UseFileServer`. Por ejemplo, dada la siguiente jerarquía de directorios de la aplicación Web:

* wwwroot

  * css

  * imágenes

  * ...

* MyStaticFiles

  * Test.png

  * default.html

Utilizando el ejemplo de jerarquía anterior, puede que los archivos estáticos, archivos de forma predeterminada y exploración de la `MyStaticFiles` directory. En el siguiente fragmento de código, que se logra con una sola llamada a `FileServerOptions`.

[!code-csharp[Main](static-files/sample/StartupUseFileServer.cs?highlight=5,6,7,8,9,10,11&name=snippet1)]

Si `enableDirectoryBrowsing` está establecido en `true` , debe llamar a `AddDirectoryBrowser` método de extensión de `Startup.ConfigureServices`:

[!code-csharp[Main](static-files/sample/StartupUseFileServer.cs?name=snippet2)]

Uso de la jerarquía de archivos y el código anterior:

| Identificador URI            |                             Respuesta  |
| ------- | ------|
| `http://<app>/StaticFiles/test.png`    |      MyStaticFiles/test.png |
| `http://<app>/StaticFiles`              |     MyStaticFiles/default.html |

Si no tiene valor predeterminado con el nombre de archivos que se encuentran en el *MyStaticFiles* directorio, http://\<aplicación > / StaticFiles devuelve la lista con vínculos activos de directorios:

![Lista de archivos estáticos](static-files/_static/db2.PNG)

> [!NOTE]
> `UseDefaultFiles`y `UseDirectoryBrowser` le llevará a la dirección url http://\<aplicación > / StaticFiles sin la barra diagonal final y causa un cliente redirigir a http://\<aplicación > /StaticFiles/ (adición de la barra diagonal final). Sin la barra diagonal final relativa direcciones URL dentro de los documentos sería incorrectas.

## <a name="fileextensioncontenttypeprovider"></a>FileExtensionContentTypeProvider

La `FileExtensionContentTypeProvider` clase contiene una colección que asigna las extensiones de archivo para tipos de contenido MIME. En el ejemplo siguiente, se registran varias extensiones de archivo para el tipo MIME conocido, se reemplaza ".rtf" y ". mp4" se quita.

[!code-csharp[Main](../fundamentals/static-files/sample/StartupFileExtensionContentTypeProvider.cs?highlight=3,4,5,6,7,8,9,10,11,12,19&name=snippet1)]

Vea [tipos de contenido MIME](http://www.iana.org/assignments/media-types/media-types.xhtml).

## <a name="non-standard-content-types"></a>Tipos de contenido no estándares

El middleware de archivos estáticos ASP.NET entiende casi 400 tipos de contenido de archivo conocidos. Si el usuario solicita un archivo de un tipo de archivo desconocido, el middleware de archivos estáticos devuelve una respuesta HTTP 404 (no encontrado). Si se habilita el examen de directorios, se mostrará un vínculo al archivo, pero el URI devolverá un error HTTP 404.

El código siguiente permite que sirve al tipo desconocido y representará el archivo desconocido como una imagen.

[!code-csharp[Main](static-files/sample/StartupServeUnknownFileTypes.cs?name=snippet1)]

Con el código anterior, se devolverá una solicitud para un archivo con un tipo de contenido desconocido como una imagen.

>[!WARNING]
> Habilitar `ServeUnknownFileTypes` es un riesgo de seguridad y no se recomienda utilizarlo.  `FileExtensionContentTypeProvider`(explicados con anterioridad) proporciona una alternativa más segura a ofrecer servicio a archivos con extensiones no estándar.

### <a name="considerations"></a>Consideraciones

>[!WARNING]
> `UseDirectoryBrowser`y `UseStaticFiles` puede producir la pérdida de información confidencial. Se recomienda **no** Habilitar examen de directorios en producción. Tenga cuidado acerca de los directorios que se habilita con `UseStaticFiles` o `UseDirectoryBrowser` como todo el directorio y todos los subdirectorios será accesibles. Se recomienda mantener contenido público en su propio directorio como  *\<contenido raíz > / wwwroot*, lejos de vistas de la aplicación, archivos de configuración, etcetera.

* Las direcciones URL para el contenido que se exponen a través de `UseDirectoryBrowser` y `UseStaticFiles` están sujetos a las mayúsculas y minúsculas y restricciones de caracteres de su sistema de archivos subyacente. Por ejemplo, Windows distingue mayúsculas de minúsculas, pero no son Mac y Linux.

* Las aplicaciones de ASP.NET Core hospedadas en IIS usar el módulo principal de ASP.NET para reenviar todas las solicitudes a la aplicación incluidas las solicitudes de archivos estáticos. No se usa el controlador de archivos estáticos de IIS porque no puede ser una oportunidad para controlar las solicitudes antes de que están controlados por el módulo principal de ASP.NET.

* Para quitar el controlador de archivos estáticos de IIS (en el nivel de servidor o un sitio Web):

     * Navegue hasta la **módulos** característica

     * Seleccione **StaticFileModule** en la lista

     * Pulse **quitar** en el **acciones** sidebar

>[!WARNING]
> Si está habilitado el controlador de archivos estáticos de IIS **y** el módulo de núcleo de ASP.NET (ANCM) no está configurado correctamente (por ejemplo si *web.config* no se implementó), se servirá archivos estáticos.

* Archivos de código (como c# y Razor) deben colocarse fuera del proyecto de aplicación `web root` (*wwwroot* de forma predeterminada). Esto crea una separación clara entre el contenido del lado de la aplicación cliente y código de origen del lado servidor, lo que impide que se filtren código de servidor.

## <a name="additional-resources"></a>Recursos adicionales

* [Middleware](middleware.md)

* [Introducción a ASP.NET Core](../index.md)
