---
title: ASP.NET Core Blazor la interoperabilidad de JavaScript
author: guardrex
description: Obtenga información sobre cómo invocar funciones de JavaScript desde los métodos .NET y .NET desde JavaScript en Blazor aplicaciones.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/23/2020
no-loc:
- Blazor
- SignalR
uid: blazor/javascript-interop
ms.openlocfilehash: c4f2444b60fc2d3a8af893df379cf62636a7bdd5
ms.sourcegitcommit: d2ba66023884f0dca115ff010bd98d5ed6459283
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/14/2020
ms.locfileid: "77213368"
---
# <a name="aspnet-core-opno-locblazor-javascript-interop"></a><span data-ttu-id="099ec-103">ASP.NET Core Blazor la interoperabilidad de JavaScript</span><span class="sxs-lookup"><span data-stu-id="099ec-103">ASP.NET Core Blazor JavaScript interop</span></span>

<span data-ttu-id="099ec-104">Por [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27)y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="099ec-104">By [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="099ec-105">Una aplicación Blazor puede invocar funciones de JavaScript desde los métodos .NET y .NET desde el código JavaScript.</span><span class="sxs-lookup"><span data-stu-id="099ec-105">A Blazor app can invoke JavaScript functions from .NET and .NET methods from JavaScript code.</span></span>

<span data-ttu-id="099ec-106">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="099ec-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="invoke-javascript-functions-from-net-methods"></a><span data-ttu-id="099ec-107">Invocar funciones de JavaScript desde métodos de .NET</span><span class="sxs-lookup"><span data-stu-id="099ec-107">Invoke JavaScript functions from .NET methods</span></span>

<span data-ttu-id="099ec-108">Hay ocasiones en las que se requiere código .NET para llamar a una función de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="099ec-108">There are times when .NET code is required to call a JavaScript function.</span></span> <span data-ttu-id="099ec-109">Por ejemplo, una llamada de JavaScript puede exponer funciones o funcionalidades del explorador de una biblioteca de JavaScript a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="099ec-109">For example, a JavaScript call can expose browser capabilities or functionality from a JavaScript library to the app.</span></span> <span data-ttu-id="099ec-110">Este escenario se denomina *interoperabilidad de JavaScript* (*interoperabilidad de JS*).</span><span class="sxs-lookup"><span data-stu-id="099ec-110">This scenario is called *JavaScript interoperability* (*JS interop*).</span></span>

<span data-ttu-id="099ec-111">Para llamar a JavaScript desde .NET, use la abstracción `IJSRuntime`.</span><span class="sxs-lookup"><span data-stu-id="099ec-111">To call into JavaScript from .NET, use the `IJSRuntime` abstraction.</span></span> <span data-ttu-id="099ec-112">Para emitir llamadas de interoperabilidad de JS, inserte la abstracción de `IJSRuntime` en el componente.</span><span class="sxs-lookup"><span data-stu-id="099ec-112">To issue JS interop calls, inject the `IJSRuntime` abstraction in your component.</span></span> <span data-ttu-id="099ec-113">El método `InvokeAsync<T>` toma un identificador de la función de JavaScript que se desea invocar junto con cualquier número de argumentos serializables de JSON.</span><span class="sxs-lookup"><span data-stu-id="099ec-113">The `InvokeAsync<T>` method takes an identifier for the JavaScript function that you wish to invoke along with any number of JSON-serializable arguments.</span></span> <span data-ttu-id="099ec-114">El identificador de función es relativo al ámbito global (`window`).</span><span class="sxs-lookup"><span data-stu-id="099ec-114">The function identifier is relative to the global scope (`window`).</span></span> <span data-ttu-id="099ec-115">Si desea llamar a `window.someScope.someFunction`, el identificador se `someScope.someFunction`.</span><span class="sxs-lookup"><span data-stu-id="099ec-115">If you wish to call `window.someScope.someFunction`, the identifier is `someScope.someFunction`.</span></span> <span data-ttu-id="099ec-116">No es necesario registrar la función antes de que se le llame.</span><span class="sxs-lookup"><span data-stu-id="099ec-116">There's no need to register the function before it's called.</span></span> <span data-ttu-id="099ec-117">El tipo de valor devuelto `T` también debe ser serializable.</span><span class="sxs-lookup"><span data-stu-id="099ec-117">The return type `T` must also be JSON serializable.</span></span> <span data-ttu-id="099ec-118">`T` debe coincidir con el tipo de .NET que se asigna mejor al tipo JSON devuelto.</span><span class="sxs-lookup"><span data-stu-id="099ec-118">`T` should match the .NET type that best maps to the JSON type returned.</span></span>

<span data-ttu-id="099ec-119">En el caso de las aplicaciones de Blazor Server que tienen habilitada la representación previa, no es posible llamar a JavaScript durante la prerepresentación inicial.</span><span class="sxs-lookup"><span data-stu-id="099ec-119">For Blazor Server apps with prerendering enabled, calling into JavaScript isn't possible during the initial prerendering.</span></span> <span data-ttu-id="099ec-120">Las llamadas de interoperabilidad de JavaScript se deben diferir hasta que se establezca la conexión con el explorador.</span><span class="sxs-lookup"><span data-stu-id="099ec-120">JavaScript interop calls must be deferred until after the connection with the browser is established.</span></span> <span data-ttu-id="099ec-121">Para obtener más información, consulte la sección [detectar cuándo se está representando una aplicación Blazor](#detect-when-a-blazor-app-is-prerendering) .</span><span class="sxs-lookup"><span data-stu-id="099ec-121">For more information, see the [Detect when a Blazor app is prerendering](#detect-when-a-blazor-app-is-prerendering) section.</span></span>

<span data-ttu-id="099ec-122">El ejemplo siguiente se basa en [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), un descodificador basado en JavaScript experimental.</span><span class="sxs-lookup"><span data-stu-id="099ec-122">The following example is based on [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), an experimental JavaScript-based decoder.</span></span> <span data-ttu-id="099ec-123">En el ejemplo se muestra cómo invocar una función de C# JavaScript desde un método.</span><span class="sxs-lookup"><span data-stu-id="099ec-123">The example demonstrates how to invoke a JavaScript function from a C# method.</span></span> <span data-ttu-id="099ec-124">La función de JavaScript acepta una matriz de bytes de un C# método, descodifica la matriz y devuelve el texto al componente que se va a mostrar.</span><span class="sxs-lookup"><span data-stu-id="099ec-124">The JavaScript function accepts a byte array from a C# method, decodes the array, and returns the text to the component for display.</span></span>

<span data-ttu-id="099ec-125">Dentro del elemento `<head>` de *wwwroot/index.html* (Blazor webassembly) o *pages/_Host. cshtml* (Blazor Server), proporcione una función de JavaScript que use `TextDecoder` para descodificar una matriz pasada y devolver el valor descodificado:</span><span class="sxs-lookup"><span data-stu-id="099ec-125">Inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server), provide a JavaScript function that uses `TextDecoder` to decode a passed array and return the decoded value:</span></span>

[!code-html[](javascript-interop/samples_snapshot/index-script-convertarray.html)]

<span data-ttu-id="099ec-126">El código JavaScript, como el código que se muestra en el ejemplo anterior, también se puede cargar desde un archivo JavaScript ( *. js*) con una referencia al archivo de script:</span><span class="sxs-lookup"><span data-stu-id="099ec-126">JavaScript code, such as the code shown in the preceding example, can also be loaded from a JavaScript file (*.js*) with a reference to the script file:</span></span>

```html
<script src="exampleJsInterop.js"></script>
```

<span data-ttu-id="099ec-127">El siguiente componente:</span><span class="sxs-lookup"><span data-stu-id="099ec-127">The following component:</span></span>

* <span data-ttu-id="099ec-128">Invoca la función `convertArray` JavaScript mediante `JSRuntime` cuando se selecciona un botón de componente (**convertir matriz**).</span><span class="sxs-lookup"><span data-stu-id="099ec-128">Invokes the `convertArray` JavaScript function using `JSRuntime` when a component button (**Convert Array**) is selected.</span></span>
* <span data-ttu-id="099ec-129">Después de llamar a la función de JavaScript, la matriz que se pasa se convierte en una cadena.</span><span class="sxs-lookup"><span data-stu-id="099ec-129">After the JavaScript function is called, the passed array is converted into a string.</span></span> <span data-ttu-id="099ec-130">La cadena se devuelve al componente para su presentación.</span><span class="sxs-lookup"><span data-stu-id="099ec-130">The string is returned to the component for display.</span></span>

[!code-razor[](javascript-interop/samples_snapshot/call-js-example.razor?highlight=2,34-35)]

## <a name="use-of-ijsruntime"></a><span data-ttu-id="099ec-131">Uso de IJSRuntime</span><span class="sxs-lookup"><span data-stu-id="099ec-131">Use of IJSRuntime</span></span>

<span data-ttu-id="099ec-132">Para usar la abstracción `IJSRuntime`, adopte cualquiera de los enfoques siguientes:</span><span class="sxs-lookup"><span data-stu-id="099ec-132">To use the `IJSRuntime` abstraction, adopt any of the following approaches:</span></span>

* <span data-ttu-id="099ec-133">Inyectar la abstracción de `IJSRuntime` en el componente de Razor ( *. Razor*):</span><span class="sxs-lookup"><span data-stu-id="099ec-133">Inject the `IJSRuntime` abstraction into the Razor component (*.razor*):</span></span>

  [!code-razor[](javascript-interop/samples_snapshot/inject-abstraction.razor?highlight=1)]

  <span data-ttu-id="099ec-134">Dentro del elemento `<head>` de *wwwroot/index.html* (Blazor webassembly) o *pages/_Host. cshtml* (Blazor Server), proporcione una función `handleTickerChanged` JavaScript.</span><span class="sxs-lookup"><span data-stu-id="099ec-134">Inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server), provide a `handleTickerChanged` JavaScript function.</span></span> <span data-ttu-id="099ec-135">Se llama a la función con `IJSRuntime.InvokeVoidAsync` y no devuelve un valor:</span><span class="sxs-lookup"><span data-stu-id="099ec-135">The function is called with `IJSRuntime.InvokeVoidAsync` and doesn't return a value:</span></span>

  [!code-html[](javascript-interop/samples_snapshot/index-script-handleTickerChanged1.html)]

* <span data-ttu-id="099ec-136">Inserte la abstracción de `IJSRuntime` en una clase ( *. CS*):</span><span class="sxs-lookup"><span data-stu-id="099ec-136">Inject the `IJSRuntime` abstraction into a class (*.cs*):</span></span>

  [!code-csharp[](javascript-interop/samples_snapshot/inject-abstraction-class.cs?highlight=5)]

  <span data-ttu-id="099ec-137">Dentro del elemento `<head>` de *wwwroot/index.html* (Blazor webassembly) o *pages/_Host. cshtml* (Blazor Server), proporcione una función `handleTickerChanged` JavaScript.</span><span class="sxs-lookup"><span data-stu-id="099ec-137">Inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server), provide a `handleTickerChanged` JavaScript function.</span></span> <span data-ttu-id="099ec-138">Se llama a la función con `JSRuntime.InvokeAsync` y devuelve un valor:</span><span class="sxs-lookup"><span data-stu-id="099ec-138">The function is called with `JSRuntime.InvokeAsync` and returns a value:</span></span>

  [!code-html[](javascript-interop/samples_snapshot/index-script-handleTickerChanged2.html)]

* <span data-ttu-id="099ec-139">Para la generación de contenido dinámico con [BuildRenderTree](xref:blazor/components#manual-rendertreebuilder-logic), use el atributo `[Inject]`:</span><span class="sxs-lookup"><span data-stu-id="099ec-139">For dynamic content generation with [BuildRenderTree](xref:blazor/components#manual-rendertreebuilder-logic), use the `[Inject]` attribute:</span></span>

  ```razor
  [Inject]
  IJSRuntime JSRuntime { get; set; }
  ```

<span data-ttu-id="099ec-140">En la aplicación de ejemplo del lado cliente que acompaña a este tema, hay dos funciones de JavaScript disponibles para la aplicación que interactúan con el DOM para recibir datos proporcionados por el usuario y mostrar un mensaje de bienvenida:</span><span class="sxs-lookup"><span data-stu-id="099ec-140">In the client-side sample app that accompanies this topic, two JavaScript functions are available to the app that interact with the DOM to receive user input and display a welcome message:</span></span>

* <span data-ttu-id="099ec-141">`showPrompt` &ndash; genera un mensaje para aceptar la entrada del usuario (el nombre del usuario) y devuelve el nombre al autor de la llamada.</span><span class="sxs-lookup"><span data-stu-id="099ec-141">`showPrompt` &ndash; Produces a prompt to accept user input (the user's name) and returns the name to the caller.</span></span>
* <span data-ttu-id="099ec-142">`displayWelcome` &ndash; asigna un mensaje de bienvenida del autor de la llamada a un objeto DOM con un `id` de `welcome`.</span><span class="sxs-lookup"><span data-stu-id="099ec-142">`displayWelcome` &ndash; Assigns a welcome message from the caller to a DOM object with an `id` of `welcome`.</span></span>

<span data-ttu-id="099ec-143">*wwwroot/exampleJsInterop. js*:</span><span class="sxs-lookup"><span data-stu-id="099ec-143">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=2-7)]

<span data-ttu-id="099ec-144">Coloque la etiqueta de `<script>` que hace referencia al archivo JavaScript en el archivo *wwwroot/index.html* (Blazor webassembly) o el archivo *pages/_Host. cshtml* (Blazor Server).</span><span class="sxs-lookup"><span data-stu-id="099ec-144">Place the `<script>` tag that references the JavaScript file in the *wwwroot/index.html* file (Blazor WebAssembly) or *Pages/_Host.cshtml* file (Blazor Server).</span></span>

<span data-ttu-id="099ec-145">*wwwroot/index.html* (Blazor webassembly):</span><span class="sxs-lookup"><span data-stu-id="099ec-145">*wwwroot/index.html* (Blazor WebAssembly):</span></span>

[!code-html[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/index.html?highlight=22)]

<span data-ttu-id="099ec-146">*Pages/_Host. cshtml* (Blazor Server):</span><span class="sxs-lookup"><span data-stu-id="099ec-146">*Pages/_Host.cshtml* (Blazor Server):</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorServerSample/Pages/_Host.cshtml?highlight=35)]

<span data-ttu-id="099ec-147">No coloque un `<script>` etiqueta en un archivo de componente porque la etiqueta de `<script>` no se puede actualizar dinámicamente.</span><span class="sxs-lookup"><span data-stu-id="099ec-147">Don't place a `<script>` tag in a component file because the `<script>` tag can't be updated dynamically.</span></span>

<span data-ttu-id="099ec-148">Los métodos de .NET interoperan con las funciones de JavaScript en el archivo *exampleJsInterop. js* llamando a `IJSRuntime.InvokeAsync<T>`.</span><span class="sxs-lookup"><span data-stu-id="099ec-148">.NET methods interop with the JavaScript functions in the *exampleJsInterop.js* file by calling `IJSRuntime.InvokeAsync<T>`.</span></span>

<span data-ttu-id="099ec-149">La abstracción `IJSRuntime` es asincrónica para permitir escenarios de Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="099ec-149">The `IJSRuntime` abstraction is asynchronous to allow for Blazor Server scenarios.</span></span> <span data-ttu-id="099ec-150">Si la aplicación es una aplicación Blazor webassembly y desea invocar una función de JavaScript sincrónicamente, se puede convertir en `IJSInProcessRuntime` y llamar a `Invoke<T>` en su lugar.</span><span class="sxs-lookup"><span data-stu-id="099ec-150">If the app is a Blazor WebAssembly app and you want to invoke a JavaScript function synchronously, downcast to `IJSInProcessRuntime` and call `Invoke<T>` instead.</span></span> <span data-ttu-id="099ec-151">Se recomienda que la mayoría de las bibliotecas de interoperabilidad de JS usen las API asincrónicas para asegurarse de que las bibliotecas están disponibles en todos los escenarios.</span><span class="sxs-lookup"><span data-stu-id="099ec-151">We recommend that most JS interop libraries use the async APIs to ensure that the libraries are available in all scenarios.</span></span>

<span data-ttu-id="099ec-152">La aplicación de ejemplo incluye un componente para mostrar la interoperabilidad de JS.</span><span class="sxs-lookup"><span data-stu-id="099ec-152">The sample app includes a component to demonstrate JS interop.</span></span> <span data-ttu-id="099ec-153">El componente:</span><span class="sxs-lookup"><span data-stu-id="099ec-153">The component:</span></span>

* <span data-ttu-id="099ec-154">Recibe la entrada del usuario a través de un mensaje de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="099ec-154">Receives user input via a JavaScript prompt.</span></span>
* <span data-ttu-id="099ec-155">Devuelve el texto del componente que se va a procesar.</span><span class="sxs-lookup"><span data-stu-id="099ec-155">Returns the text to the component for processing.</span></span>
* <span data-ttu-id="099ec-156">Llama a una segunda función de JavaScript que interactúa con el DOM para mostrar un mensaje de bienvenida.</span><span class="sxs-lookup"><span data-stu-id="099ec-156">Calls a second JavaScript function that interacts with the DOM to display a welcome message.</span></span>

<span data-ttu-id="099ec-157">*Pages/JSInterop. Razor*:</span><span class="sxs-lookup"><span data-stu-id="099ec-157">*Pages/JSInterop.razor*:</span></span>

```razor
@page "/JSInterop"
@using BlazorSample.JsInteropClasses
@inject IJSRuntime JSRuntime

<h1>JavaScript Interop</h1>

<h2>Invoke JavaScript functions from .NET methods</h2>

<button type="button" class="btn btn-primary" @onclick="TriggerJsPrompt">
    Trigger JavaScript Prompt
</button>

<h3 id="welcome" style="color:green;font-style:italic"></h3>

@code {
    public async Task TriggerJsPrompt()
    {
        // showPrompt is implemented in wwwroot/exampleJsInterop.js
        var name = await JSRuntime.InvokeAsync<string>(
                "exampleJsFunctions.showPrompt",
                "What's your name?");
        // displayWelcome is implemented in wwwroot/exampleJsInterop.js
        await JSRuntime.InvokeVoidAsync(
                "exampleJsFunctions.displayWelcome",
                $"Hello {name}! Welcome to Blazor!");
    }
}
```

1. <span data-ttu-id="099ec-158">Cuando se ejecuta `TriggerJsPrompt` seleccionando el botón **desencadenador de JavaScript** del componente, se llama a la función de JavaScript `showPrompt` proporcionada en el archivo *wwwroot/exampleJsInterop. js* .</span><span class="sxs-lookup"><span data-stu-id="099ec-158">When `TriggerJsPrompt` is executed by selecting the component's **Trigger JavaScript Prompt** button, the JavaScript `showPrompt` function provided in the *wwwroot/exampleJsInterop.js* file is called.</span></span>
1. <span data-ttu-id="099ec-159">La función `showPrompt` acepta datos proporcionados por el usuario (el nombre del usuario), que se codifica en HTML y se devuelve al componente.</span><span class="sxs-lookup"><span data-stu-id="099ec-159">The `showPrompt` function accepts user input (the user's name), which is HTML-encoded and returned to the component.</span></span> <span data-ttu-id="099ec-160">El componente almacena el nombre del usuario en una variable local, `name`.</span><span class="sxs-lookup"><span data-stu-id="099ec-160">The component stores the user's name in a local variable, `name`.</span></span>
1. <span data-ttu-id="099ec-161">La cadena almacenada en `name` se incorpora en un mensaje de bienvenida, que se pasa a una función de JavaScript, `displayWelcome`, que representa el mensaje de bienvenida en una etiqueta de encabezado.</span><span class="sxs-lookup"><span data-stu-id="099ec-161">The string stored in `name` is incorporated into a welcome message, which is passed to a JavaScript function, `displayWelcome`, which renders the welcome message into a heading tag.</span></span>

## <a name="call-a-void-javascript-function"></a><span data-ttu-id="099ec-162">Llamar a una función de JavaScript void</span><span class="sxs-lookup"><span data-stu-id="099ec-162">Call a void JavaScript function</span></span>

<span data-ttu-id="099ec-163">Las funciones de JavaScript que devuelven [void (0)/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void) o [undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) se llaman con `IJSRuntime.InvokeVoidAsync`.</span><span class="sxs-lookup"><span data-stu-id="099ec-163">JavaScript functions that return [void(0)/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void) or [undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) are called with `IJSRuntime.InvokeVoidAsync`.</span></span>

## <a name="detect-when-a-opno-locblazor-app-is-prerendering"></a><span data-ttu-id="099ec-164">Detección cuando una aplicación Blazor se está preprocesando</span><span class="sxs-lookup"><span data-stu-id="099ec-164">Detect when a Blazor app is prerendering</span></span>
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

## <a name="capture-references-to-elements"></a><span data-ttu-id="099ec-165">Capturar referencias a elementos</span><span class="sxs-lookup"><span data-stu-id="099ec-165">Capture references to elements</span></span>

<span data-ttu-id="099ec-166">Algunos escenarios de interoperabilidad de JS requieren referencias a elementos HTML.</span><span class="sxs-lookup"><span data-stu-id="099ec-166">Some JS interop scenarios require references to HTML elements.</span></span> <span data-ttu-id="099ec-167">Por ejemplo, una biblioteca de interfaz de usuario puede requerir una referencia de elemento para la inicialización, o puede que necesite llamar a las API de tipo de comando en un elemento, como `focus` o `play`.</span><span class="sxs-lookup"><span data-stu-id="099ec-167">For example, a UI library may require an element reference for initialization, or you might need to call command-like APIs on an element, such as `focus` or `play`.</span></span>

<span data-ttu-id="099ec-168">Capture las referencias a los elementos HTML de un componente con el siguiente enfoque:</span><span class="sxs-lookup"><span data-stu-id="099ec-168">Capture references to HTML elements in a component using the following approach:</span></span>

* <span data-ttu-id="099ec-169">Agregue un atributo `@ref` al elemento HTML.</span><span class="sxs-lookup"><span data-stu-id="099ec-169">Add an `@ref` attribute to the HTML element.</span></span>
* <span data-ttu-id="099ec-170">Defina un campo de tipo `ElementReference` cuyo nombre coincida con el valor del atributo `@ref`.</span><span class="sxs-lookup"><span data-stu-id="099ec-170">Define a field of type `ElementReference` whose name matches the value of the `@ref` attribute.</span></span>

<span data-ttu-id="099ec-171">En el ejemplo siguiente se muestra la captura de una referencia al elemento `username` `<input>`:</span><span class="sxs-lookup"><span data-stu-id="099ec-171">The following example shows capturing a reference to the `username` `<input>` element:</span></span>

```razor
<input @ref="username" ... />

@code {
    ElementReference username;
}
```

> [!WARNING]
> <span data-ttu-id="099ec-172">Use solo una referencia de elemento para mutar el contenido de un elemento vacío que no interactúe con Blazor.</span><span class="sxs-lookup"><span data-stu-id="099ec-172">Only use an element reference to mutate the contents of an empty element that doesn't interact with Blazor.</span></span> <span data-ttu-id="099ec-173">Este escenario es útil cuando una API de terceros proporciona contenido al elemento.</span><span class="sxs-lookup"><span data-stu-id="099ec-173">This scenario is useful when a 3rd party API supplies content to the element.</span></span> <span data-ttu-id="099ec-174">Dado que Blazor no interactúa con el elemento, no existe ninguna posibilidad de que se produzca un conflicto entre la representación de Blazordel elemento y el DOM.</span><span class="sxs-lookup"><span data-stu-id="099ec-174">Because Blazor doesn't interact with the element, there's no possibility of a conflict between Blazor's representation of the element and the DOM.</span></span>
>
> <span data-ttu-id="099ec-175">En el ejemplo siguiente, es *peligroso* mutar el contenido de la lista sin ordenar (`ul`) porque Blazor interactúa con el Dom para rellenar los elementos de lista de este elemento (`<li>`):</span><span class="sxs-lookup"><span data-stu-id="099ec-175">In the following example, it's *dangerous* to mutate the contents of the unordered list (`ul`) because Blazor interacts with the DOM to populate this element's list items (`<li>`):</span></span>
>
> ```razor
> <ul ref="MyList">
>     @foreach (var item in Todos)
>     {
>         <li>@item.Text</li>
>     }
> </ul>
> ```
>
> <span data-ttu-id="099ec-176">Si la interoperabilidad de JS cambia el contenido de los `MyList` de elemento y Blazor intenta aplicar las diferencias al elemento, las diferencias no coincidirán con el DOM.</span><span class="sxs-lookup"><span data-stu-id="099ec-176">If JS interop mutates the contents of element `MyList` and Blazor attempts to apply diffs to the element, the diffs won't match the DOM.</span></span>

<span data-ttu-id="099ec-177">En lo que respecta al código .NET, una `ElementReference` es un identificador opaco.</span><span class="sxs-lookup"><span data-stu-id="099ec-177">As far as .NET code is concerned, an `ElementReference` is an opaque handle.</span></span> <span data-ttu-id="099ec-178">Lo *único* que puede hacer con `ElementReference` es pasarlo a código JavaScript a través de la interoperabilidad de JS.</span><span class="sxs-lookup"><span data-stu-id="099ec-178">The *only* thing you can do with `ElementReference` is pass it through to JavaScript code via JS interop.</span></span> <span data-ttu-id="099ec-179">Al hacerlo, el código de JavaScript recibe una instancia de `HTMLElement`, que puede usar con las API de DOM normales.</span><span class="sxs-lookup"><span data-stu-id="099ec-179">When you do so, the JavaScript-side code receives an `HTMLElement` instance, which it can use with normal DOM APIs.</span></span>

<span data-ttu-id="099ec-180">Por ejemplo, el código siguiente define un método de extensión de .NET que permite establecer el foco en un elemento:</span><span class="sxs-lookup"><span data-stu-id="099ec-180">For example, the following code defines a .NET extension method that enables setting the focus on an element:</span></span>

<span data-ttu-id="099ec-181">*exampleJsInterop. js*:</span><span class="sxs-lookup"><span data-stu-id="099ec-181">*exampleJsInterop.js*:</span></span>

```javascript
window.exampleJsFunctions = {
  focusElement : function (element) {
    element.focus();
  }
}
```

<span data-ttu-id="099ec-182">Para llamar a una función de JavaScript que no devuelve un valor, use `IJSRuntime.InvokeVoidAsync`.</span><span class="sxs-lookup"><span data-stu-id="099ec-182">To call a JavaScript function that doesn't return a value, use `IJSRuntime.InvokeVoidAsync`.</span></span> <span data-ttu-id="099ec-183">El código siguiente establece el foco en la entrada de nombre de usuario mediante una llamada a la función de JavaScript anterior con el `ElementReference`capturado:</span><span class="sxs-lookup"><span data-stu-id="099ec-183">The following code sets the focus on the username input by calling the preceding JavaScript function with the captured `ElementReference`:</span></span>

[!code-razor[](javascript-interop/samples_snapshot/component1.razor?highlight=1,3,11-12)]

<span data-ttu-id="099ec-184">Para usar un método de extensión, cree un método de extensión estático que reciba la `IJSRuntime` instancia:</span><span class="sxs-lookup"><span data-stu-id="099ec-184">To use an extension method, create a static extension method that receives the `IJSRuntime` instance:</span></span>

```csharp
public static async Task Focus(this ElementReference elementRef, IJSRuntime jsRuntime)
{
    await jsRuntime.InvokeVoidAsync(
        "exampleJsFunctions.focusElement", elementRef);
}
```

<span data-ttu-id="099ec-185">Se llama al método `Focus` directamente en el objeto.</span><span class="sxs-lookup"><span data-stu-id="099ec-185">The `Focus` method is called directly on the object.</span></span> <span data-ttu-id="099ec-186">En el ejemplo siguiente se da por supuesto que el método `Focus` está disponible en el espacio de nombres `JsInteropClasses`:</span><span class="sxs-lookup"><span data-stu-id="099ec-186">The following example assumes that the `Focus` method is available from the `JsInteropClasses` namespace:</span></span>

[!code-razor[](javascript-interop/samples_snapshot/component2.razor?highlight=1-4,12)]

> [!IMPORTANT]
> <span data-ttu-id="099ec-187">La variable `username` solo se rellena después de representar el componente.</span><span class="sxs-lookup"><span data-stu-id="099ec-187">The `username` variable is only populated after the component is rendered.</span></span> <span data-ttu-id="099ec-188">Si se pasa un `ElementReference` sin rellenar al código JavaScript, el código JavaScript recibe un valor de `null`.</span><span class="sxs-lookup"><span data-stu-id="099ec-188">If an unpopulated `ElementReference` is passed to JavaScript code, the JavaScript code receives a value of `null`.</span></span> <span data-ttu-id="099ec-189">Para manipular las referencias de elemento una vez finalizada la representación del componente (para establecer el foco inicial en un elemento), use los [métodos de ciclo de vida del componente OnAfterRenderAsync o OnAfterRender](xref:blazor/lifecycle#after-component-render).</span><span class="sxs-lookup"><span data-stu-id="099ec-189">To manipulate element references after the component has finished rendering (to set the initial focus on an element) use the [OnAfterRenderAsync or OnAfterRender component lifecycle methods](xref:blazor/lifecycle#after-component-render).</span></span>

<span data-ttu-id="099ec-190">Al trabajar con tipos genéricos y devolver un valor, use [ValueTask\<t >](xref:System.Threading.Tasks.ValueTask`1):</span><span class="sxs-lookup"><span data-stu-id="099ec-190">When working with generic types and returning a value, use [ValueTask\<T>](xref:System.Threading.Tasks.ValueTask`1):</span></span>

```csharp
public static ValueTask<T> GenericMethod<T>(this ElementReference elementRef, 
    IJSRuntime jsRuntime)
{
    return jsRuntime.InvokeAsync<T>(
        "exampleJsFunctions.doSomethingGeneric", elementRef);
}
```

<span data-ttu-id="099ec-191">se llama a `GenericMethod` directamente en el objeto con un tipo.</span><span class="sxs-lookup"><span data-stu-id="099ec-191">`GenericMethod` is called directly on the object with a type.</span></span> <span data-ttu-id="099ec-192">En el ejemplo siguiente se da por supuesto que el `GenericMethod` está disponible en el espacio de nombres `JsInteropClasses`:</span><span class="sxs-lookup"><span data-stu-id="099ec-192">The following example assumes that the `GenericMethod` is available from the `JsInteropClasses` namespace:</span></span>

[!code-razor[](javascript-interop/samples_snapshot/component3.razor?highlight=17)]

## <a name="reference-elements-across-components"></a><span data-ttu-id="099ec-193">Hacer referencia a elementos entre componentes</span><span class="sxs-lookup"><span data-stu-id="099ec-193">Reference elements across components</span></span>

<span data-ttu-id="099ec-194">Solo se garantiza que un `ElementReference` sea válido en el método de `OnAfterRender` de un componente (y una referencia de elemento es un `struct`), por lo que no se puede pasar una referencia de elemento entre los componentes.</span><span class="sxs-lookup"><span data-stu-id="099ec-194">An `ElementReference` is only guaranteed valid in a component's `OnAfterRender` method (and an element reference is a `struct`), so an element reference can't be passed between components.</span></span>

<span data-ttu-id="099ec-195">Para que un componente primario haga que una referencia de elemento esté disponible para otros componentes, el componente primario puede:</span><span class="sxs-lookup"><span data-stu-id="099ec-195">For a parent component to make an element reference available to other components, the parent component can:</span></span>

* <span data-ttu-id="099ec-196">Permite que los componentes secundarios registren las devoluciones de llamada.</span><span class="sxs-lookup"><span data-stu-id="099ec-196">Allow child components to register callbacks.</span></span>
* <span data-ttu-id="099ec-197">Invoca las devoluciones de llamada registradas durante el evento `OnAfterRender` con la referencia de elemento que se ha pasado.</span><span class="sxs-lookup"><span data-stu-id="099ec-197">Invoke the registered callbacks during the `OnAfterRender` event with the passed element reference.</span></span> <span data-ttu-id="099ec-198">De forma indirecta, este enfoque permite que los componentes secundarios interactúen con la referencia del elemento primario.</span><span class="sxs-lookup"><span data-stu-id="099ec-198">Indirectly, this approach allows child components to interact with the parent's element reference.</span></span>

<span data-ttu-id="099ec-199">En el siguiente ejemplo de webassembly de Blazor se muestra el enfoque.</span><span class="sxs-lookup"><span data-stu-id="099ec-199">The following Blazor WebAssembly example illustrates the approach.</span></span>

<span data-ttu-id="099ec-200">En el `<head>` de *wwwroot/index.html*:</span><span class="sxs-lookup"><span data-stu-id="099ec-200">In the `<head>` of *wwwroot/index.html*:</span></span>

```html
<style>
    .red { color: red }
</style>
```

<span data-ttu-id="099ec-201">En el `<body>` de *wwwroot/index.html*:</span><span class="sxs-lookup"><span data-stu-id="099ec-201">In the `<body>` of *wwwroot/index.html*:</span></span>

```html
<script>
    function setElementClass(element, className) {
        /** @type {HTMLElement} **/
        var myElement = element;
        myElement.classList.add(className);
    }
</script>
```

<span data-ttu-id="099ec-202">*Pages/index. Razor* (componente primario):</span><span class="sxs-lookup"><span data-stu-id="099ec-202">*Pages/Index.razor* (parent component):</span></span>

```razor
@page "/"

<h1 @ref="_title">Hello, world!</h1>

Welcome to your new app.

<SurveyPrompt Parent="this" Title="How is Blazor working for you?" />
```

<span data-ttu-id="099ec-203">*Pages/index. Razor. CS*:</span><span class="sxs-lookup"><span data-stu-id="099ec-203">*Pages/Index.razor.cs*:</span></span>

```csharp
using System;
using System.Collections.Generic;
using Microsoft.AspNetCore.Components;

namespace BlazorSample.Pages
{
    public partial class Index : 
        ComponentBase, IObservable<ElementReference>, IDisposable
    {
        private bool _disposing;
        private IList<IObserver<ElementReference>> _subscriptions = 
            new List<IObserver<ElementReference>>();
        private ElementReference _title;

        protected override void OnAfterRender(bool firstRender)
        {
            base.OnAfterRender(firstRender);

            foreach (var subscription in _subscriptions)
            {
                try
                {
                    subscription.OnNext(_title);
                }
                catch (Exception)
                {
                    throw;
                }
            }
        }

        public void Dispose()
        {
            _disposing = true;

            foreach (var subscription in _subscriptions)
            {
                try
                {
                    subscription.OnCompleted();
                }
                catch (Exception)
                {
                }
            }

            _subscriptions.Clear();
        }

        public IDisposable Subscribe(IObserver<ElementReference> observer)
        {
            if (_disposing)
            {
                throw new InvalidOperationException("Parent being disposed");
            }

            _subscriptions.Add(observer);

            return new Subscription(observer, this);
        }

        private class Subscription : IDisposable
        {
            public Subscription(IObserver<ElementReference> observer, Index self)
            {
                Observer = observer;
                Self = self;
            }

            public IObserver<ElementReference> Observer { get; }
            public Index Self { get; }

            public void Dispose()
            {
                Self._subscriptions.Remove(Observer);
            }
        }
    }
}
```

<span data-ttu-id="099ec-204">*Shared/SurveyPrompt. Razor* (componente secundario):</span><span class="sxs-lookup"><span data-stu-id="099ec-204">*Shared/SurveyPrompt.razor* (child component):</span></span>

```razor
@inject IJSRuntime JS

<div class="alert alert-secondary mt-4" role="alert">
    <span class="oi oi-pencil mr-2" aria-hidden="true"></span>
    <strong>@Title</strong>

    <span class="text-nowrap">
        Please take our
        <a target="_blank" class="font-weight-bold" 
            href="https://go.microsoft.com/fwlink/?linkid=2109206">brief survey</a>
    </span>
    and tell us what you think.
</div>

@code {
    [Parameter]
    public string Title { get; set; }
}
```

<span data-ttu-id="099ec-205">*Shared/SurveyPrompt. Razor. CS*:</span><span class="sxs-lookup"><span data-stu-id="099ec-205">*Shared/SurveyPrompt.razor.cs*:</span></span>

```csharp
using System;
using Microsoft.AspNetCore.Components;

namespace BlazorSample.Shared
{
    public partial class SurveyPrompt : 
        ComponentBase, IObserver<ElementReference>, IDisposable
    {
        private IDisposable _subscription = null;

        [Parameter]
        public IObservable<ElementReference> Parent { get; set; }

        protected override void OnParametersSet()
        {
            base.OnParametersSet();

            if (_subscription != null)
            {
                _subscription.Dispose();
            }

            _subscription = Parent.Subscribe(this);
        }

        public void OnCompleted()
        {
            _subscription = null;
        }

        public void OnError(Exception error)
        {
            _subscription = null;
        }

        public void OnNext(ElementReference value)
        {
            JS.InvokeAsync<object>(
                "setElementClass", new object[] { value, "red" });
        }

        public void Dispose()
        {
            _subscription?.Dispose();
        }
    }
}
```

## <a name="invoke-net-methods-from-javascript-functions"></a><span data-ttu-id="099ec-206">Invocar métodos .NET desde funciones de JavaScript</span><span class="sxs-lookup"><span data-stu-id="099ec-206">Invoke .NET methods from JavaScript functions</span></span>

### <a name="static-net-method-call"></a><span data-ttu-id="099ec-207">Llamada al método estático de .NET</span><span class="sxs-lookup"><span data-stu-id="099ec-207">Static .NET method call</span></span>

<span data-ttu-id="099ec-208">Para invocar un método .NET estático desde JavaScript, use las funciones `DotNet.invokeMethod` o `DotNet.invokeMethodAsync`.</span><span class="sxs-lookup"><span data-stu-id="099ec-208">To invoke a static .NET method from JavaScript, use the `DotNet.invokeMethod` or `DotNet.invokeMethodAsync` functions.</span></span> <span data-ttu-id="099ec-209">Pase el identificador del método estático al que desea llamar, el nombre del ensamblado que contiene la función y cualquier argumento.</span><span class="sxs-lookup"><span data-stu-id="099ec-209">Pass in the identifier of the static method you wish to call, the name of the assembly containing the function, and any arguments.</span></span> <span data-ttu-id="099ec-210">La versión asincrónica es preferible para admitir escenarios de Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="099ec-210">The asynchronous version is preferred to support Blazor Server scenarios.</span></span> <span data-ttu-id="099ec-211">El método .NET debe ser público, estático y tener el atributo `[JSInvokable]`.</span><span class="sxs-lookup"><span data-stu-id="099ec-211">The .NET method must be public, static, and have the `[JSInvokable]` attribute.</span></span> <span data-ttu-id="099ec-212">Actualmente no se admite la llamada a métodos genéricos abiertos.</span><span class="sxs-lookup"><span data-stu-id="099ec-212">Calling open generic methods isn't currently supported.</span></span>

<span data-ttu-id="099ec-213">La aplicación de ejemplo incluye C# un método para devolver una matriz de `int`s.</span><span class="sxs-lookup"><span data-stu-id="099ec-213">The sample app includes a C# method to return an array of `int`s.</span></span> <span data-ttu-id="099ec-214">El atributo `JSInvokable` se aplica al método.</span><span class="sxs-lookup"><span data-stu-id="099ec-214">The `JSInvokable` attribute is applied to the method.</span></span>

<span data-ttu-id="099ec-215">*Pages/JsInterop. Razor*:</span><span class="sxs-lookup"><span data-stu-id="099ec-215">*Pages/JsInterop.razor*:</span></span>

```razor
<button type="button" class="btn btn-primary"
        onclick="exampleJsFunctions.returnArrayAsyncJs()">
    Trigger .NET static method ReturnArrayAsync
</button>

@code {
    [JSInvokable]
    public static Task<int[]> ReturnArrayAsync()
    {
        return Task.FromResult(new int[] { 1, 2, 3 });
    }
}
```

<span data-ttu-id="099ec-216">JavaScript atendido al cliente invoca el C# método .net.</span><span class="sxs-lookup"><span data-stu-id="099ec-216">JavaScript served to the client invokes the C# .NET method.</span></span>

<span data-ttu-id="099ec-217">*wwwroot/exampleJsInterop. js*:</span><span class="sxs-lookup"><span data-stu-id="099ec-217">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=8-14)]

<span data-ttu-id="099ec-218">Cuando se seleccione el botón **desencadenador de método estático de .net ReturnArrayAsync** , examine la salida de la consola en las herramientas de desarrollo web del explorador.</span><span class="sxs-lookup"><span data-stu-id="099ec-218">When the **Trigger .NET static method ReturnArrayAsync** button is selected, examine the console output in the browser's web developer tools.</span></span>

<span data-ttu-id="099ec-219">La salida de la consola es:</span><span class="sxs-lookup"><span data-stu-id="099ec-219">The console output is:</span></span>

```console
Array(4) [ 1, 2, 3, 4 ]
```

<span data-ttu-id="099ec-220">El cuarto valor de matriz se inserta en la matriz (`data.push(4);`) devuelta por `ReturnArrayAsync`.</span><span class="sxs-lookup"><span data-stu-id="099ec-220">The fourth array value is pushed to the array (`data.push(4);`) returned by `ReturnArrayAsync`.</span></span>

<span data-ttu-id="099ec-221">De forma predeterminada, el identificador del método es el nombre del método, pero puede especificar un identificador diferente mediante el constructor `JSInvokableAttribute`:</span><span class="sxs-lookup"><span data-stu-id="099ec-221">By default, the method identifier is the method name, but you can specify a different identifier using the `JSInvokableAttribute` constructor:</span></span>

```csharp
@code {
    [JSInvokable("DifferentMethodName")]
    public static Task<int[]> ReturnArrayAsync()
    {
        return Task.FromResult(new int[] { 1, 2, 3 });
    }
}
```

<span data-ttu-id="099ec-222">En el archivo JavaScript del lado cliente:</span><span class="sxs-lookup"><span data-stu-id="099ec-222">In the client-side JavaScript file:</span></span>

```javascript
returnArrayAsyncJs: function () {
  DotNet.invokeMethodAsync('BlazorSample', 'DifferentMethodName')
    .then(data => {
      data.push(4);
      console.log(data);
    });
}
```

### <a name="instance-method-call"></a><span data-ttu-id="099ec-223">Llamada al método de instancia</span><span class="sxs-lookup"><span data-stu-id="099ec-223">Instance method call</span></span>

<span data-ttu-id="099ec-224">También puede llamar a métodos de instancia de .NET desde JavaScript.</span><span class="sxs-lookup"><span data-stu-id="099ec-224">You can also call .NET instance methods from JavaScript.</span></span> <span data-ttu-id="099ec-225">Para invocar un método de instancia de .NET desde JavaScript:</span><span class="sxs-lookup"><span data-stu-id="099ec-225">To invoke a .NET instance method from JavaScript:</span></span>

* <span data-ttu-id="099ec-226">Pase la instancia de .NET a JavaScript ajustándola en una instancia de `DotNetObjectReference`.</span><span class="sxs-lookup"><span data-stu-id="099ec-226">Pass the .NET instance to JavaScript by wrapping it in a `DotNetObjectReference` instance.</span></span> <span data-ttu-id="099ec-227">La instancia de .NET se pasa por referencia a JavaScript.</span><span class="sxs-lookup"><span data-stu-id="099ec-227">The .NET instance is passed by reference to JavaScript.</span></span>
* <span data-ttu-id="099ec-228">Invocar métodos de instancia de .NET en la instancia de mediante las funciones `invokeMethod` o `invokeMethodAsync`.</span><span class="sxs-lookup"><span data-stu-id="099ec-228">Invoke .NET instance methods on the instance using the `invokeMethod` or `invokeMethodAsync` functions.</span></span> <span data-ttu-id="099ec-229">La instancia de .NET también se puede pasar como argumento al invocar otros métodos .NET desde JavaScript.</span><span class="sxs-lookup"><span data-stu-id="099ec-229">The .NET instance can also be passed as an argument when invoking other .NET methods from JavaScript.</span></span>

> [!NOTE]
> <span data-ttu-id="099ec-230">La aplicación de ejemplo registra mensajes en la consola del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="099ec-230">The sample app logs messages to the client-side console.</span></span> <span data-ttu-id="099ec-231">En los siguientes ejemplos mostrados por la aplicación de ejemplo, examine la salida de la consola del explorador en las herramientas de desarrollo del explorador.</span><span class="sxs-lookup"><span data-stu-id="099ec-231">For the following examples demonstrated by the sample app, examine the browser's console output in the browser's developer tools.</span></span>

<span data-ttu-id="099ec-232">Cuando se selecciona el botón **desencadenador de instancia de .net HelloHelper. SayHello** , se llama a `ExampleJsInterop.CallHelloHelperSayHello` y pasa un nombre, `Blazor`, al método.</span><span class="sxs-lookup"><span data-stu-id="099ec-232">When the **Trigger .NET instance method HelloHelper.SayHello** button is selected, `ExampleJsInterop.CallHelloHelperSayHello` is called and passes a name, `Blazor`, to the method.</span></span>

<span data-ttu-id="099ec-233">*Pages/JsInterop. Razor*:</span><span class="sxs-lookup"><span data-stu-id="099ec-233">*Pages/JsInterop.razor*:</span></span>

```razor
<button type="button" class="btn btn-primary" @onclick="TriggerNetInstanceMethod">
    Trigger .NET instance method HelloHelper.SayHello
</button>

@code {
    public async Task TriggerNetInstanceMethod()
    {
        var exampleJsInterop = new ExampleJsInterop(JSRuntime);
        await exampleJsInterop.CallHelloHelperSayHello("Blazor");
    }
}
```

<span data-ttu-id="099ec-234">`CallHelloHelperSayHello` invoca la función de JavaScript `sayHello` con una nueva instancia de `HelloHelper`.</span><span class="sxs-lookup"><span data-stu-id="099ec-234">`CallHelloHelperSayHello` invokes the JavaScript function `sayHello` with a new instance of `HelloHelper`.</span></span>

<span data-ttu-id="099ec-235">*JsInteropClasses/ExampleJsInterop. CS*:</span><span class="sxs-lookup"><span data-stu-id="099ec-235">*JsInteropClasses/ExampleJsInterop.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=10-16)]

<span data-ttu-id="099ec-236">*wwwroot/exampleJsInterop. js*:</span><span class="sxs-lookup"><span data-stu-id="099ec-236">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=15-18)]

<span data-ttu-id="099ec-237">El nombre se pasa al constructor de `HelloHelper`, que establece la propiedad `HelloHelper.Name`.</span><span class="sxs-lookup"><span data-stu-id="099ec-237">The name is passed to `HelloHelper`'s constructor, which sets the `HelloHelper.Name` property.</span></span> <span data-ttu-id="099ec-238">Cuando se ejecuta la función de JavaScript `sayHello`, `HelloHelper.SayHello` devuelve el mensaje `Hello, {Name}!`, que la función JavaScript escribe en la consola.</span><span class="sxs-lookup"><span data-stu-id="099ec-238">When the JavaScript function `sayHello` is executed, `HelloHelper.SayHello` returns the `Hello, {Name}!` message, which is written to the console by the JavaScript function.</span></span>

<span data-ttu-id="099ec-239">*JsInteropClasses/HelloHelper. CS*:</span><span class="sxs-lookup"><span data-stu-id="099ec-239">*JsInteropClasses/HelloHelper.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

<span data-ttu-id="099ec-240">Salida de la consola en las herramientas de desarrollo web del explorador:</span><span class="sxs-lookup"><span data-stu-id="099ec-240">Console output in the browser's web developer tools:</span></span>

```console
Hello, Blazor!
```

## <a name="share-interop-code-in-a-class-library"></a><span data-ttu-id="099ec-241">Compartir código de interoperabilidad en una biblioteca de clases</span><span class="sxs-lookup"><span data-stu-id="099ec-241">Share interop code in a class library</span></span>

<span data-ttu-id="099ec-242">El código de interoperabilidad de JS se puede incluir en una biblioteca de clases, lo que permite compartir el código en un paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="099ec-242">JS interop code can be included in a class library, which allows you to share the code in a NuGet package.</span></span>

<span data-ttu-id="099ec-243">La biblioteca de clases controla la incrustación de recursos de JavaScript en el ensamblado compilado.</span><span class="sxs-lookup"><span data-stu-id="099ec-243">The class library handles embedding JavaScript resources in the built assembly.</span></span> <span data-ttu-id="099ec-244">Los archivos de JavaScript se colocan en la carpeta *wwwroot* .</span><span class="sxs-lookup"><span data-stu-id="099ec-244">The JavaScript files are placed in the *wwwroot* folder.</span></span> <span data-ttu-id="099ec-245">Las herramientas se encargan de insertar los recursos cuando se compila la biblioteca.</span><span class="sxs-lookup"><span data-stu-id="099ec-245">The tooling takes care of embedding the resources when the library is built.</span></span>

<span data-ttu-id="099ec-246">Se hace referencia al paquete de NuGet compilado en el archivo de proyecto de la aplicación de la misma manera que se hace referencia a cualquier paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="099ec-246">The built NuGet package is referenced in the app's project file the same way that any NuGet package is referenced.</span></span> <span data-ttu-id="099ec-247">Una vez restaurado el paquete, el código de la aplicación puede llamar a JavaScript C#como si fuera.</span><span class="sxs-lookup"><span data-stu-id="099ec-247">After the package is restored, app code can call into JavaScript as if it were C#.</span></span>

<span data-ttu-id="099ec-248">Para más información, consulte <xref:blazor/class-libraries>.</span><span class="sxs-lookup"><span data-stu-id="099ec-248">For more information, see <xref:blazor/class-libraries>.</span></span>

## <a name="harden-js-interop-calls"></a><span data-ttu-id="099ec-249">Reprotección de llamadas de interoperabilidad de JS</span><span class="sxs-lookup"><span data-stu-id="099ec-249">Harden JS interop calls</span></span>

<span data-ttu-id="099ec-250">La interoperabilidad de JS puede producir un error debido a errores de red y debe tratarse como no confiable.</span><span class="sxs-lookup"><span data-stu-id="099ec-250">JS interop may fail due to networking errors and should be treated as unreliable.</span></span> <span data-ttu-id="099ec-251">De forma predeterminada, una aplicación de Blazor Server agota el tiempo de espera para las llamadas de interoperabilidad de JS en el servidor después de un minuto.</span><span class="sxs-lookup"><span data-stu-id="099ec-251">By default, a Blazor Server app times out JS interop calls on the server after one minute.</span></span> <span data-ttu-id="099ec-252">Si una aplicación puede tolerar un tiempo de espera más agresivo, como 10 segundos, establezca el tiempo de espera con uno de los siguientes métodos:</span><span class="sxs-lookup"><span data-stu-id="099ec-252">If an app can tolerate a more aggressive timeout, such as 10 seconds, set the timeout using one of the following approaches:</span></span>

* <span data-ttu-id="099ec-253">Globalmente en `Startup.ConfigureServices`, especifique el tiempo de espera:</span><span class="sxs-lookup"><span data-stu-id="099ec-253">Globally in `Startup.ConfigureServices`, specify the timeout:</span></span>

  ```csharp
  services.AddServerSideBlazor(
      options => options.JSInteropDefaultCallTimeout = TimeSpan.FromSeconds({SECONDS}));
  ```

* <span data-ttu-id="099ec-254">Por cada invocación en el código de componente, una sola llamada puede especificar el tiempo de espera:</span><span class="sxs-lookup"><span data-stu-id="099ec-254">Per-invocation in component code, a single call can specify the timeout:</span></span>

  ```csharp
  var result = await JSRuntime.InvokeAsync<string>("MyJSOperation", 
      TimeSpan.FromSeconds({SECONDS}), new[] { "Arg1" });
  ```

<span data-ttu-id="099ec-255">Para obtener más información sobre el agotamiento de recursos, consulte <xref:security/blazor/server>.</span><span class="sxs-lookup"><span data-stu-id="099ec-255">For more information on resource exhaustion, see <xref:security/blazor/server>.</span></span>

## <a name="perform-large-data-transfers-in-opno-locblazor-server-apps"></a><span data-ttu-id="099ec-256">Realizar transferencias de datos de gran tamaño en aplicaciones de Blazor Server</span><span class="sxs-lookup"><span data-stu-id="099ec-256">Perform large data transfers in Blazor Server apps</span></span>

<span data-ttu-id="099ec-257">En algunos escenarios, se deben transferir grandes cantidades de datos entre JavaScript y Blazor.</span><span class="sxs-lookup"><span data-stu-id="099ec-257">In some scenarios, large amounts of data must be transferred between JavaScript and Blazor.</span></span> <span data-ttu-id="099ec-258">Normalmente, se producen grandes transferencias de datos cuando:</span><span class="sxs-lookup"><span data-stu-id="099ec-258">Typically, large data transfers occur when:</span></span>

* <span data-ttu-id="099ec-259">Las API del sistema de archivos del explorador se usan para cargar o descargar un archivo.</span><span class="sxs-lookup"><span data-stu-id="099ec-259">Browser file system APIs are used to upload or download a file.</span></span>
* <span data-ttu-id="099ec-260">Se requiere la interoperabilidad con una biblioteca de terceros.</span><span class="sxs-lookup"><span data-stu-id="099ec-260">Interop with a third party library is required.</span></span>

<span data-ttu-id="099ec-261">En Blazor Server, hay una limitación para evitar el paso de mensajes de gran tamaño que pueden dar lugar a problemas de rendimiento.</span><span class="sxs-lookup"><span data-stu-id="099ec-261">In Blazor Server, a limitation is in place to prevent passing single large messages that may result in performance issues.</span></span>

<span data-ttu-id="099ec-262">Tenga en cuenta las siguientes instrucciones al desarrollar código que transfiera datos entre JavaScript y Blazor:</span><span class="sxs-lookup"><span data-stu-id="099ec-262">Consider the following guidance when developing code that transfers data between JavaScript and Blazor:</span></span>

* <span data-ttu-id="099ec-263">Segmente los datos en partes más pequeñas y envíe los segmentos de datos secuencialmente hasta que el servidor reciba todos los datos.</span><span class="sxs-lookup"><span data-stu-id="099ec-263">Slice the data into smaller pieces, and send the data segments sequentially until all of the data is received by the server.</span></span>
* <span data-ttu-id="099ec-264">No asigne objetos grandes en JavaScript y C# código.</span><span class="sxs-lookup"><span data-stu-id="099ec-264">Don't allocate large objects in JavaScript and C# code.</span></span>
* <span data-ttu-id="099ec-265">No bloquee el subproceso de interfaz de usuario principal durante períodos largos al enviar o recibir datos.</span><span class="sxs-lookup"><span data-stu-id="099ec-265">Don't block the main UI thread for long periods when sending or receiving data.</span></span>
* <span data-ttu-id="099ec-266">Libere la memoria consumida al completar o cancelar el proceso.</span><span class="sxs-lookup"><span data-stu-id="099ec-266">Free any memory consumed when the process is completed or cancelled.</span></span>
* <span data-ttu-id="099ec-267">Aplique los siguientes requisitos adicionales por motivos de seguridad:</span><span class="sxs-lookup"><span data-stu-id="099ec-267">Enforce the following additional requirements for security purposes:</span></span>
  * <span data-ttu-id="099ec-268">Declare el tamaño máximo de archivo o de datos que se puede pasar.</span><span class="sxs-lookup"><span data-stu-id="099ec-268">Declare the maximum file or data size that can be passed.</span></span>
  * <span data-ttu-id="099ec-269">Declare la tasa mínima de carga desde el cliente al servidor.</span><span class="sxs-lookup"><span data-stu-id="099ec-269">Declare the minimum upload rate from the client to the server.</span></span>
* <span data-ttu-id="099ec-270">Después de que el servidor reciba los datos, los datos pueden ser:</span><span class="sxs-lookup"><span data-stu-id="099ec-270">After the data is received by the server, the data can be:</span></span>
  * <span data-ttu-id="099ec-271">Se almacena temporalmente en un búfer de memoria hasta que se recopilan todos los segmentos.</span><span class="sxs-lookup"><span data-stu-id="099ec-271">Temporarily stored in a memory buffer until all of the segments are collected.</span></span>
  * <span data-ttu-id="099ec-272">Consumido inmediatamente.</span><span class="sxs-lookup"><span data-stu-id="099ec-272">Consumed immediately.</span></span> <span data-ttu-id="099ec-273">Por ejemplo, los datos se pueden almacenar inmediatamente en una base de datos o escribirse en el disco cuando se recibe cada segmento.</span><span class="sxs-lookup"><span data-stu-id="099ec-273">For example, the data can be stored immediately in a database or written to disk as each segment is received.</span></span>

<span data-ttu-id="099ec-274">La siguiente clase de cargador de archivos controla la interoperabilidad de JS con el cliente.</span><span class="sxs-lookup"><span data-stu-id="099ec-274">The following file uploader class handles JS interop with the client.</span></span> <span data-ttu-id="099ec-275">La clase de cargador usa la interoperabilidad de JS para:</span><span class="sxs-lookup"><span data-stu-id="099ec-275">The uploader class uses JS interop to:</span></span>

* <span data-ttu-id="099ec-276">Sondear el cliente para enviar un segmento de datos.</span><span class="sxs-lookup"><span data-stu-id="099ec-276">Poll the client to send a data segment.</span></span>
* <span data-ttu-id="099ec-277">Anule la transacción si se agota el tiempo de espera del sondeo.</span><span class="sxs-lookup"><span data-stu-id="099ec-277">Abort the transaction if polling times out.</span></span>

```csharp
using System;
using System.Buffers;
using System.Collections.Generic;
using System.IO;
using System.Threading.Tasks;
using Microsoft.JSInterop;

public class FileUploader : IDisposable
{
    private readonly IJSRuntime _jsRuntime;
    private readonly int _segmentSize = 6144;
    private readonly int _maxBase64SegmentSize = 8192;
    private readonly DotNetObjectReference<FileUploader> _thisReference;
    private List<IMemoryOwner<byte>> _uploadedSegments = 
        new List<IMemoryOwner<byte>>();

    public FileUploader(IJSRuntime jsRuntime)
    {
        _jsRuntime = jsRuntime;
    }

    public async Task<Stream> ReceiveFile(string selector, int maxSize)
    {
        var fileSize = 
            await _jsRuntime.InvokeAsync<int>("getFileSize", selector);

        if (fileSize > maxSize)
        {
            return null;
        }

        var numberOfSegments = Math.Floor(fileSize / (double)_segmentSize) + 1;
        var lastSegmentBytes = 0;
        string base64EncodedSegment;

        for (var i = 0; i < numberOfSegments; i++)
        {
            try
            {
                base64EncodedSegment = 
                    await _jsRuntime.InvokeAsync<string>(
                        "receiveSegment", i, selector);

                if (base64EncodedSegment.Length < _maxBase64SegmentSize && 
                    i < numberOfSegments - 1)
                {
                    return null;
                }
            }
            catch
            {
                return null;
            }

          var current = MemoryPool<byte>.Shared.Rent(_segmentSize);

          if (!Convert.TryFromBase64String(base64EncodedSegment, 
              current.Memory.Slice(0, _segmentSize).Span, out lastSegmentBytes))
          {
              return null;
          }

          _uploadedSegments.Add(current);
        }

        var segments = _uploadedSegments;
        _uploadedSegments = null;

        return new SegmentedStream(segments, _segmentSize, lastSegmentBytes);
    }

    public void Dispose()
    {
        if (_uploadedSegments != null)
        {
            foreach (var segment in _uploadedSegments)
            {
                segment.Dispose();
            }
        }
    }
}
```

<span data-ttu-id="099ec-278">En el ejemplo anterior:</span><span class="sxs-lookup"><span data-stu-id="099ec-278">In the preceding example:</span></span>

* <span data-ttu-id="099ec-279">La `_maxBase64SegmentSize` se establece en `8192`, que se calcula a partir de `_maxBase64SegmentSize = _segmentSize * 4 / 3`.</span><span class="sxs-lookup"><span data-stu-id="099ec-279">The `_maxBase64SegmentSize` is set to `8192`, which is calculated from `_maxBase64SegmentSize = _segmentSize * 4 / 3`.</span></span>
* <span data-ttu-id="099ec-280">Las API de bajo nivel de administración de memoria de .NET Core se usan para almacenar los segmentos de memoria en el servidor en `_uploadedSegments`.</span><span class="sxs-lookup"><span data-stu-id="099ec-280">Low level .NET Core memory management APIs are used to store the memory segments on the server in `_uploadedSegments`.</span></span>
* <span data-ttu-id="099ec-281">Se usa un método `ReceiveFile` para administrar la carga a través de la interoperabilidad JS:</span><span class="sxs-lookup"><span data-stu-id="099ec-281">A `ReceiveFile` method is used to handle the upload through JS interop:</span></span>
  * <span data-ttu-id="099ec-282">El tamaño del archivo se determina en bytes a través de la interoperabilidad de JS con `_jsRuntime.InvokeAsync<FileInfo>('getFileSize', selector)`.</span><span class="sxs-lookup"><span data-stu-id="099ec-282">The file size is determined in bytes through JS interop with `_jsRuntime.InvokeAsync<FileInfo>('getFileSize', selector)`.</span></span>
  * <span data-ttu-id="099ec-283">El número de segmentos que se van a recibir se calcula y se almacena en `numberOfSegments`.</span><span class="sxs-lookup"><span data-stu-id="099ec-283">The number of segments to receive are calculated and stored in `numberOfSegments`.</span></span>
  * <span data-ttu-id="099ec-284">Los segmentos se solicitan en un bucle `for` a través de la interoperabilidad de JS con `_jsRuntime.InvokeAsync<string>('receiveSegment', i, selector)`.</span><span class="sxs-lookup"><span data-stu-id="099ec-284">The segments are requested in a `for` loop through JS interop with `_jsRuntime.InvokeAsync<string>('receiveSegment', i, selector)`.</span></span> <span data-ttu-id="099ec-285">Todos los segmentos, pero el último debe ser 8.192 bytes antes de la descodificación.</span><span class="sxs-lookup"><span data-stu-id="099ec-285">All segments but the last must be 8,192 bytes before decoding.</span></span> <span data-ttu-id="099ec-286">El cliente se ve obligado a enviar los datos de una manera eficaz.</span><span class="sxs-lookup"><span data-stu-id="099ec-286">The client is forced to send the data in an efficient manner.</span></span>
  * <span data-ttu-id="099ec-287">Para cada segmento recibido, se realizan comprobaciones antes de descodificar con <xref:System.Convert.TryFromBase64String*>.</span><span class="sxs-lookup"><span data-stu-id="099ec-287">For each segment received, checks are performed before decoding with <xref:System.Convert.TryFromBase64String*>.</span></span>
  * <span data-ttu-id="099ec-288">Una secuencia con los datos se devuelve como un nuevo <xref:System.IO.Stream> (`SegmentedStream`) una vez completada la carga.</span><span class="sxs-lookup"><span data-stu-id="099ec-288">A stream with the data is returned as a new <xref:System.IO.Stream> (`SegmentedStream`) after the upload is complete.</span></span>

<span data-ttu-id="099ec-289">La clase de secuencia segmentada expone la lista de segmentos como un <xref:System.IO.Stream>no buscable de solo lectura:</span><span class="sxs-lookup"><span data-stu-id="099ec-289">The segmented stream class exposes the list of segments as a readonly non-seekable <xref:System.IO.Stream>:</span></span>

```csharp
using System;
using System.Buffers;
using System.Collections.Generic;
using System.IO;

public class SegmentedStream : Stream
{
    private readonly ReadOnlySequence<byte> _sequence;
    private long _currentPosition = 0;

    public SegmentedStream(IList<IMemoryOwner<byte>> segments, int segmentSize, 
        int lastSegmentSize)
    {
        if (segments.Count == 1)
        {
            _sequence = new ReadOnlySequence<byte>(
                segments[0].Memory.Slice(0, lastSegmentSize));
            return;
        }

        var sequenceSegment = new BufferSegment<byte>(
            segments[0].Memory.Slice(0, segmentSize));
        var lastSegment = sequenceSegment;

        for (int i = 1; i < segments.Count; i++)
        {
            var isLastSegment = i + 1 == segments.Count;
            lastSegment = lastSegment.Append(segments[i].Memory.Slice(
                0, isLastSegment ? lastSegmentSize : segmentSize));
        }

        _sequence = new ReadOnlySequence<byte>(
            sequenceSegment, 0, lastSegment, lastSegmentSize);
    }

    public override long Position
    {
        get => throw new NotImplementedException();
        set => throw new NotImplementedException();
    }

    public override int Read(byte[] buffer, int offset, int count)
    {
        var bytesToWrite = (int)(_currentPosition + count < _sequence.Length ? 
            count : _sequence.Length - _currentPosition);
        var data = _sequence.Slice(_currentPosition, bytesToWrite);
        data.CopyTo(buffer.AsSpan(offset, bytesToWrite));
        _currentPosition += bytesToWrite;

        return bytesToWrite;
    }

    private class BufferSegment<T> : ReadOnlySequenceSegment<T>
    {
        public BufferSegment(ReadOnlyMemory<T> memory)
        {
            Memory = memory;
        }

        public BufferSegment<T> Append(ReadOnlyMemory<T> memory)
        {
            var segment = new BufferSegment<T>(memory)
            {
                RunningIndex = RunningIndex + Memory.Length
            };

            Next = segment;

            return segment;
        }
    }

    public override bool CanRead => true;

    public override bool CanSeek => false;

    public override bool CanWrite => false;

    public override long Length => throw new NotImplementedException();

    public override void Flush() => throw new NotImplementedException();

    public override long Seek(long offset, SeekOrigin origin) => 
        throw new NotImplementedException();

    public override void SetLength(long value) => 
        throw new NotImplementedException();

    public override void Write(byte[] buffer, int offset, int count) => 
        throw new NotImplementedException();
}
```

<span data-ttu-id="099ec-290">En el código siguiente se implementan las funciones de JavaScript para recibir los datos:</span><span class="sxs-lookup"><span data-stu-id="099ec-290">The following code implements JavaScript functions to receive the data:</span></span>

```javascript
function getFileSize(selector) {
  const file = getFile(selector);
  return file.size;
}

async function receiveSegment(segmentNumber, selector) {
  const file = getFile(selector);
  var segments = getFileSegments(file);
  var index = segmentNumber * 6144;
  return await getNextChunk(file, index);
}

function getFile(selector) {
  const element = document.querySelector(selector);
  if (!element) {
    throw new Error('Invalid selector');
  }
  const files = element.files;
  if (!files || files.length === 0) {
    throw new Error(`Element ${elementId} doesn't contain any files.`);
  }
  const file = files[0];
  return file;
}

function getFileSegments(file) {
  const segments = Math.floor(size % 6144 === 0 ? size / 6144 : 1 + size / 6144);
  return segments;
}

async function getNextChunk(file, index) {
  const length = file.size - index <= 6144 ? file.size - index : 6144;
  const chunk = file.slice(index, index + length);
  index += length;
  const base64Chunk = await this.base64EncodeAsync(chunk);
  return { base64Chunk, index };
}

async function base64EncodeAsync(chunk) {
  const reader = new FileReader();
  const result = new Promise((resolve, reject) => {
    reader.addEventListener('load',
      () => {
        const base64Chunk = reader.result;
        const cleanChunk = 
          base64Chunk.replace('data:application/octet-stream;base64,', '');
        resolve(cleanChunk);
      },
      false);
    reader.addEventListener('error', reject);
  });
  reader.readAsDataURL(chunk);
  return result;
}
```

## <a name="additional-resources"></a><span data-ttu-id="099ec-291">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="099ec-291">Additional resources</span></span>

* [<span data-ttu-id="099ec-292">Ejemplo de InteropComponent. Razor (repositorio de GitHub dotnet/AspNetCore, rama de versión 3,1)</span><span class="sxs-lookup"><span data-stu-id="099ec-292">InteropComponent.razor example (dotnet/AspNetCore GitHub repository, 3.1 release branch)</span></span>](https://github.com/dotnet/AspNetCore/blob/release/3.1/src/Components/test/testassets/BasicTestApp/InteropComponent.razor)
