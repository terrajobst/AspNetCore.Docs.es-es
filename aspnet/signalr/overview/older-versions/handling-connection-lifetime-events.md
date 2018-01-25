---
uid: signalr/overview/older-versions/handling-connection-lifetime-events
title: "Comprender y controlar eventos de duración de la conexión de SignalR 1.x | Documentos de Microsoft"
author: pfletcher
description: "Este artículo describe cómo utilizar los eventos expuestos por la API de concentradores."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/05/2013
ms.topic: article
ms.assetid: e608e263-264d-448b-b0eb-6eeb77713b22
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/handling-connection-lifetime-events
msc.type: authoredcontent
ms.openlocfilehash: 4fe77769c27dd46967da2e1d68791d7142021d99
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="understanding-and-handling-connection-lifetime-events-in-signalr-1x"></a>Comprender y controlar eventos de duración de la conexión de SignalR 1.x
====================
por [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)

> Este artículo proporciona información general sobre los eventos de conexión y volver a conectarse, desconexión de SignalR que se pueden controlar y la configuración de tiempo de espera y keepalive que puede configurar.
> 
> El artículo se da por supuesto que ya tiene algunos conocimientos de eventos de duración de la conexión y SignalR. Para obtener una introducción a SignalR, consulte [SignalR - información general - Getting Started](index.md). Para obtener listas de eventos de duración de la conexión, consulte los siguientes recursos:
> 
> - [Cómo controlar los eventos de duración de conexión en la clase de base de datos central](index.md)
> - [Cómo controlar los eventos de duración de conexión en los clientes de JavaScript](index.md)
> - [Cómo controlar los eventos de duración de conexión en los clientes de .NET](index.md)


## <a name="overview"></a>Información general

Este artículo contiene las siguientes secciones:

- [Escenarios y la terminología de duración de conexión](#terminology)

    - [Conexiones SignalR, las conexiones de transporte y conexiones físicas](#signalrvstransport)
    - [Escenarios de desconexión del transporte](#transportdisconnect)
    - [Escenarios de desconexión de cliente](#clientdisconnect)
    - [Escenarios de desconexión del servidor](#serverdisconnect)
- [Configuración de tiempo de espera y keepalive](#timeoutkeepalive)

    - [ConnectionTimeout](#connectiontimeout)
    - [DisconnectTimeout](#disconnecttimeout)
    - [KeepAlive](#keepalive)
    - [Cómo cambiar la configuración de tiempo de espera y keepalive](#changetimeout)
- [Cómo notificar al usuario de desconexiones](#notifydisconnect)
- [Cómo volver a conectar de forma continua](#continuousreconnect)
- [Cómo desconectar a un cliente en el código de servidor](#disconnectclientfromserver)

Vínculos a temas de referencia de la API están a la versión de .NET 4.5 de la API. Si usa .NET 4, consulte [la versión 4 de .NET de los temas de la API](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).

<a id="terminology"></a>

## <a name="connection-lifetime-terminology-and-scenarios"></a>Escenarios y la terminología de duración de conexión

El `OnReconnected` controlador de eventos en un concentrador SignalR puede ejecutar directamente después de `OnConnected` , pero no después `OnDisconnected` para un cliente determinado. La razón que puede tener una reconexión sin realizar una desconexión es que hay varias maneras en que se utiliza la palabra "conexión" en SignalR.

<a id="signalrvstransport"></a>

### <a name="signalr-connections-transport-connections-and-physical-connections"></a>Conexiones SignalR, las conexiones de transporte y conexiones físicas

En este artículo, se diferenciará entre *conexiones SignalR*, *las conexiones de transporte*, y *conexiones físicas*:

- **Conexión de SignalR** hace referencia a una relación lógica entre un cliente y una dirección URL del servidor, mantenida por la API SignalR y una identificación exclusiva mediante un identificador de conexión. Los datos acerca de esta relación se mantienen por SignalR y se utilizan para establecer una conexión de transporte. Los extremos de la relación y SignalR se deshace de los datos cuando el cliente llama a la `Stop` método o un límite de tiempo de espera se alcanza cuando SignalR está intentando volver a establecer una conexión de transporte perdido.
- **Conexión de transporte** hace referencia a una relación lógica entre un cliente y un servidor, mantenido por uno de los cuatro API de transporte: WebSockets, eventos de servidor envió, marco para siempre o largos de sondeo. SignalR usa la API de transporte para crear una conexión de transporte y la API de transporte depende de la existencia de una conexión de red física para crear la conexión de transporte. La conexión de transporte finaliza cuando termina SignalR o cuando el transporte API detecta que se pierde la conexión física.
- **Conexión física** hace referencia a los vínculos de red físico, cables, las señales inalámbricas, enrutadores, etc., que facilitan la comunicación entre un equipo cliente y un equipo de servidor. La conexión física debe estar presente para establecer una conexión de transporte, y se debe establecer una conexión de transporte con el fin de establecer una conexión de SignalR. Sin embargo, interrumpir la conexión física no siempre finalizar inmediatamente la conexión de transporte o la conexión de SignalR, como se explicará más adelante en este tema.

En el siguiente diagrama, la conexión de SignalR se representa mediante la API de concentradores y la capa de PersistentConnection API SignalR, la conexión de transporte se representa mediante la capa de transporte y la conexión física se representa mediante las líneas entre el servidor y los clientes.

![Diagrama de arquitectura de SignalR](handling-connection-lifetime-events/_static/image1.png)

Cuando se llama a la `Start` método en un cliente de SignalR, se proporciona código de cliente de SignalR con toda la información que necesita para establecer una conexión física a un servidor. Código de cliente de SignalR usa esta información para realizar una solicitud HTTP y establecer una conexión física que usa uno de los métodos de cuatro transporte. Si se produce un error en la conexión de transporte o se produce un error en el servidor, la conexión de SignalR no se puede eliminar inmediatamente porque el cliente todavía tiene la información que necesita para volver a establecer automáticamente una nueva conexión de transporte con la misma dirección URL de SignalR. En este escenario, no está implicada ninguna intervención de la aplicación de usuario y, cuando el código de cliente de SignalR establece una nueva conexión de transporte, no se inicia una nueva conexión de SignalR. La continuidad de la conexión de SignalR se refleja en el hecho de que el identificador de conexión, que se crea cuando se llama a la `Start` método, no cambia.

El `OnReconnected` se ejecuta el controlador de eventos en el concentrador cuando una conexión de transporte se restablece automáticamente después de haberse perdido. El `OnDisconnected` controlador de eventos se ejecuta al final de una conexión de SignalR. Puede terminar una conexión SignalR en cualquiera de las maneras siguientes:

- Si el cliente llama a la `Stop` método, se envía un mensaje de detención en el servidor y cliente y servidor de terminan inmediatamente la conexión de SignalR.
- Después de que se pierde la conectividad entre cliente y servidor, el cliente intenta volver a conectarse y el servidor espera a que el cliente puede volver a conectarse. Si intenta volver a conectarse no tiene éxito y finaliza el período de tiempo de espera de desconexión, cliente y servidor de terminar la conexión de SignalR. El cliente deja de intentar volver a conectar y el servidor se deshace de su representación de la conexión de SignalR.
- Si el cliente deja de ejecutarse sin tener la posibilidad de llamar a la `Stop` método, el servidor espera a que el cliente puede volver a conectar y, a continuación, finaliza la conexión de SignalR después del período de tiempo de espera de desconexión.
- Si se detenga el servidor en ejecución, el cliente intenta volver a conectarse (volver a crea la conexión de transporte) y, a continuación, finaliza la conexión de SignalR después del período de tiempo de espera de desconexión.

Cuando no hay ningún problema de conexión, y la aplicación de usuario finaliza la conexión de SignalR mediante una llamada a la `Stop` método, la conexión de SignalR y la conexión de transporte empezar y finalizar en aproximadamente al mismo tiempo. En las siguientes secciones se describen con más detalle los otros escenarios.

<a id="transportdisconnect"></a>

### <a name="transport-disconnection-scenarios"></a>Escenarios de desconexión del transporte

Conexiones físicas pueden ser lentas o puede haber interrupciones en la conectividad. Función de factores como la longitud de la interrupción, es posible que se pierdan la conexión de transporte. SignalR, a continuación, intenta volver a establecer la conexión de transporte. En ocasiones, la conexión de transporte API detecta la interrupción y quita la conexión de transporte, y SignalR averigua inmediatamente que se pierde la conexión. En otros escenarios, ni la conexión de transporte API ni SignalR advierte inmediatamente que se perdió la conectividad. Para todos los transportes excepto sondeo prolongado, el cliente de SignalR usa una función denominada *keepalive* para comprobar si la pérdida de conectividad que la API de transporte no puede detectar. Para obtener información acerca de conexiones de sondeo largo, consulte [configuración de tiempo de espera y keepalive](#timeoutkeepalive) más adelante en este tema.

Cuando una conexión está inactiva, periódicamente el servidor envía un paquete keepalive al cliente. En la fecha en que se está escribiendo en este artículo, la frecuencia predeterminada es cada 10 segundos. Realizando escuchas para estos paquetes, los clientes pueden saber si hay un problema de conexión. Si no se recibe un paquete de keepalive cuando se esperaba, después de unos minutos el cliente supone que existen problemas de conexión, como lentitud o interrupciones. Si el keepalive todavía no se recibe después de un tiempo, el cliente supone que se ha quitado la conexión y comienza a intentando volver a conectar.

El siguiente diagrama muestra los eventos de cliente y servidor que se producen en un escenario típico cuando hay problemas con la conexión física que no son reconocidos inmediatamente por la API de transporte. El diagrama se aplica a las siguientes circunstancias:

- El transporte es WebSockets, marco indefinidamente o eventos enviados por el servidor.
- Hay distintos períodos de interrupción en la conexión de red física.
- La API de transporte no conocen la existencia de las interrupciones, por lo que SignalR se basa en la funcionalidad de keepalive para detectarlos.

![Desconexiones de transporte](handling-connection-lifetime-events/_static/image2.png)

Si el cliente entra en modo de conectarse de nuevo, pero no puede establecer una conexión de transporte dentro del límite de tiempo de espera de desconexión, el servidor finaliza la conexión de SignalR. Cuando esto ocurre, el servidor ejecuta el concentrador `OnDisconnected` método y las colas de un mensaje de desconexión para enviar al cliente en caso de que el cliente se administra a conectarse más tarde. Si el cliente, a continuación, volver a conectar, recibe el comando de desconexión y llama el `Stop` método. En este escenario, `OnReconnected` no se ejecuta cuando se vuelve a conectar en el cliente, y `OnDisconnected` no se ejecuta cuando el cliente llama a `Stop`. El siguiente diagrama muestra este escenario.

![Interrupciones del transporte - tiempo de espera del servidor](handling-connection-lifetime-events/_static/image3.png)

Los eventos de duración de conexión de SignalR que se pueden producir en el cliente son los siguientes:

- `ConnectionSlow`eventos de cliente.

    Se genera cuando una proporción de valores preestablecida del período de tiempo de espera de keepalive ha transcurrido desde el último mensaje o se haya recibido el ping de keepalive. El período de advertencia de tiempo de espera de keepalive predeterminado es 2/3 del tiempo de espera de keepalive. El tiempo de espera de keepalive es 20 segundos, por lo que la advertencia se produce en torno a 13 segundos.

    De forma predeterminada, el servidor envía pings de keepalive cada 10 segundos y el cliente comprueba los pings de keepalive sobre cada 2 segundos (un tercio de la diferencia entre el valor de tiempo de espera de keepalive y el valor de advertencia de tiempo de espera de keepalive).

    Si se convierte en la API de transporte consciente de realizar una desconexión, SignalR se podría informar de la desconexión antes de que pase el período de advertencia de tiempo de espera de keepalive. En ese caso, el `ConnectionSlow` eventos no se generarán y SignalR debería ir directamente a la `Reconnecting` eventos.
- `Reconnecting`eventos de cliente.

    Se genera cuando (a) el transporte API detecta que se pierde la conexión, o (b) el periodo de tiempo de espera de keepalive ha transcurrido desde el último mensaje o recibió keepalive ping. El código de cliente de SignalR empieza a intentar volver a conectarse. Puede controlar este evento si desea que la aplicación para realizar alguna acción cuando se pierde la conexión de transporte. Actualmente, el período de tiempo de espera de keepalive predeterminado es 20 segundos.

    Si el código de cliente intenta llamar a un método de concentrador mientras SignalR está en modo de conectarse de nuevo, SignalR intentará enviar el comando. La mayoría de los casos, se producirá un error en los intentos de este tipo, pero en algunas circunstancias podría realizarse correctamente. Para los eventos de servidor envió, indefinidamente marco y transportes de sondeo largo, SignalR usa dos canales de comunicación, uno que utiliza el cliente para enviar mensajes y otro que utiliza para recibir mensajes. El canal usado para la recepción es el permanentemente abierta y que es la que se cierra cuando se interrumpe la conexión física. El canal usado para enviar permanece disponible, por lo que si se restaura la conectividad física, una llamada de método de cliente a servidor puede ser satisfactoria antes de que se restablece el canal de recepción. El valor devuelto no se recibirán hasta SignalR abre el canal usado para recibir de nuevo.
- `Reconnected`eventos de cliente.

    Se produce cuando se restablece la conexión de transporte. El `OnReconnected` se ejecuta el controlador de eventos en el concentrador.
- `Closed`eventos de cliente (`disconnected` eventos de JavaScript).

    Se produce cuando expira el periodo de tiempo de espera de desconexión mientras que el código de cliente de SignalR está intentando volver a conectar después de perder la conexión de transporte. Desconectar el valor predeterminado es de 30 segundos de tiempo de espera. (Este evento también se desencadena cuando finaliza la conexión porque el `Stop` se llama al método.)

Las interrupciones de la conexión de transporte que no se detectan mediante la API de transporte y no retrasar la recepción de keepalive pings desde el servidor durante más tiempo del período de advertencia de tiempo de espera de keepalive no podrían hacer que cualquier conexión que se generen los eventos de duración.

Algunos entornos de red deliberadamente cerrar las conexiones inactivas y otra función de los paquetes keepalive es ayudar a evitar esto al permitir que estas redes sabe que una conexión SignalR está en uso. En casos extremos la frecuencia predeterminada de pings de keepalive no puede ser suficiente para evitar conexiones cerradas. En ese caso puede configurar los pings de keepalive para enviarse con más frecuencia. Para obtener más información, consulte [configuración de tiempo de espera y keepalive](#timeoutkeepalive) más adelante en este tema.

> [!NOTE] 
> 
> [!IMPORTANT]
> No se garantiza que la secuencia de eventos que se describen aquí. SignalR realiza todos los intentos para generar eventos de duración de la conexión de una manera predecible según este esquema, pero existen muchas variaciones de eventos de la red y muchas formas en que los marcos de comunicaciones subyacente como transporte API controlarán. Por ejemplo, el `Reconnected` eventos no podrían producirse cuando el cliente vuelve a conectarse, o la `OnConnected` controlador en el servidor puede ejecutar si el intento de establecer una conexión es correcta. Este tema describe únicamente los efectos que normalmente se genera por determinadas circunstancias típicas.


<a id="clientdisconnect"></a>

### <a name="client-disconnection-scenarios"></a>Escenarios de desconexión de cliente

En un explorador del cliente, el código de cliente de SignalR que mantiene una conexión SignalR se ejecuta en el contexto de JavaScript de una página web. Que se motivo por el que la conexión de SignalR tiene que finalizar cuando se desplaza de una página a otra y ese de por qué tiene varias conexiones con conexión de varios identificadores si se conecta desde varias ventanas del explorador o las pestañas. Cuando el usuario cierra una pestaña o ventana del explorador o navega a una página nueva o actualiza la página, la conexión de SignalR inmediatamente finaliza debido a código de cliente de SignalR controla ese evento de explorador para llamadas y el `Stop` método. En estos escenarios, o en cualquier plataforma de cliente cuando la aplicación llama a la `Stop` método, el `OnDisconnected` controlador de eventos se ejecuta inmediatamente en el servidor y el cliente genera el `Closed` evento (el evento se denomina `disconnected` en JavaScript).

Si una aplicación cliente o el equipo donde se está ejecutando en se bloquea o entra en modo de suspensión (por ejemplo, cuando el usuario cierra el equipo portátil), el servidor no se informa acerca de qué ha ocurrido. En lo que el servidor sabe, la pérdida del cliente podría ser debido a la interrupción de la conectividad y el cliente podría estar intentando volver a conectar. Por lo tanto, en estos casos el servidor espera para dar al cliente una oportunidad para volver a conectar, y `OnDisconnected` no se ejecuta hasta que el período de tiempo de espera de desconexión expire (aproximadamente 30 segundos de forma predeterminada). El siguiente diagrama muestra este escenario.

![Error en el equipo cliente](handling-connection-lifetime-events/_static/image4.png)

<a id="serverdisconnect"></a>

### <a name="server-disconnection-scenarios"></a>Escenarios de desconexión del servidor

Cuando un servidor se queda sin conexión, se reinicia, se produce un error, el dominio de aplicación recicla, etc., el resultado puede ser similar a una pérdida de conexión o la API de transporte y SignalR podrían saber inmediatamente que el servidor ya no existe, y debería comenzar SignalR intentando volver a conectar sin Cuando se genera el `ConnectionSlow` eventos. Si el cliente entra en modo de conectarse de nuevo y si el servidor recupera o se reinicia o un nuevo servidor se pone en línea antes de que expire el período de tiempo de espera de desconexión, el cliente volverá a conectarse al servidor nuevo o restaurado. En ese caso, la conexión de SignalR continúa en el cliente y el `Reconnected` evento se desencadena. En el primer servidor, `OnDisconnected` nunca se ejecuta y en el nuevo servidor, `OnReconnected` se ejecuta aunque `OnConnected` nunca se ejecutó para el cliente en ese servidor antes de. (El efecto es el mismo si el cliente se vuelve a conectar al mismo servidor después de un reciclaje del dominio de aplicación o reiniciar el equipo, porque cuando reinicia el servidor no tiene memoria de actividad de conexión anterior). En el diagrama siguiente se da por supuesto que el transporte API pasa a ser consciente de la pérdida de conexión inmediatamente, por lo que la `ConnectionSlow` no se produce el evento.

![Error de servidor y volver a conectarse](handling-connection-lifetime-events/_static/image5.png)

Si un servidor no están disponible dentro del período de tiempo de espera de desconexión, termina la conexión de SignalR. En este escenario, el `Closed` evento (`disconnected` en clientes de JavaScript) se genera en el cliente pero `OnDisconnected` nunca se llama en el servidor. En el diagrama siguiente se da por supuesto que la API de transporte no conocen la existencia de la conexión perdida, por lo que se detecta por SignalR keepalive funcionalidad y la `ConnectionSlow` evento se desencadena.

![Tiempo de espera y error de servidor](handling-connection-lifetime-events/_static/image6.png)

<a id="timeoutkeepalive"></a>

## <a name="timeout-and-keepalive-settings"></a>Configuración de tiempo de espera y keepalive

El valor predeterminado `ConnectionTimeout`, `DisconnectTimeout`, y `KeepAlive` valores son adecuados para la mayoría de los escenarios, pero se puede cambiar si el entorno tiene necesidades especiales. Por ejemplo, si su entorno de red cierra la conexión que está inactivas durante 5 segundos, tendrá que disminuya el valor de keepalive.

<a id="connectiontimeout"></a>

### <a name="connectiontimeout"></a>ConnectionTimeout

Este valor representa la cantidad de tiempo para dejar una conexión de transporte abiertos y espera una respuesta antes de cerrarla y abrir una nueva conexión. El valor predeterminado es 110 segundos.

Esta configuración aplica sólo cuando keepalive la funcionalidad está deshabilitada, que normalmente solo se aplica a la larga transporte de sondeo. El diagrama siguiente ilustra el efecto de esta configuración en un valor largo conexión de transporte de sondeo.

![Conexión de transporte de sondeo largo](handling-connection-lifetime-events/_static/image7.png)

<a id="disconnecttimeout"></a>

### <a name="disconnecttimeout"></a>DisconnectTimeout

Este valor representa la cantidad de tiempo de espera después de una conexión de transporte se pierde antes de generar el `Disconnected` eventos. El valor predeterminado es 30 segundos. Al establecer `DisconnectTimeout`, `KeepAlive` se establece automáticamente en 1/3 de la `DisconnectTimeout` valor.

<a id="keepalive"></a>

### <a name="keepalive"></a>KeepAlive

Este valor representa la cantidad de tiempo de espera antes de enviar un paquete keepalive a través de una conexión inactiva. El valor predeterminado es 10 segundos. Este valor no debe tener más de 1/3 de la `DisconnectTimeout` valor.

Si desea establecer `DisconnectTimeout` y `KeepAlive`, establezca `KeepAlive` después `DisconnectTimeout`. En caso contrario, su `KeepAlive` configuración se sobrescribirá cuando `DisconnectTimeout` establece automáticamente `KeepAlive` a 1/3 del valor de tiempo de espera.

Si desea deshabilitar la funcionalidad de keepalive, establezca `KeepAlive` en null. KeepAlive funcionalidad se deshabilita automáticamente la largos transporte de sondeo.

<a id="changetimeout"></a>

### <a name="how-to-change-timeout-and-keepalive-settings"></a>Cómo cambiar la configuración de tiempo de espera y keepalive

Para cambiar los valores predeterminados para estos valores, establecerlos `Application_Start` en su *Global.asax* de archivos, como se muestra en el ejemplo siguiente. Los valores mostrados en el código de ejemplo son igual a los valores predeterminados.

[!code-csharp[Main](handling-connection-lifetime-events/samples/sample1.cs)]

<a id="notifydisconnect"></a>

## <a name="how-to-notify-the-user-about-disconnections"></a>Cómo notificar al usuario de desconexiones

En algunas aplicaciones puede mostrar un mensaje al usuario cuando hay problemas de conectividad. Tiene varias opciones para cómo y cuándo deben hacerlo. Los ejemplos de código siguientes son para un cliente de JavaScript mediante el proxy generado.

- Controlar la `connectionSlow` evento para mostrar un mensaje en cuanto SignalR es consciente de los problemas de conexión, antes de que entre en modo de conectarse de nuevo.

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample2.js)]
- Controlar la `reconnecting` evento para mostrar un mensaje cuando SignalR es consciente de realizar una desconexión y va de modo de conectarse de nuevo.

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample3.js)]
- Controlar la `disconnected` ha agotado el evento que se muestre un mensaje cuando un intento de reconexión. En este escenario, es la única forma de volver a establecer una conexión con el servidor de nuevo reiniciar la conexión de SignalR mediante una llamada a la `Start` método, lo cual creará un nuevo identificador de conexión. El ejemplo de código siguiente usa una marca para asegurarse de que se emita la notificación solo después de un tiempo de espera de reconexión no tras una finalización normal para la conexión de SignalR producir llamando a la `Stop` método.

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample4.js)]

<a id="continuousreconnect"></a>

## <a name="how-to-continuously-reconnect"></a>Cómo volver a conectar de forma continua

En algunas aplicaciones puede volver a establecer automáticamente una conexión después de que se ha perdido y se ha agotado el intento de reconexión. Para ello, puede llamar a la `Start` método desde su `Closed` controlador de eventos (`disconnected` controlador de eventos en los clientes de JavaScript). Puede esperar un período de tiempo antes de llamar a `Start` con el fin de evitar hacerlo demasiado con frecuencia cuando el servidor o la conexión física no están disponibles. El ejemplo de código siguiente corresponde a un cliente de JavaScript mediante el proxy generado.

[!code-javascript[Main](handling-connection-lifetime-events/samples/sample5.js)]

Un posible problema de tener en cuenta en los clientes móviles es que los intentos de reconexión continua cuando la conexión física o el servidor no está disponible pudieron producir purga de batería innecesarios.

<a id="disconnectclientfromserver"></a>

## <a name="how-to-disconnect-a-client-in-server-code"></a>Cómo desconectar a un cliente en el código de servidor

SignalR versión 1.1.1 no tiene un servidor integrado API para que se desconectan los clientes. Hay [planes para agregar esta funcionalidad en el futuro](https://github.com/SignalR/SignalR/issues/2101). En la versión actual de SignalR, la manera más sencilla para desconectar a un cliente desde el servidor es implementar un método de desconexión en el cliente y llamar a ese método desde el servidor. El ejemplo de código siguiente muestra un método de desconexión de un cliente de JavaScript mediante el proxy generado.

[!code-javascript[Main](handling-connection-lifetime-events/samples/sample6.js)]

> [!WARNING]
> Seguridad: este método para que se desconectan los clientes ni a la API integrada propuesta abordará el escenario de pirateados clientes que ejecutan código malintencionado, ya que podrían volver a conectar los clientes o puede quitar el código pirateado el `stopClient` método o cambio lo que hace. El lugar adecuado para implementar la protección de estado por denegación de servicio (DOS) no está en el marco de trabajo o el nivel de servidor, sino en su lugar en la infraestructura de front-end.
