---
title: Habilitar solicitudes entre orígenes (CORS) en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo la CORS como un estándar para permitir o rechazar las solicitudes entre orígenes en una aplicación ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 11/05/2018
uid: security/cors
ms.openlocfilehash: 8e5056b448d47d75272e9394b03ce8a58b05a0f4
ms.sourcegitcommit: 09affee3d234cb27ea6fe33bc113b79e68900d22
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2018
ms.locfileid: "51191326"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a>Habilitar solicitudes entre orígenes (CORS) en ASP.NET Core

Por [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), y [Tom Dykstra](https://github.com/tdykstra)

Seguridad del explorador impide que una página web que realizan solicitudes a un dominio diferente a la que proviene la página web. Esta restricción se denomina el *directiva del mismo origen*. La directiva de mismo origen impide que un sitio malintencionado lea datos confidenciales de otro sitio. En ocasiones, es posible que desea permitir que otros sitios realizan solicitudes entre orígenes en la aplicación.

El [uso compartido de recursos entre orígenes](https://www.w3.org/TR/cors/) (CORS) es un estándar del W3C que permite que un servidor modere la directiva de mismo origen. Mediante CORS, un servidor puede permitir explícitamente algunas solicitudes entre orígenes y rechazar otras. CORS es más seguro y más flexible que las técnicas anteriores, como [JSONP](https://wikipedia.org/wiki/JSONP). En este tema se muestra cómo se habilita la CORS en una aplicación ASP.NET Core.

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

> [!NOTE]
> Internet Explorer no tiene en cuenta el puerto al comparar orígenes.

## <a name="register-cors-services"></a>Registrar servicios CORS

::: moniker range=">= aspnetcore-2.1"

Referencia de la [Microsoft.AspNetCore.App metapaquete](xref:fundamentals/metapackage-app) o agregar una referencia de paquete para el [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) paquete.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Referencia de la [metapaquete Microsoft.AspNetCore.All](xref:fundamentals/metapackage) o agregar una referencia de paquete para el [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) paquete.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Agregue una referencia de paquete a la [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) paquete.

::: moniker-end

Llame a <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> en `Startup.ConfigureServices` para agregar servicios de la CORS al contenedor de servicios de la aplicación:

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors&highlight=3)]

## <a name="enable-cors"></a>Habilitar la CORS

Después de registrar servicios CORS, use cualquiera de los métodos siguientes para habilitar la CORS en una aplicación ASP.NET Core:

* [Middleware de CORS](#enable-cors-with-cors-middleware) &ndash; directivas CORS se aplican globalmente a la aplicación a través de middleware.
* [La CORS en MVC](#enable-cors-in-mvc) &ndash; las directivas de aplicar la CORS por cada acción o por cada controlador. No se usa el Middleware de CORS.

### <a name="enable-cors-with-cors-middleware"></a>Habilitar la CORS con Middleware de CORS

Middleware de CORS controla las solicitudes de origen cruzado a la aplicación. Para habilitar el Middleware de CORS en la canalización de procesamiento de la solicitud, llame a la <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> método de extensión en `Startup.Configure`.

Middleware de CORS debe preceder a cualquier punto de conexión definido en la aplicación que desea admitir las solicitudes entre orígenes (por ejemplo, antes de llamar a `UseMvc` de Middleware de páginas de Razor y MVC).

Un *directiva de origen cruzado* puede especificarse cuando se agrega el Middleware de CORS mediante el <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> clase. Existen dos enfoques para definir una directiva CORS:

* Llamar a `UseCors` con una expresión lambda:

  [!code-csharp[](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]

  La expresión lambda toma un objeto <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder>. [Las opciones de configuración](#cors-policy-options), tales como `WithOrigins`, se describen más adelante en este tema. En el ejemplo anterior, la directiva permite que las solicitudes entre orígenes de `https://example.com` y no hay otros orígenes.

  Debe especificarse la dirección URL sin una barra diagonal final (`/`). Si la dirección URL termina con `/`, la comparación devuelve `false` y no se devuelve ningún encabezado.

  `CorsPolicyBuilder` tiene una API fluida, por lo que se pueden encadenar llamadas de métodos:

  [!code-csharp[](cors/sample/CorsExample3/Startup.cs?highlight=2-3&range=29-32)]

* Definir una o varias directivas CORS con nombre y seleccione la directiva por su nombre en tiempo de ejecución. En el ejemplo siguiente se agrega una directiva CORS definido por el usuario denominada *AllowSpecificOrigin*. Para seleccionar la directiva, pase el nombre a `UseCors`:

  [!code-csharp[](cors/sample/CorsExample2/Startup.cs?name=snippet_begin&highlight=5-6,21)]

### <a name="enable-cors-in-mvc"></a>Habilitar la CORS en MVC

Como alternativa, puede usar MVC para aplicar directivas específicas de CORS por cada acción o por cada controlador. Al usar MVC para habilitar la CORS, se usan los servicios registrados de la CORS. No se usa el Middleware de CORS.

### <a name="per-action"></a>Por acción

Para especificar una directiva CORS para una acción específica, agregue el [ &lbrack;EnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) atributo a la acción. Especifique el nombre de la directiva.

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction&highlight=2)]

### <a name="per-controller"></a>Por cada controlador

Para especificar la directiva CORS de un controlador concreto, agregue el [ &lbrack;EnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) a la clase de controlador. Especifique el nombre de la directiva.

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController&highlight=2)]

Es el orden de prioridad:

1. predeterminada
1. controlador

### <a name="disable-cors"></a>Deshabilitar la CORS

Para deshabilitar CORS para una acción o controlador, utilice la [ &lbrack;DisableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) atributo:

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction&highlight=2)]

## <a name="cors-policy-options"></a>Opciones de directiva CORS

Esta sección describen las distintas opciones que puede establecer en una directiva CORS.

* [Establecer los orígenes permitidos](#set-the-allowed-origins)
* [Establecer los métodos HTTP permitidos](#set-the-allowed-http-methods)
* [Establecer los encabezados de solicitudes permitidos](#set-the-allowed-request-headers)
* [Establecer los encabezados de respuesta expuestos](#set-the-exposed-response-headers)
* [Credenciales en solicitudes entre orígenes](#credentials-in-cross-origin-requests)
* [Establecer el tiempo de expiración de las comprobaciones preparatorias](#set-the-preflight-expiration-time)

Para algunas opciones, puede resultar útil leer la [cómo la CORS funciona](#how-cors-works) sección en primer lugar.

### <a name="set-the-allowed-origins"></a>Establecer los orígenes permitidos

El middleware de CORS en ASP.NET Core MVC tiene varias maneras de especificar los orígenes permitidos:

* <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithOrigins*>: Permite especificar una o más direcciones URL. La dirección URL puede incluir el esquema, el nombre de host y el puerto sin información de ruta de acceso. Por ejemplo: `https://example.com`. Debe especificarse la dirección URL sin una barra diagonal final (`/`).

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=20-24&highlight=4)]

* <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*>: Permite que las solicitudes CORS de todos los orígenes con cualquier esquema (`http` o `https`).

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=28-32&highlight=4)]

Considere detenidamente antes de permitir las solicitudes desde cualquier origen. Permitir las solicitudes desde cualquier origen significa que *cualquier sitio Web* puede realizar solicitudes entre orígenes en la aplicación.

::: moniker range=">= aspnetcore-2.2"

> [!NOTE]
> Especificar `AllowAnyOrigin` y `AllowCredentials` es una configuración no segura y puede dar lugar a la falsificación de solicitud entre sitios. El servicio de la CORS devuelve una respuesta no válida de la CORS cuando una aplicación está configurada con los dos.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

> [!NOTE]
> Especificar `AllowAnyOrigin` y `AllowCredentials` es una configuración no segura y puede dar lugar a la falsificación de solicitud entre sitios. Considere la posibilidad de especificar una lista exacta de los orígenes, si el cliente necesita para autorizar el acceso a los recursos de servidor.

::: moniker-end

Esta configuración afecta a [comprobaciones de las solicitudes y el encabezado de Access-Control-Allow-Origin](#preflight-requests) (descrita más adelante en este tema).

* <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> -Permite que las solicitudes CORS desde cualquier subdominio de un dominio determinado. El esquema no puede ser un carácter comodín.

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=98-104&highlight=4)]

### <a name="set-the-allowed-http-methods"></a>Establecer los métodos HTTP permitidos

Para permitir que todos los métodos HTTP, llame a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=45-50&highlight=5)]

Esta configuración afecta a [comprobaciones de las solicitudes y el encabezado de Access-Control-Allow-Methods](#preflight-requests) (descrita más adelante en este tema).

### <a name="set-the-allowed-request-headers"></a>Establecer los encabezados de solicitudes permitidos

Para permitir que los encabezados específicos que se enviarán en una solicitud de CORS, llamado *crear encabezados de solicitud*, llame a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> y especifique los encabezados permitidos:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=54-59&highlight=5)]

Para permitir que todos creación encabezados de solicitud, llame a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=63-68&highlight=5)]

Esta configuración afecta a [comprobaciones de las solicitudes y el encabezado de Access-Control-Request-Headers](#preflight-requests) (descrita más adelante en este tema).

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

De forma predeterminada, el explorador no expone todos los encabezados de respuesta a la aplicación. Para obtener más información, consulte [W3C Cross-Origin Resource Sharing (la terminología): encabezado de respuesta Simple](https://www.w3.org/TR/cors/#simple-response-header).

Los encabezados de respuesta que están disponibles de forma predeterminada son:

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

La especificación de CORS llama a estos encabezados *encabezados de respuesta simple*. Para que otros encabezados disponibles para la aplicación, llame a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=72-77&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a>Credenciales en solicitudes entre orígenes

Las credenciales requieren un tratamiento especial en una solicitud de CORS. De forma predeterminada, el explorador no envía las credenciales con una solicitud entre orígenes. Las credenciales incluyen las cookies y los esquemas de autenticación HTTP. Para enviar las credenciales con una solicitud entre orígenes, el cliente debe establecer `XMLHttpRequest.withCredentials` a `true`.

Uso de `XMLHttpRequest` directamente:

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

En jQuery:

```jQuery
$.ajax({
  type: 'get',
  url: 'https://www.example.com/home',
  xhrFields: {
    withCredentials: true
}
```

Además, el servidor debe permitir las credenciales. Para permitir que las credenciales de origen cruzado, llame a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=81-86&highlight=5)]

La respuesta HTTP incluye un `Access-Control-Allow-Credentials` encabezado, que indica al explorador que el servidor permite credenciales para una solicitud entre orígenes.

Si el explorador envía credenciales, pero la respuesta no incluye válido `Access-Control-Allow-Credentials` encabezado, el explorador no expone la respuesta a la aplicación y se produce un error en la solicitud entre orígenes.

Tenga cuidado al permitir credenciales entre orígenes. Un sitio Web en otro dominio puede enviar las credenciales de un usuario con sesión iniciada de la aplicación en el nombre de usuario sin el conocimiento del usuario.

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

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=54-59&highlight=5)]

Para permitir que todos creación encabezados de solicitud, llame a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=63-68&highlight=5)]

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

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=90-95&highlight=5)]

## <a name="how-cors-works"></a>Cómo funciona la CORS

Esta sección describe lo que ocurre en una solicitud de CORS en el nivel de los mensajes HTTP. Es importante entender cómo funciona la CORS para que la directiva CORS puede ser configurada correctamente y depurando cuando se producen comportamientos inesperados.

La especificación de CORS presenta varios encabezados HTTP nuevos que permiten las solicitudes entre orígenes. Si un explorador es compatible con CORS, establece estos encabezados automáticamente para las solicitudes entre orígenes. No se necesita código JavaScript personalizado para habilitar CORS.

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

## <a name="additional-resources"></a>Recursos adicionales

* [Recursos entre orígenes (CORS) de uso compartido](https://developer.mozilla.org/docs/Web/HTTP/CORS)
