# <a name="blazor-webassembly-sample-app"></a>Aplicación de ejemplo de webassembler increíblemente

Este ejemplo muestra el uso de escenarios increíbles descritos en la documentación de extraordinarias.

## <a name="call-web-api-example"></a>Ejemplo de llamada a la API Web

El ejemplo de API Web requiere una API Web en ejecución basada en la aplicación de <a href="https://docs.microsoft.com/aspnet/core/tutorials/first-web-api">ejemplo para el tutorial: Cree una API Web con ASP.net Core tema</a> MVC. La aplicación de ejemplo realiza solicitudes a la API Web `https://localhost:10000/api/todo`en. Si se usa una dirección de API Web diferente, actualice `ServiceEndpoint` el valor constante en el bloque del `@functions` componente de Razor.</p>

La aplicación de ejemplo realiza una solicitud de <a href="https://docs.microsoft.com/aspnet/core/security/cors">uso compartido de recursos entre orígenes (CORS)</a> desde `http://localhost:5000` o `https://localhost:5001` a la API Web. Se permiten credenciales (cookies de autorización/encabezados). Agregue la siguiente configuración de middleware de CORS al método de `Startup.Configure` la API Web antes `UseMvc`de llamar a:</p>

```csharp
app.UseCors(policy => 
    policy.WithOrigins("http://localhost:5000", "https://localhost:5001")
    .AllowAnyMethod()
    .WithHeaders(HeaderNames.ContentType, HeaderNames.Authorization)
    .AllowCredentials());
```

Ajuste los dominios y los puertos `WithOrigins` según sea necesario para la aplicación increíblemente.

La API Web está configurada para que CORS permita las cookies de autorización/encabezados y solicitudes del código de cliente, pero la API Web tal como la crea el tutorial no autoriza realmente las solicitudes. Consulte los <a href="https://docs.microsoft.com/aspnet/core/security/">artículos sobre seguridad e identidad de ASP.net Core</a> para obtener instrucciones sobre la implementación.
