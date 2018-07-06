---
uid: web-api/overview/getting-started-with-aspnet-web-api/action-results
title: Da como resultado de acción en Web API 2 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 02/03/2014
ms.assetid: 2fc4797c-38ef-4cc7-926c-ca431c4739e8
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/action-results
msc.type: authoredcontent
ms.openlocfilehash: 74a837cb9f606d78fb516968343d498d3c37c4ed
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808932"
---
<a name="action-results-in-web-api-2"></a>Resultados de acción en Web API 2
====================
por [Mike Wasson](https://github.com/MikeWasson)

Este tema describe cómo ASP.NET Web API convierte el valor devuelto de una acción de controlador en un mensaje de respuesta HTTP.

Una acción de controlador Web API puede devolver cualquiera de las siguientes acciones:

1. void
2. **HttpResponseMessage**
3. **IHttpActionResult**
4. Algún otro tipo

Dependiendo de cuál de estos se devuelven, API Web usa un mecanismo diferente para crear la respuesta HTTP.

| Tipo devuelto | Cómo se crea la respuesta Web API |
| --- | --- |
| void | Devolver vacía 204 (sin contenido) |
| **HttpResponseMessage** | Convertir directamente en un mensaje de respuesta HTTP. |
| **IHttpActionResult** | Llame a **ExecuteAsync** para crear un **HttpResponseMessage**, a continuación, convertir en un mensaje de respuesta HTTP. |
| Otro tipo | Escribir el valor devuelto serializado en el cuerpo de respuesta; Devuelve 200 (OK). |

El resto de este tema describe cada opción con más detalle.

## <a name="void"></a>void

Si el tipo de valor devuelto es `void`, Web API simplemente devuelve una respuesta HTTP vacía con el código de estado 204 (sin contenido).

Controlador de ejemplo:

[!code-csharp[Main](action-results/samples/sample1.cs)]

Respuesta HTTP:

[!code-console[Main](action-results/samples/sample2.cmd)]

## <a name="httpresponsemessage"></a>HttpResponseMessage

Si la acción devuelve un [HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx), Web API convierte el valor devuelto directamente en un mensaje de respuesta HTTP, mediante las propiedades de la **HttpResponseMessage** objeto para rellenar el respuesta.

Esta opción proporciona un gran control sobre el mensaje de respuesta. Por ejemplo, la siguiente acción del controlador establece el encabezado Cache-Control.

[!code-csharp[Main](action-results/samples/sample3.cs)]

Respuesta:

[!code-console[Main](action-results/samples/sample4.cmd?highlight=2)]

Si se pasa un modelo de dominio para el **CreateResponse** método, Web API usa un [formateador media](../formats-and-model-binding/media-formatters.md) para escribir el modelo serializado en el cuerpo de respuesta.

[!code-csharp[Main](action-results/samples/sample5.cs)]

API Web usa el encabezado Accept en la solicitud para elegir al formateador. Para obtener más información, consulte [negociación de contenido](../formats-and-model-binding/content-negotiation.md).

## <a name="ihttpactionresult"></a>IHttpActionResult

El **IHttpActionResult** interfaz se introdujo en Web API 2. Básicamente, define un **HttpResponseMessage** factory. Estas son algunas ventajas de utilizar el **IHttpActionResult** interfaz:

- Simplifica [las pruebas unitarias](../testing-and-debugging/unit-testing-controllers-in-web-api.md) los controladores.
- Mueve la lógica común para crear respuestas HTTP en clases independientes.
- Hace que la intención de la acción del controlador es más clara, ocultando los detalles de bajo nivel de construir la respuesta.

**IHttpActionResult** contiene un método único, **ExecuteAsync**, que crea de forma asincrónica un **HttpResponseMessage** instancia.

[!code-csharp[Main](action-results/samples/sample6.cs)]

Si una acción de controlador devuelve un **IHttpActionResult**, llamadas de API Web la **ExecuteAsync** método para crear un **HttpResponseMessage**. A continuación, convierte el **HttpResponseMessage** en un mensaje de respuesta HTTP.

Esta es una implementación simple que haga de **IHttpActionResult** que crea una respuesta de texto sin formato:

[!code-csharp[Main](action-results/samples/sample7.cs)]

Acción de controlador de ejemplo:

[!code-csharp[Main](action-results/samples/sample8.cs)]

Respuesta:

[!code-console[Main](action-results/samples/sample9.cmd)]

Más a menudo, usará el **IHttpActionResult** implementaciones definidas en el **[System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx)** espacio de nombres. El **ApiController** clase define los métodos auxiliares que devuelven estos resultados de acción integrada.

En el ejemplo siguiente, si la solicitud no coincide con un identificador de producto existente, el controlador llama a [ApiController.NotFound](https://msdn.microsoft.com/library/system.web.http.apicontroller.notfound.aspx) para crear una respuesta 404 (no encontrado). En caso contrario, el controlador llama a [ApiController.OK](https://msdn.microsoft.com/library/dn314591.aspx), que crea una respuesta 200 (OK) que contiene el producto.

[!code-csharp[Main](action-results/samples/sample10.cs)]

## <a name="other-return-types"></a>Otros tipos de valor devuelto

Para los demás tipos de valor devueltos, API Web usa un [formateador media](../formats-and-model-binding/media-formatters.md) para serializar el valor devuelto. API Web escribe el valor serializado en el cuerpo de respuesta. El código de estado de respuesta es 200 (OK).

[!code-csharp[Main](action-results/samples/sample11.cs)]

Una desventaja de este enfoque es que directamente no se puede devolver un código de error, por ejemplo, 404. Sin embargo, se puede producir un **HttpResponseException** para códigos de error. Para obtener más información, consulte [Exception Handling in ASP.NET Web API](../error-handling/exception-handling.md).

API Web usa el encabezado Accept en la solicitud para elegir al formateador. Para obtener más información, consulte [negociación de contenido](../formats-and-model-binding/content-negotiation.md).

Solicitud de ejemplo

[!code-console[Main](action-results/samples/sample12.cmd)]

Respuesta de ejemplo:

[!code-console[Main](action-results/samples/sample13.cmd)]
