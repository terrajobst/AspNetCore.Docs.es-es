---
title: Ciclo de vida de ASP.NET Core Blazor
author: guardrex
description: Aprenda a usar los métodos de ciclo de vida de los componentes de Razor en ASP.NET Core aplicaciones Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2019
no-loc:
- Blazor
- SignalR
uid: blazor/lifecycle
ms.openlocfilehash: ecacd0a9728cbefd716e9dc7cd8a8c62f3df6e0d
ms.sourcegitcommit: d2ba66023884f0dca115ff010bd98d5ed6459283
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/14/2020
ms.locfileid: "77213394"
---
# <a name="aspnet-core-opno-locblazor-lifecycle"></a>Ciclo de vida de ASP.NET Core Blazor

Por [Luke Latham](https://github.com/guardrex) y [Daniel Roth](https://github.com/danroth27)

El marco de Blazor incluye métodos de ciclo de vida sincrónicos y asincrónicos. Invalide los métodos del ciclo de vida para realizar operaciones adicionales en los componentes durante la inicialización y representación de componentes.

## <a name="lifecycle-methods"></a>Métodos de ciclo de vida

### <a name="component-initialization-methods"></a>Métodos de inicialización de componentes

<xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync*> y <xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitialized*> se invocan cuando se inicializa el componente después de haber recibido sus parámetros iniciales de su componente primario. Utilice `OnInitializedAsync` cuando el componente realiza una operación asincrónica y debe actualizarse cuando se complete la operación.

En el caso de una operación sincrónica, invalide `OnInitialized`:

```csharp
protected override void OnInitialized()
{
    ...
}
```

Para realizar una operación asincrónica, invalide `OnInitializedAsync` y use la palabra clave `await` en la operación:

```csharp
protected override async Task OnInitializedAsync()
{
    await ...
}
```

Blazor las aplicaciones de servidor que [preprocesan la](xref:blazor/hosting-model-configuration#render-mode) llamada de contenido `OnInitializedAsync` **_dos veces_** :

* Una vez que el componente se representa de forma estática como parte de la página.
* Una segunda vez cuando el explorador establece una conexión de vuelta al servidor.

Para evitar que el código de Desarrollador en `OnInitializedAsync` se ejecute dos veces, consulte la sección [reconexión con estado después de la representación previa](#stateful-reconnection-after-prerendering) .

Mientras que una aplicación de Blazor Server se está preprocesando, no es posible realizar ciertas acciones, como llamar a JavaScript, porque no se ha establecido una conexión con el explorador. Es posible que los componentes tengan que representarse de forma diferente cuando se representen. Para obtener más información, consulte la sección [detectar cuándo se está representando la aplicación](#detect-when-the-app-is-prerendering) .

### <a name="before-parameters-are-set"></a>Antes de establecer los parámetros

<xref:Microsoft.AspNetCore.Components.ComponentBase.SetParametersAsync*> establece los parámetros proporcionados por el elemento primario del componente en el árbol de representación:

```csharp
public override async Task SetParametersAsync(ParameterView parameters)
{
    await ...

    await base.SetParametersAsync(parameters);
}
```

<xref:Microsoft.AspNetCore.Components.ParameterView> contiene el conjunto completo de valores de parámetro cada vez que se llama a `SetParametersAsync`.

La implementación predeterminada de `SetParametersAsync` establece el valor de cada propiedad con el atributo `[Parameter]` o `[CascadingParameter]` que tiene un valor correspondiente en el `ParameterView`. Los parámetros que no tienen un valor correspondiente en `ParameterView` se dejan sin cambios.

Si no se invoca `base.SetParametersAync`, el código personalizado puede interpretar el valor de los parámetros entrantes de cualquier manera que sea necesario. Por ejemplo, no hay ningún requisito para asignar los parámetros de entrada a las propiedades de la clase.

### <a name="after-parameters-are-set"></a>Después de establecer los parámetros

se llama a <xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSetAsync*> y <xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSet*>:

* Cuando el componente se inicializa y ha recibido su primer conjunto de parámetros de su componente primario.
* Cuando el componente primario vuelve a presentar y proporciona:
  * Solo se conocen tipos inmutables primitivos de los que se ha cambiado al menos un parámetro.
  * Cualquier parámetro de tipo complejo. El marco de trabajo no puede saber si los valores de un parámetro de tipo complejo se han mutado internamente, por lo que trata el conjunto de parámetros como modificado.

```csharp
protected override async Task OnParametersSetAsync()
{
    await ...
}
```

> [!NOTE]
> El trabajo asincrónico al aplicar parámetros y valores de propiedad debe producirse durante el evento del ciclo de vida de `OnParametersSetAsync`.

```csharp
protected override void OnParametersSet()
{
    ...
}
```

### <a name="after-component-render"></a>Después de representar el componente

se llama a <xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRenderAsync*> y <xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRender*> después de que un componente haya terminado de representar. En este punto se rellenan las referencias a elementos y componentes. Use esta fase para realizar pasos de inicialización adicionales mediante el contenido representado, como la activación de bibliotecas de JavaScript de terceros que operan en los elementos DOM representados.

El parámetro `firstRender` para `OnAfterRenderAsync` y `OnAfterRender`:

* Se establece en `true` la primera vez que se representa la instancia del componente.
* Se puede utilizar para asegurarse de que el trabajo de inicialización solo se realiza una vez.

```csharp
protected override async Task OnAfterRenderAsync(bool firstRender)
{
    if (firstRender)
    {
        await ...
    }
}
```

> [!NOTE]
> El trabajo asincrónico inmediatamente después de la representación debe producirse durante el evento del ciclo de vida de `OnAfterRenderAsync`.
>
> Incluso si se devuelve un <xref:System.Threading.Tasks.Task> de `OnAfterRenderAsync`, el marco de trabajo no programa un ciclo de representación adicional para el componente una vez que la tarea se completa. Esto se hace para evitar un bucle de representación infinito. Es diferente de los otros métodos de ciclo de vida, que programan un ciclo de representación adicional una vez completada la tarea devuelta.

```csharp
protected override void OnAfterRender(bool firstRender)
{
    if (firstRender)
    {
        ...
    }
}
```

no se llama a `OnAfterRender` y `OnAfterRenderAsync` *al representarse en el servidor.*

### <a name="suppress-ui-refreshing"></a>Suprimir actualización de IU

Invalide <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender*> para suprimir la actualización de la interfaz de usuario. Si la implementación devuelve `true`, la interfaz de usuario se actualiza:

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

se llama a `ShouldRender` cada vez que se representa el componente.

Aunque se invalide `ShouldRender`, el componente siempre se representa inicialmente.

## <a name="state-changes"></a>Cambios de estado

<xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*> notifica al componente que su estado ha cambiado. Cuando es aplicable, la llamada a `StateHasChanged` hace que el componente se represente.

## <a name="handle-incomplete-async-actions-at-render"></a>Controlar acciones asincrónicas incompletas en la representación

Es posible que las acciones asincrónicas realizadas en eventos del ciclo de vida no se hayan completado antes de que se represente el componente. Los objetos podrían estar `null` o rellenarse de forma incompleta con datos mientras se ejecuta el método de ciclo de vida. Proporcione lógica de representación para confirmar que los objetos se inicializan. Representar los elementos de la interfaz de usuario del marcador de posición (por ejemplo, un mensaje de carga) mientras los objetos se `null`.

En el `FetchData` componente de las plantillas de Blazor, `OnInitializedAsync` se invalida a asincrónica recibir datos de previsión (`forecasts`). Cuando `forecasts` se `null`, se muestra al usuario un mensaje de carga. Una vez que se completa el `Task` devuelto por `OnInitializedAsync`, el componente se representará con el estado actualizado.

*Pages/FetchData. Razor* en la plantilla de Blazor Server:

[!code-razor[](lifecycle/samples_snapshot/3.x/FetchData.razor?highlight=9,21,25)]

## <a name="component-disposal-with-idisposable"></a>Eliminación de componentes con IDisposable

Si un componente implementa <xref:System.IDisposable>, se llama al [método Dispose](/dotnet/standard/garbage-collection/implementing-dispose) cuando se quita el componente de la interfaz de usuario. El siguiente componente utiliza `@implements IDisposable` y el método `Dispose`:

```razor
@using System
@implements IDisposable

...

@code {
    public void Dispose()
    {
        ...
    }
}
```

> [!NOTE]
> No se admite la llamada a <xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*> en `Dispose`. `StateHasChanged` se puede invocar como parte de la anulación del representador, por lo que no se admite la solicitud de actualizaciones de la interfaz de usuario en ese momento.

## <a name="handle-errors"></a>errores

Para obtener información sobre cómo controlar los errores durante la ejecución del método de ciclo de vida, vea <xref:blazor/handle-errors#lifecycle-methods>.

## <a name="stateful-reconnection-after-prerendering"></a>Reconexión con estado después de la representación previa

En una aplicación de Blazor Server cuando se `ServerPrerendered``RenderMode`, el componente se representa de forma estática como parte de la página. Una vez que el explorador vuelve a establecer una conexión con el servidor, el componente se *vuelve*a representar y el componente ahora es interactivo. Si el método de ciclo de vida [{Async} inicializado](xref:blazor/lifecycle#component-initialization-methods) para inicializar el componente está presente, el método se ejecuta *dos veces*:

* Cuando el componente se representa de forma estática.
* Una vez establecida la conexión al servidor.

Esto puede dar lugar a un cambio notable en los datos mostrados en la interfaz de usuario cuando el componente se representa finalmente.

Para evitar el escenario de representación doble en una aplicación de Blazor Server:

* Pase un identificador que se puede usar para almacenar en caché el estado durante la representación previa y recuperar el estado después de que se reinicie la aplicación.
* Use el identificador durante la representación previa para guardar el estado del componente.
* Use el identificador después de la representación previa para recuperar el estado almacenado en caché.

En el código siguiente se muestra una `WeatherForecastService` actualizada en una aplicación de Blazor Server basada en plantillas que evita la representación doble:

```csharp
public class WeatherForecastService
{
    private static readonly string[] _summaries = new[]
    {
        "Freezing", "Bracing", "Chilly", "Cool", "Mild",
        "Warm", "Balmy", "Hot", "Sweltering", "Scorching"
    };
    
    public WeatherForecastService(IMemoryCache memoryCache)
    {
        MemoryCache = memoryCache;
    }
    
    public IMemoryCache MemoryCache { get; }

    public Task<WeatherForecast[]> GetForecastAsync(DateTime startDate)
    {
        return MemoryCache.GetOrCreateAsync(startDate, async e =>
        {
            e.SetOptions(new MemoryCacheEntryOptions
            {
                AbsoluteExpirationRelativeToNow = 
                    TimeSpan.FromSeconds(30)
            });

            var rng = new Random();

            await Task.Delay(TimeSpan.FromSeconds(10));

            return Enumerable.Range(1, 5).Select(index => new WeatherForecast
            {
                Date = startDate.AddDays(index),
                TemperatureC = rng.Next(-20, 55),
                Summary = _summaries[rng.Next(_summaries.Length)]
            }).ToArray();
        });
    }
}
```

Para obtener más información sobre el `RenderMode`, vea <xref:blazor/hosting-model-configuration#render-mode>.

## <a name="detect-when-the-app-is-prerendering"></a>Detectar cuándo se está preprocesando la aplicación

[!INCLUDE[](~/includes/blazor-prerendering.md)]
