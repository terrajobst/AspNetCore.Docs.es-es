---
title: Prácticas recomendadas de rendimiento de ASP.NET Core
author: mjrousos
description: Sugerencias para aumentar el rendimiento en aplicaciones ASP.NET Core y evitar problemas de rendimiento comunes.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 04/13/2019
uid: performance/performance-best-practices
ms.openlocfilehash: 095db38cf3102f6e18930efdbbaeeb90dffad8af
ms.sourcegitcommit: 017b673b3c700d2976b77201d0ac30172e2abc87
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2019
ms.locfileid: "59614453"
---
# <a name="aspnet-core-performance-best-practices"></a>Prácticas recomendadas de rendimiento de ASP.NET Core

Por [Mike Rousos](https://github.com/mjrousos)

Este tema proporciona instrucciones para el rendimiento de los procedimientos recomendados con ASP.NET Core.

<a name="hot"></a>

En este documento, un *ruta de acceso del código de acceso frecuente* se define como una ruta de acceso del código que se llama con frecuencia y donde ocurre gran parte del tiempo de ejecución. Las rutas de acceso frecuente de código normalmente limitan la escalabilidad horizontal de la aplicación y el rendimiento.

## <a name="cache-aggressively"></a>Caché de forma agresiva

Almacenamiento en caché se analiza en varias partes de este documento. Para obtener más información, consulta <xref:performance/caching/response>.

## <a name="avoid-blocking-calls"></a>Evitar llamadas de bloqueo

Aplicaciones de ASP.NET Core deben diseñarse para procesar muchas solicitudes simultáneamente. Las API asincrónicas permiten un pequeño grupo de subprocesos para controlar miles de solicitudes simultáneas por no Esperando llamadas de bloqueo. En lugar de esperar en una tarea de larga ejecución sincrónica para completar, el subproceso puede trabajar en otra solicitud.

Un problema de rendimiento comunes en aplicaciones ASP.NET Core está bloqueando las llamadas que podrían ser asincrónicas. Provocar muchas llamadas bloqueos sincrónicas [privación de grupos de subprocesos](https://blogs.msdn.microsoft.com/vancem/2018/10/16/diagnosing-net-core-threadpool-starvation-with-perfview-why-my-service-is-not-saturating-all-cores-or-seems-to-stall/) y tiempos de respuesta de degradado.

**No**:

* Bloquear la ejecución asincrónica mediante una llamada a [Task.Wait](/dotnet/api/system.threading.tasks.task.wait) o [Task.Result](/dotnet/api/system.threading.tasks.task-1.result).
* Adquirir bloqueos en las rutas de código común. Las aplicaciones de ASP.NET Core son de mayor rendimiento cuando se ha diseñado como para ejecutar código en paralelo.

**Hacer**:

* Asegúrese de ["hot" las rutas de código](#hot) asincrónica.
* Llamar de forma asincrónica el acceso a datos y las API de operaciones de larga ejecución.
* Asegúrese de controlador/Razor acciones de la página asincrónica. Es asincrónica con el fin de beneficiarse de la pila de llamadas completa [async y await](/dotnet/csharp/programming-guide/concepts/async/) patrones.

Un generador de perfiles, como [PerfView](https://github.com/Microsoft/perfview), se puede usar para buscar subprocesos con frecuencia se agregan a la [grupos de subprocesos de](/windows/desktop/procthread/thread-pool). El `Microsoft-Windows-DotNETRuntime/ThreadPoolWorkerThread/Start` evento indica un subproceso que se agregan al grupo de subprocesos. <!--  For more information, see [async guidance docs](TBD-Link_To_Davifowl_Doc  -->

## <a name="minimize-large-object-allocations"></a>Minimizar las asignaciones de objetos grandes

<!-- TODO review Bill - replaced original .NET language below with .NET Core since this targets .NET Core -->
El [recolector de elementos no utilizados de .NET Core](/dotnet/standard/garbage-collection/) administra la asignación y liberación de memoria automáticamente en aplicaciones ASP.NET Core. Colección de elementos no utilizados automática generalmente significa que los desarrolladores no tienen que preocuparse por cómo y cuándo se libera memoria. Sin embargo, limpiar los objetos sin referencia necesita tiempo de CPU, por lo que los desarrolladores deben minimizar la asignación de objetos en ["hot" las rutas de código](#hot). Colección de elementos no utilizados es especialmente costosa en objetos grandes (bytes > 85 KB). Los objetos grandes se almacenan en el [montón de objetos grandes](/dotnet/standard/garbage-collection/large-object-heap) y requieren un completo (generación 2) recolección de elementos para limpiar. A diferencia de la generación 0 y recolecciones de generación 1, una recolección de generación 2 requiere una suspensión temporal de la ejecución de la aplicación. Frecuente asignación y desasignación de objetos grandes pueden provocar un rendimiento incoherente.

Recomendaciones:

* **Hacer** considere la posibilidad de almacenamiento en caché de objetos grandes que se utilizan con frecuencia. Almacenamiento en caché de objetos grandes evita que las asignaciones de costosas.
* **Hacer** grupo de búferes mediante el uso de un [ `ArrayPool<T>` ](/dotnet/api/system.buffers.arraypool-1) para almacenar matrices de gran tamaño.
* **No** asignar muchos y de corta duración de objetos grandes en ["hot" las rutas de código](#hot).

Se pueden diagnosticar problemas de memoria, como los anteriores, revisando las estadísticas de elementos no utilizados (GC) de la colección en [PerfView](https://github.com/Microsoft/perfview) y examinar:

* Tiempo de pausa de la colección de elementos no utilizados.
* ¿Qué porcentaje de tiempo del procesador se invierte en la recolección de elementos.
* ¿Cuántas colecciones de elementos no utilizados son la generación 0, 1 y 2.

Para obtener más información, consulte [colección de elementos no utilizados y rendimiento](/dotnet/standard/garbage-collection/performance).

## <a name="optimize-data-access"></a>Optimizar el acceso a datos

A menudo, las interacciones con un almacén de datos y otros servicios remotos son las partes más lentas de una aplicación ASP.NET Core. Leer y escribir datos de forma eficaz son fundamental para un buen rendimiento.

Recomendaciones:

* **Hacer** llamar a todas las API de acceso de datos de forma asincrónica.
* **No** recuperar más datos que es necesario. Escribir consultas para devolver solamente los datos que es necesarios para la solicitud HTTP actual.
* **Hacer** considere la posibilidad de almacenamiento en caché con frecuencia tiene acceso a los datos recuperados de una base de datos o servicio remoto si los datos obsoletos ligeramente están aceptables. Según el escenario, utilice un [MemoryCache](xref:performance/caching/memory) o un [DistributedCache](xref:performance/caching/distributed). Para obtener más información, consulta <xref:performance/caching/response>.
* **Hacer** minimizar ida y vuelta de red. El objetivo es recuperar los datos necesarios en una sola llamada, en lugar de varias llamadas.
* **Hacer** usar [consultas de no seguimiento](/ef/core/querying/tracking#no-tracking-queries) en Entity Framework Core al obtener acceso a datos de solo lectura. EF Core puede devolver los resultados de consultas no realizar un seguimiento de forma más eficaz.
* **Hacer** agregadas consultas LINQ y filtro (con `.Where`, `.Select`, o `.Sum` las instrucciones, por ejemplo) para que el filtrado se realiza mediante la base de datos.
* **Hacer** considere la posibilidad de que EF Core resuelve algunos operadores de consulta en el cliente, lo que puede provocar la ejecución de consultas ineficaces. Para obtener más información, consulte [problemas de rendimiento de evaluación de cliente](/ef/core/querying/client-eval#client-evaluation-performance-issues).
* **No** utilizar consultas de proyección en colecciones, que pueden dar lugar a ejecución de "N + 1" consultas SQL. Para obtener más información, consulte [optimización de subconsultas correlacionadas](/ef/core/what-is-new/ef-core-2.1#optimization-of-correlated-subqueries).

Consulte [EF de alto rendimiento](/ef/core/what-is-new/ef-core-2.0#explicitly-compiled-queries) enfoques que puede mejorar el rendimiento de aplicaciones a gran escala:

* [Agrupación de DbContext](/ef/core/what-is-new/ef-core-2.0#dbcontext-pooling)
* [Consultas compiladas de manera explícita](/ef/core/what-is-new/ef-core-2.0#explicitly-compiled-queries)

Se recomienda medir el impacto de los enfoques de alto rendimiento anteriores antes de confirmar el código base. Es posible que la complejidad adicional de las consultas compiladas no justifiquen la mejora del rendimiento.

Consulta se pueden detectar problemas revisando el tiempo dedicado a obtener acceso a datos con [Application Insights](/azure/application-insights/app-insights-overview) o con herramientas de generación de perfiles. La mayoría de las bases de datos también disponen de las estadísticas relativas a las consultas ejecutadas con frecuencia.

## <a name="pool-http-connections-with-httpclientfactory"></a>Conexiones de grupo HTTP con HttpClientFactory

Aunque [HttpClient](/dotnet/api/system.net.http.httpclient) implementa el `IDisposable` interfaz, está diseñado para su reutilización. Cerrado `HttpClient` instancias deje sockets abiertos en el `TIME_WAIT` estado durante un breve período de tiempo. Si una ruta de acceso del código que crea y se desecha `HttpClient` objetos se usa con frecuencia, la aplicación puede agotar sockets disponibles. [HttpClientFactory](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests) se introdujo en ASP.NET Core 2.1 como una solución a este problema. Controla la agrupación de conexiones de HTTP para optimizar el rendimiento y confiabilidad.

Recomendaciones:

* **No** crear y eliminar `HttpClient` instancias directamente.
* **Hacer** usar [HttpClientFactory](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests) para recuperar `HttpClient` instancias. Para obtener más información, consulte [HttpClientFactory de uso para implementar las solicitudes HTTP resistentes](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests).

## <a name="keep-common-code-paths-fast"></a>Mantener las rutas de código comunes rápidas

Desea que todo el código que se llame rápidas, con frecuencia las rutas de código son los más importantes para optimizar:

* Componentes de middleware en la canalización de procesamiento de solicitudes de la aplicación, especialmente middleware que se ejecute al principio de la canalización. Estos componentes tienen un gran impacto en el rendimiento.
* Código que se ejecuta para cada solicitud o varias veces por solicitud. Por ejemplo, el registro personalizado, los controladores de autorización o inicialización de los servicios transitorios.

Recomendaciones:

* **No** usar componentes de middleware personalizado con tareas de ejecución prolongada.
* **Hacer** usar herramientas de generación de perfiles como [herramientas de diagnóstico de Visual Studio](/visualstudio/profiling/profiling-feature-tour) o [PerfView](https://github.com/Microsoft/perfview)), para identificar ["hot" las rutas de código](#hot).

## <a name="complete-long-running-tasks-outside-of-http-requests"></a>Completar larga tareas fuera de las solicitudes HTTP

La mayoría de las solicitudes a una aplicación ASP.NET Core pueden controlarse mediante un controlador o un modelo de página de una llamada a los servicios necesarios y devolver una respuesta HTTP. Para algunas solicitudes que implican las tareas de larga ejecución, es mejor facilitar el proceso de respuesta de solicitud completa asincrónica.

Recomendaciones:

* **No** esperar a las tareas de ejecución prolongada llevar a cabo como parte del procesamiento normal de la solicitud HTTP.
* **Hacer** considere la posibilidad de controlar las solicitudes de ejecución prolongada con [servicios en segundo plano](xref:fundamentals/host/hosted-services) o fuera de proceso con un [Azure Function](/azure/azure-functions/). Completar fuera de proceso de trabajo es especialmente útil para las tareas que consumen más CPU.
* **Hacer** usar opciones de comunicación en tiempo real, como [SignalR](xref:signalr/introduction), para comunicarse con clientes de forma asincrónica.

## <a name="minify-client-assets"></a>Minimizar los recursos del cliente

Aplicaciones ASP.NET Core con servidores front-end complejo con frecuencia servir muchos archivos de imagen, CSS o JavaScript. Puede mejorar el rendimiento de las solicitudes de carga inicial por:

* La unión, que combina varios archivos en uno.
* Minificar, lo que reduce el tamaño de archivos mediante la eliminación de espacio en blanco y comentarios.

Recomendaciones:

* **Hacer** usar ASP.NET Core [compatibilidad integrada con](xref:client-side/bundling-and-minification) para agrupar y minificar recursos del cliente.
* **Hacer** considere la posibilidad de otras herramientas de terceros, como [Gulp](xref:client-side/using-gulp) o [Webpack](https://webpack.js.org/) compleja administración de clientes activos.

## <a name="compress-responses"></a>Comprimir las respuestas

 Reducir el tamaño de la respuesta normalmente aumenta la capacidad de respuesta de una aplicación, a menudo drásticamente. Una forma de reducir los tamaños de carga es comprimir las respuestas de una aplicación. Para obtener más información, consulte [compresión de respuesta](xref:performance/response-compression).

## <a name="use-the-latest-aspnet-core-release"></a>Use la versión más reciente de ASP.NET Core

Cada nueva versión de ASP.NET Core incluye mejoras de rendimiento. Las optimizaciones en .NET Core y ASP.NET Core significan que las versiones más recientes generalmente superan las versiones anteriores. Por ejemplo, .NET Core 2.1 agrega compatibilidad para expresiones regulares compiladas y saca partido de [ `Span<T>` ](https://msdn.microsoft.com/magazine/mt814808.aspx). Compatibilidad de ASP.NET Core 2.2 agregado para HTTP/2. Si el rendimiento es una prioridad, considere la posibilidad de actualizar a la versión actual de ASP.NET Core.

<!-- TODO review link and taking advantage of new [performance features](#TBD)
Maybe skip this TBD link as each version will have perf improvements -->

## <a name="minimize-exceptions"></a>Minimizar las excepciones

Las excepciones deben ser poco frecuentes. Iniciar y detectar excepciones son lento en relación con otros patrones de flujo de código. Por este motivo, las excepciones no deben utilizarse para controlar el flujo normal del programa.

Recomendaciones:

* **No** uso lanzar o capturar excepciones como medio de flujo del programa normal, especialmente en ["hot" las rutas de código](#hot).
* **Hacer** incluir lógica en la aplicación para detectar y controlar las condiciones que podrían provocar una excepción.
* **Hacer** producir o detectar las excepciones para las condiciones inusuales o inesperadas.

Herramientas de diagnóstico de aplicaciones, como Application Insights, pueden ayudar a identificar las excepciones comunes en una aplicación que puede afectar al rendimiento.
