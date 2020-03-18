---
title: Llamada a métodos de .NET desde funciones de JavaScript en ASP.NET Core Blazor
author: guardrex
description: Obtenga información sobre cómo invocar métodos de .NET desde funciones de JavaScript en aplicaciones de Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/19/2020
no-loc:
- Blazor
- SignalR
uid: blazor/call-dotnet-from-javascript
ms.openlocfilehash: f4964341e261c65269eedafbbd6e676c1054f427
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78647411"
---
# <a name="call-net-methods-from-javascript-functions-in-aspnet-core-opno-locblazor"></a>Llamada a métodos de .NET desde funciones de JavaScript en ASP.NET Core Blazor

Por [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27) y [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Una aplicación de Blazor puede invocar funciones de JavaScript desde métodos de .NET y viceversa. Estos escenarios se denominan *interoperabilidad de JavaScript* (o *interoperabilidad de JS*).

En este artículo se describe cómo invocar métodos de .NET desde JavaScript. Para obtener información sobre cómo llamar a funciones de JavaScript desde .NET, vea <xref:blazor/call-javascript-from-dotnet>.

[Vea o descargue el código de ejemplo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([cómo descargarlo](xref:index#how-to-download-a-sample))

## <a name="static-net-method-call"></a>Llamada al método estático de .NET

Para invocar un método de .NET estático desde JavaScript, use las funciones `DotNet.invokeMethod` o `DotNet.invokeMethodAsync`. Pase el identificador del método estático al que quiere llamar, el nombre del ensamblado que contiene la función, y los argumentos. La versión asincrónica es preferible para admitir escenarios de Blazor Server. El método de .NET debe ser público y estático y debe tener el atributo `[JSInvokable]`. Actualmente no se admite la llamada a métodos genéricos abiertos.

La aplicación de ejemplo incluye un método de C# para devolver una matriz `int`. El atributo `JSInvokable` se aplica al método.

*Pages/JsInterop.razor*:

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

El archivo JavaScript servido al cliente invoca el método de .NET de C#.

*wwwroot/exampleJsInterop.js*:

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=8-14)]

Cuando se selecciona el botón **Desencadenar el método estático de .NET ReturnArrayAsync**, examine la salida de la consola en las herramientas de desarrollo web del explorador.

La salida de la consola es:

```console
Array(4) [ 1, 2, 3, 4 ]
```

El cuarto valor de matriz se inserta en la matriz (`data.push(4);`) devuelta por `ReturnArrayAsync`.

De forma predeterminada, el identificador del método es su nombre, pero puede especificar un identificador diferente mediante el constructor `JSInvokableAttribute`:

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

## <a name="instance-method-call"></a>Llamada a métodos de instancia

También puede llamar a métodos de instancia de .NET desde JavaScript. Para invocar un método de instancia de .NET desde JavaScript:

* Pase la instancia de .NET por referencia a JavaScript:
  * Realice una llamada estática a `DotNetObjectReference.Create`.
  * Encapsule la instancia de en una instancia `DotNetObjectReference` y llame a `Create` en la instancia `DotNetObjectReference`. Elimine los objetos `DotNetObjectReference` (se incluye un ejemplo más adelante en esta sección).
* Invoque métodos de instancia de .NET en la instancia mediante las funciones `invokeMethod` o `invokeMethodAsync`. La instancia de .NET también se puede pasar como argumento al invocar otros métodos de .NET desde JavaScript.

> [!NOTE]
> La aplicación de ejemplo registra los mensajes en la consola del lado cliente. En los siguientes ejemplos de esta aplicación, examine la salida de la consola del explorador en las herramientas de desarrollo del explorador.

Cuando se selecciona el botón **Desencadenar el método de instancia de .NET HelloHelper.SayHello**, se llama a `ExampleJsInterop.CallHelloHelperSayHello` y pasa un nombre, `Blazor`, al método.

*Pages/JsInterop.razor*:

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

`CallHelloHelperSayHello` invoca la función `sayHello` de JavaScript con una nueva instancia de `HelloHelper`.

*JsInteropClasses/ExampleJsInterop.cs*:

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=10-16)]

*wwwroot/exampleJsInterop.js*:

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=15-18)]

El nombre se pasa al constructor de `HelloHelper`, que establece la propiedad `HelloHelper.Name`. Cuando se ejecuta la función `sayHello` de JavaScript, `HelloHelper.SayHello` devuelve el mensaje `Hello, {Name}!`, y la función de JavaScript lo escribe en la consola.

*JsInteropClasses/HelloHelper.cs*:

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

Salida de la consola en las herramientas de desarrollo web del explorador:

```console
Hello, Blazor!
```

Para evitar una fuga de memoria y permitir la recolección de elementos no usados en un componente que crea un objeto `DotNetObjectReference`, elimine el objeto de la clase que creó la instancia de `DotNetObjectReference`:

```csharp
public class ExampleJsInterop : IDisposable
{
    private readonly IJSRuntime _jsRuntime;
    private DotNetObjectReference<HelloHelper> _objRef;

    public ExampleJsInterop(IJSRuntime jsRuntime)
    {
        _jsRuntime = jsRuntime;
    }

    public ValueTask<string> CallHelloHelperSayHello(string name)
    {
        _objRef = DotNetObjectReference.Create(new HelloHelper(name));

        return _jsRuntime.InvokeAsync<string>(
            "exampleJsFunctions.sayHello",
            _objRef);
    }

    public void Dispose()
    {
        _objRef?.Dispose();
    }
}
```
  
El patrón anterior que se muestra en la clase `ExampleJsInterop` también se puede implementar en un componente:
  
```razor
@page "/JSInteropComponent"
@using BlazorSample.JsInteropClasses
@implements IDisposable
@inject IJSRuntime JSRuntime

<h1>JavaScript Interop</h1>

<button type="button" class="btn btn-primary" @onclick="TriggerNetInstanceMethod">
    Trigger .NET instance method HelloHelper.SayHello
</button>

@code {
    private DotNetObjectReference<HelloHelper> _objRef;

    public async Task TriggerNetInstanceMethod()
    {
        _objRef = DotNetObjectReference.Create(new HelloHelper("Blazor"));

        await JSRuntime.InvokeAsync<string>(
            "exampleJsFunctions.sayHello",
            _objRef);
    }

    public void Dispose()
    {
        _objRef?.Dispose();
    }
}
```

[!INCLUDE[Share interop code in a class library](~/includes/blazor-share-interop-code.md)]

## <a name="additional-resources"></a>Recursos adicionales

* <xref:blazor/call-javascript-from-dotnet>
* [Ejemplo de InteropComponent.razor (repositorio de GitHub dotnet/AspNetCore, rama de la versión 3.1)](https://github.com/dotnet/AspNetCore/blob/release/3.1/src/Components/test/testassets/BasicTestApp/InteropComponent.razor)
* [Realización de transferencias de gran cantidad de datos en aplicaciones de Blazor Server](xref:blazor/advanced-scenarios#perform-large-data-transfers-in-blazor-server-apps)
