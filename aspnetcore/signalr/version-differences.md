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
# <a name="differences-between-signalr-and-aspnet-core-signalr"></a><span data-ttu-id="49992-103">Diferencias entre SignalR y ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="49992-103">Differences between SignalR and ASP.NET Core SignalR</span></span>

<span data-ttu-id="49992-104">Núcleo de ASP.NET SignalR no es compatible con clientes o servidores para ASP.NET SignalR.</span><span class="sxs-lookup"><span data-stu-id="49992-104">ASP.NET Core SignalR is not compatible with clients or servers for ASP.NET SignalR.</span></span> <span data-ttu-id="49992-105">En este artículo se detalla las características que se han quitado o cambiado en el núcleo de ASP.NET SignalR.</span><span class="sxs-lookup"><span data-stu-id="49992-105">This article details the features which have been removed or changed in the ASP.NET Core SignalR.</span></span>

## <a name="feature-differences"></a><span data-ttu-id="49992-106">Diferencias de características</span><span class="sxs-lookup"><span data-stu-id="49992-106">Feature differences</span></span>

### <a name="automatic-reconnects"></a><span data-ttu-id="49992-107">Reconexiones automática</span><span class="sxs-lookup"><span data-stu-id="49992-107">Automatic reconnects</span></span>

<span data-ttu-id="49992-108">Ya no se admiten las reconexiones automática.</span><span class="sxs-lookup"><span data-stu-id="49992-108">Automatic reconnects are no longer supported.</span></span> <span data-ttu-id="49992-109">Anteriormente, SignalR intentó volver a conectarse al servidor si se interrumpió la conexión.</span><span class="sxs-lookup"><span data-stu-id="49992-109">Previously, SignalR tried to reconnect to the server if the connection was dropped.</span></span> <span data-ttu-id="49992-110">Ahora, si el cliente se desconecta el usuario debe iniciar explícitamente una nueva conexión si desea volver a conectarse.</span><span class="sxs-lookup"><span data-stu-id="49992-110">Now, if the client is disconnected the user must explicitly start a new connection if they want to reconnect.</span></span>

### <a name="protocol-support"></a><span data-ttu-id="49992-111">Compatibilidad con el protocolo</span><span class="sxs-lookup"><span data-stu-id="49992-111">Protocol support</span></span>

<span data-ttu-id="49992-112">ASP.NET Core SignalR admite JSON, así como un nuevo protocolo binario en función de [MessagePack](xref:signalr/messagepackhubprotocol).</span><span class="sxs-lookup"><span data-stu-id="49992-112">ASP.NET Core SignalR supports JSON, as well as a new binary protocol based on [MessagePack](xref:signalr/messagepackhubprotocol).</span></span> <span data-ttu-id="49992-113">Además, pueden crearse protocolos personalizados.</span><span class="sxs-lookup"><span data-stu-id="49992-113">Additionally, custom protocols can be created.</span></span>

## <a name="differences-on-the-server"></a><span data-ttu-id="49992-114">Diferencias en el servidor</span><span class="sxs-lookup"><span data-stu-id="49992-114">Differences on the server</span></span>

<span data-ttu-id="49992-115">Las bibliotecas de servidor de SignalR se incluyen en el `Microsoft.AspNetCore.App` paquete que forma parte de la **aplicación Web de ASP.NET Core** plantilla para los proyectos de Razor y MVC.</span><span class="sxs-lookup"><span data-stu-id="49992-115">The SignalR server-side libraries are included in the `Microsoft.AspNetCore.App` package that is part of the **ASP.NET Core Web Application** template for both Razor and MVC projects.</span></span>

<span data-ttu-id="49992-116">SignalR es un middleware de ASP.NET Core, por lo que debe configurarse mediante una llamada a `AddSignalR` en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="49992-116">SignalR is an ASP.NET Core middleware, so it must be configured by calling `AddSignalR` in `Startup.ConfigureServices`.</span></span>

```csharp
services.AddSignalR();
```

<span data-ttu-id="49992-117">Para configurar el enrutamiento, asignar rutas a los concentradores dentro de la `UseSignalR` llamada al método el `Startup.Configure` método.</span><span class="sxs-lookup"><span data-stu-id="49992-117">To configure routing, map routes to hubs inside the `UseSignalR` method call in the `Startup.Configure` method.</span></span>

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

### <a name="sticky-sessions-now-required"></a><span data-ttu-id="49992-118">Sesiones permanentes ahora obligatorio</span><span class="sxs-lookup"><span data-stu-id="49992-118">Sticky sessions now required</span></span>

<span data-ttu-id="49992-119">Debido a la escalabilidad horizontal funcionado en las versiones anteriores de SignalR, los clientes pueden volver a conectarse y enviar mensajes a cualquier servidor de la granja de servidores.</span><span class="sxs-lookup"><span data-stu-id="49992-119">Because of how scale-out worked in the previous versions of SignalR, clients could reconnect and send messages to any server in the farm.</span></span> <span data-ttu-id="49992-120">Debido a cambios en el modelo de escalabilidad horizontal, así como no compatibles con reconexiones, ya no se admite.</span><span class="sxs-lookup"><span data-stu-id="49992-120">Due to changes to the scale-out model, as well as not supporting reconnects, this is no longer supported.</span></span> <span data-ttu-id="49992-121">Ahora, una vez que el cliente se conecta al servidor debe interactuar con el mismo servidor para la duración de la conexión.</span><span class="sxs-lookup"><span data-stu-id="49992-121">Now, once the client connects to the server it needs to interact with the same server for the duration of the connection.</span></span>

### <a name="single-hub-per-connection"></a><span data-ttu-id="49992-122">Concentrador único por conexión</span><span class="sxs-lookup"><span data-stu-id="49992-122">Single hub per connection</span></span>

<span data-ttu-id="49992-123">En ASP.NET Core SignalR, se ha simplificado el modelo de conexión.</span><span class="sxs-lookup"><span data-stu-id="49992-123">In ASP.NET Core SignalR, the connection model has been simplified.</span></span> <span data-ttu-id="49992-124">Las conexiones se realizan directamente a un único concentrador, en lugar de una conexión única que se usa para compartir el acceso a los centros de varios.</span><span class="sxs-lookup"><span data-stu-id="49992-124">Connections are made directly to a single hub, rather than a single connection being used to share access to multiple hubs.</span></span>

### <a name="streaming"></a><span data-ttu-id="49992-125">Streaming</span><span class="sxs-lookup"><span data-stu-id="49992-125">Streaming</span></span>

<span data-ttu-id="49992-126">SignalR ahora admite [datos de transmisión](xref:signalr/streaming) desde el concentrador para el cliente.</span><span class="sxs-lookup"><span data-stu-id="49992-126">SignalR now supports [streaming data](xref:signalr/streaming) from the hub to the client.</span></span>

### <a name="state"></a><span data-ttu-id="49992-127">Estado</span><span class="sxs-lookup"><span data-stu-id="49992-127">State</span></span>

<span data-ttu-id="49992-128">Se quitó la capacidad de pasar información de estado arbitraria entre los clientes y el concentrador (también denominados HubState), así como la compatibilidad para mensajes de progreso.</span><span class="sxs-lookup"><span data-stu-id="49992-128">The ability to pass arbitrary state between clients and the hub (often called HubState) has been removed, as well as support for progress messages.</span></span> <span data-ttu-id="49992-129">No hay ningún equivalente de los servidores proxy de concentrador en este momento.</span><span class="sxs-lookup"><span data-stu-id="49992-129">There is no counterpart of hub proxies at the moment.</span></span>

## <a name="differences-on-the-client"></a><span data-ttu-id="49992-130">Diferencias en el cliente</span><span class="sxs-lookup"><span data-stu-id="49992-130">Differences on the client</span></span>

### <a name="typescript"></a><span data-ttu-id="49992-131">TypeScript</span><span class="sxs-lookup"><span data-stu-id="49992-131">TypeScript</span></span>

<span data-ttu-id="49992-132">La versión de ASP.NET Core de SignalR se escribe [TypeScript](https://www.typescriptlang.org/).</span><span class="sxs-lookup"><span data-stu-id="49992-132">The ASP.NET Core version of SignalR is written in [TypeScript](https://www.typescriptlang.org/).</span></span> <span data-ttu-id="49992-133">Puede escribir en JavaScript o TypeScript al usar la [cliente JavaScript](xref:signalr/javascript-client).</span><span class="sxs-lookup"><span data-stu-id="49992-133">You can write in JavaScript or TypeScript when using the [JavaScript client](xref:signalr/javascript-client).</span></span>

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a><span data-ttu-id="49992-134">El cliente de JavaScript se hospeda en [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="49992-134">The JavaScript client is hosted at [npm](https://www.npmjs.com/)</span></span>

<span data-ttu-id="49992-135">En versiones anteriores, el cliente de JavaScript se obtuvo a través de un paquete de NuGet en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="49992-135">In previous versions, the JavaScript client was obtained through a NuGet package in Visual Studio.</span></span> <span data-ttu-id="49992-136">Para las versiones principales, el [ @aspnet/signalr paquete npm](https://www.npmjs.com/package/@aspnet/signalr) contiene las bibliotecas de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="49992-136">For the Core versions, the [@aspnet/signalr npm package](https://www.npmjs.com/package/@aspnet/signalr) contains the JavaScript libraries.</span></span> <span data-ttu-id="49992-137">Este paquete no se incluye en el **aplicación Web de ASP.NET Core** plantilla.</span><span class="sxs-lookup"><span data-stu-id="49992-137">This package isn't included in the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="49992-138">Use npm para obtener e instalar la `@aspnet/signalr` paquete npm.</span><span class="sxs-lookup"><span data-stu-id="49992-138">Use npm to obtain and install the `@aspnet/signalr` npm package.</span></span>

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a><span data-ttu-id="49992-139">jQuery</span><span class="sxs-lookup"><span data-stu-id="49992-139">jQuery</span></span>

<span data-ttu-id="49992-140">Se quitó la dependencia de jQuery, sin embargo, los proyectos todavía pueden usar jQuery.</span><span class="sxs-lookup"><span data-stu-id="49992-140">The dependency on jQuery has been removed, however projects can still use jQuery.</span></span>

### <a name="javascript-client-method-syntax"></a><span data-ttu-id="49992-141">Sintaxis de método de cliente de JavaScript</span><span class="sxs-lookup"><span data-stu-id="49992-141">JavaScript client method syntax</span></span>

<span data-ttu-id="49992-142">La sintaxis de JavaScript ha cambiado desde la versión anterior de SignalR.</span><span class="sxs-lookup"><span data-stu-id="49992-142">The JavaScript syntax has changed from the previous version of SignalR.</span></span> <span data-ttu-id="49992-143">En lugar de usar el `$connection` de objetos, crear una conexión mediante el `HubConnectionBuilder` API.</span><span class="sxs-lookup"><span data-stu-id="49992-143">Rather than using the `$connection` object, create a connection using the `HubConnectionBuilder` API.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

<span data-ttu-id="49992-144">Use `connection.on` para especificar los métodos de cliente que puede llamar la central.</span><span class="sxs-lookup"><span data-stu-id="49992-144">Use `connection.on` to specify client methods that the hub can call.</span></span>

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

<span data-ttu-id="49992-145">Después de crear el método de cliente, inicie la conexión de concentrador.</span><span class="sxs-lookup"><span data-stu-id="49992-145">After creating the client method, start the hub connection.</span></span> <span data-ttu-id="49992-146">Cadena de un `catch` método para iniciar sesión o controlar los errores.</span><span class="sxs-lookup"><span data-stu-id="49992-146">Chain a `catch` method to log or handle errors.</span></span>

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a><span data-ttu-id="49992-147">Servidores proxy de concentrador</span><span class="sxs-lookup"><span data-stu-id="49992-147">Hub proxies</span></span>

<span data-ttu-id="49992-148">Servidores proxy de concentrador automáticamente ya no se genera.</span><span class="sxs-lookup"><span data-stu-id="49992-148">Hub proxies are no longer automatically generated.</span></span> <span data-ttu-id="49992-149">En su lugar, se pasa el nombre del método en el `invoke` API como una cadena.</span><span class="sxs-lookup"><span data-stu-id="49992-149">Instead, the method name is passed into the `invoke` API as a string.</span></span>

### <a name="net-and-other-clients"></a><span data-ttu-id="49992-150">.NET y otros clientes</span><span class="sxs-lookup"><span data-stu-id="49992-150">.NET and other clients</span></span>

<span data-ttu-id="49992-151">El `Microsoft.AspNetCore.SignalR.Client` paquete NuGet contiene las bibliotecas de cliente .NET para ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="49992-151">The `Microsoft.AspNetCore.SignalR.Client` NuGet package contains the .NET client libraries for ASP.NET Core SignalR.</span></span>

<span data-ttu-id="49992-152">Use el `HubConnectionBuilder` para crear y generar una instancia de una conexión a un concentrador.</span><span class="sxs-lookup"><span data-stu-id="49992-152">Use the `HubConnectionBuilder` to create and build an instance of a connection to a hub.</span></span>

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="additional-resources"></a><span data-ttu-id="49992-153">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="49992-153">Additional resources</span></span>

* [<span data-ttu-id="49992-154">Concentradores</span><span class="sxs-lookup"><span data-stu-id="49992-154">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="49992-155">Cliente de JavaScript</span><span class="sxs-lookup"><span data-stu-id="49992-155">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="49992-156">Cliente .NET</span><span class="sxs-lookup"><span data-stu-id="49992-156">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="49992-157">Plataformas compatibles</span><span class="sxs-lookup"><span data-stu-id="49992-157">Supported platforms</span></span>](xref:signalr/supported-platforms)
