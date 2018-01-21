---
title: "Habilitación de solicitudes entre orígenes (CORS)"
author: rick-anderson
description: "Este documento presentan como un estándar para permitir o rechazar las solicitudes entre orígenes en una aplicación de ASP.NET Core CORS."
ms.author: riande
manager: wpickett
ms.date: 05/17/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/cors
ms.openlocfilehash: e6b49b9dde94cc7d035ea91b992a13df8cb8caf2
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2018
---
# <a name="enabling-cross-origin-requests-cors"></a>Habilitación de solicitudes entre orígenes (CORS)

Por [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), y [Tom Dykstra](https://github.com/tdykstra)

Seguridad del explorador impide que una página web que realiza las solicitudes AJAX a otro dominio. Esta restricción se denomina la *directiva de mismo origen*y se impide que un sitio malintencionado pueda leer los datos confidenciales desde otro sitio. Sin embargo, en ocasiones, puede permitir que otros sitios realizar solicitudes entre orígenes en la API web.

[Entre el uso compartido de recursos de origen](http://www.w3.org/TR/cors/) (CORS) es un estándar de W3C que permite que un servidor relajar la directiva de mismo origen. Con CORS, un servidor puede permitir explícitamente algunas solicitudes entre orígenes y rechazar los otros usuarios. CORS es más seguro y más flexible que técnicas anteriores como [JSONP](https://wikipedia.org/wiki/JSONP). En este tema se muestra cómo habilitar CORS en una aplicación de ASP.NET Core.

## <a name="what-is-same-origin"></a>¿Qué es el "mismo origen"?

Dos direcciones URL tienen el mismo origen si tienen puertos, los hosts y los esquemas idénticos. ([RFC 6454](http://tools.ietf.org/html/rfc6454))

Estas dos direcciones URL tienen el mismo origen:

* `http://example.com/foo.html`

* `http://example.com/bar.html`

Estas direcciones URL tengan diferentes orígenes que el anterior dos:

* `http://example.net`-Dominio diferente

* `http://www.example.com/foo.html`-Otro subdominio

* `https://example.com/foo.html`-Esquema diferente

* `http://example.com:9000/foo.html`-Otro puerto

> [!NOTE]
> Internet Explorer no tiene en cuenta el puerto al comparar elementos Origin.

## <a name="setting-up-cors"></a>Configuración de CORS

Para configurar agregar CORS para su aplicación el `Microsoft.AspNetCore.Cors` paquete al proyecto.

Agregue los servicios CORS de Startup.cs:

[!code-csharp[Main](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors)]

## <a name="enabling-cors-with-middleware"></a>Habilitación de CORS con middleware

Para habilitar CORS para toda la aplicación agregar el middleware CORS a la canalización de solicitud usando el `UseCors` método de extensión. Tenga en cuenta que el middleware CORS debe preceder a cualquier extremo definido en la aplicación que desea admitir las solicitudes entre orígenes (p. ej. antes de cualquier llamada a `UseMvc`).

Puede especificar una directiva de entre orígenes cuando se agrega el middleware CORS mediante la `CorsPolicyBuilder` clase. Existen dos formas de lograr esto. La primera consiste en llamar a UseCors con una expresión lambda:

[!code-csharp[Main](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]

**Nota:** debe especificarse la dirección URL sin una barra diagonal final (`/`). Si la dirección URL finaliza con `/`, devolverá la comparación `false` y no se devolverá ningún encabezado.

La expresión lambda toma un `CorsPolicyBuilder` objeto. Encontrará una lista de los [opciones de configuración](#cors-policy-options) más adelante en este tema. En este ejemplo, la directiva permite que las solicitudes entre orígenes de `http://example.com` y no hay otros orígenes.

Tenga en cuenta que CorsPolicyBuilder tiene una API fluida, por lo que se pueden encadenar llamadas de método:

[!code-csharp[Main](../security/cors/sample/CorsExample3/Startup.cs?highlight=3&range=29-32)]

El segundo enfoque es definir una o varias directivas CORS con nombre y, a continuación, seleccione la directiva por su nombre en tiempo de ejecución.

[!code-csharp[Main](cors/sample/CorsExample2/Startup.cs?name=snippet_begin)]

Este ejemplo agrega una directiva CORS denominada "AllowSpecificOrigin". Para seleccionar la directiva, pase el nombre a `UseCors`.

## <a name="enabling-cors-in-mvc"></a>Habilitación de CORS en MVC

Como alternativa, puede usar MVC para aplicar CORS específico por cada acción, por cada controlador o globalmente para todos los controladores. Al utilizar MVC para habilitar CORS se utilizan los mismos servicios CORS, pero el middleware CORS no lo es.

### <a name="per-action"></a>Por cada acción

Para especificar una directiva CORS para una acción específica agregar el `[EnableCors]` atribuir a la acción. Especifique el nombre de la directiva.

[!code-csharp[Main](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction)]

### <a name="per-controller"></a>Por cada controlador

Para especificar la directiva CORS de un controlador específico agregar el `[EnableCors]` atributo a la clase de controlador. Especifique el nombre de la directiva.

[!code-csharp[Main](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController)]

### <a name="globally"></a>Global

Puede habilitar CORS globalmente para todos los controladores mediante la adición de la `CorsAuthorizationFilterFactory` filtro a la colección de filtros globales:

[!code-csharp[Main](cors/sample/CorsMVC/Startup2.cs?name=snippet_configureservices)]

Es el orden de prioridad: acción, controlador, de forma global. Las directivas de nivel de acción tienen prioridad sobre las directivas de nivel de controlador y las directivas de nivel de controlador tienen prioridad sobre las directivas globales.

### <a name="disable-cors"></a>Deshabilitar CORS

Para deshabilitar CORS para una acción o controlador, utilice la `[DisableCors]` atributo.

[!code-csharp[Main](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction)]

## <a name="cors-policy-options"></a>Opciones de directiva CORS

Esta sección describe las distintas opciones que se pueden establecer en una directiva CORS.

* [Establecer los orígenes permitidos](#set-the-allowed-origins)

* [Establecer los métodos HTTP permitidos](#set-the-allowed-http-methods)

* [Establecer los encabezados de solicitudes permitido](#set-the-allowed-request-headers)

* [Establecer los encabezados de respuesta expuesto](#set-the-exposed-response-headers)

* [Credenciales en solicitudes cross-origin](#credentials-in-cross-origin-requests)

* [Establecer el tiempo de expiración preparatoria](#set-the-preflight-expiration-time)

Para algunas opciones pueden serle de ayuda leer [CORS cómo funciona](#how-cors-works) primero.

### <a name="set-the-allowed-origins"></a>Establecer los orígenes permitidos

Para permitir que uno o más orígenes específicos:

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=19-23)]

Para permitir que todos los orígenes:

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs??range=27-31)]

Debe considerar detenidamente antes de permitir que las solicitudes de cualquier origen. Significa que literalmente cualquier sitio Web puede realizar las llamadas de AJAX a su API.

### <a name="set-the-allowed-http-methods"></a>Establecer los métodos HTTP permitidos

Para permitir que todos los métodos HTTP:

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=44-49)]

Esto afecta a solicitudes preparatorias y encabezado de acceso-Control-Allow-Methods.

### <a name="set-the-allowed-request-headers"></a>Establecer los encabezados de solicitudes permitido

Una solicitud preparatoria de CORS puede incluir un encabezado de acceso-Access-Control-Request-Headers, enumerar los encabezados HTTP establecidos por la aplicación (la "crear encabezados de solicitud").

A encabezados específicos de la lista blanca de direcciones:

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=53-58)]

Para permitir que todos crear encabezados de solicitud:

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=62-67)]

Los exploradores no son completamente coherentes en forma de conjunto de acceso-Access-Control-Request-Headers. Si establece encabezados en ningún otro valor distinto de "*", debe incluir al menos "Aceptar", "content-type" y "origen", además de los encabezados personalizados que desee admitir.

### <a name="set-the-exposed-response-headers"></a>Establecer los encabezados de respuesta expuesto

De forma predeterminada, el explorador no expone todos los encabezados de respuesta a la aplicación. (See [http://www.w3.org/TR/cors/#simple-response-header](http://www.w3.org/TR/cors/#simple-response-header).) Los encabezados de respuesta que están disponibles de forma predeterminada son:

* Cache-Control

* Content-Language

* Tipo de contenido

* Expira

* Última modificación

* Pragma

La especificación CORS llama a estos *encabezados de respuesta simple*. Para que otros encabezados estén disponibles para la aplicación:

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=71-76)]

### <a name="credentials-in-cross-origin-requests"></a>Credenciales en solicitudes cross-origin

Las credenciales requieren un tratamiento especial en una solicitud de CORS. De forma predeterminada, el explorador no envía ninguna credencial con una solicitud entre orígenes. Las credenciales son las cookies, así como esquemas de autenticación HTTP. Para enviar las credenciales con una solicitud entre orígenes, el cliente debe establecer XMLHttpRequest.withCredentials en true.

Utilizar directamente el objeto XMLHttpRequest:

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

Además, el servidor debe permitir las credenciales. Para permitir que las credenciales entre orígenes:

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=80-85)]

Ahora, la respuesta HTTP incluirá un encabezado de acceso-Control-Allow-Credentials, que indica al explorador que el servidor permite que las credenciales para una solicitud entre orígenes.

Si el explorador envía las credenciales, pero la respuesta no incluye un encabezado de acceso-Control-Allow-Credentials válido, el explorador no expondrá la respuesta a la aplicación y se produce un error en la solicitud de AJAX.

Tenga mucho cuidado acerca de cómo permitir credenciales entre orígenes, ya que significa que un sitio Web en otro dominio puede enviar credenciales ha iniciado la sesión de un usuario a la aplicación en nombre del usuario, sin el conocimiento del usuario. El CORS spec también los Estados que orígenes de configuración que "*" (todos los orígenes) no es válido si el encabezado de acceso-Control-Allow-Credentials está presente.

### <a name="set-the-preflight-expiration-time"></a>Establecer el tiempo de expiración preparatoria

El encabezado de acceso-Control: Max-Age especifica cuánto tiempo puede almacenarse en caché la respuesta a la solicitud preparatoria. Para establecer este encabezado:

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=89-94)]

<a name="cors-how-cors-works"></a>

## <a name="how-cors-works"></a>Funcionamiento de CORS

En esta sección se describe lo que sucede en una solicitud CORS, en el nivel de los mensajes HTTP. Es importante comprender el funcionamiento de CORS, por lo que puede configurar la directiva CORS correctamente y solucionar los problemas si lo no funciona como esperaba.

La especificación de CORS presenta varios encabezados HTTP nuevo que permiten las solicitudes entre orígenes. Si un explorador es compatible con CORS, establece estos encabezados automáticamente para las solicitudes entre orígenes; no es necesario hacer nada especial en el código de JavaScript.

Este es un ejemplo de una solicitud entre orígenes. El encabezado de "Origen" proporciona el dominio del sitio que está realizando la solicitud:

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

Si el servidor permite la solicitud, Establece el encabezado de acceso Access-Control-Allow-Origin en la respuesta. El valor de este encabezado coincide con el encabezado de origen de la solicitud, o es el valor de carácter comodín "*", lo que significa que se permite cualquier origen:

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

Si la respuesta no incluye el encabezado de acceso Access-Control-Allow-Origin, se produce un error en la solicitud de AJAX. En concreto, el explorador no permite la solicitud. Incluso si el servidor devuelve una respuesta correcta, el explorador no disponer de la respuesta a la aplicación cliente.

### <a name="preflight-requests"></a>Solicitudes preparatorias

Para algunas solicitudes CORS, el explorador envía una solicitud adicional, denominada "solicitud preparatoria," antes de enviar la solicitud real para el recurso. El explorador puede omitir la solicitud preparatoria si se cumplen las condiciones siguientes:

* El método de solicitud es GET, HEAD o POST, y

* La aplicación no establece los encabezados de solicitud que no sean Accept, Accept-Language, Content-Language, Content-Type o último--Id. de evento, y

* El encabezado Content-Type (si establece) es uno de los siguientes:

  * application/x-www-form-urlencoded

  * multipart/form-data

  * texto/sin formato

La regla acerca de los encabezados de solicitud se aplica a los encabezados de la aplicación se establece mediante una llamada a setRequestHeader en el objeto XMLHttpRequest. (La especificación de CORS llama a estos "encabezados de solicitud de autor"). La regla no se aplica a los encabezados que se puede establecer el explorador, como agente de usuario, el Host o Content-Length.

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

* Acceso Access-Control-Request-Method: El método HTTP que se usará para la solicitud real.

* Acceso Access-Control-Request-Headers: Una lista de encabezados de solicitud que la aplicación se establece en la solicitud real. (De nuevo, esto no incluye encabezados que establece el explorador.)

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

La respuesta incluye un encabezado de acceso-Control-Allow-Methods que enumera los métodos permitidos y, opcionalmente, un encabezado Access-Control-permitir-Headers, que se muestra los encabezados permitidos. Si la solicitud preparatoria se realiza correctamente, el explorador envía la solicitud real, como se describió anteriormente.
