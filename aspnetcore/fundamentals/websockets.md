---
title: Compatibilidad con WebSockets en ASP.NET Core
author: tdykstra
description: "Obtenga información sobre cómo empezar a usar WebSockets en ASP.NET Core."
manager: wpickett
ms.author: tdykstra
ms.date: 03/25/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/websockets
ms.openlocfilehash: 306eca28b9f1f66e1ccaf185ccae87db8dea1b01
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-websockets-in-aspnet-core"></a>Introducción a WebSockets en ASP.NET Core

Por [Tom Dykstra](https://github.com/tdykstra) y [Andrew Stanton-Nurse](https://github.com/anurse)

En este artículo se ofrece una introducción a WebSockets en ASP.NET Core. [WebSocket](https://wikipedia.org/wiki/WebSocket) es un protocolo que habilita canales de comunicación bidireccional persistentes a través de conexiones TCP. Se usa para aplicaciones como: chat, tableros de cotizaciones, juegos y cualquier otra aplicación donde se necesiten funciones en tiempo real en una aplicación web.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample)). Para más información, vea la sección [Pasos siguientes](#next-steps).


## <a name="prerequisites"></a>Requisitos previos

* ASP.NET Core 1.1 (no se ejecuta en 1.0)
* Cualquier sistema operativo en el que se pueda ejecutar ASP.NET Core:
  
  * Windows 7/Windows Server 2008 y versiones posteriores
  * Linux
  * macOS

* **Excepción**: si la aplicación se ejecuta en Windows con IIS o con WebListener, debe usar:

  * Windows 8/Windows Server 2012 o versiones posteriores
  * IIS 8/Express IIS 8
  * WebSocket debe estar habilitado en IIS

* Para conocer los exploradores admitidos, vea http://caniuse.com/#feat=websockets.

## <a name="when-to-use-it"></a>Cuándo usarlo

Use WebSockets cuando necesite trabajar directamente con una conexión de socket. Por ejemplo, suponga que necesita lograr el mejor rendimiento posible para un juego en tiempo real.

[ASP.NET SignalR](https://docs.microsoft.com/aspnet/signalr/overview/getting-started/introduction-to-signalr) proporciona un modelo de aplicación más completo para funciones en tiempo real, pero solo se ejecuta en ASP.NET, no en ASP.NET Core. Una versión Core de SignalR está en desarrollo; para seguir su progreso, vea el [repositorio de GitHub para SignalR Core](https://github.com/aspnet/SignalR).

Si no quiere esperar al lanzamiento de SignalR Core, puede usar WebSockets directamente. Pero es posible que tenga que desarrollar características que podría obtener en SignalR, como:

* Compatibilidad con una gama más amplia de versiones del explorador, ya que usa la reserva automática para los métodos de transporte alternativos.
* Reconexión automática cuando se produce un fallo de conexión.
* Compatibilidad con clientes que llaman a métodos en el servidor o viceversa.
* Compatibilidad con el escalado a varios servidores.

## <a name="how-to-use-it"></a>Cómo se usa

* Instale el paquete [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/).
* Configure el middleware.
* Acepte las solicitudes WebSocket.
* Envíe y reciba mensajes.

### <a name="configure-the-middleware"></a>Configurar el middleware

Agregue el middleware de WebSockets al método `Configure` de la clase `Startup`.

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSockets)]

Se pueden configurar estas opciones:

* `KeepAliveInterval`: la frecuencia con que se envían marcos "ping" al cliente, para asegurarse de que los servidores proxy mantienen abierta la conexión.
* `ReceiveBufferSize`: el tamaño del búfer usado para recibir datos. Solo los usuarios avanzados tendrán que cambiar estas opciones para ajustar el rendimiento según el tamaño de los datos.

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSocketsOptions)]

### <a name="accept-websocket-requests"></a>Aceptar solicitudes WebSocket

En algún momento posterior en el ciclo de solicitudes (más adelante en el método `Configure` o en una acción de MVC, por ejemplo) debe comprobar si se trata de una solicitud WebSocket y aceptarla.

Este ejemplo se muestra más adelante en el método `Configure`.

[!code-csharp[](websockets/sample/Startup.cs?name=AcceptWebSocket&highlight=7)]

Una solicitud WebSocket puede proceder de cualquier dirección URL, pero este código de ejemplo solo acepta solicitudes de `/ws`.

### <a name="send-and-receive-messages"></a>Enviar y recibir mensajes

El método `AcceptWebSocketAsync` actualiza la conexión TCP a una conexión WebSocket y ofrece un objeto [WebSocket](https://docs.microsoft.com/dotnet/core/api/system.net.websockets.websocket). Use el objeto WebSocket para enviar y recibir mensajes.

El código antes mostrado que acepta la solicitud WebSocket pasa el objeto `WebSocket` a un método `Echo`; aquí se trata del método `Echo`. El código recibe un mensaje y devuelve inmediatamente el mismo mensaje. Se mantiene en un bucle realizando estas acciones hasta que el cliente cierra la conexión. 

[!code-csharp[](websockets/sample/Startup.cs?name=Echo)]

Cuando se acepta el WebSocket antes de empezar este bucle, se finaliza la canalización de middleware.  Tras cerrar el socket, se desenreda la canalización. Es decir, cuando se acepta un WebSocket, la solicitud deja de avanzar por la canalización, del mismo modo que cuando se alcanza una acción de MVC, por ejemplo.  Pero cuando se termina este bucle y se cierra el socket, la solicitud vuelve a recorrer la canalización.

## <a name="next-steps"></a>Pasos siguientes

La [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) que acompaña a este artículo es una aplicación de eco sencilla. Tiene una página web que realiza conexiones WebSocket y el servidor simplemente reenvía al cliente todos los mensajes que recibe. Ejecútela desde un símbolo del sistema (no está configurada para ejecutarse desde Visual Studio con IIS Express) y navegue a http://localhost:5000. La página web muestra el estado de conexión en la parte superior izquierda:

![Estado inicial de la página web](websockets/_static/start.png)

Seleccione **Connect** (Conectar) para enviar una solicitud WebSocket para la URL mostrada.  Escriba un mensaje de prueba y seleccione **Send** (Enviar). Cuando haya terminado, seleccione **Close Socket** (Cerrar socket). Los informes de la sección **Communication Log** (Registro de comunicación) informan de cada acción de abrir, enviar y cerrar a medida que se producen.

![Estado inicial de la página web](websockets/_static/end.png)
