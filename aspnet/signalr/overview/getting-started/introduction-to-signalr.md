---
uid: signalr/overview/getting-started/introduction-to-signalr
title: "Introducción a SignalR | Documentos de Microsoft"
author: pfletcher
description: "Este artículo describe qué es SignalR y algunas de las soluciones se diseñó para crear."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 0fab5e35-8c1f-43d4-8635-b8aba8766a71
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/introduction-to-signalr
msc.type: authoredcontent
ms.openlocfilehash: 5bb49c9c2405d232ba5e067d99f8879b3bc99361
ms.sourcegitcommit: 53ee14b9c8200f44705d8997c3619fa874192d45
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/08/2018
---
<a name="introduction-to-signalr"></a>Introducción a SignalR
====================
por [Patrick Fletcher](https://github.com/pfletcher)

> Este artículo describe qué es SignalR y algunas de las soluciones se diseñó para crear. 
> 
> ## <a name="questions-and-comments"></a>Preguntas y comentarios
> 
> Vota sobre cómo le gustó este tutorial y lo que podemos mejorar en los comentarios en la parte inferior de la página. Si tiene preguntas que no están directamente relacionados con el tutorial, puede publicar para la [foro de ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](https://stackoverflow.com/questions/tagged/signalr).


## <a name="what-is-signalr"></a>¿Qué es SignalR?

ASP.NET SignalR es una biblioteca para desarrolladores ASP.NET que simplifica el proceso de agregar funcionalidad web en tiempo real a las aplicaciones. La funcionalidad web en tiempo real es la capacidad de tener la inserción de código de servidor contenido en los clientes conectados de forma instantánea cuando se encuentre disponible, en lugar de hacer que el servidor espera para que un cliente solicitar nuevos datos.

SignalR puede utilizarse para agregar a algún tipo de funcionalidad web "en tiempo real" a la aplicación ASP.NET. Mientras chat a menudo se utiliza como ejemplo, puede hacer mucho más. Siempre que un usuario actualiza una página web para ver los nuevos datos o la página implementa [sondeo largo](http://en.wikipedia.org/wiki/Push_technology#Long_polling) para recuperar nuevos datos, es un candidato para el uso de SignalR. Algunos ejemplos son paneles y supervisión de aplicaciones, aplicaciones de colaboración (por ejemplo, la edición simultánea de documentos), las actualizaciones de progreso y trabajo formularios en tiempo real.

SignalR también permite completamente nuevos tipos de aplicaciones web que requieren actualizaciones de alta frecuencia desde el servidor, por ejemplo, los juegos en tiempo real.

SignalR proporciona una API sencilla para crear llamadas a procedimiento remoto de servidor a cliente (RPC) que llaman a funciones de JavaScript de cliente exploradores (y otras plataformas de cliente) desde el código de .NET en el servidor de. SignalR también incluye API para la administración de conexión (por ejemplo, la conexión y eventos de desconexión) y agrupación de conexiones.

![Invocación de métodos con SignalR](introduction-to-signalr/_static/image1.png)

SignalR controla automáticamente la administración de conexiones y permite que los mensajes de difusión a todos los clientes conectados al mismo tiempo, como un salón de chat. También puede enviar mensajes a clientes específicos. La conexión entre el cliente y el servidor es persistente, a diferencia de una conexión HTTP clásica, que se vuelve a establecer para todas las comunicaciones.

SignalR admite la funcionalidad de "inserción de servidor", en el que el código de servidor puede llamar a código de cliente en el explorador mediante llamadas a procedimiento remoto (RPC), en lugar de con el modelo de solicitud-respuesta común en la web de hoy en día.

Aplicaciones SignalR pueden escalarse a miles de clientes que utilizan Service Bus, SQL Server o [Redis](http://redis.io).

SignalR es código abierto, accesible a través de [GitHub](https://github.com/signalr).

## <a name="signalr-and-websocket"></a>SignalR y WebSocket

SignalR usa el transporte de WebSocket de nuevo cuando sea posible y vuelve a los transportes anteriores cuando sea necesario. Mientras que por supuesto, podría escribir la aplicación con WebSocket directamente, utilizando SignalR decir mucho de la funcionalidad adicional que se debe implementar ya se habrán realizado por usted. Lo más importante, esto significa que se puede codificar la aplicación para aprovechar las ventajas de WebSocket sin tener que preocuparse sobre cómo crear una ruta de acceso de código independiente para los clientes más antiguos. SignalR también mantiene a salvo de tener que preocuparse sobre las actualizaciones de WebSocket, ya que seguirá SignalR se actualicen para admitir los cambios en el transporte subyacente, la aplicación se proporciona una interfaz coherente entre todas las versiones de WebSocket.

Mientras que por supuesto, puede crear una solución mediante WebSocket por sí sola, SignalR proporciona toda la funcionalidad que tendría que escribir usted mismo, por ejemplo, para otros transportes y revisión de la aplicación para las actualizaciones a las implementaciones de WebSocket.

<a id="transports"></a>

## <a name="transports-and-fallbacks"></a>Transportes y reservas

SignalR es una abstracción sobre algunos de los transportes que son necesarios para realizar el trabajo en tiempo real entre cliente y servidor. Una conexión de SignalR se inicia como HTTP y, a continuación, se promueve a una conexión de WebSocket si está disponible. WebSocket es el transporte ideal para SignalR, ya que hace un uso más eficaz de memoria del servidor, tiene la latencia más baja y tiene las características más subyacentes (por ejemplo, la comunicación dúplex completa entre cliente y servidor), pero también tiene los más exigentes requisitos: WebSocket requiere que el servidor a usar Windows Server 2012 o Windows 8 y .NET Framework 4.5. Si no se cumplen estos requisitos, SignalR intentará usar otros transportes para realizar las conexiones.

### <a name="html-5-transports"></a>Transportes de HTML 5

Estos transportes dependen de la compatibilidad con [HTML 5](http://en.wikipedia.org/wiki/HTML5). Si el explorador del cliente no es compatible con el estándar HTML 5, se usará los transportes anteriores.

- **WebSocket** (si el el servidor y el explorador indican que pueden admitir Websocket). WebSocket es el único tipo de transporte que establece una conexión bidireccional, persistente true entre cliente y servidor. Sin embargo, WebSocket también tiene los requisitos más exigentes; es totalmente compatible sólo en las versiones más recientes de Microsoft Internet Explorer y Google Chrome, Mozilla Firefox y solo tiene una implementación parcial en otros exploradores como Opera y Safari.
- **Eventos enviados del servidor**, también conocido como EventSource (si el explorador admite eventos enviados del servidor, que es prácticamente todos los exploradores excepto Internet Explorer.)

### <a name="comet-transports"></a>Transportes de cometa

Los transportes siguientes se basan en el [cometa](http://en.wikipedia.org/wiki/Comet_(programming)) modelo de aplicación web, en el que un explorador o en otro cliente mantiene una solicitud HTTP mantiene larga que el servidor puede utilizar para insertar datos en el cliente sin el cliente concreto solicita que.

- **Marco para siempre** (sólo para Internet Explorer). Indefinidamente marco crea un IFrame oculto que realiza una solicitud a un punto de conexión en el servidor que no se completa. A continuación, el servidor envía continuamente script al cliente que se ejecuta inmediatamente, lo que proporciona una conexión unidireccional en tiempo real del servidor al cliente. La conexión de cliente a servidor utiliza una conexión independiente desde el servidor con conexión de cliente y, al igual que una solicitud HTTP estándar, se crea una nueva conexión para cada parte de los datos que deben enviarse.
- **Tiempo de sondeo de AJAX**. Tiempo de sondeo no crea una conexión persistente, pero en su lugar sondea el servidor con una solicitud que permanece abierta hasta que el servidor responde, momento en que la conexión se cierra y se solicita una conexión nueva inmediatamente. Esto podría presentar cierta latencia mientras se restablece la conexión.

Para obtener más información sobre cuáles son los transportes admitidos en las configuraciones, vea [Supported Platforms](supported-platforms.md).

### <a name="transport-selection-process"></a>Proceso de selección de transporte

En la lista siguiente se muestra los pasos que SignalR que se usa para decidir qué transporte debe para utilizar.

1. Si el explorador es Internet Explorer 8 o versiones anteriores, se usa el sondeo largo.
2. Si se configura JSONP (es decir, el `jsonp` parámetro está establecido en `true` cuando se inicia la conexión), de sondeo largo se utiliza.
3. Si se está realizando una conexión entre dominios (es decir, si el punto de conexión de SignalR no está en el mismo dominio que la página de hospedaje), se usará WebSocket si se cumplen los criterios siguientes:

    - El cliente admite CORS (uso compartido recursos entre orígenes). Para obtener más información sobre los clientes compatibles con CORS, consulte [CORS en caniuse.com](http://www.caniuse.com/CORS).
    - El cliente admite WebSocket
    - El servidor admite WebSocket

    Si no se cumple cualquiera de estos criterios, se usará el sondeo largo. Para obtener más información sobre las conexiones entre dominios, consulte [cómo establecer una conexión entre dominios](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain).
4. Si no se configura JSONP y la conexión no está entre dominios, se usará WebSocket si el cliente y el servidor lo admiten.
5. Si el cliente o el servidor no admite WebSocket, los eventos enviados del servidor se utiliza si está disponible.
6. Si los eventos enviados del servidor no está disponible, se intenta marco indefinidamente.
7. Si se produce un error en el marco para siempre, se utiliza el sondeo largo.

<a id="MonitoringTransports"></a>
### <a name="monitoring-transports"></a>Transportes de supervisión

Puede determinar el transporte que se está usando la aplicación habilitando el registro en el centro y abrir la ventana de consola en el explorador.

Para habilitar el registro de eventos de la base de datos central en un explorador, agregue el siguiente comando para la aplicación cliente:

`$.connection.hub.logging = true;`

- En Internet Explorer, abra las herramientas de desarrollo presionando F12 y haga clic en la pestaña de la consola.

    ![Consola de Microsoft Internet Explorer](introduction-to-signalr/_static/image2.png)
- En Chrome, abra la consola, presione Ctrl + Mayús + J.

    ![Consola de Google Chrome](introduction-to-signalr/_static/image3.png)

Con la consola abierta y habilitado el registro, podrá ver qué transporte se usa signalr.

![En Internet Explorer que muestra el transporte de WebSocket de la consola](introduction-to-signalr/_static/image4.png)

### <a name="specifying-a-transport"></a>Especificar un transporte

Negociar un transporte toma una cierta cantidad de tiempo y cliente/servidor recursos. Si se conocen las capacidades de cliente, se puede especificar un transporte, cuando se inicia la conexión de cliente. El fragmento de código siguiente muestra cómo iniciar una conexión mediante el transporte de sondeo largo Ajax, que se usaría si se sabe que el cliente no era compatible con cualquier otro protocolo:

`connection.start({ transport: 'longPolling' });`

Puede especificar un orden de reserva si desea que un cliente intente transportes específicos en orden. El siguiente fragmento de código muestra cómo tratar de WebSocket y si no es posible, vaya directamente al sondeo largo.

`connection.start({ transport: ['webSockets','longPolling'] });`

Las constantes de cadena para especificar transportes se definen como sigue:

- `webSockets`
- `foreverFrame`
- `serverSentEvents`
- `longPolling`

## <a name="connections-and-hubs"></a>Las conexiones y concentradores

La API SignalR contiene dos modelos para la comunicación entre clientes y servidores: conexiones persistentes y concentradores.

Una conexión representa un punto de conexión simple para el envío de mensajes único destinatario, agrupada o difusión. La API de conexión persistente (representada en el código de .NET mediante la clase PersistentConnection) da como resultado el programador acceso directo al protocolo de comunicaciones de nivel inferior que expone SignalR. Con el modelo de comunicación conexiones le resultarán familiar a los desarrolladores que han usado las API basadas en conexión, como Windows Communication Foundation.

Un concentrador es una canalización de más alto nivel generada a partir de la API de conexión que permite que el cliente y servidor para llamar a métodos entre sí directamente. SignalR controla la distribución a través de los límites del equipo como si por arte de magia, permitiendo a los clientes llamar a métodos en el servidor como fácilmente como métodos locales y viceversa. Utilizando el modelo de comunicación de los centros le resultarán familiar a los desarrolladores que han usado la invocación remota de las API como .NET Remoting. Usar un concentrador también podrá pasar parámetros fuertemente tipados a métodos, lo que permite al enlace de modelos.

### <a name="architecture-diagram"></a>diagrama de arquitectura

El siguiente diagrama muestra la relación entre los centros, las conexiones persistentes y las tecnologías subyacentes utilizadas para los transportes.

![Diagrama de arquitectura de SignalR que muestra las API, transportes y los clientes](introduction-to-signalr/_static/image5.png)

### <a name="how-hubs-work"></a>Cómo funcionan los concentradores

Cuando el código del lado servidor llama a un método en el cliente, se enviará un paquete a través del transporte activo que contiene el nombre y los parámetros del método que se llame a (cuando se envía un objeto como un parámetro de método, se serializa mediante JSON). El cliente, a continuación, coincide con el nombre del método a métodos definidos en código de cliente. Si hay una coincidencia, el método de cliente se ejecutará con los datos del parámetro deserializado.

La llamada al método se puede supervisar con herramientas como [Fiddler.](http://fiddler2.com/) La siguiente imagen muestra una llamada de método enviada desde un servidor de SignalR a un cliente de explorador web en el panel de registros de Fiddler. Se envía la llamada al método de un concentrador llama `MoveShapeHub`, y se llama al método que se va a invocar `updateShape`.

![Vista de registro de Fiddler que muestra el tráfico de SignalR](introduction-to-signalr/_static/image6.png)

En este ejemplo, el nombre del concentrador se identifica con la `H` parámetro; el método nombre se identifica con la `M` parámetro y los datos que se envían al método se identifica con la `A` parámetro. La aplicación que generó este mensaje se crea en el [en tiempo real de alta frecuencia](tutorial-high-frequency-realtime-with-signalr.md) tutorial.

### <a name="choosing-a-communication-model"></a>Elegir un modelo de comunicación

Mayoría de las aplicaciones debe utilizar la API de concentradores. La API de conexiones podría usarse en las siguientes circunstancias:

- El formato de las necesidades reales mensaje enviado que se especifique.
- El programador prefiere trabajar con un modelo de mensajería y distribución en lugar de un modelo de invocación remota.
- Una aplicación existente que utiliza un modelo de mensajería que se va a se convierten para utilizar SignalR.
