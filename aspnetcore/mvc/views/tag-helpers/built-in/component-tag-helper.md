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
# <a name="component-tag-helper-in-aspnet-core"></a><span data-ttu-id="8cef5-103">Aplicación auxiliar de etiquetas de componentes en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8cef5-103">Component Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="8cef5-104">Por [Daniel Roth](https://github.com/danroth27) y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="8cef5-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="8cef5-105">Para representar un componente de una página o vista, use la [aplicación auxiliar de etiquetas de componentes](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ComponentTagHelper).</span><span class="sxs-lookup"><span data-stu-id="8cef5-105">To render a component from a page or view, use the [Component Tag Helper](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ComponentTagHelper).</span></span>

<span data-ttu-id="8cef5-106">La siguiente aplicación auxiliar de etiquetas de componentes representa el componente de `Counter` en una página o vista:</span><span class="sxs-lookup"><span data-stu-id="8cef5-106">The following Component Tag Helper renders the `Counter` component in a page or view:</span></span>

```cshtml
<component type="typeof(Counter)" render-mode="ServerPrerendered" />
```

<span data-ttu-id="8cef5-107">La aplicación auxiliar de etiquetas de componente también puede pasar parámetros a los componentes.</span><span class="sxs-lookup"><span data-stu-id="8cef5-107">The Component Tag Helper can also pass parameters to components.</span></span> <span data-ttu-id="8cef5-108">Considere el siguiente `ColorfulCheckbox` componente que establece el color y el tamaño de la etiqueta de la casilla:</span><span class="sxs-lookup"><span data-stu-id="8cef5-108">Consider the following `ColorfulCheckbox` component that sets the check box label's color and size:</span></span>

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

<span data-ttu-id="8cef5-109">Los [parámetros de componente](xref:blazor/components#component-parameters) `Size` (`int`) y `Color` (`string`) se pueden establecer mediante la aplicación auxiliar de etiquetas de componentes:</span><span class="sxs-lookup"><span data-stu-id="8cef5-109">The `Size` (`int`) and `Color` (`string`) [component parameters](xref:blazor/components#component-parameters) can be set by the Component Tag Helper:</span></span>

```cshtml
<component type="typeof(ColorfulCheckbox)" render-mode="ServerPrerendered" 
    param-Size="14" param-Color="@("blue")" />
```

<span data-ttu-id="8cef5-110">El siguiente código HTML se representa en la página o la vista:</span><span class="sxs-lookup"><span data-stu-id="8cef5-110">The following HTML is rendered in the page or view:</span></span>

```html
<label style="font-size:24px;color:blue">
    <input id="survey" name="blazor" type="checkbox">
    Enjoying Blazor?
</label>
```

<span data-ttu-id="8cef5-111">El paso de una cadena entrecomillada requiere una [expresión de Razor explícita](xref:mvc/views/razor#explicit-razor-expressions), como se muestra para `param-Color` en el ejemplo anterior.</span><span class="sxs-lookup"><span data-stu-id="8cef5-111">Passing a quoted string requires an [explicit Razor expression](xref:mvc/views/razor#explicit-razor-expressions), as shown for `param-Color` in the preceding example.</span></span> <span data-ttu-id="8cef5-112">El comportamiento de análisis de Razor para un valor de tipo `string` no se aplica a un atributo `param-*` porque el atributo es un tipo `object`.</span><span class="sxs-lookup"><span data-stu-id="8cef5-112">The Razor parsing behavior for a `string` type value doesn't apply to a `param-*` attribute because the attribute is an `object` type.</span></span>

<span data-ttu-id="8cef5-113">El tipo de parámetro debe ser JSON serializable, lo que normalmente significa que el tipo debe tener un constructor predeterminado y propiedades configurables.</span><span class="sxs-lookup"><span data-stu-id="8cef5-113">The parameter type must be JSON serializable, which typically means that the type must have a default constructor and settable properties.</span></span> <span data-ttu-id="8cef5-114">Por ejemplo, puede especificar un valor para `Size` y `Color` en el ejemplo anterior porque los tipos de `Size` y `Color` son tipos primitivos (`int` y `string`), que son compatibles con el serializador JSON.</span><span class="sxs-lookup"><span data-stu-id="8cef5-114">For example, you can specify a value for `Size` and `Color` in the preceding example because the types of `Size` and `Color` are primitive types (`int` and `string`), which are supported by the JSON serializer.</span></span>

<span data-ttu-id="8cef5-115"><xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode> configura si el componente:</span><span class="sxs-lookup"><span data-stu-id="8cef5-115"><xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode> configures whether the component:</span></span>

* <span data-ttu-id="8cef5-116">Se representa previamente en la página.</span><span class="sxs-lookup"><span data-stu-id="8cef5-116">Is prerendered into the page.</span></span>
* <span data-ttu-id="8cef5-117">Se representa como HTML estático en la página o si incluye la información necesaria para arrancar una aplicación Blazor desde el agente de usuario.</span><span class="sxs-lookup"><span data-stu-id="8cef5-117">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| <span data-ttu-id="8cef5-118">Modo de representación</span><span class="sxs-lookup"><span data-stu-id="8cef5-118">Render Mode</span></span> | <span data-ttu-id="8cef5-119">Descripción</span><span class="sxs-lookup"><span data-stu-id="8cef5-119">Description</span></span> |
| ----------- | ----------- |
| <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.ServerPrerendered> | <span data-ttu-id="8cef5-120">Representa el componente en código HTML estático e incluye un marcador para una aplicación Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="8cef5-120">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="8cef5-121">Cuando se inicia el agente de usuario, este marcador se usa para arrancar una aplicación Blazor.</span><span class="sxs-lookup"><span data-stu-id="8cef5-121">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Server> | <span data-ttu-id="8cef5-122">Representa un marcador para una aplicación Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="8cef5-122">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="8cef5-123">La salida del componente no está incluida.</span><span class="sxs-lookup"><span data-stu-id="8cef5-123">Output from the component isn't included.</span></span> <span data-ttu-id="8cef5-124">Cuando se inicia el agente de usuario, este marcador se usa para arrancar una aplicación Blazor.</span><span class="sxs-lookup"><span data-stu-id="8cef5-124">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Static> | <span data-ttu-id="8cef5-125">Representa el componente en HTML estático.</span><span class="sxs-lookup"><span data-stu-id="8cef5-125">Renders the component into static HTML.</span></span> |

<span data-ttu-id="8cef5-126">Mientras que las páginas y las vistas pueden utilizar componentes, lo contrario no es cierto.</span><span class="sxs-lookup"><span data-stu-id="8cef5-126">While pages and views can use components, the converse isn't true.</span></span> <span data-ttu-id="8cef5-127">Los componentes no pueden usar características específicas de la página y de la vista, como vistas y secciones parciales.</span><span class="sxs-lookup"><span data-stu-id="8cef5-127">Components can't use view- and page-specific features, such as partial views and sections.</span></span> <span data-ttu-id="8cef5-128">Para usar la lógica de una vista parcial en un componente, se debe factorizar la lógica de vista parcial en un componente.</span><span class="sxs-lookup"><span data-stu-id="8cef5-128">To use logic from a partial view in a component, factor out the partial view logic into a component.</span></span>

<span data-ttu-id="8cef5-129">No se admite la representación de componentes de servidor desde una página HTML estática.</span><span class="sxs-lookup"><span data-stu-id="8cef5-129">Rendering server components from a static HTML page isn't supported.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8cef5-130">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="8cef5-130">Additional resources</span></span>

* <xref:Microsoft.AspNetCore.Mvc.TagHelpers.ComponentTagHelper>
* <xref:mvc/views/tag-helpers/intro>
* <xref:blazor/components>
