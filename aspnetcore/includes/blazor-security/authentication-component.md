<span data-ttu-id="dbaa6-101">La página generada por el componente de `Authentication` (*pages/Authentication. Razor*) define las rutas necesarias para controlar diferentes fases de autenticación.</span><span class="sxs-lookup"><span data-stu-id="dbaa6-101">The page produced by the `Authentication` component (*Pages/Authentication.razor*) defines the routes required for handling different authentication stages.</span></span>

<span data-ttu-id="dbaa6-102">El componente de `RemoteAuthenticatorView`:</span><span class="sxs-lookup"><span data-stu-id="dbaa6-102">The `RemoteAuthenticatorView` component:</span></span>

* <span data-ttu-id="dbaa6-103">Lo proporciona el paquete de `Microsoft.AspNetCore.Components.WebAssembly.Authentication`.</span><span class="sxs-lookup"><span data-stu-id="dbaa6-103">Is provided by the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package.</span></span>
* <span data-ttu-id="dbaa6-104">Administra la realización de las acciones adecuadas en cada fase de autenticación.</span><span class="sxs-lookup"><span data-stu-id="dbaa6-104">Manages performing the appropriate actions at each stage of authentication.</span></span>

```razor
@page "/authentication/{action}"
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication

<RemoteAuthenticatorView Action="@Action" />

@code {
    [Parameter]
    public string Action { get; set; }
}
```
