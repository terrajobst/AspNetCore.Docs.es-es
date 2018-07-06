---
uid: web-api/overview/error-handling/exception-handling
title: Control de excepciones en ASP.NET Web API | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 03/12/2012
ms.assetid: cbebeb37-2594-41f2-b71a-f4f26520d512
msc.legacyurl: /web-api/overview/error-handling/exception-handling
msc.type: authoredcontent
ms.openlocfilehash: 2db00c1ab241e0dad051c2a3bcd3b0e4bbf4d5c4
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807666"
---
<a name="exception-handling-in-aspnet-web-api"></a>Control de excepciones en ASP.NET Web API
====================
por [Mike Wasson](https://github.com/MikeWasson)

En este artículo describe el error y control de excepciones en ASP.NET Web API.

- [HttpResponseException](#httpresponserexception)
- [Filtros de excepciones](#exception_filters)
- [Registrar los filtros de excepciones](#registering_exception_filters)
- [HttpError](#httperror)

<a id="httpresponserexception"></a>
## <a name="httpresponseexception"></a>HttpResponseException

¿Qué ocurre si un controlador de API Web inicia una excepción no detectada? De forma predeterminada, la mayoría de las excepciones se traduce en una respuesta HTTP con código de estado 500 Error interno del servidor.

El **HttpResponseException** tipo es un caso especial. Esta excepción, devuelve cualquier código de estado HTTP que especifique en el constructor de la excepción. Por ejemplo, devuelve 404, no se encuentra en el siguiente método si la *id* parámetro no es válido.

[!code-csharp[Main](exception-handling/samples/sample1.cs)]

Para obtener más control sobre la respuesta, también puede construir el mensaje de respuesta completo e incluir con la **HttpResponseException:** 

[!code-csharp[Main](exception-handling/samples/sample2.cs)]

<a id="exception_filters"></a>
## <a name="exception-filters"></a>Filtros de excepciones

Puede personalizar cómo la API Web controla las excepciones escribiendo un *filtro de excepción*. Un filtro de excepciones se ejecuta cuando un método de controlador produce cualquier excepción no controlada que es *no* una **HttpResponseException** excepción. El **HttpResponseException** tipo es un caso especial, porque se ha diseñado específicamente para devolver una respuesta HTTP.

Filtros de excepción implementan la **System.Web.Http.Filters.IExceptionFilter** interfaz. La manera más sencilla de escribir un filtro de excepción es derivar de la **System.Web.Http.Filters.ExceptionFilterAttribute** clase e invalidar la **OnException** método.

> [!NOTE]
> Los filtros de excepciones en ASP.NET Web API son similares a los de ASP.NET MVC. Sin embargo, se declaran en un espacio de nombres independiente y una función por separado. En concreto, el **HandleErrorAttribute** clase usada en MVC no controla las excepciones producidas por controladores de API Web.


Este es un filtro que convierte **NotImplementedException** excepciones en código de estado HTTP 501, no se ha implementado el código:

[!code-csharp[Main](exception-handling/samples/sample3.cs)]

El **respuesta** propiedad de la **HttpActionExecutedContext** objeto contiene el mensaje de respuesta HTTP que se enviará al cliente.

<a id="registering_exception_filters"></a>
## <a name="registering-exception-filters"></a>Registrar los filtros de excepciones

Hay varias formas de registrar un filtro de excepción API Web:

- Por acción
- Por el controlador
- Globalmente

Para aplicar el filtro a una acción específica, agregue el filtro como un atributo a la acción:

[!code-csharp[Main](exception-handling/samples/sample4.cs)]

Para aplicar el filtro a todas las acciones en un controlador, agregue el filtro como un atributo a la clase de controlador:

[!code-csharp[Main](exception-handling/samples/sample5.cs)]

Para aplicar el filtro globalmente a todos los controladores Web API, agregue una instancia del filtro para el **GlobalConfiguration.Configuration.Filters** colección. Filtros de excepción de esta colección se aplican a cualquier acción de controlador Web API.

[!code-csharp[Main](exception-handling/samples/sample6.cs)]

Si usa la plantilla de proyecto "Aplicación Web de ASP.NET MVC 4" para crear el proyecto, coloque el código de configuración de Web API dentro de la `WebApiConfig` (clase), que se encuentra en la aplicación\_carpeta de inicio:

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

<a id="httperror"></a>
## <a name="httperror"></a>HttpError

El **HttpError** objeto proporciona una manera coherente para devolver información de error en el cuerpo de respuesta. El ejemplo siguiente muestra cómo devolver el código de estado HTTP 404 (no encontrado) con un **HttpError** en el cuerpo de respuesta.

[!code-csharp[Main](exception-handling/samples/sample8.cs)]

**Createerrorresponse, tal y** se define un método de extensión en el **System.Net.Http.HttpRequestMessageExtensions** clase. Internamente, **createerrorresponse, tal y** crea un **HttpError** de instancia y, a continuación, se crea un **HttpResponseMessage** que contiene el **HttpError**.

En este ejemplo, si el método se realiza correctamente, devuelve el producto en la respuesta HTTP. Pero si no se encuentra el producto solicitado, la respuesta HTTP contiene un **HttpError** en el cuerpo de solicitud. La respuesta podría ser similar al siguiente:

[!code-console[Main](exception-handling/samples/sample9.cmd)]

Tenga en cuenta que el **HttpError** se serializa en JSON en este ejemplo. Una ventaja de usar **HttpError** es que va a través de la misma [negociación de contenido](../formats-and-model-binding/content-negotiation.md) y proceso de serialización como cualquier otro modelo fuertemente tipado.

### <a name="httperror-and-model-validation"></a>HttpError y validación del modelo

Para la validación del modelo, puede pasar el estado del modelo para **createerrorresponse, tal y**, para incluir los errores de validación en la respuesta:

[!code-csharp[Main](exception-handling/samples/sample10.cs)]

En este ejemplo podría devolver la respuesta siguiente:

[!code-console[Main](exception-handling/samples/sample11.cmd)]

Para obtener más información acerca de la validación del modelo, vea [validación de modelos en ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).

### <a name="using-httperror-with-httpresponseexception"></a>Uso de HttpError con HttpResponseException

Los ejemplos anteriores devuelven un **HttpResponseMessage** mensaje de la acción de controlador, aunque también puede usar **HttpResponseException** para devolver un **HttpError**. Esto le permite devolver un modelo fuertemente tipado en el caso de éxito normal, mientras sigue devolviendo **HttpError** si se produce un error:

[!code-csharp[Main](exception-handling/samples/sample12.cs)]
