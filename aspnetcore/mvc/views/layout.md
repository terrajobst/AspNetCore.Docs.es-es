---
title: "Diseño"
author: ardalis
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 29f12d1f-9734-48bd-bf1a-cee53a8ab700
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/layout
ms.openlocfilehash: 064621d8756b007c5b8859111bf3a03a0d7dda81
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
# <a name="layout"></a>Diseño

Por [Steve Smith](https://ardalis.com/)

Vistas a menudo compartan elementos visuales y mediante programación. En este artículo, aprenderá a usar diseños comunes, compartir directivas y ejecutar el código común antes de la representación de vistas en la aplicación ASP.NET.

## <a name="what-is-a-layout"></a>¿Qué es un diseño

Mayoría de las aplicaciones web tiene un diseño común que ofrece al usuario una experiencia coherente mientras navegan por las páginas. El diseño suele incluir elementos de interfaz de usuario comunes como encabezado de la aplicación, navegación o elementos de menú y pie de página.

![Ejemplo de diseño de página](layout/_static/page-layout.png)

Estructuras de HTML comunes, como scripts y hojas de estilos también frecuente por muchas páginas dentro de una aplicación. Todos estos elementos compartidos se pueden definir en un *diseño* archivo, que, a continuación, hacer referencia a cualquier vista que se utiliza dentro de la aplicación. Diseños de reducen el código duplicado en vistas, que les ayudan a seguir la [principio no repita usted mismo (SECA)](http://deviq.com/don-t-repeat-yourself/).

Por convención, el diseño predeterminado para una aplicación ASP.NET se denomina `_Layout.cshtml`. La plantilla de proyecto de Visual Studio ASP.NET Core MVC incluye este archivo de diseño en el `Views/Shared` carpeta:

![carpeta de vistas en el Explorador de soluciones](layout/_static/web-project-views.png)

Este diseño define una plantilla de nivel superior para las vistas en la aplicación. Las aplicaciones no requieren un diseño y aplicaciones pueden definir más de un diseño con distintas vistas especificar diseños diferentes.

Un ejemplo `_Layout.cshtml`:

[!code-html[Main](../../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=42,66)]

## <a name="specifying-a-layout"></a>Especificación de un diseño

Vistas de Razor tienen un `Layout` propiedad. Vistas individuales especifican un diseño mediante el establecimiento de esta propiedad:

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewStart.cshtml?highlight=2)]

El diseño especificado puede usar una ruta de acceso completa (ejemplo: `/Views/Shared/_Layout.cshtml`) o un nombre parcial (ejemplo: `_Layout`). Cuando se proporciona un nombre parcial, el motor de vista Razor buscará el archivo de diseño mediante su proceso de detección estándar. La carpeta de controlador asociados se busca en primer lugar, seguido por el `Shared` carpeta. Este proceso de detección es idéntico al utilizado para detectar [vistas parciales](partial.md).

De forma predeterminada, deben llamar todos los diseños de `RenderBody`. Siempre que la llamada a `RenderBody` es colocar, se representará el contenido de la vista.

<a name="layout-sections-label"></a>

### <a name="sections"></a>Secciones

Puede hacer referencia a un diseño, opcionalmente, uno o varios *secciones*, mediante una llamada a `RenderSection`. Secciones proporcionan una manera de organizar dónde se deben colocar determinados elementos de la página. Cada llamada a `RenderSection` puede especificar si esa sección es obligatorio u opcional. Si no se encuentra una sección obligatoria, se producirá una excepción. Vistas individuales especifican el contenido que se representará dentro de una sección con la `@section` sintaxis Razor. Si una vista define una sección, se debe representar (o se producirá un error).

Un ejemplo `@section` definición en una vista:

```html
@section Scripts {
     <script type="text/javascript" src="/scripts/main.js"></script>
   }
   ```

En el código anterior, las secuencias de comandos de validación se agregan a la `scripts` sección en una vista que incluya un formulario. Otras vistas de la misma aplicación podrían no requerir scripts adicionales y, por lo que no sería necesario definir una sección de secuencias de comandos.

Las secciones definidas en una vista solo están disponibles en su página de diseño inmediato. Sí No se pueden hacer referencia desde parciales, componentes de la vista u otras partes del sistema de la vista.

### <a name="ignoring-sections"></a>Omitir secciones

De forma predeterminada, el cuerpo y todas las secciones en una página de contenido deben todas se representan mediante la página de diseño. El motor de vista Razor exige al realizar un seguimiento si el cuerpo y cada sección se han procesado.

Para indicar al motor de vista para pasar por alto el cuerpo o secciones, llame a la `IgnoreBody` y `IgnoreSection` métodos.

El cuerpo y todas las secciones en una página de Razor deben ser representan o pasa por alto.

<a name="viewimports"></a>

## <a name="importing-shared-directives"></a>Importar directivas compartidas

Vistas pueden usar las directivas de Razor para hacer muchas cosas, como la importación de espacios de nombres o realizar [inyección de dependencia](dependency-injection.md). Se pueden especificar directivas compartidas muchas vistas en común `_ViewImports.cshtml` archivo. El `_ViewImports` archivo es compatible con las siguientes directivas:

* `@addTagHelper`

* `@removeTagHelper`

* `@tagHelperPrefix`

* `@using`

* `@model`

* `@inherits`

* `@inject`

El archivo no es compatible con otras características de Razor, como las funciones y las definiciones de sección.

Un ejemplo de `_ViewImports.cshtml` archivo:

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewImports.cshtml)]

El `_ViewImports.cshtml` de archivos para una aplicación de MVC de ASP.NET Core normalmente se coloca en el `Views` carpeta. Un `_ViewImports.cshtml` archivo puede colocarse dentro de cualquier carpeta, en el que caso solo se aplicarán a vistas dentro de esa carpeta y sus subcarpetas. `_ViewImports`se procesan los archivos a partir del nivel raíz, y, a continuación, para cada carpeta llevan a la ubicación de la propia vista, por lo que la configuración especificada en el nivel de raíz se pueden invalidar en el nivel de carpeta.

Por ejemplo, si un nivel de raíz `_ViewImports.cshtml` archivo especifica `@model` y `@addTagHelper`y otro `_ViewImports.cshtml` especifica otro archivo en la carpeta controlador de asociados de la vista `@model` y agrega otro `@addTagHelper`, la vista tendrá acceso a las aplicaciones auxiliares de etiquetas y usará el último `@model`.

Si hay varios `_ViewImports.cshtml` archivos se ejecutan para una vista, combina el comportamiento de las directivas que se incluyen en el `ViewImports.cshtml` archivos será la siguiente:

* `@addTagHelper`, `@removeTagHelper`: ejecutar, en orden

* `@tagHelperPrefix`: lo más cercano a la vista invalida cualquier otra

* `@model`: lo más cercano a la vista invalida cualquier otra

* `@inherits`: lo más cercano a la vista invalida cualquier otra

* `@using`: todos se incluyen; se omiten los duplicados

* `@inject`: para cada propiedad, lo más cercano a la vista invalida cualquier otra con el mismo nombre de propiedad

<a name="viewstart"></a>

## <a name="running-code-before-each-view"></a>Ejecutar código antes de cada vista

Si tiene código deba ejecutar antes de cada vista, esto se debe colocar en el `_ViewStart.cshtml` archivo. Por convención, el `_ViewStart.cshtml` archivo se encuentra en la `Views` carpeta. Las instrucciones que aparecen en `_ViewStart.cshtml` se ejecutan antes de cada vista completa (no los diseños y vistas no parciales). Al igual que [ViewImports.cshtml](xref:mvc/views/layout#viewimports), `_ViewStart.cshtml` tiene una estructura jerárquica. Si un `_ViewStart.cshtml` archivo se define en la carpeta de la vista asociada de controlador, se ejecutará después de la definida en la raíz de la `Views` carpeta (si existe).

Un ejemplo de `_ViewStart.cshtml` archivo:

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewStart.cshtml)]

El archivo anterior especifica que todas las vistas van a utilizar el `_Layout.cshtml` diseño.

> [!NOTE]
> Ni `_ViewStart.cshtml` ni `_ViewImports.cshtml` normalmente se colocan en la `/Views/Shared` carpeta. Las versiones de nivel de aplicación de estos archivos deben colocarse directamente en el `/Views` carpeta.
