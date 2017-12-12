---
uid: web-api/overview/error-handling/web-api-global-error-handling
title: Global control de errores en ASP.NET Web API 2 | Documentos de Microsoft
author: davidmatson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/03/2014
ms.topic: article
ms.assetid: bffd7863-f63b-4b23-a13c-372b5492e9fb
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/error-handling/web-api-global-error-handling
msc.type: authoredcontent
ms.openlocfilehash: d2bdf04b4da2a099f3a2af100b16682c68f946f2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="global-error-handling-in-aspnet-web-api-2"></a>Global control de errores en ASP.NET Web API 2
====================
por [David Matson](https://github.com/davidmatson), [Rick Anderson](https://github.com/Rick-Anderson)

Actualmente no hay ninguna manera fácil en Web API para registrar o controlar los errores de forma global. Algunas excepciones no controladas pueden procesarse mediante [filtros de excepción](exception-handling.md), pero hay una serie de casos que no pueden controlar los filtros de excepción. Por ejemplo:

1. Excepciones producidas por los constructores de controlador.
2. Excepciones producidas por controladores de mensajes.
3. Excepciones producidas durante el enrutamiento.
4. Excepciones producidas durante la serialización de contenido de respuesta.

Desea proporcionar una manera sencilla y coherente para iniciar sesión y controlar (siempre que sea posible) estas excepciones. 

Hay dos casos principales para controlar las excepciones, el caso donde podemos enviar una respuesta de error y el caso de que todos los que podemos hacer es el registro de la excepción. Un ejemplo de este último caso es cuando se produce una excepción en el medio de transmisión por secuencias el contenido de la respuesta; en ese caso es demasiado tarde para enviar un mensaje de respuesta nuevo desde el código de estado, los encabezados y parte del contenido ya han pasado a través de la conexión, por lo que simplemente se anula la conexión. Aunque no se controla la excepción para generar un nuevo mensaje de respuesta, se siguen admitiendo registrar la excepción. En casos donde podemos detectamos un error, podemos volver una respuesta de error adecuada como se muestra a continuación:

[!code-csharp[Main](web-api-global-error-handling/samples/sample1.cs?highlight=6)]

### <a name="existing-options"></a>Opciones existentes

Además [filtros de excepción](exception-handling.md), [controladores de mensajes](../advanced/http-message-handlers.md) pueden usarse hoy en día para observar todas las respuestas de nivel de 500, pero es difícil, actúa en las respuestas que carecen de contexto sobre el error original. Controladores de mensajes también disponer de algunas de las mismas limitaciones que los filtros de excepción con respecto a los casos puede controlar. Aunque API Web tienen la infraestructura de seguimiento que captura las condiciones de error la infraestructura de seguimiento es para fines de diagnóstico y no se diseñada o adecuada para ejecutar en entornos de producción. Global control de excepciones y el registro deben ser servicios que pueden ejecutar durante la producción y estar conectados a soluciones de supervisión existentes (por ejemplo, [ELMAH](https://code.google.com/p/elmah/) ).

### <a name="solution-overview"></a>Introducción a la solución

 Se proporcionan dos nuevos servicios reemplazable por el usuario, [IExceptionLogger](../releases/whats-new-in-aspnet-web-api-21.md) y IExceptionHandler, para iniciar sesión y controlar las excepciones no controladas. Los servicios son muy similares, con dos diferencias principales:

1. Se admite registrar varios registradores de excepciones pero solo un único controlador de excepciones.
2. Registradores de excepciones siempre se llaman, incluso si se está a punto de anular la conexión. Controladores de excepciones sólo se llama cuando estamos todavía puede elegir qué mensaje de respuesta para enviar.

Ambos servicios proporcionan acceso a un contexto de excepción que contiene información relevante desde el punto donde se detectó la excepción, especialmente la [HttpRequestMessage](https://msdn.microsoft.com/en-us/library/system.net.http.httprequestmessage(v=vs.110).aspx), [HttpRequestContext](https://msdn.microsoft.com/en-us/library/system.web.http.controllers.httprequestcontext(v=vs.118).aspx), produce la excepción y el origen de la excepción (detalles a continuación).

### <a name="design-principles"></a>Principios de diseño

1. **No hay cambios importantes** porque esta funcionalidad se agrega en una versión secundaria, una restricción importante que afectan a la solución es que no existe ningún cambios importantes, ya sea para escribir contratos o al comportamiento. Esta restricción descartar algunos limpieza que nos gustaría realizadas en cuanto a los bloques catch existentes activar excepciones en 500 respuestas. Este realizar una limpieza adicional es algo que tengamos que tener en cuenta para una versión posterior principal. Si esto es importante para usted, votar en él en [voz de usuario de ASP.NET Web API](http://aspnet.uservoice.com/forums/147201-asp-net-web-api/suggestions/5451321-add-flag-to-enable-iexceptionlogger-and-iexception).
2. **Mantener la coherencia con la API Web construye** canalización de filtro de la API de Web es una excelente manera de controlar los problemas de corte del cruce con la flexibilidad de aplicar la lógica en un ámbito global o de acción específica, específicas del controlador. Los filtros, incluidos los filtros de excepción, siempre tienen contextos de acción y controlador, incluso cuando se registra en el ámbito global. Que el contrato relacione con filtros, pero significa que los filtros de excepción, las ámbito incluso global, no están una buena opción para algunos casos, como excepciones de controladores de mensajes de control donde ningún contexto de acción o controlador de excepciones existe. Si desea utilizar el ámbito flexible ofrecida por los filtros para el control de excepciones, todavía necesitamos filtros de excepciones. Pero si se debe controlar la excepción fuera de un contexto de controlador, también es necesario una construcción independiente para el control de errores global completa (algo sin las restricciones de contexto controlador contexto y la acción).

### <a name="when-to-use"></a>Cuándo se debe utilizar

- Registradores de excepciones son la solución para ver todos los una excepción no controlada detectada por la API Web.
- Controladores de excepciones son la solución para personalizar todas las posibles respuestas a las excepciones no controladas detectadas por la API Web.
- Los filtros de excepción son la solución más fácil para procesar las excepciones de subconjunto no controlada relacionadas con una acción específica o el controlador.

### <a name="service-details"></a>Detalles de servicio

 Las interfaces de servicios de registrador y controlador de excepción son métodos async simple toma los contextos respectivos: 

[!code-csharp[Main](web-api-global-error-handling/samples/sample2.cs)]

 También se proporcionan clases base para ambas interfaces. Reemplazando los métodos de núcleo (sincronización o asincrónico) es todo lo que necesita para iniciar sesión o administrar en tal como se recomienda veces. Para el registro, la `ExceptionLogger` clase base se asegurará de que el método de registro core solo se llama una vez para cada excepción (incluso si más adelante propaga más arriba en la pila de llamadas y se detecta de nuevo). La `ExceptionHandler` clase base llamará el núcleo del método de control solo para bloques catch de excepciones en la parte superior de la pila de llamadas, se omitirá heredado anidado. (Versiones simplificadas de estas clases base están en el apéndice a continuación). Ambos `IExceptionLogger` y `IExceptionHandler` recibir información sobre la excepción a través de un `ExceptionContext`.

[!code-csharp[Main](web-api-global-error-handling/samples/sample3.cs)]

Cuando el marco llama a un registrador de excepciones o un controlador de excepciones, siempre proporcionará un `Exception` y `Request`. Excepto para pruebas unitarias, además, siempre proporcionará un `RequestContext`. Rara vez proporcionará un `ControllerContext` y `ActionContext` (únicamente cuando se llama desde el bloque catch para filtros de excepciones). Con poca frecuencia ofrecerá un `Response`(sólo en algunos casos IIS cuando en el medio al intentar escribir la respuesta). Tenga en cuenta que, dado que algunas de estas propiedades pueden ser `null` es responsabilidad del consumidor para comprobar si `null` antes de obtener acceso a miembros de la clase de excepción.`CatchBlock` es una cadena que indica qué bloque catch vio la excepción. Las cadenas de bloque catch son los siguientes:

- Servidor HTTP (SendAsync método)
- HttpControllerDispatcher (método de SendAsync)
- HttpBatchHandler (método de SendAsync)
- IExceptionFilter (procesamiento del ApiController de la canalización de filtro de excepción en ExecuteAsync)
- Host OWIN:

    - HttpMessageHandlerAdapter.BufferResponseContentAsync (para almacenamiento en búfer de salida)
    - HttpMessageHandlerAdapter.CopyResponseContentAsync (para la transmisión por secuencias de salida)
- Host de Web:

    - HttpControllerHandler.WriteBufferedResponseContentAsync (para almacenamiento en búfer de salida)
    - HttpControllerHandler.WriteStreamedResponseContentAsync (para la transmisión por secuencias de salida)
    - HttpControllerHandler.WriteErrorResponseContentAsync (si hay errores en la recuperación de errores en el modo de salida del búfer)

La lista de cadenas del bloque catch también está disponible a través de propiedades de solo lectura estático. (La cadena del bloque catch principales están en la ExceptionCatchBlocks estático; el resto aparece en una clase estática cada host de web y OWIN).`IsTopLevelCatchBlock` es útil para seguir el patrón recomendado del control de excepciones en la parte superior de la pila de llamadas. En lugar de activar las excepciones en 500 respuestas desde cualquier lugar que se produce un bloque catch anidado, un controlador de excepciones puede dejar que las excepciones se propaguen hasta que se va a ser visto por el host.

Además el `ExceptionContext`, un registrador Obtiene una parte más de información a través de toda la `ExceptionLoggerContext`:

[!code-csharp[Main](web-api-global-error-handling/samples/sample4.cs)]

La segunda propiedad, `CanBeHandled`, permite a un registrador identificar una excepción que no se puede controlar. Cuando la conexión está a punto de anularse y no se puede enviar ningún mensaje de respuesta nuevo, se llamará a los registradores pero el controlador ***no*** llamar, y los registradores pueden identificar este escenario de esta propiedad.

En adicionales a la `ExceptionContext`, un controlador obtiene una propiedad más que puede establecer en toda la `ExceptionHandlerContext` para controlar la excepción:

[!code-csharp[Main](web-api-global-error-handling/samples/sample5.cs)]

Un controlador de excepción indica que ha controlado una excepción al establecer el `Result` propiedad a un resultado de acción (por ejemplo, un [ExceptionResult](https://msdn.microsoft.com/en-us/library/system.web.http.results.exceptionresult(v=vs.118).aspx), [InternalServerErrorResult](https://msdn.microsoft.com/en-us/library/system.web.http.results.internalservererrorresult(v=vs.118).aspx), [ StatusCodeResult](https://msdn.microsoft.com/en-us/library/system.web.http.results.statuscoderesult(v=vs.118).aspx), o un resultado personalizado). Si el `Result` propiedad es null, se controla la excepción y se volverá a producir la excepción original.

Para las excepciones en la parte superior de la pila de llamadas, se llevaron a cabo un paso adicional para asegurarse de que la respuesta es adecuada para los autores de llamadas de API. Si la excepción se propaga hasta el host, el llamador vería la pantalla amarilla de la muerte o algún otro host proporciona respuesta que normalmente es HTML y normalmente no una respuesta de error de API adecuada. En estos casos, inicia el resultado no nulo, y solo si un controlador de excepciones personalizado establece explícitamente nuevo a `null` la excepción (no controladas) se propagará al host. Establecer `Result` a `null` en tales casos, puede ser útil para dos escenarios:

1. OWIN hospedado API Web con middleware registrado API Web antes de/fuera de control de excepciones personalizadas.
2. Depuración local a través de un explorador, donde la pantalla amarilla de la muerte es realmente una respuesta útil para una excepción no controlada.

Para los registradores de excepciones y los controladores de excepciones, no hacen nada para recuperar si el registrador o su propio controlador produce una excepción. (Distinto de permitir que la excepción propagar, votar en la parte inferior de esta página, si tiene un enfoque más adecuado.) El contrato de registradores de excepciones y controladores es que no deben permitir que las excepciones propagar hasta sus llamadores; en caso contrario, la excepción solo propagará, a menudo hasta el host genera un error HTML (por ejemplo, ASP. Pantalla de amarillo de NET) que se envían al cliente (que no suele ser la opción preferida para los autores de llamadas de API que esperan JSON o XML).

## <a name="examples"></a>Ejemplos

### <a name="tracing-exception-logger"></a>Registrador de excepciones de seguimiento

El registrador de excepciones por debajo de los datos de la excepción de envío para los orígenes de seguimiento configurados (incluidos la ventana de salida de depuración en Visual Studio).

[!code-csharp[Main](web-api-global-error-handling/samples/sample6.cs)]

### <a name="custom-error-message-exception-handler"></a>Controlador de excepciones de mensaje de Error personalizado

Lo siguiente a continuación, genera una respuesta de error personalizada a los clientes, incluida una dirección de correo electrónico para ponerse en contacto con soporte técnico.

[!code-csharp[Main](web-api-global-error-handling/samples/sample7.cs)]

## <a name="registering-exception-filters"></a>Registro de filtros de excepciones

Si usa la plantilla de proyecto de "Aplicación Web de ASP.NET MVC 4" para crear el proyecto, colocar el código de configuración de la API Web dentro de la `WebApiConfig` (clase), en la *aplicación/_iniciar* carpeta:

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

## <a name="appendix-base-class-details"></a>Apéndice: Detalles de clase Base

[!code-csharp[Main](web-api-global-error-handling/samples/sample8.cs)]
