---
uid: web-api/overview/advanced/http-cookies
title: Las Cookies HTTP en ASP.NET Web API | Documentos de Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/17/2012
ms.topic: article
ms.assetid: 243db2ec-8f67-4a5e-a382-4ddcec4b4164
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/http-cookies
msc.type: authoredcontent
ms.openlocfilehash: 363ca975cf75b635b766a53eeda87cf957eed60c
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/22/2018
---
<a name="http-cookies-in-aspnet-web-api"></a>Cookies HTTP en ASP.NET Web API
====================
por [Mike Wasson](https://github.com/MikeWasson)

En este tema se describe cómo enviar y recibir las cookies HTTP en Web API.

## <a name="background-on-http-cookies"></a>Información general sobre las Cookies HTTP

Esta sección ofrece una breve descripción de cómo se implementan las cookies en el nivel HTTP. Para obtener más información, consulte [6265 RFC](http://tools.ietf.org/html/rfc6265).

Una cookie es un fragmento de datos que envía un servidor en la respuesta HTTP. (Opcionalmente) el cliente almacena la cookie y lo devuelve en las solicitudes de subsequet. Esto permite que el cliente y el servidor compartir el estado. Para establecer una cookie, el servidor incluye un encabezado Set-Cookie en la respuesta. El formato de una cookie es un par nombre-valor, con los atributos opcionales. Por ejemplo:

[!code-powershell[Main](http-cookies/samples/sample1.ps1)]

Este es un ejemplo con atributos:

[!code-powershell[Main](http-cookies/samples/sample2.ps1)]

Para devolver una cookie en el servidor, el cliente incluye un encabezado de Cookie en las solicitudes posteriores.

[!code-console[Main](http-cookies/samples/sample3.cmd)]

![](http-cookies/_static/image1.png)

Una respuesta HTTP puede incluir varios encabezados de Set-Cookie.

[!code-powershell[Main](http-cookies/samples/sample4.ps1)]

El cliente devuelve varias cookies con un único encabezado de Cookie.

[!code-console[Main](http-cookies/samples/sample5.cmd)]

El ámbito y la duración de una cookie se controlan mediante los atributos siguientes en el encabezado Set-Cookie:

- **Dominio**: indica al cliente qué dominio debe recibir la cookie. Por ejemplo, si el dominio es "ejemplo.com", el cliente devuelve la cookie para cada subdominio de example.com. Si no se especifica, el dominio es el servidor de origen.
- **Ruta de acceso**: restringe la cookie a la ruta de acceso especificada dentro del dominio. Si no se especifica, se utiliza la ruta de acceso del URI de solicitud.
- **Expira**: establece la fecha de expiración de la cookie. El cliente elimina la cookie cuando expire.
- **Max-Age**: establece la duración máxima de la cookie. El cliente elimina la cookie cuando llega la antigüedad máxima.

Si ambos `Expires` y `Max-Age` se establecen, `Max-Age` tiene prioridad. Si no está establecida, el cliente elimina la cookie cuando finaliza la sesión actual. (El significado exacto de "sesión" viene determinado por el agente de usuario).

Sin embargo, tenga en cuenta que los clientes pueden pasar por alto las cookies. Por ejemplo, un usuario podría deshabilitar las cookies por razones de privacidad. Los clientes pueden eliminar las cookies antes de que caduquen o limitan el número de cookies almacenadas. Por motivos de privacidad, los clientes a menudo rechazan cookies "third party", donde el dominio no coincide con el servidor de origen. En resumen, el servidor no debe confiar en recibir las cookies que establece.

## <a name="cookies-in-web-api"></a>Cookies en API Web

Para agregar una cookie a una respuesta HTTP, cree un **CookieHeaderValue** instancia que representa la cookie. A continuación, llame a la **AddCookies** método de extensión, que se define en el **System.Net.Http. HttpResponseHeadersExtensions** (clase), para agregar la cookie.

Por ejemplo, el código siguiente agrega una cookie dentro de una acción de controlador:

[!code-csharp[Main](http-cookies/samples/sample6.cs)]

Tenga en cuenta que **AddCookies** toma una matriz de **CookieHeaderValue** instancias.

Para extraer las cookies de una solicitud de cliente, llame a la **GetCookies** método:

[!code-csharp[Main](http-cookies/samples/sample7.cs)]

A **CookieHeaderValue** contiene una colección de **CookieState** instancias. Cada **CookieState** representa una cookie. Utilice el método de indizador para obtener un **CookieState** por su nombre, tal como se muestra.

## <a name="structured-cookie-data"></a>Datos de la Cookie estructurado

Muchos exploradores limitan el número de cookies que almacenará&#8212;tanto el número total y el número por dominio. Por lo tanto, puede ser útil colocar datos estructurados en una sola cookie, en lugar de establecer varias cookies.

> [!NOTE]
> RFC 6265 no define la estructura de datos de la cookie.


Mediante el **CookieHeaderValue** (clase), puede pasar una lista de pares nombre / valor para los datos de la cookie. Estos pares de nombre-valor se codifican como datos de formulario con codificación URL en el encabezado Set-Cookie:

[!code-csharp[Main](http-cookies/samples/sample8.cs)]

El código anterior produce el siguiente encabezado Set-Cookie:

[!code-powershell[Main](http-cookies/samples/sample9.ps1)]

El **CookieState** clase proporciona un método de indizador para leer los valores dentro de una cookie en el mensaje de solicitud:

[!code-csharp[Main](http-cookies/samples/sample10.cs)]

## <a name="example-set-and-retrieve-cookies-in-a-message-handler"></a>Ejemplo: Establecer y recuperar las Cookies en un controlador de mensajes

Los ejemplos anteriores se ha explicado cómo usar las cookies desde dentro de un controlador de Web API. Otra opción consiste en usar [controladores de mensajes](http-message-handlers.md). Controladores de mensajes se invocan anteriormente en la canalización de controladores. Un controlador de mensajes puede leer las cookies de la solicitud antes de que la solicitud llegue el controlador o agregar cookies a la respuesta después de que el controlador genere la respuesta.

![](http-cookies/_static/image2.png)

El código siguiente muestra un controlador de mensajes para la creación de identificadores de sesión. El identificador de sesión se almacena en una cookie. El controlador comprueba la solicitud para la cookie de sesión. Si la solicitud no incluye la cookie, el controlador genera un identificador de sesión nuevo. En cualquier caso, el controlador almacena el identificador de sesión en la **HttpRequestMessage.Properties** bolsa de propiedades. También agrega la cookie de sesión a la respuesta HTTP.

Esta implementación no valida que el identificador de sesión desde el cliente realmente fue emitido por el servidor. No lo use como una forma de autenticación. El punto del ejemplo es mostrar la administración de cookies HTTP.

[!code-csharp[Main](http-cookies/samples/sample11.cs)]

Un controlador puede obtener el identificador de sesión desde el **HttpRequestMessage.Properties** bolsa de propiedades.

[!code-csharp[Main](http-cookies/samples/sample12.cs)]
