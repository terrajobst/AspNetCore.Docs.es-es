---
title: Aplicación auxiliar de etiquetas de componentes en ASP.NET Core
author: guardrex
ms.author: riande
description: Aprenda a usar la aplicación auxiliar de etiquetas de componentes de ASP.NET Core para representar componentes de Razor en páginas y vistas.
ms.custom: mvc
ms.date: 03/18/2020
no-loc:
- Blazor
- SignalR
uid: mvc/views/tag-helpers/builtin-th/component-tag-helper
ms.openlocfilehash: 801ceb73de5bb4ef7500624e1fbddbf96d1ab89c
ms.sourcegitcommit: 91dc1dd3d055b4c7d7298420927b3fd161067c64
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "80226385"
---
# <a name="component-tag-helper-in-aspnet-core"></a>Aplicación auxiliar de etiquetas de componentes en ASP.NET Core

Por [Daniel Roth](https://github.com/danroth27) y [Luke Latham](https://github.com/guardrex)

Para representar un componente de una página o vista, use la [aplicación auxiliar de etiquetas de componentes](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ComponentTagHelper).

La siguiente aplicación auxiliar de etiquetas de componentes representa el componente de `Counter` en una página o vista:

```cshtml
<component type="typeof(Counter)" render-mode="ServerPrerendered" />
```

La aplicación auxiliar de etiquetas de componente también puede pasar parámetros a los componentes. Considere el siguiente `ColorfulCheckbox` componente que establece el color y el tamaño de la etiqueta de la casilla:

```razor
<label style="font-size:@(Size)px;color:@Color">
    <input @bind="Value"
           id="survey" 
           name="blazor" 
           type="checkbox" />
    Enjoying Blazor?
</label>

@code {
    [Parameter]
    public bool Value { get; set; }

    [Parameter]
    public int Size { get; set; } = 8;

    [Parameter]
    public string Color { get; set; }

    protected override void OnInitialized()
    {
        Size += 10;
    }
}
```

Los [parámetros de componente](xref:blazor/components#component-parameters) `Size` (`int`) y `Color` (`string`) se pueden establecer mediante la aplicación auxiliar de etiquetas de componentes:

```cshtml
<component type="typeof(ColorfulCheckbox)" render-mode="ServerPrerendered" 
    param-Size="14" param-Color="@("blue")" />
```

El siguiente código HTML se representa en la página o la vista:

```html
<label style="font-size:24px;color:blue">
    <input id="survey" name="blazor" type="checkbox">
    Enjoying Blazor?
</label>
```

El paso de una cadena entrecomillada requiere una [expresión de Razor explícita](xref:mvc/views/razor#explicit-razor-expressions), como se muestra para `param-Color` en el ejemplo anterior. El comportamiento de análisis de Razor para un valor de tipo `string` no se aplica a un atributo `param-*` porque el atributo es un tipo `object`.

El tipo de parámetro debe ser JSON serializable, lo que normalmente significa que el tipo debe tener un constructor predeterminado y propiedades configurables. Por ejemplo, puede especificar un valor para `Size` y `Color` en el ejemplo anterior porque los tipos de `Size` y `Color` son tipos primitivos (`int` y `string`), que son compatibles con el serializador JSON.

<xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode> configura si el componente:

* Se representa previamente en la página.
* Se representa como HTML estático en la página o si incluye la información necesaria para arrancar una aplicación Blazor desde el agente de usuario.

| Modo de representación | Descripción |
| ----------- | ----------- |
| <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.ServerPrerendered> | Representa el componente en código HTML estático e incluye un marcador para una aplicación Blazor Server. Cuando se inicia el agente de usuario, este marcador se usa para arrancar una aplicación Blazor. |
| <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Server> | Representa un marcador para una aplicación Blazor Server. La salida del componente no está incluida. Cuando se inicia el agente de usuario, este marcador se usa para arrancar una aplicación Blazor. |
| <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Static> | Representa el componente en HTML estático. |

Mientras que las páginas y las vistas pueden utilizar componentes, lo contrario no es cierto. Los componentes no pueden usar características específicas de la página y de la vista, como vistas y secciones parciales. Para usar la lógica de una vista parcial en un componente, se debe factorizar la lógica de vista parcial en un componente.

No se admite la representación de componentes de servidor desde una página HTML estática.

## <a name="additional-resources"></a>Recursos adicionales

* <xref:Microsoft.AspNetCore.Mvc.TagHelpers.ComponentTagHelper>
* <xref:mvc/views/tag-helpers/intro>
* <xref:blazor/components>
