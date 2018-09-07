---
title: Configuración de ASP.NET Core SignalR
author: tdykstra
description: Obtenga información sobre cómo configurar aplicaciones de ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/06/2018
uid: signalr/configuration
ms.openlocfilehash: fee6e3382c14e818dff408f95770e711603f769d
ms.sourcegitcommit: 08bf41d4b3e696ab512b044970e8304816f8cc56
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/06/2018
ms.locfileid: "44039996"
---
# <a name="aspnet-core-signalr-configuration"></a><span data-ttu-id="f6022-103">Configuración de ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="f6022-103">ASP.NET Core SignalR configuration</span></span>

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="f6022-104">Opciones de serialización de JSON/MessagePack</span><span class="sxs-lookup"><span data-stu-id="f6022-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="f6022-105">ASP.NET Core SignalR admite dos protocolos para la codificación de mensajes: [JSON](https://www.json.org/) y [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="f6022-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="f6022-106">Cada protocolo tiene opciones de configuración de serialización.</span><span class="sxs-lookup"><span data-stu-id="f6022-106">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="f6022-107">Serialización de JSON se puede configurar en el servidor mediante el [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) método de extensión, que se puede agregar después [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) en su `Startup.ConfigureServices` método.</span><span class="sxs-lookup"><span data-stu-id="f6022-107">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="f6022-108">El `AddJsonProtocol` método toma un delegado que recibe un `options` objeto.</span><span class="sxs-lookup"><span data-stu-id="f6022-108">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="f6022-109">El [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) propiedad en ese objeto es un JSON.NET `JsonSerializerSettings` objeto que puede usarse para configurar la serialización de argumentos y valores devueltos.</span><span class="sxs-lookup"><span data-stu-id="f6022-109">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="f6022-110">Consulte la [documentación JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm) para obtener más detalles.</span><span class="sxs-lookup"><span data-stu-id="f6022-110">See the [JSON.NET Documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm) for more details.</span></span>

<span data-ttu-id="f6022-111">Por ejemplo, para configurar el serializador para usar nombres de propiedad "PascalCase", en lugar de los nombres predeterminados "camelCase", use el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="f6022-111">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code:</span></span>

```csharp
services.AddSignalR()
    .AddJsonHubProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver = 
        new DefaultContractResolver();
    });
```

<span data-ttu-id="f6022-112">En el cliente. NET, la misma `AddJsonHubProtocol` existe en el método de extensión [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="f6022-112">In the .NET client, the same `AddJsonHubProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="f6022-113">El `Microsoft.Extensions.DependencyInjection` se debe importar el espacio de nombres para resolver el método de extensión:</span><span class="sxs-lookup"><span data-stu-id="f6022-113">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

```csharp
// At the top of the file:
using Microsoft.Extensions.DependencyInjection;

// When constructing your connection:
var connection = new HubConnectionBuilder()
.AddJsonHubProtocol(options => {
    options.PayloadSerializerSettings.ContractResolver = 
        new DefaultContractResolver();
});
```

> [!NOTE]
> <span data-ttu-id="f6022-114">No es posible configurar la serialización de JSON en el cliente de JavaScript en este momento.</span><span class="sxs-lookup"><span data-stu-id="f6022-114">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="f6022-115">Opciones de serialización MessagePack</span><span class="sxs-lookup"><span data-stu-id="f6022-115">MessagePack serialization options</span></span>

<span data-ttu-id="f6022-116">MessagePack serialización se puede configurar si se proporciona un delegado para el [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) llamar.</span><span class="sxs-lookup"><span data-stu-id="f6022-116">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="f6022-117">Consulte [MessagePack en SignalR](xref:signalr/messagepackhubprotocol) para obtener más detalles.</span><span class="sxs-lookup"><span data-stu-id="f6022-117">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="f6022-118">No es posible configurar la serialización de MessagePack en el cliente de JavaScript en este momento.</span><span class="sxs-lookup"><span data-stu-id="f6022-118">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="f6022-119">Configurar opciones de servidor</span><span class="sxs-lookup"><span data-stu-id="f6022-119">Configure server options</span></span>

<span data-ttu-id="f6022-120">En la tabla siguiente se describe las opciones para configurar los concentradores de SignalR:</span><span class="sxs-lookup"><span data-stu-id="f6022-120">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="f6022-121">Opción</span><span class="sxs-lookup"><span data-stu-id="f6022-121">Option</span></span> | <span data-ttu-id="f6022-122">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="f6022-122">Default Value</span></span> | <span data-ttu-id="f6022-123">Descripción</span><span class="sxs-lookup"><span data-stu-id="f6022-123">Description</span></span> |
| ------ | ------------- | ----------- |
| `HandshakeTimeout` | <span data-ttu-id="f6022-124">15 segundos</span><span class="sxs-lookup"><span data-stu-id="f6022-124">15 seconds</span></span> | <span data-ttu-id="f6022-125">Si el cliente no envía un mensaje de protocolo de enlace inicial dentro de este intervalo de tiempo, se cierra la conexión.</span><span class="sxs-lookup"><span data-stu-id="f6022-125">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="f6022-126">Se trata de una opción avanzada que sólo debería modificarse si se producen errores de tiempo de espera del protocolo de enlace debido a la latencia de red graves.</span><span class="sxs-lookup"><span data-stu-id="f6022-126">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="f6022-127">Para obtener más detalles sobre el proceso de negociación, consulte el [especificación del protocolo SignalR Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="f6022-127">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="f6022-128">15 segundos</span><span class="sxs-lookup"><span data-stu-id="f6022-128">15 seconds</span></span> | <span data-ttu-id="f6022-129">Si el servidor no ha enviado un mensaje dentro de este intervalo, se envía automáticamente un mensaje ping para mantener abierta la conexión.</span><span class="sxs-lookup"><span data-stu-id="f6022-129">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="f6022-130">Al cambiar `KeepAliveInterval`, cambie el `ServerTimeout` / `serverTimeoutInMilliseconds` configuración del cliente.</span><span class="sxs-lookup"><span data-stu-id="f6022-130">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="f6022-131">Recomendado `ServerTimeout` / `serverTimeoutInMilliseconds` valor es doble el `KeepAliveInterval` valor.</span><span class="sxs-lookup"><span data-stu-id="f6022-131">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="f6022-132">Todos los protocolos instalados</span><span class="sxs-lookup"><span data-stu-id="f6022-132">All installed protocols</span></span> | <span data-ttu-id="f6022-133">Protocolos admitidos por este concentrador.</span><span class="sxs-lookup"><span data-stu-id="f6022-133">Protocols supported by this hub.</span></span> <span data-ttu-id="f6022-134">De forma predeterminada, se permiten todos los protocolos registrados en el servidor, pero se pueden quitar protocolos de esta lista para deshabilitar los protocolos específicos para los concentradores individuales.</span><span class="sxs-lookup"><span data-stu-id="f6022-134">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="f6022-135">Si `true`detallados se devuelven los mensajes de excepción a los clientes cuando se produce una excepción en un método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="f6022-135">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="f6022-136">El valor predeterminado es `false`, ya que estos mensajes de excepción pueden contener información confidencial.</span><span class="sxs-lookup"><span data-stu-id="f6022-136">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

<span data-ttu-id="f6022-137">Se pueden configurar opciones para todos los centros proporcionando un delegado de opciones para la `AddSignalR` llamar `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="f6022-137">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSignalR(hubOptions =>
    {
        hubOptions.EnableDetailedErrors = true;
        hubOptions.KeepAliveInterval = TimeSpan.FromMinutes(1);
    })
}
```

<span data-ttu-id="f6022-138">Opciones para un único centro de invalidan las opciones globales de `AddSignalR` y pueden configurarse mediante [AddHubOptions\<T >](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):</span><span class="sxs-lookup"><span data-stu-id="f6022-138">Options for a single hub override the global options provided in `AddSignalR` and can be configured using [AddHubOptions\<T>](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
}
```

<span data-ttu-id="f6022-139">Use `HttpConnectionDispatcherOptions` para configurar opciones avanzadas relacionadas con transportes y administración de búfer de memoria.</span><span class="sxs-lookup"><span data-stu-id="f6022-139">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="f6022-140">Estas opciones se configuran pasando un delegado para [MapHub\<T >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub).</span><span class="sxs-lookup"><span data-stu-id="f6022-140">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub).</span></span>

| <span data-ttu-id="f6022-141">Opción</span><span class="sxs-lookup"><span data-stu-id="f6022-141">Option</span></span> | <span data-ttu-id="f6022-142">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="f6022-142">Default Value</span></span> | <span data-ttu-id="f6022-143">Descripción</span><span class="sxs-lookup"><span data-stu-id="f6022-143">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="f6022-144">32 KB</span><span class="sxs-lookup"><span data-stu-id="f6022-144">32 KB</span></span> | <span data-ttu-id="f6022-145">El número máximo de bytes recibidos del cliente que los búferes del servidor.</span><span class="sxs-lookup"><span data-stu-id="f6022-145">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="f6022-146">Al aumentar este valor permite que el servidor recibir los mensajes más grandes, pero puede repercutir negativamente en el consumo de memoria.</span><span class="sxs-lookup"><span data-stu-id="f6022-146">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="f6022-147">Recopilar automáticamente los datos de la `Authorize` atributos aplicados a la clase de concentrador.</span><span class="sxs-lookup"><span data-stu-id="f6022-147">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="f6022-148">Una lista de [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) los objetos utilizados para determinar si un cliente está autorizado para conectarse al concentrador.</span><span class="sxs-lookup"><span data-stu-id="f6022-148">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="f6022-149">32 KB</span><span class="sxs-lookup"><span data-stu-id="f6022-149">32 KB</span></span> | <span data-ttu-id="f6022-150">El número máximo de bytes enviados por la aplicación que los búferes del servidor.</span><span class="sxs-lookup"><span data-stu-id="f6022-150">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="f6022-151">Al aumentar este valor permite que el servidor enviar los mensajes más grandes, pero puede repercutir negativamente en el consumo de memoria.</span><span class="sxs-lookup"><span data-stu-id="f6022-151">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="f6022-152">Se habilitan todos los transportes.</span><span class="sxs-lookup"><span data-stu-id="f6022-152">All Transports are enabled.</span></span> | <span data-ttu-id="f6022-153">Una máscara de bits de `HttpTransportType` valores que pueden restringir los transportes que un cliente puede usar para conectarse.</span><span class="sxs-lookup"><span data-stu-id="f6022-153">A bitmask of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="f6022-154">Véalo a continuación.</span><span class="sxs-lookup"><span data-stu-id="f6022-154">See below.</span></span> | <span data-ttu-id="f6022-155">Opciones adicionales específicas para el transporte de sondeo largo.</span><span class="sxs-lookup"><span data-stu-id="f6022-155">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="f6022-156">Véalo a continuación.</span><span class="sxs-lookup"><span data-stu-id="f6022-156">See below.</span></span> | <span data-ttu-id="f6022-157">Opciones adicionales específicas para el transporte de WebSockets.</span><span class="sxs-lookup"><span data-stu-id="f6022-157">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="f6022-158">El transporte de sondeo largo tiene opciones adicionales que se pueden configurar mediante el `LongPolling` propiedad:</span><span class="sxs-lookup"><span data-stu-id="f6022-158">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="f6022-159">Opción</span><span class="sxs-lookup"><span data-stu-id="f6022-159">Option</span></span> | <span data-ttu-id="f6022-160">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="f6022-160">Default Value</span></span> | <span data-ttu-id="f6022-161">Descripción</span><span class="sxs-lookup"><span data-stu-id="f6022-161">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="f6022-162">90 segundos</span><span class="sxs-lookup"><span data-stu-id="f6022-162">90 seconds</span></span> | <span data-ttu-id="f6022-163">La cantidad máxima de tiempo que el servidor espera para que un mensaje para enviárselo al cliente antes de finalizar una solicitud de sondeo única.</span><span class="sxs-lookup"><span data-stu-id="f6022-163">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="f6022-164">Al disminuir este valor hace que el cliente emitir solicitudes de sondeo de nuevo con más frecuencia.</span><span class="sxs-lookup"><span data-stu-id="f6022-164">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="f6022-165">El transporte de WebSocket tiene opciones adicionales que se pueden configurar mediante el `WebSockets` propiedad:</span><span class="sxs-lookup"><span data-stu-id="f6022-165">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="f6022-166">Opción</span><span class="sxs-lookup"><span data-stu-id="f6022-166">Option</span></span> | <span data-ttu-id="f6022-167">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="f6022-167">Default Value</span></span> | <span data-ttu-id="f6022-168">Descripción</span><span class="sxs-lookup"><span data-stu-id="f6022-168">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="f6022-169">5 segundos</span><span class="sxs-lookup"><span data-stu-id="f6022-169">5 seconds</span></span> | <span data-ttu-id="f6022-170">Después de cerrar el servidor, si se produce un error en el cliente cerrar dentro de este intervalo de tiempo, se termina la conexión.</span><span class="sxs-lookup"><span data-stu-id="f6022-170">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="f6022-171">Un delegado que puede usarse para establecer el `Sec-WebSocket-Protocol` encabezado en un valor personalizado.</span><span class="sxs-lookup"><span data-stu-id="f6022-171">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="f6022-172">El delegado recibe los valores solicitados por el cliente como entrada y se espera que devuelva el valor deseado.</span><span class="sxs-lookup"><span data-stu-id="f6022-172">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="f6022-173">Configurar las opciones de cliente</span><span class="sxs-lookup"><span data-stu-id="f6022-173">Configure client options</span></span>

<span data-ttu-id="f6022-174">Se pueden configurar las opciones de cliente en el `HubConnectionBuilder` tipo (disponible en los clientes de .NET y JavaScript), así como en el `HubConnection` propio.</span><span class="sxs-lookup"><span data-stu-id="f6022-174">Client options can be configured on the `HubConnectionBuilder` type (available in both .NET and JavaScript clients), as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="f6022-175">Configurar el registro</span><span class="sxs-lookup"><span data-stu-id="f6022-175">Configure logging</span></span>

<span data-ttu-id="f6022-176">El registro está configurado en el cliente de .NET mediante el `ConfigureLogging` método.</span><span class="sxs-lookup"><span data-stu-id="f6022-176">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="f6022-177">Registro de proveedores y los filtros se puede registrar en la misma manera, tal como están en el servidor.</span><span class="sxs-lookup"><span data-stu-id="f6022-177">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="f6022-178">Consulte la [registro en ASP.NET Core](xref:fundamentals/logging/index#how-to-add-providers) documentación para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="f6022-178">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index#how-to-add-providers) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="f6022-179">Con el fin de registrar los proveedores de registro, debe instalar los paquetes necesarios.</span><span class="sxs-lookup"><span data-stu-id="f6022-179">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="f6022-180">Consulte la [proveedores de registro integrados](xref:fundamentals/logging/index#built-in-logging-providers) sección de la documentación para obtener una lista completa.</span><span class="sxs-lookup"><span data-stu-id="f6022-180">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="f6022-181">Por ejemplo, para habilitar el registro de consola, instale el `Microsoft.Extensions.Logging.Console` paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="f6022-181">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="f6022-182">Llame a la `AddConsole` método de extensión:</span><span class="sxs-lookup"><span data-stu-id="f6022-182">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="f6022-183">En el cliente de JavaScript, un proceso similar `configureLogging` método existe.</span><span class="sxs-lookup"><span data-stu-id="f6022-183">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="f6022-184">Proporcione un `LogLevel` valor que indica el nivel mínimo de los mensajes de registro para generar.</span><span class="sxs-lookup"><span data-stu-id="f6022-184">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="f6022-185">Los registros se escriben en la ventana de consola del explorador.</span><span class="sxs-lookup"><span data-stu-id="f6022-185">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> <span data-ttu-id="f6022-186">Para deshabilitar el registro por completo, especifique `signalR.LogLevel.None` en el `configureLogging` método.</span><span class="sxs-lookup"><span data-stu-id="f6022-186">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="f6022-187">A continuación, se muestran los niveles de registro disponibles para el cliente de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="f6022-187">Log levels available to the JavaScript client are listed below.</span></span> <span data-ttu-id="f6022-188">Establecer el nivel de registro en uno de estos valores habilita el registro de mensajes en **o superior** ese nivel.</span><span class="sxs-lookup"><span data-stu-id="f6022-188">Setting the log level to one of these values enables logging of messages at **or above** that level.</span></span>

| <span data-ttu-id="f6022-189">Nivel</span><span class="sxs-lookup"><span data-stu-id="f6022-189">Level</span></span> | <span data-ttu-id="f6022-190">Descripción</span><span class="sxs-lookup"><span data-stu-id="f6022-190">Description</span></span> |
| ----- | ----------- |
| `None` | <span data-ttu-id="f6022-191">Se registra ningún mensaje.</span><span class="sxs-lookup"><span data-stu-id="f6022-191">No messages are logged.</span></span> |
| `Critical` | <span data-ttu-id="f6022-192">Mensajes que indican un error en toda la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f6022-192">Messages that indicate a failure in the entire app.</span></span> |
| `Error` | <span data-ttu-id="f6022-193">Mensajes que indican un error en la operación actual.</span><span class="sxs-lookup"><span data-stu-id="f6022-193">Messages that indicate a failure in the current operation.</span></span> |
| `Warning` | <span data-ttu-id="f6022-194">Mensajes que indican un problema grave.</span><span class="sxs-lookup"><span data-stu-id="f6022-194">Messages that indicate a non-fatal problem.</span></span> |
| `Information` | <span data-ttu-id="f6022-195">Mensajes informativos.</span><span class="sxs-lookup"><span data-stu-id="f6022-195">Informational messages.</span></span> |
| `Debug` | <span data-ttu-id="f6022-196">Mensajes de diagnóstico útiles para depurar.</span><span class="sxs-lookup"><span data-stu-id="f6022-196">Diagnostic messages useful for debugging.</span></span> |
| `Trace` | <span data-ttu-id="f6022-197">Mensajes de diagnóstico muy detallados diseñados para diagnosticar problemas específicos.</span><span class="sxs-lookup"><span data-stu-id="f6022-197">Very detailed diagnostic messages designed for diagnosing specific issues.</span></span> |

### <a name="configure-allowed-transports"></a><span data-ttu-id="f6022-198">Configurar transportes permitidos</span><span class="sxs-lookup"><span data-stu-id="f6022-198">Configure allowed transports</span></span>

<span data-ttu-id="f6022-199">Los transportes utilizados por SignalR se pueden configurar en el `WithUrl` llamar (`withUrl` en JavaScript).</span><span class="sxs-lookup"><span data-stu-id="f6022-199">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="f6022-200">Una operación OR bit a bit de los valores de `HttpTransportType` puede usarse para restringir el cliente para que solo use los transportes especificados.</span><span class="sxs-lookup"><span data-stu-id="f6022-200">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="f6022-201">Todos los transportes están habilitados de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="f6022-201">All transports are enabled by default.</span></span>

<span data-ttu-id="f6022-202">Por ejemplo, para deshabilitar el transporte de los eventos, pero permitir conexiones WebSockets y Long Polling:</span><span class="sxs-lookup"><span data-stu-id="f6022-202">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="f6022-203">En el cliente de JavaScript, se configuran los transportes estableciendo el `transport` campo en el objeto de opciones proporcionado para `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="f6022-203">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

### <a name="configure-bearer-authentication"></a><span data-ttu-id="f6022-204">Configurar la autenticación de portador</span><span class="sxs-lookup"><span data-stu-id="f6022-204">Configure bearer authentication</span></span>

<span data-ttu-id="f6022-205">Para proporcionar datos de autenticación junto con las solicitudes de SignalR, use el `AccessTokenProvider` opción (`accessTokenFactory` en JavaScript) para especificar una función que devuelve el token de acceso deseado.</span><span class="sxs-lookup"><span data-stu-id="f6022-205">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="f6022-206">En el cliente. NET, este token de acceso se pasa como un HTTP "Autenticación del portador" token (mediante el `Authorization` encabezado con un tipo de `Bearer`).</span><span class="sxs-lookup"><span data-stu-id="f6022-206">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="f6022-207">En el cliente de JavaScript, se usa el token de acceso como un token de portador, **excepto** en algunos casos donde las API de explorador restringir la capacidad de aplicar los encabezados (específicamente, en las solicitudes de los eventos y WebSockets).</span><span class="sxs-lookup"><span data-stu-id="f6022-207">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="f6022-208">En estos casos, el token de acceso se proporciona como un valor de cadena de consulta `access_token`.</span><span class="sxs-lookup"><span data-stu-id="f6022-208">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="f6022-209">En el cliente. NET, el `AccessTokenProvider` opción puede especificarse utilizando el delegado de opciones en `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="f6022-209">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        }
    })
    .Build();
```

<span data-ttu-id="f6022-210">En el cliente de JavaScript, el token de acceso se configura estableciendo el `accessTokenFactory` campo en el objeto de opciones de `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="f6022-210">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="f6022-211">Configurar el tiempo de espera y las opciones de mantenimiento</span><span class="sxs-lookup"><span data-stu-id="f6022-211">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="f6022-212">Opciones adicionales para configurar el tiempo de espera y el comportamiento de mantenimiento están disponibles en el `HubConnection` propio objeto:</span><span class="sxs-lookup"><span data-stu-id="f6022-212">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

| <span data-ttu-id="f6022-213">Opción de .NET</span><span class="sxs-lookup"><span data-stu-id="f6022-213">.NET Option</span></span> | <span data-ttu-id="f6022-214">Opción de JavaScript</span><span class="sxs-lookup"><span data-stu-id="f6022-214">JavaScript Option</span></span> | <span data-ttu-id="f6022-215">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="f6022-215">Default Value</span></span> | <span data-ttu-id="f6022-216">Descripción</span><span class="sxs-lookup"><span data-stu-id="f6022-216">Description</span></span> |
| ----------- | ----------------- | ------------- | ----------- |
| `ServerTimeout` | `serverTimeoutInMilliseconds` | <span data-ttu-id="f6022-217">30 segundos (30.000 milisegundos)</span><span class="sxs-lookup"><span data-stu-id="f6022-217">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="f6022-218">Tiempo de espera para la actividad del servidor.</span><span class="sxs-lookup"><span data-stu-id="f6022-218">Timeout for server activity.</span></span> <span data-ttu-id="f6022-219">Si el servidor no ha enviado un mensaje en este intervalo, el cliente considera que las ha desconectado el servidor y los desencadenadores la `Closed` eventos (`onclose` en JavaScript).</span><span class="sxs-lookup"><span data-stu-id="f6022-219">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="f6022-220">Este valor debe ser lo suficientemente grande como para un mensaje ping se envíe desde el servidor **y** recibidos por el cliente dentro del intervalo de tiempo de espera.</span><span class="sxs-lookup"><span data-stu-id="f6022-220">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="f6022-221">El valor recomendado es de un número al menos el doble del servidor `KeepAliveInterval` valor, para dejar tiempo para pings de llegada.</span><span class="sxs-lookup"><span data-stu-id="f6022-221">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="f6022-222">No se puede configurar</span><span class="sxs-lookup"><span data-stu-id="f6022-222">Not configurable</span></span> | <span data-ttu-id="f6022-223">15 segundos</span><span class="sxs-lookup"><span data-stu-id="f6022-223">15 seconds</span></span> | <span data-ttu-id="f6022-224">Tiempo de espera para el protocolo de enlace de servidor inicial.</span><span class="sxs-lookup"><span data-stu-id="f6022-224">Timeout for initial server handshake.</span></span> <span data-ttu-id="f6022-225">Si el servidor no envía una respuesta de protocolo de enlace en este intervalo, el cliente cancela el protocolo de enlace y los desencadenadores la `Closed` eventos (`onclose` en JavaScript).</span><span class="sxs-lookup"><span data-stu-id="f6022-225">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="f6022-226">Se trata de una opción avanzada que sólo debería modificarse si se producen errores de tiempo de espera del protocolo de enlace debido a la latencia de red graves.</span><span class="sxs-lookup"><span data-stu-id="f6022-226">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="f6022-227">Para obtener más detalles sobre el proceso de negociación, consulte el [especificación del protocolo SignalR Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="f6022-227">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

<span data-ttu-id="f6022-228">En el cliente. NET, se especifican los valores de tiempo de espera como `TimeSpan` valores.</span><span class="sxs-lookup"><span data-stu-id="f6022-228">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span> <span data-ttu-id="f6022-229">En el cliente de JavaScript, los valores de tiempo de espera se especifican como un número que indica la duración en milisegundos.</span><span class="sxs-lookup"><span data-stu-id="f6022-229">In the JavaScript client, timeout values are specified as a number indicating the duration in milliseconds.</span></span>

### <a name="configure-additional-options"></a><span data-ttu-id="f6022-230">Configurar opciones adicionales</span><span class="sxs-lookup"><span data-stu-id="f6022-230">Configure additional options</span></span>

<span data-ttu-id="f6022-231">Se pueden configurar opciones adicionales en el `WithUrl` (`withUrl` en JavaScript) método `HubConnectionBuilder`:</span><span class="sxs-lookup"><span data-stu-id="f6022-231">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder`:</span></span>

| <span data-ttu-id="f6022-232">Opción de .NET</span><span class="sxs-lookup"><span data-stu-id="f6022-232">.NET Option</span></span> | <span data-ttu-id="f6022-233">Opción de JavaScript</span><span class="sxs-lookup"><span data-stu-id="f6022-233">JavaScript Option</span></span> | <span data-ttu-id="f6022-234">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="f6022-234">Default Value</span></span> | <span data-ttu-id="f6022-235">Descripción</span><span class="sxs-lookup"><span data-stu-id="f6022-235">Description</span></span> |
| ----------- | ----------------- | ------------- | ----------- |
| `AccessTokenProvider` | `accessTokenFactory` | `null` | <span data-ttu-id="f6022-236">Una función que devuelve una cadena que se proporciona como un token de autenticación de portador en solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="f6022-236">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `skipNegotiation` | `false` | <span data-ttu-id="f6022-237">Establezca esta opción en `true` para omitir el paso de negociación.</span><span class="sxs-lookup"><span data-stu-id="f6022-237">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="f6022-238">**Solo se admite cuando el transporte de WebSockets es el único tipo de transporte habilitado**.</span><span class="sxs-lookup"><span data-stu-id="f6022-238">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="f6022-239">No se puede habilitar esta configuración al usar Azure SignalR Service.</span><span class="sxs-lookup"><span data-stu-id="f6022-239">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="f6022-240">No se puede configurar \*</span><span class="sxs-lookup"><span data-stu-id="f6022-240">Not configurable \*</span></span> | <span data-ttu-id="f6022-241">Empty</span><span class="sxs-lookup"><span data-stu-id="f6022-241">Empty</span></span> | <span data-ttu-id="f6022-242">Una colección de certificados TLS que se envían autenticar las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="f6022-242">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="f6022-243">No se puede configurar \*</span><span class="sxs-lookup"><span data-stu-id="f6022-243">Not configurable \*</span></span> | <span data-ttu-id="f6022-244">Empty</span><span class="sxs-lookup"><span data-stu-id="f6022-244">Empty</span></span> | <span data-ttu-id="f6022-245">Una colección de cookies HTTP para enviar con cada solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="f6022-245">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="f6022-246">No se puede configurar \*</span><span class="sxs-lookup"><span data-stu-id="f6022-246">Not configurable \*</span></span> | <span data-ttu-id="f6022-247">Empty</span><span class="sxs-lookup"><span data-stu-id="f6022-247">Empty</span></span> | <span data-ttu-id="f6022-248">Credenciales que se enviará con todas las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="f6022-248">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="f6022-249">No se puede configurar \*</span><span class="sxs-lookup"><span data-stu-id="f6022-249">Not configurable \*</span></span> | <span data-ttu-id="f6022-250">5 segundos</span><span class="sxs-lookup"><span data-stu-id="f6022-250">5 seconds</span></span> | <span data-ttu-id="f6022-251">Solo WebSockets.</span><span class="sxs-lookup"><span data-stu-id="f6022-251">WebSockets only.</span></span> <span data-ttu-id="f6022-252">La cantidad máxima de tiempo de espera a que el cliente después del cierre del servidor confirmar la solicitud de cierre.</span><span class="sxs-lookup"><span data-stu-id="f6022-252">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="f6022-253">Si el servidor no reconoció el cierre dentro de este tiempo, el cliente se desconecta.</span><span class="sxs-lookup"><span data-stu-id="f6022-253">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="f6022-254">No se puede configurar \*</span><span class="sxs-lookup"><span data-stu-id="f6022-254">Not configurable \*</span></span> | <span data-ttu-id="f6022-255">Empty</span><span class="sxs-lookup"><span data-stu-id="f6022-255">Empty</span></span> | <span data-ttu-id="f6022-256">Un diccionario de encabezados HTTP adicionales que se enviará con todas las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="f6022-256">A dictionary of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | <span data-ttu-id="f6022-257">No se puede configurar \*</span><span class="sxs-lookup"><span data-stu-id="f6022-257">Not configurable \*</span></span> | `null` | <span data-ttu-id="f6022-258">Un delegado que puede usarse para configurar o reemplazar el `HttpMessageHandler` utilizado para enviar solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="f6022-258">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="f6022-259">No se utiliza para las conexiones de WebSocket.</span><span class="sxs-lookup"><span data-stu-id="f6022-259">Not used for WebSocket connections.</span></span> <span data-ttu-id="f6022-260">Este delegado debe devolver un valor distinto de null, y recibe el valor predeterminado como un parámetro.</span><span class="sxs-lookup"><span data-stu-id="f6022-260">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="f6022-261">Modificar la configuración en ese valor predeterminado y devolverlo o devolver un nuevo `HttpMessageHandler` instancia.</span><span class="sxs-lookup"><span data-stu-id="f6022-261">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="f6022-262">**Cuando Asegúrese de reemplazar el controlador copiar la configuración que desee impedir que el controlador proporcionado, en caso contrario, las opciones configuradas (por ejemplo, Cookies y encabezados) no se aplicarán al nuevo controlador.**</span><span class="sxs-lookup"><span data-stu-id="f6022-262">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | <span data-ttu-id="f6022-263">No se puede configurar \*</span><span class="sxs-lookup"><span data-stu-id="f6022-263">Not configurable \*</span></span> | `null` | <span data-ttu-id="f6022-264">Un proxy HTTP que se utilizará al enviar solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="f6022-264">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | <span data-ttu-id="f6022-265">No se puede configurar \*</span><span class="sxs-lookup"><span data-stu-id="f6022-265">Not configurable \*</span></span> | `false` | <span data-ttu-id="f6022-266">Establezca este valor booleano para enviar las credenciales predeterminadas para las solicitudes HTTP y WebSockets.</span><span class="sxs-lookup"><span data-stu-id="f6022-266">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="f6022-267">Esto permite el uso de autenticación de Windows.</span><span class="sxs-lookup"><span data-stu-id="f6022-267">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | <span data-ttu-id="f6022-268">No se puede configurar \*</span><span class="sxs-lookup"><span data-stu-id="f6022-268">Not configurable \*</span></span> | `null` | <span data-ttu-id="f6022-269">Un delegado que puede usarse para configurar opciones adicionales de WebSocket.</span><span class="sxs-lookup"><span data-stu-id="f6022-269">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="f6022-270">Recibe una instancia de [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) que puede utilizarse para configurar las opciones.</span><span class="sxs-lookup"><span data-stu-id="f6022-270">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

<span data-ttu-id="f6022-271">Opciones marcadas con un asterisco (\*) no son configurables en el cliente de JavaScript, debido a limitaciones en el Explorador de API.</span><span class="sxs-lookup"><span data-stu-id="f6022-271">Options marked with an asterisk (\*) aren't configurable in the JavaScript client, due to limitations in browser APIs.</span></span>

<span data-ttu-id="f6022-272">En el cliente. NET, estas opciones se pueden modificar por el delegado de las opciones proporcionado para `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="f6022-272">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="f6022-273">En el cliente de JavaScript, estas opciones se pueden proporcionar en un objeto de JavaScript proporcionado a `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="f6022-273">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    });
    .build();
```

## <a name="additional-resources"></a><span data-ttu-id="f6022-274">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="f6022-274">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
