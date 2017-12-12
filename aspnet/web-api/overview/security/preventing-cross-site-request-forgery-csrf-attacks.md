---
uid: web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks
title: "Prevención de ataques de falsificación (CSRF) de solicitud entre sitios en ASP.NET Web API | Documentos de Microsoft"
author: MikeWasson
description: "Describe los ataques de falsificación (CSRF) de solicitud entre sitios y cómo implementar medidas de anti-CSRF en ASP.NET Web API."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/12/2012
ms.topic: article
ms.assetid: 81d46f14-8f48-4d8c-830d-cc8d594dc11b
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks
msc.type: authoredcontent
ms.openlocfilehash: 1cd03f3b396cc2ece1d8dbe6820f6277c02d8e62
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="preventing-cross-site-request-forgery-csrf-attacks-in-aspnet-web-api"></a>Prevención de ataques de falsificación (CSRF) de solicitud entre sitios en ASP.NET Web API
====================
por [Mike Wasson](https://github.com/MikeWasson)

Falsificación de solicitud entre sitios (CSRF) es un ataque en un sitio malintencionado envía una solicitud a un sitio vulnerable donde el usuario inició sesión en

Este es un ejemplo de un ataque CSRF:

1. Un usuario inicia sesión en www.example.com, utilizando la autenticación de formularios.
2. El servidor autentica al usuario. La respuesta del servidor incluye una cookie de autenticación.
3. Sin cerrar la sesión, el usuario visita un sitio web malintencionado. Este sitio malintencionado contiene el siguiente formulario HTML: 

    [!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample1.html)]

    Tenga en cuenta que la acción de formulario se envía al sitio vulnerable, no en el sitio malintencionado. Esta es la parte "cross-site" de CSRF.
4. El usuario hace clic en el botón Enviar. El explorador incluye la cookie de autenticación con la solicitud.
5. La solicitud se ejecuta en el servidor con el contexto de autenticación del usuario y puede hacer todo lo que puede hacer un usuario autenticado.

Aunque este ejemplo requiere que el usuario haga clic en el botón del formulario, la página malintencionada podría tal y como ejecutar fácilmente un script que envía el formulario automáticamente. Además, con SSL no evita que un ataque CSRF, dado que el sitio malintencionado puede enviar una solicitud de "https://".

Normalmente, los ataques CSRF son posibles con sitios web que usan cookies para la autenticación, porque los exploradores envían todas las cookies relevantes para el sitio web de destino. Sin embargo, los ataques CSRF no están limitados a aprovecharse de las cookies. Por ejemplo, la autenticación básica e implícita también son vulnerables. Después de un usuario inicia sesión con autenticación básica o implícita. el explorador envía automáticamente las credenciales hasta que finaliza la sesión.

## <a name="anti-forgery-tokens"></a>Tokens antifalsificación

Para ayudar a evitar ataques CSRF, ASP.NET MVC usa tokens antifalsificación, también denominados *solicitar tokens de comprobación*.

1. El cliente solicita una página HTML que contiene un formulario.
2. El servidor incluye dos tokens en la respuesta. Un símbolo (token) se envía como una cookie. El otro se coloca en un campo oculto del formulario. Los tokens se generan aleatoriamente para que un adversario no puede adivinar los valores.
3. Cuando el cliente envía el formulario, deben enviar dos tokens en el servidor. El cliente envía el token de cookie como una cookie y envía el token de formulario dentro de los datos del formulario. (Un cliente del explorador lo hace automáticamente cuando el usuario envía el formulario.)
4. Si una solicitud no incluye ambos tokens, el servidor no permite la solicitud.

Este es un ejemplo de un formulario HTML con un token de formulario oculto:

[!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample2.html)]

Tokens antifalsificación funcionan porque no puede leer la página malintencionada tokens del usuario, debido a las directivas del mismo origen. ([Directivas de mismo origen](http://www.w3.org/Security/wiki/Same_Origin_Policy) evitar que los documentos que se hospedan en dos sitios distintos del acceso al contenido de todas las demás. Por lo que en el ejemplo anterior, la página malintencionada puede enviar solicitudes a ejemplo.com, pero no puede leer la respuesta).

Para evitar ataques CSRF, use tokens antifalsificación con cualquier protocolo de autenticación en el explorador en modo silencioso envía credenciales después de que el usuario inicia sesión. Esto incluye los protocolos de autenticación basados en cookies, como la autenticación de formularios, así como los protocolos, como la autenticación básica e implícita.

Debe requerir tokens antifalsificación para los métodos nonsafe (POST, PUT, DELETE). Además, asegúrese de que los métodos de prueba de errores (GET, HEAD) no tiene efectos secundarios. Además, si habilita la compatibilidad entre dominios, como CORS o JSONP, incluso seguros métodos como GET son potencialmente vulnerables a ataques CSRF, lo que permite al atacante leer datos potencialmente confidenciales.

## <a name="anti-forgery-tokens-in-aspnet-mvc"></a>Tokens antifalsificación en ASP.NET MVC

Para agregar los tokens antifalsificación a una página de Razor, use la **HtmlHelper.AntiForgeryToken** método auxiliar:

[!code-cshtml[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample3.cshtml)]

Este método agrega el campo de formulario oculto y también establece el token de cookie.

## <a name="anti-csrf-and-ajax"></a>Anti-CSRF y AJAX

El token de formulario puede ser un problema para las solicitudes de AJAX, porque una solicitud AJAX puede enviar datos JSON, no los datos de formulario HTML. Una solución consiste en enviar los tokens en un encabezado HTTP personalizado. El código siguiente utiliza la sintaxis de Razor para generar los tokens y, a continuación, agrega los tokens a una solicitud AJAX. Los tokens se generan en el servidor mediante una llamada a **AntiForgery.GetTokens**.

[!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample4.html)]

Cuando se procesa la solicitud, extraiga los tokens en el encabezado de solicitud. A continuación, llame a la **AntiForgery.Validate** método para validar los tokens. El **validar** método produce una excepción si los tokens no son válidos.

[!code-csharp[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample5.cs)]
