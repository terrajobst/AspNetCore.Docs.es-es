---
title: Habilitación de solicitudes entre orígenes (CORS) en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo CORS como estándar para permitir o rechazar solicitudes entre orígenes en una aplicación ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 01/23/2020
uid: security/cors
ms.openlocfilehash: 57098be73164c71d1b0d1fe2f3aee7ec41a32346
ms.sourcegitcommit: eca76bd065eb94386165a0269f1e95092f23fa58
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2020
ms.locfileid: "76727312"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a>Habilitación de solicitudes entre orígenes (CORS) en ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

En este artículo se muestra cómo habilitar CORS en una aplicación ASP.NET Core.

La seguridad del explorador evita que una página web realice solicitudes a un dominio diferente del que sirvió a la Página Web. Esta restricción se llama *Directiva del mismo origen*. La Directiva del mismo origen impide que un sitio malintencionado Lea datos confidenciales de otro sitio. En ocasiones, es posible que desee permitir que otros sitios realicen solicitudes entre orígenes a la aplicación. Para obtener más información, consulte el [artículo Mozilla CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).

[Uso compartido de recursos entre orígenes](https://www.w3.org/TR/cors/) (CORS):

* Es un estándar del W3C que permite a un servidor relajar la Directiva del mismo origen.
* **No** es una característica de seguridad, CORS reduce la seguridad. Una API no es más segura al permitir CORS. Para obtener más información, vea [Cómo funciona CORS](#how-cors).
* Permite que un servidor permita explícitamente algunas solicitudes entre orígenes mientras se rechazan otras.
* Es más seguro y más flexible que las técnicas anteriores, como [JSONP](/dotnet/framework/wcf/samples/jsonp).

[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample))

## <a name="same-origin"></a>Mismo origen

Dos direcciones URL tienen el mismo origen si tienen esquemas, hosts y puertos idénticos ([RFC 6454](https://tools.ietf.org/html/rfc6454)).

Estas dos direcciones URL tienen el mismo origen:

* `https://example.com/foo.html`
* `https://example.com/bar.html`

Estas direcciones URL tienen distintos orígenes que las dos direcciones URL anteriores:

* `https://example.net` &ndash; dominio diferente
* `https://www.example.com/foo.html` &ndash; subdominio diferente
* `http://example.com/foo.html` &ndash; esquema diferente
* `https://example.com:9000/foo.html` &ndash; puerto diferente

Internet Explorer no tiene en cuenta el puerto al comparar orígenes.

## <a name="cors-with-named-policy-and-middleware"></a>CORS con Directiva con nombre y middleware

Middleware de CORS controla las solicitudes entre orígenes. El código siguiente habilita CORS para toda la aplicación con el origen especificado:

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet&highlight=8,14-23,38)]

El código anterior:

* Establece el nombre de la Directiva en "\_myAllowSpecificOrigins". El nombre de la Directiva es arbitrario.
* Llama al método de extensión <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*>, que habilita CORS.
* Llama a <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> con una [expresión lambda](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions). La expresión lambda toma un objeto de <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder>. [Las opciones de configuración](#cors-policy-options), como `WithOrigins`, se describen más adelante en este artículo.

La llamada al método <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> agrega los servicios CORS al contenedor de servicio de la aplicación:

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet2)]

Para obtener más información, vea [Opciones de directiva de CORS](#cpo) en este documento.

El método <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> puede encadenar métodos, tal y como se muestra en el código siguiente:

[!code-csharp[](cors/sample/Cors/WebAPI/Startup2.cs?name=snippet2)]

Nota: la dirección URL **no** debe contener una barra diagonal final (`/`). Si la dirección URL finaliza con `/`, la comparación devuelve `false` y no se devuelve ningún encabezado.

::: moniker range=">= aspnetcore-3.0"

<a name="acpall"></a>

### <a name="apply-cors-policies-to-all-endpoints"></a>Aplicar directivas CORS a todos los puntos de conexión

El código siguiente aplica las directivas de CORS a todos los puntos de conexión de aplicaciones a través del middleware CORS:
```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    // Preceding code ommitted.
    app.UseRouting();

    app.UseCors();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapControllers();
    });

    // Following code ommited.
}
```

> [!WARNING]
> Con el enrutamiento de punto de conexión, el middleware de CORS debe estar configurado para ejecutarse entre las llamadas a `UseRouting` y `UseEndpoints`. Una configuración incorrecta hará que el middleware deje de funcionar correctamente.

::: moniker-end

::: moniker range="<= aspnetcore-2.2"
El código siguiente aplica las directivas de CORS a todos los puntos de conexión de aplicaciones a través del middleware CORS:
```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseHsts();
    }

    app.UseCors();

    app.UseHttpsRedirection();
    app.UseMvc();
}
```
Nota: se debe llamar a `UseCors` antes de `UseMvc`.

::: moniker-end

Consulte [habilitación de CORS en Razor pages, controladores y métodos de acción](#ecors) para aplicar la Directiva CORS en el nivel de página/controlador/acción.

Consulte [prueba de CORS](#test) para obtener instrucciones sobre cómo probar el código anterior.

<a name="ecors"></a>

::: moniker range=">= aspnetcore-3.0"

## <a name="enable-cors-with-endpoint-routing"></a>Habilitación de CORS con enrutamiento de punto de conexión

Con el enrutamiento de punto de conexión, CORS se puede habilitar en cada punto de conexión mediante el `RequireCors` conjunto de métodos de extensión.

```csharp
app.UseEndpoints(endpoints =>
{
  endpoints.MapGet("/echo", async context => context.Response.WriteAsync("echo"))
    .RequireCors("policy-name");
});

```

Del mismo modo, CORS también puede habilitarse para todos los controladores:

```csharp
app.UseEndpoints(endpoints =>
{
  endpoints.MapControllers().RequireCors("policy-name");
});
```
::: moniker-end

## <a name="enable-cors-with-attributes"></a>Habilitación de CORS con atributos

El atributo [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) proporciona una alternativa a la aplicación de CORS globalmente. El atributo `[EnableCors]` habilita CORS para los puntos de conexión seleccionados, en lugar de todos los extremos.

Use `[EnableCors]` para especificar la directiva predeterminada y `[EnableCors("{Policy String}")]` para especificar una directiva.

El atributo `[EnableCors]` se puede aplicar a:

* `PageModel` de página de Razor
* Controller
* Método de acción del controlador

Puede aplicar diferentes directivas al controlador/página-modelo/acción con el atributo `[EnableCors]`. Cuando el atributo `[EnableCors]` se aplica a un método Controllers/Page-Model/Action y CORS está habilitado en middleware, se aplican ambas directivas. Se recomienda no combinar directivas. Use el `[EnableCors]` atributo o middleware, no ambos en la misma aplicación.

En el código siguiente se aplica una directiva diferente a cada método:

[!code-csharp[](cors/sample/Cors/WebAPI/Controllers/WidgetController.cs?name=snippet&highlight=6,14)]

En el código siguiente se crea una directiva predeterminada de CORS y una directiva denominada `"AnotherPolicy"`:

[!code-csharp[](cors/sample/Cors/WebAPI/StartupMultiPolicy.cs?name=snippet&highlight=12-28)]

### <a name="disable-cors"></a>Deshabilitar CORS

El atributo [&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) deshabilita CORS para el controlador/página-modelo/acción.

<a name="cpo"></a>

## <a name="cors-policy-options"></a>Opciones de directiva de CORS

En esta sección se describen las distintas opciones que se pueden establecer en una directiva de CORS:

* [Establecer los orígenes permitidos](#set-the-allowed-origins)
* [Establecer los métodos HTTP permitidos](#set-the-allowed-http-methods)
* [Establecer los encabezados de solicitud permitidos](#set-the-allowed-request-headers)
* [Establecer los encabezados de respuesta expuestos](#set-the-exposed-response-headers)
* [Credenciales en solicitudes entre orígenes](#credentials-in-cross-origin-requests)
* [Establecer la hora de expiración de la comprobación previa](#set-the-preflight-expiration-time)

se llama a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> en `Startup.ConfigureServices`. Para algunas opciones, puede resultar útil leer la sección [Cómo funciona CORS](#how-cors) en primer lugar.

## <a name="set-the-allowed-origins"></a>Establecer los orígenes permitidos

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; permite solicitudes de CORS desde todos los orígenes con cualquier esquema (`http` o `https`). `AllowAnyOrigin` es inseguro porque *cualquier sitio web* puede realizar solicitudes entre orígenes a la aplicación.

::: moniker range=">= aspnetcore-2.2"

> [!NOTE]
> Especificar `AllowAnyOrigin` y `AllowCredentials` es una configuración no segura y puede dar lugar a la falsificación de solicitudes entre sitios. El servicio CORS devuelve una respuesta de CORS no válida cuando se configura una aplicación con ambos métodos.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

> [!NOTE]
> Especificar `AllowAnyOrigin` y `AllowCredentials` es una configuración no segura y puede dar lugar a la falsificación de solicitudes entre sitios. En el caso de una aplicación segura, especifique una lista exacta de orígenes si el cliente debe autorizarse para obtener acceso a los recursos del servidor.

::: moniker-end

`AllowAnyOrigin` afecta a las solicitudes preparatorias y al encabezado `Access-Control-Allow-Origin`. Para obtener más información, consulte la sección [solicitudes preparatorias](#preflight-requests) .

::: moniker range=">= aspnetcore-2.0"

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; establece la propiedad <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> de la Directiva para que sea una función que permita que los orígenes coincidan con un dominio comodín configurado al evaluar si se permite el origen.

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=100-105&highlight=4-5)]

::: moniker-end

### <a name="set-the-allowed-http-methods"></a>Establecer los métodos HTTP permitidos

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:

* Permite cualquier método HTTP:
* Afecta a las solicitudes preparatorias y al encabezado `Access-Control-Allow-Methods`. Para obtener más información, consulte la sección [solicitudes preparatorias](#preflight-requests) .

### <a name="set-the-allowed-request-headers"></a>Establecer los encabezados de solicitudes permitidos

Para permitir el envío de encabezados específicos en una solicitud de CORS, denominados *encabezados de solicitud de autor*, llame a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> y especifique los encabezados permitidos:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

Para permitir todos los encabezados de solicitud de autor, llame a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

Esta configuración afecta a las solicitudes preparatorias y al encabezado `Access-Control-Request-Headers`. Para obtener más información, consulte la sección [solicitudes preparatorias](#preflight-requests) .

::: moniker range=">= aspnetcore-2.2"

Una coincidencia de directiva de middleware de CORS con encabezados específicos especificados por `WithHeaders` solo es posible cuando los encabezados enviados en `Access-Control-Request-Headers` coinciden exactamente con los encabezados indicados en `WithHeaders`.

Por ejemplo, considere una aplicación configurada de la siguiente manera:

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

Middleware de CORS rechaza una solicitud preparatoria con el siguiente encabezado de solicitud porque `Content-Language` ([HeaderNames. ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) no aparece en `WithHeaders`:

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

La aplicación devuelve una respuesta *200 OK* pero no envía los encabezados de CORS. Por lo tanto, el explorador no intenta la solicitud entre orígenes.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

El middleware de CORS siempre permite el envío de cuatro encabezados en el `Access-Control-Request-Headers`, independientemente de los valores configurados en CorsPolicy. Headers. Esta lista de encabezados incluye:

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Origin`

Por ejemplo, considere una aplicación configurada de la siguiente manera:

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

Middleware de CORS responde correctamente a una solicitud preparatoria con el siguiente encabezado de solicitud porque `Content-Language` siempre está en la lista de permitidos:

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

::: moniker-end

### <a name="set-the-exposed-response-headers"></a>Establecer los encabezados de respuesta expuestos

De forma predeterminada, el explorador no expone todos los encabezados de respuesta a la aplicación. Para obtener más información, consulte [uso compartido de recursos entre orígenes (terminología) de W3C: encabezado de respuesta simple](https://www.w3.org/TR/cors/#simple-response-header).

Los encabezados de respuesta que están disponibles de forma predeterminada son:

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

La especificación CORS llama a estos encabezados de *respuesta simple*. Para que otros encabezados estén disponibles para la aplicación, llame a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=73-78&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a>Credenciales en solicitudes entre orígenes

Las credenciales requieren un tratamiento especial en una solicitud de CORS. De forma predeterminada, el explorador no envía credenciales con una solicitud entre orígenes. Las credenciales incluyen cookies y esquemas de autenticación HTTP. Para enviar credenciales con una solicitud entre orígenes, el cliente debe establecer `XMLHttpRequest.withCredentials` en `true`.

Usar `XMLHttpRequest` directamente:

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

Usar jQuery:

```javascript
$.ajax({
  type: 'get',
  url: 'https://www.example.com/api/test',
  xhrFields: {
    withCredentials: true
  }
});
```

Uso de la [API fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):

```javascript
fetch('https://www.example.com/api/test', {
    credentials: 'include'
});
```

El servidor debe permitir las credenciales. Para permitir las credenciales entre orígenes, llame a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=82-87&highlight=5)]

La respuesta HTTP incluye un encabezado `Access-Control-Allow-Credentials`, que indica al explorador que el servidor permite credenciales para una solicitud entre orígenes.

Si el explorador envía credenciales pero la respuesta no incluye un encabezado de `Access-Control-Allow-Credentials` válido, el explorador no expone la respuesta a la aplicación y se produce un error en la solicitud entre orígenes.

Permitir las credenciales entre orígenes es un riesgo para la seguridad. Un sitio web en otro dominio puede enviar las credenciales del usuario que ha iniciado sesión a la aplicación en nombre del usuario sin el conocimiento del usuario. <!-- TODO Review: When using `AllowCredentials`, all CORS enabled domains must be trusted.
I don't like "all CORS enabled domains must be trusted", because it implies that if you're not using  `AllowCredentials`, domains don't need to be trusted. -->

La especificación CORS también indica que el establecimiento de orígenes en `"*"` (todos los orígenes) no es válido si el encabezado `Access-Control-Allow-Credentials` está presente.

### <a name="preflight-requests"></a>Solicitudes preparatorias

En algunas solicitudes de CORS, el explorador envía una solicitud adicional antes de efectuar la solicitud real. Esta solicitud se denomina *solicitud preparatoria*. El explorador puede omitir la solicitud preparatoria si se cumplen las condiciones siguientes:

* El método de solicitud es GET, HEAD o POST.
* La aplicación no establece encabezados de solicitud distintos de `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`o `Last-Event-ID`.
* El encabezado `Content-Type`, si se establece, tiene uno de los siguientes valores:
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

La regla de los encabezados de solicitud establecidos para la solicitud de cliente se aplica a los encabezados que establece la aplicación mediante una llamada a `setRequestHeader` en el objeto `XMLHttpRequest`. La especificación CORS llama a estos encabezados de *solicitud Author*. La regla no se aplica a los encabezados que el explorador puede establecer, como `User-Agent`, `Host`o `Content-Length`.

El siguiente es un ejemplo de una solicitud preparatoria:

```
OPTIONS https://myservice.azurewebsites.net/api/test HTTP/1.1
Accept: */*
Origin: https://myclient.azurewebsites.net
Access-Control-Request-Method: PUT
Access-Control-Request-Headers: accept, x-my-custom-header
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
Content-Length: 0
```

La solicitud previa al vuelo usa el método HTTP OPTIONs. Incluye dos encabezados especiales:

* `Access-Control-Request-Method`: método HTTP que se utilizará para la solicitud real.
* `Access-Control-Request-Headers`: una lista de encabezados de solicitud que la aplicación establece en la solicitud real. Como se indicó anteriormente, esto no incluye los encabezados que establece el explorador, como `User-Agent`.

Una solicitud preparatoria de CORS podría incluir un encabezado `Access-Control-Request-Headers`, que indica al servidor los encabezados que se envían con la solicitud real.

Para permitir encabezados específicos, llame a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

Para permitir todos los encabezados de solicitud de autor, llame a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

Los exploradores no son totalmente coherentes en el modo en que establecen `Access-Control-Request-Headers`. Si establece encabezados en algo distinto de `"*"` (o usa <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), debe incluir al menos `Accept`, `Content-Type`y `Origin`, además de los encabezados personalizados que desee admitir.

A continuación se ofrece un ejemplo de respuesta a la solicitud preparatoria (siempre que el servidor permita la solicitud):

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 0
Access-Control-Allow-Origin: https://myclient.azurewebsites.net
Access-Control-Allow-Headers: x-my-custom-header
Access-Control-Allow-Methods: PUT
Date: Wed, 20 May 2015 06:33:22 GMT
```

La respuesta incluye un encabezado `Access-Control-Allow-Methods` que enumera los métodos permitidos y, opcionalmente, un encabezado `Access-Control-Allow-Headers`, que enumera los encabezados permitidos. Si la solicitud preparatoria se realiza correctamente, el explorador envía la solicitud real.

Si se deniega la solicitud preparatoria, la aplicación devuelve una respuesta *200 OK* pero no envía los encabezados de CORS. Por lo tanto, el explorador no intenta la solicitud entre orígenes.

### <a name="set-the-preflight-expiration-time"></a>Establecer el tiempo de expiración de las comprobaciones preparatorias

El encabezado `Access-Control-Max-Age` especifica cuánto tiempo se puede almacenar en caché la respuesta a la solicitud preparatoria. Para establecer este encabezado, llame a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=91-96&highlight=5)]

<a name="how-cors"></a>

## <a name="how-cors-works"></a>Cómo funciona CORS

En esta sección se describe lo que sucede en una solicitud de [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) en el nivel de los mensajes http.

* CORS **no** es una característica de seguridad. CORS es un estándar del W3C que permite a un servidor relajar la Directiva del mismo origen.
  * Por ejemplo, un actor malintencionado podría usar [impedir el scripting entre sitios (XSS)](xref:security/cross-site-scripting) en el sitio y ejecutar una solicitud entre sitios a su sitio habilitado para CORS para robar información.
* La API no es más segura al permitir CORS.
  * Es el cliente (explorador) el que debe aplicar CORS. El servidor ejecuta la solicitud y devuelve la respuesta, es el cliente el que devuelve un error y bloquea la respuesta. Por ejemplo, cualquiera de las siguientes herramientas mostrará la respuesta del servidor:
    * [Fiddler](https://www.telerik.com/fiddler)
    * [Postman](https://www.getpostman.com/)
    * [HttpClient de .NET](/dotnet/csharp/tutorials/console-webapiclient)
    * Un explorador web escribiendo la dirección URL en la barra de direcciones.
* Es una forma de que un servidor permita que los exploradores ejecuten una solicitud de API [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) o [Fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) entre orígenes que, de lo contrario, se prohibirá.
  * Los exploradores (sin CORS) no pueden realizar solicitudes entre orígenes. Antes de CORS, se usaba [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) para eludir esta restricción. JSONP no usa XHR, usa la etiqueta `<script>` para recibir la respuesta. Los scripts pueden cargarse entre orígenes.

La [especificación CORS](https://www.w3.org/TR/cors/) presentó varios encabezados HTTP nuevos que permiten solicitudes entre orígenes. Si un explorador es compatible con CORS, establece estos encabezados automáticamente para las solicitudes entre orígenes. No se necesita código JavaScript personalizado para habilitar CORS.

A continuación se presenta un ejemplo de una solicitud entre orígenes. El encabezado `Origin` proporciona el dominio del sitio que realiza la solicitud. El encabezado `Origin` es obligatorio y debe ser diferente del host.

```
GET https://myservice.azurewebsites.net/api/test HTTP/1.1
Referer: https://myclient.azurewebsites.net/
Accept: */*
Accept-Language: en-US
Origin: https://myclient.azurewebsites.net
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
```

Si el servidor permite la solicitud, establece el encabezado `Access-Control-Allow-Origin` en la respuesta. El valor de este encabezado coincide con el encabezado `Origin` de la solicitud o es el valor de carácter comodín `"*"`, lo que significa que se permite cualquier origen:

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Type: text/plain; charset=utf-8
Access-Control-Allow-Origin: https://myclient.azurewebsites.net
Date: Wed, 20 May 2015 06:27:30 GMT
Content-Length: 12

Test message
```

Si la respuesta no incluye el encabezado `Access-Control-Allow-Origin`, se produce un error en la solicitud entre orígenes. En concreto, el explorador no permite la solicitud. Aunque el servidor devuelva una respuesta correcta, el explorador no pone la respuesta a disposición de la aplicación cliente.

<a name="test"></a>

## <a name="test-cors"></a>Prueba de CORS

Para probar CORS:

1. [Cree un proyecto de API](xref:tutorials/first-web-api). Como alternativa, puede [descargar el ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors).
1. Habilite CORS con uno de los enfoques de este documento. Por ejemplo:

  [!code-csharp[](cors/sample/Cors/WebAPI/StartupTest.cs?name=snippet2&highlight=13-18)]

  > [!WARNING]
  > `WithOrigins("https://localhost:<port>");` solo se debe usar para probar una aplicación de ejemplo similar a la [descarga del código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors).

1. Cree un proyecto de aplicación web (Razor Pages o MVC). En el ejemplo se utiliza Razor Pages. Puede crear la aplicación web en la misma solución que el proyecto de API.
1. Agregue el siguiente código resaltado al archivo *index. cshtml* :

  [!code-csharp[](cors/sample/Cors/ClientApp/Pages/Index2.cshtml?highlight=7-99)]

1. En el código anterior, reemplace `url: 'https://<web app>.azurewebsites.net/api/values/1',` por la dirección URL a la aplicación implementada.
1. Implemente el proyecto de API. Por ejemplo, [implemente en Azure](xref:host-and-deploy/azure-apps/index).
1. Ejecute la aplicación Razor Pages o MVC desde el escritorio y haga clic en el botón **probar** . Use las herramientas F12 para revisar los mensajes de error.
1. Quitar el origen localhost de `WithOrigins` e implementar la aplicación. Como alternativa, ejecute la aplicación cliente con un puerto diferente. Por ejemplo, ejecute desde Visual Studio.
1. Pruebe con la aplicación cliente. Los errores de CORS devuelven un error, pero el mensaje de error no está disponible para JavaScript. Use la pestaña consola de las herramientas F12 para ver el error. Dependiendo del explorador, obtendrá un error (en la consola de herramientas de F12) similar al siguiente:

   * Uso de Microsoft Edge:

     **SEC7120: [CORS] el origen `https://localhost:44375` no encontró `https://localhost:44375` en el encabezado de respuesta Access-Control-Allow-Origin para el recurso de origen cruzado en `https://webapi.azurewebsites.net/api/values/1`**

   * Usar Chrome:

     **La Directiva CORS ha bloqueado el acceso a XMLHttpRequest en `https://webapi.azurewebsites.net/api/values/1` desde el origen `https://localhost:44375`: no hay ningún encabezado "Access-Control-Allow-Origin" en el recurso solicitado.**
     
Los puntos de conexión habilitados para CORS se pueden probar con una herramienta, como [Fiddler](https://www.telerik.com/fiddler) o [Postman](https://www.getpostman.com/). Cuando se usa una herramienta, el origen de la solicitud especificada por el encabezado `Origin` debe ser diferente del host que recibe la solicitud. Si la solicitud no es de *origen cruzado* según el valor del encabezado `Origin`:

* No es necesario que el middleware de CORS procese la solicitud.
* Los encabezados CORS no se devuelven en la respuesta.

## <a name="cors-in-iis"></a>CORS en IIS

Al implementar en IIS, CORS debe ejecutarse antes de la autenticación de Windows si el servidor no está configurado para permitir el acceso anónimo. Para admitir este escenario, es necesario instalar y configurar el [módulo IIS CORS](https://www.iis.net/downloads/microsoft/iis-cors-module) para la aplicación.

## <a name="additional-resources"></a>Recursos adicionales

* [Uso compartido de recursos entre orígenes (CORS)](https://developer.mozilla.org/docs/Web/HTTP/CORS)
* [Introducción al módulo IIS CORS](https://blogs.iis.net/iisteam/getting-started-with-the-iis-cors-module)
