# <a name="aspnet-core-url-rewriting-sample-aspnet-core-2x"></a>Ejemplo de reescritura de dirección URL de ASP.NET Core (ASP.NET Core 2.x)

En este ejemplo se muestra el uso del software intermedio de reescritura de dirección URL de ASP.NET Core 2.x. En la aplicación se muestran las opciones de redireccionamiento de dirección URL y reescritura de dirección URL. Para obtener el ejemplo de ASP.NET Core 1.x, vea [ASP.NET Core URL Rewriting Sample (ASP.NET Core 1.x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/1.x) (Ejemplo de reescritura de dirección URL de ASP.NET Core [ASP.NET Core 1.x]).

Al ejecutar el ejemplo, se enviará una respuesta en la que aparecerá la dirección URL reescrita o redirigida al aplicar una de las reglas a una dirección URL de solicitud.

## <a name="examples-in-this-sample"></a>Ejemplos incluidos

* `AddRedirect("redirect-rule/(.*)", "redirected/$1")`
  - Código de estado correcto: 302 (Encontrado)
  - Ejemplo (redireccionamiento): **/redirect-rule/{capture_group}** a **/redirected/{capture_group}**
* `AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", skipRemainingRules: true)`
  - Código de estado correcto: 200 - Correcto
  - Ejemplo (reescritura): **/rewrite-rule/{capture_group_1}/{capture_group_2}** a **/rewritten?var1={capture_group_1}&var2={capture_group_2}**
* `AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt")`
  - Código de estado correcto: 302 (Encontrado)
  - Ejemplo (redireccionamiento): **/apache-mod-rules-redirect/{capture_group}** a **/redirected?id={capture_group}**
* `AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml")`
  - Código de estado correcto: 200 - Correcto
  - Ejemplo (reescritura): **/iis-rules-rewrite/{capture_group}** a **/rewritten?id={capture_group}**
* `Add(RedirectXMLRequests)`
  - Código de estado correcto: 301 - Movido definitivamente
  - Ejemplo (redireccionamiento): **/file.xml** a **/xmlfiles/file.xml**
* `Add(new RedirectPNGRequests(".png", "/png-images")))`<br>`Add(new RedirectPNGRequests(".jpg", "/jpg-images")))`
  - Código de estado correcto: 301 - Movido definitivamente
  - Ejemplo (redireccionamiento): **/image.png** a **/png-images/image.png**
  - Ejemplo (redireccionamiento): **/image.jpg** a **/jpg-images/image.jpg**

## <a name="using-a-physicalfileprovider"></a>Uso de `PhysicalFileProvider`
También puede obtener `IFileProvider` mediante la creación de un `PhysicalFileProvider` que se pasará a los métodos `AddApacheModRewrite()` y `AddIISUrlRewrite()`:
```csharp
using Microsoft.Extensions.FileProviders;
PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
```
## <a name="secure-redirection-extensions"></a>Extensiones de redireccionamiento seguro
En este ejemplo se incluye la configuración de `WebHostBuilder` para que la aplicación use direcciones URL (**https://localhost:5001**, **https://localhost**) y un certificado de prueba (**testCert.pfx**) para ayudar a explorar estos métodos de redireccionamiento. Agregue cualquiera de ellos a `RewriteOptions()` en **Startup.cs** para estudiar su comportamiento.


|              Método              | Código de estado |    Puerto    |
|----------------------------------|:-----------:|:----------:|
| `.AddRedirectToHttpsPermanent()` |     301     | null (465) |
|     `.AddRedirectToHttps()`      |     302     | null (465) |
|    `.AddRedirectToHttps(301)`    |     301     | null (465) |
| `.AddRedirectToHttps(301, 5001)` |     301     |    5001    |

