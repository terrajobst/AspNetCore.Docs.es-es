---
title: Diseños de los componentes de Razor
author: guardrex
description: Obtenga información sobre cómo crear componentes reutilizables de diseño para las aplicaciones de componentes de Razor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/07/2019
uid: razor-components/layouts
ms.openlocfilehash: 31ed940ce40e3ae6e3744418cf241d396308f4fe
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2019
ms.locfileid: "59515644"
---
# <a name="razor-components-layouts"></a>Diseños de los componentes de Razor

Por [Rainer Stropek](https://www.timecockpit.com)

Normalmente, las aplicaciones contienen más de un componente. Elementos de diseño, como menús, los mensajes de copyright y logotipos, deben estar presentes en todos los componentes. Copiando el código de estos elementos de diseño en todos los componentes de una aplicación no es un enfoque eficaz. Esta duplicación es difícil de mantener y probablemente se lleva a contenido incoherente con el tiempo. *Los diseños* solucionar este problema.

Técnicamente, un diseño es simplemente otro componente. Un diseño que se define en una plantilla de Razor o en C# de código y pueden contener [enlace de datos](xref:razor-components/components#data-binding), [inserción de dependencias](xref:razor-components/dependency-injection)y otras características de los componentes normales.

Dos aspectos adicionales activar un *componente* en un *diseño*

* El componente de diseño debe heredar de `LayoutComponentBase`. `LayoutComponentBase` define un `Body` propiedad que contiene el contenido que se representará dentro del diseño.
* Usa el componente de diseño la `Body` propiedad para especificar que el contenido del cuerpo deben representar mediante la sintaxis de Razor `@Body`. Durante la representación, `@Body` se sustituye por el contenido del diseño.

Ejemplo de código siguiente muestra la plantilla de Razor de un componente de diseño. Tenga en cuenta el uso de `LayoutComponentBase` y `@Body`:

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterLayout.cshtml)]

## <a name="use-a-layout-in-a-component"></a>Usar un diseño en un componente

Utilice la directiva de Razor `@layout` para aplicar un diseño a un componente. El compilador convierte esta directiva en un `LayoutAttribute`, que se aplica a la clase de componente.

Ejemplo de código siguiente muestra el concepto. El contenido de este componente se inserta en la *MasterLayout* en la posición del `@Body`:

```cshtml
@layout MasterLayout
@page "/master-list"

<h2>Master Episode List</h2>
```

## <a name="centralized-layout-selection"></a>Selección del diseño centralizada

Todas las carpetas de un una aplicación puede contener opcionalmente un archivo de plantilla denominado *_ViewImports.cshtml*. El compilador incluye las directivas especificadas en el archivo de importaciones de vista en todas las plantillas de Razor en la misma carpeta y de forma recursiva en todas sus subcarpetas. Por lo tanto, un *_ViewImports.cshtml* que contiene el archivo `@layout MainLayout` garantiza que todos los componentes en una carpeta, use el *MainLayout* diseño. No es necesario para agregar varias veces `@layout` a todos los *.razor* archivos.

Tenga en cuenta que la plantilla predeterminada usa la *_ViewImports.cshtml* mecanismo para la selección de diseño. Contiene una aplicación recién creada el *_ViewImports.cshtml* de archivos en el *componentes, páginas* carpeta.

## <a name="nested-layouts"></a>Diseños anidados

Las aplicaciones pueden constar de los diseños anidados. Un componente puede hacer referencia a un diseño que a su vez hace referencia a otra distribución. Por ejemplo, los diseños de anidamiento pueden usarse para reflejar una estructura de varios niveles de menú.

Ejemplos de código siguientes muestran cómo utilizar los diseños anidados. El *EpisodesComponent.cshtml* archivo es el componente para mostrar. Tenga en cuenta que el componente hace referencia el diseño `MasterListLayout`.

*EpisodesComponent.cshtml*:

```cshtml
@layout MasterListLayout
@page "/master-list/episodes"

<h1>Episodes</h1>
```

El *MasterListLayout.cshtml* archivo proporciona el `MasterListLayout`. El diseño hace referencia a otro diseño `MasterLayout`, donde se va a insertarse.

*MasterListLayout.cshtml*:

```cshtml
@layout MasterLayout
@inherits LayoutComponentBase

<nav>
    <!-- Menu structure of master list -->
    ...
</nav>

@Body
```

Por último, `MasterLayout` contiene los elementos de diseño de nivel superior, como el encabezado, pie de página y el menú principal.

*MasterLayout.cshtml*:

```cshtml
@inherits LayoutComponentBase

<header>...</header>
<nav>...</nav>

@Body
```
