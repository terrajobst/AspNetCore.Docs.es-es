---
uid: signalr/overview/getting-started/introduction-to-signalr
title: Introducción a SignalR | Microsoft Docs
author: pfletcher
description: Este artículo describe qué es SignalR y algunas de las soluciones que se diseñó para crear.
ms.author: riande
ms.date: 06/10/2014
ms.assetid: 0fab5e35-8c1f-43d4-8635-b8aba8766a71
msc.legacyurl: /signalr/overview/getting-started/introduction-to-signalr
msc.type: authoredcontent
ms.openlocfilehash: 0b7e223b6b793d1860797157be6021ffb7f1bc12
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090298"
---
<a name="introduction-to-signalr"></a>Introducción a SignalR
====================

Consulte [Introducción a ASP.NET Core SignalR](/aspnet/core/signalr/introduction) una versión actualizada de este tutorial que usa la versión más reciente de Visual Studio. Usa el nuevo tutorial [ASP.NET Core](/aspnet/core/), que proporciona muchas mejoras con respecto a este tutorial.

por [Patrick Fletcher](https://github.com/pfletcher)

> Este artículo describe qué es SignalR y algunas de las soluciones que se diseñó para crear. 
> 
> ## <a name="questions-and-comments"></a>Preguntas y comentarios
> 
> Deje comentarios sobre cómo le gustó de este tutorial y que podíamos mejorar en los comentarios en la parte inferior de la página. Si tiene preguntas que no están directamente relacionados con el tutorial, puede publicarlos en el [foro de ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](https://stackoverflow.com/questions/tagged/signalr).


## <a name="what-is-signalr"></a>¿Qué es SignalR?

ASP.NET SignalR es una biblioteca para desarrolladores de ASP.NET que simplifica el proceso de agregar funcionalidad web en tiempo real a las aplicaciones. La funcionalidad web en tiempo real es la capacidad de tener la inserción de código de servidor contenido en los clientes conectados al instante en cuanto están disponible, en lugar de tener el servidor espere a que un cliente pida nuevos datos.

SignalR se puede usar para agregar a algún tipo de funcionalidad web "en tiempo real" a la aplicación ASP.NET. Mientras chat a menudo se usa como ejemplo, puede hacer mucho más. Cada vez que un usuario actualiza una página web para ver los nuevos datos o la página implementa [sondeo largo](http://en.wikipedia.org/wiki/Push_technology#Long_polling) para recuperar los datos nuevos, es un candidato para el uso de SignalR. Algunos ejemplos son los paneles y supervisión de aplicaciones, aplicaciones de colaboración (por ejemplo, la edición simultánea de documentos), de trabajo actualizaciones de progreso y los formularios en tiempo real.

SignalR también permite completamente nuevos tipos de aplicaciones web que requieren actualizaciones de alta frecuencia desde el servidor, por ejemplo, los juegos en tiempo real.

SignalR proporciona una API sencilla para crear llamadas a procedimiento remoto de servidor a cliente (RPC) que llaman a funciones de JavaScript en el cliente los exploradores (y otras plataformas de cliente) desde el código de .NET del lado servidor. SignalR también incluye API para la administración de conexión (por ejemplo, conectar y desconectar eventos) y la agrupación de conexiones.

![Invocar métodos con SignalR](introduction-to-signalr/_static/image1.png)

SignalR controla automáticamente la administración de conexiones y permite que los mensajes de difusión a todos los clientes conectados al mismo tiempo, como un salón de chat. También puede enviar mensajes a clientes específicos. La conexión entre el cliente y el servidor es persistente, a diferencia de una conexión HTTP clásica, que se vuelve a establecer para todas las comunicaciones.

SignalR admite la funcionalidad de "inserción", en el que el código de servidor puede llamar a código de cliente en el explorador mediante llamadas a procedimiento remoto (RPC), en lugar de con el modelo de solicitud y respuesta comunes en la web hoy mismo.

Las aplicaciones de SignalR pueden escalarse a miles de clientes que utilizan Service Bus, SQL Server o [Redis](http://redis.io).

SignalR es código abierto, accesible a través de [GitHub](https://github.com/signalr).

## <a name="signalr-and-websocket"></a>SignalR y WebSocket

SignalR usa el transporte de WebSocket de nuevo cuando sea posible y recurre a los transportes anteriores cuando sea necesario. Aunque ciertamente podría escribir la aplicación con WebSocket directamente, utilizando SignalR significa que ya se ha hecho mucho de la funcionalidad adicional que necesita para implementar automáticamente. Lo más importante, esto significa que puede codificar la aplicación para aprovechar las ventajas de WebSocket sin tener que preocuparse sobre la creación de una ruta de acceso de código independiente para los clientes más antiguos. SignalR también intercepta el tener que preocuparse de las actualizaciones de WebSocket, como SignalR está actualizado para admitir los cambios en el transporte subyacente, que la aplicación proporciona una interfaz coherente entre las versiones de WebSocket.

<a id="transports"></a>

## <a name="transports-and-fallbacks"></a>Los transportes y reservas

SignalR es una abstracción sobre algunos de los transportes que son necesarios para realizar el trabajo en tiempo real entre cliente y servidor. Una conexión SignalR se inicia como HTTP y, a continuación, se promueve a una conexión WebSocket si está disponible. WebSocket es el transporte ideal para SignalR, ya que hace que el uso de memoria de servidor más eficaz, tiene la menor latencia y tiene las características más subyacentes (por ejemplo, la comunicación dúplex completa entre cliente y servidor), pero también tiene los más exigentes requisitos: WebSocket requiere que el servidor a usar .NET Framework 4.5 o Windows 8 y Windows Server 2012. Si no se cumplen estos requisitos, SignalR intentará usar otros transportes para realizar las conexiones.

### <a name="html-5-transports"></a>Los transportes de HTML 5

Estos transportes dependen de la compatibilidad con [HTML 5](http://en.wikipedia.org/wiki/HTML5). Si el explorador del cliente no es compatible con el estándar HTML 5, se usará los transportes anteriores.

- **WebSocket** (si la el servidor y el explorador indican que pueden admitir Websocket). WebSocket es el único tipo de transporte que establece una conexión bidireccional, persistente true entre cliente y servidor. Sin embargo, WebSocket tiene también los requisitos más estrictos; es totalmente compatible solo en las versiones más recientes de Microsoft Internet Explorer, Google Chrome y Mozilla Firefox y solo tiene una implementación parcial en otros exploradores como Opera y Safari.
- **Eventos de servidor envían**, también conocido como EventSource (si el explorador admite eventos enviados del servidor, que es básicamente todos los exploradores, excepto Internet Explorer).

### <a name="comet-transports"></a>Transportes Comet

Los siguientes transportes se basan en el [Comet](http://en.wikipedia.org/wiki/Comet_(programming)) modelo de aplicación web, en el que un explorador o en otro cliente mantiene una solicitud HTTP mantiene a la larga, que el servidor puede usar para insertar datos en el cliente sin el cliente en concreto solicitarlo.

- **Marco para siempre** (sólo para Internet Explorer). Siempre marco crea un IFrame oculto que realiza una solicitud a un punto de conexión en el servidor que no se completa. A continuación, el servidor envía continuamente script al cliente que se ejecuta inmediatamente, que proporciona una conexión unidireccional en tiempo real del servidor al cliente. La conexión de cliente a servidor utiliza una conexión independiente desde el servidor con conexión de cliente y, al igual que una solicitud HTTP estándar, se crea una nueva conexión para cada fragmento de datos que deben enviarse.
- **AJAX de sondeo prolongado**. Sondeo largo no crea una conexión persistente, pero en su lugar, sondea el servidor con una solicitud que permanece abierta hasta que el servidor responde, momento en que la conexión se cierra y se solicita una nueva conexión de inmediato. Esto puede ocasionar alguna latencia mientras se restablece la conexión.

Para obtener más información sobre cuáles son los transportes admitidos en qué configuraciones, vea [plataformas admitidas](supported-platforms.md).

### <a name="transport-selection-process"></a>Proceso de selección de transporte

La siguiente lista muestra los pasos que usa SignalR para decidir qué transporte para usar.

1. Si el explorador es Internet Explorer 8 o versiones anteriores, se usa el sondeo largo.
2. Si JSONP está configurado (es decir, el `jsonp` parámetro está establecido en `true` cuando se inicia la conexión), se usa el sondeo largo.
3. Si se realiza una conexión entre dominios (es decir, si el punto de conexión SignalR no está en el mismo dominio que la página de hospedaje), WebSocket se usará si se cumplen los criterios siguientes:

   - El cliente admite la CORS (uso compartido recursos entre orígenes). Para obtener más información sobre los clientes compatibles con la CORS, vea [CORS en caniuse.com](http://www.caniuse.com/CORS).
   - El cliente es compatible con WebSocket
   - El servidor es compatible con WebSocket

     Si no se cumplen los criterios siguientes, se usará el sondeo largo. Para obtener más información sobre las conexiones entre dominios, vea [cómo establecer una conexión entre dominios](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain).
4. Si no está configurado JSONP y la conexión no está entre dominios, se usará el WebSocket si el cliente y el servidor lo admiten.
5. Si el cliente o el servidor no es compatible con WebSocket, envía eventos de servidor se utiliza si está disponible.
6. Si envía eventos del servidor no está disponible, se intenta marco para siempre.
7. Si se produce un error en el marco para siempre, se usa el sondeo largo.

<a id="MonitoringTransports"></a>
### <a name="monitoring-transports"></a>Transportes de supervisión

Puede determinar el transporte que se está usando la aplicación habilitando el registro en el centro y abre la ventana de consola en el explorador.

Para habilitar el registro de eventos de su centro en un explorador, agregue el siguiente comando para la aplicación cliente:

`$.connection.hub.logging = true;`

- En Internet Explorer, abra las herramientas de desarrollo presionando F12 y haga clic en la pestaña de la consola.

    ![Consola de Microsoft Internet Explorer](introduction-to-signalr/_static/image2.png)
- En Chrome, abra la consola, presione Ctrl + Mayús + J.

    ![Consola de Google Chrome](introduction-to-signalr/_static/image3.png)

Con la consola abierta y habilitado el registro, podrá ver el tipo de transporte se usa signalr.

![En Internet Explorer que muestra el transporte de WebSocket de la consola](introduction-to-signalr/_static/image4.png)

### <a name="specifying-a-transport"></a>Especificar un transporte

Negociar un transporte toma una cierta cantidad de tiempo y cliente/servidor de recursos. Si se conocen las capacidades de cliente, se puede especificar un transporte, cuando se inicia la conexión de cliente. El fragmento de código siguiente muestra cómo iniciar una conexión mediante el transporte de sondeo largo Ajax, tal como se usaría si se sabe que el cliente no eran compatibles con cualquier otro protocolo:

`connection.start({ transport: 'longPolling' });`

Puede especificar un pedido de reserva si desea que un cliente para intentarlo transportes específicos en orden. El fragmento de código siguiente muestra cómo intentar WebSocket y si no es posible, ir directamente al sondeo largo.

`connection.start({ transport: ['webSockets','longPolling'] });`

Las constantes de cadena para especificar los transportes se definen como sigue:

- `webSockets`
- `foreverFrame`
- `serverSentEvents`
- `longPolling`

## <a name="connections-and-hubs"></a>Las conexiones y concentradores

La API SignalR contiene dos modelos para la comunicación entre clientes y servidores: concentradores y conexiones persistentes.

Una conexión representa un punto de conexión simple para el envío de mensajes único destinatario, agrupado o difusión. La API de conexión persistente (representado en el código de .NET mediante la clase PersistentConnection) da como resultado el programador acceso directo al protocolo de comunicaciones de nivel inferior que expone SignalR. Mediante el modelo de comunicación de las conexiones le resultarán familiar a los desarrolladores que han usado las API basadas en conexión, como Windows Communication Foundation.

Un concentrador es una canalización de más alto nivel desarrollada sobre la API de conexión que permite que el cliente y servidor llamar a métodos entre sí directamente. SignalR controla el envío a través de los límites del equipo, como por arte de magia, permitir que los clientes llamar a métodos en el servidor como fácilmente como métodos locales y viceversa. Mediante el modelo de comunicación Hubs le resultarán familiar a los desarrolladores que han usado las API, como .NET Remoting de invocación remota. Mediante un centro también le permite pasar parámetros fuertemente tipados a métodos, que habilitan al enlace de modelos.

### <a name="architecture-diagram"></a>Diagrama de arquitectura

El siguiente diagrama muestra la relación entre los concentradores, las conexiones persistentes y las tecnologías subyacentes utilizadas para los transportes.

![Diagrama de arquitectura de SignalR que muestra las API, transportes y los clientes](introduction-to-signalr/_static/image5.png)

### <a name="how-hubs-work"></a>Funcionan de concentradores

Cuando el código del lado servidor llama a un método en el cliente, se enviará un paquete a través del transporte activo que contiene el nombre y los parámetros del método que se llamará (cuando se envía un objeto como un parámetro de método, se serializa mediante JSON). El cliente, a continuación, coincide con el nombre del método a los métodos definidos en código de cliente. Si hay una coincidencia, el método de cliente se ejecutará con los datos deserializados.

La llamada al método se puede supervisar mediante herramientas como [Fiddler.](http://fiddler2.com/) La siguiente imagen muestra una llamada de método enviada desde un servidor de SignalR a un cliente de explorador web en el panel de registros de Fiddler. La llamada al método es que se envían desde un concentrador denominado `MoveShapeHub`, y se llama al método que se invoca `updateShape`.

![Vista de registro de Fiddler que muestra el tráfico de SignalR](introduction-to-signalr/_static/image6.png)

En este ejemplo, el nombre del centro se identifica con el `H` parámetro; el método nombre se identifica con el `M` parámetro y los datos que se envían al método se identifica con el `A` parámetro. La aplicación que generó este mensaje se crea en el [en tiempo real de alta frecuencia](tutorial-high-frequency-realtime-with-signalr.md) tutorial.

### <a name="choosing-a-communication-model"></a>Elegir un modelo de comunicación

Mayoría de las aplicaciones debe usar la API de concentradores. La API de conexiones podría usarse en las siguientes circunstancias:

- El formato de las necesidades reales mensaje enviado que se especifique.
- El desarrollador prefiere trabajar con un modelo de mensajería y distribución en lugar de un modelo de invocación remota.
- Una aplicación existente que utiliza un modelo de mensajería se trasladan para usar SignalR.
