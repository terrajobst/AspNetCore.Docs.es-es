La página generada por el componente de `Authentication` (*pages/Authentication. Razor*) define las rutas necesarias para controlar diferentes fases de autenticación.

El componente de `RemoteAuthenticatorView`:

* Lo proporciona el paquete de `Microsoft.AspNetCore.Components.WebAssembly.Authentication`.
* Administra la realización de las acciones adecuadas en cada fase de autenticación.

```razor
@page "/authentication/{action}"
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication

<RemoteAuthenticatorView Action="@Action" />

@code {
    [Parameter]
    public string Action { get; set; }
}
```
