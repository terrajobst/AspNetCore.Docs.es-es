---
title: Diferencias entre SignalR y ASP.NET Core SignalR
author: tdykstra
description: Diferencias entre SignalR y ASP.NET Core SignalR
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.date: 09/10/2018
uid: signalr/version-differences
ms.openlocfilehash: ea2de2606a99de70fa645c0c42303525fea0a44e
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325541"
---
# <a name="differences-between-aspnet-signalr-and-aspnet-core-signalr"></a><span data-ttu-id="b2fe4-103">Diferencias entre SignalR de ASP.NET y ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="b2fe4-103">Differences between ASP.NET SignalR and ASP.NET Core SignalR</span></span>

<span data-ttu-id="b2fe4-104">ASP.NET Core SignalR no es compatible con clientes o servidores para ASP.NET SignalR.</span><span class="sxs-lookup"><span data-stu-id="b2fe4-104">ASP.NET Core SignalR isn't compatible with clients or servers for ASP.NET SignalR.</span></span> <span data-ttu-id="b2fe4-105">En este artículo se detalla las características que se han quitado o cambiado en ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="b2fe4-105">This article details features which have been removed or changed in ASP.NET Core SignalR.</span></span>

## <a name="how-to-identify-the-signalr-version"></a><span data-ttu-id="b2fe4-106">Cómo identificar la versión de SignalR</span><span class="sxs-lookup"><span data-stu-id="b2fe4-106">How to identify the SignalR version</span></span>

|                      | <span data-ttu-id="b2fe4-107">ASP.NET SignalR</span><span class="sxs-lookup"><span data-stu-id="b2fe4-107">ASP.NET SignalR</span></span> | <span data-ttu-id="b2fe4-108">SignalR de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b2fe4-108">ASP.NET Core SignalR</span></span> |
| -------------------- | --------------- | -------------------- |
| <span data-ttu-id="b2fe4-109">Paquete de NuGet de servidor</span><span class="sxs-lookup"><span data-stu-id="b2fe4-109">Server NuGet Package</span></span> | [<span data-ttu-id="b2fe4-110">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="b2fe4-110">Microsoft.AspNet.SignalR</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/) | <span data-ttu-id="b2fe4-111">[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)</span><span class="sxs-lookup"><span data-stu-id="b2fe4-111">[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)</span></span><br><span data-ttu-id="b2fe4-112">[Microsoft.AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework)</span><span class="sxs-lookup"><span data-stu-id="b2fe4-112">[Microsoft.AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework)</span></span> |
| <span data-ttu-id="b2fe4-113">Paquetes NuGet del cliente</span><span class="sxs-lookup"><span data-stu-id="b2fe4-113">Client NuGet Packages</span></span> | [<span data-ttu-id="b2fe4-114">Microsoft.AspNet.SignalR.Client</span><span class="sxs-lookup"><span data-stu-id="b2fe4-114">Microsoft.AspNet.SignalR.Client</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Client/)<br>[<span data-ttu-id="b2fe4-115">Microsoft.AspNet.SignalR.JS</span><span class="sxs-lookup"><span data-stu-id="b2fe4-115">Microsoft.AspNet.SignalR.JS</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.JS/) | [<span data-ttu-id="b2fe4-116">Microsoft.AspNetCore.SignalR.Client</span><span class="sxs-lookup"><span data-stu-id="b2fe4-116">Microsoft.AspNetCore.SignalR.Client</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/) |
| <span data-ttu-id="b2fe4-117">Paquete de npm de cliente</span><span class="sxs-lookup"><span data-stu-id="b2fe4-117">Client npm Package</span></span> | [<span data-ttu-id="b2fe4-118">signalr</span><span class="sxs-lookup"><span data-stu-id="b2fe4-118">signalr</span></span>](https://www.npmjs.com/package/signalr) | [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) |
| <span data-ttu-id="b2fe4-119">Tipo de aplicación de servidor</span><span class="sxs-lookup"><span data-stu-id="b2fe4-119">Server App Type</span></span> | <span data-ttu-id="b2fe4-120">ASP.NET (System.Web) o autohospedaje OWIN</span><span class="sxs-lookup"><span data-stu-id="b2fe4-120">ASP.NET (System.Web) or OWIN Self-Host</span></span> | <span data-ttu-id="b2fe4-121">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b2fe4-121">ASP.NET Core</span></span> |
| <span data-ttu-id="b2fe4-122">Plataformas de servidor compatibles</span><span class="sxs-lookup"><span data-stu-id="b2fe4-122">Supported Server Platforms</span></span> | <span data-ttu-id="b2fe4-123">.NET framework 4.5 o posterior</span><span class="sxs-lookup"><span data-stu-id="b2fe4-123">.NET Framework 4.5 or later</span></span> | <span data-ttu-id="b2fe4-124">.NET Framework 4.6.1 o versiones posteriores</span><span class="sxs-lookup"><span data-stu-id="b2fe4-124">.NET Framework 4.6.1 or later</span></span><br><span data-ttu-id="b2fe4-125">.NET core 2.1 o posterior</span><span class="sxs-lookup"><span data-stu-id="b2fe4-125">.NET Core 2.1 or later</span></span> |

## <a name="feature-differences"></a><span data-ttu-id="b2fe4-126">Diferencias de características</span><span class="sxs-lookup"><span data-stu-id="b2fe4-126">Feature differences</span></span>

### <a name="automatic-reconnects"></a><span data-ttu-id="b2fe4-127">Reconexiones automática</span><span class="sxs-lookup"><span data-stu-id="b2fe4-127">Automatic reconnects</span></span>

<span data-ttu-id="b2fe4-128">Reconexiones automática no se admiten en ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="b2fe4-128">Automatic reconnects aren't supported in ASP.NET Core SignalR.</span></span> <span data-ttu-id="b2fe4-129">Si el cliente está desconectado, el usuario debe iniciar explícitamente una nueva conexión si desean volver a conectar.</span><span class="sxs-lookup"><span data-stu-id="b2fe4-129">If the client is disconnected, the user must explicitly start a new connection if they want to reconnect.</span></span> <span data-ttu-id="b2fe4-130">En ASP.NET SignalR, SignalR intenta volver a conectarse al servidor si se interrumpe la conexión.</span><span class="sxs-lookup"><span data-stu-id="b2fe4-130">In ASP.NET SignalR, SignalR attempts to reconnect to the server if the connection is dropped.</span></span> 

### <a name="protocol-support"></a><span data-ttu-id="b2fe4-131">Soporte de protocolo</span><span class="sxs-lookup"><span data-stu-id="b2fe4-131">Protocol support</span></span>

<span data-ttu-id="b2fe4-132">ASP.NET Core SignalR es compatible con JSON, así como un nuevo protocolo binario según [MessagePack](xref:signalr/messagepackhubprotocol).</span><span class="sxs-lookup"><span data-stu-id="b2fe4-132">ASP.NET Core SignalR supports JSON, as well as a new binary protocol based on [MessagePack](xref:signalr/messagepackhubprotocol).</span></span> <span data-ttu-id="b2fe4-133">Además, se pueden crear protocolos personalizados.</span><span class="sxs-lookup"><span data-stu-id="b2fe4-133">Additionally, custom protocols can be created.</span></span>

## <a name="differences-on-the-server"></a><span data-ttu-id="b2fe4-134">Diferencias en el servidor</span><span class="sxs-lookup"><span data-stu-id="b2fe4-134">Differences on the server</span></span>

<span data-ttu-id="b2fe4-135">Las bibliotecas de servidor de ASP.NET Core SignalR se incluyen en el [Microsoft.AspNetCore.App metapaquete](xref:fundamentals/metapackage-app) paquete que forma parte de la **aplicación Web ASP.NET Core** plantilla de Razor y MVC proyectos.</span><span class="sxs-lookup"><span data-stu-id="b2fe4-135">The ASP.NET Core SignalR server-side libraries are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) package that's part of the **ASP.NET Core Web Application** template for both Razor and MVC projects.</span></span>

<span data-ttu-id="b2fe4-136">ASP.NET Core SignalR es un middleware de ASP.NET Core, por lo que se debe configurar mediante una llamada a [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="b2fe4-136">ASP.NET Core SignalR is an ASP.NET Core middleware, so it must be configured by calling [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span></span>

```csharp
services.AddSignalR()
```

<span data-ttu-id="b2fe4-137">Para configurar el enrutamiento, se asignan las rutas a los concentradores dentro de la [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) llame al método el `Startup.Configure` método.</span><span class="sxs-lookup"><span data-stu-id="b2fe4-137">To configure routing, map routes to hubs inside the [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) method call in the `Startup.Configure` method.</span></span>

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

### <a name="sticky-sessions-now-required"></a><span data-ttu-id="b2fe4-138">Ahora requeridas sesiones</span><span class="sxs-lookup"><span data-stu-id="b2fe4-138">Sticky sessions now required</span></span>

<span data-ttu-id="b2fe4-139">Debido a cómo escalar horizontalmente trabajado en SignalR de ASP.NET, los clientes podrían volver a conectarse y enviar mensajes a cualquier servidor de la granja de servidores.</span><span class="sxs-lookup"><span data-stu-id="b2fe4-139">Because of how scale-out worked in ASP.NET SignalR, clients could reconnect and send messages to any server in the farm.</span></span> <span data-ttu-id="b2fe4-140">Debido a cambios en el modelo de escalabilidad horizontal, así como que no admiten reconexiones, ya no se admite.</span><span class="sxs-lookup"><span data-stu-id="b2fe4-140">Due to changes to the scale-out model, as well as not supporting reconnects, this is no longer supported.</span></span> <span data-ttu-id="b2fe4-141">Una vez que el cliente se conecta al servidor, debe interactuar con el mismo servidor para la duración de la conexión.</span><span class="sxs-lookup"><span data-stu-id="b2fe4-141">Once the client connects to the server, it must interact with the same server for the duration of the connection.</span></span>

### <a name="single-hub-per-connection"></a><span data-ttu-id="b2fe4-142">Centro único por conexión</span><span class="sxs-lookup"><span data-stu-id="b2fe4-142">Single hub per connection</span></span>

<span data-ttu-id="b2fe4-143">En ASP.NET Core SignalR, se ha simplificado el modelo de conexión.</span><span class="sxs-lookup"><span data-stu-id="b2fe4-143">In ASP.NET Core SignalR, the connection model has been simplified.</span></span> <span data-ttu-id="b2fe4-144">Las conexiones se realizan directamente en un único centro, en lugar de una sola conexión que se usa para compartir el acceso a varios centros.</span><span class="sxs-lookup"><span data-stu-id="b2fe4-144">Connections are made directly to a single hub, rather than a single connection being used to share access to multiple hubs.</span></span>

### <a name="streaming"></a><span data-ttu-id="b2fe4-145">Streaming</span><span class="sxs-lookup"><span data-stu-id="b2fe4-145">Streaming</span></span>

<span data-ttu-id="b2fe4-146">ASP.NET Core SignalR ahora admite [datos de streaming](xref:signalr/streaming) desde el concentrador para el cliente.</span><span class="sxs-lookup"><span data-stu-id="b2fe4-146">ASP.NET Core SignalR now supports [streaming data](xref:signalr/streaming) from the hub to the client.</span></span>

### <a name="state"></a><span data-ttu-id="b2fe4-147">Estado</span><span class="sxs-lookup"><span data-stu-id="b2fe4-147">State</span></span>

<span data-ttu-id="b2fe4-148">Se ha quitado la capacidad de pasar información de estado arbitraria entre los clientes y el centro de (a menudo denominada HubState), así como compatibilidad con los mensajes de progreso.</span><span class="sxs-lookup"><span data-stu-id="b2fe4-148">The ability to pass arbitrary state between clients and the hub (often called HubState) has been removed, as well as support for progress messages.</span></span> <span data-ttu-id="b2fe4-149">En este momento no hay ningún homólogo de los servidores proxy de concentrador.</span><span class="sxs-lookup"><span data-stu-id="b2fe4-149">There is no counterpart of hub proxies at the moment.</span></span>

## <a name="differences-on-the-client"></a><span data-ttu-id="b2fe4-150">Diferencias en el cliente</span><span class="sxs-lookup"><span data-stu-id="b2fe4-150">Differences on the client</span></span>

### <a name="typescript"></a><span data-ttu-id="b2fe4-151">TypeScript</span><span class="sxs-lookup"><span data-stu-id="b2fe4-151">TypeScript</span></span>

<span data-ttu-id="b2fe4-152">El cliente de ASP.NET Core SignalR está escrito en [TypeScript](https://www.typescriptlang.org/).</span><span class="sxs-lookup"><span data-stu-id="b2fe4-152">The ASP.NET Core SignalR client is written in [TypeScript](https://www.typescriptlang.org/).</span></span> <span data-ttu-id="b2fe4-153">Puede escribir en JavaScript o TypeScript al usar el [cliente JavaScript](xref:signalr/javascript-client).</span><span class="sxs-lookup"><span data-stu-id="b2fe4-153">You can write in JavaScript or TypeScript when using the [JavaScript client](xref:signalr/javascript-client).</span></span>

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a><span data-ttu-id="b2fe4-154">El cliente de JavaScript está hospedado en [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="b2fe4-154">The JavaScript client is hosted at [npm](https://www.npmjs.com/)</span></span>

<span data-ttu-id="b2fe4-155">En versiones anteriores, el cliente de JavaScript se obtuvo a través de un paquete de NuGet en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b2fe4-155">In previous versions, the JavaScript client was obtained through a NuGet package in Visual Studio.</span></span> <span data-ttu-id="b2fe4-156">Para las versiones principales, el [ @aspnet/signalr ](https://www.npmjs.com/package/@aspnet/signalr) paquete npm contiene las bibliotecas de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="b2fe4-156">For the Core versions, the [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) npm package contains the JavaScript libraries.</span></span> <span data-ttu-id="b2fe4-157">Este paquete no se incluye en el **aplicación Web ASP.NET Core** plantilla.</span><span class="sxs-lookup"><span data-stu-id="b2fe4-157">This package isn't included in the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="b2fe4-158">Utilice npm para obtener e instalar el `@aspnet/signalr` paquete npm.</span><span class="sxs-lookup"><span data-stu-id="b2fe4-158">Use npm to obtain and install the `@aspnet/signalr` npm package.</span></span>

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a><span data-ttu-id="b2fe4-159">jQuery</span><span class="sxs-lookup"><span data-stu-id="b2fe4-159">jQuery</span></span>

<span data-ttu-id="b2fe4-160">Se quitó la dependencia de jQuery, sin embargo, los proyectos pueden seguir usando jQuery.</span><span class="sxs-lookup"><span data-stu-id="b2fe4-160">The dependency on jQuery has been removed, however projects can still use jQuery.</span></span>

### <a name="javascript-client-method-syntax"></a><span data-ttu-id="b2fe4-161">Sintaxis de método del cliente de JavaScript</span><span class="sxs-lookup"><span data-stu-id="b2fe4-161">JavaScript client method syntax</span></span>

<span data-ttu-id="b2fe4-162">La sintaxis de JavaScript ha cambiado desde la versión anterior de SignalR.</span><span class="sxs-lookup"><span data-stu-id="b2fe4-162">The JavaScript syntax has changed from the previous version of SignalR.</span></span> <span data-ttu-id="b2fe4-163">En lugar de usar el `$connection` , cree una conexión con el [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) API.</span><span class="sxs-lookup"><span data-stu-id="b2fe4-163">Rather than using the `$connection` object, create a connection using the [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) API.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

<span data-ttu-id="b2fe4-164">Use la [en](/javascript/api/@aspnet/signalr/HubConnection#on) método para especificar métodos de cliente que se puede llamar la central.</span><span class="sxs-lookup"><span data-stu-id="b2fe4-164">Use the [on](/javascript/api/@aspnet/signalr/HubConnection#on) method to specify client methods that the hub can call.</span></span>

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

<span data-ttu-id="b2fe4-165">Después de crear el método de cliente, inicie la conexión de concentrador.</span><span class="sxs-lookup"><span data-stu-id="b2fe4-165">After creating the client method, start the hub connection.</span></span> <span data-ttu-id="b2fe4-166">Cadena de un [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) método para iniciar o controlar los errores.</span><span class="sxs-lookup"><span data-stu-id="b2fe4-166">Chain a [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) method to log or handle errors.</span></span>

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a><span data-ttu-id="b2fe4-167">Servidores proxy de concentrador</span><span class="sxs-lookup"><span data-stu-id="b2fe4-167">Hub proxies</span></span>

<span data-ttu-id="b2fe4-168">Automáticamente ya no se generan servidores proxy de concentrador.</span><span class="sxs-lookup"><span data-stu-id="b2fe4-168">Hub proxies are no longer automatically generated.</span></span> <span data-ttu-id="b2fe4-169">En su lugar, el nombre del método se pasa a la [invocar](/javascript/api/%40aspnet/signalr/hubconnection#invoke) API como una cadena.</span><span class="sxs-lookup"><span data-stu-id="b2fe4-169">Instead, the method name is passed into the [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) API as a string.</span></span>

### <a name="net-and-other-clients"></a><span data-ttu-id="b2fe4-170">.NET y otros clientes</span><span class="sxs-lookup"><span data-stu-id="b2fe4-170">.NET and other clients</span></span>

<span data-ttu-id="b2fe4-171">El `Microsoft.AspNetCore.SignalR.Client` paquete NuGet contiene las bibliotecas de cliente .NET para ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="b2fe4-171">The `Microsoft.AspNetCore.SignalR.Client` NuGet package contains the .NET client libraries for ASP.NET Core SignalR.</span></span>

<span data-ttu-id="b2fe4-172">Use la [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) para crear y generar una instancia de una conexión a un concentrador.</span><span class="sxs-lookup"><span data-stu-id="b2fe4-172">Use the [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) to create and build an instance of a connection to a hub.</span></span>

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="scaleout-differences"></a><span data-ttu-id="b2fe4-173">Diferencias de escalabilidad horizontal</span><span class="sxs-lookup"><span data-stu-id="b2fe4-173">Scaleout differences</span></span>

<span data-ttu-id="b2fe4-174">SignalR de ASP.NET es compatible con SQL Server y Redis.</span><span class="sxs-lookup"><span data-stu-id="b2fe4-174">ASP.NET SignalR supports both SQL Server and Redis.</span></span> <span data-ttu-id="b2fe4-175">ASP.NET Core SignalR es compatible con Azure SignalR Service y Redis.</span><span class="sxs-lookup"><span data-stu-id="b2fe4-175">ASP.NET Core SignalR supports both Azure SignalR Service and Redis.</span></span>

### <a name="aspnet"></a><span data-ttu-id="b2fe4-176">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="b2fe4-176">ASP.NET</span></span>

* [<span data-ttu-id="b2fe4-177">Escalabilidad horizontal de SignalR con Azure Service Bus</span><span class="sxs-lookup"><span data-stu-id="b2fe4-177">SignalR scaleout with Azure Service Bus</span></span>](/aspnet/signalr/overview/performance/scaleout-with-windows-azure-service-bus)
* [<span data-ttu-id="b2fe4-178">Escalabilidad horizontal de SignalR con Redis</span><span class="sxs-lookup"><span data-stu-id="b2fe4-178">SignalR scaleout with Redis</span></span>](/aspnet/signalr/overview/performance/scaleout-with-redis)
* [<span data-ttu-id="b2fe4-179">Escalabilidad horizontal de SignalR con SQL Server</span><span class="sxs-lookup"><span data-stu-id="b2fe4-179">SignalR scaleout with SQL Server</span></span>](/aspnet/signalr/overview/performance/scaleout-with-sql-server)

### <a name="aspnet-core"></a><span data-ttu-id="b2fe4-180">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b2fe4-180">ASP.NET Core</span></span>

* [<span data-ttu-id="b2fe4-181">Servicio Azure SignalR</span><span class="sxs-lookup"><span data-stu-id="b2fe4-181">Azure SignalR Service</span></span>](/azure/azure-signalr/)

## <a name="additional-resources"></a><span data-ttu-id="b2fe4-182">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="b2fe4-182">Additional resources</span></span>

* [<span data-ttu-id="b2fe4-183">Concentradores</span><span class="sxs-lookup"><span data-stu-id="b2fe4-183">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="b2fe4-184">Cliente de JavaScript</span><span class="sxs-lookup"><span data-stu-id="b2fe4-184">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="b2fe4-185">Cliente .NET</span><span class="sxs-lookup"><span data-stu-id="b2fe4-185">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="b2fe4-186">Plataformas compatibles</span><span class="sxs-lookup"><span data-stu-id="b2fe4-186">Supported platforms</span></span>](xref:signalr/supported-platforms)
