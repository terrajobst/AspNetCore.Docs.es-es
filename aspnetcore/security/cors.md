---
title: Permitir solicitudes entre orígenes (CORS) en ASP.NET Core
author: rick-anderson
description: Obtenga información acerca de cómo CORS como un estándar para permitir o rechazar las solicitudes entre orígenes en una aplicación de ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 05/17/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/cors
ms.openlocfilehash: 3c5d0840426c7ed52353a7832a1a1959027121de
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/22/2018
ms.locfileid: "30077553"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a>Permitir solicitudes entre orígenes (CORS) en ASP.NET Core

Por [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), y [Tom Dykstra](https://github.com/tdykstra)

La seguridad del explorador impide que una página web realice solicitudes AJAX a otro dominio. Esta restricción se denomina *"directiva de mismo origen"* e impide que un sitio malintencionado lea los datos confidenciales desde otro sitio. En cambio, a veces, es posible que quiera permitir que otros sitios realicen solicitudes entre orígenes en su API web.

El [uso compartido de recursos entre orígenes](http://www.w3.org/TR/cors/) (CORS) es un estándar del W3C que permite que un servidor modere la directiva de mismo origen. Mediante CORS, un servidor puede permitir explícitamente algunas solicitudes entre orígenes y rechazar otras. CORS es más seguro y flexible que técnicas anteriores como [JSONP](https://wikipedia.org/wiki/JSONP). En este tema, se muestra cómo habilitar CORS en una aplicación de ASP.NET Core.

## <a name="what-is-same-origin"></a>¿Qué es el "mismo origen"?

Dos direcciones URL tienen el mismo origen si tienen puertos, hosts y esquemas idénticos. ([RFC 6454](http://tools.ietf.org/html/rfc6454))

Estas dos direcciones URL tienen el mismo origen:

* `http://example.com/foo.html`

* `http://example.com/bar.html`

Estas direcciones URL tienen orígenes diferentes de los dos anteriores:

* `http://example.net`: dominio diferente

* `http://www.example.com/foo.html`: subdominio diferente

* `https://example.com/foo.html`: esquema diferente

* `http://example.com:9000/foo.html`: puerto diferente

> [!NOTE]
> Internet Explorer no tiene en cuenta el puerto al comparar orígenes.

## <a name="setting-up-cors"></a>Configuración de CORS

Para configurar CORS en su aplicación, agregue el paquete `Microsoft.AspNetCore.Cors` al proyecto.

Agregue los servicios CORS en Startup.cs:

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors)]

## <a name="enabling-cors-with-middleware"></a>Habilitación de CORS con middleware

Para habilitar CORS para toda la aplicación, agregue el middleware de CORS a la canalización de solicitud mediante el método de extensión `UseCors`. Tenga en cuenta que el middleware de CORS debe preceder a cualquier punto de conexión definido en la aplicación que quiera que admita las solicitudes entre orígenes (por ejemplo, antes de cualquier llamada a `UseMvc`).

Puede especificar una directiva entre orígenes al agregar el middleware de CORS mediante la clase `CorsPolicyBuilder`. Hay dos formas de hacerlo. La primera consiste en llamar a UseCors con una expresión lambda:

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]

**Nota:** Debe especificarse la dirección URL sin una barra diagonal final (`/`). Si la dirección URL finaliza con `/`, la comparación devolverá `false` y no se devolverá ningún encabezado.

La expresión lambda toma un objeto `CorsPolicyBuilder`. Encontrará una lista de las [opciones de configuración](#cors-policy-options) más adelante en este tema. En este ejemplo, la directiva permite las solicitudes entre orígenes de `http://example.com` y no de otros orígenes.

Tenga en cuenta que CorsPolicyBuilder tiene una API fluida, por lo que se pueden encadenar llamadas de método:

[!code-csharp[](../security/cors/sample/CorsExample3/Startup.cs?highlight=3&range=29-32)]

El segundo enfoque es definir una o varias directivas CORS con nombre y, después, seleccionar la directiva por su nombre en tiempo de ejecución.

[!code-csharp[](cors/sample/CorsExample2/Startup.cs?name=snippet_begin)]

Este ejemplo agrega una directiva CORS denominada "AllowSpecificOrigin". Para seleccionar la directiva, pase el nombre a `UseCors`.

## <a name="enabling-cors-in-mvc"></a>Habilitación de CORS en MVC

Como alternativa, puede usar MVC para aplicar CORS específico por cada acción, por cada controlador o globalmente para todos los controladores. Al utilizar MVC para habilitar CORS se utilizan los mismos servicios CORS, pero no el middleware CORS.

### <a name="per-action"></a>Por cada acción

Para especificar una directiva CORS para una acción específica, agregue el atributo `[EnableCors]`a la acción. Especifique el nombre de la directiva.

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction)]

### <a name="per-controller"></a>Por cada controlador

Para especificar la directiva CORS para un controlador específico, agregue el atributo `[EnableCors]` a la clase controller. Especifique el nombre de la directiva.

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController)]

### <a name="globally"></a>Global

Puede habilitar CORS globalmente para todos los controladores si agrega el filtro `CorsAuthorizationFilterFactory`a la colección de filtros globales:

[!code-csharp[](cors/sample/CorsMVC/Startup2.cs?name=snippet_configureservices)]

El orden de prioridad es: acción, controlador, global. Las directivas de nivel de acción tienen prioridad sobre las directivas de nivel de controlador y estas últimas tienen prioridad sobre las directivas globales.

### <a name="disable-cors"></a>Deshabilitar CORS

Para deshabilitar CORS para una acción o un controlador, use el atributo `[DisableCors]`.

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction)]

## <a name="cors-policy-options"></a>Opciones de directiva CORS

Esta sección describe las distintas opciones que se pueden establecer en una directiva CORS.

* [Establecer los orígenes permitidos](#set-the-allowed-origins)

* [Establecer los métodos HTTP permitidos](#set-the-allowed-http-methods)

* [Establecer los encabezados de solicitudes permitidos](#set-the-allowed-request-headers)

* [Establecer los encabezados de respuesta expuestos](#set-the-exposed-response-headers)

* [Credenciales en solicitudes cross-origin](#credentials-in-cross-origin-requests)

* [Establecer el tiempo de expiración de las comprobaciones preparatorias](#set-the-preflight-expiration-time)

Para algunas opciones pueden serle de ayuda leer [CORS cómo funciona](#how-cors-works) primero.

### <a name="set-the-allowed-origins"></a>Establecer los orígenes permitidos

Para permitir uno o más orígenes específicos:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=19-23)]

Para permitir todos los orígenes:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs??range=27-31)]

Debe considerar detenidamente antes de permitir que las solicitudes de cualquier origen. Significa que literalmente cualquier sitio Web puede realizar las llamadas de AJAX a su API.

### <a name="set-the-allowed-http-methods"></a>Establecer los métodos HTTP permitidos

Para permitir todos los métodos HTTP:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=44-49)]

Esto afecta a las solicitudes de comprobaciones preparatorias y al encabezado Access-Control-Allow-Methods.

### <a name="set-the-allowed-request-headers"></a>Establecer los encabezados de solicitudes permitidos

Una solicitud de comprobaciones preparatorias de CORS puede incluir un encabezado Access-Control-Request-Headers, en el que aparezcan los encabezados HTTP que ha establecido la aplicación (los denominados "encabezados de solicitud de autor").

Para agregar encabezados específicos a la lista de permitidos:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=53-58)]

Para permitir todos los encabezados de solicitud de autor:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=62-67)]

Los exploradores no son completamente coherentes en la forma en que establecen Access-Control-Request-Headers. Si establece encabezados en un valor que no sea "*", debe incluir al menos "accept", "content-type" y "origin", además de los encabezados personalizados que quiera admitir.

### <a name="set-the-exposed-response-headers"></a>Establecer los encabezados de respuesta expuestos

De forma predeterminada, el explorador no expone todos los encabezados de respuesta a la aplicación. (Consulte [ http://www.w3.org/TR/cors/#simple-response-header ](http://www.w3.org/TR/cors/#simple-response-header).) Los encabezados de respuesta que están disponibles de forma predeterminada son:

* Cache-Control

* Content-Language

* Content-Type

* Expires

* Last-Modified

* Pragma

La especificación CORS llama a estos *encabezados de respuesta simple*. Para que otros encabezados estén disponibles para la aplicación:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=71-76)]

### <a name="credentials-in-cross-origin-requests"></a>Credenciales en solicitudes cross-origin

Las credenciales requieren un tratamiento especial en una solicitud de CORS. De forma predeterminada, el explorador no envía las credenciales con una solicitud entre orígenes. Las credenciales son las cookies, así como esquemas de autenticación HTTP. Para enviar las credenciales con una solicitud entre orígenes, el cliente debe establecer XMLHttpRequest.withCredentials en true.

Al usar directamente el objeto XMLHttpRequest:

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'http://www.example.com/api/test');
xhr.withCredentials = true;
```

En jQuery:

```jQuery
$.ajax({
  type: 'get',
  url: 'http://www.example.com/home',
  xhrFields: {
    withCredentials: true
}
```

Además, el servidor debe permitir las credenciales. Para permitir las credenciales entre orígenes:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=80-85)]

Ahora, la respuesta HTTP incluirá un encabezado Access-Control-Allow-Credentials, que indica al explorador que el servidor permite credenciales para una solicitud entre orígenes.

Si el explorador envía las credenciales, pero la respuesta no incluye un encabezado Access-Control-Allow-Credentials válido, el explorador no expone la respuesta a la aplicación y se produce un error en la solicitud de AJAX.

Tenga cuidado al permitir credenciales entre orígenes. Un sitio web en otro dominio puede enviar las credenciales de un usuario que ha iniciado sesión a la aplicación en nombre del usuario sin su conocimiento. La especificación de CORS también indica que configurar los orígenes en "*" (todos los orígenes) no es válido si está presente el encabezado `Access-Control-Allow-Credentials`.

### <a name="set-the-preflight-expiration-time"></a>Establecer el tiempo de expiración de las comprobaciones preparatorias

El encabezado Access-Control-Max-Age especifica durante cuánto tiempo puede almacenarse en caché la respuesta a la solicitud de comprobaciones preparatorias. Para establecer este encabezado:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=89-94)]

<a name="cors-how-cors-works"></a>

## <a name="how-cors-works"></a>Funcionamiento de CORS

Esta sección describe lo que ocurre en una solicitud de CORS en el nivel de los mensajes HTTP. Es importante comprender el funcionamiento de CORS para que se pueda configurar correctamente la directiva CORS y se puedan solucionar los problemas cuando se produzcan comportamientos inesperados.

La especificación de CORS presenta varios encabezados HTTP nuevos que permiten las solicitudes entre orígenes. Si un explorador es compatible con CORS, establece estos encabezados automáticamente para las solicitudes entre orígenes. No se necesita código JavaScript personalizado para habilitar CORS.

Este es un ejemplo de una solicitud entre orígenes. El encabezado `Origin` proporciona el dominio del sitio que está realizando la solicitud:

```
GET http://myservice.azurewebsites.net/api/test HTTP/1.1
Referer: http://myclient.azurewebsites.net/
Accept: */*
Accept-Language: en-US
Origin: http://myclient.azurewebsites.net
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
```

Si el servidor permite la solicitud, establece el encabezado Access-Control-Allow-Origin en la respuesta. El valor de este encabezado coincide con el encabezado de origen de la solicitud, o es el valor de carácter comodín "*", lo que significa que se permite cualquier origen:

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Type: text/plain; charset=utf-8
Access-Control-Allow-Origin: http://myclient.azurewebsites.net
Date: Wed, 20 May 2015 06:27:30 GMT
Content-Length: 12

Test message
```

Si la respuesta no incluye el encabezado Access-Control-Allow-Origin, se produce un error en la solicitud de AJAX. En concreto, el explorador no permite la solicitud. Incluso si el servidor devuelve una respuesta correcta, el explorador no expone la respuesta a la aplicación cliente.

### <a name="preflight-requests"></a>Solicitudes preparatorias

Para algunas solicitudes CORS, el explorador envía una solicitud adicional, denominada "solicitud preparatoria," antes de enviar la solicitud real para el recurso. El explorador puede omitir la solicitud preparatoria si se cumplen las condiciones siguientes:

* El método de solicitud es GET, HEAD o POST, y

* La aplicación no establece los encabezados de solicitud que no sean Accept, Accept-Language, Content-Language, Content-Type o Last-Event-ID, y

* El encabezado Content-Type (si se establece) es uno de los siguientes:

  * application/x-www-form-urlencoded

  * multipart/form-data

  * text/plain

La regla sobre los encabezados de solicitud se aplica a los encabezados que la aplicación establece mediante una llamada a setRequestHeader en el objeto XMLHttpRequest. (La especificación de CORS llama a estos "encabezados de solicitud de autor"). La regla no se aplica a los encabezados que puede establecer el explorador, como User-Agent, Host o Content-Length.

Este es un ejemplo de una solicitud preparatoria:

```
OPTIONS http://myservice.azurewebsites.net/api/test HTTP/1.1
Accept: */*
Origin: http://myclient.azurewebsites.net
Access-Control-Request-Method: PUT
Access-Control-Request-Headers: accept, x-my-custom-header
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
Content-Length: 0
```

La solicitud preparatoria utiliza el método HTTP OPTIONS. Incluye dos encabezados especiales:

* Access-Control-Request-Method: el método HTTP que se usará para la solicitud real.

* Access-Control-Request-Headers: una lista de encabezados de solicitud que la aplicación establece en la solicitud real. (De nuevo, esto no incluye los encabezados que establece el explorador).

Esta es una respuesta de ejemplo, suponiendo que el servidor permite que la solicitud:

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 0
Access-Control-Allow-Origin: http://myclient.azurewebsites.net
Access-Control-Allow-Headers: x-my-custom-header
Access-Control-Allow-Methods: PUT
Date: Wed, 20 May 2015 06:33:22 GMT
```

La respuesta incluye un encabezado Access-Control-Allow-Methods que enumera los métodos permitidos y, opcionalmente, un encabezado Access-Control-Allow-Headers, que muestra los encabezados permitidos. Si la solicitud de comprobaciones preparatorias se realiza correctamente, el explorador envía la solicitud real, como se ha descrito anteriormente.
