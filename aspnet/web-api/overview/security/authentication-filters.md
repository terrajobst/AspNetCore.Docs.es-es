---
uid: web-api/overview/security/authentication-filters
title: Los filtros de autenticación en ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: Un filtro de autenticación es un componente que se autentica una solicitud HTTP. Web API 2 y MVC 5 admiten filtros de autenticación, pero difieren ligeramente...
ms.author: riande
ms.date: 09/25/2014
ms.assetid: b9882e53-b3ca-4def-89b0-322846973ccb
msc.legacyurl: /web-api/overview/security/authentication-filters
msc.type: authoredcontent
ms.openlocfilehash: a14facad4cbd0f9be1ff7bde2667f61ec8cc2a14
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828506"
---
<a name="authentication-filters-in-aspnet-web-api-2"></a>Filtros de autenticación en ASP.NET Web API 2
====================
por [Mike Wasson](https://github.com/MikeWasson)

> Un filtro de autenticación es un componente que se autentica una solicitud HTTP. Web API 2 y MVC 5 admiten filtros de autenticación, pero difieren ligeramente, principalmente en las convenciones de nomenclatura para la interfaz de filtro. En este tema se describe los filtros de autenticación de API Web.


Los filtros de autenticación le permiten establecer un esquema de autenticación para los controladores o acciones individuales. De este modo, la aplicación puede admitir distintos mecanismos de autenticación para los distintos recursos HTTP.

En este artículo, le mostraré el código desde el [autenticación básica](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/BasicAuthentication/ReadMe.txt) ejemplo en [ http://aspnet.codeplex.com ](http://aspnet.codeplex.com). El ejemplo muestra un filtro de autenticación que implementa el esquema de autenticación de acceso básica de HTTP (RFC 2617). El filtro se implementa en una clase denominada `IdentityBasicAuthenticationAttribute`. No muestro todo el código del ejemplo, simplemente las partes que muestran cómo escribir un filtro de autenticación.

## <a name="setting-an-authentication-filter"></a>Establecer un filtro de autenticación

Al igual que otros filtros, los filtros de autenticación pueden ser aplicados por controlador, por acción o globalmente a todos los controladores de API Web.

Para aplicar un filtro de autenticación a un controlador, decore la clase de controlador con el atributo de filtro. El código siguiente establece el `[IdentityBasicAuthentication]` filtro en una clase de controlador, que habilita la autenticación básica para todas las acciones del controlador.

[!code-csharp[Main](authentication-filters/samples/sample1.cs)]

Para aplicar el filtro a una acción, decore la acción con el filtro. El código siguiente establece la `[IdentityBasicAuthentication]` filtro en el controlador `Post` método.

[!code-csharp[Main](authentication-filters/samples/sample2.cs)]

Para aplicar el filtro a todos los controladores de API Web, agréguelo a **GlobalConfiguration.Filters**.

[!code-csharp[Main](authentication-filters/samples/sample3.cs)]

## <a name="implementing-a-web-api-authentication-filter"></a>Implementar un filtro de autenticación de API Web

En la API Web, implementan los filtros de autenticación la [System.Web.Http.Filters.IAuthenticationFilter](https://msdn.microsoft.com/library/system.web.http.filters.iauthenticationfilter.aspx) interfaz. También debe heredar de **System.Attribute**, para que se aplican como atributos.

El **IAuthenticationFilter** interfaz tiene dos métodos:

- **AuthenticateAsync** autentica la solicitud mediante la validación de credenciales en la solicitud, si está presente.
- **ChallengeAsync** agrega un desafío de autenticación a la respuesta HTTP, si es necesario.

Estos métodos se corresponden con el flujo de autenticación definido en [RFC 2612](http://tools.ietf.org/html/rfc2616) y [RFC 2617](http://tools.ietf.org/html/rfc2617):

1. El cliente envía las credenciales en el encabezado de autorización. Esto suele suceder después de que el cliente recibe una respuesta 401 (no autorizado) desde el servidor. Sin embargo, un cliente puede enviar las credenciales con cualquier solicitud, no solo después de obtener un error 401.
2. Si el servidor no aceptó las credenciales, devuelve una respuesta 401 (no autorizado). La respuesta incluye un encabezado Www-Authenticate que contiene uno o varios desafíos. Cada desafío especifica un esquema de autenticación reconocido por el servidor.

El servidor también puede devolver 401 de una solicitud anónima. De hecho, es normalmente cómo se inicia el proceso de autenticación:

1. El cliente envía una solicitud anónima.
2. El servidor devuelve 401.
3. Los clientes se vuelve a enviar la solicitud con las credenciales.

Este flujo incluye ambos *autenticación* y *autorización* pasos.

- Autenticación demuestra la identidad del cliente.
- La autorización determina si el cliente puede tener acceso a un recurso determinado.

En API Web, los filtros de autenticación controlan la autenticación, pero no la autorización. Autorización debe hacerse mediante un filtro de autorización o dentro de la acción del controlador.

Este es el flujo de la canalización de Web API 2:

1. Antes de invocar una acción, API Web crea una lista de los filtros de autenticación para esa acción. Esto incluye filtros con ámbito de acción, el ámbito de controlador y un ámbito global.
2. Las llamadas de API de Web **AuthenticateAsync** en todos los filtros de la lista. Cada filtro puede validar las credenciales en la solicitud. Si cualquier filtro que valida correctamente las credenciales, el filtro se crea un **IPrincipal** y lo asocia a la solicitud. Un filtro también puede desencadenar un error en este momento. Si es así, el resto de la canalización no se ejecuta.
3. Suponiendo que no hay ningún error, la solicitud fluye a través del resto de la canalización.
4. Por último, llama a cada filtro de autenticación Web API **ChallengeAsync** método. Filtros de usar este método para agregar un desafío a la respuesta, si es necesario. Normalmente (pero no siempre) que se debería suceder en respuesta a un error 401.

Los diagramas siguientes muestran dos casos posibles. En la primera, la solicitud se autentica correctamente el filtro de autenticación, un filtro de autorización autoriza la solicitud y la acción del controlador devuelve 200 (OK).

![](authentication-filters/_static/image1.png)

En el segundo ejemplo, el filtro de autenticación se autentica la solicitud, pero el filtro de autorización devuelve 401 (no autorizado). En este caso, no se invoca la acción del controlador. El filtro de autenticación, agrega un encabezado Www-Authenticate para la respuesta.

![](authentication-filters/_static/image2.png)

Son posibles otras combinaciones&mdash;por ejemplo, si la acción del controlador permite solicitudes anónimas, podría tener un filtro de autenticación, pero sin autorización.

## <a name="implementing-the-authenticateasync-method"></a>Implementación del método AuthenticateAsync

El **AuthenticateAsync** método intenta autenticar la solicitud. Esta es la firma de método:

[!code-csharp[Main](authentication-filters/samples/sample4.cs)]

El **AuthenticateAsync** método debe realizar una de las siguientes acciones:

1. No hay nada (no operativo).
2. Crear un **IPrincipal** y establézcalo en la solicitud.
3. Establezca un resultado de error.

Opción (1) significa que la solicitud no tenía las credenciales que entiende el filtro. Opción (2) significa que el filtro ha autenticado correctamente la solicitud. Opción (3) significa que la solicitud tenía las credenciales no válidas (por ejemplo, una contraseña incorrecta), lo que desencadena una respuesta de error.

Este es un esquema general para implementar **AuthenticateAsync**.

1. Busque las credenciales en la solicitud.
2. Si no hay ninguna credencial, no haga nada y devolver (no operativo).
3. Si hay credenciales, pero el filtro no reconoce el esquema de autenticación, no haga nada y devolver (no operativo). Otro filtro de la canalización podría entender el esquema.
4. Si no hay credenciales que comprende el filtro, intenta autenticarse en ellos.
5. Si las credenciales son perjudiciales, devuelve 401 estableciendo `context.ErrorResult`.
6. Si las credenciales son válidas, cree un **IPrincipal** y establecer `context.Principal`.

El código siguiente muestra el **AuthenticateAsync** método desde el [autenticación básica](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/BasicAuthentication/ReadMe.txt) ejemplo. Los comentarios indican cada paso. El código muestra varios tipos de error: encabezado de una autorización no tiene credenciales, las credenciales con formato incorrecto y un nombre de usuario o contraseña incorrectos.

[!code-csharp[Main](authentication-filters/samples/sample5.cs)]

## <a name="setting-an-error-result"></a>Configuración de un resultado de Error

Si las credenciales son válidas, debe establecer el filtro `context.ErrorResult` a un **IHttpActionResult** que crea una respuesta de error. Para obtener más información acerca de **IHttpActionResult**, consulte [los resultados de acción en Web API 2](../getting-started-with-aspnet-web-api/action-results.md).

El ejemplo de autenticación básica se incluye un `AuthenticationFailureResult` clase que es adecuado para este propósito.

[!code-csharp[Main](authentication-filters/samples/sample6.cs)]

## <a name="implementing-challengeasync"></a>Implementar ChallengeAsync

El propósito de la **ChallengeAsync** método consiste en agregar los desafíos de autenticación a la respuesta, si es necesario. Esta es la firma de método:

[!code-csharp[Main](authentication-filters/samples/sample7.cs)]

Se llama al método en cada filtro de autenticación en la canalización de solicitudes.

Es importante comprender que **ChallengeAsync** se denomina *antes* la respuesta HTTP se crea y posiblemente incluso antes de ejecutar la acción de controlador. Cuando **ChallengeAsync** se llama, `context.Result` contiene un **IHttpActionResult**, que se usa más adelante para crear la respuesta HTTP. Por lo que cuando **ChallengeAsync** es llamado, aún no conoce nada acerca de la respuesta HTTP. El **ChallengeAsync** método debe reemplazar el valor original de `context.Result` con un nuevo **IHttpActionResult**. Esto **IHttpActionResult** debe encapsular el original `context.Result`.

![](authentication-filters/_static/image3.png)

Le pondré el original **IHttpActionResult** el *resultados interno*y el nuevo **IHttpActionResult** el *resultado externo*. El resultado externo debe hacer lo siguiente:

1. Invoque el resultado interno para crear la respuesta HTTP.
2. Examinar la respuesta.
3. Agregar un desafío de autenticación a la respuesta, si es necesario.

En el siguiente ejemplo se toma del ejemplo de autenticación básica. Define un **IHttpActionResult** para el resultado de la externo.

[!code-csharp[Main](authentication-filters/samples/sample8.cs)]

El `InnerResult` propiedad interno mantiene **IHttpActionResult**. El `Challenge` propiedad representa un encabezado de autenticación de Www. Tenga en cuenta que **ExecuteAsync** llama primero al `InnerResult.ExecuteAsync` para crear la respuesta HTTP y, a continuación, agrega el desafío si es necesario.

Compruebe el código de respuesta antes de agregar el desafío. La mayoría de los esquemas de autenticación solo agregar un desafío si la respuesta es 401, como se muestra aquí. Sin embargo, algunos esquemas de autenticación lo agregue a un desafío para una respuesta correcta. Por ejemplo, vea [Negotiate](http://tools.ietf.org/html/rfc4559#section-5) (RFC 4559).

Dada la `AddChallengeOnUnauthorizedResult` clase, el código real en **ChallengeAsync** es sencillo. Que acaba de crear el resultado y adjuntarla a `context.Result`.

[!code-csharp[Main](authentication-filters/samples/sample9.cs)]

Nota: El ejemplo de autenticación básica abstrae un poco, esta lógica y colocarla en un método de extensión.

## <a name="combining-authentication-filters-with-host-level-authentication"></a>Combinación de filtros de autenticación con autenticación a nivel de Host

"Autenticación de nivel de host" es la autenticación realizada por el host (como IIS), antes que el marco de trabajo de solicitud llega a la API Web.

A menudo, es posible que desee habilitar la autenticación de nivel de host para el resto de la aplicación, pero deshabilitarla para que los controladores Web API. Por ejemplo, un escenario típico es habilitar la autenticación de formularios en el nivel de host, pero utiliza la autenticación basada en tokens para API Web.

Para deshabilitar la autenticación de nivel de host dentro de la canalización de API Web, llame a `config.SuppressHostPrincipal()` en la configuración. Esto hace que la API Web quitar el **IPrincipal** desde cualquier solicitud que entra en la canalización Web API. De hecho, lo &quot;anular-autentica&quot; la solicitud.

[!code-csharp[Main](authentication-filters/samples/sample10.cs)]

## <a name="additional-resources"></a>Recursos adicionales

[Filtros de seguridad de ASP.NET Web API](https://msdn.microsoft.com/magazine/dn781361.aspx) (MSDN Magazine)
