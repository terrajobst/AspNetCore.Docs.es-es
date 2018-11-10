---
uid: web-forms/overview/performance-and-caching/using-asynchronous-methods-in-aspnet-45
title: Usar métodos asincrónicos en ASP.NET 4.5 | Microsoft Docs
author: Rick-Anderson
description: Este tutorial le enseñará los aspectos básicos de la creación de una aplicación de formularios Web Forms ASP.NET asincrónica mediante Visual Studio Express 2012 para Web, que es un complemento gratuito...
ms.author: riande
ms.date: 06/06/2012
ms.assetid: a585c9a2-7c8e-478b-9706-90f3739c50d1
msc.legacyurl: /web-forms/overview/performance-and-caching/using-asynchronous-methods-in-aspnet-45
msc.type: authoredcontent
ms.openlocfilehash: 9a3c9fab4932c3bc85733a912cf1d1eaaecc1ab7
ms.sourcegitcommit: fc7eb4243188950ae1f1b52669edc007e9d0798d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/07/2018
ms.locfileid: "51225491"
---
<a name="using-asynchronous-methods-in-aspnet-45"></a>Usar métodos asincrónicos en ASP.NET 4.5
====================
por [Rick Anderson]((https://twitter.com/RickAndMSFT))

> Este tutorial le enseñará los aspectos básicos de creación de una aplicación de formularios Web Forms ASP.NET asincrónico con [Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/11), que es una versión gratuita de Microsoft Visual Studio. También puede usar [Visual Studio 2012](https://www.microsoft.com/visualstudio/11). Las secciones siguientes se incluyen en este tutorial.
> 
> - [Cómo se procesan las solicitudes por el grupo de subprocesos](#HowRequestsProcessedByTP)
> - [Elegir los métodos sincrónicos o asincrónicos](#ChoosingSyncVasync)
> - [La aplicación de ejemplo](#SampleApp)
> - [La página sincrónica Gizmos](#GizmosSynch)
> - [Creación de una página asincrónica Gizmos](#CreatingAsynchGizmos)
> - [Realizar varias operaciones en paralelo](#Parallel)
> - [Uso de un Token de cancelación](#CancelToken)
> - [Configuración del servidor para llamadas de servicio Web de alta simultaneidad y alta latencia](#ServerConfig)
> 
> Se proporciona un ejemplo completo para este tutorial en  
>  [https://github.com/RickAndMSFT/Async-ASP.NET/](https://github.com/RickAndMSFT/Async-ASP.NET/) en el [GitHub](https://github.com/) sitio.


Las páginas Web ASP.NET 4.5 en combinación [.NET 4.5](https://msdn.microsoft.com/library/w0x726c2(VS.110).aspx) permite registrar los métodos asincrónicos que devuelven un objeto de tipo [tarea](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx). .NET Framework 4 introdujo un concepto de programación asincrónico se denomina un [tarea](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) y ASP.NET 4.5 admite [tarea](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx). Las tareas se representan mediante el **tarea** tipo y los tipos relacionados en el [System.Threading.Tasks](https://msdn.microsoft.com/library/system.threading.tasks.aspx) espacio de nombres. .NET Framework 4.5 se basa en esta compatibilidad asincrónica con el [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) y [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) palabras clave que facilitan el trabajo con [tarea](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) objetos mucho menos complejos que la anterior métodos asincrónicos. El [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) palabra clave es la forma abreviada sintáctica presente para indicar que un fragmento de código debe esperar asincrónicamente en algún otro elemento de código. El [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) palabra clave representa una sugerencia que puede usar para marcar métodos como métodos asincrónicos basados en tareas. La combinación de **await**, **async**y el **tarea** objeto facilita en gran medida para que pueda escribir código asincrónico en .NET 4.5. El nuevo modelo para los métodos asincrónicos se denomina el *modelo asincrónico basado en tareas* (**pulse**). En este tutorial se da por supuesto que tiene cierta familiaridad con el uso de programación asincrónica [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) y [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) palabras clave y el [tarea](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) espacio de nombres.

Para obtener más información sobre el uso de la [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) y [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) palabras clave y el [tarea](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) espacio de nombres, vea las siguientes referencias.

- [Notas del producto: Asincronía en .NET](https://go.microsoft.com/fwlink/?LinkId=204844)
- [Preguntas más frecuentes sobre Async y Await](https://blogs.msdn.com/b/pfxteam/archive/2012/04/12/10293335.aspx)
- [Programación asincrónica de Visual Studio](https://msdn.microsoft.com/vstudio/gg316360)

## <a id="HowRequestsProcessedByTP"></a>  Cómo se procesan las solicitudes por el grupo de subprocesos

En el servidor web, .NET Framework mantiene un grupo de subprocesos que se usan para atender las solicitudes ASP.NET. Cuando llega una solicitud, se envía un subproceso del grupo para procesar la solicitud. Si la solicitud se procesa de forma sincrónica, el subproceso que procesa la solicitud está ocupado mientras la solicitud se está procesando y que el subproceso no puede atender otra solicitud.   
  
Esto podría no ser un problema, ya que se pueden hacer lo suficientemente grande como para dar cabida a muchos subprocesos ocupados del grupo de subprocesos. Sin embargo, se limita el número de subprocesos en el grupo de subprocesos (el valor máximo predeterminado de .NET 4.5 es 5000). En aplicaciones de gran tamaño con la simultaneidad alto de solicitudes de ejecución prolongada, todos los subprocesos disponibles podrían estar ocupados. Esta condición se conoce como la falta de subprocesos. Cuando se alcanza esta condición, el servidor web pone en cola las solicitudes. Si la cola de solicitudes se llena, el servidor web rechaza las solicitudes con el estado HTTP 503 (Server Too Busy). El grupo de subprocesos CLR tiene limitaciones en las inyecciones de subproceso nuevo. Si la simultaneidad es por ráfagas (es decir, el sitio web, de repente, puede obtener un gran número de solicitudes) y todos los subprocesos de solicitud disponibles están ocupados debido a llamadas de back-end con una latencia elevada, la tasa de inserción de subproceso limitado puede hacer que la aplicación responda muy mal. Además, cada subproceso nuevo agregado al grupo de subprocesos tiene sobrecarga (por ejemplo, 1 MB de memoria de pila). Una aplicación web mediante los métodos sincrónicos a las llamadas de alta latencia de servicio donde el grupo de subprocesos aumenta con el valor predeterminado de .NET 4.5 admite un máximo de 5 000 subprocesos consumiría aproximadamente 5 GB más memoria que una aplicación que pueda el servicio de la misma solicitudes con los métodos asincrónicos y solo 50 subprocesos. Cuando está realizando trabajo asincrónico, no siempre usa un subproceso. Por ejemplo, al realizar una solicitud de servicio web asincrónico, ASP.NET no usará los subprocesos entre el **async** llamada al método y el **await**. Usar el grupo de subprocesos para atender las solicitudes con una latencia elevada puede dar lugar a una superficie de memoria grande y una mala utilización del hardware del servidor.

## <a name="processing-asynchronous-requests"></a>Procesar solicitudes asincrónicas

En las aplicaciones web que aparece un gran número de solicitudes simultáneas en el inicio o que tenga una carga por ráfagas (donde simultaneidad aumenta repentinamente), realizar llamadas al servicio web asincrónico, aumentará la capacidad de respuesta de la aplicación. Una solicitud asincrónica tarda la misma cantidad de tiempo en procesarse que una solicitud sincrónica. Por ejemplo, si una solicitud realiza un servicio web de llamada requiere dos segundos en completarse, la solicitud tardará dos segundos si se realiza de forma sincrónica o asincrónica. Sin embargo, durante una llamada asincrónica, no se bloquea un subproceso deja de responder a otras solicitudes mientras espera a que finalice la primera solicitud. Por lo tanto, las solicitudes asincrónicas evitar el crecimiento de grupo de puesta en cola y el subproceso de solicitud cuando hay muchas solicitudes simultáneas que invocan las operaciones de larga ejecución.

## <a id="ChoosingSyncVasync"></a>  Elegir los métodos sincrónicos o asincrónicos

Esta sección enumeran las directrices sobre cuándo usar métodos sincrónicos o asincrónicos. Se trata de meras directrices; Examine individualmente cada aplicación para determinar si los métodos asincrónicos ayudar con el rendimiento.

En general, use los métodos sincrónicos para las condiciones siguientes:

- Las operaciones son simples o de ejecución breve.
- La simplicidad es más importante que la eficacia.
- Las operaciones son principalmente operaciones de la CPU en lugar de las operaciones que implican una amplia disco o una sobrecarga de la red. Usar métodos asincrónicos en operaciones relacionadas con la CPU no proporciona ninguna ventaja y da lugar a mayor sobrecarga.

En general, utilice los métodos asincrónicos para las condiciones siguientes:

- Se están llamando a los servicios que se pueden consumir mediante los métodos asincrónicos, y usa .NET 4.5 o superior.
- Las operaciones son enlazada a la red o puedo enlazadas en lugar de a la CPU.
- El paralelismo es más importante que la simplicidad de código.
- Desea proporcionar un mecanismo que permite a los usuarios cancelar una solicitud de ejecución prolongada.
- Cuando la ventaja de cambiar subprocesos out pondera el costo de cambiar el contexto. En general, debe convertir un método asincrónico si el método sincrónico bloquea el subproceso de solicitud ASP.NET mientras no se realiza ningún trabajo. Al realizar la llamada asincrónica, el subproceso de solicitud ASP.NET no está bloqueado realizando ningún trabajo mientras espera a que finalice la solicitud de servicio web.
- Las pruebas muestran que las operaciones bloqueos son un cuello de botella de rendimiento del sitio y que IIS puede atender más solicitudes mediante el uso de métodos asincrónicos para estas llamadas bloqueos.

  El ejemplo descargable muestra cómo utilizar los métodos asincrónicos de forma eficaz. El ejemplo proporcionado se diseñó para proporcionar una simple demostración de la programación asincrónica en ASP.NET 4.5. El ejemplo no pretende ser una arquitectura de referencia para la programación asincrónica en ASP.NET. El programa de ejemplo llama a [ASP.NET Web API](../../../web-api/index.md) métodos que a su vez llaman a [Task.Delay](https://msdn.microsoft.com/library/hh139096(VS.110).aspx) para simular las llamadas al servicio web de ejecución prolongada. La mayoría de las aplicaciones de producción no mostrarán ventajas tan evidentes al usar métodos asincrónicos.   
  
Pocas aplicaciones exigen que todos los métodos sea asincrónico. A menudo, proporciona la máxima eficacia para la cantidad de trabajo necesario convertir algunos métodos sincrónicos en métodos asincrónicos.

## <a id="SampleApp"></a>  La aplicación de ejemplo

Puede descargar la aplicación de ejemplo de [ https://github.com/RickAndMSFT/Async-ASP.NET ](https://github.com/RickAndMSFT/Async-ASP.NET) en el [GitHub](https://github.com/) sitio. El repositorio consta de tres proyectos:

- *WebAppAsync*: proyecto de The ASP.NET Web Forms que utiliza la API Web **WebAPIpwg** service. La mayoría del código para este tutorial es desde este proyecto.
- *WebAPIpgw*: proyecto de ASP.NET MVC 4 Web API que implementa el `Products, Gizmos and Widgets` controladores. Proporciona los datos para el *WebAppAsync* proyecto y el *Mvc4Async* proyecto.
- *Mvc4Async*: ASP.NET MVC 4 el proyecto que contiene el código usado en otro tutorial. Realiza llamadas de API Web a la **WebAPIpwg** service.

## <a id="GizmosSynch"></a>  La página sincrónica Gizmos

 El código siguiente muestra el `Page_Load` método sincrónico que se usa para mostrar una lista de gizmos. (En este artículo, un artículo tiene un es un dispositivo mecánico ficticio). 

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample1.cs)]

El código siguiente muestra el `GetGizmos` método del artículo tiene un servicio.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample2.cs)]

El `GizmoService GetGizmos` método pasa un URI a un servicio ASP.NET Web API HTTP que devuelve una lista de los datos gizmos. El *WebAPIpgw* proyecto contiene la implementación de la API Web `gizmos, widget` y `product` controladores.  
La siguiente imagen muestra la página gizmos desde el proyecto de ejemplo.

![Gizmos](using-asynchronous-methods-in-aspnet-45/_static/image1.png)

## <a id="CreatingAsynchGizmos"></a>  Creación de una página asincrónica Gizmos

El ejemplo usa el nuevo [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) y [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) palabras clave (disponibles en .NET 4.5 y Visual Studio 2012) para permitir que el compilador ser responsable de mantener las complicadas transformaciones necesarias para programación asincrónica. El compilador le permite escribir código mediante que construcciones de flujo de control sincrónico de la de C# y el compilador aplica automáticamente las transformaciones necesarias para utilizar devoluciones de llamada con el fin de evitar el bloqueo de subprocesos.

Páginas asincrónicas de ASP.NET deben incluir el [página](https://msdn.microsoft.com/library/ydy4x04a.aspx) la directiva con la `Async` atributo establecido en "true". El siguiente código muestra la [página](https://msdn.microsoft.com/library/ydy4x04a.aspx) la directiva con la `Async` atributo establecido en "true" para el *GizmosAsync.aspx* página.

[!code-aspx[Main](using-asynchronous-methods-in-aspnet-45/samples/sample3.aspx?highlight=1)]

El siguiente código muestra la `Gizmos` sincrónica `Page_Load` método y el `GizmosAsync` página asincrónica. Si el explorador admite el [HTML 5 &lt;marcar&gt; elemento](http://www.w3.org/wiki/HTML/Elements/mark), verá los cambios en `GizmosAsync` en amarillo resaltado.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample4.cs)]

La versión asincrónica:

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample5.cs?highlight=3,6-7,9,11)]

 Se aplicaron los cambios siguientes para permitir la `GizmosAsync` página sea asincrónico.

- El [página](https://msdn.microsoft.com/library/ydy4x04a.aspx) la directiva debe tener el `Async` atributo establecido en "true".
- El `RegisterAsyncTask` método se usa para registrar una tarea asincrónica que contiene el código que se ejecuta de forma asincrónica.
- El nuevo `GetGizmosSvcAsync` método está marcado con el [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) palabra clave, que indica al compilador que genere devoluciones de llamada para partes del cuerpo y crear automáticamente un `Task` que se devuelve.
- &quot;Async&quot; se anexa al nombre del método asincrónico. Anexa "Async" no es necesario, pero es la convención al escribir métodos asincrónicos.
- El tipo de valor devuelto de la nueva `GetGizmosSvcAsync` método es `Task`. El tipo de valor devuelto de `Task` representa el trabajo en curso y proporciona a los llamadores del método con un identificador a través de la que se va a esperar la finalización de la operación asincrónica.
- El [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) palabra clave se aplicó a la llamada al servicio web.
- Se llamó a la API del servicio web asincrónico (`GetGizmosAsync`).

Dentro de la `GetGizmosSvcAsync` otro método asincrónico, de cuerpo de método `GetGizmosAsync` se llama. `GetGizmosAsync` se devuelve inmediatamente un `Task<List<Gizmo>>` que finalmente se completará cuando los datos están disponibles. Dado que no desea hacer nada más hasta que tenga los datos de artículo tiene un, el código espera la tarea (mediante el **await** palabra clave). Puede usar el **await** palabra clave solo en los métodos anotados con el **async** palabra clave.

El **await** palabra clave no bloquea el subproceso hasta que se complete la tarea. El resto del método se registra como devolución de llamada en la tarea y se devuelve inmediatamente. Cuando finaliza la tarea en espera, se invocan esa devolución de llamada y, por tanto, reanudar la ejecución de la derecha del método donde se quedó. Para obtener más información sobre el uso de la [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) y [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) palabras clave y el [tarea](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) espacio de nombres, vea el [async referencias](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/async).

El siguiente código muestra la `GetGizmos` y `GetGizmosAsync` métodos.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample6.cs)]

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample7.cs?highlight=1,4-8)]

 Los cambios asincrónicos son similares a los realizados en el **GizmosAsync** anteriormente. 

- La firma del método se anota con el [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) palabra clave, el tipo de valor devuelto se cambió a `Task<List<Gizmo>>`, y *Async* se anexa al nombre del método.
- Asincrónico [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx) clase se utiliza en lugar de sincrónico [WebClient](https://msdn.microsoft.com/library/system.net.webclient.aspx) clase.
- El [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) palabra clave se aplicó a la [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx)[GetAsync](https://msdn.microsoft.com/library/hh158944(VS.110).aspx) método asincrónico.

La siguiente imagen muestra la vista asincrónica artículo tiene un.

![async](using-asynchronous-methods-in-aspnet-45/_static/image2.png)

La presentación de los exploradores de los datos gizmos es idéntica a la vista creada por la llamada sincrónica. La única diferencia es que la versión asincrónica puede ser un mejor rendimiento con cargas pesadas.

## <a name="registerasynctask-notes"></a>Notas de RegisterAsyncTask

Los métodos que se enlazan con `RegisterAsyncTask` se ejecutará inmediatamente después de [PreRender](https://msdn.microsoft.com/library/ms178472.aspx). También puede usar eventos de async void página directamente, como se muestra en el código siguiente:

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample8.cs)]

La desventaja de async void eventos es que los desarrolladores ya no tiene control total sobre cuándo ejecutan eventos. Por ejemplo, si ambos .aspx y. Definir master `Page_Load` eventos y uno o ambos son asincrónicas, no se puede garantizar el orden de ejecución. El mismo orden indeterminiate para controladores de eventos que no sean (como `async void Button_Click` ) se aplica. Para la mayoría de los desarrolladores, esto debe ser aceptable, pero solo aquellos que requieren un control total sobre el orden de ejecución deben usar las API como `RegisterAsyncTask` que consumen los métodos que devuelven un objeto de tarea.

## <a id="Parallel"></a>  Realizar varias operaciones en paralelo

Los métodos asincrónicos tienen una ventaja significativa sobre los métodos sincrónicos cuando una acción debe realizar varias operaciones independientes. En el ejemplo proporcionado, la página sincrónica *PWG.aspx*(para los productos, Widgets y Gizmos) muestra los resultados de tres llamadas al servicio web para obtener una lista de productos, widgets y gizmos. El [ASP.NET Web API](../../../web-api/index.md) proyecto que proporciona estos servicios usa [Task.Delay](https://msdn.microsoft.com/library/hh139096(VS.110).aspx) para simular la red lenta o latencia de llamadas. Cuando se establece el retraso de 500 milisegundos, asincrónicos *PWGasync.aspx* página tarda un poco más de 500 milisegundos en completarse mientras sincrónico `PWG` versión asume 1.500 milisegundos. Sincrónico *PWG.aspx* página se muestra en el código siguiente.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample9.cs)]

Asincrónico `PWGasync` código subyacente se muestra a continuación.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample10.cs?highlight=5,11,21)]

La siguiente imagen muestra la vista devuelta por la asincrónica *PWGasync.aspx* página.

![](using-asynchronous-methods-in-aspnet-45/_static/image3.png)

## <a id="CancelToken"></a>  Uso de un Token de cancelación

Los métodos asincrónicos que devuelven `Task`son cancelable, que se toman un [CancellationToken](https://msdn.microsoft.com/library/system.threading.cancellationtoken(VS.110).aspx) parámetro cuando se proporciona con el `AsyncTimeout` atributo de la [página](https://msdn.microsoft.com/library/ydy4x04a.aspx) directiva. El siguiente código muestra la *GizmosCancelAsync.aspx* página con un tiempo de espera de segundo.

[!code-aspx[Main](using-asynchronous-methods-in-aspnet-45/samples/sample11.aspx?highlight=1)]

El siguiente código muestra la *GizmosCancelAsync.aspx.cs* archivo.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample12.cs?highlight=6,9)]

En la aplicación de ejemplo proporcionada, seleccionando la *GizmosCancelAsync* vincular llamadas la *GizmosCancelAsync.aspx* página y muestra la cancelación (por tiempo de espera) de la llamada asincrónica. Dado que es el tiempo de retraso dentro de un intervalo aleatorio, necesita actualizar la página de un par de veces para obtener el mensaje de error de tiempo de espera.

## <a id="ServerConfig"></a>  Configuración del servidor para llamadas de servicio Web de alta simultaneidad y alta latencia

Para obtener los beneficios de una aplicación web asincrónica, es posible que deba realizar algunos cambios en la configuración predeterminada del servidor. Tenga en cuenta al configurar y probar la aplicación web asíncronos de esfuerzo lo siguiente.

- Windows 7, Windows Vista, Windows 8 y todos los sistemas operativos de cliente de Windows tiene un máximo de 10 solicitudes simultáneas. Necesitará un sistema operativo de Windows Server para ver las ventajas de los métodos asincrónicos con una carga elevada.
- Registrar .NET 4.5 con IIS desde un símbolo con privilegios elevados mediante el comando siguiente:  
  %windir%\Microsoft.NET\Framework64 \v4.0.30319\aspnet\_regiis -i.  
  Consulte [herramienta de registro de IIS en ASP.NET (Aspnet\_regiis.exe)](https://msdn.microsoft.com/library/k6h9cz8h.aspx)
- Es posible que deba aumentar el [HTTP.sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture) límite de la cola desde el valor predeterminado de 1.000 a 5.000. Si el valor es demasiado bajo, es posible que vea [HTTP.sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture) rechazar las solicitudes con el estado HTTP 503. Para cambiar el límite de cola de HTTP.sys:

    - Abra el Administrador de IIS y navegue hasta el panel grupos de aplicaciones.
    - Haga clic con el botón derecho en el grupo de aplicaciones de destino y seleccione **configuración avanzada**.  
        ![Avanzada](using-asynchronous-methods-in-aspnet-45/_static/image4.png)
    - En el **configuración avanzada** cuadro de diálogo, cambie *longitud de cola* de 1.000 a 5.000.  
        ![Longitud de cola](using-asynchronous-methods-in-aspnet-45/_static/image5.png)  
  
  Tenga en cuenta que en las imágenes anteriores, .NET framework se incluye como v4.0, aunque el grupo de aplicaciones es usar .NET 4.5. Para entender esta discrepancia, consulte lo siguiente:

- [Control de versiones de .NET y .NET Framework 4.5 Multi-Targeting - es una actualización en contexto a .NET 4.0](http://www.hanselman.com/blog/NETVersioningAndMultiTargetingNET45IsAnInplaceUpgradeToNET40.aspx)
- [Establecimiento de una aplicación IIS o grupo de aplicaciones para usar ASP.NET 3.5, en lugar de 2.0](http://www.hanselman.com/blog/HowToSetAnIISApplicationOrAppPoolToUseASPNET35RatherThan20.aspx)
- [Versiones y dependencias de .NET Framework](https://msdn.microsoft.com/library/bb822049(VS.110).aspx)

- Si la aplicación está utilizando servicios web o System.NET para comunicarse con un back-end a través de HTTP que necesite aumentar la [connectionManagement/maxconnection](https://msdn.microsoft.com/library/fb6y0fyc(VS.110).aspx) elemento. Para las aplicaciones de ASP.NET, esto está limitado por la característica de configuración automática para 12 veces el número de CPU. Esto significa que en cuádruple, puede tener como máximo 12 \* 4 = 48 conexiones simultáneas a un punto de conexión IP. Dado que esta está asociada a [autoConfig](https://msdn.microsoft.com/library/7w2sway1(VS.110).aspx), la manera más fácil aumentar `maxconnection` en ASP.NET, aplicación consiste en establecer [System.Net.ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit(VS.110).aspx) mediante programación en el de `Application_Start` método en el *global.asax* archivo. Consulte el ejemplo de descarga para obtener un ejemplo.
- En .NET 4.5, el valor predeterminado de 5000 para [MaxConcurrentRequestsPerCPU](https://blogs.msdn.com/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx) debiera estar bien.

## <a name="contributors"></a>Colaboradores

- [Levi Broderick](http://stackoverflow.com/users/59641/levi)
- [Tom Dykstra](http://www.bing.com/search?q=site%3Aasp.net+%22Tom+Dykstra%22+-forums.asp.net&amp;qs=n&amp;form=QBRE&amp;pq=site%3Aasp.net+%22tom+dykstra%22+-forums.asp.net&amp;sc=8-42&amp;sp=-1&amp;sk=)
- [Brad Wilson](http://bradwilson.typepad.com/)
- [HongMei Ge](https://blogs.msdn.com/b/hongmeig/)
