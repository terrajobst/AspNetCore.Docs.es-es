---
title: Cliente ASP.NET Core SignalR JavaScript
author: tdykstra
description: Información general de cliente de JavaScript de SignalR de ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/14/2018
uid: signalr/javascript-client
ms.openlocfilehash: 639c30f1d145a3da5e4f5857f32c1b573c1bfce2
ms.sourcegitcommit: 2c158fcfd325cad97ead608a816e525fe3dcf757
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/14/2018
ms.locfileid: "41829317"
---
# <a name="aspnet-core-signalr-javascript-client"></a>Cliente ASP.NET Core SignalR JavaScript

Por [Rachel Appel](http://twitter.com/rachelappel)

La biblioteca de cliente de ASP.NET Core SignalR JavaScript permite a los desarrolladores llamar a código de concentrador de servidor.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

## <a name="install-the-signalr-client-package"></a>Instale el paquete de cliente de SignalR

La biblioteca de cliente de JavaScript de SignalR se entrega como un [npm](https://www.npmjs.com/) paquete. Si usa Visual Studio, ejecute `npm install` desde el **Package Manager Console** mientras se encuentre en la carpeta raíz. Para Visual Studio Code, ejecute el comando desde el **Terminal integrado**.

  ```console
  npm init -y
  npm install @aspnet/signalr
  ```

NPM instala el contenido del paquete en el *node_modules\\ @aspnet\signalr\dist\browser*  carpeta. Cree una carpeta nueva denominada *signalr* bajo el *wwwroot\\lib* carpeta. Copia el *signalr.js* del archivo a la *wwwroot\lib\signalr* carpeta.

## <a name="use-the-signalr-javascript-client"></a>Usar al cliente de JavaScript de SignalR

Referencia del cliente de JavaScript de SignalR en el `<script>` elemento.

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a>Conectarse a un concentrador

El código siguiente se crea e inicia una conexión. Nombre del concentrador distingue mayúsculas de minúsculas.

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-12,28)]

### <a name="cross-origin-connections"></a>Conexiones de origen cruzado

Normalmente, exploradores de carga de las conexiones desde el mismo dominio que la página solicitada. Sin embargo, hay ocasiones cuando se requiere una conexión a otro dominio.

Para evitar que un sitio malintencionado lea datos confidenciales de otro sitio, [las conexiones de origen cruzado](xref:security/cors) están deshabilitados de forma predeterminada. Para permitir que una solicitud de origen cruzado, habilitarlo en el `Startup` clase.

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a>Llamar a métodos de concentrador de cliente

Los clientes JavaScript llamar a métodos públicos en concentradores a través de la [invocar](/javascript/api/%40aspnet/signalr/hubconnection#invoke) método de la [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection). El `invoke` método acepta dos argumentos:

* El nombre del método de concentrador. En el ejemplo siguiente, que es el nombre del método del concentrador `SendMessage`.
* Los argumentos definidos en el método de concentrador. En el ejemplo siguiente, que es el nombre del argumento `message`.

  [!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

## <a name="call-client-methods-from-hub"></a>Llamar a métodos cliente desde el concentrador

Para recibir mensajes desde el centro, defina un método mediante el [en](/javascript/api/%40aspnet/signalr/hubconnection#on) método de la `HubConnection`.

* El nombre del método de cliente de JavaScript. En el ejemplo siguiente, que es el nombre del método `ReceiveMessage`.
* Argumentos que del concentrador se pasa al método. En el ejemplo siguiente, el valor del argumento es `message`.

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

El código anterior en `connection.on` se ejecuta cuando se llama al código del lado servidor mediante el [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) método.

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

SignalR determina qué método de cliente para llamar a haciendo coincidir el nombre del método y los argumentos definan en `SendAsync` y `connection.on`.

> [!NOTE]
> Como práctica recomendada, llame a la [iniciar](/javascript/api/%40aspnet/signalr/hubconnection#start) método en el `HubConnection` después `on`. Si lo hace, garantiza que los controladores registrados antes de que se reciben los mensajes.

## <a name="error-handling-and-logging"></a>Registro y control de errores

Cadena de un `catch` método al final de la `start` método para controlar los errores del lado cliente. Use `console.error` para errores de salida a la consola del explorador.

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=28)]

Seguimiento de registro del lado cliente instalación pasando un registrador y el tipo de evento que se registran cuando se realiza la conexión. Los mensajes se registran con el nivel de registro especificado y versiones posteriores. Los niveles de registro disponibles son los siguientes:

* `signalR.LogLevel.Error` &ndash; Mensajes de error. Registros `Error` solo los mensajes.
* `signalR.LogLevel.Warning` &ndash; Mensajes de advertencia acerca de posibles errores. Los registros de `Warning`, y `Error` mensajes.
* `signalR.LogLevel.Information` &ndash; Mensajes de estado sin errores. Los registros de `Information`, `Warning`, y `Error` mensajes.
* `signalR.LogLevel.Trace` &ndash; Mensajes de seguimiento. Registra todo, incluidos los datos transportados entre cliente y concentrador.

Use la [configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) método [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) para configurar el nivel de registro. Los mensajes se registran en la consola del explorador.

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="related-resources"></a>Recursos relacionados

* [Referencia de la API de JavaScript](/javascript/api/)
* [Concentradores](xref:signalr/hubs)
* [Cliente .NET](xref:signalr/dotnet-client)
* [Publicar en Azure](xref:signalr/publish-to-azure-web-app)
* [Habilitar solicitudes entre orígenes (CORS) en ASP.NET Core](xref:security/cors)
