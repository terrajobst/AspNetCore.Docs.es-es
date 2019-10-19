Aunque se está preprocesando una aplicación de servidor increíbles, algunas acciones, como llamar a JavaScript, no son posibles porque no se ha establecido una conexión con el explorador. Es posible que los componentes tengan que representarse de forma diferente cuando se representen.

Para retrasar las llamadas de interoperabilidad de JavaScript hasta que se establezca la conexión con el explorador, puede usar el evento del ciclo de vida de los componentes de `OnAfterRenderAsync`. Solo se llama a este evento después de que la aplicación se represente por completo y se establezca la conexión del cliente.

```cshtml
@using Microsoft.JSInterop
@inject IJSRuntime JSRuntime

<input @ref="myInput" value="Value set during render" />

@code {
    private ElementReference myInput;

    protected override void OnAfterRender(bool firstRender)
    {
        if (firstRender)
        {
            JSRuntime.InvokeVoidAsync(
                "setElementValue", myInput, "Value set after render");
        }
    }
}
```

En el componente siguiente se muestra cómo usar la interoperabilidad de JavaScript como parte de la lógica de inicialización de un componente de una manera compatible con la representación previa. El componente muestra que es posible desencadenar una actualización de representación desde dentro `OnAfterRenderAsync`. El desarrollador debe evitar la creación de un bucle infinito en este escenario.

Cuando se llama a `JSRuntime.InvokeAsync`, `ElementRef` solo se utiliza en `OnAfterRenderAsync` y no en cualquier método de ciclo de vida anterior porque no hay ningún elemento de JavaScript hasta después de que se represente el componente.

se llama a `StateHasChanged` para representarlo con el nuevo estado Obtenido de la llamada de interoperabilidad de JavaScript. El código no crea un bucle infinito porque solo se llama a `StateHasChanged` cuando se `null` `infoFromJs`.

```cshtml
@page "/prerendered-interop"
@using Microsoft.AspNetCore.Components
@using Microsoft.JSInterop
@inject IJSRuntime JSRuntime

<p>
    Get value via JS interop call:
    <strong id="val-get-by-interop">@(infoFromJs ?? "No value yet")</strong>
</p>

<p>
    Set value via JS interop call:
    <input id="val-set-by-interop" @ref="myElem" />
</p>

@code {
    private string infoFromJs;
    private ElementReference myElem;

    protected override async Task OnAfterRenderAsync(bool firstRender)
    {
        if (firstRender && infoFromJs == null)
        {
            infoFromJs = await JSRuntime.InvokeAsync<string>(
                "setElementValue", myElem, "Hello from interop call");

            StateHasChanged();
        }
    }
}
```
