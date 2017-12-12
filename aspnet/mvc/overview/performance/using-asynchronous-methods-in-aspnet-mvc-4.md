---
uid: mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4
title: "Usar métodos asincrónicos en ASP.NET MVC 4 | Documentos de Microsoft"
author: Rick-Anderson
description: "Este tutorial le enseñará los aspectos básicos de la creación de una aplicación Web de ASP.NET MVC asincrónica mediante Visual Studio Express 2012 para Web, que es un tegorías libre..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/06/2012
ms.topic: article
ms.assetid: a56572ba-81c3-47af-826d-941e9c4775ec
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 6a0c8dbbd02549757c316807b8e8e64fdfd70123
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="using-asynchronous-methods-in-aspnet-mvc-4"></a>Usar métodos asincrónicos en ASP.NET MVC 4
====================
Por [Rick Anderson](https://github.com/Rick-Anderson)

> Este tutorial le enseñará los aspectos básicos de la creación de una aplicación Web de ASP.NET MVC asincrónica utilizando [Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/11/en-us), que es una versión gratuita de Microsoft Visual Studio. También puede usar [Visual Studio 2012](https://www.microsoft.com/visualstudio/11/en-us).

> Se proporciona un ejemplo completo para este tutorial en github [https://github.com/RickAndMSFT/Async-ASP.NET/](https://github.com/RickAndMSFT/Async-ASP.NET/)


ASP.NET MVC 4 [controlador](https://msdn.microsoft.com/en-us/library/system.web.mvc.controller(VS.108).aspx) clase de combinación [.NET 4.5](https://msdn.microsoft.com/en-us/library/w0x726c2(VS.110).aspx) le permite escribir métodos de acción asincrónicos que devuelven un objeto del tipo [tarea&lt;ActionResult&gt; ](https://msdn.microsoft.com/en-us/library/dd321424(VS.110).aspx). .NET Framework 4 se introdujo un concepto de programación asincrónico conocido como un [tarea](https://msdn.microsoft.com/en-us/library/system.threading.tasks.task.aspx) y es compatible con ASP.NET MVC 4 [tarea](https://msdn.microsoft.com/en-us/library/system.threading.tasks.task.aspx). Las tareas se representan por el **tarea** tipo y los tipos relacionados en el [System.Threading.Tasks](https://msdn.microsoft.com/en-us/library/system.threading.tasks.aspx) espacio de nombres. .NET Framework 4.5 se basa en esta compatibilidad asincrónica con el [await](https://msdn.microsoft.com/en-us/library/hh156528(VS.110).aspx) y [async](https://msdn.microsoft.com/en-us/library/hh156513(VS.110).aspx) palabras clave que facilitan el trabajo con [tarea](https://msdn.microsoft.com/en-us/library/system.threading.tasks.task.aspx) objetos mucho menos complejos que anterior métodos asincrónicos. El [await](https://msdn.microsoft.com/en-us/library/hh156528(VS.110).aspx) palabra clave es una abreviatura sintáctica para indicar que debe esperar un fragmento de código de forma asincrónica en alguna otra parte del código. El [async](https://msdn.microsoft.com/en-us/library/hh156513(VS.110).aspx) palabra clave representa una sugerencia que puede usar para marcar los métodos como métodos asincrónicos basado en tareas. La combinación de **await**, **async**y el **tarea** objeto, resulta más fácil de escribir código asincrónico en .NET 4.5. El nuevo modelo para los métodos asincrónicos se denomina la *modelo asincrónico basado en tareas* (**pulse**). Este tutorial se supone que dispone de cierta familiaridad con el uso de programación asincrónica [await](https://msdn.microsoft.com/en-us/library/hh156528(VS.110).aspx) y [async](https://msdn.microsoft.com/en-us/library/hh156513(VS.110).aspx) palabras clave y el [tarea](https://msdn.microsoft.com/en-us/library/system.threading.tasks.task.aspx) espacio de nombres.

Para obtener más información sobre el uso de la [await](https://msdn.microsoft.com/en-us/library/hh156528(VS.110).aspx) y [async](https://msdn.microsoft.com/en-us/library/hh156513(VS.110).aspx) palabras clave y el [tarea](https://msdn.microsoft.com/en-us/library/system.threading.tasks.task.aspx) espacio de nombres, vea las siguientes referencias.

- [Notas del producto: Asincronía en .NET](https://go.microsoft.com/fwlink/?LinkId=204844)
- [Preguntas más frecuentes de Async y Await](https://blogs.msdn.com/b/pfxteam/archive/2012/04/12/10293335.aspx)
- [Programación asincrónica de Visual Studio](https://msdn.microsoft.com/en-us/vstudio/gg316360)

## <a id="HowRequestsProcessedByTP"></a>Cómo se procesan las solicitudes por el grupo de subprocesos

En el servidor web, .NET Framework mantiene un grupo de subprocesos que se usan para atender las solicitudes ASP.NET. Cuando llega una solicitud, se envía un subproceso del grupo para procesar la solicitud. Si la solicitud se procesa de forma sincrónica, el subproceso que procesa la solicitud está ocupado mientras la solicitud se está procesando y que el subproceso no puede atender otra solicitud.   
  
Esto podría no ser un problema, porque el grupo de subprocesos se puede establecer lo suficientemente grande como para dar cabida a muchos subprocesos ocupados. Sin embargo, se limita el número de subprocesos en el grupo de subprocesos (el valor máximo de .NET 4.5 predeterminado es 5000). En las aplicaciones grandes con gran simultaneidad de las solicitudes de ejecución prolongada, todos los subprocesos disponibles pueden estar ocupados. Esta condición se conoce como la falta de subprocesos. Cuando se alcanza esta condición, el servidor web pone en cola las solicitudes. Si la cola de solicitudes se llena, el servidor web rechaza las solicitudes con estado HTTP 503 (Server Too Busy). El grupo de subprocesos CLR tiene limitaciones en inyecciones de nuevo subproceso. Si la simultaneidad es por ráfagas (es decir, el sitio web repentinamente puede obtener un gran número de solicitudes) y todos los subprocesos de solicitud disponibles están ocupados debido a llamadas de back-end con latencia alta, la velocidad de inserción limitado de subprocesos puede hacer que la aplicación responda muy bajo. Además, cada subproceso nuevo que se agrega al grupo de subprocesos tiene sobrecarga (por ejemplo, 1 MB de memoria de la pila). Una aplicación web con métodos sincrónicos para llamadas de latencia alta del servicio en el grupo de subprocesos aumenta con el valor predeterminado de .NET 4.5 admite un máximo de 5 000 subprocesos consumiría aproximadamente 5 GB más memoria que una aplicación pueda el servicio el mismo solicita utilizando métodos asincrónicos y solo 50 subprocesos. Al realizar el trabajo asincrónico, no siempre está usando un subproceso. Por ejemplo, al realizar una solicitud de servicio web asincrónico, ASP.NET no esté usando todos los subprocesos entre el **async** llamada al método y el **await**. Usar el grupo de subprocesos para atender las solicitudes con una latencia elevada puede provocar un consumo de grandes cantidades de memoria y una mala utilización del hardware del servidor.

## <a name="processing-asynchronous-requests"></a>Procesar solicitudes asincrónicas

En las aplicaciones web que ve un gran número de solicitudes simultáneas en el inicio o que tenga una carga por ráfagas (donde simultaneidad aumenta repentinamente), que realiza estas llamadas al servicio web asincrónica, aumentará la capacidad de respuesta de la aplicación. Una solicitud asincrónica tarda la misma cantidad de tiempo en procesarse que una solicitud sincrónica. Por ejemplo, si una solicitud realiza un servicio web llamada requiere dos segundos en completarse, la solicitud tardará dos segundos si se realiza de forma sincrónica o asincrónica. Sin embargo, durante una llamada asincrónica, no se bloquea un subproceso de responder a otras solicitudes mientras espera a que la primera solicitud que se complete. Por lo tanto, solicitudes asincrónicas evita el crecimiento del grupo de puesta en cola y el subproceso de solicitud cuando hay muchas solicitudes simultáneas que invocan operaciones de ejecución prolongada.

## <a id="ChoosingSyncVasync"></a>Elegir los métodos de acción sincrónicos o asincrónicos

En esta sección se muestra las directrices para decidir cuándo utilizar métodos de acción sincrónicos o asincrónicos. Se trata de meras directrices; Examinar individualmente cada aplicación para determinar si los métodos asincrónicos incrementar el rendimiento.

En general, utilice los métodos sincrónicos para las siguientes condiciones:

- Las operaciones son simples o de ejecución breve.
- La simplicidad es más importante que la eficacia.
- Las operaciones son principalmente operaciones de la CPU en lugar de las operaciones que implican una amplia disco o una sobrecarga de la red. Usar métodos de acción asincrónicos en operaciones relacionadas con la CPU no proporciona ninguna ventaja y da lugar a mayor sobrecarga.

 En general, utilice métodos asincrónicos en las siguientes condiciones:

- Se están llamando a los servicios que se pueden utilizar en métodos asincrónicos, y usa .NET 4.5 o posterior.
- Las operaciones están relacionadas con la red o enlazadas a E/s en lugar de límite de CPU.
- El paralelismo es más importante que la simplicidad de código.
- Desea proporcionar un mecanismo que permite a los usuarios cancelar una solicitud de ejecución prolongada.
- Cuando la ventaja de cambiar subprocesos out compensa el costo de cambio de contexto. En general, debe crear un método asincrónico si espera a que el método sincrónico en el subproceso de solicitud ASP.NET mientras no realiza ningún trabajo. Al realizar la llamada asincrónica, el subproceso de solicitud ASP.NET está detenido no realiza ningún trabajo mientras se espera para que la solicitud de servicio web que se complete.
- Las pruebas muestran que las operaciones bloqueo son un cuello de botella en el rendimiento del sitio y que IIS puede prestar servicio a más solicitudes mediante el uso de métodos asincrónicos para estas llamadas que causan bloqueos.

 El ejemplo descargable muestra cómo utilizar métodos de acción asincrónicos de forma eficaz. El ejemplo proporcionado se diseñó para proporcionar una sencilla demostración de la programación asincrónica en ASP.NET MVC 4 mediante .NET 4.5. El ejemplo no pretende ser una arquitectura de referencia para la programación asincrónica en ASP.NET MVC. El programa de ejemplo llama [ASP.NET Web API](../../../web-api/index.md) métodos que a su vez llaman a [Task.Delay](https://msdn.microsoft.com/en-us/library/hh139096(VS.110).aspx) para simular llamadas al servicio web de ejecución prolongada. La mayoría de las aplicaciones de producción no mostrará ventajas tan evidentes del uso de métodos de acción asincrónicos.   
  
Pocas aplicaciones exigen que todos los métodos de acción sean asincrónicos. A menudo, proporciona la máxima eficacia para la cantidad de trabajo necesario convertir algunos métodos de acción sincrónicos en métodos asincrónicos.

## <a id="SampleApp"></a>La aplicación de ejemplo

Puede descargar la aplicación de ejemplo de [https://github.com/RickAndMSFT/Async-ASP.NET/](https://github.com/RickAndMSFT/Async-ASP.NET) en el [GitHub](https://github.com/) sitio. El repositorio consta de tres proyectos:

- *Mvc4Async*: ASP.NET MVC 4 el proyecto que contiene el código utilizado en este tutorial. Realiza llamadas de API Web a la **WebAPIpgw** servicio.
- *WebAPIpgw*: proyecto de la API Web de ASP.NET MVC 4 que implementa el `Products, Gizmos and Widgets` controladores. Proporciona los datos para la *WebAppAsync* proyecto y la *Mvc4Async* proyecto.
- *WebAppAsync*: proyecto de ASP.NET Web Forms el utilizado en otro tutorial.

## <a id="GizmosSynch"></a>El método de acción sincrónico chismes

 El código siguiente muestra el `Gizmos` método de acción sincrónico que se utiliza para mostrar una lista de gizmos. (En este artículo, un artículo tiene un es un dispositivo mecánico ficticio). 

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample1.cs)]

El código siguiente muestra el `GetGizmos` método del artículo tiene un servicio.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample2.cs)]

El `GizmoService GetGizmos` método pasa un URI a un servicio ASP.NET Web API HTTP que devuelve una lista de datos de gizmos. El *WebAPIpgw* proyecto contiene la implementación de la API Web `gizmos, widget` y `product` controladores.  
La siguiente imagen muestra la vista chismes desde el proyecto de ejemplo.

![Chismes](using-asynchronous-methods-in-aspnet-mvc-4/_static/image1.png)

## <a id="CreatingAsynchGizmos"></a>Crear un método de acción asincrónico chismes

El ejemplo usa el nuevo [async](https://msdn.microsoft.com/en-us/library/hh156513(VS.110).aspx) y [await](https://msdn.microsoft.com/en-us/library/hh156528(VS.110).aspx) palabras clave (disponible en .NET 4.5 y Visual Studio 2012) para permitir que el compilador ser responsable de mantener los necesarios para las transformaciones complejas programación asincrónica. El compilador le permite escribir código mediante que construcciones de flujo de control sincrónico de la de C# y el compilador aplica automáticamente las transformaciones necesarias para usar devoluciones de llamada con el fin de evitar el bloqueo de subprocesos.

El código siguiente muestra el `Gizmos` método sincrónico y `GizmosAsync` método asincrónico. Si el explorador admite el [HTML 5 `<mark>` elemento](http://www.w3.org/wiki/HTML/Elements/mark), podrá ver los cambios en `GizmosAsync` en amarillo resaltado.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample3.cs)]

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample4.cs?highlight=1,3,5)]

 Se aplicaron los cambios siguientes para permitir la `GizmosAsync` sea asincrónico.

- El método se marca con la [async](https://msdn.microsoft.com/en-us/library/hh156513(VS.110).aspx) palabra clave, que indica al compilador para generar las devoluciones de llamada para las partes del cuerpo y para crear automáticamente un `Task<ActionResult>` que se devuelve.
- &quot;Async&quot; se anexa al nombre del método. Anexa "Async" no es necesario, pero es la convención al escribir métodos asincrónicos.
- Se cambió el tipo de valor devuelto de `ActionResult` a `Task<ActionResult>`. El tipo de valor devuelto de `Task<ActionResult>` representa el trabajo en curso y proporciona a los llamadores del método con un identificador a través de la que se va a esperar a que finalice la operación asincrónica. En este caso, el llamador es el servicio web. `Task<ActionResult>`representa en curso trabaja con un resultado de`ActionResult.`
- El [await](https://msdn.microsoft.com/en-us/library/hh156528(VS.110).aspx) palabra clave se aplicó al llamar al servicio web.
- Se llamó a la API del servicio web asincrónica (`GetGizmosAsync`).

Dentro de la `GetGizmosAsync` otro método asincrónico, del cuerpo del método `GetGizmosAsync` se llama. `GetGizmosAsync`devuelve inmediatamente un `Task<List<Gizmo>>` que finalmente se completará cuando los datos están disponibles. Dado que no desea hacer nada más, hasta que tenga los datos de artículo tiene un, el código espera la tarea (mediante el **await** palabra clave). Puede usar el **await** palabra clave solo en los métodos anotados con el **async** palabra clave.

El **await** palabra clave no bloquea el subproceso hasta que se complete la tarea. Se suscribe el resto del método como una devolución de llamada en la tarea y devuelve inmediatamente. Cuando finalmente se completa la tarea en espera, se invoque esa devolución de llamada y, por tanto, reanudar la ejecución de la derecha del método donde se quedó. Para obtener más información sobre el uso de la [await](https://msdn.microsoft.com/en-us/library/hh156528(VS.110).aspx) y [async](https://msdn.microsoft.com/en-us/library/hh156513(VS.110).aspx) palabras clave y el [tarea](https://msdn.microsoft.com/en-us/library/system.threading.tasks.task.aspx) espacio de nombres, vea la [async referencias](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/async).

El código siguiente muestra el `GetGizmos` y `GetGizmosAsync` métodos.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample5.cs)]

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample6.cs?highlight=1,4-8)]

 Los cambios asincrónicos son similares a los realizados en el **GizmosAsync** anteriormente. 

- La firma del método se anota con el [async](https://msdn.microsoft.com/en-us/library/hh156513(VS.110).aspx) palabra clave, el tipo de valor devuelto se cambió a `Task<List<Gizmo>>`, y *Async* se anexa al nombre del método.
- Asincrónico [HttpClient](https://msdn.microsoft.com/en-us/library/system.net.http.httpclient(VS.110).aspx) clase se utiliza en lugar de la [WebClient](https://msdn.microsoft.com/en-us/library/system.net.webclient.aspx) clase.
- El [await](https://msdn.microsoft.com/en-us/library/hh156528(VS.110).aspx) palabra clave se aplica a la [HttpClient](https://msdn.microsoft.com/en-us/library/system.net.http.httpclient(VS.110).aspx) métodos asincrónicos.

La siguiente imagen muestra la vista asincrónica artículo tiene un.

![async](using-asynchronous-methods-in-aspnet-mvc-4/_static/image2.png)

La presentación de los exploradores de los datos de chismes es idéntica a la vista creada por la llamada sincrónica. La única diferencia es que la versión asincrónica puede tener un mejor rendimiento con cargas elevadas.

## <a id="Parallel"></a>Realizar varias operaciones en paralelo

Métodos de acción asincrónicos tienen una importante ventaja con respecto a los métodos sincrónicos cuando una acción debe realizar varias operaciones independientes. En el ejemplo que se proporciona, el método sincrónico `PWG`(para productos, Widgets y chismes) muestra los resultados de tres llamadas a servicios web para obtener una lista de productos, widgets y chismes. El [ASP.NET Web API](../../../web-api/index.md) proyecto que proporciona estos servicios utiliza [Task.Delay](https://msdn.microsoft.com/en-us/library/hh139096(VS.110).aspx) para simular red lenta o latencia de las llamadas. Cuando se establece el retraso a 500 milisegundos, de forma asincrónica `PWGasync` método toma un poco más de 500 milisegundos para completarse mientras sincrónico `PWG` versión asume 1.500 milisegundos. Sincrónico `PWG` método se muestra en el código siguiente.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample7.cs)]

Asincrónico `PWGasync` método se muestra en el código siguiente.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample8.cs?highlight=1,3,12)]

La siguiente imagen muestra la vista devuelto desde el **PWGasync** método.

![pwgAsync](using-asynchronous-methods-in-aspnet-mvc-4/_static/image3.png)

## <a id="CancelToken"></a>Usar un Token de cancelación

Métodos de acción asincrónicos devolver `Task<ActionResult>`son cancelable, que es toman un [CancellationToken](https://msdn.microsoft.com/en-us/library/system.threading.cancellationtoken(VS.110).aspx) parámetro cuando uno se proporciona con el [AsyncTimeout](https://msdn.microsoft.com/en-us/library/system.web.mvc.asynctimeoutattribute(VS.108).aspx) atributo. El código siguiente muestra el `GizmosCancelAsync` método con un tiempo de espera de 150 milisegundos.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample9.cs?highlight=1-3,5,10)]

El código siguiente muestra la sobrecarga de GetGizmosAsync, que toma un [CancellationToken](https://msdn.microsoft.com/en-us/library/system.threading.cancellationtoken(VS.110).aspx) parámetro.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample10.cs)]

En la aplicación de ejemplo proporcionada, seleccionar la *demostración de Token de cancelación* vincular llamadas el `GizmosCancelAsync` método y se muestra la cancelación de la llamada asincrónica.

## <a id="ServerConfig"></a>Configuración del servidor para llamadas a servicios Web alta simultaneidad/alta latencia

Para obtener los beneficios derivados de una aplicación web asincrónica, deberá realizar algunos cambios en la configuración predeterminada del servidor. Tenga en cuenta al configurar y probar la aplicación web asincrónica de esfuerzo lo siguiente.

- Windows 7, Windows Vista y todos los sistemas operativos de cliente de Windows tener un máximo de 10 solicitudes simultáneas. Necesitará un sistema operativo de Windows Server para ver las ventajas de los métodos asincrónicos bajo una carga excesiva.
- Registrar .NET 4.5 con IIS desde un símbolo del sistema con privilegios elevados:  
 %windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet\_regiis -i  
 Vea [herramienta de registro de IIS en ASP.NET (Aspnet\_regiis.exe)](https://msdn.microsoft.com/en-us/library/k6h9cz8h.aspx)
- Tendrá que aumentar la [HTTP.sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture) límite de la cola desde el valor predeterminado de 1.000 a 5000. Si el valor es demasiado bajo, es posible que vea [HTTP.sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture) rechazar las solicitudes con el estado HTTP 503. Para cambiar el límite de la cola de HTTP.sys:

    - Abra el Administrador de IIS y navegue hasta el panel de grupos de aplicaciones.
    - Haga clic con el botón secundario en el grupo de aplicaciones de destino y seleccione **configuración avanzada**.  
        ![avanzadas](using-asynchronous-methods-in-aspnet-mvc-4/_static/image4.png)
    - En el **configuración avanzada** cuadro de diálogo, cambie *longitud de cola* de 1000 a 5000.  
        ![Longitud de cola](using-asynchronous-methods-in-aspnet-mvc-4/_static/image5.png)  
  
 Tenga en cuenta que en las ilustraciones anteriores, .NET framework se incluye como v4.0, incluso aunque el grupo de aplicaciones se use .NET 4.5. Para entender esta discrepancia, vea lo siguiente:

    - [Control de versiones de .NET y la compatibilidad con múltiples versiones de .NET Framework 4.5 es una actualización en contexto a .NET 4.0](http://www.hanselman.com/blog/NETVersioningAndMultiTargetingNET45IsAnInplaceUpgradeToNET40.aspx)
    - [Cómo configurar una aplicación de IIS o el grupo de aplicaciones usar ASP.NET 3.5, en lugar de 2.0](http://www.hanselman.com/blog/HowToSetAnIISApplicationOrAppPoolToUseASPNET35RatherThan20.aspx)
    - [Dependencias y versiones de .NET framework](https://msdn.microsoft.com/en-us/library/bb822049(VS.110).aspx)
- Si la aplicación está utilizando servicios web o System.NET para comunicarse con un back-end a través de HTTP que tenga que aumentar la [connectionManagement/maxconnection](https://msdn.microsoft.com/en-us/library/fb6y0fyc(VS.110).aspx) elemento. Para las aplicaciones ASP.NET, esto está limitado por la característica de configuración automática en 12 veces el número de CPU. Esto significa que en cuádruple, puede tener como máximo 12 \* 4 = 48 conexiones simultáneas a un punto de conexión IP. Dado que esto está asociado a [autoConfig](https://msdn.microsoft.com/en-us/library/7w2sway1(VS.110).aspx), la manera más fácil aumentar `maxconnection` en ASP.NET es establecer aplicación [System.Net.ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/en-us/library/system.net.servicepointmanager.defaultconnectionlimit(VS.110).aspx) mediante programación en el de `Application_Start` método en el *global.asax* archivo. Vea el ejemplo de descarga para obtener un ejemplo.
- En .NET 4.5, el valor predeterminado es 5000 para [MaxConcurrentRequestsPerCPU](https://blogs.msdn.com/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx) debiera estar bien.
