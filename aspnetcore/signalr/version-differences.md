---
title: Diferencias entre SignalR y ASP.NET Core SignalR
author: tdykstra
description: Diferencias entre SignalR y ASP.NET Core SignalR
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.date: 06/30/2018
uid: signalr/version-differences
ms.openlocfilehash: 6ed7e2e1ecadef08d71c4d7a7c3469738d07bcda
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095013"
---
# <a name="differences-between-signalr-and-aspnet-core-signalr"></a><span data-ttu-id="013f2-103">Diferencias entre SignalR y ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="013f2-103">Differences between SignalR and ASP.NET Core SignalR</span></span>

<span data-ttu-id="013f2-104">ASP.NET Core SignalR no es compatible con clientes o servidores para ASP.NET SignalR.</span><span class="sxs-lookup"><span data-stu-id="013f2-104">ASP.NET Core SignalR is not compatible with clients or servers for ASP.NET SignalR.</span></span> <span data-ttu-id="013f2-105">En este artículo se describe las características que se han quitado o cambiado en ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="013f2-105">This article details the features which have been removed or changed in the ASP.NET Core SignalR.</span></span>

## <a name="feature-differences"></a><span data-ttu-id="013f2-106">Diferencias de características</span><span class="sxs-lookup"><span data-stu-id="013f2-106">Feature differences</span></span>

### <a name="automatic-reconnects"></a><span data-ttu-id="013f2-107">Reconexiones automática</span><span class="sxs-lookup"><span data-stu-id="013f2-107">Automatic reconnects</span></span>

<span data-ttu-id="013f2-108">Ya no se admiten las reconexiones automática.</span><span class="sxs-lookup"><span data-stu-id="013f2-108">Automatic reconnects are no longer supported.</span></span> <span data-ttu-id="013f2-109">Anteriormente, SignalR intentó volver a conectarse al servidor si se quitó la conexión.</span><span class="sxs-lookup"><span data-stu-id="013f2-109">Previously, SignalR tried to reconnect to the server if the connection was dropped.</span></span> <span data-ttu-id="013f2-110">Ahora, si el cliente se desconecta el usuario debe iniciar explícitamente una nueva conexión si desea volver a conectar.</span><span class="sxs-lookup"><span data-stu-id="013f2-110">Now, if the client is disconnected the user must explicitly start a new connection if they want to reconnect.</span></span>

### <a name="protocol-support"></a><span data-ttu-id="013f2-111">Soporte de protocolo</span><span class="sxs-lookup"><span data-stu-id="013f2-111">Protocol support</span></span>

<span data-ttu-id="013f2-112">ASP.NET Core SignalR es compatible con JSON, así como un nuevo protocolo binario según [MessagePack](xref:signalr/messagepackhubprotocol).</span><span class="sxs-lookup"><span data-stu-id="013f2-112">ASP.NET Core SignalR supports JSON, as well as a new binary protocol based on [MessagePack](xref:signalr/messagepackhubprotocol).</span></span> <span data-ttu-id="013f2-113">Además, se pueden crear protocolos personalizados.</span><span class="sxs-lookup"><span data-stu-id="013f2-113">Additionally, custom protocols can be created.</span></span>

## <a name="differences-on-the-server"></a><span data-ttu-id="013f2-114">Diferencias en el servidor</span><span class="sxs-lookup"><span data-stu-id="013f2-114">Differences on the server</span></span>

<span data-ttu-id="013f2-115">Las bibliotecas del lado servidor de SignalR se incluyen en el `Microsoft.AspNetCore.App` paquete que forma parte de la **aplicación Web ASP.NET Core** plantilla para los proyectos de Razor y MVC.</span><span class="sxs-lookup"><span data-stu-id="013f2-115">The SignalR server-side libraries are included in the `Microsoft.AspNetCore.App` package that is part of the **ASP.NET Core Web Application** template for both Razor and MVC projects.</span></span>

<span data-ttu-id="013f2-116">SignalR es un middleware de ASP.NET Core, por lo que se debe configurar mediante una llamada a `AddSignalR` en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="013f2-116">SignalR is an ASP.NET Core middleware, so it must be configured by calling `AddSignalR` in `Startup.ConfigureServices`.</span></span>

```csharp
services.AddSignalR();
```

<span data-ttu-id="013f2-117">Para configurar el enrutamiento, se asignan las rutas a los concentradores dentro de la `UseSignalR` llame al método el `Startup.Configure` método.</span><span class="sxs-lookup"><span data-stu-id="013f2-117">To configure routing, map routes to hubs inside the `UseSignalR` method call in the `Startup.Configure` method.</span></span>

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

### <a name="sticky-sessions-now-required"></a><span data-ttu-id="013f2-118">Ahora requeridas sesiones</span><span class="sxs-lookup"><span data-stu-id="013f2-118">Sticky sessions now required</span></span>

<span data-ttu-id="013f2-119">Debido a cómo escalar horizontalmente trabajó en las versiones anteriores de SignalR, los clientes podrían volver a conectarse y enviar mensajes a cualquier servidor de la granja de servidores.</span><span class="sxs-lookup"><span data-stu-id="013f2-119">Because of how scale-out worked in the previous versions of SignalR, clients could reconnect and send messages to any server in the farm.</span></span> <span data-ttu-id="013f2-120">Debido a cambios en el modelo de escalabilidad horizontal, así como que no admiten reconexiones, ya no se admite.</span><span class="sxs-lookup"><span data-stu-id="013f2-120">Due to changes to the scale-out model, as well as not supporting reconnects, this is no longer supported.</span></span> <span data-ttu-id="013f2-121">Ahora, una vez que el cliente se conecta al servidor necesita interactuar con el mismo servidor para la duración de la conexión.</span><span class="sxs-lookup"><span data-stu-id="013f2-121">Now, once the client connects to the server it needs to interact with the same server for the duration of the connection.</span></span>

### <a name="single-hub-per-connection"></a><span data-ttu-id="013f2-122">Centro único por conexión</span><span class="sxs-lookup"><span data-stu-id="013f2-122">Single hub per connection</span></span>

<span data-ttu-id="013f2-123">En ASP.NET Core SignalR, se ha simplificado el modelo de conexión.</span><span class="sxs-lookup"><span data-stu-id="013f2-123">In ASP.NET Core SignalR, the connection model has been simplified.</span></span> <span data-ttu-id="013f2-124">Las conexiones se realizan directamente en un único centro, en lugar de una sola conexión que se usa para compartir el acceso a varios centros.</span><span class="sxs-lookup"><span data-stu-id="013f2-124">Connections are made directly to a single hub, rather than a single connection being used to share access to multiple hubs.</span></span>

### <a name="streaming"></a><span data-ttu-id="013f2-125">Streaming</span><span class="sxs-lookup"><span data-stu-id="013f2-125">Streaming</span></span>

<span data-ttu-id="013f2-126">SignalR ahora admite [datos de streaming](xref:signalr/streaming) desde el concentrador para el cliente.</span><span class="sxs-lookup"><span data-stu-id="013f2-126">SignalR now supports [streaming data](xref:signalr/streaming) from the hub to the client.</span></span>

### <a name="state"></a><span data-ttu-id="013f2-127">Estado</span><span class="sxs-lookup"><span data-stu-id="013f2-127">State</span></span>

<span data-ttu-id="013f2-128">Se ha quitado la capacidad de pasar información de estado arbitraria entre los clientes y el centro de (a menudo denominada HubState), así como compatibilidad con los mensajes de progreso.</span><span class="sxs-lookup"><span data-stu-id="013f2-128">The ability to pass arbitrary state between clients and the hub (often called HubState) has been removed, as well as support for progress messages.</span></span> <span data-ttu-id="013f2-129">En este momento no hay ningún homólogo de los servidores proxy de concentrador.</span><span class="sxs-lookup"><span data-stu-id="013f2-129">There is no counterpart of hub proxies at the moment.</span></span>

## <a name="differences-on-the-client"></a><span data-ttu-id="013f2-130">Diferencias en el cliente</span><span class="sxs-lookup"><span data-stu-id="013f2-130">Differences on the client</span></span>

### <a name="typescript"></a><span data-ttu-id="013f2-131">TypeScript</span><span class="sxs-lookup"><span data-stu-id="013f2-131">TypeScript</span></span>

<span data-ttu-id="013f2-132">La versión de ASP.NET Core de SignalR está escrita en [TypeScript](https://www.typescriptlang.org/).</span><span class="sxs-lookup"><span data-stu-id="013f2-132">The ASP.NET Core version of SignalR is written in [TypeScript](https://www.typescriptlang.org/).</span></span> <span data-ttu-id="013f2-133">Puede escribir en JavaScript o TypeScript al usar el [cliente JavaScript](xref:signalr/javascript-client).</span><span class="sxs-lookup"><span data-stu-id="013f2-133">You can write in JavaScript or TypeScript when using the [JavaScript client](xref:signalr/javascript-client).</span></span>

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a><span data-ttu-id="013f2-134">El cliente de JavaScript está hospedado en [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="013f2-134">The JavaScript client is hosted at [npm](https://www.npmjs.com/)</span></span>

<span data-ttu-id="013f2-135">En versiones anteriores, el cliente de JavaScript se obtuvo a través de un paquete de NuGet en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="013f2-135">In previous versions, the JavaScript client was obtained through a NuGet package in Visual Studio.</span></span> <span data-ttu-id="013f2-136">Para las versiones principales, el [ @aspnet/signalr paquete npm](https://www.npmjs.com/package/@aspnet/signalr) contiene las bibliotecas de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="013f2-136">For the Core versions, the [@aspnet/signalr npm package](https://www.npmjs.com/package/@aspnet/signalr) contains the JavaScript libraries.</span></span> <span data-ttu-id="013f2-137">Este paquete no se incluye en el **aplicación Web ASP.NET Core** plantilla.</span><span class="sxs-lookup"><span data-stu-id="013f2-137">This package isn't included in the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="013f2-138">Utilice npm para obtener e instalar el `@aspnet/signalr` paquete npm.</span><span class="sxs-lookup"><span data-stu-id="013f2-138">Use npm to obtain and install the `@aspnet/signalr` npm package.</span></span>

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a><span data-ttu-id="013f2-139">jQuery</span><span class="sxs-lookup"><span data-stu-id="013f2-139">jQuery</span></span>

<span data-ttu-id="013f2-140">Se quitó la dependencia de jQuery, sin embargo, los proyectos pueden seguir usando jQuery.</span><span class="sxs-lookup"><span data-stu-id="013f2-140">The dependency on jQuery has been removed, however projects can still use jQuery.</span></span>

### <a name="javascript-client-method-syntax"></a><span data-ttu-id="013f2-141">Sintaxis de método del cliente de JavaScript</span><span class="sxs-lookup"><span data-stu-id="013f2-141">JavaScript client method syntax</span></span>

<span data-ttu-id="013f2-142">La sintaxis de JavaScript ha cambiado desde la versión anterior de SignalR.</span><span class="sxs-lookup"><span data-stu-id="013f2-142">The JavaScript syntax has changed from the previous version of SignalR.</span></span> <span data-ttu-id="013f2-143">En lugar de usar el `$connection` , cree una conexión con el `HubConnectionBuilder` API.</span><span class="sxs-lookup"><span data-stu-id="013f2-143">Rather than using the `$connection` object, create a connection using the `HubConnectionBuilder` API.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

<span data-ttu-id="013f2-144">Use `connection.on` para especificar los métodos de cliente que puede llamar la central.</span><span class="sxs-lookup"><span data-stu-id="013f2-144">Use `connection.on` to specify client methods that the hub can call.</span></span>

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

<span data-ttu-id="013f2-145">Después de crear el método de cliente, inicie la conexión de concentrador.</span><span class="sxs-lookup"><span data-stu-id="013f2-145">After creating the client method, start the hub connection.</span></span> <span data-ttu-id="013f2-146">Cadena de un `catch` método para iniciar o controlar los errores.</span><span class="sxs-lookup"><span data-stu-id="013f2-146">Chain a `catch` method to log or handle errors.</span></span>

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a><span data-ttu-id="013f2-147">Servidores proxy de concentrador</span><span class="sxs-lookup"><span data-stu-id="013f2-147">Hub proxies</span></span>

<span data-ttu-id="013f2-148">Automáticamente ya no se generan servidores proxy de concentrador.</span><span class="sxs-lookup"><span data-stu-id="013f2-148">Hub proxies are no longer automatically generated.</span></span> <span data-ttu-id="013f2-149">En su lugar, el nombre del método se pasa a la `invoke` API como una cadena.</span><span class="sxs-lookup"><span data-stu-id="013f2-149">Instead, the method name is passed into the `invoke` API as a string.</span></span>

### <a name="net-and-other-clients"></a><span data-ttu-id="013f2-150">.NET y otros clientes</span><span class="sxs-lookup"><span data-stu-id="013f2-150">.NET and other clients</span></span>

<span data-ttu-id="013f2-151">El `Microsoft.AspNetCore.SignalR.Client` paquete NuGet contiene las bibliotecas de cliente .NET para ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="013f2-151">The `Microsoft.AspNetCore.SignalR.Client` NuGet package contains the .NET client libraries for ASP.NET Core SignalR.</span></span>

<span data-ttu-id="013f2-152">Use el `HubConnectionBuilder` para crear y generar una instancia de una conexión a un concentrador.</span><span class="sxs-lookup"><span data-stu-id="013f2-152">Use the `HubConnectionBuilder` to create and build an instance of a connection to a hub.</span></span>

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="additional-resources"></a><span data-ttu-id="013f2-153">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="013f2-153">Additional resources</span></span>

* [<span data-ttu-id="013f2-154">Concentradores</span><span class="sxs-lookup"><span data-stu-id="013f2-154">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="013f2-155">Cliente de JavaScript</span><span class="sxs-lookup"><span data-stu-id="013f2-155">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="013f2-156">Cliente .NET</span><span class="sxs-lookup"><span data-stu-id="013f2-156">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="013f2-157">Plataformas compatibles</span><span class="sxs-lookup"><span data-stu-id="013f2-157">Supported platforms</span></span>](xref:signalr/supported-platforms)
