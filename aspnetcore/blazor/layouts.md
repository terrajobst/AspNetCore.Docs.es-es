---
title: ASP.NET Core diseños de Blazor
author: guardrex
description: Aprenda a crear componentes de diseño reutilizables para aplicaciones Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2019
no-loc:
- Blazor
- SignalR
uid: blazor/layouts
ms.openlocfilehash: 8e7294f6b66d34781473522a71f929ed5f9c33f2
ms.sourcegitcommit: d2ba66023884f0dca115ff010bd98d5ed6459283
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/14/2020
ms.locfileid: "77213381"
---
# <a name="aspnet-core-opno-locblazor-layouts"></a>ASP.NET Core diseños de Blazor

Por [Rainer Stropek](https://www.timecockpit.com) y [Luke Latham](https://github.com/guardrex)

Algunos elementos de la aplicación, como los menús, los mensajes de copyright y los logotipos de la empresa, normalmente forman parte del diseño general de la aplicación y se usan en todos los componentes de la aplicación. Copiar el código de estos elementos en todos los componentes de una aplicación no es un enfoque eficaz&mdash;cada vez que uno de los elementos requiere una actualización, todos los componentes deben actualizarse. Esta duplicación es difícil de mantener y puede dar lugar a contenido incoherente a lo largo del tiempo. Los *diseños* solucionan este problema.

Técnicamente, un diseño es simplemente otro componente. Un diseño se define en una plantilla de Razor o C# en el código y puede usar el [enlace de datos](xref:blazor/components#data-binding), la [inserción de dependencias](xref:blazor/dependency-injection)y otros escenarios de componentes.

Para convertir un *componente* en un *diseño*, el componente:

* Hereda de `LayoutComponentBase`, que define una propiedad `Body` para el contenido representado dentro del diseño.
* Usa el `@Body` sintaxis Razor para especificar la ubicación en el marcado de diseño donde se representa el contenido.

En el ejemplo de código siguiente se muestra la plantilla de Razor de un componente de diseño, *MainLayout. Razor*. El diseño hereda `LayoutComponentBase` y establece el `@Body` entre la barra de navegación y el pie de página:

[!code-razor[](layouts/sample_snapshot/3.x/MainLayout.razor?highlight=1,13)]

En una aplicación basada en una de las plantillas de aplicación de Blazor, el componente `MainLayout` (*MainLayout. Razor*) se encuentra en la carpeta *compartida* de la aplicación.

## <a name="default-layout"></a>Diseño predeterminado

Especifique el diseño de la aplicación predeterminada en el componente `Router` del archivo *app. Razor* de la aplicación. El siguiente componente de `Router`, proporcionado por el Blazor plantillas predeterminadas, establece el diseño predeterminado para el componente `MainLayout`:

[!code-razor[](layouts/sample_snapshot/3.x/App1.razor?highlight=3)]

Para proporcionar un diseño predeterminado para `NotFound` contenido, especifique un `LayoutView` para `NotFound` contenido:

[!code-razor[](layouts/sample_snapshot/3.x/App2.razor?highlight=6-9)]

Para obtener más información sobre el componente de `Router`, vea <xref:blazor/routing>.

Especificar el diseño como un diseño predeterminado en el enrutador es una práctica útil porque se puede reemplazar por componente o por carpeta. Prefiere usar el enrutador para establecer el diseño predeterminado de la aplicación, ya que es la técnica más general.

## <a name="specify-a-layout-in-a-component"></a>Especificar un diseño en un componente

Use la Directiva Razor `@layout` para aplicar un diseño a un componente. El compilador convierte `@layout` en una `LayoutAttribute`, que se aplica a la clase de componente.

El contenido del componente de `MasterList` siguiente se inserta en el `MasterLayout` en la posición de `@Body`:

[!code-razor[](layouts/sample_snapshot/3.x/MasterList.razor?highlight=1)]

Al especificar el diseño directamente en un componente, se invalida un *diseño predeterminado* establecido en el enrutador o una directiva de `@layout` importada de *_Imports. Razor*.

## <a name="centralized-layout-selection"></a>Selección de diseño centralizado

Cada carpeta de una aplicación puede contener opcionalmente un archivo de plantilla denominado *_Imports. Razor*. El compilador incluye las directivas especificadas en el archivo de importaciones en todas las plantillas de Razor de la misma carpeta y de forma recursiva en todas sus subcarpetas. Por lo tanto, un archivo *_Imports. Razor* que contiene `@layout MyCoolLayout` garantiza que todos los componentes de una carpeta utilicen `MyCoolLayout`. No es necesario agregar varias veces `@layout MyCoolLayout` a todos los archivos *. Razor* dentro de la carpeta y las subcarpetas. las directivas de `@using` también se aplican a los componentes de la misma manera.

El siguiente archivo *_Imports. Razor* importa:

* `MyCoolLayout`.
* Todos los componentes de Razor en la misma carpeta y en todas las subcarpetas.
* Espacio de nombres `BlazorApp1.Data`.
 
[!code-razor[](layouts/sample_snapshot/3.x/_Imports.razor)]

El archivo *_Imports. Razor* es similar al [archivo _ViewImports. cshtml para las vistas y páginas de Razor,](xref:mvc/views/layout#importing-shared-directives) pero se aplica específicamente a los archivos de componentes de Razor.

Especificar un diseño en *_Imports. Razor* invalida un diseño especificado como el *diseño predeterminado*del enrutador.

## <a name="nested-layouts"></a>Diseños anidados

Las aplicaciones pueden estar formadas por diseños anidados. Un componente puede hacer referencia a un diseño que a su vez hace referencia a otro diseño. Por ejemplo, los diseños anidados se usan para crear una estructura de menú de varios niveles.

En el ejemplo siguiente se muestra cómo usar diseños anidados. El archivo *EpisodesComponent. Razor* es el componente que se va a mostrar. El componente hace referencia al `MasterListLayout`:

[!code-razor[](layouts/sample_snapshot/3.x/EpisodesComponent.razor?highlight=1)]

El archivo *MasterListLayout. Razor* proporciona el `MasterListLayout`. El diseño hace referencia a otro diseño, `MasterLayout`, donde se representa. `EpisodesComponent` se representa donde aparece `@Body`:

[!code-razor[](layouts/sample_snapshot/3.x/MasterListLayout.razor?highlight=1,9)]

Por último, `MasterLayout` en *MasterLayout. Razor* contiene los elementos de diseño de nivel superior, como el encabezado, el menú principal y el pie de página. `MasterListLayout` con el `EpisodesComponent` se representa donde aparece `@Body`:

[!code-razor[](layouts/sample_snapshot/3.x/MasterLayout.razor?highlight=6)]

## <a name="share-a-razor-pages-layout-with-integrated-components"></a>Compartir un diseño de Razor Pages con componentes integrados

Cuando los componentes enrutables se integran en una aplicación Razor Pages, el diseño compartido de la aplicación se puede usar con los componentes. Para más información, consulte <xref:blazor/hosting-model-configuration#integrate-razor-components-into-razor-pages-and-mvc-apps>.

## <a name="additional-resources"></a>Recursos adicionales

* <xref:mvc/views/layout>
