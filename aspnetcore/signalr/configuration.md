---
title: Configuración de ASP.NET Core SignalR
author: bradygaster
description: Obtenga información sobre cómo configurar aplicaciones de ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 02/07/2019
uid: signalr/configuration
ms.openlocfilehash: f5449a15743c1f38c550fe30945bdc19f069e3f5
ms.sourcegitcommit: b72bbc9ae91e4bd37c9ea9b2d09ebf47afb25dd7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/08/2019
ms.locfileid: "55958120"
---
# <a name="aspnet-core-signalr-configuration"></a><span data-ttu-id="f011c-103">Configuración de ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="f011c-103">ASP.NET Core SignalR configuration</span></span>

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="f011c-104">Opciones de serialización de JSON/MessagePack</span><span class="sxs-lookup"><span data-stu-id="f011c-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="f011c-105">SignalR de ASP.NET Core admite dos protocolos para la codificación de mensajes: [JSON](https://www.json.org/) y [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="f011c-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="f011c-106">Cada protocolo tiene opciones de configuración de serialización.</span><span class="sxs-lookup"><span data-stu-id="f011c-106">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="f011c-107">Serialización de JSON se puede configurar en el servidor mediante el [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) método de extensión, que se puede agregar después [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) en su `Startup.ConfigureServices` método.</span><span class="sxs-lookup"><span data-stu-id="f011c-107">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="f011c-108">El `AddJsonProtocol` método toma un delegado que recibe un `options` objeto.</span><span class="sxs-lookup"><span data-stu-id="f011c-108">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="f011c-109">El [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) propiedad en ese objeto es un JSON.NET `JsonSerializerSettings` objeto que puede usarse para configurar la serialización de argumentos y valores devueltos.</span><span class="sxs-lookup"><span data-stu-id="f011c-109">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="f011c-110">Consulte la [documentación JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm) para obtener más detalles.</span><span class="sxs-lookup"><span data-stu-id="f011c-110">See the [JSON.NET Documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm) for more details.</span></span>

<span data-ttu-id="f011c-111">Por ejemplo, para configurar el serializador para usar nombres de propiedad "PascalCase", en lugar de los nombres predeterminados "camelCase", use el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="f011c-111">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code:</span></span>

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver = 
        new DefaultContractResolver();
    });
```

<span data-ttu-id="f011c-112">En el cliente. NET, la misma `AddJsonProtocol` existe en el método de extensión [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="f011c-112">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="f011c-113">El `Microsoft.Extensions.DependencyInjection` se debe importar el espacio de nombres para resolver el método de extensión:</span><span class="sxs-lookup"><span data-stu-id="f011c-113">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="f011c-114">No es posible configurar la serialización de JSON en el cliente de JavaScript en este momento.</span><span class="sxs-lookup"><span data-stu-id="f011c-114">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="f011c-115">Opciones de serialización MessagePack</span><span class="sxs-lookup"><span data-stu-id="f011c-115">MessagePack serialization options</span></span>

<span data-ttu-id="f011c-116">MessagePack serialización se puede configurar si se proporciona un delegado para el [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) llamar.</span><span class="sxs-lookup"><span data-stu-id="f011c-116">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="f011c-117">Consulte [MessagePack en SignalR](xref:signalr/messagepackhubprotocol) para obtener más detalles.</span><span class="sxs-lookup"><span data-stu-id="f011c-117">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="f011c-118">No es posible configurar la serialización de MessagePack en el cliente de JavaScript en este momento.</span><span class="sxs-lookup"><span data-stu-id="f011c-118">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="f011c-119">Configurar opciones de servidor</span><span class="sxs-lookup"><span data-stu-id="f011c-119">Configure server options</span></span>

<span data-ttu-id="f011c-120">En la tabla siguiente se describe las opciones para configurar los concentradores de SignalR:</span><span class="sxs-lookup"><span data-stu-id="f011c-120">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="f011c-121">Opción</span><span class="sxs-lookup"><span data-stu-id="f011c-121">Option</span></span> | <span data-ttu-id="f011c-122">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="f011c-122">Default Value</span></span> | <span data-ttu-id="f011c-123">Descripción</span><span class="sxs-lookup"><span data-stu-id="f011c-123">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="f011c-124">30 segundos</span><span class="sxs-lookup"><span data-stu-id="f011c-124">30 seconds</span></span> | <span data-ttu-id="f011c-125">El servidor tendrá en cuenta el cliente desconectado si no ha recibido un mensaje (incluido keep-alive) en este intervalo.</span><span class="sxs-lookup"><span data-stu-id="f011c-125">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="f011c-126">Puede tardar más de este intervalo de tiempo de espera para el cliente realmente marcarse desconectado debido a su implementación.</span><span class="sxs-lookup"><span data-stu-id="f011c-126">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="f011c-127">El valor recomendado es doble el `KeepAliveInterval` valor.</span><span class="sxs-lookup"><span data-stu-id="f011c-127">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="f011c-128">15 segundos</span><span class="sxs-lookup"><span data-stu-id="f011c-128">15 seconds</span></span> | <span data-ttu-id="f011c-129">Si el cliente no envía un mensaje de protocolo de enlace inicial dentro de este intervalo de tiempo, se cierra la conexión.</span><span class="sxs-lookup"><span data-stu-id="f011c-129">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="f011c-130">Se trata de una opción avanzada que sólo debería modificarse si se producen errores de tiempo de espera del protocolo de enlace debido a la latencia de red graves.</span><span class="sxs-lookup"><span data-stu-id="f011c-130">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="f011c-131">Para obtener más detalles sobre el proceso de negociación, consulte el [especificación del protocolo SignalR Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="f011c-131">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="f011c-132">15 segundos</span><span class="sxs-lookup"><span data-stu-id="f011c-132">15 seconds</span></span> | <span data-ttu-id="f011c-133">Si el servidor no ha enviado un mensaje dentro de este intervalo, se envía automáticamente un mensaje ping para mantener abierta la conexión.</span><span class="sxs-lookup"><span data-stu-id="f011c-133">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="f011c-134">Al cambiar `KeepAliveInterval`, cambie el `ServerTimeout` / `serverTimeoutInMilliseconds` configuración del cliente.</span><span class="sxs-lookup"><span data-stu-id="f011c-134">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="f011c-135">Recomendado `ServerTimeout` / `serverTimeoutInMilliseconds` valor es doble el `KeepAliveInterval` valor.</span><span class="sxs-lookup"><span data-stu-id="f011c-135">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="f011c-136">Todos los protocolos instalados</span><span class="sxs-lookup"><span data-stu-id="f011c-136">All installed protocols</span></span> | <span data-ttu-id="f011c-137">Protocolos admitidos por este concentrador.</span><span class="sxs-lookup"><span data-stu-id="f011c-137">Protocols supported by this hub.</span></span> <span data-ttu-id="f011c-138">De forma predeterminada, se permiten todos los protocolos registrados en el servidor, pero se pueden quitar protocolos de esta lista para deshabilitar los protocolos específicos para los concentradores individuales.</span><span class="sxs-lookup"><span data-stu-id="f011c-138">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="f011c-139">Si `true`detallados se devuelven los mensajes de excepción a los clientes cuando se produce una excepción en un método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="f011c-139">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="f011c-140">El valor predeterminado es `false`, ya que estos mensajes de excepción pueden contener información confidencial.</span><span class="sxs-lookup"><span data-stu-id="f011c-140">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

<span data-ttu-id="f011c-141">Se pueden configurar opciones para todos los centros proporcionando un delegado de opciones para la `AddSignalR` llamar `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="f011c-141">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="f011c-142">Opciones para un único centro de invalidan las opciones globales de `AddSignalR` y pueden configurarse mediante [AddHubOptions\<T >](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):</span><span class="sxs-lookup"><span data-stu-id="f011c-142">Options for a single hub override the global options provided in `AddSignalR` and can be configured using [AddHubOptions\<T>](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="f011c-143">Opciones de configuración de HTTP avanzadas</span><span class="sxs-lookup"><span data-stu-id="f011c-143">Advanced HTTP configuration options</span></span>

<span data-ttu-id="f011c-144">Use `HttpConnectionDispatcherOptions` para configurar opciones avanzadas relacionadas con transportes y administración de búfer de memoria.</span><span class="sxs-lookup"><span data-stu-id="f011c-144">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="f011c-145">Estas opciones se configuran pasando un delegado para [MapHub\<T >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) en `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="f011c-145">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="f011c-146">En la tabla siguiente se describe las opciones para configurar las opciones avanzadas de HTTP de ASP.NET Core SignalR:</span><span class="sxs-lookup"><span data-stu-id="f011c-146">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

| <span data-ttu-id="f011c-147">Opción</span><span class="sxs-lookup"><span data-stu-id="f011c-147">Option</span></span> | <span data-ttu-id="f011c-148">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="f011c-148">Default Value</span></span> | <span data-ttu-id="f011c-149">Descripción</span><span class="sxs-lookup"><span data-stu-id="f011c-149">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="f011c-150">32 KB</span><span class="sxs-lookup"><span data-stu-id="f011c-150">32 KB</span></span> | <span data-ttu-id="f011c-151">El número máximo de bytes recibidos del cliente que los búferes del servidor.</span><span class="sxs-lookup"><span data-stu-id="f011c-151">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="f011c-152">Al aumentar este valor permite que el servidor recibir los mensajes más grandes, pero puede repercutir negativamente en el consumo de memoria.</span><span class="sxs-lookup"><span data-stu-id="f011c-152">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="f011c-153">Recopilar automáticamente los datos de la `Authorize` atributos aplicados a la clase de concentrador.</span><span class="sxs-lookup"><span data-stu-id="f011c-153">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="f011c-154">Una lista de [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) los objetos utilizados para determinar si un cliente está autorizado para conectarse al concentrador.</span><span class="sxs-lookup"><span data-stu-id="f011c-154">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="f011c-155">32 KB</span><span class="sxs-lookup"><span data-stu-id="f011c-155">32 KB</span></span> | <span data-ttu-id="f011c-156">El número máximo de bytes enviados por la aplicación que los búferes del servidor.</span><span class="sxs-lookup"><span data-stu-id="f011c-156">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="f011c-157">Al aumentar este valor permite que el servidor enviar los mensajes más grandes, pero puede repercutir negativamente en el consumo de memoria.</span><span class="sxs-lookup"><span data-stu-id="f011c-157">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="f011c-158">Se habilitan todos los transportes.</span><span class="sxs-lookup"><span data-stu-id="f011c-158">All Transports are enabled.</span></span> | <span data-ttu-id="f011c-159">Una máscara de bits de `HttpTransportType` valores que pueden restringir los transportes que un cliente puede usar para conectarse.</span><span class="sxs-lookup"><span data-stu-id="f011c-159">A bitmask of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="f011c-160">Véalo a continuación.</span><span class="sxs-lookup"><span data-stu-id="f011c-160">See below.</span></span> | <span data-ttu-id="f011c-161">Opciones adicionales específicas para el transporte de sondeo largo.</span><span class="sxs-lookup"><span data-stu-id="f011c-161">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="f011c-162">Véalo a continuación.</span><span class="sxs-lookup"><span data-stu-id="f011c-162">See below.</span></span> | <span data-ttu-id="f011c-163">Opciones adicionales específicas para el transporte de WebSockets.</span><span class="sxs-lookup"><span data-stu-id="f011c-163">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="f011c-164">El transporte de sondeo largo tiene opciones adicionales que se pueden configurar mediante el `LongPolling` propiedad:</span><span class="sxs-lookup"><span data-stu-id="f011c-164">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="f011c-165">Opción</span><span class="sxs-lookup"><span data-stu-id="f011c-165">Option</span></span> | <span data-ttu-id="f011c-166">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="f011c-166">Default Value</span></span> | <span data-ttu-id="f011c-167">Descripción</span><span class="sxs-lookup"><span data-stu-id="f011c-167">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="f011c-168">90 segundos</span><span class="sxs-lookup"><span data-stu-id="f011c-168">90 seconds</span></span> | <span data-ttu-id="f011c-169">La cantidad máxima de tiempo que el servidor espera para que un mensaje para enviárselo al cliente antes de finalizar una solicitud de sondeo única.</span><span class="sxs-lookup"><span data-stu-id="f011c-169">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="f011c-170">Al disminuir este valor hace que el cliente emitir solicitudes de sondeo de nuevo con más frecuencia.</span><span class="sxs-lookup"><span data-stu-id="f011c-170">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="f011c-171">El transporte de WebSocket tiene opciones adicionales que se pueden configurar mediante el `WebSockets` propiedad:</span><span class="sxs-lookup"><span data-stu-id="f011c-171">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="f011c-172">Opción</span><span class="sxs-lookup"><span data-stu-id="f011c-172">Option</span></span> | <span data-ttu-id="f011c-173">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="f011c-173">Default Value</span></span> | <span data-ttu-id="f011c-174">Descripción</span><span class="sxs-lookup"><span data-stu-id="f011c-174">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="f011c-175">5 segundos</span><span class="sxs-lookup"><span data-stu-id="f011c-175">5 seconds</span></span> | <span data-ttu-id="f011c-176">Después de cerrar el servidor, si se produce un error en el cliente cerrar dentro de este intervalo de tiempo, se termina la conexión.</span><span class="sxs-lookup"><span data-stu-id="f011c-176">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="f011c-177">Un delegado que puede usarse para establecer el `Sec-WebSocket-Protocol` encabezado en un valor personalizado.</span><span class="sxs-lookup"><span data-stu-id="f011c-177">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="f011c-178">El delegado recibe los valores solicitados por el cliente como entrada y se espera que devuelva el valor deseado.</span><span class="sxs-lookup"><span data-stu-id="f011c-178">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="f011c-179">Configurar las opciones de cliente</span><span class="sxs-lookup"><span data-stu-id="f011c-179">Configure client options</span></span>

<span data-ttu-id="f011c-180">Se pueden configurar las opciones de cliente en el `HubConnectionBuilder` tipo (disponible en los clientes de .NET y JavaScript), así como en el `HubConnection` propio.</span><span class="sxs-lookup"><span data-stu-id="f011c-180">Client options can be configured on the `HubConnectionBuilder` type (available in both .NET and JavaScript clients), as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="f011c-181">Configurar el registro</span><span class="sxs-lookup"><span data-stu-id="f011c-181">Configure logging</span></span>

<span data-ttu-id="f011c-182">El registro está configurado en el cliente de .NET mediante el `ConfigureLogging` método.</span><span class="sxs-lookup"><span data-stu-id="f011c-182">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="f011c-183">Registro de proveedores y los filtros se puede registrar en la misma manera, tal como están en el servidor.</span><span class="sxs-lookup"><span data-stu-id="f011c-183">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="f011c-184">Consulte la [registro en ASP.NET Core](xref:fundamentals/logging/index) documentación para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="f011c-184">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="f011c-185">Con el fin de registrar los proveedores de registro, debe instalar los paquetes necesarios.</span><span class="sxs-lookup"><span data-stu-id="f011c-185">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="f011c-186">Consulte la [proveedores de registro integrados](xref:fundamentals/logging/index#built-in-logging-providers) sección de la documentación para obtener una lista completa.</span><span class="sxs-lookup"><span data-stu-id="f011c-186">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="f011c-187">Por ejemplo, para habilitar el registro de consola, instale el `Microsoft.Extensions.Logging.Console` paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="f011c-187">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="f011c-188">Llame a la `AddConsole` método de extensión:</span><span class="sxs-lookup"><span data-stu-id="f011c-188">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="f011c-189">En el cliente de JavaScript, un proceso similar `configureLogging` método existe.</span><span class="sxs-lookup"><span data-stu-id="f011c-189">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="f011c-190">Proporcione un `LogLevel` valor que indica el nivel mínimo de los mensajes de registro para generar.</span><span class="sxs-lookup"><span data-stu-id="f011c-190">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="f011c-191">Los registros se escriben en la ventana de consola del explorador.</span><span class="sxs-lookup"><span data-stu-id="f011c-191">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> <span data-ttu-id="f011c-192">Para deshabilitar el registro por completo, especifique `signalR.LogLevel.None` en el `configureLogging` método.</span><span class="sxs-lookup"><span data-stu-id="f011c-192">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="f011c-193">A continuación, se muestran los niveles de registro disponibles para el cliente de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="f011c-193">Log levels available to the JavaScript client are listed below.</span></span> <span data-ttu-id="f011c-194">Establecer el nivel de registro en uno de estos valores habilita el registro de mensajes en **o superior** ese nivel.</span><span class="sxs-lookup"><span data-stu-id="f011c-194">Setting the log level to one of these values enables logging of messages at **or above** that level.</span></span>

| <span data-ttu-id="f011c-195">Nivel</span><span class="sxs-lookup"><span data-stu-id="f011c-195">Level</span></span> | <span data-ttu-id="f011c-196">Descripción</span><span class="sxs-lookup"><span data-stu-id="f011c-196">Description</span></span> |
| ----- | ----------- |
| `None` | <span data-ttu-id="f011c-197">Se registra ningún mensaje.</span><span class="sxs-lookup"><span data-stu-id="f011c-197">No messages are logged.</span></span> |
| `Critical` | <span data-ttu-id="f011c-198">Mensajes que indican un error en toda la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f011c-198">Messages that indicate a failure in the entire app.</span></span> |
| `Error` | <span data-ttu-id="f011c-199">Mensajes que indican un error en la operación actual.</span><span class="sxs-lookup"><span data-stu-id="f011c-199">Messages that indicate a failure in the current operation.</span></span> |
| `Warning` | <span data-ttu-id="f011c-200">Mensajes que indican un problema grave.</span><span class="sxs-lookup"><span data-stu-id="f011c-200">Messages that indicate a non-fatal problem.</span></span> |
| `Information` | <span data-ttu-id="f011c-201">Mensajes informativos.</span><span class="sxs-lookup"><span data-stu-id="f011c-201">Informational messages.</span></span> |
| `Debug` | <span data-ttu-id="f011c-202">Mensajes de diagnóstico útiles para depurar.</span><span class="sxs-lookup"><span data-stu-id="f011c-202">Diagnostic messages useful for debugging.</span></span> |
| `Trace` | <span data-ttu-id="f011c-203">Mensajes de diagnóstico muy detallados diseñados para diagnosticar problemas específicos.</span><span class="sxs-lookup"><span data-stu-id="f011c-203">Very detailed diagnostic messages designed for diagnosing specific issues.</span></span> |

### <a name="configure-allowed-transports"></a><span data-ttu-id="f011c-204">Configurar transportes permitidos</span><span class="sxs-lookup"><span data-stu-id="f011c-204">Configure allowed transports</span></span>

<span data-ttu-id="f011c-205">Los transportes utilizados por SignalR se pueden configurar en el `WithUrl` llamar (`withUrl` en JavaScript).</span><span class="sxs-lookup"><span data-stu-id="f011c-205">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="f011c-206">Una operación OR bit a bit de los valores de `HttpTransportType` puede usarse para restringir el cliente para que solo use los transportes especificados.</span><span class="sxs-lookup"><span data-stu-id="f011c-206">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="f011c-207">Todos los transportes están habilitados de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="f011c-207">All transports are enabled by default.</span></span>

<span data-ttu-id="f011c-208">Por ejemplo, para deshabilitar el transporte de los eventos, pero permitir conexiones WebSockets y Long Polling:</span><span class="sxs-lookup"><span data-stu-id="f011c-208">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="f011c-209">En el cliente de JavaScript, se configuran los transportes estableciendo el `transport` campo en el objeto de opciones proporcionado para `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="f011c-209">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

### <a name="configure-bearer-authentication"></a><span data-ttu-id="f011c-210">Configurar la autenticación de portador</span><span class="sxs-lookup"><span data-stu-id="f011c-210">Configure bearer authentication</span></span>

<span data-ttu-id="f011c-211">Para proporcionar datos de autenticación junto con las solicitudes de SignalR, use el `AccessTokenProvider` opción (`accessTokenFactory` en JavaScript) para especificar una función que devuelve el token de acceso deseado.</span><span class="sxs-lookup"><span data-stu-id="f011c-211">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="f011c-212">En el cliente. NET, este token de acceso se pasa como un HTTP "Autenticación del portador" token (mediante el `Authorization` encabezado con un tipo de `Bearer`).</span><span class="sxs-lookup"><span data-stu-id="f011c-212">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="f011c-213">En el cliente de JavaScript, se usa el token de acceso como un token de portador, **excepto** en algunos casos donde las API de explorador restringir la capacidad de aplicar los encabezados (específicamente, en las solicitudes de los eventos y WebSockets).</span><span class="sxs-lookup"><span data-stu-id="f011c-213">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="f011c-214">En estos casos, el token de acceso se proporciona como un valor de cadena de consulta `access_token`.</span><span class="sxs-lookup"><span data-stu-id="f011c-214">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="f011c-215">En el cliente. NET, el `AccessTokenProvider` opción puede especificarse utilizando el delegado de opciones en `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="f011c-215">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="f011c-216">En el cliente de JavaScript, el token de acceso se configura estableciendo el `accessTokenFactory` campo en el objeto de opciones de `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="f011c-216">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="f011c-217">Configurar el tiempo de espera y las opciones de mantenimiento</span><span class="sxs-lookup"><span data-stu-id="f011c-217">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="f011c-218">Opciones adicionales para configurar el tiempo de espera y el comportamiento de mantenimiento están disponibles en el `HubConnection` propio objeto:</span><span class="sxs-lookup"><span data-stu-id="f011c-218">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

| <span data-ttu-id="f011c-219">Opción de .NET</span><span class="sxs-lookup"><span data-stu-id="f011c-219">.NET Option</span></span> | <span data-ttu-id="f011c-220">Opción de JavaScript</span><span class="sxs-lookup"><span data-stu-id="f011c-220">JavaScript Option</span></span> | <span data-ttu-id="f011c-221">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="f011c-221">Default Value</span></span> | <span data-ttu-id="f011c-222">Descripción</span><span class="sxs-lookup"><span data-stu-id="f011c-222">Description</span></span> |
| ----------- | ----------------- | ------------- | ----------- |
| `ServerTimeout` | `serverTimeoutInMilliseconds` | <span data-ttu-id="f011c-223">30 segundos (30.000 milisegundos)</span><span class="sxs-lookup"><span data-stu-id="f011c-223">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="f011c-224">Tiempo de espera para la actividad del servidor.</span><span class="sxs-lookup"><span data-stu-id="f011c-224">Timeout for server activity.</span></span> <span data-ttu-id="f011c-225">Si el servidor no ha enviado un mensaje en este intervalo, el cliente considera que las ha desconectado el servidor y los desencadenadores la `Closed` eventos (`onclose` en JavaScript).</span><span class="sxs-lookup"><span data-stu-id="f011c-225">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="f011c-226">Este valor debe ser lo suficientemente grande como para un mensaje ping se envíe desde el servidor **y** recibidos por el cliente dentro del intervalo de tiempo de espera.</span><span class="sxs-lookup"><span data-stu-id="f011c-226">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="f011c-227">El valor recomendado es de un número al menos el doble del servidor `KeepAliveInterval` valor, para dejar tiempo para pings de llegada.</span><span class="sxs-lookup"><span data-stu-id="f011c-227">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="f011c-228">No se puede configurar</span><span class="sxs-lookup"><span data-stu-id="f011c-228">Not configurable</span></span> | <span data-ttu-id="f011c-229">15 segundos</span><span class="sxs-lookup"><span data-stu-id="f011c-229">15 seconds</span></span> | <span data-ttu-id="f011c-230">Tiempo de espera para el protocolo de enlace de servidor inicial.</span><span class="sxs-lookup"><span data-stu-id="f011c-230">Timeout for initial server handshake.</span></span> <span data-ttu-id="f011c-231">Si el servidor no envía una respuesta de protocolo de enlace en este intervalo, el cliente cancela el protocolo de enlace y los desencadenadores la `Closed` eventos (`onclose` en JavaScript).</span><span class="sxs-lookup"><span data-stu-id="f011c-231">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="f011c-232">Se trata de una opción avanzada que sólo debería modificarse si se producen errores de tiempo de espera del protocolo de enlace debido a la latencia de red graves.</span><span class="sxs-lookup"><span data-stu-id="f011c-232">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="f011c-233">Para obtener más detalles sobre el proceso de negociación, consulte el [especificación del protocolo SignalR Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="f011c-233">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

<span data-ttu-id="f011c-234">En el cliente. NET, se especifican los valores de tiempo de espera como `TimeSpan` valores.</span><span class="sxs-lookup"><span data-stu-id="f011c-234">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span> <span data-ttu-id="f011c-235">En el cliente de JavaScript, los valores de tiempo de espera se especifican como un número que indica la duración en milisegundos.</span><span class="sxs-lookup"><span data-stu-id="f011c-235">In the JavaScript client, timeout values are specified as a number indicating the duration in milliseconds.</span></span>

### <a name="configure-additional-options"></a><span data-ttu-id="f011c-236">Configurar opciones adicionales</span><span class="sxs-lookup"><span data-stu-id="f011c-236">Configure additional options</span></span>

<span data-ttu-id="f011c-237">Se pueden configurar opciones adicionales en el `WithUrl` (`withUrl` en JavaScript) método `HubConnectionBuilder`:</span><span class="sxs-lookup"><span data-stu-id="f011c-237">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder`:</span></span>

| <span data-ttu-id="f011c-238">Opción de .NET</span><span class="sxs-lookup"><span data-stu-id="f011c-238">.NET Option</span></span> | <span data-ttu-id="f011c-239">Opción de JavaScript</span><span class="sxs-lookup"><span data-stu-id="f011c-239">JavaScript Option</span></span> | <span data-ttu-id="f011c-240">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="f011c-240">Default Value</span></span> | <span data-ttu-id="f011c-241">Descripción</span><span class="sxs-lookup"><span data-stu-id="f011c-241">Description</span></span> |
| ----------- | ----------------- | ------------- | ----------- |
| `AccessTokenProvider` | `accessTokenFactory` | `null` | <span data-ttu-id="f011c-242">Una función que devuelve una cadena que se proporciona como un token de autenticación de portador en solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="f011c-242">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `skipNegotiation` | `false` | <span data-ttu-id="f011c-243">Establezca esta opción en `true` para omitir el paso de negociación.</span><span class="sxs-lookup"><span data-stu-id="f011c-243">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="f011c-244">**Solo se admite cuando el transporte de WebSockets es el único tipo de transporte habilitado**.</span><span class="sxs-lookup"><span data-stu-id="f011c-244">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="f011c-245">No se puede habilitar esta configuración al usar Azure SignalR Service.</span><span class="sxs-lookup"><span data-stu-id="f011c-245">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="f011c-246">No se puede configurar \*</span><span class="sxs-lookup"><span data-stu-id="f011c-246">Not configurable \*</span></span> | <span data-ttu-id="f011c-247">Empty</span><span class="sxs-lookup"><span data-stu-id="f011c-247">Empty</span></span> | <span data-ttu-id="f011c-248">Una colección de certificados TLS que se envían autenticar las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="f011c-248">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="f011c-249">No se puede configurar \*</span><span class="sxs-lookup"><span data-stu-id="f011c-249">Not configurable \*</span></span> | <span data-ttu-id="f011c-250">Empty</span><span class="sxs-lookup"><span data-stu-id="f011c-250">Empty</span></span> | <span data-ttu-id="f011c-251">Una colección de cookies HTTP para enviar con cada solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="f011c-251">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="f011c-252">No se puede configurar \*</span><span class="sxs-lookup"><span data-stu-id="f011c-252">Not configurable \*</span></span> | <span data-ttu-id="f011c-253">Empty</span><span class="sxs-lookup"><span data-stu-id="f011c-253">Empty</span></span> | <span data-ttu-id="f011c-254">Credenciales que se enviará con todas las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="f011c-254">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="f011c-255">No se puede configurar \*</span><span class="sxs-lookup"><span data-stu-id="f011c-255">Not configurable \*</span></span> | <span data-ttu-id="f011c-256">5 segundos</span><span class="sxs-lookup"><span data-stu-id="f011c-256">5 seconds</span></span> | <span data-ttu-id="f011c-257">Solo WebSockets.</span><span class="sxs-lookup"><span data-stu-id="f011c-257">WebSockets only.</span></span> <span data-ttu-id="f011c-258">La cantidad máxima de tiempo de espera a que el cliente después del cierre del servidor confirmar la solicitud de cierre.</span><span class="sxs-lookup"><span data-stu-id="f011c-258">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="f011c-259">Si el servidor no reconoció el cierre dentro de este tiempo, el cliente se desconecta.</span><span class="sxs-lookup"><span data-stu-id="f011c-259">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="f011c-260">No se puede configurar \*</span><span class="sxs-lookup"><span data-stu-id="f011c-260">Not configurable \*</span></span> | <span data-ttu-id="f011c-261">Empty</span><span class="sxs-lookup"><span data-stu-id="f011c-261">Empty</span></span> | <span data-ttu-id="f011c-262">Un diccionario de encabezados HTTP adicionales que se enviará con todas las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="f011c-262">A dictionary of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | <span data-ttu-id="f011c-263">No se puede configurar \*</span><span class="sxs-lookup"><span data-stu-id="f011c-263">Not configurable \*</span></span> | `null` | <span data-ttu-id="f011c-264">Un delegado que puede usarse para configurar o reemplazar el `HttpMessageHandler` utilizado para enviar solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="f011c-264">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="f011c-265">No se utiliza para las conexiones de WebSocket.</span><span class="sxs-lookup"><span data-stu-id="f011c-265">Not used for WebSocket connections.</span></span> <span data-ttu-id="f011c-266">Este delegado debe devolver un valor distinto de null, y recibe el valor predeterminado como un parámetro.</span><span class="sxs-lookup"><span data-stu-id="f011c-266">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="f011c-267">Modificar la configuración en ese valor predeterminado y devolverlo o devolver un nuevo `HttpMessageHandler` instancia.</span><span class="sxs-lookup"><span data-stu-id="f011c-267">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="f011c-268">**Cuando Asegúrese de reemplazar el controlador copiar la configuración que desee impedir que el controlador proporcionado, en caso contrario, las opciones configuradas (por ejemplo, Cookies y encabezados) no se aplicarán al nuevo controlador.**</span><span class="sxs-lookup"><span data-stu-id="f011c-268">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | <span data-ttu-id="f011c-269">No se puede configurar \*</span><span class="sxs-lookup"><span data-stu-id="f011c-269">Not configurable \*</span></span> | `null` | <span data-ttu-id="f011c-270">Un proxy HTTP que se utilizará al enviar solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="f011c-270">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | <span data-ttu-id="f011c-271">No se puede configurar \*</span><span class="sxs-lookup"><span data-stu-id="f011c-271">Not configurable \*</span></span> | `false` | <span data-ttu-id="f011c-272">Establezca este valor booleano para enviar las credenciales predeterminadas para las solicitudes HTTP y WebSockets.</span><span class="sxs-lookup"><span data-stu-id="f011c-272">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="f011c-273">Esto permite el uso de autenticación de Windows.</span><span class="sxs-lookup"><span data-stu-id="f011c-273">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | <span data-ttu-id="f011c-274">No se puede configurar \*</span><span class="sxs-lookup"><span data-stu-id="f011c-274">Not configurable \*</span></span> | `null` | <span data-ttu-id="f011c-275">Un delegado que puede usarse para configurar opciones adicionales de WebSocket.</span><span class="sxs-lookup"><span data-stu-id="f011c-275">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="f011c-276">Recibe una instancia de [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) que puede utilizarse para configurar las opciones.</span><span class="sxs-lookup"><span data-stu-id="f011c-276">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

<span data-ttu-id="f011c-277">Opciones marcadas con un asterisco (\*) no son configurables en el cliente de JavaScript, debido a limitaciones en el Explorador de API.</span><span class="sxs-lookup"><span data-stu-id="f011c-277">Options marked with an asterisk (\*) aren't configurable in the JavaScript client, due to limitations in browser APIs.</span></span>

<span data-ttu-id="f011c-278">En el cliente. NET, estas opciones se pueden modificar por el delegado de las opciones proporcionado para `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="f011c-278">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="f011c-279">En el cliente de JavaScript, estas opciones se pueden proporcionar en un objeto de JavaScript proporcionado a `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="f011c-279">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

## <a name="additional-resources"></a><span data-ttu-id="f011c-280">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="f011c-280">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
