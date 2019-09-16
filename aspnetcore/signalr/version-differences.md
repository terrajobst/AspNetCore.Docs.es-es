---
title: Diferencias entre Signalr y ASP.NET Core Signalr
author: bradygaster
description: Diferencias entre Signalr y ASP.NET Core Signalr
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.date: 11/14/2018
uid: signalr/version-differences
ms.openlocfilehash: 70b09493d9b4c96c897465d60e53e93a793c42f9
ms.sourcegitcommit: 387cf29f5d5addef2cbc70670a11d612806b36b2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/06/2019
ms.locfileid: "70746540"
---
# <a name="differences-between-aspnet-signalr-and-aspnet-core-signalr"></a>Diferencias entre ASP.NET Signalr y ASP.NET Core Signalr

ASP.NET Core Signalr no es compatible con clientes o servidores para ASP.NET Signalr. En este artículo se detallan las características que se han quitado o cambiado en ASP.NET Core Signalr.

## <a name="how-to-identify-the-signalr-version"></a>Identificación de la versión de Signalr

|                      | ASP.NET SignalR | SignalR de ASP.NET Core |
| -------------------- | --------------- | -------------------- |
| Paquete NuGet de servidor | [Microsoft.AspNet.SignalR](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/) | [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)<br>[Microsoft. AspNetCore. signalr](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework) |
| Paquetes NuGet de cliente | [Microsoft.AspNet.SignalR.Client](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Client/)<br>[Microsoft.AspNet.SignalR.JS](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.JS/) | [Microsoft.AspNetCore.SignalR.Client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/) |
| Paquete NPM de cliente | [signalr](https://www.npmjs.com/package/signalr) | [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) |
| Cliente de Java | [Repositorio de github](https://github.com/SignalR/java-client) en desuso  | Paquete Maven [com. Microsoft. signalr](https://search.maven.org/artifact/com.microsoft.signalr/signalr) |
| Tipo de aplicación de servidor | ASP.NET (System. Web) o Self-host OWIN | ASP.NET Core |
| Plataformas de servidor admitidas | .NET Framework 4,5 o posterior | .NET Framework 4.6.1 o versiones posteriores<br>.NET Core 2,1 o posterior |

## <a name="feature-differences"></a>Diferencias de características

### <a name="automatic-reconnects"></a>Reconexiones automáticas

No se admiten las reconexiones automáticas en ASP.NET Core Signalr. Si el cliente está desconectado, el usuario debe iniciar explícitamente una nueva conexión si desea volver a conectarse. En ASP.NET Signalr, Signalr intenta volver a conectarse al servidor si se quita la conexión.

### <a name="protocol-support"></a>Compatibilidad con protocolos

ASP.NET Core Signalr admite JSON, así como un nuevo protocolo binario basado en [MessagePack](xref:signalr/messagepackhubprotocol). Además, se pueden crear protocolos personalizados.

### <a name="transports"></a>Transportes

El transporte de tramas de siempre no se admite en ASP.NET Core Signalr.

## <a name="differences-on-the-server"></a>Diferencias en el servidor

Las bibliotecas del lado servidor de Signalr ASP.NET Core se incluyen en el paquete de [metapaquete Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) que forma parte de la plantilla de **aplicación Web ASP.net Core** para proyectos de Razor y MVC.

ASP.NET Core Signalr es un middleware ASP.NET Core, por lo que debe configurarse llamando a [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) en `Startup.ConfigureServices`.

```csharp
services.AddSignalR()
```

::: moniker range=">= aspnetcore-3.0"

Para configurar el enrutamiento, asigne rutas a los concentradores dentro de la llamada `Startup.Configure` al método [UseEndpoints](/dotnet/api/microsoft.aspnetcore.builder.endpointroutingapplicationbuilderextensions.useendpoints) en el método.


```csharp
app.UseRouting();

app.UseEndpoints(endpoints =>
{
    endpoints.MapHub<ChatHub>("/hub");
});
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

Para configurar el enrutamiento, asigne rutas a los concentradores dentro de la llamada `Startup.Configure` al método [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) en el método.

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

::: moniker-end

### <a name="sticky-sessions"></a>Sesiones permanentes

El modelo ampliación para ASP.NET Signalr permite a los clientes volver a conectarse y enviar mensajes a cualquier servidor de la granja. En ASP.NET Core Signalr, el cliente debe interactuar con el mismo servidor mientras dure la conexión. En el caso de ampliación con Redis, eso significa que se requieren sesiones permanentes. En el caso de ampliación con [Azure signalr Service](/azure/azure-signalr/), las sesiones permanentes no son necesarias porque el servicio administra las conexiones a los clientes.

### <a name="single-hub-per-connection"></a>Un solo concentrador por conexión

En ASP.NET Core Signalr, se ha simplificado el modelo de conexión. Las conexiones se realizan directamente en un solo concentrador, en lugar de usar una sola conexión para compartir el acceso a varios centros.

### <a name="streaming"></a>Streaming

ASP.NET Core Signalr ahora admite el [streaming de datos](xref:signalr/streaming) desde el concentrador al cliente.

### <a name="state"></a>Estado

Se ha quitado la capacidad de pasar el estado arbitrario entre los clientes y el concentrador (a menudo denominado HubState), así como la compatibilidad con los mensajes de progreso. En este momento no hay ningún homólogo de servidores proxy de concentrador.

### <a name="persistentconnection-removal"></a>Eliminación de PersistentConnection

En ASP.NET Core Signalr, se ha quitado la clase [PersistentConnection](https://docs.microsoft.com/previous-versions/aspnet/jj919047(v%3dvs.118)) .

### <a name="globalhost"></a>Host global

ASP.NET Core tiene la inserción de dependencias (DI) integrada en el marco de trabajo. Los servicios pueden usar DI para acceder a [HubContext](xref:signalr/hubcontext). El `GlobalHost` objeto que se usa en ASP.net signalr para `HubContext` obtener no existe en ASP.net Core signalr.

### <a name="hubpipeline"></a>HubPipeline

ASP.net Core signalr no admite `HubPipeline` módulos.

## <a name="differences-on-the-client"></a>Diferencias en el cliente

### <a name="typescript"></a>TypeScript

El cliente de Signalr ASP.NET Core está escrito en [TypeScript](https://www.typescriptlang.org/). Puede escribir en JavaScript o TypeScript cuando se usa el [cliente de JavaScript](xref:signalr/javascript-client).

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a>El cliente de JavaScript se hospeda en [NPM](https://www.npmjs.com/)

En versiones anteriores, el cliente de JavaScript se obtuvo a través de un paquete NuGet en Visual Studio. En el caso de las versiones [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) principales, el paquete NPM contiene las bibliotecas de JavaScript. Este paquete no está incluido en la plantilla de **aplicación Web de ASP.net Core** . Use NPM para obtener e instalar el `@aspnet/signalr` paquete NPM.

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a>jQuery

La dependencia de jQuery se ha quitado, pero los proyectos todavía pueden usar jQuery.

### <a name="internet-explorer-support"></a>Compatibilidad con Internet Explorer

ASP.NET Core Signalr requiere Microsoft Internet Explorer 11 o posterior (ASP.NET Signalr es compatible con Microsoft Internet Explorer 8 y versiones posteriores).

### <a name="javascript-client-method-syntax"></a>Sintaxis del método de cliente JavaScript

La sintaxis de JavaScript ha cambiado respecto a la versión anterior de Signalr. En lugar de usar `$connection` el objeto, cree una conexión mediante la API de [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) .

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

Use el método on para especificar los métodos [de](/javascript/api/@aspnet/signalr/HubConnection#on) cliente a los que puede llamar el concentrador.

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

Después de crear el método de cliente, inicie la conexión del concentrador. Encadenar un método [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) para registrar o controlar los errores.

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a>Servidores proxy de concentrador

Los proxies de concentrador ya no se generan automáticamente. En su lugar, el nombre del método se pasa a la API de [invocación](/javascript/api/%40aspnet/signalr/hubconnection#invoke) como una cadena.

### <a name="net-and-other-clients"></a>.NET y otros clientes

El `Microsoft.AspNetCore.SignalR.Client` paquete NuGet contiene las bibliotecas de cliente de .net para ASP.net Core signalr.

Use [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) para crear y compilar una instancia de una conexión a un concentrador.

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="scaleout-differences"></a>Diferencias de ampliación

ASP.NET Signalr admite SQL Server y Redis. ASP.NET Core Signalr es compatible con Azure Signalr Service y Redis.

### <a name="aspnet"></a>ASP.NET

* [Signalr ampliación con Azure Service Bus](/aspnet/signalr/overview/performance/scaleout-with-windows-azure-service-bus)
* [Signalr ampliación con Redis](/aspnet/signalr/overview/performance/scaleout-with-redis)
* [Signalr ampliación con SQL Server](/aspnet/signalr/overview/performance/scaleout-with-sql-server)

### <a name="aspnet-core"></a>ASP.NET Core

* [Servicio Azure SignalR](/azure/azure-signalr/)
* [Backplane de Redis](xref:signalr/redis-backplane)

## <a name="additional-resources"></a>Recursos adicionales

* [Concentradores](xref:signalr/hubs)
* [Cliente de JavaScript](xref:signalr/javascript-client)
* [Cliente .NET](xref:signalr/dotnet-client)
* [Plataformas compatibles](xref:signalr/supported-platforms)
