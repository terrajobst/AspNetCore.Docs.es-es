---
uid: mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4
title: Usar métodos asincrónicos en ASP.NET MVC 4 | Microsoft Docs
author: Rick-Anderson
description: Este tutorial le enseñará los aspectos básicos de la creación de una aplicación Web de ASP.NET MVC asincrónica mediante Visual Studio Express 2012 para Web, que es un libre ve...
ms.author: aspnetcontent
ms.date: 06/06/2012
ms.assetid: a56572ba-81c3-47af-826d-941e9c4775ec
msc.legacyurl: /mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 5eb1b6dc3e9166c1ecb8e0d1968e12e8fa07f985
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37826405"
---
<a name="using-asynchronous-methods-in-aspnet-mvc-4"></a>Usar métodos asincrónicos en ASP.NET MVC 4
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> Este tutorial le enseñará los aspectos básicos de la creación de una aplicación Web de ASP.NET MVC asincrónico con [Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/11), que es una versión gratuita de Microsoft Visual Studio. También puede usar [Visual Studio 2012](https://www.microsoft.com/visualstudio/11).
> 
> Se proporciona un ejemplo completo para este tutorial en github  [https://github.com/RickAndMSFT/Async-ASP.NET/](https://github.com/RickAndMSFT/Async-ASP.NET/)


El 4 de ASP.NET MVC [controlador](https://msdn.microsoft.com/library/system.web.mvc.controller(VS.108).aspx) clase de combinación [.NET 4.5](https://msdn.microsoft.com/library/w0x726c2(VS.110).aspx) le permite escribir métodos de acción asincrónicos que devuelven un objeto de tipo [tarea&lt;ActionResult&gt; ](https://msdn.microsoft.com/library/dd321424(VS.110).aspx). .NET Framework 4 introdujo un concepto de programación asincrónico se denomina un [tarea](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) y es compatible con ASP.NET MVC 4 [tarea](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx). Las tareas se representan mediante el **tarea** tipo y los tipos relacionados en el [System.Threading.Tasks](https://msdn.microsoft.com/library/system.threading.tasks.aspx) espacio de nombres. .NET Framework 4.5 se basa en esta compatibilidad asincrónica con el [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) y [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) palabras clave que facilitan el trabajo con [tarea](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) objetos mucho menos complejos que la anterior métodos asincrónicos. El [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) palabra clave es la forma abreviada sintáctica presente para indicar que un fragmento de código debe esperar asincrónicamente en algún otro elemento de código. El [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) palabra clave representa una sugerencia que puede usar para marcar métodos como métodos asincrónicos basados en tareas. La combinación de **await**, **async**y el **tarea** objeto facilita en gran medida para que pueda escribir código asincrónico en .NET 4.5. El nuevo modelo para los métodos asincrónicos se denomina el *modelo asincrónico basado en tareas* (**pulse**). En este tutorial se da por supuesto que tiene cierta familiaridad con el uso de programación asincrónica [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) y [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) palabras clave y el [tarea](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) espacio de nombres.

Para obtener más información sobre el uso de la [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) y [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) palabras clave y el [tarea](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) espacio de nombres, vea las siguientes referencias.

- [Notas del producto: Asincronía en .NET](https://go.microsoft.com/fwlink/?LinkId=204844)
- [Preguntas más frecuentes sobre Async y Await](https://blogs.msdn.com/b/pfxteam/archive/2012/04/12/10293335.aspx)
- [Programación asincrónica de Visual Studio](https://msdn.microsoft.com/vstudio/gg316360)

## <a id="HowRequestsProcessedByTP"></a>  Cómo se procesan las solicitudes por el grupo de subprocesos

En el servidor web, .NET Framework mantiene un grupo de subprocesos que se usan para atender las solicitudes ASP.NET. Cuando llega una solicitud, se envía un subproceso del grupo para procesar la solicitud. Si la solicitud se procesa de forma sincrónica, el subproceso que procesa la solicitud está ocupado mientras la solicitud se está procesando y que el subproceso no puede atender otra solicitud.   
  
Esto podría no ser un problema, ya que se pueden hacer lo suficientemente grande como para dar cabida a muchos subprocesos ocupados del grupo de subprocesos. Sin embargo, se limita el número de subprocesos en el grupo de subprocesos (el valor máximo predeterminado de .NET 4.5 es 5000). En aplicaciones de gran tamaño con la simultaneidad alto de solicitudes de ejecución prolongada, todos los subprocesos disponibles podrían estar ocupados. Esta condición se conoce como la falta de subprocesos. Cuando se alcanza esta condición, el servidor web pone en cola las solicitudes. Si la cola de solicitudes se llena, el servidor web rechaza las solicitudes con el estado HTTP 503 (Server Too Busy). El grupo de subprocesos CLR tiene limitaciones en las inyecciones de subproceso nuevo. Si la simultaneidad es por ráfagas (es decir, el sitio web, de repente, puede obtener un gran número de solicitudes) y todos los subprocesos de solicitud disponibles están ocupados debido a llamadas de back-end con una latencia elevada, la tasa de inserción de subproceso limitado puede hacer que la aplicación responda muy mal. Además, cada subproceso nuevo agregado al grupo de subprocesos tiene sobrecarga (por ejemplo, 1 MB de memoria de pila). Una aplicación web mediante los métodos sincrónicos a las llamadas de alta latencia de servicio donde el grupo de subprocesos aumenta con el valor predeterminado de .NET 4.5 admite un máximo de 5 000 subprocesos consumiría aproximadamente 5 GB más memoria que una aplicación que pueda el servicio de la misma solicitudes con los métodos asincrónicos y solo 50 subprocesos. Cuando está realizando trabajo asincrónico, no siempre usa un subproceso. Por ejemplo, al realizar una solicitud de servicio web asincrónico, ASP.NET no usará los subprocesos entre el **async** llamada al método y el **await**. Usar el grupo de subprocesos para atender las solicitudes con una latencia elevada puede dar lugar a una superficie de memoria grande y una mala utilización del hardware del servidor.

## <a name="processing-asynchronous-requests"></a>Procesar solicitudes asincrónicas

En una aplicación web que ve un gran número de solicitudes simultáneas en el inicio o que tenga una carga por ráfagas (donde simultaneidad aumenta repentinamente), realizar llamadas al servicio web asincrónico aumenta la capacidad de respuesta de la aplicación. Una solicitud asincrónica tarda la misma cantidad de tiempo en procesarse que una solicitud sincrónica. Si una solicitud realiza un servicio web de llamada requiere dos segundos en completarse, la solicitud tardará dos segundos si se realiza de forma sincrónica o asincrónica. Sin embargo durante una llamada asincrónica, no se bloquea un subproceso deja de responder a otras solicitudes mientras espera a que finalice la primera solicitud. Por lo tanto, las solicitudes asincrónicas evitar el crecimiento de grupo de puesta en cola y el subproceso de solicitud cuando hay muchas solicitudes simultáneas que invocan las operaciones de larga ejecución.

## <a id="ChoosingSyncVasync"></a>  Elegir los métodos de acción sincrónicos o asincrónicos

Esta sección enumeran las directrices sobre cuándo usar métodos de acción sincrónicos o asincrónicos. Se trata de meras directrices; Examine individualmente cada aplicación para determinar si los métodos asincrónicos ayudar con el rendimiento.

En general, use los métodos sincrónicos para las condiciones siguientes:

- Las operaciones son simples o de ejecución breve.
- La simplicidad es más importante que la eficacia.
- Las operaciones son principalmente operaciones de la CPU en lugar de las operaciones que implican una amplia disco o una sobrecarga de la red. Uso de métodos de acción asincrónicos en operaciones relacionadas con la CPU no proporciona ninguna ventaja y da lugar a mayor sobrecarga.

  En general, utilice los métodos asincrónicos para las condiciones siguientes:

- Se están llamando a los servicios que se pueden consumir mediante los métodos asincrónicos, y usa .NET 4.5 o superior.
- Las operaciones son enlazada a la red o puedo enlazadas en lugar de a la CPU.
- El paralelismo es más importante que la simplicidad de código.
- Desea proporcionar un mecanismo que permite a los usuarios cancelar una solicitud de ejecución prolongada.
- Cuando la ventaja de la conmutación de subprocesos supera el costo de cambiar el contexto. En general, debe convertir un método asincrónico si espera a que el método sincrónico en el subproceso de solicitud ASP.NET mientras no se realiza ningún trabajo. Al realizar la llamada asincrónica, el subproceso de solicitud ASP.NET no está detenido realizando ningún trabajo mientras espera a que finalice la solicitud de servicio web.
- Las pruebas muestran que las operaciones bloqueos son un cuello de botella de rendimiento del sitio y que IIS puede atender más solicitudes mediante el uso de métodos asincrónicos para estas llamadas bloqueos.

  El ejemplo descargable muestra cómo utilizar métodos de acción asincrónicos de forma eficaz. El ejemplo proporcionado se diseñó para proporcionar una simple demostración de la programación asincrónica en ASP.NET MVC 4 mediante .NET 4.5. El ejemplo no pretende ser una arquitectura de referencia para la programación asincrónica en ASP.NET MVC. El programa de ejemplo llama a [ASP.NET Web API](../../../web-api/index.md) métodos que a su vez llaman a [Task.Delay](https://msdn.microsoft.com/library/hh139096(VS.110).aspx) para simular las llamadas al servicio web de ejecución prolongada. La mayoría de las aplicaciones de producción no mostrarán ventajas tan evidentes del uso de métodos de acción asincrónicos.   
  
Pocas aplicaciones exigen que todos los métodos de acción sean asincrónicos. A menudo, proporciona la máxima eficacia para la cantidad de trabajo necesario convertir algunos métodos de acción sincrónicos en métodos asincrónicos.

## <a id="SampleApp"></a>  La aplicación de ejemplo

Puede descargar la aplicación de ejemplo de [ https://github.com/RickAndMSFT/Async-ASP.NET/ ](https://github.com/RickAndMSFT/Async-ASP.NET) en el [GitHub](https://github.com/) sitio. El repositorio consta de tres proyectos:

- *Mvc4Async*: ASP.NET MVC 4 el proyecto que contiene el código usado en este tutorial. Realiza llamadas de API Web a la **WebAPIpgw** service.
- *WebAPIpgw*: proyecto de ASP.NET MVC 4 Web API que implementa el `Products, Gizmos and Widgets` controladores. Proporciona los datos para el *WebAppAsync* proyecto y el *Mvc4Async* proyecto.
- *WebAppAsync*: proyecto de ASP.NET Web Forms el utilizado en otro tutorial.

## <a id="GizmosSynch"></a>  El método de acción sincrónico Gizmos

 El código siguiente muestra el `Gizmos` método de acción sincrónico que se usa para mostrar una lista de gizmos. (En este artículo, un artículo tiene un es un dispositivo mecánico ficticio). 

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample1.cs)]

El código siguiente muestra el `GetGizmos` método del artículo tiene un servicio.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample2.cs)]

El `GizmoService GetGizmos` método pasa un URI a un servicio ASP.NET Web API HTTP que devuelve una lista de los datos gizmos. El *WebAPIpgw* proyecto contiene la implementación de la API Web `gizmos, widget` y `product` controladores.  
La siguiente imagen muestra la vista gizmos desde el proyecto de ejemplo.

![Gizmos](using-asynchronous-methods-in-aspnet-mvc-4/_static/image1.png)

## <a id="CreatingAsynchGizmos"></a>  Creación de un método de acción asincrónico Gizmos

El ejemplo usa el nuevo [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) y [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) palabras clave (disponibles en .NET 4.5 y Visual Studio 2012) para permitir que el compilador ser responsable de mantener las complicadas transformaciones necesarias para programación asincrónica. El compilador le permite escribir código mediante que construcciones de flujo de control sincrónico de la de C# y el compilador aplica automáticamente las transformaciones necesarias para utilizar devoluciones de llamada con el fin de evitar el bloqueo de subprocesos.

El siguiente código muestra la `Gizmos` método sincrónico y el `GizmosAsync` método asincrónico. Si el explorador admite el [HTML 5 `<mark>` elemento](http://www.w3.org/wiki/HTML/Elements/mark), verá los cambios en `GizmosAsync` en amarillo resaltado.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample3.cs)]

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample4.cs?highlight=1,3,5)]

 Se aplicaron los cambios siguientes para permitir la `GizmosAsync` sea asincrónico.

- El método está marcado con el [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) palabra clave, que indica al compilador que genere devoluciones de llamada para partes del cuerpo y crear automáticamente un `Task<ActionResult>` que se devuelve.
- &quot;Async&quot; se anexa al nombre del método. Anexa "Async" no es necesario, pero es la convención al escribir métodos asincrónicos.
- Se ha cambiado el tipo de valor devuelto de `ActionResult` a `Task<ActionResult>`. El tipo de valor devuelto de `Task<ActionResult>` representa el trabajo en curso y proporciona a los llamadores del método con un identificador a través de la que se va a esperar la finalización de la operación asincrónica. En este caso, el llamador es el servicio web. `Task<ActionResult>` en curso representa trabaja con un resultado de `ActionResult.`
- El [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) palabra clave se aplicó a la llamada al servicio web.
- Se llamó a la API del servicio web asincrónico (`GetGizmosAsync`).

Dentro de la `GetGizmosAsync` otro método asincrónico, de cuerpo de método `GetGizmosAsync` se llama. `GetGizmosAsync` se devuelve inmediatamente un `Task<List<Gizmo>>` que finalmente se completará cuando los datos están disponibles. Dado que no desea hacer nada más hasta que tenga los datos de artículo tiene un, el código espera la tarea (mediante el **await** palabra clave). Puede usar el **await** palabra clave solo en los métodos anotados con el **async** palabra clave.

El **await** palabra clave no bloquea el subproceso hasta que se complete la tarea. El resto del método se registra como devolución de llamada en la tarea y se devuelve inmediatamente. Cuando finaliza la tarea en espera, se invocan esa devolución de llamada y, por tanto, reanudar la ejecución de la derecha del método donde se quedó. Para obtener más información sobre el uso de la [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) y [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) palabras clave y el [tarea](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) espacio de nombres, vea el [async referencias](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/async).

El siguiente código muestra la `GetGizmos` y `GetGizmosAsync` métodos.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample5.cs)]

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample6.cs?highlight=1,4-8)]

 Los cambios asincrónicos son similares a los realizados en el **GizmosAsync** anteriormente. 

- La firma del método se anota con el [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) palabra clave, el tipo de valor devuelto se cambió a `Task<List<Gizmo>>`, y *Async* se anexa al nombre del método.
- Asincrónico [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx) clase se utiliza en lugar de la [WebClient](https://msdn.microsoft.com/library/system.net.webclient.aspx) clase.
- El [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) palabra clave se aplicó a la [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx) métodos asincrónicos.

La siguiente imagen muestra la vista asincrónica artículo tiene un.

![async](using-asynchronous-methods-in-aspnet-mvc-4/_static/image2.png)

La presentación de los exploradores de los datos gizmos es idéntica a la vista creada por la llamada sincrónica. La única diferencia es que la versión asincrónica puede ser un mejor rendimiento con cargas pesadas.

## <a id="Parallel"></a>  Realizar varias operaciones en paralelo

Métodos de acción asincrónicos tienen una ventaja significativa sobre los métodos sincrónicos cuando una acción debe realizar varias operaciones independientes. En el ejemplo proporcionado, el método sincrónico `PWG`(para los productos, Widgets y Gizmos) muestra los resultados de tres llamadas al servicio web para obtener una lista de productos, widgets y gizmos. El [ASP.NET Web API](../../../web-api/index.md) proyecto que proporciona estos servicios usa [Task.Delay](https://msdn.microsoft.com/library/hh139096(VS.110).aspx) para simular la red lenta o latencia de llamadas. Cuando se establece el retraso de 500 milisegundos, asincrónicos `PWGasync` método toma un poco más de 500 milisegundos en completarse mientras sincrónico `PWG` versión asume 1.500 milisegundos. Sincrónico `PWG` método se muestra en el código siguiente.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample7.cs)]

Asincrónico `PWGasync` método se muestra en el código siguiente.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample8.cs?highlight=1,3,12)]

La siguiente imagen muestra la vista devuelta por la **PWGasync** método.

![pwgAsync](using-asynchronous-methods-in-aspnet-mvc-4/_static/image3.png)

## <a id="CancelToken"></a>  Uso de un Token de cancelación

Los métodos de acción asincrónicos que devuelven `Task<ActionResult>`son cancelable, que se toman un [CancellationToken](https://msdn.microsoft.com/library/system.threading.cancellationtoken(VS.110).aspx) parámetro cuando se proporciona con el [AsyncTimeout](https://msdn.microsoft.com/library/system.web.mvc.asynctimeoutattribute(VS.108).aspx) atributo. El código siguiente muestra el `GizmosCancelAsync` método con un tiempo de espera de 150 milisegundos.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample9.cs?highlight=1-3,5,10)]

El código siguiente muestra la sobrecarga GetGizmosAsync, que toma un [CancellationToken](https://msdn.microsoft.com/library/system.threading.cancellationtoken(VS.110).aspx) parámetro.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample10.cs)]

En la aplicación de ejemplo proporcionada, seleccionando la *demostración de Token de cancelación* vincular llamadas la `GizmosCancelAsync` método y demuestra la cancelación de la llamada asincrónica.

## <a id="ServerConfig"></a>  Configuración del servidor para llamadas de servicio Web de alta simultaneidad y alta latencia

Para obtener los beneficios de una aplicación web asincrónica, es posible que deba realizar algunos cambios en la configuración predeterminada del servidor. Tenga en cuenta al configurar y probar la aplicación web asíncronos de esfuerzo lo siguiente.

- Windows 7, Windows Vista y todos los sistemas operativos de cliente de Windows tiene un máximo de 10 solicitudes simultáneas. Necesitará un sistema operativo de Windows Server para ver las ventajas de los métodos asincrónicos con una carga elevada.
- Registrar .NET 4.5 con IIS desde un símbolo del sistema con privilegios elevados:  
  %windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet\_regiis -i.  
  Consulte [herramienta de registro de IIS en ASP.NET (Aspnet\_regiis.exe)](https://msdn.microsoft.com/library/k6h9cz8h.aspx)
- Es posible que deba aumentar el [HTTP.sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture) límite de la cola desde el valor predeterminado de 1.000 a 5.000. Si el valor es demasiado bajo, es posible que vea [HTTP.sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture) rechazar las solicitudes con el estado HTTP 503. Para cambiar el límite de cola de HTTP.sys:

    - Abra el Administrador de IIS y navegue hasta el panel grupos de aplicaciones.
    - Haga clic con el botón derecho en el grupo de aplicaciones de destino y seleccione **configuración avanzada**.  
        ![Avanzada](using-asynchronous-methods-in-aspnet-mvc-4/_static/image4.png)
    - En el **configuración avanzada** cuadro de diálogo, cambie *longitud de cola* de 1.000 a 5.000.  
        ![Longitud de cola](using-asynchronous-methods-in-aspnet-mvc-4/_static/image5.png)  
  
  Tenga en cuenta que en las imágenes anteriores, .NET framework se incluye como v4.0, aunque el grupo de aplicaciones es usar .NET 4.5. Para entender esta discrepancia, consulte lo siguiente:

    - [Control de versiones de .NET y .NET Framework 4.5 Multi-Targeting - es una actualización en contexto a .NET 4.0](http://www.hanselman.com/blog/NETVersioningAndMultiTargetingNET45IsAnInplaceUpgradeToNET40.aspx)
    - [Establecimiento de una aplicación IIS o grupo de aplicaciones para usar ASP.NET 3.5, en lugar de 2.0](http://www.hanselman.com/blog/HowToSetAnIISApplicationOrAppPoolToUseASPNET35RatherThan20.aspx)
    - [Versiones y dependencias de .NET Framework](https://msdn.microsoft.com/library/bb822049(VS.110).aspx)
- Si la aplicación está utilizando servicios web o System.NET para comunicarse con un back-end a través de HTTP que necesite aumentar la [connectionManagement/maxconnection](https://msdn.microsoft.com/library/fb6y0fyc(VS.110).aspx) elemento. Para las aplicaciones de ASP.NET, esto está limitado por la característica de configuración automática para 12 veces el número de CPU. Esto significa que en cuádruple, puede tener como máximo 12 \* 4 = 48 conexiones simultáneas a un punto de conexión IP. Dado que esta está asociada a [autoConfig](https://msdn.microsoft.com/library/7w2sway1(VS.110).aspx), la manera más fácil aumentar `maxconnection` en ASP.NET, aplicación consiste en establecer [System.Net.ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit(VS.110).aspx) mediante programación en el de `Application_Start` método en el *global.asax* archivo. Consulte el ejemplo de descarga para obtener un ejemplo.
- En .NET 4.5, el valor predeterminado de 5000 para [MaxConcurrentRequestsPerCPU](https://blogs.msdn.com/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx) debiera estar bien.
