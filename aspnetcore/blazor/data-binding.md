---
title: ASP.NET Core Blazor enlace de datos
author: guardrex
description: Obtenga información sobre los escenarios de enlace de datos para componentes y elementos DOM en Blazor aplicaciones.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2020
no-loc:
- Blazor
- SignalR
uid: blazor/data-binding
ms.openlocfilehash: c38e6095d4e93d3eead10fa8bb0356b2113bb220
ms.sourcegitcommit: 6645435fc8f5092fc7e923742e85592b56e37ada
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2020
ms.locfileid: "77453161"
---
# <a name="aspnet-core-opno-locblazor-data-binding"></a><span data-ttu-id="df327-103">ASP.NET Core Blazor enlace de datos</span><span class="sxs-lookup"><span data-stu-id="df327-103">ASP.NET Core Blazor data binding</span></span>

<span data-ttu-id="df327-104">Por [Luke Latham](https://github.com/guardrex) y [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="df327-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="df327-105">El enlace de datos tanto a componentes como a elementos DOM se realiza con el [`@bind`](xref:mvc/views/razor#bind) atributo.</span><span class="sxs-lookup"><span data-stu-id="df327-105">Data binding to both components and DOM elements is accomplished with the [`@bind`](xref:mvc/views/razor#bind) attribute.</span></span> <span data-ttu-id="df327-106">En el ejemplo siguiente se enlaza una propiedad `CurrentValue` al valor del cuadro de texto:</span><span class="sxs-lookup"><span data-stu-id="df327-106">The following example binds a `CurrentValue` property to the text box's value:</span></span>

```razor
<input @bind="CurrentValue" />

@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="df327-107">Cuando el cuadro de texto pierde el foco, se actualiza el valor de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="df327-107">When the text box loses focus, the property's value is updated.</span></span>

<span data-ttu-id="df327-108">El cuadro de texto se actualiza en la interfaz de usuario solo cuando se representa el componente, no en respuesta a cambiar el valor de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="df327-108">The text box is updated in the UI only when the component is rendered, not in response to changing the property's value.</span></span> <span data-ttu-id="df327-109">Puesto que los componentes se representan por sí solos después de que se ejecute el código del controlador de eventos, las actualizaciones de propiedades *normalmente* se reflejan en la interfaz de usuario inmediatamente después de desencadenarse un controlador de eventos.</span><span class="sxs-lookup"><span data-stu-id="df327-109">Since components render themselves after event handler code executes, property updates are *usually* reflected in the UI immediately after an event handler is triggered.</span></span>

<span data-ttu-id="df327-110">El uso de `@bind` con la propiedad `CurrentValue` (`<input @bind="CurrentValue" />`) es esencialmente equivalente a lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="df327-110">Using `@bind` with the `CurrentValue` property (`<input @bind="CurrentValue" />`) is essentially equivalent to the following:</span></span>

```razor
<input value="@CurrentValue"
    @onchange="@((ChangeEventArgs __e) => CurrentValue = 
        __e.Value.ToString())" />
        
@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="df327-111">Cuando se representa el componente, el `value` del elemento de entrada procede de la propiedad `CurrentValue`.</span><span class="sxs-lookup"><span data-stu-id="df327-111">When the component is rendered, the `value` of the input element comes from the `CurrentValue` property.</span></span> <span data-ttu-id="df327-112">Cuando el usuario escribe en el cuadro de texto y cambia el foco del elemento, se desencadena el evento `onchange` y la propiedad `CurrentValue` se establece en el valor modificado.</span><span class="sxs-lookup"><span data-stu-id="df327-112">When the user types in the text box and changes element focus, the `onchange` event is fired and the `CurrentValue` property is set to the changed value.</span></span> <span data-ttu-id="df327-113">En realidad, la generación de código es más compleja porque `@bind` administra los casos en los que se realizan las conversiones de tipos.</span><span class="sxs-lookup"><span data-stu-id="df327-113">In reality, the code generation is more complex because `@bind` handles cases where type conversions are performed.</span></span> <span data-ttu-id="df327-114">En principio, `@bind` asocia el valor actual de una expresión con un atributo `value` y controla los cambios mediante el controlador registrado.</span><span class="sxs-lookup"><span data-stu-id="df327-114">In principle, `@bind` associates the current value of an expression with a `value` attribute and handles changes using the registered handler.</span></span>

<span data-ttu-id="df327-115">Además de controlar los eventos de `onchange` con sintaxis de `@bind`, se puede enlazar una propiedad o un campo mediante otros eventos mediante la especificación de un atributo de [`@bind-value`](xref:mvc/views/razor#bind) con un parámetro de `event` ([`@bind-value:event`](xref:mvc/views/razor#bind)).</span><span class="sxs-lookup"><span data-stu-id="df327-115">In addition to handling `onchange` events with `@bind` syntax, a property or field can be bound using other events by specifying an [`@bind-value`](xref:mvc/views/razor#bind) attribute with an `event` parameter ([`@bind-value:event`](xref:mvc/views/razor#bind)).</span></span> <span data-ttu-id="df327-116">En el siguiente ejemplo se enlaza la propiedad `CurrentValue` del evento `oninput`:</span><span class="sxs-lookup"><span data-stu-id="df327-116">The following example binds the `CurrentValue` property for the `oninput` event:</span></span>

```razor
<input @bind-value="CurrentValue" @bind-value:event="oninput" />

@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="df327-117">A diferencia de `onchange`, que se activa cuando el elemento pierde el foco, `oninput` se desencadena cuando cambia el valor del cuadro de texto.</span><span class="sxs-lookup"><span data-stu-id="df327-117">Unlike `onchange`, which fires when the element loses focus, `oninput` fires when the value of the text box changes.</span></span>

<span data-ttu-id="df327-118">`@bind-value` en el ejemplo anterior enlaza:</span><span class="sxs-lookup"><span data-stu-id="df327-118">`@bind-value` in the preceding example binds:</span></span>

* <span data-ttu-id="df327-119">Expresión especificada (`CurrentValue`) en el atributo de `value` del elemento.</span><span class="sxs-lookup"><span data-stu-id="df327-119">The specified expression (`CurrentValue`) to the element's `value` attribute.</span></span>
* <span data-ttu-id="df327-120">Delegado de evento de cambio para el evento especificado por `@bind-value:event`.</span><span class="sxs-lookup"><span data-stu-id="df327-120">A change event delegate to the event specified by `@bind-value:event`.</span></span>

### <a name="unparsable-values"></a><span data-ttu-id="df327-121">Valores no analizables</span><span class="sxs-lookup"><span data-stu-id="df327-121">Unparsable values</span></span>

<span data-ttu-id="df327-122">Cuando un usuario proporciona un valor que no se pueda analizar a un elemento DataBound, el valor no analizable se revierte automáticamente a su valor anterior cuando se desencadena el evento de enlace.</span><span class="sxs-lookup"><span data-stu-id="df327-122">When a user provides an unparsable value to a databound element, the unparsable value is automatically reverted to its previous value when the bind event is triggered.</span></span>

<span data-ttu-id="df327-123">Considere el caso siguiente:</span><span class="sxs-lookup"><span data-stu-id="df327-123">Consider the following scenario:</span></span>

* <span data-ttu-id="df327-124">Un elemento `<input>` se enlaza a un tipo `int` con un valor inicial de `123`:</span><span class="sxs-lookup"><span data-stu-id="df327-124">An `<input>` element is bound to an `int` type with an initial value of `123`:</span></span>

  ```razor
  <input @bind="MyProperty" />

  @code {
      [Parameter]
      public int MyProperty { get; set; } = 123;
  }
  ```
* <span data-ttu-id="df327-125">El usuario actualiza el valor del elemento a `123.45` en la página y cambia el foco del elemento.</span><span class="sxs-lookup"><span data-stu-id="df327-125">The user updates the value of the element to `123.45` in the page and changes the element focus.</span></span>

<span data-ttu-id="df327-126">En el escenario anterior, el valor del elemento se revierte a `123`.</span><span class="sxs-lookup"><span data-stu-id="df327-126">In the preceding scenario, the element's value is reverted to `123`.</span></span> <span data-ttu-id="df327-127">Cuando el valor `123.45` se rechaza en favor del valor original de `123`, el usuario entiende que no se aceptó su valor.</span><span class="sxs-lookup"><span data-stu-id="df327-127">When the value `123.45` is rejected in favor of the original value of `123`, the user understands that their value wasn't accepted.</span></span>

<span data-ttu-id="df327-128">De forma predeterminada, el enlace se aplica al evento `onchange` del elemento (`@bind="{PROPERTY OR FIELD}"`).</span><span class="sxs-lookup"><span data-stu-id="df327-128">By default, binding applies to the element's `onchange` event (`@bind="{PROPERTY OR FIELD}"`).</span></span> <span data-ttu-id="df327-129">Use `@bind-value="{PROPERTY OR FIELD}" @bind-value:event={EVENT}` para establecer un evento diferente.</span><span class="sxs-lookup"><span data-stu-id="df327-129">Use `@bind-value="{PROPERTY OR FIELD}" @bind-value:event={EVENT}` to set a different event.</span></span> <span data-ttu-id="df327-130">En el caso del evento `oninput` (`@bind-value:event="oninput"`), la reversión se produce después de cualquier pulsación de tecla que introduzca un valor no analizable.</span><span class="sxs-lookup"><span data-stu-id="df327-130">For the `oninput` event (`@bind-value:event="oninput"`), the reversion occurs after any keystroke that introduces an unparsable value.</span></span> <span data-ttu-id="df327-131">Cuando el destino es el evento `oninput` con un tipo enlazado a `int`, se impide que un usuario escriba un carácter `.`.</span><span class="sxs-lookup"><span data-stu-id="df327-131">When targeting the `oninput` event with an `int`-bound type, a user is prevented from typing a `.` character.</span></span> <span data-ttu-id="df327-132">Se quita inmediatamente un carácter `.`, por lo que el usuario recibe comentarios inmediatos que solo se permiten números enteros.</span><span class="sxs-lookup"><span data-stu-id="df327-132">A `.` character is immediately removed, so the user receives immediate feedback that only whole numbers are permitted.</span></span> <span data-ttu-id="df327-133">Hay escenarios en los que la reversión del valor del evento `oninput` no es ideal, por ejemplo, cuando se debe permitir que el usuario borre un valor `<input>` que no se puede analizar.</span><span class="sxs-lookup"><span data-stu-id="df327-133">There are scenarios where reverting the value on the `oninput` event isn't ideal, such as when the user should be allowed to clear an unparsable `<input>` value.</span></span> <span data-ttu-id="df327-134">Las alternativas incluyen:</span><span class="sxs-lookup"><span data-stu-id="df327-134">Alternatives include:</span></span>

* <span data-ttu-id="df327-135">No utilice el evento `oninput`.</span><span class="sxs-lookup"><span data-stu-id="df327-135">Don't use the `oninput` event.</span></span> <span data-ttu-id="df327-136">Use el evento `onchange` predeterminado (`@bind="{PROPERTY OR FIELD}"`), donde no se revierte un valor no válido hasta que el elemento pierde el foco.</span><span class="sxs-lookup"><span data-stu-id="df327-136">Use the default `onchange` event (`@bind="{PROPERTY OR FIELD}"`), where an invalid value isn't reverted until the element loses focus.</span></span>
* <span data-ttu-id="df327-137">Enlazar a un tipo que acepta valores NULL, como `int?` o `string`, y proporcionar una lógica personalizada para controlar las entradas no válidas.</span><span class="sxs-lookup"><span data-stu-id="df327-137">Bind to a nullable type, such as `int?` or `string`, and provide custom logic to handle invalid entries.</span></span>
* <span data-ttu-id="df327-138">Use un [componente de validación de formulario](xref:blazor/forms-validation), como `InputNumber` o `InputDate`.</span><span class="sxs-lookup"><span data-stu-id="df327-138">Use a [form validation component](xref:blazor/forms-validation), such as `InputNumber` or `InputDate`.</span></span> <span data-ttu-id="df327-139">Los componentes de validación de formularios tienen compatibilidad integrada para administrar entradas no válidas.</span><span class="sxs-lookup"><span data-stu-id="df327-139">Form validation components have built-in support to manage invalid inputs.</span></span> <span data-ttu-id="df327-140">Componentes de validación de formularios:</span><span class="sxs-lookup"><span data-stu-id="df327-140">Form validation components:</span></span>
  * <span data-ttu-id="df327-141">Permite que el usuario proporcione entradas no válidas y reciba errores de validación en la `EditContext`asociada.</span><span class="sxs-lookup"><span data-stu-id="df327-141">Permit the user to provide invalid input and receive validation errors on the associated `EditContext`.</span></span>
  * <span data-ttu-id="df327-142">Mostrar errores de validación en la interfaz de usuario sin interferir con el usuario al escribir datos de WebForm adicionales.</span><span class="sxs-lookup"><span data-stu-id="df327-142">Display validation errors in the UI without interfering with the user entering additional webform data.</span></span>

### <a name="format-strings"></a><span data-ttu-id="df327-143">Cadenas de formato</span><span class="sxs-lookup"><span data-stu-id="df327-143">Format strings</span></span>

<span data-ttu-id="df327-144">El enlace de datos funciona con cadenas de formato <xref:System.DateTime> mediante [`@bind:format`](xref:mvc/views/razor#bind).</span><span class="sxs-lookup"><span data-stu-id="df327-144">Data binding works with <xref:System.DateTime> format strings using [`@bind:format`](xref:mvc/views/razor#bind).</span></span> <span data-ttu-id="df327-145">Otras expresiones de formato, como los formatos de moneda o número, no están disponibles en este momento.</span><span class="sxs-lookup"><span data-stu-id="df327-145">Other format expressions, such as currency or number formats, aren't available at this time.</span></span>

```razor
<input @bind="StartDate" @bind:format="yyyy-MM-dd" />

@code {
    [Parameter]
    public DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

<span data-ttu-id="df327-146">En el código anterior, el tipo de campo del elemento de `<input>` (`type`) tiene como valor predeterminado `text`.</span><span class="sxs-lookup"><span data-stu-id="df327-146">In the preceding code, the `<input>` element's field type (`type`) defaults to `text`.</span></span> <span data-ttu-id="df327-147">`@bind:format` es compatible con el enlace de los siguientes tipos de .NET:</span><span class="sxs-lookup"><span data-stu-id="df327-147">`@bind:format` is supported for binding the following .NET types:</span></span>

* <xref:System.DateTime?displayProperty=fullName>
* <span data-ttu-id="df327-148"><xref:System.DateTime?displayProperty=fullName>?</span><span class="sxs-lookup"><span data-stu-id="df327-148"><xref:System.DateTime?displayProperty=fullName>?</span></span>
* <xref:System.DateTimeOffset?displayProperty=fullName>
* <span data-ttu-id="df327-149"><xref:System.DateTimeOffset?displayProperty=fullName>?</span><span class="sxs-lookup"><span data-stu-id="df327-149"><xref:System.DateTimeOffset?displayProperty=fullName>?</span></span>

<span data-ttu-id="df327-150">El atributo `@bind:format` especifica el formato de fecha que se va a aplicar al `value` del elemento `<input>`.</span><span class="sxs-lookup"><span data-stu-id="df327-150">The `@bind:format` attribute specifies the date format to apply to the `value` of the `<input>` element.</span></span> <span data-ttu-id="df327-151">El formato también se usa para analizar el valor cuando se produce un evento `onchange`.</span><span class="sxs-lookup"><span data-stu-id="df327-151">The format is also used to parse the value when an `onchange` event occurs.</span></span>

<span data-ttu-id="df327-152">No se recomienda especificar un formato para el tipo de campo `date` porque Blazor tiene compatibilidad integrada para dar formato a las fechas.</span><span class="sxs-lookup"><span data-stu-id="df327-152">Specifying a format for the `date` field type isn't recommended because Blazor has built-in support to format dates.</span></span> <span data-ttu-id="df327-153">A pesar de la recomendación, use solo el formato de fecha `yyyy-MM-dd` para que el enlace funcione correctamente si se proporciona un formato con el tipo de campo `date`:</span><span class="sxs-lookup"><span data-stu-id="df327-153">In spite of the recommendation, only use the `yyyy-MM-dd` date format for binding to work correctly if a format is supplied with the `date` field type:</span></span>

```razor
<input type="date" @bind="StartDate" @bind:format="yyyy-MM-dd">
```

### <a name="parent-to-child-binding-with-component-parameters"></a><span data-ttu-id="df327-154">Enlace de primario a secundario con parámetros de componente</span><span class="sxs-lookup"><span data-stu-id="df327-154">Parent-to-child binding with component parameters</span></span>

<span data-ttu-id="df327-155">El enlace reconoce los parámetros de componente, donde `@bind-{property}` puede enlazar un valor de propiedad de un componente primario a un componente secundario.</span><span class="sxs-lookup"><span data-stu-id="df327-155">Binding recognizes component parameters, where `@bind-{property}` can bind a property value from a parent component down to a child component.</span></span> <span data-ttu-id="df327-156">El enlace de un elemento secundario a un elemento primario se trata en la sección [enlace de secundario a primario con enlace encadenado](#child-to-parent-binding-with-chained-bind) .</span><span class="sxs-lookup"><span data-stu-id="df327-156">Binding from a child to a parent is covered in the [Child-to-parent binding with chained bind](#child-to-parent-binding-with-chained-bind) section.</span></span>

<span data-ttu-id="df327-157">El siguiente componente secundario (`ChildComponent`) tiene un parámetro de componente `Year` y una devolución de llamada `YearChanged`:</span><span class="sxs-lookup"><span data-stu-id="df327-157">The following child component (`ChildComponent`) has a `Year` component parameter and `YearChanged` callback:</span></span>

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

<span data-ttu-id="df327-158">`EventCallback<T>` se explica en <xref:blazor/event-handling#eventcallback>.</span><span class="sxs-lookup"><span data-stu-id="df327-158">`EventCallback<T>` is explained in <xref:blazor/event-handling#eventcallback>.</span></span>

<span data-ttu-id="df327-159">El siguiente componente primario utiliza:</span><span class="sxs-lookup"><span data-stu-id="df327-159">The following parent component uses:</span></span>

* <span data-ttu-id="df327-160">`ChildComponent` y enlaza el parámetro `ParentYear` desde el elemento primario al parámetro `Year` en el componente secundario.</span><span class="sxs-lookup"><span data-stu-id="df327-160">`ChildComponent` and binds the `ParentYear` parameter from the parent to the `Year` parameter on the child component.</span></span>
* <span data-ttu-id="df327-161">El evento `onclick` se usa para desencadenar el método `ChangeTheYear`.</span><span class="sxs-lookup"><span data-stu-id="df327-161">The `onclick` event is used to trigger the `ChangeTheYear` method.</span></span> <span data-ttu-id="df327-162">Para más información, consulte <xref:blazor/event-handling>.</span><span class="sxs-lookup"><span data-stu-id="df327-162">For more information, see <xref:blazor/event-handling>.</span></span>

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

<span data-ttu-id="df327-163">Al cargar el `ParentComponent` se produce el marcado siguiente:</span><span class="sxs-lookup"><span data-stu-id="df327-163">Loading the `ParentComponent` produces the following markup:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

<span data-ttu-id="df327-164">Si el valor de la propiedad `ParentYear` se cambia seleccionando el botón en el `ParentComponent`, se actualiza la propiedad `Year` del `ChildComponent`.</span><span class="sxs-lookup"><span data-stu-id="df327-164">If the value of the `ParentYear` property is changed by selecting the button in the `ParentComponent`, the `Year` property of the `ChildComponent` is updated.</span></span> <span data-ttu-id="df327-165">El nuevo valor de `Year` se representa en la interfaz de usuario cuando se representa el `ParentComponent`:</span><span class="sxs-lookup"><span data-stu-id="df327-165">The new value of `Year` is rendered in the UI when the `ParentComponent` is rerendered:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

<span data-ttu-id="df327-166">El parámetro `Year` es enlazable porque tiene un evento Companion `YearChanged` que coincide con el tipo del parámetro `Year`.</span><span class="sxs-lookup"><span data-stu-id="df327-166">The `Year` parameter is bindable because it has a companion `YearChanged` event that matches the type of the `Year` parameter.</span></span>

<span data-ttu-id="df327-167">Por Convención, `<ChildComponent @bind-Year="ParentYear" />` es esencialmente equivalente a escribir:</span><span class="sxs-lookup"><span data-stu-id="df327-167">By convention, `<ChildComponent @bind-Year="ParentYear" />` is essentially equivalent to writing:</span></span>

```razor
<ChildComponent @bind-Year="ParentYear" @bind-Year:event="YearChanged" />
```

<span data-ttu-id="df327-168">En general, una propiedad se puede enlazar a un controlador de eventos correspondiente mediante `@bind-property:event` atributo.</span><span class="sxs-lookup"><span data-stu-id="df327-168">In general, a property can be bound to a corresponding event handler using `@bind-property:event` attribute.</span></span> <span data-ttu-id="df327-169">Por ejemplo, la propiedad `MyProp` se puede enlazar a `MyEventHandler` mediante los dos atributos siguientes:</span><span class="sxs-lookup"><span data-stu-id="df327-169">For example, the property `MyProp` can be bound to `MyEventHandler` using the following two attributes:</span></span>

```razor
<MyComponent @bind-MyProp="MyValue" @bind-MyProp:event="MyEventHandler" />
```

### <a name="child-to-parent-binding-with-chained-bind"></a><span data-ttu-id="df327-170">Enlace de secundario a primario con enlace encadenado</span><span class="sxs-lookup"><span data-stu-id="df327-170">Child-to-parent binding with chained bind</span></span>

<span data-ttu-id="df327-171">Un escenario común es encadenar un parámetro enlazado a datos a un elemento Page en la salida del componente.</span><span class="sxs-lookup"><span data-stu-id="df327-171">A common scenario is chaining a data-bound parameter to a page element in the component's output.</span></span> <span data-ttu-id="df327-172">Este escenario se denomina *enlace encadenado* porque se producen varios niveles de enlace simultáneamente.</span><span class="sxs-lookup"><span data-stu-id="df327-172">This scenario is called a *chained bind* because multiple levels of binding occur simultaneously.</span></span>

<span data-ttu-id="df327-173">No se puede implementar un enlace encadenado con `@bind` sintaxis en el elemento de la página.</span><span class="sxs-lookup"><span data-stu-id="df327-173">A chained bind can't be implemented with `@bind` syntax in the page's element.</span></span> <span data-ttu-id="df327-174">El controlador de eventos y el valor se deben especificar por separado.</span><span class="sxs-lookup"><span data-stu-id="df327-174">The event handler and value must be specified separately.</span></span> <span data-ttu-id="df327-175">Sin embargo, un componente primario puede usar la sintaxis de `@bind` con el parámetro del componente.</span><span class="sxs-lookup"><span data-stu-id="df327-175">A parent component, however, can use `@bind` syntax with the component's parameter.</span></span>

<span data-ttu-id="df327-176">El siguiente componente de `PasswordField` (*PasswordField. Razor*):</span><span class="sxs-lookup"><span data-stu-id="df327-176">The following `PasswordField` component (*PasswordField.razor*):</span></span>

* <span data-ttu-id="df327-177">Establece el valor de un elemento de `<input>` en una propiedad `Password`.</span><span class="sxs-lookup"><span data-stu-id="df327-177">Sets an `<input>` element's value to a `Password` property.</span></span>
* <span data-ttu-id="df327-178">Expone los cambios de la propiedad `Password` a un componente primario con un [EventCallback](xref:blazor/event-handling#eventcallback).</span><span class="sxs-lookup"><span data-stu-id="df327-178">Exposes changes of the `Password` property to a parent component with an [EventCallback](xref:blazor/event-handling#eventcallback).</span></span>
* <span data-ttu-id="df327-179">Usa el evento `onclick` se usa para desencadenar el método `ToggleShowPassword`.</span><span class="sxs-lookup"><span data-stu-id="df327-179">Uses the `onclick` event is used to trigger the `ToggleShowPassword` method.</span></span> <span data-ttu-id="df327-180">Para más información, consulte <xref:blazor/event-handling>.</span><span class="sxs-lookup"><span data-stu-id="df327-180">For more information, see <xref:blazor/event-handling>.</span></span>

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

<span data-ttu-id="df327-181">El componente de `PasswordField` se usa en otro componente:</span><span class="sxs-lookup"><span data-stu-id="df327-181">The `PasswordField` component is used in another component:</span></span>

```razor
@page "/ParentComponent"

<h1>Parent Component</h1>

<PasswordField @bind-Password="_password" />

@code {
    private string _password;
}
```

<span data-ttu-id="df327-182">Para realizar comprobaciones o errores de captura en la contraseña en el ejemplo anterior:</span><span class="sxs-lookup"><span data-stu-id="df327-182">To perform checks or trap errors on the password in the preceding example:</span></span>

* <span data-ttu-id="df327-183">Cree un campo de respaldo para `Password` (`_password` en el siguiente código de ejemplo).</span><span class="sxs-lookup"><span data-stu-id="df327-183">Create a backing field for `Password` (`_password` in the following example code).</span></span>
* <span data-ttu-id="df327-184">Realice las comprobaciones o errores de captura en el establecedor de `Password`.</span><span class="sxs-lookup"><span data-stu-id="df327-184">Perform the checks or trap errors in the `Password` setter.</span></span>

<span data-ttu-id="df327-185">En el ejemplo siguiente se proporcionan comentarios inmediatos al usuario si se usa un espacio en el valor de la contraseña:</span><span class="sxs-lookup"><span data-stu-id="df327-185">The following example provides immediate feedback to the user if a space is used in the password's value:</span></span>

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

### <a name="radio-buttons"></a><span data-ttu-id="df327-186">Botones de radio</span><span class="sxs-lookup"><span data-stu-id="df327-186">Radio buttons</span></span>

<span data-ttu-id="df327-187">Para obtener información sobre cómo enlazar a botones de radio en un formulario, vea <xref:blazor/forms-validation#work-with-radio-buttons>.</span><span class="sxs-lookup"><span data-stu-id="df327-187">For information on binding to radio buttons in a form, see <xref:blazor/forms-validation#work-with-radio-buttons>.</span></span>
