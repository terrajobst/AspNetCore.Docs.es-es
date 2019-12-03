---
title: ASP.NET Core Blazor la interoperabilidad de JavaScript
author: guardrex
description: Obtenga información sobre cómo invocar funciones de JavaScript desde los métodos .NET y .NET desde JavaScript en Blazor aplicaciones.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 11/23/2019
no-loc:
- Blazor
uid: blazor/javascript-interop
ms.openlocfilehash: 79555ca6c987e2ca57e0cfab9779024498fdd58b
ms.sourcegitcommit: 0dd224b2b7efca1fda0041b5c3f45080327033f6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2019
ms.locfileid: "74681035"
---
# <a name="aspnet-core-opno-locblazor-javascript-interop"></a>ASP.NET Core Blazor la interoperabilidad de JavaScript

Por [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27)y [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Una aplicación Blazor puede invocar funciones de JavaScript desde los métodos .NET y .NET desde el código JavaScript.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([cómo descargarlo](xref:index#how-to-download-a-sample))

## <a name="invoke-javascript-functions-from-net-methods"></a>Invocar funciones de JavaScript desde métodos de .NET

Hay ocasiones en las que se requiere código .NET para llamar a una función de JavaScript. Por ejemplo, una llamada de JavaScript puede exponer funciones o funcionalidades del explorador de una biblioteca de JavaScript a la aplicación. Este escenario se denomina *interoperabilidad de JavaScript* (*interoperabilidad de JS*).

Para llamar a JavaScript desde .NET, use la abstracción `IJSRuntime`. El método `InvokeAsync<T>` toma un identificador de la función de JavaScript que se desea invocar junto con cualquier número de argumentos serializables de JSON. El identificador de función es relativo al ámbito global (`window`). Si desea llamar a `window.someScope.someFunction`, el identificador se `someScope.someFunction`. No es necesario registrar la función antes de que se le llame. El tipo de valor devuelto `T` también debe ser serializable.

Para las aplicaciones de Blazor Server:

* La aplicación de servidor de Blazor procesa varias solicitudes de usuario. No llame a `JSRuntime.Current` de un componente para invocar funciones de JavaScript.
* Inserte la abstracción `IJSRuntime` y utilice el objeto insertado para emitir llamadas de interoperabilidad de JS.
* Aunque una aplicación Blazor se está preprocesando, no es posible llamar a JavaScript porque no se ha establecido una conexión con el explorador. Para obtener más información, consulte la sección [detectar cuándo se está representando una aplicación Blazor](#detect-when-a-blazor-app-is-prerendering) .

El ejemplo siguiente se basa en [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), un descodificador basado en JavaScript experimental. En el ejemplo se muestra cómo invocar una función de C# JavaScript desde un método. La función de JavaScript acepta una matriz de bytes de un C# método, descodifica la matriz y devuelve el texto al componente que se va a mostrar.

Dentro del elemento `<head>` de *wwwroot/index.html* (Blazor webassembly) o *pages/_Host. cshtml* (Blazor Server), proporcione una función de JavaScript que use `TextDecoder` para descodificar una matriz pasada y devolver el valor descodificado:

[!code-html[](javascript-interop/samples_snapshot/index-script-convertarray.html)]

El código JavaScript, como el código que se muestra en el ejemplo anterior, también se puede cargar desde un archivo JavaScript ( *. js*) con una referencia al archivo de script:

```html
<script src="exampleJsInterop.js"></script>
```

El siguiente componente:

* Invoca la función `convertArray` JavaScript mediante `JSRuntime` cuando se selecciona un botón de componente (**convertir matriz**).
* Después de llamar a la función de JavaScript, la matriz que se pasa se convierte en una cadena. La cadena se devuelve al componente para su presentación.

[!code-cshtml[](javascript-interop/samples_snapshot/call-js-example.razor?highlight=2,34-35)]

## <a name="use-of-ijsruntime"></a>Uso de IJSRuntime

Para usar la abstracción `IJSRuntime`, adopte cualquiera de los enfoques siguientes:

* Inyectar la abstracción de `IJSRuntime` en el componente de Razor ( *. Razor*):

  [!code-cshtml[](javascript-interop/samples_snapshot/inject-abstraction.razor?highlight=1)]

  Dentro del elemento `<head>` de *wwwroot/index.html* (Blazor webassembly) o *pages/_Host. cshtml* (Blazor Server), proporcione una función `handleTickerChanged` JavaScript. Se llama a la función con `IJSRuntime.InvokeVoidAsync` y no devuelve un valor:

  [!code-html[](javascript-interop/samples_snapshot/index-script-handleTickerChanged1.html)]

* Inserte la abstracción de `IJSRuntime` en una clase ( *. CS*):

  [!code-csharp[](javascript-interop/samples_snapshot/inject-abstraction-class.cs?highlight=5)]

  Dentro del elemento `<head>` de *wwwroot/index.html* (Blazor webassembly) o *pages/_Host. cshtml* (Blazor Server), proporcione una función `handleTickerChanged` JavaScript. Se llama a la función con `JSRuntime.InvokeAsync` y devuelve un valor:

  [!code-html[](javascript-interop/samples_snapshot/index-script-handleTickerChanged2.html)]

* Para la generación de contenido dinámico con [BuildRenderTree](xref:blazor/components#manual-rendertreebuilder-logic), use el atributo `[Inject]`:

  ```csharp
  [Inject]
  IJSRuntime JSRuntime { get; set; }
  ```

En la aplicación de ejemplo del lado cliente que acompaña a este tema, hay dos funciones de JavaScript disponibles para la aplicación que interactúan con el DOM para recibir datos proporcionados por el usuario y mostrar un mensaje de bienvenida:

* `showPrompt` &ndash; genera un mensaje para aceptar la entrada del usuario (el nombre del usuario) y devuelve el nombre al autor de la llamada.
* `displayWelcome` &ndash; asigna un mensaje de bienvenida del autor de la llamada a un objeto DOM con un `id` de `welcome`.

*wwwroot/exampleJsInterop. js*:

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=2-7)]

Coloque la etiqueta de `<script>` que hace referencia al archivo JavaScript en el archivo *wwwroot/index.html* (Blazor webassembly) o el archivo *pages/_Host. cshtml* (Blazor Server).

*wwwroot/index.html* (Blazor webassembly):

[!code-html[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/index.html?highlight=15)]

*Pages/_Host. cshtml* (Blazor Server):

[!code-cshtml[](./common/samples/3.x/BlazorServerSample/Pages/_Host.cshtml?highlight=21)]

No coloque un `<script>` etiqueta en un archivo de componente porque la etiqueta de `<script>` no se puede actualizar dinámicamente.

Los métodos de .NET interoperan con las funciones de JavaScript en el archivo *exampleJsInterop. js* llamando a `IJSRuntime.InvokeAsync<T>`.

La abstracción `IJSRuntime` es asincrónica para permitir escenarios de Blazor Server. Si la aplicación es una aplicación Blazor webassembly y desea invocar una función de JavaScript sincrónicamente, se puede convertir en `IJSInProcessRuntime` y llamar a `Invoke<T>` en su lugar. Se recomienda que la mayoría de las bibliotecas de interoperabilidad de JS usen las API asincrónicas para asegurarse de que las bibliotecas están disponibles en todos los escenarios.

La aplicación de ejemplo incluye un componente para mostrar la interoperabilidad de JS. El componente:

* Recibe la entrada del usuario a través de un mensaje de JavaScript.
* Devuelve el texto del componente que se va a procesar.
* Llama a una segunda función de JavaScript que interactúa con el DOM para mostrar un mensaje de bienvenida.

*Pages/JSInterop. Razor*:

[!code-cshtml[](./common/samples/3.x/BlazorWebAssemblySample/Pages/JsInterop.razor?name=snippet_JSInterop1&highlight=3,19-21,23-25)]

1. Cuando se ejecuta `TriggerJsPrompt` seleccionando el botón **desencadenador de JavaScript** del componente, se llama a la función de JavaScript `showPrompt` proporcionada en el archivo *wwwroot/exampleJsInterop. js* .
1. La función `showPrompt` acepta datos proporcionados por el usuario (el nombre del usuario), que se codifica en HTML y se devuelve al componente. El componente almacena el nombre del usuario en una variable local, `name`.
1. La cadena almacenada en `name` se incorpora en un mensaje de bienvenida, que se pasa a una función de JavaScript, `displayWelcome`, que representa el mensaje de bienvenida en una etiqueta de encabezado.

## <a name="call-a-void-javascript-function"></a>Llamar a una función de JavaScript void

Las funciones de JavaScript que devuelven [void (0)/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void) o [undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) se llaman con `IJSRuntime.InvokeVoidAsync`.

## <a name="detect-when-a-opno-locblazor-app-is-prerendering"></a>Detección cuando una aplicación Blazor se está preprocesando
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

## <a name="capture-references-to-elements"></a>Capturar referencias a elementos

Algunos escenarios de interoperabilidad de JS requieren referencias a elementos HTML. Por ejemplo, una biblioteca de interfaz de usuario puede requerir una referencia de elemento para la inicialización, o puede que necesite llamar a las API de tipo de comando en un elemento, como `focus` o `play`.

Capture las referencias a los elementos HTML de un componente con el siguiente enfoque:

* Agregue un atributo `@ref` al elemento HTML.
* Defina un campo de tipo `ElementReference` cuyo nombre coincida con el valor del atributo `@ref`.

En el ejemplo siguiente se muestra la captura de una referencia al elemento `username` `<input>`:

```cshtml
<input @ref="username" ... />

@code {
    ElementReference username;
}
```

> [!WARNING]
> Use solo una referencia de elemento para mutar el contenido de un elemento vacío que no interactúe con Blazor. Este escenario es útil cuando una API de terceros proporciona contenido al elemento. Dado que Blazor no interactúa con el elemento, no existe ninguna posibilidad de que se produzca un conflicto entre la representación de Blazordel elemento y el DOM.
>
> En el ejemplo siguiente, es *peligroso* mutar el contenido de la lista sin ordenar (`ul`) porque Blazor interactúa con el Dom para rellenar los elementos de lista de este elemento (`<li>`):
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
> Si la interoperabilidad de JS cambia el contenido de los `MyList` de elemento y Blazor intenta aplicar las diferencias al elemento, las diferencias no coincidirán con el DOM.

En lo que respecta al código .NET, una `ElementReference` es un identificador opaco. Lo *único* que puede hacer con `ElementReference` es pasarlo a código JavaScript a través de la interoperabilidad de JS. Al hacerlo, el código de JavaScript recibe una instancia de `HTMLElement`, que puede usar con las API de DOM normales.

Por ejemplo, el código siguiente define un método de extensión de .NET que permite establecer el foco en un elemento:

*exampleJsInterop. js*:

```javascript
window.exampleJsFunctions = {
  focusElement : function (element) {
    element.focus();
  }
}
```

Use `IJSRuntime.InvokeAsync<T>` y llame a `exampleJsFunctions.focusElement` con un `ElementReference` para centrar un elemento:

[!code-cshtml[](javascript-interop/samples_snapshot/component1.razor?highlight=1,3,11-12)]

Para usar un método de extensión para centrar un elemento, cree un método de extensión estático que reciba la instancia de `IJSRuntime`:

```csharp
public static Task Focus(this ElementReference elementRef, IJSRuntime jsRuntime)
{
    return jsRuntime.InvokeAsync<object>(
        "exampleJsFunctions.focusElement", elementRef);
}
```

Se llama al método directamente en el objeto. En el ejemplo siguiente se da por supuesto que el método estático `Focus` está disponible en el espacio de nombres `JsInteropClasses`:

[!code-cshtml[](javascript-interop/samples_snapshot/component2.razor?highlight=1,4,12)]

> [!IMPORTANT]
> La variable `username` solo se rellena después de representar el componente. Si se pasa un `ElementReference` sin rellenar al código JavaScript, el código JavaScript recibe un valor de `null`. Para manipular las referencias de elemento una vez finalizada la representación del componente (para establecer el foco inicial en un elemento), use los [métodos de ciclo de vida del componente OnAfterRenderAsync o OnAfterRender](xref:blazor/lifecycle#after-component-render).

## <a name="invoke-net-methods-from-javascript-functions"></a>Invocar métodos .NET desde funciones de JavaScript

### <a name="static-net-method-call"></a>Llamada al método estático de .NET

Para invocar un método .NET estático desde JavaScript, use las funciones `DotNet.invokeMethod` o `DotNet.invokeMethodAsync`. Pase el identificador del método estático al que desea llamar, el nombre del ensamblado que contiene la función y cualquier argumento. La versión asincrónica es preferible para admitir escenarios de Blazor Server. Para invocar un método .NET desde JavaScript, el método .NET debe ser público, estático y tener el atributo `[JSInvokable]`. De forma predeterminada, el identificador del método es el nombre del método, pero puede especificar un identificador diferente mediante el constructor `JSInvokableAttribute`. Actualmente no se admite la llamada a métodos genéricos abiertos.

La aplicación de ejemplo incluye C# un método para devolver una matriz de `int`s. El atributo `JSInvokable` se aplica al método.

*Pages/JsInterop. Razor*:

[!code-cshtml[](./common/samples/3.x/BlazorWebAssemblySample/Pages/JsInterop.razor?name=snippet_JSInterop2&highlight=7-11)]

JavaScript atendido al cliente invoca el C# método .net.

*wwwroot/exampleJsInterop. js*:

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=8-14)]

Cuando se seleccione el botón **desencadenador de método estático de .net ReturnArrayAsync** , examine la salida de la consola en las herramientas de desarrollo web del explorador.

La salida de la consola es:

```console
Array(4) [ 1, 2, 3, 4 ]
```

El cuarto valor de matriz se inserta en la matriz (`data.push(4);`) devuelta por `ReturnArrayAsync`.

### <a name="instance-method-call"></a>Llamada al método de instancia

También puede llamar a métodos de instancia de .NET desde JavaScript. Para invocar un método de instancia de .NET desde JavaScript:

* Pase la instancia de .NET a JavaScript ajustándola en una instancia de `DotNetObjectReference`. La instancia de .NET se pasa por referencia a JavaScript.
* Invocar métodos de instancia de .NET en la instancia de mediante las funciones `invokeMethod` o `invokeMethodAsync`. La instancia de .NET también se puede pasar como argumento al invocar otros métodos .NET desde JavaScript.

> [!NOTE]
> La aplicación de ejemplo registra mensajes en la consola del lado cliente. En los siguientes ejemplos mostrados por la aplicación de ejemplo, examine la salida de la consola del explorador en las herramientas de desarrollo del explorador.

Cuando se selecciona el botón **desencadenador de instancia de .net HelloHelper. SayHello** , se llama a `ExampleJsInterop.CallHelloHelperSayHello` y pasa un nombre, `Blazor`, al método.

*Pages/JsInterop. Razor*:

[!code-cshtml[](./common/samples/3.x/BlazorWebAssemblySample/Pages/JsInterop.razor?name=snippet_JSInterop3&highlight=8-9)]

`CallHelloHelperSayHello` invoca la función de JavaScript `sayHello` con una nueva instancia de `HelloHelper`.

*JsInteropClasses/ExampleJsInterop. CS*:

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=10-16)]

*wwwroot/exampleJsInterop. js*:

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=15-18)]

El nombre se pasa al constructor de `HelloHelper`, que establece la propiedad `HelloHelper.Name`. Cuando se ejecuta la función de JavaScript `sayHello`, `HelloHelper.SayHello` devuelve el mensaje `Hello, {Name}!`, que la función JavaScript escribe en la consola.

*JsInteropClasses/HelloHelper. CS*:

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

Salida de la consola en las herramientas de desarrollo web del explorador:

```console
Hello, Blazor!
```

## <a name="share-interop-code-in-a-class-library"></a>Compartir código de interoperabilidad en una biblioteca de clases

El código de interoperabilidad de JS se puede incluir en una biblioteca de clases, lo que permite compartir el código en un paquete NuGet.

La biblioteca de clases controla la incrustación de recursos de JavaScript en el ensamblado compilado. Los archivos de JavaScript se colocan en la carpeta *wwwroot* . Las herramientas se encargan de insertar los recursos cuando se compila la biblioteca.

Se hace referencia al paquete de NuGet compilado en el archivo de proyecto de la aplicación de la misma manera que se hace referencia a cualquier paquete NuGet. Una vez restaurado el paquete, el código de la aplicación puede llamar a JavaScript C#como si fuera.

Para obtener más información, vea <xref:blazor/class-libraries>.

## <a name="harden-js-interop-calls"></a>Reprotección de llamadas de interoperabilidad de JS

La interoperabilidad de JS puede producir un error debido a errores de red y debe tratarse como no confiable. De forma predeterminada, una aplicación de Blazor Server agota el tiempo de espera para las llamadas de interoperabilidad de JS en el servidor después de un minuto. Si una aplicación puede tolerar un tiempo de espera más agresivo, como 10 segundos, establezca el tiempo de espera con uno de los siguientes métodos:

* Globalmente en `Startup.ConfigureServices`, especifique el tiempo de espera:

  ```csharp
  services.AddServerSideBlazor(
      options => options.JSInteropDefaultCallTimeout = TimeSpan.FromSeconds({SECONDS}));
  ```

* Por cada invocación en el código de componente, una sola llamada puede especificar el tiempo de espera:

  ```csharp
  var result = await JSRuntime.InvokeAsync<string>("MyJSOperation", 
      TimeSpan.FromSeconds({SECONDS}), new[] { "Arg1" });
  ```

Para obtener más información sobre el agotamiento de recursos, consulte <xref:security/blazor/server>.
