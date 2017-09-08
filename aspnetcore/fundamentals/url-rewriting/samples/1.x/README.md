# <a name="aspnet-core-url-rewriting-sample-aspnet-core-1x"></a>Ejemplo de reescritura de la URL de núcleo de ASP.NET (ASP.NET Core 1.x)

Este ejemplo muestra el uso de ASP.NET Core 1.x Middleware de reescritura de dirección URL. La aplicación muestra el redireccionamiento de la dirección URL y opciones de reescritura de dirección URL. Para obtener el ejemplo de 2.x ASP.NET Core, vea [ejemplo de reescritura de URL de ASP.NET Core (ASP.NET Core 2.x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/2.x).

Al ejecutar el ejemplo, se enviará una respuesta que muestra la dirección URL redirigida o ha vuelto a escribir cuando una de las reglas se aplica a una dirección URL de la solicitud.

## <a name="examples-in-this-sample"></a>En los ejemplos de este ejemplo

* `AddRedirect("redirect-rule/(.*)", "$1")`
  - Código de estado correcto: 302 (encontrado)
  - Ejemplo (redirección): **/redirect-rule / {capture_group}** a **/redirected/ {capture_group}**
* `AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", skipRemainingRules: true)`
  - Código de estado correcto: 200 (correcto)
  - Ejemplo (reescritura): **/rewrite-rule / {capture_group_1} / {capture_group_2}** a **/ reescrito? var1 = {capture_group_1} & var2 = {capture_group_2}**
* `AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt")`
  - Código de estado correcto: 302 (encontrado)
  - Ejemplo (redirección): **/apache-mod-rules-redirect / {capture_group}** a **/ redirigido? Id. = {capture_group}**
* `AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml")`
  - Código de estado correcto: 200 (correcto)
  - Ejemplo (reescritura): **/iis-rules-rewrite / {capture_group}** a **/ reescrito? Id. = {capture_group}**
* `Add(RedirectXMLRequests)`
  - Código de estado correcto: 301 (movida permanentemente)
  - Ejemplo (redirección): **/file.xml** a **/xmlfiles/file.xml**
* `Add(new RedirectPNGRequests(".png", "/png-images")))`<br>`Add(new RedirectPNGRequests(".jpg", "/jpg-images")))`
  - Código de estado correcto: 301 (movida permanentemente)
  - Ejemplo (redirección): **/image.png** a **/png-images/image.png**
  - Ejemplo (redirección): **/image.jpg** a **/jpg-images/image.jpg**

## <a name="using-a-physicalfileprovider"></a>Con un`PhysicalFileProvider`
También puede obtener un `IFileProvider` mediante la creación de un `PhysicalFileProvider` que se pasará a la `AddApacheModRewrite()` y `AddIISUrlRewrite()` métodos:
```csharp
using Microsoft.Extensions.FileProviders;
PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
```
## <a name="secure-redirection-extensions"></a>Extensiones de redirección segura
Este ejemplo incluye `WebHostBuilder` configuración de la aplicación usar direcciones URL (**https://localhost:5001**, **https://localhost**) y un certificado de prueba (**testCert.pfx**) para ayudar a explorar estos redirigir métodos. Agregar cualquiera de ellos para que la `RewriteOptions()` en **Startup.cs** para estudiar su comportamiento.

Método | Código de estado | Puerto
--- | :---: | :---:
`.AddRedirectToHttpsPermanent()` | 301 | null (465)
`.AddRedirectToHttps()` | 302 | null (465)
`.AddRedirectToHttps(301)` | 301 | null (465)
`.AddRedirectToHttps(301, 5001)` | 301 | 5001
