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
# <a name="aspnet-core-opno-locblazor-javascript-interop"></a>ASP.NET Core Blazor la interoperabilidad de JavaScript

Por [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27)y [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Una aplicación Blazor puede invocar funciones de JavaScript desde los métodos .NET y .NET desde el código JavaScript.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([cómo descargarlo](xref:index#how-to-download-a-sample))

## <a name="invoke-javascript-functions-from-net-methods"></a>Invocar funciones de JavaScript desde métodos de .NET

Hay ocasiones en las que se requiere código .NET para llamar a una función de JavaScript. Por ejemplo, una llamada de JavaScript puede exponer funciones o funcionalidades del explorador de una biblioteca de JavaScript a la aplicación. Este escenario se denomina *interoperabilidad de JavaScript* (*interoperabilidad de JS*).

Para llamar a JavaScript desde .NET, use la abstracción `IJSRuntime`. Para emitir llamadas de interoperabilidad de JS, inserte la abstracción de `IJSRuntime` en el componente. El método `InvokeAsync<T>` toma un identificador de la función de JavaScript que se desea invocar junto con cualquier número de argumentos serializables de JSON. El identificador de función es relativo al ámbito global (`window`). Si desea llamar a `window.someScope.someFunction`, el identificador se `someScope.someFunction`. No es necesario registrar la función antes de que se le llame. El tipo de valor devuelto `T` también debe ser serializable. `T` debe coincidir con el tipo de .NET que se asigna mejor al tipo JSON devuelto.

En el caso de las aplicaciones de Blazor Server que tienen habilitada la representación previa, no es posible llamar a JavaScript durante la prerepresentación inicial. Las llamadas de interoperabilidad de JavaScript se deben diferir hasta que se establezca la conexión con el explorador. Para obtener más información, consulte la sección [detectar cuándo se está representando una aplicación Blazor](#detect-when-a-blazor-app-is-prerendering) .

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

[!code-razor[](javascript-interop/samples_snapshot/call-js-example.razor?highlight=2,34-35)]

## <a name="use-of-ijsruntime"></a>Uso de IJSRuntime

Para usar la abstracción `IJSRuntime`, adopte cualquiera de los enfoques siguientes:

* Inyectar la abstracción de `IJSRuntime` en el componente de Razor ( *. Razor*):

  [!code-razor[](javascript-interop/samples_snapshot/inject-abstraction.razor?highlight=1)]

  Dentro del elemento `<head>` de *wwwroot/index.html* (Blazor webassembly) o *pages/_Host. cshtml* (Blazor Server), proporcione una función `handleTickerChanged` JavaScript. Se llama a la función con `IJSRuntime.InvokeVoidAsync` y no devuelve un valor:

  [!code-html[](javascript-interop/samples_snapshot/index-script-handleTickerChanged1.html)]

* Inserte la abstracción de `IJSRuntime` en una clase ( *. CS*):

  [!code-csharp[](javascript-interop/samples_snapshot/inject-abstraction-class.cs?highlight=5)]

  Dentro del elemento `<head>` de *wwwroot/index.html* (Blazor webassembly) o *pages/_Host. cshtml* (Blazor Server), proporcione una función `handleTickerChanged` JavaScript. Se llama a la función con `JSRuntime.InvokeAsync` y devuelve un valor:

  [!code-html[](javascript-interop/samples_snapshot/index-script-handleTickerChanged2.html)]

* Para la generación de contenido dinámico con [BuildRenderTree](xref:blazor/components#manual-rendertreebuilder-logic), use el atributo `[Inject]`:

  ```razor
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

[!code-html[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/index.html?highlight=22)]

*Pages/_Host. cshtml* (Blazor Server):

[!code-cshtml[](./common/samples/3.x/BlazorServerSample/Pages/_Host.cshtml?highlight=35)]

No coloque un `<script>` etiqueta en un archivo de componente porque la etiqueta de `<script>` no se puede actualizar dinámicamente.

Los métodos de .NET interoperan con las funciones de JavaScript en el archivo *exampleJsInterop. js* llamando a `IJSRuntime.InvokeAsync<T>`.

La abstracción `IJSRuntime` es asincrónica para permitir escenarios de Blazor Server. Si la aplicación es una aplicación Blazor webassembly y desea invocar una función de JavaScript sincrónicamente, se puede convertir en `IJSInProcessRuntime` y llamar a `Invoke<T>` en su lugar. Se recomienda que la mayoría de las bibliotecas de interoperabilidad de JS usen las API asincrónicas para asegurarse de que las bibliotecas están disponibles en todos los escenarios.

La aplicación de ejemplo incluye un componente para mostrar la interoperabilidad de JS. El componente:

* Recibe la entrada del usuario a través de un mensaje de JavaScript.
* Devuelve el texto del componente que se va a procesar.
* Llama a una segunda función de JavaScript que interactúa con el DOM para mostrar un mensaje de bienvenida.

*Pages/JSInterop. Razor*:

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

```razor
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
> ```razor
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

Para llamar a una función de JavaScript que no devuelve un valor, use `IJSRuntime.InvokeVoidAsync`. El código siguiente establece el foco en la entrada de nombre de usuario mediante una llamada a la función de JavaScript anterior con el `ElementReference`capturado:

[!code-razor[](javascript-interop/samples_snapshot/component1.razor?highlight=1,3,11-12)]

Para usar un método de extensión, cree un método de extensión estático que reciba la `IJSRuntime` instancia:

```csharp
public static async Task Focus(this ElementReference elementRef, IJSRuntime jsRuntime)
{
    await jsRuntime.InvokeVoidAsync(
        "exampleJsFunctions.focusElement", elementRef);
}
```

Se llama al método `Focus` directamente en el objeto. En el ejemplo siguiente se da por supuesto que el método `Focus` está disponible en el espacio de nombres `JsInteropClasses`:

[!code-razor[](javascript-interop/samples_snapshot/component2.razor?highlight=1-4,12)]

> [!IMPORTANT]
> La variable `username` solo se rellena después de representar el componente. Si se pasa un `ElementReference` sin rellenar al código JavaScript, el código JavaScript recibe un valor de `null`. Para manipular las referencias de elemento una vez finalizada la representación del componente (para establecer el foco inicial en un elemento), use los [métodos de ciclo de vida del componente OnAfterRenderAsync o OnAfterRender](xref:blazor/lifecycle#after-component-render).

Al trabajar con tipos genéricos y devolver un valor, use [ValueTask\<t >](xref:System.Threading.Tasks.ValueTask`1):

```csharp
public static ValueTask<T> GenericMethod<T>(this ElementReference elementRef, 
    IJSRuntime jsRuntime)
{
    return jsRuntime.InvokeAsync<T>(
        "exampleJsFunctions.doSomethingGeneric", elementRef);
}
```

se llama a `GenericMethod` directamente en el objeto con un tipo. En el ejemplo siguiente se da por supuesto que el `GenericMethod` está disponible en el espacio de nombres `JsInteropClasses`:

[!code-razor[](javascript-interop/samples_snapshot/component3.razor?highlight=17)]

## <a name="reference-elements-across-components"></a>Hacer referencia a elementos entre componentes

Solo se garantiza que un `ElementReference` sea válido en el método de `OnAfterRender` de un componente (y una referencia de elemento es un `struct`), por lo que no se puede pasar una referencia de elemento entre los componentes.

Para que un componente primario haga que una referencia de elemento esté disponible para otros componentes, el componente primario puede:

* Permite que los componentes secundarios registren las devoluciones de llamada.
* Invoca las devoluciones de llamada registradas durante el evento `OnAfterRender` con la referencia de elemento que se ha pasado. De forma indirecta, este enfoque permite que los componentes secundarios interactúen con la referencia del elemento primario.

En el siguiente ejemplo de webassembly de Blazor se muestra el enfoque.

En el `<head>` de *wwwroot/index.html*:

```html
<style>
    .red { color: red }
</style>
```

En el `<body>` de *wwwroot/index.html*:

```html
<script>
    function setElementClass(element, className) {
        /** @type {HTMLElement} **/
        var myElement = element;
        myElement.classList.add(className);
    }
</script>
```

*Pages/index. Razor* (componente primario):

```razor
@page "/"

<h1 @ref="_title">Hello, world!</h1>

Welcome to your new app.

<SurveyPrompt Parent="this" Title="How is Blazor working for you?" />
```

*Pages/index. Razor. CS*:

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

*Shared/SurveyPrompt. Razor* (componente secundario):

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

*Shared/SurveyPrompt. Razor. CS*:

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

## <a name="invoke-net-methods-from-javascript-functions"></a>Invocar métodos .NET desde funciones de JavaScript

### <a name="static-net-method-call"></a>Llamada al método estático de .NET

Para invocar un método .NET estático desde JavaScript, use las funciones `DotNet.invokeMethod` o `DotNet.invokeMethodAsync`. Pase el identificador del método estático al que desea llamar, el nombre del ensamblado que contiene la función y cualquier argumento. La versión asincrónica es preferible para admitir escenarios de Blazor Server. El método .NET debe ser público, estático y tener el atributo `[JSInvokable]`. Actualmente no se admite la llamada a métodos genéricos abiertos.

La aplicación de ejemplo incluye C# un método para devolver una matriz de `int`s. El atributo `JSInvokable` se aplica al método.

*Pages/JsInterop. Razor*:

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

JavaScript atendido al cliente invoca el C# método .net.

*wwwroot/exampleJsInterop. js*:

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=8-14)]

Cuando se seleccione el botón **desencadenador de método estático de .net ReturnArrayAsync** , examine la salida de la consola en las herramientas de desarrollo web del explorador.

La salida de la consola es:

```console
Array(4) [ 1, 2, 3, 4 ]
```

El cuarto valor de matriz se inserta en la matriz (`data.push(4);`) devuelta por `ReturnArrayAsync`.

De forma predeterminada, el identificador del método es el nombre del método, pero puede especificar un identificador diferente mediante el constructor `JSInvokableAttribute`:

```csharp
@code {
    [JSInvokable("DifferentMethodName")]
    public static Task<int[]> ReturnArrayAsync()
    {
        return Task.FromResult(new int[] { 1, 2, 3 });
    }
}
```

En el archivo JavaScript del lado cliente:

```javascript
returnArrayAsyncJs: function () {
  DotNet.invokeMethodAsync('BlazorSample', 'DifferentMethodName')
    .then(data => {
      data.push(4);
      console.log(data);
    });
}
```

### <a name="instance-method-call"></a>Llamada al método de instancia

También puede llamar a métodos de instancia de .NET desde JavaScript. Para invocar un método de instancia de .NET desde JavaScript:

* Pase la instancia de .NET a JavaScript ajustándola en una instancia de `DotNetObjectReference`. La instancia de .NET se pasa por referencia a JavaScript.
* Invocar métodos de instancia de .NET en la instancia de mediante las funciones `invokeMethod` o `invokeMethodAsync`. La instancia de .NET también se puede pasar como argumento al invocar otros métodos .NET desde JavaScript.

> [!NOTE]
> La aplicación de ejemplo registra mensajes en la consola del lado cliente. En los siguientes ejemplos mostrados por la aplicación de ejemplo, examine la salida de la consola del explorador en las herramientas de desarrollo del explorador.

Cuando se selecciona el botón **desencadenador de instancia de .net HelloHelper. SayHello** , se llama a `ExampleJsInterop.CallHelloHelperSayHello` y pasa un nombre, `Blazor`, al método.

*Pages/JsInterop. Razor*:

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

Para más información, consulte <xref:blazor/class-libraries>.

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

## <a name="perform-large-data-transfers-in-opno-locblazor-server-apps"></a>Realizar transferencias de datos de gran tamaño en aplicaciones de Blazor Server

En algunos escenarios, se deben transferir grandes cantidades de datos entre JavaScript y Blazor. Normalmente, se producen grandes transferencias de datos cuando:

* Las API del sistema de archivos del explorador se usan para cargar o descargar un archivo.
* Se requiere la interoperabilidad con una biblioteca de terceros.

En Blazor Server, hay una limitación para evitar el paso de mensajes de gran tamaño que pueden dar lugar a problemas de rendimiento.

Tenga en cuenta las siguientes instrucciones al desarrollar código que transfiera datos entre JavaScript y Blazor:

* Segmente los datos en partes más pequeñas y envíe los segmentos de datos secuencialmente hasta que el servidor reciba todos los datos.
* No asigne objetos grandes en JavaScript y C# código.
* No bloquee el subproceso de interfaz de usuario principal durante períodos largos al enviar o recibir datos.
* Libere la memoria consumida al completar o cancelar el proceso.
* Aplique los siguientes requisitos adicionales por motivos de seguridad:
  * Declare el tamaño máximo de archivo o de datos que se puede pasar.
  * Declare la tasa mínima de carga desde el cliente al servidor.
* Después de que el servidor reciba los datos, los datos pueden ser:
  * Se almacena temporalmente en un búfer de memoria hasta que se recopilan todos los segmentos.
  * Consumido inmediatamente. Por ejemplo, los datos se pueden almacenar inmediatamente en una base de datos o escribirse en el disco cuando se recibe cada segmento.

La siguiente clase de cargador de archivos controla la interoperabilidad de JS con el cliente. La clase de cargador usa la interoperabilidad de JS para:

* Sondear el cliente para enviar un segmento de datos.
* Anule la transacción si se agota el tiempo de espera del sondeo.

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

En el ejemplo anterior:

* La `_maxBase64SegmentSize` se establece en `8192`, que se calcula a partir de `_maxBase64SegmentSize = _segmentSize * 4 / 3`.
* Las API de bajo nivel de administración de memoria de .NET Core se usan para almacenar los segmentos de memoria en el servidor en `_uploadedSegments`.
* Se usa un método `ReceiveFile` para administrar la carga a través de la interoperabilidad JS:
  * El tamaño del archivo se determina en bytes a través de la interoperabilidad de JS con `_jsRuntime.InvokeAsync<FileInfo>('getFileSize', selector)`.
  * El número de segmentos que se van a recibir se calcula y se almacena en `numberOfSegments`.
  * Los segmentos se solicitan en un bucle `for` a través de la interoperabilidad de JS con `_jsRuntime.InvokeAsync<string>('receiveSegment', i, selector)`. Todos los segmentos, pero el último debe ser 8.192 bytes antes de la descodificación. El cliente se ve obligado a enviar los datos de una manera eficaz.
  * Para cada segmento recibido, se realizan comprobaciones antes de descodificar con <xref:System.Convert.TryFromBase64String*>.
  * Una secuencia con los datos se devuelve como un nuevo <xref:System.IO.Stream> (`SegmentedStream`) una vez completada la carga.

La clase de secuencia segmentada expone la lista de segmentos como un <xref:System.IO.Stream>no buscable de solo lectura:

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

En el código siguiente se implementan las funciones de JavaScript para recibir los datos:

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

## <a name="additional-resources"></a>Recursos adicionales

* [Ejemplo de InteropComponent. Razor (repositorio de GitHub dotnet/AspNetCore, rama de versión 3,1)](https://github.com/dotnet/AspNetCore/blob/release/3.1/src/Components/test/testassets/BasicTestApp/InteropComponent.razor)
