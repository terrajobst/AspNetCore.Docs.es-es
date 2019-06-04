---
title: Configuración de ASP.NET Core SignalR
author: bradygaster
description: Obtenga información sobre cómo configurar aplicaciones de ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 06/03/2019
uid: signalr/configuration
ms.openlocfilehash: 6c7bd602e621917c491bfb1e26ff0fcfc3a565b0
ms.sourcegitcommit: a04eb20e81243930ec829a9db5dd5de49f669450
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/03/2019
ms.locfileid: "66470369"
---
# <a name="aspnet-core-signalr-configuration"></a><span data-ttu-id="b5b37-103">Configuración de ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="b5b37-103">ASP.NET Core SignalR configuration</span></span>

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="b5b37-104">Opciones de serialización de JSON/MessagePack</span><span class="sxs-lookup"><span data-stu-id="b5b37-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="b5b37-105">SignalR de ASP.NET Core admite dos protocolos para la codificación de mensajes: [JSON](https://www.json.org/) y [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="b5b37-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="b5b37-106">Cada protocolo tiene opciones de configuración de serialización.</span><span class="sxs-lookup"><span data-stu-id="b5b37-106">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="b5b37-107">Serialización de JSON se puede configurar en el servidor mediante el [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) método de extensión, que se puede agregar después [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) en su `Startup.ConfigureServices` método.</span><span class="sxs-lookup"><span data-stu-id="b5b37-107">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="b5b37-108">El `AddJsonProtocol` método toma un delegado que recibe un `options` objeto.</span><span class="sxs-lookup"><span data-stu-id="b5b37-108">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="b5b37-109">El [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) propiedad en ese objeto es un JSON.NET `JsonSerializerSettings` objeto que puede usarse para configurar la serialización de argumentos y valores devueltos.</span><span class="sxs-lookup"><span data-stu-id="b5b37-109">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="b5b37-110">Consulte la [documentación JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm) para obtener más detalles.</span><span class="sxs-lookup"><span data-stu-id="b5b37-110">See the [JSON.NET Documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm) for more details.</span></span>

<span data-ttu-id="b5b37-111">Por ejemplo, para configurar el serializador para usar nombres de propiedad "PascalCase", en lugar de los nombres predeterminados "camelCase", use el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="b5b37-111">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code:</span></span>

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
        new DefaultContractResolver();
    });
```

<span data-ttu-id="b5b37-112">En el cliente. NET, la misma `AddJsonProtocol` existe en el método de extensión [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="b5b37-112">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="b5b37-113">El `Microsoft.Extensions.DependencyInjection` se debe importar el espacio de nombres para resolver el método de extensión:</span><span class="sxs-lookup"><span data-stu-id="b5b37-113">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

```csharp
// At the top of the file:
using Microsoft.Extensions.DependencyInjection;

// When constructing your connection:
var connection = new HubConnectionBuilder()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
            new DefaultContractResolver();
    })
    .Build();
```

> [!NOTE]
> <span data-ttu-id="b5b37-114">No es posible configurar la serialización de JSON en el cliente de JavaScript en este momento.</span><span class="sxs-lookup"><span data-stu-id="b5b37-114">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="b5b37-115">Opciones de serialización MessagePack</span><span class="sxs-lookup"><span data-stu-id="b5b37-115">MessagePack serialization options</span></span>

<span data-ttu-id="b5b37-116">MessagePack serialización se puede configurar si se proporciona un delegado para el [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) llamar.</span><span class="sxs-lookup"><span data-stu-id="b5b37-116">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="b5b37-117">Consulte [MessagePack en SignalR](xref:signalr/messagepackhubprotocol) para obtener más detalles.</span><span class="sxs-lookup"><span data-stu-id="b5b37-117">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="b5b37-118">No es posible configurar la serialización de MessagePack en el cliente de JavaScript en este momento.</span><span class="sxs-lookup"><span data-stu-id="b5b37-118">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="b5b37-119">Configurar opciones de servidor</span><span class="sxs-lookup"><span data-stu-id="b5b37-119">Configure server options</span></span>

<span data-ttu-id="b5b37-120">En la tabla siguiente se describe las opciones para configurar los concentradores de SignalR:</span><span class="sxs-lookup"><span data-stu-id="b5b37-120">The following table describes options for configuring SignalR hubs:</span></span>

::: moniker range=">= aspnetcore-3.0"

| <span data-ttu-id="b5b37-121">Opción</span><span class="sxs-lookup"><span data-stu-id="b5b37-121">Option</span></span> | <span data-ttu-id="b5b37-122">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="b5b37-122">Default Value</span></span> | <span data-ttu-id="b5b37-123">Descripción</span><span class="sxs-lookup"><span data-stu-id="b5b37-123">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="b5b37-124">30 segundos</span><span class="sxs-lookup"><span data-stu-id="b5b37-124">30 seconds</span></span> | <span data-ttu-id="b5b37-125">El servidor tendrá en cuenta el cliente desconectado si no ha recibido un mensaje (incluido keep-alive) en este intervalo.</span><span class="sxs-lookup"><span data-stu-id="b5b37-125">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="b5b37-126">Puede tardar más de este intervalo de tiempo de espera para el cliente realmente marcarse desconectado debido a su implementación.</span><span class="sxs-lookup"><span data-stu-id="b5b37-126">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="b5b37-127">El valor recomendado es doble el `KeepAliveInterval` valor.</span><span class="sxs-lookup"><span data-stu-id="b5b37-127">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="b5b37-128">15 segundos</span><span class="sxs-lookup"><span data-stu-id="b5b37-128">15 seconds</span></span> | <span data-ttu-id="b5b37-129">Si el cliente no envía un mensaje de protocolo de enlace inicial dentro de este intervalo de tiempo, se cierra la conexión.</span><span class="sxs-lookup"><span data-stu-id="b5b37-129">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="b5b37-130">Se trata de una opción avanzada que sólo debería modificarse si se producen errores de tiempo de espera del protocolo de enlace debido a la latencia de red graves.</span><span class="sxs-lookup"><span data-stu-id="b5b37-130">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="b5b37-131">Para obtener más detalles sobre el proceso de negociación, consulte el [especificación del protocolo SignalR Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="b5b37-131">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="b5b37-132">15 segundos</span><span class="sxs-lookup"><span data-stu-id="b5b37-132">15 seconds</span></span> | <span data-ttu-id="b5b37-133">Si el servidor no ha enviado un mensaje dentro de este intervalo, se envía automáticamente un mensaje ping para mantener abierta la conexión.</span><span class="sxs-lookup"><span data-stu-id="b5b37-133">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="b5b37-134">Al cambiar `KeepAliveInterval`, cambie el `ServerTimeout` / `serverTimeoutInMilliseconds` configuración del cliente.</span><span class="sxs-lookup"><span data-stu-id="b5b37-134">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="b5b37-135">Recomendado `ServerTimeout` / `serverTimeoutInMilliseconds` valor es doble el `KeepAliveInterval` valor.</span><span class="sxs-lookup"><span data-stu-id="b5b37-135">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="b5b37-136">Todos los protocolos instalados</span><span class="sxs-lookup"><span data-stu-id="b5b37-136">All installed protocols</span></span> | <span data-ttu-id="b5b37-137">Protocolos admitidos por este concentrador.</span><span class="sxs-lookup"><span data-stu-id="b5b37-137">Protocols supported by this hub.</span></span> <span data-ttu-id="b5b37-138">De forma predeterminada, se permiten todos los protocolos registrados en el servidor, pero se pueden quitar protocolos de esta lista para deshabilitar los protocolos específicos para los concentradores individuales.</span><span class="sxs-lookup"><span data-stu-id="b5b37-138">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="b5b37-139">Si `true`detallados se devuelven los mensajes de excepción a los clientes cuando se produce una excepción en un método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="b5b37-139">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="b5b37-140">El valor predeterminado es `false`, ya que estos mensajes de excepción pueden contener información confidencial.</span><span class="sxs-lookup"><span data-stu-id="b5b37-140">The default is `false`, as these exception messages can contain sensitive information.</span></span> |
| `StreamBufferCapacity` | `10` | <span data-ttu-id="b5b37-141">El número máximo de elementos que pueden almacenarse en búfer para el cliente cargue secuencias.</span><span class="sxs-lookup"><span data-stu-id="b5b37-141">The maximum number of items that can be buffered for client upload streams.</span></span> <span data-ttu-id="b5b37-142">Si se alcanza este límite, el procesamiento de las llamadas se bloquea hasta que el servidor procesa los elementos de la secuencia.</span><span class="sxs-lookup"><span data-stu-id="b5b37-142">If this limit is reached, the processing of invocations is blocked until the server processes stream items.</span></span>|

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

| <span data-ttu-id="b5b37-143">Opción</span><span class="sxs-lookup"><span data-stu-id="b5b37-143">Option</span></span> | <span data-ttu-id="b5b37-144">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="b5b37-144">Default Value</span></span> | <span data-ttu-id="b5b37-145">Descripción</span><span class="sxs-lookup"><span data-stu-id="b5b37-145">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="b5b37-146">30 segundos</span><span class="sxs-lookup"><span data-stu-id="b5b37-146">30 seconds</span></span> | <span data-ttu-id="b5b37-147">El servidor tendrá en cuenta el cliente desconectado si no ha recibido un mensaje (incluido keep-alive) en este intervalo.</span><span class="sxs-lookup"><span data-stu-id="b5b37-147">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="b5b37-148">Puede tardar más de este intervalo de tiempo de espera para el cliente realmente marcarse desconectado debido a su implementación.</span><span class="sxs-lookup"><span data-stu-id="b5b37-148">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="b5b37-149">El valor recomendado es doble el `KeepAliveInterval` valor.</span><span class="sxs-lookup"><span data-stu-id="b5b37-149">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="b5b37-150">15 segundos</span><span class="sxs-lookup"><span data-stu-id="b5b37-150">15 seconds</span></span> | <span data-ttu-id="b5b37-151">Si el cliente no envía un mensaje de protocolo de enlace inicial dentro de este intervalo de tiempo, se cierra la conexión.</span><span class="sxs-lookup"><span data-stu-id="b5b37-151">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="b5b37-152">Se trata de una opción avanzada que sólo debería modificarse si se producen errores de tiempo de espera del protocolo de enlace debido a la latencia de red graves.</span><span class="sxs-lookup"><span data-stu-id="b5b37-152">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="b5b37-153">Para obtener más detalles sobre el proceso de negociación, consulte el [especificación del protocolo SignalR Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="b5b37-153">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="b5b37-154">15 segundos</span><span class="sxs-lookup"><span data-stu-id="b5b37-154">15 seconds</span></span> | <span data-ttu-id="b5b37-155">Si el servidor no ha enviado un mensaje dentro de este intervalo, se envía automáticamente un mensaje ping para mantener abierta la conexión.</span><span class="sxs-lookup"><span data-stu-id="b5b37-155">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="b5b37-156">Al cambiar `KeepAliveInterval`, cambie el `ServerTimeout` / `serverTimeoutInMilliseconds` configuración del cliente.</span><span class="sxs-lookup"><span data-stu-id="b5b37-156">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="b5b37-157">Recomendado `ServerTimeout` / `serverTimeoutInMilliseconds` valor es doble el `KeepAliveInterval` valor.</span><span class="sxs-lookup"><span data-stu-id="b5b37-157">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="b5b37-158">Todos los protocolos instalados</span><span class="sxs-lookup"><span data-stu-id="b5b37-158">All installed protocols</span></span> | <span data-ttu-id="b5b37-159">Protocolos admitidos por este concentrador.</span><span class="sxs-lookup"><span data-stu-id="b5b37-159">Protocols supported by this hub.</span></span> <span data-ttu-id="b5b37-160">De forma predeterminada, se permiten todos los protocolos registrados en el servidor, pero se pueden quitar protocolos de esta lista para deshabilitar los protocolos específicos para los concentradores individuales.</span><span class="sxs-lookup"><span data-stu-id="b5b37-160">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="b5b37-161">Si `true`detallados se devuelven los mensajes de excepción a los clientes cuando se produce una excepción en un método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="b5b37-161">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="b5b37-162">El valor predeterminado es `false`, ya que estos mensajes de excepción pueden contener información confidencial.</span><span class="sxs-lookup"><span data-stu-id="b5b37-162">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

::: moniker-end

<span data-ttu-id="b5b37-163">Se pueden configurar opciones para todos los centros proporcionando un delegado de opciones para la `AddSignalR` llamar `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="b5b37-163">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSignalR(hubOptions =>
    {
        hubOptions.EnableDetailedErrors = true;
        hubOptions.KeepAliveInterval = TimeSpan.FromMinutes(1);
    });
}
```

<span data-ttu-id="b5b37-164">Opciones para un único centro de invalidan las opciones globales de `AddSignalR` y pueden configurarse mediante <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span><span class="sxs-lookup"><span data-stu-id="b5b37-164">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="b5b37-165">Opciones de configuración de HTTP avanzadas</span><span class="sxs-lookup"><span data-stu-id="b5b37-165">Advanced HTTP configuration options</span></span>

<span data-ttu-id="b5b37-166">Use `HttpConnectionDispatcherOptions` para configurar opciones avanzadas relacionadas con transportes y administración de búfer de memoria.</span><span class="sxs-lookup"><span data-stu-id="b5b37-166">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="b5b37-167">Estas opciones se configuran pasando un delegado para [MapHub\<T >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) en `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="b5b37-167">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) in `Startup.Configure`.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseSignalR((configure) =>
    {
        var desiredTransports =
            HttpTransportType.WebSockets |
            HttpTransportType.LongPolling;

        configure.MapHub<MyHub>("/myhub", (options) =>
        {
            options.Transports = desiredTransports;
        });
    });
}
```

<span data-ttu-id="b5b37-168">En la tabla siguiente se describe las opciones para configurar las opciones avanzadas de HTTP de ASP.NET Core SignalR:</span><span class="sxs-lookup"><span data-stu-id="b5b37-168">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

| <span data-ttu-id="b5b37-169">Opción</span><span class="sxs-lookup"><span data-stu-id="b5b37-169">Option</span></span> | <span data-ttu-id="b5b37-170">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="b5b37-170">Default Value</span></span> | <span data-ttu-id="b5b37-171">Descripción</span><span class="sxs-lookup"><span data-stu-id="b5b37-171">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="b5b37-172">32 KB</span><span class="sxs-lookup"><span data-stu-id="b5b37-172">32 KB</span></span> | <span data-ttu-id="b5b37-173">El número máximo de bytes recibidos del cliente que los búferes del servidor.</span><span class="sxs-lookup"><span data-stu-id="b5b37-173">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="b5b37-174">Al aumentar este valor permite que el servidor recibir los mensajes más grandes, pero puede repercutir negativamente en el consumo de memoria.</span><span class="sxs-lookup"><span data-stu-id="b5b37-174">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="b5b37-175">Recopilar automáticamente los datos de la `Authorize` atributos aplicados a la clase de concentrador.</span><span class="sxs-lookup"><span data-stu-id="b5b37-175">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="b5b37-176">Una lista de [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) los objetos utilizados para determinar si un cliente está autorizado para conectarse al concentrador.</span><span class="sxs-lookup"><span data-stu-id="b5b37-176">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="b5b37-177">32 KB</span><span class="sxs-lookup"><span data-stu-id="b5b37-177">32 KB</span></span> | <span data-ttu-id="b5b37-178">El número máximo de bytes enviados por la aplicación que los búferes del servidor.</span><span class="sxs-lookup"><span data-stu-id="b5b37-178">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="b5b37-179">Al aumentar este valor permite que el servidor enviar los mensajes más grandes, pero puede repercutir negativamente en el consumo de memoria.</span><span class="sxs-lookup"><span data-stu-id="b5b37-179">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="b5b37-180">Se habilitan todos los transportes.</span><span class="sxs-lookup"><span data-stu-id="b5b37-180">All Transports are enabled.</span></span> | <span data-ttu-id="b5b37-181">Una máscara de bits de `HttpTransportType` valores que pueden restringir los transportes que un cliente puede usar para conectarse.</span><span class="sxs-lookup"><span data-stu-id="b5b37-181">A bitmask of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="b5b37-182">Véalo a continuación.</span><span class="sxs-lookup"><span data-stu-id="b5b37-182">See below.</span></span> | <span data-ttu-id="b5b37-183">Opciones adicionales específicas para el transporte de sondeo largo.</span><span class="sxs-lookup"><span data-stu-id="b5b37-183">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="b5b37-184">Véalo a continuación.</span><span class="sxs-lookup"><span data-stu-id="b5b37-184">See below.</span></span> | <span data-ttu-id="b5b37-185">Opciones adicionales específicas para el transporte de WebSockets.</span><span class="sxs-lookup"><span data-stu-id="b5b37-185">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="b5b37-186">El transporte de sondeo largo tiene opciones adicionales que se pueden configurar mediante el `LongPolling` propiedad:</span><span class="sxs-lookup"><span data-stu-id="b5b37-186">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="b5b37-187">Opción</span><span class="sxs-lookup"><span data-stu-id="b5b37-187">Option</span></span> | <span data-ttu-id="b5b37-188">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="b5b37-188">Default Value</span></span> | <span data-ttu-id="b5b37-189">Descripción</span><span class="sxs-lookup"><span data-stu-id="b5b37-189">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="b5b37-190">90 segundos</span><span class="sxs-lookup"><span data-stu-id="b5b37-190">90 seconds</span></span> | <span data-ttu-id="b5b37-191">La cantidad máxima de tiempo que el servidor espera para que un mensaje para enviárselo al cliente antes de finalizar una solicitud de sondeo única.</span><span class="sxs-lookup"><span data-stu-id="b5b37-191">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="b5b37-192">Al disminuir este valor hace que el cliente emitir solicitudes de sondeo de nuevo con más frecuencia.</span><span class="sxs-lookup"><span data-stu-id="b5b37-192">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="b5b37-193">El transporte de WebSocket tiene opciones adicionales que se pueden configurar mediante el `WebSockets` propiedad:</span><span class="sxs-lookup"><span data-stu-id="b5b37-193">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="b5b37-194">Opción</span><span class="sxs-lookup"><span data-stu-id="b5b37-194">Option</span></span> | <span data-ttu-id="b5b37-195">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="b5b37-195">Default Value</span></span> | <span data-ttu-id="b5b37-196">Descripción</span><span class="sxs-lookup"><span data-stu-id="b5b37-196">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="b5b37-197">5 segundos</span><span class="sxs-lookup"><span data-stu-id="b5b37-197">5 seconds</span></span> | <span data-ttu-id="b5b37-198">Después de cerrar el servidor, si se produce un error en el cliente cerrar dentro de este intervalo de tiempo, se termina la conexión.</span><span class="sxs-lookup"><span data-stu-id="b5b37-198">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="b5b37-199">Un delegado que puede usarse para establecer el `Sec-WebSocket-Protocol` encabezado en un valor personalizado.</span><span class="sxs-lookup"><span data-stu-id="b5b37-199">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="b5b37-200">El delegado recibe los valores solicitados por el cliente como entrada y se espera que devuelva el valor deseado.</span><span class="sxs-lookup"><span data-stu-id="b5b37-200">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="b5b37-201">Configurar las opciones de cliente</span><span class="sxs-lookup"><span data-stu-id="b5b37-201">Configure client options</span></span>

<span data-ttu-id="b5b37-202">Se pueden configurar las opciones de cliente en el `HubConnectionBuilder` tipo (disponible en los clientes de .NET y JavaScript).</span><span class="sxs-lookup"><span data-stu-id="b5b37-202">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="b5b37-203">También está disponible en el cliente de Java, pero la `HttpHubConnectionBuilder` subclase es lo que contiene las opciones de configuración de generador, así como en el `HubConnection` propio.</span><span class="sxs-lookup"><span data-stu-id="b5b37-203">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="b5b37-204">Configurar el registro</span><span class="sxs-lookup"><span data-stu-id="b5b37-204">Configure logging</span></span>

<span data-ttu-id="b5b37-205">El registro está configurado en el cliente de .NET mediante el `ConfigureLogging` método.</span><span class="sxs-lookup"><span data-stu-id="b5b37-205">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="b5b37-206">Registro de proveedores y los filtros se puede registrar en la misma manera, tal como están en el servidor.</span><span class="sxs-lookup"><span data-stu-id="b5b37-206">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="b5b37-207">Consulte la [registro en ASP.NET Core](xref:fundamentals/logging/index) documentación para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="b5b37-207">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="b5b37-208">Con el fin de registrar los proveedores de registro, debe instalar los paquetes necesarios.</span><span class="sxs-lookup"><span data-stu-id="b5b37-208">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="b5b37-209">Consulte la [proveedores de registro integrados](xref:fundamentals/logging/index#built-in-logging-providers) sección de la documentación para obtener una lista completa.</span><span class="sxs-lookup"><span data-stu-id="b5b37-209">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="b5b37-210">Por ejemplo, para habilitar el registro de consola, instale el `Microsoft.Extensions.Logging.Console` paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="b5b37-210">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="b5b37-211">Llame a la `AddConsole` método de extensión:</span><span class="sxs-lookup"><span data-stu-id="b5b37-211">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="b5b37-212">En el cliente de JavaScript, un proceso similar `configureLogging` método existe.</span><span class="sxs-lookup"><span data-stu-id="b5b37-212">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="b5b37-213">Proporcione un `LogLevel` valor que indica el nivel mínimo de los mensajes de registro para generar.</span><span class="sxs-lookup"><span data-stu-id="b5b37-213">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="b5b37-214">Los registros se escriben en la ventana de consola del explorador.</span><span class="sxs-lookup"><span data-stu-id="b5b37-214">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="b5b37-215">En lugar de un `LogLevel` valor, también puede proporcionar un `string` valor que representa un nombre de nivel de registro.</span><span class="sxs-lookup"><span data-stu-id="b5b37-215">Instead of a `LogLevel` value, you can also provide a `string` value representing a log level name.</span></span> <span data-ttu-id="b5b37-216">Esto es útil al configurar SignalR registro en entornos donde no tiene acceso a la `LogLevel` constantes.</span><span class="sxs-lookup"><span data-stu-id="b5b37-216">This is useful when configuring SignalR logging in environments where you don't have access to the `LogLevel` constants.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging("warn")
    .build();
```

<span data-ttu-id="b5b37-217">En la tabla siguiente se enumera los niveles de registro disponibles.</span><span class="sxs-lookup"><span data-stu-id="b5b37-217">The following table lists the available log levels.</span></span> <span data-ttu-id="b5b37-218">El valor que proporcione a `configureLogging` establece la **mínimo** nivel que se registrarán de registro.</span><span class="sxs-lookup"><span data-stu-id="b5b37-218">The value you provide to `configureLogging` sets the **minimum** log level that will be logged.</span></span> <span data-ttu-id="b5b37-219">Los mensajes registrados en este nivel, **o los niveles se muestran después de él en la tabla**, se registrarán.</span><span class="sxs-lookup"><span data-stu-id="b5b37-219">Messages logged at this level, **or the levels listed after it in the table**, will be logged.</span></span>

| <span data-ttu-id="b5b37-220">cadena</span><span class="sxs-lookup"><span data-stu-id="b5b37-220">string</span></span> | <span data-ttu-id="b5b37-221">LogLevel</span><span class="sxs-lookup"><span data-stu-id="b5b37-221">LogLevel</span></span> |
| - | - |
| `"trace"` | `LogLevel.Trace` |
| `"debug"` | `LogLevel.Debug` |
| <span data-ttu-id="b5b37-222">`"info"` **O** `"information"`</span><span class="sxs-lookup"><span data-stu-id="b5b37-222">`"info"` **or** `"information"`</span></span> | `LogLevel.Information` |
| <span data-ttu-id="b5b37-223">`"warn"` **O** `"warning"`</span><span class="sxs-lookup"><span data-stu-id="b5b37-223">`"warn"` **or** `"warning"`</span></span> | `LogLevel.Warning` |
| `"error"` | `LogLevel.Error` |
| `"critical"` | `LogLevel.Critical` |
| `"none"` | `LogLevel.None` |

::: moniker-end

> [!NOTE]
> <span data-ttu-id="b5b37-224">Para deshabilitar el registro por completo, especifique `signalR.LogLevel.None` en el `configureLogging` método.</span><span class="sxs-lookup"><span data-stu-id="b5b37-224">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="b5b37-225">Para obtener más información sobre el registro, consulte el [documentación de diagnóstico de SignalR](xref:signalr/diagnostics).</span><span class="sxs-lookup"><span data-stu-id="b5b37-225">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="b5b37-226">El cliente de SignalR Java usa el [SLF4J](https://www.slf4j.org/) biblioteca para el registro.</span><span class="sxs-lookup"><span data-stu-id="b5b37-226">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="b5b37-227">Es una API de alto nivel de registro que permite a los usuarios de la biblioteca elegir su propia implementación de registro específico al incorporar una dependencia de registro específico.</span><span class="sxs-lookup"><span data-stu-id="b5b37-227">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="b5b37-228">El fragmento de código siguiente muestra cómo usar `java.util.logging` con el cliente de SignalR Java.</span><span class="sxs-lookup"><span data-stu-id="b5b37-228">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="b5b37-229">Si no configura el registro de las dependencias, SLF4J carga un registrador de ninguna operación de forma predeterminada con el mensaje de advertencia siguiente:</span><span class="sxs-lookup"><span data-stu-id="b5b37-229">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="b5b37-230">Esto puede pasar por alto.</span><span class="sxs-lookup"><span data-stu-id="b5b37-230">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="b5b37-231">Configurar transportes permitidos</span><span class="sxs-lookup"><span data-stu-id="b5b37-231">Configure allowed transports</span></span>

<span data-ttu-id="b5b37-232">Los transportes utilizados por SignalR se pueden configurar en el `WithUrl` llamar (`withUrl` en JavaScript).</span><span class="sxs-lookup"><span data-stu-id="b5b37-232">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="b5b37-233">Una operación OR bit a bit de los valores de `HttpTransportType` puede usarse para restringir el cliente para que solo use los transportes especificados.</span><span class="sxs-lookup"><span data-stu-id="b5b37-233">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="b5b37-234">Todos los transportes están habilitados de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="b5b37-234">All transports are enabled by default.</span></span>

<span data-ttu-id="b5b37-235">Por ejemplo, para deshabilitar el transporte de los eventos, pero permitir conexiones WebSockets y Long Polling:</span><span class="sxs-lookup"><span data-stu-id="b5b37-235">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="b5b37-236">En el cliente de JavaScript, se configuran los transportes estableciendo el `transport` campo en el objeto de opciones proporcionado para `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="b5b37-236">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="b5b37-237">En esta versión de Java websockets de cliente es el transporte solo está disponible.</span><span class="sxs-lookup"><span data-stu-id="b5b37-237">In this version of the Java client websockets is the only available transport.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-3.0"

<span data-ttu-id="b5b37-238">En el cliente de Java, el transporte está activado con la `withTransport` método en el `HttpHubConnectionBuilder`.</span><span class="sxs-lookup"><span data-stu-id="b5b37-238">In the Java client, the transport is selected with the `withTransport` method on the `HttpHubConnectionBuilder`.</span></span> <span data-ttu-id="b5b37-239">De forma predeterminada, el cliente de Java que usa el transporte de WebSockets.</span><span class="sxs-lookup"><span data-stu-id="b5b37-239">The Java client defaults to using the WebSockets transport.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withTransport(TransportEnum.WEBSOCKETS)
    .build();
```

> [!NOTE]
> <span data-ttu-id="b5b37-240">El cliente de SignalR Java todavía no admite transporte de reserva.</span><span class="sxs-lookup"><span data-stu-id="b5b37-240">The SignalR Java client doesn't support transport fallback yet.</span></span>

::: moniker-end

### <a name="configure-bearer-authentication"></a><span data-ttu-id="b5b37-241">Configurar la autenticación de portador</span><span class="sxs-lookup"><span data-stu-id="b5b37-241">Configure bearer authentication</span></span>

<span data-ttu-id="b5b37-242">Para proporcionar datos de autenticación junto con las solicitudes de SignalR, use el `AccessTokenProvider` opción (`accessTokenFactory` en JavaScript) para especificar una función que devuelve el token de acceso deseado.</span><span class="sxs-lookup"><span data-stu-id="b5b37-242">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="b5b37-243">En el cliente. NET, este token de acceso se pasa como un HTTP "Autenticación del portador" token (mediante el `Authorization` encabezado con un tipo de `Bearer`).</span><span class="sxs-lookup"><span data-stu-id="b5b37-243">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="b5b37-244">En el cliente de JavaScript, se usa el token de acceso como un token de portador, **excepto** en algunos casos donde las API de explorador restringir la capacidad de aplicar los encabezados (específicamente, en las solicitudes de los eventos y WebSockets).</span><span class="sxs-lookup"><span data-stu-id="b5b37-244">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="b5b37-245">En estos casos, el token de acceso se proporciona como un valor de cadena de consulta `access_token`.</span><span class="sxs-lookup"><span data-stu-id="b5b37-245">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="b5b37-246">En el cliente. NET, el `AccessTokenProvider` opción puede especificarse utilizando el delegado de opciones en `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="b5b37-246">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="b5b37-247">En el cliente de JavaScript, el token de acceso se configura estableciendo el `accessTokenFactory` campo en el objeto de opciones de `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="b5b37-247">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        accessTokenFactory: () => {
            // Get and return the access token.
            // This function can return a JavaScript Promise if asynchronous
            // logic is required to retrieve the access token.
        }
    })
    .build();
```

<span data-ttu-id="b5b37-248">En el cliente de SignalR Java, puede configurar un token de portador a usar para la autenticación mediante un generador de token de acceso a la [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span><span class="sxs-lookup"><span data-stu-id="b5b37-248">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="b5b37-249">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) para proporcionar una [RxJava](https://github.com/ReactiveX/RxJava) [único\<cadena >](http://reactivex.io/documentation/single.html).</span><span class="sxs-lookup"><span data-stu-id="b5b37-249">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](http://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="b5b37-250">Con una llamada a [Single.defer](http://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), puede escribir la lógica para generar tokens de acceso para el cliente.</span><span class="sxs-lookup"><span data-stu-id="b5b37-250">With a call to [Single.defer](http://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="b5b37-251">Configurar el tiempo de espera y las opciones de mantenimiento</span><span class="sxs-lookup"><span data-stu-id="b5b37-251">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="b5b37-252">Opciones adicionales para configurar el tiempo de espera y el comportamiento de mantenimiento están disponibles en el `HubConnection` propio objeto:</span><span class="sxs-lookup"><span data-stu-id="b5b37-252">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

# <a name="nettabdotnet"></a>[<span data-ttu-id="b5b37-253">.NET</span><span class="sxs-lookup"><span data-stu-id="b5b37-253">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="b5b37-254">Opción</span><span class="sxs-lookup"><span data-stu-id="b5b37-254">Option</span></span> | <span data-ttu-id="b5b37-255">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="b5b37-255">Default value</span></span> | <span data-ttu-id="b5b37-256">Descripción</span><span class="sxs-lookup"><span data-stu-id="b5b37-256">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="b5b37-257">30 segundos (30.000 milisegundos)</span><span class="sxs-lookup"><span data-stu-id="b5b37-257">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="b5b37-258">Tiempo de espera para la actividad del servidor.</span><span class="sxs-lookup"><span data-stu-id="b5b37-258">Timeout for server activity.</span></span> <span data-ttu-id="b5b37-259">Si el servidor no ha enviado un mensaje en este intervalo, el cliente considera que las ha desconectado el servidor y los desencadenadores la `Closed` eventos (`onclose` en JavaScript).</span><span class="sxs-lookup"><span data-stu-id="b5b37-259">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="b5b37-260">Este valor debe ser lo suficientemente grande como para un mensaje ping se envíe desde el servidor **y** recibidos por el cliente dentro del intervalo de tiempo de espera.</span><span class="sxs-lookup"><span data-stu-id="b5b37-260">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="b5b37-261">El valor recomendado es de un número al menos el doble del servidor `KeepAliveInterval` valor, para dejar tiempo para pings de llegada.</span><span class="sxs-lookup"><span data-stu-id="b5b37-261">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="b5b37-262">15 segundos</span><span class="sxs-lookup"><span data-stu-id="b5b37-262">15 seconds</span></span> | <span data-ttu-id="b5b37-263">Tiempo de espera para el protocolo de enlace de servidor inicial.</span><span class="sxs-lookup"><span data-stu-id="b5b37-263">Timeout for initial server handshake.</span></span> <span data-ttu-id="b5b37-264">Si el servidor no envía una respuesta de protocolo de enlace en este intervalo, el cliente cancela el protocolo de enlace y los desencadenadores la `Closed` eventos (`onclose` en JavaScript).</span><span class="sxs-lookup"><span data-stu-id="b5b37-264">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="b5b37-265">Se trata de una opción avanzada que sólo debería modificarse si se producen errores de tiempo de espera del protocolo de enlace debido a la latencia de red graves.</span><span class="sxs-lookup"><span data-stu-id="b5b37-265">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="b5b37-266">Para obtener más detalles sobre el proceso de negociación, consulte el [especificación del protocolo SignalR Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="b5b37-266">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

<span data-ttu-id="b5b37-267">En el cliente. NET, se especifican los valores de tiempo de espera como `TimeSpan` valores.</span><span class="sxs-lookup"><span data-stu-id="b5b37-267">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="b5b37-268">JavaScript</span><span class="sxs-lookup"><span data-stu-id="b5b37-268">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="b5b37-269">Opción</span><span class="sxs-lookup"><span data-stu-id="b5b37-269">Option</span></span> | <span data-ttu-id="b5b37-270">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="b5b37-270">Default value</span></span> | <span data-ttu-id="b5b37-271">Descripción</span><span class="sxs-lookup"><span data-stu-id="b5b37-271">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="b5b37-272">30 segundos (30.000 milisegundos)</span><span class="sxs-lookup"><span data-stu-id="b5b37-272">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="b5b37-273">Tiempo de espera para la actividad del servidor.</span><span class="sxs-lookup"><span data-stu-id="b5b37-273">Timeout for server activity.</span></span> <span data-ttu-id="b5b37-274">Si el servidor no ha enviado un mensaje en este intervalo, el cliente considera que las ha desconectado el servidor y los desencadenadores la `onclose` eventos.</span><span class="sxs-lookup"><span data-stu-id="b5b37-274">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="b5b37-275">Este valor debe ser lo suficientemente grande como para un mensaje ping se envíe desde el servidor **y** recibidos por el cliente dentro del intervalo de tiempo de espera.</span><span class="sxs-lookup"><span data-stu-id="b5b37-275">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="b5b37-276">El valor recomendado es de un número al menos el doble del servidor `KeepAliveInterval` valor, para dejar tiempo para pings de llegada.</span><span class="sxs-lookup"><span data-stu-id="b5b37-276">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="b5b37-277">Java</span><span class="sxs-lookup"><span data-stu-id="b5b37-277">Java</span></span>](#tab/java)

| <span data-ttu-id="b5b37-278">Opción</span><span class="sxs-lookup"><span data-stu-id="b5b37-278">Option</span></span> | <span data-ttu-id="b5b37-279">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="b5b37-279">Default value</span></span> | <span data-ttu-id="b5b37-280">Descripción</span><span class="sxs-lookup"><span data-stu-id="b5b37-280">Description</span></span> |
| ----------- | ------------- | ----------- |
|<span data-ttu-id="b5b37-281">`getServerTimeout` `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="b5b37-281">`getServerTimeout` `setServerTimeout`</span></span> | <span data-ttu-id="b5b37-282">30 segundos (30.000 milisegundos)</span><span class="sxs-lookup"><span data-stu-id="b5b37-282">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="b5b37-283">Tiempo de espera para la actividad del servidor.</span><span class="sxs-lookup"><span data-stu-id="b5b37-283">Timeout for server activity.</span></span> <span data-ttu-id="b5b37-284">Si el servidor no ha enviado un mensaje en este intervalo, el cliente considera que las ha desconectado el servidor y los desencadenadores la `onClose` eventos.</span><span class="sxs-lookup"><span data-stu-id="b5b37-284">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="b5b37-285">Este valor debe ser lo suficientemente grande como para un mensaje ping se envíe desde el servidor **y** recibidos por el cliente dentro del intervalo de tiempo de espera.</span><span class="sxs-lookup"><span data-stu-id="b5b37-285">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="b5b37-286">El valor recomendado es de un número al menos el doble del servidor `KeepAliveInterval` valor, para dejar tiempo para pings de llegada.</span><span class="sxs-lookup"><span data-stu-id="b5b37-286">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="b5b37-287">15 segundos</span><span class="sxs-lookup"><span data-stu-id="b5b37-287">15 seconds</span></span> | <span data-ttu-id="b5b37-288">Tiempo de espera para el protocolo de enlace de servidor inicial.</span><span class="sxs-lookup"><span data-stu-id="b5b37-288">Timeout for initial server handshake.</span></span> <span data-ttu-id="b5b37-289">Si el servidor no envía una respuesta de protocolo de enlace en este intervalo, el cliente cancela el protocolo de enlace y los desencadenadores la `onClose` eventos.</span><span class="sxs-lookup"><span data-stu-id="b5b37-289">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="b5b37-290">Se trata de una opción avanzada que sólo debería modificarse si se producen errores de tiempo de espera del protocolo de enlace debido a la latencia de red graves.</span><span class="sxs-lookup"><span data-stu-id="b5b37-290">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="b5b37-291">Para obtener más detalles sobre el proceso de negociación, consulte el [especificación del protocolo SignalR Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="b5b37-291">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

---

### <a name="configure-additional-options"></a><span data-ttu-id="b5b37-292">Configurar opciones adicionales</span><span class="sxs-lookup"><span data-stu-id="b5b37-292">Configure additional options</span></span>

<span data-ttu-id="b5b37-293">Se pueden configurar opciones adicionales en el `WithUrl` (`withUrl` en JavaScript) método `HubConnectionBuilder` o en las diversas API de configuración en el `HttpHubConnectionBuilder` en el cliente de Java:</span><span class="sxs-lookup"><span data-stu-id="b5b37-293">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="nettabdotnet"></a>[<span data-ttu-id="b5b37-294">.NET</span><span class="sxs-lookup"><span data-stu-id="b5b37-294">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="b5b37-295">Opción de .NET</span><span class="sxs-lookup"><span data-stu-id="b5b37-295">.NET Option</span></span> |  <span data-ttu-id="b5b37-296">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="b5b37-296">Default value</span></span> | <span data-ttu-id="b5b37-297">Descripción</span><span class="sxs-lookup"><span data-stu-id="b5b37-297">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="b5b37-298">Una función que devuelve una cadena que se proporciona como un token de autenticación de portador en solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="b5b37-298">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="b5b37-299">Establezca esta opción en `true` para omitir el paso de negociación.</span><span class="sxs-lookup"><span data-stu-id="b5b37-299">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="b5b37-300">**Solo se admite cuando el transporte de WebSockets es el único tipo de transporte habilitado**.</span><span class="sxs-lookup"><span data-stu-id="b5b37-300">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="b5b37-301">No se puede habilitar esta configuración al usar Azure SignalR Service.</span><span class="sxs-lookup"><span data-stu-id="b5b37-301">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="b5b37-302">Empty</span><span class="sxs-lookup"><span data-stu-id="b5b37-302">Empty</span></span> | <span data-ttu-id="b5b37-303">Una colección de certificados TLS que se envían autenticar las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="b5b37-303">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="b5b37-304">Empty</span><span class="sxs-lookup"><span data-stu-id="b5b37-304">Empty</span></span> | <span data-ttu-id="b5b37-305">Una colección de cookies HTTP para enviar con cada solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="b5b37-305">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="b5b37-306">Empty</span><span class="sxs-lookup"><span data-stu-id="b5b37-306">Empty</span></span> | <span data-ttu-id="b5b37-307">Credenciales que se enviará con todas las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="b5b37-307">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="b5b37-308">5 segundos</span><span class="sxs-lookup"><span data-stu-id="b5b37-308">5 seconds</span></span> | <span data-ttu-id="b5b37-309">Solo WebSockets.</span><span class="sxs-lookup"><span data-stu-id="b5b37-309">WebSockets only.</span></span> <span data-ttu-id="b5b37-310">La cantidad máxima de tiempo de espera a que el cliente después del cierre del servidor confirmar la solicitud de cierre.</span><span class="sxs-lookup"><span data-stu-id="b5b37-310">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="b5b37-311">Si el servidor no reconoció el cierre dentro de este tiempo, el cliente se desconecta.</span><span class="sxs-lookup"><span data-stu-id="b5b37-311">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="b5b37-312">Empty</span><span class="sxs-lookup"><span data-stu-id="b5b37-312">Empty</span></span> | <span data-ttu-id="b5b37-313">Una asignación de encabezados HTTP adicionales que se enviará con todas las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="b5b37-313">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="b5b37-314">Un delegado que puede usarse para configurar o reemplazar el `HttpMessageHandler` utilizado para enviar solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="b5b37-314">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="b5b37-315">No se utiliza para las conexiones de WebSocket.</span><span class="sxs-lookup"><span data-stu-id="b5b37-315">Not used for WebSocket connections.</span></span> <span data-ttu-id="b5b37-316">Este delegado debe devolver un valor distinto de null, y recibe el valor predeterminado como un parámetro.</span><span class="sxs-lookup"><span data-stu-id="b5b37-316">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="b5b37-317">Modificar la configuración en ese valor predeterminado y devolverlo o devolver un nuevo `HttpMessageHandler` instancia.</span><span class="sxs-lookup"><span data-stu-id="b5b37-317">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="b5b37-318">**Cuando Asegúrese de reemplazar el controlador copiar la configuración que desee impedir que el controlador proporcionado, en caso contrario, las opciones configuradas (por ejemplo, Cookies y encabezados) no se aplicarán al nuevo controlador.**</span><span class="sxs-lookup"><span data-stu-id="b5b37-318">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="b5b37-319">Un proxy HTTP que se utilizará al enviar solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="b5b37-319">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="b5b37-320">Establezca este valor booleano para enviar las credenciales predeterminadas para las solicitudes HTTP y WebSockets.</span><span class="sxs-lookup"><span data-stu-id="b5b37-320">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="b5b37-321">Esto permite el uso de autenticación de Windows.</span><span class="sxs-lookup"><span data-stu-id="b5b37-321">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="b5b37-322">Un delegado que puede usarse para configurar opciones adicionales de WebSocket.</span><span class="sxs-lookup"><span data-stu-id="b5b37-322">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="b5b37-323">Recibe una instancia de [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) que puede utilizarse para configurar las opciones.</span><span class="sxs-lookup"><span data-stu-id="b5b37-323">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="b5b37-324">JavaScript</span><span class="sxs-lookup"><span data-stu-id="b5b37-324">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="b5b37-325">Opción de JavaScript</span><span class="sxs-lookup"><span data-stu-id="b5b37-325">JavaScript Option</span></span> | <span data-ttu-id="b5b37-326">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="b5b37-326">Default Value</span></span> | <span data-ttu-id="b5b37-327">Descripción</span><span class="sxs-lookup"><span data-stu-id="b5b37-327">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="b5b37-328">Una función que devuelve una cadena que se proporciona como un token de autenticación de portador en solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="b5b37-328">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="b5b37-329">Establezca esta opción en `true` para omitir el paso de negociación.</span><span class="sxs-lookup"><span data-stu-id="b5b37-329">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="b5b37-330">**Solo se admite cuando el transporte de WebSockets es el único tipo de transporte habilitado**.</span><span class="sxs-lookup"><span data-stu-id="b5b37-330">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="b5b37-331">No se puede habilitar esta configuración al usar Azure SignalR Service.</span><span class="sxs-lookup"><span data-stu-id="b5b37-331">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="b5b37-332">Java</span><span class="sxs-lookup"><span data-stu-id="b5b37-332">Java</span></span>](#tab/java)

| <span data-ttu-id="b5b37-333">Opción de Java</span><span class="sxs-lookup"><span data-stu-id="b5b37-333">Java Option</span></span> | <span data-ttu-id="b5b37-334">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="b5b37-334">Default Value</span></span> | <span data-ttu-id="b5b37-335">Descripción</span><span class="sxs-lookup"><span data-stu-id="b5b37-335">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="b5b37-336">Una función que devuelve una cadena que se proporciona como un token de autenticación de portador en solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="b5b37-336">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="b5b37-337">Establezca esta opción en `true` para omitir el paso de negociación.</span><span class="sxs-lookup"><span data-stu-id="b5b37-337">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="b5b37-338">**Solo se admite cuando el transporte de WebSockets es el único tipo de transporte habilitado**.</span><span class="sxs-lookup"><span data-stu-id="b5b37-338">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="b5b37-339">No se puede habilitar esta configuración al usar Azure SignalR Service.</span><span class="sxs-lookup"><span data-stu-id="b5b37-339">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="b5b37-340">`withHeader` `withHeaders`</span><span class="sxs-lookup"><span data-stu-id="b5b37-340">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="b5b37-341">Empty</span><span class="sxs-lookup"><span data-stu-id="b5b37-341">Empty</span></span> | <span data-ttu-id="b5b37-342">Una asignación de encabezados HTTP adicionales que se enviará con todas las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="b5b37-342">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="b5b37-343">En el cliente. NET, estas opciones se pueden modificar por el delegado de las opciones proporcionado para `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="b5b37-343">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="b5b37-344">En el cliente de JavaScript, estas opciones se pueden proporcionar en un objeto de JavaScript proporcionado a `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="b5b37-344">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="b5b37-345">En el cliente de Java, estas opciones pueden configurarse con los métodos en el `HttpHubConnectionBuilder` devuelto desde el `HubConnectionBuilder.create("HUB URL")`</span><span class="sxs-lookup"><span data-stu-id="b5b37-345">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="b5b37-346">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="b5b37-346">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
