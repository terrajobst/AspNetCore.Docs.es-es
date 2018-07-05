---
uid: signalr/overview/guide-to-the-api/handling-connection-lifetime-events
title: Entender y controlar eventos de duración de la conexión en SignalR | Microsoft Docs
author: pfletcher
description: En este artículo se describe cómo utilizar los eventos expuestos por la API Hubs.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 03960de2-8d95-4444-9169-4426dcc64913
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/guide-to-the-api/handling-connection-lifetime-events
msc.type: authoredcontent
ms.openlocfilehash: cc7725858057c6313b3b4210199c5bbb636f4592
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366908"
---
<a name="understanding-and-handling-connection-lifetime-events-in-signalr"></a>Entender y controlar eventos de duración de la conexión en SignalR
====================
por [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)

> En este artículo se proporciona información general de los eventos de conexión, la reconexión y la desconexión de SignalR que se pueden controlar y la configuración de tiempo de espera y keepalive que puede configurar.
> 
> Este artículo se supone que ya tiene algunos conocimientos de eventos de duración de la conexión y SignalR. Para obtener una introducción a SignalR, consulte [Introducción a SignalR](../getting-started/introduction-to-signalr.md). Para las listas de eventos de duración de la conexión, consulte los siguientes recursos:
> 
> - [Cómo controlar eventos de duración de la conexión en la clase Hub](hubs-api-guide-server.md#connectionlifetime)
> - [Cómo controlar eventos de duración de la conexión en los clientes de JavaScript](hubs-api-guide-javascript-client.md#connectionlifetime)
> - [Cómo controlar eventos de duración de la conexión en los clientes de .NET](hubs-api-guide-net-client.md#connectionlifetime)
> 
> ## <a name="software-versions-used-in-this-topic"></a>Versiones de software que se usa en este tema
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - Versión 2 de SignalR
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a>Versiones anteriores de este tema.
> 
> Para obtener información acerca de las versiones anteriores de SignalR, consulte [versiones anteriores de SignalR](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Preguntas y comentarios
> 
> Deje comentarios sobre cómo le gustó de este tutorial y que podíamos mejorar en los comentarios en la parte inferior de la página. Si tiene preguntas que no están directamente relacionados con el tutorial, puede publicarlos en el [foro de ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).


## <a name="overview"></a>Información general

Este artículo contiene las siguientes secciones:

- [Escenarios y la terminología de duración de conexión](#terminology)

    - [Las conexiones de SignalR, conexiones de transporte y conexiones físicas](#signalrvstransport)
    - [Escenarios de desconexión del transporte](#transportdisconnect)
    - [Escenarios de desconexión del cliente](#clientdisconnect)
    - [Escenarios de desconexión del servidor](#serverdisconnect)
- [Configuración de tiempo de espera y keepalive](#timeoutkeepalive)

    - [Valor de ConnectionTimeout](#connectiontimeout)
    - [DisconnectTimeout](#disconnecttimeout)
    - [KeepAlive](#keepalive)
    - [Cómo cambiar la configuración de tiempo de espera y keepalive](#changetimeout)
- [Cómo notificar al usuario acerca de las desconexiones](#notifydisconnect)
- [Cómo volver a conectar de forma continua](#continuousreconnect)
- [Cómo desconectar a un cliente en el código de servidor](#disconnectclientfromserver)
- [Detectar el motivo de una desconexión](#detectingreasonfordisconnection)

Son vínculos a temas de referencia de API a la versión 4.5 de .NET de la API. Si usa .NET 4, consulte [la versión 4 de .NET de los temas de la API](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).

<a id="terminology"></a>

## <a name="connection-lifetime-terminology-and-scenarios"></a>Escenarios y la terminología de duración de conexión

El `OnReconnected` puede ejecutar el controlador de eventos en un concentrador SignalR directamente después `OnConnected` pero no después `OnDisconnected` de un cliente determinado. El motivo que puede tener una reconexión sin una desconexión es que hay varias maneras en que se utiliza la palabra "conexión" en SignalR.

<a id="signalrvstransport"></a>

### <a name="signalr-connections-transport-connections-and-physical-connections"></a>Las conexiones de SignalR, conexiones de transporte y conexiones físicas

En este artículo se diferencia entre *conexiones SignalR*, *las conexiones de transporte*, y *conexiones físicas*:

- **Conexión de SignalR** hace referencia a una relación lógica entre un cliente y una dirección URL del servidor, mantenida por la API SignalR y se identifica mediante un identificador de conexión. Los datos acerca de esta relación es mantenidos por SignalR y se utilizan para establecer una conexión de transporte. Los extremos de la relación y SignalR se deshace de los datos cuando el cliente llama a la `Stop` se llega a método o un límite de tiempo de espera mientras se está intentando volver a establecer una conexión de transporte pierde SignalR.
- **Conexión de transporte** hace referencia a una relación lógica entre un cliente y un servidor, mantenido por una de las API de transporte cuatro: WebSockets, eventos de servidor envió enmarcar indefinidamente o largos de sondeo. SignalR usa la API de transporte para crear una conexión de transporte y la API de transporte depende de la existencia de una conexión de red físico para crear la conexión de transporte. La conexión de transporte finaliza cuando termina de SignalR, o cuando el transporte API detecta que se ha interrumpido la conexión física.
- **Conexión física** hace referencia a los vínculos de red físico, cables, las señales inalámbricas, enrutadores, etc., que facilitan la comunicación entre un equipo cliente y un equipo de servidor. La conexión física debe estar presente con el fin de establecer una conexión de transporte, y se debe establecer una conexión de transporte con el fin de establecer una conexión de SignalR. Sin embargo, interrumpir la conexión física no siempre finalizar inmediatamente la conexión de transporte o la conexión de SignalR, como se explicará más adelante en este tema.

En el diagrama siguiente, la conexión de SignalR está representada por la API Hubs y la capa PersistentConnection API SignalR, la conexión de transporte se representa mediante la capa de transporte y la conexión física es representada por las líneas entre el servidor y los clientes.

![Diagrama de arquitectura de SignalR](handling-connection-lifetime-events/_static/image1.png)

Cuando se llama a la `Start` método en un cliente de SignalR, se proporciona código de cliente de SignalR con toda la información que necesita con el fin de establecer una conexión a un servidor física. Código de cliente de SignalR usa esta información para realizar una solicitud HTTP y establecer una conexión física que usa uno de los métodos de transporte de cuatro. Si se produce un error en la conexión de transporte o se produce un error en el servidor, la conexión de SignalR no desaparezcan inmediatamente porque el cliente todavía tiene la información que necesita para volver a establecer automáticamente una nueva conexión de transporte con la misma dirección URL de SignalR. En este escenario, no está implicada ninguna intervención de la aplicación de usuario y, cuando el código de cliente de SignalR establece una nueva conexión de transporte, no se inicia una nueva conexión de SignalR. La continuidad de la conexión de SignalR se refleja en el hecho de que el identificador de conexión, que se crea cuando se llama a la `Start` , el método no cambia.

El `OnReconnected` controlador de eventos en el concentrador se ejecuta cuando una conexión de transporte se restablece automáticamente después de haberse perdido. El `OnDisconnected` ejecuta el controlador de eventos al final de una conexión de SignalR. Puede finalizar una conexión de SignalR en cualquiera de las maneras siguientes:

- Si el cliente llama a la `Stop` método, se envía un mensaje de detención al servidor y cliente y servidor de terminan inmediatamente la conexión de SignalR.
- Después de que se pierde la conectividad entre cliente y servidor, el cliente intenta volver a conectar y el servidor espera a que el cliente volver a conectar. Si intenta volver a conectarse no tiene éxito y finaliza el período de tiempo de espera de desconexión, cliente y servidor de terminar la conexión de SignalR. El cliente deja de intentar volver a conectar y el servidor elimina de su representación de la conexión de SignalR.
- Si el cliente deja de ejecutarse sin tener una oportunidad para llamar a la `Stop` método, el servidor espera a que el cliente puede volver a conectar y, a continuación, finaliza la conexión de SignalR tras el período de tiempo de espera de desconexión.
- Si el servidor deja de ejecutar, el cliente intenta volver a conectar (volver a crea la conexión de transporte) y, a continuación, finaliza la conexión de SignalR tras el período de tiempo de espera de desconexión.

Cuando no hay ningún problema de conexión y la aplicación de usuario finaliza la conexión de SignalR mediante una llamada a la `Stop` método, la conexión de SignalR y la conexión de transporte empiezan y terminan en aproximadamente al mismo tiempo. Las secciones siguientes describen con más detalle los otros escenarios.

<a id="transportdisconnect"></a>

### <a name="transport-disconnection-scenarios"></a>Escenarios de desconexión del transporte

Conexiones físicas podrían ser lentas o podría haber interrupciones en la conectividad. Dependiendo de factores como la longitud de la interrupción, se puede quitar la conexión de transporte. SignalR, a continuación, intenta volver a establecer la conexión de transporte. En ocasiones la conexión de transporte API detecta la interrupción y quita la conexión de transporte, y SignalR descubre inmediatamente que se pierde la conexión. En otros escenarios, ni la API de conexión de transporte ni SignalR se da cuenta inmediatamente que se perdió la conectividad. Para todos los transportes, excepto el sondeo largo, el cliente de SignalR usa una función denominada *keepalive* para comprobar si la pérdida de conectividad que la API de transporte no puede detectar. Para obtener información acerca de conexiones de sondeo largo, consulte [tiempo de espera y keepalive](#timeoutkeepalive) más adelante en este tema.

Cuando una conexión está inactiva, periódicamente el servidor envía un paquete keepalive al cliente. A partir de la fecha en que se escribe este artículo, la frecuencia predeterminada es cada 10 segundos. Realizando escuchas para estos paquetes, pueden indicar a los clientes si hay un problema de conexión. Si no se recibe un paquete keepalive cuando se esperaba, poco después el cliente asume que hay problemas de conexión, como lentitud o interrupciones. Si el keepalive todavía no se recibe después de un tiempo más largo, el cliente supone que se quitó la conexión y comienza a intentar la reconexión.

El diagrama siguiente muestra los eventos de cliente y servidor que se generan en un escenario típico cuando hay problemas con la conexión física que no se reconocen inmediatamente por la API de transporte. El diagrama se aplica a las siguientes circunstancias:

- El transporte es WebSockets, marco indefinidamente o eventos enviados por el servidor.
- Hay distintos períodos de interrupción en la conexión de red física.
- La API de transporte no conocen la existencia de las interrupciones, por lo que SignalR se basa en la funcionalidad de keepalive detectarlos.

![Desconexiones de transporte](handling-connection-lifetime-events/_static/image2.png)

Si el cliente pasa a volver a conectar el modo pero no puede establecer una conexión de transporte dentro del límite de tiempo de espera de desconexión, el servidor finaliza la conexión de SignalR. Cuando esto ocurre, el servidor ejecuta el centro `OnDisconnected` método y pone en cola un mensaje de desconexión para enviar al cliente en caso de que el cliente se administra a conectarse más tarde. Si el cliente, a continuación, volver a conectar, recibe el comando de desconexión y llama el `Stop` método. En este escenario, `OnReconnected` no se ejecuta cuando se vuelve a conectar el cliente, y `OnDisconnected` no se ejecuta cuando el cliente llama a `Stop`. El siguiente diagrama muestra este escenario.

![Interrupciones de transporte - tiempo de espera del servidor](handling-connection-lifetime-events/_static/image3.png)

Los eventos de duración de conexión de SignalR que se generen en el cliente son los siguientes:

- `ConnectionSlow` evento de cliente.

    Se genera cuando una proporción del período de tiempo de espera de keepalive preestablecida ha transcurrido desde el último mensaje o keepalive ping se recibió. El período de advertencia de tiempo de espera de keepalive predeterminado es 2/3 del tiempo de espera keepalive. El tiempo de espera de keepalive es 20 segundos, por lo que se produce la advertencia a los 13 segundos aproximadamente.

    De forma predeterminada, el servidor envía pings keepalive cada 10 segundos y el cliente comprueba si keepalive pings sobre cada 2 segundos (un tercio de la diferencia entre el valor de tiempo de espera de conexión abierta y el valor de advertencia de tiempo de espera de keepalive).

    Si el transporte API deja de tener en cuenta una desconexión, SignalR podría informar de la desconexión antes de que pase el período de advertencia de tiempo de espera de keepalive. En ese caso, el `ConnectionSlow` no se provocaría el evento, y SignalR podría ir directamente a la `Reconnecting` eventos.
- `Reconnecting` evento de cliente.

    Se genera cuando (a) el transporte API detecta que se pierde la conexión, o (b) el período de tiempo de espera de keepalive ha transcurrido desde el último mensaje o keepalive ping se recibió. El código de cliente de SignalR empieza a intentar volver a conectarse. Puede controlar este evento si desea que la aplicación para realizar alguna acción cuando se pierde una conexión de transporte. Actualmente, el período de tiempo de espera predeterminado keepalive es 20 segundos.

    Si el código de cliente intenta llamar a un método de concentrador mientras SignalR está en volver a conectarse de modo, SignalR intentará enviar el comando. La mayoría de los casos, estos intentos se producirá un error, pero en algunos casos podría realizarse correctamente. Para los eventos enviados por el servidor, siempre el marco y transportes de sondeo largo, SignalR usa dos canales de comunicación, uno que utiliza el cliente para enviar mensajes y otro que usa para recibir mensajes. El canal usado para la recepción es el permanentemente abierta y que es lo que se cierra cuando se interrumpe la conexión física. El canal que se utiliza para enviar sigue estando disponible, por lo que si se restaura la conectividad física, una llamada de método de cliente al servidor puede ser satisfactoria antes de que se restablezca el canal de recepción. El valor devuelto podría no recibir hasta SignalR vuelve a abre el canal usado para la recepción.
- `Reconnected` evento de cliente.

    Se genera cuando se restablece la conexión de transporte. El `OnReconnected` ejecuta el controlador de eventos en el concentrador.
- `Closed` evento de cliente (`disconnected` eventos en JavaScript).

    Se produce cuando expira el período de tiempo de espera de desconexión mientras el código de cliente de SignalR está intentando volver a conectarse después de perder la conexión de transporte. Desconecte el valor predeterminado es de 30 segundos de tiempo de espera. (También se genera este evento cuando la conexión finaliza debido a que el `Stop` se llama al método.)

Las interrupciones de la conexión de transporte que no se detectan mediante la API de transporte y no retrasar la recepción de keepalive pings desde el servidor durante más tiempo que el período de advertencia de tiempo de espera de keepalive quizás no provoque cualquier conexión que se generen eventos de duración.

Algunos entornos de red deliberadamente cerrar las conexiones inactivas y otra función de los paquetes keepalive es ayudar a evitar que esto al permitir que estas redes se sabe que una conexión de SignalR está en uso. En casos extremos la frecuencia predeterminada de pings keepalive no podría ser suficiente para impedir las conexiones cerradas. En ese caso puede configurar keepalive pings se envíen con más frecuencia. Para obtener más información, consulte [tiempo de espera y keepalive](#timeoutkeepalive) más adelante en este tema.

> [!NOTE] 
> 
> **Importante**: no se garantiza que la secuencia de eventos que se describen aquí. SignalR realiza todos los intentos para provocar eventos de duración de la conexión de una manera predecible según este esquema, pero existen muchas variaciones de eventos de la red y de muchas maneras en que los marcos de comunicaciones subyacente como las API de transporte de controlan. Por ejemplo, el `Reconnected` no es posible que se produce el evento cuando se vuelve a conectar el cliente, o la `OnConnected` controlador en el servidor puede ejecutar si el intento para establecer una conexión es incorrecto. Este tema describe únicamente los efectos que normalmente se generarían en determinadas circunstancias típicas.


<a id="clientdisconnect"></a>

### <a name="client-disconnection-scenarios"></a>Escenarios de desconexión del cliente

En un explorador del cliente, el código de cliente de SignalR que mantiene una conexión SignalR se ejecuta en el contexto de JavaScript de una página web. Que ha razón por la conexión de SignalR tiene que finalizar cuando se navega desde una página a otra y ese por qué tiene varias conexiones con varios identificadores de conexión si se conecta desde varias ventanas del explorador o tabulaciones. Cuando el usuario cierra una ventana o ficha, o navega a una página nueva o actualiza la página, la conexión de SignalR finaliza inmediatamente porque el código de cliente de SignalR controla ese evento de explorador para llamadas y el `Stop` método. En estos escenarios, o en cualquier plataforma de cliente cuando la aplicación llama a la `Stop` método, el `OnDisconnected` controlador de eventos que se ejecuta inmediatamente en el servidor y el cliente genera el `Closed` evento (el evento se denomina `disconnected` en JavaScript).

Si una aplicación cliente o el equipo que se está ejecutando en se bloquea o se suspende (por ejemplo, cuando el usuario cierra el equipo portátil), el servidor no se informa sobre lo que sucedió. Como sabe que el servidor, la pérdida del cliente podría ser debido a interrupciones de conectividad y el cliente podría estar intentando volver a conectar. Por lo tanto, en estos casos el servidor espera para dar al cliente una oportunidad de volver a conectar, y `OnDisconnected` no se ejecuta hasta que el período de tiempo de espera de desconexión expire (aproximadamente 30 segundos de forma predeterminada). El siguiente diagrama muestra este escenario.

![Error en el equipo cliente](handling-connection-lifetime-events/_static/image4.png)

<a id="serverdisconnect"></a>

### <a name="server-disconnection-scenarios"></a>Escenarios de desconexión del servidor

Cuando un servidor se queda sin conexión, se reinicia, se produce un error, el dominio de aplicación recicla, etc., el resultado puede ser similar a una pérdida de conexión o el transporte de API y SignalR podrían saber de inmediato que el servidor ha desaparecido y SignalR podría comenzar intentando volver a conectar sin generar el `ConnectionSlow` eventos. Si el cliente entra en volver a conectar el modo y si el servidor recupera o se reinicia o un nuevo servidor se pone en línea antes de que expire el período de tiempo de espera de desconexión, el cliente se volverá a conectar al servidor nuevo o restaurado. En ese caso, la conexión de SignalR continúa en el cliente y el `Reconnected` provoca el evento. En el primer servidor, `OnDisconnected` nunca se ejecuta y en el nuevo servidor, `OnReconnected` se ejecuta aunque `OnConnected` nunca se ejecutó para el cliente en ese servidor antes. (El efecto es el mismo si el cliente se vuelve a conectar al mismo servidor después de un reciclaje del dominio de aplicación o reiniciar el equipo, porque cuando reinicia el servidor no tiene memoria de la actividad de conexión anterior). El diagrama siguiente se da por supuesto que la API de transporte se da cuenta pierde la conexión inmediatamente, por lo que el `ConnectionSlow` no se produce el evento.

![Error del servidor y reconexión](handling-connection-lifetime-events/_static/image5.png)

Si un servidor no está disponible dentro del período de tiempo de espera de desconexión, finaliza la conexión de SignalR. En este escenario, el `Closed` eventos (`disconnected` en clientes de JavaScript) se genera en el cliente pero `OnDisconnected` nunca se llama en el servidor. El diagrama siguiente se da por supuesto que la API de transporte no conocen la existencia de la conexión perdida, por lo que se detecta mediante la funcionalidad de SignalR keepalive y `ConnectionSlow` provoca el evento.

![Tiempo de espera y errores de servidor](handling-connection-lifetime-events/_static/image6.png)

<a id="timeoutkeepalive"></a>

## <a name="timeout-and-keepalive-settings"></a>Configuración de tiempo de espera y keepalive

El valor predeterminado `ConnectionTimeout`, `DisconnectTimeout`, y `KeepAlive` valores son adecuados para la mayoría de los escenarios, pero puede cambiarse si el entorno tiene necesidades especiales. Por ejemplo, si su entorno de red cierra las conexiones que están inactivas durante 5 segundos, tendrá que disminuya el valor keepalive.

<a id="connectiontimeout"></a>

### <a name="connectiontimeout"></a>Valor de ConnectionTimeout

Esta configuración representa la cantidad de tiempo que una conexión de transporte abiertos y esperando una respuesta antes de cerrarla y abrir una nueva conexión. El valor predeterminado es de 110 segundos.

Esta configuración aplica solo cuando la funcionalidad keepalive está deshabilitada, que normalmente se aplica solo a la larga transporte de sondeo. El siguiente diagrama ilustra el efecto de esta configuración en un valor largo conexión de transporte de sondeo.

![Conexión de transporte de sondeo largo](handling-connection-lifetime-events/_static/image7.png)

<a id="disconnecttimeout"></a>

### <a name="disconnecttimeout"></a>DisconnectTimeout

Esta configuración representa la cantidad de tiempo de espera después de que se pierde una conexión de transporte antes de generar el `Disconnected` eventos. El valor predeterminado es 30 segundos. Al establecer `DisconnectTimeout`, `KeepAlive` se establece automáticamente en 1/3 de la `DisconnectTimeout` valor.

<a id="keepalive"></a>

### <a name="keepalive"></a>KeepAlive

Esta configuración representa la cantidad de tiempo de espera antes de enviar un paquete keepalive a través de una conexión inactiva. El valor predeterminado es 10 segundos. Este valor no debe tener más de 1/3 de la `DisconnectTimeout` valor.

Si desea establecer `DisconnectTimeout` y `KeepAlive`, establezca `KeepAlive` después `DisconnectTimeout`. En caso contrario, su `KeepAlive` configuración se sobrescribirá al `DisconnectTimeout` establece automáticamente `KeepAlive` a 1/3 del valor de tiempo de espera.

Si desea deshabilitar la funcionalidad de keepalive, establezca `KeepAlive` en null. KeepAlive funcionalidad se deshabilita automáticamente para la larga transporte de sondeo.

<a id="changetimeout"></a>

### <a name="how-to-change-timeout-and-keepalive-settings"></a>Cómo cambiar la configuración de tiempo de espera y keepalive

Para cambiar los valores predeterminados para estos valores, establecerlos en `Application_Start` en su *Global.asax* de archivos, como se muestra en el ejemplo siguiente. Los valores mostrados en el código de ejemplo son el mismo que los valores predeterminados.

[!code-csharp[Main](handling-connection-lifetime-events/samples/sample1.cs)]

<a id="notifydisconnect"></a>

## <a name="how-to-notify-the-user-about-disconnections"></a>Cómo notificar al usuario acerca de las desconexiones

En algunas aplicaciones puede mostrar un mensaje al usuario cuando hay problemas de conectividad. Tiene varias opciones sobre cómo y cuándo deben hacerlo. Ejemplos de código siguientes son para un cliente de JavaScript mediante el proxy generado.

- Controlar la `connectionSlow` eventos para mostrar un mensaje en cuanto SignalR es consciente de los problemas de conexión, antes de enviarlos a volver a conectarse de modo.

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample2.js)]
- Controlar la `reconnecting` eventos que se muestra un mensaje cuando SignalR es compatible con una desconexión y se va a volver a conectarse de modo.

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample3.js)]
- Controlar la `disconnected` ha agotado el evento que se muestre un mensaje cuando se intenta volver a conectarse. En este escenario, la única forma de volver a establecer una conexión con el servidor nuevo es reiniciar la conexión de SignalR mediante una llamada a la `Start` método, que creará un nuevo identificador de conexión. Ejemplo de código siguiente utiliza una marca para asegurarse de que emitir la notificación solo después de un tiempo de espera de reconexión, no después de una finalización normal a la conexión de SignalR causada por una llamada a la `Stop` método.

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample4.js)]

<a id="continuousreconnect"></a>

## <a name="how-to-continuously-reconnect"></a>Cómo volver a conectar de forma continua

En algunas aplicaciones puede volver a establecer automáticamente una conexión después de que se ha perdido y se ha agotado el intento de reconexión. Para ello, puede llamar a la `Start` método desde su `Closed` controlador de eventos (`disconnected` controlador de eventos en los clientes de JavaScript). Es posible que desee esperar un período de tiempo antes de llamar a `Start` con el fin de evitar hacerlo demasiado con frecuencia cuando el servidor o la conexión física no estén disponibles. Ejemplo de código siguiente es para un cliente de JavaScript mediante el proxy generado.

[!code-javascript[Main](handling-connection-lifetime-events/samples/sample5.js)]

Un posible problema de tener en cuenta en los clientes móviles es que los intentos de reconexión continua cuando la conexión física o el servidor no está disponible podrían producir purga innecesarios de la batería.

<a id="disconnectclientfromserver"></a>

## <a name="how-to-disconnect-a-client-in-server-code"></a>Cómo desconectar a un cliente en el código de servidor

SignalR versión 2 no tiene una API de servidor integrada para se desconectan los clientes. Hay [planes para agregar esta funcionalidad en el futuro](https://github.com/SignalR/SignalR/issues/2101). En la versión actual de SignalR, la manera más sencilla para desconectar a un cliente desde el servidor es implementar un método de desconexión del cliente y llamar a ese método desde el servidor. Ejemplo de código siguiente muestra un método de desconexión de un cliente de JavaScript mediante el proxy generado.

[!code-javascript[Main](handling-connection-lifetime-events/samples/sample6.js)]

> [!WARNING]
> Seguridad: este método para que se desconectan los clientes ni a la API integrada propuesta abordará el escenario de pirateados clientes que ejecutan código malintencionado, ya que podrían volver a conectar los clientes o el código pirateado podría quitar el `stopClient` método o cambio lo que hace. El lugar adecuado para implementar la protección de estado de denegación de servicio (DOS) no está en el marco de trabajo o la capa del servidor, sino en su lugar en la infraestructura de front-end.


<a id="detectingreasonfordisconnection"></a>
## <a name="detecting-the-reason-for-a-disconnection"></a>Detectar el motivo de una desconexión

2.1 SignalR agrega una sobrecarga en el servidor `OnDisconnect` evento que indica si el cliente se desconectó deliberadamente en lugar de tiempo de espera. El `StopCalled` del parámetro es true si el cliente cierra explícitamente la conexión. En JavaScript, si un error de servidor liderado el cliente para desconectarse, la información de error se pasará al cliente como `$.connection.hub.lastError`.

**Código de servidor de C#: `stopCalled` parámetro**

[!code-csharp[Main](handling-connection-lifetime-events/samples/sample7.cs?highlight=1,3)]

**Código de cliente de JavaScript: acceso a `lastError` en el `disconnect` eventos.**

[!code-javascript[Main](handling-connection-lifetime-events/samples/sample8.js?highlight=2-3)]
