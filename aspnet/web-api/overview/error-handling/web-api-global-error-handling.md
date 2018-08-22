---
uid: web-api/overview/error-handling/web-api-global-error-handling
title: Global Error Handling in ASP.NET Web API 2 | Microsoft Docs
author: davidmatson
description: ''
ms.author: riande
ms.date: 02/03/2014
ms.assetid: bffd7863-f63b-4b23-a13c-372b5492e9fb
msc.legacyurl: /web-api/overview/error-handling/web-api-global-error-handling
msc.type: authoredcontent
ms.openlocfilehash: a52c2a1589327421b7f498ff551145676c80e3e8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828440"
---
<a name="global-error-handling-in-aspnet-web-api-2"></a>Global Error Handling in ASP.NET Web API 2
====================
por [David Matson](https://github.com/davidmatson), [Rick Anderson](https://github.com/Rick-Anderson)

En la actualidad no hay ninguna manera fácil en Web API para registrar o controlar los errores de forma global. Algunas excepciones no controladas pueden procesarse mediante [filtros de excepción](exception-handling.md), pero hay una serie de casos que no pueden controlar los filtros de excepciones. Por ejemplo:

1. Excepciones iniciadas por constructores del controlador.
2. Excepciones producidas por controladores de mensajes.
3. Excepciones producidas durante el enrutamiento.
4. Excepciones producidas durante la serialización del contenido de respuesta.

Queremos proporcionar una forma simple y coherente para iniciar y controlar (donde sea posible) estas excepciones. 

Hay dos casos principales para controlar las excepciones, el caso donde podemos enviar una respuesta de error y el caso de que todos los que podemos hacer es la excepción del registro. Un ejemplo para el último caso es cuando se produce una excepción en el medio de transmisión por secuencias el contenido de la respuesta; en ese caso es demasiado tarde para enviar un mensaje de respuesta nuevo puesto que el código de estado, los encabezados y contenido parcial ya hayan pasado a través de la red, por lo que sólo se anula la conexión. Aunque no se controla la excepción para producir un nuevo mensaje de respuesta, todavía se admite el registro de la excepción. En casos donde podemos detectamos un error, nos podemos devolver una respuesta de error adecuado tal como se muestra en la siguiente:

[!code-csharp[Main](web-api-global-error-handling/samples/sample1.cs?highlight=6)]

### <a name="existing-options"></a>Opciones existentes

Además [filtros de excepción](exception-handling.md), [controladores de mensajes](../advanced/http-message-handlers.md) se puede usar hoy para observar todas las respuestas de nivel 500, pero es difícil, actuar en las respuestas que carecen de contexto sobre el error original. Controladores de mensajes también tienen algunas de las mismas limitaciones que los filtros de excepciones con respecto a los casos en que puede controlar. Aunque Web API tiene una infraestructura de seguimiento que capture las condiciones de error la infraestructura de seguimiento es para fines de diagnóstico no diseñada y adecuada para que se ejecutan en entornos de producción. Global control de excepciones y registro deben ser servicios que pueden ejecutar durante la producción y estar conectados a soluciones de supervisión existentes (por ejemplo, [ELMAH](https://code.google.com/p/elmah/) ).

### <a name="solution-overview"></a>Introducción a la solución

 Proporcionamos dos nuevos servicios reemplazable por el usuario, [IExceptionLogger](../releases/whats-new-in-aspnet-web-api-21.md) y IExceptionHandler, iniciar y controlar las excepciones no controladas. Los servicios son muy similares, con dos diferencias principales:

1. Admitimos registrar varios registradores de excepciones, pero solo un único controlador de excepciones.
2. Registradores de excepciones siempre llamará, incluso si se van a anular la conexión. Los controladores de excepciones solo llamará cuando hayamos terminado todavía puede elegir qué mensaje de respuesta para enviar.

Ambos servicios proporcionan acceso a un contexto de excepción que contiene información relevante desde el punto donde se detectó la excepción, especialmente la [HttpRequestMessage](https://msdn.microsoft.com/library/system.net.http.httprequestmessage(v=vs.110).aspx), [HttpRequestContext](https://msdn.microsoft.com/library/system.web.http.controllers.httprequestcontext(v=vs.118).aspx), produce la excepción y el origen de la excepción (los detalles a continuación).

### <a name="design-principles"></a>Principios de diseño

1. **No hay cambios importantes** porque esta funcionalidad se agrega en una versión secundaria, una restricción importante que afecte a la solución es que no haber ningún cambio importante, ya sea para escribir contratos o al comportamiento. Esta restricción descartado alguna limpieza que nos gustaría haber hecho en términos de bloques catch existentes convertir excepciones en 500 respuestas. Esta limpieza adicional es algo que nos podríamos tener en cuenta para una versión posterior de principal. Si esto es importante para vote al respecto en [voz del usuario de ASP.NET Web API](http://aspnet.uservoice.com/forums/147201-asp-net-web-api/suggestions/5451321-add-flag-to-enable-iexceptionlogger-and-iexception).
2. **Mantener la coherencia con la API Web construye** canalización de filtro de la API Web es una excelente manera de controlar cuestiones transversales con la flexibilidad de aplicar la lógica en un ámbito de acción específica, específicas del controlador o global. Los filtros, incluidos los filtros de excepción, siempre tienen contextos de acción y controlador, incluso cuando se registra en el ámbito global. Existe de que el contrato que tenga sentido para los filtros, pero significa que los filtros de excepciones, las incluso globalmente con ámbito, no son una buena elección para algunos casos, como las excepciones de controladores de mensajes de control donde ningún contexto de acción o controlador de excepciones. Si desea utilizar el ámbito flexible ofrecida por los filtros para el control de excepciones, todavía necesitamos los filtros de excepciones. Pero si es necesario controlar la excepción fuera de un contexto de controlador, necesitamos también una construcción independiente para el control de errores global completo (algo sin las restricciones de contexto controlador contexto y acción).

### <a name="when-to-use"></a>Cuándo usar

- Registradores de excepciones son la solución para ver todos los excepción no controlada detectada por la API Web.
- Los controladores de excepciones son la solución para personalizar todas las posibles respuestas a las excepciones no controladas detectadas por la API Web.
- Los filtros de excepciones son la solución más sencilla para procesar las excepciones no controlada de subconjunto relacionados con una acción específica o un controlador de.

### <a name="service-details"></a>Detalles del servicio

 Las interfaces de servicios de registrador y controlador de excepción son métodos asincrónico simple toma los contextos respectivos: 

[!code-csharp[Main](web-api-global-error-handling/samples/sample2.cs)]

 También se incluyen las clases base para ambas interfaces. Invalidación de los métodos de núcleo (sincrónica o asincrónica) es todo lo que es necesario para iniciar sesión o identificador en el modo recomendado veces. Para el registro, el `ExceptionLogger` se asegurará de que el método de registro core solo se llama una vez para cada excepción de clase base (incluso si más adelante se propaga aún más la seguridad de la pila de llamadas y se detecta de nuevo). La `ExceptionHandler` clase base llamará el método de control solo para las excepciones en la parte superior de la pila de llamadas, omitiendo heredado anidado bloques catch de núcleo. (Versiones simplificadas de estas clases base se encuentran en el apéndice a continuación.) Ambos `IExceptionLogger` y `IExceptionHandler` recibir información acerca de la excepción a través de un `ExceptionContext`.

[!code-csharp[Main](web-api-global-error-handling/samples/sample3.cs)]

Cuando el marco llama a un registrador de excepciones o un controlador de excepciones, siempre proporcionará un `Exception` y un `Request`. Excepto para las pruebas unitarias, también siempre proporcionará un `RequestContext`. Rara vez proporcionará un `ControllerContext` y `ActionContext` (únicamente cuando se llama desde el bloque catch para filtros de excepciones). Con poca frecuencia ofrecerá un `Response`(solo en determinados casos IIS cuando en el medio al intentar escribir la respuesta). Tenga en cuenta que dado que algunas de estas propiedades pueden ser `null` es responsabilidad del consumidor para que busque `null` antes de acceder a los miembros de la clase de excepción.`CatchBlock` es una cadena que indica qué bloque catch vio la excepción. Las cadenas de bloques catch son los siguientes:

- Servidor HTTP (método SendAsync)
- HttpControllerDispatcher (método SendAsync)
- HttpBatchHandler (método SendAsync)
- IExceptionFilter (procesamiento de ApiController de la canalización de filtro de excepción en ExecuteAsync)
- Host OWIN:

    - HttpMessageHandlerAdapter.BufferResponseContentAsync (para almacenamiento en búfer de salida)
    - HttpMessageHandlerAdapter.CopyResponseContentAsync (para la transmisión por secuencias de salida)
- Host de Web:

    - HttpControllerHandler.WriteBufferedResponseContentAsync (para almacenamiento en búfer de salida)
    - HttpControllerHandler.WriteStreamedResponseContentAsync (para la transmisión por secuencias de salida)
    - HttpControllerHandler.WriteErrorResponseContentAsync (si hay errores en la recuperación de errores en el modo de salida del búfer)

La lista de cadenas de bloques catch también está disponible a través de las propiedades de solo lectura estático. (La cadena del bloque catch core están en el ExceptionCatchBlocks estáticos; el resto aparece en una clase estática cada host OWIN y web).`IsTopLevelCatchBlock` es útil para seguir el patrón recomendado del control de excepciones sólo en la parte superior de la pila de llamadas. En lugar de activar las excepciones en 500 respuestas desde cualquier lugar que se produce un bloque catch anidado, un controlador de excepciones puede permitir que las excepciones a propagar hasta que se va a ser visto por el host.

Además el `ExceptionContext`, un registrador obtiene algo más de información a través de las completas `ExceptionLoggerContext`:

[!code-csharp[Main](web-api-global-error-handling/samples/sample4.cs)]

La segunda propiedad, `CanBeHandled`, permite a un registrador identificar una excepción que no se puede controlar. Cuando la conexión está a punto de anularse y no se puede enviar ningún mensaje de respuesta nuevo, se llamará a los registradores pero el controlador ***no*** llamará, y los registradores pueden identificar este escenario de esta propiedad.

En adicionales a la `ExceptionContext`, un controlador obtiene una propiedad más que puede establecer en el completo `ExceptionHandlerContext` para controlar la excepción:

[!code-csharp[Main](web-api-global-error-handling/samples/sample5.cs)]

Un controlador de excepción indica que ha controlado una excepción al establecer el `Result` propiedad a un resultado de acción (por ejemplo, un [ExceptionResult](https://msdn.microsoft.com/library/system.web.http.results.exceptionresult(v=vs.118).aspx), [InternalServerErrorResult](https://msdn.microsoft.com/library/system.web.http.results.internalservererrorresult(v=vs.118).aspx), [ StatusCodeResult](https://msdn.microsoft.com/library/system.web.http.results.statuscoderesult(v=vs.118).aspx), o un resultado personalizado). Si el `Result` propiedad es null, se controló la excepción y se vuelve a producir la excepción original.

Para las excepciones en la parte superior de la pila de llamadas, utilizamos un paso adicional para asegurarse de que la respuesta es adecuada para los autores de llamadas de API. Si la excepción se propaga hasta el host, el llamador podría ver la pantalla amarilla de la muerte o algún otro host proporciona la respuesta que normalmente es HTML y normalmente no una respuesta de error adecuada de API. En estos casos, se inicia el resultado no es null, y solo si un controlador de excepciones personalizado establece explícitamente, realizar una copia en `null` la excepción (no controlada) se propagará al host. Establecer `Result` a `null` en tales casos puede ser útil para dos escenarios:

1. OWIN hospeda Web API con middleware registrado antes de/externa API Web de control de excepciones personalizado.
2. Depuración local mediante un explorador, donde la pantalla amarilla de la muerte es realmente una respuesta útil para una excepción no controlada.

Para los registradores de excepciones y los controladores de excepciones, no hace nada para recuperar en caso de que el registrador o su propio controlador produce una excepción. (Distinto de permitir que la excepción de propagación, dejar comentarios en la parte inferior de esta página, si tiene un mejor enfoque). El contrato de registradores de excepciones y los controladores es que no deben permitir que las excepciones propagar hasta sus llamadores; en caso contrario, la excepción simplemente propagará, a menudo hasta el host genera un error HTML (por ejemplo, ASP. Pantalla amarilla de la red) que se envían al cliente (que no suele ser la opción preferida para los autores de llamadas de API que esperan JSON o XML).

## <a name="examples"></a>Ejemplos

### <a name="tracing-exception-logger"></a>Registrador de excepciones de seguimiento

El registrador de excepciones a continuación envía los datos de excepción a los orígenes de seguimiento configurados (incluida la ventana de salida de depuración en Visual Studio).

[!code-csharp[Main](web-api-global-error-handling/samples/sample6.cs)]

### <a name="custom-error-message-exception-handler"></a>Controlador de excepciones de mensaje de Error personalizado

Lo siguiente a continuación, genera una respuesta de error personalizado para los clientes, incluidos la dirección de correo electrónico para ponerse en contacto con soporte técnico.

[!code-csharp[Main](web-api-global-error-handling/samples/sample7.cs)]

## <a name="registering-exception-filters"></a>Registrar los filtros de excepciones

Si usa la plantilla de proyecto "Aplicación Web de ASP.NET MVC 4" para crear el proyecto, coloque el código de configuración de Web API dentro de la `WebApiConfig` clase, en el *App/_iniciar* carpeta:

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

## <a name="appendix-base-class-details"></a>Apéndice: Detalles de clase Base

[!code-csharp[Main](web-api-global-error-handling/samples/sample8.cs)]
