---
title: Diferencias entre SignalR y ASP.NET Core SignalR
author: tdykstra
description: Diferencias entre SignalR y ASP.NET Core SignalR
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.date: 08/20/2018
uid: signalr/version-differences
ms.openlocfilehash: b904f57af3700b6e1e2143913dfa08da9bf8bbd2
ms.sourcegitcommit: d27317c16f113e7c111583042ec7e4c5a26adf6f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/20/2018
ms.locfileid: "41837079"
---
# <a name="differences-between-aspnet-signalr-and-aspnet-core-signalr"></a>Diferencias entre SignalR de ASP.NET y ASP.NET Core SignalR

ASP.NET Core SignalR no es compatible con clientes o servidores para ASP.NET SignalR. En este artículo se detalla las características que se han quitado o cambiado en ASP.NET Core SignalR.

## <a name="how-to-identify-the-signalr-version"></a>Cómo identificar la versión de SignalR

|                      | ASP.NET SignalR | SignalR de ASP.NET Core |
| -------------------- | --------------- | -------------------- |
| Paquete de NuGet de servidor | [Microsoft.AspNet.SignalR](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/) | [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)<br>[Microsoft.AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework) |
| Paquetes NuGet del cliente | [Microsoft.AspNet.SignalR.Client](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Client/)<br>[Microsoft.AspNet.SignalR.JS](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.JS/) | [Microsoft.AspNetCore.SignalR.Client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/) |
| Paquete de npm de cliente | [signalr](https://www.npmjs.com/package/signalr) | [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) |
| Tipo de aplicación de servidor | ASP.NET (System.Web) o autohospedaje OWIN | ASP.NET Core |
| Plataformas de servidor compatibles | .NET framework 4.5 o posterior | .NET Framework 4.6.1 o versiones posteriores<br>.NET core 2.1 o posterior |

## <a name="feature-differences"></a>Diferencias de características

### <a name="automatic-reconnects"></a>Reconexiones automática

Ya no se admiten las reconexiones automática. Anteriormente, SignalR intentó volver a conectarse al servidor si se quitó la conexión. Ahora, si el cliente se desconecta el usuario debe iniciar explícitamente una nueva conexión si desea volver a conectar.

### <a name="protocol-support"></a>Soporte de protocolo

ASP.NET Core SignalR es compatible con JSON, así como un nuevo protocolo binario según [MessagePack](xref:signalr/messagepackhubprotocol). Además, se pueden crear protocolos personalizados.

## <a name="differences-on-the-server"></a>Diferencias en el servidor

Las bibliotecas de servidor de ASP.NET Core SignalR se incluyen en el [Microsoft.AspNetCore.App metapaquete](xref:fundamentals/metapackage-app) paquete que forma parte de la **aplicación Web ASP.NET Core** plantilla de Razor y MVC proyectos.

ASP.NET Core SignalR es un middleware de ASP.NET Core, por lo que se debe configurar mediante una llamada a [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) en `Startup.ConfigureServices`.

```csharp
services.AddSignalR()
```

Para configurar el enrutamiento, se asignan las rutas a los concentradores dentro de la [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) llame al método el `Startup.Configure` método.

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

### <a name="sticky-sessions-now-required"></a>Ahora requeridas sesiones

Debido a cómo escalar horizontalmente trabajado en SignalR de ASP.NET, los clientes podrían volver a conectarse y enviar mensajes a cualquier servidor de la granja de servidores. Debido a cambios en el modelo de escalabilidad horizontal, así como que no admiten reconexiones, ya no se admite. Una vez que el cliente se conecta al servidor, debe interactuar con el mismo servidor para la duración de la conexión.

### <a name="single-hub-per-connection"></a>Centro único por conexión

En ASP.NET Core SignalR, se ha simplificado el modelo de conexión. Las conexiones se realizan directamente en un único centro, en lugar de una sola conexión que se usa para compartir el acceso a varios centros.

### <a name="streaming"></a>Streaming

ASP.NET Core SignalR ahora admite [datos de streaming](xref:signalr/streaming) desde el concentrador para el cliente.

### <a name="state"></a>Estado

Se ha quitado la capacidad de pasar información de estado arbitraria entre los clientes y el centro de (a menudo denominada HubState), así como compatibilidad con los mensajes de progreso. En este momento no hay ningún homólogo de los servidores proxy de concentrador.

## <a name="differences-on-the-client"></a>Diferencias en el cliente

### <a name="typescript"></a>TypeScript

El cliente de ASP.NET Core SignalR está escrito en [TypeScript](https://www.typescriptlang.org/). Puede escribir en JavaScript o TypeScript al usar el [cliente JavaScript](xref:signalr/javascript-client).

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a>El cliente de JavaScript está hospedado en [npm](https://www.npmjs.com/)

En versiones anteriores, el cliente de JavaScript se obtuvo a través de un paquete de NuGet en Visual Studio. Para las versiones principales, el [ @aspnet/signalr ](https://www.npmjs.com/package/@aspnet/signalr) paquete npm contiene las bibliotecas de JavaScript. Este paquete no se incluye en el **aplicación Web ASP.NET Core** plantilla. Utilice npm para obtener e instalar el `@aspnet/signalr` paquete npm.

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a>jQuery

Se quitó la dependencia de jQuery, sin embargo, los proyectos pueden seguir usando jQuery.

### <a name="javascript-client-method-syntax"></a>Sintaxis de método del cliente de JavaScript

La sintaxis de JavaScript ha cambiado desde la versión anterior de SignalR. En lugar de usar el `$connection` , cree una conexión con el [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) API.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

Use la [en](/javascript/api/@aspnet/signalr/HubConnection#on) método para especificar métodos de cliente que se puede llamar la central.

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

Después de crear el método de cliente, inicie la conexión de concentrador. Cadena de un [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) método para iniciar o controlar los errores.

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a>Servidores proxy de concentrador

Automáticamente ya no se generan servidores proxy de concentrador. En su lugar, el nombre del método se pasa a la [invocar](/javascript/api/%40aspnet/signalr/hubconnection#invoke) API como una cadena.

### <a name="net-and-other-clients"></a>.NET y otros clientes

El `Microsoft.AspNetCore.SignalR.Client` paquete NuGet contiene las bibliotecas de cliente .NET para ASP.NET Core SignalR.

Use la [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) para crear y generar una instancia de una conexión a un concentrador.

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
