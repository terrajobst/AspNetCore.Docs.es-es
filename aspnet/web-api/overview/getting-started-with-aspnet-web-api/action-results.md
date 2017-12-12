---
uid: web-api/overview/getting-started-with-aspnet-web-api/action-results
title: "Acción da como resultado una Web API 2 | Documentos de Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/03/2014
ms.topic: article
ms.assetid: 2fc4797c-38ef-4cc7-926c-ca431c4739e8
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/action-results
msc.type: authoredcontent
ms.openlocfilehash: 68b82661b97434795e1c306b168033dfcde529bc
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="action-results-in-web-api-2"></a>Resultados de la acción en Web API 2
====================
por [Mike Wasson](https://github.com/MikeWasson)

Este tema describe cómo ASP.NET Web API convierte el valor devuelto de una acción de controlador en un mensaje de respuesta HTTP.

Una acción de controlador de API Web puede devolver cualquiera de las siguientes acciones:

1. void
2. **HttpResponseMessage**
3. **IHttpActionResult**
4. Algún otro tipo

Dependiendo de cuál de ellos se devuelve, API Web utiliza un mecanismo diferente para crear la respuesta HTTP.

| Tipo devuelto | La API Web crea la respuesta |
| --- | --- |
| void | Devolver vacía 204 (sin contenido) |
| **HttpResponseMessage** | Convertir directamente en un mensaje de respuesta HTTP. |
| **IHttpActionResult** | Llame a **ExecuteAsync** para crear un **HttpResponseMessage**, a continuación, convertir en un mensaje de respuesta HTTP. |
| Otro tipo | Escribir el valor devuelto serializado en el cuerpo de respuesta; devolver 200 (OK). |

El resto de este tema describe cada opción con más detalle.

## <a name="void"></a>void

Si el tipo de valor devuelto es `void`, API Web simplemente devuelve una respuesta HTTP vacía con código de estado 204 (sin contenido).

Controlador de ejemplo:

[!code-csharp[Main](action-results/samples/sample1.cs)]

Respuesta HTTP:

[!code-console[Main](action-results/samples/sample2.cmd)]

## <a name="httpresponsemessage"></a>HttpResponseMessage

Si la acción devuelve un [HttpResponseMessage](https://msdn.microsoft.com/en-us/library/system.net.http.httpresponsemessage.aspx), API Web convierte el valor devuelto directamente en un mensaje de respuesta HTTP, utilizando las propiedades de la **HttpResponseMessage** objeto para llenar el respuesta.

Esta opción proporciona un gran control sobre el mensaje de respuesta. Por ejemplo, la acción del controlador siguiente establece el encabezado Cache-Control.

[!code-csharp[Main](action-results/samples/sample3.cs)]

Respuesta:

[!code-console[Main](action-results/samples/sample4.cmd?highlight=2)]

Si se pasa un modelo de dominio para la **CreateResponse** /método siguiente, la API Web utiliza un [formateador del contenido multimedia](../formats-and-model-binding/media-formatters.md) para escribir el modelo serializado en el cuerpo de respuesta.

[!code-csharp[Main](action-results/samples/sample5.cs)]

API Web usa el encabezado Accept en la solicitud para elegir al formateador. Para obtener más información, consulte [negociación de contenido](../formats-and-model-binding/content-negotiation.md).

## <a name="ihttpactionresult"></a>IHttpActionResult

El **IHttpActionResult** interfaz se introdujo en API Web 2. En esencia, define un **HttpResponseMessage** generador. Aquí le presentamos algunas ventajas del uso de la **IHttpActionResult** interfaz:

- Simplifica la [pruebas unitarias](../testing-and-debugging/unit-testing-controllers-in-web-api.md) los controladores.
- Mueve la lógica común para la creación de respuestas HTTP en clases independientes.
- Hace que la intención de la acción de controlador más clara, ocultando los detalles de bajo nivel de creación de la respuesta.

**IHttpActionResult** contiene un método único, **ExecuteAsync**, que crea de forma asincrónica un **HttpResponseMessage** instancia.

[!code-csharp[Main](action-results/samples/sample6.cs)]

Si una acción de controlador devuelve un **IHttpActionResult**, llamadas de API de Web la **ExecuteAsync** método para crear un **HttpResponseMessage**. A continuación, convierte el **HttpResponseMessage** en un mensaje de respuesta HTTP.

Esta es una implementación simple que se haga de **IHttpActionResult** que crea una respuesta de texto sin formato:

[!code-csharp[Main](action-results/samples/sample7.cs)]

Acción de controlador de ejemplo:

[!code-csharp[Main](action-results/samples/sample8.cs)]

Respuesta:

[!code-console[Main](action-results/samples/sample9.cmd)]

Con más frecuencia, usará el **IHttpActionResult** implementaciones definidas en el  **[System.Web.Http.Results](https://msdn.microsoft.com/en-us/library/system.web.http.results.aspx)**  espacio de nombres. El **ApiController** clase define los métodos auxiliares que devuelven estos resultados de acción integrados.

En el ejemplo siguiente, si la solicitud no coincide con un identificador de producto existente, el controlador llama a [ApiController.NotFound](https://msdn.microsoft.com/en-us/library/system.web.http.apicontroller.notfound.aspx) para crear una respuesta 404 (no encontrado). En caso contrario, el controlador llama [ApiController.OK](https://msdn.microsoft.com/en-us/library/dn314591.aspx), que crea una respuesta 200 (OK) que contiene el producto.

[!code-csharp[Main](action-results/samples/sample10.cs)]

## <a name="other-return-types"></a>Otros tipos de valor devuelto

Para los demás tipos de valor devueltos, API Web utiliza un [formateador del contenido multimedia](../formats-and-model-binding/media-formatters.md) para serializar el valor devuelto. API Web escribe el valor serializado en el cuerpo de respuesta. El código de estado de respuesta es 200 (OK).

[!code-csharp[Main](action-results/samples/sample11.cs)]

Una desventaja de este enfoque es que no se puede devolver directamente un código de error, por ejemplo, 404. Sin embargo, se puede producir un **HttpResponseException** para códigos de error. Para obtener más información, consulte [Exception Handling in ASP.NET Web API](../error-handling/exception-handling.md).

API Web usa el encabezado Accept en la solicitud para elegir al formateador. Para obtener más información, consulte [negociación de contenido](../formats-and-model-binding/content-negotiation.md).

Solicitud de ejemplo

[!code-console[Main](action-results/samples/sample12.cmd)]

Respuesta de ejemplo:

[!code-console[Main](action-results/samples/sample13.cmd)]
