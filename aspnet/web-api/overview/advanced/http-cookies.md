---
uid: web-api/overview/advanced/http-cookies
title: Las Cookies HTTP en ASP.NET Web API | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 09/17/2012
ms.assetid: 243db2ec-8f67-4a5e-a382-4ddcec4b4164
msc.legacyurl: /web-api/overview/advanced/http-cookies
msc.type: authoredcontent
ms.openlocfilehash: 61e0c47efdd92a3a0b329930aeec757b446eb9b8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828534"
---
<a name="http-cookies-in-aspnet-web-api"></a>Cookies HTTP en ASP.NET Web API
====================
por [Mike Wasson](https://github.com/MikeWasson)

Este tema describe cómo enviar y recibir las cookies HTTP en Web API.

## <a name="background-on-http-cookies"></a>En segundo plano en las Cookies HTTP

En esta sección se ofrece una breve descripción de cómo se implementan las cookies en el nivel de HTTP. Para obtener más información, consulte [6265 RFC](http://tools.ietf.org/html/rfc6265).

Una cookie es un fragmento de datos que envía un servidor en la respuesta HTTP. (Opcionalmente) el cliente almacena la cookie y lo devuelve en las solicitudes de subsequet. Esto permite al cliente y servidor compartir el estado. Para establecer una cookie, el servidor incluye un encabezado Set-Cookie en la respuesta. El formato de una cookie es un par nombre-valor, con atributos opcionales. Por ejemplo:

[!code-powershell[Main](http-cookies/samples/sample1.ps1)]

Este es un ejemplo con atributos:

[!code-powershell[Main](http-cookies/samples/sample2.ps1)]

Para devolver una cookie en el servidor, el cliente incluye un encabezado de Cookie en solicitudes posteriores.

[!code-console[Main](http-cookies/samples/sample3.cmd)]

![](http-cookies/_static/image1.png)

Una respuesta HTTP puede incluir varios encabezados de Set-Cookie.

[!code-powershell[Main](http-cookies/samples/sample4.ps1)]

El cliente devuelve varias cookies con un único encabezado de Cookie.

[!code-console[Main](http-cookies/samples/sample5.cmd)]

El ámbito y la duración de una cookie se controlan mediante los atributos siguientes en el encabezado Set-Cookie:

- **Dominio**: indica que el cliente de dominio que debe recibir la cookie. Por ejemplo, si el dominio es "ejemplo.com", el cliente devuelve la cookie a cada subdominio de example.com. Si no se especifica, el dominio es el servidor de origen.
- **Ruta de acceso**: restringe la cookie a la ruta de acceso especificada dentro del dominio. Si no se especifica, se usa la ruta de acceso del URI de solicitud.
- **Expira**: establece la fecha de expiración de la cookie. El cliente elimina la cookie cuando expira.
- **Max-Age**: establece la edad máxima de la cookie. El cliente elimina la cookie cuando llega la antigüedad máxima.

Si ambos `Expires` y `Max-Age` se establecen, `Max-Age` tiene prioridad. Si ninguna está establecida, el cliente elimina la cookie cuando finaliza la sesión actual. (El significado exacto de "sesión" viene determinado por el agente de usuario).

Sin embargo, tenga en cuenta que los clientes pueden omitir las cookies. Por ejemplo, un usuario podría deshabilitar las cookies por motivos de privacidad. Los clientes pueden eliminar las cookies antes de que caduquen o limitan el número de cookies almacenadas. Por motivos de privacidad, los clientes a menudo rechazan cookies "third party", donde el dominio no coincide con el servidor de origen. En resumen, el servidor no debe confiar en obtener las cookies que establece.

## <a name="cookies-in-web-api"></a>Cookies en API Web

Para agregar una cookie a una respuesta HTTP, cree un **CookieHeaderValue** instancia que representa la cookie. A continuación, llame a la **AddCookies** método de extensión, que se define en el **System.Net.Http. HttpResponseHeadersExtensions** (clase), para agregar la cookie.

Por ejemplo, el código siguiente agrega una cookie dentro de una acción de controlador:

[!code-csharp[Main](http-cookies/samples/sample6.cs)]

Tenga en cuenta que **AddCookies** toma una matriz de **CookieHeaderValue** instancias.

Para extraer las cookies de una solicitud de cliente, llame a la **GetCookies** método:

[!code-csharp[Main](http-cookies/samples/sample7.cs)]

Un **CookieHeaderValue** contiene una colección de **CookieState** instancias. Cada **CookieState** representa una cookie. Use el método de indizador para obtener un **CookieState** por su nombre, como se muestra.

## <a name="structured-cookie-data"></a>Datos de la Cookie estructurado

Muchos exploradores limitan el número de cookies que se almacenarán&#8212;tanto el número total y el número por dominio. Por lo tanto, puede ser útil colocar datos estructurados en una cookie única, en lugar de establecer varias cookies.

> [!NOTE]
> RFC 6265 no define la estructura de datos de la cookie.


Mediante el **CookieHeaderValue** (clase), puede pasar una lista de pares nombre / valor para los datos de la cookie. Estos pares nombre-valor se codifican como datos de formulario con codificación URL en el encabezado Set-Cookie:

[!code-csharp[Main](http-cookies/samples/sample8.cs)]

El código anterior produce el siguiente encabezado Set-Cookie:

[!code-powershell[Main](http-cookies/samples/sample9.ps1)]

El **CookieState** clase proporciona un método de indizador para leer los valores secundarios de una cookie en el mensaje de solicitud:

[!code-csharp[Main](http-cookies/samples/sample10.cs)]

## <a name="example-set-and-retrieve-cookies-in-a-message-handler"></a>Ejemplo: Establecer y recuperar las Cookies en un controlador de mensajes

Los ejemplos anteriores mostraron cómo utilizar las cookies desde dentro de un controlador Web API. Otra opción consiste en usar [controladores de mensajes](http-message-handlers.md). Se invocan controladores de mensajes de la canalización de controladores. Un controlador de mensajes puede leer las cookies de la solicitud antes de que la solicitud alcanza el controlador o agregar cookies a la respuesta después de que el controlador genere la respuesta.

![](http-cookies/_static/image2.png)

El código siguiente muestra un controlador de mensajes para la creación de los identificadores de sesión. El identificador de sesión se almacena en una cookie. El controlador comprueba la solicitud de la cookie de sesión. Si la solicitud no incluye la cookie, el controlador genera un identificador de sesión nuevo. En cualquier caso, el controlador almacena el identificador de sesión en el **HttpRequestMessage.Properties** bolsa de propiedades. También se agrega la cookie de sesión a la respuesta HTTP.

Esta implementación no valida que el identificador de sesión desde el cliente lo emitió realmente el servidor. No se use como una forma de autenticación. El objetivo del ejemplo es mostrar la administración de cookies HTTP.

[!code-csharp[Main](http-cookies/samples/sample11.cs)]

Un controlador puede obtener el identificador de sesión desde el **HttpRequestMessage.Properties** bolsa de propiedades.

[!code-csharp[Main](http-cookies/samples/sample12.cs)]
