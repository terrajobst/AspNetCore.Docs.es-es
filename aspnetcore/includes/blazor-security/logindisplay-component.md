<span data-ttu-id="cdbfa-101">El componente de `LoginDisplay` (*Shared/LoginDisplay. Razor*) se representa en el componente `MainLayout` (*Shared/MainLayout. Razor*) y administra los comportamientos siguientes:</span><span class="sxs-lookup"><span data-stu-id="cdbfa-101">The `LoginDisplay` component (*Shared/LoginDisplay.razor*) is rendered in the `MainLayout` component (*Shared/MainLayout.razor*) and manages the following behaviors:</span></span>

* <span data-ttu-id="cdbfa-102">Para usuarios autenticados:</span><span class="sxs-lookup"><span data-stu-id="cdbfa-102">For authenticated users:</span></span>
  * <span data-ttu-id="cdbfa-103">Muestra el nombre de usuario actual.</span><span class="sxs-lookup"><span data-stu-id="cdbfa-103">Displays the current username.</span></span>
  * <span data-ttu-id="cdbfa-104">Ofrece un botón para cerrar la sesión de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="cdbfa-104">Offers a button to log out of the app.</span></span>
* <span data-ttu-id="cdbfa-105">En el caso de los usuarios anónimos, ofrece la opción de iniciar sesión.</span><span class="sxs-lookup"><span data-stu-id="cdbfa-105">For anonymous users, offers the option to log in.</span></span>

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
