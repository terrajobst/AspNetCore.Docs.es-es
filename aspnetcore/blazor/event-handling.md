---
title: Control de eventos de ASP.NET Core Blazor
author: guardrex
description: Obtenga información sobre los escenarios de control de eventos de Blazor, incluidos los tipos de argumento de evento, las devoluciones de llamada de eventos y la administración de eventos predeterminados del explorador
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2020
no-loc:
- Blazor
- SignalR
uid: blazor/event-handling
ms.openlocfilehash: 25844ef39aee849072d16f3d73eda0a1c20ee788
ms.sourcegitcommit: 6645435fc8f5092fc7e923742e85592b56e37ada
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2020
ms.locfileid: "77453167"
---
# <a name="aspnet-core-blazor-event-handling"></a><span data-ttu-id="f52bf-103">Control de eventos de ASP.NET Core increíble</span><span class="sxs-lookup"><span data-stu-id="f52bf-103">ASP.NET Core Blazor event handling</span></span>

<span data-ttu-id="f52bf-104">Por [Luke Latham](https://github.com/guardrex) y [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="f52bf-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="f52bf-105">Los componentes de Razor proporcionan características de control de eventos.</span><span class="sxs-lookup"><span data-stu-id="f52bf-105">Razor components provide event handling features.</span></span> <span data-ttu-id="f52bf-106">Para un atributo de elemento HTML denominado `on{EVENT}` (por ejemplo, `onclick` y `onsubmit`) con un valor de tipo delegado, los componentes de Razor tratan el valor del atributo como un controlador de eventos.</span><span class="sxs-lookup"><span data-stu-id="f52bf-106">For an HTML element attribute named `on{EVENT}` (for example, `onclick` and `onsubmit`) with a delegate-typed value, Razor components treats the attribute's value as an event handler.</span></span> <span data-ttu-id="f52bf-107">Siempre se da formato al nombre del atributo [`@on{EVENT}`](xref:mvc/views/razor#onevent).</span><span class="sxs-lookup"><span data-stu-id="f52bf-107">The attribute's name is always formatted [`@on{EVENT}`](xref:mvc/views/razor#onevent).</span></span>

<span data-ttu-id="f52bf-108">El código siguiente llama al método `UpdateHeading` cuando el botón está seleccionado en la interfaz de usuario:</span><span class="sxs-lookup"><span data-stu-id="f52bf-108">The following code calls the `UpdateHeading` method when the button is selected in the UI:</span></span>

```razor
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

<span data-ttu-id="f52bf-109">El código siguiente llama al método `CheckChanged` cuando se cambia la casilla en la interfaz de usuario:</span><span class="sxs-lookup"><span data-stu-id="f52bf-109">The following code calls the `CheckChanged` method when the check box is changed in the UI:</span></span>

```razor
<input type="checkbox" class="form-check-input" @onchange="CheckChanged" />

@code {
    private void CheckChanged()
    {
        ...
    }
}
```

<span data-ttu-id="f52bf-110">Los controladores de eventos también pueden ser asincrónicos y devolver un <xref:System.Threading.Tasks.Task>.</span><span class="sxs-lookup"><span data-stu-id="f52bf-110">Event handlers can also be asynchronous and return a <xref:System.Threading.Tasks.Task>.</span></span> <span data-ttu-id="f52bf-111">No es necesario llamar a [StateHasChanged](xref:blazor/lifecycle#state-changes)manualmente.</span><span class="sxs-lookup"><span data-stu-id="f52bf-111">There's no need to manually call [StateHasChanged](xref:blazor/lifecycle#state-changes).</span></span> <span data-ttu-id="f52bf-112">Las excepciones se registran cuando se producen.</span><span class="sxs-lookup"><span data-stu-id="f52bf-112">Exceptions are logged when they occur.</span></span>

<span data-ttu-id="f52bf-113">En el ejemplo siguiente, se llama a `UpdateHeading` de forma asincrónica cuando se selecciona el botón:</span><span class="sxs-lookup"><span data-stu-id="f52bf-113">In the following example, `UpdateHeading` is called asynchronously when the button is selected:</span></span>

```razor
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

## <a name="event-argument-types"></a><span data-ttu-id="f52bf-114">Tipos de argumentos de evento</span><span class="sxs-lookup"><span data-stu-id="f52bf-114">Event argument types</span></span>

<span data-ttu-id="f52bf-115">En algunos eventos, se permiten los tipos de argumento de evento.</span><span class="sxs-lookup"><span data-stu-id="f52bf-115">For some events, event argument types are permitted.</span></span> <span data-ttu-id="f52bf-116">Solo es necesario especificar un tipo de evento en la llamada al método si se usa el tipo de evento en el método.</span><span class="sxs-lookup"><span data-stu-id="f52bf-116">Specifying an event type in the method call is only necessary if the event type is used in the method.</span></span>

<span data-ttu-id="f52bf-117">Los `EventArgs` admitidos se muestran en la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="f52bf-117">Supported `EventArgs` are shown in the following table.</span></span>

| <span data-ttu-id="f52bf-118">Evento</span><span class="sxs-lookup"><span data-stu-id="f52bf-118">Event</span></span>            | <span data-ttu-id="f52bf-119">Clase</span><span class="sxs-lookup"><span data-stu-id="f52bf-119">Class</span></span>                | <span data-ttu-id="f52bf-120">Eventos y notas de DOM</span><span class="sxs-lookup"><span data-stu-id="f52bf-120">DOM events and notes</span></span> |
| ---------------- | -------------------- | -------------------- |
| <span data-ttu-id="f52bf-121">Portapapeles</span><span class="sxs-lookup"><span data-stu-id="f52bf-121">Clipboard</span></span>        | `ClipboardEventArgs` | <span data-ttu-id="f52bf-122">`oncut`, `oncopy`, `onpaste`</span><span class="sxs-lookup"><span data-stu-id="f52bf-122">`oncut`, `oncopy`, `onpaste`</span></span> |
| <span data-ttu-id="f52bf-123">Arrastre</span><span class="sxs-lookup"><span data-stu-id="f52bf-123">Drag</span></span>             | `DragEventArgs`      | <span data-ttu-id="f52bf-124">`ondrag`, `ondragstart`, `ondragenter`, `ondragleave`, `ondragover`, `ondrop`, `ondragend`</span><span class="sxs-lookup"><span data-stu-id="f52bf-124">`ondrag`, `ondragstart`, `ondragenter`, `ondragleave`, `ondragover`, `ondrop`, `ondragend`</span></span><br><br><span data-ttu-id="f52bf-125">`DataTransfer` y `DataTransferItem` mantener los datos de los elementos arrastrados.</span><span class="sxs-lookup"><span data-stu-id="f52bf-125">`DataTransfer` and `DataTransferItem` hold dragged item data.</span></span> |
| <span data-ttu-id="f52bf-126">Error</span><span class="sxs-lookup"><span data-stu-id="f52bf-126">Error</span></span>            | `ErrorEventArgs`     | `onerror` |
| <span data-ttu-id="f52bf-127">Evento</span><span class="sxs-lookup"><span data-stu-id="f52bf-127">Event</span></span>            | `EventArgs`          | <span data-ttu-id="f52bf-128">*General*</span><span class="sxs-lookup"><span data-stu-id="f52bf-128">*General*</span></span><br><span data-ttu-id="f52bf-129">`onactivate`, `onbeforeactivate`, `onbeforedeactivate`, `ondeactivate`, `onended`, `onfullscreenchange`, `onfullscreenerror`, `onloadeddata`, `onloadedmetadata`, `onpointerlockchange`, `onpointerlockerror`, `onreadystatechange`, `onscroll`</span><span class="sxs-lookup"><span data-stu-id="f52bf-129">`onactivate`, `onbeforeactivate`, `onbeforedeactivate`, `ondeactivate`, `onended`, `onfullscreenchange`, `onfullscreenerror`, `onloadeddata`, `onloadedmetadata`, `onpointerlockchange`, `onpointerlockerror`, `onreadystatechange`, `onscroll`</span></span><br><br><span data-ttu-id="f52bf-130">*Portapapeles*</span><span class="sxs-lookup"><span data-stu-id="f52bf-130">*Clipboard*</span></span><br><span data-ttu-id="f52bf-131">`onbeforecut`, `onbeforecopy`, `onbeforepaste`</span><span class="sxs-lookup"><span data-stu-id="f52bf-131">`onbeforecut`, `onbeforecopy`, `onbeforepaste`</span></span><br><br><span data-ttu-id="f52bf-132">*Entrada*</span><span class="sxs-lookup"><span data-stu-id="f52bf-132">*Input*</span></span><br><span data-ttu-id="f52bf-133">`oninvalid`, `onreset`, `onselect`, `onselectionchange`, `onselectstart`, `onsubmit`</span><span class="sxs-lookup"><span data-stu-id="f52bf-133">`oninvalid`, `onreset`, `onselect`, `onselectionchange`, `onselectstart`, `onsubmit`</span></span><br><br><span data-ttu-id="f52bf-134">*Elementos multimedia*</span><span class="sxs-lookup"><span data-stu-id="f52bf-134">*Media*</span></span><br><span data-ttu-id="f52bf-135">`oncanplay`, `oncanplaythrough`, `oncuechange`, `ondurationchange`, `onemptied`, `onpause`, `onplay`, `onplaying`, `onratechange`, `onseeked`, `onseeking`, `onstalled`, `onstop`, `onsuspend`, `ontimeupdate`, `onvolumechange`, `onwaiting`</span><span class="sxs-lookup"><span data-stu-id="f52bf-135">`oncanplay`, `oncanplaythrough`, `oncuechange`, `ondurationchange`, `onemptied`, `onpause`, `onplay`, `onplaying`, `onratechange`, `onseeked`, `onseeking`, `onstalled`, `onstop`, `onsuspend`, `ontimeupdate`, `onvolumechange`, `onwaiting`</span></span> |
| <span data-ttu-id="f52bf-136">Focus</span><span class="sxs-lookup"><span data-stu-id="f52bf-136">Focus</span></span>            | `FocusEventArgs`     | <span data-ttu-id="f52bf-137">`onfocus`, `onblur`, `onfocusin`, `onfocusout`</span><span class="sxs-lookup"><span data-stu-id="f52bf-137">`onfocus`, `onblur`, `onfocusin`, `onfocusout`</span></span><br><br><span data-ttu-id="f52bf-138">No incluye compatibilidad con `relatedTarget`.</span><span class="sxs-lookup"><span data-stu-id="f52bf-138">Doesn't include support for `relatedTarget`.</span></span> |
| <span data-ttu-id="f52bf-139">Entrada</span><span class="sxs-lookup"><span data-stu-id="f52bf-139">Input</span></span>            | `ChangeEventArgs`    | <span data-ttu-id="f52bf-140">`onchange`, `oninput`</span><span class="sxs-lookup"><span data-stu-id="f52bf-140">`onchange`, `oninput`</span></span> |
| <span data-ttu-id="f52bf-141">Teclado</span><span class="sxs-lookup"><span data-stu-id="f52bf-141">Keyboard</span></span>         | `KeyboardEventArgs`  | <span data-ttu-id="f52bf-142">`onkeydown`, `onkeypress`, `onkeyup`</span><span class="sxs-lookup"><span data-stu-id="f52bf-142">`onkeydown`, `onkeypress`, `onkeyup`</span></span> |
| <span data-ttu-id="f52bf-143">Mouse</span><span class="sxs-lookup"><span data-stu-id="f52bf-143">Mouse</span></span>            | `MouseEventArgs`     | <span data-ttu-id="f52bf-144">`onclick`, `oncontextmenu`, `ondblclick`, `onmousedown`, `onmouseup`, `onmouseover`, `onmousemove`, `onmouseout`</span><span class="sxs-lookup"><span data-stu-id="f52bf-144">`onclick`, `oncontextmenu`, `ondblclick`, `onmousedown`, `onmouseup`, `onmouseover`, `onmousemove`, `onmouseout`</span></span> |
| <span data-ttu-id="f52bf-145">Puntero del mouse</span><span class="sxs-lookup"><span data-stu-id="f52bf-145">Mouse pointer</span></span>    | `PointerEventArgs`   | <span data-ttu-id="f52bf-146">`onpointerdown`, `onpointerup`, `onpointercancel`, `onpointermove`, `onpointerover`, `onpointerout`, `onpointerenter`, `onpointerleave`, `ongotpointercapture`, `onlostpointercapture`</span><span class="sxs-lookup"><span data-stu-id="f52bf-146">`onpointerdown`, `onpointerup`, `onpointercancel`, `onpointermove`, `onpointerover`, `onpointerout`, `onpointerenter`, `onpointerleave`, `ongotpointercapture`, `onlostpointercapture`</span></span> |
| <span data-ttu-id="f52bf-147">Rueda del mouse</span><span class="sxs-lookup"><span data-stu-id="f52bf-147">Mouse wheel</span></span>      | `WheelEventArgs`     | <span data-ttu-id="f52bf-148">`onwheel`, `onmousewheel`</span><span class="sxs-lookup"><span data-stu-id="f52bf-148">`onwheel`, `onmousewheel`</span></span> |
| <span data-ttu-id="f52bf-149">Progreso</span><span class="sxs-lookup"><span data-stu-id="f52bf-149">Progress</span></span>         | `ProgressEventArgs`  | <span data-ttu-id="f52bf-150">`onabort`, `onload`, `onloadend`, `onloadstart`, `onprogress`, `ontimeout`</span><span class="sxs-lookup"><span data-stu-id="f52bf-150">`onabort`, `onload`, `onloadend`, `onloadstart`, `onprogress`, `ontimeout`</span></span> |
| <span data-ttu-id="f52bf-151">Entrada táctil</span><span class="sxs-lookup"><span data-stu-id="f52bf-151">Touch</span></span>            | `TouchEventArgs`     | <span data-ttu-id="f52bf-152">`ontouchstart`, `ontouchend`, `ontouchmove`, `ontouchenter`, `ontouchleave`, `ontouchcancel`</span><span class="sxs-lookup"><span data-stu-id="f52bf-152">`ontouchstart`, `ontouchend`, `ontouchmove`, `ontouchenter`, `ontouchleave`, `ontouchcancel`</span></span><br><br><span data-ttu-id="f52bf-153">`TouchPoint` representa un único punto de contacto en un dispositivo con distinción de toque.</span><span class="sxs-lookup"><span data-stu-id="f52bf-153">`TouchPoint` represents a single contact point on a touch-sensitive device.</span></span> |

<span data-ttu-id="f52bf-154">Para obtener más información, consulte los siguientes recursos:</span><span class="sxs-lookup"><span data-stu-id="f52bf-154">For more information, see the following resources:</span></span>

* <span data-ttu-id="f52bf-155">[Clases EventArgs en el origen de referencia de ASP.net Core (rama dotnet/aspnetcore Release/3.1)](https://github.com/dotnet/aspnetcore/tree/release/3.1/src/Components/Web/src/Web).</span><span class="sxs-lookup"><span data-stu-id="f52bf-155">[EventArgs classes in the ASP.NET Core reference source (dotnet/aspnetcore release/3.1 branch)](https://github.com/dotnet/aspnetcore/tree/release/3.1/src/Components/Web/src/Web).</span></span>
* <span data-ttu-id="f52bf-156">[Documentos web de MDN: GlobalEventHandlers](https://developer.mozilla.org/docs/Web/API/GlobalEventHandlers) &ndash; incluye información sobre qué elementos HTML admite cada evento de Dom.</span><span class="sxs-lookup"><span data-stu-id="f52bf-156">[MDN web docs: GlobalEventHandlers](https://developer.mozilla.org/docs/Web/API/GlobalEventHandlers) &ndash; Includes information on which HTML elements support each DOM event.</span></span>

## <a name="lambda-expressions"></a><span data-ttu-id="f52bf-157">Expresiones lambda</span><span class="sxs-lookup"><span data-stu-id="f52bf-157">Lambda expressions</span></span>

<span data-ttu-id="f52bf-158">También se pueden usar [expresiones lambda](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions) :</span><span class="sxs-lookup"><span data-stu-id="f52bf-158">[Lambda expressions](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions) can also be used:</span></span>

```razor
<button @onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

<span data-ttu-id="f52bf-159">A menudo resulta cómodo cerrar los valores adicionales, como al recorrer en iteración un conjunto de elementos.</span><span class="sxs-lookup"><span data-stu-id="f52bf-159">It's often convenient to close over additional values, such as when iterating over a set of elements.</span></span> <span data-ttu-id="f52bf-160">En el ejemplo siguiente se crean tres botones, cada uno de los cuales llama a `UpdateHeading` pasar un argumento de evento (`MouseEventArgs`) y su número de botón (`buttonNumber`) cuando se selecciona en la interfaz de usuario:</span><span class="sxs-lookup"><span data-stu-id="f52bf-160">The following example creates three buttons, each of which calls `UpdateHeading` passing an event argument (`MouseEventArgs`) and its button number (`buttonNumber`) when selected in the UI:</span></span>

```razor
<h2>@_message</h2>

@for (var i = 1; i < 4; i++)
{
    var buttonNumber = i;

    <button class="btn btn-primary"
            @onclick="@(e => UpdateHeading(e, buttonNumber))">
        Button #@i
    </button>
}

@code {
    private string _message = "Select a button to learn its position.";

    private void UpdateHeading(MouseEventArgs e, int buttonNumber)
    {
        _message = $"You selected Button #{buttonNumber} at " +
            $"mouse position: {e.ClientX} X {e.ClientY}.";
    }
}
```

> [!NOTE]
> <span data-ttu-id="f52bf-161">**No** utilice la variable de bucle (`i`) en un bucle de `for` directamente en una expresión lambda.</span><span class="sxs-lookup"><span data-stu-id="f52bf-161">Do **not** use the loop variable (`i`) in a `for` loop directly in a lambda expression.</span></span> <span data-ttu-id="f52bf-162">De lo contrario, todas las expresiones lambda usan la misma variable, lo que hace que el valor de `i`sea el mismo en todas las expresiones lambda.</span><span class="sxs-lookup"><span data-stu-id="f52bf-162">Otherwise the same variable is used by all lambda expressions causing `i`'s value to be the same in all lambdas.</span></span> <span data-ttu-id="f52bf-163">Capture siempre su valor en una variable local (`buttonNumber` en el ejemplo anterior) y, a continuación, úsela.</span><span class="sxs-lookup"><span data-stu-id="f52bf-163">Always capture its value in a local variable (`buttonNumber` in the preceding example) and then use it.</span></span>

## <a name="eventcallback"></a><span data-ttu-id="f52bf-164">EventCallback</span><span class="sxs-lookup"><span data-stu-id="f52bf-164">EventCallback</span></span>

<span data-ttu-id="f52bf-165">Un escenario común con los componentes anidados es el deseo de ejecutar el método de un componente primario cuando se produce un evento de componente secundario&mdash;por ejemplo, cuando se produce un evento de `onclick` en el elemento secundario.</span><span class="sxs-lookup"><span data-stu-id="f52bf-165">A common scenario with nested components is the desire to run a parent component's method when a child component event occurs&mdash;for example, when an `onclick` event occurs in the child.</span></span> <span data-ttu-id="f52bf-166">Para exponer eventos entre componentes, use una `EventCallback`.</span><span class="sxs-lookup"><span data-stu-id="f52bf-166">To expose events across components, use an `EventCallback`.</span></span> <span data-ttu-id="f52bf-167">Un componente primario puede asignar un método de devolución de llamada al `EventCallback`de un componente secundario.</span><span class="sxs-lookup"><span data-stu-id="f52bf-167">A parent component can assign a callback method to a child component's `EventCallback`.</span></span>

<span data-ttu-id="f52bf-168">En el `ChildComponent` de la aplicación de ejemplo (*Components/ChildComponent. Razor*) se muestra cómo se configura el controlador de `onclick` de un botón para recibir un `EventCallback` delegado de la `ParentComponent`del ejemplo.</span><span class="sxs-lookup"><span data-stu-id="f52bf-168">The `ChildComponent` in the sample app (*Components/ChildComponent.razor*) demonstrates how a button's `onclick` handler is set up to receive an `EventCallback` delegate from the sample's `ParentComponent`.</span></span> <span data-ttu-id="f52bf-169">El `EventCallback` se escribe con `MouseEventArgs`, que es adecuado para un evento de `onclick` desde un dispositivo periférico:</span><span class="sxs-lookup"><span data-stu-id="f52bf-169">The `EventCallback` is typed with `MouseEventArgs`, which is appropriate for an `onclick` event from a peripheral device:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=5-7,17-18)]

<span data-ttu-id="f52bf-170">El `ParentComponent` establece el `EventCallback<T>` (`OnClickCallback`) del elemento secundario en su método `ShowMessage`.</span><span class="sxs-lookup"><span data-stu-id="f52bf-170">The `ParentComponent` sets the child's `EventCallback<T>` (`OnClickCallback`) to its `ShowMessage` method.</span></span>

<span data-ttu-id="f52bf-171">*Pages/ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="f52bf-171">*Pages/ParentComponent.razor*:</span></span>

```razor
@page "/ParentComponent"

<h1>Parent-child example</h1>

<ChildComponent Title="Panel Title from Parent"
                OnClickCallback="@ShowMessage">
    Content of the child component is supplied
    by the parent component.
</ChildComponent>

<p><b>@_messageText</b></p>

@code {
    private string _messageText;

    private void ShowMessage(MouseEventArgs e)
    {
        _messageText = $"Blaze a new trail with Blazor! ({e.ScreenX}, {e.ScreenY})";
    }
}
```

<span data-ttu-id="f52bf-172">Cuando el botón está seleccionado en el `ChildComponent`:</span><span class="sxs-lookup"><span data-stu-id="f52bf-172">When the button is selected in the `ChildComponent`:</span></span>

* <span data-ttu-id="f52bf-173">Se llama al método `ShowMessage` del `ParentComponent`.</span><span class="sxs-lookup"><span data-stu-id="f52bf-173">The `ParentComponent`'s `ShowMessage` method is called.</span></span> <span data-ttu-id="f52bf-174">`_messageText` se actualiza y se muestra en el `ParentComponent`.</span><span class="sxs-lookup"><span data-stu-id="f52bf-174">`_messageText` is updated and displayed in the `ParentComponent`.</span></span>
* <span data-ttu-id="f52bf-175">No se requiere una llamada a [StateHasChanged](xref:blazor/lifecycle#state-changes) en el método de la devolución de llamada (`ShowMessage`).</span><span class="sxs-lookup"><span data-stu-id="f52bf-175">A call to [StateHasChanged](xref:blazor/lifecycle#state-changes) isn't required in the callback's method (`ShowMessage`).</span></span> <span data-ttu-id="f52bf-176">se llama a `StateHasChanged` automáticamente para rerepresentar el `ParentComponent`, del mismo modo que los eventos secundarios desencadenan la rerepresentación de componentes en los controladores de eventos que se ejecutan dentro del elemento secundario.</span><span class="sxs-lookup"><span data-stu-id="f52bf-176">`StateHasChanged` is called automatically to rerender the `ParentComponent`, just as child events trigger component rerendering in event handlers that execute within the child.</span></span>

<span data-ttu-id="f52bf-177">`EventCallback` y `EventCallback<T>` permiten los delegados asincrónicos.</span><span class="sxs-lookup"><span data-stu-id="f52bf-177">`EventCallback` and `EventCallback<T>` permit asynchronous delegates.</span></span> <span data-ttu-id="f52bf-178">`EventCallback<T>` tiene un tipo seguro y requiere un tipo de argumento específico.</span><span class="sxs-lookup"><span data-stu-id="f52bf-178">`EventCallback<T>` is strongly typed and requires a specific argument type.</span></span> <span data-ttu-id="f52bf-179">`EventCallback` tiene un tipo débil y permite cualquier tipo de argumento.</span><span class="sxs-lookup"><span data-stu-id="f52bf-179">`EventCallback` is weakly typed and allows any argument type.</span></span>

```razor
<ChildComponent 
    OnClickCallback="@(async () => { await Task.Yield(); _messageText = "Blaze It!"; })" />
```

<span data-ttu-id="f52bf-180">Invocar un `EventCallback` o `EventCallback<T>` con `InvokeAsync` y esperar el <xref:System.Threading.Tasks.Task>:</span><span class="sxs-lookup"><span data-stu-id="f52bf-180">Invoke an `EventCallback` or `EventCallback<T>` with `InvokeAsync` and await the <xref:System.Threading.Tasks.Task>:</span></span>

```csharp
await callback.InvokeAsync(arg);
```

<span data-ttu-id="f52bf-181">Use `EventCallback` y `EventCallback<T>` para el control de eventos y los parámetros de componente de enlace.</span><span class="sxs-lookup"><span data-stu-id="f52bf-181">Use `EventCallback` and `EventCallback<T>` for event handling and binding component parameters.</span></span>

<span data-ttu-id="f52bf-182">Prefiera el `EventCallback<T>` fuertemente tipado en `EventCallback`.</span><span class="sxs-lookup"><span data-stu-id="f52bf-182">Prefer the strongly typed `EventCallback<T>` over `EventCallback`.</span></span> <span data-ttu-id="f52bf-183">`EventCallback<T>` proporciona mejores comentarios de errores a los usuarios del componente.</span><span class="sxs-lookup"><span data-stu-id="f52bf-183">`EventCallback<T>` provides better error feedback to users of the component.</span></span> <span data-ttu-id="f52bf-184">Al igual que otros controladores de eventos de la interfaz de usuario, la especificación del parámetro Event es opcional.</span><span class="sxs-lookup"><span data-stu-id="f52bf-184">Similar to other UI event handlers, specifying the event parameter is optional.</span></span> <span data-ttu-id="f52bf-185">Use `EventCallback` cuando no haya ningún valor pasado a la devolución de llamada.</span><span class="sxs-lookup"><span data-stu-id="f52bf-185">Use `EventCallback` when there's no value passed to the callback.</span></span>

## <a name="prevent-default-actions"></a><span data-ttu-id="f52bf-186">Impedir acciones predeterminadas</span><span class="sxs-lookup"><span data-stu-id="f52bf-186">Prevent default actions</span></span>

<span data-ttu-id="f52bf-187">Use el atributo de directiva de [`@on{EVENT}:preventDefault`](xref:mvc/views/razor#oneventpreventdefault) para evitar la acción predeterminada de un evento.</span><span class="sxs-lookup"><span data-stu-id="f52bf-187">Use the [`@on{EVENT}:preventDefault`](xref:mvc/views/razor#oneventpreventdefault) directive attribute to prevent the default action for an event.</span></span>

<span data-ttu-id="f52bf-188">Cuando se selecciona una clave en un dispositivo de entrada y el foco del elemento está en un cuadro de texto, un explorador muestra normalmente el carácter de la tecla en el cuadro de texto.</span><span class="sxs-lookup"><span data-stu-id="f52bf-188">When a key is selected on an input device and the element focus is on a text box, a browser normally displays the key's character in the text box.</span></span> <span data-ttu-id="f52bf-189">En el ejemplo siguiente, el comportamiento predeterminado se evita especificando el atributo de directiva de `@onkeypress:preventDefault`.</span><span class="sxs-lookup"><span data-stu-id="f52bf-189">In the following example, the default behavior is prevented by specifying the `@onkeypress:preventDefault` directive attribute.</span></span> <span data-ttu-id="f52bf-190">El contador se incrementa y la clave **+** no se captura en el valor del elemento `<input>`:</span><span class="sxs-lookup"><span data-stu-id="f52bf-190">The counter increments, and the **+** key isn't captured into the `<input>` element's value:</span></span>

```razor
<input value="@_count" @onkeypress="KeyHandler" @onkeypress:preventDefault />

@code {
    private int _count = 0;

    private void KeyHandler(KeyboardEventArgs e)
    {
        if (e.Key == "+")
        {
            _count++;
        }
    }
}
```

<span data-ttu-id="f52bf-191">Especificar el `@on{EVENT}:preventDefault` atributo sin un valor es equivalente a `@on{EVENT}:preventDefault="true"`.</span><span class="sxs-lookup"><span data-stu-id="f52bf-191">Specifying the `@on{EVENT}:preventDefault` attribute without a value is equivalent to `@on{EVENT}:preventDefault="true"`.</span></span>

<span data-ttu-id="f52bf-192">El valor del atributo también puede ser una expresión.</span><span class="sxs-lookup"><span data-stu-id="f52bf-192">The value of the attribute can also be an expression.</span></span> <span data-ttu-id="f52bf-193">En el ejemplo siguiente, `_shouldPreventDefault` es un campo `bool` establecido en `true` o `false`:</span><span class="sxs-lookup"><span data-stu-id="f52bf-193">In the following example, `_shouldPreventDefault` is a `bool` field set to either `true` or `false`:</span></span>

```razor
<input @onkeypress:preventDefault="_shouldPreventDefault" />
```

<span data-ttu-id="f52bf-194">No es necesario un controlador de eventos para evitar la acción predeterminada.</span><span class="sxs-lookup"><span data-stu-id="f52bf-194">An event handler isn't required to prevent the default action.</span></span> <span data-ttu-id="f52bf-195">El controlador de eventos y evitar escenarios de acción predeterminados se pueden usar de forma independiente.</span><span class="sxs-lookup"><span data-stu-id="f52bf-195">The event handler and prevent default action scenarios can be used independently.</span></span>

## <a name="stop-event-propagation"></a><span data-ttu-id="f52bf-196">Detener propagación de eventos</span><span class="sxs-lookup"><span data-stu-id="f52bf-196">Stop event propagation</span></span>

<span data-ttu-id="f52bf-197">Use el atributo de directiva de [`@on{EVENT}:stopPropagation`](xref:mvc/views/razor#oneventstoppropagation) para detener la propagación de eventos.</span><span class="sxs-lookup"><span data-stu-id="f52bf-197">Use the [`@on{EVENT}:stopPropagation`](xref:mvc/views/razor#oneventstoppropagation) directive attribute to stop event propagation.</span></span>

<span data-ttu-id="f52bf-198">En el ejemplo siguiente, al activar la casilla se impide que los eventos de clic de la segunda `<div>` secundaria se propaguen al `<div>`primario:</span><span class="sxs-lookup"><span data-stu-id="f52bf-198">In the following example, selecting the check box prevents click events from the second child `<div>` from propagating to the parent `<div>`:</span></span>

```razor
<label>
    <input @bind="_stopPropagation" type="checkbox" />
    Stop Propagation
</label>

<div @onclick="OnSelectParentDiv">
    <h3>Parent div</h3>

    <div @onclick="OnSelectChildDiv">
        Child div that doesn't stop propagation when selected.
    </div>

    <div @onclick="OnSelectChildDiv" @onclick:stopPropagation="_stopPropagation">
        Child div that stops propagation when selected.
    </div>
</div>

@code {
    private bool _stopPropagation = false;

    private void OnSelectParentDiv() => 
        Console.WriteLine($"The parent div was selected. {DateTime.Now}");
    private void OnSelectChildDiv() => 
        Console.WriteLine($"A child div was selected. {DateTime.Now}");
}
```
