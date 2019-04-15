---
title: Crear y usar componentes de Razor
author: guardrex
description: Obtenga información sobre cómo crear y usar componentes de Razor, incluida la forma de enlazar a datos, controlar los eventos y administrar los ciclos de vida del componente.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/08/2019
uid: razor-components/components
ms.openlocfilehash: f8ac7f3844b94a162e8d1c45f80ae153d89536ee
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/10/2019
ms.locfileid: "59515697"
---
# <a name="create-and-use-razor-components"></a><span data-ttu-id="ea15e-103">Crear y usar componentes de Razor</span><span class="sxs-lookup"><span data-stu-id="ea15e-103">Create and use Razor Components</span></span>

<span data-ttu-id="ea15e-104">Por [Halter](https://github.com/guardrex), [Daniel Roth](https://github.com/danroth27), y [Morné Zaayman](https://github.com/MorneZaayman)</span><span class="sxs-lookup"><span data-stu-id="ea15e-104">By [Luke Latham](https://github.com/guardrex), [Daniel Roth](https://github.com/danroth27), and [Morné Zaayman](https://github.com/MorneZaayman)</span></span>

<span data-ttu-id="ea15e-105">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) ([cómo descargarlo](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="ea15e-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="ea15e-106">Consulte el tema [Introducción](xref:razor-components/get-started) para ver los requisitos previos.</span><span class="sxs-lookup"><span data-stu-id="ea15e-106">See the [Get started](xref:razor-components/get-started) topic for prerequisites.</span></span>

<span data-ttu-id="ea15e-107">Las aplicaciones de componentes de Razor se compilan mediante *componentes*.</span><span class="sxs-lookup"><span data-stu-id="ea15e-107">Razor Components apps are built using *components*.</span></span> <span data-ttu-id="ea15e-108">Un componente es un fragmento independiente de la interfaz de usuario (IU), como una página, cuadro de diálogo o formulario.</span><span class="sxs-lookup"><span data-stu-id="ea15e-108">A component is a self-contained chunk of user interface (UI), such as a page, dialog, or form.</span></span> <span data-ttu-id="ea15e-109">Un componente incluye marcado HTML y la lógica de procesamiento necesario para insertar datos o responder a eventos de interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="ea15e-109">A component includes HTML markup and the processing logic required to inject data or respond to UI events.</span></span> <span data-ttu-id="ea15e-110">Los componentes son flexibles y ligera.</span><span class="sxs-lookup"><span data-stu-id="ea15e-110">Components are flexible and lightweight.</span></span> <span data-ttu-id="ea15e-111">Puede anidar, volver a usar y compartirse entre proyectos.</span><span class="sxs-lookup"><span data-stu-id="ea15e-111">They can be nested, reused, and shared among projects.</span></span>

## <a name="component-classes"></a><span data-ttu-id="ea15e-112">Clases de componentes</span><span class="sxs-lookup"><span data-stu-id="ea15e-112">Component classes</span></span>

<span data-ttu-id="ea15e-113">Los componentes se implementan normalmente en archivos de componentes de Razor (*.razor*) mediante una combinación de C# y marcado HTML (*.cshtml* archivos se usan en aplicaciones Blazor).</span><span class="sxs-lookup"><span data-stu-id="ea15e-113">Components are typically implemented in Razor Component files (*.razor*) using a combination of C# and HTML markup (*.cshtml* files are used in Blazor apps).</span></span>

<span data-ttu-id="ea15e-114">Los componentes se pueden crear en las aplicaciones de componentes de Razor con el *.cshtml* extensión de archivo, siempre se identifican los archivos como archivos de Razor componente mediante el `_RazorComponentInclude` propiedad de MSBuild.</span><span class="sxs-lookup"><span data-stu-id="ea15e-114">Components can be authored in Razor Components apps using the *.cshtml* file extension as long as the files are identified as Razor Component files using the `_RazorComponentInclude` MSBuild property.</span></span> <span data-ttu-id="ea15e-115">Por ejemplo, una aplicación creada con la plantilla de Razor componente especifica que todos los *.cshtml* archivos bajo la *componentes* carpeta debe tratarse como archivos de componentes de Razor:</span><span class="sxs-lookup"><span data-stu-id="ea15e-115">For example, an app created using the Razor Component template specifies that all *.cshtml* files under the *Components* folder should be treated as Razor Components files:</span></span>

```xml
<_RazorComponentInclude>Components\**\*.cshtml</_RazorComponentInclude>
```

<span data-ttu-id="ea15e-116">La interfaz de usuario para un componente se define mediante HTML.</span><span class="sxs-lookup"><span data-stu-id="ea15e-116">The UI for a component is defined using HTML.</span></span> <span data-ttu-id="ea15e-117">La lógica de la representación dinámica (por ejemplo, bucles, instrucciones condicionales, expresiones) se agrega mediante una sintaxis de C# insertada denominada [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="ea15e-117">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="ea15e-118">Cuando se compila una aplicación de los componentes de Razor, el marcado HTML y C# lógica de representación se convierten en una clase de componente.</span><span class="sxs-lookup"><span data-stu-id="ea15e-118">When a Razor Components app is compiled, the HTML markup and C# rendering logic are converted into a component class.</span></span> <span data-ttu-id="ea15e-119">El nombre de la clase generada coincide con el nombre del archivo.</span><span class="sxs-lookup"><span data-stu-id="ea15e-119">The name of the generated class matches the name of the file.</span></span>

<span data-ttu-id="ea15e-120">Los miembros de la clase de componente se definen en un `@functions` bloque (más de un `@functions` bloque está permitido).</span><span class="sxs-lookup"><span data-stu-id="ea15e-120">Members of the component class are defined in a `@functions` block (more than one `@functions` block is permissible).</span></span> <span data-ttu-id="ea15e-121">En el `@functions` bloque, el estado de componente (propiedades, campos) se especifica con los métodos de control de eventos o para definir otra lógica de componente.</span><span class="sxs-lookup"><span data-stu-id="ea15e-121">In the `@functions` block, component state (properties, fields) is specified with methods for event handling or for defining other component logic.</span></span>

<span data-ttu-id="ea15e-122">Los miembros del componente, a continuación, se pueden usar como parte del componente de la representación lógica usando C# expresiones que empiezan por `@`.</span><span class="sxs-lookup"><span data-stu-id="ea15e-122">Component members can then be used as part of the component's rendering logic using C# expressions that start with `@`.</span></span> <span data-ttu-id="ea15e-123">Por ejemplo, un C# campo se representa con el prefijo `@` para el nombre del campo.</span><span class="sxs-lookup"><span data-stu-id="ea15e-123">For example, a C# field is rendered by prefixing `@` to the field name.</span></span> <span data-ttu-id="ea15e-124">En el ejemplo siguiente, se evalúa y se representa:</span><span class="sxs-lookup"><span data-stu-id="ea15e-124">The following example evaluates and renders:</span></span>

* `_headingFontStyle` <span data-ttu-id="ea15e-125">el valor de propiedad CSS para `font-style`.</span><span class="sxs-lookup"><span data-stu-id="ea15e-125">to the CSS property value for `font-style`.</span></span>
* `_headingText` <span data-ttu-id="ea15e-126">el contenido de la `<h1>` elemento.</span><span class="sxs-lookup"><span data-stu-id="ea15e-126">to the content of the `<h1>` element.</span></span>

```cshtml
<h1 style="font-style:@_headingFontStyle">@_headingText</h1>

@functions {
    private string _headingFontStyle = "italic";
    private string _headingText = "Put on your new Blazor!";
}
```

<span data-ttu-id="ea15e-127">Después de que el componente se representa inicialmente, el componente vuelve a generar su árbol de representación en respuesta a eventos.</span><span class="sxs-lookup"><span data-stu-id="ea15e-127">After the component is initially rendered, the component regenerates its render tree in response to events.</span></span> <span data-ttu-id="ea15e-128">Componentes de Razor, a continuación, compara el nuevo árbol de representación en el anterior y se aplica ninguna modificación a Document Object Model (DOM) del explorador.</span><span class="sxs-lookup"><span data-stu-id="ea15e-128">Razor Components then compares the new render tree against the previous one and applies any modifications to the browser's Document Object Model (DOM).</span></span>

## <a name="integrate-components-into-razor-pages-and-mvc-apps"></a><span data-ttu-id="ea15e-129">Integrar componentes en aplicaciones de las páginas de Razor y MVC</span><span class="sxs-lookup"><span data-stu-id="ea15e-129">Integrate components into Razor Pages and MVC apps</span></span>

<span data-ttu-id="ea15e-130">Utilizar componentes con las aplicaciones existentes de las páginas de Razor y MVC.</span><span class="sxs-lookup"><span data-stu-id="ea15e-130">Use components with existing Razor Pages and MVC apps.</span></span> <span data-ttu-id="ea15e-131">No hay ninguna necesidad de volver a escribir las páginas existentes o vistas a usar los componentes de Razor.</span><span class="sxs-lookup"><span data-stu-id="ea15e-131">There's no need to rewrite existing pages or views to use Razor Components.</span></span> <span data-ttu-id="ea15e-132">Cuando se representa la página o vista, los componentes son representados&dagger; al mismo tiempo.</span><span class="sxs-lookup"><span data-stu-id="ea15e-132">When the page or view is rendered, components are prerendered&dagger; at the same time.</span></span> 

> [!NOTE]
> <span data-ttu-id="ea15e-133">&dagger;Procesamiento previo del lado servidor está habilitada para las aplicaciones de componentes de Razor de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="ea15e-133">&dagger;Server-side prerendering is enabled for Razor Components apps by default.</span></span> <span data-ttu-id="ea15e-134">Las aplicaciones de cliente Blazor será compatible con procesamiento previo de la próxima versión Preview 4.</span><span class="sxs-lookup"><span data-stu-id="ea15e-134">Client-side Blazor apps will support prerendering in the upcoming Preview 4 release.</span></span> <span data-ttu-id="ea15e-135">Para obtener más información, consulte [actualizar plantillas/middleware para que use el archivo/MapFallbackToPage](https://github.com/aspnet/AspNetCore/issues/8852).</span><span class="sxs-lookup"><span data-stu-id="ea15e-135">For more information, see [Update templates/middleware to use MapFallbackToPage/File](https://github.com/aspnet/AspNetCore/issues/8852).</span></span>

<span data-ttu-id="ea15e-136">Para representar un componente de una página o vista, use el `RenderComponentAsync<TComponent>` método auxiliar HTML:</span><span class="sxs-lookup"><span data-stu-id="ea15e-136">To render a component from a page or view, use the `RenderComponentAsync<TComponent>` HTML helper method:</span></span>

```cshtml
<div id="Counter">
    @(await Html.RenderComponentAsync<Counter>(new { IncrementAmount = 10 }))
</div>
```

<span data-ttu-id="ea15e-137">Todavía no son interactivos en la versión Preview 3 componentes procesados a partir de las páginas y las vistas.</span><span class="sxs-lookup"><span data-stu-id="ea15e-137">Components rendered from pages and views aren't yet interactive in the Preview 3 release.</span></span> <span data-ttu-id="ea15e-138">Por ejemplo, la selección de un botón no desencadena una llamada al método.</span><span class="sxs-lookup"><span data-stu-id="ea15e-138">For example, selecting a button doesn't trigger a method call.</span></span> <span data-ttu-id="ea15e-139">Una vista previa futura se solucione esta limitación y agregan compatibilidad para componentes de representación mediante la sintaxis normal de elementos y atributos.</span><span class="sxs-lookup"><span data-stu-id="ea15e-139">A future preview will address this limitation and add support for rendering components using the normal element and attribute syntax.</span></span>

<span data-ttu-id="ea15e-140">Aunque las páginas y las vistas pueden usar los componentes, lo contrario no es cierto.</span><span class="sxs-lookup"><span data-stu-id="ea15e-140">While pages and views can use components, the converse isn't true.</span></span> <span data-ttu-id="ea15e-141">Los componentes no pueden usar en escenarios específicos de página y vista, como las vistas parciales y secciones.</span><span class="sxs-lookup"><span data-stu-id="ea15e-141">Components can't use view- and page-specific scenarios, such as partial views and sections.</span></span> <span data-ttu-id="ea15e-142">Para utilizar lógica de vista parcial en un componente, descarte la lógica de vista parcial en un componente.</span><span class="sxs-lookup"><span data-stu-id="ea15e-142">To use logic from partial view in a component, factor out the partial view logic into a component.</span></span>

## <a name="using-components"></a><span data-ttu-id="ea15e-143">Uso de componentes</span><span class="sxs-lookup"><span data-stu-id="ea15e-143">Using components</span></span>

<span data-ttu-id="ea15e-144">Los componentes pueden incluir otros componentes declarándolas mediante la sintaxis de elemento HTML.</span><span class="sxs-lookup"><span data-stu-id="ea15e-144">Components can include other components by declaring them using HTML element syntax.</span></span> <span data-ttu-id="ea15e-145">El marcado para utilizar un componente se parece a una etiqueta HTML en la que el nombre de la etiqueta es el tipo de componente.</span><span class="sxs-lookup"><span data-stu-id="ea15e-145">The markup for using a component looks like an HTML tag where the name of the tag is the component type.</span></span>

<span data-ttu-id="ea15e-146">El marcado siguiente representa un `HeadingComponent` (*HeadingComponent.cshtml*) instancia:</span><span class="sxs-lookup"><span data-stu-id="ea15e-146">The following markup renders a `HeadingComponent` (*HeadingComponent.cshtml*) instance:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/Index.cshtml?name=snippet_HeadingComponent)]

## <a name="component-parameters"></a><span data-ttu-id="ea15e-147">Parámetros del componente</span><span class="sxs-lookup"><span data-stu-id="ea15e-147">Component parameters</span></span>

<span data-ttu-id="ea15e-148">Pueden tener componentes *parámetros del componente*, que se definen mediante *no públicos* propiedades en la clase de componente decoran con `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="ea15e-148">Components can have *component parameters*, which are defined using *non-public* properties on the component class decorated with `[Parameter]`.</span></span> <span data-ttu-id="ea15e-149">Use atributos para especificar argumentos para un componente en el marcado.</span><span class="sxs-lookup"><span data-stu-id="ea15e-149">Use attributes to specify arguments for a component in markup.</span></span>

<span data-ttu-id="ea15e-150">En el ejemplo siguiente, la `ParentComponent` establece el valor de la `Title` propiedad de la `ChildComponent`:</span><span class="sxs-lookup"><span data-stu-id="ea15e-150">In the following example, the `ParentComponent` sets the value of the `Title` property of the `ChildComponent`:</span></span>

<span data-ttu-id="ea15e-151">*ParentComponent.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="ea15e-151">*ParentComponent.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.cshtml?name=snippet_ParentComponent&highlight=5)]

<span data-ttu-id="ea15e-152">*ChildComponent.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="ea15e-152">*ChildComponent.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ChildComponent.cshtml?highlight=7-8)]

## <a name="child-content"></a><span data-ttu-id="ea15e-153">Contenido secundario</span><span class="sxs-lookup"><span data-stu-id="ea15e-153">Child content</span></span>

<span data-ttu-id="ea15e-154">Los componentes pueden establecer el contenido de otro componente.</span><span class="sxs-lookup"><span data-stu-id="ea15e-154">Components can set the content of another component.</span></span> <span data-ttu-id="ea15e-155">El componente de asignación proporciona el contenido entre las etiquetas que especifican el componente receptor.</span><span class="sxs-lookup"><span data-stu-id="ea15e-155">The assigning component provides the content between the tags that specify the receiving component.</span></span> <span data-ttu-id="ea15e-156">Por ejemplo, un `ParentComponent` puede proporcionar contenido para la representación por un componente secundario colocando el contenido dentro de `<ChildComponent>` etiquetas.</span><span class="sxs-lookup"><span data-stu-id="ea15e-156">For example, a `ParentComponent` can provide content for rendering by a Child component by placing the content inside `<ChildComponent>` tags.</span></span>

<span data-ttu-id="ea15e-157">*ParentComponent.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="ea15e-157">*ParentComponent.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.cshtml?name=snippet_ParentComponent&highlight=6-7)]

<span data-ttu-id="ea15e-158">El componente secundario tiene un `ChildContent` propiedad que representa un `RenderFragment`.</span><span class="sxs-lookup"><span data-stu-id="ea15e-158">The Child component has a `ChildContent` property that represents a `RenderFragment`.</span></span> <span data-ttu-id="ea15e-159">El valor de `ChildContent` se coloca en el marcado del componente secundario donde se debe representar el contenido.</span><span class="sxs-lookup"><span data-stu-id="ea15e-159">The value of `ChildContent` is positioned in the child component's markup where the content should be rendered.</span></span> <span data-ttu-id="ea15e-160">En el ejemplo siguiente, el valor de `ChildContent` se recibe desde el componente primario y se representa dentro del panel de Bootstrap `panel-body`.</span><span class="sxs-lookup"><span data-stu-id="ea15e-160">In the following example, the value of `ChildContent` is received from the parent component and rendered inside the Bootstrap panel's `panel-body`.</span></span>

<span data-ttu-id="ea15e-161">*ChildComponent.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="ea15e-161">*ChildComponent.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ChildComponent.cshtml?highlight=3,10-11)]

> [!NOTE]
> <span data-ttu-id="ea15e-162">La recepción de la propiedad la `RenderFragment` contenido debe denominarse `ChildContent` por convención.</span><span class="sxs-lookup"><span data-stu-id="ea15e-162">The property receiving the `RenderFragment` content must be named `ChildContent` by convention.</span></span>

## <a name="data-binding"></a><span data-ttu-id="ea15e-163">Enlace de datos</span><span class="sxs-lookup"><span data-stu-id="ea15e-163">Data binding</span></span>

<span data-ttu-id="ea15e-164">Enlace de datos a los componentes y los elementos DOM se logra con la `bind` atributo.</span><span class="sxs-lookup"><span data-stu-id="ea15e-164">Data binding to both components and DOM elements is accomplished with the `bind` attribute.</span></span> <span data-ttu-id="ea15e-165">En el ejemplo siguiente se enlaza el `ItalicsCheck` comprueba la propiedad a la casilla de verificación Estado:</span><span class="sxs-lookup"><span data-stu-id="ea15e-165">The following example binds the `ItalicsCheck` property to the check box's checked state:</span></span>

```cshtml
<input type="checkbox" class="form-check-input" id="italicsCheck" 
    bind="@_italicsCheck">
```

<span data-ttu-id="ea15e-166">Cuando se activa y se desactiva la casilla de verificación, el valor de propiedad se actualiza a `true` y `false`, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="ea15e-166">When the check box is selected and cleared, the property's value is updated to `true` and `false`, respectively.</span></span>

<span data-ttu-id="ea15e-167">La casilla de verificación se actualiza en la interfaz de usuario solo cuando se procesa el componente, no en respuesta a cambiar el valor de propiedad.</span><span class="sxs-lookup"><span data-stu-id="ea15e-167">The check box is updated in the UI only when the component is rendered, not in response to changing the property's value.</span></span> <span data-ttu-id="ea15e-168">Puesto que los componentes de representan a sí mismos cuando se ejecuta el código del controlador de eventos, las actualizaciones de la propiedad normalmente aparecen en la interfaz de usuario inmediatamente.</span><span class="sxs-lookup"><span data-stu-id="ea15e-168">Since components render themselves after event handler code executes, property updates are usually reflected in the UI immediately.</span></span>

<span data-ttu-id="ea15e-169">Uso de `bind` con un `CurrentValue` propiedad (`<input bind="@CurrentValue">`) es esencialmente equivalente a lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="ea15e-169">Using `bind` with a `CurrentValue` property (`<input bind="@CurrentValue">`) is essentially equivalent to the following:</span></span>

```cshtml
<input value="@CurrentValue" 
    onchange="@((UIChangeEventArgs __e) => CurrentValue = __e.Value)">
```

<span data-ttu-id="ea15e-170">Cuando se procesa el componente, el `value` del elemento de entrada procede de la `CurrentValue` propiedad.</span><span class="sxs-lookup"><span data-stu-id="ea15e-170">When the component is rendered, the `value` of the input element comes from the `CurrentValue` property.</span></span> <span data-ttu-id="ea15e-171">Cuando el usuario escribe en el cuadro de texto, el `onchange` desencadena el evento y el `CurrentValue` propiedad está establecida en el valor modificado.</span><span class="sxs-lookup"><span data-stu-id="ea15e-171">When the user types in the text box, the `onchange` event is fired and the `CurrentValue` property is set to the changed value.</span></span> <span data-ttu-id="ea15e-172">En realidad, la generación de código es un poco más complejo porque `bind` controla algunos de los casos donde se realizan las conversiones de tipos.</span><span class="sxs-lookup"><span data-stu-id="ea15e-172">In reality, the code generation is a little more complex because `bind` handles a few cases where type conversions are performed.</span></span> <span data-ttu-id="ea15e-173">En principio, `bind` asocia el valor actual de una expresión con un `value` cambios de atributo y controla mediante el controlador registrado.</span><span class="sxs-lookup"><span data-stu-id="ea15e-173">In principle, `bind` associates the current value of an expression with a `value` attribute and handles changes using the registered handler.</span></span>

<span data-ttu-id="ea15e-174">Además `onchange`, la propiedad se puede enlazar con otros eventos tales como `oninput` ya que es más explícito acerca de qué se debe enlazar al:</span><span class="sxs-lookup"><span data-stu-id="ea15e-174">In addition to `onchange`, the property can be bound using other events like `oninput` by being more explicit about what to bind to:</span></span>

```cshtml
<input type="text" bind-value-oninput="@CurrentValue">
```

<span data-ttu-id="ea15e-175">A diferencia de `onchange`, `oninput` se desencadena para cada carácter que se introduce en el cuadro de texto.</span><span class="sxs-lookup"><span data-stu-id="ea15e-175">Unlike `onchange`, `oninput` fires for every character that is input into the text box.</span></span>

**<span data-ttu-id="ea15e-176">Cadenas de formato</span><span class="sxs-lookup"><span data-stu-id="ea15e-176">Format strings</span></span>**

<span data-ttu-id="ea15e-177">Enlace de datos funciona con <xref:System.DateTime> cadenas con formato.</span><span class="sxs-lookup"><span data-stu-id="ea15e-177">Data binding works with <xref:System.DateTime> format strings.</span></span> <span data-ttu-id="ea15e-178">Otras expresiones de formato, como moneda o los formatos de número, no están disponibles en este momento.</span><span class="sxs-lookup"><span data-stu-id="ea15e-178">Other format expressions, such as currency or number formats, aren't available at this time.</span></span>

```cshtml
<input bind="@StartDate" format-value="yyyy-MM-dd">

@functions {
    [Parameter]
    private DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

<span data-ttu-id="ea15e-179">El `format-value` atributo especifica el formato de fecha para aplicarlo a la `value` de la `input` elemento.</span><span class="sxs-lookup"><span data-stu-id="ea15e-179">The `format-value` attribute specifies the date format to apply to the `value` of the `input` element.</span></span> <span data-ttu-id="ea15e-180">El formato también se usa para analizar el valor cuando un `onchange` se produce el evento.</span><span class="sxs-lookup"><span data-stu-id="ea15e-180">The format is also used to parse the value when an `onchange` event occurs.</span></span>

**<span data-ttu-id="ea15e-181">Parámetros del componente</span><span class="sxs-lookup"><span data-stu-id="ea15e-181">Component parameters</span></span>**

<span data-ttu-id="ea15e-182">Enlace también reconoce los parámetros del componente, donde `bind-{property}` puede enlazar un valor de propiedad entre componentes.</span><span class="sxs-lookup"><span data-stu-id="ea15e-182">Binding also recognizes component parameters, where `bind-{property}` can bind a property value across components.</span></span>

<span data-ttu-id="ea15e-183">Usa el siguiente componente `ChildComponent` y enlaza la `ParentYear` parámetro desde el elemento primario para el `Year` parámetro del componente secundario:</span><span class="sxs-lookup"><span data-stu-id="ea15e-183">The following component uses `ChildComponent` and binds the `ParentYear` parameter from the parent to the `Year` parameter on the child component:</span></span>

<span data-ttu-id="ea15e-184">Componente primario:</span><span class="sxs-lookup"><span data-stu-id="ea15e-184">Parent component:</span></span>

```cshtml
@page "/ParentComponent"

<h1>Parent Component</h1>

<p>ParentYear: @ParentYear</p>

<ChildComponent bind-Year="@ParentYear" />

<button class="btn btn-primary" onclick="@ChangeTheYear">
    Change Year to 1986
</button>

@functions {
    [Parameter]
    private int ParentYear { get; set; } = 1978;

    private void ChangeTheYear()
    {
        ParentYear = 1986;
    }
}
```

<span data-ttu-id="ea15e-185">Componente secundario:</span><span class="sxs-lookup"><span data-stu-id="ea15e-185">Child component:</span></span>

```cshtml
<h2>Child Component</h2>

<p>Year: @Year</p>

@functions {
    [Parameter]
    private int Year { get; set; }

    [Parameter]
    private Action<int> YearChanged { get; set; }
}
```

<span data-ttu-id="ea15e-186">Cargando el `ParentComponent` genera el marcado siguiente:</span><span class="sxs-lookup"><span data-stu-id="ea15e-186">Loading the `ParentComponent` produces the following markup:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

<span data-ttu-id="ea15e-187">Si el valor de la `ParentYear` se cambia la propiedad seleccionando el botón en el `ParentComponent`, el `Year` propiedad de la `ChildComponent` se actualiza.</span><span class="sxs-lookup"><span data-stu-id="ea15e-187">If the value of the `ParentYear` property is changed by selecting the button in the `ParentComponent`, the `Year` property of the `ChildComponent` is updated.</span></span> <span data-ttu-id="ea15e-188">El nuevo valor de `Year` se representa en la interfaz de usuario cuando el `ParentComponent` es el término:</span><span class="sxs-lookup"><span data-stu-id="ea15e-188">The new value of `Year` is rendered in the UI when the `ParentComponent` is rerendered:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

<span data-ttu-id="ea15e-189">El `Year` parámetro es enlazable porque tiene un complemento `YearChanged` evento que coincide con el tipo de la `Year` parámetro.</span><span class="sxs-lookup"><span data-stu-id="ea15e-189">The `Year` parameter is bindable because it has a companion `YearChanged` event that matches the type of the `Year` parameter.</span></span>

<span data-ttu-id="ea15e-190">Por convención, `<ChildComponent bind-Year="@ParentYear" />` es esencialmente equivalente a escribir,</span><span class="sxs-lookup"><span data-stu-id="ea15e-190">By convention, `<ChildComponent bind-Year="@ParentYear" />` is essentially equivalent to writing,</span></span>

```cshtml
    <ChildComponent bind-Year-YearChanged="@ParentYear" />
```

<span data-ttu-id="ea15e-191">En general, se puede enlazar una propiedad en un controlador de eventos correspondiente mediante `bind-property-event` atributo.</span><span class="sxs-lookup"><span data-stu-id="ea15e-191">In general, a property can be bound to a corresponding event handler using `bind-property-event` attribute.</span></span>

## <a name="event-handling"></a><span data-ttu-id="ea15e-192">Control de eventos</span><span class="sxs-lookup"><span data-stu-id="ea15e-192">Event handling</span></span>

<span data-ttu-id="ea15e-193">Los componentes de Razor proporcionan características de control de eventos.</span><span class="sxs-lookup"><span data-stu-id="ea15e-193">Razor Components provide event handling features.</span></span> <span data-ttu-id="ea15e-194">Para un atributo del elemento HTML denominado `on<event>` (por ejemplo, `onclick`, `onsubmit`) con un valor con tipo de delegado, los componentes de Razor trata el valor del atributo como un controlador de eventos.</span><span class="sxs-lookup"><span data-stu-id="ea15e-194">For an HTML element attribute named `on<event>` (for example, `onclick`, `onsubmit`) with a delegate-typed value, Razor Components treats the attribute's value as an event handler.</span></span> <span data-ttu-id="ea15e-195">El nombre del atributo siempre se inicia con `on`.</span><span class="sxs-lookup"><span data-stu-id="ea15e-195">The attribute's name always starts with `on`.</span></span>

<span data-ttu-id="ea15e-196">El código siguiente llama el `UpdateHeading` método cuando se selecciona el botón en la interfaz de usuario:</span><span class="sxs-lookup"><span data-stu-id="ea15e-196">The following code calls the `UpdateHeading` method when the button is selected in the UI:</span></span>

```cshtml
<button class="btn btn-primary" onclick="@UpdateHeading">
    Update heading
</button>

@functions {
    private void UpdateHeading(UIMouseEventArgs e)
    {
        ...
    }
}
```

<span data-ttu-id="ea15e-197">El código siguiente llama el `CheckboxChanged` método cuando se cambia la casilla de verificación en la interfaz de usuario:</span><span class="sxs-lookup"><span data-stu-id="ea15e-197">The following code calls the `CheckboxChanged` method when the check box is changed in the UI:</span></span>

```cshtml
<input type="checkbox" class="form-check-input" onchange="@CheckboxChanged">

@functions {
    private void CheckboxChanged()
    {
        ...
    }
}
```

<span data-ttu-id="ea15e-198">Controladores de eventos también pueden ser asincrónico y devolver un <xref:System.Threading.Tasks.Task>.</span><span class="sxs-lookup"><span data-stu-id="ea15e-198">Event handlers can also be asynchronous and return a <xref:System.Threading.Tasks.Task>.</span></span> <span data-ttu-id="ea15e-199">No hay ninguna necesidad de llamar manualmente a `StateHasChanged()`.</span><span class="sxs-lookup"><span data-stu-id="ea15e-199">There's no need to manually call `StateHasChanged()`.</span></span> <span data-ttu-id="ea15e-200">Las excepciones se registran cuando se produzcan.</span><span class="sxs-lookup"><span data-stu-id="ea15e-200">Exceptions are logged when they occur.</span></span>

```cshtml
<button class="btn btn-primary" onclick="@UpdateHeading">
    Update heading
</button>

@functions {
    private async Task UpdateHeading(UIMouseEventArgs e)
    {
        ...
    }
}
```

<span data-ttu-id="ea15e-201">En algunos casos, se permiten los tipos de argumento de evento específica del evento.</span><span class="sxs-lookup"><span data-stu-id="ea15e-201">For some events, event-specific event argument types are permitted.</span></span> <span data-ttu-id="ea15e-202">Si el acceso a uno de estos tipos de evento no es necesario, no es necesario en la llamada al método.</span><span class="sxs-lookup"><span data-stu-id="ea15e-202">If access to one of these event types isn't necessary, it isn't required in the method call.</span></span>

<span data-ttu-id="ea15e-203">La lista de argumentos de evento compatible es:</span><span class="sxs-lookup"><span data-stu-id="ea15e-203">The list of supported event arguments is:</span></span>

* <span data-ttu-id="ea15e-204">UIEventArgs</span><span class="sxs-lookup"><span data-stu-id="ea15e-204">UIEventArgs</span></span>
* <span data-ttu-id="ea15e-205">UIChangeEventArgs</span><span class="sxs-lookup"><span data-stu-id="ea15e-205">UIChangeEventArgs</span></span>
* <span data-ttu-id="ea15e-206">UIKeyboardEventArgs</span><span class="sxs-lookup"><span data-stu-id="ea15e-206">UIKeyboardEventArgs</span></span>
* <span data-ttu-id="ea15e-207">UIMouseEventArgs</span><span class="sxs-lookup"><span data-stu-id="ea15e-207">UIMouseEventArgs</span></span>

<span data-ttu-id="ea15e-208">También se pueden usar expresiones lambda:</span><span class="sxs-lookup"><span data-stu-id="ea15e-208">Lambda expressions can also be used:</span></span>

```cshtml
<button onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

<span data-ttu-id="ea15e-209">A menudo es conveniente cerrar sobre valores adicionales, como al recorrer en iteración un conjunto de elementos.</span><span class="sxs-lookup"><span data-stu-id="ea15e-209">It's often convenient to close over additional values, such as when iterating over a set of elements.</span></span> <span data-ttu-id="ea15e-210">En el siguiente ejemplo crea tres botones, cada uno de ellos llama `UpdateHeading` pasando un argumento de evento (`UIMouseEventArgs`) y su número de botón (`buttonNumber`) cuando se selecciona en la interfaz de usuario:</span><span class="sxs-lookup"><span data-stu-id="ea15e-210">The following example creates three buttons, each of which calls `UpdateHeading` passing an event argument (`UIMouseEventArgs`) and its button number (`buttonNumber`) when selected in the UI:</span></span>

```cshtml
<h2>@message</h2>

@for (var i = 1; i < 4; i++)
{
    var buttonNumber = i;

    <button class="btn btn-primary"
            onclick="@(e => UpdateHeading(e, buttonNumber))">
        Button #@i
    </button>
}

@functions {
    private string message = "Select a button to learn its position.";

    private void UpdateHeading(UIMouseEventArgs e, int buttonNumber)
    {
        message = $"You selected Button #{buttonNumber} at " +
            "mouse position: {e.ClientX} X {e.ClientY}.";
    }
}
```

> [!NOTE]
> <span data-ttu-id="ea15e-211">Hacer **no** usar la variable de bucle (`i`) en un `for` bucle directamente en una expresión lambda.</span><span class="sxs-lookup"><span data-stu-id="ea15e-211">Do **not** use the loop variable (`i`) in a `for` loop directly in a lambda expression.</span></span> <span data-ttu-id="ea15e-212">En caso contrario, se usa la misma variable por todas las expresiones lambda causando `i`del valor para que sean iguales en todas las expresiones lambda.</span><span class="sxs-lookup"><span data-stu-id="ea15e-212">Otherwise the same variable is used by all lambda expressions causing `i`'s value to be the same in all lambdas.</span></span> <span data-ttu-id="ea15e-213">Siempre captura su valor en una variable local (`buttonNumber` en el ejemplo anterior) y, a continuación, usarla.</span><span class="sxs-lookup"><span data-stu-id="ea15e-213">Always capture its value in a local variable (`buttonNumber` in the preceding example) and then use it.</span></span>

## <a name="capture-references-to-components"></a><span data-ttu-id="ea15e-214">Capturar las referencias a componentes</span><span class="sxs-lookup"><span data-stu-id="ea15e-214">Capture references to components</span></span>

<span data-ttu-id="ea15e-215">Referencias de componentes proporcionan una manera de obtener una referencia a una instancia del componente para que pueda emitir comandos a esa instancia, como `Show` o `Reset`.</span><span class="sxs-lookup"><span data-stu-id="ea15e-215">Component references provide a way get a reference to a component instance so that you can issue commands to that instance, such as `Show` or `Reset`.</span></span> <span data-ttu-id="ea15e-216">Para capturar una referencia de componente, agregue un `ref` de atributo para el componente secundario y, a continuación, defina un campo con el mismo nombre y el mismo tipo que el componente secundario.</span><span class="sxs-lookup"><span data-stu-id="ea15e-216">To capture a component reference, add a `ref` attribute to the child component and then define a field with the same name and the same type as the child component.</span></span>

```cshtml
<MyLoginDialog ref="loginDialog" ... />

@functions {
    private MyLoginDialog loginDialog;

    private void OnSomething()
    {
        loginDialog.Show();
    }
}
```

<span data-ttu-id="ea15e-217">Cuando se procesa el componente, el `loginDialog` campo se rellena con el `MyLoginDialog` instancia del componente secundario.</span><span class="sxs-lookup"><span data-stu-id="ea15e-217">When the component is rendered, the `loginDialog` field is populated with the `MyLoginDialog` child component instance.</span></span> <span data-ttu-id="ea15e-218">A continuación, puede invocar métodos de .NET en la instancia del componente.</span><span class="sxs-lookup"><span data-stu-id="ea15e-218">You can then invoke .NET methods on the component instance.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ea15e-219">El `loginDialog` variable solo se rellena cuando se procesa el componente y su salida incluye el `MyLoginDialog` elemento.</span><span class="sxs-lookup"><span data-stu-id="ea15e-219">The `loginDialog` variable is only populated after the component is rendered and its output includes the `MyLoginDialog` element.</span></span> <span data-ttu-id="ea15e-220">Hasta ese punto, no hay nada que hacer referencia.</span><span class="sxs-lookup"><span data-stu-id="ea15e-220">Until that point, there's nothing to reference.</span></span> <span data-ttu-id="ea15e-221">Para manipular las referencias de componentes después de que el componente ha terminado de procesarse, utilice el `OnAfterRenderAsync` o `OnAfterRender` métodos.</span><span class="sxs-lookup"><span data-stu-id="ea15e-221">To manipulate components references after the component has finished rendering, use the `OnAfterRenderAsync` or `OnAfterRender` methods.</span></span>

<span data-ttu-id="ea15e-222">Mientras que captura las referencias de componentes usa una sintaxis similar a [captura las referencias de elemento](xref:razor-components/javascript-interop#capture-references-to-elements), no es un [interoperabilidad de JavaScript](xref:razor-components/javascript-interop) característica.</span><span class="sxs-lookup"><span data-stu-id="ea15e-222">While capturing component references uses a similar syntax to [capturing element references](xref:razor-components/javascript-interop#capture-references-to-elements), it isn't a [JavaScript interop](xref:razor-components/javascript-interop) feature.</span></span> <span data-ttu-id="ea15e-223">Referencias de componentes no se pasan a código de JavaScript; que se usan en el código. NET.</span><span class="sxs-lookup"><span data-stu-id="ea15e-223">Component references aren't passed to JavaScript code; they're only used in .NET code.</span></span>

> [!NOTE]
> <span data-ttu-id="ea15e-224">Hacer **no** usar referencias de componentes para el estado de los componentes secundarios se modifique.</span><span class="sxs-lookup"><span data-stu-id="ea15e-224">Do **not** use component references to mutate the state of child components.</span></span> <span data-ttu-id="ea15e-225">En su lugar, use parámetros declarativos normales para pasar datos a los componentes secundarios.</span><span class="sxs-lookup"><span data-stu-id="ea15e-225">Instead, use normal declarative parameters to pass data to child components.</span></span> <span data-ttu-id="ea15e-226">Esto hace que los componentes secundarios procesar automáticamente en el momento adecuado.</span><span class="sxs-lookup"><span data-stu-id="ea15e-226">This causes child components to rerender at the correct times automatically.</span></span>

## <a name="lifecycle-methods"></a><span data-ttu-id="ea15e-227">Métodos de ciclo de vida</span><span class="sxs-lookup"><span data-stu-id="ea15e-227">Lifecycle methods</span></span>

`OnInitAsync` <span data-ttu-id="ea15e-228">y `OnInit` ejecutar código para inicializar el componente.</span><span class="sxs-lookup"><span data-stu-id="ea15e-228">and `OnInit` execute code to initialize the component.</span></span> <span data-ttu-id="ea15e-229">Para realizar una operación asincrónica, use `OnInitAsync` y `await` palabra clave en la operación:</span><span class="sxs-lookup"><span data-stu-id="ea15e-229">To perform an asynchronous operation, use `OnInitAsync` and the `await` keyword on the operation:</span></span>

```csharp
protected override async Task OnInitAsync()
{
    await ...
}
```

<span data-ttu-id="ea15e-230">Para una operación sincrónica, use `OnInit`:</span><span class="sxs-lookup"><span data-stu-id="ea15e-230">For a synchronous operation, use `OnInit`:</span></span>

```csharp
protected override void OnInit()
{
    ...
}
```

`OnParametersSetAsync` <span data-ttu-id="ea15e-231">y `OnParametersSet` se llama cuando un componente ha recibido los parámetros de su elemento primario y los valores se asignan a las propiedades.</span><span class="sxs-lookup"><span data-stu-id="ea15e-231">and `OnParametersSet` are called when a component has received parameters from its parent and the values are assigned to properties.</span></span> <span data-ttu-id="ea15e-232">Estos métodos se ejecutan después de la inicialización de componente y, a continuación, cada vez que el componente se representa:</span><span class="sxs-lookup"><span data-stu-id="ea15e-232">These methods are executed after component initialization and then each time the component is rendered:</span></span>

```csharp
protected override async Task OnParametersSetAsync()
{
    await ...
}
```

```csharp
protected override void OnParametersSet()
{
    ...
}
```

`OnAfterRenderAsync` <span data-ttu-id="ea15e-233">y `OnAfterRender` se invocan después de un componente ha terminado de procesarse.</span><span class="sxs-lookup"><span data-stu-id="ea15e-233">and `OnAfterRender` are called after a component has finished rendering.</span></span> <span data-ttu-id="ea15e-234">Las referencias de elemento y el componente se rellenan en este momento.</span><span class="sxs-lookup"><span data-stu-id="ea15e-234">Element and component references are populated at this point.</span></span> <span data-ttu-id="ea15e-235">Use esta fase para realizar pasos de inicialización adicionales con el contenido representado, como la activación de bibliotecas de JavaScript de terceros que operan en los elementos DOM representados.</span><span class="sxs-lookup"><span data-stu-id="ea15e-235">Use this stage to perform additional initialization steps using the rendered content, such as activating third-party JavaScript libraries that operate on the rendered DOM elements.</span></span>

```csharp
protected override async Task OnAfterRenderAsync()
{
    await ...
}
```

```csharp
protected override void OnAfterRender()
{
    ...
}
```

`SetParameters` <span data-ttu-id="ea15e-236">se puede invalidar para ejecutar código antes de que se establecen los parámetros:</span><span class="sxs-lookup"><span data-stu-id="ea15e-236">can be overridden to execute code before parameters are set:</span></span>

```csharp
public override void SetParameters(ParameterCollection parameters)
{
    ...

    base.SetParameters(parameters);
}
```

<span data-ttu-id="ea15e-237">Si `base.SetParameters` no invoca, el código personalizado puede interpretar el valor de los parámetros entrantes en cualquier forma necesarios.</span><span class="sxs-lookup"><span data-stu-id="ea15e-237">If `base.SetParameters` isn't invoked, the custom code can interpret the incoming parameters value in any way required.</span></span> <span data-ttu-id="ea15e-238">Por ejemplo, los parámetros de entrada no son necesarios para asignarse a las propiedades de la clase.</span><span class="sxs-lookup"><span data-stu-id="ea15e-238">For example, the incoming parameters aren't required to be assigned to the properties on the class.</span></span>

`ShouldRender` <span data-ttu-id="ea15e-239">se puede invalidar para suprimir la actualización de la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="ea15e-239">can be overridden to suppress refreshing of the UI.</span></span> <span data-ttu-id="ea15e-240">Si la implementación devuelve `true`, se actualiza la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="ea15e-240">If the implementation returns `true`, the UI is refreshed.</span></span> <span data-ttu-id="ea15e-241">Aunque `ShouldRender` es invalidar, el componente se representa siempre inicialmente.</span><span class="sxs-lookup"><span data-stu-id="ea15e-241">Even if `ShouldRender` is overridden, the component is always initially rendered.</span></span>

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

## <a name="component-disposal-with-idisposable"></a><span data-ttu-id="ea15e-242">Eliminación de componentes con IDisposable</span><span class="sxs-lookup"><span data-stu-id="ea15e-242">Component disposal with IDisposable</span></span>

<span data-ttu-id="ea15e-243">Si implementa un componente <xref:System.IDisposable>, [método Dispose](/dotnet/standard/garbage-collection/implementing-dispose) se llama cuando se quita el componente de la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="ea15e-243">If a component implements <xref:System.IDisposable>, the [Dispose method](/dotnet/standard/garbage-collection/implementing-dispose) is called when the component is removed from the UI.</span></span> <span data-ttu-id="ea15e-244">El siguiente componente usa `@implements IDisposable` y `Dispose` método:</span><span class="sxs-lookup"><span data-stu-id="ea15e-244">The following component uses `@implements IDisposable` and the `Dispose` method:</span></span>

```csharp
@using System
@implements IDisposable

...

@functions {
    public void Dispose()
    {
        ...
    }
}
```

## <a name="routing"></a><span data-ttu-id="ea15e-245">Enrutamiento</span><span class="sxs-lookup"><span data-stu-id="ea15e-245">Routing</span></span>

<span data-ttu-id="ea15e-246">Enrutamiento en los componentes de Razor se logra al proporcionar una plantilla de ruta para cada componente puede tener acceso en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ea15e-246">Routing in Razor Components is achieved by providing a route template to each accessible component in the app.</span></span>

<span data-ttu-id="ea15e-247">Cuando un *.cshtml* de archivos con un `@page` directiva se compila, se proporciona la clase generada una <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> especificar la plantilla de ruta.</span><span class="sxs-lookup"><span data-stu-id="ea15e-247">When a *.cshtml* file with an `@page` directive is compiled, the generated class is given a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span> <span data-ttu-id="ea15e-248">En tiempo de ejecución, el enrutador busca las clases de componente con un `RouteAttribute` y representa el componente tiene una plantilla de ruta que coincida con la dirección URL solicitada.</span><span class="sxs-lookup"><span data-stu-id="ea15e-248">At runtime, the router looks for component classes with a `RouteAttribute` and renders whichever component has a route template that matches the requested URL.</span></span>

<span data-ttu-id="ea15e-249">Varias plantillas de ruta se pueden aplicar a un componente.</span><span class="sxs-lookup"><span data-stu-id="ea15e-249">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="ea15e-250">El siguiente componente responde a las solicitudes de `/BlazorRoute` y `/DifferentBlazorRoute`:</span><span class="sxs-lookup"><span data-stu-id="ea15e-250">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.cshtml?name=snippet_BlazorRoute)]

## <a name="route-parameters"></a><span data-ttu-id="ea15e-251">Parámetros de ruta</span><span class="sxs-lookup"><span data-stu-id="ea15e-251">Route parameters</span></span>

<span data-ttu-id="ea15e-252">Los componentes pueden recibir parámetros de ruta de la plantilla de ruta proporcionada en el `@page` directiva.</span><span class="sxs-lookup"><span data-stu-id="ea15e-252">Components can receive route parameters from the route template provided in the `@page` directive.</span></span> <span data-ttu-id="ea15e-253">El enrutador usa los parámetros de ruta para rellenar los parámetros correspondientes de componente.</span><span class="sxs-lookup"><span data-stu-id="ea15e-253">The router uses route parameters to populate the corresponding component parameters.</span></span>

<span data-ttu-id="ea15e-254">*RouteParameter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="ea15e-254">*RouteParameter.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.cshtml?name=snippet_RouteParameter)]

<span data-ttu-id="ea15e-255">No se admiten los parámetros opcionales, por lo que dos `@page` directivas se aplican en el ejemplo anterior.</span><span class="sxs-lookup"><span data-stu-id="ea15e-255">Optional parameters aren't supported, so two `@page` directives are applied in the example above.</span></span> <span data-ttu-id="ea15e-256">La primera permite la navegación al componente sin un parámetro.</span><span class="sxs-lookup"><span data-stu-id="ea15e-256">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="ea15e-257">El segundo `@page` directiva toma el `{text}` parámetro de ruta y asigna el valor para el `Text` propiedad.</span><span class="sxs-lookup"><span data-stu-id="ea15e-257">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="base-class-inheritance-for-a-code-behind-experience"></a><span data-ttu-id="ea15e-258">Herencia de clase base para una experiencia de "código subyacente"</span><span class="sxs-lookup"><span data-stu-id="ea15e-258">Base class inheritance for a "code-behind" experience</span></span>

<span data-ttu-id="ea15e-259">Los archivos de componente (*.cshtml*) mezclar código HTML y C# código en el mismo archivo de procesamiento.</span><span class="sxs-lookup"><span data-stu-id="ea15e-259">Component files (*.cshtml*) mix HTML markup and C# processing code in the same file.</span></span> <span data-ttu-id="ea15e-260">El `@inherits` directiva puede usarse para proporcionar una experiencia de "código" que separa el marcado del componente de código de procesamiento de las aplicaciones de componentes de Razor.</span><span class="sxs-lookup"><span data-stu-id="ea15e-260">The `@inherits` directive can be used to provide Razor Components apps with a "code-behind" experience that separates component markup from processing code.</span></span>

<span data-ttu-id="ea15e-261">El [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) muestra cómo un componente puede heredar una clase base, `BlazorRocksBase`, para proporcionar las propiedades y los métodos del componente.</span><span class="sxs-lookup"><span data-stu-id="ea15e-261">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) shows how a component can inherit a base class, `BlazorRocksBase`, to provide the component's properties and methods.</span></span>

<span data-ttu-id="ea15e-262">*BlazorRocks.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="ea15e-262">*BlazorRocks.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRocks.cshtml?name=snippet_BlazorRocks)]

<span data-ttu-id="ea15e-263">*BlazorRocksBase.cs*:</span><span class="sxs-lookup"><span data-stu-id="ea15e-263">*BlazorRocksBase.cs*:</span></span>

[!code-csharp[](common/samples/3.x/BlazorSample/Pages/BlazorRocksBase.cs)]

<span data-ttu-id="ea15e-264">La clase base debe derivarse de `ComponentBase`.</span><span class="sxs-lookup"><span data-stu-id="ea15e-264">The base class should derive from `ComponentBase`.</span></span>

## <a name="razor-support"></a><span data-ttu-id="ea15e-265">Compatibilidad con Razor</span><span class="sxs-lookup"><span data-stu-id="ea15e-265">Razor support</span></span>

**<span data-ttu-id="ea15e-266">Directivas de Razor</span><span class="sxs-lookup"><span data-stu-id="ea15e-266">Razor directives</span></span>**

<span data-ttu-id="ea15e-267">Directivas de Razor se muestran en la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="ea15e-267">Razor directives are shown in the following table.</span></span>

| <span data-ttu-id="ea15e-268">Directiva</span><span class="sxs-lookup"><span data-stu-id="ea15e-268">Directive</span></span> | <span data-ttu-id="ea15e-269">Descripción</span><span class="sxs-lookup"><span data-stu-id="ea15e-269">Description</span></span> |
| --------- | ----------- |
| [<span data-ttu-id="ea15e-270">\@Funciones</span><span class="sxs-lookup"><span data-stu-id="ea15e-270">\@functions</span></span>](xref:mvc/views/razor#section-5) | <span data-ttu-id="ea15e-271">Agrega un C# bloque de código para un componente.</span><span class="sxs-lookup"><span data-stu-id="ea15e-271">Adds a C# code block to a component.</span></span> |
| `@implements` | <span data-ttu-id="ea15e-272">Implementa una interfaz para la clase de componente generado.</span><span class="sxs-lookup"><span data-stu-id="ea15e-272">Implements an interface for the generated component class.</span></span> |
| [<span data-ttu-id="ea15e-273">\@Hereda</span><span class="sxs-lookup"><span data-stu-id="ea15e-273">\@inherits</span></span>](xref:mvc/views/razor#section-3) | <span data-ttu-id="ea15e-274">Proporciona control total sobre la clase que hereda el componente.</span><span class="sxs-lookup"><span data-stu-id="ea15e-274">Provides full control of the class that the component inherits.</span></span> |
| [<span data-ttu-id="ea15e-275">\@Insertar</span><span class="sxs-lookup"><span data-stu-id="ea15e-275">\@inject</span></span>](xref:mvc/views/razor#section-4) | <span data-ttu-id="ea15e-276">Permite servicios de inserción desde el [contenedor de servicios](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="ea15e-276">Enables service injection from the [service container](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="ea15e-277">Para más información, vea [Dependency injection into views](xref:mvc/views/dependency-injection) (Inserción de dependencias en vistas).</span><span class="sxs-lookup"><span data-stu-id="ea15e-277">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span> |
| `@layout` | <span data-ttu-id="ea15e-278">Especifica un componente de diseño.</span><span class="sxs-lookup"><span data-stu-id="ea15e-278">Specifies a layout component.</span></span> <span data-ttu-id="ea15e-279">Componentes de diseño se utilizan para evitar la incoherencia y duplicación de código.</span><span class="sxs-lookup"><span data-stu-id="ea15e-279">Layout components are used to avoid code duplication and inconsistency.</span></span> |
| [<span data-ttu-id="ea15e-280">\@Página</span><span class="sxs-lookup"><span data-stu-id="ea15e-280">\@page</span></span>](xref:razor-pages/index#razor-pages) | <span data-ttu-id="ea15e-281">Especifica que el componente debe controlar las solicitudes directamente.</span><span class="sxs-lookup"><span data-stu-id="ea15e-281">Specifies that the component should handle requests directly.</span></span> <span data-ttu-id="ea15e-282">El `@page` directiva se puede especificar con una ruta y los parámetros opcionales.</span><span class="sxs-lookup"><span data-stu-id="ea15e-282">The `@page` directive can be specified with a route and optional parameters.</span></span> <span data-ttu-id="ea15e-283">A diferencia de las páginas de Razor, el `@page` directiva no tiene que ser la primera directiva en la parte superior del archivo.</span><span class="sxs-lookup"><span data-stu-id="ea15e-283">Unlike Razor Pages, the `@page` directive doesn't need to be the first directive at the top of the file.</span></span> <span data-ttu-id="ea15e-284">Para más información, vea [Enrutamiento](xref:razor-components/routing).</span><span class="sxs-lookup"><span data-stu-id="ea15e-284">For more information, see [Routing](xref:razor-components/routing).</span></span> |
| [<span data-ttu-id="ea15e-285">\@Uso de</span><span class="sxs-lookup"><span data-stu-id="ea15e-285">\@using</span></span>](xref:mvc/views/razor#using) | <span data-ttu-id="ea15e-286">Agrega el C# `using` la directiva a la clase de componente generado.</span><span class="sxs-lookup"><span data-stu-id="ea15e-286">Adds the C# `using` directive to the generated component class.</span></span> |
| [<span data-ttu-id="ea15e-287">\@addTagHelper</span><span class="sxs-lookup"><span data-stu-id="ea15e-287">\@addTagHelper</span></span>](xref:mvc/views/razor#tag-helpers) | <span data-ttu-id="ea15e-288">Usar `@addTagHelper` para utilizar un componente en un ensamblado diferente al ensamblado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ea15e-288">Use `@addTagHelper` to use a component in a different assembly than the app's assembly.</span></span> |

**<span data-ttu-id="ea15e-289">Atributos condicionales</span><span class="sxs-lookup"><span data-stu-id="ea15e-289">Conditional attributes</span></span>**

<span data-ttu-id="ea15e-290">Los atributos se representan condicionalmente en función del valor. NET.</span><span class="sxs-lookup"><span data-stu-id="ea15e-290">Attributes are conditionally rendered based on the .NET value.</span></span> <span data-ttu-id="ea15e-291">Si el valor es `false` o `null`, no se representa el atributo.</span><span class="sxs-lookup"><span data-stu-id="ea15e-291">If the value is `false` or `null`,  the attribute isn't rendered.</span></span> <span data-ttu-id="ea15e-292">Si el valor es `true`, el atributo se representa minimizada.</span><span class="sxs-lookup"><span data-stu-id="ea15e-292">If the value is `true`, the attribute is rendered minimized.</span></span>

<span data-ttu-id="ea15e-293">En el ejemplo siguiente, `IsCompleted` determina si `checked` se representa en el marcado del control:</span><span class="sxs-lookup"><span data-stu-id="ea15e-293">In the following example, `IsCompleted` determines if `checked` is rendered in the control's markup:</span></span>

```cshtml
<input type="checkbox" checked="@IsCompleted">

@functions {
    [Parameter]
    private bool IsCompleted { get; set; }
}
```

<span data-ttu-id="ea15e-294">Si `IsCompleted` es `true`, la casilla de verificación se representa como:</span><span class="sxs-lookup"><span data-stu-id="ea15e-294">If `IsCompleted` is `true`, the check box is rendered as:</span></span>

```html
<input type="checkbox" checked>
```

<span data-ttu-id="ea15e-295">Si `IsCompleted` es `false`, la casilla de verificación se representa como:</span><span class="sxs-lookup"><span data-stu-id="ea15e-295">If `IsCompleted` is `false`, the check box is rendered as:</span></span>

```html
<input type="checkbox">
```

**<span data-ttu-id="ea15e-296">Información adicional en Razor</span><span class="sxs-lookup"><span data-stu-id="ea15e-296">Additional information on Razor</span></span>**

<span data-ttu-id="ea15e-297">Para obtener más información sobre Razor, consulte el [referencia de sintaxis de Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="ea15e-297">For more information on Razor, see the [Razor syntax reference](xref:mvc/views/razor).</span></span>

## <a name="raw-html"></a><span data-ttu-id="ea15e-298">Código HTML sin procesar</span><span class="sxs-lookup"><span data-stu-id="ea15e-298">Raw HTML</span></span>

<span data-ttu-id="ea15e-299">Las cadenas se normalmente representan mediante nodos de texto de DOM, lo que significa que cualquier marcado pueden contener se omite y se tratan como texto literal.</span><span class="sxs-lookup"><span data-stu-id="ea15e-299">Strings are normally rendered using DOM text nodes, which means that any markup they may contain is ignored and treated as literal text.</span></span> <span data-ttu-id="ea15e-300">Para representar HTML sin formato, ajustar el contenido HTML en un `MarkupString` valor.</span><span class="sxs-lookup"><span data-stu-id="ea15e-300">To render raw HTML, wrap the HTML content in a `MarkupString` value.</span></span> <span data-ttu-id="ea15e-301">El valor se puede analizar como HTML o SVG y se inserta en el DOM.</span><span class="sxs-lookup"><span data-stu-id="ea15e-301">The value is parsed as HTML or SVG and inserted into the DOM.</span></span>

> [!WARNING]
> <span data-ttu-id="ea15e-302">Representación HTML sin formato que se construye a partir de cualquiera de no confianza origen es un **riesgos de seguridad** y debe evitarse.</span><span class="sxs-lookup"><span data-stu-id="ea15e-302">Rendering raw HTML constructed from any untrusted source is a **security risk** and should be avoided!</span></span>

<span data-ttu-id="ea15e-303">El ejemplo siguiente muestra utilizando el `MarkupString` tipo para agregar un bloque de contenido HTML estático a la salida representada de un componente:</span><span class="sxs-lookup"><span data-stu-id="ea15e-303">The following example shows using the `MarkupString` type to add a block of static HTML content to the rendered output of a component:</span></span>

```html
@((MarkupString)myMarkup)

@functions {
    private string myMarkup = 
        "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="templated-components"></a><span data-ttu-id="ea15e-304">Componentes con plantilla</span><span class="sxs-lookup"><span data-stu-id="ea15e-304">Templated components</span></span>

<span data-ttu-id="ea15e-305">Los componentes con plantilla son componentes que acepten una o varias plantillas de interfaz de usuario como parámetros, que pueden usarse como parte de la lógica de representación del componente.</span><span class="sxs-lookup"><span data-stu-id="ea15e-305">Templated components are components that accept one or more UI templates as parameters, which can then be used as part of the component's rendering logic.</span></span> <span data-ttu-id="ea15e-306">Los componentes con plantilla permiten crear componentes de nivel superior que son más reutilizables que componentes normales.</span><span class="sxs-lookup"><span data-stu-id="ea15e-306">Templated components allow you to author higher-level components that are more reusable than regular components.</span></span> <span data-ttu-id="ea15e-307">Algunos ejemplos incluyen:</span><span class="sxs-lookup"><span data-stu-id="ea15e-307">A couple of examples include:</span></span>

* <span data-ttu-id="ea15e-308">Un componente de tabla que permite al usuario especificar plantillas para filas, encabezado y pie de la tabla.</span><span class="sxs-lookup"><span data-stu-id="ea15e-308">A table component that allows a user to specify templates for the table's header, rows, and footer.</span></span>
* <span data-ttu-id="ea15e-309">Un componente de lista que permite al usuario especificar una plantilla para representar los elementos en una lista.</span><span class="sxs-lookup"><span data-stu-id="ea15e-309">A list component that allows a user to specify a template for rendering items in a list.</span></span>

### <a name="template-parameters"></a><span data-ttu-id="ea15e-310">Parámetros de plantilla</span><span class="sxs-lookup"><span data-stu-id="ea15e-310">Template parameters</span></span>

<span data-ttu-id="ea15e-311">Un componente basado en plantilla se define mediante la especificación de uno o varios parámetros del componente de tipo `RenderFragment` o `RenderFragment<T>`.</span><span class="sxs-lookup"><span data-stu-id="ea15e-311">A templated component is defined by specifying one or more component parameters of type `RenderFragment` or `RenderFragment<T>`.</span></span> <span data-ttu-id="ea15e-312">Un fragmento de representación representa un segmento de la interfaz de usuario que representa el componente.</span><span class="sxs-lookup"><span data-stu-id="ea15e-312">A render fragment represents a segment of UI that is rendered by the component.</span></span> <span data-ttu-id="ea15e-313">Opcionalmente, un fragmento de representación toma un parámetro que se puede especificar cuando se invoca el fragmento de representación.</span><span class="sxs-lookup"><span data-stu-id="ea15e-313">A render fragment optionally takes a parameter that can be specified when the render fragment is invoked.</span></span>

<span data-ttu-id="ea15e-314">*Components/TableTemplate.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="ea15e-314">*Components/TableTemplate.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TableTemplate.cshtml)]

<span data-ttu-id="ea15e-315">Cuando se usa un componente con plantilla, se pueden especificar los parámetros de plantilla utilizando los elementos secundarios que coinciden con los nombres de los parámetros (`TableHeader` y `RowTemplate` en el ejemplo siguiente):</span><span class="sxs-lookup"><span data-stu-id="ea15e-315">When using a templated component, the template parameters can be specified using child elements that match the names of the parameters (`TableHeader` and `RowTemplate` in the following example):</span></span>

```cshtml
<TableTemplate Items="@pets">
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

### <a name="template-context-parameters"></a><span data-ttu-id="ea15e-316">Parámetros de contexto de la plantilla</span><span class="sxs-lookup"><span data-stu-id="ea15e-316">Template context parameters</span></span>

<span data-ttu-id="ea15e-317">Argumentos de componente de tipo `RenderFragment<T>` pasa como elementos tienen un parámetro implícito denominado `context` (por ejemplo, de ejemplo de código anterior, `@context.PetId`), pero se puede cambiar el nombre de parámetro mediante el `Context` atributo en el elemento secundario elemento.</span><span class="sxs-lookup"><span data-stu-id="ea15e-317">Component arguments of type `RenderFragment<T>` passed as elements have an implicit parameter named `context` (for example from the preceding code sample, `@context.PetId`), but you can change the parameter name using the `Context` attribute on the child element.</span></span> <span data-ttu-id="ea15e-318">En el ejemplo siguiente, la `RowTemplate` del elemento `Context` atributo especifica el `pet` parámetro:</span><span class="sxs-lookup"><span data-stu-id="ea15e-318">In the following example, the `RowTemplate` element's `Context` attribute specifies the `pet` parameter:</span></span>

```cshtml
<TableTemplate Items="@pets">
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

<span data-ttu-id="ea15e-319">Como alternativa, puede especificar el `Context` atributo del elemento de componente.</span><span class="sxs-lookup"><span data-stu-id="ea15e-319">Alternatively, you can specify the `Context` attribute on the component element.</span></span> <span data-ttu-id="ea15e-320">Especificado `Context` atributo se aplica a todos los parámetros de plantilla especificada.</span><span class="sxs-lookup"><span data-stu-id="ea15e-320">The specified `Context` attribute applies to all specified template parameters.</span></span> <span data-ttu-id="ea15e-321">Esto puede ser útil cuando desee especificar el nombre del parámetro de contenido para el contenido secundario implícita (sin ajuste de cualquier elemento secundario).</span><span class="sxs-lookup"><span data-stu-id="ea15e-321">This can be useful when you want to specify the content parameter name for implicit child content (without any wrapping child element).</span></span> <span data-ttu-id="ea15e-322">En el ejemplo siguiente, la `Context` atributo aparece en el `TableTemplate` elemento y se aplica a todos los parámetros de plantilla:</span><span class="sxs-lookup"><span data-stu-id="ea15e-322">In the following example, the `Context` attribute appears on the `TableTemplate` element and applies to all template parameters:</span></span>

```cshtml
<TableTemplate Items="@pets" Context="pet">
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

### <a name="generic-typed-components"></a><span data-ttu-id="ea15e-323">Componentes de tipo genérico</span><span class="sxs-lookup"><span data-stu-id="ea15e-323">Generic-typed components</span></span>

<span data-ttu-id="ea15e-324">A menudo genéricamente se escriben los componentes con plantilla.</span><span class="sxs-lookup"><span data-stu-id="ea15e-324">Templated components are often generically typed.</span></span> <span data-ttu-id="ea15e-325">Por ejemplo, se puede usar un componente de la plantilla de vista de lista genérico para presentar `IEnumerable<T>` valores.</span><span class="sxs-lookup"><span data-stu-id="ea15e-325">For example, a generic List View Template component can be used to render `IEnumerable<T>` values.</span></span> <span data-ttu-id="ea15e-326">Para definir un componente genérico, utilice el `@typeparam` directiva para especificar los parámetros de tipo.</span><span class="sxs-lookup"><span data-stu-id="ea15e-326">To define a generic component, use the `@typeparam` directive to specify type parameters.</span></span>

<span data-ttu-id="ea15e-327">*Components/ListViewTemplate.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="ea15e-327">*Components/ListViewTemplate.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ListViewTemplate.cshtml)]

<span data-ttu-id="ea15e-328">Al usar los componentes de tipo genérico, el parámetro de tipo se deduce si es posible:</span><span class="sxs-lookup"><span data-stu-id="ea15e-328">When using generic-typed components, the type parameter is inferred if possible:</span></span>

```cshtml
<ListViewTemplate Items="@pets">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

<span data-ttu-id="ea15e-329">En caso contrario, el parámetro de tipo debe especificarse explícitamente mediante un atributo que coincida con el nombre del parámetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="ea15e-329">Otherwise, the type parameter must be explicitly specified using an attribute that matches the name of the type parameter.</span></span> <span data-ttu-id="ea15e-330">En el ejemplo siguiente, `TItem="Pet"` especifica el tipo:</span><span class="sxs-lookup"><span data-stu-id="ea15e-330">In the following example, `TItem="Pet"` specifies the type:</span></span>

```cshtml
<ListViewTemplate Items="@pets" TItem="Pet">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

## <a name="cascading-values-and-parameters"></a><span data-ttu-id="ea15e-331">Parámetros y valores en cascada</span><span class="sxs-lookup"><span data-stu-id="ea15e-331">Cascading values and parameters</span></span>

<span data-ttu-id="ea15e-332">En algunos escenarios, es conveniente a los datos de flujo de un componente de antecesor en un componente descendiente mediante [parámetros del componente](#component-parameters), especialmente cuando hay varios niveles de componente.</span><span class="sxs-lookup"><span data-stu-id="ea15e-332">In some scenarios, it's inconvenient to flow data from an ancestor component to a descendent component using [component parameters](#component-parameters), especially when there are several component layers.</span></span> <span data-ttu-id="ea15e-333">Los valores y parámetros en cascada solucionan este problema proporcionando una manera conveniente para un componente de antecesor que proporcione un valor para todos sus componentes descendientes.</span><span class="sxs-lookup"><span data-stu-id="ea15e-333">Cascading values and parameters solve this problem by providing a convenient way for an ancestor component to provide a value to all of its descendent components.</span></span> <span data-ttu-id="ea15e-334">Los valores y parámetros en cascada también proporcionan un enfoque para coordinar los componentes.</span><span class="sxs-lookup"><span data-stu-id="ea15e-334">Cascading values and parameters also provide an approach for components to coordinate.</span></span>

### <a name="theme-example"></a><span data-ttu-id="ea15e-335">Ejemplo de tema</span><span class="sxs-lookup"><span data-stu-id="ea15e-335">Theme example</span></span>

<span data-ttu-id="ea15e-336">En la siguiente *tema* ejemplo desde la aplicación de ejemplo, el `ThemeInfo` clase especifica la información del tema para que fluyan hacia abajo de la jerarquía de componentes para que todos los botones dentro de una parte determinada de la aplicación comparten el mismo estilo.</span><span class="sxs-lookup"><span data-stu-id="ea15e-336">In the following *Theme* example from the sample app, the `ThemeInfo` class specifies the theme information to flow down the component hierarchy so that all of the buttons within a given part of the app share the same style.</span></span>

<span data-ttu-id="ea15e-337">*UIThemeClasses/ThemeInfo.cs*:</span><span class="sxs-lookup"><span data-stu-id="ea15e-337">*UIThemeClasses/ThemeInfo.cs*:</span></span>

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

<span data-ttu-id="ea15e-338">Un componente de antecesor puede proporcionar un valor en cascada mediante el componente de valor en cascada.</span><span class="sxs-lookup"><span data-stu-id="ea15e-338">An ancestor component can provide a cascading value using the Cascading Value component.</span></span> <span data-ttu-id="ea15e-339">El componente de valor en cascada ajusta un subárbol de la jerarquía de componentes y proporciona un valor único a todos los componentes dentro de ese subárbol.</span><span class="sxs-lookup"><span data-stu-id="ea15e-339">The Cascading Value component wraps a subtree of the component hierarchy and supplies a single value to all components within that subtree.</span></span>

<span data-ttu-id="ea15e-340">Por ejemplo, la aplicación de ejemplo especifica la información del tema (`ThemeInfo`) en uno de los diseños de la aplicación como un parámetro en cascada para todos los componentes que constituyen el cuerpo de diseño de la `@Body` propiedad.</span><span class="sxs-lookup"><span data-stu-id="ea15e-340">For example, the sample app specifies theme information (`ThemeInfo`) in one of the app's layouts as a cascading parameter for all components that make up the layout body of the `@Body` property.</span></span> `ButtonClass` <span data-ttu-id="ea15e-341">se asigna un valor de `btn-success` en el componente de diseño.</span><span class="sxs-lookup"><span data-stu-id="ea15e-341">is assigned a value of `btn-success` in the layout component.</span></span> <span data-ttu-id="ea15e-342">Cualquier componente descendiente puede consumir esta propiedad mediante la `ThemeInfo` objeto en cascada.</span><span class="sxs-lookup"><span data-stu-id="ea15e-342">Any descendent component can consume this property through the `ThemeInfo` cascading object.</span></span>

<span data-ttu-id="ea15e-343">*Shared/CascadingValuesParametersLayout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="ea15e-343">*Shared/CascadingValuesParametersLayout.cshtml*:</span></span>

```cshtml
@inherits LayoutComponentBase
@using BlazorSample.UIThemeClasses

<div class="container-fluid">
    <div class="row">
        <div class="col-sm-3">
            <NavMenu />
        </div>
        <div class="col-sm-9">
            <CascadingValue Value="@theme">
                <div class="content px-4">
                    @Body
                </div>
            </CascadingValue>
        </div>
    </div>
</div>

@functions {
    private ThemeInfo theme = new ThemeInfo { ButtonClass = "btn-success" };
}
```

<span data-ttu-id="ea15e-344">Para hacer uso de valores en cascada, los componentes declarar parámetros en cascada mediante el `[CascadingParameter]` de atributo o según un valor de nombre de cadena:</span><span class="sxs-lookup"><span data-stu-id="ea15e-344">To make use of cascading values, components declare cascading parameters using the `[CascadingParameter]` attribute or based on a string name value:</span></span>

```cshtml
<CascadingValue Value=@PermInfo Name="UserPermissions">...</CascadingValue>

[CascadingParameter(Name = "UserPermissions")]
private PermInfo Permissions { get; set; }
```

<span data-ttu-id="ea15e-345">Enlace con un valor de nombre de la cadena es relevante si tiene varios valores en cascada del mismo tipo y necesita diferenciarlas dentro del mismo subárbol.</span><span class="sxs-lookup"><span data-stu-id="ea15e-345">Binding with a string name value is relevant if you have multiple cascading values of the same type and need to differentiate them within the same subtree.</span></span>

<span data-ttu-id="ea15e-346">Los valores en cascada se enlazan a los parámetros en cascada por tipo.</span><span class="sxs-lookup"><span data-stu-id="ea15e-346">Cascading values are bound to cascading parameters by type.</span></span>

<span data-ttu-id="ea15e-347">En la aplicación de ejemplo, el componente de tema de los parámetros de valores en cascada se enlaza a la `ThemeInfo` valor en cascada a un parámetro en cascada.</span><span class="sxs-lookup"><span data-stu-id="ea15e-347">In the sample app, the Cascading Values Parameters Theme component binds to the `ThemeInfo` cascading value to a cascading parameter.</span></span> <span data-ttu-id="ea15e-348">El parámetro se usa para establecer la clase CSS para uno de los botones mostrados por el componente.</span><span class="sxs-lookup"><span data-stu-id="ea15e-348">The parameter is used to set the CSS class for one of the buttons displayed by the component.</span></span>

<span data-ttu-id="ea15e-349">*Pages/CascadingValuesParametersTheme.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="ea15e-349">*Pages/CascadingValuesParametersTheme.cshtml*:</span></span>

```cshtml
@page "/cascadingvaluesparameterstheme"
@layout CascadingValuesParametersLayout
@using BlazorSample.UIThemeClasses

<h1>Cascading Values & Parameters</h1>

<p>Current count: @currentCount</p>

<p>
    <button class="btn" onclick="@IncrementCount">
        Increment Counter (Unthemed)
    </button>
</p>

<p>
    <button class="btn @ThemeInfo.ButtonClass" onclick="@IncrementCount">
        Increment Counter (Themed)
    </button>
</p>

@functions {
    private int currentCount = 0;

    [CascadingParameter] protected ThemeInfo ThemeInfo { get; set; }

    private void IncrementCount()
    {
        currentCount++;
    }
}
```

### <a name="tabset-example"></a><span data-ttu-id="ea15e-350">Ejemplo de TabSet</span><span class="sxs-lookup"><span data-stu-id="ea15e-350">TabSet example</span></span>

<span data-ttu-id="ea15e-351">Los parámetros en cascada también habilitar componentes colaborar a través de la jerarquía de componentes.</span><span class="sxs-lookup"><span data-stu-id="ea15e-351">Cascading parameters also enable components to collaborate across the component hierarchy.</span></span> <span data-ttu-id="ea15e-352">Por ejemplo, considere la siguiente *TabSet* ejemplo en la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="ea15e-352">For example, consider the following *TabSet* example in the sample app.</span></span>

<span data-ttu-id="ea15e-353">La aplicación de ejemplo tiene un `ITab` interfaz que implemente de pestañas:</span><span class="sxs-lookup"><span data-stu-id="ea15e-353">The sample app has an `ITab` interface that tabs implement:</span></span>

[!code-cs[](common/samples/3.x/BlazorSample/UIInterfaces/ITab.cs)]

<span data-ttu-id="ea15e-354">El componente de TabSet de parámetros de valores en cascada utiliza el componente de conjunto de pestañas, que contiene varios componentes de la pestaña:</span><span class="sxs-lookup"><span data-stu-id="ea15e-354">The Cascading Values Parameters TabSet component uses the Tab Set component, which contains several Tab components:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/CascadingValuesParametersTabSet.cshtml?name=snippet_TabSet)]

<span data-ttu-id="ea15e-355">Los componentes de la pestaña secundarios explícitamente no se pasan como parámetros para el conjunto de pestañas.</span><span class="sxs-lookup"><span data-stu-id="ea15e-355">The child Tab components aren't explicitly passed as parameters to the Tab Set.</span></span> <span data-ttu-id="ea15e-356">En su lugar, los componentes de la pestaña secundarios forman parte del contenido secundario de la pestaña configurar.</span><span class="sxs-lookup"><span data-stu-id="ea15e-356">Instead, the child Tab components are part of the child content of the Tab Set.</span></span> <span data-ttu-id="ea15e-357">Sin embargo, el conjunto de pestañas todavía necesita saber acerca de cada componente de la pestaña para que pueda representar los encabezados y la pestaña activa. Para habilitar esta coordinación sin necesidad de código adicional, el componente de conjunto de pestañas *puede proporcionar como un valor en cascada* que, a continuación, recoge los componentes de la pestaña descendientes.</span><span class="sxs-lookup"><span data-stu-id="ea15e-357">However, the Tab Set still needs to know about each Tab component so that it can render the headers and the active tab. To enable this coordination without requiring additional code, the Tab Set component *can provide itself as a cascading value* that is then picked up by the descendent Tab components.</span></span>

<span data-ttu-id="ea15e-358">*Components/TabSet.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="ea15e-358">*Components/TabSet.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TabSet.cshtml)]

<span data-ttu-id="ea15e-359">La captura de los componentes de pestaña descendiente el conjunto de pestañas que lo contiene como un parámetro en cascada, por lo que los componentes de la ficha que se agreguen a la coordenada y de conjunto de pestañas en la ficha que está activa.</span><span class="sxs-lookup"><span data-stu-id="ea15e-359">The descendent Tab components capture the containing Tab Set as a cascading parameter, so the Tab components add themselves to the Tab Set and coordinate on which tab is active.</span></span>

<span data-ttu-id="ea15e-360">*Components/Tab.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="ea15e-360">*Components/Tab.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/Tab.cshtml)]

## <a name="razor-templates"></a><span data-ttu-id="ea15e-361">Plantillas de Razor</span><span class="sxs-lookup"><span data-stu-id="ea15e-361">Razor templates</span></span>

<span data-ttu-id="ea15e-362">Representar fragmentos se pueden definir mediante la sintaxis de la plantilla de Razor.</span><span class="sxs-lookup"><span data-stu-id="ea15e-362">Render fragments can be defined using Razor template syntax.</span></span> <span data-ttu-id="ea15e-363">Las plantillas de Razor son una manera de definir un fragmento de código de interfaz de usuario y asumir el formato siguiente:</span><span class="sxs-lookup"><span data-stu-id="ea15e-363">Razor templates are a way to define a UI snippet and assume the following format:</span></span>

```cshtml
@<tag>...</tag>
```

<span data-ttu-id="ea15e-364">El ejemplo siguiente muestra cómo especificar `RenderFragment` y `RenderFragment<T>` valores.</span><span class="sxs-lookup"><span data-stu-id="ea15e-364">The following example illustrates how to specify `RenderFragment` and `RenderFragment<T>` values.</span></span>

<span data-ttu-id="ea15e-365">*RazorTemplates.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="ea15e-365">*RazorTemplates.cshtml*:</span></span>

```cshtml
@{
    RenderFragment template = @<p>The time is @DateTime.Now.</p>;
    RenderFragment<Pet> petTemplate = (pet) => @<p>Your pet's name is @pet.Name.</p>;
}
```

<span data-ttu-id="ea15e-366">Representar fragmentos definidos con Razor plantillas se pueden pasar como argumentos a los componentes con plantilla o presentar directamente.</span><span class="sxs-lookup"><span data-stu-id="ea15e-366">Render fragments defined using Razor templates can be passed as arguments to templated components or rendered directly.</span></span> <span data-ttu-id="ea15e-367">Por ejemplo, las plantillas anteriores se representan directamente con el siguiente marcado de Razor:</span><span class="sxs-lookup"><span data-stu-id="ea15e-367">For example, the previous templates are directly rendered with the following Razor markup:</span></span>

```cshtml
@template

@petTemplate(new Pet { Name = "Rex" })
```

<span data-ttu-id="ea15e-368">Salida representada:</span><span class="sxs-lookup"><span data-stu-id="ea15e-368">Rendered output:</span></span>

```
The time is 10/04/2018 01:26:52.

Your pet's name is Rex.
```

## <a name="manual-rendertreebuilder-logic"></a><span data-ttu-id="ea15e-369">Lógica de RenderTreeBuilder manual</span><span class="sxs-lookup"><span data-stu-id="ea15e-369">Manual RenderTreeBuilder logic</span></span>

`Microsoft.AspNetCore.Components.RenderTree` <span data-ttu-id="ea15e-370">Proporciona métodos para manipular los componentes y elementos, incluida la creación de componentes manualmente en C# código.</span><span class="sxs-lookup"><span data-stu-id="ea15e-370">provides methods for manipulating components and elements, including building components manually in C# code.</span></span>

> [!NOTE]
> <span data-ttu-id="ea15e-371">El uso de `RenderTreeBuilder` crear componentes es un escenario avanzado.</span><span class="sxs-lookup"><span data-stu-id="ea15e-371">Use of `RenderTreeBuilder` to create components is an advanced scenario.</span></span> <span data-ttu-id="ea15e-372">Un componente tiene un formato incorrecto (por ejemplo, una etiqueta de marcado sin cerrar) puede provocar un comportamiento indefinido.</span><span class="sxs-lookup"><span data-stu-id="ea15e-372">A malformed component (for example, an unclosed markup tag) can result in undefined behavior.</span></span>

<span data-ttu-id="ea15e-373">Tenga en cuenta los siguientes componentes de mascota detalles (*PetDetails.razor* en componentes de Razor; *PetDetails.cshtml* en Blazor), que pueden crearse manualmente en otro componente:</span><span class="sxs-lookup"><span data-stu-id="ea15e-373">Consider the following Pet Details component (*PetDetails.razor* in Razor Components; *PetDetails.cshtml* in Blazor), which can be manually built into another component:</span></span>

```cshtml
<h2>Pet Details Component</h2>

<p>@PetDetailsQuote<p>

@functions
{
    [Parameter]
    string PetDetailsQuote { get; set; }
}
```

<span data-ttu-id="ea15e-374">En el ejemplo siguiente, el bucle en el `CreateComponent` método genera tres componentes de los detalles de mascota.</span><span class="sxs-lookup"><span data-stu-id="ea15e-374">In the following example, the loop in the `CreateComponent` method generates three Pet Details components.</span></span> <span data-ttu-id="ea15e-375">Al llamar a `RenderTreeBuilder` métodos para crear los componentes (`OpenComponent` y `AddAttribute`), números de secuencia son números de línea de código fuente.</span><span class="sxs-lookup"><span data-stu-id="ea15e-375">When calling `RenderTreeBuilder` methods to create the components (`OpenComponent` and `AddAttribute`), sequence numbers are source code line numbers.</span></span> <span data-ttu-id="ea15e-376">Los componentes de Razor algoritmo de diferencias se basa en los números de secuencia correspondiente a diferentes líneas de código, no se diferencia llamar a las invocaciones.</span><span class="sxs-lookup"><span data-stu-id="ea15e-376">The Razor Components difference algorithm relies on the sequence numbers corresponding to distinct lines of code, not distinct call invocations.</span></span> <span data-ttu-id="ea15e-377">Al crear un componente con `RenderTreeBuilder` codificar los métodos, los argumentos para los números de secuencia.</span><span class="sxs-lookup"><span data-stu-id="ea15e-377">When creating a component with `RenderTreeBuilder` methods, hardcode the arguments for sequence numbers.</span></span> **<span data-ttu-id="ea15e-378">Uso de un cálculo o el contador para generar el número de secuencia puede provocar un rendimiento deficiente.</span><span class="sxs-lookup"><span data-stu-id="ea15e-378">Using a calculation or counter to generate the sequence number can lead to poor performance.</span></span>**

<span data-ttu-id="ea15e-379">Compila el componente (*BuiltContent.razor* en componentes de Razor; *BuiltContent.cshtml* en Blazor):</span><span class="sxs-lookup"><span data-stu-id="ea15e-379">Built component (*BuiltContent.razor* in Razor Components; *BuiltContent.cshtml* in Blazor):</span></span>

```cshtml
@page "/BuiltContent"

<h1>Build a component</h1>

@CustomRender

<button type="button" onclick="@RenderComponent">
    Create three Pet Details components
</button>

@functions {
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
