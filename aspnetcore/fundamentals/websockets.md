---
title: Compatibilidad con WebSockets en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo empezar a usar WebSockets en ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/28/2018
uid: fundamentals/websockets
ms.openlocfilehash: b0f1aeff6c7a5777993459274293ba23f2d9dc12
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2018
ms.locfileid: "50206745"
---
# <a name="websockets-support-in-aspnet-core"></a>Compatibilidad con WebSockets en ASP.NET Core

Por [Tom Dykstra](https://github.com/tdykstra) y [Andrew Stanton-Nurse](https://github.com/anurse)

En este artículo se ofrece una introducción a WebSockets en ASP.NET Core. [WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) es un protocolo que habilita canales de comunicación bidireccional persistentes a través de conexiones TCP. Se usa en aplicaciones que sacan partido de comunicaciones rápidas y en tiempo real, como las aplicaciones de chat, panel y juegos.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample)). Para más información, vea la sección [Pasos siguientes](#next-steps).

## <a name="prerequisites"></a>Requisitos previos

* ASP.NET Core 1.1 o posterior
* Cualquier sistema operativo que admita ASP.NET Core:
  
  * Windows 7/Windows Server 2008 o posterior
  * Linux
  * macOS
  
* Si la aplicación se ejecuta en Windows con IIS:

  * Windows 8/Windows Server 2012 o versiones posteriores
  * IIS 8/Express IIS 8
  * WebSockets debe estar habilitado (consulte la sección [Compatibilidad con IIS/IIS Express](#iisiis-express-support)).
  
* Si la aplicación se ejecuta en [HTTP.sys](xref:fundamentals/servers/httpsys):

  * Windows 8/Windows Server 2012 o versiones posteriores

* Para saber qué exploradores son compatibles, vea https://caniuse.com/#feat=websockets.

## <a name="when-to-use-websockets"></a>Cuándo usar WebSockets

Use WebSockets para trabajar directamente con una conexión de socket. Por ejemplo, úselo para lograr el mejor rendimiento posible en un juego en tiempo real.

[SignalR de ASP.NET Core](xref:signalr/introduction) es una biblioteca que simplifica la adición de la funcionalidad web en tiempo real a las aplicaciones. Usa WebSockets siempre que sea posible.

## <a name="how-to-use-websockets"></a>Cómo usar WebSockets

* Instale el paquete [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/).
* Configure el middleware.
* Acepte las solicitudes WebSocket.
* Envíe y reciba mensajes.

### <a name="configure-the-middleware"></a>Configurar el middleware

Agregue el middleware de WebSockets al método `Configure` de la clase `Startup`:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSockets)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=UseWebSockets)]

::: moniker-end

Se pueden configurar estas opciones:

* `KeepAliveInterval`: la frecuencia con que se envían marcos "ping" al cliente, para asegurarse de que los servidores proxy mantienen abierta la conexión.
* `ReceiveBufferSize`: el tamaño del búfer usado para recibir datos. Puede que los usuarios avanzados tengan que cambiar estas opciones para ajustar el rendimiento según el tamaño de los datos.

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSocketsOptions)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=UseWebSocketsOptions)]

::: moniker-end

### <a name="accept-websocket-requests"></a>Aceptar solicitudes WebSocket

En algún momento posterior en el ciclo de solicitudes (más adelante en el método `Configure` o en una acción de MVC, por ejemplo) debe comprobar si se trata de una solicitud WebSocket y aceptarla.

El siguiente ejemplo se corresponde con un momento más adelante en el método `Configure`:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=AcceptWebSocket&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=AcceptWebSocket&highlight=7)]

::: moniker-end

Una solicitud WebSocket puede proceder de cualquier dirección URL, pero este código de ejemplo solo acepta solicitudes de `/ws`.

### <a name="send-and-receive-messages"></a>Enviar y recibir mensajes

El método `AcceptWebSocketAsync` actualiza la conexión TCP a una conexión WebSocket y proporciona un objeto [WebSocket](/dotnet/core/api/system.net.websockets.websocket). Use el objeto `WebSocket` para enviar y recibir mensajes.

El código antes mostrado que acepta la solicitud WebSocket pasa el objeto `WebSocket` a un método `Echo`. El código recibe un mensaje y devuelve inmediatamente el mismo mensaje. Los mensajes se envían y reciben en un bucle hasta que el cliente cierra la conexión:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=Echo)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=Echo)]

::: moniker-end

Cuando la conexión WebSocket se acepta antes de que el bucle comience, la canalización de middleware finaliza. Tras cerrar el socket, se desenreda la canalización. Es decir, la solicitud deja de avanzar en la canalización cuando WebSocket se acepta, pero cuando el bucle termina y el socket se cierra, la solicitud vuelve a recorrer la canalización.

## <a name="iisiis-express-support"></a>Compatibilidad con IIS/IIS Express

El protocolo WebSocket se puede usar en Windows Server 2012 o posterior, y en Windows 8 o posterior con IIS o IIS Express 8 o posterior.

> [!NOTE]
> WebSocket siempre está habilitado cuando se usa IIS Express.

### <a name="enabling-websockets-on-iis"></a>Habilitación de WebSocket en IIS

Para habilitar la compatibilidad con el protocolo WebSocket en Windows Server 2012 o posterior:

> [!NOTE]
> Estos pasos no son necesarios cuando se usa IIS Express

1. Use el asistente **Agregar roles y características** del menú **Administrar** o el vínculo de **Administrador del servidor**.
1. Seleccione **Instalación basada en características o en roles**. Seleccione **Siguiente**.
1. Seleccione el servidor que corresponda (el servidor local está seleccionado de forma predeterminada). Seleccione **Siguiente**.
1. Expanda **Servidor web (IIS)** en el árbol **Roles**, expanda **Servidor web** y, por último, expanda **Desarrollo de aplicaciones**.
1. Seleccione **Protocolo WebSocket**. Seleccione **Siguiente**.
1. Si no necesita más características, haga clic en **Siguiente**.
1. Haga clic en **Instalar**.
1. Cuando la instalación finalice, haga clic en **Cerrar** para salir del asistente.

Para habilitar la compatibilidad con el protocolo WebSocket en Windows Server 8 o posterior:

> [!NOTE]
> Estos pasos no son necesarios cuando se usa IIS Express

1. Vaya a **Panel de control** > **Programas** > **Programas y características** > **Activar o desactivar las características de Windows** (lado izquierdo de la pantalla).
1. Abra los siguientes nodos: **Internet Information Services** > **Servicios World Wide Web** > **Características de desarrollo de aplicaciones**.
1. Seleccione la característica **Protocolo WebSocket**. Seleccione **Aceptar**.

### <a name="disable-websocket-when-using-socketio-on-nodejs"></a>Deshabilitar WebSocket al usar socket.io en Node.js

Si usa la compatibilidad de WebSocket en [socket.io](https://socket.io/) en [Node.js](https://nodejs.org/), deshabilite el módulo IIS WebSocket predeterminado, usando para ello el elemento `webSocket` de *web.config* o de *applicationHost.config*. Si este paso no se lleva a cabo, el módulo IIS WebSocket intenta controlar la comunicación de WebSocket en lugar de Node.js y la aplicación.

```xml
<system.webServer>
  <webSocket enabled="false" />
</system.webServer>
```

## <a name="next-steps"></a>Pasos siguientes

La [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/samples) que acompaña a este artículo es una aplicación de eco. Tiene una página web que realiza las conexiones WebSocket y el servidor reenvía de vuelta al cliente todos los mensajes que reciba. Ejecute la aplicación desde un símbolo del sistema (no está configurada para ejecutarse desde Visual Studio con IIS Express) y vaya a http://localhost:5000. En la página web se muestra el estado de conexión en la parte superior izquierda:

![Estado inicial de la página web](websockets/_static/start.png)

Seleccione **Connect** (Conectar) para enviar una solicitud WebSocket para la URL mostrada. Escriba un mensaje de prueba y seleccione **Send** (Enviar). Cuando haya terminado, seleccione **Close Socket** (Cerrar socket). Los informes de la sección **Communication Log** (Registro de comunicación) informan de cada acción de abrir, enviar y cerrar a medida que se producen.

![Estado inicial de la página web](websockets/_static/end.png)
