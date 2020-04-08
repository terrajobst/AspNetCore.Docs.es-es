---
no-loc:
- Blazor
- SignalR
ms.openlocfilehash: 5f3e22e04fe18149ec5a8acb42f42a8ef83a7664
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "78647531"
---
Durante la representación previa de una aplicación de Blazor Server, no es posible realizar ciertas acciones (como llamar a JavaScript), ya que no se ha establecido una conexión con el explorador. Es posible que los componentes tengan que representarse de forma diferente cuando se representen previamente.

Para retrasar las llamadas de interoperabilidad de JavaScript hasta que se establezca la conexión con el explorador, puede usar el [evento de ciclo de vida del componente OnAfterRenderAsync](xref:blazor/lifecycle#after-component-render). Solo se llama a este evento después de que la aplicación se represente por completo y se establezca la conexión con el cliente.

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

Para el código de ejemplo anterior, proporcione una función de JavaScript `setElementText` dentro del elemento `<head>` de *wwwroot/index.html* (WebAssembly de Blazor) o *Pages/_Host.cshtml* (Blazor Server). Se llama a la función con `IJSRuntime.InvokeVoidAsync` y no se devuelve un valor:

```html
<script>
  window.setElementText = (element, text) => element.innerText = text;
</script>
```

> [!WARNING]
> En el ejemplo anterior se modifica el Document Object Model (DOM) directamente solo con fines de demostración. No se recomienda modificar directamente el DOM con JavaScript en la mayoría de los escenarios, ya que JavaScript puede interferir con el seguimiento de cambios de Blazor.

En el componente siguiente se muestra cómo usar la interoperabilidad de JavaScript como parte de la lógica de inicialización de un componente de una manera compatible con la representación previa. El componente muestra que es posible desencadenar una actualización de representación desde dentro de `OnAfterRenderAsync`. El desarrollador debe evitar la creación de un bucle infinito en este escenario.

Cuando se llama a `JSRuntime.InvokeAsync`, `ElementRef` solo se utiliza en `OnAfterRenderAsync` y no en ningún otro método de ciclo de vida anterior porque no existe ningún elemento de JavaScript hasta después de representar el componente.

Se llama a [StateHasChanged](xref:blazor/lifecycle#state-changes) para representar el componente con el nuevo estado obtenido de la llamada de interoperabilidad de JavaScript. El código no crea un bucle infinito porque solo se llama a `StateHasChanged` cuando `infoFromJs` es `null`.

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

Para el código de ejemplo anterior, proporcione una función de JavaScript `setElementText` dentro del elemento `<head>` de *wwwroot/index.html* (WebAssembly de Blazor) o *Pages/_Host.cshtml* (Blazor Server). Se llama a la función con `IJSRuntime.InvokeAsync` y se devuelve un valor:

```html
<script>
  window.setElementText = (element, text) => {
    element.innerText = text;
    return text;
  };
</script>
```

> [!WARNING]
> En el ejemplo anterior se modifica el Document Object Model (DOM) directamente solo con fines de demostración. No se recomienda modificar directamente el DOM con JavaScript en la mayoría de los escenarios, ya que JavaScript puede interferir con el seguimiento de cambios de Blazor.
