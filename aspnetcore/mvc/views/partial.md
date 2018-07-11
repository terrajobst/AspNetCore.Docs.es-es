---
title: Vistas parciales en ASP.NET Core
author: ardalis
description: Obtenga información sobre las vistas parciales, que son vistas que se representan dentro de otra vista, y cuándo deben usarse en aplicaciones ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 07/06/2018
uid: mvc/views/partial
ms.openlocfilehash: 9f90ce39929d0dbc216b47d76d652c1fca866ec2
ms.sourcegitcommit: a09820f91e71a7d98b7347bf93210abb9e995e22
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/06/2018
ms.locfileid: "37889121"
---
# <a name="partial-views-in-aspnet-core"></a>Vistas parciales en ASP.NET Core

Por [Steve Smith](https://ardalis.com/), [Maher JENDOUBI](https://twitter.com/maherjend), [Rick Anderson](https://twitter.com/RickAndMSFT) y [Scott Sauber](https://twitter.com/scottsauber)

ASP.NET Core MVC admite vistas parciales, las cuales resultan útiles para compartir partes reutilizables de páginas web entre distintas vistas.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/partial/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-are-partial-views"></a>¿Qué son las vistas parciales?

Una vista parcial es una vista que se representa dentro de otra vista. El HTML generado al ejecutar la vista parcial se representa en la vista que realiza la llamada (o principal). Al igual que las vistas, las vistas parciales usan la extensión de archivo *.cshtml*.

Por ejemplo, la plantilla de proyecto **Aplicación web** de ASP.NET Core 2.1 incluye una vista parcial *_CookieConsentPartial.cshtml*. La vista parcial se carga desde *_Layout.cshtml*:

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Shared/_Layout.cshtml?name=snippet_CookieConsentPartial)]

## <a name="when-to-use-partial-views"></a>¿Cuándo se usan las vistas parciales?

Las vistas parciales son una forma eficaz de dividir vistas de gran tamaño en componentes más pequeños. Pueden reducir la duplicación del contenido de la vista y permitir reutilizar los elementos de la vista. Se deben especificar elementos de diseño comunes en [_Layout.cshtml](xref:mvc/views/layout). El contenido reutilizable que no es de diseño se puede encapsular en las vistas parciales.

En una página compleja compuesta por varias partes lógicas, puede ser útil trabajar con cada parte como su propia vista parcial. Cada parte de la página puede verse de forma aislada del resto de la página. La vista de la propia página se vuelve más sencilla, puesto que solo contiene la estructura general de la página y las llamadas para representar las vistas parciales.

> [!TIP]
> Siga el principio [Una vez y solo una (DRY)](https://deviq.com/don-t-repeat-yourself/) en las vistas.

## <a name="declare-partial-views"></a>Declaración de vistas parciales

Las vistas parciales se crean como una vista normal: se crea un archivo *.cshtml* dentro de la carpeta *Views*. No hay ninguna diferencia semántica entre una vista parcial y una vista normal, pero se representan de forma diferente. Puede hacer que una vista se devuelva directamente desde la clase [ViewResult](/dotnet/api/microsoft.aspnetcore.mvc.viewresult) de un controlador y puede usar la misma vista como una vista parcial. La diferencia principal entre el modo en que se representa una vista y una vista parcial es que las vistas parciales no ejecutan *_ViewStart.cshtml*. Las vistas normales sí ejecutan *_ViewStart.cshtml*. Más información sobre *_ViewStart.cshtml* en [Diseño](xref:mvc/views/layout).

Por convención, los nombres de archivo de las vistas parciales suelen empezar por `_`. Esta convención de nomenclatura no es un requisito, pero resulta útil para diferenciar visualmente las vistas parciales de las vistas normales.

## <a name="reference-a-partial-view"></a>Referencia a una vista parcial

Dentro de una página de vista, hay varias maneras de representar una vista parcial. El procedimiento recomendado es usar el procesamiento asincrónico.

::: moniker range=">= aspnetcore-2.1"

### <a name="partial-tag-helper"></a>Aplicación auxiliar de etiquetas parciales

La aplicación auxiliar de etiquetas parciales requiere ASP.NET Core 2.1 o posterior. Representa asincrónicamente y usa una sintaxis similar a HTML:

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Home/Discovery.cshtml?name=snippet_PartialTagHelper)]

Para obtener más información, vea <xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper>.

::: moniker-end

### <a name="asynchronous-html-helper"></a>Aplicación auxiliar HTML asincrónica

Cuando se usa una aplicación auxiliar HTML, el procedimiento recomendado es usar [PartialAsync](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partialasync#Microsoft_AspNetCore_Mvc_Rendering_HtmlHelperPartialExtensions_PartialAsync_Microsoft_AspNetCore_Mvc_Rendering_IHtmlHelper_System_String_). Devuelve un tipo [IHtmlContent](/dotnet/api/microsoft.aspnetcore.html.ihtmlcontent) encapsulado en una clase `Task`. Para hacer referencia al método, se agrega a la llamada el prefijo `@`:

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Home/Discovery.cshtml?name=snippet_PartialAsync)]

También puede representar una vista parcial con [RenderPartialAsync](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartialasync). Este método no devuelve ningún resultado, sino que transmite por secuencias la salida representada directamente a la respuesta. Como el método no devuelve ningún resultado, debe llamarse desde un bloque de código de Razor:

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Home/Discovery.cshtml?name=snippet_RenderPartialAsync)]

Dado que transmite por secuencias el resultado directamente, `RenderPartialAsync` puede funcionar mejor en algunos escenarios, pero se recomienda usar `PartialAsync`.

### <a name="synchronous-html-helper"></a>Aplicación auxiliar HTML sincrónica

[Partial](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partial) y [RenderPartial](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartial) son los equivalentes sincrónicos de `PartialAsync` y `RenderPartialAsync`, respectivamente. El uso de los equivalentes sincrónicos no es aconsejable, ya que hay escenarios donde producen interbloqueos. Las versiones futuras no contendrán métodos sincrónicos.

> [!IMPORTANT]
> Si las vistas necesitan ejecutar código, use un [componente de vista](xref:mvc/views/view-components) en lugar de una vista parcial.

::: moniker range=">= aspnetcore-2.1"

En ASP.NET Core 2.1 o posterior, una llamada a `Partial` o a `RenderPartial` da como resultado una advertencia de analizador. Por ejemplo, el uso de `Partial` da como resultado el mensaje de advertencia siguiente:

> Use of IHtmlHelper.Partial may result in application deadlocks. Consider using `<partial>` Tag Helper or `IHtmlHelper.PartialAsync`. (El uso de IHtmlHelper.Partial puede producir interbloqueos de aplicaciones. Considere el uso de la aplicación auxiliar de etiquetas `<partial>` o `IHtmlHelper.PartialAsync`.)

Reemplace las llamadas a `@Html.Partial` por `@await Html.PartialAsync` o la aplicación auxiliar de etiquetas parciales. Para más información sobre la migración de la aplicación auxiliar de etiquetas parciales, vea [Migración desde una aplicación auxiliar HTML](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper#migrate-from-an-html-helper).

::: moniker-end

## <a name="partial-view-discovery"></a>Detección de vistas parciales

Cuando se hace referencia a una vista parcial, se puede hacer referencia a su ubicación de varias maneras. Por ejemplo:

::: moniker range=">= aspnetcore-2.1"

```cshtml
// Uses a view in current folder with this name.
// If none is found, searches the Shared folder.
<partial name="_ViewName" />

// A view with this name must be in the same folder
<partial name="_ViewName.cshtml" />

// Locate the view based on the app root.
// Paths that start with "/" or "~/" refer to the app root.
<partial name="~/Views/Folder/_ViewName.cshtml" />
<partial name="/Views/Folder/_ViewName.cshtml" />

// Locate the view using a relative path
<partial name="../Account/_LoginPartial.cshtml" />
```

En el ejemplo anterior se usa la aplicación auxiliar de etiquetas parciales, lo que requiere ASP.NET Core 2.1 o posterior. En el ejemplo siguiente se usan aplicaciones auxiliares HTML asincrónicas para realizar la misma tarea.

::: moniker-end

```cshtml
// Uses a view in current folder with this name.
// If none is found, searches the Shared folder.
@await Html.PartialAsync("_ViewName")

// A view with this name must be in the same folder
@await Html.PartialAsync("_ViewName.cshtml")

// Locate the view based on the app root.
// Paths that start with "/" or "~/" refer to the app root.
@await Html.PartialAsync("~/Views/Folder/_ViewName.cshtml")
@await Html.PartialAsync("/Views/Folder/_ViewName.cshtml")

// Locate the view using a relative path
@await Html.PartialAsync("../Account/_LoginPartial.cshtml")
```

Puede tener diferentes vistas parciales con el mismo nombre de archivo en distintas carpetas de vistas. Cuando se hace referencia a las vistas por su nombre (sin extensión de archivo), las vistas de cada carpeta usan la vista parcial en la misma carpeta que ellas. También puede especificar una vista parcial predeterminada que vaya a usar, colocándola en la carpeta *Shared*. Las vistas que no tengan su propia versión de la vista parcial usarán la vista parcial compartida. Puede tener una vista parcial predeterminada (en *Shared*), que se haya reemplazado por una vista parcial con el mismo nombre y en la misma carpeta que la vista principal.

Las vistas parciales se pueden *encadenar*. Es decir, una vista parcial puede llamar a otra vista parcial (siempre que no creen un bucle). Dentro de cada vista o vista parcial, las rutas de acceso relativas son siempre relativas con respecto a esa vista, no con respecto a la vista principal o de raíz.

> [!NOTE]
> Una `section` de [Razor](xref:mvc/views/razor) definida en una vista parcial no es visible para las vistas principales. La `section` solo es visible para la vista parcial en la que está definida.

## <a name="access-data-from-partial-views"></a>Acceso a datos desde vistas parciales

Cuando se crea una instancia de una vista parcial, obtiene una copia del diccionario de `ViewData` de la vista principal. Las actualizaciones realizadas en los datos dentro de la vista parcial no se conservan en la vista principal. Los cambios de `ViewData` en una vista parcial se pierden cuando se devuelve la vista parcial.

Puede pasar una instancia de [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) a la vista parcial:

```cshtml
@await Html.PartialAsync("_PartialName", customViewData)
```

Puede pasar un modelo a una vista parcial. El modelo puede ser el modelo de vista de la página o un objeto personalizado. Puede pasar un modelo a `PartialAsync` o a `RenderPartialAsync`:

```cshtml
@await Html.PartialAsync("_PartialName", viewModel)
```

Puede pasar una instancia de `ViewDataDictionary` y un modelo de vista a una vista parcial:

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Articles/Read.cshtml?name=snippet_PartialAsync)]

El marcado siguiente muestra la vista *Views/Articles/Read.cshtml*, que contiene dos vistas parciales. La segunda vista parcial se pasa a un modelo y `ViewData` a la vista parcial. Use la sobrecarga del constructor de `ViewDataDictionary` resaltado para pasar un nuevo diccionario de `ViewData` a la vez que conserva el diccionario `ViewData` existente.

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Articles/Read.cshtml?name=snippet_ReadPartialView&highlight=17-20)]

*Views/Shared/_AuthorPartial*:

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Shared/_AuthorPartial.cshtml)]

La vista parcial *_ArticleSection*:

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Articles/_ArticleSection.cshtml)]

En tiempo de ejecución, las vistas parciales se representan en la vista principal, que a su vez se representa dentro de *_Layout.cshtml* compartido.

![resultados de vista parcial](partial/_static/output.png)

## <a name="additional-resources"></a>Recursos adicionales

::: moniker range=">= aspnetcore-2.1"

* <xref:mvc/views/razor>
* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper>
* <xref:mvc/views/view-components>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

* <xref:mvc/views/razor>
* <xref:mvc/views/view-components>

::: moniker-end