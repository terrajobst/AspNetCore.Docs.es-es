---
title: ASP.NET Core Blazor la interoperabilidad de JavaScript
author: guardrex
description: Obtenga información sobre cómo invocar funciones de JavaScript desde los métodos .NET y .NET desde JavaScript en Blazor aplicaciones.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/16/2019
no-loc:
- Blazor
uid: blazor/javascript-interop
ms.openlocfilehash: 76437ef00e00f5de1b995b4f0b1a09e5876dff8f
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2019
ms.locfileid: "73962844"
---
# <a name="aspnet-core-opno-locblazor-javascript-interop"></a><span data-ttu-id="2d61f-103">ASP.NET Core Blazor la interoperabilidad de JavaScript</span><span class="sxs-lookup"><span data-stu-id="2d61f-103">ASP.NET Core Blazor JavaScript interop</span></span>

<span data-ttu-id="2d61f-104">Por [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27)y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="2d61f-104">By [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="2d61f-105">Una aplicación Blazor puede invocar funciones de JavaScript desde los métodos .NET y .NET desde el código JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2d61f-105">A Blazor app can invoke JavaScript functions from .NET and .NET methods from JavaScript code.</span></span>

<span data-ttu-id="2d61f-106">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2d61f-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="invoke-javascript-functions-from-net-methods"></a><span data-ttu-id="2d61f-107">Invocar funciones de JavaScript desde métodos de .NET</span><span class="sxs-lookup"><span data-stu-id="2d61f-107">Invoke JavaScript functions from .NET methods</span></span>

<span data-ttu-id="2d61f-108">Hay ocasiones en las que se requiere código .NET para llamar a una función de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2d61f-108">There are times when .NET code is required to call a JavaScript function.</span></span> <span data-ttu-id="2d61f-109">Por ejemplo, una llamada de JavaScript puede exponer funciones o funcionalidades del explorador de una biblioteca de JavaScript a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2d61f-109">For example, a JavaScript call can expose browser capabilities or functionality from a JavaScript library to the app.</span></span>

<span data-ttu-id="2d61f-110">Para llamar a JavaScript desde .NET, use la abstracción `IJSRuntime`.</span><span class="sxs-lookup"><span data-stu-id="2d61f-110">To call into JavaScript from .NET, use the `IJSRuntime` abstraction.</span></span> <span data-ttu-id="2d61f-111">El método `InvokeAsync<T>` toma un identificador de la función de JavaScript que desea invocar junto con cualquier número de argumentos serializables de JSON.</span><span class="sxs-lookup"><span data-stu-id="2d61f-111">The `InvokeAsync<T>` method takes an identifier for the JavaScript function that you wish to invoke along with any number of JSON-serializable arguments.</span></span> <span data-ttu-id="2d61f-112">El identificador de función es relativo al ámbito global (`window`).</span><span class="sxs-lookup"><span data-stu-id="2d61f-112">The function identifier is relative to the global scope (`window`).</span></span> <span data-ttu-id="2d61f-113">Si desea llamar a `window.someScope.someFunction`, el identificador es `someScope.someFunction`.</span><span class="sxs-lookup"><span data-stu-id="2d61f-113">If you wish to call `window.someScope.someFunction`, the identifier is `someScope.someFunction`.</span></span> <span data-ttu-id="2d61f-114">No es necesario registrar la función antes de que se le llame.</span><span class="sxs-lookup"><span data-stu-id="2d61f-114">There's no need to register the function before it's called.</span></span> <span data-ttu-id="2d61f-115">El tipo de valor devuelto `T` también debe ser serializable.</span><span class="sxs-lookup"><span data-stu-id="2d61f-115">The return type `T` must also be JSON serializable.</span></span>

<span data-ttu-id="2d61f-116">Para las aplicaciones de Blazor Server:</span><span class="sxs-lookup"><span data-stu-id="2d61f-116">For Blazor Server apps:</span></span>

* <span data-ttu-id="2d61f-117">La aplicación de servidor de Blazor procesa varias solicitudes de usuario.</span><span class="sxs-lookup"><span data-stu-id="2d61f-117">Multiple user requests are processed by the Blazor Server app.</span></span> <span data-ttu-id="2d61f-118">No llame a `JSRuntime.Current` en un componente para invocar funciones de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2d61f-118">Don't call `JSRuntime.Current` in a component to invoke JavaScript functions.</span></span>
* <span data-ttu-id="2d61f-119">Inserte la abstracción `IJSRuntime` y utilice el objeto insertado para emitir llamadas de interoperabilidad de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2d61f-119">Inject the `IJSRuntime` abstraction and use the injected object to issue JavaScript interop calls.</span></span>
* <span data-ttu-id="2d61f-120">Aunque una aplicación Blazor se está preprocesando, no es posible llamar a JavaScript porque no se ha establecido una conexión con el explorador.</span><span class="sxs-lookup"><span data-stu-id="2d61f-120">While a Blazor app is prerendering, calling into JavaScript isn't possible because a connection with the browser hasn't been established.</span></span> <span data-ttu-id="2d61f-121">Para obtener más información, consulte la sección [detectar cuándo se está representando una aplicación Blazor](#detect-when-a-blazor-app-is-prerendering) .</span><span class="sxs-lookup"><span data-stu-id="2d61f-121">For more information, see the [Detect when a Blazor app is prerendering](#detect-when-a-blazor-app-is-prerendering) section.</span></span>

<span data-ttu-id="2d61f-122">El ejemplo siguiente se basa en [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), un descodificador basado en JavaScript experimental.</span><span class="sxs-lookup"><span data-stu-id="2d61f-122">The following example is based on [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), an experimental JavaScript-based decoder.</span></span> <span data-ttu-id="2d61f-123">En el ejemplo se muestra cómo invocar una función de C# JavaScript desde un método.</span><span class="sxs-lookup"><span data-stu-id="2d61f-123">The example demonstrates how to invoke a JavaScript function from a C# method.</span></span> <span data-ttu-id="2d61f-124">La función de JavaScript acepta una matriz de bytes de un C# método, descodifica la matriz y devuelve el texto al componente que se va a mostrar.</span><span class="sxs-lookup"><span data-stu-id="2d61f-124">The JavaScript function accepts a byte array from a C# method, decodes the array, and returns the text to the component for display.</span></span>

<span data-ttu-id="2d61f-125">Dentro del elemento `<head>` de *wwwroot/index.html* (Blazor webassembly) o *pages/_Host. cshtml* (Blazor Server), proporcione una función de JavaScript que use `TextDecoder` para descodificar una matriz pasada y devolver el valor descodificado:</span><span class="sxs-lookup"><span data-stu-id="2d61f-125">Inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server), provide a JavaScript function that uses `TextDecoder` to decode a passed array and return the decoded value:</span></span>

[!code-html[](javascript-interop/samples_snapshot/index-script-convertarray.html)]

<span data-ttu-id="2d61f-126">El código JavaScript, como el código que se muestra en el ejemplo anterior, también se puede cargar desde un archivo JavaScript ( *. js*) con una referencia al archivo de script:</span><span class="sxs-lookup"><span data-stu-id="2d61f-126">JavaScript code, such as the code shown in the preceding example, can also be loaded from a JavaScript file (*.js*) with a reference to the script file:</span></span>

```html
<script src="exampleJsInterop.js"></script>
```

<span data-ttu-id="2d61f-127">El siguiente componente:</span><span class="sxs-lookup"><span data-stu-id="2d61f-127">The following component:</span></span>

* <span data-ttu-id="2d61f-128">Invoca la función `convertArray` JavaScript mediante `JSRuntime` cuando se selecciona un botón de componente (**convertir matriz**).</span><span class="sxs-lookup"><span data-stu-id="2d61f-128">Invokes the `convertArray` JavaScript function using `JSRuntime` when a component button (**Convert Array**) is selected.</span></span>
* <span data-ttu-id="2d61f-129">Después de llamar a la función de JavaScript, la matriz que se pasa se convierte en una cadena.</span><span class="sxs-lookup"><span data-stu-id="2d61f-129">After the JavaScript function is called, the passed array is converted into a string.</span></span> <span data-ttu-id="2d61f-130">La cadena se devuelve al componente para su presentación.</span><span class="sxs-lookup"><span data-stu-id="2d61f-130">The string is returned to the component for display.</span></span>

[!code-cshtml[](javascript-interop/samples_snapshot/call-js-example.razor?highlight=2,34-35)]

##  <a name="use-of-ijsruntime"></a><span data-ttu-id="2d61f-131">Uso de IJSRuntime</span><span class="sxs-lookup"><span data-stu-id="2d61f-131">Use of IJSRuntime</span></span>

<span data-ttu-id="2d61f-132">Para usar la abstracción `IJSRuntime`, adopte cualquiera de los métodos siguientes:</span><span class="sxs-lookup"><span data-stu-id="2d61f-132">To use the `IJSRuntime` abstraction, adopt any of the following approaches:</span></span>

* <span data-ttu-id="2d61f-133">Inserte la abstracción `IJSRuntime` en el componente de Razor ( *. Razor*):</span><span class="sxs-lookup"><span data-stu-id="2d61f-133">Inject the `IJSRuntime` abstraction into the Razor component (*.razor*):</span></span>

  [!code-cshtml[](javascript-interop/samples_snapshot/inject-abstraction.razor?highlight=1)]

  <span data-ttu-id="2d61f-134">Dentro del elemento `<head>` de *wwwroot/index.html* (Blazor webassembly) o *pages/_Host. cshtml* (Blazor Server), proporcione una función `handleTickerChanged` JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2d61f-134">Inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server), provide a `handleTickerChanged` JavaScript function.</span></span> <span data-ttu-id="2d61f-135">Se llama a la función con `IJSRuntime.InvokeVoidAsync` y no devuelve un valor:</span><span class="sxs-lookup"><span data-stu-id="2d61f-135">The function is called with `IJSRuntime.InvokeVoidAsync` and doesn't return a value:</span></span>

  [!code-html[](javascript-interop/samples_snapshot/index-script-handleTickerChanged1.html)]

* <span data-ttu-id="2d61f-136">Inserte la abstracción `IJSRuntime` en una clase ( *. CS*):</span><span class="sxs-lookup"><span data-stu-id="2d61f-136">Inject the `IJSRuntime` abstraction into a class (*.cs*):</span></span>

  [!code-csharp[](javascript-interop/samples_snapshot/inject-abstraction-class.cs?highlight=5)]

  <span data-ttu-id="2d61f-137">Dentro del elemento `<head>` de *wwwroot/index.html* (Blazor webassembly) o *pages/_Host. cshtml* (Blazor Server), proporcione una función `handleTickerChanged` JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2d61f-137">Inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server), provide a `handleTickerChanged` JavaScript function.</span></span> <span data-ttu-id="2d61f-138">Se llama a la función con `JSRuntime.InvokeAsync` y devuelve un valor:</span><span class="sxs-lookup"><span data-stu-id="2d61f-138">The function is called with `JSRuntime.InvokeAsync` and returns a value:</span></span>

  [!code-html[](javascript-interop/samples_snapshot/index-script-handleTickerChanged2.html)]

* <span data-ttu-id="2d61f-139">Para la generación de contenido dinámico con [BuildRenderTree](xref:blazor/components#manual-rendertreebuilder-logic), use el atributo `[Inject]`:</span><span class="sxs-lookup"><span data-stu-id="2d61f-139">For dynamic content generation with [BuildRenderTree](xref:blazor/components#manual-rendertreebuilder-logic), use the `[Inject]` attribute:</span></span>

  ```csharp
  [Inject]
  IJSRuntime JSRuntime { get; set; }
  ```

<span data-ttu-id="2d61f-140">En la aplicación de ejemplo del lado cliente que acompaña a este tema, hay dos funciones de JavaScript disponibles para la aplicación que interactúan con el DOM para recibir datos proporcionados por el usuario y mostrar un mensaje de bienvenida:</span><span class="sxs-lookup"><span data-stu-id="2d61f-140">In the client-side sample app that accompanies this topic, two JavaScript functions are available to the app that interact with the DOM to receive user input and display a welcome message:</span></span>

* <span data-ttu-id="2d61f-141">`showPrompt` &ndash; genera un mensaje para aceptar la entrada del usuario (el nombre del usuario) y devuelve el nombre al autor de la llamada.</span><span class="sxs-lookup"><span data-stu-id="2d61f-141">`showPrompt` &ndash; Produces a prompt to accept user input (the user's name) and returns the name to the caller.</span></span>
* <span data-ttu-id="2d61f-142">`displayWelcome` &ndash; asigna un mensaje de bienvenida del autor de la llamada a un objeto DOM con un `id` de `welcome`.</span><span class="sxs-lookup"><span data-stu-id="2d61f-142">`displayWelcome` &ndash; Assigns a welcome message from the caller to a DOM object with an `id` of `welcome`.</span></span>

<span data-ttu-id="2d61f-143">*wwwroot/exampleJsInterop. js*:</span><span class="sxs-lookup"><span data-stu-id="2d61f-143">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=2-7)]

<span data-ttu-id="2d61f-144">Coloque la etiqueta de `<script>` que hace referencia al archivo JavaScript en el archivo *wwwroot/index.html* (Blazor webassembly) o el archivo *pages/_Host. cshtml* (Blazor Server).</span><span class="sxs-lookup"><span data-stu-id="2d61f-144">Place the `<script>` tag that references the JavaScript file in the *wwwroot/index.html* file (Blazor WebAssembly) or *Pages/_Host.cshtml* file (Blazor Server).</span></span>

<span data-ttu-id="2d61f-145">*wwwroot/index.html* (Blazor webassembly):</span><span class="sxs-lookup"><span data-stu-id="2d61f-145">*wwwroot/index.html* (Blazor WebAssembly):</span></span>

[!code-html[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/index.html?highlight=15)]

<span data-ttu-id="2d61f-146">*Pages/_Host. cshtml* (Blazor Server):</span><span class="sxs-lookup"><span data-stu-id="2d61f-146">*Pages/_Host.cshtml* (Blazor Server):</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorServerSample/Pages/_Host.cshtml?highlight=21)]

<span data-ttu-id="2d61f-147">No coloque una etiqueta `<script>` en un archivo de componente porque la etiqueta `<script>` no se puede actualizar dinámicamente.</span><span class="sxs-lookup"><span data-stu-id="2d61f-147">Don't place a `<script>` tag in a component file because the `<script>` tag can't be updated dynamically.</span></span>

<span data-ttu-id="2d61f-148">Los métodos de .NET interoperan con las funciones de JavaScript en el archivo *exampleJsInterop. js* llamando a `IJSRuntime.InvokeAsync<T>`.</span><span class="sxs-lookup"><span data-stu-id="2d61f-148">.NET methods interop with the JavaScript functions in the *exampleJsInterop.js* file by calling `IJSRuntime.InvokeAsync<T>`.</span></span>

<span data-ttu-id="2d61f-149">La abstracción `IJSRuntime` es asincrónica para permitir escenarios de Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="2d61f-149">The `IJSRuntime` abstraction is asynchronous to allow for Blazor Server scenarios.</span></span> <span data-ttu-id="2d61f-150">Si la aplicación es una aplicación Blazor webassembly y desea invocar una función de JavaScript sincrónicamente, se puede convertir en `IJSInProcessRuntime` y llamar a `Invoke<T>` en su lugar.</span><span class="sxs-lookup"><span data-stu-id="2d61f-150">If the app is a Blazor WebAssembly app and you want to invoke a JavaScript function synchronously, downcast to `IJSInProcessRuntime` and call `Invoke<T>` instead.</span></span> <span data-ttu-id="2d61f-151">Se recomienda que la mayoría de las bibliotecas de interoperabilidad de JavaScript usen las API asincrónicas para asegurarse de que las bibliotecas están disponibles en todos los escenarios.</span><span class="sxs-lookup"><span data-stu-id="2d61f-151">We recommend that most JavaScript interop libraries use the async APIs to ensure that the libraries are available in all scenarios.</span></span>

<span data-ttu-id="2d61f-152">La aplicación de ejemplo incluye un componente para mostrar la interoperabilidad de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2d61f-152">The sample app includes a component to demonstrate JavaScript interop.</span></span> <span data-ttu-id="2d61f-153">El componente:</span><span class="sxs-lookup"><span data-stu-id="2d61f-153">The component:</span></span>

* <span data-ttu-id="2d61f-154">Recibe la entrada del usuario a través de un mensaje de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2d61f-154">Receives user input via a JavaScript prompt.</span></span>
* <span data-ttu-id="2d61f-155">Devuelve el texto del componente que se va a procesar.</span><span class="sxs-lookup"><span data-stu-id="2d61f-155">Returns the text to the component for processing.</span></span>
* <span data-ttu-id="2d61f-156">Llama a una segunda función de JavaScript que interactúa con el DOM para mostrar un mensaje de bienvenida.</span><span class="sxs-lookup"><span data-stu-id="2d61f-156">Calls a second JavaScript function that interacts with the DOM to display a welcome message.</span></span>

<span data-ttu-id="2d61f-157">*Pages/JSInterop. Razor*:</span><span class="sxs-lookup"><span data-stu-id="2d61f-157">*Pages/JSInterop.razor*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorWebAssemblySample/Pages/JsInterop.razor?name=snippet_JSInterop1&highlight=3,19-21,23-25)]

1. <span data-ttu-id="2d61f-158">Cuando se ejecuta `TriggerJsPrompt` seleccionando el botón **desencadenador de JavaScript** del componente, se llama a la función JavaScript `showPrompt` proporcionada en el archivo *wwwroot/exampleJsInterop. js* .</span><span class="sxs-lookup"><span data-stu-id="2d61f-158">When `TriggerJsPrompt` is executed by selecting the component's **Trigger JavaScript Prompt** button, the JavaScript `showPrompt` function provided in the *wwwroot/exampleJsInterop.js* file is called.</span></span>
1. <span data-ttu-id="2d61f-159">La función `showPrompt` acepta datos proporcionados por el usuario (el nombre del usuario), que se codifica en HTML y se devuelve al componente.</span><span class="sxs-lookup"><span data-stu-id="2d61f-159">The `showPrompt` function accepts user input (the user's name), which is HTML-encoded and returned to the component.</span></span> <span data-ttu-id="2d61f-160">El componente almacena el nombre del usuario en una variable local, `name`.</span><span class="sxs-lookup"><span data-stu-id="2d61f-160">The component stores the user's name in a local variable, `name`.</span></span>
1. <span data-ttu-id="2d61f-161">La cadena almacenada en `name` se incorpora en un mensaje de bienvenida, que se pasa a una función de JavaScript, `displayWelcome`, que representa el mensaje de bienvenida en una etiqueta de encabezado.</span><span class="sxs-lookup"><span data-stu-id="2d61f-161">The string stored in `name` is incorporated into a welcome message, which is passed to a JavaScript function, `displayWelcome`, which renders the welcome message into a heading tag.</span></span>

## <a name="call-a-void-javascript-function"></a><span data-ttu-id="2d61f-162">Llamar a una función de JavaScript void</span><span class="sxs-lookup"><span data-stu-id="2d61f-162">Call a void JavaScript function</span></span>

<span data-ttu-id="2d61f-163">Se llama a las funciones de JavaScript que devuelven [void (0)/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void) o [undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) con `IJSRuntime.InvokeVoidAsync`.</span><span class="sxs-lookup"><span data-stu-id="2d61f-163">JavaScript functions that return [void(0)/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void) or [undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) are called with `IJSRuntime.InvokeVoidAsync`.</span></span>

## <a name="detect-when-a-opno-locblazor-app-is-prerendering"></a><span data-ttu-id="2d61f-164">Detección cuando una aplicación Blazor se está preprocesando</span><span class="sxs-lookup"><span data-stu-id="2d61f-164">Detect when a Blazor app is prerendering</span></span>
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

## <a name="capture-references-to-elements"></a><span data-ttu-id="2d61f-165">Capturar referencias a elementos</span><span class="sxs-lookup"><span data-stu-id="2d61f-165">Capture references to elements</span></span>

<span data-ttu-id="2d61f-166">Algunos escenarios de [interoperabilidad de JavaScript](xref:blazor/javascript-interop) requieren referencias a elementos HTML.</span><span class="sxs-lookup"><span data-stu-id="2d61f-166">Some [JavaScript interop](xref:blazor/javascript-interop) scenarios require references to HTML elements.</span></span> <span data-ttu-id="2d61f-167">Por ejemplo, una biblioteca de interfaz de usuario puede requerir una referencia de elemento para la inicialización, o puede que necesite llamar a las API de tipo de comando en un elemento, como `focus` o `play`.</span><span class="sxs-lookup"><span data-stu-id="2d61f-167">For example, a UI library may require an element reference for initialization, or you might need to call command-like APIs on an element, such as `focus` or `play`.</span></span>

<span data-ttu-id="2d61f-168">Capture las referencias a los elementos HTML de un componente con el siguiente enfoque:</span><span class="sxs-lookup"><span data-stu-id="2d61f-168">Capture references to HTML elements in a component using the following approach:</span></span>

* <span data-ttu-id="2d61f-169">Agregue un atributo `@ref` al elemento HTML.</span><span class="sxs-lookup"><span data-stu-id="2d61f-169">Add an `@ref` attribute to the HTML element.</span></span>
* <span data-ttu-id="2d61f-170">Defina un campo de tipo `ElementReference` cuyo nombre coincida con el valor del atributo `@ref`.</span><span class="sxs-lookup"><span data-stu-id="2d61f-170">Define a field of type `ElementReference` whose name matches the value of the `@ref` attribute.</span></span>

<span data-ttu-id="2d61f-171">En el ejemplo siguiente se muestra la captura de una referencia al elemento `username` `<input>`:</span><span class="sxs-lookup"><span data-stu-id="2d61f-171">The following example shows capturing a reference to the `username` `<input>` element:</span></span>

```cshtml
<input @ref="username" ... />

@code {
    ElementReference username;
}
```

> [!NOTE]
> <span data-ttu-id="2d61f-172">**No** utilice referencias a elementos capturados como forma de rellenar el Dom.</span><span class="sxs-lookup"><span data-stu-id="2d61f-172">Do **not** use captured element references as a way of populating the DOM.</span></span> <span data-ttu-id="2d61f-173">Si lo hace, puede interferir con el modelo de representación declarativa.</span><span class="sxs-lookup"><span data-stu-id="2d61f-173">Doing so may interfere with the declarative rendering model.</span></span>

<span data-ttu-id="2d61f-174">En lo que respecta al código de .NET, un `ElementReference` es un identificador opaco.</span><span class="sxs-lookup"><span data-stu-id="2d61f-174">As far as .NET code is concerned, an `ElementReference` is an opaque handle.</span></span> <span data-ttu-id="2d61f-175">Lo *único* que puede hacer con `ElementReference` es pasarlo a código JavaScript a través de la interoperabilidad de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2d61f-175">The *only* thing you can do with `ElementReference` is pass it through to JavaScript code via JavaScript interop.</span></span> <span data-ttu-id="2d61f-176">Al hacerlo, el código de JavaScript recibe una instancia `HTMLElement`, que puede usar con las API de DOM normales.</span><span class="sxs-lookup"><span data-stu-id="2d61f-176">When you do so, the JavaScript-side code receives an `HTMLElement` instance, which it can use with normal DOM APIs.</span></span>

<span data-ttu-id="2d61f-177">Por ejemplo, el código siguiente define un método de extensión de .NET que permite establecer el foco en un elemento:</span><span class="sxs-lookup"><span data-stu-id="2d61f-177">For example, the following code defines a .NET extension method that enables setting the focus on an element:</span></span>

<span data-ttu-id="2d61f-178">*exampleJsInterop. js*:</span><span class="sxs-lookup"><span data-stu-id="2d61f-178">*exampleJsInterop.js*:</span></span>

```javascript
window.exampleJsFunctions = {
  focusElement : function (element) {
    element.focus();
  }
}
```

<span data-ttu-id="2d61f-179">Use `IJSRuntime.InvokeAsync<T>` y llame a `exampleJsFunctions.focusElement` con un `ElementReference` para centrar un elemento:</span><span class="sxs-lookup"><span data-stu-id="2d61f-179">Use `IJSRuntime.InvokeAsync<T>` and call `exampleJsFunctions.focusElement` with an `ElementReference` to focus an element:</span></span>

[!code-cshtml[](javascript-interop/samples_snapshot/component1.razor?highlight=1,3,11-12)]

<span data-ttu-id="2d61f-180">Para usar un método de extensión para centrar un elemento, cree un método de extensión estático que reciba la instancia `IJSRuntime`:</span><span class="sxs-lookup"><span data-stu-id="2d61f-180">To use an extension method to focus an element, create a static extension method that receives the `IJSRuntime` instance:</span></span>

```csharp
public static Task Focus(this ElementReference elementRef, IJSRuntime jsRuntime)
{
    return jsRuntime.InvokeAsync<object>(
        "exampleJsFunctions.focusElement", elementRef);
}
```

<span data-ttu-id="2d61f-181">Se llama al método directamente en el objeto.</span><span class="sxs-lookup"><span data-stu-id="2d61f-181">The method is called directly on the object.</span></span> <span data-ttu-id="2d61f-182">En el ejemplo siguiente se da por supuesto que el método estático `Focus` está disponible en el espacio de nombres `JsInteropClasses`:</span><span class="sxs-lookup"><span data-stu-id="2d61f-182">The following example assumes that the static `Focus` method is available from the `JsInteropClasses` namespace:</span></span>

[!code-cshtml[](javascript-interop/samples_snapshot/component2.razor?highlight=1,4,12)]

> [!IMPORTANT]
> <span data-ttu-id="2d61f-183">La variable `username` solo se rellena después de representar el componente.</span><span class="sxs-lookup"><span data-stu-id="2d61f-183">The `username` variable is only populated after the component is rendered.</span></span> <span data-ttu-id="2d61f-184">Si se pasa un `ElementReference` sin rellenar al código JavaScript, el código JavaScript recibe un valor de `null`.</span><span class="sxs-lookup"><span data-stu-id="2d61f-184">If an unpopulated `ElementReference` is passed to JavaScript code, the JavaScript code receives a value of `null`.</span></span> <span data-ttu-id="2d61f-185">Para manipular las referencias de elemento una vez finalizada la representación del componente (para establecer el foco inicial en un elemento), use los métodos de ciclo de [vida del componente](xref:blazor/components#lifecycle-methods)`OnAfterRenderAsync` o `OnAfterRender`.</span><span class="sxs-lookup"><span data-stu-id="2d61f-185">To manipulate element references after the component has finished rendering (to set the initial focus on an element) use the `OnAfterRenderAsync` or `OnAfterRender` [component lifecycle methods](xref:blazor/components#lifecycle-methods).</span></span>

## <a name="invoke-net-methods-from-javascript-functions"></a><span data-ttu-id="2d61f-186">Invocar métodos .NET desde funciones de JavaScript</span><span class="sxs-lookup"><span data-stu-id="2d61f-186">Invoke .NET methods from JavaScript functions</span></span>

### <a name="static-net-method-call"></a><span data-ttu-id="2d61f-187">Llamada al método estático de .NET</span><span class="sxs-lookup"><span data-stu-id="2d61f-187">Static .NET method call</span></span>

<span data-ttu-id="2d61f-188">Para invocar un método .NET estático desde JavaScript, use las funciones `DotNet.invokeMethod` o `DotNet.invokeMethodAsync`.</span><span class="sxs-lookup"><span data-stu-id="2d61f-188">To invoke a static .NET method from JavaScript, use the `DotNet.invokeMethod` or `DotNet.invokeMethodAsync` functions.</span></span> <span data-ttu-id="2d61f-189">Pase el identificador del método estático al que desea llamar, el nombre del ensamblado que contiene la función y cualquier argumento.</span><span class="sxs-lookup"><span data-stu-id="2d61f-189">Pass in the identifier of the static method you wish to call, the name of the assembly containing the function, and any arguments.</span></span> <span data-ttu-id="2d61f-190">La versión asincrónica es preferible para admitir escenarios de Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="2d61f-190">The asynchronous version is preferred to support Blazor Server scenarios.</span></span> <span data-ttu-id="2d61f-191">Para invocar un método .NET desde JavaScript, el método .NET debe ser público, estático y tener el atributo `[JSInvokable]`.</span><span class="sxs-lookup"><span data-stu-id="2d61f-191">To invoke a .NET method from JavaScript, the .NET method must be public, static, and have the `[JSInvokable]` attribute.</span></span> <span data-ttu-id="2d61f-192">De forma predeterminada, el identificador del método es el nombre del método, pero puede especificar un identificador diferente mediante el constructor `JSInvokableAttribute`.</span><span class="sxs-lookup"><span data-stu-id="2d61f-192">By default, the method identifier is the method name, but you can specify a different identifier using the `JSInvokableAttribute` constructor.</span></span> <span data-ttu-id="2d61f-193">Actualmente no se admite la llamada a métodos genéricos abiertos.</span><span class="sxs-lookup"><span data-stu-id="2d61f-193">Calling open generic methods isn't currently supported.</span></span>

<span data-ttu-id="2d61f-194">La aplicación de ejemplo incluye C# un método para devolver una matriz de `int`S.</span><span class="sxs-lookup"><span data-stu-id="2d61f-194">The sample app includes a C# method to return an array of `int`s.</span></span> <span data-ttu-id="2d61f-195">El atributo `JSInvokable` se aplica al método.</span><span class="sxs-lookup"><span data-stu-id="2d61f-195">The `JSInvokable` attribute is applied to the method.</span></span>

<span data-ttu-id="2d61f-196">*Pages/JsInterop. Razor*:</span><span class="sxs-lookup"><span data-stu-id="2d61f-196">*Pages/JsInterop.razor*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorWebAssemblySample/Pages/JsInterop.razor?name=snippet_JSInterop2&highlight=7-11)]

<span data-ttu-id="2d61f-197">JavaScript atendido al cliente invoca el C# método .net.</span><span class="sxs-lookup"><span data-stu-id="2d61f-197">JavaScript served to the client invokes the C# .NET method.</span></span>

<span data-ttu-id="2d61f-198">*wwwroot/exampleJsInterop. js*:</span><span class="sxs-lookup"><span data-stu-id="2d61f-198">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=8-14)]

<span data-ttu-id="2d61f-199">Cuando se seleccione el botón **desencadenador de método estático de .net ReturnArrayAsync** , examine la salida de la consola en las herramientas de desarrollo web del explorador.</span><span class="sxs-lookup"><span data-stu-id="2d61f-199">When the **Trigger .NET static method ReturnArrayAsync** button is selected, examine the console output in the browser's web developer tools.</span></span>

<span data-ttu-id="2d61f-200">La salida de la consola es:</span><span class="sxs-lookup"><span data-stu-id="2d61f-200">The console output is:</span></span>

```console
Array(4) [ 1, 2, 3, 4 ]
```

<span data-ttu-id="2d61f-201">El cuarto valor de matriz se inserta en la matriz (`data.push(4);`) devuelta por `ReturnArrayAsync`.</span><span class="sxs-lookup"><span data-stu-id="2d61f-201">The fourth array value is pushed to the array (`data.push(4);`) returned by `ReturnArrayAsync`.</span></span>

### <a name="instance-method-call"></a><span data-ttu-id="2d61f-202">Llamada al método de instancia</span><span class="sxs-lookup"><span data-stu-id="2d61f-202">Instance method call</span></span>

<span data-ttu-id="2d61f-203">También puede llamar a métodos de instancia de .NET desde JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2d61f-203">You can also call .NET instance methods from JavaScript.</span></span> <span data-ttu-id="2d61f-204">Para invocar un método de instancia de .NET desde JavaScript:</span><span class="sxs-lookup"><span data-stu-id="2d61f-204">To invoke a .NET instance method from JavaScript:</span></span>

* <span data-ttu-id="2d61f-205">Pase la instancia de .NET a JavaScript ajustándola en una instancia de `DotNetObjectReference`.</span><span class="sxs-lookup"><span data-stu-id="2d61f-205">Pass the .NET instance to JavaScript by wrapping it in a `DotNetObjectReference` instance.</span></span> <span data-ttu-id="2d61f-206">La instancia de .NET se pasa por referencia a JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2d61f-206">The .NET instance is passed by reference to JavaScript.</span></span>
* <span data-ttu-id="2d61f-207">Invocar métodos de instancia de .NET en la instancia de mediante las funciones `invokeMethod` o `invokeMethodAsync`.</span><span class="sxs-lookup"><span data-stu-id="2d61f-207">Invoke .NET instance methods on the instance using the `invokeMethod` or `invokeMethodAsync` functions.</span></span> <span data-ttu-id="2d61f-208">La instancia de .NET también se puede pasar como argumento al invocar otros métodos .NET desde JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2d61f-208">The .NET instance can also be passed as an argument when invoking other .NET methods from JavaScript.</span></span>

> [!NOTE]
> <span data-ttu-id="2d61f-209">La aplicación de ejemplo registra mensajes en la consola del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="2d61f-209">The sample app logs messages to the client-side console.</span></span> <span data-ttu-id="2d61f-210">En los siguientes ejemplos mostrados por la aplicación de ejemplo, examine la salida de la consola del explorador en las herramientas de desarrollo del explorador.</span><span class="sxs-lookup"><span data-stu-id="2d61f-210">For the following examples demonstrated by the sample app, examine the browser's console output in the browser's developer tools.</span></span>

<span data-ttu-id="2d61f-211">Cuando se selecciona el botón **desencadenador de instancia de .net HelloHelper. SayHello** , se llama a `ExampleJsInterop.CallHelloHelperSayHello` y pasa un nombre, `Blazor`, al método.</span><span class="sxs-lookup"><span data-stu-id="2d61f-211">When the **Trigger .NET instance method HelloHelper.SayHello** button is selected, `ExampleJsInterop.CallHelloHelperSayHello` is called and passes a name, `Blazor`, to the method.</span></span>

<span data-ttu-id="2d61f-212">*Pages/JsInterop. Razor*:</span><span class="sxs-lookup"><span data-stu-id="2d61f-212">*Pages/JsInterop.razor*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorWebAssemblySample/Pages/JsInterop.razor?name=snippet_JSInterop3&highlight=8-9)]

<span data-ttu-id="2d61f-213">`CallHelloHelperSayHello` invoca la función de JavaScript `sayHello` con una nueva instancia de `HelloHelper`.</span><span class="sxs-lookup"><span data-stu-id="2d61f-213">`CallHelloHelperSayHello` invokes the JavaScript function `sayHello` with a new instance of `HelloHelper`.</span></span>

<span data-ttu-id="2d61f-214">*JsInteropClasses/ExampleJsInterop. CS*:</span><span class="sxs-lookup"><span data-stu-id="2d61f-214">*JsInteropClasses/ExampleJsInterop.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=10-16)]

<span data-ttu-id="2d61f-215">*wwwroot/exampleJsInterop. js*:</span><span class="sxs-lookup"><span data-stu-id="2d61f-215">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=15-18)]

<span data-ttu-id="2d61f-216">El nombre se pasa al constructor de `HelloHelper`, que establece la propiedad `HelloHelper.Name`.</span><span class="sxs-lookup"><span data-stu-id="2d61f-216">The name is passed to `HelloHelper`'s constructor, which sets the `HelloHelper.Name` property.</span></span> <span data-ttu-id="2d61f-217">Cuando se ejecuta la función de JavaScript `sayHello`, `HelloHelper.SayHello` devuelve el mensaje `Hello, {Name}!`, que la función JavaScript escribe en la consola.</span><span class="sxs-lookup"><span data-stu-id="2d61f-217">When the JavaScript function `sayHello` is executed, `HelloHelper.SayHello` returns the `Hello, {Name}!` message, which is written to the console by the JavaScript function.</span></span>

<span data-ttu-id="2d61f-218">*JsInteropClasses/HelloHelper. CS*:</span><span class="sxs-lookup"><span data-stu-id="2d61f-218">*JsInteropClasses/HelloHelper.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

<span data-ttu-id="2d61f-219">Salida de la consola en las herramientas de desarrollo web del explorador:</span><span class="sxs-lookup"><span data-stu-id="2d61f-219">Console output in the browser's web developer tools:</span></span>

```console
Hello, Blazor!
```

## <a name="share-interop-code-in-a-class-library"></a><span data-ttu-id="2d61f-220">Compartir código de interoperabilidad en una biblioteca de clases</span><span class="sxs-lookup"><span data-stu-id="2d61f-220">Share interop code in a class library</span></span>

<span data-ttu-id="2d61f-221">El código de interoperabilidad de JavaScript se puede incluir en una biblioteca de clases, lo que permite compartir el código en un paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="2d61f-221">JavaScript interop code can be included in a class library, which allows you to share the code in a NuGet package.</span></span>

<span data-ttu-id="2d61f-222">La biblioteca de clases controla la incrustación de recursos de JavaScript en el ensamblado compilado.</span><span class="sxs-lookup"><span data-stu-id="2d61f-222">The class library handles embedding JavaScript resources in the built assembly.</span></span> <span data-ttu-id="2d61f-223">Los archivos de JavaScript se colocan en la carpeta *wwwroot* .</span><span class="sxs-lookup"><span data-stu-id="2d61f-223">The JavaScript files are placed in the *wwwroot* folder.</span></span> <span data-ttu-id="2d61f-224">Las herramientas se encargan de insertar los recursos cuando se compila la biblioteca.</span><span class="sxs-lookup"><span data-stu-id="2d61f-224">The tooling takes care of embedding the resources when the library is built.</span></span>

<span data-ttu-id="2d61f-225">Se hace referencia al paquete de NuGet compilado en el archivo de proyecto de la aplicación de la misma manera que se hace referencia a cualquier paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="2d61f-225">The built NuGet package is referenced in the app's project file the same way that any NuGet package is referenced.</span></span> <span data-ttu-id="2d61f-226">Una vez restaurado el paquete, el código de la aplicación puede llamar a JavaScript C#como si fuera.</span><span class="sxs-lookup"><span data-stu-id="2d61f-226">After the package is restored, app code can call into JavaScript as if it were C#.</span></span>

<span data-ttu-id="2d61f-227">Para obtener más información, vea <xref:blazor/class-libraries>.</span><span class="sxs-lookup"><span data-stu-id="2d61f-227">For more information, see <xref:blazor/class-libraries>.</span></span>

## <a name="harden-js-interop-calls"></a><span data-ttu-id="2d61f-228">Reprotección de llamadas de interoperabilidad de JS</span><span class="sxs-lookup"><span data-stu-id="2d61f-228">Harden JS interop calls</span></span>

<span data-ttu-id="2d61f-229">La interoperabilidad de JS puede producir un error debido a errores de red y debe tratarse como no confiable.</span><span class="sxs-lookup"><span data-stu-id="2d61f-229">JS interop may fail due to networking errors and should be treated as unreliable.</span></span> <span data-ttu-id="2d61f-230">De forma predeterminada, una aplicación de Blazor Server agota el tiempo de espera para las llamadas de interoperabilidad de JS en el servidor después de un minuto.</span><span class="sxs-lookup"><span data-stu-id="2d61f-230">By default, a Blazor Server app times out JS interop calls on the server after one minute.</span></span> <span data-ttu-id="2d61f-231">Si una aplicación puede tolerar un tiempo de espera más agresivo, como 10 segundos, establezca el tiempo de espera con uno de los siguientes métodos:</span><span class="sxs-lookup"><span data-stu-id="2d61f-231">If an app can tolerate a more aggressive timeout, such as 10 seconds, set the timeout using one of the following approaches:</span></span>

* <span data-ttu-id="2d61f-232">Globalmente en `Startup.ConfigureServices`, especifique el tiempo de espera:</span><span class="sxs-lookup"><span data-stu-id="2d61f-232">Globally in `Startup.ConfigureServices`, specify the timeout:</span></span>

  ```csharp
  services.AddServerSideBlazor(
      options => options.JSInteropDefaultCallTimeout = TimeSpan.FromSeconds({SECONDS}));
  ```

* <span data-ttu-id="2d61f-233">Por cada invocación en el código de componente, una sola llamada puede especificar el tiempo de espera:</span><span class="sxs-lookup"><span data-stu-id="2d61f-233">Per-invocation in component code, a single call can specify the timeout:</span></span>

  ```csharp
  var result = await JSRuntime.InvokeAsync<string>("MyJSOperation", 
      TimeSpan.FromSeconds({SECONDS}), new[] { "Arg1" });
  ```

<span data-ttu-id="2d61f-234">Para obtener más información sobre el agotamiento de recursos, consulte <xref:security/blazor/server>.</span><span class="sxs-lookup"><span data-stu-id="2d61f-234">For more information on resource exhaustion, see <xref:security/blazor/server>.</span></span>
