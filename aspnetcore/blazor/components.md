---
title: Crear y usar ASP.NET Core componentes de Razor
author: guardrex
description: Obtenga información sobre cómo crear y usar componentes de Razor, incluido cómo enlazar a datos, controlar eventos y administrar ciclos de vida de componentes.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/05/2019
uid: blazor/components
ms.openlocfilehash: cd48111e8d601fc67e8a938fcdd686759a9ddeca
ms.sourcegitcommit: ce2bfb01f2cc7dd83f8a97da0689d232c71bcdc4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2019
ms.locfileid: "72531122"
---
# <a name="create-and-use-aspnet-core-razor-components"></a><span data-ttu-id="cfbec-103">Crear y usar ASP.NET Core componentes de Razor</span><span class="sxs-lookup"><span data-stu-id="cfbec-103">Create and use ASP.NET Core Razor components</span></span>

<span data-ttu-id="cfbec-104">Por [Luke Latham](https://github.com/guardrex) y [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="cfbec-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="cfbec-105">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="cfbec-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="cfbec-106">Las aplicaciones increíbles se compilan con *componentes*.</span><span class="sxs-lookup"><span data-stu-id="cfbec-106">Blazor apps are built using *components*.</span></span> <span data-ttu-id="cfbec-107">Un componente es un fragmento independiente de la interfaz de usuario (IU), como una página, un cuadro de diálogo o un formulario.</span><span class="sxs-lookup"><span data-stu-id="cfbec-107">A component is a self-contained chunk of user interface (UI), such as a page, dialog, or form.</span></span> <span data-ttu-id="cfbec-108">Un componente incluye el marcado HTML y la lógica de procesamiento necesaria para insertar datos o responder a los eventos de la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="cfbec-108">A component includes HTML markup and the processing logic required to inject data or respond to UI events.</span></span> <span data-ttu-id="cfbec-109">Los componentes son flexibles y ligeros.</span><span class="sxs-lookup"><span data-stu-id="cfbec-109">Components are flexible and lightweight.</span></span> <span data-ttu-id="cfbec-110">Se pueden anidar, reutilizar y compartir entre proyectos.</span><span class="sxs-lookup"><span data-stu-id="cfbec-110">They can be nested, reused, and shared among projects.</span></span>

## <a name="component-classes"></a><span data-ttu-id="cfbec-111">Clases de componentes</span><span class="sxs-lookup"><span data-stu-id="cfbec-111">Component classes</span></span>

<span data-ttu-id="cfbec-112">Los componentes se implementan en archivos de componentes de [Razor](xref:mvc/views/razor) ( *. Razor*) C# mediante una combinación de y el marcado HTML.</span><span class="sxs-lookup"><span data-stu-id="cfbec-112">Components are implemented in [Razor](xref:mvc/views/razor) component files (*.razor*) using a combination of C# and HTML markup.</span></span> <span data-ttu-id="cfbec-113">Un componente de extraordinariamente se conoce como *componente de Razor*.</span><span class="sxs-lookup"><span data-stu-id="cfbec-113">A component in Blazor is formally referred to as a *Razor component*.</span></span>

<span data-ttu-id="cfbec-114">El nombre de un componente debe empezar con un carácter en mayúsculas.</span><span class="sxs-lookup"><span data-stu-id="cfbec-114">A component's name must start with an uppercase character.</span></span> <span data-ttu-id="cfbec-115">Por ejemplo, *MyCoolComponent. Razor* es válido y *MyCoolComponent. Razor* no es válido.</span><span class="sxs-lookup"><span data-stu-id="cfbec-115">For example, *MyCoolComponent.razor* is valid, and *myCoolComponent.razor* is invalid.</span></span>

<span data-ttu-id="cfbec-116">La interfaz de usuario de un componente se define mediante HTML.</span><span class="sxs-lookup"><span data-stu-id="cfbec-116">The UI for a component is defined using HTML.</span></span> <span data-ttu-id="cfbec-117">La lógica de la representación dinámica (por ejemplo, bucles, instrucciones condicionales, expresiones) se agrega mediante una sintaxis de C# insertada denominada [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="cfbec-117">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="cfbec-118">Cuando se compila una aplicación, el marcado HTML y C# la lógica de representación se convierten en una clase de componente.</span><span class="sxs-lookup"><span data-stu-id="cfbec-118">When an app is compiled, the HTML markup and C# rendering logic are converted into a component class.</span></span> <span data-ttu-id="cfbec-119">El nombre de la clase generada coincide con el nombre del archivo.</span><span class="sxs-lookup"><span data-stu-id="cfbec-119">The name of the generated class matches the name of the file.</span></span>

<span data-ttu-id="cfbec-120">Los miembros de la clase de componente se definen en un bloque `@code`.</span><span class="sxs-lookup"><span data-stu-id="cfbec-120">Members of the component class are defined in an `@code` block.</span></span> <span data-ttu-id="cfbec-121">En el bloque `@code`, el estado de componente (propiedades, campos) se especifica con métodos para el control de eventos o para definir otra lógica de componentes.</span><span class="sxs-lookup"><span data-stu-id="cfbec-121">In the `@code` block, component state (properties, fields) is specified with methods for event handling or for defining other component logic.</span></span> <span data-ttu-id="cfbec-122">Se permite emplear más de un bloque `@code`.</span><span class="sxs-lookup"><span data-stu-id="cfbec-122">More than one `@code` block is permissible.</span></span>

> [!NOTE]
> <span data-ttu-id="cfbec-123">En las versiones preliminares anteriores de ASP.NET Core 3,0, se usaban bloques `@functions` para el mismo propósito que los bloques `@code` en los componentes de Razor.</span><span class="sxs-lookup"><span data-stu-id="cfbec-123">In prior previews of ASP.NET Core 3.0, `@functions` blocks were used for the same purpose as `@code` blocks in Razor components.</span></span> <span data-ttu-id="cfbec-124">los bloques `@functions` continúan funcionando en los componentes de Razor, pero se recomienda usar el bloque `@code` en ASP.NET Core 3,0 Preview 6 o posterior.</span><span class="sxs-lookup"><span data-stu-id="cfbec-124">`@functions` blocks continue to function in Razor components, but we recommend using the `@code` block in ASP.NET Core 3.0 Preview 6 or later.</span></span>

<span data-ttu-id="cfbec-125">Los miembros de componente se pueden usar como parte de la lógica de representación del C# componente mediante expresiones que comienzan por `@`.</span><span class="sxs-lookup"><span data-stu-id="cfbec-125">Component members can be used as part of the component's rendering logic using C# expressions that start with `@`.</span></span> <span data-ttu-id="cfbec-126">Por ejemplo, un C# campo se representa mediante el prefijo `@` al nombre del campo.</span><span class="sxs-lookup"><span data-stu-id="cfbec-126">For example, a C# field is rendered by prefixing `@` to the field name.</span></span> <span data-ttu-id="cfbec-127">En el siguiente ejemplo se evalúa y representa:</span><span class="sxs-lookup"><span data-stu-id="cfbec-127">The following example evaluates and renders:</span></span>

* <span data-ttu-id="cfbec-128">`_headingFontStyle` al valor de la propiedad CSS para `font-style`.</span><span class="sxs-lookup"><span data-stu-id="cfbec-128">`_headingFontStyle` to the CSS property value for `font-style`.</span></span>
* <span data-ttu-id="cfbec-129">`_headingText` al contenido del elemento `<h1>`.</span><span class="sxs-lookup"><span data-stu-id="cfbec-129">`_headingText` to the content of the `<h1>` element.</span></span>

```cshtml
<h1 style="font-style:@_headingFontStyle">@_headingText</h1>

@code {
    private string _headingFontStyle = "italic";
    private string _headingText = "Put on your new Blazor!";
}
```

<span data-ttu-id="cfbec-130">Una vez que el componente se representa inicialmente, el componente regenera su árbol de representación en respuesta a los eventos.</span><span class="sxs-lookup"><span data-stu-id="cfbec-130">After the component is initially rendered, the component regenerates its render tree in response to events.</span></span> <span data-ttu-id="cfbec-131">Después compara el nuevo árbol de representación con el anterior y aplica las modificaciones realizadas en el Document Object Model del explorador (DOM).</span><span class="sxs-lookup"><span data-stu-id="cfbec-131">Blazor then compares the new render tree against the previous one and applies any modifications to the browser's Document Object Model (DOM).</span></span>

<span data-ttu-id="cfbec-132">Los componentes son C# clases normales y se pueden colocar en cualquier parte dentro de un proyecto.</span><span class="sxs-lookup"><span data-stu-id="cfbec-132">Components are ordinary C# classes and can be placed anywhere within a project.</span></span> <span data-ttu-id="cfbec-133">Los componentes que generan páginas web normalmente residen en la carpeta *pages* .</span><span class="sxs-lookup"><span data-stu-id="cfbec-133">Components that produce webpages usually reside in the *Pages* folder.</span></span> <span data-ttu-id="cfbec-134">Los componentes que no son de página se colocan con frecuencia en la carpeta *compartida* o en una carpeta personalizada agregada al proyecto.</span><span class="sxs-lookup"><span data-stu-id="cfbec-134">Non-page components are frequently placed in the *Shared* folder or a custom folder added to the project.</span></span> <span data-ttu-id="cfbec-135">Para usar una carpeta personalizada, agregue el espacio de nombres de la carpeta personalizada al componente primario o al archivo *_Imports. Razor* de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="cfbec-135">To use a custom folder, add the custom folder's namespace to either the parent component or to the app's *_Imports.razor* file.</span></span> <span data-ttu-id="cfbec-136">Por ejemplo, el siguiente espacio de nombres hace que los componentes de una carpeta *Components* estén disponibles cuando el espacio de nombres raíz de la aplicación es `WebApplication`:</span><span class="sxs-lookup"><span data-stu-id="cfbec-136">For example, the following namespace makes components in a *Components* folder available when the app's root namespace is `WebApplication`:</span></span>

```cshtml
@using WebApplication.Components
```

## <a name="integrate-components-into-razor-pages-and-mvc-apps"></a><span data-ttu-id="cfbec-137">Integración de componentes en aplicaciones de Razor Pages y MVC</span><span class="sxs-lookup"><span data-stu-id="cfbec-137">Integrate components into Razor Pages and MVC apps</span></span>

<span data-ttu-id="cfbec-138">Usar componentes con las aplicaciones de Razor Pages y MVC existentes.</span><span class="sxs-lookup"><span data-stu-id="cfbec-138">Use components with existing Razor Pages and MVC apps.</span></span> <span data-ttu-id="cfbec-139">No es necesario volver a escribir las páginas o vistas existentes para utilizar los componentes de Razor.</span><span class="sxs-lookup"><span data-stu-id="cfbec-139">There's no need to rewrite existing pages or views to use Razor components.</span></span> <span data-ttu-id="cfbec-140">Cuando se representa la página o la vista, los componentes se preprocesan al mismo tiempo.</span><span class="sxs-lookup"><span data-stu-id="cfbec-140">When the page or view is rendered, components are prerendered at the same time.</span></span>

<span data-ttu-id="cfbec-141">Para representar un componente de una página o vista, use el método auxiliar HTML `RenderComponentAsync<TComponent>`:</span><span class="sxs-lookup"><span data-stu-id="cfbec-141">To render a component from a page or view, use the `RenderComponentAsync<TComponent>` HTML helper method:</span></span>

```cshtml
<div id="MyComponent">
    @(await Html.RenderComponentAsync<MyComponent>(RenderMode.ServerPrerendered))
</div>
```

<span data-ttu-id="cfbec-142">Mientras que las páginas y las vistas pueden utilizar componentes, el opuesto no es cierto.</span><span class="sxs-lookup"><span data-stu-id="cfbec-142">While pages and views can use components, the converse isn't true.</span></span> <span data-ttu-id="cfbec-143">Los componentes no pueden usar escenarios específicos de la página y de la vista, como vistas y secciones parciales.</span><span class="sxs-lookup"><span data-stu-id="cfbec-143">Components can't use view- and page-specific scenarios, such as partial views and sections.</span></span> <span data-ttu-id="cfbec-144">Para usar la lógica de la vista parcial en un componente, se debe factorizar la lógica de vista parcial en un componente.</span><span class="sxs-lookup"><span data-stu-id="cfbec-144">To use logic from partial view in a component, factor out the partial view logic into a component.</span></span>

<span data-ttu-id="cfbec-145">Para obtener más información sobre cómo se representan los componentes y el estado del componente se administra en las aplicaciones de servidor increíbles, consulte el artículo <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="cfbec-145">For more information on how components are rendered and component state is managed in Blazor Server apps, see the <xref:blazor/hosting-models> article.</span></span>

## <a name="use-components"></a><span data-ttu-id="cfbec-146">Uso de componentes</span><span class="sxs-lookup"><span data-stu-id="cfbec-146">Use components</span></span>

<span data-ttu-id="cfbec-147">Los componentes pueden incluir otros componentes declarándolos mediante la sintaxis de elementos HTML.</span><span class="sxs-lookup"><span data-stu-id="cfbec-147">Components can include other components by declaring them using HTML element syntax.</span></span> <span data-ttu-id="cfbec-148">El marcado para utilizar un componente se parece a una etiqueta HTML en la que el nombre de la etiqueta es el tipo de componente.</span><span class="sxs-lookup"><span data-stu-id="cfbec-148">The markup for using a component looks like an HTML tag where the name of the tag is the component type.</span></span>

<span data-ttu-id="cfbec-149">El enlace de atributo distingue mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="cfbec-149">Attribute binding is case sensitive.</span></span> <span data-ttu-id="cfbec-150">Por ejemplo, `@bind` es válido y `@Bind` no es válido.</span><span class="sxs-lookup"><span data-stu-id="cfbec-150">For example, `@bind` is valid, and `@Bind` is invalid.</span></span>

<span data-ttu-id="cfbec-151">El marcado siguiente en *index. Razor* representa una instancia de `HeadingComponent`:</span><span class="sxs-lookup"><span data-stu-id="cfbec-151">The following markup in *Index.razor* renders a `HeadingComponent` instance:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Pages/Index.razor?name=snippet_HeadingComponent)]

<span data-ttu-id="cfbec-152">*Components/HeadingComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="cfbec-152">*Components/HeadingComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Components/HeadingComponent.razor)]

<span data-ttu-id="cfbec-153">Si un componente contiene un elemento HTML con una primera letra mayúscula que no coincide con un nombre de componente, se emite una advertencia que indica que el elemento tiene un nombre inesperado.</span><span class="sxs-lookup"><span data-stu-id="cfbec-153">If a component contains an HTML element with an uppercase first letter that doesn't match a component name, a warning is emitted indicating that the element has an unexpected name.</span></span> <span data-ttu-id="cfbec-154">Agregar una instrucción `@using` para el espacio de nombres del componente hace que el componente esté disponible, lo que elimina la advertencia.</span><span class="sxs-lookup"><span data-stu-id="cfbec-154">Adding an `@using` statement for the component's namespace makes the component available, which removes the warning.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="cfbec-155">Parámetros del componente</span><span class="sxs-lookup"><span data-stu-id="cfbec-155">Component parameters</span></span>

<span data-ttu-id="cfbec-156">Los componentes pueden tener *parámetros de componente*, que se definen mediante propiedades públicas en la clase de componente con el atributo `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="cfbec-156">Components can have *component parameters*, which are defined using public properties on the component class with the `[Parameter]` attribute.</span></span> <span data-ttu-id="cfbec-157">Use atributos para especificar argumentos para un componente en el marcado.</span><span class="sxs-lookup"><span data-stu-id="cfbec-157">Use attributes to specify arguments for a component in markup.</span></span>

<span data-ttu-id="cfbec-158">*Components/ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="cfbec-158">*Components/ChildComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=11-12)]

<span data-ttu-id="cfbec-159">En el ejemplo siguiente, el `ParentComponent` establece el valor de la propiedad `Title` del `ChildComponent`.</span><span class="sxs-lookup"><span data-stu-id="cfbec-159">In the following example, the `ParentComponent` sets the value of the `Title` property of the `ChildComponent`.</span></span>

<span data-ttu-id="cfbec-160">*Pages/ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="cfbec-160">*Pages/ParentComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=5-6)]

## <a name="child-content"></a><span data-ttu-id="cfbec-161">Contenido secundario</span><span class="sxs-lookup"><span data-stu-id="cfbec-161">Child content</span></span>

<span data-ttu-id="cfbec-162">Los componentes pueden establecer el contenido de otro componente.</span><span class="sxs-lookup"><span data-stu-id="cfbec-162">Components can set the content of another component.</span></span> <span data-ttu-id="cfbec-163">El componente de asignación proporciona el contenido entre las etiquetas que especifican el componente receptor.</span><span class="sxs-lookup"><span data-stu-id="cfbec-163">The assigning component provides the content between the tags that specify the receiving component.</span></span>

<span data-ttu-id="cfbec-164">En el ejemplo siguiente, el `ChildComponent` tiene una propiedad `ChildContent` que representa un @no__t 2, que representa un segmento de la interfaz de usuario que se va a representar.</span><span class="sxs-lookup"><span data-stu-id="cfbec-164">In the following example, the `ChildComponent` has a `ChildContent` property that represents a `RenderFragment`, which represents a segment of UI to render.</span></span> <span data-ttu-id="cfbec-165">El valor de `ChildContent` se coloca en el marcado del componente en el que se debe representar el contenido.</span><span class="sxs-lookup"><span data-stu-id="cfbec-165">The value of `ChildContent` is positioned in the component's markup where the content should be rendered.</span></span> <span data-ttu-id="cfbec-166">El valor de `ChildContent` se recibe del componente primario y se representa dentro del `panel-body` del panel de arranque.</span><span class="sxs-lookup"><span data-stu-id="cfbec-166">The value of `ChildContent` is received from the parent component and rendered inside the Bootstrap panel's `panel-body`.</span></span>

<span data-ttu-id="cfbec-167">*Components/ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="cfbec-167">*Components/ChildComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=3,14-15)]

> [!NOTE]
> <span data-ttu-id="cfbec-168">La propiedad que recibe el contenido de `RenderFragment` debe denominarse `ChildContent` por Convención.</span><span class="sxs-lookup"><span data-stu-id="cfbec-168">The property receiving the `RenderFragment` content must be named `ChildContent` by convention.</span></span>

<span data-ttu-id="cfbec-169">El siguiente `ParentComponent` puede proporcionar contenido para representar el `ChildComponent` colocando el contenido dentro de las etiquetas `<ChildComponent>`.</span><span class="sxs-lookup"><span data-stu-id="cfbec-169">The following `ParentComponent` can provide content for rendering the `ChildComponent` by placing the content inside the `<ChildComponent>` tags.</span></span>

<span data-ttu-id="cfbec-170">*Pages/ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="cfbec-170">*Pages/ParentComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=7-8)]

## <a name="attribute-splatting-and-arbitrary-parameters"></a><span data-ttu-id="cfbec-171">Atributos expansión y parámetros arbitrarios</span><span class="sxs-lookup"><span data-stu-id="cfbec-171">Attribute splatting and arbitrary parameters</span></span>

<span data-ttu-id="cfbec-172">Los componentes de pueden capturar y representar atributos adicionales además de los parámetros declarados del componente.</span><span class="sxs-lookup"><span data-stu-id="cfbec-172">Components can capture and render additional attributes in addition to the component's declared parameters.</span></span> <span data-ttu-id="cfbec-173">Los atributos adicionales se pueden capturar en un diccionario y, después, *splatted* en un elemento cuando el componente se representa mediante la directiva [de Razor @attributes](xref:mvc/views/razor#attributes) .</span><span class="sxs-lookup"><span data-stu-id="cfbec-173">Additional attributes can be captured in a dictionary and then *splatted* onto an element when the component is rendered using the [@attributes](xref:mvc/views/razor#attributes) Razor directive.</span></span> <span data-ttu-id="cfbec-174">Este escenario es útil cuando se define un componente que genera un elemento de marcado que admite diversas personalizaciones.</span><span class="sxs-lookup"><span data-stu-id="cfbec-174">This scenario is useful when defining a component that produces a markup element that supports a variety of customizations.</span></span> <span data-ttu-id="cfbec-175">Por ejemplo, puede resultar tedioso definir atributos por separado para un `<input>` que admita muchos parámetros.</span><span class="sxs-lookup"><span data-stu-id="cfbec-175">For example, it can be tedious to define attributes separately for an `<input>` that supports many parameters.</span></span>

<span data-ttu-id="cfbec-176">En el ejemplo siguiente, el primer elemento `<input>` (`id="useIndividualParams"`) usa parámetros de componente individuales, mientras que el segundo elemento @no__t 2 (`id="useAttributesDict"`) utiliza el atributo expansión:</span><span class="sxs-lookup"><span data-stu-id="cfbec-176">In the following example, the first `<input>` element (`id="useIndividualParams"`) uses individual component parameters, while the second `<input>` element (`id="useAttributesDict"`) uses attribute splatting:</span></span>

```cshtml
<input id="useIndividualParams"
       maxlength="@Maxlength"
       placeholder="@Placeholder"
       required="@Required"
       size="@Size" />

<input id="useAttributesDict"
       @attributes="InputAttributes" />

@code {
    [Parameter]
    public string Maxlength { get; set; } = "10";

    [Parameter]
    public string Placeholder { get; set; } = "Input placeholder text";

    [Parameter]
    public string Required { get; set; } = "required";

    [Parameter]
    public string Size { get; set; } = "50";

    [Parameter]
    public Dictionary<string, object> InputAttributes { get; set; } =
        new Dictionary<string, object>()
        {
            { "maxlength", "10" },
            { "placeholder", "Input placeholder text" },
            { "required", "required" },
            { "size", "50" }
        };
}
```

<span data-ttu-id="cfbec-177">El tipo del parámetro debe implementar `IEnumerable<KeyValuePair<string, object>>` con claves de cadena.</span><span class="sxs-lookup"><span data-stu-id="cfbec-177">The type of the parameter must implement `IEnumerable<KeyValuePair<string, object>>` with string keys.</span></span> <span data-ttu-id="cfbec-178">El uso de `IReadOnlyDictionary<string, object>` también es una opción en este escenario.</span><span class="sxs-lookup"><span data-stu-id="cfbec-178">Using `IReadOnlyDictionary<string, object>` is also an option in this scenario.</span></span>

<span data-ttu-id="cfbec-179">Los elementos `<input>` representados con ambos enfoques son idénticos:</span><span class="sxs-lookup"><span data-stu-id="cfbec-179">The rendered `<input>` elements using both approaches is identical:</span></span>

```html
<input id="useIndividualParams"
       maxlength="10"
       placeholder="Input placeholder text"
       required="required"
       size="50">

<input id="useAttributesDict"
       maxlength="10"
       placeholder="Input placeholder text"
       required="required"
       size="50">
```

<span data-ttu-id="cfbec-180">Para aceptar atributos arbitrarios, defina un parámetro de componente mediante el atributo `[Parameter]` con la propiedad `CaptureUnmatchedValues` establecida en `true`:</span><span class="sxs-lookup"><span data-stu-id="cfbec-180">To accept arbitrary attributes, define a component parameter using the `[Parameter]` attribute with the `CaptureUnmatchedValues` property set to `true`:</span></span>

```cshtml
@code {
    [Parameter(CaptureUnmatchedValues = true)]
    public Dictionary<string, object> InputAttributes { get; set; }
}
```

<span data-ttu-id="cfbec-181">La propiedad `CaptureUnmatchedValues` en `[Parameter]` permite que el parámetro coincida con todos los atributos que no coinciden con ningún otro parámetro.</span><span class="sxs-lookup"><span data-stu-id="cfbec-181">The `CaptureUnmatchedValues` property on `[Parameter]` allows the parameter to match all attributes that don't match any other parameter.</span></span> <span data-ttu-id="cfbec-182">Un componente solo puede definir un parámetro con `CaptureUnmatchedValues`.</span><span class="sxs-lookup"><span data-stu-id="cfbec-182">A component can only define a single parameter with `CaptureUnmatchedValues`.</span></span> <span data-ttu-id="cfbec-183">El tipo de propiedad que se usa con `CaptureUnmatchedValues` debe poder asignarse desde `Dictionary<string, object>` con claves de cadena.</span><span class="sxs-lookup"><span data-stu-id="cfbec-183">The property type used with `CaptureUnmatchedValues` must be assignable from `Dictionary<string, object>` with string keys.</span></span> <span data-ttu-id="cfbec-184">`IEnumerable<KeyValuePair<string, object>>` o `IReadOnlyDictionary<string, object>` también son opciones en este escenario.</span><span class="sxs-lookup"><span data-stu-id="cfbec-184">`IEnumerable<KeyValuePair<string, object>>` or `IReadOnlyDictionary<string, object>` are also options in this scenario.</span></span>

## <a name="data-binding"></a><span data-ttu-id="cfbec-185">Enlace de datos</span><span class="sxs-lookup"><span data-stu-id="cfbec-185">Data binding</span></span>

<span data-ttu-id="cfbec-186">El enlace de datos tanto a componentes como a elementos DOM se realiza con el atributo [@bind](xref:mvc/views/razor#bind) .</span><span class="sxs-lookup"><span data-stu-id="cfbec-186">Data binding to both components and DOM elements is accomplished with the [@bind](xref:mvc/views/razor#bind) attribute.</span></span> <span data-ttu-id="cfbec-187">En el ejemplo siguiente se enlaza una propiedad `CurrentValue` al valor del cuadro de texto:</span><span class="sxs-lookup"><span data-stu-id="cfbec-187">The following example binds a `CurrentValue` property to the text box's value:</span></span>

```cshtml
<input @bind="CurrentValue" />

@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="cfbec-188">Cuando el cuadro de texto pierde el foco, se actualiza el valor de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="cfbec-188">When the text box loses focus, the property's value is updated.</span></span>

<span data-ttu-id="cfbec-189">El cuadro de texto se actualiza en la interfaz de usuario solo cuando se representa el componente, no en respuesta a cambiar el valor de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="cfbec-189">The text box is updated in the UI only when the component is rendered, not in response to changing the property's value.</span></span> <span data-ttu-id="cfbec-190">Puesto que los componentes se representan por sí solos después de que se ejecute el código del controlador de eventos, las actualizaciones de propiedades *normalmente* se reflejan en la interfaz de usuario inmediatamente después de desencadenarse un controlador de eventos.</span><span class="sxs-lookup"><span data-stu-id="cfbec-190">Since components render themselves after event handler code executes, property updates are *usually* reflected in the UI immediately after an event handler is triggered.</span></span>

<span data-ttu-id="cfbec-191">El uso de `@bind` con la propiedad `CurrentValue` (`<input @bind="CurrentValue" />`) es esencialmente equivalente a lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="cfbec-191">Using `@bind` with the `CurrentValue` property (`<input @bind="CurrentValue" />`) is essentially equivalent to the following:</span></span>

```cshtml
<input value="@CurrentValue"
    @onchange="@((ChangeEventArgs __e) => CurrentValue = 
        __e.Value.ToString())" />
        
@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="cfbec-192">Cuando se representa el componente, el `value` del elemento de entrada procede de la propiedad `CurrentValue`.</span><span class="sxs-lookup"><span data-stu-id="cfbec-192">When the component is rendered, the `value` of the input element comes from the `CurrentValue` property.</span></span> <span data-ttu-id="cfbec-193">Cuando el usuario escribe en el cuadro de texto y cambia el foco del elemento, se desencadena el evento `onchange` y la propiedad `CurrentValue` se establece en el valor modificado.</span><span class="sxs-lookup"><span data-stu-id="cfbec-193">When the user types in the text box and changes element focus, the `onchange` event is fired and the `CurrentValue` property is set to the changed value.</span></span> <span data-ttu-id="cfbec-194">En realidad, la generación de código es más compleja porque `@bind` controla los casos en los que se realizan las conversiones de tipos.</span><span class="sxs-lookup"><span data-stu-id="cfbec-194">In reality, the code generation is more complex because `@bind` handles cases where type conversions are performed.</span></span> <span data-ttu-id="cfbec-195">En principio, `@bind` asocia el valor actual de una expresión con un atributo `value` y controla los cambios mediante el controlador registrado.</span><span class="sxs-lookup"><span data-stu-id="cfbec-195">In principle, `@bind` associates the current value of an expression with a `value` attribute and handles changes using the registered handler.</span></span>

<span data-ttu-id="cfbec-196">Además de controlar los eventos `onchange` con la sintaxis `@bind`, se puede enlazar una propiedad o un campo mediante otros eventos mediante la especificación de un atributo [@bind-value](xref:mvc/views/razor#bind) con un parámetro `event` ([@bind-value:event](xref:mvc/views/razor#bind)).</span><span class="sxs-lookup"><span data-stu-id="cfbec-196">In addition to handling `onchange` events with `@bind` syntax, a property or field can be bound using other events by specifying an [@bind-value](xref:mvc/views/razor#bind) attribute with an `event` parameter ([@bind-value:event](xref:mvc/views/razor#bind)).</span></span> <span data-ttu-id="cfbec-197">En el ejemplo siguiente se enlaza la propiedad `CurrentValue` para el evento `oninput`:</span><span class="sxs-lookup"><span data-stu-id="cfbec-197">The following example binds the `CurrentValue` property for the `oninput` event:</span></span>

```cshtml
<input @bind-value="CurrentValue" @bind-value:event="oninput" />

@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="cfbec-198">A diferencia de `onchange`, que se activa cuando el elemento pierde el foco, `oninput` se activa cuando cambia el valor del cuadro de texto.</span><span class="sxs-lookup"><span data-stu-id="cfbec-198">Unlike `onchange`, which fires when the element loses focus, `oninput` fires when the value of the text box changes.</span></span>

<span data-ttu-id="cfbec-199">**Valores no analizables**</span><span class="sxs-lookup"><span data-stu-id="cfbec-199">**Unparsable values**</span></span>

<span data-ttu-id="cfbec-200">Cuando un usuario proporciona un valor que no se pueda analizar a un elemento DataBound, el valor no analizable se revierte automáticamente a su valor anterior cuando se desencadena el evento de enlace.</span><span class="sxs-lookup"><span data-stu-id="cfbec-200">When a user provides an unparsable value to a databound element, the unparsable value is automatically reverted to its previous value when the bind event is triggered.</span></span>

<span data-ttu-id="cfbec-201">Considere el caso siguiente:</span><span class="sxs-lookup"><span data-stu-id="cfbec-201">Consider the following scenario:</span></span>

* <span data-ttu-id="cfbec-202">Un elemento `<input>` se enlaza a un tipo `int` con un valor inicial de `123`:</span><span class="sxs-lookup"><span data-stu-id="cfbec-202">An `<input>` element is bound to an `int` type with an initial value of `123`:</span></span>

  ```cshtml
  <input @bind="MyProperty" />

  @code {
      [Parameter]
      public int MyProperty { get; set; } = 123;
  }
  ```
* <span data-ttu-id="cfbec-203">El usuario actualiza el valor del elemento a `123.45` en la página y cambia el foco del elemento.</span><span class="sxs-lookup"><span data-stu-id="cfbec-203">The user updates the value of the element to `123.45` in the page and changes the element focus.</span></span>

<span data-ttu-id="cfbec-204">En el escenario anterior, el valor del elemento se revierte a `123`.</span><span class="sxs-lookup"><span data-stu-id="cfbec-204">In the preceding scenario, the element's value is reverted to `123`.</span></span> <span data-ttu-id="cfbec-205">Cuando el valor `123.45` se rechaza en favor del valor original de `123`, el usuario entiende que no se aceptó su valor.</span><span class="sxs-lookup"><span data-stu-id="cfbec-205">When the value `123.45` is rejected in favor of the original value of `123`, the user understands that their value wasn't accepted.</span></span>

<span data-ttu-id="cfbec-206">De forma predeterminada, el enlace se aplica al evento `onchange` del elemento (`@bind="{PROPERTY OR FIELD}"`).</span><span class="sxs-lookup"><span data-stu-id="cfbec-206">By default, binding applies to the element's `onchange` event (`@bind="{PROPERTY OR FIELD}"`).</span></span> <span data-ttu-id="cfbec-207">Use `@bind-value="{PROPERTY OR FIELD}" @bind-value:event={EVENT}` para establecer un evento diferente.</span><span class="sxs-lookup"><span data-stu-id="cfbec-207">Use `@bind-value="{PROPERTY OR FIELD}" @bind-value:event={EVENT}` to set a different event.</span></span> <span data-ttu-id="cfbec-208">En el caso del evento `oninput` (`@bind-value:event="oninput"`), la reversión se produce después de cualquier pulsación de tecla que introduce un valor que no se pudo analizar.</span><span class="sxs-lookup"><span data-stu-id="cfbec-208">For the `oninput` event (`@bind-value:event="oninput"`), the reversion occurs after any keystroke that introduces an unparsable value.</span></span> <span data-ttu-id="cfbec-209">Cuando el destino es el evento `oninput` con un tipo enlazado @no__t -1, se impide que un usuario escriba un carácter `.`.</span><span class="sxs-lookup"><span data-stu-id="cfbec-209">When targeting the `oninput` event with an `int`-bound type, a user is prevented from typing a `.` character.</span></span> <span data-ttu-id="cfbec-210">Se quita inmediatamente un carácter @no__t 0, por lo que el usuario recibe comentarios inmediatos que solo se permiten números enteros.</span><span class="sxs-lookup"><span data-stu-id="cfbec-210">A `.` character is immediately removed, so the user receives immediate feedback that only whole numbers are permitted.</span></span> <span data-ttu-id="cfbec-211">Hay escenarios en los que la reversión del valor del evento `oninput` no es ideal, por ejemplo, cuando se debe permitir que el usuario borre un valor `<input>` no analizable.</span><span class="sxs-lookup"><span data-stu-id="cfbec-211">There are scenarios where reverting the value on the `oninput` event isn't ideal, such as when the user should be allowed to clear an unparsable `<input>` value.</span></span> <span data-ttu-id="cfbec-212">Las alternativas incluyen:</span><span class="sxs-lookup"><span data-stu-id="cfbec-212">Alternatives include:</span></span>

* <span data-ttu-id="cfbec-213">No use el evento `oninput`.</span><span class="sxs-lookup"><span data-stu-id="cfbec-213">Don't use the `oninput` event.</span></span> <span data-ttu-id="cfbec-214">Use el evento `onchange` predeterminado (`@bind="{PROPERTY OR FIELD}"`), donde no se revierte un valor no válido hasta que el elemento pierde el foco.</span><span class="sxs-lookup"><span data-stu-id="cfbec-214">Use the default `onchange` event (`@bind="{PROPERTY OR FIELD}"`), where an invalid value isn't reverted until the element loses focus.</span></span>
* <span data-ttu-id="cfbec-215">Enlazar a un tipo que acepta valores NULL, como `int?` o `string`, y proporcionar una lógica personalizada para controlar las entradas no válidas.</span><span class="sxs-lookup"><span data-stu-id="cfbec-215">Bind to a nullable type, such as `int?` or `string`, and provide custom logic to handle invalid entries.</span></span>
* <span data-ttu-id="cfbec-216">Use un [componente de validación de formulario](xref:blazor/forms-validation), como `InputNumber` o `InputDate`.</span><span class="sxs-lookup"><span data-stu-id="cfbec-216">Use a [form validation component](xref:blazor/forms-validation), such as `InputNumber` or `InputDate`.</span></span> <span data-ttu-id="cfbec-217">Los componentes de validación de formularios tienen compatibilidad integrada para administrar entradas no válidas.</span><span class="sxs-lookup"><span data-stu-id="cfbec-217">Form validation components have built-in support to manage invalid inputs.</span></span> <span data-ttu-id="cfbec-218">Componentes de validación de formularios:</span><span class="sxs-lookup"><span data-stu-id="cfbec-218">Form validation components:</span></span>
  * <span data-ttu-id="cfbec-219">Permite que el usuario proporcione entradas no válidas y reciba errores de validación en el @no__t asociado-0.</span><span class="sxs-lookup"><span data-stu-id="cfbec-219">Permit the user to provide invalid input and receive validation errors on the associated `EditContext`.</span></span>
  * <span data-ttu-id="cfbec-220">Mostrar errores de validación en la interfaz de usuario sin interferir con el usuario al escribir datos de WebForm adicionales.</span><span class="sxs-lookup"><span data-stu-id="cfbec-220">Display validation errors in the UI without interfering with the user entering additional webform data.</span></span>

<span data-ttu-id="cfbec-221">**Globalización**</span><span class="sxs-lookup"><span data-stu-id="cfbec-221">**Globalization**</span></span>

<span data-ttu-id="cfbec-222">se da formato a los valores de `@bind` para mostrarlos y analizarlos con las reglas de la referencia cultural actual.</span><span class="sxs-lookup"><span data-stu-id="cfbec-222">`@bind` values are formatted for display and parsed using the current culture's rules.</span></span>

<span data-ttu-id="cfbec-223">Se puede tener acceso a la referencia cultural actual desde la propiedad <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="cfbec-223">The current culture can be accessed from the <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName> property.</span></span>

<span data-ttu-id="cfbec-224">[CultureInfo. InvariantCulture](xref:System.Globalization.CultureInfo.InvariantCulture) se usa para los siguientes tipos de campo (`<input type="{TYPE}" />`):</span><span class="sxs-lookup"><span data-stu-id="cfbec-224">[CultureInfo.InvariantCulture](xref:System.Globalization.CultureInfo.InvariantCulture) is used for the following field types (`<input type="{TYPE}" />`):</span></span>

* `date`
* `number`

<span data-ttu-id="cfbec-225">Los tipos de campo anteriores:</span><span class="sxs-lookup"><span data-stu-id="cfbec-225">The preceding field types:</span></span>

* <span data-ttu-id="cfbec-226">Se muestran con sus reglas de formato basadas en explorador adecuadas.</span><span class="sxs-lookup"><span data-stu-id="cfbec-226">Are displayed using their appropriate browser-based formatting rules.</span></span>
* <span data-ttu-id="cfbec-227">No puede contener texto de forma libre.</span><span class="sxs-lookup"><span data-stu-id="cfbec-227">Can't contain free-form text.</span></span>
* <span data-ttu-id="cfbec-228">Proporcionar características de interacción con el usuario en función de la implementación del explorador.</span><span class="sxs-lookup"><span data-stu-id="cfbec-228">Provide user interaction characteristics based on the browser's implementation.</span></span>

<span data-ttu-id="cfbec-229">Los siguientes tipos de campo tienen requisitos de formato específicos y no se admiten en estos momentos porque no son compatibles con todos los exploradores principales:</span><span class="sxs-lookup"><span data-stu-id="cfbec-229">The following field types have specific formatting requirements and aren't currently supported by Blazor because they aren't supported by all major browsers:</span></span>

* `datetime-local`
* `month`
* `week`

<span data-ttu-id="cfbec-230">`@bind` admite el parámetro `@bind:culture` para proporcionar un @no__t 2 para analizar y dar formato a un valor.</span><span class="sxs-lookup"><span data-stu-id="cfbec-230">`@bind` supports the `@bind:culture` parameter to provide a <xref:System.Globalization.CultureInfo?displayProperty=fullName> for parsing and formatting a value.</span></span> <span data-ttu-id="cfbec-231">No se recomienda especificar una referencia cultural al usar los tipos de campo `date` y `number`.</span><span class="sxs-lookup"><span data-stu-id="cfbec-231">Specifying a culture isn't recommended when using the `date` and `number` field types.</span></span> <span data-ttu-id="cfbec-232">`date` y `number` tienen compatibilidad increíblemente integrada que proporciona la referencia cultural necesaria.</span><span class="sxs-lookup"><span data-stu-id="cfbec-232">`date` and `number` have built-in Blazor support that provides the required culture.</span></span>

<span data-ttu-id="cfbec-233">Para obtener información sobre cómo establecer la referencia cultural del usuario, consulte la sección [Localization](#localization) .</span><span class="sxs-lookup"><span data-stu-id="cfbec-233">For information on how to set the user's culture, see the [Localization](#localization) section.</span></span>

<span data-ttu-id="cfbec-234">**Cadenas de formato**</span><span class="sxs-lookup"><span data-stu-id="cfbec-234">**Format strings**</span></span>

<span data-ttu-id="cfbec-235">El enlace de datos funciona con cadenas de formato <xref:System.DateTime> mediante [@bind:format](xref:mvc/views/razor#bind).</span><span class="sxs-lookup"><span data-stu-id="cfbec-235">Data binding works with <xref:System.DateTime> format strings using [@bind:format](xref:mvc/views/razor#bind).</span></span> <span data-ttu-id="cfbec-236">Otras expresiones de formato, como los formatos de moneda o número, no están disponibles en este momento.</span><span class="sxs-lookup"><span data-stu-id="cfbec-236">Other format expressions, such as currency or number formats, aren't available at this time.</span></span>

```cshtml
<input @bind="StartDate" @bind:format="yyyy-MM-dd" />

@code {
    [Parameter]
    public DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

<span data-ttu-id="cfbec-237">En el código anterior, el tipo de campo del elemento `<input>` (`type`) tiene como valor predeterminado `text`.</span><span class="sxs-lookup"><span data-stu-id="cfbec-237">In the preceding code, the `<input>` element's field type (`type`) defaults to `text`.</span></span> <span data-ttu-id="cfbec-238">`@bind:format` es compatible con el enlace de los siguientes tipos de .NET:</span><span class="sxs-lookup"><span data-stu-id="cfbec-238">`@bind:format` is supported for binding the following .NET types:</span></span>

* <xref:System.DateTime?displayProperty=fullName>
* <span data-ttu-id="cfbec-239"><xref:System.DateTime?displayProperty=fullName>?</span><span class="sxs-lookup"><span data-stu-id="cfbec-239"><xref:System.DateTime?displayProperty=fullName>?</span></span>
* <xref:System.DateTimeOffset?displayProperty=fullName>
* <span data-ttu-id="cfbec-240"><xref:System.DateTimeOffset?displayProperty=fullName>?</span><span class="sxs-lookup"><span data-stu-id="cfbec-240"><xref:System.DateTimeOffset?displayProperty=fullName>?</span></span>

<span data-ttu-id="cfbec-241">El atributo `@bind:format` especifica el formato de fecha que se va a aplicar al `value` del elemento `<input>`.</span><span class="sxs-lookup"><span data-stu-id="cfbec-241">The `@bind:format` attribute specifies the date format to apply to the `value` of the `<input>` element.</span></span> <span data-ttu-id="cfbec-242">El formato también se usa para analizar el valor cuando se produce un evento `onchange`.</span><span class="sxs-lookup"><span data-stu-id="cfbec-242">The format is also used to parse the value when an `onchange` event occurs.</span></span>

<span data-ttu-id="cfbec-243">No se recomienda especificar un formato para el tipo de campo `date` porque increíbles tiene compatibilidad integrada para dar formato a las fechas.</span><span class="sxs-lookup"><span data-stu-id="cfbec-243">Specifying a format for the `date` field type isn't recommended because Blazor has built-in support to format dates.</span></span>

<span data-ttu-id="cfbec-244">**Parámetros de componente**</span><span class="sxs-lookup"><span data-stu-id="cfbec-244">**Component parameters**</span></span>

<span data-ttu-id="cfbec-245">El enlace reconoce los parámetros de componente, donde `@bind-{property}` puede enlazar un valor de propiedad entre los componentes.</span><span class="sxs-lookup"><span data-stu-id="cfbec-245">Binding recognizes component parameters, where `@bind-{property}` can bind a property value across components.</span></span>

<span data-ttu-id="cfbec-246">El siguiente componente secundario (`ChildComponent`) tiene un parámetro de componente `Year` y una devolución de llamada `YearChanged`:</span><span class="sxs-lookup"><span data-stu-id="cfbec-246">The following child component (`ChildComponent`) has a `Year` component parameter and `YearChanged` callback:</span></span>

```cshtml
<h2>Child Component</h2>

<p>Year: @Year</p>

@code {
    [Parameter]
    public int Year { get; set; }

    [Parameter]
    public EventCallback<int> YearChanged { get; set; }
}
```

<span data-ttu-id="cfbec-247">`EventCallback<T>` se explica en la sección [EventCallback](#eventcallback) .</span><span class="sxs-lookup"><span data-stu-id="cfbec-247">`EventCallback<T>` is explained in the [EventCallback](#eventcallback) section.</span></span>

<span data-ttu-id="cfbec-248">El siguiente componente primario usa `ChildComponent` y enlaza el parámetro `ParentYear` desde el elemento primario al parámetro `Year` del componente secundario:</span><span class="sxs-lookup"><span data-stu-id="cfbec-248">The following parent component uses `ChildComponent` and binds the `ParentYear` parameter from the parent to the `Year` parameter on the child component:</span></span>

```cshtml
@page "/ParentComponent"

<h1>Parent Component</h1>

<p>ParentYear: @ParentYear</p>

<ChildComponent @bind-Year="ParentYear" />

<button class="btn btn-primary" @onclick="ChangeTheYear">
    Change Year to 1986
</button>

@code {
    [Parameter]
    public int ParentYear { get; set; } = 1978;

    private void ChangeTheYear()
    {
        ParentYear = 1986;
    }
}
```

<span data-ttu-id="cfbec-249">Al cargar el `ParentComponent`, se genera el siguiente marcado:</span><span class="sxs-lookup"><span data-stu-id="cfbec-249">Loading the `ParentComponent` produces the following markup:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

<span data-ttu-id="cfbec-250">Si se cambia el valor de la propiedad `ParentYear` mediante la selección del botón en el `ParentComponent`, se actualiza la propiedad `Year` del `ChildComponent`.</span><span class="sxs-lookup"><span data-stu-id="cfbec-250">If the value of the `ParentYear` property is changed by selecting the button in the `ParentComponent`, the `Year` property of the `ChildComponent` is updated.</span></span> <span data-ttu-id="cfbec-251">El nuevo valor de `Year` se representa en la interfaz de usuario cuando se representa el `ParentComponent`:</span><span class="sxs-lookup"><span data-stu-id="cfbec-251">The new value of `Year` is rendered in the UI when the `ParentComponent` is rerendered:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

<span data-ttu-id="cfbec-252">El parámetro `Year` es enlazable porque tiene un evento Companion `YearChanged` que coincide con el tipo del parámetro `Year`.</span><span class="sxs-lookup"><span data-stu-id="cfbec-252">The `Year` parameter is bindable because it has a companion `YearChanged` event that matches the type of the `Year` parameter.</span></span>

<span data-ttu-id="cfbec-253">Por Convención, `<ChildComponent @bind-Year="ParentYear" />` es esencialmente equivalente a escribir:</span><span class="sxs-lookup"><span data-stu-id="cfbec-253">By convention, `<ChildComponent @bind-Year="ParentYear" />` is essentially equivalent to writing:</span></span>

```cshtml
<ChildComponent @bind-Year="ParentYear" @bind-Year:event="YearChanged" />
```

<span data-ttu-id="cfbec-254">En general, una propiedad se puede enlazar a un controlador de eventos correspondiente mediante el atributo `@bind-property:event`.</span><span class="sxs-lookup"><span data-stu-id="cfbec-254">In general, a property can be bound to a corresponding event handler using `@bind-property:event` attribute.</span></span> <span data-ttu-id="cfbec-255">Por ejemplo, la propiedad `MyProp` se puede enlazar a `MyEventHandler` mediante los dos atributos siguientes:</span><span class="sxs-lookup"><span data-stu-id="cfbec-255">For example, the property `MyProp` can be bound to `MyEventHandler` using the following two attributes:</span></span>

```cshtml
<MyComponent @bind-MyProp="MyValue" @bind-MyProp:event="MyEventHandler" />
```

## <a name="event-handling"></a><span data-ttu-id="cfbec-256">Control de eventos</span><span class="sxs-lookup"><span data-stu-id="cfbec-256">Event handling</span></span>

<span data-ttu-id="cfbec-257">Los componentes de Razor proporcionan características de control de eventos.</span><span class="sxs-lookup"><span data-stu-id="cfbec-257">Razor components provide event handling features.</span></span> <span data-ttu-id="cfbec-258">Para un atributo de elemento HTML denominado `on{event}` (por ejemplo, `onclick` y `onsubmit`) con un valor de tipo delegado, los componentes de Razor tratan el valor del atributo como un controlador de eventos.</span><span class="sxs-lookup"><span data-stu-id="cfbec-258">For an HTML element attribute named `on{event}` (for example, `onclick` and `onsubmit`) with a delegate-typed value, Razor components treats the attribute's value as an event handler.</span></span> <span data-ttu-id="cfbec-259">Siempre se da formato al nombre del atributo [@on {Event}](xref:mvc/views/razor#onevent).</span><span class="sxs-lookup"><span data-stu-id="cfbec-259">The attribute's name is always formatted [@on{event}](xref:mvc/views/razor#onevent).</span></span>

<span data-ttu-id="cfbec-260">El código siguiente llama al método `UpdateHeading` cuando se selecciona el botón en la interfaz de usuario:</span><span class="sxs-lookup"><span data-stu-id="cfbec-260">The following code calls the `UpdateHeading` method when the button is selected in the UI:</span></span>

```cshtml
<button class="btn btn-primary" @onclick="UpdateHeading">
    Update heading
</button>

@code {
    private void UpdateHeading(MouseEventArgs e)
    {
        ...
    }
}
```

<span data-ttu-id="cfbec-261">El código siguiente llama al método `CheckChanged` cuando se cambia la casilla en la interfaz de usuario:</span><span class="sxs-lookup"><span data-stu-id="cfbec-261">The following code calls the `CheckChanged` method when the check box is changed in the UI:</span></span>

```cshtml
<input type="checkbox" class="form-check-input" @onchange="CheckChanged" />

@code {
    private void CheckChanged()
    {
        ...
    }
}
```

<span data-ttu-id="cfbec-262">Los controladores de eventos también pueden ser asincrónicos y devolver un <xref:System.Threading.Tasks.Task>.</span><span class="sxs-lookup"><span data-stu-id="cfbec-262">Event handlers can also be asynchronous and return a <xref:System.Threading.Tasks.Task>.</span></span> <span data-ttu-id="cfbec-263">No es necesario llamar a `StateHasChanged()` de forma manual.</span><span class="sxs-lookup"><span data-stu-id="cfbec-263">There's no need to manually call `StateHasChanged()`.</span></span> <span data-ttu-id="cfbec-264">Las excepciones se registran cuando se producen.</span><span class="sxs-lookup"><span data-stu-id="cfbec-264">Exceptions are logged when they occur.</span></span>

<span data-ttu-id="cfbec-265">En el ejemplo siguiente, se llama a `UpdateHeading` de forma asincrónica cuando se selecciona el botón:</span><span class="sxs-lookup"><span data-stu-id="cfbec-265">In the following example, `UpdateHeading` is called asynchronously when the button is selected:</span></span>

```cshtml
<button class="btn btn-primary" @onclick="UpdateHeading">
    Update heading
</button>

@code {
    private async Task UpdateHeading(MouseEventArgs e)
    {
        ...
    }
}
```

### <a name="event-argument-types"></a><span data-ttu-id="cfbec-266">Tipos de argumentos de evento</span><span class="sxs-lookup"><span data-stu-id="cfbec-266">Event argument types</span></span>

<span data-ttu-id="cfbec-267">En algunos eventos, se permiten los tipos de argumento de evento.</span><span class="sxs-lookup"><span data-stu-id="cfbec-267">For some events, event argument types are permitted.</span></span> <span data-ttu-id="cfbec-268">Si no es necesario el acceso a uno de estos tipos de evento, no es necesario en la llamada al método.</span><span class="sxs-lookup"><span data-stu-id="cfbec-268">If access to one of these event types isn't necessary, it isn't required in the method call.</span></span>

<span data-ttu-id="cfbec-269">En la tabla siguiente se muestran @no__t admitidas: 0.</span><span class="sxs-lookup"><span data-stu-id="cfbec-269">Supported `EventArgs` are shown in the following table.</span></span>

| <span data-ttu-id="cfbec-270">evento</span><span class="sxs-lookup"><span data-stu-id="cfbec-270">Event</span></span> | <span data-ttu-id="cfbec-271">Clase</span><span class="sxs-lookup"><span data-stu-id="cfbec-271">Class</span></span> |
| ----- | ----- |
| <span data-ttu-id="cfbec-272">Portapapeles</span><span class="sxs-lookup"><span data-stu-id="cfbec-272">Clipboard</span></span>        | `ClipboardEventArgs` |
| <span data-ttu-id="cfbec-273">Arrastre</span><span class="sxs-lookup"><span data-stu-id="cfbec-273">Drag</span></span>             | <span data-ttu-id="cfbec-274">`DragEventArgs` &ndash; `DataTransfer` y `DataTransferItem` contienen datos de elementos arrastrados.</span><span class="sxs-lookup"><span data-stu-id="cfbec-274">`DragEventArgs` &ndash; `DataTransfer` and `DataTransferItem` hold dragged item data.</span></span> |
| <span data-ttu-id="cfbec-275">Error</span><span class="sxs-lookup"><span data-stu-id="cfbec-275">Error</span></span>            | `ErrorEventArgs` |
| <span data-ttu-id="cfbec-276">Foco</span><span class="sxs-lookup"><span data-stu-id="cfbec-276">Focus</span></span>            | <span data-ttu-id="cfbec-277">`FocusEventArgs` &ndash; no incluye compatibilidad con `relatedTarget`.</span><span class="sxs-lookup"><span data-stu-id="cfbec-277">`FocusEventArgs` &ndash; Doesn't include support for `relatedTarget`.</span></span> |
| <span data-ttu-id="cfbec-278">Cambio de`<input>`</span><span class="sxs-lookup"><span data-stu-id="cfbec-278">`<input>` change</span></span> | `ChangeEventArgs` |
| <span data-ttu-id="cfbec-279">Teclado</span><span class="sxs-lookup"><span data-stu-id="cfbec-279">Keyboard</span></span>         | `KeyboardEventArgs` |
| <span data-ttu-id="cfbec-280">Mouse</span><span class="sxs-lookup"><span data-stu-id="cfbec-280">Mouse</span></span>            | `MouseEventArgs` |
| <span data-ttu-id="cfbec-281">Puntero del mouse</span><span class="sxs-lookup"><span data-stu-id="cfbec-281">Mouse pointer</span></span>    | `PointerEventArgs` |
| <span data-ttu-id="cfbec-282">Rueda del mouse</span><span class="sxs-lookup"><span data-stu-id="cfbec-282">Mouse wheel</span></span>      | `WheelEventArgs` |
| <span data-ttu-id="cfbec-283">Progreso</span><span class="sxs-lookup"><span data-stu-id="cfbec-283">Progress</span></span>         | `ProgressEventArgs` |
| <span data-ttu-id="cfbec-284">Entrada táctil</span><span class="sxs-lookup"><span data-stu-id="cfbec-284">Touch</span></span>            | <span data-ttu-id="cfbec-285">`TouchEventArgs` &ndash; `TouchPoint` representa un único punto de contacto en un dispositivo sensible al tacto.</span><span class="sxs-lookup"><span data-stu-id="cfbec-285">`TouchEventArgs` &ndash; `TouchPoint` represents a single contact point on a touch-sensitive device.</span></span> |

<span data-ttu-id="cfbec-286">Para obtener información sobre las propiedades y el comportamiento de control de eventos de los eventos de la tabla anterior, vea [clases EventArgs en el origen de referencia (rama ASPNET/AspNetCore Release/3.0)](https://github.com/aspnet/AspNetCore/tree/release/3.0/src/Components/Web/src/Web).</span><span class="sxs-lookup"><span data-stu-id="cfbec-286">For information on the properties and event handling behavior of the events in the preceding table, see [EventArgs classes in the reference source (aspnet/AspNetCore release/3.0 branch)](https://github.com/aspnet/AspNetCore/tree/release/3.0/src/Components/Web/src/Web).</span></span>

### <a name="lambda-expressions"></a><span data-ttu-id="cfbec-287">Expresiones lambda</span><span class="sxs-lookup"><span data-stu-id="cfbec-287">Lambda expressions</span></span>

<span data-ttu-id="cfbec-288">También se pueden usar expresiones lambda:</span><span class="sxs-lookup"><span data-stu-id="cfbec-288">Lambda expressions can also be used:</span></span>

```cshtml
<button @onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

<span data-ttu-id="cfbec-289">A menudo resulta cómodo cerrar los valores adicionales, como al recorrer en iteración un conjunto de elementos.</span><span class="sxs-lookup"><span data-stu-id="cfbec-289">It's often convenient to close over additional values, such as when iterating over a set of elements.</span></span> <span data-ttu-id="cfbec-290">En el ejemplo siguiente se crean tres botones, cada uno de los cuales llama a `UpdateHeading` pasando un argumento de evento (`MouseEventArgs`) y su número de botón (@no__t 2) cuando se selecciona en la interfaz de usuario:</span><span class="sxs-lookup"><span data-stu-id="cfbec-290">The following example creates three buttons, each of which calls `UpdateHeading` passing an event argument (`MouseEventArgs`) and its button number (`buttonNumber`) when selected in the UI:</span></span>

```cshtml
<h2>@message</h2>

@for (var i = 1; i < 4; i++)
{
    var buttonNumber = i;

    <button class="btn btn-primary"
            @onclick="@(e => UpdateHeading(e, buttonNumber))">
        Button #@i
    </button>
}

@code {
    private string message = "Select a button to learn its position.";

    private void UpdateHeading(MouseEventArgs e, int buttonNumber)
    {
        message = $"You selected Button #{buttonNumber} at " +
            $"mouse position: {e.ClientX} X {e.ClientY}.";
    }
}
```

> [!NOTE]
> <span data-ttu-id="cfbec-291">**No** use la variable de bucle (`i`) en un bucle `for` directamente en una expresión lambda.</span><span class="sxs-lookup"><span data-stu-id="cfbec-291">Do **not** use the loop variable (`i`) in a `for` loop directly in a lambda expression.</span></span> <span data-ttu-id="cfbec-292">De lo contrario, todas las expresiones lambda usan la misma variable, lo que hace que el valor de `i` sea el mismo en todas las expresiones lambda.</span><span class="sxs-lookup"><span data-stu-id="cfbec-292">Otherwise the same variable is used by all lambda expressions causing `i`'s value to be the same in all lambdas.</span></span> <span data-ttu-id="cfbec-293">Capture siempre su valor en una variable local (`buttonNumber` en el ejemplo anterior) y, a continuación, úsela.</span><span class="sxs-lookup"><span data-stu-id="cfbec-293">Always capture its value in a local variable (`buttonNumber` in the preceding example) and then use it.</span></span>

### <a name="eventcallback"></a><span data-ttu-id="cfbec-294">EventCallback</span><span class="sxs-lookup"><span data-stu-id="cfbec-294">EventCallback</span></span>

<span data-ttu-id="cfbec-295">Un escenario común con los componentes anidados es el deseo de ejecutar el método de un componente primario cuando se produce un evento de componente secundario en el ejemplo no__t-0for, cuando se produce un evento `onclick` en el elemento secundario.</span><span class="sxs-lookup"><span data-stu-id="cfbec-295">A common scenario with nested components is the desire to run a parent component's method when a child component event occurs&mdash;for example, when an `onclick` event occurs in the child.</span></span> <span data-ttu-id="cfbec-296">Para exponer eventos entre componentes, use un `EventCallback`.</span><span class="sxs-lookup"><span data-stu-id="cfbec-296">To expose events across components, use an `EventCallback`.</span></span> <span data-ttu-id="cfbec-297">Un componente primario puede asignar un método de devolución de llamada al `EventCallback` de un componente secundario.</span><span class="sxs-lookup"><span data-stu-id="cfbec-297">A parent component can assign a callback method to a child component's `EventCallback`.</span></span>

<span data-ttu-id="cfbec-298">En el `ChildComponent` de la aplicación de ejemplo se muestra cómo se configura el controlador de `onclick` de un botón para recibir un delegado de @no__t 2 del `ParentComponent` del ejemplo.</span><span class="sxs-lookup"><span data-stu-id="cfbec-298">The `ChildComponent` in the sample app demonstrates how a button's `onclick` handler is set up to receive an `EventCallback` delegate from the sample's `ParentComponent`.</span></span> <span data-ttu-id="cfbec-299">El `EventCallback` se escribe con `MouseEventArgs`, que es adecuado para un evento de @no__t 2 desde un dispositivo periférico:</span><span class="sxs-lookup"><span data-stu-id="cfbec-299">The `EventCallback` is typed with `MouseEventArgs`, which is appropriate for an `onclick` event from a peripheral device:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=5-7,17-18)]

<span data-ttu-id="cfbec-300">El `ParentComponent` establece el `EventCallback<T>` del elemento secundario en el método `ShowMessage`:</span><span class="sxs-lookup"><span data-stu-id="cfbec-300">The `ParentComponent` sets the child's `EventCallback<T>` to its `ShowMessage` method:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=6,16-19)]

<span data-ttu-id="cfbec-301">Cuando el botón está seleccionado en el `ChildComponent`:</span><span class="sxs-lookup"><span data-stu-id="cfbec-301">When the button is selected in the `ChildComponent`:</span></span>

* <span data-ttu-id="cfbec-302">Se llama al método `ShowMessage` de `ParentComponent`.</span><span class="sxs-lookup"><span data-stu-id="cfbec-302">The `ParentComponent`'s `ShowMessage` method is called.</span></span> <span data-ttu-id="cfbec-303">`messageText` se actualiza y se muestra en el `ParentComponent`.</span><span class="sxs-lookup"><span data-stu-id="cfbec-303">`messageText` is updated and displayed in the `ParentComponent`.</span></span>
* <span data-ttu-id="cfbec-304">No se requiere una llamada a `StateHasChanged` en el método de la devolución de llamada (`ShowMessage`).</span><span class="sxs-lookup"><span data-stu-id="cfbec-304">A call to `StateHasChanged` isn't required in the callback's method (`ShowMessage`).</span></span> <span data-ttu-id="cfbec-305">se llama a `StateHasChanged` automáticamente para rerepresentar el `ParentComponent`, del mismo modo que los eventos secundarios desencadenan la rerepresentación de componentes en los controladores de eventos que se ejecutan dentro del elemento secundario.</span><span class="sxs-lookup"><span data-stu-id="cfbec-305">`StateHasChanged` is called automatically to rerender the `ParentComponent`, just as child events trigger component rerendering in event handlers that execute within the child.</span></span>

<span data-ttu-id="cfbec-306">`EventCallback` y `EventCallback<T>` permiten los delegados asincrónicos.</span><span class="sxs-lookup"><span data-stu-id="cfbec-306">`EventCallback` and `EventCallback<T>` permit asynchronous delegates.</span></span> <span data-ttu-id="cfbec-307">`EventCallback<T>` tiene un tipo seguro y requiere un tipo de argumento específico.</span><span class="sxs-lookup"><span data-stu-id="cfbec-307">`EventCallback<T>` is strongly typed and requires a specific argument type.</span></span> <span data-ttu-id="cfbec-308">`EventCallback` tiene un tipo débil y permite cualquier tipo de argumento.</span><span class="sxs-lookup"><span data-stu-id="cfbec-308">`EventCallback` is weakly typed and allows any argument type.</span></span>

```cshtml
<p><b>@messageText</b></p>

@{ var message = "Default Text"; }

<ChildComponent 
    OnClick="@(async () => { await Task.Yield(); messageText = "Blaze It!"; })" />

@code {
    private string messageText;
}
```

<span data-ttu-id="cfbec-309">Invocar un `EventCallback` o `EventCallback<T>` con `InvokeAsync` y esperar el <xref:System.Threading.Tasks.Task>:</span><span class="sxs-lookup"><span data-stu-id="cfbec-309">Invoke an `EventCallback` or `EventCallback<T>` with `InvokeAsync` and await the <xref:System.Threading.Tasks.Task>:</span></span>

```csharp
await callback.InvokeAsync(arg);
```

<span data-ttu-id="cfbec-310">Use `EventCallback` y `EventCallback<T>` para el control de eventos y los parámetros de componente de enlace.</span><span class="sxs-lookup"><span data-stu-id="cfbec-310">Use `EventCallback` and `EventCallback<T>` for event handling and binding component parameters.</span></span>

<span data-ttu-id="cfbec-311">Prefiera el @no__t fuertemente tipado en `EventCallback`.</span><span class="sxs-lookup"><span data-stu-id="cfbec-311">Prefer the strongly typed `EventCallback<T>` over `EventCallback`.</span></span> <span data-ttu-id="cfbec-312">`EventCallback<T>` proporciona mejores comentarios de errores a los usuarios del componente.</span><span class="sxs-lookup"><span data-stu-id="cfbec-312">`EventCallback<T>` provides better error feedback to users of the component.</span></span> <span data-ttu-id="cfbec-313">Al igual que otros controladores de eventos de la interfaz de usuario, la especificación del parámetro Event es opcional.</span><span class="sxs-lookup"><span data-stu-id="cfbec-313">Similar to other UI event handlers, specifying the event parameter is optional.</span></span> <span data-ttu-id="cfbec-314">Use `EventCallback` cuando no haya ningún valor pasado a la devolución de llamada.</span><span class="sxs-lookup"><span data-stu-id="cfbec-314">Use `EventCallback` when there's no value passed to the callback.</span></span>

## <a name="chained-bind"></a><span data-ttu-id="cfbec-315">Enlace encadenado</span><span class="sxs-lookup"><span data-stu-id="cfbec-315">Chained bind</span></span>

<span data-ttu-id="cfbec-316">Un escenario común es encadenar un parámetro enlazado a datos a un elemento Page en la salida del componente.</span><span class="sxs-lookup"><span data-stu-id="cfbec-316">A common scenario is chaining a data-bound parameter to a page element in the component's output.</span></span> <span data-ttu-id="cfbec-317">Este escenario se denomina *enlace encadenado* porque se producen varios niveles de enlace simultáneamente.</span><span class="sxs-lookup"><span data-stu-id="cfbec-317">This scenario is called a *chained bind* because multiple levels of binding occur simultaneously.</span></span>

<span data-ttu-id="cfbec-318">No se puede implementar un enlace encadenado con la sintaxis `@bind` en el elemento de la página.</span><span class="sxs-lookup"><span data-stu-id="cfbec-318">A chained bind can't be implemented with `@bind` syntax in the page's element.</span></span> <span data-ttu-id="cfbec-319">El controlador de eventos y el valor se deben especificar por separado.</span><span class="sxs-lookup"><span data-stu-id="cfbec-319">The event handler and value must be specified separately.</span></span> <span data-ttu-id="cfbec-320">Sin embargo, un componente primario puede usar la sintaxis `@bind` con el parámetro del componente.</span><span class="sxs-lookup"><span data-stu-id="cfbec-320">A parent component, however, can use `@bind` syntax with the component's parameter.</span></span>

<span data-ttu-id="cfbec-321">El siguiente componente `PasswordField` (*PasswordField. Razor*):</span><span class="sxs-lookup"><span data-stu-id="cfbec-321">The following `PasswordField` component (*PasswordField.razor*):</span></span>

* <span data-ttu-id="cfbec-322">Establece el valor de un elemento `<input>` en una propiedad `Password`.</span><span class="sxs-lookup"><span data-stu-id="cfbec-322">Sets an `<input>` element's value to a `Password` property.</span></span>
* <span data-ttu-id="cfbec-323">Expone los cambios de la propiedad `Password` a un componente primario con un [EventCallback](#eventcallback).</span><span class="sxs-lookup"><span data-stu-id="cfbec-323">Exposes changes of the `Password` property to a parent component with an [EventCallback](#eventcallback).</span></span>

```cshtml
Password: 

<input @oninput="OnPasswordChanged" 
       required 
       type="@(showPassword ? "text" : "password")" 
       value="@Password" />

<button class="btn btn-primary" @onclick="ToggleShowPassword">
    Show password
</button>

@code {
    private bool showPassword;

    [Parameter]
    public string Password { get; set; }

    [Parameter]
    public EventCallback<string> PasswordChanged { get; set; }

    private Task OnPasswordChanged(ChangeEventArgs e)
    {
        Password = e.Value.ToString();

        return PasswordChanged.InvokeAsync(Password);
    }

    private void ToggleShowPassword()
    {
        showPassword = !showPassword;
    }
}
```

<span data-ttu-id="cfbec-324">El componente `PasswordField` se usa en otro componente:</span><span class="sxs-lookup"><span data-stu-id="cfbec-324">The `PasswordField` component is used in another component:</span></span>

```cshtml
<PasswordField @bind-Password="password" />

@code {
    private string password;
}
```

<span data-ttu-id="cfbec-325">Para realizar comprobaciones o errores de captura en la contraseña en el ejemplo anterior:</span><span class="sxs-lookup"><span data-stu-id="cfbec-325">To perform checks or trap errors on the password in the preceding example:</span></span>

* <span data-ttu-id="cfbec-326">Cree un campo de respaldo para `Password` (`password` en el siguiente código de ejemplo).</span><span class="sxs-lookup"><span data-stu-id="cfbec-326">Create a backing field for `Password` (`password` in the following example code).</span></span>
* <span data-ttu-id="cfbec-327">Realice las comprobaciones o errores de captura en el establecedor @no__t 0.</span><span class="sxs-lookup"><span data-stu-id="cfbec-327">Perform the checks or trap errors in the `Password` setter.</span></span>

<span data-ttu-id="cfbec-328">En el ejemplo siguiente se proporcionan comentarios inmediatos al usuario si se usa un espacio en el valor de la contraseña:</span><span class="sxs-lookup"><span data-stu-id="cfbec-328">The following example provides immediate feedback to the user if a space is used in the password's value:</span></span>

```cshtml
Password: 

<input @oninput="OnPasswordChanged" 
       required 
       type="@(showPassword ? "text" : "password")" 
       value="@Password" />

<button class="btn btn-primary" @onclick="ToggleShowPassword">
    Show password
</button>

<span class="text-danger">@validationMessage</span>

@code {
    private bool showPassword;
    private string password;
    private string validationMessage;

    [Parameter]
    public string Password
    {
        get { return password ?? string.Empty; }
        set
        {
            if (password != value)
            {
                if (value.Contains(' '))
                {
                    validationMessage = "Spaces not allowed!";
                }
                else
                {
                    password = value;
                    validationMessage = string.Empty;
                }
            }
        }
    }

    [Parameter]
    public EventCallback<string> PasswordChanged { get; set; }

    private Task OnPasswordChanged(ChangeEventArgs e)
    {
        Password = e.Value.ToString();

        return PasswordChanged.InvokeAsync(Password);
    }

    private void ToggleShowPassword()
    {
        showPassword = !showPassword;
    }
}
```

## <a name="capture-references-to-components"></a><span data-ttu-id="cfbec-329">Capturar referencias a componentes</span><span class="sxs-lookup"><span data-stu-id="cfbec-329">Capture references to components</span></span>

<span data-ttu-id="cfbec-330">Las referencias de componente proporcionan una manera de hacer referencia a una instancia de componente para que pueda emitir comandos a esa instancia, como `Show` o `Reset`.</span><span class="sxs-lookup"><span data-stu-id="cfbec-330">Component references provide a way to reference a component instance so that you can issue commands to that instance, such as `Show` or `Reset`.</span></span> <span data-ttu-id="cfbec-331">Para capturar una referencia de componente:</span><span class="sxs-lookup"><span data-stu-id="cfbec-331">To capture a component reference:</span></span>

* <span data-ttu-id="cfbec-332">Agregue un atributo [@ref](xref:mvc/views/razor#ref) al componente secundario.</span><span class="sxs-lookup"><span data-stu-id="cfbec-332">Add an [@ref](xref:mvc/views/razor#ref) attribute to the child component.</span></span>
* <span data-ttu-id="cfbec-333">Defina un campo con el mismo tipo que el componente secundario.</span><span class="sxs-lookup"><span data-stu-id="cfbec-333">Define a field with the same type as the child component.</span></span>

```cshtml
<MyLoginDialog @ref="loginDialog" ... />

@code {
    private MyLoginDialog loginDialog;

    private void OnSomething()
    {
        loginDialog.Show();
    }
}
```

<span data-ttu-id="cfbec-334">Cuando se representa el componente, el campo `loginDialog` se rellena con la instancia del componente secundario `MyLoginDialog`.</span><span class="sxs-lookup"><span data-stu-id="cfbec-334">When the component is rendered, the `loginDialog` field is populated with the `MyLoginDialog` child component instance.</span></span> <span data-ttu-id="cfbec-335">Después, puede invocar métodos .NET en la instancia del componente.</span><span class="sxs-lookup"><span data-stu-id="cfbec-335">You can then invoke .NET methods on the component instance.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="cfbec-336">La variable `loginDialog` solo se rellena después de que el componente se represente y su salida incluye el elemento `MyLoginDialog`.</span><span class="sxs-lookup"><span data-stu-id="cfbec-336">The `loginDialog` variable is only populated after the component is rendered and its output includes the `MyLoginDialog` element.</span></span> <span data-ttu-id="cfbec-337">Hasta ese momento, no hay nada que hacer referencia.</span><span class="sxs-lookup"><span data-stu-id="cfbec-337">Until that point, there's nothing to reference.</span></span> <span data-ttu-id="cfbec-338">Para manipular las referencias de componentes una vez finalizada la representación del componente, use los [métodos OnAfterRenderAsync o OnAfterRender](#lifecycle-methods).</span><span class="sxs-lookup"><span data-stu-id="cfbec-338">To manipulate components references after the component has finished rendering, use the [OnAfterRenderAsync or OnAfterRender methods](#lifecycle-methods).</span></span>

<span data-ttu-id="cfbec-339">Al capturar referencias de componentes, use una sintaxis similar para [capturar referencias de elemento](xref:blazor/javascript-interop#capture-references-to-elements), no es una característica de [interoperabilidad de JavaScript](xref:blazor/javascript-interop) .</span><span class="sxs-lookup"><span data-stu-id="cfbec-339">While capturing component references use a similar syntax to [capturing element references](xref:blazor/javascript-interop#capture-references-to-elements), it isn't a [JavaScript interop](xref:blazor/javascript-interop) feature.</span></span> <span data-ttu-id="cfbec-340">Las referencias de componente no se pasan al código JavaScript @ no__t-0they're solo se usa en código de .NET.</span><span class="sxs-lookup"><span data-stu-id="cfbec-340">Component references aren't passed to JavaScript code&mdash;they're only used in .NET code.</span></span>

> [!NOTE]
> <span data-ttu-id="cfbec-341">**No** utilice referencias de componentes para mutar el estado de los componentes secundarios.</span><span class="sxs-lookup"><span data-stu-id="cfbec-341">Do **not** use component references to mutate the state of child components.</span></span> <span data-ttu-id="cfbec-342">En su lugar, use parámetros declarativos normales para pasar datos a componentes secundarios.</span><span class="sxs-lookup"><span data-stu-id="cfbec-342">Instead, use normal declarative parameters to pass data to child components.</span></span> <span data-ttu-id="cfbec-343">El uso de parámetros declarativos normales da como resultado componentes secundarios que se representarán automáticamente en las horas correctas.</span><span class="sxs-lookup"><span data-stu-id="cfbec-343">Use of normal declarative parameters result in child components that rerender at the correct times automatically.</span></span>

## <a name="invoke-component-methods-externally-to-update-state"></a><span data-ttu-id="cfbec-344">Invocar métodos de componentes externamente para actualizar el estado</span><span class="sxs-lookup"><span data-stu-id="cfbec-344">Invoke component methods externally to update state</span></span>

<span data-ttu-id="cfbec-345">Increíble utiliza un `SynchronizationContext` para aplicar un único subproceso lógico de ejecución.</span><span class="sxs-lookup"><span data-stu-id="cfbec-345">Blazor uses a `SynchronizationContext` to enforce a single logical thread of execution.</span></span> <span data-ttu-id="cfbec-346">Los métodos de ciclo de vida de un componente y todas las devoluciones de llamada de eventos que se producen con el método increíblemente se ejecutan en este `SynchronizationContext`.</span><span class="sxs-lookup"><span data-stu-id="cfbec-346">A component's lifecycle methods and any event callbacks that are raised by Blazor are executed on this `SynchronizationContext`.</span></span> <span data-ttu-id="cfbec-347">En caso de que un componente deba actualizarse en función de un evento externo, como un temporizador u otras notificaciones, use el método `InvokeAsync`, que se enviará al `SynchronizationContext` de la última actualización.</span><span class="sxs-lookup"><span data-stu-id="cfbec-347">In the event a component must be updated based on an external event, such as a timer or other notifications, use the `InvokeAsync` method, which will dispatch to Blazor's `SynchronizationContext`.</span></span>

<span data-ttu-id="cfbec-348">Por ejemplo, considere un *servicio de notificador* que puede notificar a cualquier componente de escucha del estado actualizado:</span><span class="sxs-lookup"><span data-stu-id="cfbec-348">For example, consider a *notifier service* that can notify any listening component of the updated state:</span></span>

```csharp
public class NotifierService
{
    // Can be called from anywhere
    public async Task Update(string key, int value)
    {
        if (Notify != null)
        {
            await Notify.Invoke(key, value);
        }
    }

    public event Func<string, int, Task> Notify;
}
```

<span data-ttu-id="cfbec-349">Uso del `NotifierService` para actualizar un componente:</span><span class="sxs-lookup"><span data-stu-id="cfbec-349">Usage of the `NotifierService` to update a component:</span></span>

```cshtml
@page "/"
@inject NotifierService Notifier
@implements IDisposable

<p>Last update: @lastNotification.key = @lastNotification.value</p>

@code {
    private (string key, int value) lastNotification;

    protected override void OnInitialized()
    {
        Notifier.Notify += OnNotify;
    }

    public async Task OnNotify(string key, int value)
    {
        await InvokeAsync(() =>
        {
            lastNotification = (key, value);
            StateHasChanged();
        });
    }

    public void Dispose()
    {
        Notifier.Notify -= OnNotify;
    }
}
```

<span data-ttu-id="cfbec-350">En el ejemplo anterior, `NotifierService` invoca el método `OnNotify` del componente fuera del `SynchronizationContext` del último.</span><span class="sxs-lookup"><span data-stu-id="cfbec-350">In the preceding example, `NotifierService` invokes the component's `OnNotify` method outside of Blazor's `SynchronizationContext`.</span></span> <span data-ttu-id="cfbec-351">`InvokeAsync` se usa para cambiar al contexto correcto y poner en cola una representación.</span><span class="sxs-lookup"><span data-stu-id="cfbec-351">`InvokeAsync` is used to switch to the correct context and queue a render.</span></span>

## <a name="use-key-to-control-the-preservation-of-elements-and-components"></a><span data-ttu-id="cfbec-352">Use \@key para controlar la preservación de elementos y componentes</span><span class="sxs-lookup"><span data-stu-id="cfbec-352">Use \@key to control the preservation of elements and components</span></span>

<span data-ttu-id="cfbec-353">Cuando se representa una lista de elementos o componentes, y los elementos o componentes cambian posteriormente, el algoritmo de diferenciación de más increíble debe decidir cuáles de los elementos o componentes anteriores se pueden conservar y cómo deben asignarse los objetos de modelo.</span><span class="sxs-lookup"><span data-stu-id="cfbec-353">When rendering a list of elements or components and the elements or components subsequently change, Blazor's diffing algorithm must decide which of the previous elements or components can be retained and how model objects should map to them.</span></span> <span data-ttu-id="cfbec-354">Normalmente, este proceso es automático y se puede omitir, pero hay casos en los que puede que desee controlar el proceso.</span><span class="sxs-lookup"><span data-stu-id="cfbec-354">Normally, this process is automatic and can be ignored, but there are cases where you may want to control the process.</span></span>

<span data-ttu-id="cfbec-355">Considere el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="cfbec-355">Consider the following example:</span></span>

```csharp
@foreach (var person in People)
{
    <DetailsEditor Details="person.Details" />
}

@code {
    [Parameter]
    public IEnumerable<Person> People { get; set; }
}
```

<span data-ttu-id="cfbec-356">El contenido de la colección `People` puede cambiar con entradas insertadas, eliminadas o reordenadas.</span><span class="sxs-lookup"><span data-stu-id="cfbec-356">The contents of the `People` collection may change with inserted, deleted, or re-ordered entries.</span></span> <span data-ttu-id="cfbec-357">Cuando se representa el componente, el componente `<DetailsEditor>` puede cambiar para recibir diferentes valores de parámetro de `Details`.</span><span class="sxs-lookup"><span data-stu-id="cfbec-357">When the component rerenders, the `<DetailsEditor>` component may change to receive different `Details` parameter values.</span></span> <span data-ttu-id="cfbec-358">Esto puede producir una rerepresentación más compleja de lo esperado.</span><span class="sxs-lookup"><span data-stu-id="cfbec-358">This may cause more complex rerendering than expected.</span></span> <span data-ttu-id="cfbec-359">En algunos casos, la rerepresentación puede producir diferencias de comportamiento visibles, como el foco del elemento perdido.</span><span class="sxs-lookup"><span data-stu-id="cfbec-359">In some cases, rerendering can lead to visible behavior differences, such as lost element focus.</span></span>

<span data-ttu-id="cfbec-360">El proceso de asignación se puede controlar con el atributo de directiva `@key`.</span><span class="sxs-lookup"><span data-stu-id="cfbec-360">The mapping process can be controlled with the `@key` directive attribute.</span></span> <span data-ttu-id="cfbec-361">`@key` hace que el algoritmo de comparación garantice la preservación de elementos o componentes basados en el valor de la clave:</span><span class="sxs-lookup"><span data-stu-id="cfbec-361">`@key` causes the diffing algorithm to guarantee preservation of elements or components based on the key's value:</span></span>

```csharp
@foreach (var person in People)
{
    <DetailsEditor @key="person" Details="person.Details" />
}

@code {
    [Parameter]
    public IEnumerable<Person> People { get; set; }
}
```

<span data-ttu-id="cfbec-362">Cuando cambia la colección `People`, el algoritmo de comparación mantiene la asociación entre las instancias de `<DetailsEditor>` y `person`:</span><span class="sxs-lookup"><span data-stu-id="cfbec-362">When the `People` collection changes, the diffing algorithm retains the association between `<DetailsEditor>` instances and `person` instances:</span></span>

* <span data-ttu-id="cfbec-363">Si se elimina un `Person` de la lista de `People`, solo se quita de la interfaz de usuario la instancia de `<DetailsEditor>` correspondiente.</span><span class="sxs-lookup"><span data-stu-id="cfbec-363">If a `Person` is deleted from the `People` list, only the corresponding `<DetailsEditor>` instance is removed from the UI.</span></span> <span data-ttu-id="cfbec-364">Otras instancias permanecen sin cambios.</span><span class="sxs-lookup"><span data-stu-id="cfbec-364">Other instances are left unchanged.</span></span>
* <span data-ttu-id="cfbec-365">Si se inserta un `Person` en alguna posición de la lista, se inserta una nueva instancia de `<DetailsEditor>` en esa posición correspondiente.</span><span class="sxs-lookup"><span data-stu-id="cfbec-365">If a `Person` is inserted at some position in the list, one new `<DetailsEditor>` instance is inserted at that corresponding position.</span></span> <span data-ttu-id="cfbec-366">Otras instancias permanecen sin cambios.</span><span class="sxs-lookup"><span data-stu-id="cfbec-366">Other instances are left unchanged.</span></span>
* <span data-ttu-id="cfbec-367">Si se vuelven a ordenar las entradas `Person`, las instancias de `<DetailsEditor>` correspondientes se conservan y se vuelven a ordenar en la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="cfbec-367">If `Person` entries are re-ordered, the corresponding `<DetailsEditor>` instances are preserved and re-ordered in the UI.</span></span>

<span data-ttu-id="cfbec-368">En algunos escenarios, el uso de `@key` reduce la complejidad de la rerepresentación y evita posibles problemas con las partes con estado del DOM que cambian, como la posición del foco.</span><span class="sxs-lookup"><span data-stu-id="cfbec-368">In some scenarios, use of `@key` minimizes the complexity of rerendering and avoids potential issues with stateful parts of the DOM changing, such as focus position.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="cfbec-369">Las claves son locales para cada elemento contenedor o componente.</span><span class="sxs-lookup"><span data-stu-id="cfbec-369">Keys are local to each container element or component.</span></span> <span data-ttu-id="cfbec-370">Las claves no se comparan globalmente en todo el documento.</span><span class="sxs-lookup"><span data-stu-id="cfbec-370">Keys aren't compared globally across the document.</span></span>

### <a name="when-to-use-key"></a><span data-ttu-id="cfbec-371">Cuándo usar \@key</span><span class="sxs-lookup"><span data-stu-id="cfbec-371">When to use \@key</span></span>

<span data-ttu-id="cfbec-372">Normalmente, tiene sentido usar `@key` siempre que se represente una lista (por ejemplo, en un bloque `@foreach`) y exista un valor adecuado para definir la `@key`.</span><span class="sxs-lookup"><span data-stu-id="cfbec-372">Typically, it makes sense to use `@key` whenever a list is rendered (for example, in a `@foreach` block) and a suitable value exists to define the `@key`.</span></span>

<span data-ttu-id="cfbec-373">También puede usar `@key` para evitar que un elemento o subárbol de componentes se conserven cuando cambia un objeto:</span><span class="sxs-lookup"><span data-stu-id="cfbec-373">You can also use `@key` to prevent Blazor from preserving an element or component subtree when an object changes:</span></span>

```cshtml
<div @key="currentPerson">
    ... content that depends on currentPerson ...
</div>
```

<span data-ttu-id="cfbec-374">Si `@currentPerson` cambia, la Directiva de atributo `@key` fuerza a increíblemente a descartar todo el @no__t 2 y sus descendientes y volver a generar el subárbol dentro de la interfaz de usuario con nuevos elementos y componentes.</span><span class="sxs-lookup"><span data-stu-id="cfbec-374">If `@currentPerson` changes, the `@key` attribute directive forces Blazor to discard the entire `<div>` and its descendants and rebuild the subtree within the UI with new elements and components.</span></span> <span data-ttu-id="cfbec-375">Esto puede ser útil si necesita asegurarse de que no se conserva ningún estado de la interfaz de usuario cuando `@currentPerson` cambia.</span><span class="sxs-lookup"><span data-stu-id="cfbec-375">This can be useful if you need to guarantee that no UI state is preserved when `@currentPerson` changes.</span></span>

### <a name="when-not-to-use-key"></a><span data-ttu-id="cfbec-376">Cuándo no se debe usar \@key</span><span class="sxs-lookup"><span data-stu-id="cfbec-376">When not to use \@key</span></span>

<span data-ttu-id="cfbec-377">Existe un costo de rendimiento al diferenciar con `@key`.</span><span class="sxs-lookup"><span data-stu-id="cfbec-377">There's a performance cost when diffing with `@key`.</span></span> <span data-ttu-id="cfbec-378">El costo de rendimiento no es grande, pero solo se especifica `@key` si el control del elemento o las reglas de conservación de componentes se benefician de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="cfbec-378">The performance cost isn't large, but only specify `@key` if controlling the element or component preservation rules benefit the app.</span></span>

<span data-ttu-id="cfbec-379">Incluso si no se usa `@key`, Increíblementeer conserva las instancias de componentes y elementos secundarios lo máximo posible.</span><span class="sxs-lookup"><span data-stu-id="cfbec-379">Even if `@key` isn't used, Blazor preserves child element and component instances as much as possible.</span></span> <span data-ttu-id="cfbec-380">La única ventaja de utilizar `@key` es el control sobre *Cómo* se asignan las instancias de modelo a las instancias de componente conservadas, en lugar del algoritmo de diferenciación que selecciona la asignación.</span><span class="sxs-lookup"><span data-stu-id="cfbec-380">The only advantage to using `@key` is control over *how* model instances are mapped to the preserved component instances, instead of the diffing algorithm selecting the mapping.</span></span>

### <a name="what-values-to-use-for-key"></a><span data-ttu-id="cfbec-381">Qué valores se deben usar para \@key</span><span class="sxs-lookup"><span data-stu-id="cfbec-381">What values to use for \@key</span></span>

<span data-ttu-id="cfbec-382">Por lo general, tiene sentido proporcionar uno de los siguientes tipos de valor para `@key`:</span><span class="sxs-lookup"><span data-stu-id="cfbec-382">Generally, it makes sense to supply one of the following kinds of value for `@key`:</span></span>

* <span data-ttu-id="cfbec-383">Instancias de objeto de modelo (por ejemplo, una instancia de `Person` como en el ejemplo anterior).</span><span class="sxs-lookup"><span data-stu-id="cfbec-383">Model object instances (for example, a `Person` instance as in the earlier example).</span></span> <span data-ttu-id="cfbec-384">Esto garantiza la conservación en función de la igualdad de la referencia de objeto.</span><span class="sxs-lookup"><span data-stu-id="cfbec-384">This ensures preservation based on object reference equality.</span></span>
* <span data-ttu-id="cfbec-385">Los identificadores únicos (por ejemplo, los valores de clave principal de tipo `int`, `string` o `Guid`).</span><span class="sxs-lookup"><span data-stu-id="cfbec-385">Unique identifiers (for example, primary key values of type `int`, `string`, or `Guid`).</span></span>

<span data-ttu-id="cfbec-386">Asegúrese de que los valores usados para `@key` no entren en conflicto.</span><span class="sxs-lookup"><span data-stu-id="cfbec-386">Ensure that values used for `@key` don't clash.</span></span> <span data-ttu-id="cfbec-387">Si se detectan valores en conflicto en el mismo elemento primario, el valor de la excepción extraordinaria produce una excepción porque no puede asignar de forma determinista los elementos o componentes antiguos a los nuevos elementos o componentes.</span><span class="sxs-lookup"><span data-stu-id="cfbec-387">If clashing values are detected within the same parent element, Blazor throws an exception because it can't deterministically map old elements or components to new elements or components.</span></span> <span data-ttu-id="cfbec-388">Utilice solo valores distintos, como instancias de objeto o valores de clave principal.</span><span class="sxs-lookup"><span data-stu-id="cfbec-388">Only use distinct values, such as object instances or primary key values.</span></span>

## <a name="lifecycle-methods"></a><span data-ttu-id="cfbec-389">Métodos de ciclo de vida</span><span class="sxs-lookup"><span data-stu-id="cfbec-389">Lifecycle methods</span></span>

<span data-ttu-id="cfbec-390">`OnInitializedAsync` y `OnInitialized` ejecutan código para inicializar el componente.</span><span class="sxs-lookup"><span data-stu-id="cfbec-390">`OnInitializedAsync` and `OnInitialized` execute code to initialize the component.</span></span> <span data-ttu-id="cfbec-391">Para realizar una operación asincrónica, use `OnInitializedAsync` y la palabra clave `await` en la operación:</span><span class="sxs-lookup"><span data-stu-id="cfbec-391">To perform an asynchronous operation, use `OnInitializedAsync` and the `await` keyword on the operation:</span></span>

```csharp
protected override async Task OnInitializedAsync()
{
    await ...
}
```

> [!NOTE]
> <span data-ttu-id="cfbec-392">El trabajo asincrónico durante la inicialización del componente debe producirse durante el evento del ciclo de vida `OnInitializedAsync`.</span><span class="sxs-lookup"><span data-stu-id="cfbec-392">Asynchronous work during component initialization must occur during the `OnInitializedAsync` lifecycle event.</span></span>

<span data-ttu-id="cfbec-393">En el caso de una operación sincrónica, utilice `OnInitialized`:</span><span class="sxs-lookup"><span data-stu-id="cfbec-393">For a synchronous operation, use `OnInitialized`:</span></span>

```csharp
protected override void OnInitialized()
{
    ...
}
```

<span data-ttu-id="cfbec-394">se llama a `OnParametersSetAsync` y `OnParametersSet` cuando un componente ha recibido parámetros de su elemento primario y los valores se asignan a las propiedades.</span><span class="sxs-lookup"><span data-stu-id="cfbec-394">`OnParametersSetAsync` and `OnParametersSet` are called when a component has received parameters from its parent and the values are assigned to properties.</span></span> <span data-ttu-id="cfbec-395">Estos métodos se ejecutan después de la inicialización del componente y cada vez que se representa el componente:</span><span class="sxs-lookup"><span data-stu-id="cfbec-395">These methods are executed after component initialization and each time the component is rendered:</span></span>

```csharp
protected override async Task OnParametersSetAsync()
{
    await ...
}
```

> [!NOTE]
> <span data-ttu-id="cfbec-396">El trabajo asincrónico al aplicar parámetros y valores de propiedad debe producirse durante el evento del ciclo de vida de `OnParametersSetAsync`.</span><span class="sxs-lookup"><span data-stu-id="cfbec-396">Asynchronous work when applying parameters and property values must occur during the `OnParametersSetAsync` lifecycle event.</span></span>

```csharp
protected override void OnParametersSet()
{
    ...
}
```

<span data-ttu-id="cfbec-397">se llama a `OnAfterRenderAsync` y `OnAfterRender` después de que un componente haya terminado de representarse.</span><span class="sxs-lookup"><span data-stu-id="cfbec-397">`OnAfterRenderAsync` and `OnAfterRender` are called after a component has finished rendering.</span></span> <span data-ttu-id="cfbec-398">En este punto se rellenan las referencias a elementos y componentes.</span><span class="sxs-lookup"><span data-stu-id="cfbec-398">Element and component references are populated at this point.</span></span> <span data-ttu-id="cfbec-399">Use esta fase para realizar pasos de inicialización adicionales mediante el contenido representado, como la activación de bibliotecas de JavaScript de terceros que operan en los elementos DOM representados.</span><span class="sxs-lookup"><span data-stu-id="cfbec-399">Use this stage to perform additional initialization steps using the rendered content, such as activating third-party JavaScript libraries that operate on the rendered DOM elements.</span></span>

<span data-ttu-id="cfbec-400">no se llama a `OnAfterRender` *al prerepresentar en el servidor.*</span><span class="sxs-lookup"><span data-stu-id="cfbec-400">`OnAfterRender` *isn't called when prerendering on the server.*</span></span>

<span data-ttu-id="cfbec-401">El parámetro `firstRender` de `OnAfterRenderAsync` y `OnAfterRender` es:</span><span class="sxs-lookup"><span data-stu-id="cfbec-401">The `firstRender` parameter for `OnAfterRenderAsync` and `OnAfterRender` is:</span></span>

* <span data-ttu-id="cfbec-402">Establezca en `true` la primera vez que se invoca la instancia de componente.</span><span class="sxs-lookup"><span data-stu-id="cfbec-402">Set to `true` the first time that the component instance is invoked.</span></span>
* <span data-ttu-id="cfbec-403">Garantiza que el trabajo de inicialización solo se realiza una vez.</span><span class="sxs-lookup"><span data-stu-id="cfbec-403">Ensures that initialization work is only performed once.</span></span>

```csharp
protected override async Task OnAfterRenderAsync(bool firstRender)
{
    if (firstRender)
    {
        await ...
    }
}
```

> [!NOTE]
> <span data-ttu-id="cfbec-404">El trabajo asincrónico inmediatamente después de la representación debe producirse durante el evento del ciclo de vida `OnAfterRenderAsync`.</span><span class="sxs-lookup"><span data-stu-id="cfbec-404">Asynchronous work immediately after rendering must occur during the `OnAfterRenderAsync` lifecycle event.</span></span>

```csharp
protected override void OnAfterRender(bool firstRender)
{
    if (firstRender)
    {
        ...
    }
}
```

### <a name="handle-incomplete-async-actions-at-render"></a><span data-ttu-id="cfbec-405">Controlar acciones asincrónicas incompletas en la representación</span><span class="sxs-lookup"><span data-stu-id="cfbec-405">Handle incomplete async actions at render</span></span>

<span data-ttu-id="cfbec-406">Es posible que las acciones asincrónicas realizadas en eventos del ciclo de vida no se hayan completado antes de que se represente el componente.</span><span class="sxs-lookup"><span data-stu-id="cfbec-406">Asynchronous actions performed in lifecycle events may not have completed before the component is rendered.</span></span> <span data-ttu-id="cfbec-407">Los objetos pueden ser `null` o rellenados incompletamente con datos mientras se ejecuta el método de ciclo de vida.</span><span class="sxs-lookup"><span data-stu-id="cfbec-407">Objects might be `null` or incompletely populated with data while the lifecycle method is executing.</span></span> <span data-ttu-id="cfbec-408">Proporcione lógica de representación para confirmar que los objetos se inicializan.</span><span class="sxs-lookup"><span data-stu-id="cfbec-408">Provide rendering logic to confirm that objects are initialized.</span></span> <span data-ttu-id="cfbec-409">Representar los elementos de la interfaz de usuario del marcador de posición (por ejemplo, un mensaje de carga) mientras que los objetos son `null`.</span><span class="sxs-lookup"><span data-stu-id="cfbec-409">Render placeholder UI elements (for example, a loading message) while objects are `null`.</span></span>

<span data-ttu-id="cfbec-410">En el componente `FetchData` de las plantillas extraordinarias, `OnInitializedAsync` se invalida para asincrónica recibir los datos de previsión (@no__t 2).</span><span class="sxs-lookup"><span data-stu-id="cfbec-410">In the `FetchData` component of the Blazor templates, `OnInitializedAsync` is overridden to asychronously receive forecast data (`forecasts`).</span></span> <span data-ttu-id="cfbec-411">Cuando `forecasts` es `null`, se muestra al usuario un mensaje de carga.</span><span class="sxs-lookup"><span data-stu-id="cfbec-411">When `forecasts` is `null`, a loading message is displayed to the user.</span></span> <span data-ttu-id="cfbec-412">Una vez finalizada la `Task` devuelta por `OnInitializedAsync`, el componente se representará con el estado actualizado.</span><span class="sxs-lookup"><span data-stu-id="cfbec-412">After the `Task` returned by `OnInitializedAsync` completes, the component is rerendered with the updated state.</span></span>

<span data-ttu-id="cfbec-413">*Pages/FetchData.razor*:</span><span class="sxs-lookup"><span data-stu-id="cfbec-413">*Pages/FetchData.razor*:</span></span>

[!code-cshtml[](components/samples_snapshot/3.x/FetchData.razor?highlight=9)]

### <a name="execute-code-before-parameters-are-set"></a><span data-ttu-id="cfbec-414">Ejecutar código antes de establecer los parámetros</span><span class="sxs-lookup"><span data-stu-id="cfbec-414">Execute code before parameters are set</span></span>

<span data-ttu-id="cfbec-415">`SetParameters` se puede invalidar para ejecutar el código antes de que se establezcan los parámetros:</span><span class="sxs-lookup"><span data-stu-id="cfbec-415">`SetParameters` can be overridden to execute code before parameters are set:</span></span>

```csharp
public override void SetParameters(ParameterView parameters)
{
    ...

    base.SetParameters(parameters);
}
```

<span data-ttu-id="cfbec-416">Si no se invoca `base.SetParameters`, el código personalizado puede interpretar el valor de los parámetros entrantes de cualquier manera que sea necesario.</span><span class="sxs-lookup"><span data-stu-id="cfbec-416">If `base.SetParameters` isn't invoked, the custom code can interpret the incoming parameters value in any way required.</span></span> <span data-ttu-id="cfbec-417">Por ejemplo, no es necesario asignar los parámetros de entrada a las propiedades de la clase.</span><span class="sxs-lookup"><span data-stu-id="cfbec-417">For example, the incoming parameters aren't required to be assigned to the properties on the class.</span></span>

### <a name="suppress-refreshing-of-the-ui"></a><span data-ttu-id="cfbec-418">Suprimir la actualización de la interfaz de usuario</span><span class="sxs-lookup"><span data-stu-id="cfbec-418">Suppress refreshing of the UI</span></span>

<span data-ttu-id="cfbec-419">`ShouldRender` se puede invalidar para suprimir la actualización de la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="cfbec-419">`ShouldRender` can be overridden to suppress refreshing of the UI.</span></span> <span data-ttu-id="cfbec-420">Si la implementación devuelve `true`, se actualiza la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="cfbec-420">If the implementation returns `true`, the UI is refreshed.</span></span> <span data-ttu-id="cfbec-421">Aunque se invalide `ShouldRender`, el componente siempre se representa inicialmente.</span><span class="sxs-lookup"><span data-stu-id="cfbec-421">Even if `ShouldRender` is overridden, the component is always initially rendered.</span></span>

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

## <a name="component-disposal-with-idisposable"></a><span data-ttu-id="cfbec-422">Eliminación de componentes con IDisposable</span><span class="sxs-lookup"><span data-stu-id="cfbec-422">Component disposal with IDisposable</span></span>

<span data-ttu-id="cfbec-423">Si un componente implementa <xref:System.IDisposable>, se llama al [método Dispose](/dotnet/standard/garbage-collection/implementing-dispose) cuando se quita el componente de la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="cfbec-423">If a component implements <xref:System.IDisposable>, the [Dispose method](/dotnet/standard/garbage-collection/implementing-dispose) is called when the component is removed from the UI.</span></span> <span data-ttu-id="cfbec-424">El siguiente componente usa `@implements IDisposable` y el método `Dispose`:</span><span class="sxs-lookup"><span data-stu-id="cfbec-424">The following component uses `@implements IDisposable` and the `Dispose` method:</span></span>

```csharp
@using System
@implements IDisposable

...

@code {
    public void Dispose()
    {
        ...
    }
}
```

## <a name="routing"></a><span data-ttu-id="cfbec-425">Enrutamiento</span><span class="sxs-lookup"><span data-stu-id="cfbec-425">Routing</span></span>

<span data-ttu-id="cfbec-426">El enrutamiento en extraordinaria se consigue proporcionando una plantilla de ruta a cada componente accesible en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="cfbec-426">Routing in Blazor is achieved by providing a route template to each accessible component in the app.</span></span>

<span data-ttu-id="cfbec-427">Cuando se compila un archivo de Razor con una directiva `@page`, a la clase generada se le asigna un <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> que especifica la plantilla de ruta.</span><span class="sxs-lookup"><span data-stu-id="cfbec-427">When a Razor file with an `@page` directive is compiled, the generated class is given a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span> <span data-ttu-id="cfbec-428">En tiempo de ejecución, el enrutador busca clases de componentes con un `RouteAttribute` y representa el componente que tiene una plantilla de ruta que coincide con la dirección URL solicitada.</span><span class="sxs-lookup"><span data-stu-id="cfbec-428">At runtime, the router looks for component classes with a `RouteAttribute` and renders whichever component has a route template that matches the requested URL.</span></span>

<span data-ttu-id="cfbec-429">Se pueden aplicar varias plantillas de ruta a un componente.</span><span class="sxs-lookup"><span data-stu-id="cfbec-429">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="cfbec-430">El siguiente componente responde a las solicitudes de `/BlazorRoute` y `/DifferentBlazorRoute`:</span><span class="sxs-lookup"><span data-stu-id="cfbec-430">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

## <a name="route-parameters"></a><span data-ttu-id="cfbec-431">Parámetros de ruta</span><span class="sxs-lookup"><span data-stu-id="cfbec-431">Route parameters</span></span>

<span data-ttu-id="cfbec-432">Los componentes pueden recibir parámetros de ruta de la plantilla de ruta proporcionada en la Directiva `@page`.</span><span class="sxs-lookup"><span data-stu-id="cfbec-432">Components can receive route parameters from the route template provided in the `@page` directive.</span></span> <span data-ttu-id="cfbec-433">El enrutador usa parámetros de ruta para rellenar los parámetros de componente correspondientes.</span><span class="sxs-lookup"><span data-stu-id="cfbec-433">The router uses route parameters to populate the corresponding component parameters.</span></span>

<span data-ttu-id="cfbec-434">*Componente de parámetro de ruta*:</span><span class="sxs-lookup"><span data-stu-id="cfbec-434">*Route Parameter component*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Pages/RouteParameter.razor?name=snippet_RouteParameter)]

<span data-ttu-id="cfbec-435">Los parámetros opcionales no se admiten, por lo que se aplican dos directivas `@page` en el ejemplo anterior.</span><span class="sxs-lookup"><span data-stu-id="cfbec-435">Optional parameters aren't supported, so two `@page` directives are applied in the example above.</span></span> <span data-ttu-id="cfbec-436">El primero permite la navegación al componente sin un parámetro.</span><span class="sxs-lookup"><span data-stu-id="cfbec-436">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="cfbec-437">La segunda Directiva `@page` toma el parámetro de ruta `{text}` y asigna el valor a la propiedad `Text`.</span><span class="sxs-lookup"><span data-stu-id="cfbec-437">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="base-class-inheritance-for-a-code-behind-experience"></a><span data-ttu-id="cfbec-438">Herencia de clase base para una experiencia de "código subyacente"</span><span class="sxs-lookup"><span data-stu-id="cfbec-438">Base class inheritance for a "code-behind" experience</span></span>

<span data-ttu-id="cfbec-439">Los archivos de componentes mezclan C# el marcado HTML y el código de procesamiento en el mismo archivo.</span><span class="sxs-lookup"><span data-stu-id="cfbec-439">Component files mix HTML markup and C# processing code in the same file.</span></span> <span data-ttu-id="cfbec-440">La Directiva `@inherits` se puede usar para proporcionar aplicaciones increíbles con una experiencia de "código subyacente" que separa el marcado de componentes del código de procesamiento.</span><span class="sxs-lookup"><span data-stu-id="cfbec-440">The `@inherits` directive can be used to provide Blazor apps with a "code-behind" experience that separates component markup from processing code.</span></span>

<span data-ttu-id="cfbec-441">La [aplicación de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) muestra cómo un componente puede heredar una clase base, `BlazorRocksBase`, para proporcionar las propiedades y los métodos del componente.</span><span class="sxs-lookup"><span data-stu-id="cfbec-441">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) shows how a component can inherit a base class, `BlazorRocksBase`, to provide the component's properties and methods.</span></span>

<span data-ttu-id="cfbec-442">*Pages/BlazorRocks. Razor*:</span><span class="sxs-lookup"><span data-stu-id="cfbec-442">*Pages/BlazorRocks.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Pages/BlazorRocks.razor?name=snippet_BlazorRocks)]

<span data-ttu-id="cfbec-443">*BlazorRocksBase.CS*:</span><span class="sxs-lookup"><span data-stu-id="cfbec-443">*BlazorRocksBase.cs*:</span></span>

[!code-csharp[](common/samples/3.x/BlazorWebAssemblySample/Pages/BlazorRocksBase.cs)]

<span data-ttu-id="cfbec-444">La clase base debe derivar de `ComponentBase`.</span><span class="sxs-lookup"><span data-stu-id="cfbec-444">The base class should derive from `ComponentBase`.</span></span>

## <a name="import-components"></a><span data-ttu-id="cfbec-445">Importar componentes</span><span class="sxs-lookup"><span data-stu-id="cfbec-445">Import components</span></span>

<span data-ttu-id="cfbec-446">El espacio de nombres de un componente creado con Razor se basa en (en orden de prioridad):</span><span class="sxs-lookup"><span data-stu-id="cfbec-446">The namespace of a component authored with Razor is based on (in priority order):</span></span>

* <span data-ttu-id="cfbec-447">designación de [@namespace](xref:mvc/views/razor#namespace) en el marcado de archivo Razor ( *. Razor*) (`@namespace BlazorSample.MyNamespace`).</span><span class="sxs-lookup"><span data-stu-id="cfbec-447">[@namespace](xref:mvc/views/razor#namespace) designation in Razor file (*.razor*) markup (`@namespace BlazorSample.MyNamespace`).</span></span>
* <span data-ttu-id="cfbec-448">Del proyecto `RootNamespace` en el archivo de proyecto (`<RootNamespace>BlazorSample</RootNamespace>`).</span><span class="sxs-lookup"><span data-stu-id="cfbec-448">The project's `RootNamespace` in the project file (`<RootNamespace>BlazorSample</RootNamespace>`).</span></span>
* <span data-ttu-id="cfbec-449">Nombre del proyecto, tomado del nombre de archivo del archivo de proyecto ( *. csproj*) y la ruta de acceso de la raíz del proyecto al componente.</span><span class="sxs-lookup"><span data-stu-id="cfbec-449">The project name, taken from the project file's file name (*.csproj*), and the path from the project root to the component.</span></span> <span data-ttu-id="cfbec-450">Por ejemplo, el marco de trabajo resuelve *{root Project}/pages/index.Razor* (*BlazorSample. csproj*) en el espacio de nombres `BlazorSample.Pages`.</span><span class="sxs-lookup"><span data-stu-id="cfbec-450">For example, the framework resolves *{PROJECT ROOT}/Pages/Index.razor* (*BlazorSample.csproj*) to the namespace `BlazorSample.Pages`.</span></span> <span data-ttu-id="cfbec-451">Los componentes C# siguen las reglas de enlace de nombres.</span><span class="sxs-lookup"><span data-stu-id="cfbec-451">Components follow C# name binding rules.</span></span> <span data-ttu-id="cfbec-452">En el caso del componente `Index` en este ejemplo, los componentes del ámbito son todos los componentes:</span><span class="sxs-lookup"><span data-stu-id="cfbec-452">For the `Index` component in this example, the components in scope are all of the components:</span></span>
  * <span data-ttu-id="cfbec-453">En la misma carpeta, *páginas*.</span><span class="sxs-lookup"><span data-stu-id="cfbec-453">In the same folder, *Pages*.</span></span>
  * <span data-ttu-id="cfbec-454">Los componentes de la raíz del proyecto que no especifican explícitamente un espacio de nombres diferente.</span><span class="sxs-lookup"><span data-stu-id="cfbec-454">The components in the project's root that don't explicitly specify a different namespace.</span></span>

<span data-ttu-id="cfbec-455">Los componentes definidos en un espacio de nombres diferente se incluyen en el ámbito mediante la directiva [@using](xref:mvc/views/razor#using) de Razor.</span><span class="sxs-lookup"><span data-stu-id="cfbec-455">Components defined in a different namespace are brought into scope using Razor's [@using](xref:mvc/views/razor#using) directive.</span></span>

<span data-ttu-id="cfbec-456">Si hay otro componente, `NavMenu.razor`, en la carpeta *BlazorSample/Shared/* , el componente se puede usar en `Index.razor` con la siguiente instrucción `@using`:</span><span class="sxs-lookup"><span data-stu-id="cfbec-456">If another component, `NavMenu.razor`, exists in the *BlazorSample/Shared/* folder, the component can be used in `Index.razor` with the following `@using` statement:</span></span>

```cshtml
@using BlazorSample.Shared

This is the Index page.

<NavMenu></NavMenu>
```

<span data-ttu-id="cfbec-457">También se puede hacer referencia a los componentes mediante sus nombres completos, que no requieren la directiva [@using](xref:mvc/views/razor#using) :</span><span class="sxs-lookup"><span data-stu-id="cfbec-457">Components can also be referenced using their fully qualified names, which doesn't require the [@using](xref:mvc/views/razor#using) directive:</span></span>

```cshtml
This is the Index page.

<BlazorSample.Shared.NavMenu></BlazorSample.Shared.NavMenu>
```

> [!NOTE]
> <span data-ttu-id="cfbec-458">No se admite la calificación `global::`.</span><span class="sxs-lookup"><span data-stu-id="cfbec-458">The `global::` qualification isn't supported.</span></span>
>
> <span data-ttu-id="cfbec-459">No se admite la importación de componentes con instrucciones `using` con alias (por ejemplo, `@using Foo = Bar`).</span><span class="sxs-lookup"><span data-stu-id="cfbec-459">Importing components with aliased `using` statements (for example, `@using Foo = Bar`) isn't supported.</span></span>
>
> <span data-ttu-id="cfbec-460">No se admiten nombres parcialmente completos.</span><span class="sxs-lookup"><span data-stu-id="cfbec-460">Partially qualified names aren't supported.</span></span> <span data-ttu-id="cfbec-461">Por ejemplo, no se admite agregar `@using BlazorSample` y hacer referencia a `NavMenu.razor` con `<Shared.NavMenu></Shared.NavMenu>`.</span><span class="sxs-lookup"><span data-stu-id="cfbec-461">For example, adding `@using BlazorSample` and referencing `NavMenu.razor` with `<Shared.NavMenu></Shared.NavMenu>` isn't supported.</span></span>

## <a name="conditional-html-element-attributes"></a><span data-ttu-id="cfbec-462">Atributos de elementos HTML condicionales</span><span class="sxs-lookup"><span data-stu-id="cfbec-462">Conditional HTML element attributes</span></span>

<span data-ttu-id="cfbec-463">Los atributos de elemento HTML se representan condicionalmente según el valor de .NET.</span><span class="sxs-lookup"><span data-stu-id="cfbec-463">HTML element attributes are conditionally rendered based on the .NET value.</span></span> <span data-ttu-id="cfbec-464">Si el valor es `false` o `null`, el atributo no se representa.</span><span class="sxs-lookup"><span data-stu-id="cfbec-464">If the value is `false` or `null`, the attribute isn't rendered.</span></span> <span data-ttu-id="cfbec-465">Si el valor es `true`, el atributo se representa minimizado.</span><span class="sxs-lookup"><span data-stu-id="cfbec-465">If the value is `true`, the attribute is rendered minimized.</span></span>

<span data-ttu-id="cfbec-466">En el ejemplo siguiente, `IsCompleted` determina si `checked` se representa en el marcado del elemento:</span><span class="sxs-lookup"><span data-stu-id="cfbec-466">In the following example, `IsCompleted` determines if `checked` is rendered in the element's markup:</span></span>

```cshtml
<input type="checkbox" checked="@IsCompleted" />

@code {
    [Parameter]
    public bool IsCompleted { get; set; }
}
```

<span data-ttu-id="cfbec-467">Si `IsCompleted` es `true`, la casilla se representa como:</span><span class="sxs-lookup"><span data-stu-id="cfbec-467">If `IsCompleted` is `true`, the check box is rendered as:</span></span>

```html
<input type="checkbox" checked />
```

<span data-ttu-id="cfbec-468">Si `IsCompleted` es `false`, la casilla se representa como:</span><span class="sxs-lookup"><span data-stu-id="cfbec-468">If `IsCompleted` is `false`, the check box is rendered as:</span></span>

```html
<input type="checkbox" />
```

<span data-ttu-id="cfbec-469">Para obtener más información, vea <xref:mvc/views/razor>.</span><span class="sxs-lookup"><span data-stu-id="cfbec-469">For more information, see <xref:mvc/views/razor>.</span></span>

> [!WARNING]
> <span data-ttu-id="cfbec-470">Algunos atributos HTML, como [Aria-pressed](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/button_role#Toggle_buttons), no funcionan correctamente cuando el tipo .net es un `bool`.</span><span class="sxs-lookup"><span data-stu-id="cfbec-470">Some HTML attributes, such as [aria-pressed](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/button_role#Toggle_buttons), don't function properly when the .NET type is a `bool`.</span></span> <span data-ttu-id="cfbec-471">En esos casos, use un tipo `string` en lugar de un `bool`.</span><span class="sxs-lookup"><span data-stu-id="cfbec-471">In those cases, use a `string` type instead of a `bool`.</span></span>

## <a name="raw-html"></a><span data-ttu-id="cfbec-472">HTML sin formato</span><span class="sxs-lookup"><span data-stu-id="cfbec-472">Raw HTML</span></span>

<span data-ttu-id="cfbec-473">Normalmente, las cadenas se representan mediante nodos de texto DOM, lo que significa que cualquier marcado que pueda contener se omite y se trata como texto literal.</span><span class="sxs-lookup"><span data-stu-id="cfbec-473">Strings are normally rendered using DOM text nodes, which means that any markup they may contain is ignored and treated as literal text.</span></span> <span data-ttu-id="cfbec-474">Para representar HTML sin formato, ajuste el contenido HTML en un valor `MarkupString`.</span><span class="sxs-lookup"><span data-stu-id="cfbec-474">To render raw HTML, wrap the HTML content in a `MarkupString` value.</span></span> <span data-ttu-id="cfbec-475">El valor se analiza como HTML o SVG y se inserta en el DOM.</span><span class="sxs-lookup"><span data-stu-id="cfbec-475">The value is parsed as HTML or SVG and inserted into the DOM.</span></span>

> [!WARNING]
> <span data-ttu-id="cfbec-476">La representación de HTML sin formato construido a partir de un origen que no es de confianza es un riesgo para la **seguridad** y debe evitarse.</span><span class="sxs-lookup"><span data-stu-id="cfbec-476">Rendering raw HTML constructed from any untrusted source is a **security risk** and should be avoided!</span></span>

<span data-ttu-id="cfbec-477">En el ejemplo siguiente se muestra cómo usar el tipo `MarkupString` para agregar un bloque de contenido HTML estático a la salida representada de un componente:</span><span class="sxs-lookup"><span data-stu-id="cfbec-477">The following example shows using the `MarkupString` type to add a block of static HTML content to the rendered output of a component:</span></span>

```html
@((MarkupString)myMarkup)

@code {
    private string myMarkup = 
        "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="templated-components"></a><span data-ttu-id="cfbec-478">Componentes con plantilla</span><span class="sxs-lookup"><span data-stu-id="cfbec-478">Templated components</span></span>

<span data-ttu-id="cfbec-479">Los componentes con plantilla son componentes que aceptan una o varias plantillas de interfaz de usuario como parámetros, que se pueden usar como parte de la lógica de representación del componente.</span><span class="sxs-lookup"><span data-stu-id="cfbec-479">Templated components are components that accept one or more UI templates as parameters, which can then be used as part of the component's rendering logic.</span></span> <span data-ttu-id="cfbec-480">Los componentes con plantilla permiten crear componentes de nivel superior que son más reutilizables que los componentes normales.</span><span class="sxs-lookup"><span data-stu-id="cfbec-480">Templated components allow you to author higher-level components that are more reusable than regular components.</span></span> <span data-ttu-id="cfbec-481">Algunos ejemplos son:</span><span class="sxs-lookup"><span data-stu-id="cfbec-481">A couple of examples include:</span></span>

* <span data-ttu-id="cfbec-482">Componente de tabla que permite a un usuario especificar plantillas para el encabezado, las filas y el pie de página de la tabla.</span><span class="sxs-lookup"><span data-stu-id="cfbec-482">A table component that allows a user to specify templates for the table's header, rows, and footer.</span></span>
* <span data-ttu-id="cfbec-483">Componente de lista que permite a un usuario especificar una plantilla para representar elementos en una lista.</span><span class="sxs-lookup"><span data-stu-id="cfbec-483">A list component that allows a user to specify a template for rendering items in a list.</span></span>

### <a name="template-parameters"></a><span data-ttu-id="cfbec-484">Parámetros de plantilla</span><span class="sxs-lookup"><span data-stu-id="cfbec-484">Template parameters</span></span>

<span data-ttu-id="cfbec-485">Un componente con plantilla se define especificando uno o más parámetros de componente de tipo `RenderFragment` o `RenderFragment<T>`.</span><span class="sxs-lookup"><span data-stu-id="cfbec-485">A templated component is defined by specifying one or more component parameters of type `RenderFragment` or `RenderFragment<T>`.</span></span> <span data-ttu-id="cfbec-486">Un fragmento de representación representa un segmento de interfaz de usuario que se va a representar.</span><span class="sxs-lookup"><span data-stu-id="cfbec-486">A render fragment represents a segment of UI to render.</span></span> <span data-ttu-id="cfbec-487">`RenderFragment<T>` toma un parámetro de tipo que se puede especificar cuando se invoca el fragmento de representación.</span><span class="sxs-lookup"><span data-stu-id="cfbec-487">`RenderFragment<T>` takes a type parameter that can be specified when the render fragment is invoked.</span></span>

<span data-ttu-id="cfbec-488">componente `TableTemplate`:</span><span class="sxs-lookup"><span data-stu-id="cfbec-488">`TableTemplate` component:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Components/TableTemplate.razor)]

<span data-ttu-id="cfbec-489">Cuando se usa un componente con plantilla, los parámetros de plantilla se pueden especificar utilizando los elementos secundarios que coinciden con los nombres de los parámetros (`TableHeader` y `RowTemplate` en el ejemplo siguiente):</span><span class="sxs-lookup"><span data-stu-id="cfbec-489">When using a templated component, the template parameters can be specified using child elements that match the names of the parameters (`TableHeader` and `RowTemplate` in the following example):</span></span>

```cshtml
<TableTemplate Items="pets">
    <TableHeader>
        <th>ID</th>
        <th>Name</th>
    </TableHeader>
    <RowTemplate>
        <td>@context.PetId</td>
        <td>@context.Name</td>
    </RowTemplate>
</TableTemplate>
```

### <a name="template-context-parameters"></a><span data-ttu-id="cfbec-490">Parámetros de contexto de plantilla</span><span class="sxs-lookup"><span data-stu-id="cfbec-490">Template context parameters</span></span>

<span data-ttu-id="cfbec-491">Los argumentos de los componentes de tipo `RenderFragment<T>` que se pasan como elementos tienen un parámetro implícito denominado `context` (por ejemplo, en el ejemplo de código anterior, `@context.PetId`), pero puede cambiar el nombre del parámetro mediante el atributo `Context` en el elemento secundario.</span><span class="sxs-lookup"><span data-stu-id="cfbec-491">Component arguments of type `RenderFragment<T>` passed as elements have an implicit parameter named `context` (for example from the preceding code sample, `@context.PetId`), but you can change the parameter name using the `Context` attribute on the child element.</span></span> <span data-ttu-id="cfbec-492">En el ejemplo siguiente, el atributo `Context` del elemento `RowTemplate` especifica el parámetro `pet`:</span><span class="sxs-lookup"><span data-stu-id="cfbec-492">In the following example, the `RowTemplate` element's `Context` attribute specifies the `pet` parameter:</span></span>

```cshtml
<TableTemplate Items="pets">
    <TableHeader>
        <th>ID</th>
        <th>Name</th>
    </TableHeader>
    <RowTemplate Context="pet">
        <td>@pet.PetId</td>
        <td>@pet.Name</td>
    </RowTemplate>
</TableTemplate>
```

<span data-ttu-id="cfbec-493">También puede especificar el atributo `Context` en el elemento de componente.</span><span class="sxs-lookup"><span data-stu-id="cfbec-493">Alternatively, you can specify the `Context` attribute on the component element.</span></span> <span data-ttu-id="cfbec-494">El atributo `Context` especificado se aplica a todos los parámetros de plantilla especificados.</span><span class="sxs-lookup"><span data-stu-id="cfbec-494">The specified `Context` attribute applies to all specified template parameters.</span></span> <span data-ttu-id="cfbec-495">Esto puede ser útil si desea especificar el nombre del parámetro de contenido para el contenido secundario implícito (sin ningún elemento secundario de ajuste).</span><span class="sxs-lookup"><span data-stu-id="cfbec-495">This can be useful when you want to specify the content parameter name for implicit child content (without any wrapping child element).</span></span> <span data-ttu-id="cfbec-496">En el ejemplo siguiente, el atributo `Context` aparece en el elemento `TableTemplate` y se aplica a todos los parámetros de plantilla:</span><span class="sxs-lookup"><span data-stu-id="cfbec-496">In the following example, the `Context` attribute appears on the `TableTemplate` element and applies to all template parameters:</span></span>

```cshtml
<TableTemplate Items="pets" Context="pet">
    <TableHeader>
        <th>ID</th>
        <th>Name</th>
    </TableHeader>
    <RowTemplate>
        <td>@pet.PetId</td>
        <td>@pet.Name</td>
    </RowTemplate>
</TableTemplate>
```

### <a name="generic-typed-components"></a><span data-ttu-id="cfbec-497">Componentes de tipo genérico</span><span class="sxs-lookup"><span data-stu-id="cfbec-497">Generic-typed components</span></span>

<span data-ttu-id="cfbec-498">Los componentes con plantilla suelen tener tipos genéricos.</span><span class="sxs-lookup"><span data-stu-id="cfbec-498">Templated components are often generically typed.</span></span> <span data-ttu-id="cfbec-499">Por ejemplo, se puede usar un componente genérico `ListViewTemplate` para representar valores de `IEnumerable<T>`.</span><span class="sxs-lookup"><span data-stu-id="cfbec-499">For example, a generic `ListViewTemplate` component can be used to render `IEnumerable<T>` values.</span></span> <span data-ttu-id="cfbec-500">Para definir un componente genérico, utilice la directiva [@typeparam](xref:mvc/views/razor#typeparam) para especificar parámetros de tipo:</span><span class="sxs-lookup"><span data-stu-id="cfbec-500">To define a generic component, use the [@typeparam](xref:mvc/views/razor#typeparam) directive to specify type parameters:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Components/ListViewTemplate.razor)]

<span data-ttu-id="cfbec-501">Cuando se usan componentes de tipo genérico, el parámetro de tipo se infiere si es posible:</span><span class="sxs-lookup"><span data-stu-id="cfbec-501">When using generic-typed components, the type parameter is inferred if possible:</span></span>

```cshtml
<ListViewTemplate Items="pets">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

<span data-ttu-id="cfbec-502">De lo contrario, el parámetro de tipo se debe especificar explícitamente mediante un atributo que coincida con el nombre del parámetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="cfbec-502">Otherwise, the type parameter must be explicitly specified using an attribute that matches the name of the type parameter.</span></span> <span data-ttu-id="cfbec-503">En el ejemplo siguiente, `TItem="Pet"` especifica el tipo:</span><span class="sxs-lookup"><span data-stu-id="cfbec-503">In the following example, `TItem="Pet"` specifies the type:</span></span>

```cshtml
<ListViewTemplate Items="pets" TItem="Pet">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

## <a name="cascading-values-and-parameters"></a><span data-ttu-id="cfbec-504">Valores y parámetros en cascada</span><span class="sxs-lookup"><span data-stu-id="cfbec-504">Cascading values and parameters</span></span>

<span data-ttu-id="cfbec-505">En algunos escenarios, no es conveniente fluir los datos de un componente antecesor a un componente descendiente mediante [parámetros de componente](#component-parameters), especialmente cuando hay varios niveles de componentes.</span><span class="sxs-lookup"><span data-stu-id="cfbec-505">In some scenarios, it's inconvenient to flow data from an ancestor component to a descendent component using [component parameters](#component-parameters), especially when there are several component layers.</span></span> <span data-ttu-id="cfbec-506">Los valores y parámetros en cascada solucionan este problema proporcionando un método cómodo para que un componente antecesor proporcione un valor a todos sus componentes descendientes.</span><span class="sxs-lookup"><span data-stu-id="cfbec-506">Cascading values and parameters solve this problem by providing a convenient way for an ancestor component to provide a value to all of its descendent components.</span></span> <span data-ttu-id="cfbec-507">Los valores y parámetros en cascada también proporcionan un enfoque para que los componentes se coordinen.</span><span class="sxs-lookup"><span data-stu-id="cfbec-507">Cascading values and parameters also provide an approach for components to coordinate.</span></span>

### <a name="theme-example"></a><span data-ttu-id="cfbec-508">Ejemplo de tema</span><span class="sxs-lookup"><span data-stu-id="cfbec-508">Theme example</span></span>

<span data-ttu-id="cfbec-509">En el ejemplo siguiente de la aplicación de ejemplo, la clase `ThemeInfo` especifica la información del tema que va a fluir hacia abajo en la jerarquía de componentes para que todos los botones de una parte determinada de la aplicación compartan el mismo estilo.</span><span class="sxs-lookup"><span data-stu-id="cfbec-509">In the following example from the sample app, the `ThemeInfo` class specifies the theme information to flow down the component hierarchy so that all of the buttons within a given part of the app share the same style.</span></span>

<span data-ttu-id="cfbec-510">*UIThemeClasses/ThemeInfo. CS*:</span><span class="sxs-lookup"><span data-stu-id="cfbec-510">*UIThemeClasses/ThemeInfo.cs*:</span></span>

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

<span data-ttu-id="cfbec-511">Un componente antecesor puede proporcionar un valor en cascada mediante el componente de valor en cascada.</span><span class="sxs-lookup"><span data-stu-id="cfbec-511">An ancestor component can provide a cascading value using the Cascading Value component.</span></span> <span data-ttu-id="cfbec-512">El componente `CascadingValue` encapsula un subárbol de la jerarquía de componentes y proporciona un valor único a todos los componentes de ese subárbol.</span><span class="sxs-lookup"><span data-stu-id="cfbec-512">The `CascadingValue` component wraps a subtree of the component hierarchy and supplies a single value to all components within that subtree.</span></span>

<span data-ttu-id="cfbec-513">Por ejemplo, la aplicación de ejemplo especifica información de tema (`ThemeInfo`) en uno de los diseños de la aplicación como un parámetro en cascada para todos los componentes que constituyen el cuerpo del diseño de la propiedad `@Body`.</span><span class="sxs-lookup"><span data-stu-id="cfbec-513">For example, the sample app specifies theme information (`ThemeInfo`) in one of the app's layouts as a cascading parameter for all components that make up the layout body of the `@Body` property.</span></span> <span data-ttu-id="cfbec-514">`ButtonClass` tiene asignado un valor de `btn-success` en el componente de diseño.</span><span class="sxs-lookup"><span data-stu-id="cfbec-514">`ButtonClass` is assigned a value of `btn-success` in the layout component.</span></span> <span data-ttu-id="cfbec-515">Cualquier componente descendiente puede consumir esta propiedad a través del objeto en cascada `ThemeInfo`.</span><span class="sxs-lookup"><span data-stu-id="cfbec-515">Any descendent component can consume this property through the `ThemeInfo` cascading object.</span></span>

<span data-ttu-id="cfbec-516">componente `CascadingValuesParametersLayout`:</span><span class="sxs-lookup"><span data-stu-id="cfbec-516">`CascadingValuesParametersLayout` component:</span></span>

```cshtml
@inherits LayoutComponentBase
@using BlazorSample.UIThemeClasses

<div class="container-fluid">
    <div class="row">
        <div class="col-sm-3">
            <NavMenu />
        </div>
        <div class="col-sm-9">
            <CascadingValue Value="theme">
                <div class="content px-4">
                    @Body
                </div>
            </CascadingValue>
        </div>
    </div>
</div>

@code {
    private ThemeInfo theme = new ThemeInfo { ButtonClass = "btn-success" };
}
```

<span data-ttu-id="cfbec-517">Para usar valores en cascada, los componentes declaran parámetros en cascada mediante el atributo `[CascadingParameter]`.</span><span class="sxs-lookup"><span data-stu-id="cfbec-517">To make use of cascading values, components declare cascading parameters using the `[CascadingParameter]` attribute.</span></span> <span data-ttu-id="cfbec-518">Los valores en cascada se enlazan a los parámetros en cascada por tipo.</span><span class="sxs-lookup"><span data-stu-id="cfbec-518">Cascading values are bound to cascading parameters by type.</span></span>

<span data-ttu-id="cfbec-519">En la aplicación de ejemplo, el componente `CascadingValuesParametersTheme` enlaza el valor en cascada `ThemeInfo` a un parámetro en cascada.</span><span class="sxs-lookup"><span data-stu-id="cfbec-519">In the sample app, the `CascadingValuesParametersTheme` component binds the `ThemeInfo` cascading value to a cascading parameter.</span></span> <span data-ttu-id="cfbec-520">El parámetro se usa para establecer la clase CSS para uno de los botones mostrados por el componente.</span><span class="sxs-lookup"><span data-stu-id="cfbec-520">The parameter is used to set the CSS class for one of the buttons displayed by the component.</span></span>

<span data-ttu-id="cfbec-521">componente `CascadingValuesParametersTheme`:</span><span class="sxs-lookup"><span data-stu-id="cfbec-521">`CascadingValuesParametersTheme` component:</span></span>

```cshtml
@page "/cascadingvaluesparameterstheme"
@layout CascadingValuesParametersLayout
@using BlazorSample.UIThemeClasses

<h1>Cascading Values & Parameters</h1>

<p>Current count: @currentCount</p>

<p>
    <button class="btn" @onclick="IncrementCount">
        Increment Counter (Unthemed)
    </button>
</p>

<p>
    <button class="btn @ThemeInfo.ButtonClass" @onclick="IncrementCount">
        Increment Counter (Themed)
    </button>
</p>

@code {
    private int currentCount = 0;

    [CascadingParameter]
    protected ThemeInfo ThemeInfo { get; set; }

    private void IncrementCount()
    {
        currentCount++;
    }
}
```

<span data-ttu-id="cfbec-522">Para poner en cascada varios valores del mismo tipo en el mismo subárbol, proporcione una cadena única `Name` para cada componente `CascadingValue` y su @no__t 2 correspondiente.</span><span class="sxs-lookup"><span data-stu-id="cfbec-522">To cascade multiple values of the same type within the same subtree, provide a unique `Name` string to each `CascadingValue` component and its corresponding `CascadingParameter`.</span></span> <span data-ttu-id="cfbec-523">En el ejemplo siguiente, dos componentes `CascadingValue` colocan en cascada diferentes instancias de `MyCascadingType` por nombre:</span><span class="sxs-lookup"><span data-stu-id="cfbec-523">In the following example, two `CascadingValue` components cascade different instances of `MyCascadingType` by name:</span></span>

```cshtml
<CascadingValue Value=@ParentCascadeParameter1 Name="CascadeParam1">
    <CascadingValue Value=@ParentCascadeParameter2 Name="CascadeParam2">
        ...
    </CascadingValue>
</CascadingValue>

@code {
    private MyCascadingType ParentCascadeParameter1;

    [Parameter]
    public MyCascadingType ParentCascadeParameter2 { get; set; }

    ...
}
```

<span data-ttu-id="cfbec-524">En un componente descendiente, los parámetros en cascada reciben sus valores de los valores en cascada correspondientes del componente antecesor por nombre:</span><span class="sxs-lookup"><span data-stu-id="cfbec-524">In a descendant component, the cascaded parameters receive their values from the corresponding cascaded values in the ancestor component by name:</span></span>

```cshtml
...

@code {
    [CascadingParameter(Name = "CascadeParam1")]
    protected MyCascadingType ChildCascadeParameter1 { get; set; }
    
    [CascadingParameter(Name = "CascadeParam2")]
    protected MyCascadingType ChildCascadeParameter2 { get; set; }
}
```

### <a name="tabset-example"></a><span data-ttu-id="cfbec-525">Ejemplo de TabSet</span><span class="sxs-lookup"><span data-stu-id="cfbec-525">TabSet example</span></span>

<span data-ttu-id="cfbec-526">Los parámetros en cascada también permiten que los componentes colaboren en la jerarquía de componentes.</span><span class="sxs-lookup"><span data-stu-id="cfbec-526">Cascading parameters also enable components to collaborate across the component hierarchy.</span></span> <span data-ttu-id="cfbec-527">Por ejemplo, considere el siguiente ejemplo de *TabSet* en la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="cfbec-527">For example, consider the following *TabSet* example in the sample app.</span></span>

<span data-ttu-id="cfbec-528">La aplicación de ejemplo tiene una interfaz `ITab` que implementan las pestañas:</span><span class="sxs-lookup"><span data-stu-id="cfbec-528">The sample app has an `ITab` interface that tabs implement:</span></span>

[!code-csharp[](common/samples/3.x/BlazorWebAssemblySample/UIInterfaces/ITab.cs)]

<span data-ttu-id="cfbec-529">El componente `CascadingValuesParametersTabSet` usa el componente `TabSet`, que contiene varios componentes `Tab`:</span><span class="sxs-lookup"><span data-stu-id="cfbec-529">The `CascadingValuesParametersTabSet` component uses the `TabSet` component, which contains several `Tab` components:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Pages/CascadingValuesParametersTabSet.razor?name=snippet_TabSet)]

<span data-ttu-id="cfbec-530">Los componentes secundarios `Tab` no se pasan explícitamente como parámetros a la `TabSet`.</span><span class="sxs-lookup"><span data-stu-id="cfbec-530">The child `Tab` components aren't explicitly passed as parameters to the `TabSet`.</span></span> <span data-ttu-id="cfbec-531">En su lugar, los componentes secundarios `Tab` forman parte del contenido secundario del `TabSet`.</span><span class="sxs-lookup"><span data-stu-id="cfbec-531">Instead, the child `Tab` components are part of the child content of the `TabSet`.</span></span> <span data-ttu-id="cfbec-532">Sin embargo, el `TabSet` todavía necesita saber sobre cada componente `Tab` para que pueda representar los encabezados y la pestaña activa. Para habilitar esta coordinación sin necesidad de código adicional, el componente @no__t 2 *puede proporcionarse como un valor en cascada* que los componentes descendientes de @no__t 4 recogen a continuación.</span><span class="sxs-lookup"><span data-stu-id="cfbec-532">However, the `TabSet` still needs to know about each `Tab` component so that it can render the headers and the active tab. To enable this coordination without requiring additional code, the `TabSet` component *can provide itself as a cascading value* that is then picked up by the descendent `Tab` components.</span></span>

<span data-ttu-id="cfbec-533">componente `TabSet`:</span><span class="sxs-lookup"><span data-stu-id="cfbec-533">`TabSet` component:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Components/TabSet.razor)]

<span data-ttu-id="cfbec-534">Los componentes descendientes de `Tab` capturan el que contiene `TabSet` como parámetro en cascada, por lo que los componentes @no__t 2 se agregan a los `TabSet` y a la coordenada en la que la pestaña está activa.</span><span class="sxs-lookup"><span data-stu-id="cfbec-534">The descendent `Tab` components capture the containing `TabSet` as a cascading parameter, so the `Tab` components add themselves to the `TabSet` and coordinate on which tab is active.</span></span>

<span data-ttu-id="cfbec-535">componente `Tab`:</span><span class="sxs-lookup"><span data-stu-id="cfbec-535">`Tab` component:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Components/Tab.razor)]

## <a name="razor-templates"></a><span data-ttu-id="cfbec-536">Plantillas de Razor</span><span class="sxs-lookup"><span data-stu-id="cfbec-536">Razor templates</span></span>

<span data-ttu-id="cfbec-537">Los fragmentos de representación se pueden definir mediante la sintaxis de plantilla de Razor.</span><span class="sxs-lookup"><span data-stu-id="cfbec-537">Render fragments can be defined using Razor template syntax.</span></span> <span data-ttu-id="cfbec-538">Las plantillas de Razor son una manera de definir un fragmento de la interfaz de usuario y suponer el siguiente formato:</span><span class="sxs-lookup"><span data-stu-id="cfbec-538">Razor templates are a way to define a UI snippet and assume the following format:</span></span>

```cshtml
@<{HTML tag}>...</{HTML tag}>
```

<span data-ttu-id="cfbec-539">En el ejemplo siguiente se muestra cómo especificar los valores `RenderFragment` y `RenderFragment<T>` y las plantillas de representación directamente en un componente.</span><span class="sxs-lookup"><span data-stu-id="cfbec-539">The following example illustrates how to specify `RenderFragment` and `RenderFragment<T>` values and render templates directly in a component.</span></span> <span data-ttu-id="cfbec-540">Los fragmentos de representación también se pueden pasar como argumentos a [componentes con plantilla](#templated-components).</span><span class="sxs-lookup"><span data-stu-id="cfbec-540">Render fragments can also be passed as arguments to [templated components](#templated-components).</span></span>

```cshtml
@timeTemplate

@petTemplate(new Pet { Name = "Rex" })

@code {
    private RenderFragment timeTemplate = @<p>The time is @DateTime.Now.</p>;
    private RenderFragment<Pet> petTemplate = 
        (pet) => @<p>Your pet's name is @pet.Name.</p>;

    private class Pet
    {
        public string Name { get; set; }
    }
}
```

<span data-ttu-id="cfbec-541">Salida representada del código anterior:</span><span class="sxs-lookup"><span data-stu-id="cfbec-541">Rendered output of the preceding code:</span></span>

```html
<p>The time is 10/04/2018 01:26:52.</p>

<p>Your pet's name is Rex.</p>
```

## <a name="manual-rendertreebuilder-logic"></a><span data-ttu-id="cfbec-542">Lógica de RenderTreeBuilder manual</span><span class="sxs-lookup"><span data-stu-id="cfbec-542">Manual RenderTreeBuilder logic</span></span>

<span data-ttu-id="cfbec-543">`Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder` proporciona métodos para manipular componentes y elementos, incluida la compilación manual de componentes C# en el código.</span><span class="sxs-lookup"><span data-stu-id="cfbec-543">`Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder` provides methods for manipulating components and elements, including building components manually in C# code.</span></span>

> [!NOTE]
> <span data-ttu-id="cfbec-544">El uso de `RenderTreeBuilder` para crear componentes es un escenario avanzado.</span><span class="sxs-lookup"><span data-stu-id="cfbec-544">Use of `RenderTreeBuilder` to create components is an advanced scenario.</span></span> <span data-ttu-id="cfbec-545">Un componente con formato incorrecto (por ejemplo, una etiqueta de marcado sin cerrar) puede dar lugar a un comportamiento indefinido.</span><span class="sxs-lookup"><span data-stu-id="cfbec-545">A malformed component (for example, an unclosed markup tag) can result in undefined behavior.</span></span>

<span data-ttu-id="cfbec-546">Tenga en cuenta el siguiente componente `PetDetails`, que se puede integrar manualmente en otro componente:</span><span class="sxs-lookup"><span data-stu-id="cfbec-546">Consider the following `PetDetails` component, which can be manually built into another component:</span></span>

```cshtml
<h2>Pet Details Component</h2>

<p>@PetDetailsQuote</p>

@code
{
    [Parameter]
    public string PetDetailsQuote { get; set; }
}
```

<span data-ttu-id="cfbec-547">En el ejemplo siguiente, el bucle del método `CreateComponent` genera tres componentes `PetDetails`.</span><span class="sxs-lookup"><span data-stu-id="cfbec-547">In the following example, the loop in the `CreateComponent` method generates three `PetDetails` components.</span></span> <span data-ttu-id="cfbec-548">Cuando se llama a métodos `RenderTreeBuilder` para crear los componentes (`OpenComponent` y `AddAttribute`), los números de secuencia son números de línea de código fuente.</span><span class="sxs-lookup"><span data-stu-id="cfbec-548">When calling `RenderTreeBuilder` methods to create the components (`OpenComponent` and `AddAttribute`), sequence numbers are source code line numbers.</span></span> <span data-ttu-id="cfbec-549">El algoritmo de diferencia más increíble se basa en los números de secuencia correspondientes a líneas de código distintas, no a invocaciones de llamada distintas.</span><span class="sxs-lookup"><span data-stu-id="cfbec-549">The Blazor difference algorithm relies on the sequence numbers corresponding to distinct lines of code, not distinct call invocations.</span></span> <span data-ttu-id="cfbec-550">Al crear un componente con métodos `RenderTreeBuilder`, codifique los argumentos para los números de secuencia.</span><span class="sxs-lookup"><span data-stu-id="cfbec-550">When creating a component with `RenderTreeBuilder` methods, hardcode the arguments for sequence numbers.</span></span> <span data-ttu-id="cfbec-551">**El uso de un cálculo o un contador para generar el número de secuencia puede dar lugar a un rendimiento deficiente.**</span><span class="sxs-lookup"><span data-stu-id="cfbec-551">**Using a calculation or counter to generate the sequence number can lead to poor performance.**</span></span> <span data-ttu-id="cfbec-552">Para obtener más información, vea la sección [números de secuencia relacionados con números de línea de código y no orden de ejecución](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) .</span><span class="sxs-lookup"><span data-stu-id="cfbec-552">For more information, see the [Sequence numbers relate to code line numbers and not execution order](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) section.</span></span>

<span data-ttu-id="cfbec-553">componente `BuiltContent`:</span><span class="sxs-lookup"><span data-stu-id="cfbec-553">`BuiltContent` component:</span></span>

```cshtml
@page "/BuiltContent"

<h1>Build a component</h1>

@CustomRender

<button type="button" @onclick="RenderComponent">
    Create three Pet Details components
</button>

@code {
    private RenderFragment CustomRender { get; set; }
    
    private RenderFragment CreateComponent() => builder =>
    {
        for (var i = 0; i < 3; i++) 
        {
            builder.OpenComponent(0, typeof(PetDetails));
            builder.AddAttribute(1, "PetDetailsQuote", "Someone's best friend!");
            builder.CloseComponent();
        }
    };    
    
    private void RenderComponent()
    {
        CustomRender = CreateComponent();
    }
}
```

> <span data-ttu-id="cfbec-554">! ATENCIÓN Los tipos de `Microsoft.AspNetCore.Components.RenderTree` permiten el procesamiento de los *resultados* de las operaciones de representación.</span><span class="sxs-lookup"><span data-stu-id="cfbec-554">![WARNING] The types in `Microsoft.AspNetCore.Components.RenderTree` allow processing of the *results* of rendering operations.</span></span> <span data-ttu-id="cfbec-555">Estos son los detalles internos de la implementación de la plataforma más extraordinaria.</span><span class="sxs-lookup"><span data-stu-id="cfbec-555">These are internal details of the Blazor framework implementation.</span></span> <span data-ttu-id="cfbec-556">Estos tipos se deben considerar *inestables* y estar sujetos a cambios en futuras versiones.</span><span class="sxs-lookup"><span data-stu-id="cfbec-556">These types should be considered *unstable* and subject to change in future releases.</span></span>

### <a name="sequence-numbers-relate-to-code-line-numbers-and-not-execution-order"></a><span data-ttu-id="cfbec-557">Los números de secuencia se relacionan con los números de línea de código y no el orden de ejecución</span><span class="sxs-lookup"><span data-stu-id="cfbec-557">Sequence numbers relate to code line numbers and not execution order</span></span>

<span data-ttu-id="cfbec-558">Los archivos increíblemente `.razor` siempre se compilan.</span><span class="sxs-lookup"><span data-stu-id="cfbec-558">Blazor `.razor` files are always compiled.</span></span> <span data-ttu-id="cfbec-559">Esta es potencialmente una gran ventaja para `.razor` porque el paso de compilación se puede usar para insertar información que mejora el rendimiento de la aplicación en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="cfbec-559">This is potentially a great advantage for `.razor` because the compile step can be used to inject information that improve app performance at runtime.</span></span>

<span data-ttu-id="cfbec-560">Un ejemplo clave de estas mejoras implican *los números de secuencia*.</span><span class="sxs-lookup"><span data-stu-id="cfbec-560">A key example of these improvements involve *sequence numbers*.</span></span> <span data-ttu-id="cfbec-561">Los números de secuencia indican al tiempo de ejecución los resultados de los que proceden las líneas de código distintas y ordenadas.</span><span class="sxs-lookup"><span data-stu-id="cfbec-561">Sequence numbers indicate to the runtime which outputs came from which distinct and ordered lines of code.</span></span> <span data-ttu-id="cfbec-562">El motor en tiempo de ejecución utiliza esta información para generar diferencias de árbol eficientes en el tiempo lineal, que es mucho más rápido de lo que suele ser posible para un algoritmo de comparación de árboles generales.</span><span class="sxs-lookup"><span data-stu-id="cfbec-562">The runtime uses this information to generate efficient tree diffs in linear time, which is far faster than is normally possible for a general tree diff algorithm.</span></span>

<span data-ttu-id="cfbec-563">Considere el siguiente archivo simple `.razor`:</span><span class="sxs-lookup"><span data-stu-id="cfbec-563">Consider the following simple `.razor` file:</span></span>

```cshtml
@if (someFlag)
{
    <text>First</text>
}

Second
```

<span data-ttu-id="cfbec-564">El código anterior se compila en algo similar a lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="cfbec-564">The preceding code compiles to something like the following:</span></span>

```csharp
if (someFlag)
{
    builder.AddContent(0, "First");
}

builder.AddContent(1, "Second");
```

<span data-ttu-id="cfbec-565">Cuando el código se ejecuta por primera vez, si `someFlag` es `true`, el generador recibe:</span><span class="sxs-lookup"><span data-stu-id="cfbec-565">When the code executes for the first time, if `someFlag` is `true`, the builder receives:</span></span>

| <span data-ttu-id="cfbec-566">Secuencia</span><span class="sxs-lookup"><span data-stu-id="cfbec-566">Sequence</span></span> | <span data-ttu-id="cfbec-567">Type</span><span class="sxs-lookup"><span data-stu-id="cfbec-567">Type</span></span>      | <span data-ttu-id="cfbec-568">Datos</span><span class="sxs-lookup"><span data-stu-id="cfbec-568">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="cfbec-569">0</span><span class="sxs-lookup"><span data-stu-id="cfbec-569">0</span></span>        | <span data-ttu-id="cfbec-570">Nodo de texto</span><span class="sxs-lookup"><span data-stu-id="cfbec-570">Text node</span></span> | <span data-ttu-id="cfbec-571">First</span><span class="sxs-lookup"><span data-stu-id="cfbec-571">First</span></span>  |
| <span data-ttu-id="cfbec-572">1</span><span class="sxs-lookup"><span data-stu-id="cfbec-572">1</span></span>        | <span data-ttu-id="cfbec-573">Nodo de texto</span><span class="sxs-lookup"><span data-stu-id="cfbec-573">Text node</span></span> | <span data-ttu-id="cfbec-574">Second</span><span class="sxs-lookup"><span data-stu-id="cfbec-574">Second</span></span> |

<span data-ttu-id="cfbec-575">Imagine que `someFlag` se convierte en `false` y que el marcado se representará de nuevo.</span><span class="sxs-lookup"><span data-stu-id="cfbec-575">Imagine that `someFlag` becomes `false`, and the markup is rendered again.</span></span> <span data-ttu-id="cfbec-576">Esta vez, el generador recibe:</span><span class="sxs-lookup"><span data-stu-id="cfbec-576">This time, the builder receives:</span></span>

| <span data-ttu-id="cfbec-577">Secuencia</span><span class="sxs-lookup"><span data-stu-id="cfbec-577">Sequence</span></span> | <span data-ttu-id="cfbec-578">Type</span><span class="sxs-lookup"><span data-stu-id="cfbec-578">Type</span></span>       | <span data-ttu-id="cfbec-579">Datos</span><span class="sxs-lookup"><span data-stu-id="cfbec-579">Data</span></span>   |
| :------: | ---------- | :----: |
| <span data-ttu-id="cfbec-580">1</span><span class="sxs-lookup"><span data-stu-id="cfbec-580">1</span></span>        | <span data-ttu-id="cfbec-581">Nodo de texto</span><span class="sxs-lookup"><span data-stu-id="cfbec-581">Text node</span></span>  | <span data-ttu-id="cfbec-582">Second</span><span class="sxs-lookup"><span data-stu-id="cfbec-582">Second</span></span> |

<span data-ttu-id="cfbec-583">Cuando el tiempo de ejecución realiza una comparación, observa que se quitó el elemento en la secuencia `0`, por lo que genera el siguiente *script de edición*trivial:</span><span class="sxs-lookup"><span data-stu-id="cfbec-583">When the runtime performs a diff, it sees that the item at sequence `0` was removed, so it generates the following trivial *edit script*:</span></span>

* <span data-ttu-id="cfbec-584">Quitar el primer nodo de texto.</span><span class="sxs-lookup"><span data-stu-id="cfbec-584">Remove the first text node.</span></span>

#### <a name="what-goes-wrong-if-you-generate-sequence-numbers-programmatically"></a><span data-ttu-id="cfbec-585">Qué sucede si se generan números de secuencia mediante programación</span><span class="sxs-lookup"><span data-stu-id="cfbec-585">What goes wrong if you generate sequence numbers programmatically</span></span>

<span data-ttu-id="cfbec-586">Imagine que escribió la siguiente lógica del generador de árboles de representación:</span><span class="sxs-lookup"><span data-stu-id="cfbec-586">Imagine instead that you wrote the following render tree builder logic:</span></span>

```csharp
var seq = 0;

if (someFlag)
{
    builder.AddContent(seq++, "First");
}

builder.AddContent(seq++, "Second");
```

<span data-ttu-id="cfbec-587">Ahora, el primer resultado es:</span><span class="sxs-lookup"><span data-stu-id="cfbec-587">Now, the first output is:</span></span>

| <span data-ttu-id="cfbec-588">Secuencia</span><span class="sxs-lookup"><span data-stu-id="cfbec-588">Sequence</span></span> | <span data-ttu-id="cfbec-589">Type</span><span class="sxs-lookup"><span data-stu-id="cfbec-589">Type</span></span>      | <span data-ttu-id="cfbec-590">Datos</span><span class="sxs-lookup"><span data-stu-id="cfbec-590">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="cfbec-591">0</span><span class="sxs-lookup"><span data-stu-id="cfbec-591">0</span></span>        | <span data-ttu-id="cfbec-592">Nodo de texto</span><span class="sxs-lookup"><span data-stu-id="cfbec-592">Text node</span></span> | <span data-ttu-id="cfbec-593">First</span><span class="sxs-lookup"><span data-stu-id="cfbec-593">First</span></span>  |
| <span data-ttu-id="cfbec-594">1</span><span class="sxs-lookup"><span data-stu-id="cfbec-594">1</span></span>        | <span data-ttu-id="cfbec-595">Nodo de texto</span><span class="sxs-lookup"><span data-stu-id="cfbec-595">Text node</span></span> | <span data-ttu-id="cfbec-596">Second</span><span class="sxs-lookup"><span data-stu-id="cfbec-596">Second</span></span> |

<span data-ttu-id="cfbec-597">Este resultado es idéntico al caso anterior, por lo que no existe ningún problema negativo.</span><span class="sxs-lookup"><span data-stu-id="cfbec-597">This outcome is identical to the prior case, so no negative issues exist.</span></span> <span data-ttu-id="cfbec-598">`someFlag` es `false` en la segunda representación y el resultado es:</span><span class="sxs-lookup"><span data-stu-id="cfbec-598">`someFlag` is `false` on the second rendering, and the output is:</span></span>

| <span data-ttu-id="cfbec-599">Secuencia</span><span class="sxs-lookup"><span data-stu-id="cfbec-599">Sequence</span></span> | <span data-ttu-id="cfbec-600">Type</span><span class="sxs-lookup"><span data-stu-id="cfbec-600">Type</span></span>      | <span data-ttu-id="cfbec-601">Datos</span><span class="sxs-lookup"><span data-stu-id="cfbec-601">Data</span></span>   |
| :------: | --------- | ------ |
| <span data-ttu-id="cfbec-602">0</span><span class="sxs-lookup"><span data-stu-id="cfbec-602">0</span></span>        | <span data-ttu-id="cfbec-603">Nodo de texto</span><span class="sxs-lookup"><span data-stu-id="cfbec-603">Text node</span></span> | <span data-ttu-id="cfbec-604">Second</span><span class="sxs-lookup"><span data-stu-id="cfbec-604">Second</span></span> |

<span data-ttu-id="cfbec-605">Esta vez, el algoritmo de comparación ve que se han producido *dos* cambios y el algoritmo genera el siguiente script de edición:</span><span class="sxs-lookup"><span data-stu-id="cfbec-605">This time, the diff algorithm sees that *two* changes have occurred, and the algorithm generates the following edit script:</span></span>

* <span data-ttu-id="cfbec-606">Cambie el valor del primer nodo de texto a `Second`.</span><span class="sxs-lookup"><span data-stu-id="cfbec-606">Change the value of the first text node to `Second`.</span></span>
* <span data-ttu-id="cfbec-607">Quite el segundo nodo de texto.</span><span class="sxs-lookup"><span data-stu-id="cfbec-607">Remove the second text node.</span></span>

<span data-ttu-id="cfbec-608">La generación de los números de secuencia ha perdido toda la información útil sobre el lugar en el que estaban presentes las ramas y los bucles de `if/else` en el código original.</span><span class="sxs-lookup"><span data-stu-id="cfbec-608">Generating the sequence numbers has lost all the useful information about where the `if/else` branches and loops were present in the original code.</span></span> <span data-ttu-id="cfbec-609">Esto da como resultado una diferencia **dos veces mayor que** antes.</span><span class="sxs-lookup"><span data-stu-id="cfbec-609">This results in a diff **twice as long** as before.</span></span>

<span data-ttu-id="cfbec-610">Este es un ejemplo trivial.</span><span class="sxs-lookup"><span data-stu-id="cfbec-610">This is a trivial example.</span></span> <span data-ttu-id="cfbec-611">En casos más realistas con estructuras complejas y profundamente anidadas, y especialmente con bucles, el costo de rendimiento es más grave.</span><span class="sxs-lookup"><span data-stu-id="cfbec-611">In more realistic cases with complex and deeply nested structures, and especially with loops, the performance cost is more severe.</span></span> <span data-ttu-id="cfbec-612">En lugar de identificar inmediatamente qué bloques o ramas de bucle se han insertado o quitado, el algoritmo de comparación tiene que recurse profundamente en los árboles de representación y, normalmente, compilar scripts de edición mucho más largos porque está informada sobre cómo las estructuras antiguas y nuevas se relacionan entre sí.</span><span class="sxs-lookup"><span data-stu-id="cfbec-612">Instead of immediately identifying which loop blocks or branches have been inserted or removed, the diff algorithm has to recurse deeply into the render trees and usually build far longer edit scripts because it is misinformed about how the old and new structures relate to each other.</span></span>

#### <a name="guidance-and-conclusions"></a><span data-ttu-id="cfbec-613">Instrucciones y conclusiones</span><span class="sxs-lookup"><span data-stu-id="cfbec-613">Guidance and conclusions</span></span>

* <span data-ttu-id="cfbec-614">El rendimiento de la aplicación se ve afectado si los números de secuencia se generan dinámicamente.</span><span class="sxs-lookup"><span data-stu-id="cfbec-614">App performance suffers if sequence numbers are generated dynamically.</span></span>
* <span data-ttu-id="cfbec-615">El marco no puede crear sus propios números de secuencia automáticamente en tiempo de ejecución porque la información necesaria no existe a menos que se Capture en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="cfbec-615">The framework can't create its own sequence numbers automatically at runtime because the necessary information doesn't exist unless it's captured at compile time.</span></span>
* <span data-ttu-id="cfbec-616">No escriba grandes bloques de lógica de `RenderTreeBuilder` implementada de forma manual.</span><span class="sxs-lookup"><span data-stu-id="cfbec-616">Don't write long blocks of manually-implemented `RenderTreeBuilder` logic.</span></span> <span data-ttu-id="cfbec-617">Prefiere `.razor` archivos y permitir que el compilador trate los números de secuencia.</span><span class="sxs-lookup"><span data-stu-id="cfbec-617">Prefer `.razor` files and allow the compiler to deal with the sequence numbers.</span></span> <span data-ttu-id="cfbec-618">Si no puede evitar la lógica de `RenderTreeBuilder` manual, divida bloques largos de código en fragmentos más pequeños encapsulados en `OpenRegion` @ no__t-2 @ no__t-3 llamadas.</span><span class="sxs-lookup"><span data-stu-id="cfbec-618">If you're unable to avoid manual `RenderTreeBuilder` logic, split long blocks of code into smaller pieces wrapped in `OpenRegion`/`CloseRegion` calls.</span></span> <span data-ttu-id="cfbec-619">Cada región tiene su propio espacio independiente de números de secuencia, por lo que puede reiniciar desde cero (o cualquier otro número arbitrario) dentro de cada región.</span><span class="sxs-lookup"><span data-stu-id="cfbec-619">Each region has its own separate space of sequence numbers, so you can restart from zero (or any other arbitrary number) inside each region.</span></span>
* <span data-ttu-id="cfbec-620">Si los números de secuencia están codificados, el algoritmo de comparación solo requiere que los números de secuencia aumenten en valor.</span><span class="sxs-lookup"><span data-stu-id="cfbec-620">If sequence numbers are hardcoded, the diff algorithm only requires that sequence numbers increase in value.</span></span> <span data-ttu-id="cfbec-621">El valor inicial y los huecos son irrelevantes.</span><span class="sxs-lookup"><span data-stu-id="cfbec-621">The initial value and gaps are irrelevant.</span></span> <span data-ttu-id="cfbec-622">Una opción legítima es usar el número de línea de código como el número de secuencia, o comenzar a partir de cero y aumentar por unos o cientos (o cualquier intervalo preferido).</span><span class="sxs-lookup"><span data-stu-id="cfbec-622">One legitimate option is to use the code line number as the sequence number, or start from zero and increase by ones or hundreds (or any preferred interval).</span></span> 
* <span data-ttu-id="cfbec-623">El uso de los números de secuencia es más rápido, mientras que otros marcos de interfaz de usuario de comparación de árboles no los utilizan.</span><span class="sxs-lookup"><span data-stu-id="cfbec-623">Blazor uses sequence numbers, while other tree-diffing UI frameworks don't use them.</span></span> <span data-ttu-id="cfbec-624">La diferenciación es mucho más rápida cuando se utilizan números de secuencia y más increíble tiene la ventaja de un paso de compilación que se ocupa de los números de secuencia automáticamente para los programadores que crean archivos `.razor`.</span><span class="sxs-lookup"><span data-stu-id="cfbec-624">Diffing is far faster when sequence numbers are used, and Blazor has the advantage of a compile step that deals with sequence numbers automatically for developers authoring `.razor` files.</span></span>

## <a name="localization"></a><span data-ttu-id="cfbec-625">Localización</span><span class="sxs-lookup"><span data-stu-id="cfbec-625">Localization</span></span>

<span data-ttu-id="cfbec-626">Las aplicaciones de servidor increíbles se localizan con [middleware de localización](xref:fundamentals/localization#localization-middleware).</span><span class="sxs-lookup"><span data-stu-id="cfbec-626">Blazor Server apps are localized using [Localization Middleware](xref:fundamentals/localization#localization-middleware).</span></span> <span data-ttu-id="cfbec-627">El middleware selecciona la referencia cultural adecuada para los usuarios que solicitan recursos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="cfbec-627">The middleware selects the appropriate culture for users requesting resources from the app.</span></span>

<span data-ttu-id="cfbec-628">La referencia cultural se puede establecer utilizando uno de los métodos siguientes:</span><span class="sxs-lookup"><span data-stu-id="cfbec-628">The culture can be set using one of the following approaches:</span></span>

* [<span data-ttu-id="cfbec-629">Cookies</span><span class="sxs-lookup"><span data-stu-id="cfbec-629">Cookies</span></span>](#cookies)
* [<span data-ttu-id="cfbec-630">Proporcionar la interfaz de usuario para elegir la referencia cultural</span><span class="sxs-lookup"><span data-stu-id="cfbec-630">Provide UI to choose the culture</span></span>](#provide-ui-to-choose-the-culture)

<span data-ttu-id="cfbec-631">Para obtener más información y ejemplos, vea <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="cfbec-631">For more information and examples, see <xref:fundamentals/localization>.</span></span>

### <a name="cookies"></a><span data-ttu-id="cfbec-632">Cookies</span><span class="sxs-lookup"><span data-stu-id="cfbec-632">Cookies</span></span>

<span data-ttu-id="cfbec-633">Una cookie de referencia cultural de localización puede conservar la referencia cultural del usuario.</span><span class="sxs-lookup"><span data-stu-id="cfbec-633">A localization culture cookie can persist the user's culture.</span></span> <span data-ttu-id="cfbec-634">La cookie la crea el método `OnGet` de la página host de la aplicación (*pages/host. cshtml. CS*).</span><span class="sxs-lookup"><span data-stu-id="cfbec-634">The cookie is created by the `OnGet` method of the app's host page (*Pages/Host.cshtml.cs*).</span></span> <span data-ttu-id="cfbec-635">El middleware de localización lee la cookie en solicitudes posteriores para establecer la referencia cultural del usuario.</span><span class="sxs-lookup"><span data-stu-id="cfbec-635">The Localization Middleware reads the cookie on subsequent requests to set the user's culture.</span></span> 

<span data-ttu-id="cfbec-636">El uso de una cookie garantiza que la conexión de WebSocket puede propagar correctamente la referencia cultural.</span><span class="sxs-lookup"><span data-stu-id="cfbec-636">Use of a cookie ensures that the WebSocket connection can correctly propagate the culture.</span></span> <span data-ttu-id="cfbec-637">Si los esquemas de localización se basan en la ruta de acceso URL o la cadena de consulta, es posible que el esquema no funcione con WebSockets, por lo que no se puede conservar la referencia cultural.</span><span class="sxs-lookup"><span data-stu-id="cfbec-637">If localization schemes are based on the URL path or query string, the scheme might not be able to work with WebSockets, thus fail to persist the culture.</span></span> <span data-ttu-id="cfbec-638">Por lo tanto, el uso de una cookie de referencia cultural de localización es el enfoque recomendado.</span><span class="sxs-lookup"><span data-stu-id="cfbec-638">Therefore, use of a localization culture cookie is the recommended approach.</span></span>

<span data-ttu-id="cfbec-639">Cualquier técnica se puede usar para asignar una referencia cultural si la referencia cultural se conserva en una cookie de localización.</span><span class="sxs-lookup"><span data-stu-id="cfbec-639">Any technique can be used to assign a culture if the culture is persisted in a localization cookie.</span></span> <span data-ttu-id="cfbec-640">Si la aplicación ya tiene un esquema de localización establecido para ASP.NET Core del lado servidor, siga usando la infraestructura de localización existente de la aplicación y establezca la cookie de la cultura de localización en el esquema de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="cfbec-640">If the app already has an established localization scheme for server-side ASP.NET Core, continue to use the app's existing localization infrastructure and set the localization culture cookie within the app's scheme.</span></span>

<span data-ttu-id="cfbec-641">En el ejemplo siguiente se muestra cómo establecer la referencia cultural actual en una cookie que puede ser leída por el middleware de localización.</span><span class="sxs-lookup"><span data-stu-id="cfbec-641">The following example shows how to set the current culture in a cookie that can be read by the Localization Middleware.</span></span> <span data-ttu-id="cfbec-642">Cree un archivo *pages/host. cshtml. CS* con el siguiente contenido en la aplicación de servidor increíbles:</span><span class="sxs-lookup"><span data-stu-id="cfbec-642">Create a *Pages/Host.cshtml.cs* file with the following contents in the Blazor Server app:</span></span>

```csharp
public class HostModel : PageModel
{
    public void OnGet()
    {
        HttpContext.Response.Cookies.Append(
            CookieRequestCultureProvider.DefaultCookieName,
            CookieRequestCultureProvider.MakeCookieValue(
                new RequestCulture(
                    CultureInfo.CurrentCulture,
                    CultureInfo.CurrentUICulture)));
    }
}
```

<span data-ttu-id="cfbec-643">La localización se controla en la aplicación:</span><span class="sxs-lookup"><span data-stu-id="cfbec-643">Localization is handled in the app:</span></span>

1. <span data-ttu-id="cfbec-644">El explorador envía una solicitud HTTP inicial a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="cfbec-644">The browser sends an initial HTTP request to the app.</span></span>
1. <span data-ttu-id="cfbec-645">El middleware de localización asigna la referencia cultural.</span><span class="sxs-lookup"><span data-stu-id="cfbec-645">The culture is assigned by the Localization Middleware.</span></span>
1. <span data-ttu-id="cfbec-646">El método `OnGet` de *_Host. cshtml. CS* conserva la referencia cultural en una cookie como parte de la respuesta.</span><span class="sxs-lookup"><span data-stu-id="cfbec-646">The `OnGet` method in *_Host.cshtml.cs* persists the culture in a cookie as part of the response.</span></span>
1. <span data-ttu-id="cfbec-647">El explorador abre una conexión WebSocket para crear una sesión de servidor increíblemente interactiva.</span><span class="sxs-lookup"><span data-stu-id="cfbec-647">The browser opens a WebSocket connection to create an interactive Blazor Server session.</span></span>
1. <span data-ttu-id="cfbec-648">El middleware de localización lee la cookie y asigna la referencia cultural.</span><span class="sxs-lookup"><span data-stu-id="cfbec-648">The Localization Middleware reads the cookie and assigns the culture.</span></span>
1. <span data-ttu-id="cfbec-649">La sesión de servidor extraordinaria se inicia con la referencia cultural correcta.</span><span class="sxs-lookup"><span data-stu-id="cfbec-649">The Blazor Server session begins with the correct culture.</span></span>

## <a name="provide-ui-to-choose-the-culture"></a><span data-ttu-id="cfbec-650">Proporcionar la interfaz de usuario para elegir la referencia cultural</span><span class="sxs-lookup"><span data-stu-id="cfbec-650">Provide UI to choose the culture</span></span>

<span data-ttu-id="cfbec-651">Para proporcionar una interfaz de usuario que permita a los usuarios seleccionar una referencia cultural, se recomienda un *enfoque basado en redirección* .</span><span class="sxs-lookup"><span data-stu-id="cfbec-651">To provide UI to allow a user to select a culture, a *redirect-based approach* is recommended.</span></span> <span data-ttu-id="cfbec-652">El proceso es similar a lo que ocurre en una aplicación web cuando un usuario intenta acceder a un recurso seguro @ no__t-0The se redirige a una página de inicio de sesión y después se redirige al recurso original.</span><span class="sxs-lookup"><span data-stu-id="cfbec-652">The process is similar to what happens in a web app when a user attempts to access a secure resource&mdash;the user is redirected to a sign-in page and then redirected back to the original resource.</span></span> 

<span data-ttu-id="cfbec-653">La aplicación conserva la referencia cultural seleccionada del usuario a través de una redirección a un controlador.</span><span class="sxs-lookup"><span data-stu-id="cfbec-653">The app persists the user's selected culture via a redirect to a controller.</span></span> <span data-ttu-id="cfbec-654">El controlador establece la referencia cultural seleccionada del usuario en una cookie y redirige al usuario de nuevo al URI original.</span><span class="sxs-lookup"><span data-stu-id="cfbec-654">The controller sets the user's selected culture into a cookie and redirects the user back to the original URI.</span></span>

<span data-ttu-id="cfbec-655">Establezca un punto de conexión HTTP en el servidor para establecer la referencia cultural seleccionada del usuario en una cookie y volver a realizar la redirección al URI original:</span><span class="sxs-lookup"><span data-stu-id="cfbec-655">Establish an HTTP endpoint on the server to set the user's selected culture in a cookie and perform the redirect back to the original URI:</span></span>

```csharp
[Route("[controller]/[action]")]
public class CultureController : Controller
{
    public IActionResult SetCulture(string culture, string redirectUri)
    {
        if (culture != null)
        {
            HttpContext.Response.Cookies.Append(
                CookieRequestCultureProvider.DefaultCookieName,
                CookieRequestCultureProvider.MakeCookieValue(
                    new RequestCulture(culture)));
        }

        return LocalRedirect(redirectUri);
    }
}
```

> [!WARNING]
> <span data-ttu-id="cfbec-656">Use el resultado de la acción `LocalRedirect` para evitar ataques de redireccionamiento abierto.</span><span class="sxs-lookup"><span data-stu-id="cfbec-656">Use the `LocalRedirect` action result to prevent open redirect attacks.</span></span> <span data-ttu-id="cfbec-657">Para obtener más información, vea <xref:security/preventing-open-redirects>.</span><span class="sxs-lookup"><span data-stu-id="cfbec-657">For more information, see <xref:security/preventing-open-redirects>.</span></span>

<span data-ttu-id="cfbec-658">El siguiente componente muestra un ejemplo de cómo realizar la redirección inicial cuando el usuario selecciona una referencia cultural:</span><span class="sxs-lookup"><span data-stu-id="cfbec-658">The following component shows an example of how to perform the initial redirection when the user selects a culture:</span></span>

```cshtml
@inject NavigationManager NavigationManager

<h3>Select your language</h3>

<select @onchange="OnSelected">
    <option>Select...</option>
    <option value="en-US">English</option>
    <option value="fr-FR">Français</option>
</select>

@code {
    private double textNumber;

    private void OnSelected(ChangeEventArgs e)
    {
        var culture = (string)e.Value;
        var uri = new Uri(NavigationManager.Uri())
            .GetComponents(UriComponents.PathAndQuery, UriFormat.Unescaped);
        var query = $"?culture={Uri.EscapeDataString(culture)}&" +
            $"redirectUri={Uri.EscapeDataString(uri)}";

        NavigationManager.NavigateTo("/Culture/SetCulture" + query, forceLoad: true);
    }
}
```

### <a name="use-net-localization-scenarios-in-blazor-apps"></a><span data-ttu-id="cfbec-659">Uso de escenarios de localización de .NET en aplicaciones increíbles</span><span class="sxs-lookup"><span data-stu-id="cfbec-659">Use .NET localization scenarios in Blazor apps</span></span>

<span data-ttu-id="cfbec-660">Dentro de las aplicaciones increíbles, están disponibles los siguientes escenarios de localización y globalización de .NET:</span><span class="sxs-lookup"><span data-stu-id="cfbec-660">Inside Blazor apps, the following .NET localization and globalization scenarios are available:</span></span>

* <span data-ttu-id="cfbec-661">. Sistema de recursos de la red</span><span class="sxs-lookup"><span data-stu-id="cfbec-661">.NET's resources system</span></span>
* <span data-ttu-id="cfbec-662">Formato de fecha y número específico de la referencia cultural</span><span class="sxs-lookup"><span data-stu-id="cfbec-662">Culture-specific number and date formatting</span></span>

<span data-ttu-id="cfbec-663">La funcionalidad `@bind` de la extraordinariación realiza la globalización en función de la referencia cultural actual del usuario.</span><span class="sxs-lookup"><span data-stu-id="cfbec-663">Blazor's `@bind` functionality performs globalization based on the user's current culture.</span></span> <span data-ttu-id="cfbec-664">Para obtener más información, vea la sección [enlace de datos](#data-binding) .</span><span class="sxs-lookup"><span data-stu-id="cfbec-664">For more information, see the [Data binding](#data-binding) section.</span></span>

<span data-ttu-id="cfbec-665">Actualmente se admite un conjunto limitado de escenarios de localización de ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="cfbec-665">A limited set of ASP.NET Core's localization scenarios are currently supported:</span></span>

* <span data-ttu-id="cfbec-666">`IStringLocalizer<>` *es compatible* con aplicaciones increíbles.</span><span class="sxs-lookup"><span data-stu-id="cfbec-666">`IStringLocalizer<>` *is supported* in Blazor apps.</span></span>
* <span data-ttu-id="cfbec-667">`IHtmlLocalizer<>`, `IViewLocalizer<>` y la localización de anotaciones de datos son escenarios de MVC ASP.NET Core y **no se admiten** en aplicaciones increíbles.</span><span class="sxs-lookup"><span data-stu-id="cfbec-667">`IHtmlLocalizer<>`, `IViewLocalizer<>`, and Data Annotations localization are ASP.NET Core MVC scenarios and **not supported** in Blazor apps.</span></span>

<span data-ttu-id="cfbec-668">Para obtener más información, vea <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="cfbec-668">For more information, see <xref:fundamentals/localization>.</span></span>

## <a name="scalable-vector-graphics-svg-images"></a><span data-ttu-id="cfbec-669">Imágenes de gráficos vectoriales escalables (SVG)</span><span class="sxs-lookup"><span data-stu-id="cfbec-669">Scalable Vector Graphics (SVG) images</span></span>

<span data-ttu-id="cfbec-670">Dado que el increíble procesador representa imágenes HTML, compatibles con el explorador, incluidas las imágenes SVG (Scalable Vector Graphics) ( *. svg*), se admiten a través de la etiqueta `<img>`:</span><span class="sxs-lookup"><span data-stu-id="cfbec-670">Since Blazor renders HTML, browser-supported images, including Scalable Vector Graphics (SVG) images (*.svg*), are supported via the `<img>` tag:</span></span>

```html
<img alt="Example image" src="some-image.svg" />
```

<span data-ttu-id="cfbec-671">Del mismo modo, las imágenes SVG se admiten en las reglas CSS de un archivo de hoja de estilos ( *. CSS*):</span><span class="sxs-lookup"><span data-stu-id="cfbec-671">Similarly, SVG images are supported in the CSS rules of a stylesheet file (*.css*):</span></span>

```css
.my-element {
    background-image: url("some-image.svg");
}
```

<span data-ttu-id="cfbec-672">Sin embargo, el marcado SVG en línea no se admite en todos los escenarios.</span><span class="sxs-lookup"><span data-stu-id="cfbec-672">However, inline SVG markup isn't supported in all scenarios.</span></span> <span data-ttu-id="cfbec-673">Si coloca una etiqueta `<svg>` directamente en un archivo de componente ( *. Razor*), se admite la representación de imágenes básica, pero aún no se admiten muchos escenarios avanzados.</span><span class="sxs-lookup"><span data-stu-id="cfbec-673">If you place an `<svg>` tag directly into a component file (*.razor*), basic image rendering is supported but many advanced scenarios aren't yet supported.</span></span> <span data-ttu-id="cfbec-674">Por ejemplo, las etiquetas `<use>` no se respetan actualmente y `@bind` no se pueden usar con algunas etiquetas SVG.</span><span class="sxs-lookup"><span data-stu-id="cfbec-674">For example, `<use>` tags aren't currently respected, and `@bind` can't be used with some SVG tags.</span></span> <span data-ttu-id="cfbec-675">Esperamos abordar estas limitaciones en una versión futura.</span><span class="sxs-lookup"><span data-stu-id="cfbec-675">We expect to address these limitations in a future release.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="cfbec-676">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="cfbec-676">Additional resources</span></span>

* <span data-ttu-id="cfbec-677"><xref:security/blazor/server> &ndash; incluye instrucciones sobre la creación de aplicaciones de servidor increíbles que deben competir con el agotamiento de recursos.</span><span class="sxs-lookup"><span data-stu-id="cfbec-677"><xref:security/blazor/server> &ndash; Includes guidance on building Blazor Server apps that must contend with resource exhaustion.</span></span>
