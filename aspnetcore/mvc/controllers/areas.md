---
title: Áreas de ASP.NET Core
author: rick-anderson
description: Obtenga información sobre las áreas, una característica de ASP.NET MVC que se usa para organizar funciones relacionadas en un grupo como un espacio de nombres independiente (para el enrutamiento) y una estructura de carpetas (para las vistas).
ms.author: riande
ms.date: 02/14/2019
uid: mvc/controllers/areas
ms.openlocfilehash: 79bc023a7bd00a9d4de375e3cddaafd148251469
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2019
ms.locfileid: "58264770"
---
# <a name="areas-in-aspnet-core"></a>Áreas de ASP.NET Core

Por [Dhananjay Kumar](https://twitter.com/debug_mode) y [Rick Anderson](https://twitter.com/RickAndMSFT)

Las áreas son una característica de ASP.NET que se usa para organizar funciones relacionadas en un grupo como un espacio de nombres independiente (para el enrutamiento) y una estructura de carpetas (para las vistas). El uso de áreas crea una jerarquía para el enrutamiento mediante la adición de otro parámetro de ruta, `area`, a `controller` y `action`, o bien a un elemento `page` de página de Razor.

Las áreas ofrecen una manera de dividir una aplicación web ASP.NET Core en grupos funcionales más pequeños, cada uno con su propio conjunto de Razor Pages, controladores, vistas y modelos. Un área es en realidad una estructura dentro de una aplicación. En un proyecto web ASP.NET Core, los componentes lógicos como Páginas, Modelo, Vista y Controlador se mantienen en carpetas diferentes. El runtime de ASP.NET Core usa convenciones de nomenclatura para crear la relación entre estos componentes. Para una aplicación grande, puede ser conveniente dividir la aplicación en distintas áreas de funciones de alto nivel. Por ejemplo, una aplicación de comercio electrónico con varias unidades de negocio, como las de finalización de la compra, facturación y búsqueda. Cada una de estas unidades tiene un área propia para controlar vistas, controladores, Razor Pages y modelos.

Considere el uso de áreas en un proyecto en los casos siguientes:

* La aplicación está formada por varios componentes funcionales generales que se pueden separar de forma lógica.
* Le interesa dividir la aplicación para que se pueda trabajar en cada área funcional de forma independiente.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample)). En el ejemplo de descarga se proporciona una aplicación básica para probar las áreas.

Si va a usar Razor Pages, consulte [Áreas con Razor Pages](#areas-with-razor-pages) en este documento.

## <a name="areas-for-controllers-with-views"></a>Áreas para controladores con vistas

Una aplicación web ASP.NET Core típica en la que se usen áreas, controladores y vistas contiene lo siguiente:

* Una [estructura de carpetas de área](#area-folder-structure).
* Controladores decorados con el atributo [&lbrack;Area&rbrack;](#attribute) para asociar el controlador el área: [!code-csharp[](areas/samples/MVCareas/Areas/Products/Controllers/ManageController.cs?name=snippet2)]
* La [ruta de área agregada al inicio](#add-area-route): [!code-csharp[](areas/samples/MVCareas/Startup.cs?name=snippet2&highlight=3-6)]

### <a name="area-folder-structure"></a>Estructura de carpetas de área

Considere una aplicación que tiene dos grupos lógicos, *Productos* y *Servicios*. Mediante las áreas, la estructura de carpetas sería similar a la siguiente:

* Nombre del proyecto
  * Áreas
    * Productos
      * Controladores
        * HomeController.cs
        * ManageController.cs
      * Vistas
        * Página principal
          * Index.cshtml
        * Administrar
          * Index.cshtml
          * About.cshtml
    * Servicios
      * Controladores
        * HomeController.cs
      * Vistas
        * Página principal
          * Index.cshtml

Aunque el diseño anterior es típico cuando se usan áreas, solo los archivos de vista tienen que usar esta estructura de carpetas. La detección de vista busca un archivo de vista de área coincidente en el orden siguiente:

```text
/Areas/<Area-Name>/Views/<Controller-Name>/<Action-Name>.cshtml
/Areas/<Area-Name>/Views/Shared/<Action-Name>.cshtml
/Views/Shared/<Action-Name>.cshtml
/Pages/Shared/<Action-Name>.cshtml
   ```

La ubicación de las carpetas que no son de vista, como *Controllers* y *Models*, **no** importa. Por ejemplo, las carpetas *Controllers* y *Models* no son necesarias. El contenido de *Controllers* y *Models* es código que se compila en un archivo .dll. El contenido de *Views* no se compila hasta que se haya realizado una solicitud a esa vista.

<a name="attribute"></a>

### <a name="associate-the-controller-with-an-area"></a>Asociación del controlador a un área

Los controladores de área se designan con el atributo [&lbrack;Area&rbrack;](xref:Microsoft.AspNetCore.Mvc.AreaAttribute):

[!code-csharp[](areas/samples/MVCareas/Areas/Products/Controllers/ManageController.cs?highlight=5&name=snippet)]

### <a name="add-area-route"></a>Adición de la ruta de área

Las rutas de área normalmente usan el enrutamiento convencional en lugar del enrutamiento mediante atributos. El enrutamiento convencional depende del orden. En general, las rutas con áreas deben colocarse antes en la tabla de rutas, ya que son más específicas que las rutas sin un área.

Se puede `{area:...}` usar como un token en las plantillas de ruta, si el espacio de direcciones URL es uniforme en todas las áreas:

[!code-csharp[](areas/samples/MVCareas/Startup.cs?name=snippet&highlight=18-21)]

En el código anterior, `exists` aplica la restricción de que la ruta debe coincidir con un área. El uso de `{area:...}` es el mecanismo menos complicado para agregar enrutamiento a las áreas.

En el código siguiente se usa <xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*> para crear dos rutas de área con nombre:

[!code-csharp[](areas/samples/MVCareas/StartupMapAreaRoute.cs?name=snippet&highlight=18-27)]

Cuando use `MapAreaRoute` con ASP.NET Core 2.2, vea [este problema de GitHub](https://github.com/aspnet/AspNetCore/issues/7772).

Para más información, vea [Enrutamiento de áreas](xref:mvc/controllers/routing#areas).

### <a name="link-generation-with-mvc-areas"></a>Generación de vínculos con áreas de MVC

En el código siguiente de la [descarga de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) se muestra la generación de vínculos con el área especificada:

[!code-cshtml[](areas/samples/MVCareas/Views/Shared/_testLinksPartial.cshtml?name=snippet)]

Los vínculos generados con el código anterior son válidos en cualquier parte de la aplicación.

En la descarga de ejemplo se incluye una [vista parcial](xref:mvc/views/partial) que contiene los vínculos anteriores y los mismos vínculos sin especificar el área. En el [archivo de diseño](xref:mvc/views/layout) se hace referencia a la vista parcial, por lo que en todas las páginas de la aplicación se muestran los vínculos generados. Los vínculos generados sin especificar el área solo son válidos cuando se les hace referencia desde una página en el mismo área y controlador.

Cuando no se especifica el área o el controlador, el enrutamiento depende de los valores de *ambiente*. Los valores de ruta actuales de la solicitud actual se consideran valores de ambiente para la generación de vínculos. En muchos casos de la aplicación de ejemplo, el uso de valores de ambiente genera vínculos incorrectos.

Para más información, vea [Enrutar a acciones de controlador](xref:mvc/controllers/routing).

### <a name="shared-layout-for-areas-using-the-viewstartcshtml-file"></a>Diseño Compartido para áreas con el archivo _ViewStart.cshtml

Para compartir un diseño común para toda la aplicación, mueva el archivo *_ViewStart.cshtml* a la carpeta raíz de la aplicación.

<a name="rename"></a>

### <a name="change-default-area-folder-where-views-are-stored"></a>Cambio de la carpeta de área predeterminada donde se almacenan las vistas

En el código siguiente se cambia la carpeta de área predeterminada de `"Areas"` a `"MyAreas"`:

[!code-csharp[](areas/samples/MVCareas/Startup2.cs?name=snippet)]

<a name="arp"></a>

## <a name="areas-with-razor-pages"></a>Áreas con Razor Pages

Las áreas con Razor Pages requieren la carpeta *Areas/&lt;nombre de área&gt;/Pages* en la raíz de la aplicación. La siguiente estructura de carpetas se usa con la [descarga de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/areas/samples).

* Nombre del proyecto
  * Áreas
    * Productos
      * Páginas
        * _ViewImports
        * Acerca de
        * Índice
    * Servicios
      * Páginas
        * Administrar
          * Acerca de
          * Índice

### <a name="link-generation-with-razor-pages-and-areas"></a>Generación de vínculos con Razor Pages y áreas

El código siguiente de la [descarga de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/areas/samples/RPareas) muestra la generación de vínculos con el área especificada (por ejemplo, `asp-area="Products"`):

[!code-cshtml[](areas/samples/RPareas/Pages/Shared/_testLinksPartial.cshtml?name=snippet)]

Los vínculos generados con el código anterior son válidos en cualquier parte de la aplicación.

En la descarga de ejemplo se incluye una [vista parcial](xref:mvc/views/partial) que contiene los vínculos anteriores y los mismos vínculos sin especificar el área. En el [archivo de diseño](xref:mvc/views/layout) se hace referencia a la vista parcial, por lo que en todas las páginas de la aplicación se muestran los vínculos generados. Los vínculos generados sin especificar el área solo son válidos cuando se les hace referencia desde una página de la misma área.

Cuando no se especifica el área, el enrutamiento depende de los valores *ambient*. Los valores de ruta actuales de la solicitud actual se consideran valores de ambiente para la generación de vínculos. En muchos casos de la aplicación de ejemplo, el uso de valores de ambiente genera vínculos incorrectos. Por ejemplo, tenga en cuenta los vínculos generados desde el código siguiente:

[!code-cshtml[](areas/samples/RPareas/Pages/Shared/_testLinksPartial.cshtml?name=snippet2)]

En el código anterior:

* El vínculo generado a partir de `<a asp-page="/Manage/About">` es correcto solo cuando la última solicitud fue de una página del área `Services`. Por ejemplo, `/Services/Manage/`, `/Services/Manage/Index` o `/Services/Manage/About`.
* El vínculo generado a partir de `<a asp-page="/About">` es correcto solo cuando la última solicitud fue de una página de `/Home`.
* El código está tomado de la [descarga de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/areas/samples/RPareas).

### <a name="import-namespace-and-tag-helpers-with-viewimports-file"></a>Importación del espacio de nombres y los asistentes de etiquetas con el archivo _ViewImports

Se puede agregar un archivo *_ViewImports* a la carpeta *Pages* de cada área para importar el espacio de nombres y los asistentes de etiquetas en cada página de Razor de la carpeta.

Tenga en cuenta el área *Services* del código de ejemplo, que no contiene un archivo *_ViewImports*. El marcado siguiente muestra la página de Razor */Services/Manage/About*:

[!code-cshtml[](areas/samples/RPareas/Areas/Services/Pages/Manage/About.cshtml)]

En el marcado anterior:

* El nombre de dominio completo debe usarse para especificar el modelo (`@model RPareas.Areas.Services.Pages.Manage.AboutModel`).
* Los [asistentes de etiquetas](xref:mvc/views/tag-helpers/intro) se habilitan mediante `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers`

En la descarga de ejemplo, el área Products contiene el siguiente archivo *_ViewImports*:

[!code-cshtml[](areas/samples/RPareas/Areas/Products/Pages/_ViewImports.cshtml)]

El siguiente marcado muestra la página de Razor */Products/About*: [!code-cshtml[](areas/samples/RPareas/Areas/Products/Pages/About.cshtml)]

En el archivo anterior, el espacio de nombres y la directiva `@addTagHelper` se importan en el archivo mediante el archivo *Areas/Products/Pages/_ViewImports.cshtml*:

Para más información, consulte [Administración del ámbito de los asistentes de etiquetas](xref:mvc/views/tag-helpers/intro?view=aspnetcore-2.2#managing-tag-helper-scope) y [Importar directivas compartidas](xref:mvc/views/layout#importing-shared-directives).

### <a name="shared-layout-for-razor-pages-areas"></a>Diseño compartido para áreas de Razor Pages

Para compartir un diseño común para toda la aplicación, mueva el archivo *_ViewStart.cshtml* a la carpeta raíz de la aplicación.

### <a name="publishing-areas"></a>Publicación de áreas

Todos los archivos `*.cshtml` y `wwwroot/**` se publican en la salida cuando se incluye `<Project Sdk="Microsoft.NET.Sdk.Web">` en el archivo .csproj*.