---
title: Vistas parciales
author: ardalis
description: "Usar las vistas parciales en ASP.NET MVC de núcleo"
ms.author: riande
manager: wpickett
ms.date: 03/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/partial
ms.openlocfilehash: 5919c273de2a298c3e407f118ac478e6a6031332
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
# <a name="partial-views"></a>Vistas parciales

Por [Steve Smith](https://ardalis.com/), [Maher JENDOUBI](https://twitter.com/maherjend), y [Rick Anderson](https://twitter.com/RickAndMSFT)

Núcleo de ASP.NET MVC admite vistas parciales, que son útiles cuando tienen partes reutilizables de páginas web que desea compartir entre las distintas vistas.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/partial/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-are-partial-views"></a>¿Cuáles son las vistas parciales?

Una vista parcial es una vista que se representa dentro de otra vista. La salida HTML generada mediante la ejecución de la vista parcial se representa en la vista que realiza la llamada (o principal). Como vistas, vistas parciales utilizan la *.cshtml* la extensión de archivo.

## <a name="when-should-i-use-partial-views"></a>¿Cuándo debo utilizar las vistas parciales?

Las vistas parciales son una forma eficaz de separación de vistas de gran tamaño en componentes más pequeños. Pueden reducir la duplicación del contenido de la vista y permitir que los elementos de vista pueden reutilizarse. Se deben especificar los elementos de diseño comunes en [_Layout.cshtml](layout.md). Contenido reutilizable de diseño no se puede encapsular en las vistas parciales.

Si tiene una página compleja que consta de varias partes lógicas, puede ser útil trabajar con cada pieza como su propia vista parcial. Cada parte de la página puede verse de forma aislada del resto de la página y la vista para la propia página es mucho más sencilla, puesto que solo contiene la estructura de la página y las llamadas a representar vistas parciales general.

Sugerencia: Siga el [no repita usted mismo principio](http://deviq.com/don-t-repeat-yourself/) en las vistas.

## <a name="declaring-partial-views"></a>Declarar las vistas parciales

Las vistas parciales se crean como cualquier otra vista: se crea un *.cshtml* archivo dentro de la *vistas* carpeta. No hay ninguna diferencia semántica entre una vista parcial y una vista normal: solo se representará de forma diferente. Puede tener una vista que se devuelve directamente desde un controlador `ViewResult`, y puede usarse la misma vista como una vista parcial. La diferencia principal entre cómo se representan una vista y una vista parcial es que no se ejecutan las vistas parciales *_ViewStart.cshtml* (mientras vistas - más información sobre *_ViewStart.cshtml* en [diseño ](layout.md)).

## <a name="referencing-a-partial-view"></a>Hacer referencia a una vista parcial

Desde dentro de una página de vista, hay varias maneras en que puede representar una vista parcial. Es la más sencilla de usar `Html.Partial`, que devuelve un `IHtmlString` y pueden hacer referencia a un prefijo de la llamada con `@`:

[!code-html[Main](partial/sample/src/PartialViewsSample/Views/Home/About.cshtml?range=9)]

El `PartialAsync` método está disponible para parcial vistas que contienen código asincrónico (aunque generalmente no se recomienda el código en las vistas):

[!code-html[Main](partial/sample/src/PartialViewsSample/Views/Home/About.cshtml?range=8)]

Puede representar una vista parcial con `RenderPartial`. Este método no devuelve ningún resultado; transmite por secuencias la salida representada directamente a la respuesta. Dado que no devuelve ningún resultado, debe llamarse dentro de un bloque de código Razor (también puede llamar a `RenderPartialAsync` si es necesario):

[!code-html[Main](partial/sample/src/PartialViewsSample/Views/Home/About.cshtml?range=10-12)]

Dado que transmite por secuencias el resultado directamente, `RenderPartial` y `RenderPartialAsync` puede funcionar mejor en algunos escenarios. Sin embargo, en la mayoría de los casos, recomienda use `Partial` y `PartialAsync`.

> [!NOTE]
> Si las vistas que se necesitan ejecutar el código, el patrón recomendado consiste en utilizar un [del componente vista](view-components.md) en lugar de una vista parcial.

### <a name="partial-view-discovery"></a>Detección de la vista parcial

Cuando se hace referencia a una vista parcial, puede hacer referencia a su ubicación de varias maneras:

```text
// Uses a view in current folder with this name
// If none is found, searches the Shared folder
@Html.Partial("ViewName")

// A view with this name must be in the same folder
@Html.Partial("ViewName.cshtml")

// Locate the view based on the application root
// Paths that start with "/" or "~/" refer to the application root
@Html.Partial("~/Views/Folder/ViewName.cshtml")
@Html.Partial("/Views/Folder/ViewName.cshtml")

// Locate the view using relative paths
@Html.Partial("../Account/LoginPartial.cshtml")
```

Puede tener diferentes vistas parciales con el mismo nombre en las carpetas de la vista diferente. Cuando se hace referencia a las vistas por su nombre (sin extensión de archivo), vistas de cada carpeta utilizará la vista parcial en la misma carpeta con ellos. También puede especificar una vista parcial predeterminada que se utiliza colocándolo en la *Shared* carpeta. Se usará la vista parcial compartida por todas las vistas que no tienen su propia versión de la vista parcial. Puede tener una vista parcial predeterminada (en *Shared*), que se haya reemplazado por una vista parcial con el mismo nombre en la misma carpeta que la vista primaria.

Las vistas parciales pueden ser *encadenadas*. Es decir, una vista parcial puede llamar a otra vista parcial (siempre que no se crean un bucle). Dentro de cada vista o la vista parcial, rutas de acceso relativas son siempre con respecto a esa vista de la vista, no en la raíz o primaria.

> [!NOTE]
> Si declara un [Razor](razor.md) `section` en una vista parcial, no será visible para sus principales; se limitará a la vista parcial.

## <a name="accessing-data-from-partial-views"></a>Acceso a datos desde las vistas parciales

Cuando se crea una instancia de una vista parcial, obtiene una copia de la vista primaria `ViewData` diccionario. Las actualizaciones realizadas en los datos dentro de la vista parcial no se conservan en la vista primaria. `ViewData`cambiar en una parcial vista se pierde cuando se devuelve la vista parcial.

Puede pasar una instancia de `ViewDataDictionary` a la vista parcial:

```csharp
@Html.Partial("PartialName", customViewData)
   ```

También puede pasar un modelo en una vista parcial. Puede tratarse de modelo de vista de la página, o una parte de este o un objeto personalizado. Puede pasar un modelo a `Partial`,`PartialAsync`, `RenderPartial`, o `RenderPartialAsync`:

```csharp
@Html.Partial("PartialName", viewModel)
   ```

Puede pasar una instancia de `ViewDataDictionary` y un modelo de vista para una vista parcial:

[!code-html[Main](partial/sample/src/PartialViewsSample/Views/Articles/Read.cshtml?range=15-16)]

El marcado siguiente muestra la *Views/Articles/Read.cshtml* vista que contiene dos vistas parciales. La segunda vista parcial que se pasa en un modelo y `ViewData` a la vista parcial. Puede pasar nuevos `ViewData` diccionario conservando existente `ViewData` si utiliza la sobrecarga del constructor de la `ViewDataDictionary` resaltado a continuación:

[!code-html[Main](partial/sample/src/PartialViewsSample/Views/Articles/Read.cshtml)]

*Views/Shared/AuthorPartial*:

[!code-html[Main](partial/sample/src/PartialViewsSample/Views/Shared/AuthorPartial.cshtml)]

El *ArticleSection* parcial:

[!code-html[Main](partial/sample/src/PartialViewsSample/Views/Articles/ArticleSection.cshtml)]

En tiempo de ejecución, las parciales se representan en la vista primaria, que a su vez se representa en el recurso compartido *_Layout.cshtml*

![salida de la vista parcial](partial/_static/output.png)
