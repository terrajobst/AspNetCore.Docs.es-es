El componente de `LoginDisplay` (*Shared/LoginDisplay. Razor*) se representa en el componente `MainLayout` (*Shared/MainLayout. Razor*) y administra los comportamientos siguientes:

* Para usuarios autenticados:
  * Muestra el nombre de usuario actual.
  * Ofrece un botón para cerrar la sesión de la aplicación.
* En el caso de los usuarios anónimos, ofrece la opción de iniciar sesión.

```razor
@using Microsoft.AspNetCore.Components.Authorization
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication
@inject NavigationManager Navigation
@inject SignOutSessionStateManager SignOutManager

<AuthorizeView>
    <Authorized>
        Hello, @context.User.Identity.Name!
        <button class="nav-link btn btn-link" @onclick="BeginSignOut">
            Log out
        </button>
    </Authorized>
    <NotAuthorized>
        <a href="authentication/login">Log in</a>
    </NotAuthorized>
</AuthorizeView>

@code {
    private async Task BeginSignOut(MouseEventArgs args)
    {
        await SignOutManager.SetSignOutState();
        Navigation.NavigateTo("authentication/logout");
    }
}
```
