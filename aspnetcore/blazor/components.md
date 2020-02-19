---
title: Crear y usar ASP.NET Core componentes de Razor
author: guardrex
description: Obtenga información sobre cómo crear y usar componentes de Razor, incluido cómo enlazar a datos, controlar eventos y administrar ciclos de vida de componentes.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2020
no-loc:
- Blazor
- SignalR
uid: blazor/components
ms.openlocfilehash: f9b4eab29fafe8113528062f57d28dadd0f57577
ms.sourcegitcommit: 6645435fc8f5092fc7e923742e85592b56e37ada
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2020
ms.locfileid: "77447105"
---
# <a name="create-and-use-aspnet-core-razor-components"></a><span data-ttu-id="958cf-103">Crear y usar ASP.NET Core componentes de Razor</span><span class="sxs-lookup"><span data-stu-id="958cf-103">Create and use ASP.NET Core Razor components</span></span>

<span data-ttu-id="958cf-104">Por [Luke Latham](https://github.com/guardrex) y [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="958cf-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="958cf-105">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="958cf-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

Blazor<span data-ttu-id="958cf-106"> aplicaciones se compilan con *componentes*de.</span><span class="sxs-lookup"><span data-stu-id="958cf-106"> apps are built using *components*.</span></span> <span data-ttu-id="958cf-107">Un componente es un fragmento independiente de la interfaz de usuario (IU), como una página, un cuadro de diálogo o un formulario.</span><span class="sxs-lookup"><span data-stu-id="958cf-107">A component is a self-contained chunk of user interface (UI), such as a page, dialog, or form.</span></span> <span data-ttu-id="958cf-108">Un componente incluye el marcado HTML y la lógica de procesamiento necesaria para insertar datos o responder a los eventos de la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="958cf-108">A component includes HTML markup and the processing logic required to inject data or respond to UI events.</span></span> <span data-ttu-id="958cf-109">Los componentes son flexibles y ligeros.</span><span class="sxs-lookup"><span data-stu-id="958cf-109">Components are flexible and lightweight.</span></span> <span data-ttu-id="958cf-110">Se pueden anidar, reutilizar y compartir entre proyectos.</span><span class="sxs-lookup"><span data-stu-id="958cf-110">They can be nested, reused, and shared among projects.</span></span>

## <a name="component-classes"></a><span data-ttu-id="958cf-111">Clases de componentes</span><span class="sxs-lookup"><span data-stu-id="958cf-111">Component classes</span></span>

<span data-ttu-id="958cf-112">Los componentes se implementan en archivos de componentes de [Razor](xref:mvc/views/razor) ( *. Razor*) C# mediante una combinación de y el marcado HTML.</span><span class="sxs-lookup"><span data-stu-id="958cf-112">Components are implemented in [Razor](xref:mvc/views/razor) component files (*.razor*) using a combination of C# and HTML markup.</span></span> <span data-ttu-id="958cf-113">Un componente de Blazor se conoce formalmente como un *componente de Razor*.</span><span class="sxs-lookup"><span data-stu-id="958cf-113">A component in Blazor is formally referred to as a *Razor component*.</span></span>

<span data-ttu-id="958cf-114">El nombre de un componente debe empezar con un carácter en mayúsculas.</span><span class="sxs-lookup"><span data-stu-id="958cf-114">A component's name must start with an uppercase character.</span></span> <span data-ttu-id="958cf-115">Por ejemplo, *MyCoolComponent. Razor* es válido y *MyCoolComponent. Razor* no es válido.</span><span class="sxs-lookup"><span data-stu-id="958cf-115">For example, *MyCoolComponent.razor* is valid, and *myCoolComponent.razor* is invalid.</span></span>

<span data-ttu-id="958cf-116">La interfaz de usuario de un componente se define mediante HTML.</span><span class="sxs-lookup"><span data-stu-id="958cf-116">The UI for a component is defined using HTML.</span></span> <span data-ttu-id="958cf-117">La lógica de la representación dinámica (por ejemplo, bucles, instrucciones condicionales, expresiones) se agrega mediante una sintaxis de C# insertada denominada [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="958cf-117">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="958cf-118">Cuando se compila una aplicación, el marcado HTML y C# la lógica de representación se convierten en una clase de componente.</span><span class="sxs-lookup"><span data-stu-id="958cf-118">When an app is compiled, the HTML markup and C# rendering logic are converted into a component class.</span></span> <span data-ttu-id="958cf-119">El nombre de la clase generada coincide con el nombre del archivo.</span><span class="sxs-lookup"><span data-stu-id="958cf-119">The name of the generated class matches the name of the file.</span></span>

<span data-ttu-id="958cf-120">Los miembros de la clase de componente se definen en un bloque `@code`.</span><span class="sxs-lookup"><span data-stu-id="958cf-120">Members of the component class are defined in an `@code` block.</span></span> <span data-ttu-id="958cf-121">En el bloque `@code`, el estado de componente (propiedades, campos) se especifica con métodos para el control de eventos o para definir otra lógica de componentes.</span><span class="sxs-lookup"><span data-stu-id="958cf-121">In the `@code` block, component state (properties, fields) is specified with methods for event handling or for defining other component logic.</span></span> <span data-ttu-id="958cf-122">Se permite emplear más de un bloque `@code`.</span><span class="sxs-lookup"><span data-stu-id="958cf-122">More than one `@code` block is permissible.</span></span>

<span data-ttu-id="958cf-123">Los miembros de componente se pueden usar como parte de la lógica de representación del C# componente mediante expresiones que comienzan con `@`.</span><span class="sxs-lookup"><span data-stu-id="958cf-123">Component members can be used as part of the component's rendering logic using C# expressions that start with `@`.</span></span> <span data-ttu-id="958cf-124">Por ejemplo, un C# campo se representa mediante el prefijo `@` al nombre del campo.</span><span class="sxs-lookup"><span data-stu-id="958cf-124">For example, a C# field is rendered by prefixing `@` to the field name.</span></span> <span data-ttu-id="958cf-125">En el siguiente ejemplo se evalúa y representa:</span><span class="sxs-lookup"><span data-stu-id="958cf-125">The following example evaluates and renders:</span></span>

* <span data-ttu-id="958cf-126">`_headingFontStyle` al valor de la propiedad CSS para `font-style`.</span><span class="sxs-lookup"><span data-stu-id="958cf-126">`_headingFontStyle` to the CSS property value for `font-style`.</span></span>
* <span data-ttu-id="958cf-127">`_headingText` al contenido del elemento `<h1>`.</span><span class="sxs-lookup"><span data-stu-id="958cf-127">`_headingText` to the content of the `<h1>` element.</span></span>

```razor
<h1 style="font-style:@_headingFontStyle">@_headingText</h1>

@code {
    private string _headingFontStyle = "italic";
    private string _headingText = "Put on your new Blazor!";
}
```

<span data-ttu-id="958cf-128">Una vez que el componente se representa inicialmente, el componente regenera su árbol de representación en respuesta a los eventos.</span><span class="sxs-lookup"><span data-stu-id="958cf-128">After the component is initially rendered, the component regenerates its render tree in response to events.</span></span> <span data-ttu-id="958cf-129">a continuación, Blazor compara el nuevo árbol de representación con el anterior y aplica cualquier modificación a la Document Object Model del explorador (DOM).</span><span class="sxs-lookup"><span data-stu-id="958cf-129">Blazor then compares the new render tree against the previous one and applies any modifications to the browser's Document Object Model (DOM).</span></span>

<span data-ttu-id="958cf-130">Los componentes son C# clases normales y se pueden colocar en cualquier parte dentro de un proyecto.</span><span class="sxs-lookup"><span data-stu-id="958cf-130">Components are ordinary C# classes and can be placed anywhere within a project.</span></span> <span data-ttu-id="958cf-131">Los componentes que generan páginas web normalmente residen en la carpeta *pages* .</span><span class="sxs-lookup"><span data-stu-id="958cf-131">Components that produce webpages usually reside in the *Pages* folder.</span></span> <span data-ttu-id="958cf-132">Los componentes que no son de página se colocan con frecuencia en la carpeta *compartida* o en una carpeta personalizada agregada al proyecto.</span><span class="sxs-lookup"><span data-stu-id="958cf-132">Non-page components are frequently placed in the *Shared* folder or a custom folder added to the project.</span></span>

<span data-ttu-id="958cf-133">Normalmente, el espacio de nombres de un componente se deriva del espacio de nombres raíz de la aplicación y la ubicación del componente (carpeta) dentro de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="958cf-133">Typically, a component's namespace is derived from the app's root namespace and the component's location (folder) within the app.</span></span> <span data-ttu-id="958cf-134">Si el espacio de nombres raíz de la aplicación es `BlazorApp` y el componente `Counter` reside en la carpeta *páginas* :</span><span class="sxs-lookup"><span data-stu-id="958cf-134">If the app's root namespace is `BlazorApp` and the `Counter` component resides in the *Pages* folder:</span></span>

* <span data-ttu-id="958cf-135">El espacio de nombres del componente de `Counter` es `BlazorApp.Pages`.</span><span class="sxs-lookup"><span data-stu-id="958cf-135">The `Counter` component's namespace is `BlazorApp.Pages`.</span></span>
* <span data-ttu-id="958cf-136">El nombre de tipo completo del componente es `BlazorApp.Pages.Counter`.</span><span class="sxs-lookup"><span data-stu-id="958cf-136">The fully qualified type name of the component is `BlazorApp.Pages.Counter`.</span></span>

<span data-ttu-id="958cf-137">Para obtener más información, vea la sección [importar componentes](#import-components) .</span><span class="sxs-lookup"><span data-stu-id="958cf-137">For more information, see the [Import components](#import-components) section.</span></span>

<span data-ttu-id="958cf-138">Para usar una carpeta personalizada, agregue el espacio de nombres de la carpeta personalizada al componente primario o al archivo *_Imports. Razor* de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="958cf-138">To use a custom folder, add the custom folder's namespace to either the parent component or to the app's *_Imports.razor* file.</span></span> <span data-ttu-id="958cf-139">Por ejemplo, el siguiente espacio de nombres hace que los componentes de una carpeta *Components* estén disponibles cuando se `BlazorApp`el espacio de nombres raíz de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="958cf-139">For example, the following namespace makes components in a *Components* folder available when the app's root namespace is `BlazorApp`:</span></span>

```razor
@using BlazorApp.Components
```

## <a name="tag-helpers-arent-used-in-components"></a><span data-ttu-id="958cf-140">Las aplicaciones auxiliares de etiquetas no se usan en los componentes</span><span class="sxs-lookup"><span data-stu-id="958cf-140">Tag Helpers aren't used in components</span></span>

<span data-ttu-id="958cf-141">Las [aplicaciones auxiliares de etiquetas](xref:mvc/views/tag-helpers/intro) no se admiten en los componentes de Razor (archivos *. Razor* ).</span><span class="sxs-lookup"><span data-stu-id="958cf-141">[Tag Helpers](xref:mvc/views/tag-helpers/intro) aren't supported in Razor components (*.razor* files).</span></span> <span data-ttu-id="958cf-142">Para proporcionar funcionalidad similar a la aplicación auxiliar de etiquetas en Blazor, cree un componente con la misma funcionalidad que la aplicación auxiliar de etiquetas y use el componente en su lugar.</span><span class="sxs-lookup"><span data-stu-id="958cf-142">To provide Tag Helper-like functionality in Blazor, create a component with the same functionality as the Tag Helper and use the component instead.</span></span>

## <a name="use-components"></a><span data-ttu-id="958cf-143">Uso de componentes</span><span class="sxs-lookup"><span data-stu-id="958cf-143">Use components</span></span>

<span data-ttu-id="958cf-144">Los componentes pueden incluir otros componentes declarándolos mediante la sintaxis de elementos HTML.</span><span class="sxs-lookup"><span data-stu-id="958cf-144">Components can include other components by declaring them using HTML element syntax.</span></span> <span data-ttu-id="958cf-145">El marcado para utilizar un componente se parece a una etiqueta HTML en la que el nombre de la etiqueta es el tipo de componente.</span><span class="sxs-lookup"><span data-stu-id="958cf-145">The markup for using a component looks like an HTML tag where the name of the tag is the component type.</span></span>

<span data-ttu-id="958cf-146">El enlace de atributo distingue mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="958cf-146">Attribute binding is case sensitive.</span></span> <span data-ttu-id="958cf-147">Por ejemplo, `@bind` es válido y `@Bind` no es válido.</span><span class="sxs-lookup"><span data-stu-id="958cf-147">For example, `@bind` is valid, and `@Bind` is invalid.</span></span>

<span data-ttu-id="958cf-148">El marcado siguiente en *index. Razor* representa una instancia de `HeadingComponent`:</span><span class="sxs-lookup"><span data-stu-id="958cf-148">The following markup in *Index.razor* renders a `HeadingComponent` instance:</span></span>

```razor
<HeadingComponent />
```

<span data-ttu-id="958cf-149">*Components/HeadingComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="958cf-149">*Components/HeadingComponent.razor*:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/HeadingComponent.razor)]

<span data-ttu-id="958cf-150">Si un componente contiene un elemento HTML con una primera letra mayúscula que no coincide con un nombre de componente, se emite una advertencia que indica que el elemento tiene un nombre inesperado.</span><span class="sxs-lookup"><span data-stu-id="958cf-150">If a component contains an HTML element with an uppercase first letter that doesn't match a component name, a warning is emitted indicating that the element has an unexpected name.</span></span> <span data-ttu-id="958cf-151">La adición de una directiva de `@using` para el espacio de nombres del componente hace que el componente esté disponible, lo que resuelve la advertencia.</span><span class="sxs-lookup"><span data-stu-id="958cf-151">Adding an `@using` directive for the component's namespace makes the component available, which resolves the warning.</span></span>

## <a name="routing"></a><span data-ttu-id="958cf-152">Enrutamiento</span><span class="sxs-lookup"><span data-stu-id="958cf-152">Routing</span></span>

<span data-ttu-id="958cf-153">El enrutamiento en Blazor se consigue proporcionando una plantilla de ruta a cada componente accesible en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="958cf-153">Routing in Blazor is achieved by providing a route template to each accessible component in the app.</span></span>

<span data-ttu-id="958cf-154">Cuando se compila un archivo de Razor con una directiva de `@page`, a la clase generada se le asigna un <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> que especifica la plantilla de ruta.</span><span class="sxs-lookup"><span data-stu-id="958cf-154">When a Razor file with an `@page` directive is compiled, the generated class is given a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span> <span data-ttu-id="958cf-155">En tiempo de ejecución, el enrutador busca clases de componentes con un `RouteAttribute` y representa el componente que tiene una plantilla de ruta que coincide con la dirección URL solicitada.</span><span class="sxs-lookup"><span data-stu-id="958cf-155">At runtime, the router looks for component classes with a `RouteAttribute` and renders whichever component has a route template that matches the requested URL.</span></span>

```razor
@page "/ParentComponent"

...
```

<span data-ttu-id="958cf-156">Para más información, consulte <xref:blazor/routing>.</span><span class="sxs-lookup"><span data-stu-id="958cf-156">For more information, see <xref:blazor/routing>.</span></span>

## <a name="parameters"></a><span data-ttu-id="958cf-157">Parámetros</span><span class="sxs-lookup"><span data-stu-id="958cf-157">Parameters</span></span>

### <a name="route-parameters"></a><span data-ttu-id="958cf-158">Parámetros de ruta</span><span class="sxs-lookup"><span data-stu-id="958cf-158">Route parameters</span></span>

<span data-ttu-id="958cf-159">Los componentes pueden recibir parámetros de ruta de la plantilla de ruta proporcionada en la Directiva de `@page`.</span><span class="sxs-lookup"><span data-stu-id="958cf-159">Components can receive route parameters from the route template provided in the `@page` directive.</span></span> <span data-ttu-id="958cf-160">El enrutador usa parámetros de ruta para rellenar los parámetros de componente correspondientes.</span><span class="sxs-lookup"><span data-stu-id="958cf-160">The router uses route parameters to populate the corresponding component parameters.</span></span>

<span data-ttu-id="958cf-161">*Pages/RouteParameter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="958cf-161">*Pages/RouteParameter.razor*:</span></span>

[!code-razor[](components/samples_snapshot/RouteParameter.razor?highlight=2,7-8)]

<span data-ttu-id="958cf-162">Los parámetros opcionales no se admiten, por lo que se aplican dos directivas de `@page` en el ejemplo anterior.</span><span class="sxs-lookup"><span data-stu-id="958cf-162">Optional parameters aren't supported, so two `@page` directives are applied in the preceding example.</span></span> <span data-ttu-id="958cf-163">El primero permite la navegación al componente sin un parámetro.</span><span class="sxs-lookup"><span data-stu-id="958cf-163">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="958cf-164">La segunda Directiva de `@page` recibe el parámetro `{text}` Route y asigna el valor a la propiedad `Text`.</span><span class="sxs-lookup"><span data-stu-id="958cf-164">The second `@page` directive receives the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

<span data-ttu-id="958cf-165">La sintaxis de *los parámetros catch-all* (`*`/`**`), que capturan la ruta de acceso en varios límites de carpeta, **no** se admite en los componentes de Razor ( *. Razor*).</span><span class="sxs-lookup"><span data-stu-id="958cf-165">*Catch-all* parameter syntax (`*`/`**`), which captures the path across multiple folder boundaries, is **not** supported in Razor components (*.razor*).</span></span>

### <a name="component-parameters"></a><span data-ttu-id="958cf-166">Parámetros del componente</span><span class="sxs-lookup"><span data-stu-id="958cf-166">Component parameters</span></span>

<span data-ttu-id="958cf-167">Los componentes pueden tener *parámetros de componente*, que se definen mediante propiedades públicas en la clase de componente con el atributo `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="958cf-167">Components can have *component parameters*, which are defined using public properties on the component class with the `[Parameter]` attribute.</span></span> <span data-ttu-id="958cf-168">Use atributos para especificar argumentos para un componente en el marcado.</span><span class="sxs-lookup"><span data-stu-id="958cf-168">Use attributes to specify arguments for a component in markup.</span></span>

<span data-ttu-id="958cf-169">*Components/ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="958cf-169">*Components/ChildComponent.razor*:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=2,11-12)]

<span data-ttu-id="958cf-170">En el ejemplo siguiente de la aplicación de ejemplo, el `ParentComponent` establece el valor de la propiedad `Title` de la `ChildComponent`.</span><span class="sxs-lookup"><span data-stu-id="958cf-170">In the following example from the sample app, the `ParentComponent` sets the value of the `Title` property of the `ChildComponent`.</span></span>

<span data-ttu-id="958cf-171">*Pages/ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="958cf-171">*Pages/ParentComponent.razor*:</span></span>

[!code-razor[](components/samples_snapshot/ParentComponent.razor?highlight=5-6)]

## <a name="child-content"></a><span data-ttu-id="958cf-172">Contenido secundario</span><span class="sxs-lookup"><span data-stu-id="958cf-172">Child content</span></span>

<span data-ttu-id="958cf-173">Los componentes pueden establecer el contenido de otro componente.</span><span class="sxs-lookup"><span data-stu-id="958cf-173">Components can set the content of another component.</span></span> <span data-ttu-id="958cf-174">El componente de asignación proporciona el contenido entre las etiquetas que especifican el componente receptor.</span><span class="sxs-lookup"><span data-stu-id="958cf-174">The assigning component provides the content between the tags that specify the receiving component.</span></span>

<span data-ttu-id="958cf-175">En el ejemplo siguiente, el `ChildComponent` tiene una propiedad `ChildContent` que representa un `RenderFragment`, que representa un segmento de interfaz de usuario que se va a representar.</span><span class="sxs-lookup"><span data-stu-id="958cf-175">In the following example, the `ChildComponent` has a `ChildContent` property that represents a `RenderFragment`, which represents a segment of UI to render.</span></span> <span data-ttu-id="958cf-176">El valor de `ChildContent` se coloca en el marcado del componente en el que se debe representar el contenido.</span><span class="sxs-lookup"><span data-stu-id="958cf-176">The value of `ChildContent` is positioned in the component's markup where the content should be rendered.</span></span> <span data-ttu-id="958cf-177">El valor de `ChildContent` se recibe del componente primario y se representa dentro del `panel-body`del panel de arranque.</span><span class="sxs-lookup"><span data-stu-id="958cf-177">The value of `ChildContent` is received from the parent component and rendered inside the Bootstrap panel's `panel-body`.</span></span>

<span data-ttu-id="958cf-178">*Components/ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="958cf-178">*Components/ChildComponent.razor*:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=3,14-15)]

> [!NOTE]
> <span data-ttu-id="958cf-179">La propiedad que recibe el contenido del `RenderFragment` debe denominarse `ChildContent` por Convención.</span><span class="sxs-lookup"><span data-stu-id="958cf-179">The property receiving the `RenderFragment` content must be named `ChildContent` by convention.</span></span>

<span data-ttu-id="958cf-180">El `ParentComponent` de la aplicación de ejemplo puede proporcionar contenido para representar el `ChildComponent` colocando el contenido dentro de las etiquetas de `<ChildComponent>`.</span><span class="sxs-lookup"><span data-stu-id="958cf-180">The `ParentComponent` in the sample app can provide content for rendering the `ChildComponent` by placing the content inside the `<ChildComponent>` tags.</span></span>

<span data-ttu-id="958cf-181">*Pages/ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="958cf-181">*Pages/ParentComponent.razor*:</span></span>

[!code-razor[](components/samples_snapshot/ParentComponent.razor?highlight=7-8)]

## <a name="attribute-splatting-and-arbitrary-parameters"></a><span data-ttu-id="958cf-182">Atributos expansión y parámetros arbitrarios</span><span class="sxs-lookup"><span data-stu-id="958cf-182">Attribute splatting and arbitrary parameters</span></span>

<span data-ttu-id="958cf-183">Los componentes de pueden capturar y representar atributos adicionales además de los parámetros declarados del componente.</span><span class="sxs-lookup"><span data-stu-id="958cf-183">Components can capture and render additional attributes in addition to the component's declared parameters.</span></span> <span data-ttu-id="958cf-184">Los atributos adicionales se pueden capturar en un diccionario y, después, *splatted* en un elemento cuando el componente se representa mediante la Directiva de Razor [`@attributes`](xref:mvc/views/razor#attributes) .</span><span class="sxs-lookup"><span data-stu-id="958cf-184">Additional attributes can be captured in a dictionary and then *splatted* onto an element when the component is rendered using the [`@attributes`](xref:mvc/views/razor#attributes) Razor directive.</span></span> <span data-ttu-id="958cf-185">Este escenario es útil cuando se define un componente que genera un elemento de marcado que admite diversas personalizaciones.</span><span class="sxs-lookup"><span data-stu-id="958cf-185">This scenario is useful when defining a component that produces a markup element that supports a variety of customizations.</span></span> <span data-ttu-id="958cf-186">Por ejemplo, puede resultar tedioso definir atributos por separado para una `<input>` que admita muchos parámetros.</span><span class="sxs-lookup"><span data-stu-id="958cf-186">For example, it can be tedious to define attributes separately for an `<input>` that supports many parameters.</span></span>

<span data-ttu-id="958cf-187">En el ejemplo siguiente, el primer elemento `<input>` (`id="useIndividualParams"`) utiliza parámetros de componente individuales, mientras que el segundo elemento `<input>` (`id="useAttributesDict"`) utiliza el atributo expansión:</span><span class="sxs-lookup"><span data-stu-id="958cf-187">In the following example, the first `<input>` element (`id="useIndividualParams"`) uses individual component parameters, while the second `<input>` element (`id="useAttributesDict"`) uses attribute splatting:</span></span>

```razor
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

<span data-ttu-id="958cf-188">El tipo del parámetro debe implementar `IEnumerable<KeyValuePair<string, object>>` con claves de cadena.</span><span class="sxs-lookup"><span data-stu-id="958cf-188">The type of the parameter must implement `IEnumerable<KeyValuePair<string, object>>` with string keys.</span></span> <span data-ttu-id="958cf-189">El uso de `IReadOnlyDictionary<string, object>` también es una opción en este escenario.</span><span class="sxs-lookup"><span data-stu-id="958cf-189">Using `IReadOnlyDictionary<string, object>` is also an option in this scenario.</span></span>

<span data-ttu-id="958cf-190">Los elementos `<input>` representados con ambos enfoques son idénticos:</span><span class="sxs-lookup"><span data-stu-id="958cf-190">The rendered `<input>` elements using both approaches is identical:</span></span>

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

<span data-ttu-id="958cf-191">Para aceptar atributos arbitrarios, defina un parámetro de componente mediante el atributo `[Parameter]` con la propiedad `CaptureUnmatchedValues` establecida en `true`:</span><span class="sxs-lookup"><span data-stu-id="958cf-191">To accept arbitrary attributes, define a component parameter using the `[Parameter]` attribute with the `CaptureUnmatchedValues` property set to `true`:</span></span>

```razor
@code {
    [Parameter(CaptureUnmatchedValues = true)]
    public Dictionary<string, object> InputAttributes { get; set; }
}
```

<span data-ttu-id="958cf-192">La propiedad `CaptureUnmatchedValues` en `[Parameter]` permite que el parámetro coincida con todos los atributos que no coinciden con ningún otro parámetro.</span><span class="sxs-lookup"><span data-stu-id="958cf-192">The `CaptureUnmatchedValues` property on `[Parameter]` allows the parameter to match all attributes that don't match any other parameter.</span></span> <span data-ttu-id="958cf-193">Un componente solo puede definir un parámetro con `CaptureUnmatchedValues`.</span><span class="sxs-lookup"><span data-stu-id="958cf-193">A component can only define a single parameter with `CaptureUnmatchedValues`.</span></span> <span data-ttu-id="958cf-194">El tipo de propiedad que se usa con `CaptureUnmatchedValues` se debe poder asignar desde `Dictionary<string, object>` con claves de cadena.</span><span class="sxs-lookup"><span data-stu-id="958cf-194">The property type used with `CaptureUnmatchedValues` must be assignable from `Dictionary<string, object>` with string keys.</span></span> <span data-ttu-id="958cf-195">`IEnumerable<KeyValuePair<string, object>>` o `IReadOnlyDictionary<string, object>` también son opciones en este escenario.</span><span class="sxs-lookup"><span data-stu-id="958cf-195">`IEnumerable<KeyValuePair<string, object>>` or `IReadOnlyDictionary<string, object>` are also options in this scenario.</span></span>

<span data-ttu-id="958cf-196">La posición de `@attributes` relativa a la posición de los atributos de elemento es importante.</span><span class="sxs-lookup"><span data-stu-id="958cf-196">The position of `@attributes` relative to the position of element attributes is important.</span></span> <span data-ttu-id="958cf-197">Cuando `@attributes` se splatted en el elemento, los atributos se procesan de derecha a izquierda (último a primero).</span><span class="sxs-lookup"><span data-stu-id="958cf-197">When `@attributes` are splatted on the element, the attributes are processed from right to left (last to first).</span></span> <span data-ttu-id="958cf-198">Considere el siguiente ejemplo de un componente que utiliza un componente de `Child`:</span><span class="sxs-lookup"><span data-stu-id="958cf-198">Consider the following example of a component that consumes a `Child` component:</span></span>

<span data-ttu-id="958cf-199">*ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="958cf-199">*ParentComponent.razor*:</span></span>

```razor
<ChildComponent extra="10" />
```

<span data-ttu-id="958cf-200">*ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="958cf-200">*ChildComponent.razor*:</span></span>

```razor
<div @attributes="AdditionalAttributes" extra="5" />

[Parameter(CaptureUnmatchedValues = true)]
public IDictionary<string, object> AdditionalAttributes { get; set; }
```

<span data-ttu-id="958cf-201">El atributo `extra` del componente de `Child` se establece a la derecha de `@attributes`.</span><span class="sxs-lookup"><span data-stu-id="958cf-201">The `Child` component's `extra` attribute is set to the right of `@attributes`.</span></span> <span data-ttu-id="958cf-202">El `<div>` representado del componente `Parent` contiene `extra="5"` cuando se pasa a través del atributo adicional, ya que los atributos se procesan de derecha a izquierda (último a primero):</span><span class="sxs-lookup"><span data-stu-id="958cf-202">The `Parent` component's rendered `<div>` contains `extra="5"` when passed through the additional attribute because the attributes are processed right to left (last to first):</span></span>

```html
<div extra="5" />
```

<span data-ttu-id="958cf-203">En el ejemplo siguiente, el orden de `extra` y `@attributes` se invierte en el `<div>`del componente `Child`:</span><span class="sxs-lookup"><span data-stu-id="958cf-203">In the following example, the order of `extra` and `@attributes` is reversed in the `Child` component's `<div>`:</span></span>

<span data-ttu-id="958cf-204">*ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="958cf-204">*ParentComponent.razor*:</span></span>

```razor
<ChildComponent extra="10" />
```

<span data-ttu-id="958cf-205">*ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="958cf-205">*ChildComponent.razor*:</span></span>

```razor
<div extra="5" @attributes="AdditionalAttributes" />

[Parameter(CaptureUnmatchedValues = true)]
public IDictionary<string, object> AdditionalAttributes { get; set; }
```

<span data-ttu-id="958cf-206">El `<div>` representado en el componente `Parent` contiene `extra="10"` cuando se pasa a través del atributo adicional:</span><span class="sxs-lookup"><span data-stu-id="958cf-206">The rendered `<div>` in the `Parent` component contains `extra="10"` when passed through the additional attribute:</span></span>

```html
<div extra="10" />
```

## <a name="capture-references-to-components"></a><span data-ttu-id="958cf-207">Capturar referencias a componentes</span><span class="sxs-lookup"><span data-stu-id="958cf-207">Capture references to components</span></span>

<span data-ttu-id="958cf-208">Las referencias de componente proporcionan una manera de hacer referencia a una instancia de componente para que pueda emitir comandos a esa instancia, como `Show` o `Reset`.</span><span class="sxs-lookup"><span data-stu-id="958cf-208">Component references provide a way to reference a component instance so that you can issue commands to that instance, such as `Show` or `Reset`.</span></span> <span data-ttu-id="958cf-209">Para capturar una referencia de componente:</span><span class="sxs-lookup"><span data-stu-id="958cf-209">To capture a component reference:</span></span>

* <span data-ttu-id="958cf-210">Agregue un atributo [`@ref`](xref:mvc/views/razor#ref) al componente secundario.</span><span class="sxs-lookup"><span data-stu-id="958cf-210">Add an [`@ref`](xref:mvc/views/razor#ref) attribute to the child component.</span></span>
* <span data-ttu-id="958cf-211">Defina un campo con el mismo tipo que el componente secundario.</span><span class="sxs-lookup"><span data-stu-id="958cf-211">Define a field with the same type as the child component.</span></span>

```razor
<MyLoginDialog @ref="_loginDialog" ... />

@code {
    private MyLoginDialog _loginDialog;

    private void OnSomething()
    {
        _loginDialog.Show();
    }
}
```

<span data-ttu-id="958cf-212">Cuando se representa el componente, el campo de `_loginDialog` se rellena con la instancia del componente secundario `MyLoginDialog`.</span><span class="sxs-lookup"><span data-stu-id="958cf-212">When the component is rendered, the `_loginDialog` field is populated with the `MyLoginDialog` child component instance.</span></span> <span data-ttu-id="958cf-213">Después, puede invocar métodos .NET en la instancia del componente.</span><span class="sxs-lookup"><span data-stu-id="958cf-213">You can then invoke .NET methods on the component instance.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="958cf-214">La variable `_loginDialog` solo se rellena después de que el componente se represente y su salida incluye el elemento `MyLoginDialog`.</span><span class="sxs-lookup"><span data-stu-id="958cf-214">The `_loginDialog` variable is only populated after the component is rendered and its output includes the `MyLoginDialog` element.</span></span> <span data-ttu-id="958cf-215">Hasta ese momento, no hay nada que hacer referencia.</span><span class="sxs-lookup"><span data-stu-id="958cf-215">Until that point, there's nothing to reference.</span></span> <span data-ttu-id="958cf-216">Para manipular las referencias de componentes una vez finalizada la representación del componente, use los [métodos OnAfterRenderAsync o OnAfterRender](xref:blazor/lifecycle#after-component-render).</span><span class="sxs-lookup"><span data-stu-id="958cf-216">To manipulate components references after the component has finished rendering, use the [OnAfterRenderAsync or OnAfterRender methods](xref:blazor/lifecycle#after-component-render).</span></span>

<span data-ttu-id="958cf-217">Al capturar referencias de componentes, use una sintaxis similar para [capturar referencias de elemento](xref:blazor/javascript-interop#capture-references-to-elements), no es una característica de [interoperabilidad de JavaScript](xref:blazor/javascript-interop) .</span><span class="sxs-lookup"><span data-stu-id="958cf-217">While capturing component references use a similar syntax to [capturing element references](xref:blazor/javascript-interop#capture-references-to-elements), it isn't a [JavaScript interop](xref:blazor/javascript-interop) feature.</span></span> <span data-ttu-id="958cf-218">Las referencias a componentes no se pasan al código JavaScript&mdash;solo se usan en código .NET.</span><span class="sxs-lookup"><span data-stu-id="958cf-218">Component references aren't passed to JavaScript code&mdash;they're only used in .NET code.</span></span>

> [!NOTE]
> <span data-ttu-id="958cf-219">**No** utilice referencias de componentes para mutar el estado de los componentes secundarios.</span><span class="sxs-lookup"><span data-stu-id="958cf-219">Do **not** use component references to mutate the state of child components.</span></span> <span data-ttu-id="958cf-220">En su lugar, use parámetros declarativos normales para pasar datos a componentes secundarios.</span><span class="sxs-lookup"><span data-stu-id="958cf-220">Instead, use normal declarative parameters to pass data to child components.</span></span> <span data-ttu-id="958cf-221">El uso de parámetros declarativos normales da como resultado componentes secundarios que se representarán automáticamente en las horas correctas.</span><span class="sxs-lookup"><span data-stu-id="958cf-221">Use of normal declarative parameters result in child components that rerender at the correct times automatically.</span></span>

## <a name="invoke-component-methods-externally-to-update-state"></a><span data-ttu-id="958cf-222">Invocar métodos de componentes externamente para actualizar el estado</span><span class="sxs-lookup"><span data-stu-id="958cf-222">Invoke component methods externally to update state</span></span>

Blazor<span data-ttu-id="958cf-223"> usa un `SynchronizationContext` para aplicar un único subproceso lógico de ejecución.</span><span class="sxs-lookup"><span data-stu-id="958cf-223"> uses a `SynchronizationContext` to enforce a single logical thread of execution.</span></span> <span data-ttu-id="958cf-224">[Los métodos de ciclo de vida](xref:blazor/lifecycle) de un componente y las devoluciones de llamada de eventos que se producen en Blazor se ejecutan en esta `SynchronizationContext`.</span><span class="sxs-lookup"><span data-stu-id="958cf-224">A component's [lifecycle methods](xref:blazor/lifecycle) and any event callbacks that are raised by Blazor are executed on this `SynchronizationContext`.</span></span> <span data-ttu-id="958cf-225">En caso de que un componente deba actualizarse en función de un evento externo, como un temporizador u otras notificaciones, use el método `InvokeAsync`, que se enviará al `SynchronizationContext`de Blazor.</span><span class="sxs-lookup"><span data-stu-id="958cf-225">In the event a component must be updated based on an external event, such as a timer or other notifications, use the `InvokeAsync` method, which will dispatch to Blazor's `SynchronizationContext`.</span></span>

<span data-ttu-id="958cf-226">Por ejemplo, considere un *servicio de notificador* que puede notificar a cualquier componente de escucha del estado actualizado:</span><span class="sxs-lookup"><span data-stu-id="958cf-226">For example, consider a *notifier service* that can notify any listening component of the updated state:</span></span>

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

<span data-ttu-id="958cf-227">Registre el `NotifierService` como singletion:</span><span class="sxs-lookup"><span data-stu-id="958cf-227">Register the `NotifierService` as a singletion:</span></span>

* <span data-ttu-id="958cf-228">En Blazor webassembly, registre el servicio en `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="958cf-228">In Blazor WebAssembly, register the service in `Program.Main`:</span></span>

  ```csharp
  builder.Services.AddSingleton<NotifierService>();
  ```

* <span data-ttu-id="958cf-229">En Blazor Server, registre el servicio en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="958cf-229">In Blazor Server, register the service in `Startup.ConfigureServices`:</span></span>

  ```csharp
  services.AddSingleton<NotifierService>();
  ```

<span data-ttu-id="958cf-230">Use el `NotifierService` para actualizar un componente:</span><span class="sxs-lookup"><span data-stu-id="958cf-230">Use the `NotifierService` to update a component:</span></span>

```razor
@page "/"
@inject NotifierService Notifier
@implements IDisposable

<p>Last update: @_lastNotification.key = @_lastNotification.value</p>

@code {
    private (string key, int value) _lastNotification;

    protected override void OnInitialized()
    {
        Notifier.Notify += OnNotify;
    }

    public async Task OnNotify(string key, int value)
    {
        await InvokeAsync(() =>
        {
            _lastNotification = (key, value);
            StateHasChanged();
        });
    }

    public void Dispose()
    {
        Notifier.Notify -= OnNotify;
    }
}
```

<span data-ttu-id="958cf-231">En el ejemplo anterior, `NotifierService` invoca el método de `OnNotify` del componente fuera del `SynchronizationContext`de Blazor.</span><span class="sxs-lookup"><span data-stu-id="958cf-231">In the preceding example, `NotifierService` invokes the component's `OnNotify` method outside of Blazor's `SynchronizationContext`.</span></span> <span data-ttu-id="958cf-232">`InvokeAsync` se utiliza para cambiar al contexto correcto y poner en cola una representación.</span><span class="sxs-lookup"><span data-stu-id="958cf-232">`InvokeAsync` is used to switch to the correct context and queue a render.</span></span>

## <a name="use-key-to-control-the-preservation-of-elements-and-components"></a><span data-ttu-id="958cf-233">Usar \@clave para controlar la preservación de elementos y componentes</span><span class="sxs-lookup"><span data-stu-id="958cf-233">Use \@key to control the preservation of elements and components</span></span>

<span data-ttu-id="958cf-234">Cuando se representa una lista de elementos o componentes, y los elementos o componentes cambian posteriormente, el algoritmo de comparación de Blazordebe decidir cuáles de los elementos o componentes anteriores se pueden conservar y cómo deben asignarse los objetos de modelo.</span><span class="sxs-lookup"><span data-stu-id="958cf-234">When rendering a list of elements or components and the elements or components subsequently change, Blazor's diffing algorithm must decide which of the previous elements or components can be retained and how model objects should map to them.</span></span> <span data-ttu-id="958cf-235">Normalmente, este proceso es automático y se puede omitir, pero hay casos en los que puede que desee controlar el proceso.</span><span class="sxs-lookup"><span data-stu-id="958cf-235">Normally, this process is automatic and can be ignored, but there are cases where you may want to control the process.</span></span>

<span data-ttu-id="958cf-236">Considere el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="958cf-236">Consider the following example:</span></span>

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

<span data-ttu-id="958cf-237">El contenido de la colección de `People` puede cambiar con entradas insertadas, eliminadas o reordenadas.</span><span class="sxs-lookup"><span data-stu-id="958cf-237">The contents of the `People` collection may change with inserted, deleted, or re-ordered entries.</span></span> <span data-ttu-id="958cf-238">Cuando se representa el componente, el componente de `<DetailsEditor>` puede cambiar para recibir diferentes valores de parámetro de `Details`.</span><span class="sxs-lookup"><span data-stu-id="958cf-238">When the component rerenders, the `<DetailsEditor>` component may change to receive different `Details` parameter values.</span></span> <span data-ttu-id="958cf-239">Esto puede producir una rerepresentación más compleja de lo esperado.</span><span class="sxs-lookup"><span data-stu-id="958cf-239">This may cause more complex rerendering than expected.</span></span> <span data-ttu-id="958cf-240">En algunos casos, la rerepresentación puede producir diferencias de comportamiento visibles, como el foco del elemento perdido.</span><span class="sxs-lookup"><span data-stu-id="958cf-240">In some cases, rerendering can lead to visible behavior differences, such as lost element focus.</span></span>

<span data-ttu-id="958cf-241">El proceso de asignación se puede controlar con el `@key` atributo de directiva.</span><span class="sxs-lookup"><span data-stu-id="958cf-241">The mapping process can be controlled with the `@key` directive attribute.</span></span> <span data-ttu-id="958cf-242">`@key` hace que el algoritmo de comparación garantice la preservación de elementos o componentes basados en el valor de la clave:</span><span class="sxs-lookup"><span data-stu-id="958cf-242">`@key` causes the diffing algorithm to guarantee preservation of elements or components based on the key's value:</span></span>

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

<span data-ttu-id="958cf-243">Cuando cambia la colección de `People`, el algoritmo de comparación mantiene la asociación entre `<DetailsEditor>` instancias y `person` instancias:</span><span class="sxs-lookup"><span data-stu-id="958cf-243">When the `People` collection changes, the diffing algorithm retains the association between `<DetailsEditor>` instances and `person` instances:</span></span>

* <span data-ttu-id="958cf-244">Si se elimina un `Person` de la lista de `People`, solo se quita la instancia de `<DetailsEditor>` correspondiente de la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="958cf-244">If a `Person` is deleted from the `People` list, only the corresponding `<DetailsEditor>` instance is removed from the UI.</span></span> <span data-ttu-id="958cf-245">Otras instancias permanecen sin cambios.</span><span class="sxs-lookup"><span data-stu-id="958cf-245">Other instances are left unchanged.</span></span>
* <span data-ttu-id="958cf-246">Si se inserta un `Person` en alguna posición de la lista, se inserta una nueva instancia de `<DetailsEditor>` en la posición correspondiente.</span><span class="sxs-lookup"><span data-stu-id="958cf-246">If a `Person` is inserted at some position in the list, one new `<DetailsEditor>` instance is inserted at that corresponding position.</span></span> <span data-ttu-id="958cf-247">Otras instancias permanecen sin cambios.</span><span class="sxs-lookup"><span data-stu-id="958cf-247">Other instances are left unchanged.</span></span>
* <span data-ttu-id="958cf-248">Si se vuelven a ordenar `Person` entradas, se conservan y se vuelven a ordenar las instancias de `<DetailsEditor>` correspondientes en la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="958cf-248">If `Person` entries are re-ordered, the corresponding `<DetailsEditor>` instances are preserved and re-ordered in the UI.</span></span>

<span data-ttu-id="958cf-249">En algunos escenarios, el uso de `@key` minimiza la complejidad de la rerepresentación y evita posibles problemas con las partes con estado del DOM que cambian, como la posición del foco.</span><span class="sxs-lookup"><span data-stu-id="958cf-249">In some scenarios, use of `@key` minimizes the complexity of rerendering and avoids potential issues with stateful parts of the DOM changing, such as focus position.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="958cf-250">Las claves son locales para cada elemento contenedor o componente.</span><span class="sxs-lookup"><span data-stu-id="958cf-250">Keys are local to each container element or component.</span></span> <span data-ttu-id="958cf-251">Las claves no se comparan globalmente en todo el documento.</span><span class="sxs-lookup"><span data-stu-id="958cf-251">Keys aren't compared globally across the document.</span></span>

### <a name="when-to-use-key"></a><span data-ttu-id="958cf-252">Cuándo usar la clave de \@</span><span class="sxs-lookup"><span data-stu-id="958cf-252">When to use \@key</span></span>

<span data-ttu-id="958cf-253">Normalmente, tiene sentido usar `@key` cada vez que se representa una lista (por ejemplo, en un bloque de `@foreach`) y existe un valor adecuado para definir el `@key`.</span><span class="sxs-lookup"><span data-stu-id="958cf-253">Typically, it makes sense to use `@key` whenever a list is rendered (for example, in a `@foreach` block) and a suitable value exists to define the `@key`.</span></span>

<span data-ttu-id="958cf-254">También puede usar `@key` para evitar que Blazor conserven un subárbol de elementos o componentes cuando cambie un objeto:</span><span class="sxs-lookup"><span data-stu-id="958cf-254">You can also use `@key` to prevent Blazor from preserving an element or component subtree when an object changes:</span></span>

```razor
<div @key="currentPerson">
    ... content that depends on currentPerson ...
</div>
```

<span data-ttu-id="958cf-255">Si `@currentPerson` cambia, la Directiva de `@key` atributo fuerza a Blazor a descartar toda la `<div>` y sus descendientes y volver a generar el subárbol dentro de la interfaz de usuario con nuevos elementos y componentes.</span><span class="sxs-lookup"><span data-stu-id="958cf-255">If `@currentPerson` changes, the `@key` attribute directive forces Blazor to discard the entire `<div>` and its descendants and rebuild the subtree within the UI with new elements and components.</span></span> <span data-ttu-id="958cf-256">Esto puede ser útil si necesita garantizar que no se conserva ningún estado de la interfaz de usuario cuando `@currentPerson` cambios.</span><span class="sxs-lookup"><span data-stu-id="958cf-256">This can be useful if you need to guarantee that no UI state is preserved when `@currentPerson` changes.</span></span>

### <a name="when-not-to-use-key"></a><span data-ttu-id="958cf-257">Cuándo no usar la clave \@</span><span class="sxs-lookup"><span data-stu-id="958cf-257">When not to use \@key</span></span>

<span data-ttu-id="958cf-258">Existe un costo de rendimiento al diferenciar con `@key`.</span><span class="sxs-lookup"><span data-stu-id="958cf-258">There's a performance cost when diffing with `@key`.</span></span> <span data-ttu-id="958cf-259">El costo de rendimiento no es grande, pero solo especifica `@key` si el control del elemento o las reglas de conservación de componentes se benefician de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="958cf-259">The performance cost isn't large, but only specify `@key` if controlling the element or component preservation rules benefit the app.</span></span>

<span data-ttu-id="958cf-260">Incluso si no se utiliza `@key`, Blazor conserva las instancias de componente y elemento secundario lo máximo posible.</span><span class="sxs-lookup"><span data-stu-id="958cf-260">Even if `@key` isn't used, Blazor preserves child element and component instances as much as possible.</span></span> <span data-ttu-id="958cf-261">La única ventaja de utilizar `@key` es el control sobre *Cómo* se asignan las instancias de modelo a las instancias de componente conservadas, en lugar del algoritmo de diferenciación que selecciona la asignación.</span><span class="sxs-lookup"><span data-stu-id="958cf-261">The only advantage to using `@key` is control over *how* model instances are mapped to the preserved component instances, instead of the diffing algorithm selecting the mapping.</span></span>

### <a name="what-values-to-use-for-key"></a><span data-ttu-id="958cf-262">Qué valores se deben usar para la clave de \@</span><span class="sxs-lookup"><span data-stu-id="958cf-262">What values to use for \@key</span></span>

<span data-ttu-id="958cf-263">Por lo general, tiene sentido proporcionar uno de los siguientes tipos de valor para `@key`:</span><span class="sxs-lookup"><span data-stu-id="958cf-263">Generally, it makes sense to supply one of the following kinds of value for `@key`:</span></span>

* <span data-ttu-id="958cf-264">Instancias de objeto de modelo (por ejemplo, una instancia de `Person` como en el ejemplo anterior).</span><span class="sxs-lookup"><span data-stu-id="958cf-264">Model object instances (for example, a `Person` instance as in the earlier example).</span></span> <span data-ttu-id="958cf-265">Esto garantiza la conservación en función de la igualdad de la referencia de objeto.</span><span class="sxs-lookup"><span data-stu-id="958cf-265">This ensures preservation based on object reference equality.</span></span>
* <span data-ttu-id="958cf-266">Los identificadores únicos (por ejemplo, los valores de clave principal de tipo `int`, `string`o `Guid`).</span><span class="sxs-lookup"><span data-stu-id="958cf-266">Unique identifiers (for example, primary key values of type `int`, `string`, or `Guid`).</span></span>

<span data-ttu-id="958cf-267">Asegúrese de que los valores usados para `@key` no entren en conflicto.</span><span class="sxs-lookup"><span data-stu-id="958cf-267">Ensure that values used for `@key` don't clash.</span></span> <span data-ttu-id="958cf-268">Si se detectan valores en conflicto en el mismo elemento primario, Blazor produce una excepción porque no puede asignar de forma determinista elementos o componentes antiguos a nuevos elementos o componentes.</span><span class="sxs-lookup"><span data-stu-id="958cf-268">If clashing values are detected within the same parent element, Blazor throws an exception because it can't deterministically map old elements or components to new elements or components.</span></span> <span data-ttu-id="958cf-269">Utilice solo valores distintos, como instancias de objeto o valores de clave principal.</span><span class="sxs-lookup"><span data-stu-id="958cf-269">Only use distinct values, such as object instances or primary key values.</span></span>

## <a name="partial-class-support"></a><span data-ttu-id="958cf-270">Compatibilidad de clases parciales</span><span class="sxs-lookup"><span data-stu-id="958cf-270">Partial class support</span></span>

<span data-ttu-id="958cf-271">Los componentes de Razor se generan como clases parciales.</span><span class="sxs-lookup"><span data-stu-id="958cf-271">Razor components are generated as partial classes.</span></span> <span data-ttu-id="958cf-272">Los componentes de Razor se crean mediante cualquiera de los métodos siguientes:</span><span class="sxs-lookup"><span data-stu-id="958cf-272">Razor components are authored using either of the following approaches:</span></span>

* <span data-ttu-id="958cf-273">C#el código se define en un bloque [`@code`](xref:mvc/views/razor#code) con marcado HTML y código Razor en un único archivo.</span><span class="sxs-lookup"><span data-stu-id="958cf-273">C# code is defined in an [`@code`](xref:mvc/views/razor#code) block with HTML markup and Razor code in a single file.</span></span> Blazor<span data-ttu-id="958cf-274"> plantillas definen sus componentes de Razor mediante este enfoque.</span><span class="sxs-lookup"><span data-stu-id="958cf-274"> templates define their Razor components using this approach.</span></span>
* <span data-ttu-id="958cf-275">C#el código se coloca en un archivo de código subyacente definido como una clase parcial.</span><span class="sxs-lookup"><span data-stu-id="958cf-275">C# code is placed in a code-behind file defined as a partial class.</span></span>

<span data-ttu-id="958cf-276">En el ejemplo siguiente se muestra el componente de `Counter` predeterminado con un bloque de `@code` en una aplicación generada a partir de una plantilla de Blazor.</span><span class="sxs-lookup"><span data-stu-id="958cf-276">The following example shows the default `Counter` component with an `@code` block in an app generated from a Blazor template.</span></span> <span data-ttu-id="958cf-277">El marcado HTML, el código Razor C# y el código se encuentran en el mismo archivo:</span><span class="sxs-lookup"><span data-stu-id="958cf-277">HTML markup, Razor code, and C# code are in the same file:</span></span>

<span data-ttu-id="958cf-278">*Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="958cf-278">*Counter.razor*:</span></span>

```razor
@page "/counter"

<h1>Counter</h1>

<p>Current count: @_currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>

@code {
    private int _currentCount = 0;

    void IncrementCount()
    {
        _currentCount++;
    }
}
```

<span data-ttu-id="958cf-279">El componente de `Counter` también se puede crear con un archivo de código subyacente con una clase parcial:</span><span class="sxs-lookup"><span data-stu-id="958cf-279">The `Counter` component can also be created using a code-behind file with a partial class:</span></span>

<span data-ttu-id="958cf-280">*Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="958cf-280">*Counter.razor*:</span></span>

```razor
@page "/counter"

<h1>Counter</h1>

<p>Current count: @_currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>
```

<span data-ttu-id="958cf-281">*Counter.Razor.CS*:</span><span class="sxs-lookup"><span data-stu-id="958cf-281">*Counter.razor.cs*:</span></span>

```csharp
namespace BlazorApp.Pages
{
    public partial class Counter
    {
        private int _currentCount = 0;

        void IncrementCount()
        {
            _currentCount++;
        }
    }
}
```

<span data-ttu-id="958cf-282">Agregue los espacios de nombres necesarios al archivo de clase parcial según sea necesario.</span><span class="sxs-lookup"><span data-stu-id="958cf-282">Add any required namespaces to the partial class file as needed.</span></span> <span data-ttu-id="958cf-283">Los espacios de nombres típicos utilizados por los componentes de Razor incluyen:</span><span class="sxs-lookup"><span data-stu-id="958cf-283">Typical namespaces used by Razor components include:</span></span>

```csharp
using Microsoft.AspNetCore.Authorization;
using Microsoft.AspNetCore.Components;
using Microsoft.AspNetCore.Components.Authorization;
using Microsoft.AspNetCore.Components.Forms;
using Microsoft.AspNetCore.Components.Routing;
using Microsoft.AspNetCore.Components.Web;
```

## <a name="specify-a-base-class"></a><span data-ttu-id="958cf-284">Especificar una clase base</span><span class="sxs-lookup"><span data-stu-id="958cf-284">Specify a base class</span></span>

<span data-ttu-id="958cf-285">La directiva [`@inherits`](xref:mvc/views/razor#inherits) se puede utilizar para especificar una clase base para un componente.</span><span class="sxs-lookup"><span data-stu-id="958cf-285">The [`@inherits`](xref:mvc/views/razor#inherits) directive can be used to specify a base class for a component.</span></span> <span data-ttu-id="958cf-286">En el ejemplo siguiente se muestra cómo un componente puede heredar una clase base, `BlazorRocksBase`, para proporcionar las propiedades y los métodos del componente.</span><span class="sxs-lookup"><span data-stu-id="958cf-286">The following example shows how a component can inherit a base class, `BlazorRocksBase`, to provide the component's properties and methods.</span></span> <span data-ttu-id="958cf-287">La clase base debe derivar de `ComponentBase`.</span><span class="sxs-lookup"><span data-stu-id="958cf-287">The base class should derive from `ComponentBase`.</span></span>

<span data-ttu-id="958cf-288">*Pages/BlazorRocks. Razor*:</span><span class="sxs-lookup"><span data-stu-id="958cf-288">*Pages/BlazorRocks.razor*:</span></span>

```razor
@page "/BlazorRocks"
@inherits BlazorRocksBase

<h1>@BlazorRocksText</h1>
```

<span data-ttu-id="958cf-289">*BlazorRocksBase.CS*:</span><span class="sxs-lookup"><span data-stu-id="958cf-289">*BlazorRocksBase.cs*:</span></span>

```csharp
using Microsoft.AspNetCore.Components;

namespace BlazorSample
{
    public class BlazorRocksBase : ComponentBase
    {
        public string BlazorRocksText { get; set; } = 
            "Blazor rocks the browser!";
    }
}
```

## <a name="specify-an-attribute"></a><span data-ttu-id="958cf-290">Especificar un atributo</span><span class="sxs-lookup"><span data-stu-id="958cf-290">Specify an attribute</span></span>

<span data-ttu-id="958cf-291">Los atributos se pueden especificar en los componentes de Razor con la directiva [`@attribute`](xref:mvc/views/razor#attribute) .</span><span class="sxs-lookup"><span data-stu-id="958cf-291">Attributes can be specified in Razor components with the [`@attribute`](xref:mvc/views/razor#attribute) directive.</span></span> <span data-ttu-id="958cf-292">En el ejemplo siguiente se aplica el atributo `[Authorize]` a la clase Component:</span><span class="sxs-lookup"><span data-stu-id="958cf-292">The following example applies the `[Authorize]` attribute to the component class:</span></span>

```razor
@page "/"
@attribute [Authorize]
```

## <a name="import-components"></a><span data-ttu-id="958cf-293">Importar componentes</span><span class="sxs-lookup"><span data-stu-id="958cf-293">Import components</span></span>

<span data-ttu-id="958cf-294">El espacio de nombres de un componente creado con Razor se basa en (en orden de prioridad):</span><span class="sxs-lookup"><span data-stu-id="958cf-294">The namespace of a component authored with Razor is based on (in priority order):</span></span>

* <span data-ttu-id="958cf-295">[`@namespace`](xref:mvc/views/razor#namespace) designación en el marcado de archivos Razor ( *. Razor*) (`@namespace BlazorSample.MyNamespace`).</span><span class="sxs-lookup"><span data-stu-id="958cf-295">[`@namespace`](xref:mvc/views/razor#namespace) designation in Razor file (*.razor*) markup (`@namespace BlazorSample.MyNamespace`).</span></span>
* <span data-ttu-id="958cf-296">`RootNamespace` del proyecto en el archivo de proyecto (`<RootNamespace>BlazorSample</RootNamespace>`).</span><span class="sxs-lookup"><span data-stu-id="958cf-296">The project's `RootNamespace` in the project file (`<RootNamespace>BlazorSample</RootNamespace>`).</span></span>
* <span data-ttu-id="958cf-297">Nombre del proyecto, tomado del nombre de archivo del archivo de proyecto ( *. csproj*) y la ruta de acceso de la raíz del proyecto al componente.</span><span class="sxs-lookup"><span data-stu-id="958cf-297">The project name, taken from the project file's file name (*.csproj*), and the path from the project root to the component.</span></span> <span data-ttu-id="958cf-298">Por ejemplo, el marco de trabajo resuelve *{root Project}/pages/index.Razor* (*BlazorSample. csproj*) en el espacio de nombres `BlazorSample.Pages`.</span><span class="sxs-lookup"><span data-stu-id="958cf-298">For example, the framework resolves *{PROJECT ROOT}/Pages/Index.razor* (*BlazorSample.csproj*) to the namespace `BlazorSample.Pages`.</span></span> <span data-ttu-id="958cf-299">Los componentes C# siguen las reglas de enlace de nombres.</span><span class="sxs-lookup"><span data-stu-id="958cf-299">Components follow C# name binding rules.</span></span> <span data-ttu-id="958cf-300">En el caso del componente `Index` en este ejemplo, los componentes del ámbito son todos los componentes:</span><span class="sxs-lookup"><span data-stu-id="958cf-300">For the `Index` component in this example, the components in scope are all of the components:</span></span>
  * <span data-ttu-id="958cf-301">En la misma carpeta, *páginas*.</span><span class="sxs-lookup"><span data-stu-id="958cf-301">In the same folder, *Pages*.</span></span>
  * <span data-ttu-id="958cf-302">Los componentes de la raíz del proyecto que no especifican explícitamente un espacio de nombres diferente.</span><span class="sxs-lookup"><span data-stu-id="958cf-302">The components in the project's root that don't explicitly specify a different namespace.</span></span>

<span data-ttu-id="958cf-303">Los componentes definidos en un espacio de nombres diferente se incluyen en el ámbito mediante la Directiva de [`@using`](xref:mvc/views/razor#using) de Razor.</span><span class="sxs-lookup"><span data-stu-id="958cf-303">Components defined in a different namespace are brought into scope using Razor's [`@using`](xref:mvc/views/razor#using) directive.</span></span>

<span data-ttu-id="958cf-304">Si existe otro componente, `NavMenu.razor`, en la carpeta *BlazorSample/Shared/* , el componente se puede utilizar en `Index.razor` con la siguiente instrucción `@using`:</span><span class="sxs-lookup"><span data-stu-id="958cf-304">If another component, `NavMenu.razor`, exists in the *BlazorSample/Shared/* folder, the component can be used in `Index.razor` with the following `@using` statement:</span></span>

```razor
@using BlazorSample.Shared

This is the Index page.

<NavMenu></NavMenu>
```

<span data-ttu-id="958cf-305">También se puede hacer referencia a los componentes mediante sus nombres completos, que no requieren la directiva [`@using`](xref:mvc/views/razor#using) :</span><span class="sxs-lookup"><span data-stu-id="958cf-305">Components can also be referenced using their fully qualified names, which doesn't require the [`@using`](xref:mvc/views/razor#using) directive:</span></span>

```razor
This is the Index page.

<BlazorSample.Shared.NavMenu></BlazorSample.Shared.NavMenu>
```

> [!NOTE]
> <span data-ttu-id="958cf-306">No se admite la calificación `global::`.</span><span class="sxs-lookup"><span data-stu-id="958cf-306">The `global::` qualification isn't supported.</span></span>
>
> <span data-ttu-id="958cf-307">No se admite la importación de componentes con instrucciones de `using` con alias (por ejemplo, `@using Foo = Bar`).</span><span class="sxs-lookup"><span data-stu-id="958cf-307">Importing components with aliased `using` statements (for example, `@using Foo = Bar`) isn't supported.</span></span>
>
> <span data-ttu-id="958cf-308">No se admiten nombres parcialmente completos.</span><span class="sxs-lookup"><span data-stu-id="958cf-308">Partially qualified names aren't supported.</span></span> <span data-ttu-id="958cf-309">Por ejemplo, no se admite agregar `@using BlazorSample` y hacer referencia a `NavMenu.razor` con `<Shared.NavMenu></Shared.NavMenu>`.</span><span class="sxs-lookup"><span data-stu-id="958cf-309">For example, adding `@using BlazorSample` and referencing `NavMenu.razor` with `<Shared.NavMenu></Shared.NavMenu>` isn't supported.</span></span>

## <a name="conditional-html-element-attributes"></a><span data-ttu-id="958cf-310">Atributos de elementos HTML condicionales</span><span class="sxs-lookup"><span data-stu-id="958cf-310">Conditional HTML element attributes</span></span>

<span data-ttu-id="958cf-311">Los atributos de elemento HTML se representan condicionalmente según el valor de .NET.</span><span class="sxs-lookup"><span data-stu-id="958cf-311">HTML element attributes are conditionally rendered based on the .NET value.</span></span> <span data-ttu-id="958cf-312">Si el valor es `false` o `null`, el atributo no se representa.</span><span class="sxs-lookup"><span data-stu-id="958cf-312">If the value is `false` or `null`, the attribute isn't rendered.</span></span> <span data-ttu-id="958cf-313">Si el valor es `true`, el atributo se representa minimizado.</span><span class="sxs-lookup"><span data-stu-id="958cf-313">If the value is `true`, the attribute is rendered minimized.</span></span>

<span data-ttu-id="958cf-314">En el ejemplo siguiente, `IsCompleted` determina si `checked` se representa en el marcado del elemento:</span><span class="sxs-lookup"><span data-stu-id="958cf-314">In the following example, `IsCompleted` determines if `checked` is rendered in the element's markup:</span></span>

```razor
<input type="checkbox" checked="@IsCompleted" />

@code {
    [Parameter]
    public bool IsCompleted { get; set; }
}
```

<span data-ttu-id="958cf-315">Si `IsCompleted` se `true`, la casilla se representa como:</span><span class="sxs-lookup"><span data-stu-id="958cf-315">If `IsCompleted` is `true`, the check box is rendered as:</span></span>

```html
<input type="checkbox" checked />
```

<span data-ttu-id="958cf-316">Si `IsCompleted` se `false`, la casilla se representa como:</span><span class="sxs-lookup"><span data-stu-id="958cf-316">If `IsCompleted` is `false`, the check box is rendered as:</span></span>

```html
<input type="checkbox" />
```

<span data-ttu-id="958cf-317">Para más información, consulte <xref:mvc/views/razor>.</span><span class="sxs-lookup"><span data-stu-id="958cf-317">For more information, see <xref:mvc/views/razor>.</span></span>

> [!WARNING]
> <span data-ttu-id="958cf-318">Algunos atributos HTML, como [Aria-pressed](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/button_role#Toggle_buttons), no funcionan correctamente cuando el tipo .net es un `bool`.</span><span class="sxs-lookup"><span data-stu-id="958cf-318">Some HTML attributes, such as [aria-pressed](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/button_role#Toggle_buttons), don't function properly when the .NET type is a `bool`.</span></span> <span data-ttu-id="958cf-319">En esos casos, use un tipo de `string` en lugar de un `bool`.</span><span class="sxs-lookup"><span data-stu-id="958cf-319">In those cases, use a `string` type instead of a `bool`.</span></span>

## <a name="raw-html"></a><span data-ttu-id="958cf-320">HTML sin formato</span><span class="sxs-lookup"><span data-stu-id="958cf-320">Raw HTML</span></span>

<span data-ttu-id="958cf-321">Normalmente, las cadenas se representan mediante nodos de texto DOM, lo que significa que cualquier marcado que pueda contener se omite y se trata como texto literal.</span><span class="sxs-lookup"><span data-stu-id="958cf-321">Strings are normally rendered using DOM text nodes, which means that any markup they may contain is ignored and treated as literal text.</span></span> <span data-ttu-id="958cf-322">Para representar HTML sin formato, ajuste el contenido HTML en un valor de `MarkupString`.</span><span class="sxs-lookup"><span data-stu-id="958cf-322">To render raw HTML, wrap the HTML content in a `MarkupString` value.</span></span> <span data-ttu-id="958cf-323">El valor se analiza como HTML o SVG y se inserta en el DOM.</span><span class="sxs-lookup"><span data-stu-id="958cf-323">The value is parsed as HTML or SVG and inserted into the DOM.</span></span>

> [!WARNING]
> <span data-ttu-id="958cf-324">La representación de HTML sin formato construido a partir de un origen que no es de confianza es un riesgo para la **seguridad** y debe evitarse.</span><span class="sxs-lookup"><span data-stu-id="958cf-324">Rendering raw HTML constructed from any untrusted source is a **security risk** and should be avoided!</span></span>

<span data-ttu-id="958cf-325">En el ejemplo siguiente se muestra el uso del tipo de `MarkupString` para agregar un bloque de contenido HTML estático a la salida representada de un componente:</span><span class="sxs-lookup"><span data-stu-id="958cf-325">The following example shows using the `MarkupString` type to add a block of static HTML content to the rendered output of a component:</span></span>

```html
@((MarkupString)_myMarkup)

@code {
    private string _myMarkup = 
        "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="cascading-values-and-parameters"></a><span data-ttu-id="958cf-326">Valores y parámetros en cascada</span><span class="sxs-lookup"><span data-stu-id="958cf-326">Cascading values and parameters</span></span>

<span data-ttu-id="958cf-327">En algunos escenarios, no es conveniente fluir los datos de un componente antecesor a un componente descendiente mediante [parámetros de componente](#component-parameters), especialmente cuando hay varios niveles de componentes.</span><span class="sxs-lookup"><span data-stu-id="958cf-327">In some scenarios, it's inconvenient to flow data from an ancestor component to a descendent component using [component parameters](#component-parameters), especially when there are several component layers.</span></span> <span data-ttu-id="958cf-328">Los valores y parámetros en cascada solucionan este problema proporcionando un método cómodo para que un componente antecesor proporcione un valor a todos sus componentes descendientes.</span><span class="sxs-lookup"><span data-stu-id="958cf-328">Cascading values and parameters solve this problem by providing a convenient way for an ancestor component to provide a value to all of its descendent components.</span></span> <span data-ttu-id="958cf-329">Los valores y parámetros en cascada también proporcionan un enfoque para que los componentes se coordinen.</span><span class="sxs-lookup"><span data-stu-id="958cf-329">Cascading values and parameters also provide an approach for components to coordinate.</span></span>

### <a name="theme-example"></a><span data-ttu-id="958cf-330">Ejemplo de tema</span><span class="sxs-lookup"><span data-stu-id="958cf-330">Theme example</span></span>

<span data-ttu-id="958cf-331">En el ejemplo siguiente de la aplicación de ejemplo, la clase `ThemeInfo` especifica la información del tema que va a fluir hacia abajo en la jerarquía de componentes para que todos los botones de una parte determinada de la aplicación compartan el mismo estilo.</span><span class="sxs-lookup"><span data-stu-id="958cf-331">In the following example from the sample app, the `ThemeInfo` class specifies the theme information to flow down the component hierarchy so that all of the buttons within a given part of the app share the same style.</span></span>

<span data-ttu-id="958cf-332">*UIThemeClasses/ThemeInfo. CS*:</span><span class="sxs-lookup"><span data-stu-id="958cf-332">*UIThemeClasses/ThemeInfo.cs*:</span></span>

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

<span data-ttu-id="958cf-333">Un componente antecesor puede proporcionar un valor en cascada mediante el componente de valor en cascada.</span><span class="sxs-lookup"><span data-stu-id="958cf-333">An ancestor component can provide a cascading value using the Cascading Value component.</span></span> <span data-ttu-id="958cf-334">El componente `CascadingValue` encapsula un subárbol de la jerarquía de componentes y proporciona un valor único a todos los componentes de ese subárbol.</span><span class="sxs-lookup"><span data-stu-id="958cf-334">The `CascadingValue` component wraps a subtree of the component hierarchy and supplies a single value to all components within that subtree.</span></span>

<span data-ttu-id="958cf-335">Por ejemplo, la aplicación de ejemplo especifica información de tema (`ThemeInfo`) en uno de los diseños de la aplicación como parámetro en cascada para todos los componentes que componen el cuerpo del diseño de la propiedad `@Body`.</span><span class="sxs-lookup"><span data-stu-id="958cf-335">For example, the sample app specifies theme information (`ThemeInfo`) in one of the app's layouts as a cascading parameter for all components that make up the layout body of the `@Body` property.</span></span> <span data-ttu-id="958cf-336">a `ButtonClass` se le asigna un valor de `btn-success` en el componente de diseño.</span><span class="sxs-lookup"><span data-stu-id="958cf-336">`ButtonClass` is assigned a value of `btn-success` in the layout component.</span></span> <span data-ttu-id="958cf-337">Cualquier componente descendiente puede consumir esta propiedad mediante el `ThemeInfo` objeto en cascada.</span><span class="sxs-lookup"><span data-stu-id="958cf-337">Any descendent component can consume this property through the `ThemeInfo` cascading object.</span></span>

<span data-ttu-id="958cf-338">`CascadingValuesParametersLayout` componente:</span><span class="sxs-lookup"><span data-stu-id="958cf-338">`CascadingValuesParametersLayout` component:</span></span>

```razor
@inherits LayoutComponentBase
@using BlazorSample.UIThemeClasses

<div class="container-fluid">
    <div class="row">
        <div class="col-sm-3">
            <NavMenu />
        </div>
        <div class="col-sm-9">
            <CascadingValue Value="_theme">
                <div class="content px-4">
                    @Body
                </div>
            </CascadingValue>
        </div>
    </div>
</div>

@code {
    private ThemeInfo _theme = new ThemeInfo { ButtonClass = "btn-success" };
}
```

<span data-ttu-id="958cf-339">Para usar valores en cascada, los componentes declaran parámetros en cascada mediante el atributo `[CascadingParameter]`.</span><span class="sxs-lookup"><span data-stu-id="958cf-339">To make use of cascading values, components declare cascading parameters using the `[CascadingParameter]` attribute.</span></span> <span data-ttu-id="958cf-340">Los valores en cascada se enlazan a los parámetros en cascada por tipo.</span><span class="sxs-lookup"><span data-stu-id="958cf-340">Cascading values are bound to cascading parameters by type.</span></span>

<span data-ttu-id="958cf-341">En la aplicación de ejemplo, el componente `CascadingValuesParametersTheme` enlaza el valor en cascada `ThemeInfo` a un parámetro en cascada.</span><span class="sxs-lookup"><span data-stu-id="958cf-341">In the sample app, the `CascadingValuesParametersTheme` component binds the `ThemeInfo` cascading value to a cascading parameter.</span></span> <span data-ttu-id="958cf-342">El parámetro se usa para establecer la clase CSS para uno de los botones mostrados por el componente.</span><span class="sxs-lookup"><span data-stu-id="958cf-342">The parameter is used to set the CSS class for one of the buttons displayed by the component.</span></span>

<span data-ttu-id="958cf-343">`CascadingValuesParametersTheme` componente:</span><span class="sxs-lookup"><span data-stu-id="958cf-343">`CascadingValuesParametersTheme` component:</span></span>

```razor
@page "/cascadingvaluesparameterstheme"
@layout CascadingValuesParametersLayout
@using BlazorSample.UIThemeClasses

<h1>Cascading Values & Parameters</h1>

<p>Current count: @_currentCount</p>

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
    private int _currentCount = 0;

    [CascadingParameter]
    protected ThemeInfo ThemeInfo { get; set; }

    private void IncrementCount()
    {
        _currentCount++;
    }
}
```

<span data-ttu-id="958cf-344">Para poner en cascada varios valores del mismo tipo en el mismo subárbol, proporcione una cadena de `Name` única para cada componente `CascadingValue` y su `CascadingParameter`correspondiente.</span><span class="sxs-lookup"><span data-stu-id="958cf-344">To cascade multiple values of the same type within the same subtree, provide a unique `Name` string to each `CascadingValue` component and its corresponding `CascadingParameter`.</span></span> <span data-ttu-id="958cf-345">En el ejemplo siguiente, dos componentes de `CascadingValue` en cascada son instancias diferentes de `MyCascadingType` por nombre:</span><span class="sxs-lookup"><span data-stu-id="958cf-345">In the following example, two `CascadingValue` components cascade different instances of `MyCascadingType` by name:</span></span>

```razor
<CascadingValue Value=@_parentCascadeParameter1 Name="CascadeParam1">
    <CascadingValue Value=@ParentCascadeParameter2 Name="CascadeParam2">
        ...
    </CascadingValue>
</CascadingValue>

@code {
    private MyCascadingType _parentCascadeParameter1;

    [Parameter]
    public MyCascadingType ParentCascadeParameter2 { get; set; }

    ...
}
```

<span data-ttu-id="958cf-346">En un componente descendiente, los parámetros en cascada reciben sus valores de los valores en cascada correspondientes del componente antecesor por nombre:</span><span class="sxs-lookup"><span data-stu-id="958cf-346">In a descendant component, the cascaded parameters receive their values from the corresponding cascaded values in the ancestor component by name:</span></span>

```razor
...

@code {
    [CascadingParameter(Name = "CascadeParam1")]
    protected MyCascadingType ChildCascadeParameter1 { get; set; }
    
    [CascadingParameter(Name = "CascadeParam2")]
    protected MyCascadingType ChildCascadeParameter2 { get; set; }
}
```

### <a name="tabset-example"></a><span data-ttu-id="958cf-347">Ejemplo de TabSet</span><span class="sxs-lookup"><span data-stu-id="958cf-347">TabSet example</span></span>

<span data-ttu-id="958cf-348">Los parámetros en cascada también permiten que los componentes colaboren en la jerarquía de componentes.</span><span class="sxs-lookup"><span data-stu-id="958cf-348">Cascading parameters also enable components to collaborate across the component hierarchy.</span></span> <span data-ttu-id="958cf-349">Por ejemplo, considere el siguiente ejemplo de *TabSet* en la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="958cf-349">For example, consider the following *TabSet* example in the sample app.</span></span>

<span data-ttu-id="958cf-350">La aplicación de ejemplo tiene una interfaz `ITab` que las pestañas implementan:</span><span class="sxs-lookup"><span data-stu-id="958cf-350">The sample app has an `ITab` interface that tabs implement:</span></span>

[!code-csharp[](common/samples/3.x/BlazorWebAssemblySample/UIInterfaces/ITab.cs)]

<span data-ttu-id="958cf-351">El componente `CascadingValuesParametersTabSet` usa el componente `TabSet`, que contiene varios componentes `Tab`:</span><span class="sxs-lookup"><span data-stu-id="958cf-351">The `CascadingValuesParametersTabSet` component uses the `TabSet` component, which contains several `Tab` components:</span></span>

```razor
<TabSet>
    <Tab Title="First tab">
        <h4>Greetings from the first tab!</h4>

        <label>
            <input type="checkbox" @bind="showThirdTab" />
            Toggle third tab
        </label>
    </Tab>
    <Tab Title="Second tab">
        <h4>The second tab says Hello World!</h4>
    </Tab>

    @if (showThirdTab)
    {
        <Tab Title="Third tab">
            <h4>Welcome to the disappearing third tab!</h4>
            <p>Toggle this tab from the first tab.</p>
        </Tab>
    }
</TabSet>
```

<span data-ttu-id="958cf-352">Los componentes de `Tab` secundarios no se pasan explícitamente como parámetros a la `TabSet`.</span><span class="sxs-lookup"><span data-stu-id="958cf-352">The child `Tab` components aren't explicitly passed as parameters to the `TabSet`.</span></span> <span data-ttu-id="958cf-353">En su lugar, los componentes secundarios `Tab` forman parte del contenido secundario del `TabSet`.</span><span class="sxs-lookup"><span data-stu-id="958cf-353">Instead, the child `Tab` components are part of the child content of the `TabSet`.</span></span> <span data-ttu-id="958cf-354">Sin embargo, el `TabSet` todavía necesita saber sobre cada componente `Tab` para que pueda representar los encabezados y la pestaña activa. Para habilitar esta coordinación sin necesidad de código adicional, el componente de `TabSet` *puede proporcionarse como un valor en cascada* que los componentes de `Tab` descendientes recogen.</span><span class="sxs-lookup"><span data-stu-id="958cf-354">However, the `TabSet` still needs to know about each `Tab` component so that it can render the headers and the active tab. To enable this coordination without requiring additional code, the `TabSet` component *can provide itself as a cascading value* that is then picked up by the descendent `Tab` components.</span></span>

<span data-ttu-id="958cf-355">`TabSet` componente:</span><span class="sxs-lookup"><span data-stu-id="958cf-355">`TabSet` component:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/TabSet.razor)]

<span data-ttu-id="958cf-356">Los componentes de `Tab` descendientes capturan el `TabSet` contenedor como un parámetro en cascada, por lo que los componentes de `Tab` se agregan a los `TabSet` y coordenadas en los que la pestaña está activa.</span><span class="sxs-lookup"><span data-stu-id="958cf-356">The descendent `Tab` components capture the containing `TabSet` as a cascading parameter, so the `Tab` components add themselves to the `TabSet` and coordinate on which tab is active.</span></span>

<span data-ttu-id="958cf-357">`Tab` componente:</span><span class="sxs-lookup"><span data-stu-id="958cf-357">`Tab` component:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/Tab.razor)]

## <a name="razor-templates"></a><span data-ttu-id="958cf-358">Plantillas de Razor</span><span class="sxs-lookup"><span data-stu-id="958cf-358">Razor templates</span></span>

<span data-ttu-id="958cf-359">Los fragmentos de representación se pueden definir mediante la sintaxis de plantilla de Razor.</span><span class="sxs-lookup"><span data-stu-id="958cf-359">Render fragments can be defined using Razor template syntax.</span></span> <span data-ttu-id="958cf-360">Las plantillas de Razor son una manera de definir un fragmento de la interfaz de usuario y suponer el siguiente formato:</span><span class="sxs-lookup"><span data-stu-id="958cf-360">Razor templates are a way to define a UI snippet and assume the following format:</span></span>

```razor
@<{HTML tag}>...</{HTML tag}>
```

<span data-ttu-id="958cf-361">En el ejemplo siguiente se muestra cómo especificar los valores de `RenderFragment` y `RenderFragment<T>` y las plantillas de representación directamente en un componente.</span><span class="sxs-lookup"><span data-stu-id="958cf-361">The following example illustrates how to specify `RenderFragment` and `RenderFragment<T>` values and render templates directly in a component.</span></span> <span data-ttu-id="958cf-362">Los fragmentos de representación también se pueden pasar como argumentos a [componentes con plantilla](xref:blazor/templated-components).</span><span class="sxs-lookup"><span data-stu-id="958cf-362">Render fragments can also be passed as arguments to [templated components](xref:blazor/templated-components).</span></span>

```razor
@_timeTemplate

@_petTemplate(new Pet { Name = "Rex" })

@code {
    private RenderFragment _timeTemplate = @<p>The time is @DateTime.Now.</p>;
    private RenderFragment<Pet> _petTemplate = (pet) => @<p>Pet: @pet.Name</p>;

    private class Pet
    {
        public string Name { get; set; }
    }
}
```

<span data-ttu-id="958cf-363">Salida representada del código anterior:</span><span class="sxs-lookup"><span data-stu-id="958cf-363">Rendered output of the preceding code:</span></span>

```html
<p>The time is 10/04/2018 01:26:52.</p>

<p>Pet: Rex</p>
```

## <a name="scalable-vector-graphics-svg-images"></a><span data-ttu-id="958cf-364">Imágenes de gráficos vectoriales escalables (SVG)</span><span class="sxs-lookup"><span data-stu-id="958cf-364">Scalable Vector Graphics (SVG) images</span></span>

<span data-ttu-id="958cf-365">Dado que Blazor representa imágenes HTML, compatibles con el explorador, incluidas las imágenes SVG (Scalable Vector Graphics) ( *. svg*), se admiten a través de la etiqueta `<img>`:</span><span class="sxs-lookup"><span data-stu-id="958cf-365">Since Blazor renders HTML, browser-supported images, including Scalable Vector Graphics (SVG) images (*.svg*), are supported via the `<img>` tag:</span></span>

```html
<img alt="Example image" src="some-image.svg" />
```

<span data-ttu-id="958cf-366">Del mismo modo, las imágenes SVG se admiten en las reglas CSS de un archivo de hoja de estilos ( *. CSS*):</span><span class="sxs-lookup"><span data-stu-id="958cf-366">Similarly, SVG images are supported in the CSS rules of a stylesheet file (*.css*):</span></span>

```css
.my-element {
    background-image: url("some-image.svg");
}
```

<span data-ttu-id="958cf-367">Sin embargo, el marcado SVG en línea no se admite en todos los escenarios.</span><span class="sxs-lookup"><span data-stu-id="958cf-367">However, inline SVG markup isn't supported in all scenarios.</span></span> <span data-ttu-id="958cf-368">Si coloca una etiqueta de `<svg>` directamente en un archivo de componente ( *. Razor*), se admite la representación de imágenes básica, pero aún no se admiten muchos escenarios avanzados.</span><span class="sxs-lookup"><span data-stu-id="958cf-368">If you place an `<svg>` tag directly into a component file (*.razor*), basic image rendering is supported but many advanced scenarios aren't yet supported.</span></span> <span data-ttu-id="958cf-369">Por ejemplo, `<use>` etiquetas no se respetan actualmente y `@bind` no se pueden usar con algunas etiquetas SVG.</span><span class="sxs-lookup"><span data-stu-id="958cf-369">For example, `<use>` tags aren't currently respected, and `@bind` can't be used with some SVG tags.</span></span> <span data-ttu-id="958cf-370">Esperamos abordar estas limitaciones en una versión futura.</span><span class="sxs-lookup"><span data-stu-id="958cf-370">We expect to address these limitations in a future release.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="958cf-371">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="958cf-371">Additional resources</span></span>

* <span data-ttu-id="958cf-372"><xref:security/blazor/server> &ndash; incluye instrucciones sobre la creación de aplicaciones de Blazor Server que deben competir con el agotamiento de recursos.</span><span class="sxs-lookup"><span data-stu-id="958cf-372"><xref:security/blazor/server> &ndash; Includes guidance on building Blazor Server apps that must contend with resource exhaustion.</span></span>
