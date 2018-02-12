---
title: Vistas parciales
author: ardalis
description: Usar vistas parciales en ASP.NET Core MVC
manager: wpickett
ms.author: riande
ms.date: 03/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/partial
ms.openlocfilehash: 169948e5d7dc8068463ed61114666148b785b217
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
# <a name="partial-views"></a>Vistas parciales

Por [Steve Smith](https://ardalis.com/), [Maher JENDOUBI](https://twitter.com/maherjend) y [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core MVC admite vistas parciales, las cuales resultan útiles cuando tiene partes reutilizables de páginas web que quiere compartir entre distintas vistas.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/partial/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-are-partial-views"></a>¿Qué son las vistas parciales?

Una vista parcial es una vista que se representa dentro de otra vista. El HTML generado al ejecutar la vista parcial se representa en la vista que realiza la llamada (o principal). Al igual que las vistas, las vistas parciales usan la extensión de archivo *.cshtml*.

## <a name="when-should-i-use-partial-views"></a>¿Cuándo debo usar vistas parciales?

Las vistas parciales son una forma eficaz de dividir vistas de gran tamaño en componentes más pequeños. Pueden reducir la duplicación del contenido de la vista y permitir reutilizar los elementos de la vista. Se deben especificar elementos de diseño comunes en [_Layout.cshtml](layout.md). El contenido reutilizable que no es de diseño se puede encapsular en las vistas parciales.

Si tiene una página compleja formada por varias partes lógicas, puede ser útil trabajar con cada parte como su propia vista parcial. Cada parte de la página puede visualizarse de forma aislada del resto de la página y la vista para la propia página se vuelve mucho más sencilla, ya que solo contiene la estructura general de la página y las llamadas para representar las vistas parciales.

Sugerencia: siga el principio [Una vez y solo una (DRY)](http://deviq.com/don-t-repeat-yourself/) en las vistas.

## <a name="declaring-partial-views"></a>Declarar vistas parciales

Las vistas parciales se crean como cualquier otra vista: se crea un archivo *.cshtml* dentro de la carpeta *Views*. No hay ninguna diferencia semántica entre una vista parcial y una vista normal: simplemente se representan de forma diferente. Puede hacer que una vista se devuelva directamente desde el `ViewResult` de un controlador y puede usar la misma vista como una vista parcial. La diferencia principal entre el modo en que se representa una vista y una vista parcial es que las vistas parciales no ejecutan *_ViewStart.cshtml*, mientras que las vistas normales sí (más información sobre *_ViewStart.cshtml* en [Diseño](layout.md)).

## <a name="referencing-a-partial-view"></a>Referencia a una vista parcial

Desde una página de vista, hay varias maneras de representar una vista parcial. La forma más sencilla consiste en usar `Html.Partial`, que devuelve un `IHtmlString` y se puede hacer referencia a ella agregando un prefijo a la llamada con `@`:

[!code-html[Main](partial/sample/src/PartialViewsSample/Views/Home/About.cshtml?range=9)]

El método `PartialAsync` está disponible para las vistas parciales que contienen código asincrónico (aunque normalmente no se recomienda el código en las vistas):

[!code-html[Main](partial/sample/src/PartialViewsSample/Views/Home/About.cshtml?range=8)]

Puede representar una vista parcial con `RenderPartial`. Este método no devuelve ningún resultado; transmite por secuencias la salida representada directamente a la respuesta. Como no devuelve ningún resultado, debe llamarlo desde un bloque de código de Razor (también puede llamar a `RenderPartialAsync` si es necesario):

[!code-html[Main](partial/sample/src/PartialViewsSample/Views/Home/About.cshtml?range=10-12)]

Dado que transmite por secuencias el resultado directamente, `RenderPartial` y `RenderPartialAsync` pueden funcionar mejor en algunos escenarios. Pero en la mayoría de los casos se recomienda usar `Partial` y `PartialAsync`.

> [!NOTE]
> Si las vistas necesitan ejecutar código, el patrón recomendado es usar un [componente de vista](view-components.md) en lugar de una vista parcial.

### <a name="partial-view-discovery"></a>Detección de vistas parciales

Cuando se hace referencia a una vista parcial, se puede hacer referencia a su ubicación de varias maneras:

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

Puede tener diferentes vistas parciales con el mismo nombre en distintas carpetas de vistas. Cuando se hace referencia a las vistas por su nombre (sin extensión de archivo), las vistas de cada carpeta usarán la vista parcial en la misma carpeta que ellas. También puede especificar una vista parcial predeterminada que vaya a usar, colocándola en la carpeta *Shared*. Cualquier vista que no tenga su propia versión de la vista parcial usará la vista parcial compartida. Puede tener una vista parcial predeterminada (en *Shared*), que se haya reemplazado por una vista parcial con el mismo nombre y en la misma carpeta que la vista principal.

Las vistas parciales se pueden *encadenar*. Es decir, una vista parcial puede llamar a otra vista parcial (siempre que no creen un bucle). Dentro de cada vista o vista parcial, las rutas de acceso relativas son siempre relativas con respecto a esa vista, no con respecto a la vista principal o de raíz.

> [!NOTE]
> Si declara un `section` de [Razor](razor.md) en una vista parcial, no estará visible para sus vistas principales, sino que se limitará a la vista parcial.

## <a name="accessing-data-from-partial-views"></a>Acceso a datos desde vistas parciales

Cuando se crea una instancia de una vista parcial, obtiene una copia del diccionario de `ViewData` de la vista principal. Las actualizaciones realizadas en los datos dentro de la vista parcial no se conservan en la vista principal. `ViewData` cambiado en una parcial vista se pierde cuando se devuelve la vista parcial.

Puede pasar una instancia de `ViewDataDictionary` a la vista parcial:

```csharp
@Html.Partial("PartialName", customViewData)
   ```

También puede pasar un modelo a una vista parcial. Puede tratarse del modelo de vista de la página, o una parte de él, o un objeto personalizado. Puede pasar un modelo a `Partial`, `PartialAsync`, `RenderPartial` o `RenderPartialAsync`:

```csharp
@Html.Partial("PartialName", viewModel)
   ```

Puede pasar una instancia de `ViewDataDictionary` y un modelo de vista a una vista parcial:

[!code-html[Main](partial/sample/src/PartialViewsSample/Views/Articles/Read.cshtml?range=15-16)]

El marcado siguiente muestra la vista *Views/Articles/Read.cshtml* que contiene dos vistas parciales. La segunda vista parcial se pasa a un modelo y `ViewData` a la vista parcial. Puede pasar un nuevo diccionario de `ViewData` a la vez que conserva el `ViewData` existente si usa la sobrecarga del constructor de `ViewDataDictionary` resaltado aquí abajo:

[!code-html[Main](partial/sample/src/PartialViewsSample/Views/Articles/Read.cshtml)]

*Views/Shared/AuthorPartial*:

[!code-html[Main](partial/sample/src/PartialViewsSample/Views/Shared/AuthorPartial.cshtml)]

La vista parcial *ArticleSection*:

[!code-html[Main](partial/sample/src/PartialViewsSample/Views/Articles/ArticleSection.cshtml)]

En tiempo de ejecución, las vistas parciales se representan en la vista principal, que a su vez se representa dentro de *_Layout.cshtml* compartido.

![resultados de vista parcial](partial/_static/output.png)
