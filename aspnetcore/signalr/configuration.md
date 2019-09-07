---
title: Configuración de ASP.NET Core Signalr
author: bradygaster
description: Aprenda a configurar aplicaciones de ASP.NET Core Signalr.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 08/05/2019
uid: signalr/configuration
ms.openlocfilehash: 156ffac83fbdf61fd88ad8acc307c2c701c46bca
ms.sourcegitcommit: f65d8765e4b7c894481db9b37aa6969abc625a48
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/06/2019
ms.locfileid: "70773929"
---
# <a name="aspnet-core-signalr-configuration"></a><span data-ttu-id="b3a4a-103">Configuración de ASP.NET Core Signalr</span><span class="sxs-lookup"><span data-stu-id="b3a4a-103">ASP.NET Core SignalR configuration</span></span>

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="b3a4a-104">Opciones de serialización de JSON/MessagePack</span><span class="sxs-lookup"><span data-stu-id="b3a4a-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="b3a4a-105">ASP.NET Core Signalr admite dos protocolos para codificar los mensajes: [JSON](https://www.json.org/) y [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="b3a4a-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="b3a4a-106">Cada protocolo tiene opciones de configuración de serialización.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-106">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="b3a4a-107">La serialización de JSON se puede configurar en el servidor mediante el método de extensión [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) , que se puede agregar después `Startup.ConfigureServices` de [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) en el método.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-107">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="b3a4a-108">El `AddJsonProtocol` método toma un delegado que recibe un `options` objeto.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-108">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="b3a4a-109">La propiedad [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) de ese objeto es un objeto `JsonSerializerSettings` JSON.net que se puede usar para configurar la serialización de argumentos y valores devueltos.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-109">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="b3a4a-110">Consulte la [documentación de JSON.net](https://www.newtonsoft.com/json/help/html/Introduction.htm) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-110">See the [JSON.NET Documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm) for more details.</span></span>

<span data-ttu-id="b3a4a-111">Por ejemplo, para configurar el serializador de modo que use nombres de propiedad "PascalCase", en lugar de los nombres predeterminados de "camelCase", use el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="b3a4a-111">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code:</span></span>

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
        new DefaultContractResolver();
    });
```

<span data-ttu-id="b3a4a-112">En el cliente de .net, el `AddJsonProtocol` mismo método de extensión existe en [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="b3a4a-112">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="b3a4a-113">El `Microsoft.Extensions.DependencyInjection` espacio de nombres se debe importar para resolver el método de extensión:</span><span class="sxs-lookup"><span data-stu-id="b3a4a-113">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="b3a4a-114">En este momento, no es posible configurar la serialización de JSON en el cliente de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-114">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="b3a4a-115">Opciones de serialización de MessagePack</span><span class="sxs-lookup"><span data-stu-id="b3a4a-115">MessagePack serialization options</span></span>

<span data-ttu-id="b3a4a-116">La serialización de MessagePack se puede configurar proporcionando un delegado a la llamada a [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) .</span><span class="sxs-lookup"><span data-stu-id="b3a4a-116">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="b3a4a-117">Consulte [MessagePack en signalr](xref:signalr/messagepackhubprotocol) para obtener más detalles.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-117">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="b3a4a-118">En este momento, no es posible configurar la serialización de MessagePack en el cliente de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-118">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="b3a4a-119">Configurar opciones de servidor</span><span class="sxs-lookup"><span data-stu-id="b3a4a-119">Configure server options</span></span>

<span data-ttu-id="b3a4a-120">En la tabla siguiente se describen las opciones para configurar los concentradores de Signalr:</span><span class="sxs-lookup"><span data-stu-id="b3a4a-120">The following table describes options for configuring SignalR hubs:</span></span>

::: moniker range=">= aspnetcore-3.0"

| <span data-ttu-id="b3a4a-121">Opción</span><span class="sxs-lookup"><span data-stu-id="b3a4a-121">Option</span></span> | <span data-ttu-id="b3a4a-122">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="b3a4a-122">Default Value</span></span> | <span data-ttu-id="b3a4a-123">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="b3a4a-123">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="b3a4a-124">30 segundos</span><span class="sxs-lookup"><span data-stu-id="b3a4a-124">30 seconds</span></span> | <span data-ttu-id="b3a4a-125">El servidor considerará que el cliente se ha desconectado Si no ha recibido un mensaje (incluido Keep-Alive) en este intervalo.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-125">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="b3a4a-126">Podría tardar más de este intervalo de tiempo de espera para que el cliente se marque realmente como desconectado, debido a la manera en que se implementa.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-126">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="b3a4a-127">El valor recomendado es el doble `KeepAliveInterval` del valor.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-127">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="b3a4a-128">15 segundos</span><span class="sxs-lookup"><span data-stu-id="b3a4a-128">15 seconds</span></span> | <span data-ttu-id="b3a4a-129">Si el cliente no envía un mensaje de enlace inicial dentro de este intervalo de tiempo, la conexión se cierra.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-129">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="b3a4a-130">Se trata de una configuración avanzada que solo se debe modificar si se producen errores de tiempo de espera del Protocolo de enlace debido a una latencia de red grave.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-130">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="b3a4a-131">Para más información sobre el proceso de enlace, consulte la [especificación del protocolo signalr Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="b3a4a-131">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="b3a4a-132">15 segundos</span><span class="sxs-lookup"><span data-stu-id="b3a4a-132">15 seconds</span></span> | <span data-ttu-id="b3a4a-133">Si el servidor no ha enviado un mensaje dentro de este intervalo, se envía automáticamente un mensaje ping para mantener abierta la conexión.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-133">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="b3a4a-134">Al cambiar `KeepAliveInterval`, cambie la `ServerTimeout` /configuraciónenel cliente`serverTimeoutInMilliseconds` .</span><span class="sxs-lookup"><span data-stu-id="b3a4a-134">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="b3a4a-135">El valor `ServerTimeout` recomendado / `KeepAliveInterval` es eldobledelvalor.`serverTimeoutInMilliseconds`</span><span class="sxs-lookup"><span data-stu-id="b3a4a-135">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="b3a4a-136">Todos los protocolos instalados</span><span class="sxs-lookup"><span data-stu-id="b3a4a-136">All installed protocols</span></span> | <span data-ttu-id="b3a4a-137">Protocolos admitidos por este centro.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-137">Protocols supported by this hub.</span></span> <span data-ttu-id="b3a4a-138">De forma predeterminada, todos los protocolos registrados en el servidor están permitidos, pero los protocolos se pueden quitar de esta lista para deshabilitar determinados protocolos para centros individuales.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-138">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="b3a4a-139">Si `true`es, los mensajes de excepción detallados se devuelven a los clientes cuando se produce una excepción en un método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-139">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="b3a4a-140">El valor predeterminado `false`es, ya que estos mensajes de excepción pueden contener información confidencial.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-140">The default is `false`, as these exception messages can contain sensitive information.</span></span> |
| `StreamBufferCapacity` | `10` | <span data-ttu-id="b3a4a-141">Número máximo de elementos que se pueden almacenar en búfer para las secuencias de carga de cliente.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-141">The maximum number of items that can be buffered for client upload streams.</span></span> <span data-ttu-id="b3a4a-142">Si se alcanza este límite, el procesamiento de las invocaciones se bloquea hasta que el servidor procesa los elementos de la secuencia.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-142">If this limit is reached, the processing of invocations is blocked until the server processes stream items.</span></span>|
| `MaximumReceiveMessageSize` | <span data-ttu-id="b3a4a-143">32 KB</span><span class="sxs-lookup"><span data-stu-id="b3a4a-143">32 KB</span></span> | <span data-ttu-id="b3a4a-144">Tamaño máximo de un único mensaje de concentrador entrante.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-144">Maximum size of a single incoming hub message.</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.2"

| <span data-ttu-id="b3a4a-145">Opción</span><span class="sxs-lookup"><span data-stu-id="b3a4a-145">Option</span></span> | <span data-ttu-id="b3a4a-146">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="b3a4a-146">Default Value</span></span> | <span data-ttu-id="b3a4a-147">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="b3a4a-147">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="b3a4a-148">30 segundos</span><span class="sxs-lookup"><span data-stu-id="b3a4a-148">30 seconds</span></span> | <span data-ttu-id="b3a4a-149">El servidor considerará que el cliente se ha desconectado Si no ha recibido un mensaje (incluido Keep-Alive) en este intervalo.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-149">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="b3a4a-150">Podría tardar más de este intervalo de tiempo de espera para que el cliente se marque realmente como desconectado, debido a la manera en que se implementa.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-150">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="b3a4a-151">El valor recomendado es el doble `KeepAliveInterval` del valor.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-151">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="b3a4a-152">15 segundos</span><span class="sxs-lookup"><span data-stu-id="b3a4a-152">15 seconds</span></span> | <span data-ttu-id="b3a4a-153">Si el cliente no envía un mensaje de enlace inicial dentro de este intervalo de tiempo, la conexión se cierra.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-153">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="b3a4a-154">Se trata de una configuración avanzada que solo se debe modificar si se producen errores de tiempo de espera del Protocolo de enlace debido a una latencia de red grave.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-154">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="b3a4a-155">Para más información sobre el proceso de enlace, consulte la [especificación del protocolo signalr Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="b3a4a-155">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="b3a4a-156">15 segundos</span><span class="sxs-lookup"><span data-stu-id="b3a4a-156">15 seconds</span></span> | <span data-ttu-id="b3a4a-157">Si el servidor no ha enviado un mensaje dentro de este intervalo, se envía automáticamente un mensaje ping para mantener abierta la conexión.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-157">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="b3a4a-158">Al cambiar `KeepAliveInterval`, cambie la `ServerTimeout` /configuraciónenel cliente`serverTimeoutInMilliseconds` .</span><span class="sxs-lookup"><span data-stu-id="b3a4a-158">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="b3a4a-159">El valor `ServerTimeout` recomendado / `KeepAliveInterval` es eldobledelvalor.`serverTimeoutInMilliseconds`</span><span class="sxs-lookup"><span data-stu-id="b3a4a-159">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="b3a4a-160">Todos los protocolos instalados</span><span class="sxs-lookup"><span data-stu-id="b3a4a-160">All installed protocols</span></span> | <span data-ttu-id="b3a4a-161">Protocolos admitidos por este centro.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-161">Protocols supported by this hub.</span></span> <span data-ttu-id="b3a4a-162">De forma predeterminada, todos los protocolos registrados en el servidor están permitidos, pero los protocolos se pueden quitar de esta lista para deshabilitar determinados protocolos para centros individuales.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-162">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="b3a4a-163">Si `true`es, los mensajes de excepción detallados se devuelven a los clientes cuando se produce una excepción en un método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-163">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="b3a4a-164">El valor predeterminado `false`es, ya que estos mensajes de excepción pueden contener información confidencial.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-164">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| <span data-ttu-id="b3a4a-165">Opción</span><span class="sxs-lookup"><span data-stu-id="b3a4a-165">Option</span></span> | <span data-ttu-id="b3a4a-166">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="b3a4a-166">Default Value</span></span> | <span data-ttu-id="b3a4a-167">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="b3a4a-167">Description</span></span> |
| ------ | ------------- | ----------- |
| `HandshakeTimeout` | <span data-ttu-id="b3a4a-168">15 segundos</span><span class="sxs-lookup"><span data-stu-id="b3a4a-168">15 seconds</span></span> | <span data-ttu-id="b3a4a-169">Si el cliente no envía un mensaje de enlace inicial dentro de este intervalo de tiempo, la conexión se cierra.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-169">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="b3a4a-170">Se trata de una configuración avanzada que solo se debe modificar si se producen errores de tiempo de espera del Protocolo de enlace debido a una latencia de red grave.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-170">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="b3a4a-171">Para más información sobre el proceso de enlace, consulte la [especificación del protocolo signalr Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="b3a4a-171">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="b3a4a-172">15 segundos</span><span class="sxs-lookup"><span data-stu-id="b3a4a-172">15 seconds</span></span> | <span data-ttu-id="b3a4a-173">Si el servidor no ha enviado un mensaje dentro de este intervalo, se envía automáticamente un mensaje ping para mantener abierta la conexión.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-173">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="b3a4a-174">Al cambiar `KeepAliveInterval`, cambie la `ServerTimeout` /configuraciónenel cliente`serverTimeoutInMilliseconds` .</span><span class="sxs-lookup"><span data-stu-id="b3a4a-174">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="b3a4a-175">El valor `ServerTimeout` recomendado / `KeepAliveInterval` es eldobledelvalor.`serverTimeoutInMilliseconds`</span><span class="sxs-lookup"><span data-stu-id="b3a4a-175">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="b3a4a-176">Todos los protocolos instalados</span><span class="sxs-lookup"><span data-stu-id="b3a4a-176">All installed protocols</span></span> | <span data-ttu-id="b3a4a-177">Protocolos admitidos por este centro.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-177">Protocols supported by this hub.</span></span> <span data-ttu-id="b3a4a-178">De forma predeterminada, todos los protocolos registrados en el servidor están permitidos, pero los protocolos se pueden quitar de esta lista para deshabilitar determinados protocolos para centros individuales.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-178">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="b3a4a-179">Si `true`es, los mensajes de excepción detallados se devuelven a los clientes cuando se produce una excepción en un método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-179">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="b3a4a-180">El valor predeterminado `false`es, ya que estos mensajes de excepción pueden contener información confidencial.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-180">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

::: moniker-end

<span data-ttu-id="b3a4a-181">Las opciones se pueden configurar para todos los concentradores proporcionando un delegado de `AddSignalR` opciones a `Startup.ConfigureServices`la llamada en.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-181">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="b3a4a-182">Las opciones de un solo concentrador invalidan las `AddSignalR` opciones globales proporcionadas en <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>y se pueden configurar mediante:</span><span class="sxs-lookup"><span data-stu-id="b3a4a-182">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="b3a4a-183">Opciones de configuración de HTTP avanzadas</span><span class="sxs-lookup"><span data-stu-id="b3a4a-183">Advanced HTTP configuration options</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="b3a4a-184">Use `HttpConnectionDispatcherOptions` para configurar opciones avanzadas relacionadas con los transportes y la administración del búfer de memoria.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-184">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="b3a4a-185">Estas opciones se configuran pasando un delegado [a\<MapHub T >](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub) en. `Startup.Configure`</span><span class="sxs-lookup"><span data-stu-id="b3a4a-185">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub) in `Startup.Configure`.</span></span>

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
::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="b3a4a-186">Use `HttpConnectionDispatcherOptions` para configurar opciones avanzadas relacionadas con los transportes y la administración del búfer de memoria.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-186">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="b3a4a-187">Estas opciones se configuran pasando un delegado [a\<MapHub T >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) en. `Startup.Configure`</span><span class="sxs-lookup"><span data-stu-id="b3a4a-187">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) in `Startup.Configure`.</span></span>

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

::: moniker-end

<span data-ttu-id="b3a4a-188">En la tabla siguiente se describen las opciones para configurar las opciones de HTTP avanzadas de ASP.NET Core Signalr:</span><span class="sxs-lookup"><span data-stu-id="b3a4a-188">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

::: moniker range=">= aspnetcore-3.0"

| <span data-ttu-id="b3a4a-189">Opción</span><span class="sxs-lookup"><span data-stu-id="b3a4a-189">Option</span></span> | <span data-ttu-id="b3a4a-190">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="b3a4a-190">Default Value</span></span> | <span data-ttu-id="b3a4a-191">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="b3a4a-191">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="b3a4a-192">32 KB</span><span class="sxs-lookup"><span data-stu-id="b3a4a-192">32 KB</span></span> | <span data-ttu-id="b3a4a-193">Número máximo de bytes recibidos del cliente que el servidor almacena en búfer antes de aplicar la presión de reserva.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-193">The maximum number of bytes received from the client that the server buffers before applying backpressure.</span></span> <span data-ttu-id="b3a4a-194">Aumentar este valor permite que el servidor reciba mensajes más grandes con más rapidez sin aplicar la presión, pero puede aumentar el consumo de memoria.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-194">Increasing this value allows the server to receive larger messages more quickly without applying backpressure, but can increase memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="b3a4a-195">Los datos se recopilan `Authorize` automáticamente a partir de los atributos aplicados a la clase hub.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-195">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="b3a4a-196">Una lista de objetos [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) que se usan para determinar si un cliente está autorizado para conectarse al centro.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-196">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="b3a4a-197">32 KB</span><span class="sxs-lookup"><span data-stu-id="b3a4a-197">32 KB</span></span> | <span data-ttu-id="b3a4a-198">Número máximo de bytes enviados por la aplicación que el servidor almacena en búfer antes de observar la contrapresión.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-198">The maximum number of bytes sent by the app that the server buffers before observing backpressure.</span></span> <span data-ttu-id="b3a4a-199">El aumento de este valor permite que el servidor almacene en búfer mensajes más grandes con más rapidez sin esperar la presión, pero puede aumentar el consumo de memoria.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-199">Increasing this value allows the server to buffer larger messages more quickly without awaiting backpressure, but can increase memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="b3a4a-200">Todos los transportes están habilitados.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-200">All Transports are enabled.</span></span> | <span data-ttu-id="b3a4a-201">Una enumeración de marcas `HttpTransportType` de bits que puede restringir los transportes que un cliente puede utilizar para conectarse.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-201">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="b3a4a-202">Véalo a continuación.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-202">See below.</span></span> | <span data-ttu-id="b3a4a-203">Opciones adicionales específicas del transporte de sondeo prolongado.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-203">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="b3a4a-204">Véalo a continuación.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-204">See below.</span></span> | <span data-ttu-id="b3a4a-205">Opciones adicionales específicas del transporte de WebSockets.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-205">Additional options specific to the WebSockets transport.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-3.0"

| <span data-ttu-id="b3a4a-206">Opción</span><span class="sxs-lookup"><span data-stu-id="b3a4a-206">Option</span></span> | <span data-ttu-id="b3a4a-207">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="b3a4a-207">Default Value</span></span> | <span data-ttu-id="b3a4a-208">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="b3a4a-208">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="b3a4a-209">32 KB</span><span class="sxs-lookup"><span data-stu-id="b3a4a-209">32 KB</span></span> | <span data-ttu-id="b3a4a-210">Número máximo de bytes recibidos del cliente que el servidor almacena en búfer.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-210">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="b3a4a-211">Aumentar este valor permite que el servidor reciba mensajes de mayor tamaño, pero puede afectar negativamente al consumo de memoria.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-211">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="b3a4a-212">Los datos se recopilan `Authorize` automáticamente a partir de los atributos aplicados a la clase hub.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-212">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="b3a4a-213">Una lista de objetos [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) que se usan para determinar si un cliente está autorizado para conectarse al centro.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-213">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="b3a4a-214">32 KB</span><span class="sxs-lookup"><span data-stu-id="b3a4a-214">32 KB</span></span> | <span data-ttu-id="b3a4a-215">Número máximo de bytes enviados por la aplicación que el servidor almacena en búfer.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-215">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="b3a4a-216">Al aumentar este valor, el servidor puede enviar mensajes de mayor tamaño, pero puede afectar negativamente al consumo de memoria.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-216">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="b3a4a-217">Todos los transportes están habilitados.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-217">All Transports are enabled.</span></span> | <span data-ttu-id="b3a4a-218">Una enumeración de marcas `HttpTransportType` de bits que puede restringir los transportes que un cliente puede utilizar para conectarse.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-218">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="b3a4a-219">Véalo a continuación.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-219">See below.</span></span> | <span data-ttu-id="b3a4a-220">Opciones adicionales específicas del transporte de sondeo prolongado.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-220">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="b3a4a-221">Véalo a continuación.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-221">See below.</span></span> | <span data-ttu-id="b3a4a-222">Opciones adicionales específicas del transporte de WebSockets.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-222">Additional options specific to the WebSockets transport.</span></span> |

::: moniker-end

<span data-ttu-id="b3a4a-223">El transporte de sondeo largo tiene opciones adicionales que se pueden configurar mediante `LongPolling` la propiedad:</span><span class="sxs-lookup"><span data-stu-id="b3a4a-223">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="b3a4a-224">Opción</span><span class="sxs-lookup"><span data-stu-id="b3a4a-224">Option</span></span> | <span data-ttu-id="b3a4a-225">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="b3a4a-225">Default Value</span></span> | <span data-ttu-id="b3a4a-226">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="b3a4a-226">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="b3a4a-227">90 segundos</span><span class="sxs-lookup"><span data-stu-id="b3a4a-227">90 seconds</span></span> | <span data-ttu-id="b3a4a-228">Cantidad máxima de tiempo que el servidor espera a que se envíe un mensaje al cliente antes de finalizar una única solicitud de sondeo.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-228">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="b3a4a-229">Al disminuir este valor, el cliente emite nuevas solicitudes de sondeo con mayor frecuencia.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-229">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="b3a4a-230">El transporte de WebSocket tiene opciones adicionales que se pueden configurar mediante `WebSockets` la propiedad:</span><span class="sxs-lookup"><span data-stu-id="b3a4a-230">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="b3a4a-231">Opción</span><span class="sxs-lookup"><span data-stu-id="b3a4a-231">Option</span></span> | <span data-ttu-id="b3a4a-232">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="b3a4a-232">Default Value</span></span> | <span data-ttu-id="b3a4a-233">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="b3a4a-233">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="b3a4a-234">5 segundos</span><span class="sxs-lookup"><span data-stu-id="b3a4a-234">5 seconds</span></span> | <span data-ttu-id="b3a4a-235">Una vez que se cierra el servidor, si el cliente no se cierra dentro de este intervalo de tiempo, se finaliza la conexión.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-235">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="b3a4a-236">Delegado que se puede utilizar para establecer el `Sec-WebSocket-Protocol` encabezado en un valor personalizado.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-236">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="b3a4a-237">El delegado recibe los valores solicitados por el cliente como entrada y se espera que devuelva el valor deseado.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-237">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="b3a4a-238">Configurar opciones de cliente</span><span class="sxs-lookup"><span data-stu-id="b3a4a-238">Configure client options</span></span>

<span data-ttu-id="b3a4a-239">Las opciones de cliente se pueden configurar `HubConnectionBuilder` en el tipo (disponible en los clientes de .net y JavaScript).</span><span class="sxs-lookup"><span data-stu-id="b3a4a-239">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="b3a4a-240">También está disponible en el cliente de Java, pero la `HttpHubConnectionBuilder` subclase es lo que contiene las opciones de configuración del generador, así como `HubConnection` en el propio.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-240">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="b3a4a-241">Configurar el registro</span><span class="sxs-lookup"><span data-stu-id="b3a4a-241">Configure logging</span></span>

<span data-ttu-id="b3a4a-242">El registro se configura en el cliente .net mediante `ConfigureLogging` el método.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-242">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="b3a4a-243">Los proveedores de registro y los filtros se pueden registrar de la misma manera que en el servidor.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-243">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="b3a4a-244">Consulte la documentación de [Inicio de sesión ASP.net Core](xref:fundamentals/logging/index) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-244">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="b3a4a-245">Para registrar los proveedores de registro, debe instalar los paquetes necesarios.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-245">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="b3a4a-246">Consulte la sección [proveedores de registro integrados](xref:fundamentals/logging/index#built-in-logging-providers) de los documentos para obtener una lista completa.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-246">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="b3a4a-247">Por ejemplo, para habilitar el registro de la consola `Microsoft.Extensions.Logging.Console` , instale el paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-247">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="b3a4a-248">Llame al `AddConsole` método de extensión:</span><span class="sxs-lookup"><span data-stu-id="b3a4a-248">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="b3a4a-249">En el cliente de JavaScript, existe `configureLogging` un método similar.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-249">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="b3a4a-250">Proporcione un `LogLevel` valor que indique el nivel mínimo de mensajes de registro que se va a producir.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-250">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="b3a4a-251">Los registros se escriben en la ventana de la consola del explorador.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-251">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="b3a4a-252">En lugar de un `LogLevel` valor, también puede proporcionar un `string` valor que represente un nombre de nivel de registro.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-252">Instead of a `LogLevel` value, you can also provide a `string` value representing a log level name.</span></span> <span data-ttu-id="b3a4a-253">Esto resulta útil cuando se configura el registro de signalr en entornos donde no se tiene `LogLevel` acceso a las constantes.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-253">This is useful when configuring SignalR logging in environments where you don't have access to the `LogLevel` constants.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging("warn")
    .build();
```

<span data-ttu-id="b3a4a-254">En la tabla siguiente se enumeran los niveles de registro disponibles.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-254">The following table lists the available log levels.</span></span> <span data-ttu-id="b3a4a-255">El valor que se proporciona `configureLogging` para establecer el nivel de registro **mínimo** que se registrará.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-255">The value you provide to `configureLogging` sets the **minimum** log level that will be logged.</span></span> <span data-ttu-id="b3a4a-256">Se registrarán los mensajes registrados en este nivel, **o los niveles que se enumeran en la tabla**.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-256">Messages logged at this level, **or the levels listed after it in the table**, will be logged.</span></span>

| <span data-ttu-id="b3a4a-257">Cadena</span><span class="sxs-lookup"><span data-stu-id="b3a4a-257">String</span></span>                      | <span data-ttu-id="b3a4a-258">LogLevel</span><span class="sxs-lookup"><span data-stu-id="b3a4a-258">LogLevel</span></span>               |
| --------------------------- | ---------------------- |
| `trace`                     | `LogLevel.Trace`       |
| `debug`                     | `LogLevel.Debug`       |
| <span data-ttu-id="b3a4a-259">`info`**o bien**`information`</span><span class="sxs-lookup"><span data-stu-id="b3a4a-259">`info` **or** `information`</span></span> | `LogLevel.Information` |
| <span data-ttu-id="b3a4a-260">`warn`**o bien**`warning`</span><span class="sxs-lookup"><span data-stu-id="b3a4a-260">`warn` **or** `warning`</span></span>     | `LogLevel.Warning`     |
| `error`                     | `LogLevel.Error`       |
| `critical`                  | `LogLevel.Critical`    |
| `none`                      | `LogLevel.None`        |

::: moniker-end

> [!NOTE]
> <span data-ttu-id="b3a4a-261">Para deshabilitar el registro por `signalR.LogLevel.None` completo, `configureLogging` especifique en el método.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-261">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="b3a4a-262">Para obtener más información sobre el registro, consulte la documentación sobre el [diagnóstico de signalr](xref:signalr/diagnostics).</span><span class="sxs-lookup"><span data-stu-id="b3a4a-262">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="b3a4a-263">El cliente de Signalr Java usa la biblioteca [SLF4J](https://www.slf4j.org/) para el registro.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-263">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="b3a4a-264">Se trata de una API de registro de alto nivel que permite a los usuarios de la biblioteca elegir su propia implementación de registro específica mediante la incorporación de una dependencia de registro específica.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-264">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="b3a4a-265">En el fragmento de código siguiente se muestra `java.util.logging` cómo usar con el cliente de signalr Java.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-265">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="b3a4a-266">Si no configura el registro en las dependencias, SLF4J carga un registrador de no operaciones predeterminado con el siguiente mensaje de ADVERTENCIA:</span><span class="sxs-lookup"><span data-stu-id="b3a4a-266">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="b3a4a-267">Esto se puede omitir sin ningún riesgo.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-267">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="b3a4a-268">Configurar transportes permitidos</span><span class="sxs-lookup"><span data-stu-id="b3a4a-268">Configure allowed transports</span></span>

<span data-ttu-id="b3a4a-269">Los transportes utilizados por signalr se pueden configurar en la `WithUrl` llamada (`withUrl` en JavaScript).</span><span class="sxs-lookup"><span data-stu-id="b3a4a-269">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="b3a4a-270">Una operación OR bit a bit de los `HttpTransportType` valores de se puede utilizar para restringir el cliente de modo que solo utilice los transportes especificados.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-270">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="b3a4a-271">Todos los transportes están habilitados de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-271">All transports are enabled by default.</span></span>

<span data-ttu-id="b3a4a-272">Por ejemplo, para deshabilitar el transporte de eventos enviados por el servidor, pero permitir WebSockets y conexiones de sondeo largas:</span><span class="sxs-lookup"><span data-stu-id="b3a4a-272">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="b3a4a-273">En el cliente de JavaScript, los transportes se configuran estableciendo `transport` el campo en el objeto de opciones `withUrl`proporcionado a:</span><span class="sxs-lookup"><span data-stu-id="b3a4a-273">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="b3a4a-274">En esta versión del cliente de Java, WebSockets es el único transporte disponible.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-274">In this version of the Java client websockets is the only available transport.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-3.0"

<span data-ttu-id="b3a4a-275">En el cliente de Java, el transporte se selecciona con `withTransport` el método `HttpHubConnectionBuilder`en.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-275">In the Java client, the transport is selected with the `withTransport` method on the `HttpHubConnectionBuilder`.</span></span> <span data-ttu-id="b3a4a-276">De forma predeterminada, el cliente de Java usa el transporte de WebSockets.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-276">The Java client defaults to using the WebSockets transport.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withTransport(TransportEnum.WEBSOCKETS)
    .build();
```

> [!NOTE]
> <span data-ttu-id="b3a4a-277">El cliente de Signalr Java no admite todavía la reserva de transporte.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-277">The SignalR Java client doesn't support transport fallback yet.</span></span>

::: moniker-end

### <a name="configure-bearer-authentication"></a><span data-ttu-id="b3a4a-278">Configurar la autenticación de portador</span><span class="sxs-lookup"><span data-stu-id="b3a4a-278">Configure bearer authentication</span></span>

<span data-ttu-id="b3a4a-279">Para proporcionar datos de autenticación junto con solicitudes de signalr, `AccessTokenProvider` use la`accessTokenFactory` opción (en JavaScript) para especificar una función que devuelva el token de acceso deseado.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-279">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="b3a4a-280">En el cliente de .net, este token de acceso se pasa como un token de "autenticación de portador" de http `Authorization` (mediante el encabezado con `Bearer`un tipo de).</span><span class="sxs-lookup"><span data-stu-id="b3a4a-280">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="b3a4a-281">En el cliente de JavaScript, el token de acceso se usa como un token de portador, **excepto** en algunos casos en los que las API del explorador restringen la capacidad de aplicar encabezados (específicamente, en los eventos enviados por el servidor y en las solicitudes de WebSockets).</span><span class="sxs-lookup"><span data-stu-id="b3a4a-281">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="b3a4a-282">En estos casos, el token de acceso se proporciona como un valor `access_token`de cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-282">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="b3a4a-283">En el cliente de .net, `AccessTokenProvider` se puede especificar la opción mediante el delegado de `WithUrl`opciones en:</span><span class="sxs-lookup"><span data-stu-id="b3a4a-283">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="b3a4a-284">En el cliente de JavaScript, el token de acceso se configura estableciendo `accessTokenFactory` el campo en el objeto de `withUrl`opciones en:</span><span class="sxs-lookup"><span data-stu-id="b3a4a-284">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

<span data-ttu-id="b3a4a-285">En el cliente de Signalr Java, puede configurar un token de portador que se usará para la autenticación proporcionando un generador de tokens de acceso a [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span><span class="sxs-lookup"><span data-stu-id="b3a4a-285">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="b3a4a-286">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) para proporcionar un [RxJava](https://github.com/ReactiveX/RxJava) de una [única\<cadena>](https://reactivex.io/documentation/single.html).</span><span class="sxs-lookup"><span data-stu-id="b3a4a-286">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="b3a4a-287">Con una llamada a [Single. defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), puede escribir lógica para generar tokens de acceso para el cliente.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-287">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="b3a4a-288">Configurar las opciones de tiempo de espera y mantenimiento de conexión</span><span class="sxs-lookup"><span data-stu-id="b3a4a-288">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="b3a4a-289">En el propio objeto están disponibles opciones adicionales para configurar el `HubConnection` comportamiento de tiempo de espera y mantenimiento de conexión:</span><span class="sxs-lookup"><span data-stu-id="b3a4a-289">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>


::: moniker range=">= aspnetcore-2.2"

# <a name="nettabdotnet"></a>[<span data-ttu-id="b3a4a-290">.NET</span><span class="sxs-lookup"><span data-stu-id="b3a4a-290">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="b3a4a-291">Opción</span><span class="sxs-lookup"><span data-stu-id="b3a4a-291">Option</span></span> | <span data-ttu-id="b3a4a-292">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="b3a4a-292">Default value</span></span> | <span data-ttu-id="b3a4a-293">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="b3a4a-293">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="b3a4a-294">30 segundos (30.000 milisegundos)</span><span class="sxs-lookup"><span data-stu-id="b3a4a-294">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="b3a4a-295">Tiempo de espera para la actividad del servidor.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-295">Timeout for server activity.</span></span> <span data-ttu-id="b3a4a-296">Si el servidor no ha enviado un mensaje en este intervalo, el cliente considera que el servidor está desconectado y desencadena `Closed` el evento`onclose` (en JavaScript).</span><span class="sxs-lookup"><span data-stu-id="b3a4a-296">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="b3a4a-297">Este valor debe ser lo suficientemente grande como para que se envíe un mensaje ping desde el servidor **y** el cliente lo reciba dentro del intervalo de tiempo de espera.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-297">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="b3a4a-298">El valor recomendado es un número, como mínimo, el doble `KeepAliveInterval` del valor del servidor para dejar tiempo para que lleguen los ping.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-298">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="b3a4a-299">15 segundos</span><span class="sxs-lookup"><span data-stu-id="b3a4a-299">15 seconds</span></span> | <span data-ttu-id="b3a4a-300">Tiempo de espera para el protocolo de enlace inicial del servidor.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-300">Timeout for initial server handshake.</span></span> <span data-ttu-id="b3a4a-301">Si el servidor no envía una respuesta de protocolo de enlace en este intervalo, el cliente cancela el protocolo de enlace y desencadena `Closed` el evento`onclose` (en JavaScript).</span><span class="sxs-lookup"><span data-stu-id="b3a4a-301">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="b3a4a-302">Se trata de una configuración avanzada que solo se debe modificar si se producen errores de tiempo de espera del Protocolo de enlace debido a una latencia de red grave.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-302">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="b3a4a-303">Para más información sobre el proceso de enlace, consulte la [especificación del protocolo signalr Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="b3a4a-303">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="b3a4a-304">15 segundos</span><span class="sxs-lookup"><span data-stu-id="b3a4a-304">15 seconds</span></span> | <span data-ttu-id="b3a4a-305">Determina el intervalo en el que el cliente envía mensajes de ping.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-305">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="b3a4a-306">Al enviar cualquier mensaje desde el cliente, se restablece el temporizador al inicio del intervalo.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-306">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="b3a4a-307">Si el cliente no ha enviado un mensaje en `ClientTimeoutInterval` el conjunto del servidor, el servidor considera que el cliente está desconectado.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-307">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

<span data-ttu-id="b3a4a-308">En el cliente de .net, los valores de tiempo `TimeSpan` de espera se especifican como valores.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-308">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="b3a4a-309">JavaScript</span><span class="sxs-lookup"><span data-stu-id="b3a4a-309">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="b3a4a-310">Opción</span><span class="sxs-lookup"><span data-stu-id="b3a4a-310">Option</span></span> | <span data-ttu-id="b3a4a-311">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="b3a4a-311">Default value</span></span> | <span data-ttu-id="b3a4a-312">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="b3a4a-312">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="b3a4a-313">30 segundos (30.000 milisegundos)</span><span class="sxs-lookup"><span data-stu-id="b3a4a-313">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="b3a4a-314">Tiempo de espera para la actividad del servidor.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-314">Timeout for server activity.</span></span> <span data-ttu-id="b3a4a-315">Si el servidor no ha enviado un mensaje en este intervalo, el cliente considera que el servidor está desconectado y desencadena `onclose` el evento.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-315">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="b3a4a-316">Este valor debe ser lo suficientemente grande como para que se envíe un mensaje ping desde el servidor **y** el cliente lo reciba dentro del intervalo de tiempo de espera.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-316">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="b3a4a-317">El valor recomendado es un número, como mínimo, el doble `KeepAliveInterval` del valor del servidor para dejar tiempo para que lleguen los ping.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-317">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `keepAliveIntervalInMilliseconds` | <span data-ttu-id="b3a4a-318">15 segundos (15.000 milisegundos)</span><span class="sxs-lookup"><span data-stu-id="b3a4a-318">15 seconds (15,000 miliseconds)</span></span> | <span data-ttu-id="b3a4a-319">Determina el intervalo en el que el cliente envía mensajes de ping.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-319">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="b3a4a-320">Al enviar cualquier mensaje desde el cliente, se restablece el temporizador al inicio del intervalo.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-320">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="b3a4a-321">Si el cliente no ha enviado un mensaje en `ClientTimeoutInterval` el conjunto del servidor, el servidor considera que el cliente está desconectado.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-321">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="b3a4a-322">Java</span><span class="sxs-lookup"><span data-stu-id="b3a4a-322">Java</span></span>](#tab/java)

| <span data-ttu-id="b3a4a-323">Opción</span><span class="sxs-lookup"><span data-stu-id="b3a4a-323">Option</span></span> | <span data-ttu-id="b3a4a-324">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="b3a4a-324">Default value</span></span> | <span data-ttu-id="b3a4a-325">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="b3a4a-325">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="b3a4a-326">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="b3a4a-326">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="b3a4a-327">30 segundos (30.000 milisegundos)</span><span class="sxs-lookup"><span data-stu-id="b3a4a-327">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="b3a4a-328">Tiempo de espera para la actividad del servidor.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-328">Timeout for server activity.</span></span> <span data-ttu-id="b3a4a-329">Si el servidor no ha enviado un mensaje en este intervalo, el cliente considera que el servidor está desconectado y desencadena `onClose` el evento.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-329">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="b3a4a-330">Este valor debe ser lo suficientemente grande como para que se envíe un mensaje ping desde el servidor **y** el cliente lo reciba dentro del intervalo de tiempo de espera.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-330">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="b3a4a-331">El valor recomendado es un número, como mínimo, el doble `KeepAliveInterval` del valor del servidor para dejar tiempo para que lleguen los ping.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-331">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="b3a4a-332">15 segundos</span><span class="sxs-lookup"><span data-stu-id="b3a4a-332">15 seconds</span></span> | <span data-ttu-id="b3a4a-333">Tiempo de espera para el protocolo de enlace inicial del servidor.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-333">Timeout for initial server handshake.</span></span> <span data-ttu-id="b3a4a-334">Si el servidor no envía una respuesta de protocolo de enlace en este intervalo, el cliente cancela el protocolo de enlace y desencadena `onClose` el evento.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-334">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="b3a4a-335">Se trata de una configuración avanzada que solo se debe modificar si se producen errores de tiempo de espera del Protocolo de enlace debido a una latencia de red grave.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-335">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="b3a4a-336">Para más información sobre el proceso de enlace, consulte la [especificación del protocolo signalr Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="b3a4a-336">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| <span data-ttu-id="b3a4a-337">`getKeepAliveInterval` / `setKeepAliveInterval`</span><span class="sxs-lookup"><span data-stu-id="b3a4a-337">`getKeepAliveInterval` / `setKeepAliveInterval`</span></span> | <span data-ttu-id="b3a4a-338">15 segundos (15.000 milisegundos)</span><span class="sxs-lookup"><span data-stu-id="b3a4a-338">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="b3a4a-339">Determina el intervalo en el que el cliente envía mensajes de ping.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-339">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="b3a4a-340">Al enviar cualquier mensaje desde el cliente, se restablece el temporizador al inicio del intervalo.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-340">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="b3a4a-341">Si el cliente no ha enviado un mensaje en `ClientTimeoutInterval` el conjunto del servidor, el servidor considera que el cliente está desconectado.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-341">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="nettabdotnet"></a>[<span data-ttu-id="b3a4a-342">.NET</span><span class="sxs-lookup"><span data-stu-id="b3a4a-342">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="b3a4a-343">Opción</span><span class="sxs-lookup"><span data-stu-id="b3a4a-343">Option</span></span> | <span data-ttu-id="b3a4a-344">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="b3a4a-344">Default value</span></span> | <span data-ttu-id="b3a4a-345">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="b3a4a-345">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="b3a4a-346">30 segundos (30.000 milisegundos)</span><span class="sxs-lookup"><span data-stu-id="b3a4a-346">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="b3a4a-347">Tiempo de espera para la actividad del servidor.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-347">Timeout for server activity.</span></span> <span data-ttu-id="b3a4a-348">Si el servidor no ha enviado un mensaje en este intervalo, el cliente considera que el servidor está desconectado y desencadena `Closed` el evento`onclose` (en JavaScript).</span><span class="sxs-lookup"><span data-stu-id="b3a4a-348">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="b3a4a-349">Este valor debe ser lo suficientemente grande como para que se envíe un mensaje ping desde el servidor **y** el cliente lo reciba dentro del intervalo de tiempo de espera.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-349">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="b3a4a-350">El valor recomendado es un número, como mínimo, el doble `KeepAliveInterval` del valor del servidor para dejar tiempo para que lleguen los ping.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-350">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="b3a4a-351">15 segundos</span><span class="sxs-lookup"><span data-stu-id="b3a4a-351">15 seconds</span></span> | <span data-ttu-id="b3a4a-352">Tiempo de espera para el protocolo de enlace inicial del servidor.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-352">Timeout for initial server handshake.</span></span> <span data-ttu-id="b3a4a-353">Si el servidor no envía una respuesta de protocolo de enlace en este intervalo, el cliente cancela el protocolo de enlace y desencadena `Closed` el evento`onclose` (en JavaScript).</span><span class="sxs-lookup"><span data-stu-id="b3a4a-353">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="b3a4a-354">Se trata de una configuración avanzada que solo se debe modificar si se producen errores de tiempo de espera del Protocolo de enlace debido a una latencia de red grave.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-354">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="b3a4a-355">Para más información sobre el proceso de enlace, consulte la [especificación del protocolo signalr Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="b3a4a-355">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

<span data-ttu-id="b3a4a-356">En el cliente de .net, los valores de tiempo `TimeSpan` de espera se especifican como valores.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-356">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="b3a4a-357">JavaScript</span><span class="sxs-lookup"><span data-stu-id="b3a4a-357">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="b3a4a-358">Opción</span><span class="sxs-lookup"><span data-stu-id="b3a4a-358">Option</span></span> | <span data-ttu-id="b3a4a-359">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="b3a4a-359">Default value</span></span> | <span data-ttu-id="b3a4a-360">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="b3a4a-360">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="b3a4a-361">30 segundos (30.000 milisegundos)</span><span class="sxs-lookup"><span data-stu-id="b3a4a-361">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="b3a4a-362">Tiempo de espera para la actividad del servidor.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-362">Timeout for server activity.</span></span> <span data-ttu-id="b3a4a-363">Si el servidor no ha enviado un mensaje en este intervalo, el cliente considera que el servidor está desconectado y desencadena `onclose` el evento.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-363">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="b3a4a-364">Este valor debe ser lo suficientemente grande como para que se envíe un mensaje ping desde el servidor **y** el cliente lo reciba dentro del intervalo de tiempo de espera.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-364">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="b3a4a-365">El valor recomendado es un número, como mínimo, el doble `KeepAliveInterval` del valor del servidor para dejar tiempo para que lleguen los ping.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-365">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="b3a4a-366">Java</span><span class="sxs-lookup"><span data-stu-id="b3a4a-366">Java</span></span>](#tab/java)

| <span data-ttu-id="b3a4a-367">Opción</span><span class="sxs-lookup"><span data-stu-id="b3a4a-367">Option</span></span> | <span data-ttu-id="b3a4a-368">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="b3a4a-368">Default value</span></span> | <span data-ttu-id="b3a4a-369">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="b3a4a-369">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="b3a4a-370">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="b3a4a-370">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="b3a4a-371">30 segundos (30.000 milisegundos)</span><span class="sxs-lookup"><span data-stu-id="b3a4a-371">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="b3a4a-372">Tiempo de espera para la actividad del servidor.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-372">Timeout for server activity.</span></span> <span data-ttu-id="b3a4a-373">Si el servidor no ha enviado un mensaje en este intervalo, el cliente considera que el servidor está desconectado y desencadena `onClose` el evento.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-373">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="b3a4a-374">Este valor debe ser lo suficientemente grande como para que se envíe un mensaje ping desde el servidor **y** el cliente lo reciba dentro del intervalo de tiempo de espera.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-374">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="b3a4a-375">El valor recomendado es un número al menos el doble del valor `KeepAliveInterval` del servidor, para permitir que llegue el tiempo para que lleguen los ping.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-375">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="b3a4a-376">15 segundos</span><span class="sxs-lookup"><span data-stu-id="b3a4a-376">15 seconds</span></span> | <span data-ttu-id="b3a4a-377">Tiempo de espera para el protocolo de enlace inicial del servidor.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-377">Timeout for initial server handshake.</span></span> <span data-ttu-id="b3a4a-378">Si el servidor no envía una respuesta de protocolo de enlace en este intervalo, el cliente cancela el protocolo de enlace y desencadena `onClose` el evento.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-378">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="b3a4a-379">Se trata de una configuración avanzada que solo se debe modificar si se producen errores de tiempo de espera del Protocolo de enlace debido a una latencia de red grave.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-379">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="b3a4a-380">Para más información sobre el proceso de enlace, consulte la [especificación del protocolo signalr Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="b3a4a-380">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

::: moniker-end

---

### <a name="configure-additional-options"></a><span data-ttu-id="b3a4a-381">Configurar opciones adicionales</span><span class="sxs-lookup"><span data-stu-id="b3a4a-381">Configure additional options</span></span>

<span data-ttu-id="b3a4a-382">Se pueden configurar opciones adicionales en el `WithUrl` método`withUrl` (en JavaScript) en `HubConnectionBuilder` `HttpHubConnectionBuilder` o en las diversas API de configuración de en el cliente de Java:</span><span class="sxs-lookup"><span data-stu-id="b3a4a-382">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="nettabdotnet"></a>[<span data-ttu-id="b3a4a-383">.NET</span><span class="sxs-lookup"><span data-stu-id="b3a4a-383">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="b3a4a-384">Opción de .NET</span><span class="sxs-lookup"><span data-stu-id="b3a4a-384">.NET Option</span></span> |  <span data-ttu-id="b3a4a-385">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="b3a4a-385">Default value</span></span> | <span data-ttu-id="b3a4a-386">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="b3a4a-386">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="b3a4a-387">Función que devuelve una cadena que se proporciona como un token de autenticación de portador en solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-387">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="b3a4a-388">Establézcalo en `true` para omitir el paso de negociación.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-388">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="b3a4a-389">**Solo se admite cuando el transporte de WebSockets es el único transporte habilitado**.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-389">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="b3a4a-390">Esta configuración no se puede habilitar cuando se usa el servicio Azure Signalr.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-390">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="b3a4a-391">Empty</span><span class="sxs-lookup"><span data-stu-id="b3a4a-391">Empty</span></span> | <span data-ttu-id="b3a4a-392">Colección de certificados TLS que se enviarán a las solicitudes de autenticación.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-392">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="b3a4a-393">Empty</span><span class="sxs-lookup"><span data-stu-id="b3a4a-393">Empty</span></span> | <span data-ttu-id="b3a4a-394">Colección de cookies HTTP que se enviarán con cada solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-394">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="b3a4a-395">Empty</span><span class="sxs-lookup"><span data-stu-id="b3a4a-395">Empty</span></span> | <span data-ttu-id="b3a4a-396">Credenciales que se van a enviar con cada solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-396">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="b3a4a-397">5 segundos</span><span class="sxs-lookup"><span data-stu-id="b3a4a-397">5 seconds</span></span> | <span data-ttu-id="b3a4a-398">Solo WebSockets.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-398">WebSockets only.</span></span> <span data-ttu-id="b3a4a-399">Cantidad máxima de tiempo que el cliente espera después de cerrarse para que el servidor confirme la solicitud de cierre.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-399">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="b3a4a-400">Si el servidor no reconoce el cierre dentro de este tiempo, el cliente se desconecta.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-400">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="b3a4a-401">Empty</span><span class="sxs-lookup"><span data-stu-id="b3a4a-401">Empty</span></span> | <span data-ttu-id="b3a4a-402">Asignación de encabezados HTTP adicionales que se van a enviar con cada solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-402">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="b3a4a-403">Delegado que se puede utilizar para configurar o reemplazar el `HttpMessageHandler` utilizado para enviar solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-403">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="b3a4a-404">No se usa para las conexiones WebSocket.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-404">Not used for WebSocket connections.</span></span> <span data-ttu-id="b3a4a-405">Este delegado debe devolver un valor distinto de NULL y recibe el valor predeterminado como parámetro.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-405">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="b3a4a-406">Modifique la configuración de ese valor predeterminado y devuelva o devuelva una nueva `HttpMessageHandler` instancia.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-406">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="b3a4a-407">**Al reemplazar el controlador, asegúrese de copiar la configuración que desea conservar del controlador proporcionado; de lo contrario, las opciones configuradas (como cookies y encabezados) no se aplicarán al nuevo controlador.**</span><span class="sxs-lookup"><span data-stu-id="b3a4a-407">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="b3a4a-408">Proxy HTTP que se va a usar al enviar solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-408">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="b3a4a-409">Establezca este valor booleano para enviar las credenciales predeterminadas para las solicitudes HTTP y WebSockets.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-409">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="b3a4a-410">Esto habilita el uso de la autenticación de Windows.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-410">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="b3a4a-411">Delegado que se puede usar para configurar opciones adicionales de WebSocket.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-411">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="b3a4a-412">Recibe una instancia de [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) que se puede usar para configurar las opciones.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-412">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="b3a4a-413">JavaScript</span><span class="sxs-lookup"><span data-stu-id="b3a4a-413">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="b3a4a-414">Opción de JavaScript</span><span class="sxs-lookup"><span data-stu-id="b3a4a-414">JavaScript Option</span></span> | <span data-ttu-id="b3a4a-415">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="b3a4a-415">Default Value</span></span> | <span data-ttu-id="b3a4a-416">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="b3a4a-416">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="b3a4a-417">Función que devuelve una cadena que se proporciona como un token de autenticación de portador en solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-417">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="b3a4a-418">Establézcalo en `true` para omitir el paso de negociación.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-418">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="b3a4a-419">**Solo se admite cuando el transporte de WebSockets es el único transporte habilitado**.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-419">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="b3a4a-420">Esta configuración no se puede habilitar cuando se usa el servicio Azure Signalr.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-420">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="b3a4a-421">Java</span><span class="sxs-lookup"><span data-stu-id="b3a4a-421">Java</span></span>](#tab/java)

| <span data-ttu-id="b3a4a-422">Opción de Java</span><span class="sxs-lookup"><span data-stu-id="b3a4a-422">Java Option</span></span> | <span data-ttu-id="b3a4a-423">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="b3a4a-423">Default Value</span></span> | <span data-ttu-id="b3a4a-424">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="b3a4a-424">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="b3a4a-425">Función que devuelve una cadena que se proporciona como un token de autenticación de portador en solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-425">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="b3a4a-426">Establézcalo en `true` para omitir el paso de negociación.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-426">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="b3a4a-427">**Solo se admite cuando el transporte de WebSockets es el único transporte habilitado**.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-427">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="b3a4a-428">Esta configuración no se puede habilitar cuando se usa el servicio Azure Signalr.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-428">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="b3a4a-429">`withHeader``withHeaders`</span><span class="sxs-lookup"><span data-stu-id="b3a4a-429">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="b3a4a-430">Empty</span><span class="sxs-lookup"><span data-stu-id="b3a4a-430">Empty</span></span> | <span data-ttu-id="b3a4a-431">Asignación de encabezados HTTP adicionales que se van a enviar con cada solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="b3a4a-431">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="b3a4a-432">En el cliente de .NET, estas opciones se pueden modificar mediante el delegado de opciones `WithUrl`proporcionado a:</span><span class="sxs-lookup"><span data-stu-id="b3a4a-432">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="b3a4a-433">En el cliente de JavaScript, estas opciones se pueden proporcionar en un objeto de JavaScript `withUrl`proporcionado a:</span><span class="sxs-lookup"><span data-stu-id="b3a4a-433">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="b3a4a-434">En el cliente de Java, estas opciones se pueden configurar con los métodos `HttpHubConnectionBuilder` del devuelto desde`HubConnectionBuilder.create("HUB URL")`</span><span class="sxs-lookup"><span data-stu-id="b3a4a-434">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="b3a4a-435">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="b3a4a-435">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
