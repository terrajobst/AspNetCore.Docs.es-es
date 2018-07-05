---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-21
title: Novedades de ASP.NET Web API 2.1 | Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: b6721bba-38c8-48c4-acbf-274c1a34e817
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: 8e0501570e6dc6a9a6f69a642f9ab031c5497b5b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37385705"
---
<a name="whats-new-in-aspnet-web-api-21"></a>Novedades de ASP.NET Web API 2.1
====================
por [Microsoft](https://github.com/microsoft)

Este tema describe cuáles son las novedades para ASP.NET Web API 2.1.

- [Descarga](#download)
- [Documentación](#documentation)
- [Nuevas características en ASP.NET Web API 2.1](#new-features)

    - [Control de errores global](#global-error)
    - [Mejoras de enrutamiento de atributo](#attribute-routing)
    - [Mejoras en la página de ayuda](#help-page)
    - [Compatibilidad con IgnoreRoute](#ignoreroute)
    - [Formateador de tipo de medio BSON](#bson)
    - [Mejor compatibilidad para los filtros de asincronía](#async-filters)
    - [Consulta de análisis para el cliente de biblioteca de formato](#query-parsing)
- [Problemas conocidos y cambios importantes](#known-issues)
- [Correcciones de errores](#bug-fixes)

<a id="download"></a>
## <a name="download"></a>Descargar

Las características en tiempo de ejecución se publican como paquetes de NuGet en la Galería de NuGet. Siguen todos los paquetes en tiempo de ejecución el [Versionamiento semántico](http://semver.org/) especificación. El paquete de RTM de ASP.NET Web API 2.1 más reciente tiene la siguiente versión: "5.1.2". Puede instalar o actualizar estos paquetes a través de [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/). La versión también incluye los correspondientes paquetes localizados en NuGet.

Puede instalar o actualizar a los paquetes de NuGet publicados mediante la consola de administrador de paquetes de NuGet:

[!code-console[Main](whats-new-in-aspnet-web-api-21/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>Documentación

Tutoriales y otra información sobre RTM de ASP.NET Web API 2.1 están disponibles desde el sitio web ASP.NET ([https://www.asp.net/web-api](../../index.md)).

<a id="new-features"></a>
## <a name="new-features-in-aspnet-web-api-21"></a>Nuevas características en ASP.NET Web API 2.1

<a id="global-error"></a>
### <a name="global-error-handling"></a>Control de errores global

Ahora se pueden registrar todas las excepciones no controladas a través de un mecanismo central, y se puede personalizar el comportamiento de las excepciones no controladas.

El marco de trabajo admite varios registradores de excepciones, que se vea la excepción no controlada e información sobre el contexto en el que se produjo, por ejemplo, la solicitud que se está procesando en el momento.

Por ejemplo, el código siguiente usa System.Diagnostics.TraceSource para registrar todas las excepciones no controladas:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample2.cs)]

También puede reemplazar el controlador de excepciones de forma predeterminada, se produce por lo que puede personalizar completamente el mensaje de respuesta HTTP que se envía cuando una excepción no controlada.

Hemos proporcionado un [ejemplo](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/Elmah/ReadMe.txt) que registra las excepciones no controladas de todo mediante el marco ELMAH popular.

<a id="attribute-routing"></a>
### <a name="attribute-routing-improvements"></a>Mejoras de enrutamiento de atributo

Enrutamiento mediante atributos ahora es compatible con las restricciones, lo que permite la selección de rutas basadas en el encabezado y control de versiones. Además, muchos aspectos de las rutas de atributo ahora son personalizables a través de la **IDirectRouteFactory** interfaz y **RouteFactoryAttribute** clase. El prefijo de ruta ahora es extensible a través de la **IRoutePrefix** interfaz y **RoutePrefixAttribute** clase.

Hemos proporcionado un [ejemplo](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/RoutingConstraintsSample/ReadMe.txt) que usa las restricciones para filtrar los controladores dinámicamente por un encabezado HTTP 'api-version'.

<a id="help-page"></a>
### <a name="help-page-improvements"></a>Mejoras en la página de ayuda

Web API 2.1 incluye las siguientes mejoras a [páginas de Ayuda de API](../getting-started-with-aspnet-web-api/creating-api-help-pages.md):

- Documentación de las propiedades individuales de los parámetros o tipos de valor devuelto de acciones.
- Documentación de anotaciones del modelo de datos.

El diseño de interfaz de usuario de las páginas de ayuda también se actualizó para incorporar estos cambios.

<a id="ignoreroute"></a>
### <a name="ignoreroute-support"></a>Compatibilidad con IgnoreRoute

Web admite API 2.1 omitiendo los patrones de URL en el enrutamiento de Web API, a través de un conjunto de **IgnoreRoute** métodos de extensión en **HttpRouteCollection**. Estos métodos hacer que la API Web pasar por alto las direcciones URL que coinciden con una plantilla especificada y permitir que el host aplicar un procesamiento adicional si es necesario.

El ejemplo siguiente se pasa por alto los URI que comienzan con un &quot;contenido&quot; segmento:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample3.cs)]

<a id="bson"></a>
### <a name="bson-media-type-formatter"></a>Formateador de tipo de medio BSON

Web API ahora admite la [BSON](http://bsonspec.org/) formato, tanto en el cliente y en el servidor.

Para habilitar BSON en el servidor, agregue el **BsonMediaTypeFormatter** a la colección de formateadores:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample4.cs)]

Le mostramos cómo un cliente .NET puede utilizar el formato BSON:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample5.cs)]

Hemos proporcionado un [ejemplo](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/ReadMe.txt) que muestra el cliente y el servidor.

Para obtener más información, consulte [compatibilidad con BSON en Web API 2.1](../formats-and-model-binding/bson-support-in-web-api-21.md)

<a id="async-filters"></a>
### <a name="better-support-for-async-filters"></a>Mejor compatibilidad para los filtros de asincronía

Web API ahora admite un método sencillo para crear filtros que se ejecutan de forma asincrónica. Esta característica es útil es el filtro se debe realizar una acción asincrónica, como el acceso a una base de datos. Anteriormente, para crear un filtro asincrónico, había que implementar la interfaz de filtro personalmente, dado que las clases de base de filtro solo exponen los métodos sincrónicos. Ahora puede invalidar el virtual `On*Async` métodos de filtro de la clase base.

Por ejemplo:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample6.cs)]

El **AuthorizationFilterAttribute**, **ActionFilterAttribute**, y **ExceptionFilterAttribute** todas las clases de compatibilidad con async en Web API 2.1.

<a id="query-parsing"></a>
### <a name="query-parsing-for-the-client-formatting-library"></a>Consulta de análisis para el cliente de biblioteca de formato

Anteriormente, **System.Net.Http.Formatting** admiten análisis y actualizar las consultas URI para el código del lado servidor, pero la biblioteca portable equivalente falta esta característica. En la Web API 2.1, una aplicación cliente puede ahora fácilmente analizar y actualizar una cadena de consulta.

Los ejemplos siguientes muestran cómo analizar, modificar y generar las consultas URI. (Los ejemplos muestran una aplicación de consola para simplificar las cosas).

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample7.cs)]

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a>Problemas conocidos y cambios importantes

Esta sección describen problemas conocidos y cambios importantes en ASP.NET Web API 2.1 RTM.

### <a name="attribute-routing"></a>Enrutamiento mediante atributos

Ambigüedades en coincidencias de enrutamiento de atributos ahora notificar un error en lugar de elegir a la primera coincidencia.

Las rutas de atributo se prohíbe la *{controller}* parámetro y del uso de la *{action}* parámetro en las rutas se coloca en las acciones. Estos parámetros es muy probable que provocarían las ambigüedades.

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a>Scaffolding de MVC o API Web en un proyecto con 5.1 da como resultado paquetes 5.0 paquetes para las que aún no existen en el proyecto

Actualizar paquetes de NuGet para ASP.NET Web API 2.1 RTM no actualiza las herramientas de Visual Studio, como el scaffolding de ASP.NET o la plantilla de proyecto de aplicación Web ASP.NET. Usan la versión anterior de los paquetes en tiempo de ejecución ASP.NET (5.0.0.0). Como resultado, el scaffolding de ASP.NET se instalará la versión anterior (5.0.0.0) de los paquetes necesarios, si aún no están disponibles en los proyectos. Sin embargo, el scaffolding de ASP.NET en Visual Studio 2013 RTM o Update 1 no sobrescribe los paquetes más recientes en sus proyectos.

Si usa scaffolding de ASP.NET después de actualizar los paquetes de Web API 2.1 o ASP.NET MVC 5.1, asegúrese de que las versiones de API Web y MVC son coherentes.

### <a name="type-renames"></a>Cambia el nombre de tipo

Algunos de los tipos utilizados para la extensibilidad de enrutamiento de atributos se cambió el nombre de la versión RC a RTM 2.1.

| Nombre de tipo anterior (2.1 RC) | Nuevo tipo de nombre (RTM 2.1) |
| --- | --- |
| IDirectRouteProvider | IDirectRouteFactory |
| RouteProviderAttribute | RouteFactoryAttribute |
| DirectRouteProviderContext | DirectRouteFactoryContext |

### <a name="exception-filters-do-not-unwrap-aggregate-exceptions-thrown-in-async-actions"></a>Los filtros de excepciones desencapsular agregadas excepciones producidas en acciones asincrónicas

Anteriormente, si una acción asincrónica produjo una **AggregateException**, un filtro de excepción sería desencapsular la excepción, y **OnException** obtendría la excepción base. 2.1, el filtro de excepción desencapsular, y **OnException** obtiene original **AggregateException**.

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a>Correcciones de errores

Esta versión también incluye varias correcciones de errores. Puede encontrar una lista completa aquí:

- [5.1.0 paquete](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=Web%20API|Web%20API%20OData&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [5.1.1 paquete](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.1.1%20RTM&assignedTo=All&component=Web%20API&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

El 5.1.2 paquete contiene actualizaciones de IntelliSense, pero no hay correcciones de errores.
