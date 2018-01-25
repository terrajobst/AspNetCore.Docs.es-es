---
title: Compatibilidad con WebSockets en ASP.NET Core
author: tdykstra
description: "Obtenga información acerca de cómo empezar a usar WebSockets en ASP.NET Core."
ms.author: tdykstra
manager: wpickett
ms.date: 03/25/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: fundamentals/websockets
ms.openlocfilehash: 6f335376c72cd0c68f4667cf0e661a25bf448980
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
# <a name="introduction-to-websockets-in-aspnet-core"></a>Introducción a WebSockets en el núcleo de ASP.NET

Por [Tom Dykstra](https://github.com/tdykstra) y [Andrew Stanton-enfermera](https://github.com/anurse)

Este artículo explica cómo empezar a usar WebSockets en ASP.NET Core. [WebSocket](https://wikipedia.org/wiki/WebSocket) es un protocolo que habilita canales de comunicación bidireccional persistentes a través de conexiones TCP. Se utiliza para las aplicaciones como chat, tableros de cotizaciones, juegos, en cualquier lugar desea utilizar las funciones en tiempo real en una aplicación web.

[Ver o descargar el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([cómo descargar](xref:tutorials/index#how-to-download-a-sample)). Consulte la [pasos](#next-steps) sección para obtener más información.


## <a name="prerequisites"></a>Requisitos previos

* Núcleo de ASP.NET 1.1 (no se ejecuta en 1.0)
* Cualquier sistema operativo que se ejecuta ASP.NET Core en:
  
  * Windows 7 / Windows Server 2008 y versiones posterior
  * Linux
  * macOS

* **Excepción**: si la aplicación se ejecuta en Windows con IIS, o con WebListener, debe utilizar:

  * Windows 8 / Windows Server 2012 o posterior
  * IIS 8 / Express IIS 8
  * WebSocket debe estar habilitada en IIS

* Para los exploradores admitidos, consulte http://caniuse.com/#feat=websockets.

## <a name="when-to-use-it"></a>Cuándo usarlo

Usar WebSockets cuando necesite trabajar directamente con una conexión de socket. Por ejemplo, quizá necesite el mejor rendimiento posible para un juego en tiempo real.

[ASP.NET SignalR](https://docs.microsoft.com/aspnet/signalr/overview/getting-started/introduction-to-signalr) proporciona una experiencia mejor modelo de aplicación para la funcionalidad en tiempo real, pero sólo se ejecuta en ASP.NET, no ASP.NET Core. Una versión de SignalR Core está en desarrollo; Para seguir su progreso, vea el [repositorio de GitHub para SignalR Core](https://github.com/aspnet/SignalR).

Si no desea esperar SignalR Core, ahora puede usar WebSockets directamente. Pero es posible que deba desarrollar características que se proporciona SignalR, como:

* Compatibilidad con una gama más amplia de versiones del explorador utilizando la reserva automática a los métodos de transporte alternativo.
* Reconexión automática cuando un fallo de conexión.
* Compatibilidad con clientes que llaman a métodos en el servidor o viceversa.
* Soporte técnico para el escalado a varios servidores.

## <a name="how-to-use-it"></a>Cómo se usa

* Instalar el [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) paquete.
* Configurar el middleware.
* Aceptar las solicitudes WebSocket.
* Enviar y recibir mensajes.

### <a name="configure-the-middleware"></a>Configurar el middleware

Agregar el middleware de WebSockets en el `Configure` método de la `Startup` clase.

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSockets)]

Se pueden configurar las siguientes opciones:

* `KeepAliveInterval`-La frecuencia con que enviar tramas de "ping" al cliente, para asegurarse de servidores proxy mantener abierta la conexión.
* `ReceiveBufferSize`-El tamaño del búfer usado para recibir datos. Sólo los usuarios avanzados necesitaría cambiarlo, para la optimización de rendimiento en función del tamaño de sus datos.

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSocketsOptions)]

### <a name="accept-websocket-requests"></a>Acepte las solicitudes WebSocket

En algún lugar más adelante en el ciclo de vida de la solicitud (más adelante en el `Configure` método o en una acción de MVC, por ejemplo) Compruebe si es una solicitud de WebSocket y aceptar la solicitud de WebSocket.

Este ejemplo es de más adelante en el `Configure` método.

[!code-csharp[](websockets/sample/Startup.cs?name=AcceptWebSocket&highlight=7)]

Una solicitud de WebSocket puede proceder cualquier dirección URL, pero este código de ejemplo solo acepta las solicitudes de `/ws`.

### <a name="send-and-receive-messages"></a>Enviar y recibir mensajes

El `AcceptWebSocketAsync` método actualiza la conexión TCP a una conexión de WebSocket y le ofrece una [WebSocket](https://docs.microsoft.com/dotnet/core/api/system.net.websockets.websocket) objeto. Utilizar el objeto de WebSocket para enviar y recibir mensajes.

El código mostrado anteriormente que acepta la solicitud de WebSocket pasa el `WebSocket` el objeto a una `Echo` método; este es el `Echo` (método). El código recibe un mensaje y devuelve inmediatamente el mismo mensaje. Se mantiene en un bucle realice hasta que el cliente cierra la conexión. 

[!code-csharp[](websockets/sample/Startup.cs?name=Echo)]

Cuando se acepta el socket Web antes de comenzar este bucle, se finaliza la canalización de middleware.  Tras cerrar el socket, se desenreda la canalización. Es decir, se detiene el solicitud avanzando en la canalización al aceptar un WebSocket, al igual que lo haría cuando se alcanza una acción de MVC, por ejemplo.  Pero cuando haya terminado este bucle y cierra el socket, la solicitud se realizará copia de seguridad de la canalización.

## <a name="next-steps"></a>Pasos siguientes

El [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) que muestra en este artículo es una aplicación de repetición simple. Tiene una página web que realiza las conexiones de WebSocket y el servidor solo vuelve a enviar al cliente todos los mensajes que recibe. Ejecutar desde un símbolo del sistema (no configuró para ejecutar desde Visual Studio con IIS Express) y vaya a http://localhost: 5000. La página web muestra el estado de conexión en la parte superior izquierda:

![Estado inicial de la página web](websockets/_static/start.png)

Seleccione **conectar** para enviar una solicitud de WebSocket para la dirección URL mostrada.  Escriba un mensaje de prueba y seleccione **enviar**. Cuando hayas terminado, selecciona **Socket cerrar**. El **Log comunicación** sección notifica cada apertura, envío y acción al cerrar tal y como ocurre.

![Estado inicial de la página web](websockets/_static/end.png)
