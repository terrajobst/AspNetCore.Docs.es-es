# <a name="blazor-webassembly-sample-app"></a>Aplicación de ejemplo de webassembler increíblemente

Este ejemplo muestra el uso de escenarios increíbles descritos en la documentación de extraordinarias.

## <a name="call-web-api-example"></a>Ejemplo de llamada a la API Web

El ejemplo de la API Web requiere una API Web en ejecución basada en la aplicación de ejemplo para el tema <a href="https://docs.microsoft.com/aspnet/core/tutorials/first-web-api">creación de una API Web con ASP.net Core</a> , que de forma predeterminada usa el mismo puerto HTTPS (5001) que la aplicación de ejemplo extraordinaria. Para usar ambas aplicaciones en el mismo equipo al mismo tiempo, cambie el puerto de la API Web (por ejemplo, use el puerto 10000). La aplicación de ejemplo realiza solicitudes a la API Web en `https://localhost:10000/api/TodoItems`. Si se usa una dirección de API Web diferente, actualice el valor constante `ServiceEndpoint` en el bloque `@code` del componente Razor.</p>

La aplicación de ejemplo realiza una solicitud de <a href="https://docs.microsoft.com/aspnet/core/security/cors">uso compartido de recursos entre orígenes (CORS)</a> desde `http://localhost:5000` o `https://localhost:5001` a la API Web. Se permiten credenciales (cookies de autorización/encabezados). Agregue la siguiente configuración de middleware de CORS al método `Startup.Configure` de la API Web:</p>

```csharp
app.UseCors(policy => 
    policy.WithOrigins("http://localhost:5000", "https://localhost:5001")
    .AllowAnyMethod()
    .WithHeaders(HeaderNames.ContentType, HeaderNames.Authorization)
    .AllowCredentials());
```

Ajuste los dominios y puertos de `WithOrigins` según sea necesario para la aplicación extraordinaria.

La API Web está configurada para que CORS permita las cookies de autorización/encabezados y solicitudes del código de cliente, pero la API Web tal como la crea el tutorial no autoriza realmente las solicitudes. Consulte los <a href="https://docs.microsoft.com/aspnet/core/security/">artículos sobre seguridad e identidad de ASP.net Core</a> para obtener instrucciones sobre la implementación.
