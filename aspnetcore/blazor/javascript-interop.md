---
title: ASP.NET Core Blazor la interoperabilidad de JavaScript
author: guardrex
description: Obtenga información sobre cómo invocar funciones de JavaScript desde los métodos .NET y .NET desde JavaScript en Blazor aplicaciones.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 11/21/2019
no-loc:
- Blazor
uid: blazor/javascript-interop
ms.openlocfilehash: f55eda512f8dcf0695c2e7f4655db83b26ea4159
ms.sourcegitcommit: 3e503ef510008e77be6dd82ee79213c9f7b97607
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/22/2019
ms.locfileid: "74317194"
---
# <a name="aspnet-core-opno-locblazor-javascript-interop"></a><span data-ttu-id="7c9c4-103">ASP.NET Core Blazor la interoperabilidad de JavaScript</span><span class="sxs-lookup"><span data-stu-id="7c9c4-103">ASP.NET Core Blazor JavaScript interop</span></span>

<span data-ttu-id="7c9c4-104">Por [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27)y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="7c9c4-104">By [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="7c9c4-105">Una aplicación Blazor puede invocar funciones de JavaScript desde los métodos .NET y .NET desde el código JavaScript.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-105">A Blazor app can invoke JavaScript functions from .NET and .NET methods from JavaScript code.</span></span>

<span data-ttu-id="7c9c4-106">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7c9c4-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="invoke-javascript-functions-from-net-methods"></a><span data-ttu-id="7c9c4-107">Invocar funciones de JavaScript desde métodos de .NET</span><span class="sxs-lookup"><span data-stu-id="7c9c4-107">Invoke JavaScript functions from .NET methods</span></span>

<span data-ttu-id="7c9c4-108">Hay ocasiones en las que se requiere código .NET para llamar a una función de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-108">There are times when .NET code is required to call a JavaScript function.</span></span> <span data-ttu-id="7c9c4-109">Por ejemplo, una llamada de JavaScript puede exponer funciones o funcionalidades del explorador de una biblioteca de JavaScript a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-109">For example, a JavaScript call can expose browser capabilities or functionality from a JavaScript library to the app.</span></span> <span data-ttu-id="7c9c4-110">Este escenario se denomina *interoperabilidad de JavaScript* (*interoperabilidad de JS*).</span><span class="sxs-lookup"><span data-stu-id="7c9c4-110">This scenario is called *JavaScript interoperability* (*JS interop*).</span></span>

<span data-ttu-id="7c9c4-111">Para llamar a JavaScript desde .NET, use la abstracción `IJSRuntime`.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-111">To call into JavaScript from .NET, use the `IJSRuntime` abstraction.</span></span> <span data-ttu-id="7c9c4-112">El método `InvokeAsync<T>` toma un identificador de la función de JavaScript que se desea invocar junto con cualquier número de argumentos serializables de JSON.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-112">The `InvokeAsync<T>` method takes an identifier for the JavaScript function that you wish to invoke along with any number of JSON-serializable arguments.</span></span> <span data-ttu-id="7c9c4-113">El identificador de función es relativo al ámbito global (`window`).</span><span class="sxs-lookup"><span data-stu-id="7c9c4-113">The function identifier is relative to the global scope (`window`).</span></span> <span data-ttu-id="7c9c4-114">Si desea llamar a `window.someScope.someFunction`, el identificador se `someScope.someFunction`.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-114">If you wish to call `window.someScope.someFunction`, the identifier is `someScope.someFunction`.</span></span> <span data-ttu-id="7c9c4-115">No es necesario registrar la función antes de que se le llame.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-115">There's no need to register the function before it's called.</span></span> <span data-ttu-id="7c9c4-116">El tipo de valor devuelto `T` también debe ser serializable.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-116">The return type `T` must also be JSON serializable.</span></span>

<span data-ttu-id="7c9c4-117">Para las aplicaciones de Blazor Server:</span><span class="sxs-lookup"><span data-stu-id="7c9c4-117">For Blazor Server apps:</span></span>

* <span data-ttu-id="7c9c4-118">La aplicación de servidor de Blazor procesa varias solicitudes de usuario.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-118">Multiple user requests are processed by the Blazor Server app.</span></span> <span data-ttu-id="7c9c4-119">No llame a `JSRuntime.Current` de un componente para invocar funciones de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-119">Don't call `JSRuntime.Current` in a component to invoke JavaScript functions.</span></span>
* <span data-ttu-id="7c9c4-120">Inserte la abstracción `IJSRuntime` y utilice el objeto insertado para emitir llamadas de interoperabilidad de JS.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-120">Inject the `IJSRuntime` abstraction and use the injected object to issue JS interop calls.</span></span>
* <span data-ttu-id="7c9c4-121">Aunque una aplicación Blazor se está preprocesando, no es posible llamar a JavaScript porque no se ha establecido una conexión con el explorador.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-121">While a Blazor app is prerendering, calling into JavaScript isn't possible because a connection with the browser hasn't been established.</span></span> <span data-ttu-id="7c9c4-122">Para obtener más información, consulte la sección [detectar cuándo se está representando una aplicación Blazor](#detect-when-a-blazor-app-is-prerendering) .</span><span class="sxs-lookup"><span data-stu-id="7c9c4-122">For more information, see the [Detect when a Blazor app is prerendering](#detect-when-a-blazor-app-is-prerendering) section.</span></span>

<span data-ttu-id="7c9c4-123">El ejemplo siguiente se basa en [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), un descodificador basado en JavaScript experimental.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-123">The following example is based on [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), an experimental JavaScript-based decoder.</span></span> <span data-ttu-id="7c9c4-124">En el ejemplo se muestra cómo invocar una función de C# JavaScript desde un método.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-124">The example demonstrates how to invoke a JavaScript function from a C# method.</span></span> <span data-ttu-id="7c9c4-125">La función de JavaScript acepta una matriz de bytes de un C# método, descodifica la matriz y devuelve el texto al componente que se va a mostrar.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-125">The JavaScript function accepts a byte array from a C# method, decodes the array, and returns the text to the component for display.</span></span>

<span data-ttu-id="7c9c4-126">Dentro del elemento `<head>` de *wwwroot/index.html* (Blazor webassembly) o *pages/_Host. cshtml* (Blazor Server), proporcione una función de JavaScript que use `TextDecoder` para descodificar una matriz pasada y devolver el valor descodificado:</span><span class="sxs-lookup"><span data-stu-id="7c9c4-126">Inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server), provide a JavaScript function that uses `TextDecoder` to decode a passed array and return the decoded value:</span></span>

[!code-html[](javascript-interop/samples_snapshot/index-script-convertarray.html)]

<span data-ttu-id="7c9c4-127">El código JavaScript, como el código que se muestra en el ejemplo anterior, también se puede cargar desde un archivo JavaScript ( *. js*) con una referencia al archivo de script:</span><span class="sxs-lookup"><span data-stu-id="7c9c4-127">JavaScript code, such as the code shown in the preceding example, can also be loaded from a JavaScript file (*.js*) with a reference to the script file:</span></span>

```html
<script src="exampleJsInterop.js"></script>
```

<span data-ttu-id="7c9c4-128">El siguiente componente:</span><span class="sxs-lookup"><span data-stu-id="7c9c4-128">The following component:</span></span>

* <span data-ttu-id="7c9c4-129">Invoca la función `convertArray` JavaScript mediante `JSRuntime` cuando se selecciona un botón de componente (**convertir matriz**).</span><span class="sxs-lookup"><span data-stu-id="7c9c4-129">Invokes the `convertArray` JavaScript function using `JSRuntime` when a component button (**Convert Array**) is selected.</span></span>
* <span data-ttu-id="7c9c4-130">Después de llamar a la función de JavaScript, la matriz que se pasa se convierte en una cadena.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-130">After the JavaScript function is called, the passed array is converted into a string.</span></span> <span data-ttu-id="7c9c4-131">La cadena se devuelve al componente para su presentación.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-131">The string is returned to the component for display.</span></span>

[!code-cshtml[](javascript-interop/samples_snapshot/call-js-example.razor?highlight=2,34-35)]

## <a name="use-of-ijsruntime"></a><span data-ttu-id="7c9c4-132">Uso de IJSRuntime</span><span class="sxs-lookup"><span data-stu-id="7c9c4-132">Use of IJSRuntime</span></span>

<span data-ttu-id="7c9c4-133">Para usar la abstracción `IJSRuntime`, adopte cualquiera de los enfoques siguientes:</span><span class="sxs-lookup"><span data-stu-id="7c9c4-133">To use the `IJSRuntime` abstraction, adopt any of the following approaches:</span></span>

* <span data-ttu-id="7c9c4-134">Inyectar la abstracción de `IJSRuntime` en el componente de Razor ( *. Razor*):</span><span class="sxs-lookup"><span data-stu-id="7c9c4-134">Inject the `IJSRuntime` abstraction into the Razor component (*.razor*):</span></span>

  [!code-cshtml[](javascript-interop/samples_snapshot/inject-abstraction.razor?highlight=1)]

  <span data-ttu-id="7c9c4-135">Dentro del elemento `<head>` de *wwwroot/index.html* (Blazor webassembly) o *pages/_Host. cshtml* (Blazor Server), proporcione una función `handleTickerChanged` JavaScript.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-135">Inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server), provide a `handleTickerChanged` JavaScript function.</span></span> <span data-ttu-id="7c9c4-136">Se llama a la función con `IJSRuntime.InvokeVoidAsync` y no devuelve un valor:</span><span class="sxs-lookup"><span data-stu-id="7c9c4-136">The function is called with `IJSRuntime.InvokeVoidAsync` and doesn't return a value:</span></span>

  [!code-html[](javascript-interop/samples_snapshot/index-script-handleTickerChanged1.html)]

* <span data-ttu-id="7c9c4-137">Inserte la abstracción de `IJSRuntime` en una clase ( *. CS*):</span><span class="sxs-lookup"><span data-stu-id="7c9c4-137">Inject the `IJSRuntime` abstraction into a class (*.cs*):</span></span>

  [!code-csharp[](javascript-interop/samples_snapshot/inject-abstraction-class.cs?highlight=5)]

  <span data-ttu-id="7c9c4-138">Dentro del elemento `<head>` de *wwwroot/index.html* (Blazor webassembly) o *pages/_Host. cshtml* (Blazor Server), proporcione una función `handleTickerChanged` JavaScript.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-138">Inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server), provide a `handleTickerChanged` JavaScript function.</span></span> <span data-ttu-id="7c9c4-139">Se llama a la función con `JSRuntime.InvokeAsync` y devuelve un valor:</span><span class="sxs-lookup"><span data-stu-id="7c9c4-139">The function is called with `JSRuntime.InvokeAsync` and returns a value:</span></span>

  [!code-html[](javascript-interop/samples_snapshot/index-script-handleTickerChanged2.html)]

* <span data-ttu-id="7c9c4-140">Para la generación de contenido dinámico con [BuildRenderTree](xref:blazor/components#manual-rendertreebuilder-logic), use el atributo `[Inject]`:</span><span class="sxs-lookup"><span data-stu-id="7c9c4-140">For dynamic content generation with [BuildRenderTree](xref:blazor/components#manual-rendertreebuilder-logic), use the `[Inject]` attribute:</span></span>

  ```csharp
  [Inject]
  IJSRuntime JSRuntime { get; set; }
  ```

<span data-ttu-id="7c9c4-141">En la aplicación de ejemplo del lado cliente que acompaña a este tema, hay dos funciones de JavaScript disponibles para la aplicación que interactúan con el DOM para recibir datos proporcionados por el usuario y mostrar un mensaje de bienvenida:</span><span class="sxs-lookup"><span data-stu-id="7c9c4-141">In the client-side sample app that accompanies this topic, two JavaScript functions are available to the app that interact with the DOM to receive user input and display a welcome message:</span></span>

* <span data-ttu-id="7c9c4-142">`showPrompt` &ndash; genera un mensaje para aceptar la entrada del usuario (el nombre del usuario) y devuelve el nombre al autor de la llamada.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-142">`showPrompt` &ndash; Produces a prompt to accept user input (the user's name) and returns the name to the caller.</span></span>
* <span data-ttu-id="7c9c4-143">`displayWelcome` &ndash; asigna un mensaje de bienvenida del autor de la llamada a un objeto DOM con un `id` de `welcome`.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-143">`displayWelcome` &ndash; Assigns a welcome message from the caller to a DOM object with an `id` of `welcome`.</span></span>

<span data-ttu-id="7c9c4-144">*wwwroot/exampleJsInterop.js*:</span><span class="sxs-lookup"><span data-stu-id="7c9c4-144">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=2-7)]

<span data-ttu-id="7c9c4-145">Coloque la etiqueta de `<script>` que hace referencia al archivo JavaScript en el archivo *wwwroot/index.html* (Blazor webassembly) o el archivo *pages/_Host. cshtml* (Blazor Server).</span><span class="sxs-lookup"><span data-stu-id="7c9c4-145">Place the `<script>` tag that references the JavaScript file in the *wwwroot/index.html* file (Blazor WebAssembly) or *Pages/_Host.cshtml* file (Blazor Server).</span></span>

<span data-ttu-id="7c9c4-146">*wwwroot/index.html* (Blazor webassembly):</span><span class="sxs-lookup"><span data-stu-id="7c9c4-146">*wwwroot/index.html* (Blazor WebAssembly):</span></span>

[!code-html[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/index.html?highlight=15)]

<span data-ttu-id="7c9c4-147">*Pages/_Host. cshtml* (Blazor Server):</span><span class="sxs-lookup"><span data-stu-id="7c9c4-147">*Pages/_Host.cshtml* (Blazor Server):</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorServerSample/Pages/_Host.cshtml?highlight=21)]

<span data-ttu-id="7c9c4-148">No coloque un `<script>` etiqueta en un archivo de componente porque la etiqueta de `<script>` no se puede actualizar dinámicamente.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-148">Don't place a `<script>` tag in a component file because the `<script>` tag can't be updated dynamically.</span></span>

<span data-ttu-id="7c9c4-149">Los métodos de .NET interoperan con las funciones de JavaScript en el archivo *exampleJsInterop. js* llamando a `IJSRuntime.InvokeAsync<T>`.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-149">.NET methods interop with the JavaScript functions in the *exampleJsInterop.js* file by calling `IJSRuntime.InvokeAsync<T>`.</span></span>

<span data-ttu-id="7c9c4-150">La abstracción `IJSRuntime` es asincrónica para permitir escenarios de Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-150">The `IJSRuntime` abstraction is asynchronous to allow for Blazor Server scenarios.</span></span> <span data-ttu-id="7c9c4-151">Si la aplicación es una aplicación Blazor webassembly y desea invocar una función de JavaScript sincrónicamente, se puede convertir en `IJSInProcessRuntime` y llamar a `Invoke<T>` en su lugar.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-151">If the app is a Blazor WebAssembly app and you want to invoke a JavaScript function synchronously, downcast to `IJSInProcessRuntime` and call `Invoke<T>` instead.</span></span> <span data-ttu-id="7c9c4-152">Se recomienda que la mayoría de las bibliotecas de interoperabilidad de JS usen las API asincrónicas para asegurarse de que las bibliotecas están disponibles en todos los escenarios.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-152">We recommend that most JS interop libraries use the async APIs to ensure that the libraries are available in all scenarios.</span></span>

<span data-ttu-id="7c9c4-153">La aplicación de ejemplo incluye un componente para mostrar la interoperabilidad de JS.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-153">The sample app includes a component to demonstrate JS interop.</span></span> <span data-ttu-id="7c9c4-154">El componente:</span><span class="sxs-lookup"><span data-stu-id="7c9c4-154">The component:</span></span>

* <span data-ttu-id="7c9c4-155">Recibe la entrada del usuario a través de un mensaje de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-155">Receives user input via a JavaScript prompt.</span></span>
* <span data-ttu-id="7c9c4-156">Devuelve el texto del componente que se va a procesar.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-156">Returns the text to the component for processing.</span></span>
* <span data-ttu-id="7c9c4-157">Llama a una segunda función de JavaScript que interactúa con el DOM para mostrar un mensaje de bienvenida.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-157">Calls a second JavaScript function that interacts with the DOM to display a welcome message.</span></span>

<span data-ttu-id="7c9c4-158">*Pages/JSInterop. Razor*:</span><span class="sxs-lookup"><span data-stu-id="7c9c4-158">*Pages/JSInterop.razor*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorWebAssemblySample/Pages/JsInterop.razor?name=snippet_JSInterop1&highlight=3,19-21,23-25)]

1. <span data-ttu-id="7c9c4-159">Cuando se ejecuta `TriggerJsPrompt` seleccionando el botón **desencadenador de JavaScript** del componente, se llama a la función de JavaScript `showPrompt` proporcionada en el archivo *wwwroot/exampleJsInterop. js* .</span><span class="sxs-lookup"><span data-stu-id="7c9c4-159">When `TriggerJsPrompt` is executed by selecting the component's **Trigger JavaScript Prompt** button, the JavaScript `showPrompt` function provided in the *wwwroot/exampleJsInterop.js* file is called.</span></span>
1. <span data-ttu-id="7c9c4-160">La función `showPrompt` acepta datos proporcionados por el usuario (el nombre del usuario), que se codifica en HTML y se devuelve al componente.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-160">The `showPrompt` function accepts user input (the user's name), which is HTML-encoded and returned to the component.</span></span> <span data-ttu-id="7c9c4-161">El componente almacena el nombre del usuario en una variable local, `name`.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-161">The component stores the user's name in a local variable, `name`.</span></span>
1. <span data-ttu-id="7c9c4-162">La cadena almacenada en `name` se incorpora en un mensaje de bienvenida, que se pasa a una función de JavaScript, `displayWelcome`, que representa el mensaje de bienvenida en una etiqueta de encabezado.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-162">The string stored in `name` is incorporated into a welcome message, which is passed to a JavaScript function, `displayWelcome`, which renders the welcome message into a heading tag.</span></span>

## <a name="call-a-void-javascript-function"></a><span data-ttu-id="7c9c4-163">Llamar a una función de JavaScript void</span><span class="sxs-lookup"><span data-stu-id="7c9c4-163">Call a void JavaScript function</span></span>

<span data-ttu-id="7c9c4-164">Las funciones de JavaScript que devuelven [void (0)/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void) o [undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) se llaman con `IJSRuntime.InvokeVoidAsync`.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-164">JavaScript functions that return [void(0)/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void) or [undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) are called with `IJSRuntime.InvokeVoidAsync`.</span></span>

## <a name="detect-when-a-opno-locblazor-app-is-prerendering"></a><span data-ttu-id="7c9c4-165">Detección cuando una aplicación Blazor se está preprocesando</span><span class="sxs-lookup"><span data-stu-id="7c9c4-165">Detect when a Blazor app is prerendering</span></span>
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

## <a name="capture-references-to-elements"></a><span data-ttu-id="7c9c4-166">Capturar referencias a elementos</span><span class="sxs-lookup"><span data-stu-id="7c9c4-166">Capture references to elements</span></span>

<span data-ttu-id="7c9c4-167">Algunos escenarios de interoperabilidad de JS requieren referencias a elementos HTML.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-167">Some JS interop scenarios require references to HTML elements.</span></span> <span data-ttu-id="7c9c4-168">Por ejemplo, una biblioteca de interfaz de usuario puede requerir una referencia de elemento para la inicialización, o puede que necesite llamar a las API de tipo de comando en un elemento, como `focus` o `play`.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-168">For example, a UI library may require an element reference for initialization, or you might need to call command-like APIs on an element, such as `focus` or `play`.</span></span>

<span data-ttu-id="7c9c4-169">Capture las referencias a los elementos HTML de un componente con el siguiente enfoque:</span><span class="sxs-lookup"><span data-stu-id="7c9c4-169">Capture references to HTML elements in a component using the following approach:</span></span>

* <span data-ttu-id="7c9c4-170">Agregue un atributo `@ref` al elemento HTML.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-170">Add an `@ref` attribute to the HTML element.</span></span>
* <span data-ttu-id="7c9c4-171">Defina un campo de tipo `ElementReference` cuyo nombre coincida con el valor del atributo `@ref`.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-171">Define a field of type `ElementReference` whose name matches the value of the `@ref` attribute.</span></span>

<span data-ttu-id="7c9c4-172">En el ejemplo siguiente se muestra la captura de una referencia al elemento `username` `<input>`:</span><span class="sxs-lookup"><span data-stu-id="7c9c4-172">The following example shows capturing a reference to the `username` `<input>` element:</span></span>

```cshtml
<input @ref="username" ... />

@code {
    ElementReference username;
}
```

> [!WARNING]
> <span data-ttu-id="7c9c4-173">Use solo una referencia de elemento para mutar el contenido de un elemento vacío que no interactúe con Blazor.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-173">Only use an element reference to mutate the contents of an empty element that doesn't interact with Blazor.</span></span> <span data-ttu-id="7c9c4-174">Este escenario es útil cuando una API de terceros proporciona contenido al elemento.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-174">This scenario is useful when a 3rd party API supplies content to the element.</span></span> <span data-ttu-id="7c9c4-175">Dado que Blazor no interactúa con el elemento, no existe ninguna posibilidad de que se produzca un conflicto entre la representación de Blazordel elemento y el DOM.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-175">Because Blazor doesn't interact with the element, there's no possibility of a conflict between Blazor's representation of the element and the DOM.</span></span>
>
> <span data-ttu-id="7c9c4-176">En el ejemplo siguiente, es *peligroso* mutar el contenido de la lista sin ordenar (`ul`) porque Blazor interactúa con el Dom para rellenar los elementos de lista de este elemento (`<li>`):</span><span class="sxs-lookup"><span data-stu-id="7c9c4-176">In the following example, it's *dangerous* to mutate the contents of the unordered list (`ul`) because Blazor interacts with the DOM to populate this element's list items (`<li>`):</span></span>
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
> <span data-ttu-id="7c9c4-177">Si la interoperabilidad de JS cambia el contenido de los `MyList` de elemento y Blazor intenta aplicar las diferencias al elemento, las diferencias no coincidirán con el DOM.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-177">If JS interop mutates the contents of element `MyList` and Blazor attempts to apply diffs to the element, the diffs won't match the DOM.</span></span>

<span data-ttu-id="7c9c4-178">En lo que respecta al código .NET, una `ElementReference` es un identificador opaco.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-178">As far as .NET code is concerned, an `ElementReference` is an opaque handle.</span></span> <span data-ttu-id="7c9c4-179">Lo *único* que puede hacer con `ElementReference` es pasarlo a código JavaScript a través de la interoperabilidad de JS.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-179">The *only* thing you can do with `ElementReference` is pass it through to JavaScript code via JS interop.</span></span> <span data-ttu-id="7c9c4-180">Al hacerlo, el código de JavaScript recibe una instancia de `HTMLElement`, que puede usar con las API de DOM normales.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-180">When you do so, the JavaScript-side code receives an `HTMLElement` instance, which it can use with normal DOM APIs.</span></span>

<span data-ttu-id="7c9c4-181">Por ejemplo, el código siguiente define un método de extensión de .NET que permite establecer el foco en un elemento:</span><span class="sxs-lookup"><span data-stu-id="7c9c4-181">For example, the following code defines a .NET extension method that enables setting the focus on an element:</span></span>

<span data-ttu-id="7c9c4-182">*exampleJsInterop.js*:</span><span class="sxs-lookup"><span data-stu-id="7c9c4-182">*exampleJsInterop.js*:</span></span>

```javascript
window.exampleJsFunctions = {
  focusElement : function (element) {
    element.focus();
  }
}
```

<span data-ttu-id="7c9c4-183">Use `IJSRuntime.InvokeAsync<T>` y llame a `exampleJsFunctions.focusElement` con un `ElementReference` para centrar un elemento:</span><span class="sxs-lookup"><span data-stu-id="7c9c4-183">Use `IJSRuntime.InvokeAsync<T>` and call `exampleJsFunctions.focusElement` with an `ElementReference` to focus an element:</span></span>

[!code-cshtml[](javascript-interop/samples_snapshot/component1.razor?highlight=1,3,11-12)]

<span data-ttu-id="7c9c4-184">Para usar un método de extensión para centrar un elemento, cree un método de extensión estático que reciba la instancia de `IJSRuntime`:</span><span class="sxs-lookup"><span data-stu-id="7c9c4-184">To use an extension method to focus an element, create a static extension method that receives the `IJSRuntime` instance:</span></span>

```csharp
public static Task Focus(this ElementReference elementRef, IJSRuntime jsRuntime)
{
    return jsRuntime.InvokeAsync<object>(
        "exampleJsFunctions.focusElement", elementRef);
}
```

<span data-ttu-id="7c9c4-185">Se llama al método directamente en el objeto.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-185">The method is called directly on the object.</span></span> <span data-ttu-id="7c9c4-186">En el ejemplo siguiente se da por supuesto que el método estático `Focus` está disponible en el espacio de nombres `JsInteropClasses`:</span><span class="sxs-lookup"><span data-stu-id="7c9c4-186">The following example assumes that the static `Focus` method is available from the `JsInteropClasses` namespace:</span></span>

[!code-cshtml[](javascript-interop/samples_snapshot/component2.razor?highlight=1,4,12)]

> [!IMPORTANT]
> <span data-ttu-id="7c9c4-187">La variable `username` solo se rellena después de representar el componente.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-187">The `username` variable is only populated after the component is rendered.</span></span> <span data-ttu-id="7c9c4-188">Si se pasa un `ElementReference` sin rellenar al código JavaScript, el código JavaScript recibe un valor de `null`.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-188">If an unpopulated `ElementReference` is passed to JavaScript code, the JavaScript code receives a value of `null`.</span></span> <span data-ttu-id="7c9c4-189">Para manipular las referencias de elemento una vez finalizada la representación del componente (para establecer el foco inicial en un elemento), use los [métodos de ciclo de vida](xref:blazor/components#lifecycle-methods)de los componentes `OnAfterRenderAsync` o `OnAfterRender`.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-189">To manipulate element references after the component has finished rendering (to set the initial focus on an element) use the `OnAfterRenderAsync` or `OnAfterRender` [component lifecycle methods](xref:blazor/components#lifecycle-methods).</span></span>

## <a name="invoke-net-methods-from-javascript-functions"></a><span data-ttu-id="7c9c4-190">Invocar métodos .NET desde funciones de JavaScript</span><span class="sxs-lookup"><span data-stu-id="7c9c4-190">Invoke .NET methods from JavaScript functions</span></span>

### <a name="static-net-method-call"></a><span data-ttu-id="7c9c4-191">Llamada al método estático de .NET</span><span class="sxs-lookup"><span data-stu-id="7c9c4-191">Static .NET method call</span></span>

<span data-ttu-id="7c9c4-192">Para invocar un método .NET estático desde JavaScript, use las funciones `DotNet.invokeMethod` o `DotNet.invokeMethodAsync`.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-192">To invoke a static .NET method from JavaScript, use the `DotNet.invokeMethod` or `DotNet.invokeMethodAsync` functions.</span></span> <span data-ttu-id="7c9c4-193">Pase el identificador del método estático al que desea llamar, el nombre del ensamblado que contiene la función y cualquier argumento.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-193">Pass in the identifier of the static method you wish to call, the name of the assembly containing the function, and any arguments.</span></span> <span data-ttu-id="7c9c4-194">La versión asincrónica es preferible para admitir escenarios de Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-194">The asynchronous version is preferred to support Blazor Server scenarios.</span></span> <span data-ttu-id="7c9c4-195">Para invocar un método .NET desde JavaScript, el método .NET debe ser público, estático y tener el atributo `[JSInvokable]`.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-195">To invoke a .NET method from JavaScript, the .NET method must be public, static, and have the `[JSInvokable]` attribute.</span></span> <span data-ttu-id="7c9c4-196">De forma predeterminada, el identificador del método es el nombre del método, pero puede especificar un identificador diferente mediante el constructor `JSInvokableAttribute`.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-196">By default, the method identifier is the method name, but you can specify a different identifier using the `JSInvokableAttribute` constructor.</span></span> <span data-ttu-id="7c9c4-197">Actualmente no se admite la llamada a métodos genéricos abiertos.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-197">Calling open generic methods isn't currently supported.</span></span>

<span data-ttu-id="7c9c4-198">La aplicación de ejemplo incluye C# un método para devolver una matriz de `int`s.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-198">The sample app includes a C# method to return an array of `int`s.</span></span> <span data-ttu-id="7c9c4-199">El atributo `JSInvokable` se aplica al método.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-199">The `JSInvokable` attribute is applied to the method.</span></span>

<span data-ttu-id="7c9c4-200">*Pages/JsInterop. Razor*:</span><span class="sxs-lookup"><span data-stu-id="7c9c4-200">*Pages/JsInterop.razor*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorWebAssemblySample/Pages/JsInterop.razor?name=snippet_JSInterop2&highlight=7-11)]

<span data-ttu-id="7c9c4-201">JavaScript atendido al cliente invoca el C# método .net.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-201">JavaScript served to the client invokes the C# .NET method.</span></span>

<span data-ttu-id="7c9c4-202">*wwwroot/exampleJsInterop.js*:</span><span class="sxs-lookup"><span data-stu-id="7c9c4-202">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=8-14)]

<span data-ttu-id="7c9c4-203">Cuando se seleccione el botón **desencadenador de método estático de .net ReturnArrayAsync** , examine la salida de la consola en las herramientas de desarrollo web del explorador.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-203">When the **Trigger .NET static method ReturnArrayAsync** button is selected, examine the console output in the browser's web developer tools.</span></span>

<span data-ttu-id="7c9c4-204">La salida de la consola es:</span><span class="sxs-lookup"><span data-stu-id="7c9c4-204">The console output is:</span></span>

```console
Array(4) [ 1, 2, 3, 4 ]
```

<span data-ttu-id="7c9c4-205">El cuarto valor de matriz se inserta en la matriz (`data.push(4);`) devuelta por `ReturnArrayAsync`.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-205">The fourth array value is pushed to the array (`data.push(4);`) returned by `ReturnArrayAsync`.</span></span>

### <a name="instance-method-call"></a><span data-ttu-id="7c9c4-206">Llamada al método de instancia</span><span class="sxs-lookup"><span data-stu-id="7c9c4-206">Instance method call</span></span>

<span data-ttu-id="7c9c4-207">También puede llamar a métodos de instancia de .NET desde JavaScript.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-207">You can also call .NET instance methods from JavaScript.</span></span> <span data-ttu-id="7c9c4-208">Para invocar un método de instancia de .NET desde JavaScript:</span><span class="sxs-lookup"><span data-stu-id="7c9c4-208">To invoke a .NET instance method from JavaScript:</span></span>

* <span data-ttu-id="7c9c4-209">Pase la instancia de .NET a JavaScript ajustándola en una instancia de `DotNetObjectReference`.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-209">Pass the .NET instance to JavaScript by wrapping it in a `DotNetObjectReference` instance.</span></span> <span data-ttu-id="7c9c4-210">La instancia de .NET se pasa por referencia a JavaScript.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-210">The .NET instance is passed by reference to JavaScript.</span></span>
* <span data-ttu-id="7c9c4-211">Invocar métodos de instancia de .NET en la instancia de mediante las funciones `invokeMethod` o `invokeMethodAsync`.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-211">Invoke .NET instance methods on the instance using the `invokeMethod` or `invokeMethodAsync` functions.</span></span> <span data-ttu-id="7c9c4-212">La instancia de .NET también se puede pasar como argumento al invocar otros métodos .NET desde JavaScript.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-212">The .NET instance can also be passed as an argument when invoking other .NET methods from JavaScript.</span></span>

> [!NOTE]
> <span data-ttu-id="7c9c4-213">La aplicación de ejemplo registra mensajes en la consola del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-213">The sample app logs messages to the client-side console.</span></span> <span data-ttu-id="7c9c4-214">En los siguientes ejemplos mostrados por la aplicación de ejemplo, examine la salida de la consola del explorador en las herramientas de desarrollo del explorador.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-214">For the following examples demonstrated by the sample app, examine the browser's console output in the browser's developer tools.</span></span>

<span data-ttu-id="7c9c4-215">Cuando se selecciona el botón **desencadenador de instancia de .net HelloHelper. SayHello** , se llama a `ExampleJsInterop.CallHelloHelperSayHello` y pasa un nombre, `Blazor`, al método.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-215">When the **Trigger .NET instance method HelloHelper.SayHello** button is selected, `ExampleJsInterop.CallHelloHelperSayHello` is called and passes a name, `Blazor`, to the method.</span></span>

<span data-ttu-id="7c9c4-216">*Pages/JsInterop. Razor*:</span><span class="sxs-lookup"><span data-stu-id="7c9c4-216">*Pages/JsInterop.razor*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorWebAssemblySample/Pages/JsInterop.razor?name=snippet_JSInterop3&highlight=8-9)]

<span data-ttu-id="7c9c4-217">`CallHelloHelperSayHello` invoca la función de JavaScript `sayHello` con una nueva instancia de `HelloHelper`.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-217">`CallHelloHelperSayHello` invokes the JavaScript function `sayHello` with a new instance of `HelloHelper`.</span></span>

<span data-ttu-id="7c9c4-218">*JsInteropClasses/ExampleJsInterop. CS*:</span><span class="sxs-lookup"><span data-stu-id="7c9c4-218">*JsInteropClasses/ExampleJsInterop.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=10-16)]

<span data-ttu-id="7c9c4-219">*wwwroot/exampleJsInterop.js*:</span><span class="sxs-lookup"><span data-stu-id="7c9c4-219">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=15-18)]

<span data-ttu-id="7c9c4-220">El nombre se pasa al constructor de `HelloHelper`, que establece la propiedad `HelloHelper.Name`.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-220">The name is passed to `HelloHelper`'s constructor, which sets the `HelloHelper.Name` property.</span></span> <span data-ttu-id="7c9c4-221">Cuando se ejecuta la función de JavaScript `sayHello`, `HelloHelper.SayHello` devuelve el mensaje `Hello, {Name}!`, que la función JavaScript escribe en la consola.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-221">When the JavaScript function `sayHello` is executed, `HelloHelper.SayHello` returns the `Hello, {Name}!` message, which is written to the console by the JavaScript function.</span></span>

<span data-ttu-id="7c9c4-222">*JsInteropClasses/HelloHelper.cs*:</span><span class="sxs-lookup"><span data-stu-id="7c9c4-222">*JsInteropClasses/HelloHelper.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

<span data-ttu-id="7c9c4-223">Salida de la consola en las herramientas de desarrollo web del explorador:</span><span class="sxs-lookup"><span data-stu-id="7c9c4-223">Console output in the browser's web developer tools:</span></span>

```console
Hello, Blazor!
```

## <a name="share-interop-code-in-a-class-library"></a><span data-ttu-id="7c9c4-224">Compartir código de interoperabilidad en una biblioteca de clases</span><span class="sxs-lookup"><span data-stu-id="7c9c4-224">Share interop code in a class library</span></span>

<span data-ttu-id="7c9c4-225">El código de interoperabilidad de JS se puede incluir en una biblioteca de clases, lo que permite compartir el código en un paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-225">JS interop code can be included in a class library, which allows you to share the code in a NuGet package.</span></span>

<span data-ttu-id="7c9c4-226">La biblioteca de clases controla la incrustación de recursos de JavaScript en el ensamblado compilado.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-226">The class library handles embedding JavaScript resources in the built assembly.</span></span> <span data-ttu-id="7c9c4-227">Los archivos de JavaScript se colocan en la carpeta *wwwroot* .</span><span class="sxs-lookup"><span data-stu-id="7c9c4-227">The JavaScript files are placed in the *wwwroot* folder.</span></span> <span data-ttu-id="7c9c4-228">Las herramientas se encargan de insertar los recursos cuando se compila la biblioteca.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-228">The tooling takes care of embedding the resources when the library is built.</span></span>

<span data-ttu-id="7c9c4-229">Se hace referencia al paquete de NuGet compilado en el archivo de proyecto de la aplicación de la misma manera que se hace referencia a cualquier paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-229">The built NuGet package is referenced in the app's project file the same way that any NuGet package is referenced.</span></span> <span data-ttu-id="7c9c4-230">Una vez restaurado el paquete, el código de la aplicación puede llamar a JavaScript C#como si fuera.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-230">After the package is restored, app code can call into JavaScript as if it were C#.</span></span>

<span data-ttu-id="7c9c4-231">Para obtener más información, consulta <xref:blazor/class-libraries>.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-231">For more information, see <xref:blazor/class-libraries>.</span></span>

## <a name="harden-js-interop-calls"></a><span data-ttu-id="7c9c4-232">Reprotección de llamadas de interoperabilidad de JS</span><span class="sxs-lookup"><span data-stu-id="7c9c4-232">Harden JS interop calls</span></span>

<span data-ttu-id="7c9c4-233">La interoperabilidad de JS puede producir un error debido a errores de red y debe tratarse como no confiable.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-233">JS interop may fail due to networking errors and should be treated as unreliable.</span></span> <span data-ttu-id="7c9c4-234">De forma predeterminada, una aplicación de Blazor Server agota el tiempo de espera para las llamadas de interoperabilidad de JS en el servidor después de un minuto.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-234">By default, a Blazor Server app times out JS interop calls on the server after one minute.</span></span> <span data-ttu-id="7c9c4-235">Si una aplicación puede tolerar un tiempo de espera más agresivo, como 10 segundos, establezca el tiempo de espera con uno de los siguientes métodos:</span><span class="sxs-lookup"><span data-stu-id="7c9c4-235">If an app can tolerate a more aggressive timeout, such as 10 seconds, set the timeout using one of the following approaches:</span></span>

* <span data-ttu-id="7c9c4-236">Globalmente en `Startup.ConfigureServices`, especifique el tiempo de espera:</span><span class="sxs-lookup"><span data-stu-id="7c9c4-236">Globally in `Startup.ConfigureServices`, specify the timeout:</span></span>

  ```csharp
  services.AddServerSideBlazor(
      options => options.JSInteropDefaultCallTimeout = TimeSpan.FromSeconds({SECONDS}));
  ```

* <span data-ttu-id="7c9c4-237">Por cada invocación en el código de componente, una sola llamada puede especificar el tiempo de espera:</span><span class="sxs-lookup"><span data-stu-id="7c9c4-237">Per-invocation in component code, a single call can specify the timeout:</span></span>

  ```csharp
  var result = await JSRuntime.InvokeAsync<string>("MyJSOperation", 
      TimeSpan.FromSeconds({SECONDS}), new[] { "Arg1" });
  ```

<span data-ttu-id="7c9c4-238">Para obtener más información sobre el agotamiento de recursos, consulte <xref:security/blazor/server>.</span><span class="sxs-lookup"><span data-stu-id="7c9c4-238">For more information on resource exhaustion, see <xref:security/blazor/server>.</span></span>
