<span data-ttu-id="02a4a-101">El componente `App` (*app. Razor*) es similar al componente `App` que se encuentra en las aplicaciones de servidor increíbles:</span><span class="sxs-lookup"><span data-stu-id="02a4a-101">The `App` component (*App.razor*) is similar to the `App` component found in Blazor Server apps:</span></span>

* <span data-ttu-id="02a4a-102">El componente `CascadingAuthenticationState` administra la exposición del `AuthenticationState` al resto de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="02a4a-102">The `CascadingAuthenticationState` component manages exposing the `AuthenticationState` to the rest of the app.</span></span>
* <span data-ttu-id="02a4a-103">El componente de `AuthorizeRouteView` se asegura de que el usuario actual está autorizado para tener acceso a una página determinada o, de lo contrario, representa el componente de `RedirectToLogin`.</span><span class="sxs-lookup"><span data-stu-id="02a4a-103">The `AuthorizeRouteView` component makes sure that the current user is authorized to access a given page or otherwise renders the `RedirectToLogin` component.</span></span>
* <span data-ttu-id="02a4a-104">El componente `RedirectToLogin` administra el redireccionamiento de usuarios no autorizados a la página de inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="02a4a-104">The `RedirectToLogin` component manages redirecting unauthorized users to the login page.</span></span>

```razor
<CascadingAuthenticationState>
    <Router AppAssembly="@typeof(Program).Assembly">
        <Found Context="routeData">
            <AuthorizeRouteView RouteData="@routeData" 
                DefaultLayout="@typeof(MainLayout)">
                <NotAuthorized>
                    <RedirectToLogin />
                </NotAuthorized>
            </AuthorizeRouteView>
        </Found>
        <NotFound>
            <LayoutView Layout="@typeof(MainLayout)">
                <p>Sorry, there's nothing at this address.</p>
            </LayoutView>
        </NotFound>
    </Router>
</CascadingAuthenticationState>
```
