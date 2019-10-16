---
title: Interoperabilidad de JavaScript de ASP.NET Core increíblemente
author: guardrex
description: Obtenga información sobre cómo invocar funciones de JavaScript desde los métodos de .net y .net desde JavaScript en aplicaciones increíbles.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
uid: blazor/javascript-interop
ms.openlocfilehash: b4776a20c6da6c722d2c057d19863c570f530a21
ms.sourcegitcommit: 35a86ce48041caaf6396b1e88b0472578ba24483
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2019
ms.locfileid: "72391065"
---
# <a name="aspnet-core-blazor-javascript-interop"></a><span data-ttu-id="15fbb-103">Interoperabilidad de JavaScript de ASP.NET Core increíblemente</span><span class="sxs-lookup"><span data-stu-id="15fbb-103">ASP.NET Core Blazor JavaScript interop</span></span>

<span data-ttu-id="15fbb-104">Por [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27)y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="15fbb-104">By [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="15fbb-105">Una aplicación extraordinaria puede invocar funciones de JavaScript desde los métodos de .NET y .net desde el código JavaScript.</span><span class="sxs-lookup"><span data-stu-id="15fbb-105">A Blazor app can invoke JavaScript functions from .NET and .NET methods from JavaScript code.</span></span>

<span data-ttu-id="15fbb-106">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="15fbb-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="invoke-javascript-functions-from-net-methods"></a><span data-ttu-id="15fbb-107">Invocar funciones de JavaScript desde métodos de .NET</span><span class="sxs-lookup"><span data-stu-id="15fbb-107">Invoke JavaScript functions from .NET methods</span></span>

<span data-ttu-id="15fbb-108">Hay ocasiones en las que se requiere código .NET para llamar a una función de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="15fbb-108">There are times when .NET code is required to call a JavaScript function.</span></span> <span data-ttu-id="15fbb-109">Por ejemplo, una llamada de JavaScript puede exponer funciones o funcionalidades del explorador de una biblioteca de JavaScript a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="15fbb-109">For example, a JavaScript call can expose browser capabilities or functionality from a JavaScript library to the app.</span></span>

<span data-ttu-id="15fbb-110">Para llamar a JavaScript desde .NET, use la abstracción `IJSRuntime`.</span><span class="sxs-lookup"><span data-stu-id="15fbb-110">To call into JavaScript from .NET, use the `IJSRuntime` abstraction.</span></span> <span data-ttu-id="15fbb-111">El método `InvokeAsync<T>` toma un identificador de la función de JavaScript que desea invocar junto con cualquier número de argumentos serializables de JSON.</span><span class="sxs-lookup"><span data-stu-id="15fbb-111">The `InvokeAsync<T>` method takes an identifier for the JavaScript function that you wish to invoke along with any number of JSON-serializable arguments.</span></span> <span data-ttu-id="15fbb-112">El identificador de función es relativo al ámbito global (`window`).</span><span class="sxs-lookup"><span data-stu-id="15fbb-112">The function identifier is relative to the global scope (`window`).</span></span> <span data-ttu-id="15fbb-113">Si desea llamar a `window.someScope.someFunction`, el identificador es `someScope.someFunction`.</span><span class="sxs-lookup"><span data-stu-id="15fbb-113">If you wish to call `window.someScope.someFunction`, the identifier is `someScope.someFunction`.</span></span> <span data-ttu-id="15fbb-114">No es necesario registrar la función antes de que se le llame.</span><span class="sxs-lookup"><span data-stu-id="15fbb-114">There's no need to register the function before it's called.</span></span> <span data-ttu-id="15fbb-115">El tipo de valor devuelto `T` también debe ser serializable.</span><span class="sxs-lookup"><span data-stu-id="15fbb-115">The return type `T` must also be JSON serializable.</span></span>

<span data-ttu-id="15fbb-116">Para las aplicaciones de servidor increíbles:</span><span class="sxs-lookup"><span data-stu-id="15fbb-116">For Blazor Server apps:</span></span>

* <span data-ttu-id="15fbb-117">La aplicación de servidor increíblemente procesa varias solicitudes de usuario.</span><span class="sxs-lookup"><span data-stu-id="15fbb-117">Multiple user requests are processed by the Blazor Server app.</span></span> <span data-ttu-id="15fbb-118">No llame a `JSRuntime.Current` en un componente para invocar funciones de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="15fbb-118">Don't call `JSRuntime.Current` in a component to invoke JavaScript functions.</span></span>
* <span data-ttu-id="15fbb-119">Inserte la abstracción `IJSRuntime` y utilice el objeto insertado para emitir llamadas de interoperabilidad de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="15fbb-119">Inject the `IJSRuntime` abstraction and use the injected object to issue JavaScript interop calls.</span></span>
* <span data-ttu-id="15fbb-120">Aunque una aplicación increíblemente es una representación previa, no es posible llamar a JavaScript porque no se ha establecido una conexión con el explorador.</span><span class="sxs-lookup"><span data-stu-id="15fbb-120">While a Blazor app is prerendering, calling into JavaScript isn't possible because a connection with the browser hasn't been established.</span></span> <span data-ttu-id="15fbb-121">Para obtener más información, consulte la sección [detectar cuándo una aplicación increíblemente es una representación](#detect-when-a-blazor-app-is-prerendering) previa.</span><span class="sxs-lookup"><span data-stu-id="15fbb-121">For more information, see the [Detect when a Blazor app is prerendering](#detect-when-a-blazor-app-is-prerendering) section.</span></span>

<span data-ttu-id="15fbb-122">El ejemplo siguiente se basa en [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), un descodificador basado en JavaScript experimental.</span><span class="sxs-lookup"><span data-stu-id="15fbb-122">The following example is based on [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), an experimental JavaScript-based decoder.</span></span> <span data-ttu-id="15fbb-123">En el ejemplo se muestra cómo invocar una función de C# JavaScript desde un método.</span><span class="sxs-lookup"><span data-stu-id="15fbb-123">The example demonstrates how to invoke a JavaScript function from a C# method.</span></span> <span data-ttu-id="15fbb-124">La función de JavaScript acepta una matriz de bytes de un C# método, descodifica la matriz y devuelve el texto al componente que se va a mostrar.</span><span class="sxs-lookup"><span data-stu-id="15fbb-124">The JavaScript function accepts a byte array from a C# method, decodes the array, and returns the text to the component for display.</span></span>

<span data-ttu-id="15fbb-125">Dentro del elemento `<head>` de *wwwroot/index.html* (webassembler) o *pages/_Host. cshtml* (servidor increíblemente), proporcione una función que use `TextDecoder` para descodificar una matriz pasada:</span><span class="sxs-lookup"><span data-stu-id="15fbb-125">Inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server), provide a function that uses `TextDecoder` to decode a passed array:</span></span>

[!code-html[](javascript-interop/samples_snapshot/index-script.html)]

<span data-ttu-id="15fbb-126">El código JavaScript, como el código que se muestra en el ejemplo anterior, también se puede cargar desde un archivo JavaScript ( *. js*) con una referencia al archivo de script:</span><span class="sxs-lookup"><span data-stu-id="15fbb-126">JavaScript code, such as the code shown in the preceding example, can also be loaded from a JavaScript file (*.js*) with a reference to the script file:</span></span>

```html
<script src="exampleJsInterop.js"></script>
```

<span data-ttu-id="15fbb-127">El siguiente componente:</span><span class="sxs-lookup"><span data-stu-id="15fbb-127">The following component:</span></span>

* <span data-ttu-id="15fbb-128">Invoca la función JavaScript `ConvertArray` mediante `JsRuntime` cuando se selecciona un botón de componente (**convertir matriz**).</span><span class="sxs-lookup"><span data-stu-id="15fbb-128">Invokes the `ConvertArray` JavaScript function using `JsRuntime` when a component button (**Convert Array**) is selected.</span></span>
* <span data-ttu-id="15fbb-129">Después de llamar a la función de JavaScript, la matriz que se pasa se convierte en una cadena.</span><span class="sxs-lookup"><span data-stu-id="15fbb-129">After the JavaScript function is called, the passed array is converted into a string.</span></span> <span data-ttu-id="15fbb-130">La cadena se devuelve al componente para su presentación.</span><span class="sxs-lookup"><span data-stu-id="15fbb-130">The string is returned to the component for display.</span></span>

[!code-cshtml[](javascript-interop/samples_snapshot/call-js-example.razor?highlight=2,34-35)]

<span data-ttu-id="15fbb-131">Para usar la abstracción `IJSRuntime`, adopte cualquiera de los métodos siguientes:</span><span class="sxs-lookup"><span data-stu-id="15fbb-131">To use the `IJSRuntime` abstraction, adopt any of the following approaches:</span></span>

* <span data-ttu-id="15fbb-132">Inserte la abstracción `IJSRuntime` en el componente de Razor ( *. Razor*):</span><span class="sxs-lookup"><span data-stu-id="15fbb-132">Inject the `IJSRuntime` abstraction into the Razor component (*.razor*):</span></span>

  [!code-cshtml[](javascript-interop/samples_snapshot/inject-abstraction.razor?highlight=1)]

* <span data-ttu-id="15fbb-133">Inserte la abstracción `IJSRuntime` en una clase ( *. CS*):</span><span class="sxs-lookup"><span data-stu-id="15fbb-133">Inject the `IJSRuntime` abstraction into a class (*.cs*):</span></span>

  [!code-csharp[](javascript-interop/samples_snapshot/inject-abstraction-class.cs?highlight=5)]

* <span data-ttu-id="15fbb-134">Para la generación de contenido dinámico con [BuildRenderTree](xref:blazor/components#manual-rendertreebuilder-logic), use el atributo `[Inject]`:</span><span class="sxs-lookup"><span data-stu-id="15fbb-134">For dynamic content generation with [BuildRenderTree](xref:blazor/components#manual-rendertreebuilder-logic), use the `[Inject]` attribute:</span></span>

  ```csharp
  [Inject]
  IJSRuntime JSRuntime { get; set; }
  ```

<span data-ttu-id="15fbb-135">En la aplicación de ejemplo del lado cliente que acompaña a este tema, hay dos funciones de JavaScript disponibles para la aplicación que interactúan con el DOM para recibir datos proporcionados por el usuario y mostrar un mensaje de bienvenida:</span><span class="sxs-lookup"><span data-stu-id="15fbb-135">In the client-side sample app that accompanies this topic, two JavaScript functions are available to the app that interact with the DOM to receive user input and display a welcome message:</span></span>

* <span data-ttu-id="15fbb-136">`showPrompt` &ndash; genera un mensaje para aceptar la entrada del usuario (el nombre del usuario) y devuelve el nombre al autor de la llamada.</span><span class="sxs-lookup"><span data-stu-id="15fbb-136">`showPrompt` &ndash; Produces a prompt to accept user input (the user's name) and returns the name to the caller.</span></span>
* <span data-ttu-id="15fbb-137">`displayWelcome` &ndash; asigna un mensaje de bienvenida del autor de la llamada a un objeto DOM con un @no__t 2 de `welcome`.</span><span class="sxs-lookup"><span data-stu-id="15fbb-137">`displayWelcome` &ndash; Assigns a welcome message from the caller to a DOM object with an `id` of `welcome`.</span></span>

<span data-ttu-id="15fbb-138">*wwwroot/exampleJsInterop. js*:</span><span class="sxs-lookup"><span data-stu-id="15fbb-138">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=2-7)]

<span data-ttu-id="15fbb-139">Coloque la etiqueta `<script>` que hace referencia al archivo JavaScript en el archivo *wwwroot/index.html* (webassembly) o el archivo *pages/_Host. cshtml* (servidor increíble).</span><span class="sxs-lookup"><span data-stu-id="15fbb-139">Place the `<script>` tag that references the JavaScript file in the *wwwroot/index.html* file (Blazor WebAssembly) or *Pages/_Host.cshtml* file (Blazor Server).</span></span>

<span data-ttu-id="15fbb-140">*wwwroot/index.html* (webassembly increíblemente):</span><span class="sxs-lookup"><span data-stu-id="15fbb-140">*wwwroot/index.html* (Blazor WebAssembly):</span></span>

[!code-html[](./common/samples/3.x/BlazorSample/wwwroot/index.html?highlight=15)]

<span data-ttu-id="15fbb-141">*Pages/_Host. cshtml* (servidor increíble):</span><span class="sxs-lookup"><span data-stu-id="15fbb-141">*Pages/_Host.cshtml* (Blazor Server):</span></span>

[!code-cshtml[](javascript-interop/samples_snapshot/_Host.cshtml?highlight=29)]

<span data-ttu-id="15fbb-142">No coloque una etiqueta `<script>` en un archivo de componente porque la etiqueta `<script>` no se puede actualizar dinámicamente.</span><span class="sxs-lookup"><span data-stu-id="15fbb-142">Don't place a `<script>` tag in a component file because the `<script>` tag can't be updated dynamically.</span></span>

<span data-ttu-id="15fbb-143">Los métodos de .NET interoperan con las funciones de JavaScript en el archivo *exampleJsInterop. js* llamando a `IJSRuntime.InvokeAsync<T>`.</span><span class="sxs-lookup"><span data-stu-id="15fbb-143">.NET methods interop with the JavaScript functions in the *exampleJsInterop.js* file by calling `IJSRuntime.InvokeAsync<T>`.</span></span>

<span data-ttu-id="15fbb-144">La abstracción `IJSRuntime` es asincrónica para permitir escenarios de servidor increíbles.</span><span class="sxs-lookup"><span data-stu-id="15fbb-144">The `IJSRuntime` abstraction is asynchronous to allow for Blazor Server scenarios.</span></span> <span data-ttu-id="15fbb-145">Si la aplicación es una aplicación de webassembly increíblemente alta y desea invocar una función de JavaScript sincrónicamente, la conversión a `IJSInProcessRuntime` y llama a `Invoke<T>` en su lugar.</span><span class="sxs-lookup"><span data-stu-id="15fbb-145">If the app is a Blazor WebAssembly app and you want to invoke a JavaScript function synchronously, downcast to `IJSInProcessRuntime` and call `Invoke<T>` instead.</span></span> <span data-ttu-id="15fbb-146">Se recomienda que la mayoría de las bibliotecas de interoperabilidad de JavaScript usen las API asincrónicas para asegurarse de que las bibliotecas están disponibles en todos los escenarios.</span><span class="sxs-lookup"><span data-stu-id="15fbb-146">We recommend that most JavaScript interop libraries use the async APIs to ensure that the libraries are available in all scenarios.</span></span>

<span data-ttu-id="15fbb-147">La aplicación de ejemplo incluye un componente para mostrar la interoperabilidad de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="15fbb-147">The sample app includes a component to demonstrate JavaScript interop.</span></span> <span data-ttu-id="15fbb-148">El componente:</span><span class="sxs-lookup"><span data-stu-id="15fbb-148">The component:</span></span>

* <span data-ttu-id="15fbb-149">Recibe la entrada del usuario a través de un mensaje de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="15fbb-149">Receives user input via a JavaScript prompt.</span></span>
* <span data-ttu-id="15fbb-150">Devuelve el texto del componente que se va a procesar.</span><span class="sxs-lookup"><span data-stu-id="15fbb-150">Returns the text to the component for processing.</span></span>
* <span data-ttu-id="15fbb-151">Llama a una segunda función de JavaScript que interactúa con el DOM para mostrar un mensaje de bienvenida.</span><span class="sxs-lookup"><span data-stu-id="15fbb-151">Calls a second JavaScript function that interacts with the DOM to display a welcome message.</span></span>

<span data-ttu-id="15fbb-152">*Pages/JSInterop. Razor*:</span><span class="sxs-lookup"><span data-stu-id="15fbb-152">*Pages/JSInterop.razor*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.razor?name=snippet_JSInterop1&highlight=3,19-21,23-25)]

1. <span data-ttu-id="15fbb-153">Cuando se ejecuta `TriggerJsPrompt` seleccionando el botón **desencadenador de JavaScript** del componente, se llama a la función JavaScript `showPrompt` proporcionada en el archivo *wwwroot/exampleJsInterop. js* .</span><span class="sxs-lookup"><span data-stu-id="15fbb-153">When `TriggerJsPrompt` is executed by selecting the component's **Trigger JavaScript Prompt** button, the JavaScript `showPrompt` function provided in the *wwwroot/exampleJsInterop.js* file is called.</span></span>
1. <span data-ttu-id="15fbb-154">La función `showPrompt` acepta datos proporcionados por el usuario (el nombre del usuario), que se codifica en HTML y se devuelve al componente.</span><span class="sxs-lookup"><span data-stu-id="15fbb-154">The `showPrompt` function accepts user input (the user's name), which is HTML-encoded and returned to the component.</span></span> <span data-ttu-id="15fbb-155">El componente almacena el nombre del usuario en una variable local, `name`.</span><span class="sxs-lookup"><span data-stu-id="15fbb-155">The component stores the user's name in a local variable, `name`.</span></span>
1. <span data-ttu-id="15fbb-156">La cadena almacenada en `name` se incorpora en un mensaje de bienvenida, que se pasa a una función de JavaScript, `displayWelcome`, que representa el mensaje de bienvenida en una etiqueta de encabezado.</span><span class="sxs-lookup"><span data-stu-id="15fbb-156">The string stored in `name` is incorporated into a welcome message, which is passed to a JavaScript function, `displayWelcome`, which renders the welcome message into a heading tag.</span></span>

## <a name="call-a-void-javascript-function"></a><span data-ttu-id="15fbb-157">Llamar a una función de JavaScript void</span><span class="sxs-lookup"><span data-stu-id="15fbb-157">Call a void JavaScript function</span></span>

<span data-ttu-id="15fbb-158">Se llama a las funciones de JavaScript que devuelven [void (0)/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void) o [undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) con `IJSRuntime.InvokeVoidAsync`.</span><span class="sxs-lookup"><span data-stu-id="15fbb-158">JavaScript functions that return [void(0)/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void) or [undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) are called with `IJSRuntime.InvokeVoidAsync`.</span></span>

## <a name="detect-when-a-blazor-app-is-prerendering"></a><span data-ttu-id="15fbb-159">Detección cuando una aplicación increíblemente se está preprocesando</span><span class="sxs-lookup"><span data-stu-id="15fbb-159">Detect when a Blazor app is prerendering</span></span>
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

## <a name="capture-references-to-elements"></a><span data-ttu-id="15fbb-160">Capturar referencias a elementos</span><span class="sxs-lookup"><span data-stu-id="15fbb-160">Capture references to elements</span></span>

<span data-ttu-id="15fbb-161">Algunos escenarios de [interoperabilidad de JavaScript](xref:blazor/javascript-interop) requieren referencias a elementos HTML.</span><span class="sxs-lookup"><span data-stu-id="15fbb-161">Some [JavaScript interop](xref:blazor/javascript-interop) scenarios require references to HTML elements.</span></span> <span data-ttu-id="15fbb-162">Por ejemplo, una biblioteca de interfaz de usuario puede requerir una referencia de elemento para la inicialización, o puede que necesite llamar a las API de tipo de comando en un elemento, como `focus` o `play`.</span><span class="sxs-lookup"><span data-stu-id="15fbb-162">For example, a UI library may require an element reference for initialization, or you might need to call command-like APIs on an element, such as `focus` or `play`.</span></span>

<span data-ttu-id="15fbb-163">Capture las referencias a los elementos HTML de un componente con el siguiente enfoque:</span><span class="sxs-lookup"><span data-stu-id="15fbb-163">Capture references to HTML elements in a component using the following approach:</span></span>

* <span data-ttu-id="15fbb-164">Agregue un atributo `@ref` al elemento HTML.</span><span class="sxs-lookup"><span data-stu-id="15fbb-164">Add an `@ref` attribute to the HTML element.</span></span>
* <span data-ttu-id="15fbb-165">Defina un campo de tipo `ElementReference` cuyo nombre coincida con el valor del atributo `@ref`.</span><span class="sxs-lookup"><span data-stu-id="15fbb-165">Define a field of type `ElementReference` whose name matches the value of the `@ref` attribute.</span></span>

<span data-ttu-id="15fbb-166">En el ejemplo siguiente se muestra la captura de una referencia al elemento `username` `<input>`:</span><span class="sxs-lookup"><span data-stu-id="15fbb-166">The following example shows capturing a reference to the `username` `<input>` element:</span></span>

```cshtml
<input @ref="username" ... />

@code {
    ElementReference username;
}
```

> [!NOTE]
> <span data-ttu-id="15fbb-167">**No** utilice referencias a elementos capturados como forma de rellenar el Dom.</span><span class="sxs-lookup"><span data-stu-id="15fbb-167">Do **not** use captured element references as a way of populating the DOM.</span></span> <span data-ttu-id="15fbb-168">Si lo hace, puede interferir con el modelo de representación declarativa.</span><span class="sxs-lookup"><span data-stu-id="15fbb-168">Doing so may interfere with the declarative rendering model.</span></span>

<span data-ttu-id="15fbb-169">En lo que respecta al código de .NET, un `ElementReference` es un identificador opaco.</span><span class="sxs-lookup"><span data-stu-id="15fbb-169">As far as .NET code is concerned, an `ElementReference` is an opaque handle.</span></span> <span data-ttu-id="15fbb-170">Lo *único* que puede hacer con `ElementReference` es pasarlo a código JavaScript a través de la interoperabilidad de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="15fbb-170">The *only* thing you can do with `ElementReference` is pass it through to JavaScript code via JavaScript interop.</span></span> <span data-ttu-id="15fbb-171">Al hacerlo, el código de JavaScript recibe una instancia `HTMLElement`, que puede usar con las API de DOM normales.</span><span class="sxs-lookup"><span data-stu-id="15fbb-171">When you do so, the JavaScript-side code receives an `HTMLElement` instance, which it can use with normal DOM APIs.</span></span>

<span data-ttu-id="15fbb-172">Por ejemplo, el código siguiente define un método de extensión de .NET que permite establecer el foco en un elemento:</span><span class="sxs-lookup"><span data-stu-id="15fbb-172">For example, the following code defines a .NET extension method that enables setting the focus on an element:</span></span>

<span data-ttu-id="15fbb-173">*exampleJsInterop. js*:</span><span class="sxs-lookup"><span data-stu-id="15fbb-173">*exampleJsInterop.js*:</span></span>

```javascript
window.exampleJsFunctions = {
  focusElement : function (element) {
    element.focus();
  }
}
```

<span data-ttu-id="15fbb-174">Use `IJSRuntime.InvokeAsync<T>` y llame a `exampleJsFunctions.focusElement` con un @no__t 2 para centrar un elemento:</span><span class="sxs-lookup"><span data-stu-id="15fbb-174">Use `IJSRuntime.InvokeAsync<T>` and call `exampleJsFunctions.focusElement` with an `ElementReference` to focus an element:</span></span>

[!code-cshtml[](javascript-interop/samples_snapshot/component1.razor?highlight=1,3,11-12)]

<span data-ttu-id="15fbb-175">Para usar un método de extensión para centrar un elemento, cree un método de extensión estático que reciba la instancia `IJSRuntime`:</span><span class="sxs-lookup"><span data-stu-id="15fbb-175">To use an extension method to focus an element, create a static extension method that receives the `IJSRuntime` instance:</span></span>

```csharp
public static Task Focus(this ElementReference elementRef, IJSRuntime jsRuntime)
{
    return jsRuntime.InvokeAsync<object>(
        "exampleJsFunctions.focusElement", elementRef);
}
```

<span data-ttu-id="15fbb-176">Se llama al método directamente en el objeto.</span><span class="sxs-lookup"><span data-stu-id="15fbb-176">The method is called directly on the object.</span></span> <span data-ttu-id="15fbb-177">En el ejemplo siguiente se da por supuesto que el método estático `Focus` está disponible en el espacio de nombres `JsInteropClasses`:</span><span class="sxs-lookup"><span data-stu-id="15fbb-177">The following example assumes that the static `Focus` method is available from the `JsInteropClasses` namespace:</span></span>

[!code-cshtml[](javascript-interop/samples_snapshot/component2.razor?highlight=1,4,12)]

> [!IMPORTANT]
> <span data-ttu-id="15fbb-178">La variable `username` solo se rellena después de representar el componente.</span><span class="sxs-lookup"><span data-stu-id="15fbb-178">The `username` variable is only populated after the component is rendered.</span></span> <span data-ttu-id="15fbb-179">Si se pasa un `ElementReference` sin rellenar al código JavaScript, el código JavaScript recibe un valor de `null`.</span><span class="sxs-lookup"><span data-stu-id="15fbb-179">If an unpopulated `ElementReference` is passed to JavaScript code, the JavaScript code receives a value of `null`.</span></span> <span data-ttu-id="15fbb-180">Para manipular las referencias de elemento una vez finalizada la representación del componente (para establecer el foco inicial en un elemento), use los métodos de ciclo de [vida del componente](xref:blazor/components#lifecycle-methods)`OnAfterRenderAsync` o `OnAfterRender`.</span><span class="sxs-lookup"><span data-stu-id="15fbb-180">To manipulate element references after the component has finished rendering (to set the initial focus on an element) use the `OnAfterRenderAsync` or `OnAfterRender` [component lifecycle methods](xref:blazor/components#lifecycle-methods).</span></span>

## <a name="invoke-net-methods-from-javascript-functions"></a><span data-ttu-id="15fbb-181">Invocar métodos .NET desde funciones de JavaScript</span><span class="sxs-lookup"><span data-stu-id="15fbb-181">Invoke .NET methods from JavaScript functions</span></span>

### <a name="static-net-method-call"></a><span data-ttu-id="15fbb-182">Llamada al método estático de .NET</span><span class="sxs-lookup"><span data-stu-id="15fbb-182">Static .NET method call</span></span>

<span data-ttu-id="15fbb-183">Para invocar un método .NET estático desde JavaScript, use las funciones `DotNet.invokeMethod` o `DotNet.invokeMethodAsync`.</span><span class="sxs-lookup"><span data-stu-id="15fbb-183">To invoke a static .NET method from JavaScript, use the `DotNet.invokeMethod` or `DotNet.invokeMethodAsync` functions.</span></span> <span data-ttu-id="15fbb-184">Pase el identificador del método estático al que desea llamar, el nombre del ensamblado que contiene la función y cualquier argumento.</span><span class="sxs-lookup"><span data-stu-id="15fbb-184">Pass in the identifier of the static method you wish to call, the name of the assembly containing the function, and any arguments.</span></span> <span data-ttu-id="15fbb-185">Se prefiere la versión asincrónica para admitir escenarios de servidor increíbles.</span><span class="sxs-lookup"><span data-stu-id="15fbb-185">The asynchronous version is preferred to support Blazor Server scenarios.</span></span> <span data-ttu-id="15fbb-186">Para invocar un método .NET desde JavaScript, el método .NET debe ser público, estático y tener el atributo `[JSInvokable]`.</span><span class="sxs-lookup"><span data-stu-id="15fbb-186">To invoke a .NET method from JavaScript, the .NET method must be public, static, and have the `[JSInvokable]` attribute.</span></span> <span data-ttu-id="15fbb-187">De forma predeterminada, el identificador del método es el nombre del método, pero puede especificar un identificador diferente mediante el constructor `JSInvokableAttribute`.</span><span class="sxs-lookup"><span data-stu-id="15fbb-187">By default, the method identifier is the method name, but you can specify a different identifier using the `JSInvokableAttribute` constructor.</span></span> <span data-ttu-id="15fbb-188">Actualmente no se admite la llamada a métodos genéricos abiertos.</span><span class="sxs-lookup"><span data-stu-id="15fbb-188">Calling open generic methods isn't currently supported.</span></span>

<span data-ttu-id="15fbb-189">La aplicación de ejemplo incluye C# un método para devolver una matriz de `int`S.</span><span class="sxs-lookup"><span data-stu-id="15fbb-189">The sample app includes a C# method to return an array of `int`s.</span></span> <span data-ttu-id="15fbb-190">El atributo `JSInvokable` se aplica al método.</span><span class="sxs-lookup"><span data-stu-id="15fbb-190">The `JSInvokable` attribute is applied to the method.</span></span>

<span data-ttu-id="15fbb-191">*Pages/JsInterop. Razor*:</span><span class="sxs-lookup"><span data-stu-id="15fbb-191">*Pages/JsInterop.razor*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.razor?name=snippet_JSInterop2&highlight=7-11)]

<span data-ttu-id="15fbb-192">JavaScript atendido al cliente invoca el C# método .net.</span><span class="sxs-lookup"><span data-stu-id="15fbb-192">JavaScript served to the client invokes the C# .NET method.</span></span>

<span data-ttu-id="15fbb-193">*wwwroot/exampleJsInterop. js*:</span><span class="sxs-lookup"><span data-stu-id="15fbb-193">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=8-14)]

<span data-ttu-id="15fbb-194">Cuando se seleccione el botón **desencadenador de método estático de .net ReturnArrayAsync** , examine la salida de la consola en las herramientas de desarrollo web del explorador.</span><span class="sxs-lookup"><span data-stu-id="15fbb-194">When the **Trigger .NET static method ReturnArrayAsync** button is selected, examine the console output in the browser's web developer tools.</span></span>

<span data-ttu-id="15fbb-195">La salida de la consola es:</span><span class="sxs-lookup"><span data-stu-id="15fbb-195">The console output is:</span></span>

```console
Array(4) [ 1, 2, 3, 4 ]
```

<span data-ttu-id="15fbb-196">El cuarto valor de matriz se inserta en la matriz (`data.push(4);`) devuelta por `ReturnArrayAsync`.</span><span class="sxs-lookup"><span data-stu-id="15fbb-196">The fourth array value is pushed to the array (`data.push(4);`) returned by `ReturnArrayAsync`.</span></span>

### <a name="instance-method-call"></a><span data-ttu-id="15fbb-197">Llamada al método de instancia</span><span class="sxs-lookup"><span data-stu-id="15fbb-197">Instance method call</span></span>

<span data-ttu-id="15fbb-198">También puede llamar a métodos de instancia de .NET desde JavaScript.</span><span class="sxs-lookup"><span data-stu-id="15fbb-198">You can also call .NET instance methods from JavaScript.</span></span> <span data-ttu-id="15fbb-199">Para invocar un método de instancia de .NET desde JavaScript:</span><span class="sxs-lookup"><span data-stu-id="15fbb-199">To invoke a .NET instance method from JavaScript:</span></span>

* <span data-ttu-id="15fbb-200">Pase la instancia de .NET a JavaScript ajustándola en una instancia de `DotNetObjectReference`.</span><span class="sxs-lookup"><span data-stu-id="15fbb-200">Pass the .NET instance to JavaScript by wrapping it in a `DotNetObjectReference` instance.</span></span> <span data-ttu-id="15fbb-201">La instancia de .NET se pasa por referencia a JavaScript.</span><span class="sxs-lookup"><span data-stu-id="15fbb-201">The .NET instance is passed by reference to JavaScript.</span></span>
* <span data-ttu-id="15fbb-202">Invocar métodos de instancia de .NET en la instancia de mediante las funciones `invokeMethod` o `invokeMethodAsync`.</span><span class="sxs-lookup"><span data-stu-id="15fbb-202">Invoke .NET instance methods on the instance using the `invokeMethod` or `invokeMethodAsync` functions.</span></span> <span data-ttu-id="15fbb-203">La instancia de .NET también se puede pasar como argumento al invocar otros métodos .NET desde JavaScript.</span><span class="sxs-lookup"><span data-stu-id="15fbb-203">The .NET instance can also be passed as an argument when invoking other .NET methods from JavaScript.</span></span>

> [!NOTE]
> <span data-ttu-id="15fbb-204">La aplicación de ejemplo registra mensajes en la consola del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="15fbb-204">The sample app logs messages to the client-side console.</span></span> <span data-ttu-id="15fbb-205">En los siguientes ejemplos mostrados por la aplicación de ejemplo, examine la salida de la consola del explorador en las herramientas de desarrollo del explorador.</span><span class="sxs-lookup"><span data-stu-id="15fbb-205">For the following examples demonstrated by the sample app, examine the browser's console output in the browser's developer tools.</span></span>

<span data-ttu-id="15fbb-206">Cuando se selecciona el botón **desencadenador de instancia de .net HelloHelper. SayHello** , se llama a `ExampleJsInterop.CallHelloHelperSayHello` y se pasa un nombre, `Blazor`, al método.</span><span class="sxs-lookup"><span data-stu-id="15fbb-206">When the **Trigger .NET instance method HelloHelper.SayHello** button is selected, `ExampleJsInterop.CallHelloHelperSayHello` is called and passes a name, `Blazor`, to the method.</span></span>

<span data-ttu-id="15fbb-207">*Pages/JsInterop. Razor*:</span><span class="sxs-lookup"><span data-stu-id="15fbb-207">*Pages/JsInterop.razor*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.razor?name=snippet_JSInterop3&highlight=8-9)]

<span data-ttu-id="15fbb-208">`CallHelloHelperSayHello` invoca la función de JavaScript `sayHello` con una nueva instancia de `HelloHelper`.</span><span class="sxs-lookup"><span data-stu-id="15fbb-208">`CallHelloHelperSayHello` invokes the JavaScript function `sayHello` with a new instance of `HelloHelper`.</span></span>

<span data-ttu-id="15fbb-209">*JsInteropClasses/ExampleJsInterop. CS*:</span><span class="sxs-lookup"><span data-stu-id="15fbb-209">*JsInteropClasses/ExampleJsInterop.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=10-16)]

<span data-ttu-id="15fbb-210">*wwwroot/exampleJsInterop. js*:</span><span class="sxs-lookup"><span data-stu-id="15fbb-210">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=15-18)]

<span data-ttu-id="15fbb-211">El nombre se pasa al constructor de `HelloHelper`, que establece la propiedad `HelloHelper.Name`.</span><span class="sxs-lookup"><span data-stu-id="15fbb-211">The name is passed to `HelloHelper`'s constructor, which sets the `HelloHelper.Name` property.</span></span> <span data-ttu-id="15fbb-212">Cuando se ejecuta la función de JavaScript `sayHello`, `HelloHelper.SayHello` devuelve el mensaje @no__t 2, que la función JavaScript escribe en la consola.</span><span class="sxs-lookup"><span data-stu-id="15fbb-212">When the JavaScript function `sayHello` is executed, `HelloHelper.SayHello` returns the `Hello, {Name}!` message, which is written to the console by the JavaScript function.</span></span>

<span data-ttu-id="15fbb-213">*JsInteropClasses/HelloHelper. CS*:</span><span class="sxs-lookup"><span data-stu-id="15fbb-213">*JsInteropClasses/HelloHelper.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

<span data-ttu-id="15fbb-214">Salida de la consola en las herramientas de desarrollo web del explorador:</span><span class="sxs-lookup"><span data-stu-id="15fbb-214">Console output in the browser's web developer tools:</span></span>

```console
Hello, Blazor!
```

## <a name="share-interop-code-in-a-class-library"></a><span data-ttu-id="15fbb-215">Compartir código de interoperabilidad en una biblioteca de clases</span><span class="sxs-lookup"><span data-stu-id="15fbb-215">Share interop code in a class library</span></span>

<span data-ttu-id="15fbb-216">El código de interoperabilidad de JavaScript se puede incluir en una biblioteca de clases, lo que permite compartir el código en un paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="15fbb-216">JavaScript interop code can be included in a class library, which allows you to share the code in a NuGet package.</span></span>

<span data-ttu-id="15fbb-217">La biblioteca de clases controla la incrustación de recursos de JavaScript en el ensamblado compilado.</span><span class="sxs-lookup"><span data-stu-id="15fbb-217">The class library handles embedding JavaScript resources in the built assembly.</span></span> <span data-ttu-id="15fbb-218">Los archivos de JavaScript se colocan en la carpeta *wwwroot* .</span><span class="sxs-lookup"><span data-stu-id="15fbb-218">The JavaScript files are placed in the *wwwroot* folder.</span></span> <span data-ttu-id="15fbb-219">Las herramientas se encargan de insertar los recursos cuando se compila la biblioteca.</span><span class="sxs-lookup"><span data-stu-id="15fbb-219">The tooling takes care of embedding the resources when the library is built.</span></span>

<span data-ttu-id="15fbb-220">Se hace referencia al paquete de NuGet compilado en el archivo de proyecto de la aplicación de la misma manera que se hace referencia a cualquier paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="15fbb-220">The built NuGet package is referenced in the app's project file the same way that any NuGet package is referenced.</span></span> <span data-ttu-id="15fbb-221">Una vez restaurado el paquete, el código de la aplicación puede llamar a JavaScript C#como si fuera.</span><span class="sxs-lookup"><span data-stu-id="15fbb-221">After the package is restored, app code can call into JavaScript as if it were C#.</span></span>

<span data-ttu-id="15fbb-222">Para obtener más información, vea <xref:blazor/class-libraries>.</span><span class="sxs-lookup"><span data-stu-id="15fbb-222">For more information, see <xref:blazor/class-libraries>.</span></span>

## <a name="harden-js-interop-calls"></a><span data-ttu-id="15fbb-223">Reprotección de llamadas de interoperabilidad de JS</span><span class="sxs-lookup"><span data-stu-id="15fbb-223">Harden JS interop calls</span></span>

<span data-ttu-id="15fbb-224">La interoperabilidad de JS puede producir un error debido a errores de red y debe tratarse como no confiable.</span><span class="sxs-lookup"><span data-stu-id="15fbb-224">JS interop may fail due to networking errors and should be treated as unreliable.</span></span> <span data-ttu-id="15fbb-225">De forma predeterminada, una aplicación de servidor increíblemente agota el tiempo de espera de las llamadas de interoperabilidad de JS en el servidor después de un minuto.</span><span class="sxs-lookup"><span data-stu-id="15fbb-225">By default, a Blazor Server app times out JS interop calls on the server after one minute.</span></span> <span data-ttu-id="15fbb-226">Si una aplicación puede tolerar un tiempo de espera más agresivo, como 10 segundos, establezca el tiempo de espera con uno de los siguientes métodos:</span><span class="sxs-lookup"><span data-stu-id="15fbb-226">If an app can tolerate a more aggressive timeout, such as 10 seconds, set the timeout using one of the following approaches:</span></span>

* <span data-ttu-id="15fbb-227">Globalmente en `Startup.ConfigureServices`, especifique el tiempo de espera:</span><span class="sxs-lookup"><span data-stu-id="15fbb-227">Globally in `Startup.ConfigureServices`, specify the timeout:</span></span>

  ```csharp
  services.AddServerSideBlazor(
      options => options.JSInteropDefaultCallTimeout = TimeSpan.FromSeconds({SECONDS}));
  ```

* <span data-ttu-id="15fbb-228">Por cada invocación en el código de componente, una sola llamada puede especificar el tiempo de espera:</span><span class="sxs-lookup"><span data-stu-id="15fbb-228">Per-invocation in component code, a single call can specify the timeout:</span></span>

  ```csharp
  var result = await JSRuntime.InvokeAsync<string>("MyJSOperation", 
      TimeSpan.FromSeconds({SECONDS}), new[] { "Arg1" });
  ```

<span data-ttu-id="15fbb-229">Para obtener más información sobre el agotamiento de recursos, consulte <xref:security/blazor/server>.</span><span class="sxs-lookup"><span data-stu-id="15fbb-229">For more information on resource exhaustion, see <xref:security/blazor/server>.</span></span>
