---
uid: signalr/overview/testing-and-debugging/enabling-signalr-tracing
title: Habilitar el seguimiento de SignalR | Microsoft Docs
author: tfitzmac
description: Este documento describe cómo habilitar y configurar el seguimiento de los clientes y servidores de SignalR. El seguimiento le permite ver información acerca de los eventos de diagnóstico...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/08/2014
ms.topic: article
ms.assetid: 30060acb-be3e-4347-996f-3870f0c37829
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/testing-and-debugging/enabling-signalr-tracing
msc.type: authoredcontent
ms.openlocfilehash: 7f7401814e8ca9e61e62a3ebf993b03f92570984
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37373248"
---
<a name="enabling-signalr-tracing"></a>Habilitar el seguimiento de SignalR
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este documento describe cómo habilitar y configurar el seguimiento de los clientes y servidores de SignalR. El seguimiento le permite ver información acerca de los eventos de diagnóstico en la aplicación de SignalR.
> 
> En este tema se escribió originalmente por Patrick Fletcher.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET Framework 4.5
> - Versión 2 de SignalR
>   
> 
> 
> ## <a name="questions-and-comments"></a>Preguntas y comentarios
> 
> Deje comentarios sobre cómo le gustó de este tutorial y que podíamos mejorar en los comentarios en la parte inferior de la página. Si tiene preguntas que no están directamente relacionados con el tutorial, puede publicarlos en el [foro de ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).


Cuando se habilita el seguimiento, una aplicación de SignalR crea entradas de registro de eventos. Puede registrar los eventos desde el cliente y el servidor. El seguimiento en la conexión de los registros de servidor, el proveedor de escalado horizontal y los eventos de bus de mensajes. El seguimiento de los eventos de conexión de los registros de cliente. En SignalR 2.1 y versiones posteriores, el seguimiento en el cliente registra todo el contenido de los mensajes de invocación de concentrador.

## <a name="contents"></a>Contenido

- [Habilitar el seguimiento en el servidor](#server)

    - [Registro de eventos de servidor en los archivos de texto](#server_text)
    - [Registro de eventos de servidor en el registro de eventos](#server_eventlog)
- [Habilitar el seguimiento en el cliente de .NET (aplicaciones de escritorio de Windows)](#net_client)

    - [Registro de eventos de cliente de escritorio en la consola](#desktop_console)
    - [Registro de eventos de cliente de escritorio en un archivo de texto](#desktop_text)
- [Habilitar el seguimiento en los clientes de Windows Phone 8](#phone)

    - [Registro de eventos de cliente de Windows Phone en la interfaz de usuario](#phone_ui)
    - [Registro de eventos de cliente de Windows Phone en la consola de depuración](#phone_debug)
- [Habilitar el seguimiento en el cliente de JavaScript](#javascript)

<a id="server"></a>
## <a name="enabling-tracing-on-the-server"></a>Habilitar el seguimiento en el servidor

Habilita el seguimiento en el servidor en el archivo de configuración de la aplicación (App.config o Web.config según el tipo de proyecto.) Especifica qué categorías de eventos que desee registrar. En el archivo de configuración también especifique si desea registrar los eventos en un archivo de texto, el registro de eventos de Windows o un registro personalizado mediante la implementación de [TraceListener](https://msdn.microsoft.com/library/system.diagnostics.tracelistener(v=vs.110).aspx).

Las categorías de eventos de servidor incluyen a los siguientes tipos de mensajes:

| Origen | Mensajes |
| --- | --- |
| SignalR.SqlMessageBus | Instalación del proveedor de escalabilidad horizontal de Bus de mensajes de SQL, la operación de base de datos, errores y eventos de tiempo de espera |
| SignalR.ServiceBusMessageBus | Creación de tema de proveedor de escalabilidad horizontal de bus de servicio y suscripción, errores y eventos de mensajería |
| SignalR.RedisMessageBus | Eventos de error, conexión y desconexión del proveedor de ampliación de Redis |
| SignalR.ScaleoutMessageBus | Eventos de mensajería de escalabilidad horizontal |
| SignalR.Transports.WebSocketTransport | Eventos de conexión, la desconexión, mensajería y error de transporte de WebSocket |
| SignalR.Transports.ServerSentEventsTransport | Eventos de conexión, la desconexión, mensajería y error de transporte ServerSentEvents |
| SignalR.Transports.ForeverFrameTransport | Eventos de conexión, la desconexión, mensajería y error de transporte ForeverFrame |
| SignalR.Transports.LongPollingTransport | Eventos de conexión, la desconexión, mensajería y error de transporte LongPolling |
| SignalR.Transports.TransportHeartBeat | Eventos keepalive, conexión y desconexión de transporte |
| SignalR.ReflectedHubDescriptorProvider | Eventos de detección de concentrador |

<a id="server_text"></a>
### <a name="logging-server-events-to-text-files"></a>Registro de eventos de servidor en los archivos de texto

El código siguiente muestra cómo habilitar el seguimiento para cada categoría de eventos. Este ejemplo configura la aplicación para registrar eventos en archivos de texto.

**Código de servidor XML para habilitar el seguimiento**

[!code-html[Main](enabling-signalr-tracing/samples/sample1.html)]

En el código anterior, el `SignalRSwitch` entrada especifica la [TraceLevel](https://msdn.microsoft.com/library/system.diagnostics.tracelevel(v=vs.110).aspx) utilizado para los eventos enviados en el registro especificado. En este caso, se establece `Verbose` lo que significa que todos los mensajes de traza y de depuración se registran.

La salida siguiente muestra las entradas de la `transports.log.txt` archivo para una aplicación mediante el archivo de configuración anterior. Muestra una nueva conexión, una conexión quitada y eventos de latido del transporte.

[!code-console[Main](enabling-signalr-tracing/samples/sample2.cmd)]

<a id="server_eventlog"></a>
### <a name="logging-server-events-to-the-event-log"></a>Registro de eventos de servidor en el registro de eventos

Para registrar eventos en el registro de eventos en lugar de un archivo de texto, cambie los valores de las entradas de la `sharedListeners` nodo. El código siguiente muestra cómo registrar eventos de servidor en el registro de eventos:

**Código de servidor XML para registrar eventos en el registro de eventos**

[!code-xml[Main](enabling-signalr-tracing/samples/sample3.xml)]

Los eventos se registran en el registro de aplicación y están disponibles mediante el Visor de eventos, como se muestra a continuación:

![Visor de eventos que muestra los registros de SignalR](enabling-signalr-tracing/_static/image1.png)

> [!NOTE]
> Al utilizar el registro de eventos, establezca la **TraceLevel** a **Error** para que puedan administrar el número de mensajes.

<a id="net_client"></a>
## <a name="enabling-tracing-in-the-net-client-windows-desktop-apps"></a>Habilitar el seguimiento en el cliente de .NET (aplicaciones de escritorio de Windows)

El cliente .NET puede registrar los eventos en la consola, un archivo de texto o en un registro personalizado mediante la implementación de [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx).

Para habilitar el registro en el cliente. NET, establezca la conexión `TraceLevel` propiedad a un [TraceLevels](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.tracelevels(v=vs.118).aspx) valor y el `TraceWriter` propiedad NA platnou [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx) instancia.

<a id="desktop_console"></a>
### <a name="logging-desktop-client-events-to-the-console"></a>Registro de eventos de cliente de escritorio en la consola

El código de C# siguiente muestra cómo registrar eventos en el cliente de .NET en la consola:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample4.cs?highlight=2-3)]

<a id="desktop_text"></a>
### <a name="logging-desktop-client-events-to-a-text-file"></a>Registro de eventos de cliente de escritorio en un archivo de texto

El código de C# siguiente muestra cómo registrar eventos en el cliente de .NET en un archivo de texto:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample5.cs?highlight=4-5)]

La salida siguiente muestra las entradas de la `ClientLog.txt` archivo para una aplicación mediante el archivo de configuración anterior. Muestra el cliente que se conecta al servidor y el centro de invocar un método de cliente denominado `addMessage`:

[!code-console[Main](enabling-signalr-tracing/samples/sample6.cmd)]

<a id="phone"></a>
## <a name="enabling-tracing-in-windows-phone-8-clients"></a>Habilitar el seguimiento en los clientes de Windows Phone 8

Aplicaciones de SignalR para aplicaciones de Windows Phone usan el mismo cliente de .NET como aplicaciones de escritorio, pero [Console.Out](https://msdn.microsoft.com/library/system.console.out(v=vs.110).aspx) y escribir en un archivo con [StreamWriter](https://msdn.microsoft.com/library/system.io.streamwriter(v=vs.110).aspx) no están disponibles. En su lugar, deberá crear una implementación personalizada de [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) para realizar el seguimiento. 

<a id="phone_ui"></a>
### <a name="logging-windows-phone-client-events-to-the-ui"></a>Registro de eventos de cliente de Windows Phone en la interfaz de usuario

El [SignalR codebase](https://github.com/SignalR/SignalR/archive/master.zip) incluye un ejemplo de Windows Phone que escribe la salida de seguimiento en un [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) utiliza una [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) implementación denominada `TextBlockWriter`. Esta clase puede encontrarse en el **samples/Microsoft.AspNet.SignalR.Client.WP8.Samples** proyecto. Al crear una instancia de `TextBlockWriter`, pase actual [SynchronizationContext](https://msdn.microsoft.com/library/system.threading.synchronizationcontext(v=vs.110).aspx)y un [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) donde creará un [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) desea usar para seguimiento salida:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample7.cs)]

A continuación, se escribirá la salida del seguimiento a un nuevo [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) creado en el [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) que le haya pasado:

![](enabling-signalr-tracing/_static/image2.png)

<a id="phone_debug"></a>
### <a name="logging-windows-phone-client-events-to-the-debug-console"></a>Registro de eventos de cliente de Windows Phone en la consola de depuración

Para enviar la salida a la consola de depuración, en lugar de la interfaz de usuario, cree una implementación de [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) que escribe en la ventana de depuración y asígnelo a la conexión [TraceWriter](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connection.tracewriter(v=vs.118).aspx) propiedad:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample8.cs)]

A continuación, se escribirá la información de seguimiento en la ventana de depuración en Visual Studio:

![](enabling-signalr-tracing/_static/image3.png)

<a id="javascript"></a>
## <a name="enabling-tracing-in-the-javascript-client"></a>Habilitar el seguimiento en el cliente de JavaScript

Para habilitar el registro de cliente en una conexión, establezca el `logging` propiedad del objeto de conexión antes de llamar a la `start` método para establecer la conexión.

**Código de JavaScript de cliente para habilitar el seguimiento de la consola del explorador (con el proxy generado)**

[!code-javascript[Main](enabling-signalr-tracing/samples/sample9.js?highlight=1)]

**Código de JavaScript de cliente para habilitar el seguimiento de la consola del explorador (sin el proxy generado)**

[!code-javascript[Main](enabling-signalr-tracing/samples/sample10.js?highlight=2)]

Cuando se habilita el seguimiento, el cliente de JavaScript registra eventos en la consola del explorador. Para obtener acceso a la consola del explorador, vea [supervisión transportes](../getting-started/introduction-to-signalr.md#MonitoringTransports).

Captura de pantalla siguiente muestra a un cliente de JavaScript de SignalR con el seguimiento habilitado. Muestra eventos de invocación de concentrador y de conexión en la consola del explorador:

![Eventos de seguimiento de SignalR en la consola del explorador](enabling-signalr-tracing/_static/image4.png)
