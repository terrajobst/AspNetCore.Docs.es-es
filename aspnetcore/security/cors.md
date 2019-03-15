---
title: Habilitar solicitudes entre orígenes (CORS) en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo la CORS como un estándar para permitir o rechazar las solicitudes entre orígenes en una aplicación ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 02/27/2019
uid: security/cors
ms.openlocfilehash: 6be8b4da1642a9eff021371c229a17071d6e9bfb
ms.sourcegitcommit: d913bca90373c07f89b1d1df01af5fc01fc908ef
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/14/2019
ms.locfileid: "57978476"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a>Habilitar solicitudes entre orígenes (CORS) en ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

En este artículo se muestra cómo se habilita la CORS en una aplicación ASP.NET Core.

Seguridad del explorador impide que una página web que realizan solicitudes a un dominio diferente a la que proviene la página web. Esta restricción se denomina el *directiva del mismo origen*. La directiva de mismo origen impide que un sitio malintencionado lea datos confidenciales de otro sitio. En ocasiones, es posible que desea permitir que otros sitios realizan solicitudes entre orígenes en la aplicación. Para obtener más información, consulte el [artículo Mozilla CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).

[Cross Origin Resource Sharing](https://www.w3.org/TR/cors/) (CORS):

* Es un W3C estándar que permite que un servidor a ser menos exigentes con la directiva de mismo origen.
* Es **no** una característica de seguridad, la CORS relaja la seguridad. Una API no es más segura, ya que permite la CORS. Para obtener más información, consulte [cómo la CORS funciona](#how-cors).
* Permite que un servidor permitir explícitamente algunas solicitudes entre orígenes y rechaza otras.
* Es más seguro y más flexible que las técnicas anteriores, como [JSONP](/dotnet/framework/wcf/samples/jsonp).

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/2.2-stage-samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))

## <a name="same-origin"></a>Mismo origen

Dos direcciones URL tienen el mismo origen si tienen esquemas idénticos, los hosts y los puertos ([6454 RFC](https://tools.ietf.org/html/rfc6454)).

Estas dos direcciones URL tienen el mismo origen:

* `https://example.com/foo.html`
* `https://example.com/bar.html`

Estas direcciones URL tienen orígenes diferentes que las dos direcciones URL anteriores:

* `https://example.net` &ndash; Dominio diferente
* `https://www.example.com/foo.html` &ndash; Subdominio distinto
* `http://example.com/foo.html` &ndash; Esquema diferente
* `https://example.com:9000/foo.html` &ndash; Otro puerto

Internet Explorer no tiene en cuenta el puerto al comparar orígenes.

## <a name="cors-with-named-policy-and-middleware"></a>CORS con middleware y directivas con nombre

Middleware de CORS controla las solicitudes entre orígenes. El código siguiente permite la CORS para toda la aplicación con el origen especificado:

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet&highlight=8,14-23,38)]

El código anterior:

* El nombre de la directiva se establece en "\_myAllowSpecificOrigins". El nombre de la directiva es arbitrario.
* Las llamadas del <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> método de extensión, lo que permite núcleos.
* Las llamadas <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> con un [expresión lambda](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions). La expresión lambda toma un objeto <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder>. [Las opciones de configuración](#cors-policy-options), tales como `WithOrigins`, se describen más adelante en este artículo.

El <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> llamada al método agrega los servicios de la CORS al contenedor de servicios de la aplicación:

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet2)]

Para obtener más información, consulte [las opciones de directiva CORS](#cpo) en este documento.

El <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> método puede encadenar métodos, como se muestra en el código siguiente:

[!code-csharp[](cors/sample/Cors/WebAPI/Startup2.cs?name=snippet2)]

El siguiente código resaltado aplica a las directivas de la CORS a todos los extremos de las aplicaciones a través de Middleware de CORS:

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

Consulte [habilitar CORS en las páginas de Razor, controladores y métodos de acción](#ecors) para aplicar la directiva CORS en el nivel de página o controlador/acción.

Nota:

* `UseCors` debe llamarse antes `UseMvc`.
* La dirección URL debe **no** contienen una barra diagonal final (`/`). Si la dirección URL termina con `/`, la comparación devuelve `false` y no se devuelve ningún encabezado.

Consulte [prueba CORS](#test) para obtener instrucciones sobre cómo probar el código anterior.

<a name="ecors"></a>

## <a name="enable-cors-with-attributes"></a>Habilitar la CORS con atributos

El [ &lbrack;EnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) atributo proporciona una alternativa a la aplicación la CORS globalmente. El `[EnableCors]` atributo habilita la CORS para los puntos de conexión seleccionados, en lugar de todos los puntos de conexión.

Use `[EnableCors]` para especificar la directiva predeterminada y `[EnableCors("{Policy String}")]` para especificar una directiva.

El `[EnableCors]` atributo puede aplicarse a:

* Página de Razor `PageModel`
* Controlador
* Método de acción de controlador

Puede aplicar diferentes directivas a/página-modelo o acción de controlador con el `[EnableCors]` atributo. Cuando el `[EnableCors]` atributo se aplica a un método de acción o controladores/página-modelo y se ha habilitado CORS en middleware, se aplicarán ambas directivas. No se recomienda combinar las directivas. Use el `[EnableCors]` atributo o un software intermedio, no a ambos en la misma aplicación.

El código siguiente aplica una directiva diferente a cada método:

[!code-csharp[](cors/sample/Cors/WebAPI/Controllers/WidgetController.cs?name=snippet&highlight=6,14)]

El código siguiente crea una directiva predeterminada CORS y una directiva denominada `"AnotherPolicy"`:

[!code-csharp[](cors/sample/Cors/WebAPI/StartupMultiPolicy.cs?name=snippet&highlight=12-28)]

### <a name="disable-cors"></a>Deshabilitar la CORS

El [ &lbrack;DisableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) atributo deshabilita la CORS para la acción o controlador/página-modelo.

<a name="cpo"></a>

## <a name="cors-policy-options"></a>Opciones de directiva CORS

En esta sección se describe las distintas opciones que se pueden establecer en una directiva CORS:

* [Establecer los orígenes permitidos](#set-the-allowed-origins)
* [Establecer los métodos HTTP permitidos](#set-the-allowed-http-methods)
* [Establecer los encabezados de solicitudes permitidos](#set-the-allowed-request-headers)
* [Establecer los encabezados de respuesta expuestos](#set-the-exposed-response-headers)
* [Credenciales en solicitudes entre orígenes](#credentials-in-cross-origin-requests)
* [Establecer el tiempo de expiración de las comprobaciones preparatorias](#set-the-preflight-expiration-time)

 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> se llama en `Startup.ConfigureServices`. Para algunas opciones, puede resultar útil leer la [cómo la CORS funciona](#how-cors) sección en primer lugar.

## <a name="set-the-allowed-origins"></a>Establecer los orígenes permitidos

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; Permite que las solicitudes CORS de todos los orígenes con cualquier esquema (`http` o `https`). `AllowAnyOrigin` no es segura porque *cualquier sitio Web* puede realizar solicitudes entre orígenes en la aplicación.

  ::: moniker range=">= aspnetcore-2.2"

  > [!NOTE]
  > Especificar `AllowAnyOrigin` y `AllowCredentials` es una configuración no segura y puede dar lugar a la falsificación de solicitud entre sitios. El servicio de la CORS devuelve una respuesta no válida de la CORS cuando una aplicación está configurada con ambos métodos.

  ::: moniker-end

  ::: moniker range="< aspnetcore-2.2"

  > [!NOTE]
  > Especificar `AllowAnyOrigin` y `AllowCredentials` es una configuración no segura y puede dar lugar a la falsificación de solicitud entre sitios. Para una aplicación segura, especifique una lista exacta de los orígenes, si el cliente debe autorizar a sí mismo para tener acceso a los recursos del servidor.

  ::: moniker-end

<!-- REVIEW required
I changed from
Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration. **This** setting affects preflight requests and the ...
to
**`AllowAnyOrigin`** affects preflight requests and the

to remove the ambiguous **This**. 
-->

  `AllowAnyOrigin` afecta a las solicitudes de comprobaciones y `Access-Control-Allow-Origin` encabezado. Para obtener más información, consulte el [las solicitudes de comprobaciones](#preflight-requests) sección.

::: moniker range=">= aspnetcore-2.0"

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; Establece el <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> propiedad de la directiva a una función que permite orígenes para que coincida con un dominio con comodín configurado al evaluar si se permite el origen.

  [!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=100-104&highlight=4)]

::: moniker-end

### <a name="set-the-allowed-http-methods"></a>Establecer los métodos HTTP permitidos

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:

* Permite que cualquier método HTTP:
* Afecta a las solicitudes de comprobaciones y `Access-Control-Allow-Methods` encabezado. Para obtener más información, consulte el [las solicitudes de comprobaciones](#preflight-requests) sección.

### <a name="set-the-allowed-request-headers"></a>Establecer los encabezados de solicitudes permitidos

Para permitir que los encabezados específicos que se enviarán en una solicitud de CORS, llamado *crear encabezados de solicitud*, llame a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> y especifique los encabezados permitidos:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

Para permitir que todos creación encabezados de solicitud, llame a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

Esta configuración afecta a las solicitudes de preflight y `Access-Control-Request-Headers` encabezado. Para obtener más información, consulte el [las solicitudes de comprobaciones](#preflight-requests) sección.

::: moniker range=">= aspnetcore-2.2"

Una coincidencia de directiva de Middleware de CORS a encabezados específicos especificado por `WithHeaders` solo es posible cuando se envían los encabezados `Access-Control-Request-Headers` coincidir exactamente con los encabezados que se indica en `WithHeaders`.

Por ejemplo, considere una aplicación configurada como sigue:

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

Middleware de CORS rechaza una solicitud preparatoria con el siguiente encabezado de solicitud porque `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) no aparece en `WithHeaders`:

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

Devuelve la aplicación un *200 Aceptar* respuesta pero no envía de vuelta los encabezados CORS. Por lo tanto, el explorador no intenta la solicitud entre orígenes.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Middleware de CORS siempre permite cuatro encabezados en el `Access-Control-Request-Headers` para enviarse independientemente de los valores configurados en CorsPolicy.Headers. Esta lista de encabezados incluye:

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Origin`

Por ejemplo, considere una aplicación configurada como sigue:

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

Middleware de CORS responde correctamente a una solicitud preparatoria con el siguiente encabezado de solicitud porque `Content-Language` siempre es la lista de permitidos:

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

::: moniker-end

### <a name="set-the-exposed-response-headers"></a>Establecer los encabezados de respuesta expuestos

De forma predeterminada, el explorador no expone todos los encabezados de respuesta a la aplicación. Para obtener más información, consulte [W3C Cross-Origin Resource Sharing (la terminología): Encabezado de respuesta simple](https://www.w3.org/TR/cors/#simple-response-header).

Los encabezados de respuesta que están disponibles de forma predeterminada son:

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

La especificación de CORS llama a estos encabezados *encabezados de respuesta simple*. Para que otros encabezados disponibles para la aplicación, llame a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=73-78&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a>Credenciales en solicitudes entre orígenes

Las credenciales requieren un tratamiento especial en una solicitud de CORS. De forma predeterminada, el explorador no envía las credenciales con una solicitud entre orígenes. Las credenciales incluyen las cookies y los esquemas de autenticación HTTP. Para enviar las credenciales con una solicitud entre orígenes, el cliente debe establecer `XMLHttpRequest.withCredentials` a `true`.

Uso de `XMLHttpRequest` directamente:

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

Uso de jQuery:

```javascript
$.ajax({
  type: 'get',
  url: 'https://www.example.com/api/test',
  xhrFields: {
    withCredentials: true
  }
});
```

Mediante el [capturar API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):

```javascript
fetch('https://www.example.com/api/test', {
    credentials: 'include'
});
```

El servidor debe permitir las credenciales. Para permitir que las credenciales de origen cruzado, llame a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=82-87&highlight=5)]

La respuesta HTTP incluye un `Access-Control-Allow-Credentials` encabezado, que indica al explorador que el servidor permite credenciales para una solicitud entre orígenes.

Si el explorador envía credenciales, pero la respuesta no incluye válido `Access-Control-Allow-Credentials` encabezado, el explorador no expone la respuesta a la aplicación y se produce un error en la solicitud entre orígenes.

Permitir las credenciales de origen cruzado es un riesgo de seguridad. Un sitio Web en otro dominio puede enviar las credenciales de un usuario con sesión iniciada de la aplicación en el nombre de usuario sin el conocimiento del usuario. <!-- TODO Review: When using `AllowCredentials`, all CORS enabled domains must be trusted.
I don't like "all CORS enabled domains must be trusted", because it implies that if you're not using  `AllowCredentials`, domains don't need to be trusted. -->

La especificación de CORS también indica ese valor orígenes a `"*"` (todos los orígenes) no es válido si el `Access-Control-Allow-Credentials` encabezado está presente.

### <a name="preflight-requests"></a>Solicitudes preparatorias

Para algunas solicitudes CORS, el explorador envía una solicitud adicional antes de realizar la solicitud real. Se llama a esta solicitud una *solicitud preparatoria*. El explorador puede omitir la solicitud preparatoria si se cumplen las condiciones siguientes:

* El método de solicitud es GET, HEAD o POST.
* La aplicación, no establezca los encabezados de solicitud distinto `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, o `Last-Event-ID`.
* El `Content-Type` encabezado, si establece, tiene uno de los siguientes valores:
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

La regla en los encabezados de solicitud establecido para la solicitud de cliente se aplica a los encabezados de la aplicación se establece mediante una llamada a `setRequestHeader` en el `XMLHttpRequest` objeto. La especificación de CORS llama a estos encabezados *crear encabezados de solicitud*. La regla no se aplica a los encabezados que se puede establecer el explorador, tales como `User-Agent`, `Host`, o `Content-Length`.

Este es un ejemplo de una solicitud de preflight:

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

La solicitud preparatoria usa el método HTTP OPTIONS. Incluye dos encabezados especiales:

* `Access-Control-Request-Method`: El método HTTP que se usará para la solicitud real.
* `Access-Control-Request-Headers`: Una lista de encabezados de solicitud que la aplicación se establece en la solicitud real. Como se indicó anteriormente, esto no incluye los encabezados que establece el explorador, como `User-Agent`.

Una solicitud preparatoria de CORS puede incluir un `Access-Control-Request-Headers` encabezado, que indica al servidor los encabezados que se envían con la solicitud real.

Para permitir encabezados específicos, llamar a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

Para permitir que todos creación encabezados de solicitud, llame a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

Los exploradores no están totalmente coherentes en cómo establezca `Access-Control-Request-Headers`. Si establece los encabezados en algo distinto `"*"` (o use <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), debe incluir al menos `Accept`, `Content-Type`, y `Origin`, además de los encabezados personalizados que desee admitir.

Este es un ejemplo de respuesta a la solicitud preparatoria (suponiendo que el servidor permite la solicitud):

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

La respuesta incluye un `Access-Control-Allow-Methods` encabezado que se enumera los métodos permitidos y, opcionalmente, un `Access-Control-Allow-Headers` encabezado, que se enumera los encabezados permitidos. Si la solicitud preparatoria se realiza correctamente, el explorador envía la solicitud real.

Si se deniega la solicitud preparatoria, la aplicación devuelve un *200 Aceptar* respuesta pero no envía de vuelta los encabezados CORS. Por lo tanto, el explorador no intenta la solicitud entre orígenes.

### <a name="set-the-preflight-expiration-time"></a>Establecer el tiempo de expiración de las comprobaciones preparatorias

El `Access-Control-Max-Age` encabezado especifica cuánto tiempo puede almacenar en caché la respuesta a la solicitud preparatoria. Para establecer este encabezado, llame a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=91-96&highlight=5)]

<a name="how-cors"></a>

## <a name="how-cors-works"></a>Cómo funciona la CORS

Esta sección describe lo que ocurre en un [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) solicitud en el nivel de los mensajes HTTP.

* CORS es **no** una característica de seguridad. CORS es un estándar de W3C que permite que un servidor a ser menos exigentes con la directiva de mismo origen.
  * Por ejemplo, podría utilizar un individuo malintencionado [evitar Cross-Site Scripting (XSS)](xref:security/cross-site-scripting) en su sitio y ejecutar una solicitud entre sitios a su sitio habilitado para CORS para robar información.
* La API no es más segura, ya que permite la CORS.
  * Depende del cliente (explorador) exigir la CORS. El servidor ejecuta la solicitud y devuelve la respuesta, es el cliente que devuelve un error y bloques de la respuesta. Por ejemplo, cualquiera de las siguientes herramientas mostrará la respuesta del servidor:
     * [Fiddler](https://www.telerik.com/fiddler)
     * [Postman](https://www.getpostman.com/)
     * [.NET HttpClient](/dotnet/csharp/tutorials/console-webapiclient)
     * Un explorador web escribiendo la dirección URL en la barra de direcciones.
* Es una manera de ejecutar un origen cruzado para un servidor permitir que los exploradores [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) o [API capturar](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) solicitud que en caso contrario, podría estar prohibido.
  * Los exploradores (sin la CORS) no pueden realizar solicitudes entre orígenes. Antes de la CORS, [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) se usó para eludir esta restricción. JSONP no usa XHR, usa el `<script>` etiqueta para recibir la respuesta. Las secuencias de comandos pueden estar cargado entre orígenes.

El [especificación CORS]() introdujo varios encabezados HTTP nuevo que permiten a las solicitudes entre orígenes. Si un explorador es compatible con CORS, establece estos encabezados automáticamente para las solicitudes entre orígenes. No se necesita código JavaScript personalizado para habilitar CORS.

El siguiente es un ejemplo de una solicitud entre orígenes. El encabezado `Origin` proporciona el dominio del sitio que está realizando la solicitud:

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

Si el servidor permite la solicitud, Establece la `Access-Control-Allow-Origin` encabezado en la respuesta. El valor de este encabezado ya sea con la `Origin` encabezado de la solicitud o es el valor de carácter comodín `"*"`, lo que significa que se permite cualquier origen:

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

Si la respuesta no incluye el `Access-Control-Allow-Origin` encabezado, la solicitud de origen cruzado se produce un error. En concreto, el explorador no permite la solicitud. Incluso si el servidor devuelve una respuesta correcta, el explorador no disponer de la respuesta a la aplicación cliente.

<a name="test"></a>

## <a name="test-cors"></a>Prueba de CORS

Para probar la CORS:

1. [Crear un proyecto de API](xref:tutorials/first-web-api). Como alternativa, puede [descargar el ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cors/sample/Cors).
1. Habilitar la CORS mediante uno de los enfoques de este documento. Por ejemplo:

  [!code-csharp[](cors/sample/Cors/WebAPI/StartupTest.cs?name=snippet2&highlight=13-18)]
  
  > [!WARNING]
  > `WithOrigins("https://localhost:<port>");` solo debe usarse para probar una aplicación de ejemplo similar a la [descargar código de ejemplo](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/cors/sample/Cors).

1. Cree un proyecto de aplicación web (páginas de Razor o MVC). El ejemplo utiliza las páginas de Razor. Puede crear la aplicación web en la misma solución que el proyecto de API.
1. Agregue el código resaltado siguiente a la *Index.cshtml* archivo:

  [!code-csharp[](cors/sample/Cors/ClientApp/Pages/Index2.cshtml?highlight=7-99)]

1. En el código anterior, reemplace `url: 'https://<web app>.azurewebsites.net/api/values/1',` con la dirección URL a la aplicación implementada.
1. Implementar el proyecto de API. Por ejemplo, [implementar en Azure](xref:host-and-deploy/azure-apps/index).
1. Ejecute la aplicación de las páginas de Razor o MVC desde el escritorio y haga clic en el **prueba** botón. Use las herramientas de F12 para revisar los mensajes de error.
1. Quitar el origen de localhost de `WithOrigins` e implementar la aplicación. Como alternativa, puede ejecutar la aplicación cliente con un puerto diferente. Por ejemplo, ejecutar desde Visual Studio.
1. Prueba con la aplicación cliente. Errores de la CORS devuelven un error, pero el mensaje de error no está disponible en JavaScript. Utilice la pestaña de la consola en las herramientas de F12 para ver el error. Dependiendo del explorador, obtendrá un error (en la consola de herramientas de F12) similar al siguiente:

  * Con Microsoft Edge:

    **SEC7120: [CORS] origen `https://localhost:44375` no se encontró `https://localhost:44375` en el encabezado de respuesta de Access-Control-Allow-Origin para recursos entre orígenes en `https://webapi.azurewebsites.net/api/values/1`**

  * Uso de Chrome:

    **Acceso a XMLHttpRequest en `https://webapi.azurewebsites.net/api/values/1` desde origen `https://localhost:44375` ha sido bloqueada por la directiva CORS: Ningún encabezado "Access-Control-Allow-Origin" está presente en el recurso solicitado.**

## <a name="additional-resources"></a>Recursos adicionales

* [Recursos entre orígenes (CORS) de uso compartido](https://developer.mozilla.org/docs/Web/HTTP/CORS)
