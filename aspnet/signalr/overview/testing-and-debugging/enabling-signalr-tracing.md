---
uid: signalr/overview/testing-and-debugging/enabling-signalr-tracing
title: Habilitar el seguimiento de SignalR | Documentos de Microsoft
author: tfitzmac
description: Este documento describe cómo habilitar y configurar el seguimiento de los clientes y servidores de SignalR. El seguimiento le permite ver información acerca de los eventos de diagnóstico...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/08/2014
ms.topic: article
ms.assetid: 30060acb-be3e-4347-996f-3870f0c37829
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/testing-and-debugging/enabling-signalr-tracing
msc.type: authoredcontent
ms.openlocfilehash: ac979acf162084a195bb769f842e77ad2498c7f3
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
ms.locfileid: "28032823"
---
<a name="enabling-signalr-tracing"></a>Habilitar el seguimiento de SignalR
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este documento describe cómo habilitar y configurar el seguimiento de los clientes y servidores de SignalR. El seguimiento le permite ver información de diagnóstico acerca de los eventos de la aplicación de SignalR.
> 
> En este tema se escribió originalmente por Patrick Fletcher.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET Framework 4.5
> - SignalR versión 2
>   
> 
> 
> ## <a name="questions-and-comments"></a>Preguntas y comentarios
> 
> Vota sobre cómo le gustó este tutorial y lo que podemos mejorar en los comentarios en la parte inferior de la página. Si tiene preguntas que no están directamente relacionados con el tutorial, puede publicar para la [foro de ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).


Cuando se habilita el seguimiento, una aplicación de SignalR crea entradas de registro de eventos. Puede registrar eventos desde el cliente y el servidor. En la conexión del servidor de registros, proveedor de ampliación y eventos de bus de mensajes de seguimiento. Seguimiento de los eventos de conexión de registros de cliente. En SignalR 2.1 y versiones posteriores, el seguimiento en el cliente registra todo el contenido de los mensajes de invocación de concentrador.

## <a name="contents"></a>Contenido

- [Habilitar el seguimiento en el servidor](#server)

    - [Registro de eventos de servidor en archivos de texto](#server_text)
    - [Registro de eventos de servidor en el registro de eventos](#server_eventlog)
- [Habilitar el seguimiento en el cliente de .NET (aplicaciones de escritorio de Windows)](#net_client)

    - [Registro de eventos de cliente de escritorio en la consola de](#desktop_console)
    - [Registro de eventos de cliente de escritorio en un archivo de texto](#desktop_text)
- [Habilitar el seguimiento en los clientes de Windows Phone 8](#phone)

    - [Registro de eventos de cliente de Windows Phone en la interfaz de usuario](#phone_ui)
    - [Registro de eventos de cliente de Windows Phone en la consola de depuración](#phone_debug)
- [Habilitar el seguimiento en el cliente de JavaScript](#javascript)

<a id="server"></a>
## <a name="enabling-tracing-on-the-server"></a>Habilitar el seguimiento en el servidor

Se habilita el seguimiento en el servidor en el archivo de configuración de la aplicación (App.config o Web.config según el tipo de proyecto.) Especificar qué categorías de eventos que desee registrar. En el archivo de configuración, también especificar si se debe registrar los eventos en un archivo de texto, el registro de eventos de Windows o un registro personalizado mediante la implementación de [TraceListener](https://msdn.microsoft.com/library/system.diagnostics.tracelistener(v=vs.110).aspx).

Las categorías de eventos de servidor incluyen a los siguientes tipos de mensajes:

| Origen | Mensajes |
| --- | --- |
| SignalR.SqlMessageBus | El programa de instalación del proveedor de ampliación de Bus de mensajes de SQL, la operación de base de datos, errores y eventos de tiempo de espera |
| SignalR.ServiceBusMessageBus | Creación de tema de proveedor de ampliación de bus de servicio y suscripción, errores y eventos de mensajería |
| SignalR.RedisMessageBus | Eventos de conexión, desconexión y error de proveedor de ampliación de Redis |
| SignalR.ScaleoutMessageBus | Eventos de mensajería de ampliación |
| SignalR.Transports.WebSocketTransport | Eventos de conexión, desconexión, mensajería y error de transporte de WebSocket |
| SignalR.Transports.ServerSentEventsTransport | Eventos de conexión, desconexión, mensajería y error de transporte ServerSentEvents |
| SignalR.Transports.ForeverFrameTransport | Eventos de conexión, desconexión, mensajería y error de transporte ForeverFrame |
| SignalR.Transports.LongPollingTransport | Eventos de conexión, desconexión, mensajería y error de transporte LongPolling |
| SignalR.Transports.TransportHeartBeat | Eventos de conexión y desconexión, keepalive de transporte |
| SignalR.ReflectedHubDescriptorProvider | Eventos de detección de base de datos central |

<a id="server_text"></a>
### <a name="logging-server-events-to-text-files"></a>Registro de eventos de servidor en archivos de texto

El código siguiente muestra cómo habilitar el seguimiento para cada categoría de eventos. Este ejemplo configura la aplicación para registrar eventos en archivos de texto.

**Código de servidor XML para habilitar el seguimiento**

[!code-html[Main](enabling-signalr-tracing/samples/sample1.html)]

En el código anterior, el `SignalRSwitch` entrada especifica la [TraceLevel](https://msdn.microsoft.com/library/system.diagnostics.tracelevel(v=vs.110).aspx) utilizados para los eventos enviados en el registro especificado. En este caso, se establece `Verbose` lo que significa que todos los de depuración y seguimiento de mensajes se registran.

La siguiente salida muestra las entradas de la `transports.log.txt` archivo para una aplicación con el archivo de configuración anterior. Muestra una nueva conexión, una conexión quitada y eventos de latido del transporte.

[!code-console[Main](enabling-signalr-tracing/samples/sample2.cmd)]

<a id="server_eventlog"></a>
### <a name="logging-server-events-to-the-event-log"></a>Registro de eventos de servidor en el registro de eventos

Para registrar eventos en el registro de eventos en lugar de un archivo de texto, cambiar los valores de las entradas de la `sharedListeners` nodo. El código siguiente muestra cómo registrar eventos de servidor en el registro de eventos:

**Código de servidor XML para registrar eventos en el registro de eventos**

[!code-xml[Main](enabling-signalr-tracing/samples/sample3.xml)]

Los eventos se registran en el registro de aplicación y están disponibles mediante el Visor de eventos, tal y como se muestra a continuación:

![Visor de eventos que muestra registros de SignalR](enabling-signalr-tracing/_static/image1.png)

> [!NOTE]
> Al utilizar el registro de eventos, establezca la **TraceLevel** a **Error** para mantener el número de mensajes fáciles de administrar.

<a id="net_client"></a>
## <a name="enabling-tracing-in-the-net-client-windows-desktop-apps"></a>Habilitar el seguimiento en el cliente de .NET (aplicaciones de escritorio de Windows)

El cliente .NET puede registrar eventos en la consola, un archivo de texto, o en un registro personalizado mediante la implementación de [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx).

Para habilitar el registro en el cliente. NET, establezca la conexión `TraceLevel` propiedad a una [TraceLevels](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.tracelevels(v=vs.118).aspx) valor y el `TraceWriter` propiedad válido [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx) instancia.

<a id="desktop_console"></a>
### <a name="logging-desktop-client-events-to-the-console"></a>Registro de eventos de cliente de escritorio en la consola de

El siguiente código de C# muestra cómo registrar los eventos en el cliente de .NET en la consola:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample4.cs?highlight=2-3)]

<a id="desktop_text"></a>
### <a name="logging-desktop-client-events-to-a-text-file"></a>Registro de eventos de cliente de escritorio en un archivo de texto

El siguiente código de C# muestra cómo registrar los eventos en el cliente de .NET en un archivo de texto:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample5.cs?highlight=4-5)]

La siguiente salida muestra las entradas de la `ClientLog.txt` archivo para una aplicación con el archivo de configuración anterior. Muestra el cliente que se conecta al servidor y el concentrador de invocar un método de cliente denominado `addMessage`:

[!code-console[Main](enabling-signalr-tracing/samples/sample6.cmd)]

<a id="phone"></a>
## <a name="enabling-tracing-in-windows-phone-8-clients"></a>Habilitar el seguimiento en los clientes de Windows Phone 8

Aplicaciones de SignalR para las aplicaciones de Windows Phone utilizan el mismo cliente de .NET como aplicaciones de escritorio, pero [Console.Out](https://msdn.microsoft.com/library/system.console.out(v=vs.110).aspx) y escribir en un archivo con [StreamWriter](https://msdn.microsoft.com/library/system.io.streamwriter(v=vs.110).aspx) no están disponibles. En su lugar, debe crear una implementación personalizada de [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) para el seguimiento. 

<a id="phone_ui"></a>
### <a name="logging-windows-phone-client-events-to-the-ui"></a>Registro de eventos de cliente de Windows Phone en la interfaz de usuario

El [SignalR codebase](https://github.com/SignalR/SignalR/archive/master.zip) incluye un ejemplo de Windows Phone que escribe los resultados del seguimiento a un [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) usando un comparador [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) implementación llama `TextBlockWriter`. Esta clase puede encontrarse en el **samples/Microsoft.AspNet.SignalR.Client.WP8.Samples** proyecto. Al crear una instancia de `TextBlockWriter`, pasar actual [SynchronizationContext](https://msdn.microsoft.com/library/system.threading.synchronizationcontext(v=vs.110).aspx)y un [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) donde va a crear un [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) desea usar para seguimiento resultado:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample7.cs)]

El resultado del seguimiento, a continuación, se escribirá en un nuevo [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) creado en el [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) se pasó en:

![](enabling-signalr-tracing/_static/image2.png)

<a id="phone_debug"></a>
### <a name="logging-windows-phone-client-events-to-the-debug-console"></a>Registro de eventos de cliente de Windows Phone en la consola de depuración

Para enviar la salida a la consola de depuración, en lugar de la interfaz de usuario, cree una implementación de [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) que escribe en la ventana de depuración y asignarlo a la conexión [TraceWriter](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connection.tracewriter(v=vs.118).aspx) propiedad:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample8.cs)]

Información de seguimiento, a continuación, se escribirán en la ventana de depuración en Visual Studio:

![](enabling-signalr-tracing/_static/image3.png)

<a id="javascript"></a>
## <a name="enabling-tracing-in-the-javascript-client"></a>Habilitar el seguimiento en el cliente de JavaScript

Para habilitar el registro de cliente en una conexión, establecer el `logging` propiedad en el objeto de conexión antes de llamar a la `start` método para establecer la conexión.

**Código de JavaScript de cliente para habilitar el seguimiento de la consola del explorador (con el proxy generado)**

[!code-javascript[Main](enabling-signalr-tracing/samples/sample9.js?highlight=1)]

**Código de JavaScript de cliente para habilitar el seguimiento de la consola del explorador (sin el proxy generado)**

[!code-javascript[Main](enabling-signalr-tracing/samples/sample10.js?highlight=2)]

Cuando el seguimiento está habilitado, el cliente de JavaScript registra eventos en la consola del explorador. Para obtener acceso a la consola del explorador, consulte [supervisión transportes](../getting-started/introduction-to-signalr.md#MonitoringTransports).

Captura de pantalla siguiente muestra a un cliente de SignalR JavaScript con el seguimiento habilitado. Muestra eventos de invocación de concentrador y de conexión en la consola del explorador:

![Eventos de seguimiento de SignalR en la consola del explorador](enabling-signalr-tracing/_static/image4.png)
