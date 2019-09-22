<span data-ttu-id="5ac1c-101">Aunque se está preprocesando una aplicación de servidor increíbles, algunas acciones, como llamar a JavaScript, no son posibles porque no se ha establecido una conexión con el explorador.</span><span class="sxs-lookup"><span data-stu-id="5ac1c-101">While a Blazor Server app is prerendering, certain actions, such as calling into JavaScript, aren't possible because a connection with the browser hasn't been established.</span></span> <span data-ttu-id="5ac1c-102">Es posible que los componentes tengan que representarse de forma diferente cuando se representen.</span><span class="sxs-lookup"><span data-stu-id="5ac1c-102">Components may need to render differently when prerendered.</span></span>

<span data-ttu-id="5ac1c-103">Para retrasar las llamadas de interoperabilidad de JavaScript hasta que se establezca la conexión con `OnAfterRenderAsync` el explorador, puede usar el evento del ciclo de vida del componente.</span><span class="sxs-lookup"><span data-stu-id="5ac1c-103">To delay JavaScript interop calls until after the connection with the browser is established, you can use the `OnAfterRenderAsync` component lifecycle event.</span></span> <span data-ttu-id="5ac1c-104">Solo se llama a este evento después de que la aplicación se represente por completo y se establezca la conexión del cliente.</span><span class="sxs-lookup"><span data-stu-id="5ac1c-104">This event is only called after the app is fully rendered and the client connection is established.</span></span>

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
            JSRuntime.InvokeAsync<object>(
                "setElementValue", myInput, "Value set after render");
        }
    }
}
```

<span data-ttu-id="5ac1c-105">En el componente siguiente se muestra cómo usar la interoperabilidad de JavaScript como parte de la lógica de inicialización de un componente de una manera compatible con la representación previa.</span><span class="sxs-lookup"><span data-stu-id="5ac1c-105">The following component demonstrates how to use JavaScript interop as part of a component's initialization logic in a way that's compatible with prerendering.</span></span> <span data-ttu-id="5ac1c-106">El componente muestra que es posible desencadenar una actualización de representación desde dentro `OnAfterRenderAsync`.</span><span class="sxs-lookup"><span data-stu-id="5ac1c-106">The component shows that it's possible to trigger a rendering update from inside `OnAfterRenderAsync`.</span></span> <span data-ttu-id="5ac1c-107">El desarrollador debe evitar la creación de un bucle infinito en este escenario.</span><span class="sxs-lookup"><span data-stu-id="5ac1c-107">The developer must avoid creating an infinite loop in this scenario.</span></span>

<span data-ttu-id="5ac1c-108">Donde `JSRuntime.InvokeAsync` se llama a `ElementRef` , solo se usa `OnAfterRenderAsync` en y no en cualquier método de ciclo de vida anterior porque no hay ningún elemento de JavaScript hasta después de que se represente el componente.</span><span class="sxs-lookup"><span data-stu-id="5ac1c-108">Where `JSRuntime.InvokeAsync` is called, `ElementRef` is only used in `OnAfterRenderAsync` and not in any earlier lifecycle method because there's no JavaScript element until after the component is rendered.</span></span>

<span data-ttu-id="5ac1c-109">`StateHasChanged`se llama a para rerepresentar el componente con el nuevo estado Obtenido de la llamada de interoperabilidad de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="5ac1c-109">`StateHasChanged` is called to rerender the component with the new state obtained from the JavaScript interop call.</span></span> <span data-ttu-id="5ac1c-110">El código no crea un bucle infinito porque `StateHasChanged` solo se llama cuando `infoFromJs` es `null`.</span><span class="sxs-lookup"><span data-stu-id="5ac1c-110">The code doesn't create an infinite loop because `StateHasChanged` is only called when `infoFromJs` is `null`.</span></span>

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