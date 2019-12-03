---
title: ASP.NET Core Blazor la interoperabilidad de JavaScript
author: guardrex
description: Obtenga información sobre cómo invocar funciones de JavaScript desde los métodos .NET y .NET desde JavaScript en Blazor aplicaciones.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/02/2019
no-loc:
- Blazor
uid: blazor/javascript-interop
ms.openlocfilehash: 108fdac8667f407adba3470de4eb8e35883cefbf
ms.sourcegitcommit: 169ea5116de729c803685725d96450a270bc55b7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2019
ms.locfileid: "74733835"
---
# <a name="aspnet-core-opno-locblazor-javascript-interop"></a><span data-ttu-id="c2e34-103">ASP.NET Core Blazor la interoperabilidad de JavaScript</span><span class="sxs-lookup"><span data-stu-id="c2e34-103">ASP.NET Core Blazor JavaScript interop</span></span>

<span data-ttu-id="c2e34-104">Por [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27)y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="c2e34-104">By [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="c2e34-105">Una aplicación Blazor puede invocar funciones de JavaScript desde los métodos .NET y .NET desde el código JavaScript.</span><span class="sxs-lookup"><span data-stu-id="c2e34-105">A Blazor app can invoke JavaScript functions from .NET and .NET methods from JavaScript code.</span></span>

<span data-ttu-id="c2e34-106">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c2e34-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="invoke-javascript-functions-from-net-methods"></a><span data-ttu-id="c2e34-107">Invocar funciones de JavaScript desde métodos de .NET</span><span class="sxs-lookup"><span data-stu-id="c2e34-107">Invoke JavaScript functions from .NET methods</span></span>

<span data-ttu-id="c2e34-108">Hay ocasiones en las que se requiere código .NET para llamar a una función de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="c2e34-108">There are times when .NET code is required to call a JavaScript function.</span></span> <span data-ttu-id="c2e34-109">Por ejemplo, una llamada de JavaScript puede exponer funciones o funcionalidades del explorador de una biblioteca de JavaScript a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c2e34-109">For example, a JavaScript call can expose browser capabilities or functionality from a JavaScript library to the app.</span></span> <span data-ttu-id="c2e34-110">Este escenario se denomina *interoperabilidad de JavaScript* (*interoperabilidad de JS*).</span><span class="sxs-lookup"><span data-stu-id="c2e34-110">This scenario is called *JavaScript interoperability* (*JS interop*).</span></span>

<span data-ttu-id="c2e34-111">Para llamar a JavaScript desde .NET, use la abstracción `IJSRuntime`.</span><span class="sxs-lookup"><span data-stu-id="c2e34-111">To call into JavaScript from .NET, use the `IJSRuntime` abstraction.</span></span> <span data-ttu-id="c2e34-112">El método `InvokeAsync<T>` toma un identificador de la función de JavaScript que se desea invocar junto con cualquier número de argumentos serializables de JSON.</span><span class="sxs-lookup"><span data-stu-id="c2e34-112">The `InvokeAsync<T>` method takes an identifier for the JavaScript function that you wish to invoke along with any number of JSON-serializable arguments.</span></span> <span data-ttu-id="c2e34-113">El identificador de función es relativo al ámbito global (`window`).</span><span class="sxs-lookup"><span data-stu-id="c2e34-113">The function identifier is relative to the global scope (`window`).</span></span> <span data-ttu-id="c2e34-114">Si desea llamar a `window.someScope.someFunction`, el identificador se `someScope.someFunction`.</span><span class="sxs-lookup"><span data-stu-id="c2e34-114">If you wish to call `window.someScope.someFunction`, the identifier is `someScope.someFunction`.</span></span> <span data-ttu-id="c2e34-115">No es necesario registrar la función antes de que se le llame.</span><span class="sxs-lookup"><span data-stu-id="c2e34-115">There's no need to register the function before it's called.</span></span> <span data-ttu-id="c2e34-116">El tipo de valor devuelto `T` también debe ser serializable.</span><span class="sxs-lookup"><span data-stu-id="c2e34-116">The return type `T` must also be JSON serializable.</span></span> <span data-ttu-id="c2e34-117">`T` debe coincidir con el tipo de .NET que se asigna mejor al tipo JSON devuelto.</span><span class="sxs-lookup"><span data-stu-id="c2e34-117">`T` should match the .NET type that best maps to the JSON type returned.</span></span>

<span data-ttu-id="c2e34-118">Para las aplicaciones de Blazor Server:</span><span class="sxs-lookup"><span data-stu-id="c2e34-118">For Blazor Server apps:</span></span>

* <span data-ttu-id="c2e34-119">La aplicación de servidor de Blazor procesa varias solicitudes de usuario.</span><span class="sxs-lookup"><span data-stu-id="c2e34-119">Multiple user requests are processed by the Blazor Server app.</span></span> <span data-ttu-id="c2e34-120">No llame a `JSRuntime.Current` de un componente para invocar funciones de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="c2e34-120">Don't call `JSRuntime.Current` in a component to invoke JavaScript functions.</span></span>
* <span data-ttu-id="c2e34-121">Inserte la abstracción `IJSRuntime` y utilice el objeto insertado para emitir llamadas de interoperabilidad de JS.</span><span class="sxs-lookup"><span data-stu-id="c2e34-121">Inject the `IJSRuntime` abstraction and use the injected object to issue JS interop calls.</span></span>
* <span data-ttu-id="c2e34-122">Aunque una aplicación Blazor se está preprocesando, no es posible llamar a JavaScript porque no se ha establecido una conexión con el explorador.</span><span class="sxs-lookup"><span data-stu-id="c2e34-122">While a Blazor app is prerendering, calling into JavaScript isn't possible because a connection with the browser hasn't been established.</span></span> <span data-ttu-id="c2e34-123">Para obtener más información, consulte la sección [detectar cuándo se está representando una aplicación Blazor](#detect-when-a-blazor-app-is-prerendering) .</span><span class="sxs-lookup"><span data-stu-id="c2e34-123">For more information, see the [Detect when a Blazor app is prerendering](#detect-when-a-blazor-app-is-prerendering) section.</span></span>

<span data-ttu-id="c2e34-124">El ejemplo siguiente se basa en [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), un descodificador basado en JavaScript experimental.</span><span class="sxs-lookup"><span data-stu-id="c2e34-124">The following example is based on [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), an experimental JavaScript-based decoder.</span></span> <span data-ttu-id="c2e34-125">En el ejemplo se muestra cómo invocar una función de C# JavaScript desde un método.</span><span class="sxs-lookup"><span data-stu-id="c2e34-125">The example demonstrates how to invoke a JavaScript function from a C# method.</span></span> <span data-ttu-id="c2e34-126">La función de JavaScript acepta una matriz de bytes de un C# método, descodifica la matriz y devuelve el texto al componente que se va a mostrar.</span><span class="sxs-lookup"><span data-stu-id="c2e34-126">The JavaScript function accepts a byte array from a C# method, decodes the array, and returns the text to the component for display.</span></span>

<span data-ttu-id="c2e34-127">Dentro del elemento `<head>` de *wwwroot/index.html* (Blazor webassembly) o *pages/_Host. cshtml* (Blazor Server), proporcione una función de JavaScript que use `TextDecoder` para descodificar una matriz pasada y devolver el valor descodificado:</span><span class="sxs-lookup"><span data-stu-id="c2e34-127">Inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server), provide a JavaScript function that uses `TextDecoder` to decode a passed array and return the decoded value:</span></span>

[!code-html[](javascript-interop/samples_snapshot/index-script-convertarray.html)]

<span data-ttu-id="c2e34-128">El código JavaScript, como el código que se muestra en el ejemplo anterior, también se puede cargar desde un archivo JavaScript ( *. js*) con una referencia al archivo de script:</span><span class="sxs-lookup"><span data-stu-id="c2e34-128">JavaScript code, such as the code shown in the preceding example, can also be loaded from a JavaScript file (*.js*) with a reference to the script file:</span></span>

```html
<script src="exampleJsInterop.js"></script>
```

<span data-ttu-id="c2e34-129">El siguiente componente:</span><span class="sxs-lookup"><span data-stu-id="c2e34-129">The following component:</span></span>

* <span data-ttu-id="c2e34-130">Invoca la función `convertArray` JavaScript mediante `JSRuntime` cuando se selecciona un botón de componente (**convertir matriz**).</span><span class="sxs-lookup"><span data-stu-id="c2e34-130">Invokes the `convertArray` JavaScript function using `JSRuntime` when a component button (**Convert Array**) is selected.</span></span>
* <span data-ttu-id="c2e34-131">Después de llamar a la función de JavaScript, la matriz que se pasa se convierte en una cadena.</span><span class="sxs-lookup"><span data-stu-id="c2e34-131">After the JavaScript function is called, the passed array is converted into a string.</span></span> <span data-ttu-id="c2e34-132">La cadena se devuelve al componente para su presentación.</span><span class="sxs-lookup"><span data-stu-id="c2e34-132">The string is returned to the component for display.</span></span>

[!code-cshtml[](javascript-interop/samples_snapshot/call-js-example.razor?highlight=2,34-35)]

## <a name="use-of-ijsruntime"></a><span data-ttu-id="c2e34-133">Uso de IJSRuntime</span><span class="sxs-lookup"><span data-stu-id="c2e34-133">Use of IJSRuntime</span></span>

<span data-ttu-id="c2e34-134">Para usar la abstracción `IJSRuntime`, adopte cualquiera de los enfoques siguientes:</span><span class="sxs-lookup"><span data-stu-id="c2e34-134">To use the `IJSRuntime` abstraction, adopt any of the following approaches:</span></span>

* <span data-ttu-id="c2e34-135">Inyectar la abstracción de `IJSRuntime` en el componente de Razor ( *. Razor*):</span><span class="sxs-lookup"><span data-stu-id="c2e34-135">Inject the `IJSRuntime` abstraction into the Razor component (*.razor*):</span></span>

  [!code-cshtml[](javascript-interop/samples_snapshot/inject-abstraction.razor?highlight=1)]

  <span data-ttu-id="c2e34-136">Dentro del elemento `<head>` de *wwwroot/index.html* (Blazor webassembly) o *pages/_Host. cshtml* (Blazor Server), proporcione una función `handleTickerChanged` JavaScript.</span><span class="sxs-lookup"><span data-stu-id="c2e34-136">Inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server), provide a `handleTickerChanged` JavaScript function.</span></span> <span data-ttu-id="c2e34-137">Se llama a la función con `IJSRuntime.InvokeVoidAsync` y no devuelve un valor:</span><span class="sxs-lookup"><span data-stu-id="c2e34-137">The function is called with `IJSRuntime.InvokeVoidAsync` and doesn't return a value:</span></span>

  [!code-html[](javascript-interop/samples_snapshot/index-script-handleTickerChanged1.html)]

* <span data-ttu-id="c2e34-138">Inserte la abstracción de `IJSRuntime` en una clase ( *. CS*):</span><span class="sxs-lookup"><span data-stu-id="c2e34-138">Inject the `IJSRuntime` abstraction into a class (*.cs*):</span></span>

  [!code-csharp[](javascript-interop/samples_snapshot/inject-abstraction-class.cs?highlight=5)]

  <span data-ttu-id="c2e34-139">Dentro del elemento `<head>` de *wwwroot/index.html* (Blazor webassembly) o *pages/_Host. cshtml* (Blazor Server), proporcione una función `handleTickerChanged` JavaScript.</span><span class="sxs-lookup"><span data-stu-id="c2e34-139">Inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server), provide a `handleTickerChanged` JavaScript function.</span></span> <span data-ttu-id="c2e34-140">Se llama a la función con `JSRuntime.InvokeAsync` y devuelve un valor:</span><span class="sxs-lookup"><span data-stu-id="c2e34-140">The function is called with `JSRuntime.InvokeAsync` and returns a value:</span></span>

  [!code-html[](javascript-interop/samples_snapshot/index-script-handleTickerChanged2.html)]

* <span data-ttu-id="c2e34-141">Para la generación de contenido dinámico con [BuildRenderTree](xref:blazor/components#manual-rendertreebuilder-logic), use el atributo `[Inject]`:</span><span class="sxs-lookup"><span data-stu-id="c2e34-141">For dynamic content generation with [BuildRenderTree](xref:blazor/components#manual-rendertreebuilder-logic), use the `[Inject]` attribute:</span></span>

  ```csharp
  [Inject]
  IJSRuntime JSRuntime { get; set; }
  ```

<span data-ttu-id="c2e34-142">En la aplicación de ejemplo del lado cliente que acompaña a este tema, hay dos funciones de JavaScript disponibles para la aplicación que interactúan con el DOM para recibir datos proporcionados por el usuario y mostrar un mensaje de bienvenida:</span><span class="sxs-lookup"><span data-stu-id="c2e34-142">In the client-side sample app that accompanies this topic, two JavaScript functions are available to the app that interact with the DOM to receive user input and display a welcome message:</span></span>

* <span data-ttu-id="c2e34-143">`showPrompt` &ndash; genera un mensaje para aceptar la entrada del usuario (el nombre del usuario) y devuelve el nombre al autor de la llamada.</span><span class="sxs-lookup"><span data-stu-id="c2e34-143">`showPrompt` &ndash; Produces a prompt to accept user input (the user's name) and returns the name to the caller.</span></span>
* <span data-ttu-id="c2e34-144">`displayWelcome` &ndash; asigna un mensaje de bienvenida del autor de la llamada a un objeto DOM con un `id` de `welcome`.</span><span class="sxs-lookup"><span data-stu-id="c2e34-144">`displayWelcome` &ndash; Assigns a welcome message from the caller to a DOM object with an `id` of `welcome`.</span></span>

<span data-ttu-id="c2e34-145">*wwwroot/exampleJsInterop. js*:</span><span class="sxs-lookup"><span data-stu-id="c2e34-145">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=2-7)]

<span data-ttu-id="c2e34-146">Coloque la etiqueta de `<script>` que hace referencia al archivo JavaScript en el archivo *wwwroot/index.html* (Blazor webassembly) o el archivo *pages/_Host. cshtml* (Blazor Server).</span><span class="sxs-lookup"><span data-stu-id="c2e34-146">Place the `<script>` tag that references the JavaScript file in the *wwwroot/index.html* file (Blazor WebAssembly) or *Pages/_Host.cshtml* file (Blazor Server).</span></span>

<span data-ttu-id="c2e34-147">*wwwroot/index.html* (Blazor webassembly):</span><span class="sxs-lookup"><span data-stu-id="c2e34-147">*wwwroot/index.html* (Blazor WebAssembly):</span></span>

[!code-html[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/index.html?highlight=15)]

<span data-ttu-id="c2e34-148">*Pages/_Host. cshtml* (Blazor Server):</span><span class="sxs-lookup"><span data-stu-id="c2e34-148">*Pages/_Host.cshtml* (Blazor Server):</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorServerSample/Pages/_Host.cshtml?highlight=21)]

<span data-ttu-id="c2e34-149">No coloque un `<script>` etiqueta en un archivo de componente porque la etiqueta de `<script>` no se puede actualizar dinámicamente.</span><span class="sxs-lookup"><span data-stu-id="c2e34-149">Don't place a `<script>` tag in a component file because the `<script>` tag can't be updated dynamically.</span></span>

<span data-ttu-id="c2e34-150">Los métodos de .NET interoperan con las funciones de JavaScript en el archivo *exampleJsInterop. js* llamando a `IJSRuntime.InvokeAsync<T>`.</span><span class="sxs-lookup"><span data-stu-id="c2e34-150">.NET methods interop with the JavaScript functions in the *exampleJsInterop.js* file by calling `IJSRuntime.InvokeAsync<T>`.</span></span>

<span data-ttu-id="c2e34-151">La abstracción `IJSRuntime` es asincrónica para permitir escenarios de Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="c2e34-151">The `IJSRuntime` abstraction is asynchronous to allow for Blazor Server scenarios.</span></span> <span data-ttu-id="c2e34-152">Si la aplicación es una aplicación Blazor webassembly y desea invocar una función de JavaScript sincrónicamente, se puede convertir en `IJSInProcessRuntime` y llamar a `Invoke<T>` en su lugar.</span><span class="sxs-lookup"><span data-stu-id="c2e34-152">If the app is a Blazor WebAssembly app and you want to invoke a JavaScript function synchronously, downcast to `IJSInProcessRuntime` and call `Invoke<T>` instead.</span></span> <span data-ttu-id="c2e34-153">Se recomienda que la mayoría de las bibliotecas de interoperabilidad de JS usen las API asincrónicas para asegurarse de que las bibliotecas están disponibles en todos los escenarios.</span><span class="sxs-lookup"><span data-stu-id="c2e34-153">We recommend that most JS interop libraries use the async APIs to ensure that the libraries are available in all scenarios.</span></span>

<span data-ttu-id="c2e34-154">La aplicación de ejemplo incluye un componente para mostrar la interoperabilidad de JS.</span><span class="sxs-lookup"><span data-stu-id="c2e34-154">The sample app includes a component to demonstrate JS interop.</span></span> <span data-ttu-id="c2e34-155">El componente:</span><span class="sxs-lookup"><span data-stu-id="c2e34-155">The component:</span></span>

* <span data-ttu-id="c2e34-156">Recibe la entrada del usuario a través de un mensaje de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="c2e34-156">Receives user input via a JavaScript prompt.</span></span>
* <span data-ttu-id="c2e34-157">Devuelve el texto del componente que se va a procesar.</span><span class="sxs-lookup"><span data-stu-id="c2e34-157">Returns the text to the component for processing.</span></span>
* <span data-ttu-id="c2e34-158">Llama a una segunda función de JavaScript que interactúa con el DOM para mostrar un mensaje de bienvenida.</span><span class="sxs-lookup"><span data-stu-id="c2e34-158">Calls a second JavaScript function that interacts with the DOM to display a welcome message.</span></span>

<span data-ttu-id="c2e34-159">*Pages/JSInterop. Razor*:</span><span class="sxs-lookup"><span data-stu-id="c2e34-159">*Pages/JSInterop.razor*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorWebAssemblySample/Pages/JsInterop.razor?name=snippet_JSInterop1&highlight=3,19-21,23-25)]

1. <span data-ttu-id="c2e34-160">Cuando se ejecuta `TriggerJsPrompt` seleccionando el botón **desencadenador de JavaScript** del componente, se llama a la función de JavaScript `showPrompt` proporcionada en el archivo *wwwroot/exampleJsInterop. js* .</span><span class="sxs-lookup"><span data-stu-id="c2e34-160">When `TriggerJsPrompt` is executed by selecting the component's **Trigger JavaScript Prompt** button, the JavaScript `showPrompt` function provided in the *wwwroot/exampleJsInterop.js* file is called.</span></span>
1. <span data-ttu-id="c2e34-161">La función `showPrompt` acepta datos proporcionados por el usuario (el nombre del usuario), que se codifica en HTML y se devuelve al componente.</span><span class="sxs-lookup"><span data-stu-id="c2e34-161">The `showPrompt` function accepts user input (the user's name), which is HTML-encoded and returned to the component.</span></span> <span data-ttu-id="c2e34-162">El componente almacena el nombre del usuario en una variable local, `name`.</span><span class="sxs-lookup"><span data-stu-id="c2e34-162">The component stores the user's name in a local variable, `name`.</span></span>
1. <span data-ttu-id="c2e34-163">La cadena almacenada en `name` se incorpora en un mensaje de bienvenida, que se pasa a una función de JavaScript, `displayWelcome`, que representa el mensaje de bienvenida en una etiqueta de encabezado.</span><span class="sxs-lookup"><span data-stu-id="c2e34-163">The string stored in `name` is incorporated into a welcome message, which is passed to a JavaScript function, `displayWelcome`, which renders the welcome message into a heading tag.</span></span>

## <a name="call-a-void-javascript-function"></a><span data-ttu-id="c2e34-164">Llamar a una función de JavaScript void</span><span class="sxs-lookup"><span data-stu-id="c2e34-164">Call a void JavaScript function</span></span>

<span data-ttu-id="c2e34-165">Las funciones de JavaScript que devuelven [void (0)/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void) o [undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) se llaman con `IJSRuntime.InvokeVoidAsync`.</span><span class="sxs-lookup"><span data-stu-id="c2e34-165">JavaScript functions that return [void(0)/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void) or [undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) are called with `IJSRuntime.InvokeVoidAsync`.</span></span>

## <a name="detect-when-a-opno-locblazor-app-is-prerendering"></a><span data-ttu-id="c2e34-166">Detección cuando una aplicación Blazor se está preprocesando</span><span class="sxs-lookup"><span data-stu-id="c2e34-166">Detect when a Blazor app is prerendering</span></span>
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

## <a name="capture-references-to-elements"></a><span data-ttu-id="c2e34-167">Capturar referencias a elementos</span><span class="sxs-lookup"><span data-stu-id="c2e34-167">Capture references to elements</span></span>

<span data-ttu-id="c2e34-168">Algunos escenarios de interoperabilidad de JS requieren referencias a elementos HTML.</span><span class="sxs-lookup"><span data-stu-id="c2e34-168">Some JS interop scenarios require references to HTML elements.</span></span> <span data-ttu-id="c2e34-169">Por ejemplo, una biblioteca de interfaz de usuario puede requerir una referencia de elemento para la inicialización, o puede que necesite llamar a las API de tipo de comando en un elemento, como `focus` o `play`.</span><span class="sxs-lookup"><span data-stu-id="c2e34-169">For example, a UI library may require an element reference for initialization, or you might need to call command-like APIs on an element, such as `focus` or `play`.</span></span>

<span data-ttu-id="c2e34-170">Capture las referencias a los elementos HTML de un componente con el siguiente enfoque:</span><span class="sxs-lookup"><span data-stu-id="c2e34-170">Capture references to HTML elements in a component using the following approach:</span></span>

* <span data-ttu-id="c2e34-171">Agregue un atributo `@ref` al elemento HTML.</span><span class="sxs-lookup"><span data-stu-id="c2e34-171">Add an `@ref` attribute to the HTML element.</span></span>
* <span data-ttu-id="c2e34-172">Defina un campo de tipo `ElementReference` cuyo nombre coincida con el valor del atributo `@ref`.</span><span class="sxs-lookup"><span data-stu-id="c2e34-172">Define a field of type `ElementReference` whose name matches the value of the `@ref` attribute.</span></span>

<span data-ttu-id="c2e34-173">En el ejemplo siguiente se muestra la captura de una referencia al elemento `username` `<input>`:</span><span class="sxs-lookup"><span data-stu-id="c2e34-173">The following example shows capturing a reference to the `username` `<input>` element:</span></span>

```cshtml
<input @ref="username" ... />

@code {
    ElementReference username;
}
```

> [!WARNING]
> <span data-ttu-id="c2e34-174">Use solo una referencia de elemento para mutar el contenido de un elemento vacío que no interactúe con Blazor.</span><span class="sxs-lookup"><span data-stu-id="c2e34-174">Only use an element reference to mutate the contents of an empty element that doesn't interact with Blazor.</span></span> <span data-ttu-id="c2e34-175">Este escenario es útil cuando una API de terceros proporciona contenido al elemento.</span><span class="sxs-lookup"><span data-stu-id="c2e34-175">This scenario is useful when a 3rd party API supplies content to the element.</span></span> <span data-ttu-id="c2e34-176">Dado que Blazor no interactúa con el elemento, no existe ninguna posibilidad de que se produzca un conflicto entre la representación de Blazordel elemento y el DOM.</span><span class="sxs-lookup"><span data-stu-id="c2e34-176">Because Blazor doesn't interact with the element, there's no possibility of a conflict between Blazor's representation of the element and the DOM.</span></span>
>
> <span data-ttu-id="c2e34-177">En el ejemplo siguiente, es *peligroso* mutar el contenido de la lista sin ordenar (`ul`) porque Blazor interactúa con el Dom para rellenar los elementos de lista de este elemento (`<li>`):</span><span class="sxs-lookup"><span data-stu-id="c2e34-177">In the following example, it's *dangerous* to mutate the contents of the unordered list (`ul`) because Blazor interacts with the DOM to populate this element's list items (`<li>`):</span></span>
>
> ```cshtml
> <ul ref="MyList">
>     @foreach (var item in Todos)
>     {
>         <li>@item.Text</li>
>     }
> </ul>
> ```
>
> <span data-ttu-id="c2e34-178">Si la interoperabilidad de JS cambia el contenido de los `MyList` de elemento y Blazor intenta aplicar las diferencias al elemento, las diferencias no coincidirán con el DOM.</span><span class="sxs-lookup"><span data-stu-id="c2e34-178">If JS interop mutates the contents of element `MyList` and Blazor attempts to apply diffs to the element, the diffs won't match the DOM.</span></span>

<span data-ttu-id="c2e34-179">En lo que respecta al código .NET, una `ElementReference` es un identificador opaco.</span><span class="sxs-lookup"><span data-stu-id="c2e34-179">As far as .NET code is concerned, an `ElementReference` is an opaque handle.</span></span> <span data-ttu-id="c2e34-180">Lo *único* que puede hacer con `ElementReference` es pasarlo a código JavaScript a través de la interoperabilidad de JS.</span><span class="sxs-lookup"><span data-stu-id="c2e34-180">The *only* thing you can do with `ElementReference` is pass it through to JavaScript code via JS interop.</span></span> <span data-ttu-id="c2e34-181">Al hacerlo, el código de JavaScript recibe una instancia de `HTMLElement`, que puede usar con las API de DOM normales.</span><span class="sxs-lookup"><span data-stu-id="c2e34-181">When you do so, the JavaScript-side code receives an `HTMLElement` instance, which it can use with normal DOM APIs.</span></span>

<span data-ttu-id="c2e34-182">Por ejemplo, el código siguiente define un método de extensión de .NET que permite establecer el foco en un elemento:</span><span class="sxs-lookup"><span data-stu-id="c2e34-182">For example, the following code defines a .NET extension method that enables setting the focus on an element:</span></span>

<span data-ttu-id="c2e34-183">*exampleJsInterop. js*:</span><span class="sxs-lookup"><span data-stu-id="c2e34-183">*exampleJsInterop.js*:</span></span>

```javascript
window.exampleJsFunctions = {
  focusElement : function (element) {
    element.focus();
  }
}
```

<span data-ttu-id="c2e34-184">Para llamar a una función de JavaScript que no devuelve un valor, use `IJSRuntime.InvokeVoidAsync`.</span><span class="sxs-lookup"><span data-stu-id="c2e34-184">To call a JavaScript function that doesn't return a value, use `IJSRuntime.InvokeVoidAsync`.</span></span> <span data-ttu-id="c2e34-185">El código siguiente establece el foco en la entrada de nombre de usuario mediante una llamada a la función de JavaScript anterior con el `ElementReference`capturado:</span><span class="sxs-lookup"><span data-stu-id="c2e34-185">The following code sets the focus on the username input by calling the preceding JavaScript function with the captured `ElementReference`:</span></span>

[!code-cshtml[](javascript-interop/samples_snapshot/component1.razor?highlight=1,3,11-12)]

<span data-ttu-id="c2e34-186">Para usar un método de extensión, cree un método de extensión estático que reciba la `IJSRuntime` instancia:</span><span class="sxs-lookup"><span data-stu-id="c2e34-186">To use an extension method, create a static extension method that receives the `IJSRuntime` instance:</span></span>

```csharp
public static async Task Focus(this ElementReference elementRef, IJSRuntime jsRuntime)
{
    await jsRuntime.InvokeVoidAsync(
        "exampleJsFunctions.focusElement", elementRef);
}
```

<span data-ttu-id="c2e34-187">Se llama al método `Focus` directamente en el objeto.</span><span class="sxs-lookup"><span data-stu-id="c2e34-187">The `Focus` method is called directly on the object.</span></span> <span data-ttu-id="c2e34-188">En el ejemplo siguiente se da por supuesto que el método `Focus` está disponible en el espacio de nombres `JsInteropClasses`:</span><span class="sxs-lookup"><span data-stu-id="c2e34-188">The following example assumes that the `Focus` method is available from the `JsInteropClasses` namespace:</span></span>

[!code-cshtml[](javascript-interop/samples_snapshot/component2.razor?highlight=1-4,12)]

> [!IMPORTANT]
> <span data-ttu-id="c2e34-189">La variable `username` solo se rellena después de representar el componente.</span><span class="sxs-lookup"><span data-stu-id="c2e34-189">The `username` variable is only populated after the component is rendered.</span></span> <span data-ttu-id="c2e34-190">Si se pasa un `ElementReference` sin rellenar al código JavaScript, el código JavaScript recibe un valor de `null`.</span><span class="sxs-lookup"><span data-stu-id="c2e34-190">If an unpopulated `ElementReference` is passed to JavaScript code, the JavaScript code receives a value of `null`.</span></span> <span data-ttu-id="c2e34-191">Para manipular las referencias de elemento una vez finalizada la representación del componente (para establecer el foco inicial en un elemento), use los [métodos de ciclo de vida del componente OnAfterRenderAsync o OnAfterRender](xref:blazor/lifecycle#after-component-render).</span><span class="sxs-lookup"><span data-stu-id="c2e34-191">To manipulate element references after the component has finished rendering (to set the initial focus on an element) use the [OnAfterRenderAsync or OnAfterRender component lifecycle methods](xref:blazor/lifecycle#after-component-render).</span></span>

<span data-ttu-id="c2e34-192">Al trabajar con tipos genéricos y devolver un valor, use [ValueTask\<t >](xref:System.Threading.Tasks.ValueTask`1):</span><span class="sxs-lookup"><span data-stu-id="c2e34-192">When working with generic types and returning a value, use [ValueTask\<T>](xref:System.Threading.Tasks.ValueTask`1):</span></span>

```csharp
public static ValueTask<T> GenericMethod<T>(this ElementReference elementRef, 
    IJSRuntime jsRuntime)
{
    return jsRuntime.InvokeAsync<T>(
        "exampleJsFunctions.doSomethingGeneric", elementRef);
}
```

<span data-ttu-id="c2e34-193">se llama a `GenericMethod` directamente en el objeto con un tipo.</span><span class="sxs-lookup"><span data-stu-id="c2e34-193">`GenericMethod` is called directly on the object with a type.</span></span> <span data-ttu-id="c2e34-194">En el ejemplo siguiente se da por supuesto que el `GenericMethod` está disponible en el espacio de nombres `JsInteropClasses`:</span><span class="sxs-lookup"><span data-stu-id="c2e34-194">The following example assumes that the `GenericMethod` is available from the `JsInteropClasses` namespace:</span></span>

[!code-cshtml[](javascript-interop/samples_snapshot/component3.razor?highlight=17)]

## <a name="invoke-net-methods-from-javascript-functions"></a><span data-ttu-id="c2e34-195">Invocar métodos .NET desde funciones de JavaScript</span><span class="sxs-lookup"><span data-stu-id="c2e34-195">Invoke .NET methods from JavaScript functions</span></span>

### <a name="static-net-method-call"></a><span data-ttu-id="c2e34-196">Llamada al método estático de .NET</span><span class="sxs-lookup"><span data-stu-id="c2e34-196">Static .NET method call</span></span>

<span data-ttu-id="c2e34-197">Para invocar un método .NET estático desde JavaScript, use las funciones `DotNet.invokeMethod` o `DotNet.invokeMethodAsync`.</span><span class="sxs-lookup"><span data-stu-id="c2e34-197">To invoke a static .NET method from JavaScript, use the `DotNet.invokeMethod` or `DotNet.invokeMethodAsync` functions.</span></span> <span data-ttu-id="c2e34-198">Pase el identificador del método estático al que desea llamar, el nombre del ensamblado que contiene la función y cualquier argumento.</span><span class="sxs-lookup"><span data-stu-id="c2e34-198">Pass in the identifier of the static method you wish to call, the name of the assembly containing the function, and any arguments.</span></span> <span data-ttu-id="c2e34-199">La versión asincrónica es preferible para admitir escenarios de Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="c2e34-199">The asynchronous version is preferred to support Blazor Server scenarios.</span></span> <span data-ttu-id="c2e34-200">Para invocar un método .NET desde JavaScript, el método .NET debe ser público, estático y tener el atributo `[JSInvokable]`.</span><span class="sxs-lookup"><span data-stu-id="c2e34-200">To invoke a .NET method from JavaScript, the .NET method must be public, static, and have the `[JSInvokable]` attribute.</span></span> <span data-ttu-id="c2e34-201">De forma predeterminada, el identificador del método es el nombre del método, pero puede especificar un identificador diferente mediante el constructor `JSInvokableAttribute`.</span><span class="sxs-lookup"><span data-stu-id="c2e34-201">By default, the method identifier is the method name, but you can specify a different identifier using the `JSInvokableAttribute` constructor.</span></span> <span data-ttu-id="c2e34-202">Actualmente no se admite la llamada a métodos genéricos abiertos.</span><span class="sxs-lookup"><span data-stu-id="c2e34-202">Calling open generic methods isn't currently supported.</span></span>

<span data-ttu-id="c2e34-203">La aplicación de ejemplo incluye C# un método para devolver una matriz de `int`s.</span><span class="sxs-lookup"><span data-stu-id="c2e34-203">The sample app includes a C# method to return an array of `int`s.</span></span> <span data-ttu-id="c2e34-204">El atributo `JSInvokable` se aplica al método.</span><span class="sxs-lookup"><span data-stu-id="c2e34-204">The `JSInvokable` attribute is applied to the method.</span></span>

<span data-ttu-id="c2e34-205">*Pages/JsInterop. Razor*:</span><span class="sxs-lookup"><span data-stu-id="c2e34-205">*Pages/JsInterop.razor*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorWebAssemblySample/Pages/JsInterop.razor?name=snippet_JSInterop2&highlight=7-11)]

<span data-ttu-id="c2e34-206">JavaScript atendido al cliente invoca el C# método .net.</span><span class="sxs-lookup"><span data-stu-id="c2e34-206">JavaScript served to the client invokes the C# .NET method.</span></span>

<span data-ttu-id="c2e34-207">*wwwroot/exampleJsInterop. js*:</span><span class="sxs-lookup"><span data-stu-id="c2e34-207">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=8-14)]

<span data-ttu-id="c2e34-208">Cuando se seleccione el botón **desencadenador de método estático de .net ReturnArrayAsync** , examine la salida de la consola en las herramientas de desarrollo web del explorador.</span><span class="sxs-lookup"><span data-stu-id="c2e34-208">When the **Trigger .NET static method ReturnArrayAsync** button is selected, examine the console output in the browser's web developer tools.</span></span>

<span data-ttu-id="c2e34-209">La salida de la consola es:</span><span class="sxs-lookup"><span data-stu-id="c2e34-209">The console output is:</span></span>

```console
Array(4) [ 1, 2, 3, 4 ]
```

<span data-ttu-id="c2e34-210">El cuarto valor de matriz se inserta en la matriz (`data.push(4);`) devuelta por `ReturnArrayAsync`.</span><span class="sxs-lookup"><span data-stu-id="c2e34-210">The fourth array value is pushed to the array (`data.push(4);`) returned by `ReturnArrayAsync`.</span></span>

### <a name="instance-method-call"></a><span data-ttu-id="c2e34-211">Llamada al método de instancia</span><span class="sxs-lookup"><span data-stu-id="c2e34-211">Instance method call</span></span>

<span data-ttu-id="c2e34-212">También puede llamar a métodos de instancia de .NET desde JavaScript.</span><span class="sxs-lookup"><span data-stu-id="c2e34-212">You can also call .NET instance methods from JavaScript.</span></span> <span data-ttu-id="c2e34-213">Para invocar un método de instancia de .NET desde JavaScript:</span><span class="sxs-lookup"><span data-stu-id="c2e34-213">To invoke a .NET instance method from JavaScript:</span></span>

* <span data-ttu-id="c2e34-214">Pase la instancia de .NET a JavaScript ajustándola en una instancia de `DotNetObjectReference`.</span><span class="sxs-lookup"><span data-stu-id="c2e34-214">Pass the .NET instance to JavaScript by wrapping it in a `DotNetObjectReference` instance.</span></span> <span data-ttu-id="c2e34-215">La instancia de .NET se pasa por referencia a JavaScript.</span><span class="sxs-lookup"><span data-stu-id="c2e34-215">The .NET instance is passed by reference to JavaScript.</span></span>
* <span data-ttu-id="c2e34-216">Invocar métodos de instancia de .NET en la instancia de mediante las funciones `invokeMethod` o `invokeMethodAsync`.</span><span class="sxs-lookup"><span data-stu-id="c2e34-216">Invoke .NET instance methods on the instance using the `invokeMethod` or `invokeMethodAsync` functions.</span></span> <span data-ttu-id="c2e34-217">La instancia de .NET también se puede pasar como argumento al invocar otros métodos .NET desde JavaScript.</span><span class="sxs-lookup"><span data-stu-id="c2e34-217">The .NET instance can also be passed as an argument when invoking other .NET methods from JavaScript.</span></span>

> [!NOTE]
> <span data-ttu-id="c2e34-218">La aplicación de ejemplo registra mensajes en la consola del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="c2e34-218">The sample app logs messages to the client-side console.</span></span> <span data-ttu-id="c2e34-219">En los siguientes ejemplos mostrados por la aplicación de ejemplo, examine la salida de la consola del explorador en las herramientas de desarrollo del explorador.</span><span class="sxs-lookup"><span data-stu-id="c2e34-219">For the following examples demonstrated by the sample app, examine the browser's console output in the browser's developer tools.</span></span>

<span data-ttu-id="c2e34-220">Cuando se selecciona el botón **desencadenador de instancia de .net HelloHelper. SayHello** , se llama a `ExampleJsInterop.CallHelloHelperSayHello` y pasa un nombre, `Blazor`, al método.</span><span class="sxs-lookup"><span data-stu-id="c2e34-220">When the **Trigger .NET instance method HelloHelper.SayHello** button is selected, `ExampleJsInterop.CallHelloHelperSayHello` is called and passes a name, `Blazor`, to the method.</span></span>

<span data-ttu-id="c2e34-221">*Pages/JsInterop. Razor*:</span><span class="sxs-lookup"><span data-stu-id="c2e34-221">*Pages/JsInterop.razor*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorWebAssemblySample/Pages/JsInterop.razor?name=snippet_JSInterop3&highlight=8-9)]

<span data-ttu-id="c2e34-222">`CallHelloHelperSayHello` invoca la función de JavaScript `sayHello` con una nueva instancia de `HelloHelper`.</span><span class="sxs-lookup"><span data-stu-id="c2e34-222">`CallHelloHelperSayHello` invokes the JavaScript function `sayHello` with a new instance of `HelloHelper`.</span></span>

<span data-ttu-id="c2e34-223">*JsInteropClasses/ExampleJsInterop. CS*:</span><span class="sxs-lookup"><span data-stu-id="c2e34-223">*JsInteropClasses/ExampleJsInterop.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=10-16)]

<span data-ttu-id="c2e34-224">*wwwroot/exampleJsInterop. js*:</span><span class="sxs-lookup"><span data-stu-id="c2e34-224">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=15-18)]

<span data-ttu-id="c2e34-225">El nombre se pasa al constructor de `HelloHelper`, que establece la propiedad `HelloHelper.Name`.</span><span class="sxs-lookup"><span data-stu-id="c2e34-225">The name is passed to `HelloHelper`'s constructor, which sets the `HelloHelper.Name` property.</span></span> <span data-ttu-id="c2e34-226">Cuando se ejecuta la función de JavaScript `sayHello`, `HelloHelper.SayHello` devuelve el mensaje `Hello, {Name}!`, que la función JavaScript escribe en la consola.</span><span class="sxs-lookup"><span data-stu-id="c2e34-226">When the JavaScript function `sayHello` is executed, `HelloHelper.SayHello` returns the `Hello, {Name}!` message, which is written to the console by the JavaScript function.</span></span>

<span data-ttu-id="c2e34-227">*JsInteropClasses/HelloHelper. CS*:</span><span class="sxs-lookup"><span data-stu-id="c2e34-227">*JsInteropClasses/HelloHelper.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

<span data-ttu-id="c2e34-228">Salida de la consola en las herramientas de desarrollo web del explorador:</span><span class="sxs-lookup"><span data-stu-id="c2e34-228">Console output in the browser's web developer tools:</span></span>

```console
Hello, Blazor!
```

## <a name="share-interop-code-in-a-class-library"></a><span data-ttu-id="c2e34-229">Compartir código de interoperabilidad en una biblioteca de clases</span><span class="sxs-lookup"><span data-stu-id="c2e34-229">Share interop code in a class library</span></span>

<span data-ttu-id="c2e34-230">El código de interoperabilidad de JS se puede incluir en una biblioteca de clases, lo que permite compartir el código en un paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="c2e34-230">JS interop code can be included in a class library, which allows you to share the code in a NuGet package.</span></span>

<span data-ttu-id="c2e34-231">La biblioteca de clases controla la incrustación de recursos de JavaScript en el ensamblado compilado.</span><span class="sxs-lookup"><span data-stu-id="c2e34-231">The class library handles embedding JavaScript resources in the built assembly.</span></span> <span data-ttu-id="c2e34-232">Los archivos de JavaScript se colocan en la carpeta *wwwroot* .</span><span class="sxs-lookup"><span data-stu-id="c2e34-232">The JavaScript files are placed in the *wwwroot* folder.</span></span> <span data-ttu-id="c2e34-233">Las herramientas se encargan de insertar los recursos cuando se compila la biblioteca.</span><span class="sxs-lookup"><span data-stu-id="c2e34-233">The tooling takes care of embedding the resources when the library is built.</span></span>

<span data-ttu-id="c2e34-234">Se hace referencia al paquete de NuGet compilado en el archivo de proyecto de la aplicación de la misma manera que se hace referencia a cualquier paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="c2e34-234">The built NuGet package is referenced in the app's project file the same way that any NuGet package is referenced.</span></span> <span data-ttu-id="c2e34-235">Una vez restaurado el paquete, el código de la aplicación puede llamar a JavaScript C#como si fuera.</span><span class="sxs-lookup"><span data-stu-id="c2e34-235">After the package is restored, app code can call into JavaScript as if it were C#.</span></span>

<span data-ttu-id="c2e34-236">Para obtener más información, vea <xref:blazor/class-libraries>.</span><span class="sxs-lookup"><span data-stu-id="c2e34-236">For more information, see <xref:blazor/class-libraries>.</span></span>

## <a name="harden-js-interop-calls"></a><span data-ttu-id="c2e34-237">Reprotección de llamadas de interoperabilidad de JS</span><span class="sxs-lookup"><span data-stu-id="c2e34-237">Harden JS interop calls</span></span>

<span data-ttu-id="c2e34-238">La interoperabilidad de JS puede producir un error debido a errores de red y debe tratarse como no confiable.</span><span class="sxs-lookup"><span data-stu-id="c2e34-238">JS interop may fail due to networking errors and should be treated as unreliable.</span></span> <span data-ttu-id="c2e34-239">De forma predeterminada, una aplicación de Blazor Server agota el tiempo de espera para las llamadas de interoperabilidad de JS en el servidor después de un minuto.</span><span class="sxs-lookup"><span data-stu-id="c2e34-239">By default, a Blazor Server app times out JS interop calls on the server after one minute.</span></span> <span data-ttu-id="c2e34-240">Si una aplicación puede tolerar un tiempo de espera más agresivo, como 10 segundos, establezca el tiempo de espera con uno de los siguientes métodos:</span><span class="sxs-lookup"><span data-stu-id="c2e34-240">If an app can tolerate a more aggressive timeout, such as 10 seconds, set the timeout using one of the following approaches:</span></span>

* <span data-ttu-id="c2e34-241">Globalmente en `Startup.ConfigureServices`, especifique el tiempo de espera:</span><span class="sxs-lookup"><span data-stu-id="c2e34-241">Globally in `Startup.ConfigureServices`, specify the timeout:</span></span>

  ```csharp
  services.AddServerSideBlazor(
      options => options.JSInteropDefaultCallTimeout = TimeSpan.FromSeconds({SECONDS}));
  ```

* <span data-ttu-id="c2e34-242">Por cada invocación en el código de componente, una sola llamada puede especificar el tiempo de espera:</span><span class="sxs-lookup"><span data-stu-id="c2e34-242">Per-invocation in component code, a single call can specify the timeout:</span></span>

  ```csharp
  var result = await JSRuntime.InvokeAsync<string>("MyJSOperation", 
      TimeSpan.FromSeconds({SECONDS}), new[] { "Arg1" });
  ```

<span data-ttu-id="c2e34-243">Para obtener más información sobre el agotamiento de recursos, consulte <xref:security/blazor/server>.</span><span class="sxs-lookup"><span data-stu-id="c2e34-243">For more information on resource exhaustion, see <xref:security/blazor/server>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c2e34-244">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="c2e34-244">Additional resources</span></span>

* [<span data-ttu-id="c2e34-245">Ejemplo de InteropComponent. Razor (repositorio de GitHub de ASPNET/AspNetCore, rama de versión 3,0)</span><span class="sxs-lookup"><span data-stu-id="c2e34-245">InteropComponent.razor example (aspnet/AspNetCore GitHub repository, 3.0 release branch)</span></span>](https://github.com/aspnet/AspNetCore/blob/release/3.0/src/Components/test/testassets/BasicTestApp/InteropComponent.razor)
