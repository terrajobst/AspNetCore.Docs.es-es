---
title: "Diseño"
author: ardalis
description: 
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/layout
ms.openlocfilehash: 3e9e5949d8940a33508e24f0da015b49b7ba468c
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
# <a name="layout"></a>Diseño

Por [Steve Smith](https://ardalis.com/)

Las vistas a menudo comparten elementos visuales y elementos mediante programación. En este artículo aprenderá a usar diseños comunes, compartir directivas y ejecutar código común antes de representar vistas en una aplicación ASP.NET.

## <a name="what-is-a-layout"></a>Qué es un diseño

La mayoría de las aplicaciones web tienen un diseño común que ofrece al usuario una experiencia coherente mientras navegan por sus páginas. El diseño suele incluir elementos comunes en la interfaz de usuario, como el encabezado, los elementos de navegación o de menú y el pie de página de la aplicación.

![Ejemplo de diseño de página](layout/_static/page-layout.png)

Las estructuras HTML comunes, como los scripts y las hojas de estilos también se usan con frecuencia en muchas páginas dentro de una aplicación. Todos estos elementos compartidos se pueden definir en un archivo de *diseño*, al que se puede hacer referencia por cualquier vista que se use en la aplicación. Los diseños reducen el código duplicado en las vistas y ayudan a seguir el principio [Una vez y solo una (DRY)](http://deviq.com/don-t-repeat-yourself/).

Por convención, el diseño predeterminado para una aplicación ASP.NET se denomina `_Layout.cshtml`. La plantilla de proyecto de Visual Studio ASP.NET Core MVC incluye este archivo de diseño en la carpeta `Views/Shared`:

![carpeta de vistas en el Explorador de soluciones](layout/_static/web-project-views.png)

Este diseño define una plantilla de nivel superior para las vistas en la aplicación. Las aplicaciones no necesitan un diseño y las aplicaciones pueden definir más de un diseño con distintas vistas que especifiquen diseños diferentes.

Un ejemplo `_Layout.cshtml`:

[!code-html[Main](../../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=42,66)]

## <a name="specifying-a-layout"></a>Especificar un diseño

Las vistas de Razor tienen una propiedad `Layout`. Las vistas individuales especifican un diseño al configurar esta propiedad:

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewStart.cshtml?highlight=2)]

El diseño especificado puede usar una ruta de acceso completa (ejemplo: `/Views/Shared/_Layout.cshtml`) o un nombre parcial (ejemplo: `_Layout`). Cuando se proporciona un nombre parcial, el motor de vista de Razor buscará el archivo de diseño mediante su proceso de detección estándar. Primero se busca en la carpeta asociada al controlador y después en la carpeta `Shared`. Este proceso de detección es idéntico al usado para detectar [vistas parciales](partial.md).

De forma predeterminada, todos los diseños deben llamar a `RenderBody`. Cada vez que se realiza la llamada a `RenderBody`, se representa el contenido de la vista.

<a name="layout-sections-label"></a>

### <a name="sections"></a>Secciones

Opcionalmente, un diseño puede hacer referencia a una o varias *secciones* mediante una llamada a `RenderSection`. Las secciones permiten organizar dónde se deben colocar determinados elementos de la página. Cada llamada a `RenderSection` puede especificar si esa sección es obligatoria u opcional. Si no se encuentra una sección obligatoria, se producirá una excepción. Las vistas individuales especifican el contenido que se va a representar dentro de una sección con la sintaxis `@section` de Razor. Si una vista define una sección, se debe representar (o se producirá un error).

Ejemplo de definición de `@section` en una vista:

```html
@section Scripts {
     <script type="text/javascript" src="/scripts/main.js"></script>
   }
   ```

En el código anterior, se agregan scripts de validación a la sección `scripts` en una vista que incluye un formulario. Es posible que otras vistas de la misma aplicación no necesiten scripts adicionales, por lo que no sería necesario definir una sección de scripts.

Las secciones definidas en una vista solo están disponibles en su página de diseño inmediato. No se puede hacer referencia a ellas desde líneas de código parcialmente ejecutadas, componentes de vista u otros elementos del sistema de vistas.

### <a name="ignoring-sections"></a>Omitir secciones

De forma predeterminada, el cuerpo y todas las secciones de una página de contenido deben representarse mediante la página de diseño. Para cumplir con esto, el motor de vistas de Razor comprueba si el cuerpo y cada sección se han representado.

Para indicar al motor de vistas que pase por alto el cuerpo o las secciones, llame a los métodos `IgnoreBody` y `IgnoreSection`.

El cuerpo y todas las secciones de una página de Razor deben representarse o pasarse por alto.

<a name="viewimports"></a>

## <a name="importing-shared-directives"></a>Importar directivas compartidas

Las vistas pueden usar directivas de Razor para hacer muchas cosas, como importar espacios de nombres o realizar una [inserción de dependencias](dependency-injection.md). Se pueden especificar varias directivas compartidas por muchas vistas en un archivo `_ViewImports.cshtml` común. El archivo `_ViewImports` es compatible con estas directivas:

* `@addTagHelper`

* `@removeTagHelper`

* `@tagHelperPrefix`

* `@using`

* `@model`

* `@inherits`

* `@inject`

El archivo no es compatible con otras características de Razor, como las funciones y las definiciones de sección.

Archivo `_ViewImports.cshtml` de ejemplo:

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewImports.cshtml)]

El archivo `_ViewImports.cshtml` para una aplicación ASP.NET Core MVC normalmente se coloca en la carpeta `Views`. Un archivo `_ViewImports.cshtml` puede colocarse dentro de cualquier carpeta, en cuyo caso solo se aplicará a vistas dentro de esa carpeta y sus subcarpetas. Los archivos `_ViewImports` se procesan a partir del nivel de raíz y después para cada carpeta que lleva hasta la ubicación de la propia vista, por lo que la configuración especificada en el nivel de raíz es posible que se invalide en el nivel de carpeta.

Por ejemplo, si un archivo `_ViewImports.cshtml` de nivel de raíz especifica `@model` y `@addTagHelper`, y otro archivo `_ViewImports.cshtml` en la carpeta asociada al controlador de la vista especifica un `@model` diferente y agrega otro `@addTagHelper`, la vista tendrá acceso a las aplicaciones auxiliares de etiquetas y usará el último `@model`.

Si se ejecutan varios archivos `_ViewImports.cshtml` para una vista, el comportamiento combinado de las directivas incluidas en los archivos `ViewImports.cshtml` será el siguiente:

* `@addTagHelper`, `@removeTagHelper`: todos se ejecutan en orden

* `@tagHelperPrefix`: el más cercano a la vista invalida los demás

* `@model`: el más cercano a la vista invalida los demás

* `@inherits`: el más cercano a la vista invalida los demás

* `@using`: todos se incluyen y se omiten los duplicados

* `@inject`: para cada propiedad, la más cercana a la vista invalida cualquier otra con el mismo nombre de propiedad

<a name="viewstart"></a>

## <a name="running-code-before-each-view"></a>Ejecutar código antes de cada vista

Si tiene código que debe ejecutar antes de cada vista, debe colocarlo en el archivo `_ViewStart.cshtml`. Por convención, el archivo `_ViewStart.cshtml` se encuentra en la carpeta `Views`. Las instrucciones que aparecen en `_ViewStart.cshtml` se ejecutan antes de cada vista completa (no los diseños ni las vistas parciales). Al igual que [ViewImports.cshtml](xref:mvc/views/layout#viewimports), `_ViewStart.cshtml` tiene una estructura jerárquica. Si se define un archivo `_ViewStart.cshtml` en la carpeta de vista asociada al controlador, se ejecutará después del que está definido en la raíz de la carpeta `Views` (si existe).

Archivo `_ViewStart.cshtml` de ejemplo:

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewStart.cshtml)]

El archivo anterior especifica que todas las vistas usarán el diseño `_Layout.cshtml`.

> [!NOTE]
> Ni `_ViewStart.cshtml` ni `_ViewImports.cshtml` se colocan normalmente en la carpeta `/Views/Shared`. Las versiones de nivel de aplicación de estos archivos deben colocarse directamente en la carpeta `/Views`.
