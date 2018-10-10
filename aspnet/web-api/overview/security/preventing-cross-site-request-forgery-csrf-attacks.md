---
uid: web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks
title: Prevención de ataques de falsificación (CSRF) de solicitud entre sitios en ASP.NET MVC
author: MikeWasson
description: Describe el ataque de solicitudes entre sitios (CSRF) de la falsificación y cómo implementar medidas de anti-CSRF en Web de ASP.NET MVC.
ms.author: riande
ms.date: 12/12/2012
ms.assetid: 81d46f14-8f48-4d8c-830d-cc8d594dc11b
msc.legacyurl: /web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks
msc.type: authoredcontent
ms.openlocfilehash: c88975d1c205e9d0733bfb4c710b92bc8fdaaa7a
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/10/2018
ms.locfileid: "48911502"
---
<a name="preventing-cross-site-request-forgery-csrf-attacks-in-aspnet-mvc-application"></a>Prevención de ataques de falsificación (CSRF) de solicitudes entre sitios en la aplicación ASP.NET MVC
====================
por [Mike Wasson](https://github.com/MikeWasson)

Falsificación de solicitud entre sitios (CSRF) es un ataque en un sitio malintencionado envía una solicitud a un sitio vulnerable donde el usuario ha iniciado en

Este es un ejemplo de un ataque CSRF:

1. Un usuario inicia sesión en `www.example.com` utilizando la autenticación de formularios.
2. El servidor autentica al usuario. La respuesta del servidor incluye una cookie de autenticación.
3. Sin cerrar la sesión, el usuario visita un sitio web malicioso. Este sitio malintencionado contiene el siguiente formulario HTML: 

    [!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample1.html)]

    Tenga en cuenta que la acción de formulario se publica en el sitio vulnerable, no en el sitio malintencionado. Esta es la parte "cross-site" de CSRF.
4. El usuario hace clic con el botón Enviar. El explorador incluye la cookie de autenticación con la solicitud.
5. La solicitud se ejecuta en el servidor con contexto de autenticación del usuario y puede hacer cualquier cosa que puede hacer un usuario autenticado.

Aunque este ejemplo requiere que el usuario haga clic en el botón del formulario, la página malintencionada podría tal como ejecutar fácilmente una secuencia de comandos que envía el formulario automáticamente. Además, con SSL no impide que un ataque CSRF, dado que el sitio Web malintencionado puede enviar una solicitud de "https://".

Normalmente, los ataques CSRF son posibles con sitios web que usan cookies para la autenticación, porque los exploradores envían todas las cookies relevantes al sitio web de destino. Sin embargo, los ataques CSRF no están limitados a aprovechar las cookies. Por ejemplo, la autenticación básica e implícita también son vulnerables. Después de un usuario inicia sesión con la autenticación básica o implícita. el explorador envía automáticamente las credenciales hasta que finaliza la sesión.

## <a name="anti-forgery-tokens"></a>Tokens antifalsificación

Para ayudar a evitar ataques CSRF, ASP.NET MVC usa tokens antifalsificación, también denominados *solicitar tokens de comprobación*.

1. El cliente solicita una página HTML que contiene un formulario.
2. El servidor incluye dos tokens en la respuesta. Un token se envía como una cookie. El otro se coloca en un campo de formulario oculto. Los tokens se generan aleatoriamente para que un adversario no puede adivinar los valores.
3. Cuando el cliente envía el formulario, ambos tokens debe enviar al servidor. El cliente envía el token de cookie como una cookie y envía el token de formulario dentro de los datos del formulario. (Un cliente del explorador hace automáticamente cuando el usuario envía el formulario.)
4. Si una solicitud no incluye ambos tokens, el servidor no permite la solicitud.

Este es un ejemplo de un formulario HTML con un token de formulario oculto:

[!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample2.html)]

Tokens antifalsificación funcionan porque la página malintencionada no puede leer los tokens de usuario, debido a las directivas del mismo origen. ([Directivas del mismo origen](http://www.w3.org/Security/wiki/Same_Origin_Policy) evitar que los documentos hospedados en dos sitios diferentes de obtener acceso al contenido de los demás. Por lo que en el ejemplo anterior, la página malintencionada puede enviar solicitudes a ejemplo.com, pero no puede leer la respuesta).

Para evitar ataques CSRF, use tokens antifalsificación con cualquier protocolo de autenticación que el explorador envía automáticamente las credenciales después de que el usuario inicia sesión. Esto incluye los protocolos de autenticación basada en cookies, como la autenticación de formularios, así como protocolos, como la autenticación básica e implícita.

Debe solicitar tokens antifalsificación para todos los métodos nonsafe (POST, PUT, DELETE). Además, asegúrese de que los métodos de prueba de errores (GET, HEAD) no tienen efectos secundarios. Además, si habilita la compatibilidad entre dominios, como la CORS o JSONP, métodos seguros incluso como obtener son potencialmente vulnerables a ataques CSRF, lo que permite al atacante leer datos potencialmente confidenciales.

## <a name="anti-forgery-tokens-in-aspnet-mvc"></a>Tokens antifalsificación en ASP.NET MVC

Para agregar los tokens antifalsificación a una página de Razor, use el **HtmlHelper.AntiForgeryToken** método auxiliar:

[!code-cshtml[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample3.cshtml)]

Este método agrega el campo de formulario oculto y también establece el token de cookie.

## <a name="anti-csrf-and-ajax"></a>Anti-CSRF y AJAX

El token de formulario puede ser un problema para las solicitudes AJAX, porque una solicitud AJAX podría enviar datos JSON, no los datos de formulario HTML. Una solución consiste en enviar los tokens en un encabezado HTTP personalizado. El código siguiente usa la sintaxis de Razor para generar los tokens y, a continuación, agrega a una solicitud AJAX. Los tokens se generan en el servidor mediante una llamada a **AntiForgery.GetTokens**.

[!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample4.html)]

Al procesar la solicitud, extraiga los tokens del encabezado de solicitud. A continuación, llame a la **AntiForgery.Validate** método para validar los tokens. El **validar** método produce una excepción si los tokens no son válidos.

[!code-csharp[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample5.cs)]
