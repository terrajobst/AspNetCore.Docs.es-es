---
uid: web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
title: Enrutamiento en ASP.NET Web API | Documentos de Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/11/2012
ms.topic: article
ms.assetid: 0675bdc7-282f-4f47-b7f3-7e02133940ca
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: aa0ecc96029051fef6a81ac08f7fcf52a24de59c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="routing-in-aspnet-web-api"></a>Enrutamiento en ASP.NET Web API
====================
por [Mike Wasson](https://github.com/MikeWasson)

Este artículo describe cómo ASP.NET Web API enruta las solicitudes HTTP a los controladores.

> [!NOTE]
> Si está familiarizado con ASP.NET MVC, Web API enrutamiento es muy similar para el enrutamiento de MVC. La diferencia principal es que las API de Web usa el método HTTP, no la ruta de acceso URI, para seleccionar la acción. También puede utilizar el enrutamiento basado en MVC en Web API. Este artículo no supone ningún conocimiento de ASP.NET MVC.


## <a name="routing-tables"></a>Tablas de enrutamiento

En ASP.NET Web API, un *controlador* es una clase que controla las solicitudes HTTP. Se llaman a los métodos públicos del controlador de *métodos de acción* o simplemente *acciones*. Cuando el marco Web API recibe una solicitud, enruta la solicitud a una acción.

Para determinar qué acción va a invocar, el marco de trabajo usa un *tabla de enrutamiento*. La plantilla de proyecto de Visual Studio para la API Web crea una ruta predeterminada:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample1.cs)]

Esta ruta se define en el archivo WebApiConfig.cs, que se coloca en la aplicación\_directorio de inicio:

![](routing-in-aspnet-web-api/_static/image1.png)

Para obtener más información sobre la **WebApiConfig** de clases, consulte [configurar ASP.NET Web API](../advanced/configuring-aspnet-web-api.md).

Si autohospedaje API Web, debe establecer la tabla de enrutamiento directamente en el **HttpSelfHostConfiguration** objeto. Para obtener más información, consulte [autohospedaje una API Web](../older-versions/self-host-a-web-api.md).

Cada entrada de la tabla de enrutamiento contiene un *plantilla de ruta*. La plantilla de ruta predeterminada de Web API es &quot;api / {controller} / {id}&quot;. En esta plantilla, &quot;api&quot; es un segmento de ruta de acceso literal y {controller} y {id} son variables de marcador de posición.

Cuando el marco Web API recibe una solicitud HTTP, intenta hacer coincidir el URI con una de las plantillas de ruta en la tabla de enrutamiento. Si coincide con ninguna ruta, el cliente recibe un error 404. Por ejemplo, el URI siguiente coincide con la ruta predeterminada:

- / api/contactos
- /API/Contacts/1
- /API/Products/gizmo1

Sin embargo, el siguiente URI no coincide, porque carece del &quot;api&quot; segmento:

- / contactos/1

> [!NOTE]
> La razón para usar "api" en la ruta es evitar conflictos con el enrutamiento de ASP.NET MVC. De este modo, puede tener &quot;/pone en contacto con&quot; vaya a un controlador MVC, y &quot;/api/contactos&quot; vaya a un controlador de Web API. Por supuesto, si no le gusta esta convención, puede cambiar la tabla de rutas predeterminadas.

Una vez que se encuentra una ruta coincidente, API Web selecciona la acción y el controlador:

- Para buscar el controlador, se agrega API Web &quot;controlador&quot; en el valor de la *{controller}* variable.
- Para encontrar la acción, Web API examina el método HTTP y, a continuación, busca una acción cuyo nombre comienza con ese nombre de método HTTP. Por ejemplo, con una solicitud GET, API Web busca una acción que comienza con &quot;obtener... &quot;, como &quot;GetContact&quot; o &quot;GetAllContacts&quot;. Esta convención se aplica solo a GET, POST, PUT y DELETE métodos. Puede habilitar otros métodos HTTP mediante el uso de atributos en el controlador. Veremos un ejemplo de esto más adelante.
- Otras variables de marcador de posición en la plantilla de ruta, como *{id},* se asignan a parámetros de acción.

Veamos un ejemplo. Supongamos que define el siguiente controlador:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample2.cs)]

Estas son algunas posibles solicitudes HTTP, junto con la acción que se invoca para cada uno:

| Método HTTP | Ruta de acceso URI | Acción | Parámetro |
| --- | --- | --- | --- |
| GET | API y productos | GetAllProducts | *(ninguno)* |
| GET | productos/API/4 | GetProductById | 4 |
| SUPRIMIR | productos/API/4 | DeleteProduct | 4 |
| EXPONER | API y productos | *(ninguna coincidencia)* |  |

Tenga en cuenta que la *{id}* segmento del URI, si está presente, se asigna a la *identificador* parámetro de la acción. En este ejemplo, el controlador define dos métodos GET, uno con un *identificador* parámetro y otra sin parámetros.

Además, tenga en cuenta que la solicitud POST producirá un error, porque el controlador no define una &quot;Post... &quot; (método).

## <a name="routing-variations"></a>Variaciones de enrutamientos

La sección anterior describe el mecanismo básico de enrutamiento de ASP.NET Web API. Esta sección describe algunas variaciones.

### <a name="http-methods"></a>Métodos HTTP

En lugar de utilizar la convención de nomenclatura para métodos HTTP, puede especificar explícitamente el método HTTP para una acción decorando el método de acción con el **HttpGet**, **HttpPut**, **HttpPost** , o **HttpDelete** atributo.

En el ejemplo siguiente, el método FindProduct se asigna a las solicitudes GET:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample3.cs)]

Para permitir que varios métodos HTTP para una acción, o para permitir que los métodos HTTP distinto de GET, PUT, POST y DELETE, use la **AcceptVerbs** atributo, que toma una lista de métodos HTTP.

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample4.cs)]

<a id="routing_by_action_name"></a>
### <a name="routing-by-action-name"></a>Enrutamiento por nombre de acción

Con la plantilla de enrutamiento de forma predeterminada, la API Web usa el método HTTP para seleccionar la acción. Sin embargo, también puede crear una ruta donde se incluye el nombre de acción en el URI:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample5.cs)]

En esta plantilla de ruta, el *{action}* el método de acción en el controlador de nombres de parámetro. Con este estilo de enrutamiento, utilice atributos para especificar los métodos HTTP permitidos. Por ejemplo, suponga que el controlador tiene el siguiente método:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample6.cs)]

En este caso, una solicitud GET de "api/productos/detalles/1" asignaría al método de detalles. Este estilo de enrutamiento es similar a ASP.NET MVC y puede ser adecuado para una API de estilo RPC.

Puede reemplazar el nombre de acción mediante el **ActionName** atributo. En el ejemplo siguiente, hay dos acciones que se asignan a &quot;api/productos/miniatura/*identificador*. Admite una de ellas GET y el otro admite POST:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample7.cs)]

### <a name="non-actions"></a>Sin acciones

Para evitar que un método invocando como una acción, use la **NonAction** atributo. Esto indica al marco que el método no es una acción, aunque en caso contrario, coincidiría con las reglas de enrutamiento.

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample8.cs)]

## <a name="further-reading"></a>Información adicional

Este tema proporciona una visión general de enrutamiento. Para obtener información más detallada, vea [enrutamiento y selección de acción](routing-and-action-selection.md), que describen exactamente cómo el marco de trabajo coincide con un URI para una ruta, selecciona un controlador y, a continuación, selecciona la acción que se va a invocar.
