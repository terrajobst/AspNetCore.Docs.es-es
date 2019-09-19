---
title: Habilitación de solicitudes entre orígenes (CORS) en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo CORS como estándar para permitir o rechazar solicitudes entre orígenes en una aplicación ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 04/07/2019
uid: security/cors
ms.openlocfilehash: a34b77ad799a00707048c923b82b48774ce91682
ms.sourcegitcommit: b1e480e1736b0fe0e4d8dce4a4cf5c8e47fc2101
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/19/2019
ms.locfileid: "71108075"
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

* `https://example.net`&ndash; Dominio diferente
* `https://www.example.com/foo.html`&ndash; Subdominio diferente
* `http://example.com/foo.html`&ndash; Esquema diferente
* `https://example.com:9000/foo.html`&ndash; Puerto diferente

Internet Explorer no tiene en cuenta el puerto al comparar orígenes.

## <a name="cors-with-named-policy-and-middleware"></a>CORS con Directiva con nombre y middleware

Middleware de CORS controla las solicitudes entre orígenes. El código siguiente habilita CORS para toda la aplicación con el origen especificado:

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet&highlight=8,14-23,38)]

El código anterior:

* Establece el nombre de la directiva\_en "myAllowSpecificOrigins". El nombre de la Directiva es arbitrario.
* Llama al <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> método de extensión, que habilita CORS.
* Llama <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> a con una [expresión lambda](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions). La expresión lambda toma un objeto <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder>. [Las opciones de configuración](#cors-policy-options), `WithOrigins`como, se describen más adelante en este artículo.

La <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> llamada al método agrega los servicios CORS al contenedor de servicio de la aplicación:

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet2)]

Para obtener más información, vea [Opciones de directiva de CORS](#cpo) en este documento.

El <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> método puede encadenar métodos, tal y como se muestra en el código siguiente:

[!code-csharp[](cors/sample/Cors/WebAPI/Startup2.cs?name=snippet2)]

Nota: La dirección URL **no** debe contener una barra diagonal final`/`(). Si la dirección URL termina con `/`, la comparación devuelve `false` y no se devuelve ningún encabezado.

::: moniker range=">= aspnetcore-3.0"

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
> Con el enrutamiento de punto de conexión, el middleware de CORS debe estar configurado para `UseRouting` ejecutarse entre las llamadas a y. `UseEndpoints` Una configuración incorrecta hará que el middleware deje de funcionar correctamente.

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
Nota: `UseCors` se debe llamar a `UseMvc`antes de.

::: moniker-end

Consulte [habilitación de CORS en Razor pages, controladores y métodos de acción](#ecors) para aplicar la Directiva CORS en el nivel de página/controlador/acción.

Consulte [prueba de CORS](#test) para obtener instrucciones sobre cómo probar el código anterior.

<a name="ecors"></a>

::: moniker range=">= aspnetcore-3.0"

## <a name="enable-cors-with-endpoint-routing"></a>Habilitación de CORS con enrutamiento de punto de conexión

Con el enrutamiento de punto de conexión, CORS se puede habilitar en cada punto de `RequireCors` conexión mediante el conjunto de métodos de extensión.

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

El [ &lbrack;atributoEnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) proporciona una alternativa a la aplicación de CORS globalmente. El `[EnableCors]` atributo habilita CORS para los puntos de conexión seleccionados, en lugar de todos los extremos.

Se `[EnableCors]` usa para especificar la directiva predeterminada `[EnableCors("{Policy String}")]` y para especificar una directiva.

El `[EnableCors]` atributo se puede aplicar a:

* Página de Razor`PageModel`
* Responsable del tratamiento de datos
* Método de acción del controlador

Puede aplicar diferentes directivas al controlador/página-modelo o acción con el `[EnableCors]` atributo. Cuando el `[EnableCors]` atributo se aplica a un método Controllers/Page-Model/Action y CORS está habilitado en middleware, se aplican ambas directivas. Se recomienda no combinar directivas. Use el `[EnableCors]` atributo o middleware, no ambos en la misma aplicación.

En el código siguiente se aplica una directiva diferente a cada método:

[!code-csharp[](cors/sample/Cors/WebAPI/Controllers/WidgetController.cs?name=snippet&highlight=6,14)]

En el código siguiente se crea una directiva predeterminada de CORS y `"AnotherPolicy"`una directiva denominada:

[!code-csharp[](cors/sample/Cors/WebAPI/StartupMultiPolicy.cs?name=snippet&highlight=12-28)]

### <a name="disable-cors"></a>Deshabilitar CORS

[ El&lbrack;atributoDisableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) deshabilita CORS para el controlador/página-modelo/acción.

<a name="cpo"></a>

## <a name="cors-policy-options"></a>Opciones de directiva de CORS

En esta sección se describen las distintas opciones que se pueden establecer en una directiva de CORS:

* [Establecer los orígenes permitidos](#set-the-allowed-origins)
* [Establecer los métodos HTTP permitidos](#set-the-allowed-http-methods)
* [Establecer los encabezados de solicitudes permitidos](#set-the-allowed-request-headers)
* [Establecer los encabezados de respuesta expuestos](#set-the-exposed-response-headers)
* [Credenciales en solicitudes entre orígenes](#credentials-in-cross-origin-requests)
* [Establecer el tiempo de expiración de las comprobaciones preparatorias](#set-the-preflight-expiration-time)

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*>se llama a `Startup.ConfigureServices`en. Para algunas opciones, puede resultar útil leer la sección [Cómo funciona CORS](#how-cors) en primer lugar.

## <a name="set-the-allowed-origins"></a>Establecer los orígenes permitidos

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*>Permite solicitudes de CORS de todos los orígenes con cualquier`http` esquema `https`(o). &ndash; `AllowAnyOrigin`no es seguro porque *cualquier sitio web* puede realizar solicitudes entre orígenes a la aplicación.

::: moniker range=">= aspnetcore-2.2"

> [!NOTE]
> `AllowAnyOrigin` Especificar y `AllowCredentials` es una configuración no segura y puede dar lugar a la falsificación de solicitudes entre sitios. El servicio CORS devuelve una respuesta de CORS no válida cuando se configura una aplicación con ambos métodos.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

> [!NOTE]
> `AllowAnyOrigin` Especificar y `AllowCredentials` es una configuración no segura y puede dar lugar a la falsificación de solicitudes entre sitios. En el caso de una aplicación segura, especifique una lista exacta de orígenes si el cliente debe autorizarse para obtener acceso a los recursos del servidor.

::: moniker-end

`AllowAnyOrigin`afecta a las solicitudes preparatorias `Access-Control-Allow-Origin` y al encabezado. Para obtener más información, consulte la sección [solicitudes preparatorias](#preflight-requests) .

::: moniker range=">= aspnetcore-2.0"

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*>&ndash; Establece la<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> propiedad de la Directiva para que sea una función que permita que los orígenes coincidan con un dominio comodín configurado al evaluar si se permite el origen.

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=100-104&highlight=4)]

::: moniker-end

### <a name="set-the-allowed-http-methods"></a>Establecer los métodos HTTP permitidos

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:

* Permite cualquier método HTTP:
* Afecta a las solicitudes preparatorias `Access-Control-Allow-Methods` y al encabezado. Para obtener más información, consulte la sección [solicitudes preparatorias](#preflight-requests) .

### <a name="set-the-allowed-request-headers"></a>Establecer los encabezados de solicitudes permitidos

Para permitir el envío de encabezados específicos en una solicitud de CORS, denominados *encabezados de solicitud*de autor <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> , llame a y especifique los encabezados permitidos:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

Para permitir todos los encabezados de solicitud de autor <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>, llame a:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

Esta configuración afecta a las solicitudes preparatorias `Access-Control-Request-Headers` y al encabezado. Para obtener más información, consulte la sección [solicitudes preparatorias](#preflight-requests) .

::: moniker range=">= aspnetcore-2.2"

Una coincidencia de directiva de middleware de CORS con encabezados específicos `WithHeaders` especificados por solo es posible cuando los encabezados `Access-Control-Request-Headers` enviados coinciden exactamente con los encabezados `WithHeaders`indicados en.

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

El middleware de CORS siempre permite que se envíen cuatro encabezados en el `Access-Control-Request-Headers` , independientemente de los valores configurados en CorsPolicy. Headers. Esta lista de encabezados incluye:

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Origin`

Por ejemplo, considere una aplicación configurada de la siguiente manera:

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

El middleware de CORS responde correctamente a una solicitud preparatoria con el siguiente encabezado de solicitud `Content-Language` porque siempre está en la lista de permitidos:

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

::: moniker-end

### <a name="set-the-exposed-response-headers"></a>Establecer los encabezados de respuesta expuestos

De forma predeterminada, el explorador no expone todos los encabezados de respuesta a la aplicación. Para obtener más información, [consulte uso compartido de recursos entre orígenes de W3C (terminología): Encabezado](https://www.w3.org/TR/cors/#simple-response-header)de respuesta simple.

Los encabezados de respuesta que están disponibles de forma predeterminada son:

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

La especificación CORS llama a estos encabezados de *respuesta simple*. Para que otros encabezados estén disponibles para la aplicación, llame <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>a:

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

El servidor debe permitir las credenciales. Para permitir las credenciales entre orígenes, <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>llame a:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=82-87&highlight=5)]

La respuesta http incluye un `Access-Control-Allow-Credentials` encabezado, que indica al explorador que el servidor permite credenciales para una solicitud entre orígenes.

Si el explorador envía credenciales pero la respuesta no incluye un `Access-Control-Allow-Credentials` encabezado válido, el explorador no expone la respuesta a la aplicación y se produce un error en la solicitud entre orígenes.

Permitir las credenciales entre orígenes es un riesgo para la seguridad. Un sitio web en otro dominio puede enviar las credenciales del usuario que ha iniciado sesión a la aplicación en nombre del usuario sin el conocimiento del usuario. <!-- TODO Review: When using `AllowCredentials`, all CORS enabled domains must be trusted.
I don't like "all CORS enabled domains must be trusted", because it implies that if you're not using  `AllowCredentials`, domains don't need to be trusted. -->

La especificación CORS también indica que el establecimiento de `"*"` orígenes en (todos los orígenes) no `Access-Control-Allow-Credentials` es válido si el encabezado está presente.

### <a name="preflight-requests"></a>Solicitudes preparatorias

En algunas solicitudes de CORS, el explorador envía una solicitud adicional antes de efectuar la solicitud real. Esta solicitud se denomina *solicitud preparatoria*. El explorador puede omitir la solicitud preparatoria si se cumplen las condiciones siguientes:

* El método de solicitud es GET, HEAD o POST.
* La aplicación no establece encabezados de solicitud distintos de `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`o `Last-Event-ID`.
* El `Content-Type` encabezado, si está establecido, tiene uno de los valores siguientes:
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

La regla de los encabezados de solicitud establecidos para la solicitud de cliente se aplica a los encabezados que establece la `setRequestHeader` aplicación mediante `XMLHttpRequest` una llamada a en el objeto. La especificación CORS llama a estos encabezados de *solicitud Author*. La regla no se aplica a los encabezados que el explorador puede establecer, `User-Agent`como `Host`, o `Content-Length`.

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

* `Access-Control-Request-Method`: Método HTTP que se utilizará para la solicitud real.
* `Access-Control-Request-Headers`: Una lista de encabezados de solicitud que la aplicación establece en la solicitud real. Como se indicó anteriormente, esto no incluye los encabezados que establece el explorador, `User-Agent`como.

Una solicitud preparatoria de CORS podría incluir un `Access-Control-Request-Headers` encabezado, que indica al servidor los encabezados que se envían con la solicitud real.

Para permitir encabezados específicos, llame <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>a:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

Para permitir todos los encabezados de solicitud de autor <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>, llame a:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

Los exploradores no son totalmente coherentes en `Access-Control-Request-Headers`el modo en que se establecen. Si establece encabezados en un valor `"*"` distinto de (o use <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), debe incluir al menos `Accept`, `Content-Type`y `Origin`, además de los encabezados personalizados que desee admitir.

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

La respuesta incluye un `Access-Control-Allow-Methods` encabezado que enumera los métodos permitidos y, opcionalmente, un `Access-Control-Allow-Headers` encabezado, que enumera los encabezados permitidos. Si la solicitud preparatoria se realiza correctamente, el explorador envía la solicitud real.

Si se deniega la solicitud preparatoria, la aplicación devuelve una respuesta *200 OK* pero no envía los encabezados de CORS. Por lo tanto, el explorador no intenta la solicitud entre orígenes.

### <a name="set-the-preflight-expiration-time"></a>Establecer el tiempo de expiración de las comprobaciones preparatorias

El `Access-Control-Max-Age` encabezado especifica cuánto tiempo se puede almacenar en caché la respuesta a la solicitud preparatoria. Para establecer este encabezado, llame <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>a:

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
  * Los exploradores (sin CORS) no pueden realizar solicitudes entre orígenes. Antes de CORS, se usaba [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) para eludir esta restricción. JSONP no usa XHR, usa la `<script>` etiqueta para recibir la respuesta. Los scripts pueden cargarse entre orígenes.

La [especificación CORS](https://www.w3.org/TR/cors/) presentó varios encabezados HTTP nuevos que permiten solicitudes entre orígenes. Si un explorador es compatible con CORS, establece estos encabezados automáticamente para las solicitudes entre orígenes. No se necesita código JavaScript personalizado para habilitar CORS.

A continuación se presenta un ejemplo de una solicitud entre orígenes. El encabezado `Origin` proporciona el dominio del sitio que está realizando la solicitud:

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

Si el servidor permite la solicitud, establece el `Access-Control-Allow-Origin` encabezado en la respuesta. El valor de este encabezado coincide con el `Origin` encabezado de la solicitud o con el valor `"*"`comodín, lo que significa que se permite cualquier origen:

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

Si la respuesta no incluye el `Access-Control-Allow-Origin` encabezado, se produce un error en la solicitud de origen cruzado. En concreto, el explorador no permite la solicitud. Aunque el servidor devuelva una respuesta correcta, el explorador no pone la respuesta a disposición de la aplicación cliente.

<a name="test"></a>

## <a name="test-cors"></a>Prueba de CORS

Para probar CORS:

1. [Cree un proyecto de API](xref:tutorials/first-web-api). Como alternativa, puede [descargar el ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors).
1. Habilite CORS con uno de los enfoques de este documento. Por ejemplo:

  [!code-csharp[](cors/sample/Cors/WebAPI/StartupTest.cs?name=snippet2&highlight=13-18)]

  > [!WARNING]
  > `WithOrigins("https://localhost:<port>");`solo se debe usar para probar una aplicación de ejemplo similar a la [descarga del código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors).

1. Cree un proyecto de aplicación web (Razor Pages o MVC). En el ejemplo se utiliza Razor Pages. Puede crear la aplicación web en la misma solución que el proyecto de API.
1. Agregue el siguiente código resaltado al archivo *index. cshtml* :

  [!code-csharp[](cors/sample/Cors/ClientApp/Pages/Index2.cshtml?highlight=7-99)]

1. En el código anterior, reemplace `url: 'https://<web app>.azurewebsites.net/api/values/1',` por la dirección URL a la aplicación implementada.
1. Implemente el proyecto de API. Por ejemplo, [implemente en Azure](xref:host-and-deploy/azure-apps/index).
1. Ejecute la aplicación Razor Pages o MVC desde el escritorio y haga clic en el botón **probar** . Use las herramientas F12 para revisar los mensajes de error.
1. Quite el origen localhost de `WithOrigins` e implemente la aplicación. Como alternativa, ejecute la aplicación cliente con un puerto diferente. Por ejemplo, ejecute desde Visual Studio.
1. Pruebe con la aplicación cliente. Los errores de CORS devuelven un error, pero el mensaje de error no está disponible para JavaScript. Use la pestaña consola de las herramientas F12 para ver el error. Dependiendo del explorador, obtendrá un error (en la consola de herramientas de F12) similar al siguiente:

   * Uso de Microsoft Edge:

     **SEC7120: [CORS] el origen `https://localhost:44375` no se encontró `https://localhost:44375` en el encabezado de respuesta Access-Control-Allow-Origin para el recurso entre orígenes en`https://webapi.azurewebsites.net/api/values/1`**

   * Usar Chrome:

     **La Directiva CORS ha `https://webapi.azurewebsites.net/api/values/1` bloqueado el `https://localhost:44375` acceso a XMLHttpRequest desde el origen. No existe ningún encabezado "Access-Control-Allow-Origin" en el recurso solicitado.**

## <a name="additional-resources"></a>Recursos adicionales

* [Uso compartido de recursos entre orígenes (CORS)](https://developer.mozilla.org/docs/Web/HTTP/CORS)
