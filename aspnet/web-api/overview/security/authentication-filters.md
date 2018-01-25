---
uid: web-api/overview/security/authentication-filters
title: "Filtros de autenticación en ASP.NET Web API 2 | Documentos de Microsoft"
author: MikeWasson
description: "Un filtro de autenticación es un componente que autentica una solicitud HTTP. API Web 2 y MVC 5 admiten filtros de autenticación, pero se diferencian ligeramente..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/25/2014
ms.topic: article
ms.assetid: b9882e53-b3ca-4def-89b0-322846973ccb
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/authentication-filters
msc.type: authoredcontent
ms.openlocfilehash: 7c704cc351876b49ec143a49b25cc0ca83876e06
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="authentication-filters-in-aspnet-web-api-2"></a>Filtros de autenticación en ASP.NET Web API 2
====================
por [Mike Wasson](https://github.com/MikeWasson)

> Un filtro de autenticación es un componente que autentica una solicitud HTTP. API Web 2 y MVC 5 admiten filtros de autenticación, pero se diferencian ligeramente, principalmente en las convenciones de nomenclatura para la interfaz de filtro. Este tema describen los filtros de autenticación Web API.


Filtros de autenticación le permiten establecer un esquema de autenticación de controladores individuales o acciones. De este modo, la aplicación puede admitir mecanismos de autenticación diferentes para los distintos recursos HTTP.

En este artículo, voy a explicar el código de la [la autenticación básica](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/BasicAuthentication/ReadMe.txt) ejemplo en [http://aspnet.codeplex.com](http://aspnet.codeplex.com). El ejemplo muestra un filtro de autenticación que implementa el esquema de autenticación de acceso básica de HTTP (RFC 2617). El filtro se implementa en una clase denominada `IdentityBasicAuthenticationAttribute`. No todo el código del ejemplo, mostrar sólo las partes que muestran cómo crear un filtro de autenticación.

## <a name="setting-an-authentication-filter"></a>Establecer un filtro de autenticación

Al igual que otros filtros, filtros de autenticación pueden ser aplicada por controlador, por cada acción o globalmente a todos los controladores de API Web.

Para aplicar un filtro de autenticación a un controlador, decore la clase de controlador con el atributo de filtro. El código siguiente establece el `[IdentityBasicAuthentication]` filtro en una clase de controlador, lo que permite la autenticación básica para todas las acciones del controlador.

[!code-csharp[Main](authentication-filters/samples/sample1.cs)]

Para aplicar el filtro a una acción, decorar la acción con el filtro. El código siguiente establece el `[IdentityBasicAuthentication]` filtro en el controlador `Post` método.

[!code-csharp[Main](authentication-filters/samples/sample2.cs)]

Para aplicar el filtro a todos los controladores de API Web, agréguelo a **GlobalConfiguration.Filters**.

[!code-csharp[Main](authentication-filters/samples/sample3.cs)]

## <a name="implementing-a-web-api-authentication-filter"></a>Implementar un filtro de autenticación de Web API

En la API de Web, los filtros de autenticación implementan la [System.Web.Http.Filters.IAuthenticationFilter](https://msdn.microsoft.com/library/system.web.http.filters.iauthenticationfilter.aspx) interfaz. También debe heredar de **System.Attribute**, para que se apliquen como atributos.

El **IAuthenticationFilter** interfaz tiene dos métodos:

- **AuthenticateAsync** autentica la solicitud mediante la validación de credenciales en la solicitud, si está presente.
- **ChallengeAsync** agrega un desafío de autenticación a la respuesta HTTP, si es necesario.

Estos métodos se corresponden con el flujo de autenticación definido en [RFC 2612](http://tools.ietf.org/html/rfc2616) y [RFC 2617](http://tools.ietf.org/html/rfc2617):

1. El cliente envía las credenciales en el encabezado de autorización. Esto suele suceder después de que el cliente recibe una respuesta 401 (no autorizado) desde el servidor. Sin embargo, un cliente puede enviar credenciales con cualquier solicitud, no solo después de obtener 401.
2. Si el servidor no aceptó las credenciales, devuelve una respuesta 401 (no autorizado). La respuesta incluye un encabezado Www-Authenticate que contiene uno o varios desafíos. Cada desafío especifica un esquema de autenticación reconocido por el servidor.

El servidor puede devolver 401 de una solicitud anónima. De hecho, es normalmente cómo se inicia el proceso de autenticación:

1. El cliente envía una solicitud anónima.
2. El servidor devuelve 401.
3. Los clientes vuelve a enviar la solicitud con credenciales.

Este flujo incluye ambos *autenticación* y *autorización* pasos.

- Autenticación demuestra la identidad del cliente.
- La autorización determina si el cliente puede tener acceso a un recurso determinado.

En la API de Web, filtros de autenticación controlan la autenticación, pero no la autorización. Autorización debe realizarse mediante un filtro de autorización o dentro de la acción del controlador.

Este es el flujo de la canalización de Web API 2:

1. Antes de invocar una acción, API Web crea una lista de los filtros de autenticación para esa acción. Esto incluye filtros con ámbito de acción, el ámbito de controlador y el ámbito global.
2. Llamadas a la API de Web **AuthenticateAsync** en todos los filtros de la lista. Cada filtro puede validar las credenciales en la solicitud. Si cualquier filtro que valida correctamente las credenciales, el filtro se crea un **IPrincipal** y lo adjunta a la solicitud. Un filtro también puede desencadenar un error en este momento. Si es así, el resto de la canalización no se ejecuta.
3. Suponiendo que no hay ningún error, la solicitud fluye a través del resto de la canalización.
4. Por último, llama a cada filtro de autenticación Web API **ChallengeAsync** método. Filtros de utilizar este método para agregar un desafío a la respuesta, si es necesario. Normalmente (aunque no siempre) que se podría producir en respuesta a un error 401.

Los siguientes diagramas muestran dos casos posibles. En la primera, el filtro de autenticación autentica correctamente la solicitud, un filtro de autorización que autoriza la solicitud y la acción del controlador devuelve 200 (OK).

![](authentication-filters/_static/image1.png)

En el segundo ejemplo, el filtro de autenticación autentica la solicitud, pero el filtro de autorización devuelve 401 (no autorizado). En este caso, no se invoca la acción del controlador. El filtro de autenticación agrega un encabezado Www-Authenticate para la respuesta.

![](authentication-filters/_static/image2.png)

Son posibles otras combinaciones&mdash;por ejemplo, si la acción del controlador permite las solicitudes anónimas, podría tener un filtro de autenticación, pero no tiene autorización.

## <a name="implementing-the-authenticateasync-method"></a>Implementar el método AuthenticateAsync

El **AuthenticateAsync** método intenta autenticar la solicitud. Aquí se muestra la signatura de método:

[!code-csharp[Main](authentication-filters/samples/sample4.cs)]

El **AuthenticateAsync** método debe realizar una de las siguientes acciones:

1. No hay nada (no-op).
2. Crear un **IPrincipal** y establézcalo en la solicitud.
3. Establezca un resultado de error.

Opción (1) significa que la solicitud no tenía las credenciales que entienda el filtro. Significa que la opción (2) el filtro autentica correctamente la solicitud. Opción (3) significa la solicitud tenía las credenciales no válidas (por ejemplo, una contraseña incorrecta), lo que desencadena una respuesta de error.

Este es un esquema general para implementar **AuthenticateAsync**.

1. Busque las credenciales en la solicitud.
2. Si no hay ninguna credencial, no haga nada y devolver (no-op).
3. Si hay credenciales, pero el filtro no reconoce el esquema de autenticación, no haga nada y devolver (no-op). Otro filtro en la canalización podría entender que el esquema.
4. Si no hay credenciales que entienda el filtro, intente realizar la autenticación de ellos.
5. Si las credenciales son incorrectas, se devuelve 401 estableciendo `context.ErrorResult`.
6. Si las credenciales son válidas, cree un **IPrincipal** y establecer `context.Principal`.

El siguiente código muestra la **AuthenticateAsync** método desde el [la autenticación básica](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/BasicAuthentication/ReadMe.txt) ejemplo. Cada paso indican los comentarios. El código muestra varios tipos de error: encabezado de una autorización no tiene las credenciales, las credenciales con formato incorrecto y nombre de usuario/contraseña incorrecta.

[!code-csharp[Main](authentication-filters/samples/sample5.cs)]

## <a name="setting-an-error-result"></a>Establecer un resultado de Error

Si las credenciales son válidas, debe establecer el filtro `context.ErrorResult` a una **IHttpActionResult** que crea una respuesta de error. Para obtener más información acerca de **IHttpActionResult**, consulte [resultados de la acción en API Web 2](../getting-started-with-aspnet-web-api/action-results.md).

El ejemplo de la autenticación básica se incluye un `AuthenticationFailureResult` clase que es adecuado para este propósito.

[!code-csharp[Main](authentication-filters/samples/sample6.cs)]

## <a name="implementing-challengeasync"></a>Implementar ChallengeAsync

El propósito de la **ChallengeAsync** método consiste en Agregar desafíos de autenticación a la respuesta, si es necesario. Aquí se muestra la signatura de método:

[!code-csharp[Main](authentication-filters/samples/sample7.cs)]

Se llama al método en cada filtro de autenticación en la canalización de solicitudes.

Es importante entender que **ChallengeAsync** se denomina *antes de* la respuesta HTTP se haya creado y posiblemente incluso antes de que se ejecuta la acción de controlador. Cuando **ChallengeAsync** se llama, `context.Result` contiene un **IHttpActionResult**, que se utiliza posteriormente para crear la respuesta HTTP. Por lo que cuando **ChallengeAsync** es llama, no sabe nada acerca de la respuesta HTTP todavía. El **ChallengeAsync** método debe reemplazar el valor original de `context.Result` con un nuevo **IHttpActionResult**. Esto **IHttpActionResult** debe contener el original `context.Result`.

![](authentication-filters/_static/image3.png)

A la que llamaré original **IHttpActionResult** el *resultados interno*y a los nuevos **IHttpActionResult** el *resultado externo*. El resultado externo debe hacer lo siguiente:

1. Invoque el resultado interno para crear la respuesta HTTP.
2. Examine la respuesta.
3. Agregue un desafío de autenticación a la respuesta, si es necesario.

En el siguiente ejemplo se toma del ejemplo de la autenticación básica. Define un **IHttpActionResult** para el resultado externo.

[!code-csharp[Main](authentication-filters/samples/sample8.cs)]

El `InnerResult` propiedad contiene interna **IHttpActionResult**. El `Challenge` propiedad representa un encabezado de autenticación Www. Tenga en cuenta que **ExecuteAsync** llama primero `InnerResult.ExecuteAsync` para crear la respuesta HTTP y, a continuación, agrega el desafío si es necesario.

Compruebe el código de respuesta antes de agregar el desafío. La mayoría de los esquemas de autenticación solo agregar un desafío si la respuesta es 401, como se muestra aquí. Sin embargo, algunos esquemas de autenticación agregar un desafío para una respuesta correcta. Por ejemplo, vea [Negotiate](http://tools.ietf.org/html/rfc4559#section-5) (RFC 4559).

Dada la `AddChallengeOnUnauthorizedResult` de la clase, el código real en **ChallengeAsync** es sencillo. Que acaba de crear el resultado y asóciela al `context.Result`.

[!code-csharp[Main](authentication-filters/samples/sample9.cs)]

Nota: El ejemplo de la autenticación básica abstrae un poco, esta lógica mediante la colocación en un método de extensión.

## <a name="combining-authentication-filters-with-host-level-authentication"></a>Combinación de filtros de autenticación con autenticación a nivel de Host

"Autenticación de nivel de host" es la autenticación realizada por el host (como IIS), antes el marco de trabajo de solicitud llega a la API Web.

A menudo, puede que desee para habilitar la autenticación de nivel de host para el resto de la aplicación, pero deshabilitar para los controladores de API Web. Por ejemplo, un escenario típico es habilitar la autenticación de formularios en el nivel de host, pero usa autenticación basada en el símbolo (token) de API Web.

Para deshabilitar la autenticación de nivel de host dentro de la canalización Web API, llamar a `config.SuppressHostPrincipal()` en la configuración. Esto hace que la API Web quitar la **IPrincipal** desde cualquier solicitud que entra en la canalización Web API. De hecho, se &quot;anular-autentica&quot; la solicitud.

[!code-csharp[Main](authentication-filters/samples/sample10.cs)]

## <a name="additional-resources"></a>Recursos adicionales

[Los filtros de seguridad de ASP.NET Web API](https://msdn.microsoft.com/magazine/dn781361.aspx) (MSDN Magazine)
