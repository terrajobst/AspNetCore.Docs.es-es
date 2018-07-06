---
uid: web-api/overview/security/authentication-and-authorization-in-aspnet-web-api
title: Autenticación y autorización en ASP.NET Web API | Microsoft Docs
author: MikeWasson
description: Se proporciona información general de autenticación y autorización en ASP.NET Web API.
ms.author: aspnetcontent
ms.date: 11/27/2012
ms.assetid: 6dfb51ea-9f4d-4e70-916c-8ef8344a88d6
msc.legacyurl: /web-api/overview/security/authentication-and-authorization-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: f882a9bb2922d0eb28eee7cdba0896bdc5318c2a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37812839"
---
<a name="authentication-and-authorization-in-aspnet-web-api"></a>Autenticación y autorización en ASP.NET Web API
====================
por [Mike Wasson](https://github.com/MikeWasson)

Ha creado una API web, pero ahora desea controlar el acceso a él. En esta serie de artículos, daremos un vistazo a algunas opciones para proteger una API web de los usuarios no autorizados. Esta serie tratará de autenticación y autorización.

- *Autenticación* es conocer la identidad del usuario. Por ejemplo, Alice inicia sesión con su nombre de usuario y contraseña, y el servidor utiliza la contraseña para autenticar a Alice.
- *Autorización* consiste en decidir si un usuario puede realizar una acción. Por ejemplo, Alice dispone del permiso para obtener un recurso pero no crear un recurso.

El primer artículo de la serie ofrece una visión general de autenticación y autorización en ASP.NET Web API. Otros temas describen escenarios comunes de autenticación para API Web.

> [!NOTE]
> Gracias a las personas que revisan esta serie y proporcionaron sus valiosos comentarios: Rick Anderson, Levi Broderick, Barry Dorrans, Tom Dykstra, Hongmei Ge, David Matson, Daniel Roth, Tim Teebken.


## <a name="authentication"></a>Autenticación

API Web se da por supuesto que la autenticación se produce en el host. Para el hospedaje en web, el host es IIS, que usa los módulos HTTP para la autenticación. Puede configurar el proyecto para usar cualquiera de los módulos de autenticación integrados en IIS o ASP.NET, o escribir su propio módulo HTTP para realizar la autenticación personalizada.

Cuando el host las autentica el usuario, se crea un *principal*, que es un [IPrincipal](https://msdn.microsoft.com/library/System.Security.Principal.IPrincipal.aspx) objeto que representa el contexto de seguridad en que se ejecuta el código. El host adjunta la entidad de seguridad para el subproceso actual estableciendo **Thread.CurrentPrincipal**. La entidad de seguridad contiene un asociado **identidad** objeto que contiene información sobre el usuario. Si el usuario está autenticado, el **Identity.IsAuthenticated** propiedad devuelve **true**. Para las solicitudes anónimas, **IsAuthenticated** devuelve **false**. Para obtener más información acerca de las entidades, consulte [Role-Based Security](https://msdn.microsoft.com/library/shz8h065.aspx).

### <a name="http-message-handlers-for-authentication"></a>Controladores de mensajes HTTP para la autenticación

En lugar de usar el host para la autenticación, puede colocar lógica de autenticación en un [controlador de mensajes HTTP](../advanced/http-message-handlers.md). En ese caso, el controlador de mensajes examina la solicitud HTTP y establece la entidad de seguridad.

¿Cuándo deben usar controladores de mensajes para la autenticación? Estos son algunos inconvenientes:

- Un módulo HTTP ve todas las solicitudes que pasan por la canalización de ASP.NET. Un controlador de mensajes solo ve las solicitudes que se enrutan a la API Web.
- Puede establecer controladores de mensajes por ruta, lo que le permite aplicar un esquema de autenticación a una ruta específica.
- Los módulos HTTP son específicos de IIS. Controladores de mensajes son independientes de host para que se puedan usar con el hospedaje web y autoalojamiento.
- Los módulos HTTP participan en el registro de IIS, auditoría y así sucesivamente.
- Los módulos HTTP que se ejecutan anteriormente en la canalización. Si administra la autenticación en un controlador de mensajes, la entidad de seguridad no obtener o establecer hasta que se ejecuta el controlador. Además, la entidad de seguridad se vuelve a la entidad de seguridad anterior cuando sale de la respuesta el controlador de mensajes.

Por lo general, si no tiene que admiten autohospedaje, un módulo HTTP es una opción mejor. Si tiene que admiten autohospedaje, considere la posibilidad de un controlador de mensajes.

### <a name="setting-the-principal"></a>Establecer la entidad de seguridad

Si la aplicación realiza cualquier lógica de autenticación personalizada, debe establecer la entidad de seguridad en dos lugares:

- **Thread.CurrentPrincipal**. Esta propiedad es la manera estándar para establecer la entidad de seguridad del subproceso en. NET.
- **HttpContext.Current.User**. Esta propiedad es específica de ASP.NET.

El código siguiente muestra cómo establecer la entidad de seguridad:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample1.cs)]

Para el hospedaje en web, debe establecer la entidad de seguridad en ambos lugares; en caso contrario, el contexto de seguridad puede volverse incoherente. Para el autohospedaje, sin embargo, **HttpContext.Current** es null. Para asegurarse de que el código es independiente del host, por lo tanto, comprobar valores null antes de asignar a **HttpContext.Current**, como se muestra.

## <a name="authorization"></a>Autorización

Tiene lugar la autorización más adelante en la canalización, más cercana al controlador. Que le permite tomar decisiones más granulares cuando usted otorga acceso a los recursos.

- *Los filtros de autorización* ejecutar antes de la acción del controlador. Si la solicitud no está autorizada, el filtro devuelve una respuesta de error y no se invoca la acción.
- Dentro de una acción de controlador, puede obtener la entidad de seguridad actual de la **ApiController.User** propiedad. Por ejemplo, puede filtrar una lista de recursos según el nombre de usuario, devuelve solo aquellos recursos que pertenecen a ese usuario.

![](authentication-and-authorization-in-aspnet-web-api/_static/image1.png)

<a id="auth3"></a>
### <a name="using-the-authorize-attribute"></a>Mediante la [autorizar] atributo

API Web proporciona un filtro de autorización integrado, [AuthorizeAttribute](https://msdn.microsoft.com/library/system.web.http.authorizeattribute.aspx). Este filtro comprueba si el usuario está autenticado. Si no, devuelve código de estado HTTP 401 (no autorizado), sin invocar la acción.

Puede aplicar el filtro globalmente, en el nivel de controlador, o en el nivel de acciones individuales.

**Globalmente**: para restringir el acceso para cada controlador Web API, agregue el **AuthorizeAttribute** filtro a la lista de filtros globales:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample2.cs)]

**Controlador**: para restringir el acceso de un controlador concreto, agregue el filtro como un atributo al controlador:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample3.cs)]

**Acción**: para restringir el acceso a acciones específicas, agregue el atributo al método de acción:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample4.cs)]

Como alternativa, puede restringir el controlador y, a continuación, permitir acceso anónimo a acciones específicas, mediante el `[AllowAnonymous]` atributo. En el ejemplo siguiente, la `Post` método está restringido, pero el `Get` método permite el acceso anónimo.

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample5.cs)]

En los ejemplos anteriores, el filtro permite que cualquier usuario autenticado tener acceso a los métodos restringidos; solo los usuarios anónimos se mantienen. También puede limitar el acceso a usuarios específicos o a los usuarios con roles específicos:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample6.cs)]

> [!NOTE]
> El **AuthorizeAttribute** filtro para los controladores Web API se encuentra en la **System.Web.Http** espacio de nombres. Hay un filtro similar para los controladores MVC en el **System.Web.Mvc** espacio de nombres, que no es compatible con controladores de API Web.


### <a name="custom-authorization-filters"></a>Filtros de autorización personalizada

Para escribir un filtro de autorización personalizado, derive de uno de estos tipos:

- **AuthorizeAttribute**. Extienda esta clase para llevar a cabo una lógica de autorización según el usuario actual y los roles del usuario.
- **AuthorizationFilterAttribute**. Extienda esta clase para llevar a cabo una lógica de autorización sincrónica que no es necesariamente basada en el usuario actual o el rol.
- **IAuthorizationFilter**. Implemente esta interfaz para llevar a cabo una lógica de autorización asincrónica; Por ejemplo, si la lógica de autorización realiza llamadas asincrónicas de E/S o de red. (Si la lógica de autorización está enlazado a la CPU, resulta más sencillo que derivar **AuthorizationFilterAttribute**, ya que es necesario escribir un método asincrónico.)

El siguiente diagrama muestra la jerarquía de clases para el **AuthorizeAttribute** clase.

![](authentication-and-authorization-in-aspnet-web-api/_static/image2.png)

### <a name="authorization-inside-a-controller-action"></a>Autorización dentro de una acción de controlador

En algunos casos, puede permitir que una solicitud para continuar, pero cambiar el comportamiento en función de la entidad de seguridad. Por ejemplo, puede cambiar la información que devuelve según el rol del usuario. Dentro de un método de controlador, puede obtener desde el principio de actual la **ApiController.User** propiedad.

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample7.cs)]
