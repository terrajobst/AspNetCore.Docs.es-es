---
title: Diferencias entre SignalR y ASP.NET Core SignalR
author: rachelappel
description: Diferencias entre SignalR y ASP.NET Core SignalR
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.date: 06/30/2018
uid: signalr/version-differences
ms.openlocfilehash: fd314d93333bd159aef4bb4863be50c646809cf0
ms.sourcegitcommit: 1faf2525902236428dae6a59e375519bafd5d6d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2018
ms.locfileid: "37090072"
---
# <a name="differences-between-signalr-and-aspnet-core-signalr"></a>Diferencias entre SignalR y ASP.NET Core SignalR

Núcleo de ASP.NET SignalR no es compatible con clientes o servidores para ASP.NET SignalR. En este artículo se detalla las características que se han quitado o cambiado en el núcleo de ASP.NET SignalR.

## <a name="feature-differences"></a>Diferencias de características

### <a name="automatic-reconnects"></a>Reconexiones automática

Ya no se admiten las reconexiones automática. Anteriormente, SignalR intentó volver a conectarse al servidor si se interrumpió la conexión. Ahora, si el cliente se desconecta el usuario debe iniciar explícitamente una nueva conexión si desea volver a conectarse.

### <a name="protocol-support"></a>Compatibilidad con el protocolo

ASP.NET Core SignalR admite JSON, así como un nuevo protocolo binario en función de [MessagePack](xref:signalr/messagepackhubprotocol). Además, pueden crearse protocolos personalizados.

## <a name="differences-on-the-server"></a>Diferencias en el servidor

Las bibliotecas de servidor de SignalR se incluyen en el `Microsoft.AspNetCore.App` paquete que forma parte de la **aplicación Web de ASP.NET Core** plantilla para los proyectos de Razor y MVC.

SignalR es un middleware de ASP.NET Core, por lo que debe configurarse mediante una llamada a `AddSignalR` en `Startup.ConfigureServices`.

```csharp
services.AddSignalR();
```

Para configurar el enrutamiento, asignar rutas a los concentradores dentro de la `UseSignalR` llamada al método el `Startup.Configure` método.

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

### <a name="sticky-sessions-now-required"></a>Sesiones permanentes ahora obligatorio

Debido a la escalabilidad horizontal funcionado en las versiones anteriores de SignalR, los clientes pueden volver a conectarse y enviar mensajes a cualquier servidor de la granja de servidores. Debido a cambios en el modelo de escalabilidad horizontal, así como no compatibles con reconexiones, ya no se admite. Ahora, una vez que el cliente se conecta al servidor debe interactuar con el mismo servidor para la duración de la conexión.

### <a name="single-hub-per-connection"></a>Concentrador único por conexión

En ASP.NET Core SignalR, se ha simplificado el modelo de conexión. Las conexiones se realizan directamente a un único concentrador, en lugar de una conexión única que se usa para compartir el acceso a los centros de varios.

### <a name="streaming"></a>Streaming

SignalR ahora admite [datos de transmisión](xref:signalr/streaming) desde el concentrador para el cliente.

### <a name="state"></a>Estado

Se quitó la capacidad de pasar información de estado arbitraria entre los clientes y el concentrador (también denominados HubState), así como la compatibilidad para mensajes de progreso. No hay ningún equivalente de los servidores proxy de concentrador en este momento.

## <a name="differences-on-the-client"></a>Diferencias en el cliente

### <a name="typescript"></a>TypeScript

La versión de ASP.NET Core de SignalR se escribe [TypeScript](https://www.typescriptlang.org/). Puede escribir en JavaScript o TypeScript al usar la [cliente JavaScript](xref:signalr/javascript-client).

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a>El cliente de JavaScript se hospeda en [npm](https://www.npmjs.com/)

En versiones anteriores, el cliente de JavaScript se obtuvo a través de un paquete de NuGet en Visual Studio. Para las versiones principales, el [ @aspnet/signalr paquete npm](https://www.npmjs.com/package/@aspnet/signalr) contiene las bibliotecas de JavaScript. Este paquete no se incluye en el **aplicación Web de ASP.NET Core** plantilla. Use npm para obtener e instalar la `@aspnet/signalr` paquete npm.

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a>jQuery

Se quitó la dependencia de jQuery, sin embargo, los proyectos todavía pueden usar jQuery.

### <a name="javascript-client-method-syntax"></a>Sintaxis de método de cliente de JavaScript

La sintaxis de JavaScript ha cambiado desde la versión anterior de SignalR. En lugar de usar el `$connection` de objetos, crear una conexión mediante el `HubConnectionBuilder` API.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

Use `connection.on` para especificar los métodos de cliente que puede llamar la central.

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

Después de crear el método de cliente, inicie la conexión de concentrador. Cadena de un `catch` método para iniciar sesión o controlar los errores.

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a>Servidores proxy de concentrador

Servidores proxy de concentrador automáticamente ya no se genera. En su lugar, se pasa el nombre del método en el `invoke` API como una cadena.

### <a name="net-and-other-clients"></a>.NET y otros clientes

El `Microsoft.AspNetCore.SignalR.Client` paquete NuGet contiene las bibliotecas de cliente .NET para ASP.NET Core SignalR.

Use el `HubConnectionBuilder` para crear y generar una instancia de una conexión a un concentrador.

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="additional-resources"></a>Recursos adicionales

* [Concentradores](xref:signalr/hubs)
* [Cliente de JavaScript](xref:signalr/javascript-client)
* [Cliente .NET](xref:signalr/dotnet-client)
* [Plataformas compatibles](xref:signalr/supported-platforms)
