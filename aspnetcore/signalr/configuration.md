---
title: Configuración de SignalR de ASP.NET Core
author: bradygaster
description: Obtenga información sobre cómo configurar ASP.NET Core aplicaciones de SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 12/10/2019
no-loc:
- SignalR
uid: signalr/configuration
ms.openlocfilehash: c225ff88110dc17185a430ac1c422d2433306115
ms.sourcegitcommit: 2cb857f0de774df421e35289662ba92cfe56ffd1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2019
ms.locfileid: "75358117"
---
# <a name="aspnet-core-opno-locsignalr-configuration"></a><span data-ttu-id="64230-103">Configuración de SignalR de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="64230-103">ASP.NET Core SignalR configuration</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="64230-104">Opciones de serialización de JSON/MessagePack</span><span class="sxs-lookup"><span data-stu-id="64230-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="64230-105">ASP.NET Core SignalR admite dos protocolos para codificar los mensajes: [JSON](https://www.json.org/) y [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="64230-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="64230-106">Cada protocolo tiene opciones de configuración de serialización.</span><span class="sxs-lookup"><span data-stu-id="64230-106">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="64230-107">La serialización de JSON se puede configurar en el servidor mediante el método de extensión [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) .</span><span class="sxs-lookup"><span data-stu-id="64230-107">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method.</span></span> <span data-ttu-id="64230-108">`AddJsonProtocol` se puede agregar después de [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="64230-108">`AddJsonProtocol` can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="64230-109">El método `AddJsonProtocol` toma un delegado que recibe un objeto `options`.</span><span class="sxs-lookup"><span data-stu-id="64230-109">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="64230-110">La propiedad [PayloadSerializerOptions](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializeroptions) de ese objeto es una `System.Text.Json` <xref:System.Text.Json.JsonSerializerOptions> objeto que se puede usar para configurar la serialización de argumentos y valores devueltos.</span><span class="sxs-lookup"><span data-stu-id="64230-110">The [PayloadSerializerOptions](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializeroptions) property on that object is a `System.Text.Json` <xref:System.Text.Json.JsonSerializerOptions> object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="64230-111">Para obtener más información, vea la [documentación de System. Text. JSON](/dotnet/api/system.text.json).</span><span class="sxs-lookup"><span data-stu-id="64230-111">For more information, see the [System.Text.Json documentation](/dotnet/api/system.text.json).</span></span>

<span data-ttu-id="64230-112">Por ejemplo, para configurar el serializador para que no cambie el uso de mayúsculas y minúsculas de los nombres de propiedad, en lugar de los nombres de "camelCase" predeterminados, use el código siguiente en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="64230-112">As an example, to configure the serializer to not change the casing of property names, instead of the default "camelCase" names, use the following code in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerOptions.PropertyNamingPolicy = null
    });
```

<span data-ttu-id="64230-113">En el cliente de .NET, el mismo método de extensión `AddJsonProtocol` existe en [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="64230-113">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="64230-114">El espacio de nombres `Microsoft.Extensions.DependencyInjection` se debe importar para resolver el método de extensión:</span><span class="sxs-lookup"><span data-stu-id="64230-114">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

```csharp
// At the top of the file:
using Microsoft.Extensions.DependencyInjection;

// When constructing your connection:
var connection = new HubConnectionBuilder()
    .AddJsonProtocol(options => {
        options.PayloadSerializerOptions.PropertyNamingPolicy = null;
    })
    .Build();
```

> [!NOTE]
> <span data-ttu-id="64230-115">En este momento, no es posible configurar la serialización de JSON en el cliente de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="64230-115">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="switch-to-newtonsoftjson"></a><span data-ttu-id="64230-116">Cambiar a Newtonsoft. JSON</span><span class="sxs-lookup"><span data-stu-id="64230-116">Switch to Newtonsoft.Json</span></span>

<span data-ttu-id="64230-117">Si necesita características de `Newtonsoft.Json` que no se admiten en `System.Text.Json`, consulte [cambiar a Newtonsoft. JSON](xref:migration/22-to-30#switch-to-newtonsoftjson).</span><span class="sxs-lookup"><span data-stu-id="64230-117">If you need features of `Newtonsoft.Json` that aren't supported in `System.Text.Json`, See [Switch to Newtonsoft.Json](xref:migration/22-to-30#switch-to-newtonsoftjson).</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="64230-118">Opciones de serialización de MessagePack</span><span class="sxs-lookup"><span data-stu-id="64230-118">MessagePack serialization options</span></span>

<span data-ttu-id="64230-119">La serialización de MessagePack se puede configurar proporcionando un delegado a la llamada a [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) .</span><span class="sxs-lookup"><span data-stu-id="64230-119">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="64230-120">Vea [MessagePack en SignalR](xref:signalr/messagepackhubprotocol) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="64230-120">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="64230-121">En este momento, no es posible configurar la serialización de MessagePack en el cliente de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="64230-121">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="64230-122">Configurar opciones de servidor</span><span class="sxs-lookup"><span data-stu-id="64230-122">Configure server options</span></span>

<span data-ttu-id="64230-123">En la tabla siguiente se describen las opciones para configurar los concentradores de SignalR:</span><span class="sxs-lookup"><span data-stu-id="64230-123">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="64230-124">Opción</span><span class="sxs-lookup"><span data-stu-id="64230-124">Option</span></span> | <span data-ttu-id="64230-125">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="64230-125">Default Value</span></span> | <span data-ttu-id="64230-126">Descripción</span><span class="sxs-lookup"><span data-stu-id="64230-126">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="64230-127">30 segundos</span><span class="sxs-lookup"><span data-stu-id="64230-127">30 seconds</span></span> | <span data-ttu-id="64230-128">El servidor considerará que el cliente se ha desconectado Si no ha recibido un mensaje (incluido Keep-Alive) en este intervalo.</span><span class="sxs-lookup"><span data-stu-id="64230-128">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="64230-129">Podría tardar más de este intervalo de tiempo de espera para que el cliente se marque realmente como desconectado, debido a la manera en que se implementa.</span><span class="sxs-lookup"><span data-stu-id="64230-129">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="64230-130">El valor recomendado es el doble del valor de `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="64230-130">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="64230-131">15 segundos</span><span class="sxs-lookup"><span data-stu-id="64230-131">15 seconds</span></span> | <span data-ttu-id="64230-132">Si el cliente no envía un mensaje de enlace inicial dentro de este intervalo de tiempo, la conexión se cierra.</span><span class="sxs-lookup"><span data-stu-id="64230-132">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="64230-133">Se trata de una configuración avanzada que solo se debe modificar si se producen errores de tiempo de espera del Protocolo de enlace debido a una latencia de red grave.</span><span class="sxs-lookup"><span data-stu-id="64230-133">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="64230-134">Para obtener más información sobre el proceso de enlace, consulte la [especificación del Protocolo de concentrador deSignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="64230-134">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="64230-135">15 segundos</span><span class="sxs-lookup"><span data-stu-id="64230-135">15 seconds</span></span> | <span data-ttu-id="64230-136">Si el servidor no ha enviado un mensaje dentro de este intervalo, se envía automáticamente un mensaje ping para mantener abierta la conexión.</span><span class="sxs-lookup"><span data-stu-id="64230-136">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="64230-137">Al cambiar `KeepAliveInterval`, cambie la configuración de `serverTimeoutInMilliseconds` de /de `ServerTimeout`en el cliente.</span><span class="sxs-lookup"><span data-stu-id="64230-137">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="64230-138">El `ServerTimeout`recomendado /valor de `serverTimeoutInMilliseconds` es el doble del valor de `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="64230-138">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="64230-139">Todos los protocolos instalados</span><span class="sxs-lookup"><span data-stu-id="64230-139">All installed protocols</span></span> | <span data-ttu-id="64230-140">Protocolos admitidos por este centro.</span><span class="sxs-lookup"><span data-stu-id="64230-140">Protocols supported by this hub.</span></span> <span data-ttu-id="64230-141">De forma predeterminada, todos los protocolos registrados en el servidor están permitidos, pero los protocolos se pueden quitar de esta lista para deshabilitar determinados protocolos para centros individuales.</span><span class="sxs-lookup"><span data-stu-id="64230-141">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="64230-142">Si `true`, los mensajes de excepción detallados se devuelven a los clientes cuando se produce una excepción en un método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="64230-142">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="64230-143">El valor predeterminado es `false`, ya que estos mensajes de excepción pueden contener información confidencial.</span><span class="sxs-lookup"><span data-stu-id="64230-143">The default is `false`, as these exception messages can contain sensitive information.</span></span> |
| `StreamBufferCapacity` | `10` | <span data-ttu-id="64230-144">Número máximo de elementos que se pueden almacenar en búfer para las secuencias de carga de cliente.</span><span class="sxs-lookup"><span data-stu-id="64230-144">The maximum number of items that can be buffered for client upload streams.</span></span> <span data-ttu-id="64230-145">Si se alcanza este límite, el procesamiento de las invocaciones se bloquea hasta que el servidor procesa los elementos de la secuencia.</span><span class="sxs-lookup"><span data-stu-id="64230-145">If this limit is reached, the processing of invocations is blocked until the server processes stream items.</span></span>|
| `MaximumReceiveMessageSize` | <span data-ttu-id="64230-146">32 KB</span><span class="sxs-lookup"><span data-stu-id="64230-146">32 KB</span></span> | <span data-ttu-id="64230-147">Tamaño máximo de un único mensaje de concentrador entrante.</span><span class="sxs-lookup"><span data-stu-id="64230-147">Maximum size of a single incoming hub message.</span></span> |

<span data-ttu-id="64230-148">Las opciones se pueden configurar para todos los concentradores proporcionando un delegado de opciones a la llamada `AddSignalR` en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="64230-148">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="64230-149">Las opciones de un solo concentrador invalidan las opciones globales proporcionadas en `AddSignalR` y se pueden configurar mediante <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span><span class="sxs-lookup"><span data-stu-id="64230-149">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="64230-150">Opciones de configuración de HTTP avanzadas</span><span class="sxs-lookup"><span data-stu-id="64230-150">Advanced HTTP configuration options</span></span>

<span data-ttu-id="64230-151">Use `HttpConnectionDispatcherOptions` para configurar opciones avanzadas relacionadas con los transportes y la administración del búfer de memoria.</span><span class="sxs-lookup"><span data-stu-id="64230-151">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="64230-152">Estas opciones se configuran pasando un delegado a [MapHub\<t >](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub) en `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="64230-152">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub) in `Startup.Configure`.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseRouting();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapHub<MyHub>("/myhub", options =>
        {
            options.Transports =
                HttpTransportType.WebSockets |
                HttpTransportType.LongPolling;
        });
    });
}
```

<span data-ttu-id="64230-153">En la tabla siguiente se describen las opciones para configurar las opciones de HTTP avanzadas de ASP.NET Core SignalR:</span><span class="sxs-lookup"><span data-stu-id="64230-153">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

| <span data-ttu-id="64230-154">Opción</span><span class="sxs-lookup"><span data-stu-id="64230-154">Option</span></span> | <span data-ttu-id="64230-155">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="64230-155">Default Value</span></span> | <span data-ttu-id="64230-156">Descripción</span><span class="sxs-lookup"><span data-stu-id="64230-156">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="64230-157">32 KB</span><span class="sxs-lookup"><span data-stu-id="64230-157">32 KB</span></span> | <span data-ttu-id="64230-158">Número máximo de bytes recibidos del cliente que el servidor almacena en búfer antes de aplicar la presión de reserva.</span><span class="sxs-lookup"><span data-stu-id="64230-158">The maximum number of bytes received from the client that the server buffers before applying backpressure.</span></span> <span data-ttu-id="64230-159">Aumentar este valor permite que el servidor reciba mensajes más grandes con más rapidez sin aplicar la presión, pero puede aumentar el consumo de memoria.</span><span class="sxs-lookup"><span data-stu-id="64230-159">Increasing this value allows the server to receive larger messages more quickly without applying backpressure, but can increase memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="64230-160">Los datos se recopilan automáticamente a partir de los atributos `Authorize` aplicados a la clase hub.</span><span class="sxs-lookup"><span data-stu-id="64230-160">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="64230-161">Una lista de objetos [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) que se usan para determinar si un cliente está autorizado para conectarse al centro.</span><span class="sxs-lookup"><span data-stu-id="64230-161">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="64230-162">32 KB</span><span class="sxs-lookup"><span data-stu-id="64230-162">32 KB</span></span> | <span data-ttu-id="64230-163">Número máximo de bytes enviados por la aplicación que el servidor almacena en búfer antes de observar la contrapresión.</span><span class="sxs-lookup"><span data-stu-id="64230-163">The maximum number of bytes sent by the app that the server buffers before observing backpressure.</span></span> <span data-ttu-id="64230-164">El aumento de este valor permite que el servidor almacene en búfer mensajes más grandes con más rapidez sin esperar la presión, pero puede aumentar el consumo de memoria.</span><span class="sxs-lookup"><span data-stu-id="64230-164">Increasing this value allows the server to buffer larger messages more quickly without awaiting backpressure, but can increase memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="64230-165">Todos los transportes están habilitados.</span><span class="sxs-lookup"><span data-stu-id="64230-165">All Transports are enabled.</span></span> | <span data-ttu-id="64230-166">Una enumeración de marcas de bits de `HttpTransportType` valores que pueden restringir los transportes que un cliente puede utilizar para conectarse.</span><span class="sxs-lookup"><span data-stu-id="64230-166">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="64230-167">Véalo a continuación.</span><span class="sxs-lookup"><span data-stu-id="64230-167">See below.</span></span> | <span data-ttu-id="64230-168">Opciones adicionales específicas del transporte de sondeo prolongado.</span><span class="sxs-lookup"><span data-stu-id="64230-168">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="64230-169">Véalo a continuación.</span><span class="sxs-lookup"><span data-stu-id="64230-169">See below.</span></span> | <span data-ttu-id="64230-170">Opciones adicionales específicas del transporte de WebSockets.</span><span class="sxs-lookup"><span data-stu-id="64230-170">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="64230-171">El transporte de sondeo largo tiene opciones adicionales que se pueden configurar mediante la propiedad `LongPolling`:</span><span class="sxs-lookup"><span data-stu-id="64230-171">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="64230-172">Opción</span><span class="sxs-lookup"><span data-stu-id="64230-172">Option</span></span> | <span data-ttu-id="64230-173">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="64230-173">Default Value</span></span> | <span data-ttu-id="64230-174">Descripción</span><span class="sxs-lookup"><span data-stu-id="64230-174">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="64230-175">90 segundos</span><span class="sxs-lookup"><span data-stu-id="64230-175">90 seconds</span></span> | <span data-ttu-id="64230-176">Cantidad máxima de tiempo que el servidor espera a que se envíe un mensaje al cliente antes de finalizar una única solicitud de sondeo.</span><span class="sxs-lookup"><span data-stu-id="64230-176">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="64230-177">Al disminuir este valor, el cliente emite nuevas solicitudes de sondeo con mayor frecuencia.</span><span class="sxs-lookup"><span data-stu-id="64230-177">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="64230-178">El transporte de WebSocket tiene opciones adicionales que se pueden configurar mediante la propiedad `WebSockets`:</span><span class="sxs-lookup"><span data-stu-id="64230-178">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="64230-179">Opción</span><span class="sxs-lookup"><span data-stu-id="64230-179">Option</span></span> | <span data-ttu-id="64230-180">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="64230-180">Default Value</span></span> | <span data-ttu-id="64230-181">Descripción</span><span class="sxs-lookup"><span data-stu-id="64230-181">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="64230-182">5 segundos</span><span class="sxs-lookup"><span data-stu-id="64230-182">5 seconds</span></span> | <span data-ttu-id="64230-183">Una vez que se cierra el servidor, si el cliente no se cierra dentro de este intervalo de tiempo, se finaliza la conexión.</span><span class="sxs-lookup"><span data-stu-id="64230-183">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="64230-184">Delegado que se puede utilizar para establecer el encabezado de `Sec-WebSocket-Protocol` en un valor personalizado.</span><span class="sxs-lookup"><span data-stu-id="64230-184">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="64230-185">El delegado recibe los valores solicitados por el cliente como entrada y se espera que devuelva el valor deseado.</span><span class="sxs-lookup"><span data-stu-id="64230-185">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="64230-186">Configurar opciones de cliente</span><span class="sxs-lookup"><span data-stu-id="64230-186">Configure client options</span></span>

<span data-ttu-id="64230-187">Las opciones de cliente se pueden configurar en el tipo de `HubConnectionBuilder` (disponible en los clientes de .NET y JavaScript).</span><span class="sxs-lookup"><span data-stu-id="64230-187">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="64230-188">También está disponible en el cliente de Java, pero la subclase `HttpHubConnectionBuilder` es lo que contiene las opciones de configuración del generador, así como en el `HubConnection` mismo.</span><span class="sxs-lookup"><span data-stu-id="64230-188">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="64230-189">registro</span><span class="sxs-lookup"><span data-stu-id="64230-189">Configure logging</span></span>

<span data-ttu-id="64230-190">El registro se configura en el cliente .NET mediante el método `ConfigureLogging`.</span><span class="sxs-lookup"><span data-stu-id="64230-190">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="64230-191">Los proveedores de registro y los filtros se pueden registrar de la misma manera que en el servidor.</span><span class="sxs-lookup"><span data-stu-id="64230-191">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="64230-192">Consulte la documentación de [Inicio de sesión ASP.net Core](xref:fundamentals/logging/index) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="64230-192">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="64230-193">Para registrar los proveedores de registro, debe instalar los paquetes necesarios.</span><span class="sxs-lookup"><span data-stu-id="64230-193">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="64230-194">Consulte la sección [proveedores de registro integrados](xref:fundamentals/logging/index#built-in-logging-providers) de los documentos para obtener una lista completa.</span><span class="sxs-lookup"><span data-stu-id="64230-194">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="64230-195">Por ejemplo, para habilitar el registro de la consola, instale el paquete NuGet `Microsoft.Extensions.Logging.Console`.</span><span class="sxs-lookup"><span data-stu-id="64230-195">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="64230-196">Llame al método de extensión `AddConsole`:</span><span class="sxs-lookup"><span data-stu-id="64230-196">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="64230-197">En el cliente de JavaScript, existe un método `configureLogging` similar.</span><span class="sxs-lookup"><span data-stu-id="64230-197">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="64230-198">Proporcione un valor `LogLevel` que indique el nivel mínimo de mensajes de registro que se va a producir.</span><span class="sxs-lookup"><span data-stu-id="64230-198">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="64230-199">Los registros se escriben en la ventana de la consola del explorador.</span><span class="sxs-lookup"><span data-stu-id="64230-199">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

<span data-ttu-id="64230-200">En lugar de un valor `LogLevel`, también puede proporcionar un valor `string` que represente un nombre de nivel de registro.</span><span class="sxs-lookup"><span data-stu-id="64230-200">Instead of a `LogLevel` value, you can also provide a `string` value representing a log level name.</span></span> <span data-ttu-id="64230-201">Esto resulta útil al configurar el registro de SignalR en entornos en los que no tiene acceso a las constantes de `LogLevel`.</span><span class="sxs-lookup"><span data-stu-id="64230-201">This is useful when configuring SignalR logging in environments where you don't have access to the `LogLevel` constants.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging("warn")
    .build();
```

<span data-ttu-id="64230-202">En la tabla siguiente se enumeran los niveles de registro disponibles.</span><span class="sxs-lookup"><span data-stu-id="64230-202">The following table lists the available log levels.</span></span> <span data-ttu-id="64230-203">El valor que se proporciona a `configureLogging` establece el nivel de registro **mínimo** que se registrará.</span><span class="sxs-lookup"><span data-stu-id="64230-203">The value you provide to `configureLogging` sets the **minimum** log level that will be logged.</span></span> <span data-ttu-id="64230-204">Se registrarán los mensajes registrados en este nivel, **o los niveles que se enumeran en la tabla**.</span><span class="sxs-lookup"><span data-stu-id="64230-204">Messages logged at this level, **or the levels listed after it in the table**, will be logged.</span></span>

| <span data-ttu-id="64230-205">Cadena</span><span class="sxs-lookup"><span data-stu-id="64230-205">String</span></span>                      | <span data-ttu-id="64230-206">LogLevel</span><span class="sxs-lookup"><span data-stu-id="64230-206">LogLevel</span></span>               |
| --------------------------- | ---------------------- |
| `trace`                     | `LogLevel.Trace`       |
| `debug`                     | `LogLevel.Debug`       |
| <span data-ttu-id="64230-207">`info` **o** `information`</span><span class="sxs-lookup"><span data-stu-id="64230-207">`info` **or** `information`</span></span> | `LogLevel.Information` |
| <span data-ttu-id="64230-208">`warn` **o** `warning`</span><span class="sxs-lookup"><span data-stu-id="64230-208">`warn` **or** `warning`</span></span>     | `LogLevel.Warning`     |
| `error`                     | `LogLevel.Error`       |
| `critical`                  | `LogLevel.Critical`    |
| `none`                      | `LogLevel.None`        |

> [!NOTE]
> <span data-ttu-id="64230-209">Para deshabilitar el registro por completo, especifique `signalR.LogLevel.None` en el método `configureLogging`.</span><span class="sxs-lookup"><span data-stu-id="64230-209">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="64230-210">Para obtener más información sobre el registro, consulte la [documentación de diagnóstico deSignalR](xref:signalr/diagnostics).</span><span class="sxs-lookup"><span data-stu-id="64230-210">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="64230-211">El cliente de Java de SignalR usa la biblioteca [SLF4J](https://www.slf4j.org/) para el registro.</span><span class="sxs-lookup"><span data-stu-id="64230-211">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="64230-212">Se trata de una API de registro de alto nivel que permite a los usuarios de la biblioteca elegir su propia implementación de registro específica mediante la incorporación de una dependencia de registro específica.</span><span class="sxs-lookup"><span data-stu-id="64230-212">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="64230-213">En el fragmento de código siguiente se muestra cómo usar `java.util.logging` con el cliente de Java SignalR.</span><span class="sxs-lookup"><span data-stu-id="64230-213">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="64230-214">Si no configura el registro en las dependencias, SLF4J carga un registrador de no operaciones predeterminado con el siguiente mensaje de ADVERTENCIA:</span><span class="sxs-lookup"><span data-stu-id="64230-214">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="64230-215">Esto se puede omitir sin ningún riesgo.</span><span class="sxs-lookup"><span data-stu-id="64230-215">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="64230-216">Configurar transportes permitidos</span><span class="sxs-lookup"><span data-stu-id="64230-216">Configure allowed transports</span></span>

<span data-ttu-id="64230-217">Los transportes utilizados por SignalR pueden configurarse en la llamada `WithUrl` (`withUrl` en JavaScript).</span><span class="sxs-lookup"><span data-stu-id="64230-217">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="64230-218">Una operación OR bit a bit de los valores de `HttpTransportType` se puede utilizar para restringir el cliente de modo que solo utilice los transportes especificados.</span><span class="sxs-lookup"><span data-stu-id="64230-218">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="64230-219">Todos los transportes están habilitados de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="64230-219">All transports are enabled by default.</span></span>

<span data-ttu-id="64230-220">Por ejemplo, para deshabilitar el transporte de eventos enviados por el servidor, pero permitir WebSockets y conexiones de sondeo largas:</span><span class="sxs-lookup"><span data-stu-id="64230-220">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="64230-221">En el cliente de JavaScript, los transportes se configuran estableciendo el campo `transport` en el objeto de opciones proporcionado para `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="64230-221">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

<span data-ttu-id="64230-222">En esta versión del cliente de Java, WebSockets es el único transporte disponible.</span><span class="sxs-lookup"><span data-stu-id="64230-222">In this version of the Java client websockets is the only available transport.</span></span>

<span data-ttu-id="64230-223">En el cliente de Java, el transporte se selecciona con el método `withTransport` en el `HttpHubConnectionBuilder`.</span><span class="sxs-lookup"><span data-stu-id="64230-223">In the Java client, the transport is selected with the `withTransport` method on the `HttpHubConnectionBuilder`.</span></span> <span data-ttu-id="64230-224">De forma predeterminada, el cliente de Java usa el transporte de WebSockets.</span><span class="sxs-lookup"><span data-stu-id="64230-224">The Java client defaults to using the WebSockets transport.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withTransport(TransportEnum.WEBSOCKETS)
    .build();
```

> [!NOTE]
> <span data-ttu-id="64230-225">El cliente de Java SignalR no es compatible aún con la reserva de transporte.</span><span class="sxs-lookup"><span data-stu-id="64230-225">The SignalR Java client doesn't support transport fallback yet.</span></span>

### <a name="configure-bearer-authentication"></a><span data-ttu-id="64230-226">Configurar la autenticación de portador</span><span class="sxs-lookup"><span data-stu-id="64230-226">Configure bearer authentication</span></span>

<span data-ttu-id="64230-227">Para proporcionar datos de autenticación junto con solicitudes de SignalR, use la opción `AccessTokenProvider` (`accessTokenFactory` en JavaScript) para especificar una función que devuelva el token de acceso deseado.</span><span class="sxs-lookup"><span data-stu-id="64230-227">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="64230-228">En el cliente .NET, este token de acceso se pasa como un token de "autenticación de portador" de HTTP (mediante el encabezado `Authorization` con un tipo de `Bearer`).</span><span class="sxs-lookup"><span data-stu-id="64230-228">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="64230-229">En el cliente de JavaScript, el token de acceso se usa como un token de portador, **excepto** en algunos casos en los que las API del explorador restringen la capacidad de aplicar encabezados (específicamente, en los eventos enviados por el servidor y en las solicitudes de WebSockets).</span><span class="sxs-lookup"><span data-stu-id="64230-229">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="64230-230">En estos casos, el token de acceso se proporciona como un valor de cadena de consulta `access_token`.</span><span class="sxs-lookup"><span data-stu-id="64230-230">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="64230-231">En el cliente .NET, se puede especificar la opción `AccessTokenProvider` mediante el delegado Options en `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="64230-231">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="64230-232">En el cliente de JavaScript, el token de acceso se configura estableciendo el campo `accessTokenFactory` del objeto de opciones en `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="64230-232">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

<span data-ttu-id="64230-233">En el cliente de SignalR Java, puede configurar un token de portador que se usará para la autenticación proporcionando un generador de tokens de acceso a [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span><span class="sxs-lookup"><span data-stu-id="64230-233">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="64230-234">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) para proporcionar un [RxJava](https://github.com/ReactiveX/RxJava) de una [única\<cadena>](https://reactivex.io/documentation/single.html).</span><span class="sxs-lookup"><span data-stu-id="64230-234">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="64230-235">Con una llamada a [Single. defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), puede escribir lógica para generar tokens de acceso para el cliente.</span><span class="sxs-lookup"><span data-stu-id="64230-235">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="64230-236">Configurar las opciones de tiempo de espera y mantenimiento de conexión</span><span class="sxs-lookup"><span data-stu-id="64230-236">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="64230-237">Las opciones adicionales para configurar el comportamiento de tiempo de espera y mantenimiento de conexión están disponibles en el propio `HubConnection` objeto:</span><span class="sxs-lookup"><span data-stu-id="64230-237">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

# <a name="nettabdotnet"></a>[<span data-ttu-id="64230-238">.NET</span><span class="sxs-lookup"><span data-stu-id="64230-238">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="64230-239">Opción</span><span class="sxs-lookup"><span data-stu-id="64230-239">Option</span></span> | <span data-ttu-id="64230-240">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="64230-240">Default value</span></span> | <span data-ttu-id="64230-241">Descripción</span><span class="sxs-lookup"><span data-stu-id="64230-241">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="64230-242">30 segundos (30.000 milisegundos)</span><span class="sxs-lookup"><span data-stu-id="64230-242">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="64230-243">Tiempo de espera para la actividad del servidor.</span><span class="sxs-lookup"><span data-stu-id="64230-243">Timeout for server activity.</span></span> <span data-ttu-id="64230-244">Si el servidor no ha enviado un mensaje en este intervalo, el cliente considera que el servidor está desconectado y desencadena el evento `Closed` (`onclose` en JavaScript).</span><span class="sxs-lookup"><span data-stu-id="64230-244">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="64230-245">Este valor debe ser lo suficientemente grande como para que se envíe un mensaje ping desde el servidor **y** el cliente lo reciba dentro del intervalo de tiempo de espera.</span><span class="sxs-lookup"><span data-stu-id="64230-245">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="64230-246">El valor recomendado es un número al menos el doble del valor del `KeepAliveInterval` del servidor para permitir que lleguen los ping.</span><span class="sxs-lookup"><span data-stu-id="64230-246">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="64230-247">15 segundos</span><span class="sxs-lookup"><span data-stu-id="64230-247">15 seconds</span></span> | <span data-ttu-id="64230-248">Tiempo de espera para el protocolo de enlace inicial del servidor.</span><span class="sxs-lookup"><span data-stu-id="64230-248">Timeout for initial server handshake.</span></span> <span data-ttu-id="64230-249">Si el servidor no envía una respuesta de protocolo de enlace en este intervalo, el cliente cancela el protocolo de enlace y desencadena el evento `Closed` (`onclose` en JavaScript).</span><span class="sxs-lookup"><span data-stu-id="64230-249">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="64230-250">Se trata de una configuración avanzada que solo se debe modificar si se producen errores de tiempo de espera del Protocolo de enlace debido a una latencia de red grave.</span><span class="sxs-lookup"><span data-stu-id="64230-250">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="64230-251">Para obtener más información sobre el proceso de enlace, consulte la [especificación del Protocolo de concentrador deSignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="64230-251">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="64230-252">15 segundos</span><span class="sxs-lookup"><span data-stu-id="64230-252">15 seconds</span></span> | <span data-ttu-id="64230-253">Determina el intervalo en el que el cliente envía mensajes de ping.</span><span class="sxs-lookup"><span data-stu-id="64230-253">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="64230-254">Al enviar cualquier mensaje desde el cliente, se restablece el temporizador al inicio del intervalo.</span><span class="sxs-lookup"><span data-stu-id="64230-254">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="64230-255">Si el cliente no ha enviado un mensaje en el `ClientTimeoutInterval` establecido en el servidor, el servidor considera que el cliente está desconectado.</span><span class="sxs-lookup"><span data-stu-id="64230-255">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

<span data-ttu-id="64230-256">En el cliente de .NET, los valores de tiempo de espera se especifican como valores de `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="64230-256">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="64230-257">JavaScript</span><span class="sxs-lookup"><span data-stu-id="64230-257">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="64230-258">Opción</span><span class="sxs-lookup"><span data-stu-id="64230-258">Option</span></span> | <span data-ttu-id="64230-259">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="64230-259">Default value</span></span> | <span data-ttu-id="64230-260">Descripción</span><span class="sxs-lookup"><span data-stu-id="64230-260">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="64230-261">30 segundos (30.000 milisegundos)</span><span class="sxs-lookup"><span data-stu-id="64230-261">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="64230-262">Tiempo de espera para la actividad del servidor.</span><span class="sxs-lookup"><span data-stu-id="64230-262">Timeout for server activity.</span></span> <span data-ttu-id="64230-263">Si el servidor no ha enviado un mensaje en este intervalo, el cliente considera que el servidor está desconectado y desencadena el evento `onclose`.</span><span class="sxs-lookup"><span data-stu-id="64230-263">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="64230-264">Este valor debe ser lo suficientemente grande como para que se envíe un mensaje ping desde el servidor **y** el cliente lo reciba dentro del intervalo de tiempo de espera.</span><span class="sxs-lookup"><span data-stu-id="64230-264">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="64230-265">El valor recomendado es un número al menos el doble del valor del `KeepAliveInterval` del servidor para permitir que lleguen los ping.</span><span class="sxs-lookup"><span data-stu-id="64230-265">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `keepAliveIntervalInMilliseconds` | <span data-ttu-id="64230-266">15 segundos (15.000 milisegundos)</span><span class="sxs-lookup"><span data-stu-id="64230-266">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="64230-267">Determina el intervalo en el que el cliente envía mensajes de ping.</span><span class="sxs-lookup"><span data-stu-id="64230-267">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="64230-268">Al enviar cualquier mensaje desde el cliente, se restablece el temporizador al inicio del intervalo.</span><span class="sxs-lookup"><span data-stu-id="64230-268">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="64230-269">Si el cliente no ha enviado un mensaje en el `ClientTimeoutInterval` establecido en el servidor, el servidor considera que el cliente está desconectado.</span><span class="sxs-lookup"><span data-stu-id="64230-269">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="64230-270">Java</span><span class="sxs-lookup"><span data-stu-id="64230-270">Java</span></span>](#tab/java)

| <span data-ttu-id="64230-271">Opción</span><span class="sxs-lookup"><span data-stu-id="64230-271">Option</span></span> | <span data-ttu-id="64230-272">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="64230-272">Default value</span></span> | <span data-ttu-id="64230-273">Descripción</span><span class="sxs-lookup"><span data-stu-id="64230-273">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="64230-274">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="64230-274">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="64230-275">30 segundos (30.000 milisegundos)</span><span class="sxs-lookup"><span data-stu-id="64230-275">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="64230-276">Tiempo de espera para la actividad del servidor.</span><span class="sxs-lookup"><span data-stu-id="64230-276">Timeout for server activity.</span></span> <span data-ttu-id="64230-277">Si el servidor no ha enviado un mensaje en este intervalo, el cliente considera que el servidor está desconectado y desencadena el evento `onClose`.</span><span class="sxs-lookup"><span data-stu-id="64230-277">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="64230-278">Este valor debe ser lo suficientemente grande como para que se envíe un mensaje ping desde el servidor **y** el cliente lo reciba dentro del intervalo de tiempo de espera.</span><span class="sxs-lookup"><span data-stu-id="64230-278">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="64230-279">El valor recomendado es un número al menos el doble del valor del `KeepAliveInterval` del servidor para permitir que lleguen los ping.</span><span class="sxs-lookup"><span data-stu-id="64230-279">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="64230-280">15 segundos</span><span class="sxs-lookup"><span data-stu-id="64230-280">15 seconds</span></span> | <span data-ttu-id="64230-281">Tiempo de espera para el protocolo de enlace inicial del servidor.</span><span class="sxs-lookup"><span data-stu-id="64230-281">Timeout for initial server handshake.</span></span> <span data-ttu-id="64230-282">Si el servidor no envía una respuesta de protocolo de enlace en este intervalo, el cliente cancela el protocolo de enlace y desencadena el evento `onClose`.</span><span class="sxs-lookup"><span data-stu-id="64230-282">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="64230-283">Se trata de una configuración avanzada que solo se debe modificar si se producen errores de tiempo de espera del Protocolo de enlace debido a una latencia de red grave.</span><span class="sxs-lookup"><span data-stu-id="64230-283">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="64230-284">Para obtener más información sobre el proceso de enlace, consulte la [especificación del Protocolo de concentrador deSignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="64230-284">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| <span data-ttu-id="64230-285">`getKeepAliveInterval` / `setKeepAliveInterval`</span><span class="sxs-lookup"><span data-stu-id="64230-285">`getKeepAliveInterval` / `setKeepAliveInterval`</span></span> | <span data-ttu-id="64230-286">15 segundos (15.000 milisegundos)</span><span class="sxs-lookup"><span data-stu-id="64230-286">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="64230-287">Determina el intervalo en el que el cliente envía mensajes de ping.</span><span class="sxs-lookup"><span data-stu-id="64230-287">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="64230-288">Al enviar cualquier mensaje desde el cliente, se restablece el temporizador al inicio del intervalo.</span><span class="sxs-lookup"><span data-stu-id="64230-288">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="64230-289">Si el cliente no ha enviado un mensaje en el `ClientTimeoutInterval` establecido en el servidor, el servidor considera que el cliente está desconectado.</span><span class="sxs-lookup"><span data-stu-id="64230-289">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

---

### <a name="configure-additional-options"></a><span data-ttu-id="64230-290">Configuración de opciones adicionales</span><span class="sxs-lookup"><span data-stu-id="64230-290">Configure additional options</span></span>

<span data-ttu-id="64230-291">Se pueden configurar opciones adicionales en el método `WithUrl` (`withUrl` en JavaScript) en `HubConnectionBuilder` o en las diversas API de configuración de la `HttpHubConnectionBuilder` en el cliente de Java:</span><span class="sxs-lookup"><span data-stu-id="64230-291">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="nettabdotnet"></a>[<span data-ttu-id="64230-292">.NET</span><span class="sxs-lookup"><span data-stu-id="64230-292">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="64230-293">Opción de .NET</span><span class="sxs-lookup"><span data-stu-id="64230-293">.NET Option</span></span> |  <span data-ttu-id="64230-294">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="64230-294">Default value</span></span> | <span data-ttu-id="64230-295">Descripción</span><span class="sxs-lookup"><span data-stu-id="64230-295">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="64230-296">Función que devuelve una cadena que se proporciona como un token de autenticación de portador en solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="64230-296">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="64230-297">Establezca este valor en `true` para omitir el paso de negociación.</span><span class="sxs-lookup"><span data-stu-id="64230-297">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="64230-298">**Solo se admite cuando el transporte de WebSockets es el único transporte habilitado**.</span><span class="sxs-lookup"><span data-stu-id="64230-298">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="64230-299">Esta configuración no se puede habilitar cuando se usa el servicio Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="64230-299">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="64230-300">Empty</span><span class="sxs-lookup"><span data-stu-id="64230-300">Empty</span></span> | <span data-ttu-id="64230-301">Colección de certificados TLS que se enviarán a las solicitudes de autenticación.</span><span class="sxs-lookup"><span data-stu-id="64230-301">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="64230-302">Empty</span><span class="sxs-lookup"><span data-stu-id="64230-302">Empty</span></span> | <span data-ttu-id="64230-303">Colección de cookies HTTP que se enviarán con cada solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="64230-303">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="64230-304">Empty</span><span class="sxs-lookup"><span data-stu-id="64230-304">Empty</span></span> | <span data-ttu-id="64230-305">Credenciales que se van a enviar con cada solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="64230-305">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="64230-306">5 segundos</span><span class="sxs-lookup"><span data-stu-id="64230-306">5 seconds</span></span> | <span data-ttu-id="64230-307">Solo WebSockets.</span><span class="sxs-lookup"><span data-stu-id="64230-307">WebSockets only.</span></span> <span data-ttu-id="64230-308">Cantidad máxima de tiempo que el cliente espera después de cerrarse para que el servidor confirme la solicitud de cierre.</span><span class="sxs-lookup"><span data-stu-id="64230-308">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="64230-309">Si el servidor no reconoce el cierre dentro de este tiempo, el cliente se desconecta.</span><span class="sxs-lookup"><span data-stu-id="64230-309">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="64230-310">Empty</span><span class="sxs-lookup"><span data-stu-id="64230-310">Empty</span></span> | <span data-ttu-id="64230-311">Asignación de encabezados HTTP adicionales que se van a enviar con cada solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="64230-311">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="64230-312">Delegado que se puede utilizar para configurar o reemplazar el `HttpMessageHandler` utilizado para enviar solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="64230-312">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="64230-313">No se usa para las conexiones WebSocket.</span><span class="sxs-lookup"><span data-stu-id="64230-313">Not used for WebSocket connections.</span></span> <span data-ttu-id="64230-314">Este delegado debe devolver un valor distinto de NULL y recibe el valor predeterminado como parámetro.</span><span class="sxs-lookup"><span data-stu-id="64230-314">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="64230-315">Modifique la configuración de ese valor predeterminado y devuelva o devuelva una nueva instancia de `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="64230-315">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="64230-316">**Al reemplazar el controlador, asegúrese de copiar la configuración que desea conservar del controlador proporcionado; de lo contrario, las opciones configuradas (como cookies y encabezados) no se aplicarán al nuevo controlador.**</span><span class="sxs-lookup"><span data-stu-id="64230-316">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="64230-317">Proxy HTTP que se va a usar al enviar solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="64230-317">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="64230-318">Establezca este valor booleano para enviar las credenciales predeterminadas para las solicitudes HTTP y WebSockets.</span><span class="sxs-lookup"><span data-stu-id="64230-318">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="64230-319">Esto habilita el uso de la autenticación de Windows.</span><span class="sxs-lookup"><span data-stu-id="64230-319">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="64230-320">Delegado que se puede usar para configurar opciones adicionales de WebSocket.</span><span class="sxs-lookup"><span data-stu-id="64230-320">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="64230-321">Recibe una instancia de [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) que se puede usar para configurar las opciones.</span><span class="sxs-lookup"><span data-stu-id="64230-321">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="64230-322">JavaScript</span><span class="sxs-lookup"><span data-stu-id="64230-322">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="64230-323">Opción de JavaScript</span><span class="sxs-lookup"><span data-stu-id="64230-323">JavaScript Option</span></span> | <span data-ttu-id="64230-324">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="64230-324">Default Value</span></span> | <span data-ttu-id="64230-325">Descripción</span><span class="sxs-lookup"><span data-stu-id="64230-325">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="64230-326">Función que devuelve una cadena que se proporciona como un token de autenticación de portador en solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="64230-326">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="64230-327">Establezca este valor en `true` para omitir el paso de negociación.</span><span class="sxs-lookup"><span data-stu-id="64230-327">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="64230-328">**Solo se admite cuando el transporte de WebSockets es el único transporte habilitado**.</span><span class="sxs-lookup"><span data-stu-id="64230-328">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="64230-329">Esta configuración no se puede habilitar cuando se usa el servicio Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="64230-329">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="64230-330">Java</span><span class="sxs-lookup"><span data-stu-id="64230-330">Java</span></span>](#tab/java)

| <span data-ttu-id="64230-331">Opción de Java</span><span class="sxs-lookup"><span data-stu-id="64230-331">Java Option</span></span> | <span data-ttu-id="64230-332">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="64230-332">Default Value</span></span> | <span data-ttu-id="64230-333">Descripción</span><span class="sxs-lookup"><span data-stu-id="64230-333">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="64230-334">Función que devuelve una cadena que se proporciona como un token de autenticación de portador en solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="64230-334">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="64230-335">Establezca este valor en `true` para omitir el paso de negociación.</span><span class="sxs-lookup"><span data-stu-id="64230-335">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="64230-336">**Solo se admite cuando el transporte de WebSockets es el único transporte habilitado**.</span><span class="sxs-lookup"><span data-stu-id="64230-336">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="64230-337">Esta configuración no se puede habilitar cuando se usa el servicio Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="64230-337">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="64230-338">`withHeader` `withHeaders`</span><span class="sxs-lookup"><span data-stu-id="64230-338">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="64230-339">Empty</span><span class="sxs-lookup"><span data-stu-id="64230-339">Empty</span></span> | <span data-ttu-id="64230-340">Asignación de encabezados HTTP adicionales que se van a enviar con cada solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="64230-340">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="64230-341">En el cliente .NET, estas opciones se pueden modificar mediante el delegado de opciones proporcionado a `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="64230-341">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="64230-342">En el cliente de JavaScript, estas opciones se pueden proporcionar en un objeto de JavaScript proporcionado a `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="64230-342">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="64230-343">En el cliente de Java, estas opciones se pueden configurar con los métodos del `HttpHubConnectionBuilder` devueltos desde el `HubConnectionBuilder.create("HUB URL")`</span><span class="sxs-lookup"><span data-stu-id="64230-343">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="64230-344">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="64230-344">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>

::: moniker-end
::: moniker range="= aspnetcore-2.2"

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="64230-345">Opciones de serialización de JSON/MessagePack</span><span class="sxs-lookup"><span data-stu-id="64230-345">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="64230-346">ASP.NET Core SignalR admite dos protocolos para codificar los mensajes: [JSON](https://www.json.org/) y [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="64230-346">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="64230-347">Cada protocolo tiene opciones de configuración de serialización.</span><span class="sxs-lookup"><span data-stu-id="64230-347">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="64230-348">La serialización de JSON se puede configurar en el servidor mediante el método de extensión [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) , que se puede agregar después de [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) en el método de `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="64230-348">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="64230-349">El método `AddJsonProtocol` toma un delegado que recibe un objeto `options`.</span><span class="sxs-lookup"><span data-stu-id="64230-349">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="64230-350">La propiedad [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) de ese objeto es un objeto de `JsonSerializerSettings` de JSON.net que se puede usar para configurar la serialización de argumentos y valores devueltos.</span><span class="sxs-lookup"><span data-stu-id="64230-350">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="64230-351">Para más información, vea la [documentación de JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span><span class="sxs-lookup"><span data-stu-id="64230-351">For more information, see the [JSON.NET documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span></span>
 
<span data-ttu-id="64230-352">Por ejemplo, para configurar el serializador de modo que use nombres de propiedad "PascalCase", en lugar de los nombres predeterminados de "camelCase", use el siguiente código en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="64230-352">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code in `Startup.ConfigureServices`:</span></span>
 
```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
            new DefaultContractResolver();
    });
```

<span data-ttu-id="64230-353">En el cliente de .NET, el mismo método de extensión `AddJsonProtocol` existe en [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="64230-353">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="64230-354">El espacio de nombres `Microsoft.Extensions.DependencyInjection` se debe importar para resolver el método de extensión:</span><span class="sxs-lookup"><span data-stu-id="64230-354">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="64230-355">En este momento, no es posible configurar la serialización de JSON en el cliente de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="64230-355">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="64230-356">Opciones de serialización de MessagePack</span><span class="sxs-lookup"><span data-stu-id="64230-356">MessagePack serialization options</span></span>

<span data-ttu-id="64230-357">La serialización de MessagePack se puede configurar proporcionando un delegado a la llamada a [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) .</span><span class="sxs-lookup"><span data-stu-id="64230-357">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="64230-358">Vea [MessagePack en SignalR](xref:signalr/messagepackhubprotocol) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="64230-358">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="64230-359">En este momento, no es posible configurar la serialización de MessagePack en el cliente de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="64230-359">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="64230-360">Configurar opciones de servidor</span><span class="sxs-lookup"><span data-stu-id="64230-360">Configure server options</span></span>

<span data-ttu-id="64230-361">En la tabla siguiente se describen las opciones para configurar los concentradores de SignalR:</span><span class="sxs-lookup"><span data-stu-id="64230-361">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="64230-362">Opción</span><span class="sxs-lookup"><span data-stu-id="64230-362">Option</span></span> | <span data-ttu-id="64230-363">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="64230-363">Default Value</span></span> | <span data-ttu-id="64230-364">Descripción</span><span class="sxs-lookup"><span data-stu-id="64230-364">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="64230-365">30 segundos</span><span class="sxs-lookup"><span data-stu-id="64230-365">30 seconds</span></span> | <span data-ttu-id="64230-366">El servidor considerará que el cliente se ha desconectado Si no ha recibido un mensaje (incluido Keep-Alive) en este intervalo.</span><span class="sxs-lookup"><span data-stu-id="64230-366">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="64230-367">Podría tardar más de este intervalo de tiempo de espera para que el cliente se marque realmente como desconectado, debido a la manera en que se implementa.</span><span class="sxs-lookup"><span data-stu-id="64230-367">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="64230-368">El valor recomendado es el doble del valor de `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="64230-368">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="64230-369">15 segundos</span><span class="sxs-lookup"><span data-stu-id="64230-369">15 seconds</span></span> | <span data-ttu-id="64230-370">Si el cliente no envía un mensaje de enlace inicial dentro de este intervalo de tiempo, la conexión se cierra.</span><span class="sxs-lookup"><span data-stu-id="64230-370">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="64230-371">Se trata de una configuración avanzada que solo se debe modificar si se producen errores de tiempo de espera del Protocolo de enlace debido a una latencia de red grave.</span><span class="sxs-lookup"><span data-stu-id="64230-371">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="64230-372">Para obtener más información sobre el proceso de enlace, consulte la [especificación del Protocolo de concentrador deSignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="64230-372">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="64230-373">15 segundos</span><span class="sxs-lookup"><span data-stu-id="64230-373">15 seconds</span></span> | <span data-ttu-id="64230-374">Si el servidor no ha enviado un mensaje dentro de este intervalo, se envía automáticamente un mensaje ping para mantener abierta la conexión.</span><span class="sxs-lookup"><span data-stu-id="64230-374">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="64230-375">Al cambiar `KeepAliveInterval`, cambie la configuración de `serverTimeoutInMilliseconds` de /de `ServerTimeout`en el cliente.</span><span class="sxs-lookup"><span data-stu-id="64230-375">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="64230-376">El `ServerTimeout`recomendado /valor de `serverTimeoutInMilliseconds` es el doble del valor de `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="64230-376">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="64230-377">Todos los protocolos instalados</span><span class="sxs-lookup"><span data-stu-id="64230-377">All installed protocols</span></span> | <span data-ttu-id="64230-378">Protocolos admitidos por este centro.</span><span class="sxs-lookup"><span data-stu-id="64230-378">Protocols supported by this hub.</span></span> <span data-ttu-id="64230-379">De forma predeterminada, todos los protocolos registrados en el servidor están permitidos, pero los protocolos se pueden quitar de esta lista para deshabilitar determinados protocolos para centros individuales.</span><span class="sxs-lookup"><span data-stu-id="64230-379">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="64230-380">Si `true`, los mensajes de excepción detallados se devuelven a los clientes cuando se produce una excepción en un método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="64230-380">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="64230-381">El valor predeterminado es `false`, ya que estos mensajes de excepción pueden contener información confidencial.</span><span class="sxs-lookup"><span data-stu-id="64230-381">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

<span data-ttu-id="64230-382">Las opciones se pueden configurar para todos los concentradores proporcionando un delegado de opciones a la llamada `AddSignalR` en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="64230-382">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="64230-383">Las opciones de un solo concentrador invalidan las opciones globales proporcionadas en `AddSignalR` y se pueden configurar mediante <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span><span class="sxs-lookup"><span data-stu-id="64230-383">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="64230-384">Opciones de configuración de HTTP avanzadas</span><span class="sxs-lookup"><span data-stu-id="64230-384">Advanced HTTP configuration options</span></span>

<span data-ttu-id="64230-385">Use `HttpConnectionDispatcherOptions` para configurar opciones avanzadas relacionadas con los transportes y la administración del búfer de memoria.</span><span class="sxs-lookup"><span data-stu-id="64230-385">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="64230-386">Estas opciones se configuran pasando un delegado a [MapHub\<t >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) en `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="64230-386">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="64230-387">En la tabla siguiente se describen las opciones para configurar las opciones de HTTP avanzadas de ASP.NET Core SignalR:</span><span class="sxs-lookup"><span data-stu-id="64230-387">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

| <span data-ttu-id="64230-388">Opción</span><span class="sxs-lookup"><span data-stu-id="64230-388">Option</span></span> | <span data-ttu-id="64230-389">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="64230-389">Default Value</span></span> | <span data-ttu-id="64230-390">Descripción</span><span class="sxs-lookup"><span data-stu-id="64230-390">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="64230-391">32 KB</span><span class="sxs-lookup"><span data-stu-id="64230-391">32 KB</span></span> | <span data-ttu-id="64230-392">Número máximo de bytes recibidos del cliente que el servidor almacena en búfer.</span><span class="sxs-lookup"><span data-stu-id="64230-392">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="64230-393">Aumentar este valor permite que el servidor reciba mensajes de mayor tamaño, pero puede afectar negativamente al consumo de memoria.</span><span class="sxs-lookup"><span data-stu-id="64230-393">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="64230-394">Los datos se recopilan automáticamente a partir de los atributos `Authorize` aplicados a la clase hub.</span><span class="sxs-lookup"><span data-stu-id="64230-394">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="64230-395">Una lista de objetos [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) que se usan para determinar si un cliente está autorizado para conectarse al centro.</span><span class="sxs-lookup"><span data-stu-id="64230-395">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="64230-396">32 KB</span><span class="sxs-lookup"><span data-stu-id="64230-396">32 KB</span></span> | <span data-ttu-id="64230-397">Número máximo de bytes enviados por la aplicación que el servidor almacena en búfer.</span><span class="sxs-lookup"><span data-stu-id="64230-397">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="64230-398">Al aumentar este valor, el servidor puede enviar mensajes de mayor tamaño, pero puede afectar negativamente al consumo de memoria.</span><span class="sxs-lookup"><span data-stu-id="64230-398">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="64230-399">Todos los transportes están habilitados.</span><span class="sxs-lookup"><span data-stu-id="64230-399">All Transports are enabled.</span></span> | <span data-ttu-id="64230-400">Una enumeración de marcas de bits de `HttpTransportType` valores que pueden restringir los transportes que un cliente puede utilizar para conectarse.</span><span class="sxs-lookup"><span data-stu-id="64230-400">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="64230-401">Véalo a continuación.</span><span class="sxs-lookup"><span data-stu-id="64230-401">See below.</span></span> | <span data-ttu-id="64230-402">Opciones adicionales específicas del transporte de sondeo prolongado.</span><span class="sxs-lookup"><span data-stu-id="64230-402">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="64230-403">Véalo a continuación.</span><span class="sxs-lookup"><span data-stu-id="64230-403">See below.</span></span> | <span data-ttu-id="64230-404">Opciones adicionales específicas del transporte de WebSockets.</span><span class="sxs-lookup"><span data-stu-id="64230-404">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="64230-405">El transporte de sondeo largo tiene opciones adicionales que se pueden configurar mediante la propiedad `LongPolling`:</span><span class="sxs-lookup"><span data-stu-id="64230-405">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="64230-406">Opción</span><span class="sxs-lookup"><span data-stu-id="64230-406">Option</span></span> | <span data-ttu-id="64230-407">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="64230-407">Default Value</span></span> | <span data-ttu-id="64230-408">Descripción</span><span class="sxs-lookup"><span data-stu-id="64230-408">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="64230-409">90 segundos</span><span class="sxs-lookup"><span data-stu-id="64230-409">90 seconds</span></span> | <span data-ttu-id="64230-410">Cantidad máxima de tiempo que el servidor espera a que se envíe un mensaje al cliente antes de finalizar una única solicitud de sondeo.</span><span class="sxs-lookup"><span data-stu-id="64230-410">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="64230-411">Al disminuir este valor, el cliente emite nuevas solicitudes de sondeo con mayor frecuencia.</span><span class="sxs-lookup"><span data-stu-id="64230-411">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="64230-412">El transporte de WebSocket tiene opciones adicionales que se pueden configurar mediante la propiedad `WebSockets`:</span><span class="sxs-lookup"><span data-stu-id="64230-412">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="64230-413">Opción</span><span class="sxs-lookup"><span data-stu-id="64230-413">Option</span></span> | <span data-ttu-id="64230-414">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="64230-414">Default Value</span></span> | <span data-ttu-id="64230-415">Descripción</span><span class="sxs-lookup"><span data-stu-id="64230-415">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="64230-416">5 segundos</span><span class="sxs-lookup"><span data-stu-id="64230-416">5 seconds</span></span> | <span data-ttu-id="64230-417">Una vez que se cierra el servidor, si el cliente no se cierra dentro de este intervalo de tiempo, se finaliza la conexión.</span><span class="sxs-lookup"><span data-stu-id="64230-417">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="64230-418">Delegado que se puede utilizar para establecer el encabezado de `Sec-WebSocket-Protocol` en un valor personalizado.</span><span class="sxs-lookup"><span data-stu-id="64230-418">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="64230-419">El delegado recibe los valores solicitados por el cliente como entrada y se espera que devuelva el valor deseado.</span><span class="sxs-lookup"><span data-stu-id="64230-419">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="64230-420">Configurar opciones de cliente</span><span class="sxs-lookup"><span data-stu-id="64230-420">Configure client options</span></span>

<span data-ttu-id="64230-421">Las opciones de cliente se pueden configurar en el tipo de `HubConnectionBuilder` (disponible en los clientes de .NET y JavaScript).</span><span class="sxs-lookup"><span data-stu-id="64230-421">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="64230-422">También está disponible en el cliente de Java, pero la subclase `HttpHubConnectionBuilder` es lo que contiene las opciones de configuración del generador, así como en el `HubConnection` mismo.</span><span class="sxs-lookup"><span data-stu-id="64230-422">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="64230-423">registro</span><span class="sxs-lookup"><span data-stu-id="64230-423">Configure logging</span></span>

<span data-ttu-id="64230-424">El registro se configura en el cliente .NET mediante el método `ConfigureLogging`.</span><span class="sxs-lookup"><span data-stu-id="64230-424">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="64230-425">Los proveedores de registro y los filtros se pueden registrar de la misma manera que en el servidor.</span><span class="sxs-lookup"><span data-stu-id="64230-425">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="64230-426">Consulte la documentación de [Inicio de sesión ASP.net Core](xref:fundamentals/logging/index) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="64230-426">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="64230-427">Para registrar los proveedores de registro, debe instalar los paquetes necesarios.</span><span class="sxs-lookup"><span data-stu-id="64230-427">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="64230-428">Consulte la sección [proveedores de registro integrados](xref:fundamentals/logging/index#built-in-logging-providers) de los documentos para obtener una lista completa.</span><span class="sxs-lookup"><span data-stu-id="64230-428">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="64230-429">Por ejemplo, para habilitar el registro de la consola, instale el paquete NuGet `Microsoft.Extensions.Logging.Console`.</span><span class="sxs-lookup"><span data-stu-id="64230-429">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="64230-430">Llame al método de extensión `AddConsole`:</span><span class="sxs-lookup"><span data-stu-id="64230-430">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="64230-431">En el cliente de JavaScript, existe un método `configureLogging` similar.</span><span class="sxs-lookup"><span data-stu-id="64230-431">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="64230-432">Proporcione un valor `LogLevel` que indique el nivel mínimo de mensajes de registro que se va a producir.</span><span class="sxs-lookup"><span data-stu-id="64230-432">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="64230-433">Los registros se escriben en la ventana de la consola del explorador.</span><span class="sxs-lookup"><span data-stu-id="64230-433">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> <span data-ttu-id="64230-434">Para deshabilitar el registro por completo, especifique `signalR.LogLevel.None` en el método `configureLogging`.</span><span class="sxs-lookup"><span data-stu-id="64230-434">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="64230-435">Para obtener más información sobre el registro, consulte la [documentación de diagnóstico deSignalR](xref:signalr/diagnostics).</span><span class="sxs-lookup"><span data-stu-id="64230-435">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="64230-436">El cliente de Java de SignalR usa la biblioteca [SLF4J](https://www.slf4j.org/) para el registro.</span><span class="sxs-lookup"><span data-stu-id="64230-436">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="64230-437">Se trata de una API de registro de alto nivel que permite a los usuarios de la biblioteca elegir su propia implementación de registro específica mediante la incorporación de una dependencia de registro específica.</span><span class="sxs-lookup"><span data-stu-id="64230-437">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="64230-438">En el fragmento de código siguiente se muestra cómo usar `java.util.logging` con el cliente de Java SignalR.</span><span class="sxs-lookup"><span data-stu-id="64230-438">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="64230-439">Si no configura el registro en las dependencias, SLF4J carga un registrador de no operaciones predeterminado con el siguiente mensaje de ADVERTENCIA:</span><span class="sxs-lookup"><span data-stu-id="64230-439">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="64230-440">Esto se puede omitir sin ningún riesgo.</span><span class="sxs-lookup"><span data-stu-id="64230-440">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="64230-441">Configurar transportes permitidos</span><span class="sxs-lookup"><span data-stu-id="64230-441">Configure allowed transports</span></span>

<span data-ttu-id="64230-442">Los transportes utilizados por SignalR pueden configurarse en la llamada `WithUrl` (`withUrl` en JavaScript).</span><span class="sxs-lookup"><span data-stu-id="64230-442">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="64230-443">Una operación OR bit a bit de los valores de `HttpTransportType` se puede utilizar para restringir el cliente de modo que solo utilice los transportes especificados.</span><span class="sxs-lookup"><span data-stu-id="64230-443">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="64230-444">Todos los transportes están habilitados de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="64230-444">All transports are enabled by default.</span></span>

<span data-ttu-id="64230-445">Por ejemplo, para deshabilitar el transporte de eventos enviados por el servidor, pero permitir WebSockets y conexiones de sondeo largas:</span><span class="sxs-lookup"><span data-stu-id="64230-445">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="64230-446">En el cliente de JavaScript, los transportes se configuran estableciendo el campo `transport` en el objeto de opciones proporcionado para `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="64230-446">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

<span data-ttu-id="64230-447">En esta versión del cliente de Java, WebSockets es el único transporte disponible.</span><span class="sxs-lookup"><span data-stu-id="64230-447">In this version of the Java client websockets is the only available transport.</span></span>

### <a name="configure-bearer-authentication"></a><span data-ttu-id="64230-448">Configurar la autenticación de portador</span><span class="sxs-lookup"><span data-stu-id="64230-448">Configure bearer authentication</span></span>

<span data-ttu-id="64230-449">Para proporcionar datos de autenticación junto con solicitudes de SignalR, use la opción `AccessTokenProvider` (`accessTokenFactory` en JavaScript) para especificar una función que devuelva el token de acceso deseado.</span><span class="sxs-lookup"><span data-stu-id="64230-449">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="64230-450">En el cliente .NET, este token de acceso se pasa como un token de "autenticación de portador" de HTTP (mediante el encabezado `Authorization` con un tipo de `Bearer`).</span><span class="sxs-lookup"><span data-stu-id="64230-450">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="64230-451">En el cliente de JavaScript, el token de acceso se usa como un token de portador, **excepto** en algunos casos en los que las API del explorador restringen la capacidad de aplicar encabezados (específicamente, en los eventos enviados por el servidor y en las solicitudes de WebSockets).</span><span class="sxs-lookup"><span data-stu-id="64230-451">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="64230-452">En estos casos, el token de acceso se proporciona como un valor de cadena de consulta `access_token`.</span><span class="sxs-lookup"><span data-stu-id="64230-452">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="64230-453">En el cliente .NET, se puede especificar la opción `AccessTokenProvider` mediante el delegado Options en `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="64230-453">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="64230-454">En el cliente de JavaScript, el token de acceso se configura estableciendo el campo `accessTokenFactory` del objeto de opciones en `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="64230-454">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

<span data-ttu-id="64230-455">En el cliente de SignalR Java, puede configurar un token de portador que se usará para la autenticación proporcionando un generador de tokens de acceso a [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span><span class="sxs-lookup"><span data-stu-id="64230-455">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="64230-456">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) para proporcionar un [RxJava](https://github.com/ReactiveX/RxJava) de una [única\<cadena>](https://reactivex.io/documentation/single.html).</span><span class="sxs-lookup"><span data-stu-id="64230-456">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="64230-457">Con una llamada a [Single. defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), puede escribir lógica para generar tokens de acceso para el cliente.</span><span class="sxs-lookup"><span data-stu-id="64230-457">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="64230-458">Configurar las opciones de tiempo de espera y mantenimiento de conexión</span><span class="sxs-lookup"><span data-stu-id="64230-458">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="64230-459">Las opciones adicionales para configurar el comportamiento de tiempo de espera y mantenimiento de conexión están disponibles en el propio `HubConnection` objeto:</span><span class="sxs-lookup"><span data-stu-id="64230-459">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

# <a name="nettabdotnet"></a>[<span data-ttu-id="64230-460">.NET</span><span class="sxs-lookup"><span data-stu-id="64230-460">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="64230-461">Opción</span><span class="sxs-lookup"><span data-stu-id="64230-461">Option</span></span> | <span data-ttu-id="64230-462">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="64230-462">Default value</span></span> | <span data-ttu-id="64230-463">Descripción</span><span class="sxs-lookup"><span data-stu-id="64230-463">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="64230-464">30 segundos (30.000 milisegundos)</span><span class="sxs-lookup"><span data-stu-id="64230-464">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="64230-465">Tiempo de espera para la actividad del servidor.</span><span class="sxs-lookup"><span data-stu-id="64230-465">Timeout for server activity.</span></span> <span data-ttu-id="64230-466">Si el servidor no ha enviado un mensaje en este intervalo, el cliente considera que el servidor está desconectado y desencadena el evento `Closed` (`onclose` en JavaScript).</span><span class="sxs-lookup"><span data-stu-id="64230-466">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="64230-467">Este valor debe ser lo suficientemente grande como para que se envíe un mensaje ping desde el servidor **y** el cliente lo reciba dentro del intervalo de tiempo de espera.</span><span class="sxs-lookup"><span data-stu-id="64230-467">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="64230-468">El valor recomendado es un número al menos el doble del valor del `KeepAliveInterval` del servidor para permitir que lleguen los ping.</span><span class="sxs-lookup"><span data-stu-id="64230-468">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="64230-469">15 segundos</span><span class="sxs-lookup"><span data-stu-id="64230-469">15 seconds</span></span> | <span data-ttu-id="64230-470">Tiempo de espera para el protocolo de enlace inicial del servidor.</span><span class="sxs-lookup"><span data-stu-id="64230-470">Timeout for initial server handshake.</span></span> <span data-ttu-id="64230-471">Si el servidor no envía una respuesta de protocolo de enlace en este intervalo, el cliente cancela el protocolo de enlace y desencadena el evento `Closed` (`onclose` en JavaScript).</span><span class="sxs-lookup"><span data-stu-id="64230-471">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="64230-472">Se trata de una configuración avanzada que solo se debe modificar si se producen errores de tiempo de espera del Protocolo de enlace debido a una latencia de red grave.</span><span class="sxs-lookup"><span data-stu-id="64230-472">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="64230-473">Para obtener más información sobre el proceso de enlace, consulte la [especificación del Protocolo de concentrador deSignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="64230-473">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="64230-474">15 segundos</span><span class="sxs-lookup"><span data-stu-id="64230-474">15 seconds</span></span> | <span data-ttu-id="64230-475">Determina el intervalo en el que el cliente envía mensajes de ping.</span><span class="sxs-lookup"><span data-stu-id="64230-475">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="64230-476">Al enviar cualquier mensaje desde el cliente, se restablece el temporizador al inicio del intervalo.</span><span class="sxs-lookup"><span data-stu-id="64230-476">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="64230-477">Si el cliente no ha enviado un mensaje en el `ClientTimeoutInterval` establecido en el servidor, el servidor considera que el cliente está desconectado.</span><span class="sxs-lookup"><span data-stu-id="64230-477">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

<span data-ttu-id="64230-478">En el cliente de .NET, los valores de tiempo de espera se especifican como valores de `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="64230-478">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="64230-479">JavaScript</span><span class="sxs-lookup"><span data-stu-id="64230-479">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="64230-480">Opción</span><span class="sxs-lookup"><span data-stu-id="64230-480">Option</span></span> | <span data-ttu-id="64230-481">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="64230-481">Default value</span></span> | <span data-ttu-id="64230-482">Descripción</span><span class="sxs-lookup"><span data-stu-id="64230-482">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="64230-483">30 segundos (30.000 milisegundos)</span><span class="sxs-lookup"><span data-stu-id="64230-483">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="64230-484">Tiempo de espera para la actividad del servidor.</span><span class="sxs-lookup"><span data-stu-id="64230-484">Timeout for server activity.</span></span> <span data-ttu-id="64230-485">Si el servidor no ha enviado un mensaje en este intervalo, el cliente considera que el servidor está desconectado y desencadena el evento `onclose`.</span><span class="sxs-lookup"><span data-stu-id="64230-485">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="64230-486">Este valor debe ser lo suficientemente grande como para que se envíe un mensaje ping desde el servidor **y** el cliente lo reciba dentro del intervalo de tiempo de espera.</span><span class="sxs-lookup"><span data-stu-id="64230-486">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="64230-487">El valor recomendado es un número al menos el doble del valor del `KeepAliveInterval` del servidor para permitir que lleguen los ping.</span><span class="sxs-lookup"><span data-stu-id="64230-487">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `keepAliveIntervalInMilliseconds` | <span data-ttu-id="64230-488">15 segundos (15.000 milisegundos)</span><span class="sxs-lookup"><span data-stu-id="64230-488">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="64230-489">Determina el intervalo en el que el cliente envía mensajes de ping.</span><span class="sxs-lookup"><span data-stu-id="64230-489">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="64230-490">Al enviar cualquier mensaje desde el cliente, se restablece el temporizador al inicio del intervalo.</span><span class="sxs-lookup"><span data-stu-id="64230-490">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="64230-491">Si el cliente no ha enviado un mensaje en el `ClientTimeoutInterval` establecido en el servidor, el servidor considera que el cliente está desconectado.</span><span class="sxs-lookup"><span data-stu-id="64230-491">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="64230-492">Java</span><span class="sxs-lookup"><span data-stu-id="64230-492">Java</span></span>](#tab/java)

| <span data-ttu-id="64230-493">Opción</span><span class="sxs-lookup"><span data-stu-id="64230-493">Option</span></span> | <span data-ttu-id="64230-494">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="64230-494">Default value</span></span> | <span data-ttu-id="64230-495">Descripción</span><span class="sxs-lookup"><span data-stu-id="64230-495">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="64230-496">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="64230-496">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="64230-497">30 segundos (30.000 milisegundos)</span><span class="sxs-lookup"><span data-stu-id="64230-497">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="64230-498">Tiempo de espera para la actividad del servidor.</span><span class="sxs-lookup"><span data-stu-id="64230-498">Timeout for server activity.</span></span> <span data-ttu-id="64230-499">Si el servidor no ha enviado un mensaje en este intervalo, el cliente considera que el servidor está desconectado y desencadena el evento `onClose`.</span><span class="sxs-lookup"><span data-stu-id="64230-499">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="64230-500">Este valor debe ser lo suficientemente grande como para que se envíe un mensaje ping desde el servidor **y** el cliente lo reciba dentro del intervalo de tiempo de espera.</span><span class="sxs-lookup"><span data-stu-id="64230-500">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="64230-501">El valor recomendado es un número al menos el doble del valor del `KeepAliveInterval` del servidor para permitir que lleguen los ping.</span><span class="sxs-lookup"><span data-stu-id="64230-501">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="64230-502">15 segundos</span><span class="sxs-lookup"><span data-stu-id="64230-502">15 seconds</span></span> | <span data-ttu-id="64230-503">Tiempo de espera para el protocolo de enlace inicial del servidor.</span><span class="sxs-lookup"><span data-stu-id="64230-503">Timeout for initial server handshake.</span></span> <span data-ttu-id="64230-504">Si el servidor no envía una respuesta de protocolo de enlace en este intervalo, el cliente cancela el protocolo de enlace y desencadena el evento `onClose`.</span><span class="sxs-lookup"><span data-stu-id="64230-504">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="64230-505">Se trata de una configuración avanzada que solo se debe modificar si se producen errores de tiempo de espera del Protocolo de enlace debido a una latencia de red grave.</span><span class="sxs-lookup"><span data-stu-id="64230-505">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="64230-506">Para obtener más información sobre el proceso de enlace, consulte la [especificación del Protocolo de concentrador deSignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="64230-506">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| <span data-ttu-id="64230-507">`getKeepAliveInterval` / `setKeepAliveInterval`</span><span class="sxs-lookup"><span data-stu-id="64230-507">`getKeepAliveInterval` / `setKeepAliveInterval`</span></span> | <span data-ttu-id="64230-508">15 segundos (15.000 milisegundos)</span><span class="sxs-lookup"><span data-stu-id="64230-508">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="64230-509">Determina el intervalo en el que el cliente envía mensajes de ping.</span><span class="sxs-lookup"><span data-stu-id="64230-509">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="64230-510">Al enviar cualquier mensaje desde el cliente, se restablece el temporizador al inicio del intervalo.</span><span class="sxs-lookup"><span data-stu-id="64230-510">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="64230-511">Si el cliente no ha enviado un mensaje en el `ClientTimeoutInterval` establecido en el servidor, el servidor considera que el cliente está desconectado.</span><span class="sxs-lookup"><span data-stu-id="64230-511">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

---

### <a name="configure-additional-options"></a><span data-ttu-id="64230-512">Configuración de opciones adicionales</span><span class="sxs-lookup"><span data-stu-id="64230-512">Configure additional options</span></span>

<span data-ttu-id="64230-513">Se pueden configurar opciones adicionales en el método `WithUrl` (`withUrl` en JavaScript) en `HubConnectionBuilder` o en las diversas API de configuración de la `HttpHubConnectionBuilder` en el cliente de Java:</span><span class="sxs-lookup"><span data-stu-id="64230-513">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="nettabdotnet"></a>[<span data-ttu-id="64230-514">.NET</span><span class="sxs-lookup"><span data-stu-id="64230-514">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="64230-515">Opción de .NET</span><span class="sxs-lookup"><span data-stu-id="64230-515">.NET Option</span></span> |  <span data-ttu-id="64230-516">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="64230-516">Default value</span></span> | <span data-ttu-id="64230-517">Descripción</span><span class="sxs-lookup"><span data-stu-id="64230-517">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="64230-518">Función que devuelve una cadena que se proporciona como un token de autenticación de portador en solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="64230-518">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="64230-519">Establezca este valor en `true` para omitir el paso de negociación.</span><span class="sxs-lookup"><span data-stu-id="64230-519">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="64230-520">**Solo se admite cuando el transporte de WebSockets es el único transporte habilitado**.</span><span class="sxs-lookup"><span data-stu-id="64230-520">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="64230-521">Esta configuración no se puede habilitar cuando se usa el servicio Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="64230-521">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="64230-522">Empty</span><span class="sxs-lookup"><span data-stu-id="64230-522">Empty</span></span> | <span data-ttu-id="64230-523">Colección de certificados TLS que se enviarán a las solicitudes de autenticación.</span><span class="sxs-lookup"><span data-stu-id="64230-523">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="64230-524">Empty</span><span class="sxs-lookup"><span data-stu-id="64230-524">Empty</span></span> | <span data-ttu-id="64230-525">Colección de cookies HTTP que se enviarán con cada solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="64230-525">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="64230-526">Empty</span><span class="sxs-lookup"><span data-stu-id="64230-526">Empty</span></span> | <span data-ttu-id="64230-527">Credenciales que se van a enviar con cada solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="64230-527">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="64230-528">5 segundos</span><span class="sxs-lookup"><span data-stu-id="64230-528">5 seconds</span></span> | <span data-ttu-id="64230-529">Solo WebSockets.</span><span class="sxs-lookup"><span data-stu-id="64230-529">WebSockets only.</span></span> <span data-ttu-id="64230-530">Cantidad máxima de tiempo que el cliente espera después de cerrarse para que el servidor confirme la solicitud de cierre.</span><span class="sxs-lookup"><span data-stu-id="64230-530">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="64230-531">Si el servidor no reconoce el cierre dentro de este tiempo, el cliente se desconecta.</span><span class="sxs-lookup"><span data-stu-id="64230-531">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="64230-532">Empty</span><span class="sxs-lookup"><span data-stu-id="64230-532">Empty</span></span> | <span data-ttu-id="64230-533">Asignación de encabezados HTTP adicionales que se van a enviar con cada solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="64230-533">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="64230-534">Delegado que se puede utilizar para configurar o reemplazar el `HttpMessageHandler` utilizado para enviar solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="64230-534">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="64230-535">No se usa para las conexiones WebSocket.</span><span class="sxs-lookup"><span data-stu-id="64230-535">Not used for WebSocket connections.</span></span> <span data-ttu-id="64230-536">Este delegado debe devolver un valor distinto de NULL y recibe el valor predeterminado como parámetro.</span><span class="sxs-lookup"><span data-stu-id="64230-536">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="64230-537">Modifique la configuración de ese valor predeterminado y devuelva o devuelva una nueva instancia de `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="64230-537">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="64230-538">**Al reemplazar el controlador, asegúrese de copiar la configuración que desea conservar del controlador proporcionado; de lo contrario, las opciones configuradas (como cookies y encabezados) no se aplicarán al nuevo controlador.**</span><span class="sxs-lookup"><span data-stu-id="64230-538">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="64230-539">Proxy HTTP que se va a usar al enviar solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="64230-539">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="64230-540">Establezca este valor booleano para enviar las credenciales predeterminadas para las solicitudes HTTP y WebSockets.</span><span class="sxs-lookup"><span data-stu-id="64230-540">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="64230-541">Esto habilita el uso de la autenticación de Windows.</span><span class="sxs-lookup"><span data-stu-id="64230-541">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="64230-542">Delegado que se puede usar para configurar opciones adicionales de WebSocket.</span><span class="sxs-lookup"><span data-stu-id="64230-542">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="64230-543">Recibe una instancia de [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) que se puede usar para configurar las opciones.</span><span class="sxs-lookup"><span data-stu-id="64230-543">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="64230-544">JavaScript</span><span class="sxs-lookup"><span data-stu-id="64230-544">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="64230-545">Opción de JavaScript</span><span class="sxs-lookup"><span data-stu-id="64230-545">JavaScript Option</span></span> | <span data-ttu-id="64230-546">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="64230-546">Default Value</span></span> | <span data-ttu-id="64230-547">Descripción</span><span class="sxs-lookup"><span data-stu-id="64230-547">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="64230-548">Función que devuelve una cadena que se proporciona como un token de autenticación de portador en solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="64230-548">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="64230-549">Establezca este valor en `true` para omitir el paso de negociación.</span><span class="sxs-lookup"><span data-stu-id="64230-549">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="64230-550">**Solo se admite cuando el transporte de WebSockets es el único transporte habilitado**.</span><span class="sxs-lookup"><span data-stu-id="64230-550">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="64230-551">Esta configuración no se puede habilitar cuando se usa el servicio Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="64230-551">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="64230-552">Java</span><span class="sxs-lookup"><span data-stu-id="64230-552">Java</span></span>](#tab/java)

| <span data-ttu-id="64230-553">Opción de Java</span><span class="sxs-lookup"><span data-stu-id="64230-553">Java Option</span></span> | <span data-ttu-id="64230-554">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="64230-554">Default Value</span></span> | <span data-ttu-id="64230-555">Descripción</span><span class="sxs-lookup"><span data-stu-id="64230-555">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="64230-556">Función que devuelve una cadena que se proporciona como un token de autenticación de portador en solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="64230-556">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="64230-557">Establezca este valor en `true` para omitir el paso de negociación.</span><span class="sxs-lookup"><span data-stu-id="64230-557">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="64230-558">**Solo se admite cuando el transporte de WebSockets es el único transporte habilitado**.</span><span class="sxs-lookup"><span data-stu-id="64230-558">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="64230-559">Esta configuración no se puede habilitar cuando se usa el servicio Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="64230-559">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="64230-560">`withHeader` `withHeaders`</span><span class="sxs-lookup"><span data-stu-id="64230-560">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="64230-561">Empty</span><span class="sxs-lookup"><span data-stu-id="64230-561">Empty</span></span> | <span data-ttu-id="64230-562">Asignación de encabezados HTTP adicionales que se van a enviar con cada solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="64230-562">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="64230-563">En el cliente .NET, estas opciones se pueden modificar mediante el delegado de opciones proporcionado a `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="64230-563">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="64230-564">En el cliente de JavaScript, estas opciones se pueden proporcionar en un objeto de JavaScript proporcionado a `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="64230-564">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="64230-565">En el cliente de Java, estas opciones se pueden configurar con los métodos del `HttpHubConnectionBuilder` devueltos desde el `HubConnectionBuilder.create("HUB URL")`</span><span class="sxs-lookup"><span data-stu-id="64230-565">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="64230-566">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="64230-566">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>

::: moniker-end
::: moniker range="< aspnetcore-2.2"

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="64230-567">Opciones de serialización de JSON/MessagePack</span><span class="sxs-lookup"><span data-stu-id="64230-567">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="64230-568">ASP.NET Core SignalR admite dos protocolos para codificar los mensajes: [JSON](https://www.json.org/) y [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="64230-568">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="64230-569">Cada protocolo tiene opciones de configuración de serialización.</span><span class="sxs-lookup"><span data-stu-id="64230-569">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="64230-570">La serialización de JSON se puede configurar en el servidor mediante el método de extensión [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) , que se puede agregar después de [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) en el método de `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="64230-570">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="64230-571">El método `AddJsonProtocol` toma un delegado que recibe un objeto `options`.</span><span class="sxs-lookup"><span data-stu-id="64230-571">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="64230-572">La propiedad [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) de ese objeto es un objeto de `JsonSerializerSettings` de JSON.net que se puede usar para configurar la serialización de argumentos y valores devueltos.</span><span class="sxs-lookup"><span data-stu-id="64230-572">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="64230-573">Para más información, vea la [documentación de JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span><span class="sxs-lookup"><span data-stu-id="64230-573">For more information, see the [JSON.NET documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span></span>
 
<span data-ttu-id="64230-574">Por ejemplo, para configurar el serializador de modo que use nombres de propiedad "PascalCase", en lugar de los nombres predeterminados de "camelCase", use el siguiente código en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="64230-574">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code in `Startup.ConfigureServices`:</span></span>
 
```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
            new DefaultContractResolver();
    });
```

<span data-ttu-id="64230-575">En el cliente de .NET, el mismo método de extensión `AddJsonProtocol` existe en [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="64230-575">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="64230-576">El espacio de nombres `Microsoft.Extensions.DependencyInjection` se debe importar para resolver el método de extensión:</span><span class="sxs-lookup"><span data-stu-id="64230-576">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="64230-577">En este momento, no es posible configurar la serialización de JSON en el cliente de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="64230-577">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="64230-578">Opciones de serialización de MessagePack</span><span class="sxs-lookup"><span data-stu-id="64230-578">MessagePack serialization options</span></span>

<span data-ttu-id="64230-579">La serialización de MessagePack se puede configurar proporcionando un delegado a la llamada a [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) .</span><span class="sxs-lookup"><span data-stu-id="64230-579">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="64230-580">Vea [MessagePack en SignalR](xref:signalr/messagepackhubprotocol) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="64230-580">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="64230-581">En este momento, no es posible configurar la serialización de MessagePack en el cliente de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="64230-581">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="64230-582">Configurar opciones de servidor</span><span class="sxs-lookup"><span data-stu-id="64230-582">Configure server options</span></span>

<span data-ttu-id="64230-583">En la tabla siguiente se describen las opciones para configurar los concentradores de SignalR:</span><span class="sxs-lookup"><span data-stu-id="64230-583">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="64230-584">Opción</span><span class="sxs-lookup"><span data-stu-id="64230-584">Option</span></span> | <span data-ttu-id="64230-585">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="64230-585">Default Value</span></span> | <span data-ttu-id="64230-586">Descripción</span><span class="sxs-lookup"><span data-stu-id="64230-586">Description</span></span> |
| ------ | ------------- | ----------- |
| `HandshakeTimeout` | <span data-ttu-id="64230-587">15 segundos</span><span class="sxs-lookup"><span data-stu-id="64230-587">15 seconds</span></span> | <span data-ttu-id="64230-588">Si el cliente no envía un mensaje de enlace inicial dentro de este intervalo de tiempo, la conexión se cierra.</span><span class="sxs-lookup"><span data-stu-id="64230-588">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="64230-589">Se trata de una configuración avanzada que solo se debe modificar si se producen errores de tiempo de espera del Protocolo de enlace debido a una latencia de red grave.</span><span class="sxs-lookup"><span data-stu-id="64230-589">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="64230-590">Para obtener más información sobre el proceso de enlace, consulte la [especificación del Protocolo de concentrador deSignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="64230-590">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="64230-591">15 segundos</span><span class="sxs-lookup"><span data-stu-id="64230-591">15 seconds</span></span> | <span data-ttu-id="64230-592">Si el servidor no ha enviado un mensaje dentro de este intervalo, se envía automáticamente un mensaje ping para mantener abierta la conexión.</span><span class="sxs-lookup"><span data-stu-id="64230-592">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="64230-593">Al cambiar `KeepAliveInterval`, cambie la configuración de `serverTimeoutInMilliseconds` de /de `ServerTimeout`en el cliente.</span><span class="sxs-lookup"><span data-stu-id="64230-593">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="64230-594">El `ServerTimeout`recomendado /valor de `serverTimeoutInMilliseconds` es el doble del valor de `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="64230-594">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="64230-595">Todos los protocolos instalados</span><span class="sxs-lookup"><span data-stu-id="64230-595">All installed protocols</span></span> | <span data-ttu-id="64230-596">Protocolos admitidos por este centro.</span><span class="sxs-lookup"><span data-stu-id="64230-596">Protocols supported by this hub.</span></span> <span data-ttu-id="64230-597">De forma predeterminada, todos los protocolos registrados en el servidor están permitidos, pero los protocolos se pueden quitar de esta lista para deshabilitar determinados protocolos para centros individuales.</span><span class="sxs-lookup"><span data-stu-id="64230-597">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="64230-598">Si `true`, los mensajes de excepción detallados se devuelven a los clientes cuando se produce una excepción en un método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="64230-598">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="64230-599">El valor predeterminado es `false`, ya que estos mensajes de excepción pueden contener información confidencial.</span><span class="sxs-lookup"><span data-stu-id="64230-599">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

<span data-ttu-id="64230-600">Las opciones se pueden configurar para todos los concentradores proporcionando un delegado de opciones a la llamada `AddSignalR` en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="64230-600">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="64230-601">Las opciones de un solo concentrador invalidan las opciones globales proporcionadas en `AddSignalR` y se pueden configurar mediante <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span><span class="sxs-lookup"><span data-stu-id="64230-601">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="64230-602">Opciones de configuración de HTTP avanzadas</span><span class="sxs-lookup"><span data-stu-id="64230-602">Advanced HTTP configuration options</span></span>

<span data-ttu-id="64230-603">Use `HttpConnectionDispatcherOptions` para configurar opciones avanzadas relacionadas con los transportes y la administración del búfer de memoria.</span><span class="sxs-lookup"><span data-stu-id="64230-603">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="64230-604">Estas opciones se configuran pasando un delegado a [MapHub\<t >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) en `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="64230-604">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="64230-605">En la tabla siguiente se describen las opciones para configurar las opciones de HTTP avanzadas de ASP.NET Core SignalR:</span><span class="sxs-lookup"><span data-stu-id="64230-605">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

| <span data-ttu-id="64230-606">Opción</span><span class="sxs-lookup"><span data-stu-id="64230-606">Option</span></span> | <span data-ttu-id="64230-607">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="64230-607">Default Value</span></span> | <span data-ttu-id="64230-608">Descripción</span><span class="sxs-lookup"><span data-stu-id="64230-608">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="64230-609">32 KB</span><span class="sxs-lookup"><span data-stu-id="64230-609">32 KB</span></span> | <span data-ttu-id="64230-610">Número máximo de bytes recibidos del cliente que el servidor almacena en búfer.</span><span class="sxs-lookup"><span data-stu-id="64230-610">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="64230-611">Aumentar este valor permite que el servidor reciba mensajes de mayor tamaño, pero puede afectar negativamente al consumo de memoria.</span><span class="sxs-lookup"><span data-stu-id="64230-611">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="64230-612">Los datos se recopilan automáticamente a partir de los atributos `Authorize` aplicados a la clase hub.</span><span class="sxs-lookup"><span data-stu-id="64230-612">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="64230-613">Una lista de objetos [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) que se usan para determinar si un cliente está autorizado para conectarse al centro.</span><span class="sxs-lookup"><span data-stu-id="64230-613">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="64230-614">32 KB</span><span class="sxs-lookup"><span data-stu-id="64230-614">32 KB</span></span> | <span data-ttu-id="64230-615">Número máximo de bytes enviados por la aplicación que el servidor almacena en búfer.</span><span class="sxs-lookup"><span data-stu-id="64230-615">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="64230-616">Al aumentar este valor, el servidor puede enviar mensajes de mayor tamaño, pero puede afectar negativamente al consumo de memoria.</span><span class="sxs-lookup"><span data-stu-id="64230-616">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="64230-617">Todos los transportes están habilitados.</span><span class="sxs-lookup"><span data-stu-id="64230-617">All Transports are enabled.</span></span> | <span data-ttu-id="64230-618">Una enumeración de marcas de bits de `HttpTransportType` valores que pueden restringir los transportes que un cliente puede utilizar para conectarse.</span><span class="sxs-lookup"><span data-stu-id="64230-618">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="64230-619">Véalo a continuación.</span><span class="sxs-lookup"><span data-stu-id="64230-619">See below.</span></span> | <span data-ttu-id="64230-620">Opciones adicionales específicas del transporte de sondeo prolongado.</span><span class="sxs-lookup"><span data-stu-id="64230-620">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="64230-621">Véalo a continuación.</span><span class="sxs-lookup"><span data-stu-id="64230-621">See below.</span></span> | <span data-ttu-id="64230-622">Opciones adicionales específicas del transporte de WebSockets.</span><span class="sxs-lookup"><span data-stu-id="64230-622">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="64230-623">El transporte de sondeo largo tiene opciones adicionales que se pueden configurar mediante la propiedad `LongPolling`:</span><span class="sxs-lookup"><span data-stu-id="64230-623">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="64230-624">Opción</span><span class="sxs-lookup"><span data-stu-id="64230-624">Option</span></span> | <span data-ttu-id="64230-625">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="64230-625">Default Value</span></span> | <span data-ttu-id="64230-626">Descripción</span><span class="sxs-lookup"><span data-stu-id="64230-626">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="64230-627">90 segundos</span><span class="sxs-lookup"><span data-stu-id="64230-627">90 seconds</span></span> | <span data-ttu-id="64230-628">Cantidad máxima de tiempo que el servidor espera a que se envíe un mensaje al cliente antes de finalizar una única solicitud de sondeo.</span><span class="sxs-lookup"><span data-stu-id="64230-628">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="64230-629">Al disminuir este valor, el cliente emite nuevas solicitudes de sondeo con mayor frecuencia.</span><span class="sxs-lookup"><span data-stu-id="64230-629">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="64230-630">El transporte de WebSocket tiene opciones adicionales que se pueden configurar mediante la propiedad `WebSockets`:</span><span class="sxs-lookup"><span data-stu-id="64230-630">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="64230-631">Opción</span><span class="sxs-lookup"><span data-stu-id="64230-631">Option</span></span> | <span data-ttu-id="64230-632">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="64230-632">Default Value</span></span> | <span data-ttu-id="64230-633">Descripción</span><span class="sxs-lookup"><span data-stu-id="64230-633">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="64230-634">5 segundos</span><span class="sxs-lookup"><span data-stu-id="64230-634">5 seconds</span></span> | <span data-ttu-id="64230-635">Una vez que se cierra el servidor, si el cliente no se cierra dentro de este intervalo de tiempo, se finaliza la conexión.</span><span class="sxs-lookup"><span data-stu-id="64230-635">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="64230-636">Delegado que se puede utilizar para establecer el encabezado de `Sec-WebSocket-Protocol` en un valor personalizado.</span><span class="sxs-lookup"><span data-stu-id="64230-636">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="64230-637">El delegado recibe los valores solicitados por el cliente como entrada y se espera que devuelva el valor deseado.</span><span class="sxs-lookup"><span data-stu-id="64230-637">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="64230-638">Configurar opciones de cliente</span><span class="sxs-lookup"><span data-stu-id="64230-638">Configure client options</span></span>

<span data-ttu-id="64230-639">Las opciones de cliente se pueden configurar en el tipo de `HubConnectionBuilder` (disponible en los clientes de .NET y JavaScript).</span><span class="sxs-lookup"><span data-stu-id="64230-639">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="64230-640">También está disponible en el cliente de Java, pero la subclase `HttpHubConnectionBuilder` es lo que contiene las opciones de configuración del generador, así como en el `HubConnection` mismo.</span><span class="sxs-lookup"><span data-stu-id="64230-640">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="64230-641">registro</span><span class="sxs-lookup"><span data-stu-id="64230-641">Configure logging</span></span>

<span data-ttu-id="64230-642">El registro se configura en el cliente .NET mediante el método `ConfigureLogging`.</span><span class="sxs-lookup"><span data-stu-id="64230-642">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="64230-643">Los proveedores de registro y los filtros se pueden registrar de la misma manera que en el servidor.</span><span class="sxs-lookup"><span data-stu-id="64230-643">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="64230-644">Consulte la documentación de [Inicio de sesión ASP.net Core](xref:fundamentals/logging/index) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="64230-644">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="64230-645">Para registrar los proveedores de registro, debe instalar los paquetes necesarios.</span><span class="sxs-lookup"><span data-stu-id="64230-645">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="64230-646">Consulte la sección [proveedores de registro integrados](xref:fundamentals/logging/index#built-in-logging-providers) de los documentos para obtener una lista completa.</span><span class="sxs-lookup"><span data-stu-id="64230-646">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="64230-647">Por ejemplo, para habilitar el registro de la consola, instale el paquete NuGet `Microsoft.Extensions.Logging.Console`.</span><span class="sxs-lookup"><span data-stu-id="64230-647">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="64230-648">Llame al método de extensión `AddConsole`:</span><span class="sxs-lookup"><span data-stu-id="64230-648">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="64230-649">En el cliente de JavaScript, existe un método `configureLogging` similar.</span><span class="sxs-lookup"><span data-stu-id="64230-649">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="64230-650">Proporcione un valor `LogLevel` que indique el nivel mínimo de mensajes de registro que se va a producir.</span><span class="sxs-lookup"><span data-stu-id="64230-650">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="64230-651">Los registros se escriben en la ventana de la consola del explorador.</span><span class="sxs-lookup"><span data-stu-id="64230-651">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> <span data-ttu-id="64230-652">Para deshabilitar el registro por completo, especifique `signalR.LogLevel.None` en el método `configureLogging`.</span><span class="sxs-lookup"><span data-stu-id="64230-652">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="64230-653">Para obtener más información sobre el registro, consulte la [documentación de diagnóstico deSignalR](xref:signalr/diagnostics).</span><span class="sxs-lookup"><span data-stu-id="64230-653">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="64230-654">El cliente de Java de SignalR usa la biblioteca [SLF4J](https://www.slf4j.org/) para el registro.</span><span class="sxs-lookup"><span data-stu-id="64230-654">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="64230-655">Se trata de una API de registro de alto nivel que permite a los usuarios de la biblioteca elegir su propia implementación de registro específica mediante la incorporación de una dependencia de registro específica.</span><span class="sxs-lookup"><span data-stu-id="64230-655">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="64230-656">En el fragmento de código siguiente se muestra cómo usar `java.util.logging` con el cliente de Java SignalR.</span><span class="sxs-lookup"><span data-stu-id="64230-656">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="64230-657">Si no configura el registro en las dependencias, SLF4J carga un registrador de no operaciones predeterminado con el siguiente mensaje de ADVERTENCIA:</span><span class="sxs-lookup"><span data-stu-id="64230-657">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="64230-658">Esto se puede omitir sin ningún riesgo.</span><span class="sxs-lookup"><span data-stu-id="64230-658">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="64230-659">Configurar transportes permitidos</span><span class="sxs-lookup"><span data-stu-id="64230-659">Configure allowed transports</span></span>

<span data-ttu-id="64230-660">Los transportes utilizados por SignalR pueden configurarse en la llamada `WithUrl` (`withUrl` en JavaScript).</span><span class="sxs-lookup"><span data-stu-id="64230-660">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="64230-661">Una operación OR bit a bit de los valores de `HttpTransportType` se puede utilizar para restringir el cliente de modo que solo utilice los transportes especificados.</span><span class="sxs-lookup"><span data-stu-id="64230-661">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="64230-662">Todos los transportes están habilitados de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="64230-662">All transports are enabled by default.</span></span>

<span data-ttu-id="64230-663">Por ejemplo, para deshabilitar el transporte de eventos enviados por el servidor, pero permitir WebSockets y conexiones de sondeo largas:</span><span class="sxs-lookup"><span data-stu-id="64230-663">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="64230-664">En el cliente de JavaScript, los transportes se configuran estableciendo el campo `transport` en el objeto de opciones proporcionado para `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="64230-664">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

### <a name="configure-bearer-authentication"></a><span data-ttu-id="64230-665">Configurar la autenticación de portador</span><span class="sxs-lookup"><span data-stu-id="64230-665">Configure bearer authentication</span></span>

<span data-ttu-id="64230-666">Para proporcionar datos de autenticación junto con solicitudes de SignalR, use la opción `AccessTokenProvider` (`accessTokenFactory` en JavaScript) para especificar una función que devuelva el token de acceso deseado.</span><span class="sxs-lookup"><span data-stu-id="64230-666">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="64230-667">En el cliente .NET, este token de acceso se pasa como un token de "autenticación de portador" de HTTP (mediante el encabezado `Authorization` con un tipo de `Bearer`).</span><span class="sxs-lookup"><span data-stu-id="64230-667">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="64230-668">En el cliente de JavaScript, el token de acceso se usa como un token de portador, **excepto** en algunos casos en los que las API del explorador restringen la capacidad de aplicar encabezados (específicamente, en los eventos enviados por el servidor y en las solicitudes de WebSockets).</span><span class="sxs-lookup"><span data-stu-id="64230-668">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="64230-669">En estos casos, el token de acceso se proporciona como un valor de cadena de consulta `access_token`.</span><span class="sxs-lookup"><span data-stu-id="64230-669">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="64230-670">En el cliente .NET, se puede especificar la opción `AccessTokenProvider` mediante el delegado Options en `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="64230-670">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="64230-671">En el cliente de JavaScript, el token de acceso se configura estableciendo el campo `accessTokenFactory` del objeto de opciones en `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="64230-671">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

<span data-ttu-id="64230-672">En el cliente de SignalR Java, puede configurar un token de portador que se usará para la autenticación proporcionando un generador de tokens de acceso a [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span><span class="sxs-lookup"><span data-stu-id="64230-672">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="64230-673">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) para proporcionar un [RxJava](https://github.com/ReactiveX/RxJava) de una [única\<cadena>](https://reactivex.io/documentation/single.html).</span><span class="sxs-lookup"><span data-stu-id="64230-673">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="64230-674">Con una llamada a [Single. defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), puede escribir lógica para generar tokens de acceso para el cliente.</span><span class="sxs-lookup"><span data-stu-id="64230-674">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="64230-675">Configurar las opciones de tiempo de espera y mantenimiento de conexión</span><span class="sxs-lookup"><span data-stu-id="64230-675">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="64230-676">Las opciones adicionales para configurar el comportamiento de tiempo de espera y mantenimiento de conexión están disponibles en el propio `HubConnection` objeto:</span><span class="sxs-lookup"><span data-stu-id="64230-676">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

# <a name="nettabdotnet"></a>[<span data-ttu-id="64230-677">.NET</span><span class="sxs-lookup"><span data-stu-id="64230-677">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="64230-678">Opción</span><span class="sxs-lookup"><span data-stu-id="64230-678">Option</span></span> | <span data-ttu-id="64230-679">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="64230-679">Default value</span></span> | <span data-ttu-id="64230-680">Descripción</span><span class="sxs-lookup"><span data-stu-id="64230-680">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="64230-681">30 segundos (30.000 milisegundos)</span><span class="sxs-lookup"><span data-stu-id="64230-681">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="64230-682">Tiempo de espera para la actividad del servidor.</span><span class="sxs-lookup"><span data-stu-id="64230-682">Timeout for server activity.</span></span> <span data-ttu-id="64230-683">Si el servidor no ha enviado un mensaje en este intervalo, el cliente considera que el servidor está desconectado y desencadena el evento `Closed` (`onclose` en JavaScript).</span><span class="sxs-lookup"><span data-stu-id="64230-683">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="64230-684">Este valor debe ser lo suficientemente grande como para que se envíe un mensaje ping desde el servidor **y** el cliente lo reciba dentro del intervalo de tiempo de espera.</span><span class="sxs-lookup"><span data-stu-id="64230-684">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="64230-685">El valor recomendado es un número al menos el doble del valor del `KeepAliveInterval` del servidor para permitir que lleguen los ping.</span><span class="sxs-lookup"><span data-stu-id="64230-685">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="64230-686">15 segundos</span><span class="sxs-lookup"><span data-stu-id="64230-686">15 seconds</span></span> | <span data-ttu-id="64230-687">Tiempo de espera para el protocolo de enlace inicial del servidor.</span><span class="sxs-lookup"><span data-stu-id="64230-687">Timeout for initial server handshake.</span></span> <span data-ttu-id="64230-688">Si el servidor no envía una respuesta de protocolo de enlace en este intervalo, el cliente cancela el protocolo de enlace y desencadena el evento `Closed` (`onclose` en JavaScript).</span><span class="sxs-lookup"><span data-stu-id="64230-688">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="64230-689">Se trata de una configuración avanzada que solo se debe modificar si se producen errores de tiempo de espera del Protocolo de enlace debido a una latencia de red grave.</span><span class="sxs-lookup"><span data-stu-id="64230-689">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="64230-690">Para obtener más información sobre el proceso de enlace, consulte la [especificación del Protocolo de concentrador deSignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="64230-690">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

<span data-ttu-id="64230-691">En el cliente de .NET, los valores de tiempo de espera se especifican como valores de `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="64230-691">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="64230-692">JavaScript</span><span class="sxs-lookup"><span data-stu-id="64230-692">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="64230-693">Opción</span><span class="sxs-lookup"><span data-stu-id="64230-693">Option</span></span> | <span data-ttu-id="64230-694">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="64230-694">Default value</span></span> | <span data-ttu-id="64230-695">Descripción</span><span class="sxs-lookup"><span data-stu-id="64230-695">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="64230-696">30 segundos (30.000 milisegundos)</span><span class="sxs-lookup"><span data-stu-id="64230-696">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="64230-697">Tiempo de espera para la actividad del servidor.</span><span class="sxs-lookup"><span data-stu-id="64230-697">Timeout for server activity.</span></span> <span data-ttu-id="64230-698">Si el servidor no ha enviado un mensaje en este intervalo, el cliente considera que el servidor está desconectado y desencadena el evento `onclose`.</span><span class="sxs-lookup"><span data-stu-id="64230-698">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="64230-699">Este valor debe ser lo suficientemente grande como para que se envíe un mensaje ping desde el servidor **y** el cliente lo reciba dentro del intervalo de tiempo de espera.</span><span class="sxs-lookup"><span data-stu-id="64230-699">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="64230-700">El valor recomendado es un número al menos el doble del valor del `KeepAliveInterval` del servidor para permitir que lleguen los ping.</span><span class="sxs-lookup"><span data-stu-id="64230-700">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="64230-701">Java</span><span class="sxs-lookup"><span data-stu-id="64230-701">Java</span></span>](#tab/java)

| <span data-ttu-id="64230-702">Opción</span><span class="sxs-lookup"><span data-stu-id="64230-702">Option</span></span> | <span data-ttu-id="64230-703">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="64230-703">Default value</span></span> | <span data-ttu-id="64230-704">Descripción</span><span class="sxs-lookup"><span data-stu-id="64230-704">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="64230-705">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="64230-705">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="64230-706">30 segundos (30.000 milisegundos)</span><span class="sxs-lookup"><span data-stu-id="64230-706">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="64230-707">Tiempo de espera para la actividad del servidor.</span><span class="sxs-lookup"><span data-stu-id="64230-707">Timeout for server activity.</span></span> <span data-ttu-id="64230-708">Si el servidor no ha enviado un mensaje en este intervalo, el cliente considera que el servidor está desconectado y desencadena el evento `onClose`.</span><span class="sxs-lookup"><span data-stu-id="64230-708">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="64230-709">Este valor debe ser lo suficientemente grande como para que se envíe un mensaje ping desde el servidor **y** el cliente lo reciba dentro del intervalo de tiempo de espera.</span><span class="sxs-lookup"><span data-stu-id="64230-709">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="64230-710">El valor recomendado es un número al menos el doble del valor del `KeepAliveInterval` del servidor, para permitir que llegue el tiempo para que lleguen los ping.</span><span class="sxs-lookup"><span data-stu-id="64230-710">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="64230-711">15 segundos</span><span class="sxs-lookup"><span data-stu-id="64230-711">15 seconds</span></span> | <span data-ttu-id="64230-712">Tiempo de espera para el protocolo de enlace inicial del servidor.</span><span class="sxs-lookup"><span data-stu-id="64230-712">Timeout for initial server handshake.</span></span> <span data-ttu-id="64230-713">Si el servidor no envía una respuesta de protocolo de enlace en este intervalo, el cliente cancela el protocolo de enlace y desencadena el evento `onClose`.</span><span class="sxs-lookup"><span data-stu-id="64230-713">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="64230-714">Se trata de una configuración avanzada que solo se debe modificar si se producen errores de tiempo de espera del Protocolo de enlace debido a una latencia de red grave.</span><span class="sxs-lookup"><span data-stu-id="64230-714">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="64230-715">Para obtener más información sobre el proceso de enlace, consulte la [especificación del Protocolo de concentrador deSignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="64230-715">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

---

### <a name="configure-additional-options"></a><span data-ttu-id="64230-716">Configuración de opciones adicionales</span><span class="sxs-lookup"><span data-stu-id="64230-716">Configure additional options</span></span>

<span data-ttu-id="64230-717">Se pueden configurar opciones adicionales en el método `WithUrl` (`withUrl` en JavaScript) en `HubConnectionBuilder` o en las diversas API de configuración de la `HttpHubConnectionBuilder` en el cliente de Java:</span><span class="sxs-lookup"><span data-stu-id="64230-717">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="nettabdotnet"></a>[<span data-ttu-id="64230-718">.NET</span><span class="sxs-lookup"><span data-stu-id="64230-718">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="64230-719">Opción de .NET</span><span class="sxs-lookup"><span data-stu-id="64230-719">.NET Option</span></span> |  <span data-ttu-id="64230-720">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="64230-720">Default value</span></span> | <span data-ttu-id="64230-721">Descripción</span><span class="sxs-lookup"><span data-stu-id="64230-721">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="64230-722">Función que devuelve una cadena que se proporciona como un token de autenticación de portador en solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="64230-722">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="64230-723">Establezca este valor en `true` para omitir el paso de negociación.</span><span class="sxs-lookup"><span data-stu-id="64230-723">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="64230-724">**Solo se admite cuando el transporte de WebSockets es el único transporte habilitado**.</span><span class="sxs-lookup"><span data-stu-id="64230-724">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="64230-725">Esta configuración no se puede habilitar cuando se usa el servicio Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="64230-725">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="64230-726">Empty</span><span class="sxs-lookup"><span data-stu-id="64230-726">Empty</span></span> | <span data-ttu-id="64230-727">Colección de certificados TLS que se enviarán a las solicitudes de autenticación.</span><span class="sxs-lookup"><span data-stu-id="64230-727">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="64230-728">Empty</span><span class="sxs-lookup"><span data-stu-id="64230-728">Empty</span></span> | <span data-ttu-id="64230-729">Colección de cookies HTTP que se enviarán con cada solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="64230-729">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="64230-730">Empty</span><span class="sxs-lookup"><span data-stu-id="64230-730">Empty</span></span> | <span data-ttu-id="64230-731">Credenciales que se van a enviar con cada solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="64230-731">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="64230-732">5 segundos</span><span class="sxs-lookup"><span data-stu-id="64230-732">5 seconds</span></span> | <span data-ttu-id="64230-733">Solo WebSockets.</span><span class="sxs-lookup"><span data-stu-id="64230-733">WebSockets only.</span></span> <span data-ttu-id="64230-734">Cantidad máxima de tiempo que el cliente espera después de cerrarse para que el servidor confirme la solicitud de cierre.</span><span class="sxs-lookup"><span data-stu-id="64230-734">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="64230-735">Si el servidor no reconoce el cierre dentro de este tiempo, el cliente se desconecta.</span><span class="sxs-lookup"><span data-stu-id="64230-735">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="64230-736">Empty</span><span class="sxs-lookup"><span data-stu-id="64230-736">Empty</span></span> | <span data-ttu-id="64230-737">Asignación de encabezados HTTP adicionales que se van a enviar con cada solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="64230-737">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="64230-738">Delegado que se puede utilizar para configurar o reemplazar el `HttpMessageHandler` utilizado para enviar solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="64230-738">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="64230-739">No se usa para las conexiones WebSocket.</span><span class="sxs-lookup"><span data-stu-id="64230-739">Not used for WebSocket connections.</span></span> <span data-ttu-id="64230-740">Este delegado debe devolver un valor distinto de NULL y recibe el valor predeterminado como parámetro.</span><span class="sxs-lookup"><span data-stu-id="64230-740">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="64230-741">Modifique la configuración de ese valor predeterminado y devuelva o devuelva una nueva instancia de `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="64230-741">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="64230-742">**Al reemplazar el controlador, asegúrese de copiar la configuración que desea conservar del controlador proporcionado; de lo contrario, las opciones configuradas (como cookies y encabezados) no se aplicarán al nuevo controlador.**</span><span class="sxs-lookup"><span data-stu-id="64230-742">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="64230-743">Proxy HTTP que se va a usar al enviar solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="64230-743">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="64230-744">Establezca este valor booleano para enviar las credenciales predeterminadas para las solicitudes HTTP y WebSockets.</span><span class="sxs-lookup"><span data-stu-id="64230-744">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="64230-745">Esto habilita el uso de la autenticación de Windows.</span><span class="sxs-lookup"><span data-stu-id="64230-745">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="64230-746">Delegado que se puede usar para configurar opciones adicionales de WebSocket.</span><span class="sxs-lookup"><span data-stu-id="64230-746">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="64230-747">Recibe una instancia de [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) que se puede usar para configurar las opciones.</span><span class="sxs-lookup"><span data-stu-id="64230-747">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="64230-748">JavaScript</span><span class="sxs-lookup"><span data-stu-id="64230-748">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="64230-749">Opción de JavaScript</span><span class="sxs-lookup"><span data-stu-id="64230-749">JavaScript Option</span></span> | <span data-ttu-id="64230-750">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="64230-750">Default Value</span></span> | <span data-ttu-id="64230-751">Descripción</span><span class="sxs-lookup"><span data-stu-id="64230-751">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="64230-752">Función que devuelve una cadena que se proporciona como un token de autenticación de portador en solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="64230-752">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="64230-753">Establezca este valor en `true` para omitir el paso de negociación.</span><span class="sxs-lookup"><span data-stu-id="64230-753">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="64230-754">**Solo se admite cuando el transporte de WebSockets es el único transporte habilitado**.</span><span class="sxs-lookup"><span data-stu-id="64230-754">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="64230-755">Esta configuración no se puede habilitar cuando se usa el servicio Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="64230-755">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="64230-756">Java</span><span class="sxs-lookup"><span data-stu-id="64230-756">Java</span></span>](#tab/java)

| <span data-ttu-id="64230-757">Opción de Java</span><span class="sxs-lookup"><span data-stu-id="64230-757">Java Option</span></span> | <span data-ttu-id="64230-758">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="64230-758">Default Value</span></span> | <span data-ttu-id="64230-759">Descripción</span><span class="sxs-lookup"><span data-stu-id="64230-759">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="64230-760">Función que devuelve una cadena que se proporciona como un token de autenticación de portador en solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="64230-760">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="64230-761">Establezca este valor en `true` para omitir el paso de negociación.</span><span class="sxs-lookup"><span data-stu-id="64230-761">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="64230-762">**Solo se admite cuando el transporte de WebSockets es el único transporte habilitado**.</span><span class="sxs-lookup"><span data-stu-id="64230-762">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="64230-763">Esta configuración no se puede habilitar cuando se usa el servicio Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="64230-763">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="64230-764">`withHeader` `withHeaders`</span><span class="sxs-lookup"><span data-stu-id="64230-764">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="64230-765">Empty</span><span class="sxs-lookup"><span data-stu-id="64230-765">Empty</span></span> | <span data-ttu-id="64230-766">Asignación de encabezados HTTP adicionales que se van a enviar con cada solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="64230-766">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="64230-767">En el cliente .NET, estas opciones se pueden modificar mediante el delegado de opciones proporcionado a `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="64230-767">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="64230-768">En el cliente de JavaScript, estas opciones se pueden proporcionar en un objeto de JavaScript proporcionado a `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="64230-768">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="64230-769">En el cliente de Java, estas opciones se pueden configurar con los métodos del `HttpHubConnectionBuilder` devueltos desde el `HubConnectionBuilder.create("HUB URL")`</span><span class="sxs-lookup"><span data-stu-id="64230-769">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="64230-770">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="64230-770">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>

::: moniker-end