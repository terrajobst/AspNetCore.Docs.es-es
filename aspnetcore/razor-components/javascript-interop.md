---
title: Interoperabilidad de JavaScript de los componentes de Razor
author: guardrex
description: Obtenga información sobre cómo invocar funciones de JavaScript desde .NET y .NET los métodos de JavaScript en aplicaciones Blazor y componentes de Razor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/08/2019
uid: razor-components/javascript-interop
ms.openlocfilehash: f2588f4ed1ec2f01218283625fae4632d0a8ae58
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/10/2019
ms.locfileid: "59515681"
---
# <a name="razor-components-javascript-interop"></a>Interoperabilidad de JavaScript de los componentes de Razor

Por [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), y [Luke Latham](https://github.com/guardrex)

Una aplicación de los componentes de Razor puede invocar las funciones de JavaScript desde .NET y .NET métodos desde el código de JavaScript.

## <a name="invoke-javascript-functions-from-net-methods"></a>Invocar funciones de JavaScript desde los métodos de .NET

Hay veces que el código de .NET necesario para llamar a una función de JavaScript. Por ejemplo, una llamada de JavaScript puede exponer las funciones del explorador o la funcionalidad de una biblioteca de JavaScript a la aplicación.

Para llamar a JavaScript desde. NET, use el `IJSRuntime` abstracción. El `InvokeAsync<T>` método toma un identificador para la función de JavaScript que se desea invocar junto con cualquier número de argumentos serializables con JSON. Es el identificador de función en relación con el ámbito global (`window`). Si desea llamar `window.someScope.someFunction`, el identificador es `someScope.someFunction`. No hay ninguna necesidad de registrar la función antes de que se llama. El tipo de valor devuelto `T` también debe ser JSON serializable.

Para las aplicaciones de ASP.NET Core Razor componentes del lado servidor:

* Se procesan varias solicitudes de usuario mediante la aplicación del lado servidor. No llame a `JSRuntime.Current` en un componente para invocar las funciones de JavaScript.
* Insertar el `IJSRuntime` abstracción y use el objeto insertado para emitir llamadas de interoperabilidad de JavaScript.

En el siguiente ejemplo se basa en [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), un descodificador experimental basadas en JavaScript. El ejemplo muestra cómo invocar una función de JavaScript desde un C# método. La función de JavaScript acepta una matriz de bytes desde un C# método, descodifica la matriz y devuelve el texto al componente para su presentación.

Dentro de la `<head>` elemento de *wwwroot/index.HTML*, proporcione una función que usa `TextDecoder` para descodificar una matriz pasada:

```html
<script>
  window.ConvertArray = (win1251Array) => {
    var win1251decoder = new TextDecoder('windows-1251');
    var bytes = new Uint8Array(win1251Array);
    var decodedArray = win1251decoder.decode(bytes);
    console.log(decodedArray);
    return decodedArray;
  };
</script>
```

Código de JavaScript, como el código se muestra en el ejemplo anterior, también se puede cargar desde un archivo JavaScript (*.js*) con una referencia al archivo de script en el *wwwroot/index.HTML* archivo:

```html
<script src="exampleJsInterop.js"></script>
```

Los siguientes componentes:

* Invoca el `ConvertArray` función de JavaScript mediante `JsRuntime` al seleccionar un botón de componente (**convertir matriz**) está seleccionado.
* Después de llamar a la función de JavaScript, la matriz pasada se convierte en una cadena. Se devuelve la cadena para el componente para su presentación.

```cshtml
@page "/"
@inject IJSRuntime JsRuntime;

<h1>Call JavaScript Function Example</h1>

<button type="button" class="btn btn-primary" onclick="@ConvertArray">
    Convert Array
</button>

<p class="mt-2" style="font-size:1.6em">
    <span class="badge badge-success">
        @ConvertedText
    </span>
</p>

@functions {
    // Quote (c)2005 Universal Pictures: Serenity
    // https://www.uphe.com/movies/serenity
    // David Krumholtz on IMDB: https://www.imdb.com/name/nm0472710/

    private MarkupString ConvertedText =
        new MarkupString("Select the <b>Convert Array</b> button.");

    private uint[] QuoteArray = new uint[]
        {
            60, 101, 109, 62, 67, 97, 110, 39, 116, 32, 115, 116, 111, 112, 32,
            116, 104, 101, 32, 115, 105, 103, 110, 97, 108, 44, 32, 77, 97,
            108, 46, 60, 47, 101, 109, 62, 32, 45, 32, 77, 114, 46, 32, 85, 110,
            105, 118, 101, 114, 115, 101, 10, 10,
        };

    private async void ConvertArray()
    {
        var text =
            await JsRuntime.InvokeAsync<string>("ConvertArray", QuoteArray);

        ConvertedText = new MarkupString(text);

        StateHasChanged();
    }
}
```

Para usar el `IJSRuntime` abstracción, adoptar cualquiera de los enfoques siguientes:

* Insertar el `IJSRuntime` abstracción en el archivo Razor (*.razor*, *.cshtml*):

  ```cshtml
  @inject IJSRuntime JSRuntime

  @functions {
      public override void OnInit()
      {
          StocksService.OnStockTickerUpdated += stockUpdate =>
          {
              JSRuntime.InvokeAsync<object>(
                  "handleTickerChanged",
                  stockUpdate.symbol,
                  stockUpdate.price);
          };
      }
  }
  ```

* Insertar el `IJSRuntime` abstracción en una clase (*.cs*):

  ```csharp
  public class JsInteropClasses
  {
      private readonly IJSRuntime _jsRuntime;

      public JsInteropClasses(IJSRuntime jsRuntime)
      {
          _jsRuntime = jsRuntime;
      }

      public Task<string> TickerChanged(string data)
      {
          // The handleTickerChanged JavaScript method is implemented
          // in a JavaScript file, such as 'wwwroot/tickerJsInterop.js'.
          return _jsRuntime.InvokeAsync<object>(
              "handleTickerChanged",
              stockUpdate.symbol,
              stockUpdate.price);
      }
  }
  ```

* Para la generación de contenido dinámico con `BuildRenderTree`, utilice el `[Inject]` atributo:

  ```csharp
  [Inject] IJSRuntime JSRuntime { get; set; }
  ```

En la aplicación de ejemplo de cliente que acompaña a este tema, dos funciones de JavaScript están disponibles para la aplicación del lado cliente que interactúan con el DOM para recibir entradas del usuario y mostrar un mensaje de bienvenida:

* `showPrompt` &ndash; Genera un aviso para aceptar la entrada del usuario (el nombre del usuario) y devuelve el nombre al llamador.
* `displayWelcome` &ndash; Asigna un mensaje de bienvenida de la persona que llama a un objeto DOM con un `id` de `welcome`.

*wwwroot/exampleJsInterop.js*:

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=2-7)]

Colocar el `<script>` etiqueta a la que hace referencia al archivo JavaScript en el *wwwroot/index.HTML* archivo:

[!code-html[](./common/samples/3.x/BlazorSample/wwwroot/index.html?highlight=15)]

No coloque un `<script>` de etiquetas en un archivo de componente porque el `<script>` etiqueta no puede actualizarse dinámicamente.

Funciones de interoperabilidad de los métodos de .NET con el código JavaScript en el *exampleJsInterop.js* archivo mediante una llamada a `IJSRuntime.InvokeAsync<T>`.

El `IJSRuntime` abstracción es asincrónica para permitir escenarios de servidor. Si la aplicación se ejecuta del lado cliente y desea invocar una función de JavaScript de forma sincrónica, inferiores a `IJSInProcessRuntime` y llamar a `Invoke<T>` en su lugar. Se recomienda que la mayoría de las bibliotecas JavaScript interoperabilidad usa las API asincrónicas para asegurarse de que las bibliotecas están disponibles en todos los escenarios, del lado cliente o servidor.

La aplicación de ejemplo incluye un componente para demostrar la interoperabilidad de JavaScript. El componente:

* Recibe la entrada de usuario a través de un símbolo del sistema de JavaScript.
* Devuelve el texto para el componente para su procesamiento.
* Llama a una segunda función de JavaScript que interactúa con el DOM para mostrar un mensaje de bienvenida.

*Pages/JSInterop.cshtml*:

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.cshtml?name=snippet_JSInterop1&highlight=3,19-21,23-25)]

1. Cuando `TriggerJsPrompt` se ejecuta al seleccionar el componente **desencadenador JavaScript Prompt** button, el código JavaScript `showPrompt` función incluida en el *wwwroot/exampleJsInterop.js* archivo es se llama.
1. El `showPrompt` función acepta la entrada del usuario (el nombre del usuario), que está codificado en HTML y se devuelven al componente. El componente almacena el nombre del usuario en una variable local, `name`.
1. La cadena almacenada en `name` se incorpora a un mensaje de bienvenida, que se pasa a una función de JavaScript, `displayWelcome`, que representa el mensaje de bienvenida en una etiqueta de encabezado.

## <a name="capture-references-to-elements"></a>Capturar las referencias a elementos

Algunos [JavaScript interoperabilidad](xref:razor-components/javascript-interop) escenarios requieren referencias a elementos HTML. Por ejemplo, una biblioteca de interfaz de usuario puede requerir una referencia de elemento para la inicialización, o es posible que deba llamar a API de comando en un elemento, como `focus` o `play`.

Puede capturar las referencias a elementos HTML en un componente mediante la adición de un `ref` atributo al elemento HTML y, a continuación, definir un campo de tipo `ElementRef` cuyo nombre coincide con el valor de la `ref` atributo.

El ejemplo siguiente muestra la captura de una referencia al elemento de entrada de nombre de usuario:

```cshtml
<input ref="username" ...>

@functions {
    ElementRef username;
}
```

> [!NOTE]
> Hacer **no** utilizar referencias de elemento capturado como una manera de rellenar el DOM. Si lo hace, puede interferir con el modelo de representación declarativa.

En cuanto a código. NET, un `ElementRef` es un identificador opaco. El *sólo* lo que puede hacer con `ElementRef` es pasar a través al código de JavaScript a través de interoperabilidad de JavaScript. Al hacerlo, el código JavaScript del lado recibe un `HTMLElement` instancia, que puede usar con las API de DOM normal.

Por ejemplo, el código siguiente define un método de extensión de .NET que permite establecer el foco en un elemento:

*exampleJsInterop.js*:

```javascript
window.exampleJsFunctions = {
  focusElement : function (element) {
    element.focus();
  }
}
```

Use `IJSRuntime.InvokeAsync<T>` y llamar a `exampleJsFunctions.focusElement` con un `ElementRef` centrar un elemento:

[!code-cshtml[](javascript-interop/samples_snapshot/component1.cshtml?highlight=1,3,7,11-12)]

Para usar un método de extensión para centrar un elemento, cree un método de extensión estático que recibe el `IJSRuntime` instancia:

```csharp
public static Task Focus(this ElementRef elementRef, IJSRuntime jsRuntime)
{
    return jsRuntime.InvokeAsync<object>(
        "exampleJsFunctions.focusElement", elementRef);
}
```

Se llama al método directamente en el objeto. En el siguiente ejemplo se da por supuesto que estático `Focus` método está disponible desde el `JsInteropClasses` espacio de nombres:

[!code-cshtml[](javascript-interop/samples_snapshot/component2.cshtml?highlight=1,4,8,12)]

> [!IMPORTANT]
> El `username` variable solo se rellena cuando se representa el componente y su salida incluye el `>` elemento. Si intenta pasar un vacía `ElementRef` al código de JavaScript, recibe el código de JavaScript `null`. Para manipular las referencias del elemento después de que el componente ha terminado de representación (para establecer el foco inicial en un elemento) use el `OnAfterRenderAsync` o `OnAfterRender` [métodos del ciclo de vida del componente](xref:razor-components/components#lifecycle-methods).

## <a name="invoke-net-methods-from-javascript-functions"></a>Invocar métodos de .NET desde las funciones de JavaScript

### <a name="static-net-method-call"></a>Llamada de método estática de .NET

Para invocar un método estático de .NET desde JavaScript, use el `DotNet.invokeMethod` o `DotNet.invokeMethodAsync` funciones. Pasar el identificador del método estático que desea llamar, el nombre del ensamblado que contiene la función y los argumentos. La versión asincrónica es preferible para admitir escenarios de servidor. Para que se pueden invocar desde JavaScript, el método de .NET debe ser público, estático y decorados con `[JSInvokable]`. De forma predeterminada, el identificador de método es el nombre del método, pero puede especificar un identificador diferente utilizando el `JSInvokableAttribute` constructor. Actualmente no se admite llamar a métodos genéricos abiertos.

La aplicación de ejemplo incluye un C# método para devolver una matriz de `int`s. El método está decorado con el `JSInvokable` atributo.

*Pages/JsInterop.cshtml*:

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.cshtml?name=snippet_JSInterop2&highlight=7-11)]

Proporciona al cliente de JavaScript invoca el C# método de. NET.

*wwwroot/exampleJsInterop.js*:

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=8-14)]

Cuando el **método estático de .NET de desencadenador ReturnArrayAsync** botón está seleccionado, examine la salida de consola en herramientas de desarrollo del explorador web.

Es el resultado de la consola:

```console
Array(4) [ 1, 2, 3, 4 ]
```

El cuarto valor de matriz se inserta en la matriz (`data.push(4);`) devuelto por `ReturnArrayAsync`.

### <a name="instance-method-call"></a>Llamada al método de instancia

También puede llamar a métodos de instancia de .NET desde JavaScript. Para invocar un método de instancia de .NET desde JavaScript, primero pasa la instancia de .NET a JavaScript incluyéndolo en una `DotNetObjectRef` instancia. La instancia de .NET se pasa por referencia a JavaScript, y puede invocar métodos de instancia de .NET en la instancia utilizando el `invokeMethod` o `invokeMethodAsync` funciones. La instancia de .NET también se puede pasar como argumento al invocar otros métodos de .NET desde JavaScript.

> [!NOTE]
> La aplicación de ejemplo registra los mensajes en la consola de cliente. Los ejemplos siguientes que se muestra en la aplicación de ejemplo, examine la salida de la consola del explorador en herramientas de desarrollo del explorador.

Cuando el **método de instancia de desencadenador .NET HelloHelper.SayHello** botón está seleccionado, `ExampleJsInterop.CallHelloHelperSayHello` se llama y se pasa un nombre, `Blazor`, al método.

*Pages/JsInterop.cshtml*:

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.cshtml?name=snippet_JSInterop3&highlight=8-9)]

`CallHelloHelperSayHello` invoca la función de JavaScript `sayHello` con una nueva instancia de `HelloHelper`.

*JsInteropClasses/ExampleJsInterop.cs*:

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=10-16)]

*wwwroot/exampleJsInterop.js*:

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=15-18)]

El nombre se pasa a `HelloHelper`del constructor, que establece el `HelloHelper.Name` propiedad. Cuando la función de JavaScript `sayHello` se ejecuta, `HelloHelper.SayHello` devuelve el `Hello, {Name}!` mensaje, que se escribe en la consola de la función de JavaScript.

*JsInteropClasses/HelloHelper.cs*:

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

Salida de herramientas para desarrolladores del explorador web en la consola:

```console
Hello, Blazor!
```

## <a name="share-interop-code-in-a-razor-component-class-library"></a>Compartir código de interoperabilidad en una biblioteca de clases de componente de Razor

Código de interoperabilidad de JavaScript se puede incluir en una biblioteca de clases de componente de Razor (`dotnet new razorclasslib`), que le permite compartir el código en un paquete de NuGet.

La biblioteca de clases de componente de Razor controla la incrustación de recursos de JavaScript en el ensamblado compilado. Los archivos JavaScript se colocan en el *wwwroot* carpeta. Las herramientas se encarga de incrustación de recursos cuando se compila la biblioteca.

El paquete de NuGet integrado se hace referencia en el archivo de proyecto de la aplicación tal como se hace referencia a un paquete NuGet normal. Después de restaura la aplicación, puede llamar código de la aplicación en JavaScript como si fuese C#.

Para obtener más información, consulta <xref:razor-components/class-libraries>.
