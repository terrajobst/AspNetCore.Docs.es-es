El componente `FetchData` muestra cómo:

* Aprovisione un token de acceso.
* Use el token de acceso para llamar a una API de recursos protegidos en la aplicación de *servidor* .

La Directiva de `@attribute [Authorize]` indica al sistema de autorización webassembly de increíble que el usuario debe estar autorizado para visitar este componente. La presencia del atributo en la aplicación *cliente* no impide que se llame a la API del servidor sin credenciales adecuadas. La aplicación de *servidor* también debe usar `[Authorize]` en los puntos de conexión adecuados para protegerlos correctamente.

`AuthenticationService.RequestAccessToken();` se encarga de solicitar un token de acceso que se pueda agregar a la solicitud para llamar a la API. Si el token se almacena en caché o el servicio puede aprovisionar un nuevo token de acceso sin la interacción del usuario, la solicitud de token se realiza correctamente. De lo contrario, se produce un error en la solicitud de token.

Para obtener el token real que se va a incluir en la solicitud, la aplicación debe comprobar que la solicitud se ha realizado correctamente llamando a `tokenResult.TryGetToken(out var token)`. 

Si la solicitud se realizó correctamente, la variable de token se rellena con el token de acceso. La propiedad `Value` del token expone la cadena literal que se va a incluir en el encabezado de solicitud `Authorization`.

Si se produjo un error en la solicitud porque no se pudo aprovisionar el token sin la interacción del usuario, el resultado del token contiene una dirección URL de redireccionamiento. Al navegar a esta dirección URL, el usuario se dirige a la página de inicio de sesión y vuelve a la página actual después de una autenticación correcta.

```razor
@page "/fetchdata"
...
@attribute [Authorize]

...

@code {
    private WeatherForecast[] forecasts;

    protected override async Task OnInitializedAsync()
    {
        var httpClient = new HttpClient();
        httpClient.BaseAddress = new Uri(Navigation.BaseUri);

        var tokenResult = await AuthenticationService.RequestAccessToken();

        if (tokenResult.TryGetToken(out var token))
        {
            httpClient.DefaultRequestHeaders.Add("Authorization", 
                $"Bearer {token.Value}");
            forecasts = await httpClient.GetJsonAsync<WeatherForecast[]>(
                "WeatherForecast");
        }
        else
        {
            Navigation.NavigateTo(tokenResult.RedirectUrl);
        }

    }
}
```

Para obtener más información, consulte guardar el estado de la [aplicación antes de una operación de autenticación](xref:security/blazor/webassembly/additional-scenarios#save-app-state-before-an-authentication-operation).
