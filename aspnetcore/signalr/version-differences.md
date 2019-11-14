---
title: Diferencias entre SignalR y ASP.NET Core SignalR
author: bradygaster
description: Diferencias entre SignalR y ASP.NET Core SignalR
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/version-differences
ms.openlocfilehash: 0f644c132b0fcf9a0ecf0ab181791a6477c97f76
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963719"
---
# <a name="differences-between-aspnet-opno-locsignalr-and-aspnet-core-opno-locsignalr"></a><span data-ttu-id="0ed01-103">Diferencias entre ASP.NET SignalR y ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="0ed01-103">Differences between ASP.NET SignalR and ASP.NET Core SignalR</span></span>

<span data-ttu-id="0ed01-104">ASP.NET Core SignalR no es compatible con clientes o servidores de ASP.NET SignalR.</span><span class="sxs-lookup"><span data-stu-id="0ed01-104">ASP.NET Core SignalR isn't compatible with clients or servers for ASP.NET SignalR.</span></span> <span data-ttu-id="0ed01-105">En este artículo se detallan las características que se han quitado o cambiado en ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="0ed01-105">This article details features which have been removed or changed in ASP.NET Core SignalR.</span></span>

## <a name="how-to-identify-the-opno-locsignalr-version"></a><span data-ttu-id="0ed01-106">Cómo identificar la versión de SignalR</span><span class="sxs-lookup"><span data-stu-id="0ed01-106">How to identify the SignalR version</span></span>

|                      | <span data-ttu-id="0ed01-107">SignalR ASP.NET</span><span class="sxs-lookup"><span data-stu-id="0ed01-107">ASP.NET SignalR</span></span> | <span data-ttu-id="0ed01-108">ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="0ed01-108">ASP.NET Core SignalR</span></span> |
| -------------------- | --------------- | -------------------- |
| <span data-ttu-id="0ed01-109">Paquete NuGet de servidor</span><span class="sxs-lookup"><span data-stu-id="0ed01-109">Server NuGet Package</span></span> | <span data-ttu-id="0ed01-110">[Microsoft. AspNet.SignalR](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/)</span><span class="sxs-lookup"><span data-stu-id="0ed01-110">[Microsoft.AspNet.SignalR](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/)</span></span> | <span data-ttu-id="0ed01-111">[Microsoft. AspNetCore. app](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.net Core)</span><span class="sxs-lookup"><span data-stu-id="0ed01-111">[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)</span></span><br><span data-ttu-id="0ed01-112">[Microsoft. AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/)</span><span class="sxs-lookup"><span data-stu-id="0ed01-112">[Microsoft.AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/)</span></span> <span data-ttu-id="0ed01-113">(.NET Framework)</span><span class="sxs-lookup"><span data-stu-id="0ed01-113">(.NET Framework)</span></span> |
| <span data-ttu-id="0ed01-114">Paquetes NuGet de cliente</span><span class="sxs-lookup"><span data-stu-id="0ed01-114">Client NuGet Packages</span></span> | <span data-ttu-id="0ed01-115">[Microsoft. AspNet.SignalR. Nº](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Client/)</span><span class="sxs-lookup"><span data-stu-id="0ed01-115">[Microsoft.AspNet.SignalR.Client](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Client/)</span></span><br><span data-ttu-id="0ed01-116">[Microsoft. AspNet.SignalR. ASPX](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.JS/)</span><span class="sxs-lookup"><span data-stu-id="0ed01-116">[Microsoft.AspNet.SignalR.JS](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.JS/)</span></span> | <span data-ttu-id="0ed01-117">[Microsoft. AspNetCore.SignalR. Nº](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/)</span><span class="sxs-lookup"><span data-stu-id="0ed01-117">[Microsoft.AspNetCore.SignalR.Client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/)</span></span> |
| <span data-ttu-id="0ed01-118">Paquete NPM de cliente</span><span class="sxs-lookup"><span data-stu-id="0ed01-118">Client npm Package</span></span> | [<span data-ttu-id="0ed01-119">signalr</span><span class="sxs-lookup"><span data-stu-id="0ed01-119">signalr</span></span>](https://www.npmjs.com/package/signalr) | [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) |
| <span data-ttu-id="0ed01-120">Cliente de Java</span><span class="sxs-lookup"><span data-stu-id="0ed01-120">Java Client</span></span> | <span data-ttu-id="0ed01-121">[Repositorio de github](https://github.com/SignalR/java-client) (desusado)</span><span class="sxs-lookup"><span data-stu-id="0ed01-121">[GitHub Repository](https://github.com/SignalR/java-client) (deprecated)</span></span>  | <span data-ttu-id="0ed01-122">Paquete Maven [com. Microsoft. signalr](https://search.maven.org/artifact/com.microsoft.signalr/signalr)</span><span class="sxs-lookup"><span data-stu-id="0ed01-122">Maven package [com.microsoft.signalr](https://search.maven.org/artifact/com.microsoft.signalr/signalr)</span></span> |
| <span data-ttu-id="0ed01-123">Tipo de aplicación de servidor</span><span class="sxs-lookup"><span data-stu-id="0ed01-123">Server App Type</span></span> | <span data-ttu-id="0ed01-124">ASP.NET (System. Web) o Self-host OWIN</span><span class="sxs-lookup"><span data-stu-id="0ed01-124">ASP.NET (System.Web) or OWIN Self-Host</span></span> | <span data-ttu-id="0ed01-125">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0ed01-125">ASP.NET Core</span></span> |
| <span data-ttu-id="0ed01-126">Plataformas de servidor admitidas</span><span class="sxs-lookup"><span data-stu-id="0ed01-126">Supported Server Platforms</span></span> | <span data-ttu-id="0ed01-127">.NET Framework 4,5 o posterior</span><span class="sxs-lookup"><span data-stu-id="0ed01-127">.NET Framework 4.5 or later</span></span> | <span data-ttu-id="0ed01-128">.NET Framework 4.6.1 o versiones posteriores</span><span class="sxs-lookup"><span data-stu-id="0ed01-128">.NET Framework 4.6.1 or later</span></span><br><span data-ttu-id="0ed01-129">.NET Core 2,1 o posterior</span><span class="sxs-lookup"><span data-stu-id="0ed01-129">.NET Core 2.1 or later</span></span> |

## <a name="feature-differences"></a><span data-ttu-id="0ed01-130">Diferencias de características</span><span class="sxs-lookup"><span data-stu-id="0ed01-130">Feature differences</span></span>

### <a name="automatic-reconnects"></a><span data-ttu-id="0ed01-131">Reconexiones automáticas</span><span class="sxs-lookup"><span data-stu-id="0ed01-131">Automatic reconnects</span></span>

<span data-ttu-id="0ed01-132">No se admiten las reconexiones automáticas en ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="0ed01-132">Automatic reconnects aren't supported in ASP.NET Core SignalR.</span></span> <span data-ttu-id="0ed01-133">Si el cliente está desconectado, el usuario debe iniciar explícitamente una nueva conexión si desea volver a conectarse.</span><span class="sxs-lookup"><span data-stu-id="0ed01-133">If the client is disconnected, the user must explicitly start a new connection if they want to reconnect.</span></span> <span data-ttu-id="0ed01-134">En ASP.NET SignalR, SignalR intenta volver a conectarse al servidor si se quita la conexión.</span><span class="sxs-lookup"><span data-stu-id="0ed01-134">In ASP.NET SignalR, SignalR attempts to reconnect to the server if the connection is dropped.</span></span>

### <a name="protocol-support"></a><span data-ttu-id="0ed01-135">Compatibilidad con protocolos</span><span class="sxs-lookup"><span data-stu-id="0ed01-135">Protocol support</span></span>

<span data-ttu-id="0ed01-136">ASP.NET Core SignalR admite JSON, así como un nuevo protocolo binario basado en [MessagePack](xref:signalr/messagepackhubprotocol).</span><span class="sxs-lookup"><span data-stu-id="0ed01-136">ASP.NET Core SignalR supports JSON, as well as a new binary protocol based on [MessagePack](xref:signalr/messagepackhubprotocol).</span></span> <span data-ttu-id="0ed01-137">Además, se pueden crear protocolos personalizados.</span><span class="sxs-lookup"><span data-stu-id="0ed01-137">Additionally, custom protocols can be created.</span></span>

### <a name="transports"></a><span data-ttu-id="0ed01-138">Transportes</span><span class="sxs-lookup"><span data-stu-id="0ed01-138">Transports</span></span>

<span data-ttu-id="0ed01-139">El transporte de tramas Forever no se admite en ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="0ed01-139">The Forever Frame transport is not supported in ASP.NET Core SignalR.</span></span>

## <a name="differences-on-the-server"></a><span data-ttu-id="0ed01-140">Diferencias en el servidor</span><span class="sxs-lookup"><span data-stu-id="0ed01-140">Differences on the server</span></span>

<span data-ttu-id="0ed01-141">Las bibliotecas del lado del servidor de ASP.NET Core SignalR se incluyen en el paquete de [metapaquete Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) que forma parte de la plantilla de **aplicación Web de ASP.net Core** para proyectos de Razor y MVC.</span><span class="sxs-lookup"><span data-stu-id="0ed01-141">The ASP.NET Core SignalR server-side libraries are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) package that's part of the **ASP.NET Core Web Application** template for both Razor and MVC projects.</span></span>

<span data-ttu-id="0ed01-142">ASP.NET Core SignalR es un middleware de ASP.NET Core, por lo que debe configurarse llamando a [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="0ed01-142">ASP.NET Core SignalR is an ASP.NET Core middleware, so it must be configured by calling [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span></span>

```csharp
services.AddSignalR()
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="0ed01-143">Para configurar el enrutamiento, asigne rutas a los concentradores dentro de la llamada al método [UseEndpoints](/dotnet/api/microsoft.aspnetcore.builder.endpointroutingapplicationbuilderextensions.useendpoints) en el método `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="0ed01-143">To configure routing, map routes to hubs inside the [UseEndpoints](/dotnet/api/microsoft.aspnetcore.builder.endpointroutingapplicationbuilderextensions.useendpoints) method call in the `Startup.Configure` method.</span></span>


```csharp
app.UseRouting();

app.UseEndpoints(endpoints =>
{
    endpoints.MapHub<ChatHub>("/hub");
});
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="0ed01-144">Para configurar el enrutamiento, asigne rutas a los concentradores dentro de la llamada al método [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) en el método `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="0ed01-144">To configure routing, map routes to hubs inside the [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) method call in the `Startup.Configure` method.</span></span>

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

::: moniker-end

### <a name="sticky-sessions"></a><span data-ttu-id="0ed01-145">Sesiones permanentes</span><span class="sxs-lookup"><span data-stu-id="0ed01-145">Sticky sessions</span></span>

<span data-ttu-id="0ed01-146">El modelo ampliación para ASP.NET SignalR permite a los clientes volver a conectarse y enviar mensajes a cualquier servidor de la granja.</span><span class="sxs-lookup"><span data-stu-id="0ed01-146">The scaleout model for ASP.NET SignalR allows clients to reconnect and send messages to any server in the farm.</span></span> <span data-ttu-id="0ed01-147">En ASP.NET Core SignalR, el cliente debe interactuar con el mismo servidor mientras dure la conexión.</span><span class="sxs-lookup"><span data-stu-id="0ed01-147">In ASP.NET Core SignalR, the client must interact with the same server for the duration of the connection.</span></span> <span data-ttu-id="0ed01-148">En el caso de ampliación con Redis, eso significa que se requieren sesiones permanentes.</span><span class="sxs-lookup"><span data-stu-id="0ed01-148">For scaleout using Redis, that means sticky sessions are required.</span></span> <span data-ttu-id="0ed01-149">En el caso de ampliación con el [servicio Azure SignalR](/azure/azure-signalr/), no se requieren sesiones permanentes porque el servicio administra las conexiones a los clientes.</span><span class="sxs-lookup"><span data-stu-id="0ed01-149">For scaleout using [Azure SignalR Service](/azure/azure-signalr/), sticky sessions are not required because the service handles connections to clients.</span></span>

### <a name="single-hub-per-connection"></a><span data-ttu-id="0ed01-150">Un solo concentrador por conexión</span><span class="sxs-lookup"><span data-stu-id="0ed01-150">Single hub per connection</span></span>

<span data-ttu-id="0ed01-151">En ASP.NET Core SignalR, se ha simplificado el modelo de conexión.</span><span class="sxs-lookup"><span data-stu-id="0ed01-151">In ASP.NET Core SignalR, the connection model has been simplified.</span></span> <span data-ttu-id="0ed01-152">Las conexiones se realizan directamente en un solo concentrador, en lugar de usar una sola conexión para compartir el acceso a varios centros.</span><span class="sxs-lookup"><span data-stu-id="0ed01-152">Connections are made directly to a single hub, rather than a single connection being used to share access to multiple hubs.</span></span>

### <a name="streaming"></a><span data-ttu-id="0ed01-153">Streaming</span><span class="sxs-lookup"><span data-stu-id="0ed01-153">Streaming</span></span>

<span data-ttu-id="0ed01-154">ASP.NET Core SignalR ahora admite el [streaming de datos](xref:signalr/streaming) del concentrador al cliente.</span><span class="sxs-lookup"><span data-stu-id="0ed01-154">ASP.NET Core SignalR now supports [streaming data](xref:signalr/streaming) from the hub to the client.</span></span>

### <a name="state"></a><span data-ttu-id="0ed01-155">Estado</span><span class="sxs-lookup"><span data-stu-id="0ed01-155">State</span></span>

<span data-ttu-id="0ed01-156">Se ha quitado la capacidad de pasar el estado arbitrario entre los clientes y el concentrador (a menudo denominado HubState), así como la compatibilidad con los mensajes de progreso.</span><span class="sxs-lookup"><span data-stu-id="0ed01-156">The ability to pass arbitrary state between clients and the hub (often called HubState) has been removed, as well as support for progress messages.</span></span> <span data-ttu-id="0ed01-157">En este momento no hay ningún homólogo de servidores proxy de concentrador.</span><span class="sxs-lookup"><span data-stu-id="0ed01-157">There is no counterpart of hub proxies at the moment.</span></span>

### <a name="persistentconnection-removal"></a><span data-ttu-id="0ed01-158">Eliminación de PersistentConnection</span><span class="sxs-lookup"><span data-stu-id="0ed01-158">PersistentConnection removal</span></span>

<span data-ttu-id="0ed01-159">En ASP.NET Core SignalR, se ha quitado la clase [PersistentConnection](https://docs.microsoft.com/previous-versions/aspnet/jj919047(v%3dvs.118)) .</span><span class="sxs-lookup"><span data-stu-id="0ed01-159">In ASP.NET Core SignalR, the [PersistentConnection](https://docs.microsoft.com/previous-versions/aspnet/jj919047(v%3dvs.118)) class has been removed.</span></span>

### <a name="globalhost"></a><span data-ttu-id="0ed01-160">Host global</span><span class="sxs-lookup"><span data-stu-id="0ed01-160">GlobalHost</span></span>

<span data-ttu-id="0ed01-161">ASP.NET Core tiene la inserción de dependencias (DI) integrada en el marco de trabajo.</span><span class="sxs-lookup"><span data-stu-id="0ed01-161">ASP.NET Core has dependency injection (DI) built into the framework.</span></span> <span data-ttu-id="0ed01-162">Los servicios pueden usar DI para acceder a [HubContext](xref:signalr/hubcontext).</span><span class="sxs-lookup"><span data-stu-id="0ed01-162">Services can use DI to access the [HubContext](xref:signalr/hubcontext).</span></span> <span data-ttu-id="0ed01-163">El objeto `GlobalHost` que se usa en el SignalR ASP.NET para obtener una `HubContext` no existe en ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="0ed01-163">The `GlobalHost` object that is used in ASP.NET SignalR to get a `HubContext` doesn't exist in ASP.NET Core SignalR.</span></span>

### <a name="hubpipeline"></a><span data-ttu-id="0ed01-164">HubPipeline</span><span class="sxs-lookup"><span data-stu-id="0ed01-164">HubPipeline</span></span>

<span data-ttu-id="0ed01-165">ASP.NET Core SignalR no es compatible con los módulos de `HubPipeline`.</span><span class="sxs-lookup"><span data-stu-id="0ed01-165">ASP.NET Core SignalR doesn't have support for `HubPipeline` modules.</span></span>

## <a name="differences-on-the-client"></a><span data-ttu-id="0ed01-166">Diferencias en el cliente</span><span class="sxs-lookup"><span data-stu-id="0ed01-166">Differences on the client</span></span>

### <a name="typescript"></a><span data-ttu-id="0ed01-167">TypeScript</span><span class="sxs-lookup"><span data-stu-id="0ed01-167">TypeScript</span></span>

<span data-ttu-id="0ed01-168">El cliente de SignalR de ASP.NET Core está escrito en [TypeScript](https://www.typescriptlang.org/).</span><span class="sxs-lookup"><span data-stu-id="0ed01-168">The ASP.NET Core SignalR client is written in [TypeScript](https://www.typescriptlang.org/).</span></span> <span data-ttu-id="0ed01-169">Puede escribir en JavaScript o TypeScript cuando se usa el [cliente de JavaScript](xref:signalr/javascript-client).</span><span class="sxs-lookup"><span data-stu-id="0ed01-169">You can write in JavaScript or TypeScript when using the [JavaScript client](xref:signalr/javascript-client).</span></span>

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a><span data-ttu-id="0ed01-170">El cliente de JavaScript se hospeda en [NPM](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="0ed01-170">The JavaScript client is hosted at [npm](https://www.npmjs.com/)</span></span>

<span data-ttu-id="0ed01-171">En versiones anteriores, el cliente de JavaScript se obtuvo a través de un paquete NuGet en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0ed01-171">In previous versions, the JavaScript client was obtained through a NuGet package in Visual Studio.</span></span> <span data-ttu-id="0ed01-172">En el caso de las versiones principales, el paquete de [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) NPM contiene las bibliotecas de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="0ed01-172">For the Core versions, the [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) npm package contains the JavaScript libraries.</span></span> <span data-ttu-id="0ed01-173">Este paquete no está incluido en la plantilla de **aplicación Web de ASP.net Core** .</span><span class="sxs-lookup"><span data-stu-id="0ed01-173">This package isn't included in the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="0ed01-174">Use NPM para obtener e instalar el paquete de `@aspnet/signalr` NPM.</span><span class="sxs-lookup"><span data-stu-id="0ed01-174">Use npm to obtain and install the `@aspnet/signalr` npm package.</span></span>

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a><span data-ttu-id="0ed01-175">jQuery</span><span class="sxs-lookup"><span data-stu-id="0ed01-175">jQuery</span></span>

<span data-ttu-id="0ed01-176">La dependencia de jQuery se ha quitado, pero los proyectos todavía pueden usar jQuery.</span><span class="sxs-lookup"><span data-stu-id="0ed01-176">The dependency on jQuery has been removed, however projects can still use jQuery.</span></span>

### <a name="internet-explorer-support"></a><span data-ttu-id="0ed01-177">Compatibilidad con Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="0ed01-177">Internet Explorer support</span></span>

<span data-ttu-id="0ed01-178">ASP.NET Core SignalR requiere Microsoft Internet Explorer 11 o posterior (ASP.NET SignalR compatible con Microsoft Internet Explorer 8 y versiones posteriores).</span><span class="sxs-lookup"><span data-stu-id="0ed01-178">ASP.NET Core SignalR requires Microsoft Internet Explorer 11 or later (ASP.NET SignalR supported Microsoft Internet Explorer 8 and later).</span></span>

### <a name="javascript-client-method-syntax"></a><span data-ttu-id="0ed01-179">Sintaxis del método de cliente JavaScript</span><span class="sxs-lookup"><span data-stu-id="0ed01-179">JavaScript client method syntax</span></span>

<span data-ttu-id="0ed01-180">La sintaxis de JavaScript ha cambiado respecto a la versión anterior de SignalR.</span><span class="sxs-lookup"><span data-stu-id="0ed01-180">The JavaScript syntax has changed from the previous version of SignalR.</span></span> <span data-ttu-id="0ed01-181">En lugar de usar el objeto de `$connection`, cree una conexión mediante la API de [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) .</span><span class="sxs-lookup"><span data-stu-id="0ed01-181">Rather than using the `$connection` object, create a connection using the [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) API.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

<span data-ttu-id="0ed01-182">Use el método on para especificar los métodos [de](/javascript/api/@aspnet/signalr/HubConnection#on) cliente a los que puede llamar el concentrador.</span><span class="sxs-lookup"><span data-stu-id="0ed01-182">Use the [on](/javascript/api/@aspnet/signalr/HubConnection#on) method to specify client methods that the hub can call.</span></span>

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

<span data-ttu-id="0ed01-183">Después de crear el método de cliente, inicie la conexión del concentrador.</span><span class="sxs-lookup"><span data-stu-id="0ed01-183">After creating the client method, start the hub connection.</span></span> <span data-ttu-id="0ed01-184">Encadenar un método [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) para registrar o controlar los errores.</span><span class="sxs-lookup"><span data-stu-id="0ed01-184">Chain a [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) method to log or handle errors.</span></span>

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a><span data-ttu-id="0ed01-185">Servidores proxy de concentrador</span><span class="sxs-lookup"><span data-stu-id="0ed01-185">Hub proxies</span></span>

<span data-ttu-id="0ed01-186">Los proxies de concentrador ya no se generan automáticamente.</span><span class="sxs-lookup"><span data-stu-id="0ed01-186">Hub proxies are no longer automatically generated.</span></span> <span data-ttu-id="0ed01-187">En su lugar, el nombre del método se pasa a la API de [invocación](/javascript/api/%40aspnet/signalr/hubconnection#invoke) como una cadena.</span><span class="sxs-lookup"><span data-stu-id="0ed01-187">Instead, the method name is passed into the [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) API as a string.</span></span>

### <a name="net-and-other-clients"></a><span data-ttu-id="0ed01-188">.NET y otros clientes</span><span class="sxs-lookup"><span data-stu-id="0ed01-188">.NET and other clients</span></span>

<span data-ttu-id="0ed01-189">El paquete NuGet `Microsoft.AspNetCore.SignalR.Client` contiene las bibliotecas de cliente de .NET para ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="0ed01-189">The `Microsoft.AspNetCore.SignalR.Client` NuGet package contains the .NET client libraries for ASP.NET Core SignalR.</span></span>

<span data-ttu-id="0ed01-190">Use [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) para crear y compilar una instancia de una conexión a un concentrador.</span><span class="sxs-lookup"><span data-stu-id="0ed01-190">Use the [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) to create and build an instance of a connection to a hub.</span></span>

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="scaleout-differences"></a><span data-ttu-id="0ed01-191">Diferencias de ampliación</span><span class="sxs-lookup"><span data-stu-id="0ed01-191">Scaleout differences</span></span>

<span data-ttu-id="0ed01-192">ASP.NET SignalR admite SQL Server y Redis.</span><span class="sxs-lookup"><span data-stu-id="0ed01-192">ASP.NET SignalR supports SQL Server and Redis.</span></span> <span data-ttu-id="0ed01-193">ASP.NET Core SignalR admite el servicio Azure SignalR y Redis.</span><span class="sxs-lookup"><span data-stu-id="0ed01-193">ASP.NET Core SignalR supports Azure SignalR Service and Redis.</span></span>

### <a name="aspnet"></a><span data-ttu-id="0ed01-194">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="0ed01-194">ASP.NET</span></span>

* <span data-ttu-id="0ed01-195">[SignalR ampliación con Azure Service Bus](/aspnet/signalr/overview/performance/scaleout-with-windows-azure-service-bus)</span><span class="sxs-lookup"><span data-stu-id="0ed01-195">[SignalR scaleout with Azure Service Bus](/aspnet/signalr/overview/performance/scaleout-with-windows-azure-service-bus)</span></span>
* <span data-ttu-id="0ed01-196">[SignalR ampliación con Redis](/aspnet/signalr/overview/performance/scaleout-with-redis)</span><span class="sxs-lookup"><span data-stu-id="0ed01-196">[SignalR scaleout with Redis](/aspnet/signalr/overview/performance/scaleout-with-redis)</span></span>
* <span data-ttu-id="0ed01-197">[SignalR ampliación con SQL Server](/aspnet/signalr/overview/performance/scaleout-with-sql-server)</span><span class="sxs-lookup"><span data-stu-id="0ed01-197">[SignalR scaleout with SQL Server](/aspnet/signalr/overview/performance/scaleout-with-sql-server)</span></span>

### <a name="aspnet-core"></a><span data-ttu-id="0ed01-198">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0ed01-198">ASP.NET Core</span></span>

* <span data-ttu-id="0ed01-199">[Servicio Azure SignalR](/azure/azure-signalr/)</span><span class="sxs-lookup"><span data-stu-id="0ed01-199">[Azure SignalR Service](/azure/azure-signalr/)</span></span>
* [<span data-ttu-id="0ed01-200">Backplane de Redis</span><span class="sxs-lookup"><span data-stu-id="0ed01-200">Redis Backplane</span></span>](xref:signalr/redis-backplane)

## <a name="additional-resources"></a><span data-ttu-id="0ed01-201">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="0ed01-201">Additional resources</span></span>

* [<span data-ttu-id="0ed01-202">Concentradores</span><span class="sxs-lookup"><span data-stu-id="0ed01-202">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="0ed01-203">Cliente de JavaScript</span><span class="sxs-lookup"><span data-stu-id="0ed01-203">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="0ed01-204">Cliente .NET</span><span class="sxs-lookup"><span data-stu-id="0ed01-204">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="0ed01-205">Plataformas compatibles</span><span class="sxs-lookup"><span data-stu-id="0ed01-205">Supported platforms</span></span>](xref:signalr/supported-platforms)
