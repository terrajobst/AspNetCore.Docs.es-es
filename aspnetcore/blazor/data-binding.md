---
title: Enlace de datos de ASP.NET Core Blazor
author: guardrex
description: Obtenga información sobre los escenarios de enlace de datos para componentes y elementos DOM en aplicaciones de Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/24/2020
no-loc:
- Blazor
- SignalR
uid: blazor/data-binding
ms.openlocfilehash: 92377730b9d353a507ffd384710fb979affe7265
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78648227"
---
# <a name="aspnet-core-opno-locblazor-data-binding"></a><span data-ttu-id="5143e-103">Enlace de datos de ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="5143e-103">ASP.NET Core Blazor data binding</span></span>

<span data-ttu-id="5143e-104">Por [Luke Latham](https://github.com/guardrex) y [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="5143e-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="5143e-105">El enlace de datos a componentes y elementos DOM se logra mediante el atributo [`@bind`](xref:mvc/views/razor#bind).</span><span class="sxs-lookup"><span data-stu-id="5143e-105">Data binding to both components and DOM elements is accomplished with the [`@bind`](xref:mvc/views/razor#bind) attribute.</span></span> <span data-ttu-id="5143e-106">En el ejemplo siguiente se enlaza una propiedad `CurrentValue` al valor del cuadro de texto:</span><span class="sxs-lookup"><span data-stu-id="5143e-106">The following example binds a `CurrentValue` property to the text box's value:</span></span>

```razor
<input @bind="CurrentValue" />

@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="5143e-107">Cuando el cuadro de texto pierde el foco, se actualiza el valor de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="5143e-107">When the text box loses focus, the property's value is updated.</span></span>

<span data-ttu-id="5143e-108">El cuadro de texto se actualiza en la interfaz de usuario solo cuando se representa el componente, no en respuesta al cambio de valor de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="5143e-108">The text box is updated in the UI only when the component is rendered, not in response to changing the property's value.</span></span> <span data-ttu-id="5143e-109">Como los componentes se representan a sí mismos después de que se ejecute el código del controlador de eventos, las actualizaciones de propiedades *normalmente* se reflejan en la interfaz de usuario justo después de que se desencadene un controlador de eventos.</span><span class="sxs-lookup"><span data-stu-id="5143e-109">Since components render themselves after event handler code executes, property updates are *usually* reflected in the UI immediately after an event handler is triggered.</span></span>

<span data-ttu-id="5143e-110">Usar `@bind` con la propiedad `CurrentValue` (`<input @bind="CurrentValue" />`) es prácticamente igual que usar lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="5143e-110">Using `@bind` with the `CurrentValue` property (`<input @bind="CurrentValue" />`) is essentially equivalent to the following:</span></span>

```razor
<input value="@CurrentValue"
    @onchange="@((ChangeEventArgs __e) => CurrentValue = 
        __e.Value.ToString())" />
        
@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="5143e-111">Cuando se representa el componente, el valor (`value`) del elemento de entrada procede de la propiedad `CurrentValue`.</span><span class="sxs-lookup"><span data-stu-id="5143e-111">When the component is rendered, the `value` of the input element comes from the `CurrentValue` property.</span></span> <span data-ttu-id="5143e-112">Cuando el usuario escribe en el cuadro de texto y cambia el foco del elemento, se desencadena el evento `onchange` y la propiedad `CurrentValue` se establece en el valor modificado.</span><span class="sxs-lookup"><span data-stu-id="5143e-112">When the user types in the text box and changes element focus, the `onchange` event is fired and the `CurrentValue` property is set to the changed value.</span></span> <span data-ttu-id="5143e-113">En realidad, la generación de código es más compleja, porque `@bind` administra los casos en los que se realizan conversiones de tipos.</span><span class="sxs-lookup"><span data-stu-id="5143e-113">In reality, the code generation is more complex because `@bind` handles cases where type conversions are performed.</span></span> <span data-ttu-id="5143e-114">En principio, `@bind` asocia el valor actual de una expresión con un atributo `value` y controla los cambios mediante el controlador registrado.</span><span class="sxs-lookup"><span data-stu-id="5143e-114">In principle, `@bind` associates the current value of an expression with a `value` attribute and handles changes using the registered handler.</span></span>

<span data-ttu-id="5143e-115">También puede enlazar una propiedad o un campo con otros eventos incluyendo un atributo `@bind:event` con un parámetro `event`.</span><span class="sxs-lookup"><span data-stu-id="5143e-115">Bind a property or field on other events by also including an `@bind:event` attribute with an `event` parameter.</span></span> <span data-ttu-id="5143e-116">En el siguiente ejemplo se enlaza la propiedad `CurrentValue` en el evento `oninput`:</span><span class="sxs-lookup"><span data-stu-id="5143e-116">The following example binds the `CurrentValue` property on the `oninput` event:</span></span>

```razor
<input @bind="CurrentValue" @bind:event="oninput" />

@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="5143e-117">A diferencia de `onchange`, que se activa cuando el elemento pierde el foco, `oninput` se desencadena cuando cambia el valor del cuadro de texto.</span><span class="sxs-lookup"><span data-stu-id="5143e-117">Unlike `onchange`, which fires when the element loses focus, `oninput` fires when the value of the text box changes.</span></span>

<span data-ttu-id="5143e-118">Use `@bind-{ATTRIBUTE}` con la sintaxis `@bind-{ATTRIBUTE}:event` para enlazar atributos de elementos que no sean `value`.</span><span class="sxs-lookup"><span data-stu-id="5143e-118">Use `@bind-{ATTRIBUTE}` with `@bind-{ATTRIBUTE}:event` syntax to bind element attributes other than `value`.</span></span> <span data-ttu-id="5143e-119">En el ejemplo siguiente, el estilo del párrafo se actualiza cuando cambia el valor de `_paragraphStyle`:</span><span class="sxs-lookup"><span data-stu-id="5143e-119">In the following example, the paragraph's style is updated when the `_paragraphStyle` value changes:</span></span>

```razor
@page "/binding-example"

<p>
    <input type="text" @bind="_paragraphStyle" />
</p>

<p @bind-style="_paragraphStyle" @bind-style:event="onchange">
    Blazorify the app!
</p>

@code {
    private string _paragraphStyle = "color:red";
}
```

## <a name="unparsable-values"></a><span data-ttu-id="5143e-120">Valores no analizables</span><span class="sxs-lookup"><span data-stu-id="5143e-120">Unparsable values</span></span>

<span data-ttu-id="5143e-121">Cuando un usuario proporciona un valor no analizable a un elemento enlazado a datos, el valor no analizable se revierte automáticamente a su valor anterior al desencadenar el evento de enlace.</span><span class="sxs-lookup"><span data-stu-id="5143e-121">When a user provides an unparsable value to a databound element, the unparsable value is automatically reverted to its previous value when the bind event is triggered.</span></span>

<span data-ttu-id="5143e-122">Considere el caso siguiente:</span><span class="sxs-lookup"><span data-stu-id="5143e-122">Consider the following scenario:</span></span>

* <span data-ttu-id="5143e-123">Un elemento `<input>` se enlaza a un tipo `int` con un valor inicial de `123`:</span><span class="sxs-lookup"><span data-stu-id="5143e-123">An `<input>` element is bound to an `int` type with an initial value of `123`:</span></span>

  ```razor
  <input @bind="MyProperty" />

  @code {
      [Parameter]
      public int MyProperty { get; set; } = 123;
  }
  ```
* <span data-ttu-id="5143e-124">El usuario actualiza el valor del elemento a `123.45` en la página y cambia el foco del elemento.</span><span class="sxs-lookup"><span data-stu-id="5143e-124">The user updates the value of the element to `123.45` in the page and changes the element focus.</span></span>

<span data-ttu-id="5143e-125">En el escenario anterior, el valor del elemento se revierte a `123`.</span><span class="sxs-lookup"><span data-stu-id="5143e-125">In the preceding scenario, the element's value is reverted to `123`.</span></span> <span data-ttu-id="5143e-126">Cuando el valor `123.45` se rechaza en favor del valor original de `123`, el usuario entiende que no se ha aceptado su valor.</span><span class="sxs-lookup"><span data-stu-id="5143e-126">When the value `123.45` is rejected in favor of the original value of `123`, the user understands that their value wasn't accepted.</span></span>

<span data-ttu-id="5143e-127">De forma predeterminada, el enlace de datos se aplica al evento `onchange` del elemento (`@bind="{PROPERTY OR FIELD}"`).</span><span class="sxs-lookup"><span data-stu-id="5143e-127">By default, binding applies to the element's `onchange` event (`@bind="{PROPERTY OR FIELD}"`).</span></span> <span data-ttu-id="5143e-128">Use `@bind="{PROPERTY OR FIELD}" @bind:event={EVENT}` para desencadenar el enlace en un evento diferente.</span><span class="sxs-lookup"><span data-stu-id="5143e-128">Use `@bind="{PROPERTY OR FIELD}" @bind:event={EVENT}` to trigger binding on a different event.</span></span> <span data-ttu-id="5143e-129">En el caso del evento `oninput` (`@bind:event="oninput"`), la reversión se produce después de cualquier pulsación de tecla que introduzca un valor no analizable.</span><span class="sxs-lookup"><span data-stu-id="5143e-129">For the `oninput` event (`@bind:event="oninput"`), the reversion occurs after any keystroke that introduces an unparsable value.</span></span> <span data-ttu-id="5143e-130">Cuando el destino es el evento `oninput` con un tipo enlazado a `int`, se impide que el usuario escriba el carácter `.`.</span><span class="sxs-lookup"><span data-stu-id="5143e-130">When targeting the `oninput` event with an `int`-bound type, a user is prevented from typing a `.` character.</span></span> <span data-ttu-id="5143e-131">El carácter `.` se elimina de forma inmediata, por lo que el usuario recibe un comentario al instante informándole de que solo se permiten números enteros.</span><span class="sxs-lookup"><span data-stu-id="5143e-131">A `.` character is immediately removed, so the user receives immediate feedback that only whole numbers are permitted.</span></span> <span data-ttu-id="5143e-132">Hay escenarios en los que la reversión del valor del evento `oninput` no es ideal, por ejemplo, cuando se debe permitir que el usuario borre un valor `<input>` que no se puede analizar.</span><span class="sxs-lookup"><span data-stu-id="5143e-132">There are scenarios where reverting the value on the `oninput` event isn't ideal, such as when the user should be allowed to clear an unparsable `<input>` value.</span></span> <span data-ttu-id="5143e-133">De forma alternativa, puede hacer lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="5143e-133">Alternatives include:</span></span>

* <span data-ttu-id="5143e-134">No use el evento `oninput`.</span><span class="sxs-lookup"><span data-stu-id="5143e-134">Don't use the `oninput` event.</span></span> <span data-ttu-id="5143e-135">Use el evento `onchange` predeterminado (especifique solo `@bind="{PROPERTY OR FIELD}"`) para que no se revierta un valor no válido hasta que el elemento pierda el foco.</span><span class="sxs-lookup"><span data-stu-id="5143e-135">Use the default `onchange` event (only specify `@bind="{PROPERTY OR FIELD}"`), where an invalid value isn't reverted until the element loses focus.</span></span>
* <span data-ttu-id="5143e-136">Enlace a un tipo que acepte valores NULL, como `int?` o `string`, y proporcione una lógica personalizada para administrar las entradas no válidas.</span><span class="sxs-lookup"><span data-stu-id="5143e-136">Bind to a nullable type, such as `int?` or `string`, and provide custom logic to handle invalid entries.</span></span>
* <span data-ttu-id="5143e-137">Use un [componente de validación de formularios](xref:blazor/forms-validation), como `InputNumber` o `InputDate`.</span><span class="sxs-lookup"><span data-stu-id="5143e-137">Use a [form validation component](xref:blazor/forms-validation), such as `InputNumber` or `InputDate`.</span></span> <span data-ttu-id="5143e-138">Los componentes de validación de formularios tienen compatibilidad integrada para administrar entradas no válidas.</span><span class="sxs-lookup"><span data-stu-id="5143e-138">Form validation components have built-in support to manage invalid inputs.</span></span> <span data-ttu-id="5143e-139">Componentes de validación de formularios:</span><span class="sxs-lookup"><span data-stu-id="5143e-139">Form validation components:</span></span>
  * <span data-ttu-id="5143e-140">Permiten que el usuario proporcione entradas no válidas y reciba errores de validación en la clase `EditContext` asociada.</span><span class="sxs-lookup"><span data-stu-id="5143e-140">Permit the user to provide invalid input and receive validation errors on the associated `EditContext`.</span></span>
  * <span data-ttu-id="5143e-141">Muestran errores de validación en la interfaz de usuario sin impedir que el usuario escriba datos adicionales en el formulario web.</span><span class="sxs-lookup"><span data-stu-id="5143e-141">Display validation errors in the UI without interfering with the user entering additional webform data.</span></span>

## <a name="format-strings"></a><span data-ttu-id="5143e-142">Cadenas de formato</span><span class="sxs-lookup"><span data-stu-id="5143e-142">Format strings</span></span>

<span data-ttu-id="5143e-143">El enlace de datos funciona con cadenas de formato <xref:System.DateTime> mediante [`@bind:format`](xref:mvc/views/razor#bind).</span><span class="sxs-lookup"><span data-stu-id="5143e-143">Data binding works with <xref:System.DateTime> format strings using [`@bind:format`](xref:mvc/views/razor#bind).</span></span> <span data-ttu-id="5143e-144">Otras expresiones de formato, como los formatos de moneda o número, no están disponibles en este momento.</span><span class="sxs-lookup"><span data-stu-id="5143e-144">Other format expressions, such as currency or number formats, aren't available at this time.</span></span>

```razor
<input @bind="StartDate" @bind:format="yyyy-MM-dd" />

@code {
    [Parameter]
    public DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

<span data-ttu-id="5143e-145">En el código anterior, el tipo de campo (`type`) del elemento `<input>` tiene como valor predeterminado `text`.</span><span class="sxs-lookup"><span data-stu-id="5143e-145">In the preceding code, the `<input>` element's field type (`type`) defaults to `text`.</span></span> <span data-ttu-id="5143e-146">`@bind:format` se admite para enlazar los siguientes tipos de .NET:</span><span class="sxs-lookup"><span data-stu-id="5143e-146">`@bind:format` is supported for binding the following .NET types:</span></span>

* <xref:System.DateTime?displayProperty=fullName>
* <span data-ttu-id="5143e-147"><xref:System.DateTime?displayProperty=fullName>?</span><span class="sxs-lookup"><span data-stu-id="5143e-147"><xref:System.DateTime?displayProperty=fullName>?</span></span>
* <xref:System.DateTimeOffset?displayProperty=fullName>
* <span data-ttu-id="5143e-148"><xref:System.DateTimeOffset?displayProperty=fullName>?</span><span class="sxs-lookup"><span data-stu-id="5143e-148"><xref:System.DateTimeOffset?displayProperty=fullName>?</span></span>

<span data-ttu-id="5143e-149">El atributo `@bind:format` especifica el formato de fecha que se va a aplicar al valor (`value`) del elemento `<input>`.</span><span class="sxs-lookup"><span data-stu-id="5143e-149">The `@bind:format` attribute specifies the date format to apply to the `value` of the `<input>` element.</span></span> <span data-ttu-id="5143e-150">El formato también se usa para analizar el valor cuando se produce un evento `onchange`.</span><span class="sxs-lookup"><span data-stu-id="5143e-150">The format is also used to parse the value when an `onchange` event occurs.</span></span>

<span data-ttu-id="5143e-151">No se recomienda especificar un formato para el tipo de campo `date` porque Blazor tiene compatibilidad integrada para dar formato a las fechas.</span><span class="sxs-lookup"><span data-stu-id="5143e-151">Specifying a format for the `date` field type isn't recommended because Blazor has built-in support to format dates.</span></span> <span data-ttu-id="5143e-152">A pesar de la recomendación, use solo el formato de fecha `yyyy-MM-dd` para que el enlace funcione correctamente si se proporciona un formato con el tipo de campo `date`:</span><span class="sxs-lookup"><span data-stu-id="5143e-152">In spite of the recommendation, only use the `yyyy-MM-dd` date format for binding to work correctly if a format is supplied with the `date` field type:</span></span>

```razor
<input type="date" @bind="StartDate" @bind:format="yyyy-MM-dd">
```

## <a name="parent-to-child-binding-with-component-parameters"></a><span data-ttu-id="5143e-153">Enlace de componentes primarios a secundarios con parámetros de componente</span><span class="sxs-lookup"><span data-stu-id="5143e-153">Parent-to-child binding with component parameters</span></span>

<span data-ttu-id="5143e-154">El enlace reconoce los parámetros de componente, por lo que `@bind-{PROPERTY}` puede enlazar un valor de propiedad de un componente primario con un componente secundario.</span><span class="sxs-lookup"><span data-stu-id="5143e-154">Binding recognizes component parameters, where `@bind-{PROPERTY}` can bind a property value from a parent component down to a child component.</span></span> <span data-ttu-id="5143e-155">En la sección [Enlace de componentes secundarios a primarios con un enlace encadenado](#child-to-parent-binding-with-chained-bind) se describe como enlazar un elemento secundario con uno primario.</span><span class="sxs-lookup"><span data-stu-id="5143e-155">Binding from a child to a parent is covered in the [Child-to-parent binding with chained bind](#child-to-parent-binding-with-chained-bind) section.</span></span>

<span data-ttu-id="5143e-156">El siguiente componente secundario (`ChildComponent`) tiene un parámetro de componente `Year` y un evento `YearChanged`:</span><span class="sxs-lookup"><span data-stu-id="5143e-156">The following child component (`ChildComponent`) has a `Year` component parameter and `YearChanged` callback:</span></span>

```razor
<h2>Child Component</h2>

<p>Year: @Year</p>

@code {
    [Parameter]
    public int Year { get; set; }

    [Parameter]
    public EventCallback<int> YearChanged { get; set; }
}
```

<span data-ttu-id="5143e-157">`EventCallback<T>` se explica en <xref:blazor/event-handling#eventcallback>.</span><span class="sxs-lookup"><span data-stu-id="5143e-157">`EventCallback<T>` is explained in <xref:blazor/event-handling#eventcallback>.</span></span>

<span data-ttu-id="5143e-158">El siguiente componente primario usa:</span><span class="sxs-lookup"><span data-stu-id="5143e-158">The following parent component uses:</span></span>

* <span data-ttu-id="5143e-159">`ChildComponent` y enlaza el parámetro `ParentYear` desde el elemento primario con el parámetro `Year` en el componente secundario.</span><span class="sxs-lookup"><span data-stu-id="5143e-159">`ChildComponent` and binds the `ParentYear` parameter from the parent to the `Year` parameter on the child component.</span></span>
* <span data-ttu-id="5143e-160">El evento `onclick` se usa para desencadenar el método `ChangeTheYear`.</span><span class="sxs-lookup"><span data-stu-id="5143e-160">The `onclick` event is used to trigger the `ChangeTheYear` method.</span></span> <span data-ttu-id="5143e-161">Para obtener más información, vea <xref:blazor/event-handling>.</span><span class="sxs-lookup"><span data-stu-id="5143e-161">For more information, see <xref:blazor/event-handling>.</span></span>

```razor
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

<span data-ttu-id="5143e-162">Al cargar el componente primario, `ParentComponent`, se produce el siguiente marcado:</span><span class="sxs-lookup"><span data-stu-id="5143e-162">Loading the `ParentComponent` produces the following markup:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

<span data-ttu-id="5143e-163">Si el valor de la propiedad `ParentYear` se cambia seleccionando el botón del componente primario (`ParentComponent`), se actualiza la propiedad `Year` del componente secundario (`ChildComponent`).</span><span class="sxs-lookup"><span data-stu-id="5143e-163">If the value of the `ParentYear` property is changed by selecting the button in the `ParentComponent`, the `Year` property of the `ChildComponent` is updated.</span></span> <span data-ttu-id="5143e-164">El nuevo valor de `Year` se representa en la interfaz de usuario junto con el componente primario (`ParentComponent`):</span><span class="sxs-lookup"><span data-stu-id="5143e-164">The new value of `Year` is rendered in the UI when the `ParentComponent` is rerendered:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

<span data-ttu-id="5143e-165">El parámetro `Year` se puede enlazar porque tiene un evento `YearChanged` complementario que coincide con el tipo del parámetro `Year`.</span><span class="sxs-lookup"><span data-stu-id="5143e-165">The `Year` parameter is bindable because it has a companion `YearChanged` event that matches the type of the `Year` parameter.</span></span>

<span data-ttu-id="5143e-166">Por convención, `<ChildComponent @bind-Year="ParentYear" />` es equivalente a escribir:</span><span class="sxs-lookup"><span data-stu-id="5143e-166">By convention, `<ChildComponent @bind-Year="ParentYear" />` is essentially equivalent to writing:</span></span>

```razor
<ChildComponent @bind-Year="ParentYear" @bind-Year:event="YearChanged" />
```

<span data-ttu-id="5143e-167">En general, una propiedad se puede enlazar a un controlador de eventos correspondiente si se incluye un atributo `@bind-{PROPRETY}:event`.</span><span class="sxs-lookup"><span data-stu-id="5143e-167">In general, a property can be bound to a corresponding event handler by including an `@bind-{PROPRETY}:event` attribute.</span></span> <span data-ttu-id="5143e-168">Por ejemplo, la propiedad `MyProp` se puede enlazar a `MyEventHandler` mediante los dos atributos siguientes:</span><span class="sxs-lookup"><span data-stu-id="5143e-168">For example, the property `MyProp` can be bound to `MyEventHandler` using the following two attributes:</span></span>

```razor
<MyComponent @bind-MyProp="MyValue" @bind-MyProp:event="MyEventHandler" />
```

## <a name="child-to-parent-binding-with-chained-bind"></a><span data-ttu-id="5143e-169">Enlace de componentes secundarios a primarios con un enlace encadenado</span><span class="sxs-lookup"><span data-stu-id="5143e-169">Child-to-parent binding with chained bind</span></span>

<span data-ttu-id="5143e-170">Un escenario común es encadenar un parámetro enlazado a datos a un elemento de página en la salida del componente.</span><span class="sxs-lookup"><span data-stu-id="5143e-170">A common scenario is chaining a data-bound parameter to a page element in the component's output.</span></span> <span data-ttu-id="5143e-171">Este escenario se denomina *enlace encadenado* porque se producen varios niveles de enlace simultáneamente.</span><span class="sxs-lookup"><span data-stu-id="5143e-171">This scenario is called a *chained bind* because multiple levels of binding occur simultaneously.</span></span>

<span data-ttu-id="5143e-172">No se puede implementar un enlace encadenado con sintaxis `@bind` en el elemento de la página.</span><span class="sxs-lookup"><span data-stu-id="5143e-172">A chained bind can't be implemented with `@bind` syntax in the page's element.</span></span> <span data-ttu-id="5143e-173">El controlador de eventos y el valor se deben especificar por separado.</span><span class="sxs-lookup"><span data-stu-id="5143e-173">The event handler and value must be specified separately.</span></span> <span data-ttu-id="5143e-174">Sin embargo, un componente primario puede usar la sintaxis `@bind` con el parámetro del componente.</span><span class="sxs-lookup"><span data-stu-id="5143e-174">A parent component, however, can use `@bind` syntax with the component's parameter.</span></span>

<span data-ttu-id="5143e-175">El siguiente componente `PasswordField` (*PasswordField.razor*):</span><span class="sxs-lookup"><span data-stu-id="5143e-175">The following `PasswordField` component (*PasswordField.razor*):</span></span>

* <span data-ttu-id="5143e-176">Establece el valor del elemento `<input>` en una propiedad `Password`.</span><span class="sxs-lookup"><span data-stu-id="5143e-176">Sets an `<input>` element's value to a `Password` property.</span></span>
* <span data-ttu-id="5143e-177">Expone los cambios de la propiedad `Password` en un componente primario con [EventCallback](xref:blazor/event-handling#eventcallback).</span><span class="sxs-lookup"><span data-stu-id="5143e-177">Exposes changes of the `Password` property to a parent component with an [EventCallback](xref:blazor/event-handling#eventcallback).</span></span>
* <span data-ttu-id="5143e-178">Usa el evento `onclick` para desencadenar el método `ToggleShowPassword`.</span><span class="sxs-lookup"><span data-stu-id="5143e-178">Uses the `onclick` event is used to trigger the `ToggleShowPassword` method.</span></span> <span data-ttu-id="5143e-179">Para obtener más información, vea <xref:blazor/event-handling>.</span><span class="sxs-lookup"><span data-stu-id="5143e-179">For more information, see <xref:blazor/event-handling>.</span></span>

```razor
<h1>Child Component</h2>

Password: 

<input @oninput="OnPasswordChanged" 
       required 
       type="@(_showPassword ? "text" : "password")" 
       value="@Password" />

<button class="btn btn-primary" @onclick="ToggleShowPassword">
    Show password
</button>

@code {
    private bool _showPassword;

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
        _showPassword = !_showPassword;
    }
}
```

<span data-ttu-id="5143e-180">El componente `PasswordField` se usa en otro componente:</span><span class="sxs-lookup"><span data-stu-id="5143e-180">The `PasswordField` component is used in another component:</span></span>

```razor
@page "/ParentComponent"

<h1>Parent Component</h1>

<PasswordField @bind-Password="_password" />

@code {
    private string _password;
}
```

<span data-ttu-id="5143e-181">Para realizar comprobaciones o detectar errores en la contraseña en el ejemplo anterior:</span><span class="sxs-lookup"><span data-stu-id="5143e-181">To perform checks or trap errors on the password in the preceding example:</span></span>

* <span data-ttu-id="5143e-182">Cree un campo de respaldo para `Password` (`_password` en el siguiente código de ejemplo).</span><span class="sxs-lookup"><span data-stu-id="5143e-182">Create a backing field for `Password` (`_password` in the following example code).</span></span>
* <span data-ttu-id="5143e-183">Realice las comprobaciones o detecte los errores en el establecedor de `Password`.</span><span class="sxs-lookup"><span data-stu-id="5143e-183">Perform the checks or trap errors in the `Password` setter.</span></span>

<span data-ttu-id="5143e-184">El ejemplo siguiente informa de inmediato al usuario si se usa un espacio en el valor de la contraseña:</span><span class="sxs-lookup"><span data-stu-id="5143e-184">The following example provides immediate feedback to the user if a space is used in the password's value:</span></span>

```razor
@page "/ParentComponent"

<h1>Parent Component</h1>

Password: 

<input @oninput="OnPasswordChanged" 
       required 
       type="@(_showPassword ? "text" : "password")" 
       value="@Password" />

<button class="btn btn-primary" @onclick="ToggleShowPassword">
    Show password
</button>

<span class="text-danger">@_validationMessage</span>

@code {
    private bool _showPassword;
    private string _password;
    private string _validationMessage;

    [Parameter]
    public string Password
    {
        get { return _password ?? string.Empty; }
        set
        {
            if (_password != value)
            {
                if (value.Contains(' '))
                {
                    _validationMessage = "Spaces not allowed!";
                }
                else
                {
                    _password = value;
                    _validationMessage = string.Empty;
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
        _showPassword = !_showPassword;
    }
}
```

## <a name="radio-buttons"></a><span data-ttu-id="5143e-185">Botones de radio</span><span class="sxs-lookup"><span data-stu-id="5143e-185">Radio buttons</span></span>

<span data-ttu-id="5143e-186">Para obtener información sobre cómo enlazar a botones de radio en un formulario, vea <xref:blazor/forms-validation#work-with-radio-buttons>.</span><span class="sxs-lookup"><span data-stu-id="5143e-186">For information on binding to radio buttons in a form, see <xref:blazor/forms-validation#work-with-radio-buttons>.</span></span>
