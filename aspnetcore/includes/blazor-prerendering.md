Aunque una aplicación del lado servidor es más extraordinaria, algunas acciones, como llamar a JavaScript, no son posibles porque no se ha establecido una conexión con el explorador. Es posible que los componentes tengan que representarse de forma diferente cuando se representen.

Para retrasar las llamadas de interoperabilidad de JavaScript hasta que se establezca la conexión con `OnAfterRenderAsync` el explorador, puede usar el evento del ciclo de vida del componente. Solo se llama a este evento después de que la aplicación se represente por completo y se establezca la conexión del cliente.

```cshtml
@using Microsoft.JSInterop
@inject IJSRuntime JSRuntime

<input @ref="myInput" @ref:suppressField value="Value set during render" />

@code {
    private ElementReference myInput;

    protected override void OnAfterRender()
    {
        JSRuntime.InvokeAsync<object>(
            "setElementValue", myInput, "Value set after render");
    }
}
```

En el componente siguiente se muestra cómo usar la interoperabilidad de JavaScript como parte de la lógica de inicialización de un componente de una manera compatible con la representación previa. El componente muestra que es posible desencadenar una actualización de representación desde dentro `OnAfterRenderAsync`. El desarrollador debe evitar la creación de un bucle infinito en este escenario.

Donde `JSRuntime.InvokeAsync` se llama a `ElementRef` , solo se usa `OnAfterRenderAsync` en y no en cualquier método de ciclo de vida anterior porque no hay ningún elemento de JavaScript hasta después de que se represente el componente.

`StateHasChanged`se llama a para rerepresentar el componente con el nuevo estado Obtenido de la llamada de interoperabilidad de JavaScript. El código no crea un bucle infinito porque `StateHasChanged` solo se llama cuando `infoFromJs` es `null`.

```cshtml
@page "/prerendered-interop"
@using Microsoft.AspNetCore.Components
@using Microsoft.JSInterop
@inject IComponentContext ComponentContext
@inject IJSRuntime JSRuntime

<p>
    Get value via JS interop call:
    <strong id="val-get-by-interop">@(infoFromJs ?? "No value yet")</strong>
</p>

<p>
    Set value via JS interop call:
    <input id="val-set-by-interop" @ref="myElem" @ref:suppressField />
</p>

@code {
    private string infoFromJs;
    private ElementReference myElem;

    protected override async Task OnAfterRenderAsync()
    {
        // TEMPORARY: Currently we need this guard to avoid making the interop
        // call during prerendering. Soon this will be unnecessary because we
        // will change OnAfterRenderAsync so that it won't run during the
        // prerendering phase.
        if (!ComponentContext.IsConnected)
        {
            return;
        }

        if (infoFromJs == null)
        {
            infoFromJs = await JSRuntime.InvokeAsync<string>(
                "setElementValue", myElem, "Hello from interop call");

            StateHasChanged();
        }
    }
}
```

Para representar condicionalmente contenido diferente en función de si la aplicación está representando actualmente el contenido, `IsConnected` use la propiedad `IComponentContext` en el servicio. Cuando se ejecuta el lado servidor `IsConnected` , solo `true` devuelve si hay una conexión activa con el cliente. Siempre devuelve `true` cuando se ejecuta el lado cliente.

```cshtml
@page "/isconnected-example"
@using Microsoft.AspNetCore.Components.Services
@inject IComponentContext ComponentContext

<h1>IsConnected Example</h1>

<p>
    Current state:
    <strong id="connected-state">
        @(ComponentContext.IsConnected ? "connected" : "not connected")
    </strong>
</p>

<p>
    Clicks:
    <strong id="count">@count</strong>
    <button id="increment-count" @onclick="@(() => count++)">Click me</button>
</p>

@code {
    private int count;
}
```
