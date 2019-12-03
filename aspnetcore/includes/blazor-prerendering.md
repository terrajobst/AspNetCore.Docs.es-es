Aunque se está preprocesando una aplicación de servidor increíbles, algunas acciones, como llamar a JavaScript, no son posibles porque no se ha establecido una conexión con el explorador. Es posible que los componentes tengan que representarse de forma diferente cuando se representen.

Para retrasar las llamadas de interoperabilidad de JavaScript hasta que se establezca la conexión con el explorador, puede usar el [evento del ciclo de vida del componente OnAfterRenderAsync](xref:blazor/lifecycle#after-component-render). Solo se llama a este evento después de que la aplicación se represente por completo y se establezca la conexión del cliente.

```cshtml
@using Microsoft.JSInterop
@inject IJSRuntime JSRuntime

<div @ref="divElement">Text during render</div>

@code {
    private ElementReference divElement;

    protected override async Task OnAfterRenderAsync(bool firstRender)
    {
        if (firstRender)
        {
            await JSRuntime.InvokeVoidAsync(
                "setElementText", divElement, "Text after render");
        }
    }
}
```

Para el código de ejemplo anterior, proporcione una `setElementText` función de JavaScript dentro del elemento `<head>` de *wwwroot/index.html* (webassembly) o *pages/_Host. cshtml* (servidor increíble). Se llama a la función con `IJSRuntime.InvokeVoidAsync` y no devuelve un valor:

```html
<script>
  window.setElementText = (element, text) => element.innerText = text;
</script>
```

> [!WARNING]
> En el ejemplo anterior se modifica el Document Object Model (DOM) directamente solo con fines de demostración. La modificación directa del DOM con JavaScript no se recomienda en la mayoría de los escenarios, ya que JavaScript puede interferir con el seguimiento de cambios de la extraordinaria.

En el componente siguiente se muestra cómo usar la interoperabilidad de JavaScript como parte de la lógica de inicialización de un componente de una manera compatible con la representación previa. El componente muestra que es posible desencadenar una actualización de representación desde dentro `OnAfterRenderAsync`. El desarrollador debe evitar la creación de un bucle infinito en este escenario.

Cuando se llama a `JSRuntime.InvokeAsync`, `ElementRef` solo se utiliza en `OnAfterRenderAsync` y no en cualquier método de ciclo de vida anterior porque no hay ningún elemento de JavaScript hasta después de que se represente el componente.

Se llama a [StateHasChanged](xref:blazor/lifecycle#state-changes) para rerepresentar el componente con el nuevo estado Obtenido de la llamada de interoperabilidad de JavaScript. El código no crea un bucle infinito porque solo se llama a `StateHasChanged` cuando se `null``infoFromJs`.

```cshtml
@page "/prerendered-interop"
@using Microsoft.AspNetCore.Components
@using Microsoft.JSInterop
@inject IJSRuntime JSRuntime

<p>
    Get value via JS interop call:
    <strong id="val-get-by-interop">@(infoFromJs ?? "No value yet")</strong>
</p>

Set value via JS interop call:
<div id="val-set-by-interop" @ref="divElement"></div>

@code {
    private string infoFromJs;
    private ElementReference divElement;

    protected override async Task OnAfterRenderAsync(bool firstRender)
    {
        if (firstRender && infoFromJs == null)
        {
            infoFromJs = await JSRuntime.InvokeAsync<string>(
                "setElementText", divElement, "Hello from interop call!");

            StateHasChanged();
        }
    }
}
```

Para el código de ejemplo anterior, proporcione una `setElementText` función de JavaScript dentro del elemento `<head>` de *wwwroot/index.html* (webassembly) o *pages/_Host. cshtml* (servidor increíble). Se llama a la función con `IJSRuntime.InvokeAsync` y devuelve un valor:

```html
<script>
  window.setElementText = (element, text) => {
    element.innerText = text;
    return text;
  };
</script>
```

> [!WARNING]
> En el ejemplo anterior se modifica el Document Object Model (DOM) directamente solo con fines de demostración. La modificación directa del DOM con JavaScript no se recomienda en la mayoría de los escenarios, ya que JavaScript puede interferir con el seguimiento de cambios de la extraordinaria.
