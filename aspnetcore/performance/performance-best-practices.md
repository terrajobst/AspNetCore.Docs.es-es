---
title: prácticas recomendadas de rendimiento ASP.NET Core
author: mjrousos
description: Sugerencias para aumentar el rendimiento en aplicaciones ASP.NET Core y evitar problemas de rendimiento comunes.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 04/06/2020
no-loc:
- SignalR
uid: performance/performance-best-practices
ms.openlocfilehash: 068a35fbe410dad24030fe68c0dfd062b402212c
ms.sourcegitcommit: f0aeeab6ab6e09db713bb9b7862c45f4d447771b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80977189"
---
# <a name="aspnet-core-performance-best-practices"></a>prácticas recomendadas de rendimiento ASP.NET Core

Por [Mike Rousos](https://github.com/mjrousos)

En este artículo se proporcionan directrices para las prácticas recomendadas de rendimiento con ASP.NET Core.

## <a name="cache-aggressively"></a>Caché agresivamente

El almacenamiento en caché se discute en varias partes de este documento. Para obtener más información, vea <xref:performance/caching/response>.

## <a name="understand-hot-code-paths"></a>Comprender las rutas de código en caliente

En este documento, una ruta de acceso de *código activo* se define como una ruta de acceso de código a la que se llama con frecuencia y donde se produce gran parte del tiempo de ejecución. Las rutas de acceso de código activo suelen limitar el escalado horizontal y el rendimiento de la aplicación y se describen en varias partes de este documento.

## <a name="avoid-blocking-calls"></a>Evite bloquear llamadas

ASP.NET aplicaciones principales deben diseñarse para procesar muchas solicitudes simultáneamente. Las API asincrónicas permiten que un pequeño grupo de subprocesos controle miles de solicitudes simultáneas al no esperar al bloquear las llamadas. En lugar de esperar a que se complete una tarea sincrónica de larga ejecución, el subproceso puede trabajar en otra solicitud.

Un problema de rendimiento común en las aplicaciones de ASP.NET Core es bloquear las llamadas que podrían ser asincrónicas. Muchas llamadas de bloqueo sincrónico conducen a la [inanición](https://blogs.msdn.microsoft.com/vancem/2018/10/16/diagnosing-net-core-threadpool-starvation-with-perfview-why-my-service-is-not-saturating-all-cores-or-seems-to-stall/) del grupo de subprocesos y a tiempos de respuesta degradados.

**No**:

* Bloquee la ejecución asincrónica llamando a [Task.Wait](/dotnet/api/system.threading.tasks.task.wait) o [Task.Result](/dotnet/api/system.threading.tasks.task-1.result).
* Adquirir bloqueos en rutas de acceso de código comunes. ASP.NET aplicaciones principales son más performant cuando se diseñan para ejecutar código en paralelo.
* Llame a [Task.Run](/dotnet/api/system.threading.tasks.task.run) e inmediatamente espere. ASP.NET Core ya ejecuta código de aplicación en subprocesos normales del grupo de subprocesos, por lo que llamar a Task.Run solo da como resultado una programación de grupo de subprocesos innecesaria adicional. Incluso si el código programado bloquearía un subproceso, Task.Run no lo impide.

**Hacer**:

* Haga que las rutas de [acceso de código en caliente](#understand-hot-code-paths) sean asincrónicas.
* Acceso a datos de llamadas, E/S y API de operaciones de ejecución prolongada de forma asincrónica si hay disponible una API asincrónica. **No** utilice [Task.Run](/dotnet/api/system.threading.tasks.task.run) para hacer que una API sincronizante sea asincrónica.
* Hacer que las acciones de la página del controlador/de la maquinilla de afeitar sean asincrónicas. Toda la pila de llamadas es asíncrona para beneficiarse de patrones [asincrónicos/espera.](/dotnet/csharp/programming-guide/concepts/async/)

Un generador de perfiles, como [PerfView](https://github.com/Microsoft/perfview), se puede utilizar para buscar subprocesos que se agregan con frecuencia al grupo de [subprocesos](/windows/desktop/procthread/thread-pools). El `Microsoft-Windows-DotNETRuntime/ThreadPoolWorkerThread/Start` evento indica un subproceso agregado al grupo de subprocesos. <!--  For more information, see [async guidance docs](TBD-Link_To_Davifowl_Doc)  -->

## <a name="minimize-large-object-allocations"></a>Minimizar las asignaciones de objetos grandes

El recolector de [elementos no utilizados](/dotnet/standard/garbage-collection/) de .NET Core administra la asignación y la liberación de memoria automáticamente en ASP.NET aplicaciones principales. La recolección automática de elementos no utilizados generalmente significa que los desarrolladores no tienen que preocuparse por cómo o cuándo se libera la memoria. Sin embargo, la limpieza de objetos sin referencia requiere tiempo de CPU, por lo que los desarrolladores deben minimizar la asignación de objetos en rutas de acceso de [código activo.](#understand-hot-code-paths) La recolección de elementos no utilizados es especialmente costosa en objetos grandes (> 85 K bytes). Los objetos grandes se almacenan en el montón de [objetos grandes](/dotnet/standard/garbage-collection/large-object-heap) y requieren una recolección de elementos no utilizados completa (generación 2) para limpiar. A diferencia de las colecciones de generación 0 y generación 1, una colección de generación 2 requiere una suspensión temporal de la ejecución de la aplicación. La asignación y desasignación frecuentes de objetos grandes puede provocar un rendimiento incoherente.

Recomendaciones:

* **Considere** la posibilidad de almacenar en caché objetos grandes que se usan con frecuencia. El almacenamiento en caché de objetos grandes evita asignaciones costosas.
* **Realice** búferes de grupo mediante un [>DeT\<arrayPool](/dotnet/api/system.buffers.arraypool-1) para almacenar matrices grandes.
* **No** asigne muchos objetos grandes de corta duración en rutas de acceso de [código activo.](#understand-hot-code-paths)

Los problemas de memoria, como los anteriores, se pueden diagnosticar revisando las estadísticas de recolección de elementos no utilizados (GC) en [PerfView](https://github.com/Microsoft/perfview) y examinando:

* Tiempo de pausa de la recolección de basura.
* ¿Qué porcentaje del tiempo de procesador se invierte en la recolección de elementos no utilizados.
* Cuántas recolecciones de elementos no utilizados son la generación 0, 1 y 2.

Para obtener más información, consulte Recolección y rendimiento de [elementos no utilizados](/dotnet/standard/garbage-collection/performance).

## <a name="optimize-data-access-and-io"></a>Optimice el acceso a los datos y la E/S

Las interacciones con un almacén de datos y otros servicios remotos suelen ser las partes más lentas de una aplicación ASP.NET Core. Leer y escribir datos de manera eficiente es fundamental para un buen rendimiento.

Recomendaciones:

* **Llame** a todas las API de acceso a datos de forma asincrónica.
* **No** recupere más datos de los necesarios. Escriba consultas para devolver solo los datos necesarios para la solicitud HTTP actual.
* **Considere** la posibilidad de almacenar en caché los datos a los que se accede con frecuencia recuperados de una base de datos o servicio remoto si se aceptan datos ligeramente desactualizados. Según el escenario, use [MemoryCache](xref:performance/caching/memory) o [DistributedCache](xref:performance/caching/distributed). Para obtener más información, vea <xref:performance/caching/response>.
* **Minimice** los viajes de ida y vuelta de la red. El objetivo es recuperar los datos necesarios en una sola llamada en lugar de varias llamadas.
* **Use** [consultas sin seguimiento](/ef/core/querying/tracking#no-tracking-queries) en Entity Framework Core al obtener acceso a los datos con fines de solo lectura. EF Core puede devolver los resultados de las consultas sin seguimiento de forma más eficaz.
* **Filtrar** y agregar consultas `.Where`LINQ `.Select`(con , , o `.Sum` instrucciones, por ejemplo) para que la base de datos realice el filtrado.
* **Tenga** en cuenta que EF Core resuelve algunos operadores de consulta en el cliente, lo que puede dar lugar a una ejecución de consultas ineficaz. Para obtener más información, consulte Problemas de rendimiento de [la evaluación](/ef/core/querying/client-eval#client-evaluation-performance-issues)de clientes.
* **No** utilice consultas de proyección en colecciones, lo que puede dar lugar a la ejecución de consultas SQL "N + 1". Para obtener más información, consulte [Optimización de subconsultas correlacionadas.](/ef/core/what-is-new/ef-core-2.1#optimization-of-correlated-subqueries)

Consulte [EF High Performance](/ef/core/what-is-new/ef-core-2.0#explicitly-compiled-queries) para obtener enfoques que pueden mejorar el rendimiento en aplicaciones de gran escala:

* [Agrupación de DbContext](/ef/core/what-is-new/ef-core-2.0#dbcontext-pooling)
* [Consultas compiladas de manera explícita](/ef/core/what-is-new/ef-core-2.0#explicitly-compiled-queries)

Se recomienda medir el impacto de los enfoques de alto rendimiento anteriores antes de confirmar la base de código. La complejidad adicional de las consultas compiladas puede no justificar la mejora del rendimiento.

Los problemas de consulta se pueden detectar revisando el tiempo dedicado a obtener acceso a los datos con [Application Insights](/azure/application-insights/app-insights-overview) o con herramientas de generación de perfiles. La mayoría de las bases de datos también hacen que las estadísticas estén disponibles en relación con las consultas ejecutadas con frecuencia.

## <a name="pool-http-connections-with-httpclientfactory"></a>Agrupar conexiones HTTP con HttpClientFactory

Aunque [HttpClient](/dotnet/api/system.net.http.httpclient) implementa `IDisposable` la interfaz, está diseñada para su reutilización. Las `HttpClient` instancias cerradas `TIME_WAIT` dejan los sockets abiertos en el estado durante un breve período de tiempo. Si se usa con frecuencia `HttpClient` una ruta de acceso de código que crea y elimina objetos, la aplicación puede agotar los sockets disponibles. [HttpClientFactory](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests) se introdujo en ASP.NET Core 2.1 como solución a este problema. Controla la agrupación de conexiones HTTP para optimizar el rendimiento y la fiabilidad.

Recomendaciones:

* **No** cree ni `HttpClient` elimine instancias directamente.
* **Utilice** [HttpClientFactory](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests) para `HttpClient` recuperar instancias. Para obtener más información, consulte el artículo sobre el [uso de HttpClientFactory para implementar solicitudes HTTP resistentes](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests).

## <a name="keep-common-code-paths-fast"></a>Mantenga las rutas de código comunes rápidamente

Quieres que todo tu código sea rápido. Las rutas de acceso de código denominadas con frecuencia son las más importantes de optimizar. Se incluyen los siguientes:

* Los componentes de middleware de la canalización de procesamiento de solicitudes de la aplicación, especialmente el middleware, se ejecutan al principio de la canalización. Estos componentes tienen un gran impacto en el rendimiento.
* Código que se ejecuta para cada solicitud o varias veces por solicitud. Por ejemplo, registro personalizado, controladores de autorización o inicialización de servicios transitorios.

Recomendaciones:

* **No** utilice componentes de middleware personalizados con tareas de ejecución prolongada.
* **Use** herramientas de generación de perfiles de rendimiento, como [Visual Studio Diagnostic Tools](/visualstudio/profiling/profiling-feature-tour) o [PerfView](https://github.com/Microsoft/perfview)), para identificar rutas de acceso de [código en caliente.](#understand-hot-code-paths)

## <a name="complete-long-running-tasks-outside-of-http-requests"></a>Completar tareas de larga ejecución fuera de las solicitudes HTTP

La mayoría de las solicitudes a una aplicación ASP.NET Core se pueden controlar mediante un controlador o modelo de página que llama a los servicios necesarios y devuelve una respuesta HTTP. Para algunas solicitudes que implican tareas de ejecución prolongada, es mejor hacer que todo el proceso de solicitud-respuesta sea asincrónico.

Recomendaciones:

* **No** espere a que se completen las tareas de ejecución prolongada como parte del procesamiento de solicitudes HTTP normal.
* **Considere** la posibilidad de controlar las solicitudes de ejecución prolongada con [servicios en segundo plano](xref:fundamentals/host/hosted-services) o fuera de proceso con una [función](/azure/azure-functions/)de Azure . Completar el trabajo fuera de proceso es especialmente beneficioso para las tareas intensivas de CPU.
* **Utilice** opciones de comunicación en [SignalR](xref:signalr/introduction)tiempo real, como , para comunicarse con los clientes de forma asincrónica.

## <a name="minify-client-assets"></a>Minify activos de clientes

ASP.NET aplicaciones Core con front-ends complejos con frecuencia sirven muchos archivos JavaScript, CSS o de imagen. El rendimiento de las solicitudes de carga iniciales se puede mejorar mediante:

* Agrupación, que combina varios archivos en uno.
* Minifying, que reduce el tamaño de los archivos mediante la eliminación de espacios en blanco y comentarios.

Recomendaciones:

* **Utilice** la compatibilidad [integrada](xref:client-side/bundling-and-minification) de ASP.NET Core para agrupar y comonizar los activos de cliente.
* **Considere** otras herramientas de terceros, como [Webpack](https://webpack.js.org/), para la gestión compleja de activos de cliente.

## <a name="compress-responses"></a>Comprimir respuestas

 Reducir el tamaño de la respuesta suele aumentar la capacidad de respuesta de una aplicación, a menudo de forma espectacular. Una forma de reducir el tamaño de la carga útil es comprimir las respuestas de una aplicación. Para obtener más información, consulte [Compresión de respuesta](xref:performance/response-compression).

## <a name="use-the-latest-aspnet-core-release"></a>Utilice la última versión de ASP.NET Core

Cada nueva versión de ASP.NET Core incluye mejoras de rendimiento. Las optimizaciones en .NET Core y ASP.NET Core significan que las versiones más recientes suelen superar a las versiones anteriores. Por ejemplo, .NET Core 2.1 agregó compatibilidad con expresiones regulares compiladas y se benefició de [Span\<T>](https://msdn.microsoft.com/magazine/mt814808.aspx). ASP.NET Core 2.2 agregó compatibilidad con HTTP/2. [ASP.NET Core 3.0 añade muchas mejoras](xref:aspnetcore-3.0) que reducen el uso de memoria y mejoran el rendimiento. Si el rendimiento es una prioridad, considere la posibilidad de actualizar a la versión actual de ASP.NET Core.

## <a name="minimize-exceptions"></a>Minimizar las excepciones

Las excepciones deben ser raras. Lanzar y detectar excepciones es lento en relación con otros patrones de flujo de código. Debido a esto, las excepciones no se deben usar para controlar el flujo normal del programa.

Recomendaciones:

* **No** utilice excepciones de lanzamiento o captura como medio de flujo de programa normal, especialmente en rutas de acceso de [código en caliente.](#understand-hot-code-paths)
* **Incluya** lógica en la aplicación para detectar y controlar las condiciones que provocarían una excepción.
* **Inicie** o detecte excepciones para condiciones inusuales o inesperadas.

Las herramientas de diagnóstico de aplicaciones, como Application Insights, pueden ayudar a identificar excepciones comunes en una aplicación que pueden afectar al rendimiento.

## <a name="performance-and-reliability"></a>Rendimiento y confiabilidad

En las secciones siguientes se proporcionan sugerencias de rendimiento y soluciones y problemas de confiabilidad conocidos.

## <a name="avoid-synchronous-read-or-write-on-httprequesthttpresponse-body"></a>Evite la lectura o escritura sincrónica en el cuerpo de HttpRequest/HttpResponse

Todas las E/S de ASP.NET Core son asincrónicas. Los servidores `Stream` implementan la interfaz, que tiene sobrecargas sincrónicas y asincrónicas. Los asincrónicos deben preferirse para evitar el bloqueo de subprocesos del grupo de subprocesos. El bloqueo de subprocesos puede provocar la inanición del grupo de subprocesos.

**No haga lo siguiente:** En el ejemplo <xref:System.IO.StreamReader.ReadToEnd*>siguiente se utiliza el archivo . Bloquea el subproceso actual para esperar el resultado. Este es un ejemplo de [sincronización sobre async](https://github.com/davidfowl/AspNetCoreDiagnosticScenarios/blob/master/AsyncGuidance.md#warning-sync-over-async
).

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/MyFirstController.cs?name=snippet1)]

En el código `Get` anterior, lee sincrónicamente todo el cuerpo de la solicitud HTTP en la memoria. Si el cliente se está cargando lentamente, la aplicación está realizando la sincronización a través de async. La aplicación se sincroniza sobre async porque Kestrel **NO** admite lecturas sincrónicas.

**Haga esto:** En el <xref:System.IO.StreamReader.ReadToEndAsync*> ejemplo siguiente se utiliza y no se bloquea el subproceso durante la lectura.

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/MyFirstController.cs?name=snippet2)]

El código anterior lee de forma asincrónica todo el cuerpo de la solicitud HTTP en la memoria.

> [!WARNING]
> Si la solicitud es grande, leer todo el cuerpo de la solicitud HTTP en la memoria podría dar lugar a una condición de memoria insuficiente (OOM). OOM puede dar lugar a una denegación de servicio.  Para obtener más información, consulte Evitar la lectura de cuerpos de [solicitud grandes o cuerpos](#arlb) de respuesta en la memoria de este documento.

**Haga esto:** El ejemplo siguiente es totalmente asincrónico mediante un cuerpo de solicitud no almacenado en búfer:

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/MyFirstController.cs?name=snippet3)]

El código anterior deserializa de forma asincrónica el cuerpo de la solicitud en un objeto de C.

## <a name="prefer-readformasync-over-requestform"></a>Prefiere ReadFormAsync sobre Request.Form

Use `HttpContext.Request.ReadFormAsync` en lugar de `HttpContext.Request.Form`.
`HttpContext.Request.Form`solo se puede leer de forma segura con las siguientes condiciones:

* El formulario ha sido leído `ReadFormAsync`por una llamada a , y
* El valor del formulario almacenado en caché se lee mediante`HttpContext.Request.Form`

**No haga lo siguiente:** En el `HttpContext.Request.Form`ejemplo siguiente se utiliza .  `HttpContext.Request.Form`utiliza [la sincronización a través de async](https://github.com/davidfowl/AspNetCoreDiagnosticScenarios/blob/master/AsyncGuidance.md#warning-sync-over-async
) y puede provocar la inanición del grupo de subprocesos.

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/MySecondController.cs?name=snippet1)]

**Haga esto:** En el `HttpContext.Request.ReadFormAsync` ejemplo siguiente se usa para leer el cuerpo del formulario de forma asincrónica.

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/MySecondController.cs?name=snippet2)]

<a name="arlb"></a>

## <a name="avoid-reading-large-request-bodies-or-response-bodies-into-memory"></a>Evite leer en la memoria grandes cuerpos de solicitud o cuerpos de respuesta

En .NET, cada asignación de objetos superior a 85 KB termina en el montón de objetos grandes ([LOH](https://blogs.msdn.microsoft.com/maoni/2006/04/19/large-object-heap/)). Los objetos grandes son caros de dos maneras:

* El costo de asignación es alto porque se debe borrar la memoria de un objeto grande recién asignado. Clr garantiza que se borra la memoria de todos los objetos recién asignados.
* LOH se recoge con el resto del montón. LOH requiere una recolección completa de [elementos no utilizados](/dotnet/standard/garbage-collection/fundamentals) o una [recolección Gen2.](/dotnet/standard/garbage-collection/fundamentals#generations)

Esta entrada de [blog](https://adamsitnik.com/Array-Pool/#the-problem) describe el problema sucintamente:

> Cuando se asigna un objeto grande, se marca como objeto Gen 2. No Gen 0 como para objetos pequeños. Las consecuencias son que si se queda sin memoria en LOH, GC limpia todo el montón administrado, no solo LOH. Así que limpia Gen 0, Gen 1 y Gen 2 incluyendo LOH. Esto se denomina recolección de elementos no utilizados completa y es la recolección de elementos no utilizados que consume más tiempo. Para muchas aplicaciones, puede ser aceptable. Pero definitivamente no para servidores web de alto rendimiento, donde se necesitan pocos búferes de memoria grandes para manejar una solicitud web promedio (leer desde un socket, descomprimir, decodificar JSON & más).

Almacenar ingenuamente una gran solicitud o `byte[]` cuerpo `string`de respuesta en un solo o :

* Puede resultar en quedarse rápidamente sin espacio en el LOH.
* Puede causar problemas de rendimiento para la aplicación debido a los archivos de nivel de rendimiento completos que se ejecutan.

## <a name="working-with-a-synchronous-data-processing-api"></a>Trabajar con una API de procesamiento de datos sincrónico

Cuando se utiliza un serializador/deserializador que solo admite lecturas y escrituras sincrónicas (por ejemplo, [JSON.NET):](https://www.newtonsoft.com/json/help/html/Introduction.htm)

* Almacenar en búfer los datos en la memoria de forma asincrónica antes de pasarlos al serializador/deserializador.

> [!WARNING]
> Si la solicitud es grande, podría dar lugar a una condición de memoria insuficiente (OOM). OOM puede dar lugar a una denegación de servicio.  Para obtener más información, consulte Evitar la lectura de cuerpos de [solicitud grandes o cuerpos](#arlb) de respuesta en la memoria de este documento.

ASP.NET Core 3.0 utiliza <xref:System.Text.Json> de forma predeterminada para la serialización JSON. <xref:System.Text.Json>:

* Lee y escribe JSON de forma asincrónica.
* Está optimizado para texto UTF-8.
* Normalmente, mayor rendimiento que `Newtonsoft.Json`.

## <a name="do-not-store-ihttpcontextaccessorhttpcontext-in-a-field"></a>No almacene IHttpContextAccessor.HttpContext en un campo

[El IHttpContextAccessor.HttpContext](xref:Microsoft.AspNetCore.Http.IHttpContextAccessor.HttpContext) `HttpContext` devuelve el de la solicitud activa cuando se tiene acceso desde el subproceso de solicitud. No `IHttpContextAccessor.HttpContext` **se** debe almacenar en un campo o variable.

**No haga lo siguiente:** En el ejemplo `HttpContext` siguiente se almacena el en un campo y, a continuación, intenta usarlo más adelante.

[!code-csharp[](performance-best-practices/samples/3.0/MyType.cs?name=snippet1)]

El código anterior captura con frecuencia `HttpContext` un null o incorrecto en el constructor.

**Haga esto:** El siguiente ejemplo:

* Almacena <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> el en un campo.
* Utiliza `HttpContext` el campo en el momento `null`correcto y comprueba el archivo .

[!code-csharp[](performance-best-practices/samples/3.0/MyType.cs?name=snippet2)]

## <a name="do-not-access-httpcontext-from-multiple-threads"></a>No acceda a HttpContext desde varios subprocesos

`HttpContext`*NO* es seguro para subprocesos. El `HttpContext` acceso desde varios subprocesos en paralelo puede dar lugar a un comportamiento indefinido, como bloqueos, bloqueos y daños en los datos.

**No haga lo siguiente:** En el ejemplo siguiente se realizan tres solicitudes paralelas y se registra la ruta de acceso de solicitud entrante antes y después de la solicitud HTTP saliente. Se tiene acceso a la ruta de acceso de la solicitud desde varios subprocesos, potencialmente en paralelo.

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/AsyncFirstController.cs?name=snippet1&highlight=25,28)]

**Haga esto:** En el ejemplo siguiente se copian todos los datos de la solicitud entrante antes de realizar las tres solicitudes paralelas.

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/AsyncFirstController.cs?name=snippet2&highlight=6,8,22,28)]

## <a name="do-not-use-the-httpcontext-after-the-request-is-complete"></a>No utilice HttpContext una vez completada la solicitud

`HttpContext`solo es válido siempre que haya una solicitud HTTP activa en la canalización ASP.NET Core. Toda la canalización de ASP.NET Core es una cadena asincrónica de delegados que ejecuta cada solicitud. Cuando `Task` se completa el devuelto `HttpContext` de esta cadena, se recicla.

**No haga lo siguiente:** En el `async void` ejemplo siguiente se utiliza, `await` lo que hace que la solicitud HTTP se complete cuando se alcanza la primera:

* Lo que **SIEMPRE** es una mala práctica en ASP.NET aplicaciones principales.
* Accede a `HttpResponse` la después de que se complete la solicitud HTTP.
* Bloquea el proceso.

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/AsyncBadVoidController.cs?name=snippet1)]

**Haga esto:** En el ejemplo `Task` siguiente se devuelve un al marco de trabajo, por lo que la solicitud HTTP no se completa hasta que se completa la acción.

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/AsyncSecondController.cs?name=snippet1)]

## <a name="do-not-capture-the-httpcontext-in-background-threads"></a>No capture el HttpContext en subprocesos en segundo plano

**No haga lo siguiente:** En el ejemplo siguiente se `HttpContext` muestra `Controller` un cierre que captura la propiedad desde la propiedad. Esta es una mala práctica porque el elemento de trabajo podría:

* Ejecutar fuera del ámbito de la solicitud.
* Intento de leer `HttpContext`el error .

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/FireAndForgetFirstController.cs?name=snippet1)]

**Haga esto:** El siguiente ejemplo:

* Copia los datos necesarios en la tarea en segundo plano durante la solicitud.
* No hace referencia a nada del controlador.

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/FireAndForgetFirstController.cs?name=snippet2)]

Las tareas en segundo plano deben implementarse como servicios hospedados. Para más información, consulte [Tareas en segundo plano con servicios hospedados](xref:fundamentals/host/hosted-services).

## <a name="do-not-capture-services-injected-into-the-controllers-on-background-threads"></a>No capture los servicios inyectados en los controladores en subprocesos en segundo plano

**No haga lo siguiente:** En el ejemplo siguiente se `DbContext` muestra `Controller` un cierre que captura el parámetro de acción desde. Esta es una mala práctica.  El elemento de trabajo podría ejecutarse fuera del ámbito de solicitud. El `ContosoDbContext` ámbito es la solicitud, `ObjectDisposedException`lo que resulta en un archivo .

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/FireAndForgetSecondController.cs?name=snippet1)]

**Haga esto:** El siguiente ejemplo:

* Inyecta un <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> para crear un ámbito en el elemento de trabajo en segundo plano. `IServiceScopeFactory`es un singleton.
* Crea un nuevo ámbito de inserción de dependencias en el subproceso en segundo plano.
* No hace referencia a nada del controlador.
* No captura la `ContosoDbContext` solicitud entrante.

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/FireAndForgetSecondController.cs?name=snippet2)]

El siguiente código resaltado:

* Crea un ámbito para la duración de la operación en segundo plano y resuelve los servicios desde ella.
* Se `ContosoDbContext` utiliza desde el ámbito correcto.

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/FireAndForgetSecondController.cs?name=snippet2&highlight=9-16)]

## <a name="do-not-modify-the-status-code-or-headers-after-the-response-body-has-started"></a>No modifique el código de estado o los encabezados después de que el cuerpo de la respuesta se haya iniciado

ASP.NET Core no almacena en búfer el cuerpo de la respuesta HTTP. La primera vez que se escribe la respuesta:

* Los encabezados se envían junto con ese fragmento del cuerpo al cliente.
* Ya no es posible cambiar los encabezados de respuesta.

**No haga lo siguiente:** El código siguiente intenta agregar encabezados de respuesta después de que la respuesta ya se ha iniciado:

[!code-csharp[](performance-best-practices/samples/3.0/Startup22.cs?name=snippet1)]

En el código `context.Response.Headers["test"] = "test value";` anterior, se `next()` producirá una excepción si se ha escrito en la respuesta.

**Haga esto:** En el ejemplo siguiente se comprueba si la respuesta HTTP se ha iniciado antes de modificar los encabezados.

[!code-csharp[](performance-best-practices/samples/3.0/Startup22.cs?name=snippet2)]

**Haga esto:** En el `HttpResponse.OnStarting` ejemplo siguiente se utiliza para establecer los encabezados antes de que los encabezados de respuesta se vacíen en el cliente.

La comprobación de si la respuesta no se ha iniciado permite registrar una devolución de llamada que se invocará justo antes de que se escriban los encabezados de respuesta. Comprobación de si la respuesta no se ha iniciado:

* Proporciona la capacidad de anexar o invalidar encabezados justo a tiempo.
* No requiere conocimiento del siguiente middleware en la canalización.

[!code-csharp[](performance-best-practices/samples/3.0/Startup22.cs?name=snippet3)]

## <a name="do-not-call-next-if-you-have-already-started-writing-to-the-response-body"></a>No llame a next() si ya ha comenzado a escribir al cuerpo de la respuesta

Los componentes solo esperan que se les llame si es posible que manejen y manipulen la respuesta.
