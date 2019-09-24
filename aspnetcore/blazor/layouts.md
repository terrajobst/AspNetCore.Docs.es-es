---
title: ASP.NET Core diseños increíbles
author: guardrex
description: Aprenda a crear componentes de diseño reutilizables para aplicaciones increíbles.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/23/2019
uid: blazor/layouts
ms.openlocfilehash: 064ffafb8e7f3e76e5bd7bde0e06ef271fe92542
ms.sourcegitcommit: 79eeb17604b536e8f34641d1e6b697fb9a2ee21f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2019
ms.locfileid: "71211588"
---
# <a name="aspnet-core-blazor-layouts"></a>ASP.NET Core diseños increíbles

Por [Rainer Stropek](https://www.timecockpit.com) y [Luke Latham](https://github.com/guardrex)

Algunos elementos de la aplicación, como los menús, los mensajes de copyright y los logotipos de la empresa, normalmente forman parte del diseño general de la aplicación y se usan en todos los componentes de la aplicación. Copiar el código de estos elementos en todos los componentes de una aplicación no es un enfoque&mdash;eficaz cada vez que uno de los elementos requiere una actualización, todos los componentes deben actualizarse. Esta duplicación es difícil de mantener y puede dar lugar a contenido incoherente a lo largo del tiempo. Los *diseños* solucionan este problema.

Técnicamente, un diseño es simplemente otro componente. Un diseño se define en una plantilla de Razor o C# en el código y puede usar el [enlace de datos](xref:blazor/components#data-binding), la [inserción de dependencias](xref:blazor/dependency-injection)y otros escenarios de componentes.

Para convertir un *componente* en un *diseño*, el componente:

* Hereda de `LayoutComponentBase`, que define una `Body` propiedad para el contenido representado dentro del diseño.
* Utiliza el sintaxis Razor `@Body` para especificar la ubicación en el marcado de diseño donde se representa el contenido.

En el ejemplo de código siguiente se muestra la plantilla de Razor de un componente de diseño, *MainLayout. Razor*. El diseño hereda `LayoutComponentBase` y `@Body` establece entre la barra de navegación y el pie de página:

[!code-cshtml[](layouts/sample_snapshot/3.x/MainLayout.razor?highlight=1,13)]

En una aplicación basada en una de las plantillas de aplicación de extraordinarias `MainLayout` , el componente (*MainLayout. Razor*) está en la carpeta *compartida* de la aplicación.

## <a name="default-layout"></a>Diseño predeterminado

Especifique el diseño de aplicación predeterminado en `Router` el componente del archivo *app. Razor* de la aplicación. El siguiente `Router` componente, que proporcionan las plantillas de increíble predeterminada, establece el diseño predeterminado para el `MainLayout` componente:

[!code-cshtml[](layouts/sample_snapshot/3.x/App1.razor?highlight=3)]

Para proporcionar un diseño predeterminado para `NotFound` el contenido, especifique `LayoutView` un `NotFound` para contenido:

[!code-cshtml[](layouts/sample_snapshot/3.x/App2.razor?highlight=6-9)]

Para obtener más información sobre `Router` el componente, <xref:blazor/routing>vea.

Especificar el diseño como un diseño predeterminado en el enrutador es una práctica útil porque se puede reemplazar por componente o por carpeta. Prefiere usar el enrutador para establecer el diseño predeterminado de la aplicación, ya que es la técnica más general.

## <a name="specify-a-layout-in-a-component"></a>Especificar un diseño en un componente

Use la directiva `@layout` Razor para aplicar un diseño a un componente. El compilador `@layout` convierte `LayoutAttribute`en, que se aplica a la clase de componente.

El contenido del componente siguiente `MasterList` se inserta en la `MasterLayout` posición de `@Body`:

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterList.razor?highlight=1)]

Al especificar el diseño directamente en un componente, se invalida un *diseño predeterminado* establecido en el enrutador `@layout` o una directiva importada desde *_Imports. Razor*.

## <a name="centralized-layout-selection"></a>Selección de diseño centralizado

Cada carpeta de una aplicación puede contener opcionalmente un archivo de plantilla denominado *_Imports. Razor*. El compilador incluye las directivas especificadas en el archivo de importaciones en todas las plantillas de Razor de la misma carpeta y de forma recursiva en todas sus subcarpetas. Por lo tanto, un archivo *_Imports. Razor* que contenga `@layout MyCoolLayout` garantiza que todos los componentes de `MyCoolLayout`una carpeta usan. No es necesario agregar `@layout MyCoolLayout` varias veces a todos los archivos *. Razor* dentro de la carpeta y las subcarpetas. `@using`las directivas también se aplican a los componentes de la misma manera.

El siguiente archivo *_Imports. Razor* importa:

* `MyCoolLayout`.
* Todos los componentes de Razor en la misma carpeta y en todas las subcarpetas.
* El espacio de nombres `BlazorApp1.Data` .
 
[!code-cshtml[](layouts/sample_snapshot/3.x/_Imports.razor)]

El archivo *_Imports. Razor* es similar al [archivo _ViewImports. cshtml para las vistas y páginas de Razor,](xref:mvc/views/layout#importing-shared-directives) pero se aplica específicamente a los archivos de componentes de Razor.

Al especificar un diseño en *_Imports. Razor* , se invalida un diseño especificado como el *diseño predeterminado*del enrutador.

## <a name="nested-layouts"></a>Diseños anidados

Las aplicaciones pueden estar formadas por diseños anidados. Un componente puede hacer referencia a un diseño que a su vez hace referencia a otro diseño. Por ejemplo, los diseños anidados se usan para crear una estructura de menú de varios niveles.

En el ejemplo siguiente se muestra cómo usar diseños anidados. El archivo *EpisodesComponent. Razor* es el componente que se va a mostrar. El componente hace referencia `MasterListLayout`a:

[!code-cshtml[](layouts/sample_snapshot/3.x/EpisodesComponent.razor?highlight=1)]

El archivo *MasterListLayout. Razor* proporciona el `MasterListLayout`. El diseño hace referencia a otro `MasterLayout`diseño,, donde se representa. `EpisodesComponent`se representa donde `@Body` aparece:

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterListLayout.razor?highlight=1,9)]

Por último `MasterLayout` , en *MasterLayout. Razor* contiene los elementos de diseño de nivel superior, como el encabezado, el menú principal y el pie de página. `MasterListLayout``@Body` con, `EpisodesComponent` se representa donde aparece:

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterLayout.razor?highlight=6)]

## <a name="additional-resources"></a>Recursos adicionales

* <xref:mvc/views/layout>
