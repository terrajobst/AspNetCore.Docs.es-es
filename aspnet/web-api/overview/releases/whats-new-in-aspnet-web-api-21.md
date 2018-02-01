---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-21
title: "' S New en ASP.NET Web API 2.1 | Documentos de Microsoft"
author: microsoft
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: b6721bba-38c8-48c4-acbf-274c1a34e817
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: cc5dc111d88cc7dae6a4a93203317fa0769d5427
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="whats-new-in-aspnet-web-api-21"></a>' S New en ASP.NET Web API 2.1
====================
por [Microsoft](https://github.com/microsoft)

Este tema describe lo que es nuevo en ASP.NET Web API 2.1.

- [Descarga](#download)
- [Documentación](#documentation)
- [Nuevas características de ASP.NET Web API 2.1](#new-features)

    - [Control de errores global](#global-error)
    - [Atributo mejoras de enrutamiento](#attribute-routing)
    - [Mejoras de la página de ayuda](#help-page)
    - [Compatibilidad con IgnoreRoute](#ignoreroute)
    - [Formateador de tipo de medio BSON](#bson)
    - [Una mejor compatibilidad con filtros de Async](#async-filters)
    - [Consulta de análisis para el cliente de biblioteca de formato](#query-parsing)
- [Problemas conocidos y los cambios recientes](#known-issues)
- [Correcciones de errores](#bug-fixes)

<a id="download"></a>
## <a name="download"></a>Descargar

Las características en tiempo de ejecución se publican como paquetes de NuGet en la Galería de NuGet. Siguen todos los paquetes en tiempo de ejecución el [control de versiones semántico](http://semver.org/) especificación. El paquete de RTM de ASP.NET Web API 2.1 más reciente tiene la siguiente versión: "5.1.2". Puede instalar o actualizar estos paquetes a través de [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/). La versión también incluye paquetes localizados correspondientes en NuGet.

Puede instalar o actualizar a los paquetes de NuGet publicados mediante la consola de administrador de paquetes de NuGet:

[!code-console[Main](whats-new-in-aspnet-web-api-21/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>Documentación

Tutoriales y otra información sobre ASP.NET Web API 2.1 RTM están disponibles desde el sitio web ASP.NET ([https://www.asp.net/web-api](../../index.md)).

<a id="new-features"></a>
## <a name="new-features-in-aspnet-web-api-21"></a>Nuevas características de ASP.NET Web API 2.1

<a id="global-error"></a>
### <a name="global-error-handling"></a>Control de errores global

Ahora se pueden registrar todas las excepciones no controladas a través de un mecanismo central, y se puede personalizar el comportamiento de las excepciones no controladas.

El marco de trabajo admite varios registradores de excepciones, que se vea la excepción no controlada y la información sobre el contexto en el que se produjo, por ejemplo, la solicitud que se está procesando en el momento.

Por ejemplo, el código siguiente usa System.Diagnostics.TraceSource para registrar todas las excepciones no controladas:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample2.cs)]

También puede reemplazar el controlador de excepciones de manera predeterminada, por lo que puede personalizar completamente el mensaje de respuesta HTTP que se envía cuando una excepción no controlada se produce.

Hemos preparado una [ejemplo](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/Elmah/ReadMe.txt) que registra todas las no controladas excepciones a través del marco ELMAH popular.

<a id="attribute-routing"></a>
### <a name="attribute-routing-improvements"></a>Atributo mejoras de enrutamiento

Ruta de atributo ahora es compatible con las restricciones, habilitar el control de versiones y la selección de rutas basadas en el encabezado. Además, muchos aspectos de las rutas de atributo ahora son personalizables a través de la **IDirectRouteFactory** interfaz y **RouteFactoryAttribute** clase. El prefijo de ruta ahora es extensible a través de la **IRoutePrefix** interfaz y **RoutePrefixAttribute** clase.

Hemos preparado una [ejemplo](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/RoutingConstraintsSample/ReadMe.txt) que utiliza las restricciones para filtrar dinámicamente los controladores por un encabezado HTTP 'api-version'.

<a id="help-page"></a>
### <a name="help-page-improvements"></a>Mejoras de la página de ayuda

Web API 2.1 incluye las siguientes mejoras para [páginas de Ayuda de API](../getting-started-with-aspnet-web-api/creating-api-help-pages.md):

- Documentación de las propiedades individuales de los parámetros o tipos de valor devuelto de acciones.
- Documentación de anotaciones del modelo de datos.

También se actualizó el diseño de interfaz de usuario de las páginas de ayuda, para dar cabida a estos cambios.

<a id="ignoreroute"></a>
### <a name="ignoreroute-support"></a>Compatibilidad con IgnoreRoute

Web API 2.1 es compatible con omitiendo los modelos de dirección URL en el enrutamiento de API Web a través de un conjunto de **IgnoreRoute** métodos de extensión en **HttpRouteCollection**. Estos métodos generan Web API pasar por alto las direcciones URL que coinciden con una plantilla especificada y permitir que el host aplicar un procesamiento adicional si es necesario.

En el ejemplo siguiente, se omite el URI que se inician con un &quot;contenido&quot; segmento:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample3.cs)]

<a id="bson"></a>
### <a name="bson-media-type-formatter"></a>Formateador de tipo de medio BSON

Web API ahora admite la [BSON](http://bsonspec.org/) formato, en el cliente y en el servidor.

Para habilitar BSON en el servidor, agregue el **BsonMediaTypeFormatter** a la colección de formateadores:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample4.cs)]

Le mostramos cómo un cliente .NET puede consumir formato BSON:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample5.cs)]

Hemos preparado una [ejemplo](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/ReadMe.txt) que muestra el cliente y el servidor.

Para obtener más información, vea [soporte BSON en Web API 2.1](../formats-and-model-binding/bson-support-in-web-api-21.md)

<a id="async-filters"></a>
### <a name="better-support-for-async-filters"></a>Una mejor compatibilidad con filtros de Async

Web API ahora es compatible con una forma sencilla de crear filtros que se ejecutan de forma asincrónica. Esta característica es útil es el filtro que se necesita para realizar una acción asincrónica, como el acceso a una base de datos. Anteriormente, para crear un filtro de async, se tenían que implementar la interfaz de filtro usted mismo, ya que las clases de base de filtro solo exponen métodos sincrónicos. Ahora puede invalidar el virtual `On*Async` métodos del filtro de clase base.

Por ejemplo:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample6.cs)]

El **AuthorizationFilterAttribute**, **ActionFilterAttribute**, y **ExceptionFilterAttribute** todas clases admiten asincrónico en Web API 2.1.

<a id="query-parsing"></a>
### <a name="query-parsing-for-the-client-formatting-library"></a>Consulta de análisis para el cliente de biblioteca de formato

Anteriormente, **System.Net.Http.Formatting** admiten análisis y actualizar las consultas URI para el código de servidor, pero la biblioteca portable equivalente faltaba esta característica. En la Web API 2.1, una aplicación cliente puede ahora fácilmente analizar y actualizar una cadena de consulta.

Los ejemplos siguientes muestran cómo analizar, modificar y generar las consultas URI. (Los ejemplos muestran una aplicación de consola para simplificar el trabajo).

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample7.cs)]

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a>Problemas conocidos y los cambios recientes

Esta sección describen problemas conocidos y cambios importantes en la RTM de ASP.NET Web API 2.1.

### <a name="attribute-routing"></a>Ruta de atributo

Ambigüedades en coincidencias de enrutamiento de atributo ahora notifican un error en lugar de elegir a la primera coincidencia.

Rutas de atributo se les prohíbe usar la *{controller}* parámetro y del uso de la *{action}* parámetro en las rutas se coloca en las acciones. Estos parámetros es muy probable que hará que las ambigüedades.

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a>Scaffolding MVC o Web API en un proyecto con 5.1 resultados de paquetes en 5.0 paquetes para los que ya no existen en el proyecto

Actualizar paquetes de NuGet para ASP.NET Web API 2.1 RTM no actualizar las herramientas de Visual Studio, como la técnica scaffolding de ASP.NET o la plantilla de proyecto de aplicación Web ASP.NET. Usan la versión anterior de los paquetes en tiempo de ejecución ASP.NET (5.0.0.0). Como resultado, el scaffolding de ASP.NET se instalará la versión anterior (5.0.0.0) de los paquetes necesarios, si aún no están disponibles en los proyectos. Sin embargo, el scaffolding de ASP.NET en Visual Studio 2013 RTM o Update 1 sobrescribir los paquetes más recientes de los proyectos.

Si utiliza ASP.NET scaffolding después de actualizar los paquetes de Web API 2.1 o ASP.NET MVC 5.1, asegúrese de que las versiones de API Web y MVC son coherentes.

### <a name="type-renames"></a>Cambia el nombre de tipo

Algunos de los tipos utilizados para la extensibilidad de enrutamiento de atributo se cambió el nombre de la versión RC a RTM 2.1.

| Nombre de tipo anterior (2.1 RC) | Nuevo tipo de nombre (RTM 2.1) |
| --- | --- |
| IDirectRouteProvider | IDirectRouteFactory |
| RouteProviderAttribute | RouteFactoryAttribute |
| DirectRouteProviderContext | DirectRouteFactoryContext |

### <a name="exception-filters-do-not-unwrap-aggregate-exceptions-thrown-in-async-actions"></a>Los filtros de excepción desencapsular agregadas excepciones en las acciones de asincrónico

Anteriormente, si una acción asincrónica produjo una **AggregateException**, un filtro de excepción podría unwrap la excepción, y **OnException** obtendría la excepción base. En 2.1, el filtro de excepción desencapsular, y **OnException** Obtiene la versión original **AggregateException**.

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a>Correcciones de errores

Esta versión también incluye varias correcciones de errores. Puede encontrar la lista completa aquí:

- [5.1.0 paquete](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=Web%20API|Web%20API%20OData&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [5.1.1 paquete de](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.1.1%20RTM&assignedTo=All&component=Web%20API&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

El 5.1.2 paquete contiene actualizaciones de IntelliSense, pero no hay correcciones de errores.
