<span data-ttu-id="24fa0-101">Aunque se está preprocesando una aplicación de servidor increíbles, algunas acciones, como llamar a JavaScript, no son posibles porque no se ha establecido una conexión con el explorador.</span><span class="sxs-lookup"><span data-stu-id="24fa0-101">While a Blazor Server app is prerendering, certain actions, such as calling into JavaScript, aren't possible because a connection with the browser hasn't been established.</span></span> <span data-ttu-id="24fa0-102">Es posible que los componentes tengan que representarse de forma diferente cuando se representen.</span><span class="sxs-lookup"><span data-stu-id="24fa0-102">Components may need to render differently when prerendered.</span></span>

<span data-ttu-id="24fa0-103">Para retrasar las llamadas de interoperabilidad de JavaScript hasta que se establezca la conexión con el explorador, puede usar el [evento del ciclo de vida del componente OnAfterRenderAsync](xref:blazor/lifecycle#after-component-render).</span><span class="sxs-lookup"><span data-stu-id="24fa0-103">To delay JavaScript interop calls until after the connection with the browser is established, you can use the [OnAfterRenderAsync component lifecycle event](xref:blazor/lifecycle#after-component-render).</span></span> <span data-ttu-id="24fa0-104">Solo se llama a este evento después de que la aplicación se represente por completo y se establezca la conexión del cliente.</span><span class="sxs-lookup"><span data-stu-id="24fa0-104">This event is only called after the app is fully rendered and the client connection is established.</span></span>

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

<span data-ttu-id="24fa0-105">Para el código de ejemplo anterior, proporcione una `setElementText` función de JavaScript dentro del elemento `<head>` de *wwwroot/index.html* (webassembly) o *pages/_Host. cshtml* (servidor increíble).</span><span class="sxs-lookup"><span data-stu-id="24fa0-105">For the preceding example code, provide a `setElementText` JavaScript function inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server).</span></span> <span data-ttu-id="24fa0-106">Se llama a la función con `IJSRuntime.InvokeVoidAsync` y no devuelve un valor:</span><span class="sxs-lookup"><span data-stu-id="24fa0-106">The function is called with `IJSRuntime.InvokeVoidAsync` and doesn't return a value:</span></span>

```html
<script>
  window.setElementText = (element, text) => element.innerText = text;
</script>
```

> [!WARNING]
> <span data-ttu-id="24fa0-107">En el ejemplo anterior se modifica el Document Object Model (DOM) directamente solo con fines de demostración.</span><span class="sxs-lookup"><span data-stu-id="24fa0-107">The preceding example modifies the Document Object Model (DOM) directly for demonstration purposes only.</span></span> <span data-ttu-id="24fa0-108">La modificación directa del DOM con JavaScript no se recomienda en la mayoría de los escenarios, ya que JavaScript puede interferir con el seguimiento de cambios de la extraordinaria.</span><span class="sxs-lookup"><span data-stu-id="24fa0-108">Directly modifying the DOM with JavaScript isn't recommended in most scenarios because JavaScript can interfere with Blazor's change tracking.</span></span>

<span data-ttu-id="24fa0-109">En el componente siguiente se muestra cómo usar la interoperabilidad de JavaScript como parte de la lógica de inicialización de un componente de una manera compatible con la representación previa.</span><span class="sxs-lookup"><span data-stu-id="24fa0-109">The following component demonstrates how to use JavaScript interop as part of a component's initialization logic in a way that's compatible with prerendering.</span></span> <span data-ttu-id="24fa0-110">El componente muestra que es posible desencadenar una actualización de representación desde dentro `OnAfterRenderAsync`.</span><span class="sxs-lookup"><span data-stu-id="24fa0-110">The component shows that it's possible to trigger a rendering update from inside `OnAfterRenderAsync`.</span></span> <span data-ttu-id="24fa0-111">El desarrollador debe evitar la creación de un bucle infinito en este escenario.</span><span class="sxs-lookup"><span data-stu-id="24fa0-111">The developer must avoid creating an infinite loop in this scenario.</span></span>

<span data-ttu-id="24fa0-112">Cuando se llama a `JSRuntime.InvokeAsync`, `ElementRef` solo se utiliza en `OnAfterRenderAsync` y no en cualquier método de ciclo de vida anterior porque no hay ningún elemento de JavaScript hasta después de que se represente el componente.</span><span class="sxs-lookup"><span data-stu-id="24fa0-112">Where `JSRuntime.InvokeAsync` is called, `ElementRef` is only used in `OnAfterRenderAsync` and not in any earlier lifecycle method because there's no JavaScript element until after the component is rendered.</span></span>

<span data-ttu-id="24fa0-113">Se llama a [StateHasChanged](xref:blazor/lifecycle#state-changes) para rerepresentar el componente con el nuevo estado Obtenido de la llamada de interoperabilidad de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="24fa0-113">[StateHasChanged](xref:blazor/lifecycle#state-changes) is called to rerender the component with the new state obtained from the JavaScript interop call.</span></span> <span data-ttu-id="24fa0-114">El código no crea un bucle infinito porque solo se llama a `StateHasChanged` cuando se `null``infoFromJs`.</span><span class="sxs-lookup"><span data-stu-id="24fa0-114">The code doesn't create an infinite loop because `StateHasChanged` is only called when `infoFromJs` is `null`.</span></span>

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

<span data-ttu-id="24fa0-115">Para el código de ejemplo anterior, proporcione una `setElementText` función de JavaScript dentro del elemento `<head>` de *wwwroot/index.html* (webassembly) o *pages/_Host. cshtml* (servidor increíble).</span><span class="sxs-lookup"><span data-stu-id="24fa0-115">For the preceding example code, provide a `setElementText` JavaScript function inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server).</span></span> <span data-ttu-id="24fa0-116">Se llama a la función con `IJSRuntime.InvokeAsync` y devuelve un valor:</span><span class="sxs-lookup"><span data-stu-id="24fa0-116">The function is called with `IJSRuntime.InvokeAsync` and returns a value:</span></span>

```html
<script>
  window.setElementText = (element, text) => {
    element.innerText = text;
    return text;
  };
</script>
```

> [!WARNING]
> <span data-ttu-id="24fa0-117">En el ejemplo anterior se modifica el Document Object Model (DOM) directamente solo con fines de demostración.</span><span class="sxs-lookup"><span data-stu-id="24fa0-117">The preceding example modifies the Document Object Model (DOM) directly for demonstration purposes only.</span></span> <span data-ttu-id="24fa0-118">La modificación directa del DOM con JavaScript no se recomienda en la mayoría de los escenarios, ya que JavaScript puede interferir con el seguimiento de cambios de la extraordinaria.</span><span class="sxs-lookup"><span data-stu-id="24fa0-118">Directly modifying the DOM with JavaScript isn't recommended in most scenarios because JavaScript can interfere with Blazor's change tracking.</span></span>
