---
title: Configuración de ASP.NET Core SignalR
author: bradygaster
description: Obtenga información sobre cómo configurar aplicaciones de ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 02/07/2019
uid: signalr/configuration
ms.openlocfilehash: 070d6fed26b6d14c4b8a35d0f7d94abafb08993b
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665420"
---
# <a name="aspnet-core-signalr-configuration"></a><span data-ttu-id="ae513-103">Configuración de ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="ae513-103">ASP.NET Core SignalR configuration</span></span>

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="ae513-104">Opciones de serialización de JSON/MessagePack</span><span class="sxs-lookup"><span data-stu-id="ae513-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="ae513-105">SignalR de ASP.NET Core admite dos protocolos para la codificación de mensajes: [JSON](https://www.json.org/) y [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="ae513-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="ae513-106">Cada protocolo tiene opciones de configuración de serialización.</span><span class="sxs-lookup"><span data-stu-id="ae513-106">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="ae513-107">Serialización de JSON se puede configurar en el servidor mediante el [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) método de extensión, que se puede agregar después [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) en su `Startup.ConfigureServices` método.</span><span class="sxs-lookup"><span data-stu-id="ae513-107">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="ae513-108">El `AddJsonProtocol` método toma un delegado que recibe un `options` objeto.</span><span class="sxs-lookup"><span data-stu-id="ae513-108">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="ae513-109">El [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) propiedad en ese objeto es un JSON.NET `JsonSerializerSettings` objeto que puede usarse para configurar la serialización de argumentos y valores devueltos.</span><span class="sxs-lookup"><span data-stu-id="ae513-109">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="ae513-110">Consulte la [documentación JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm) para obtener más detalles.</span><span class="sxs-lookup"><span data-stu-id="ae513-110">See the [JSON.NET Documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm) for more details.</span></span>

<span data-ttu-id="ae513-111">Por ejemplo, para configurar el serializador para usar nombres de propiedad "PascalCase", en lugar de los nombres predeterminados "camelCase", use el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="ae513-111">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code:</span></span>

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver = 
        new DefaultContractResolver();
    });
```

<span data-ttu-id="ae513-112">En el cliente. NET, la misma `AddJsonProtocol` existe en el método de extensión [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="ae513-112">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="ae513-113">El `Microsoft.Extensions.DependencyInjection` se debe importar el espacio de nombres para resolver el método de extensión:</span><span class="sxs-lookup"><span data-stu-id="ae513-113">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="ae513-114">No es posible configurar la serialización de JSON en el cliente de JavaScript en este momento.</span><span class="sxs-lookup"><span data-stu-id="ae513-114">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="ae513-115">Opciones de serialización MessagePack</span><span class="sxs-lookup"><span data-stu-id="ae513-115">MessagePack serialization options</span></span>

<span data-ttu-id="ae513-116">MessagePack serialización se puede configurar si se proporciona un delegado para el [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) llamar.</span><span class="sxs-lookup"><span data-stu-id="ae513-116">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="ae513-117">Consulte [MessagePack en SignalR](xref:signalr/messagepackhubprotocol) para obtener más detalles.</span><span class="sxs-lookup"><span data-stu-id="ae513-117">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="ae513-118">No es posible configurar la serialización de MessagePack en el cliente de JavaScript en este momento.</span><span class="sxs-lookup"><span data-stu-id="ae513-118">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="ae513-119">Configurar opciones de servidor</span><span class="sxs-lookup"><span data-stu-id="ae513-119">Configure server options</span></span>

<span data-ttu-id="ae513-120">En la tabla siguiente se describe las opciones para configurar los concentradores de SignalR:</span><span class="sxs-lookup"><span data-stu-id="ae513-120">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="ae513-121">Opción</span><span class="sxs-lookup"><span data-stu-id="ae513-121">Option</span></span> | <span data-ttu-id="ae513-122">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="ae513-122">Default Value</span></span> | <span data-ttu-id="ae513-123">Descripción</span><span class="sxs-lookup"><span data-stu-id="ae513-123">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="ae513-124">30 segundos</span><span class="sxs-lookup"><span data-stu-id="ae513-124">30 seconds</span></span> | <span data-ttu-id="ae513-125">El servidor tendrá en cuenta el cliente desconectado si no ha recibido un mensaje (incluido keep-alive) en este intervalo.</span><span class="sxs-lookup"><span data-stu-id="ae513-125">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="ae513-126">Puede tardar más de este intervalo de tiempo de espera para el cliente realmente marcarse desconectado debido a su implementación.</span><span class="sxs-lookup"><span data-stu-id="ae513-126">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="ae513-127">El valor recomendado es doble el `KeepAliveInterval` valor.</span><span class="sxs-lookup"><span data-stu-id="ae513-127">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="ae513-128">15 segundos</span><span class="sxs-lookup"><span data-stu-id="ae513-128">15 seconds</span></span> | <span data-ttu-id="ae513-129">Si el cliente no envía un mensaje de protocolo de enlace inicial dentro de este intervalo de tiempo, se cierra la conexión.</span><span class="sxs-lookup"><span data-stu-id="ae513-129">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="ae513-130">Se trata de una opción avanzada que sólo debería modificarse si se producen errores de tiempo de espera del protocolo de enlace debido a la latencia de red graves.</span><span class="sxs-lookup"><span data-stu-id="ae513-130">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="ae513-131">Para obtener más detalles sobre el proceso de negociación, consulte el [especificación del protocolo SignalR Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="ae513-131">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="ae513-132">15 segundos</span><span class="sxs-lookup"><span data-stu-id="ae513-132">15 seconds</span></span> | <span data-ttu-id="ae513-133">Si el servidor no ha enviado un mensaje dentro de este intervalo, se envía automáticamente un mensaje ping para mantener abierta la conexión.</span><span class="sxs-lookup"><span data-stu-id="ae513-133">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="ae513-134">Al cambiar `KeepAliveInterval`, cambie el `ServerTimeout` / `serverTimeoutInMilliseconds` configuración del cliente.</span><span class="sxs-lookup"><span data-stu-id="ae513-134">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="ae513-135">Recomendado `ServerTimeout` / `serverTimeoutInMilliseconds` valor es doble el `KeepAliveInterval` valor.</span><span class="sxs-lookup"><span data-stu-id="ae513-135">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="ae513-136">Todos los protocolos instalados</span><span class="sxs-lookup"><span data-stu-id="ae513-136">All installed protocols</span></span> | <span data-ttu-id="ae513-137">Protocolos admitidos por este concentrador.</span><span class="sxs-lookup"><span data-stu-id="ae513-137">Protocols supported by this hub.</span></span> <span data-ttu-id="ae513-138">De forma predeterminada, se permiten todos los protocolos registrados en el servidor, pero se pueden quitar protocolos de esta lista para deshabilitar los protocolos específicos para los concentradores individuales.</span><span class="sxs-lookup"><span data-stu-id="ae513-138">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="ae513-139">Si `true`detallados se devuelven los mensajes de excepción a los clientes cuando se produce una excepción en un método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="ae513-139">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="ae513-140">El valor predeterminado es `false`, ya que estos mensajes de excepción pueden contener información confidencial.</span><span class="sxs-lookup"><span data-stu-id="ae513-140">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

<span data-ttu-id="ae513-141">Se pueden configurar opciones para todos los centros proporcionando un delegado de opciones para la `AddSignalR` llamar `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="ae513-141">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="ae513-142">Opciones para un único centro de invalidan las opciones globales de `AddSignalR` y pueden configurarse mediante <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span><span class="sxs-lookup"><span data-stu-id="ae513-142">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="ae513-143">Opciones de configuración de HTTP avanzadas</span><span class="sxs-lookup"><span data-stu-id="ae513-143">Advanced HTTP configuration options</span></span>

<span data-ttu-id="ae513-144">Use `HttpConnectionDispatcherOptions` para configurar opciones avanzadas relacionadas con transportes y administración de búfer de memoria.</span><span class="sxs-lookup"><span data-stu-id="ae513-144">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="ae513-145">Estas opciones se configuran pasando un delegado para [MapHub\<T >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) en `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="ae513-145">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="ae513-146">En la tabla siguiente se describe las opciones para configurar las opciones avanzadas de HTTP de ASP.NET Core SignalR:</span><span class="sxs-lookup"><span data-stu-id="ae513-146">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

| <span data-ttu-id="ae513-147">Opción</span><span class="sxs-lookup"><span data-stu-id="ae513-147">Option</span></span> | <span data-ttu-id="ae513-148">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="ae513-148">Default Value</span></span> | <span data-ttu-id="ae513-149">Descripción</span><span class="sxs-lookup"><span data-stu-id="ae513-149">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="ae513-150">32 KB</span><span class="sxs-lookup"><span data-stu-id="ae513-150">32 KB</span></span> | <span data-ttu-id="ae513-151">El número máximo de bytes recibidos del cliente que los búferes del servidor.</span><span class="sxs-lookup"><span data-stu-id="ae513-151">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="ae513-152">Al aumentar este valor permite que el servidor recibir los mensajes más grandes, pero puede repercutir negativamente en el consumo de memoria.</span><span class="sxs-lookup"><span data-stu-id="ae513-152">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="ae513-153">Recopilar automáticamente los datos de la `Authorize` atributos aplicados a la clase de concentrador.</span><span class="sxs-lookup"><span data-stu-id="ae513-153">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="ae513-154">Una lista de [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) los objetos utilizados para determinar si un cliente está autorizado para conectarse al concentrador.</span><span class="sxs-lookup"><span data-stu-id="ae513-154">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="ae513-155">32 KB</span><span class="sxs-lookup"><span data-stu-id="ae513-155">32 KB</span></span> | <span data-ttu-id="ae513-156">El número máximo de bytes enviados por la aplicación que los búferes del servidor.</span><span class="sxs-lookup"><span data-stu-id="ae513-156">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="ae513-157">Al aumentar este valor permite que el servidor enviar los mensajes más grandes, pero puede repercutir negativamente en el consumo de memoria.</span><span class="sxs-lookup"><span data-stu-id="ae513-157">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="ae513-158">Se habilitan todos los transportes.</span><span class="sxs-lookup"><span data-stu-id="ae513-158">All Transports are enabled.</span></span> | <span data-ttu-id="ae513-159">Una máscara de bits de `HttpTransportType` valores que pueden restringir los transportes que un cliente puede usar para conectarse.</span><span class="sxs-lookup"><span data-stu-id="ae513-159">A bitmask of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="ae513-160">Véalo a continuación.</span><span class="sxs-lookup"><span data-stu-id="ae513-160">See below.</span></span> | <span data-ttu-id="ae513-161">Opciones adicionales específicas para el transporte de sondeo largo.</span><span class="sxs-lookup"><span data-stu-id="ae513-161">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="ae513-162">Véalo a continuación.</span><span class="sxs-lookup"><span data-stu-id="ae513-162">See below.</span></span> | <span data-ttu-id="ae513-163">Opciones adicionales específicas para el transporte de WebSockets.</span><span class="sxs-lookup"><span data-stu-id="ae513-163">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="ae513-164">El transporte de sondeo largo tiene opciones adicionales que se pueden configurar mediante el `LongPolling` propiedad:</span><span class="sxs-lookup"><span data-stu-id="ae513-164">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="ae513-165">Opción</span><span class="sxs-lookup"><span data-stu-id="ae513-165">Option</span></span> | <span data-ttu-id="ae513-166">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="ae513-166">Default Value</span></span> | <span data-ttu-id="ae513-167">Descripción</span><span class="sxs-lookup"><span data-stu-id="ae513-167">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="ae513-168">90 segundos</span><span class="sxs-lookup"><span data-stu-id="ae513-168">90 seconds</span></span> | <span data-ttu-id="ae513-169">La cantidad máxima de tiempo que el servidor espera para que un mensaje para enviárselo al cliente antes de finalizar una solicitud de sondeo única.</span><span class="sxs-lookup"><span data-stu-id="ae513-169">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="ae513-170">Al disminuir este valor hace que el cliente emitir solicitudes de sondeo de nuevo con más frecuencia.</span><span class="sxs-lookup"><span data-stu-id="ae513-170">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="ae513-171">El transporte de WebSocket tiene opciones adicionales que se pueden configurar mediante el `WebSockets` propiedad:</span><span class="sxs-lookup"><span data-stu-id="ae513-171">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="ae513-172">Opción</span><span class="sxs-lookup"><span data-stu-id="ae513-172">Option</span></span> | <span data-ttu-id="ae513-173">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="ae513-173">Default Value</span></span> | <span data-ttu-id="ae513-174">Descripción</span><span class="sxs-lookup"><span data-stu-id="ae513-174">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="ae513-175">5 segundos</span><span class="sxs-lookup"><span data-stu-id="ae513-175">5 seconds</span></span> | <span data-ttu-id="ae513-176">Después de cerrar el servidor, si se produce un error en el cliente cerrar dentro de este intervalo de tiempo, se termina la conexión.</span><span class="sxs-lookup"><span data-stu-id="ae513-176">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="ae513-177">Un delegado que puede usarse para establecer el `Sec-WebSocket-Protocol` encabezado en un valor personalizado.</span><span class="sxs-lookup"><span data-stu-id="ae513-177">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="ae513-178">El delegado recibe los valores solicitados por el cliente como entrada y se espera que devuelva el valor deseado.</span><span class="sxs-lookup"><span data-stu-id="ae513-178">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="ae513-179">Configurar las opciones de cliente</span><span class="sxs-lookup"><span data-stu-id="ae513-179">Configure client options</span></span>

<span data-ttu-id="ae513-180">Se pueden configurar las opciones de cliente en el `HubConnectionBuilder` tipo (disponible en los clientes de .NET y JavaScript).</span><span class="sxs-lookup"><span data-stu-id="ae513-180">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="ae513-181">También está disponible en el cliente de Java, pero la `HttpHubConnectionBuilder` subclase es lo que contiene las opciones de configuración de generador, así como en el `HubConnection` propio.</span><span class="sxs-lookup"><span data-stu-id="ae513-181">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="ae513-182">Configurar el registro</span><span class="sxs-lookup"><span data-stu-id="ae513-182">Configure logging</span></span>

<span data-ttu-id="ae513-183">El registro está configurado en el cliente de .NET mediante el `ConfigureLogging` método.</span><span class="sxs-lookup"><span data-stu-id="ae513-183">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="ae513-184">Registro de proveedores y los filtros se puede registrar en la misma manera, tal como están en el servidor.</span><span class="sxs-lookup"><span data-stu-id="ae513-184">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="ae513-185">Consulte la [registro en ASP.NET Core](xref:fundamentals/logging/index) documentación para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="ae513-185">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="ae513-186">Con el fin de registrar los proveedores de registro, debe instalar los paquetes necesarios.</span><span class="sxs-lookup"><span data-stu-id="ae513-186">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="ae513-187">Consulte la [proveedores de registro integrados](xref:fundamentals/logging/index#built-in-logging-providers) sección de la documentación para obtener una lista completa.</span><span class="sxs-lookup"><span data-stu-id="ae513-187">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="ae513-188">Por ejemplo, para habilitar el registro de consola, instale el `Microsoft.Extensions.Logging.Console` paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="ae513-188">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="ae513-189">Llame a la `AddConsole` método de extensión:</span><span class="sxs-lookup"><span data-stu-id="ae513-189">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="ae513-190">En el cliente de JavaScript, un proceso similar `configureLogging` método existe.</span><span class="sxs-lookup"><span data-stu-id="ae513-190">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="ae513-191">Proporcione un `LogLevel` valor que indica el nivel mínimo de los mensajes de registro para generar.</span><span class="sxs-lookup"><span data-stu-id="ae513-191">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="ae513-192">Los registros se escriben en la ventana de consola del explorador.</span><span class="sxs-lookup"><span data-stu-id="ae513-192">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> <span data-ttu-id="ae513-193">Para deshabilitar el registro por completo, especifique `signalR.LogLevel.None` en el `configureLogging` método.</span><span class="sxs-lookup"><span data-stu-id="ae513-193">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="ae513-194">Para obtener más información sobre el registro, consulte el [documentación de diagnóstico de SignalR](xref:signalr/diagnostics).</span><span class="sxs-lookup"><span data-stu-id="ae513-194">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="ae513-195">El cliente de SignalR Java usa el [SLF4J](https://www.slf4j.org/) biblioteca para el registro.</span><span class="sxs-lookup"><span data-stu-id="ae513-195">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="ae513-196">Es una API de alto nivel de registro que permite a los usuarios de la biblioteca elegir su propia implementación de registro específico al incorporar una dependencia de registro específico.</span><span class="sxs-lookup"><span data-stu-id="ae513-196">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="ae513-197">El fragmento de código siguiente muestra cómo usar `java.util.logging` con el cliente de SignalR Java.</span><span class="sxs-lookup"><span data-stu-id="ae513-197">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="ae513-198">Si no configura el registro de las dependencias, SLF4J carga un registrador de ninguna operación de forma predeterminada con el mensaje de advertencia siguiente:</span><span class="sxs-lookup"><span data-stu-id="ae513-198">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="ae513-199">Esto puede pasar por alto.</span><span class="sxs-lookup"><span data-stu-id="ae513-199">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="ae513-200">Configurar transportes permitidos</span><span class="sxs-lookup"><span data-stu-id="ae513-200">Configure allowed transports</span></span>

<span data-ttu-id="ae513-201">Los transportes utilizados por SignalR se pueden configurar en el `WithUrl` llamar (`withUrl` en JavaScript).</span><span class="sxs-lookup"><span data-stu-id="ae513-201">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="ae513-202">Una operación OR bit a bit de los valores de `HttpTransportType` puede usarse para restringir el cliente para que solo use los transportes especificados.</span><span class="sxs-lookup"><span data-stu-id="ae513-202">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="ae513-203">Todos los transportes están habilitados de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="ae513-203">All transports are enabled by default.</span></span>

<span data-ttu-id="ae513-204">Por ejemplo, para deshabilitar el transporte de los eventos, pero permitir conexiones WebSockets y Long Polling:</span><span class="sxs-lookup"><span data-stu-id="ae513-204">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="ae513-205">En el cliente de JavaScript, se configuran los transportes estableciendo el `transport` campo en el objeto de opciones proporcionado para `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="ae513-205">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="ae513-206">En esta versión de Java websockets de cliente es el transporte solo está disponible.</span><span class="sxs-lookup"><span data-stu-id="ae513-206">In this version of the Java client websockets is the only available transport.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-3.0"

<span data-ttu-id="ae513-207">En el cliente de Java, el transporte está activado con la `withTransport` método en el `HttpHubConnectionBuilder`.</span><span class="sxs-lookup"><span data-stu-id="ae513-207">In the Java client, the transport is selected with the `withTransport` method on the `HttpHubConnectionBuilder`.</span></span> <span data-ttu-id="ae513-208">De forma predeterminada, el cliente de Java que usa el transporte de WebSockets.</span><span class="sxs-lookup"><span data-stu-id="ae513-208">The Java client defaults to using the WebSockets transport.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withTransport(TransportEnum.WEBSOCKETS)
    .build();
```
> [!NOTE]
> <span data-ttu-id="ae513-209">El cliente de SignalR Java todavía no admite transporte de reserva.</span><span class="sxs-lookup"><span data-stu-id="ae513-209">The SignalR Java client doesn't support transport fallback yet.</span></span>

::: moniker-end

### <a name="configure-bearer-authentication"></a><span data-ttu-id="ae513-210">Configurar la autenticación de portador</span><span class="sxs-lookup"><span data-stu-id="ae513-210">Configure bearer authentication</span></span>

<span data-ttu-id="ae513-211">Para proporcionar datos de autenticación junto con las solicitudes de SignalR, use el `AccessTokenProvider` opción (`accessTokenFactory` en JavaScript) para especificar una función que devuelve el token de acceso deseado.</span><span class="sxs-lookup"><span data-stu-id="ae513-211">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="ae513-212">En el cliente. NET, este token de acceso se pasa como un HTTP "Autenticación del portador" token (mediante el `Authorization` encabezado con un tipo de `Bearer`).</span><span class="sxs-lookup"><span data-stu-id="ae513-212">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="ae513-213">En el cliente de JavaScript, se usa el token de acceso como un token de portador, **excepto** en algunos casos donde las API de explorador restringir la capacidad de aplicar los encabezados (específicamente, en las solicitudes de los eventos y WebSockets).</span><span class="sxs-lookup"><span data-stu-id="ae513-213">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="ae513-214">En estos casos, el token de acceso se proporciona como un valor de cadena de consulta `access_token`.</span><span class="sxs-lookup"><span data-stu-id="ae513-214">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="ae513-215">En el cliente. NET, el `AccessTokenProvider` opción puede especificarse utilizando el delegado de opciones en `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="ae513-215">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="ae513-216">En el cliente de JavaScript, el token de acceso se configura estableciendo el `accessTokenFactory` campo en el objeto de opciones de `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="ae513-216">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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


<span data-ttu-id="ae513-217">En el cliente de SignalR Java, puede configurar un token de portador a usar para la autenticación mediante un generador de token de acceso a la [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span><span class="sxs-lookup"><span data-stu-id="ae513-217">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="ae513-218">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) para proporcionar una [RxJava](https://github.com/ReactiveX/RxJava) [único<String>](http://reactivex.io/documentation/single.html).</span><span class="sxs-lookup"><span data-stu-id="ae513-218">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single<String>](http://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="ae513-219">Con una llamada a [Single.defer](http://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), puede escribir la lógica para generar tokens de acceso para el cliente.</span><span class="sxs-lookup"><span data-stu-id="ae513-219">With a call to [Single.defer](http://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="ae513-220">Configurar el tiempo de espera y las opciones de mantenimiento</span><span class="sxs-lookup"><span data-stu-id="ae513-220">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="ae513-221">Opciones adicionales para configurar el tiempo de espera y el comportamiento de mantenimiento están disponibles en el `HubConnection` propio objeto:</span><span class="sxs-lookup"><span data-stu-id="ae513-221">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

# <a name="nettabdotnet"></a>[<span data-ttu-id="ae513-222">.NET</span><span class="sxs-lookup"><span data-stu-id="ae513-222">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="ae513-223">Opción</span><span class="sxs-lookup"><span data-stu-id="ae513-223">Option</span></span> | <span data-ttu-id="ae513-224">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="ae513-224">Default value</span></span> | <span data-ttu-id="ae513-225">Descripción</span><span class="sxs-lookup"><span data-stu-id="ae513-225">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="ae513-226">30 segundos (30.000 milisegundos)</span><span class="sxs-lookup"><span data-stu-id="ae513-226">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="ae513-227">Tiempo de espera para la actividad del servidor.</span><span class="sxs-lookup"><span data-stu-id="ae513-227">Timeout for server activity.</span></span> <span data-ttu-id="ae513-228">Si el servidor no ha enviado un mensaje en este intervalo, el cliente considera que las ha desconectado el servidor y los desencadenadores la `Closed` eventos (`onclose` en JavaScript).</span><span class="sxs-lookup"><span data-stu-id="ae513-228">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="ae513-229">Este valor debe ser lo suficientemente grande como para un mensaje ping se envíe desde el servidor **y** recibidos por el cliente dentro del intervalo de tiempo de espera.</span><span class="sxs-lookup"><span data-stu-id="ae513-229">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="ae513-230">El valor recomendado es de un número al menos el doble del servidor `KeepAliveInterval` valor, para dejar tiempo para pings de llegada.</span><span class="sxs-lookup"><span data-stu-id="ae513-230">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="ae513-231">15 segundos</span><span class="sxs-lookup"><span data-stu-id="ae513-231">15 seconds</span></span> | <span data-ttu-id="ae513-232">Tiempo de espera para el protocolo de enlace de servidor inicial.</span><span class="sxs-lookup"><span data-stu-id="ae513-232">Timeout for initial server handshake.</span></span> <span data-ttu-id="ae513-233">Si el servidor no envía una respuesta de protocolo de enlace en este intervalo, el cliente cancela el protocolo de enlace y los desencadenadores la `Closed` eventos (`onclose` en JavaScript).</span><span class="sxs-lookup"><span data-stu-id="ae513-233">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="ae513-234">Se trata de una opción avanzada que sólo debería modificarse si se producen errores de tiempo de espera del protocolo de enlace debido a la latencia de red graves.</span><span class="sxs-lookup"><span data-stu-id="ae513-234">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="ae513-235">Para obtener más detalles sobre el proceso de negociación, consulte el [especificación del protocolo SignalR Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="ae513-235">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

<span data-ttu-id="ae513-236">En el cliente. NET, se especifican los valores de tiempo de espera como `TimeSpan` valores.</span><span class="sxs-lookup"><span data-stu-id="ae513-236">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="ae513-237">JavaScript</span><span class="sxs-lookup"><span data-stu-id="ae513-237">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="ae513-238">Opción</span><span class="sxs-lookup"><span data-stu-id="ae513-238">Option</span></span> | <span data-ttu-id="ae513-239">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="ae513-239">Default value</span></span> | <span data-ttu-id="ae513-240">Descripción</span><span class="sxs-lookup"><span data-stu-id="ae513-240">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="ae513-241">30 segundos (30.000 milisegundos)</span><span class="sxs-lookup"><span data-stu-id="ae513-241">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="ae513-242">Tiempo de espera para la actividad del servidor.</span><span class="sxs-lookup"><span data-stu-id="ae513-242">Timeout for server activity.</span></span> <span data-ttu-id="ae513-243">Si el servidor no ha enviado un mensaje en este intervalo, el cliente considera que las ha desconectado el servidor y los desencadenadores la `onclose` eventos.</span><span class="sxs-lookup"><span data-stu-id="ae513-243">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="ae513-244">Este valor debe ser lo suficientemente grande como para un mensaje ping se envíe desde el servidor **y** recibidos por el cliente dentro del intervalo de tiempo de espera.</span><span class="sxs-lookup"><span data-stu-id="ae513-244">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="ae513-245">El valor recomendado es de un número al menos el doble del servidor `KeepAliveInterval` valor, para dejar tiempo para pings de llegada.</span><span class="sxs-lookup"><span data-stu-id="ae513-245">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="ae513-246">Java</span><span class="sxs-lookup"><span data-stu-id="ae513-246">Java</span></span>](#tab/java)

| <span data-ttu-id="ae513-247">Opción</span><span class="sxs-lookup"><span data-stu-id="ae513-247">Option</span></span> | <span data-ttu-id="ae513-248">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="ae513-248">Default value</span></span> | <span data-ttu-id="ae513-249">Descripción</span><span class="sxs-lookup"><span data-stu-id="ae513-249">Description</span></span> |
| ----------- | ------------- | ----------- |
|<span data-ttu-id="ae513-250">`getServerTimeout` `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="ae513-250">`getServerTimeout` `setServerTimeout`</span></span> | <span data-ttu-id="ae513-251">30 segundos (30.000 milisegundos)</span><span class="sxs-lookup"><span data-stu-id="ae513-251">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="ae513-252">Tiempo de espera para la actividad del servidor.</span><span class="sxs-lookup"><span data-stu-id="ae513-252">Timeout for server activity.</span></span> <span data-ttu-id="ae513-253">Si el servidor no ha enviado un mensaje en este intervalo, el cliente considera que las ha desconectado el servidor y los desencadenadores la `onClose` eventos.</span><span class="sxs-lookup"><span data-stu-id="ae513-253">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="ae513-254">Este valor debe ser lo suficientemente grande como para un mensaje ping se envíe desde el servidor **y** recibidos por el cliente dentro del intervalo de tiempo de espera.</span><span class="sxs-lookup"><span data-stu-id="ae513-254">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="ae513-255">El valor recomendado es de un número al menos el doble del servidor `KeepAliveInterval` valor, para dejar tiempo para pings de llegada.</span><span class="sxs-lookup"><span data-stu-id="ae513-255">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="ae513-256">15 segundos</span><span class="sxs-lookup"><span data-stu-id="ae513-256">15 seconds</span></span> | <span data-ttu-id="ae513-257">Tiempo de espera para el protocolo de enlace de servidor inicial.</span><span class="sxs-lookup"><span data-stu-id="ae513-257">Timeout for initial server handshake.</span></span> <span data-ttu-id="ae513-258">Si el servidor no envía una respuesta de protocolo de enlace en este intervalo, el cliente cancela el protocolo de enlace y los desencadenadores la `onClose` eventos.</span><span class="sxs-lookup"><span data-stu-id="ae513-258">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="ae513-259">Se trata de una opción avanzada que sólo debería modificarse si se producen errores de tiempo de espera del protocolo de enlace debido a la latencia de red graves.</span><span class="sxs-lookup"><span data-stu-id="ae513-259">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="ae513-260">Para obtener más detalles sobre el proceso de negociación, consulte el [especificación del protocolo SignalR Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="ae513-260">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

---

### <a name="configure-additional-options"></a><span data-ttu-id="ae513-261">Configurar opciones adicionales</span><span class="sxs-lookup"><span data-stu-id="ae513-261">Configure additional options</span></span>

<span data-ttu-id="ae513-262">Se pueden configurar opciones adicionales en el `WithUrl` (`withUrl` en JavaScript) método `HubConnectionBuilder` o en las diversas API de configuración en el `HttpHubConnectionBuilder` en el cliente de Java:</span><span class="sxs-lookup"><span data-stu-id="ae513-262">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="nettabdotnet"></a>[<span data-ttu-id="ae513-263">.NET</span><span class="sxs-lookup"><span data-stu-id="ae513-263">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="ae513-264">Opción de .NET</span><span class="sxs-lookup"><span data-stu-id="ae513-264">.NET Option</span></span> |  <span data-ttu-id="ae513-265">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="ae513-265">Default value</span></span> | <span data-ttu-id="ae513-266">Descripción</span><span class="sxs-lookup"><span data-stu-id="ae513-266">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="ae513-267">Una función que devuelve una cadena que se proporciona como un token de autenticación de portador en solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="ae513-267">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="ae513-268">Establezca esta opción en `true` para omitir el paso de negociación.</span><span class="sxs-lookup"><span data-stu-id="ae513-268">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="ae513-269">**Solo se admite cuando el transporte de WebSockets es el único tipo de transporte habilitado**.</span><span class="sxs-lookup"><span data-stu-id="ae513-269">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="ae513-270">No se puede habilitar esta configuración al usar Azure SignalR Service.</span><span class="sxs-lookup"><span data-stu-id="ae513-270">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="ae513-271">Empty</span><span class="sxs-lookup"><span data-stu-id="ae513-271">Empty</span></span> | <span data-ttu-id="ae513-272">Una colección de certificados TLS que se envían autenticar las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="ae513-272">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="ae513-273">Empty</span><span class="sxs-lookup"><span data-stu-id="ae513-273">Empty</span></span> | <span data-ttu-id="ae513-274">Una colección de cookies HTTP para enviar con cada solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="ae513-274">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="ae513-275">Empty</span><span class="sxs-lookup"><span data-stu-id="ae513-275">Empty</span></span> | <span data-ttu-id="ae513-276">Credenciales que se enviará con todas las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="ae513-276">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="ae513-277">5 segundos</span><span class="sxs-lookup"><span data-stu-id="ae513-277">5 seconds</span></span> | <span data-ttu-id="ae513-278">Solo WebSockets.</span><span class="sxs-lookup"><span data-stu-id="ae513-278">WebSockets only.</span></span> <span data-ttu-id="ae513-279">La cantidad máxima de tiempo de espera a que el cliente después del cierre del servidor confirmar la solicitud de cierre.</span><span class="sxs-lookup"><span data-stu-id="ae513-279">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="ae513-280">Si el servidor no reconoció el cierre dentro de este tiempo, el cliente se desconecta.</span><span class="sxs-lookup"><span data-stu-id="ae513-280">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="ae513-281">Empty</span><span class="sxs-lookup"><span data-stu-id="ae513-281">Empty</span></span> | <span data-ttu-id="ae513-282">Una asignación de encabezados HTTP adicionales que se enviará con todas las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="ae513-282">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="ae513-283">Un delegado que puede usarse para configurar o reemplazar el `HttpMessageHandler` utilizado para enviar solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="ae513-283">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="ae513-284">No se utiliza para las conexiones de WebSocket.</span><span class="sxs-lookup"><span data-stu-id="ae513-284">Not used for WebSocket connections.</span></span> <span data-ttu-id="ae513-285">Este delegado debe devolver un valor distinto de null, y recibe el valor predeterminado como un parámetro.</span><span class="sxs-lookup"><span data-stu-id="ae513-285">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="ae513-286">Modificar la configuración en ese valor predeterminado y devolverlo o devolver un nuevo `HttpMessageHandler` instancia.</span><span class="sxs-lookup"><span data-stu-id="ae513-286">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="ae513-287">**Cuando Asegúrese de reemplazar el controlador copiar la configuración que desee impedir que el controlador proporcionado, en caso contrario, las opciones configuradas (por ejemplo, Cookies y encabezados) no se aplicarán al nuevo controlador.**</span><span class="sxs-lookup"><span data-stu-id="ae513-287">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="ae513-288">Un proxy HTTP que se utilizará al enviar solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="ae513-288">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="ae513-289">Establezca este valor booleano para enviar las credenciales predeterminadas para las solicitudes HTTP y WebSockets.</span><span class="sxs-lookup"><span data-stu-id="ae513-289">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="ae513-290">Esto permite el uso de autenticación de Windows.</span><span class="sxs-lookup"><span data-stu-id="ae513-290">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="ae513-291">Un delegado que puede usarse para configurar opciones adicionales de WebSocket.</span><span class="sxs-lookup"><span data-stu-id="ae513-291">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="ae513-292">Recibe una instancia de [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) que puede utilizarse para configurar las opciones.</span><span class="sxs-lookup"><span data-stu-id="ae513-292">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="ae513-293">JavaScript</span><span class="sxs-lookup"><span data-stu-id="ae513-293">JavaScript</span></span>](#tab/javascript)
| <span data-ttu-id="ae513-294">Opción de JavaScript</span><span class="sxs-lookup"><span data-stu-id="ae513-294">JavaScript Option</span></span> | <span data-ttu-id="ae513-295">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="ae513-295">Default Value</span></span> | <span data-ttu-id="ae513-296">Descripción</span><span class="sxs-lookup"><span data-stu-id="ae513-296">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="ae513-297">Una función que devuelve una cadena que se proporciona como un token de autenticación de portador en solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="ae513-297">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="ae513-298">Establezca esta opción en `true` para omitir el paso de negociación.</span><span class="sxs-lookup"><span data-stu-id="ae513-298">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="ae513-299">**Solo se admite cuando el transporte de WebSockets es el único tipo de transporte habilitado**.</span><span class="sxs-lookup"><span data-stu-id="ae513-299">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="ae513-300">No se puede habilitar esta configuración al usar Azure SignalR Service.</span><span class="sxs-lookup"><span data-stu-id="ae513-300">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="ae513-301">Java</span><span class="sxs-lookup"><span data-stu-id="ae513-301">Java</span></span>](#tab/java)
| <span data-ttu-id="ae513-302">Opción de Java</span><span class="sxs-lookup"><span data-stu-id="ae513-302">Java Option</span></span> | <span data-ttu-id="ae513-303">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="ae513-303">Default Value</span></span> | <span data-ttu-id="ae513-304">Descripción</span><span class="sxs-lookup"><span data-stu-id="ae513-304">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="ae513-305">Una función que devuelve una cadena que se proporciona como un token de autenticación de portador en solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="ae513-305">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="ae513-306">Establezca esta opción en `true` para omitir el paso de negociación.</span><span class="sxs-lookup"><span data-stu-id="ae513-306">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="ae513-307">**Solo se admite cuando el transporte de WebSockets es el único tipo de transporte habilitado**.</span><span class="sxs-lookup"><span data-stu-id="ae513-307">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="ae513-308">No se puede habilitar esta configuración al usar Azure SignalR Service.</span><span class="sxs-lookup"><span data-stu-id="ae513-308">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="ae513-309">`withHeader` `withHeaders`</span><span class="sxs-lookup"><span data-stu-id="ae513-309">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="ae513-310">Empty</span><span class="sxs-lookup"><span data-stu-id="ae513-310">Empty</span></span> | <span data-ttu-id="ae513-311">Una asignación de encabezados HTTP adicionales que se enviará con todas las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="ae513-311">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="ae513-312">En el cliente. NET, estas opciones se pueden modificar por el delegado de las opciones proporcionado para `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="ae513-312">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="ae513-313">En el cliente de JavaScript, estas opciones se pueden proporcionar en un objeto de JavaScript proporcionado a `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="ae513-313">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="ae513-314">En el cliente de Java, estas opciones pueden configurarse con los métodos en el `HttpHubConnectionBuilder` devuelto desde el `HubConnectionBuilder.create("HUB URL")`</span><span class="sxs-lookup"><span data-stu-id="ae513-314">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>


```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="ae513-315">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="ae513-315">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
