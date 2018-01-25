---
uid: web-forms/overview/performance-and-caching/using-asynchronous-methods-in-aspnet-45
title: "Usar métodos asíncronos en ASP.NET 4.5 | Documentos de Microsoft"
author: Rick-Anderson
description: "Este tutorial le enseñará los aspectos básicos de la creación de una aplicación de formularios Web Forms de ASP.NET asincrónica mediante Visual Studio Express 2012 para Web, que es una descarga..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/06/2012
ms.topic: article
ms.assetid: a585c9a2-7c8e-478b-9706-90f3739c50d1
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/performance-and-caching/using-asynchronous-methods-in-aspnet-45
msc.type: authoredcontent
ms.openlocfilehash: 73e46134cfafb9edc4c1888211eab44b8f2bf828
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="using-asynchronous-methods-in-aspnet-45"></a>Usar métodos asíncronos en ASP.NET 4.5
====================
Por [Rick Anderson](https://github.com/Rick-Anderson)

> Este tutorial le enseñará los aspectos básicos de la creación de una aplicación de formularios Web Forms de ASP.NET asincrónica utilizando [Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/11), que es una versión gratuita de Microsoft Visual Studio. También puede usar [Visual Studio 2012](https://www.microsoft.com/visualstudio/11). En este tutorial se incluyen las siguientes secciones.
> 
> - [Cómo se procesan las solicitudes por el grupo de subprocesos](#HowRequestsProcessedByTP)
> - [Elegir los métodos sincrónicos o asincrónicos](#ChoosingSyncVasync)
> - [La aplicación de ejemplo](#SampleApp)
> - [La página sincrónica chismes](#GizmosSynch)
> - [Crear una página chismes asincrónica](#CreatingAsynchGizmos)
> - [Realizar varias operaciones en paralelo](#Parallel)
> - [Usar un Token de cancelación](#CancelToken)
> - [Configuración del servidor para llamadas a servicios Web alta simultaneidad/alta latencia](#ServerConfig)
> 
> Se proporciona un ejemplo completo para este tutorial en  
>  [https://github.com/RickAndMSFT/Async-ASP.NET/](https://github.com/RickAndMSFT/Async-ASP.NET/) en el [GitHub](https://github.com/) sitio.


Páginas Web ASP.NET 4.5 en combinación [.NET 4.5](https://msdn.microsoft.com/library/w0x726c2(VS.110).aspx) permite registrar los métodos asincrónicos que devuelven un objeto del tipo [tarea](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx). .NET Framework 4 se introdujo un concepto de programación asincrónico conocido como un [tarea](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) y ASP.NET 4.5 admite [tarea](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx). Las tareas se representan por el **tarea** tipo y los tipos relacionados en el [System.Threading.Tasks](https://msdn.microsoft.com/library/system.threading.tasks.aspx) espacio de nombres. .NET Framework 4.5 se basa en esta compatibilidad asincrónica con el [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) y [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) palabras clave que facilitan el trabajo con [tarea](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) objetos mucho menos complejos que anterior métodos asincrónicos. El [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) palabra clave es una abreviatura sintáctica para indicar que debe esperar un fragmento de código de forma asincrónica en alguna otra parte del código. El [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) palabra clave representa una sugerencia que puede usar para marcar los métodos como métodos asincrónicos basado en tareas. La combinación de **await**, **async**y el **tarea** objeto, resulta más fácil de escribir código asincrónico en .NET 4.5. El nuevo modelo para los métodos asincrónicos se denomina la *modelo asincrónico basado en tareas* (**pulse**). Este tutorial se supone que dispone de cierta familiaridad con el uso de programación asincrónica [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) y [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) palabras clave y el [tarea](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) espacio de nombres.

Para obtener más información sobre el uso de la [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) y [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) palabras clave y el [tarea](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) espacio de nombres, vea las siguientes referencias.

- [Notas del producto: Asincronía en .NET](https://go.microsoft.com/fwlink/?LinkId=204844)
- [Preguntas más frecuentes de Async y Await](https://blogs.msdn.com/b/pfxteam/archive/2012/04/12/10293335.aspx)
- [Programación asincrónica de Visual Studio](https://msdn.microsoft.com/vstudio/gg316360)

## <a id="HowRequestsProcessedByTP"></a>Cómo se procesan las solicitudes por el grupo de subprocesos

En el servidor web, .NET Framework mantiene un grupo de subprocesos que se usan para atender las solicitudes ASP.NET. Cuando llega una solicitud, se envía un subproceso del grupo para procesar la solicitud. Si la solicitud se procesa de forma sincrónica, el subproceso que procesa la solicitud está ocupado mientras la solicitud se está procesando y que el subproceso no puede atender otra solicitud.   
  
Esto podría no ser un problema, porque el grupo de subprocesos se puede establecer lo suficientemente grande como para dar cabida a muchos subprocesos ocupados. Sin embargo, se limita el número de subprocesos en el grupo de subprocesos (el valor máximo de .NET 4.5 predeterminado es 5000). En las aplicaciones grandes con gran simultaneidad de las solicitudes de ejecución prolongada, todos los subprocesos disponibles pueden estar ocupados. Esta condición se conoce como la falta de subprocesos. Cuando se alcanza esta condición, el servidor web pone en cola las solicitudes. Si la cola de solicitudes se llena, el servidor web rechaza las solicitudes con estado HTTP 503 (Server Too Busy). El grupo de subprocesos CLR tiene limitaciones en inyecciones de nuevo subproceso. Si la simultaneidad es por ráfagas (es decir, el sitio web repentinamente puede obtener un gran número de solicitudes) y todos los subprocesos de solicitud disponibles están ocupados debido a llamadas de back-end con latencia alta, la velocidad de inserción limitado de subprocesos puede hacer que la aplicación responda muy bajo. Además, cada subproceso nuevo que se agrega al grupo de subprocesos tiene sobrecarga (por ejemplo, 1 MB de memoria de la pila). Una aplicación web con métodos sincrónicos para llamadas de latencia alta del servicio en el grupo de subprocesos aumenta con el valor predeterminado de .NET 4.5 admite un máximo de 5 000 subprocesos consumiría aproximadamente 5 GB más memoria que una aplicación pueda el servicio el mismo solicita utilizando métodos asincrónicos y solo 50 subprocesos. Al realizar el trabajo asincrónico, no siempre está usando un subproceso. Por ejemplo, al realizar una solicitud de servicio web asincrónico, ASP.NET no esté usando todos los subprocesos entre el **async** llamada al método y el **await**. Usar el grupo de subprocesos para atender las solicitudes con una latencia elevada puede provocar un consumo de grandes cantidades de memoria y una mala utilización del hardware del servidor.

## <a name="processing-asynchronous-requests"></a>Procesar solicitudes asincrónicas

En las aplicaciones web que aparece un gran número de solicitudes simultáneas en el inicio o que tenga una carga por ráfagas (donde simultaneidad aumenta repentinamente), que realiza llamadas al servicio web asincrónica, aumentará la capacidad de respuesta de la aplicación. Una solicitud asincrónica tarda la misma cantidad de tiempo en procesarse que una solicitud sincrónica. Por ejemplo, si una solicitud realiza un servicio web llamada requiere dos segundos en completarse, la solicitud tardará dos segundos si se realiza de forma sincrónica o asincrónica. Sin embargo, durante una llamada asincrónica, no se bloquea un subproceso de responder a otras solicitudes mientras espera a que la primera solicitud que se complete. Por lo tanto, solicitudes asincrónicas evita el crecimiento del grupo de puesta en cola y el subproceso de solicitud cuando hay muchas solicitudes simultáneas que invocan operaciones de ejecución prolongada.

## <a id="ChoosingSyncVasync"></a>Elegir los métodos sincrónicos o asincrónicos

En esta sección se muestra las directrices acerca de cuándo utilizar métodos sincrónicos o asincrónicos. Se trata de meras directrices; Examinar individualmente cada aplicación para determinar si los métodos asincrónicos incrementar el rendimiento.

En general, utilice los métodos sincrónicos para las siguientes condiciones:

- Las operaciones son simples o de ejecución breve.
- La simplicidad es más importante que la eficacia.
- Las operaciones son principalmente operaciones de la CPU en lugar de las operaciones que implican una amplia disco o una sobrecarga de la red. Usar métodos asincrónicos en operaciones relacionadas con la CPU no proporciona ninguna ventaja y da lugar a mayor sobrecarga.

 En general, utilice métodos asincrónicos en las siguientes condiciones:

- Se están llamando a los servicios que se pueden utilizar en métodos asincrónicos, y usa .NET 4.5 o posterior.
- Las operaciones están relacionadas con la red o enlazadas a E/s en lugar de límite de CPU.
- El paralelismo es más importante que la simplicidad de código.
- Desea proporcionar un mecanismo que permite a los usuarios cancelar una solicitud de ejecución prolongada.
- Cuando la ventaja de cambiar subprocesos out compensa el costo de cambio de contexto. En general, debe crear un método asincrónico si el método sincrónico bloquea el subproceso de solicitud ASP.NET mientras no realiza ningún trabajo. Al realizar la llamada asincrónica, el subproceso de solicitud ASP.NET no se impide que no se realiza ningún trabajo mientras se espera para que la solicitud de servicio web que se complete.
- Las pruebas muestran que las operaciones bloqueo son un cuello de botella en el rendimiento del sitio y que IIS puede prestar servicio a más solicitudes mediante el uso de métodos asincrónicos para estas llamadas que causan bloqueos.

 El ejemplo descargable muestra cómo utilizar métodos asincrónicos de forma eficaz. El ejemplo proporcionado se diseñó para proporcionar una sencilla demostración de la programación asincrónica en ASP.NET 4.5. El ejemplo no pretende ser una arquitectura de referencia para la programación asincrónica en ASP.NET. El programa de ejemplo llama [ASP.NET Web API](../../../web-api/index.md) métodos que a su vez llaman a [Task.Delay](https://msdn.microsoft.com/library/hh139096(VS.110).aspx) para simular llamadas al servicio web de ejecución prolongada. La mayoría de las aplicaciones de producción no mostrará ventajas tan evidentes al usar métodos asincrónicos.   
  
Pocas aplicaciones exigen que todos los métodos sea asincrónico. A menudo, proporciona la máxima eficacia para la cantidad de trabajo necesario convertir algunos métodos sincrónicos en métodos asincrónicos.

## <a id="SampleApp"></a>La aplicación de ejemplo

Puede descargar la aplicación de ejemplo de [https://github.com/RickAndMSFT/Async-ASP.NET](https://github.com/RickAndMSFT/Async-ASP.NET) en el [GitHub](https://github.com/) sitio. El repositorio consta de tres proyectos:

- *WebAppAsync*: proyecto de ASP.NET Web Forms el que utiliza la API Web **WebAPIpwg** servicio. La mayoría del código de este tutorial es desde este proyecto.
- *WebAPIpgw*: proyecto de la API Web de ASP.NET MVC 4 que implementa el `Products, Gizmos and Widgets` controladores. Proporciona los datos para la *WebAppAsync* proyecto y la *Mvc4Async* proyecto.
- *Mvc4Async*: ASP.NET MVC 4 el proyecto que contiene el código utilizado en otro tutorial. Realiza llamadas de API Web a la **WebAPIpwg** servicio.

## <a id="GizmosSynch"></a>La página sincrónica chismes

 El código siguiente muestra el `Page_Load` método sincrónico que se utiliza para mostrar una lista de gizmos. (En este artículo, un artículo tiene un es un dispositivo mecánico ficticio). 

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample1.cs)]

El código siguiente muestra el `GetGizmos` método del artículo tiene un servicio.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample2.cs)]

El `GizmoService GetGizmos` método pasa un URI a un servicio ASP.NET Web API HTTP que devuelve una lista de datos de gizmos. El *WebAPIpgw* proyecto contiene la implementación de la API Web `gizmos, widget` y `product` controladores.  
La siguiente imagen muestra la página chismes desde el proyecto de ejemplo.

![Gizmos](using-asynchronous-methods-in-aspnet-45/_static/image1.png)

## <a id="CreatingAsynchGizmos"></a>Crear una página chismes asincrónica

El ejemplo usa el nuevo [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) y [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) palabras clave (disponible en .NET 4.5 y Visual Studio 2012) para permitir que el compilador ser responsable de mantener los necesarios para las transformaciones complejas programación asincrónica. El compilador le permite escribir código mediante que construcciones de flujo de control sincrónico de la de C# y el compilador aplica automáticamente las transformaciones necesarias para usar devoluciones de llamada con el fin de evitar el bloqueo de subprocesos.

Páginas asincrónicas de ASP.NET deben incluir el [página](https://msdn.microsoft.com/library/ydy4x04a.aspx) directiva con la `Async` atributo establecido en "true". El código siguiente muestra el [página](https://msdn.microsoft.com/library/ydy4x04a.aspx) directiva con la `Async` atributo establecido en "true" para el *GizmosAsync.aspx* página.

[!code-aspx[Main](using-asynchronous-methods-in-aspnet-45/samples/sample3.aspx?highlight=1)]

El código siguiente muestra el `Gizmos` sincrónico `Page_Load` método y la `GizmosAsync` página asincrónica. Si el explorador admite el [HTML 5 &lt;marcar&gt; elemento](http://www.w3.org/wiki/HTML/Elements/mark), podrá ver los cambios en `GizmosAsync` en amarillo resaltado.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample4.cs)]

La versión asincrónica:

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample5.cs?highlight=3,6-7,9,11)]

 Se aplicaron los cambios siguientes para permitir la `GizmosAsync` página sea asincrónico.

- El [página](https://msdn.microsoft.com/library/ydy4x04a.aspx) directiva debe tener la `Async` atributo establecido en "true".
- El `RegisterAsyncTask` método se usa para registrar una tarea asincrónica que contiene el código que se ejecuta de forma asincrónica.
- El nuevo `GetGizmosSvcAsync` método está marcado con el [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) palabra clave, que indica al compilador para generar las devoluciones de llamada para las partes del cuerpo y para crear automáticamente un `Task` que se devuelve.
- &quot;Async&quot; se anexa al nombre del método asincrónico. Anexa "Async" no es necesario, pero es la convención al escribir métodos asincrónicos.
- El tipo de valor devuelto de la nueva nueva `GetGizmosSvcAsync` método es `Task`. El tipo de valor devuelto de `Task` representa el trabajo en curso y proporciona a los llamadores del método con un identificador a través de la que se va a esperar a que finalice la operación asincrónica.
- El [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) palabra clave se aplicó al llamar al servicio web.
- Se llamó a la API del servicio web asincrónica (`GetGizmosAsync`).

Dentro de la `GetGizmosSvcAsync` otro método asincrónico, del cuerpo del método `GetGizmosAsync` se llama. `GetGizmosAsync`devuelve inmediatamente un `Task<List<Gizmo>>` que finalmente se completará cuando los datos están disponibles. Dado que no desea hacer nada más, hasta que tenga los datos de artículo tiene un, el código espera la tarea (mediante el **await** palabra clave). Puede usar el **await** palabra clave solo en los métodos anotados con el **async** palabra clave.

El **await** palabra clave no bloquea el subproceso hasta que se complete la tarea. Se suscribe el resto del método como una devolución de llamada en la tarea y devuelve inmediatamente. Cuando finalmente se completa la tarea en espera, se invoque esa devolución de llamada y, por tanto, reanudar la ejecución de la derecha del método donde se quedó. Para obtener más información sobre el uso de la [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) y [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) palabras clave y el [tarea](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) espacio de nombres, vea la [async referencias](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/async).

El código siguiente muestra el `GetGizmos` y `GetGizmosAsync` métodos.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample6.cs)]

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample7.cs?highlight=1,4-8)]

 Los cambios asincrónicos son similares a los realizados en el **GizmosAsync** anteriormente. 

- La firma del método se anota con el [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) palabra clave, el tipo de valor devuelto se cambió a `Task<List<Gizmo>>`, y *Async* se anexa al nombre del método.
- Asincrónico [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx) clase se utiliza en lugar de sincrónico [WebClient](https://msdn.microsoft.com/library/system.net.webclient.aspx) clase.
- El [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) palabra clave se aplica a la [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx)[GetAsync](https://msdn.microsoft.com/library/hh158944(VS.110).aspx) método asincrónico.

La siguiente imagen muestra la vista asincrónica artículo tiene un.

![async](using-asynchronous-methods-in-aspnet-45/_static/image2.png)

La presentación de los exploradores de los datos de chismes es idéntica a la vista creada por la llamada sincrónica. La única diferencia es que la versión asincrónica puede tener un mejor rendimiento con cargas elevadas.

## <a name="registerasynctask-notes"></a>Notas de RegisterAsyncTask

Métodos enlazan con `RegisterAsyncTask` se ejecutará inmediatamente después de [PreRender](https://msdn.microsoft.com/library/ms178472.aspx). También puede usar eventos en una página void async directamente, tal como se muestra en el código siguiente:

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample8.cs)]

La desventaja de async void eventos es que los desarrolladores ya no tiene control total sobre cuándo ejecutan eventos. Por ejemplo, si ambos .aspx y un. Definir master `Page_Load` eventos y uno o ambos son asincrónicas, no se puede garantizar el orden de ejecución. El mismo orden de indeterminiate para controladores de eventos no (como `async void Button_Click` ) se aplica. Para la mayoría de los desarrolladores, esto debería ser aceptable, pero solo aquellos que requieren un control total sobre el orden de ejecución deben usar las API como `RegisterAsyncTask` que usan los métodos que devuelven un objeto de tarea.

## <a id="Parallel"></a>Realizar varias operaciones en paralelo

Métodos asincrónicos tienen una importante ventaja con respecto a los métodos sincrónicos cuando una acción debe realizar varias operaciones independientes. En el ejemplo proporcionado, la página sincrónica *PWG.aspx*(para productos, Widgets y chismes) muestra los resultados de tres llamadas a servicios web para obtener una lista de productos, widgets y chismes. El [ASP.NET Web API](../../../web-api/index.md) proyecto que proporciona estos servicios utiliza [Task.Delay](https://msdn.microsoft.com/library/hh139096(VS.110).aspx) para simular red lenta o latencia de las llamadas. Cuando se establece el retraso a 500 milisegundos, de forma asincrónica *PWGasync.aspx* página tarda un poco más de 500 milisegundos para completarse mientras sincrónico `PWG` versión asume 1.500 milisegundos. Sincrónico *PWG.aspx* página se muestra en el código siguiente.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample9.cs)]

Asincrónico `PWGasync` código subyacente se muestra a continuación.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample10.cs?highlight=5,11,21)]

La siguiente imagen muestra la vista procedente de la asincrónica *PWGasync.aspx* página.

![](using-asynchronous-methods-in-aspnet-45/_static/image3.png)

## <a id="CancelToken"></a>Usar un Token de cancelación

Los métodos asincrónicos que devuelven `Task`son cancelable, que es toman un [CancellationToken](https://msdn.microsoft.com/library/system.threading.cancellationtoken(VS.110).aspx) parámetro cuando se proporciona uno con el `AsyncTimeout` atributo de la [página](https://msdn.microsoft.com/library/ydy4x04a.aspx) directiva. El código siguiente muestra el *GizmosCancelAsync.aspx* página con un tiempo de espera de segundo.

[!code-aspx[Main](using-asynchronous-methods-in-aspnet-45/samples/sample11.aspx?highlight=1)]

El código siguiente muestra el *GizmosCancelAsync.aspx.cs* archivo.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample12.cs?highlight=6,9)]

En la aplicación de ejemplo proporcionada, seleccionando la *GizmosCancelAsync* vincular llamadas el *GizmosCancelAsync.aspx* página y muestra la cancelación (por el tiempo de espera) de la llamada asincrónica. Dado que es el tiempo de retraso dentro de un intervalo aleatorio, debe actualizar la página de un par de veces para obtener el mensaje de error de tiempo de espera.

## <a id="ServerConfig"></a>Configuración del servidor para llamadas a servicios Web alta simultaneidad/alta latencia

Para obtener los beneficios derivados de una aplicación web asincrónica, deberá realizar algunos cambios en la configuración predeterminada del servidor. Tenga en cuenta al configurar y probar la aplicación web asincrónica de esfuerzo lo siguiente.

- Windows 7, Windows Vista, Windows 8 y todos los sistemas operativos de cliente de Windows tener un máximo de 10 solicitudes simultáneas. Necesitará un sistema operativo de Windows Server para ver las ventajas de los métodos asincrónicos bajo una carga excesiva.
- Registrar .NET 4.5 con IIS desde un símbolo con privilegios elevados mediante el siguiente comando:  
 %windir%\Microsoft.NET\Framework64 \v4.0.30319\aspnet\_regiis -i  
 Vea [herramienta de registro de IIS en ASP.NET (Aspnet\_regiis.exe)](https://msdn.microsoft.com/library/k6h9cz8h.aspx)
- Tendrá que aumentar la [HTTP.sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture) límite de la cola desde el valor predeterminado de 1.000 a 5000. Si el valor es demasiado bajo, es posible que vea [HTTP.sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture) rechazar las solicitudes con el estado HTTP 503. Para cambiar el límite de la cola de HTTP.sys:

    - Abra el Administrador de IIS y navegue hasta el panel de grupos de aplicaciones.
    - Haga clic con el botón secundario en el grupo de aplicaciones de destino y seleccione **configuración avanzada**.  
        ![advanced](using-asynchronous-methods-in-aspnet-45/_static/image4.png)
    - En el **configuración avanzada** cuadro de diálogo, cambie *longitud de cola* de 1000 a 5000.  
        ![Longitud de cola](using-asynchronous-methods-in-aspnet-45/_static/image5.png)  
  
 Tenga en cuenta que en las ilustraciones anteriores, .NET framework se incluye como v4.0, incluso aunque el grupo de aplicaciones se use .NET 4.5. Para entender esta discrepancia, vea lo siguiente:

        - [.NET Versioning and Multi-Targeting - .NET 4.5 is an in-place upgrade to .NET 4.0](http://www.hanselman.com/blog/NETVersioningAndMultiTargetingNET45IsAnInplaceUpgradeToNET40.aspx)
        - [How to set an IIS Application or AppPool to use ASP.NET 3.5 rather than 2.0](http://www.hanselman.com/blog/HowToSetAnIISApplicationOrAppPoolToUseASPNET35RatherThan20.aspx)
        - [.NET Framework Versions and Dependencies](https://msdn.microsoft.com/library/bb822049(VS.110).aspx)
- Si la aplicación está utilizando servicios web o System.NET para comunicarse con un back-end a través de HTTP que tenga que aumentar la [connectionManagement/maxconnection](https://msdn.microsoft.com/library/fb6y0fyc(VS.110).aspx) elemento. Para las aplicaciones ASP.NET, esto está limitado por la característica de configuración automática en 12 veces el número de CPU. Esto significa que en cuádruple, puede tener como máximo 12 \* 4 = 48 conexiones simultáneas a un punto de conexión IP. Dado que esto está asociado a [autoConfig](https://msdn.microsoft.com/library/7w2sway1(VS.110).aspx), la manera más fácil aumentar `maxconnection` en ASP.NET es establecer aplicación [System.Net.ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit(VS.110).aspx) mediante programación en el de `Application_Start` método en el *global.asax* archivo. Vea el ejemplo de descarga para obtener un ejemplo.
- En .NET 4.5, el valor predeterminado es 5000 para [MaxConcurrentRequestsPerCPU](https://blogs.msdn.com/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx) debiera estar bien.

## <a name="contributors"></a>Colaboradores

- [Levi Broderick](http://stackoverflow.com/users/59641/levi)
- [Tom Dykstra](http://www.bing.com/search?q=site%3Aasp.net+%22Tom+Dykstra%22+-forums.asp.net&amp;qs=n&amp;form=QBRE&amp;pq=site%3Aasp.net+%22tom+dykstra%22+-forums.asp.net&amp;sc=8-42&amp;sp=-1&amp;sk=)
- [Brad Wilson](http://bradwilson.typepad.com/)
- [Ge HongMei](https://blogs.msdn.com/b/hongmeig/)
