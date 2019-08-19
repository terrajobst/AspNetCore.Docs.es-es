---
title: Interoperabilidad de JavaScript de ASP.NET Core increíblemente
author: guardrex
description: Obtenga información sobre cómo invocar funciones de JavaScript desde los métodos de .net y .net desde JavaScript en aplicaciones increíbles.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/13/2019
uid: blazor/javascript-interop
ms.openlocfilehash: 00ea14ca95c328b5f8779785a92aa0720a96eb05
ms.sourcegitcommit: 7a46973998623aead757ad386fe33602b1658793
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/15/2019
ms.locfileid: "69487549"
---
# <a name="aspnet-core-blazor-javascript-interop"></a>Interoperabilidad de JavaScript de ASP.NET Core increíblemente

Por [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27)y [Luke Latham](https://github.com/guardrex)

Una aplicación extraordinaria puede invocar funciones de JavaScript desde los métodos de .NET y .net desde el código JavaScript.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([cómo descargarlo](xref:index#how-to-download-a-sample))

## <a name="invoke-javascript-functions-from-net-methods"></a>Invocar funciones de JavaScript desde métodos de .NET

Hay ocasiones en las que se requiere código .NET para llamar a una función de JavaScript. Por ejemplo, una llamada de JavaScript puede exponer funciones o funcionalidades del explorador de una biblioteca de JavaScript a la aplicación.

Para llamar a JavaScript desde .net, use la `IJSRuntime` abstracción. El `InvokeAsync<T>` método toma un identificador de la función de JavaScript que desea invocar junto con cualquier número de argumentos serializables de JSON. El identificador de función es relativo al ámbito global (`window`). Si desea llamar a `window.someScope.someFunction`, el identificador es. `someScope.someFunction` No es necesario registrar la función antes de que se le llame. El tipo `T` de valor devuelto también debe ser serializable.

Para las aplicaciones del lado servidor:

* La aplicación del lado servidor procesa varias solicitudes de usuario. No llame `JSRuntime.Current` a en un componente para invocar funciones de JavaScript.
* Inyectar `IJSRuntime` la abstracción y usar el objeto insertado para emitir llamadas de interoperabilidad de JavaScript.
* Aunque una aplicación increíblemente es una representación previa, no es posible llamar a JavaScript porque no se ha establecido una conexión con el explorador. Para obtener más información, consulte la sección [detectar cuándo una aplicación increíblemente es una representación](#detect-when-a-blazor-app-is-prerendering) previa.

El ejemplo siguiente se basa en [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), un descodificador basado en JavaScript experimental. En el ejemplo se muestra cómo invocar una función de C# JavaScript desde un método. La función de JavaScript acepta una matriz de bytes de un C# método, descodifica la matriz y devuelve el texto al componente que se va a mostrar.

Dentro del `<head>` elemento de *wwwroot/index.html* (el lado del cliente de increíble) o *pages/_Host. cshtml* (servidor más increíble), proporcione una función que use `TextDecoder` para descodificar una matriz pasada:

[!code-html[](javascript-interop/samples_snapshot/index-script.html)]

El código JavaScript, como el código que se muestra en el ejemplo anterior, también se puede cargar desde un archivo JavaScript ( *. js*) con una referencia al archivo de script:

```html
<script src="exampleJsInterop.js"></script>
```

El siguiente componente:

* Invoca la función `ConvertArray` de JavaScript mediante `JsRuntime` cuando se selecciona un botón de componente (**convertir matriz**).
* Después de llamar a la función de JavaScript, la matriz que se pasa se convierte en una cadena. La cadena se devuelve al componente para su presentación.

[!code-cshtml[](javascript-interop/samples_snapshot/call-js-example.razor?highlight=2,34-35)]

Para usar la `IJSRuntime` abstracción, adopte cualquiera de los métodos siguientes:

* Inyectar `IJSRuntime` la abstracción en el componente de Razor ( *. Razor*):

  [!code-cshtml[](javascript-interop/samples_snapshot/inject-abstraction.razor?highlight=1)]

* Inyectar `IJSRuntime` la abstracción en una clase ( *. CS*):

  [!code-csharp[](javascript-interop/samples_snapshot/inject-abstraction-class.cs?highlight=5)]

* Para la generación de contenido dinámico con [BuildRenderTree](xref:blazor/components#manual-rendertreebuilder-logic), `[Inject]` use el atributo:

  ```csharp
  [Inject]
  IJSRuntime JSRuntime { get; set; }
  ```

En la aplicación de ejemplo del lado cliente que acompaña a este tema, hay dos funciones de JavaScript disponibles para la aplicación del lado cliente que interactúan con el DOM para recibir datos proporcionados por el usuario y mostrar un mensaje de bienvenida:

* `showPrompt`&ndash; Genera un mensaje para aceptar la entrada del usuario (el nombre del usuario) y devuelve el nombre al autor de la llamada.
* `displayWelcome`Asigna un mensaje de bienvenida del llamador a un objeto DOM con un `id` de `welcome`. &ndash;

*wwwroot/exampleJsInterop.js*:

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=2-7)]

Coloque la `<script>` etiqueta que hace referencia al archivo JavaScript en el archivo *wwwroot/index.html* (en el lado cliente) o el archivo *pages/_Host. cshtml* (el servidor es más rápido).

*wwwroot/index.html* (El lado cliente es más rápido):

[!code-html[](./common/samples/3.x/BlazorSample/wwwroot/index.html?highlight=15)]

*Pages/_Host. cshtml* (Servidor más rápido):

[!code-cshtml[](javascript-interop/samples_snapshot/_Host.cshtml?highlight=29)]

No coloque una `<script>` etiqueta en un archivo de componente porque `<script>` la etiqueta no se puede actualizar dinámicamente.

Los métodos de .NET interoperan con las funciones de JavaScript en el archivo `IJSRuntime.InvokeAsync<T>` *exampleJsInterop. js* mediante una llamada a.

La `IJSRuntime` abstracción es asincrónica para permitir escenarios del lado servidor. Si la aplicación se ejecuta en el lado cliente y desea invocar una función de JavaScript sincrónicamente, `Invoke<T>` convertir en de forma sincrónica y llamar a `IJSInProcessRuntime` en su lugar. Se recomienda que la mayoría de las bibliotecas de interoperabilidad de JavaScript usen las API asincrónicas para asegurarse de que las bibliotecas están disponibles en todos los escenarios, en el lado cliente o en el servidor.

La aplicación de ejemplo incluye un componente para mostrar la interoperabilidad de JavaScript. El componente:

* Recibe la entrada del usuario a través de un mensaje de JavaScript.
* Devuelve el texto del componente que se va a procesar.
* Llama a una segunda función de JavaScript que interactúa con el DOM para mostrar un mensaje de bienvenida.

*Pages/JSInterop. Razor*:

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.razor?name=snippet_JSInterop1&highlight=3,19-21,23-25)]

1. Cuando `TriggerJsPrompt` se ejecuta seleccionando el botón desencadenador de **JavaScript** del componente, se `showPrompt` llama a la función JavaScript proporcionada en el archivo *wwwroot/exampleJsInterop. js* .
1. La `showPrompt` función acepta la entrada del usuario (el nombre del usuario), que está codificada en HTML y se devuelve al componente. El componente almacena el nombre del usuario en una variable local, `name`.
1. La cadena almacenada `name` en se incorpora en un mensaje de bienvenida, que se pasa a una función `displayWelcome`de JavaScript,, que representa el mensaje de bienvenida en una etiqueta de encabezado.

## <a name="call-a-void-javascript-function"></a>Llamar a una función de JavaScript void

Se llama a las funciones de JavaScript que devuelven [void (0)/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void) o [undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) con `IJSRuntime.InvokeAsync<object>`, que devuelve `null`.

## <a name="detect-when-a-blazor-app-is-prerendering"></a>Detección cuando una aplicación increíblemente se está preprocesando
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

## <a name="capture-references-to-elements"></a>Capturar referencias a elementos

Algunos escenarios de interoperabilidad de [JavaScript](xref:blazor/javascript-interop) requieren referencias a elementos HTML. Por ejemplo, una biblioteca de interfaz de usuario puede requerir una referencia de elemento para la inicialización, o puede que necesite llamar a las API de tipo `focus` de `play`comando en un elemento, como o.

Capture las referencias a los elementos HTML de un componente con el siguiente enfoque:

* Agregue un `@ref` atributo al elemento HTML.
* Defina un campo de tipo `ElementReference` cuyo nombre coincida con el valor `@ref` del atributo.
* Proporcione el `@ref:suppressField` parámetro, que suprime la generación de campos de respaldo. Para obtener más información, vea el apartado sobre [Cómo quitar @ref la compatibilidad automática de campos de respaldo para en 3.0.0-preview9](https://github.com/aspnet/Announcements/issues/381).

En el ejemplo siguiente se muestra la captura de `username` una referencia al `<input>` elemento:

```cshtml
<input @ref="username" @ref:suppressField ... />

@code {
    ElementReference username;
}
```

> [!NOTE]
> **No** utilice referencias a elementos capturados como una manera de rellenar o manipular el Dom cuando increíble interactúe con los elementos a los que se hace referencia. Si lo hace, puede interferir con el modelo de representación declarativa.

En lo que respecta al código de .net, `ElementReference` es un identificador opaco. Lo *único* que puede hacer con `ElementReference` es pasarlo a código JavaScript a través de la interoperabilidad de JavaScript. Al hacerlo, el código de JavaScript recibe una `HTMLElement` instancia de, que puede usar con las API de Dom normales.

Por ejemplo, el código siguiente define un método de extensión de .NET que permite establecer el foco en un elemento:

*exampleJsInterop.js*:

```javascript
window.exampleJsFunctions = {
  focusElement : function (element) {
    element.focus();
  }
}
```

Use `IJSRuntime.InvokeAsync<T>` y llame `exampleJsFunctions.focusElement` a con `ElementReference` para enfocar un elemento:

```cshtml
@inject IJSRuntime JSRuntime

<input @ref="username" @ref:suppressField />
<button @onclick="SetFocus">Set focus on username</button>

@code {
    private ElementReference username;

    public async void SetFocus()
    {
        await JSRuntime.InvokeAsync<object>(
                "exampleJsFunctions.focusElement", username);
    }
}
```

Para usar un método de extensión para centrar un elemento, cree un método de extensión estático `IJSRuntime` que reciba la instancia:

```csharp
public static Task Focus(this ElementReference elementRef, IJSRuntime jsRuntime)
{
    return jsRuntime.InvokeAsync<object>(
        "exampleJsFunctions.focusElement", elementRef);
}
```

Se llama al método directamente en el objeto. En el ejemplo siguiente se da por `Focus` supuesto que el método estático `JsInteropClasses` está disponible en el espacio de nombres:

```cshtml
@inject IJSRuntime JSRuntime
@using JsInteropClasses

<input @ref="username" @ref:suppressField />
<button @onclick="SetFocus">Set focus on username</button>

@code {
    private ElementReference username;

    public async Task SetFocus()
    {
        await username.Focus(JSRuntime);
    }
}
```

> [!IMPORTANT]
> La `username` variable solo se rellena después de representar el componente. Si se pasa un `ElementReference` sin rellenado al código JavaScript, el código JavaScript recibe un `null`valor de. Para manipular las referencias de elemento una vez finalizada la representación del componente (para establecer el foco inicial en un elemento) `OnAfterRenderAsync` , `OnAfterRender` use los métodos de ciclo de vida de los [componentes](xref:blazor/components#lifecycle-methods)o.

<!-- HOLD https://github.com/aspnet/AspNetCore.Docs/pull/13818
Capture a reference to an HTML element in a component by adding an `@ref` attribute to the HTML element. The following example shows capturing a reference to the `username` `<input>` element:

```cshtml
<input @ref="username" ... />
```

> [!NOTE]
> Do **not** use captured element references as a way of populating or manipulating the DOM when Blazor interacts with the elements referenced. Doing so may interfere with the declarative rendering model.

As far as .NET code is concerned, an `ElementReference` is an opaque handle. The *only* thing you can do with `ElementReference` is pass it through to JavaScript code via JavaScript interop. When you do so, the JavaScript-side code receives an `HTMLElement` instance, which it can use with normal DOM APIs.

For example, the following code defines a .NET extension method that enables setting the focus on an element:

*exampleJsInterop.js*:

```javascript
window.exampleJsFunctions = {
  focusElement : function (element) {
    element.focus();
  }
}
```

Use `IJSRuntime.InvokeAsync<T>` and call `exampleJsFunctions.focusElement` with an `ElementReference` to focus an element:

[!code-cshtml[](javascript-interop/samples_snapshot/component1.razor?highlight=1,3,9-10)]

To use an extension method to focus an element, create a static extension method that receives the `IJSRuntime` instance:

```csharp
public static Task Focus(this ElementReference elementRef, IJSRuntime jsRuntime)
{
    return jsRuntime.InvokeAsync<object>(
        "exampleJsFunctions.focusElement", elementRef);
}
```

The method is called directly on the object. The following example assumes that the static `Focus` method is available from the `JsInteropClasses` namespace:

[!code-cshtml[](javascript-interop/samples_snapshot/component2.razor?highlight=1,4,10)]

> [!IMPORTANT]
> The `username` variable is only populated after the component is rendered. If an unpopulated `ElementReference` is passed to JavaScript code, the JavaScript code receives a value of `null`. To manipulate element references after the component has finished rendering (to set the initial focus on an element) use the `OnAfterRenderAsync` or `OnAfterRender` [component lifecycle methods](xref:blazor/components#lifecycle-methods).
-->

## <a name="invoke-net-methods-from-javascript-functions"></a>Invocar métodos .NET desde funciones de JavaScript

### <a name="static-net-method-call"></a>Llamada al método estático de .NET

Para invocar un método .net estático desde JavaScript, use `DotNet.invokeMethod` las `DotNet.invokeMethodAsync` funciones o. Pase el identificador del método estático al que desea llamar, el nombre del ensamblado que contiene la función y cualquier argumento. Se prefiere la versión asincrónica para admitir escenarios del lado servidor. Para invocar un método .net desde JavaScript, el método .net debe ser público, estático y tener el `[JSInvokable]` atributo. De forma predeterminada, el identificador del método es el nombre del método, pero puede especificar un identificador diferente mediante el `JSInvokableAttribute` constructor. Actualmente no se admite la llamada a métodos genéricos abiertos.

La aplicación de ejemplo incluye C# un método para devolver una matriz `int`de s. El `JSInvokable` atributo se aplica al método.

*Pages/JsInterop. Razor*:

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.razor?name=snippet_JSInterop2&highlight=7-11)]

JavaScript atendido al cliente invoca el C# método .net.

*wwwroot/exampleJsInterop.js*:

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=8-14)]

Cuando se seleccione el botón desencadenador de **método estático de .net ReturnArrayAsync** , examine la salida de la consola en las herramientas de desarrollo web del explorador.

La salida de la consola es:

```console
Array(4) [ 1, 2, 3, 4 ]
```

El cuarto valor de matriz se inserta en la matriz`data.push(4);`() devuelta por. `ReturnArrayAsync`

### <a name="instance-method-call"></a>Llamada al método de instancia

También puede llamar a métodos de instancia de .NET desde JavaScript. Para invocar un método de instancia de .NET desde JavaScript:

* Pase la instancia de .net a JavaScript ajustándola en una `DotNetObjectRef` instancia de. La instancia de .NET se pasa por referencia a JavaScript.
* Invocar métodos de instancia de .net en la `invokeMethod` instancia `invokeMethodAsync` de mediante las funciones o. La instancia de .NET también se puede pasar como argumento al invocar otros métodos .NET desde JavaScript.

> [!NOTE]
> La aplicación de ejemplo registra mensajes en la consola del lado cliente. En los siguientes ejemplos mostrados por la aplicación de ejemplo, examine la salida de la consola del explorador en las herramientas de desarrollo del explorador.

Cuando se selecciona el botón desencadenador de **instancia de .net HelloHelper. SayHello** , `ExampleJsInterop.CallHelloHelperSayHello` se llama a y se `Blazor`pasa un nombre,, al método.

*Pages/JsInterop. Razor*:

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.razor?name=snippet_JSInterop3&highlight=8-9)]

`CallHelloHelperSayHello`invoca la función `sayHello` de JavaScript con una nueva instancia de `HelloHelper`.

*JsInteropClasses/ExampleJsInterop. CS*:

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=10-16)]

*wwwroot/exampleJsInterop.js*:

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=15-18)]

El nombre se pasa al `HelloHelper`constructor de, que establece la `HelloHelper.Name` propiedad. Cuando se ejecuta la `sayHello` función de JavaScript `HelloHelper.SayHello` , devuelve `Hello, {Name}!` el mensaje, que la función de JavaScript escribe en la consola.

*JsInteropClasses/HelloHelper.cs*:

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

Salida de la consola en las herramientas de desarrollo web del explorador:

```console
Hello, Blazor!
```

## <a name="share-interop-code-in-a-class-library"></a>Compartir código de interoperabilidad en una biblioteca de clases

El código de interoperabilidad de JavaScript se puede incluir en una biblioteca de clases, lo que permite compartir el código en un paquete NuGet.

La biblioteca de clases controla la incrustación de recursos de JavaScript en el ensamblado compilado. Los archivos de JavaScript se colocan en la carpeta *wwwroot* . Las herramientas se encargan de insertar los recursos cuando se compila la biblioteca.

Se hace referencia al paquete de NuGet compilado en el archivo de proyecto de la aplicación de la misma manera que se hace referencia a cualquier paquete NuGet. Una vez restaurado el paquete, el código de la aplicación puede llamar a JavaScript C#como si fuera.

Para obtener más información, consulta <xref:blazor/class-libraries>.
