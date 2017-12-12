---
uid: web-api/overview/error-handling/exception-handling
title: Control de excepciones en ASP.NET Web API | Documentos de Microsoft
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/12/2012
ms.topic: article
ms.assetid: cbebeb37-2594-41f2-b71a-f4f26520d512
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/error-handling/exception-handling
msc.type: authoredcontent
ms.openlocfilehash: c65ddcca012840d70ab5a33af92edb30041be971
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="exception-handling-in-aspnet-web-api"></a>Control de excepciones en ASP.NET Web API
====================
por [Mike Wasson](https://github.com/MikeWasson)

Este artículo describe el error y control de excepciones en ASP.NET Web API.

- [HttpResponseException](#httpresponserexception)
- [Filtros de excepciones](#exception_filters)
- [Registro de filtros de excepciones](#registering_exception_filters)
- [HttpError](#httperror)

<a id="httpresponserexception"></a>
## <a name="httpresponseexception"></a>HttpResponseException

¿Qué ocurre si un controlador de Web API produce una excepción no detectada? De forma predeterminada, la mayoría de las excepciones se traduce a una respuesta HTTP con código de estado 500 Error interno del servidor.

El **HttpResponseException** tipo es un caso especial. Esta excepción devuelve cualquier código de estado HTTP que especifica en el constructor de la excepción. Por ejemplo, el método siguiente devuelve 404, no se encuentra, si la *identificador* parámetro no es válido.

[!code-csharp[Main](exception-handling/samples/sample1.cs)]

Para tener más control sobre la respuesta, también puede construir el mensaje de respuesta completo e incluir con el **HttpResponseException:** 

[!code-csharp[Main](exception-handling/samples/sample2.cs)]

<a id="exception_filters"></a>
## <a name="exception-filters"></a>Filtros de excepciones

Puede personalizar cómo API Web controla las excepciones mediante la escritura de un *filtro de excepción*. Un filtro de excepción se ejecuta cuando un método de controlador produce cualquier excepción no controlada que es *no* una **HttpResponseException** excepción. El **HttpResponseException** tipo es un caso especial, puesto que se ha diseñado específicamente para devolver una respuesta HTTP.

Filtros de excepción implementan la **System.Web.Http.Filters.IExceptionFilter** interfaz. La manera más sencilla de crear un filtro de excepción es derivar desde la **System.Web.Http.Filters.ExceptionFilterAttribute** clase e invalidar el **OnException** método.

> [!NOTE]
> Los filtros de excepción en ASP.NET Web API son similares a las de ASP.NET MVC. Sin embargo, se declaran en un espacio de nombres independiente y una función por separado. En concreto, el **HandleErrorAttribute** clase usada en MVC no controla las excepciones producidas por controladores de API Web.


Este es un filtro que convierte **NotImplementedException** excepciones en código de estado HTTP 501, no se ha implementado de código:

[!code-csharp[Main](exception-handling/samples/sample3.cs)]

El **respuesta** propiedad de la **HttpActionExecutedContext** objeto contiene el mensaje de respuesta HTTP que se enviará al cliente.

<a id="registering_exception_filters"></a>
## <a name="registering-exception-filters"></a>Registro de filtros de excepciones

Hay varias formas de registrar un filtro de excepción Web API:

- Forma acción
- Por el controlador
- Global

Para aplicar el filtro a una acción concreta, agregue el filtro como un atributo a la acción:

[!code-csharp[Main](exception-handling/samples/sample4.cs)]

Para aplicar el filtro a todas las acciones en un controlador, agregue el filtro como un atributo a la clase de controlador:

[!code-csharp[Main](exception-handling/samples/sample5.cs)]

Para aplicar el filtro globalmente en todos los controladores de API Web, debe agregar una instancia del filtro para la **GlobalConfiguration.Configuration.Filters** colección. Filtros de excepción de esta colección se aplican a cualquier acción de controlador Web API.

[!code-csharp[Main](exception-handling/samples/sample6.cs)]

Si usa la plantilla de proyecto de "Aplicación Web de ASP.NET MVC 4" para crear el proyecto, colocar el código de configuración de la API Web dentro de la `WebApiConfig` (clase), que se encuentra en la aplicación\_carpeta de inicio:

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

<a id="httperror"></a>
## <a name="httperror"></a>HttpError

El **HttpError** objeto proporciona una manera coherente para devolver información de error en el cuerpo de respuesta. En el ejemplo siguiente se muestra cómo devolver el código de estado HTTP 404 (no encontrado) con un **HttpError** en el cuerpo de respuesta.

[!code-csharp[Main](exception-handling/samples/sample8.cs)]

**CreateErrorResponse** se define un método de extensión en la **System.Net.Http.HttpRequestMessageExtensions** clase. Internamente, **CreateErrorResponse** crea un **HttpError** de la instancia y, a continuación, se crea un **HttpResponseMessage** que contiene el **HttpError**.

En este ejemplo, si el método se realiza correctamente, devuelve el producto en la respuesta HTTP. Pero si el producto solicitado no se encuentra, la respuesta HTTP contiene un **HttpError** en el cuerpo de solicitud. La respuesta podría ser similar al siguiente:

[!code-console[Main](exception-handling/samples/sample9.cmd)]

Tenga en cuenta que la **HttpError** se serializa a JSON en este ejemplo. Una ventaja de usar **HttpError** es que se lleva a cabo la misma [negociación de contenido](../formats-and-model-binding/content-negotiation.md) y proceso de serialización que cualquier otro modelo fuertemente tipado.

### <a name="httperror-and-model-validation"></a>HttpError y validación del modelo

Para la validación del modelo, puede pasar el estado del modelo para **CreateErrorResponse**, para incluir los errores de validación en la respuesta:

[!code-csharp[Main](exception-handling/samples/sample10.cs)]

En este ejemplo podría devolver la respuesta siguiente:

[!code-console[Main](exception-handling/samples/sample11.cmd)]

Para obtener más información acerca de la validación del modelo, vea [validación del modelo en ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).

### <a name="using-httperror-with-httpresponseexception"></a>Usar HttpError con HttpResponseException

Los ejemplos anteriores devuelven un **HttpResponseMessage** mensaje desde la acción de controlador, pero también puede usar **HttpResponseException** para devolver un **HttpError**. Esto le permite devolver un modelo fuertemente tipadas en el caso de éxito normal, mientras se sigue devolviendo **HttpError** si se produce un error:

[!code-csharp[Main](exception-handling/samples/sample12.cs)]
