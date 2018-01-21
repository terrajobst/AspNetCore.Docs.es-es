---
title: "Trabajar con archivos estáticos en ASP.NET Core"
author: rick-anderson
description: "Aprenda a servir y proteger archivos estáticos y configurar archivos estáticos hospedaje comportamientos de middleware en una aplicación web de ASP.NET Core."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/18/2018
ms.devlang: csharp
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/static-files
ms.openlocfilehash: 60b205bf0a45e2965f9dab46f46956947ae513fd
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2018
---
# <a name="work-with-static-files-in-aspnet-core"></a>Trabajar con archivos estáticos en ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Scott Addie](https://twitter.com/Scott_Addie)

Los archivos estáticos, como HTML, CSS, imágenes y JavaScript, están activos de que una aplicación de ASP.NET Core sirve directamente a los clientes. Alguna configuración es necesaria para habilitar el servicio de estos archivos.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/static-files/samples) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

## <a name="serve-static-files"></a>Servir archivos estáticos

Archivos estáticos se almacenan en el directorio del proyecto web raíz. El directorio predeterminado es  *\<content_root > / wwwroot*, pero puede cambiarse a través de la [UseWebRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usewebroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseWebRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) método. Vea [raíz del contenido](xref:fundamentals/index#content-root) y [raíz Web](xref:fundamentals/index#web-root) para obtener más información.

Host de la aplicación web debe se conocen la existencia del directorio raíz del contenido.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

El `WebHost.CreateDefaultBuilder` método establece la raíz de contenido en el directorio actual:

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=9)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Establece el directorio raíz contenida en el directorio actual mediante la invocación de [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseContentRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) dentro de `Program.Main`:

[!code-csharp[](static-files/samples/1x/Program.cs?name=snippet_ProgramClass&highlight=7)]

---

Archivos estáticos son accesibles a través de una ruta de acceso relativa a la raíz de web. Por ejemplo, el **aplicación Web** plantilla de proyecto contiene varias carpetas dentro de la *wwwroot* carpeta:

* **wwwroot**
  * **css**
  * **images**
  * **js**

El formato del URI para tener acceso a un archivo en el *imágenes* subcarpeta es *http://\<server_address >/images /\<Nombre_archivo_imágenes >*. Por ejemplo, *http://localhost:9189/images/banner3.svg*.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Si el destino es .NET Framework, agregue el [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) paquete al proyecto. Si el destino es .NET Core, el [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) incluye este paquete.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Agregar el [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) paquete al proyecto.

---

Configurar la [middleware](xref:fundamentals/middleware) lo que permite a los servidores de archivos estáticos.

### <a name="serve-files-inside-of-web-root"></a>Servir archivos dentro de la raíz de web

Invocar la [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) método dentro de `Startup.Configure`:

[!code-csharp[](static-files/samples/1x/StartupStaticFiles.cs?name=snippet_ConfigureMethod&highlight=3)]

Sin parámetros `UseStaticFiles` sobrecarga del método marca los archivos en la raíz web como servable. Las siguientes referencias de marcado *wwwroot/images/banner1.svg*:

[!code-cshtml[](static-files/samples/1x/Views/Home/Index.cshtml?name=snippet_static_file_wwwroot)]

### <a name="serve-files-outside-of-web-root"></a>Servir archivos fuera de la raíz de web

Considere la posibilidad de una jerarquía de directorios en el que residen los archivos estáticos que se sirvan fuera de la raíz de web:

* **wwwroot**
  * **css**
  * **images**
  * **js**
* **MyStaticFiles**
  * **images**
      * *banner1.svg*

Puede tener acceso una solicitud de la *banner1.svg* archivo configurando el middleware de archivos estáticos como sigue:

[!code-csharp[](static-files/samples/1x/StartupTwoStaticFiles.cs?name=snippet_ConfigureMethod&highlight=5-10)]

En el código anterior, el *MyStaticFiles* jerarquía de directorios se expone públicamente a través de la *StaticFiles* segmento de URI. Una solicitud para *http://\<server_address > /StaticFiles/images/banner1.svg* sirve la *banner1.svg* archivo.

Las siguientes referencias de marcado *MyStaticFiles/images/banner1.svg*:

[!code-cshtml[](static-files/samples/1x/Views/Home/Index.cshtml?name=snippet_static_file_outside)]

### <a name="set-http-response-headers"></a>Establecer encabezados de respuesta HTTP

A [StaticFileOptions](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions) objeto puede utilizarse para establecer encabezados de respuesta HTTP. Además de configurar los servicios de archivos estáticos desde la raíz de web, el código siguiente establece el `Cache-Control` encabezado:

[!code-csharp[](static-files/samples/1x/StartupAddHeader.cs?name=snippet_ConfigureMethod)]

El [HeaderDictionaryExtensions.Append](/dotnet/api/microsoft.aspnetcore.http.headerdictionaryextensions.append) método existe en el [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) paquete.

Los archivos se han realizado públicamente almacenable en caché durante 10 minutos (600 segundos):

![Encabezados de respuesta que muestra el encabezado Cache-Control se ha agregado](static-files/_static/add-header.png)

## <a name="static-file-authorization"></a>Autorización de archivos estáticos

El middleware de archivos estáticos no proporciona comprobaciones de autorización. Los archivos atendido por él, las de incluidas *wwwroot*, sean accesibles públicamente. Para servir archivos según su autorización:

* Almacenarlos fuera de *wwwroot* y cualquier directorio accesible para el middleware de archivos estáticos **y**
* Servir ellos a través de un método de acción al que se aplica la autorización. Devolver un [FileResult](/dotnet/api/microsoft.aspnetcore.mvc.fileresult) objeto:

[!code-csharp[](static-files/samples/1x/Controllers/HomeController.cs?name=snippet_BannerImageAction)]

## <a name="enable-directory-browsing"></a>Habilitar examen de directorios

Examen de directorios permite a los usuarios de su aplicación web ver una lista de directorios y archivos contenidos en un directorio especificado. Examen de directorios está deshabilitado de forma predeterminada por motivos de seguridad (consulte [consideraciones](#considerations)). Exploración mediante la invocación de directorios que habilitar la [UseDirectoryBrowser](/dotnet/api/microsoft.aspnetcore.builder.directorybrowserextensions.usedirectorybrowser#Microsoft_AspNetCore_Builder_DirectoryBrowserExtensions_UseDirectoryBrowser_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_DirectoryBrowserOptions_) método `Startup.Configure`:

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureMethod&highlight=12-17)]

Agregar servicios requeridos invocando la [AddDirectoryBrowser](/dotnet/api/microsoft.extensions.dependencyinjection.directorybrowserserviceextensions.adddirectorybrowser#Microsoft_Extensions_DependencyInjection_DirectoryBrowserServiceExtensions_AddDirectoryBrowser_Microsoft_Extensions_DependencyInjection_IServiceCollection_) método desde `Startup.ConfigureServices`:

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureServicesMethod&highlight=3)]

El código anterior permite la exploración de directorios de la *wwwroot/imágenes* utilizando la dirección URL de carpeta *http://\<server_address > / MyImages*, con vínculos a cada archivo y carpeta:

![examen de directorios](static-files/_static/dir-browse.png)

Vea [consideraciones](#considerations) sobre los riesgos de seguridad al habilitar la exploración.

Tenga en cuenta los dos `UseStaticFiles` llama en el ejemplo siguiente. La primera llamada permite que el servicio de archivos estáticos en la *wwwroot* carpeta. La segunda llamada habilita el examen de directorios de la *wwwroot/imágenes* utilizando la dirección URL de carpeta *http://\<server_address > / MyImages*:

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureMethod&highlight=3,5)]

## <a name="serve-a-default-document"></a>Proporcione un documento predeterminado

Establecer una página principal predeterminada proporciona a los visitantes un punto de partida lógico al visitar el sitio. Para dar servicio a una página predeterminada sin que el usuario calificar por completo el URI, llame a la [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) método desde `Startup.Configure`:

[!code-csharp[](static-files/samples/1x/StartupEmpty.cs?name=snippet_ConfigureMethod&highlight=3)]

> [!IMPORTANT]
> `UseDefaultFiles`debe llamarse antes `UseStaticFiles` para servir el archivo predeterminado. `UseDefaultFiles`es un sistema de reescritura de dirección URL que no sirva realmente el archivo. Habilitar el middleware de archivos estáticos a través de `UseStaticFiles` para servir el archivo.

Con `UseDefaultFiles`, las solicitudes a una búsqueda de la carpeta para:

* *default.htm*
* *default.html*
* *index.htm*
* *index.html*

El primer archivo que se encuentra en la lista se sirve como si el URI completo de fuera de la solicitud. La dirección URL del explorador continúa reflejar el identificador URI solicitado.

El código siguiente cambia el nombre de archivo predeterminado que *mydefault.html*:

[!code-csharp[](static-files/samples/1x/StartupDefault.cs?name=snippet_ConfigureMethod)]

## <a name="usefileserver"></a>UseFileServer

[UseFileServer](/dotnet/api/microsoft.aspnetcore.builder.fileserverextensions.usefileserver#Microsoft_AspNetCore_Builder_FileServerExtensions_UseFileServer_Microsoft_AspNetCore_Builder_IApplicationBuilder_) combina la funcionalidad de `UseStaticFiles`, `UseDefaultFiles`, y `UseDirectoryBrowser`.

El código siguiente permite a los servidores de archivos estáticos y el archivo predeterminado. Examen de directorios no está habilitado.

```csharp
app.UseFileServer();
```

El código siguiente se basa en la sobrecarga sin parámetros habilitando el examen de directorios:

```csharp
app.UseFileServer(enableDirectoryBrowsing: true);
```

Tenga en cuenta la siguiente jerarquía de directorios:

* **wwwroot**
  * **css**
  * **images**
  * **js**
* **MyStaticFiles**
  * **images**
      * *banner1.svg*
  * *default.html*

El código siguiente permite archivos estáticos, archivos de forma predeterminada y examen de directorios de `MyStaticFiles`:

[!code-csharp[](static-files/samples/1x/StartupUseFileServer.cs?name=snippet_ConfigureMethod&highlight=5-11)]

`AddDirectoryBrowser`se debe llamar cuando la `EnableDirectoryBrowsing` es el valor de la propiedad `true`:

[!code-csharp[](static-files/samples/1x/StartupUseFileServer.cs?name=snippet_ConfigureServicesMethod)]

De código mediante la jerarquía de archivos y anteriores, para resolver las direcciones URL como se indica a continuación:

| Identificador URI            |                             Respuesta  |
| ------- | ------|
| *http://\<server_address>/StaticFiles/images/banner1.svg*    |      MyStaticFiles/images/banner1.svg |
| *http://\<server_address>/StaticFiles*             |     MyStaticFiles/default.html |

Si no existe ningún archivo con el nombre predeterminado en el *MyStaticFiles* directorio, *http://\<server_address > / StaticFiles* devuelve la lista con vínculos activos de directorios:

![Lista de archivos estáticos](static-files/_static/db2.png)

> [!NOTE]
> `UseDefaultFiles`y `UseDirectoryBrowser` utilizar la dirección URL *http://\<server_address > / StaticFiles* sin la barra diagonal final para desencadenar un cliente redirigir a *http://\<server_address > / StaticFiles /*. Tenga en cuenta la adición de la barra diagonal final. Direcciones URL relativas dentro de los documentos se consideran no válidas sin una barra diagonal final.

## <a name="fileextensioncontenttypeprovider"></a>FileExtensionContentTypeProvider

El [FileExtensionContentTypeProvider](/dotnet/api/microsoft.aspnetcore.staticfiles.fileextensioncontenttypeprovider) clase contiene un `Mappings` propiedad que actúa como una asignación de extensiones de archivo para tipos de contenido MIME. En el ejemplo siguiente, se registran varias extensiones de archivo a los tipos MIME conocidos. El *.rtf* se reemplaza la extensión, y *. mp4* se quita.

[!code-csharp[](static-files/samples/1x/StartupFileExtensionContentTypeProvider.cs?name=snippet_ConfigureMethod&highlight=3-12,19)]

Vea [tipos de contenido MIME](http://www.iana.org/assignments/media-types/media-types.xhtml).

## <a name="non-standard-content-types"></a>Tipos de contenido no estándares

El middleware de archivos estáticos entiende casi 400 tipos de contenido de archivo conocidos. Si el usuario solicita un archivo de un tipo de archivo desconocido, el middleware de archivos estáticos devuelve una respuesta HTTP 404 (no encontrado). Si se habilita el examen de directorios, se muestra un vínculo al archivo. El URI devuelve un error HTTP 404.

El código siguiente permite que sirve al tipo desconocido y procesa el archivo como una imagen desconocido:

[!code-csharp[](static-files/samples/1x/StartupServeUnknownFileTypes.cs?name=snippet_ConfigureMethod)]

Con el código anterior, una solicitud para un archivo con un tipo de contenido desconocido se devuelve como una imagen.

> [!WARNING]
> Habilitar [ServeUnknownFileTypes](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions.serveunknownfiletypes#Microsoft_AspNetCore_Builder_StaticFileOptions_ServeUnknownFileTypes) supone un riesgo para la seguridad. Está deshabilitado de forma predeterminada y no se recomienda su uso. [FileExtensionContentTypeProvider](#fileextensioncontenttypeprovider) proporciona una alternativa más segura a ofrecer servicio a archivos con extensiones no estándar.

### <a name="considerations"></a>Consideraciones

> [!WARNING]
> `UseDirectoryBrowser`y `UseStaticFiles` puede producir la pérdida de información confidencial. Se recomienda deshabilitar examen de directorios en producción. Revise cuidadosamente los directorios que se habilitan mediante `UseStaticFiles` o `UseDirectoryBrowser`. Todo el directorio y sus subdirectorios dejan de ser accesibles públicamente. Almacén de archivos adecuado para servir al público en un directorio dedicado, como  *\<content_root > / wwwroot*. Separar estos archivos de las vistas de MVC, las páginas de Razor (solo 2.x), archivos de configuración, etcetera.

* Las direcciones URL para el contenido que se exponen a través de `UseDirectoryBrowser` y `UseStaticFiles` están sujetos a las mayúsculas y minúsculas y restricciones de caracteres del sistema de archivos subyacente. Por ejemplo, distingue mayúsculas de minúsculas Windows&mdash;no Mac y Linux.

* Aplicaciones de ASP.NET Core hospedadas en IIS uso el [módulo de núcleo de ASP.NET (ANCM)](xref:fundamentals/servers/aspnet-core-module) para reenviar todas las solicitudes a la aplicación, incluidas las solicitudes de archivos estáticos. No se usa el controlador de archivos estáticos de IIS. No tiene ninguna posibilidad de controlar las solicitudes antes de que está controlando el ANCM.

* Complete los pasos siguientes en el Administrador de IIS para quitar el controlador de archivos estáticos de IIS en el nivel de servidor o un sitio Web:
    1. Navegue hasta la **módulos** característica.
    1. Seleccione **StaticFileModule** en la lista.
    1. Haga clic en **quitar** en el **acciones** sidebar.

> [!WARNING]
> Si está habilitado el controlador de archivos estáticos de IIS **y** la ANCM está configurada incorrectamente, se sirven archivos estáticos. Esto sucede, por ejemplo, si la *web.config* archivo no está implementado.

* Coloque los archivos de código (incluidas *.cs* y *.cshtml*) fuera de la aplicación raíz del proyecto web. Por lo tanto, se crea una separación lógica entre la aplicación de cliente contenido y código de servidor. Esto impide que se filtren código del lado servidor.

## <a name="additional-resources"></a>Recursos adicionales

* [Middleware](xref:fundamentals/middleware)

* [Introducción a ASP.NET Core](xref:index)
