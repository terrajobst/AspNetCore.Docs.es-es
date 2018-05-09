---
title: Cliente de SignalR JavaScript Core ASP.NET
author: rachelappel
description: Información general de cliente de ASP.NET Core SignalR JavaScript.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 04/06/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/javascript-client
ms.openlocfilehash: d2530fe3c4b47687d3ef4015624499d96fea2d7b
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/08/2018
---
# <a name="aspnet-core-signalr-javascript-client"></a>Cliente de SignalR JavaScript Core ASP.NET

Por [Rachel Appel](http://twitter.com/rachelappel)

[!INCLUDE [2.1 preview notice](~/includes/2.1.md)]

La biblioteca de cliente de ASP.NET Core SignalR JavaScript permite a los desarrolladores llamar a código de concentrador de servidor.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

## <a name="install-the-signalr-client-package"></a>Instale el paquete de cliente de SignalR

La biblioteca de cliente de SignalR JavaScript se entrega como un [npm](https://www.npmjs.com/) paquete. Si está utilizando Visual Studio, ejecute `npm install` desde el **Package Manager Console** mientras se encuentre en la carpeta raíz. Para el código de Visual Studio, ejecute el comando desde el **Terminal integrado**.

  ```console
   npm init -y
   npm install @aspnet/signalr
  ```

NPM instala el contenido del paquete en el *node_modules\\ @aspnet\signalr\dist\browser*  carpeta. Cree una carpeta nueva denominada *signalr* en el *wwwroot\\lib* carpeta. Copia la *signalr.js* del archivo a la *wwwroot\lib\signalr* carpeta.

## <a name="use-the-signalr-javascript-client"></a>Usar al cliente de SignalR JavaScript

Referencia de cliente de SignalR JavaScript de la `<script>` elemento.

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a>Conectarse a un concentrador

El código siguiente se crea e inicia una conexión. Nombre del concentrador distingue mayúsculas de minúsculas.

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-12,28)]

### <a name="cross-origin-connections"></a>Conexiones entre orígenes

Por lo general, los exploradores cargar las conexiones desde el mismo dominio que la página solicitada. Sin embargo, hay ocasiones cuando se requiere una conexión a otro dominio.

Para evitar que un sitio malintencionado pueda leer los datos confidenciales desde otro sitio, [las conexiones entre orígenes](xref:security/cors) están deshabilitadas de forma predeterminada. Para permitir que una solicitud entre orígenes, habilitarla en la `Startup` clase.

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-34,55)]

## <a name="call-hub-methods-from-client"></a>Llamar a métodos de concentrador de cliente

Los clientes de JavaScript llama al métodos públicos en concentradores por usando `connection.invoke`. El `invoke` método acepta dos argumentos:

* El nombre del método de concentrador. En el ejemplo siguiente, el nombre de la base de datos central es `SendMessage`.
* Los argumentos definidos en el método de concentrador. En el ejemplo siguiente, el nombre del argumento es `message`.

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

## <a name="call-client-methods-from-hub"></a>Llamar a métodos de cliente desde el concentrador

Para recibir mensajes desde el concentrador, defina un método mediante el `connection.on` método.

* El nombre del método de cliente de JavaScript. En el ejemplo siguiente, el nombre de método es `ReceiveMessage`.
* Argumentos de que la central se pasa al método. En el ejemplo siguiente, el valor del argumento es `message`.

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

El código anterior en `connection.on` se ejecuta cuando llama a código del lado servidor mediante el `SendAsync` método.

[!code-javascript[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

SignalR determina a qué método de cliente para llamar a haciendo coincidir el nombre de método y los argumentos definan en `SendAsync` y `connection.on`.

> [!NOTE]
> Como práctica recomendada, llame a `connection.start` después `connection.on` por lo que los controladores están registrados antes de que se reciben los mensajes.

## <a name="error-handling-and-logging"></a>Registro y control de errores

Cadena de un `catch` método al final de la `connection.start` método para controlar los errores de cliente. Use `console.error` para errores de salida a la consola del explorador.

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=28)]

Configurar la traza de registro del lado cliente pasando un registrador y el tipo de evento que se registran cuando se realiza la conexión. Los mensajes se registran con el nivel de registro especificado y versiones posteriores. Niveles de registro disponibles son los siguientes:

* `signalR.LogLevel.Error` : Mensajes de error. Registros `Error` sólo mensajes.
* `signalR.LogLevel.Warning` : Mensajes de advertencia acerca de posibles errores. Registros de `Warning`, y `Error` mensajes.
* `signalR.LogLevel.Information` : Mensajes de estado sin errores. Registros de `Information`, `Warning`, y `Error` mensajes.
* `signalR.LogLevel.Trace` : Mensajes de seguimiento. Registra todo, incluidos los datos transportados entre cliente y concentrador.

Use la `configureLogging` método en `HubConnectionBuilder` para configurar el nivel de registro. Los mensajes se registran en la consola del explorador.

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=11)]

## <a name="related-resources"></a>Recursos relacionados

* [Concentradores de SignalR Core de ASP.NET](xref:signalr/hubs)
* [Permitir solicitudes entre orígenes (CORS) en ASP.NET Core](xref:security/cors)