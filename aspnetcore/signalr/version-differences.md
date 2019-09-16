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
# <a name="differences-between-aspnet-signalr-and-aspnet-core-signalr"></a><span data-ttu-id="ae40d-103">Diferencias entre ASP.NET Signalr y ASP.NET Core Signalr</span><span class="sxs-lookup"><span data-stu-id="ae40d-103">Differences between ASP.NET SignalR and ASP.NET Core SignalR</span></span>

<span data-ttu-id="ae40d-104">ASP.NET Core Signalr no es compatible con clientes o servidores para ASP.NET Signalr.</span><span class="sxs-lookup"><span data-stu-id="ae40d-104">ASP.NET Core SignalR isn't compatible with clients or servers for ASP.NET SignalR.</span></span> <span data-ttu-id="ae40d-105">En este artículo se detallan las características que se han quitado o cambiado en ASP.NET Core Signalr.</span><span class="sxs-lookup"><span data-stu-id="ae40d-105">This article details features which have been removed or changed in ASP.NET Core SignalR.</span></span>

## <a name="how-to-identify-the-signalr-version"></a><span data-ttu-id="ae40d-106">Identificación de la versión de Signalr</span><span class="sxs-lookup"><span data-stu-id="ae40d-106">How to identify the SignalR version</span></span>

|                      | <span data-ttu-id="ae40d-107">ASP.NET SignalR</span><span class="sxs-lookup"><span data-stu-id="ae40d-107">ASP.NET SignalR</span></span> | <span data-ttu-id="ae40d-108">SignalR de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ae40d-108">ASP.NET Core SignalR</span></span> |
| -------------------- | --------------- | -------------------- |
| <span data-ttu-id="ae40d-109">Paquete NuGet de servidor</span><span class="sxs-lookup"><span data-stu-id="ae40d-109">Server NuGet Package</span></span> | [<span data-ttu-id="ae40d-110">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="ae40d-110">Microsoft.AspNet.SignalR</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/) | <span data-ttu-id="ae40d-111">[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)</span><span class="sxs-lookup"><span data-stu-id="ae40d-111">[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)</span></span><br><span data-ttu-id="ae40d-112">[Microsoft. AspNetCore. signalr](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework)</span><span class="sxs-lookup"><span data-stu-id="ae40d-112">[Microsoft.AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework)</span></span> |
| <span data-ttu-id="ae40d-113">Paquetes NuGet de cliente</span><span class="sxs-lookup"><span data-stu-id="ae40d-113">Client NuGet Packages</span></span> | [<span data-ttu-id="ae40d-114">Microsoft.AspNet.SignalR.Client</span><span class="sxs-lookup"><span data-stu-id="ae40d-114">Microsoft.AspNet.SignalR.Client</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Client/)<br>[<span data-ttu-id="ae40d-115">Microsoft.AspNet.SignalR.JS</span><span class="sxs-lookup"><span data-stu-id="ae40d-115">Microsoft.AspNet.SignalR.JS</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.JS/) | [<span data-ttu-id="ae40d-116">Microsoft.AspNetCore.SignalR.Client</span><span class="sxs-lookup"><span data-stu-id="ae40d-116">Microsoft.AspNetCore.SignalR.Client</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/) |
| <span data-ttu-id="ae40d-117">Paquete NPM de cliente</span><span class="sxs-lookup"><span data-stu-id="ae40d-117">Client npm Package</span></span> | [<span data-ttu-id="ae40d-118">signalr</span><span class="sxs-lookup"><span data-stu-id="ae40d-118">signalr</span></span>](https://www.npmjs.com/package/signalr) | [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) |
| <span data-ttu-id="ae40d-119">Cliente de Java</span><span class="sxs-lookup"><span data-stu-id="ae40d-119">Java Client</span></span> | <span data-ttu-id="ae40d-120">[Repositorio de github](https://github.com/SignalR/java-client) en desuso</span><span class="sxs-lookup"><span data-stu-id="ae40d-120">[GitHub Repository](https://github.com/SignalR/java-client) (deprecated)</span></span>  | <span data-ttu-id="ae40d-121">Paquete Maven [com. Microsoft. signalr](https://search.maven.org/artifact/com.microsoft.signalr/signalr)</span><span class="sxs-lookup"><span data-stu-id="ae40d-121">Maven package [com.microsoft.signalr](https://search.maven.org/artifact/com.microsoft.signalr/signalr)</span></span> |
| <span data-ttu-id="ae40d-122">Tipo de aplicación de servidor</span><span class="sxs-lookup"><span data-stu-id="ae40d-122">Server App Type</span></span> | <span data-ttu-id="ae40d-123">ASP.NET (System. Web) o Self-host OWIN</span><span class="sxs-lookup"><span data-stu-id="ae40d-123">ASP.NET (System.Web) or OWIN Self-Host</span></span> | <span data-ttu-id="ae40d-124">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ae40d-124">ASP.NET Core</span></span> |
| <span data-ttu-id="ae40d-125">Plataformas de servidor admitidas</span><span class="sxs-lookup"><span data-stu-id="ae40d-125">Supported Server Platforms</span></span> | <span data-ttu-id="ae40d-126">.NET Framework 4,5 o posterior</span><span class="sxs-lookup"><span data-stu-id="ae40d-126">.NET Framework 4.5 or later</span></span> | <span data-ttu-id="ae40d-127">.NET Framework 4.6.1 o versiones posteriores</span><span class="sxs-lookup"><span data-stu-id="ae40d-127">.NET Framework 4.6.1 or later</span></span><br><span data-ttu-id="ae40d-128">.NET Core 2,1 o posterior</span><span class="sxs-lookup"><span data-stu-id="ae40d-128">.NET Core 2.1 or later</span></span> |

## <a name="feature-differences"></a><span data-ttu-id="ae40d-129">Diferencias de características</span><span class="sxs-lookup"><span data-stu-id="ae40d-129">Feature differences</span></span>

### <a name="automatic-reconnects"></a><span data-ttu-id="ae40d-130">Reconexiones automáticas</span><span class="sxs-lookup"><span data-stu-id="ae40d-130">Automatic reconnects</span></span>

<span data-ttu-id="ae40d-131">No se admiten las reconexiones automáticas en ASP.NET Core Signalr.</span><span class="sxs-lookup"><span data-stu-id="ae40d-131">Automatic reconnects aren't supported in ASP.NET Core SignalR.</span></span> <span data-ttu-id="ae40d-132">Si el cliente está desconectado, el usuario debe iniciar explícitamente una nueva conexión si desea volver a conectarse.</span><span class="sxs-lookup"><span data-stu-id="ae40d-132">If the client is disconnected, the user must explicitly start a new connection if they want to reconnect.</span></span> <span data-ttu-id="ae40d-133">En ASP.NET Signalr, Signalr intenta volver a conectarse al servidor si se quita la conexión.</span><span class="sxs-lookup"><span data-stu-id="ae40d-133">In ASP.NET SignalR, SignalR attempts to reconnect to the server if the connection is dropped.</span></span>

### <a name="protocol-support"></a><span data-ttu-id="ae40d-134">Compatibilidad con protocolos</span><span class="sxs-lookup"><span data-stu-id="ae40d-134">Protocol support</span></span>

<span data-ttu-id="ae40d-135">ASP.NET Core Signalr admite JSON, así como un nuevo protocolo binario basado en [MessagePack](xref:signalr/messagepackhubprotocol).</span><span class="sxs-lookup"><span data-stu-id="ae40d-135">ASP.NET Core SignalR supports JSON, as well as a new binary protocol based on [MessagePack](xref:signalr/messagepackhubprotocol).</span></span> <span data-ttu-id="ae40d-136">Además, se pueden crear protocolos personalizados.</span><span class="sxs-lookup"><span data-stu-id="ae40d-136">Additionally, custom protocols can be created.</span></span>

### <a name="transports"></a><span data-ttu-id="ae40d-137">Transportes</span><span class="sxs-lookup"><span data-stu-id="ae40d-137">Transports</span></span>

<span data-ttu-id="ae40d-138">El transporte de tramas de siempre no se admite en ASP.NET Core Signalr.</span><span class="sxs-lookup"><span data-stu-id="ae40d-138">The Forever Frame transport is not supported in ASP.NET Core SignalR.</span></span>

## <a name="differences-on-the-server"></a><span data-ttu-id="ae40d-139">Diferencias en el servidor</span><span class="sxs-lookup"><span data-stu-id="ae40d-139">Differences on the server</span></span>

<span data-ttu-id="ae40d-140">Las bibliotecas del lado servidor de Signalr ASP.NET Core se incluyen en el paquete de [metapaquete Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) que forma parte de la plantilla de **aplicación Web ASP.net Core** para proyectos de Razor y MVC.</span><span class="sxs-lookup"><span data-stu-id="ae40d-140">The ASP.NET Core SignalR server-side libraries are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) package that's part of the **ASP.NET Core Web Application** template for both Razor and MVC projects.</span></span>

<span data-ttu-id="ae40d-141">ASP.NET Core Signalr es un middleware ASP.NET Core, por lo que debe configurarse llamando a [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="ae40d-141">ASP.NET Core SignalR is an ASP.NET Core middleware, so it must be configured by calling [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span></span>

```csharp
services.AddSignalR()
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="ae40d-142">Para configurar el enrutamiento, asigne rutas a los concentradores dentro de la llamada `Startup.Configure` al método [UseEndpoints](/dotnet/api/microsoft.aspnetcore.builder.endpointroutingapplicationbuilderextensions.useendpoints) en el método.</span><span class="sxs-lookup"><span data-stu-id="ae40d-142">To configure routing, map routes to hubs inside the [UseEndpoints](/dotnet/api/microsoft.aspnetcore.builder.endpointroutingapplicationbuilderextensions.useendpoints) method call in the `Startup.Configure` method.</span></span>


```csharp
app.UseRouting();

app.UseEndpoints(endpoints =>
{
    endpoints.MapHub<ChatHub>("/hub");
});
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="ae40d-143">Para configurar el enrutamiento, asigne rutas a los concentradores dentro de la llamada `Startup.Configure` al método [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) en el método.</span><span class="sxs-lookup"><span data-stu-id="ae40d-143">To configure routing, map routes to hubs inside the [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) method call in the `Startup.Configure` method.</span></span>

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

::: moniker-end

### <a name="sticky-sessions"></a><span data-ttu-id="ae40d-144">Sesiones permanentes</span><span class="sxs-lookup"><span data-stu-id="ae40d-144">Sticky sessions</span></span>

<span data-ttu-id="ae40d-145">El modelo ampliación para ASP.NET Signalr permite a los clientes volver a conectarse y enviar mensajes a cualquier servidor de la granja.</span><span class="sxs-lookup"><span data-stu-id="ae40d-145">The scaleout model for ASP.NET SignalR allows clients to reconnect and send messages to any server in the farm.</span></span> <span data-ttu-id="ae40d-146">En ASP.NET Core Signalr, el cliente debe interactuar con el mismo servidor mientras dure la conexión.</span><span class="sxs-lookup"><span data-stu-id="ae40d-146">In ASP.NET Core SignalR, the client must interact with the same server for the duration of the connection.</span></span> <span data-ttu-id="ae40d-147">En el caso de ampliación con Redis, eso significa que se requieren sesiones permanentes.</span><span class="sxs-lookup"><span data-stu-id="ae40d-147">For scaleout using Redis, that means sticky sessions are required.</span></span> <span data-ttu-id="ae40d-148">En el caso de ampliación con [Azure signalr Service](/azure/azure-signalr/), las sesiones permanentes no son necesarias porque el servicio administra las conexiones a los clientes.</span><span class="sxs-lookup"><span data-stu-id="ae40d-148">For scaleout using [Azure SignalR Service](/azure/azure-signalr/), sticky sessions are not required because the service handles connections to clients.</span></span>

### <a name="single-hub-per-connection"></a><span data-ttu-id="ae40d-149">Un solo concentrador por conexión</span><span class="sxs-lookup"><span data-stu-id="ae40d-149">Single hub per connection</span></span>

<span data-ttu-id="ae40d-150">En ASP.NET Core Signalr, se ha simplificado el modelo de conexión.</span><span class="sxs-lookup"><span data-stu-id="ae40d-150">In ASP.NET Core SignalR, the connection model has been simplified.</span></span> <span data-ttu-id="ae40d-151">Las conexiones se realizan directamente en un solo concentrador, en lugar de usar una sola conexión para compartir el acceso a varios centros.</span><span class="sxs-lookup"><span data-stu-id="ae40d-151">Connections are made directly to a single hub, rather than a single connection being used to share access to multiple hubs.</span></span>

### <a name="streaming"></a><span data-ttu-id="ae40d-152">Streaming</span><span class="sxs-lookup"><span data-stu-id="ae40d-152">Streaming</span></span>

<span data-ttu-id="ae40d-153">ASP.NET Core Signalr ahora admite el [streaming de datos](xref:signalr/streaming) desde el concentrador al cliente.</span><span class="sxs-lookup"><span data-stu-id="ae40d-153">ASP.NET Core SignalR now supports [streaming data](xref:signalr/streaming) from the hub to the client.</span></span>

### <a name="state"></a><span data-ttu-id="ae40d-154">Estado</span><span class="sxs-lookup"><span data-stu-id="ae40d-154">State</span></span>

<span data-ttu-id="ae40d-155">Se ha quitado la capacidad de pasar el estado arbitrario entre los clientes y el concentrador (a menudo denominado HubState), así como la compatibilidad con los mensajes de progreso.</span><span class="sxs-lookup"><span data-stu-id="ae40d-155">The ability to pass arbitrary state between clients and the hub (often called HubState) has been removed, as well as support for progress messages.</span></span> <span data-ttu-id="ae40d-156">En este momento no hay ningún homólogo de servidores proxy de concentrador.</span><span class="sxs-lookup"><span data-stu-id="ae40d-156">There is no counterpart of hub proxies at the moment.</span></span>

### <a name="persistentconnection-removal"></a><span data-ttu-id="ae40d-157">Eliminación de PersistentConnection</span><span class="sxs-lookup"><span data-stu-id="ae40d-157">PersistentConnection removal</span></span>

<span data-ttu-id="ae40d-158">En ASP.NET Core Signalr, se ha quitado la clase [PersistentConnection](https://docs.microsoft.com/previous-versions/aspnet/jj919047(v%3dvs.118)) .</span><span class="sxs-lookup"><span data-stu-id="ae40d-158">In ASP.NET Core SignalR, the [PersistentConnection](https://docs.microsoft.com/previous-versions/aspnet/jj919047(v%3dvs.118)) class has been removed.</span></span>

### <a name="globalhost"></a><span data-ttu-id="ae40d-159">Host global</span><span class="sxs-lookup"><span data-stu-id="ae40d-159">GlobalHost</span></span>

<span data-ttu-id="ae40d-160">ASP.NET Core tiene la inserción de dependencias (DI) integrada en el marco de trabajo.</span><span class="sxs-lookup"><span data-stu-id="ae40d-160">ASP.NET Core has dependency injection (DI) built into the framework.</span></span> <span data-ttu-id="ae40d-161">Los servicios pueden usar DI para acceder a [HubContext](xref:signalr/hubcontext).</span><span class="sxs-lookup"><span data-stu-id="ae40d-161">Services can use DI to access the [HubContext](xref:signalr/hubcontext).</span></span> <span data-ttu-id="ae40d-162">El `GlobalHost` objeto que se usa en ASP.net signalr para `HubContext` obtener no existe en ASP.net Core signalr.</span><span class="sxs-lookup"><span data-stu-id="ae40d-162">The `GlobalHost` object that is used in ASP.NET SignalR to get a `HubContext` doesn't exist in ASP.NET Core SignalR.</span></span>

### <a name="hubpipeline"></a><span data-ttu-id="ae40d-163">HubPipeline</span><span class="sxs-lookup"><span data-stu-id="ae40d-163">HubPipeline</span></span>

<span data-ttu-id="ae40d-164">ASP.net Core signalr no admite `HubPipeline` módulos.</span><span class="sxs-lookup"><span data-stu-id="ae40d-164">ASP.NET Core SignalR doesn't have support for `HubPipeline` modules.</span></span>

## <a name="differences-on-the-client"></a><span data-ttu-id="ae40d-165">Diferencias en el cliente</span><span class="sxs-lookup"><span data-stu-id="ae40d-165">Differences on the client</span></span>

### <a name="typescript"></a><span data-ttu-id="ae40d-166">TypeScript</span><span class="sxs-lookup"><span data-stu-id="ae40d-166">TypeScript</span></span>

<span data-ttu-id="ae40d-167">El cliente de Signalr ASP.NET Core está escrito en [TypeScript](https://www.typescriptlang.org/).</span><span class="sxs-lookup"><span data-stu-id="ae40d-167">The ASP.NET Core SignalR client is written in [TypeScript](https://www.typescriptlang.org/).</span></span> <span data-ttu-id="ae40d-168">Puede escribir en JavaScript o TypeScript cuando se usa el [cliente de JavaScript](xref:signalr/javascript-client).</span><span class="sxs-lookup"><span data-stu-id="ae40d-168">You can write in JavaScript or TypeScript when using the [JavaScript client](xref:signalr/javascript-client).</span></span>

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a><span data-ttu-id="ae40d-169">El cliente de JavaScript se hospeda en [NPM](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="ae40d-169">The JavaScript client is hosted at [npm](https://www.npmjs.com/)</span></span>

<span data-ttu-id="ae40d-170">En versiones anteriores, el cliente de JavaScript se obtuvo a través de un paquete NuGet en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ae40d-170">In previous versions, the JavaScript client was obtained through a NuGet package in Visual Studio.</span></span> <span data-ttu-id="ae40d-171">En el caso de las versiones [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) principales, el paquete NPM contiene las bibliotecas de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ae40d-171">For the Core versions, the [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) npm package contains the JavaScript libraries.</span></span> <span data-ttu-id="ae40d-172">Este paquete no está incluido en la plantilla de **aplicación Web de ASP.net Core** .</span><span class="sxs-lookup"><span data-stu-id="ae40d-172">This package isn't included in the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="ae40d-173">Use NPM para obtener e instalar el `@aspnet/signalr` paquete NPM.</span><span class="sxs-lookup"><span data-stu-id="ae40d-173">Use npm to obtain and install the `@aspnet/signalr` npm package.</span></span>

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a><span data-ttu-id="ae40d-174">jQuery</span><span class="sxs-lookup"><span data-stu-id="ae40d-174">jQuery</span></span>

<span data-ttu-id="ae40d-175">La dependencia de jQuery se ha quitado, pero los proyectos todavía pueden usar jQuery.</span><span class="sxs-lookup"><span data-stu-id="ae40d-175">The dependency on jQuery has been removed, however projects can still use jQuery.</span></span>

### <a name="internet-explorer-support"></a><span data-ttu-id="ae40d-176">Compatibilidad con Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="ae40d-176">Internet Explorer support</span></span>

<span data-ttu-id="ae40d-177">ASP.NET Core Signalr requiere Microsoft Internet Explorer 11 o posterior (ASP.NET Signalr es compatible con Microsoft Internet Explorer 8 y versiones posteriores).</span><span class="sxs-lookup"><span data-stu-id="ae40d-177">ASP.NET Core SignalR requires Microsoft Internet Explorer 11 or later (ASP.NET SignalR supported Microsoft Internet Explorer 8 and later).</span></span>

### <a name="javascript-client-method-syntax"></a><span data-ttu-id="ae40d-178">Sintaxis del método de cliente JavaScript</span><span class="sxs-lookup"><span data-stu-id="ae40d-178">JavaScript client method syntax</span></span>

<span data-ttu-id="ae40d-179">La sintaxis de JavaScript ha cambiado respecto a la versión anterior de Signalr.</span><span class="sxs-lookup"><span data-stu-id="ae40d-179">The JavaScript syntax has changed from the previous version of SignalR.</span></span> <span data-ttu-id="ae40d-180">En lugar de usar `$connection` el objeto, cree una conexión mediante la API de [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) .</span><span class="sxs-lookup"><span data-stu-id="ae40d-180">Rather than using the `$connection` object, create a connection using the [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) API.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

<span data-ttu-id="ae40d-181">Use el método on para especificar los métodos [de](/javascript/api/@aspnet/signalr/HubConnection#on) cliente a los que puede llamar el concentrador.</span><span class="sxs-lookup"><span data-stu-id="ae40d-181">Use the [on](/javascript/api/@aspnet/signalr/HubConnection#on) method to specify client methods that the hub can call.</span></span>

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

<span data-ttu-id="ae40d-182">Después de crear el método de cliente, inicie la conexión del concentrador.</span><span class="sxs-lookup"><span data-stu-id="ae40d-182">After creating the client method, start the hub connection.</span></span> <span data-ttu-id="ae40d-183">Encadenar un método [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) para registrar o controlar los errores.</span><span class="sxs-lookup"><span data-stu-id="ae40d-183">Chain a [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) method to log or handle errors.</span></span>

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a><span data-ttu-id="ae40d-184">Servidores proxy de concentrador</span><span class="sxs-lookup"><span data-stu-id="ae40d-184">Hub proxies</span></span>

<span data-ttu-id="ae40d-185">Los proxies de concentrador ya no se generan automáticamente.</span><span class="sxs-lookup"><span data-stu-id="ae40d-185">Hub proxies are no longer automatically generated.</span></span> <span data-ttu-id="ae40d-186">En su lugar, el nombre del método se pasa a la API de [invocación](/javascript/api/%40aspnet/signalr/hubconnection#invoke) como una cadena.</span><span class="sxs-lookup"><span data-stu-id="ae40d-186">Instead, the method name is passed into the [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) API as a string.</span></span>

### <a name="net-and-other-clients"></a><span data-ttu-id="ae40d-187">.NET y otros clientes</span><span class="sxs-lookup"><span data-stu-id="ae40d-187">.NET and other clients</span></span>

<span data-ttu-id="ae40d-188">El `Microsoft.AspNetCore.SignalR.Client` paquete NuGet contiene las bibliotecas de cliente de .net para ASP.net Core signalr.</span><span class="sxs-lookup"><span data-stu-id="ae40d-188">The `Microsoft.AspNetCore.SignalR.Client` NuGet package contains the .NET client libraries for ASP.NET Core SignalR.</span></span>

<span data-ttu-id="ae40d-189">Use [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) para crear y compilar una instancia de una conexión a un concentrador.</span><span class="sxs-lookup"><span data-stu-id="ae40d-189">Use the [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) to create and build an instance of a connection to a hub.</span></span>

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="scaleout-differences"></a><span data-ttu-id="ae40d-190">Diferencias de ampliación</span><span class="sxs-lookup"><span data-stu-id="ae40d-190">Scaleout differences</span></span>

<span data-ttu-id="ae40d-191">ASP.NET Signalr admite SQL Server y Redis.</span><span class="sxs-lookup"><span data-stu-id="ae40d-191">ASP.NET SignalR supports SQL Server and Redis.</span></span> <span data-ttu-id="ae40d-192">ASP.NET Core Signalr es compatible con Azure Signalr Service y Redis.</span><span class="sxs-lookup"><span data-stu-id="ae40d-192">ASP.NET Core SignalR supports Azure SignalR Service and Redis.</span></span>

### <a name="aspnet"></a><span data-ttu-id="ae40d-193">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ae40d-193">ASP.NET</span></span>

* [<span data-ttu-id="ae40d-194">Signalr ampliación con Azure Service Bus</span><span class="sxs-lookup"><span data-stu-id="ae40d-194">SignalR scaleout with Azure Service Bus</span></span>](/aspnet/signalr/overview/performance/scaleout-with-windows-azure-service-bus)
* [<span data-ttu-id="ae40d-195">Signalr ampliación con Redis</span><span class="sxs-lookup"><span data-stu-id="ae40d-195">SignalR scaleout with Redis</span></span>](/aspnet/signalr/overview/performance/scaleout-with-redis)
* [<span data-ttu-id="ae40d-196">Signalr ampliación con SQL Server</span><span class="sxs-lookup"><span data-stu-id="ae40d-196">SignalR scaleout with SQL Server</span></span>](/aspnet/signalr/overview/performance/scaleout-with-sql-server)

### <a name="aspnet-core"></a><span data-ttu-id="ae40d-197">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ae40d-197">ASP.NET Core</span></span>

* [<span data-ttu-id="ae40d-198">Servicio Azure SignalR</span><span class="sxs-lookup"><span data-stu-id="ae40d-198">Azure SignalR Service</span></span>](/azure/azure-signalr/)
* [<span data-ttu-id="ae40d-199">Backplane de Redis</span><span class="sxs-lookup"><span data-stu-id="ae40d-199">Redis Backplane</span></span>](xref:signalr/redis-backplane)

## <a name="additional-resources"></a><span data-ttu-id="ae40d-200">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="ae40d-200">Additional resources</span></span>

* [<span data-ttu-id="ae40d-201">Concentradores</span><span class="sxs-lookup"><span data-stu-id="ae40d-201">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="ae40d-202">Cliente de JavaScript</span><span class="sxs-lookup"><span data-stu-id="ae40d-202">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="ae40d-203">Cliente .NET</span><span class="sxs-lookup"><span data-stu-id="ae40d-203">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="ae40d-204">Plataformas compatibles</span><span class="sxs-lookup"><span data-stu-id="ae40d-204">Supported platforms</span></span>](xref:signalr/supported-platforms)
