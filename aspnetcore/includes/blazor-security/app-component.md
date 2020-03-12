El componente `App` (*app. Razor*) es similar al componente `App` que se encuentra en las aplicaciones de servidor increíbles:

* El componente `CascadingAuthenticationState` administra la exposición del `AuthenticationState` al resto de la aplicación.
* El componente de `AuthorizeRouteView` se asegura de que el usuario actual está autorizado para tener acceso a una página determinada o, de lo contrario, representa el componente de `RedirectToLogin`.
* El componente `RedirectToLogin` administra el redireccionamiento de usuarios no autorizados a la página de inicio de sesión.

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
