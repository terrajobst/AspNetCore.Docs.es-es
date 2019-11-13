---
title: Configuración de SignalR de ASP.NET Core
author: bradygaster
description: Obtenga información sobre cómo configurar ASP.NET Core aplicaciones de SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/configuration
ms.openlocfilehash: 682cc36216a4dc9b38c87b4f67100ab565a70e5c
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963982"
---
# <a name="aspnet-core-opno-locsignalr-configuration"></a><span data-ttu-id="7245c-103">Configuración de SignalR de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7245c-103">ASP.NET Core SignalR configuration</span></span>

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="7245c-104">Opciones de serialización de JSON/MessagePack</span><span class="sxs-lookup"><span data-stu-id="7245c-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="7245c-105">ASP.NET Core SignalR admite dos protocolos para codificar los mensajes: [JSON](https://www.json.org/) y [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="7245c-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="7245c-106">Cada protocolo tiene opciones de configuración de serialización.</span><span class="sxs-lookup"><span data-stu-id="7245c-106">Each protocol has serialization configuration options.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="7245c-107">La serialización de JSON se puede configurar en el servidor mediante el método de extensión [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) .</span><span class="sxs-lookup"><span data-stu-id="7245c-107">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method.</span></span> <span data-ttu-id="7245c-108">`AddJsonProtocol` se puede agregar después de [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="7245c-108">`AddJsonProtocol` can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="7245c-109">El método `AddJsonProtocol` toma un delegado que recibe un objeto `options`.</span><span class="sxs-lookup"><span data-stu-id="7245c-109">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="7245c-110">La propiedad [PayloadSerializerOptions](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializeroptions) de ese objeto es una `System.Text.Json` <xref:System.Text.Json.JsonSerializerOptions> objeto que se puede usar para configurar la serialización de argumentos y valores devueltos.</span><span class="sxs-lookup"><span data-stu-id="7245c-110">The [PayloadSerializerOptions](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializeroptions) property on that object is a `System.Text.Json` <xref:System.Text.Json.JsonSerializerOptions> object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="7245c-111">Para obtener más información, vea la [documentación de System. Text. JSON](/dotnet/api/system.text.json).</span><span class="sxs-lookup"><span data-stu-id="7245c-111">For more information, see the [System.Text.Json documentation](/dotnet/api/system.text.json).</span></span>

<span data-ttu-id="7245c-112">Por ejemplo, para configurar el serializador para que no cambie el uso de mayúsculas y minúsculas de los nombres de propiedad, en lugar de los nombres de "camelCase" predeterminados, use el código siguiente en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="7245c-112">As an example, to configure the serializer to not change the casing of property names, instead of the default "camelCase" names, use the following code in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerOptions.PropertyNamingPolicy = null
    });
```

<span data-ttu-id="7245c-113">En el cliente de .NET, el mismo método de extensión `AddJsonProtocol` existe en [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="7245c-113">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="7245c-114">El espacio de nombres `Microsoft.Extensions.DependencyInjection` se debe importar para resolver el método de extensión:</span><span class="sxs-lookup"><span data-stu-id="7245c-114">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="7245c-115">En este momento, no es posible configurar la serialización de JSON en el cliente de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="7245c-115">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="switch-to-newtonsoftjson"></a><span data-ttu-id="7245c-116">Cambiar a Newtonsoft. JSON</span><span class="sxs-lookup"><span data-stu-id="7245c-116">Switch to Newtonsoft.Json</span></span>

<span data-ttu-id="7245c-117">Si necesita características de `Newtonsoft.Json` que no se admiten en `System.Text.Json`, consulte [cambiar a Newtonsoft. JSON](xref:migration/22-to-30#switch-to-newtonsoftjson).</span><span class="sxs-lookup"><span data-stu-id="7245c-117">If you need features of `Newtonsoft.Json` that aren't supported in `System.Text.Json`, See [Switch to Newtonsoft.Json](xref:migration/22-to-30#switch-to-newtonsoftjson).</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="7245c-118">La serialización de JSON se puede configurar en el servidor mediante el método de extensión [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) , que se puede agregar después de [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) en el método de `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="7245c-118">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="7245c-119">El método `AddJsonProtocol` toma un delegado que recibe un objeto `options`.</span><span class="sxs-lookup"><span data-stu-id="7245c-119">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="7245c-120">La propiedad [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) de ese objeto es un objeto de `JsonSerializerSettings` de JSON.net que se puede usar para configurar la serialización de argumentos y valores devueltos.</span><span class="sxs-lookup"><span data-stu-id="7245c-120">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="7245c-121">Para obtener más información, vea la [documentación de JSON.net](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span><span class="sxs-lookup"><span data-stu-id="7245c-121">For more information, see the [JSON.NET documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span></span>
 
<span data-ttu-id="7245c-122">Por ejemplo, para configurar el serializador de modo que use nombres de propiedad "PascalCase", en lugar de los nombres predeterminados de "camelCase", use el siguiente código en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="7245c-122">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code in `Startup.ConfigureServices`:</span></span>
 
```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
            new DefaultContractResolver();
    });
```

<span data-ttu-id="7245c-123">En el cliente de .NET, el mismo método de extensión `AddJsonProtocol` existe en [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="7245c-123">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="7245c-124">El espacio de nombres `Microsoft.Extensions.DependencyInjection` se debe importar para resolver el método de extensión:</span><span class="sxs-lookup"><span data-stu-id="7245c-124">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="7245c-125">En este momento, no es posible configurar la serialización de JSON en el cliente de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="7245c-125">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

::: moniker-end

### <a name="messagepack-serialization-options"></a><span data-ttu-id="7245c-126">Opciones de serialización de MessagePack</span><span class="sxs-lookup"><span data-stu-id="7245c-126">MessagePack serialization options</span></span>

<span data-ttu-id="7245c-127">La serialización de MessagePack se puede configurar proporcionando un delegado a la llamada a [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) .</span><span class="sxs-lookup"><span data-stu-id="7245c-127">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="7245c-128">Vea [MessagePack en SignalR](xref:signalr/messagepackhubprotocol) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="7245c-128">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="7245c-129">En este momento, no es posible configurar la serialización de MessagePack en el cliente de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="7245c-129">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="7245c-130">Configurar opciones de servidor</span><span class="sxs-lookup"><span data-stu-id="7245c-130">Configure server options</span></span>

<span data-ttu-id="7245c-131">En la tabla siguiente se describen las opciones para configurar los concentradores de SignalR:</span><span class="sxs-lookup"><span data-stu-id="7245c-131">The following table describes options for configuring SignalR hubs:</span></span>

::: moniker range=">= aspnetcore-3.0"

| <span data-ttu-id="7245c-132">Opción</span><span class="sxs-lookup"><span data-stu-id="7245c-132">Option</span></span> | <span data-ttu-id="7245c-133">Default Value</span><span class="sxs-lookup"><span data-stu-id="7245c-133">Default Value</span></span> | <span data-ttu-id="7245c-134">Descripción</span><span class="sxs-lookup"><span data-stu-id="7245c-134">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="7245c-135">30 segundos</span><span class="sxs-lookup"><span data-stu-id="7245c-135">30 seconds</span></span> | <span data-ttu-id="7245c-136">El servidor considerará que el cliente se ha desconectado Si no ha recibido un mensaje (incluido Keep-Alive) en este intervalo.</span><span class="sxs-lookup"><span data-stu-id="7245c-136">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="7245c-137">Podría tardar más de este intervalo de tiempo de espera para que el cliente se marque realmente como desconectado, debido a la manera en que se implementa.</span><span class="sxs-lookup"><span data-stu-id="7245c-137">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="7245c-138">El valor recomendado es el doble del valor de `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="7245c-138">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="7245c-139">15 segundos</span><span class="sxs-lookup"><span data-stu-id="7245c-139">15 seconds</span></span> | <span data-ttu-id="7245c-140">Si el cliente no envía un mensaje de enlace inicial dentro de este intervalo de tiempo, la conexión se cierra.</span><span class="sxs-lookup"><span data-stu-id="7245c-140">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="7245c-141">Se trata de una configuración avanzada que solo se debe modificar si se producen errores de tiempo de espera del Protocolo de enlace debido a una latencia de red grave.</span><span class="sxs-lookup"><span data-stu-id="7245c-141">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="7245c-142">Para obtener más información sobre el proceso de enlace, consulte la [especificación del Protocolo de concentrador deSignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="7245c-142">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="7245c-143">15 segundos</span><span class="sxs-lookup"><span data-stu-id="7245c-143">15 seconds</span></span> | <span data-ttu-id="7245c-144">Si el servidor no ha enviado un mensaje dentro de este intervalo, se envía automáticamente un mensaje ping para mantener abierta la conexión.</span><span class="sxs-lookup"><span data-stu-id="7245c-144">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="7245c-145">Al cambiar `KeepAliveInterval`, cambie la configuración de `serverTimeoutInMilliseconds` de /de `ServerTimeout`en el cliente.</span><span class="sxs-lookup"><span data-stu-id="7245c-145">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="7245c-146">El `ServerTimeout`recomendado /valor de `serverTimeoutInMilliseconds` es el doble del valor de `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="7245c-146">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="7245c-147">Todos los protocolos instalados</span><span class="sxs-lookup"><span data-stu-id="7245c-147">All installed protocols</span></span> | <span data-ttu-id="7245c-148">Protocolos admitidos por este centro.</span><span class="sxs-lookup"><span data-stu-id="7245c-148">Protocols supported by this hub.</span></span> <span data-ttu-id="7245c-149">De forma predeterminada, todos los protocolos registrados en el servidor están permitidos, pero los protocolos se pueden quitar de esta lista para deshabilitar determinados protocolos para centros individuales.</span><span class="sxs-lookup"><span data-stu-id="7245c-149">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="7245c-150">Si `true`, los mensajes de excepción detallados se devuelven a los clientes cuando se produce una excepción en un método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="7245c-150">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="7245c-151">El valor predeterminado es `false`, ya que estos mensajes de excepción pueden contener información confidencial.</span><span class="sxs-lookup"><span data-stu-id="7245c-151">The default is `false`, as these exception messages can contain sensitive information.</span></span> |
| `StreamBufferCapacity` | `10` | <span data-ttu-id="7245c-152">Número máximo de elementos que se pueden almacenar en búfer para las secuencias de carga de cliente.</span><span class="sxs-lookup"><span data-stu-id="7245c-152">The maximum number of items that can be buffered for client upload streams.</span></span> <span data-ttu-id="7245c-153">Si se alcanza este límite, el procesamiento de las invocaciones se bloquea hasta que el servidor procesa los elementos de la secuencia.</span><span class="sxs-lookup"><span data-stu-id="7245c-153">If this limit is reached, the processing of invocations is blocked until the server processes stream items.</span></span>|
| `MaximumReceiveMessageSize` | <span data-ttu-id="7245c-154">32 KB</span><span class="sxs-lookup"><span data-stu-id="7245c-154">32 KB</span></span> | <span data-ttu-id="7245c-155">Tamaño máximo de un único mensaje de concentrador entrante.</span><span class="sxs-lookup"><span data-stu-id="7245c-155">Maximum size of a single incoming hub message.</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.2"

| <span data-ttu-id="7245c-156">Opción</span><span class="sxs-lookup"><span data-stu-id="7245c-156">Option</span></span> | <span data-ttu-id="7245c-157">Default Value</span><span class="sxs-lookup"><span data-stu-id="7245c-157">Default Value</span></span> | <span data-ttu-id="7245c-158">Descripción</span><span class="sxs-lookup"><span data-stu-id="7245c-158">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="7245c-159">30 segundos</span><span class="sxs-lookup"><span data-stu-id="7245c-159">30 seconds</span></span> | <span data-ttu-id="7245c-160">El servidor considerará que el cliente se ha desconectado Si no ha recibido un mensaje (incluido Keep-Alive) en este intervalo.</span><span class="sxs-lookup"><span data-stu-id="7245c-160">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="7245c-161">Podría tardar más de este intervalo de tiempo de espera para que el cliente se marque realmente como desconectado, debido a la manera en que se implementa.</span><span class="sxs-lookup"><span data-stu-id="7245c-161">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="7245c-162">El valor recomendado es el doble del valor de `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="7245c-162">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="7245c-163">15 segundos</span><span class="sxs-lookup"><span data-stu-id="7245c-163">15 seconds</span></span> | <span data-ttu-id="7245c-164">Si el cliente no envía un mensaje de enlace inicial dentro de este intervalo de tiempo, la conexión se cierra.</span><span class="sxs-lookup"><span data-stu-id="7245c-164">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="7245c-165">Se trata de una configuración avanzada que solo se debe modificar si se producen errores de tiempo de espera del Protocolo de enlace debido a una latencia de red grave.</span><span class="sxs-lookup"><span data-stu-id="7245c-165">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="7245c-166">Para obtener más información sobre el proceso de enlace, consulte la [especificación del Protocolo de concentrador deSignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="7245c-166">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="7245c-167">15 segundos</span><span class="sxs-lookup"><span data-stu-id="7245c-167">15 seconds</span></span> | <span data-ttu-id="7245c-168">Si el servidor no ha enviado un mensaje dentro de este intervalo, se envía automáticamente un mensaje ping para mantener abierta la conexión.</span><span class="sxs-lookup"><span data-stu-id="7245c-168">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="7245c-169">Al cambiar `KeepAliveInterval`, cambie la configuración de `serverTimeoutInMilliseconds` de /de `ServerTimeout`en el cliente.</span><span class="sxs-lookup"><span data-stu-id="7245c-169">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="7245c-170">El `ServerTimeout`recomendado /valor de `serverTimeoutInMilliseconds` es el doble del valor de `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="7245c-170">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="7245c-171">Todos los protocolos instalados</span><span class="sxs-lookup"><span data-stu-id="7245c-171">All installed protocols</span></span> | <span data-ttu-id="7245c-172">Protocolos admitidos por este centro.</span><span class="sxs-lookup"><span data-stu-id="7245c-172">Protocols supported by this hub.</span></span> <span data-ttu-id="7245c-173">De forma predeterminada, todos los protocolos registrados en el servidor están permitidos, pero los protocolos se pueden quitar de esta lista para deshabilitar determinados protocolos para centros individuales.</span><span class="sxs-lookup"><span data-stu-id="7245c-173">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="7245c-174">Si `true`, los mensajes de excepción detallados se devuelven a los clientes cuando se produce una excepción en un método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="7245c-174">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="7245c-175">El valor predeterminado es `false`, ya que estos mensajes de excepción pueden contener información confidencial.</span><span class="sxs-lookup"><span data-stu-id="7245c-175">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| <span data-ttu-id="7245c-176">Opción</span><span class="sxs-lookup"><span data-stu-id="7245c-176">Option</span></span> | <span data-ttu-id="7245c-177">Default Value</span><span class="sxs-lookup"><span data-stu-id="7245c-177">Default Value</span></span> | <span data-ttu-id="7245c-178">Descripción</span><span class="sxs-lookup"><span data-stu-id="7245c-178">Description</span></span> |
| ------ | ------------- | ----------- |
| `HandshakeTimeout` | <span data-ttu-id="7245c-179">15 segundos</span><span class="sxs-lookup"><span data-stu-id="7245c-179">15 seconds</span></span> | <span data-ttu-id="7245c-180">Si el cliente no envía un mensaje de enlace inicial dentro de este intervalo de tiempo, la conexión se cierra.</span><span class="sxs-lookup"><span data-stu-id="7245c-180">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="7245c-181">Se trata de una configuración avanzada que solo se debe modificar si se producen errores de tiempo de espera del Protocolo de enlace debido a una latencia de red grave.</span><span class="sxs-lookup"><span data-stu-id="7245c-181">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="7245c-182">Para obtener más información sobre el proceso de enlace, consulte la [especificación del Protocolo de concentrador deSignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="7245c-182">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="7245c-183">15 segundos</span><span class="sxs-lookup"><span data-stu-id="7245c-183">15 seconds</span></span> | <span data-ttu-id="7245c-184">Si el servidor no ha enviado un mensaje dentro de este intervalo, se envía automáticamente un mensaje ping para mantener abierta la conexión.</span><span class="sxs-lookup"><span data-stu-id="7245c-184">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="7245c-185">Al cambiar `KeepAliveInterval`, cambie la configuración de `serverTimeoutInMilliseconds` de /de `ServerTimeout`en el cliente.</span><span class="sxs-lookup"><span data-stu-id="7245c-185">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="7245c-186">El `ServerTimeout`recomendado /valor de `serverTimeoutInMilliseconds` es el doble del valor de `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="7245c-186">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="7245c-187">Todos los protocolos instalados</span><span class="sxs-lookup"><span data-stu-id="7245c-187">All installed protocols</span></span> | <span data-ttu-id="7245c-188">Protocolos admitidos por este centro.</span><span class="sxs-lookup"><span data-stu-id="7245c-188">Protocols supported by this hub.</span></span> <span data-ttu-id="7245c-189">De forma predeterminada, todos los protocolos registrados en el servidor están permitidos, pero los protocolos se pueden quitar de esta lista para deshabilitar determinados protocolos para centros individuales.</span><span class="sxs-lookup"><span data-stu-id="7245c-189">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="7245c-190">Si `true`, los mensajes de excepción detallados se devuelven a los clientes cuando se produce una excepción en un método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="7245c-190">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="7245c-191">El valor predeterminado es `false`, ya que estos mensajes de excepción pueden contener información confidencial.</span><span class="sxs-lookup"><span data-stu-id="7245c-191">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

::: moniker-end

<span data-ttu-id="7245c-192">Las opciones se pueden configurar para todos los concentradores proporcionando un delegado de opciones a la llamada `AddSignalR` en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="7245c-192">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="7245c-193">Las opciones de un solo concentrador invalidan las opciones globales proporcionadas en `AddSignalR` y se pueden configurar mediante <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span><span class="sxs-lookup"><span data-stu-id="7245c-193">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="7245c-194">Opciones de configuración de HTTP avanzadas</span><span class="sxs-lookup"><span data-stu-id="7245c-194">Advanced HTTP configuration options</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="7245c-195">Use `HttpConnectionDispatcherOptions` para configurar opciones avanzadas relacionadas con los transportes y la administración del búfer de memoria.</span><span class="sxs-lookup"><span data-stu-id="7245c-195">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="7245c-196">Estas opciones se configuran pasando un delegado a [MapHub\<t >](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub) en `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="7245c-196">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="7245c-197">Use `HttpConnectionDispatcherOptions` para configurar opciones avanzadas relacionadas con los transportes y la administración del búfer de memoria.</span><span class="sxs-lookup"><span data-stu-id="7245c-197">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="7245c-198">Estas opciones se configuran pasando un delegado a [MapHub\<t >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) en `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="7245c-198">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="7245c-199">En la tabla siguiente se describen las opciones para configurar las opciones de HTTP avanzadas de ASP.NET Core SignalR:</span><span class="sxs-lookup"><span data-stu-id="7245c-199">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

::: moniker range=">= aspnetcore-3.0"

| <span data-ttu-id="7245c-200">Opción</span><span class="sxs-lookup"><span data-stu-id="7245c-200">Option</span></span> | <span data-ttu-id="7245c-201">Default Value</span><span class="sxs-lookup"><span data-stu-id="7245c-201">Default Value</span></span> | <span data-ttu-id="7245c-202">Descripción</span><span class="sxs-lookup"><span data-stu-id="7245c-202">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="7245c-203">32 KB</span><span class="sxs-lookup"><span data-stu-id="7245c-203">32 KB</span></span> | <span data-ttu-id="7245c-204">Número máximo de bytes recibidos del cliente que el servidor almacena en búfer antes de aplicar la presión de reserva.</span><span class="sxs-lookup"><span data-stu-id="7245c-204">The maximum number of bytes received from the client that the server buffers before applying backpressure.</span></span> <span data-ttu-id="7245c-205">Aumentar este valor permite que el servidor reciba mensajes más grandes con más rapidez sin aplicar la presión, pero puede aumentar el consumo de memoria.</span><span class="sxs-lookup"><span data-stu-id="7245c-205">Increasing this value allows the server to receive larger messages more quickly without applying backpressure, but can increase memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="7245c-206">Los datos se recopilan automáticamente a partir de los atributos `Authorize` aplicados a la clase hub.</span><span class="sxs-lookup"><span data-stu-id="7245c-206">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="7245c-207">Una lista de objetos [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) que se usan para determinar si un cliente está autorizado para conectarse al centro.</span><span class="sxs-lookup"><span data-stu-id="7245c-207">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="7245c-208">32 KB</span><span class="sxs-lookup"><span data-stu-id="7245c-208">32 KB</span></span> | <span data-ttu-id="7245c-209">Número máximo de bytes enviados por la aplicación que el servidor almacena en búfer antes de observar la contrapresión.</span><span class="sxs-lookup"><span data-stu-id="7245c-209">The maximum number of bytes sent by the app that the server buffers before observing backpressure.</span></span> <span data-ttu-id="7245c-210">El aumento de este valor permite que el servidor almacene en búfer mensajes más grandes con más rapidez sin esperar la presión, pero puede aumentar el consumo de memoria.</span><span class="sxs-lookup"><span data-stu-id="7245c-210">Increasing this value allows the server to buffer larger messages more quickly without awaiting backpressure, but can increase memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="7245c-211">Todos los transportes están habilitados.</span><span class="sxs-lookup"><span data-stu-id="7245c-211">All Transports are enabled.</span></span> | <span data-ttu-id="7245c-212">Una enumeración de marcas de bits de `HttpTransportType` valores que pueden restringir los transportes que un cliente puede utilizar para conectarse.</span><span class="sxs-lookup"><span data-stu-id="7245c-212">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="7245c-213">Véalo a continuación.</span><span class="sxs-lookup"><span data-stu-id="7245c-213">See below.</span></span> | <span data-ttu-id="7245c-214">Opciones adicionales específicas del transporte de sondeo prolongado.</span><span class="sxs-lookup"><span data-stu-id="7245c-214">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="7245c-215">Véalo a continuación.</span><span class="sxs-lookup"><span data-stu-id="7245c-215">See below.</span></span> | <span data-ttu-id="7245c-216">Opciones adicionales específicas del transporte de WebSockets.</span><span class="sxs-lookup"><span data-stu-id="7245c-216">Additional options specific to the WebSockets transport.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-3.0"

| <span data-ttu-id="7245c-217">Opción</span><span class="sxs-lookup"><span data-stu-id="7245c-217">Option</span></span> | <span data-ttu-id="7245c-218">Default Value</span><span class="sxs-lookup"><span data-stu-id="7245c-218">Default Value</span></span> | <span data-ttu-id="7245c-219">Descripción</span><span class="sxs-lookup"><span data-stu-id="7245c-219">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="7245c-220">32 KB</span><span class="sxs-lookup"><span data-stu-id="7245c-220">32 KB</span></span> | <span data-ttu-id="7245c-221">Número máximo de bytes recibidos del cliente que el servidor almacena en búfer.</span><span class="sxs-lookup"><span data-stu-id="7245c-221">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="7245c-222">Aumentar este valor permite que el servidor reciba mensajes de mayor tamaño, pero puede afectar negativamente al consumo de memoria.</span><span class="sxs-lookup"><span data-stu-id="7245c-222">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="7245c-223">Los datos se recopilan automáticamente a partir de los atributos `Authorize` aplicados a la clase hub.</span><span class="sxs-lookup"><span data-stu-id="7245c-223">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="7245c-224">Una lista de objetos [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) que se usan para determinar si un cliente está autorizado para conectarse al centro.</span><span class="sxs-lookup"><span data-stu-id="7245c-224">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="7245c-225">32 KB</span><span class="sxs-lookup"><span data-stu-id="7245c-225">32 KB</span></span> | <span data-ttu-id="7245c-226">Número máximo de bytes enviados por la aplicación que el servidor almacena en búfer.</span><span class="sxs-lookup"><span data-stu-id="7245c-226">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="7245c-227">Al aumentar este valor, el servidor puede enviar mensajes de mayor tamaño, pero puede afectar negativamente al consumo de memoria.</span><span class="sxs-lookup"><span data-stu-id="7245c-227">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="7245c-228">Todos los transportes están habilitados.</span><span class="sxs-lookup"><span data-stu-id="7245c-228">All Transports are enabled.</span></span> | <span data-ttu-id="7245c-229">Una enumeración de marcas de bits de `HttpTransportType` valores que pueden restringir los transportes que un cliente puede utilizar para conectarse.</span><span class="sxs-lookup"><span data-stu-id="7245c-229">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="7245c-230">Véalo a continuación.</span><span class="sxs-lookup"><span data-stu-id="7245c-230">See below.</span></span> | <span data-ttu-id="7245c-231">Opciones adicionales específicas del transporte de sondeo prolongado.</span><span class="sxs-lookup"><span data-stu-id="7245c-231">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="7245c-232">Véalo a continuación.</span><span class="sxs-lookup"><span data-stu-id="7245c-232">See below.</span></span> | <span data-ttu-id="7245c-233">Opciones adicionales específicas del transporte de WebSockets.</span><span class="sxs-lookup"><span data-stu-id="7245c-233">Additional options specific to the WebSockets transport.</span></span> |

::: moniker-end

<span data-ttu-id="7245c-234">El transporte de sondeo largo tiene opciones adicionales que se pueden configurar mediante la propiedad `LongPolling`:</span><span class="sxs-lookup"><span data-stu-id="7245c-234">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="7245c-235">Opción</span><span class="sxs-lookup"><span data-stu-id="7245c-235">Option</span></span> | <span data-ttu-id="7245c-236">Default Value</span><span class="sxs-lookup"><span data-stu-id="7245c-236">Default Value</span></span> | <span data-ttu-id="7245c-237">Descripción</span><span class="sxs-lookup"><span data-stu-id="7245c-237">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="7245c-238">90 segundos</span><span class="sxs-lookup"><span data-stu-id="7245c-238">90 seconds</span></span> | <span data-ttu-id="7245c-239">Cantidad máxima de tiempo que el servidor espera a que se envíe un mensaje al cliente antes de finalizar una única solicitud de sondeo.</span><span class="sxs-lookup"><span data-stu-id="7245c-239">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="7245c-240">Al disminuir este valor, el cliente emite nuevas solicitudes de sondeo con mayor frecuencia.</span><span class="sxs-lookup"><span data-stu-id="7245c-240">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="7245c-241">El transporte de WebSocket tiene opciones adicionales que se pueden configurar mediante la propiedad `WebSockets`:</span><span class="sxs-lookup"><span data-stu-id="7245c-241">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="7245c-242">Opción</span><span class="sxs-lookup"><span data-stu-id="7245c-242">Option</span></span> | <span data-ttu-id="7245c-243">Default Value</span><span class="sxs-lookup"><span data-stu-id="7245c-243">Default Value</span></span> | <span data-ttu-id="7245c-244">Descripción</span><span class="sxs-lookup"><span data-stu-id="7245c-244">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="7245c-245">5 segundos</span><span class="sxs-lookup"><span data-stu-id="7245c-245">5 seconds</span></span> | <span data-ttu-id="7245c-246">Una vez que se cierra el servidor, si el cliente no se cierra dentro de este intervalo de tiempo, se finaliza la conexión.</span><span class="sxs-lookup"><span data-stu-id="7245c-246">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="7245c-247">Delegado que se puede utilizar para establecer el encabezado de `Sec-WebSocket-Protocol` en un valor personalizado.</span><span class="sxs-lookup"><span data-stu-id="7245c-247">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="7245c-248">El delegado recibe los valores solicitados por el cliente como entrada y se espera que devuelva el valor deseado.</span><span class="sxs-lookup"><span data-stu-id="7245c-248">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="7245c-249">Configurar opciones de cliente</span><span class="sxs-lookup"><span data-stu-id="7245c-249">Configure client options</span></span>

<span data-ttu-id="7245c-250">Las opciones de cliente se pueden configurar en el tipo de `HubConnectionBuilder` (disponible en los clientes de .NET y JavaScript).</span><span class="sxs-lookup"><span data-stu-id="7245c-250">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="7245c-251">También está disponible en el cliente de Java, pero la subclase `HttpHubConnectionBuilder` es lo que contiene las opciones de configuración del generador, así como en el `HubConnection` mismo.</span><span class="sxs-lookup"><span data-stu-id="7245c-251">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="7245c-252">Configurar el registro</span><span class="sxs-lookup"><span data-stu-id="7245c-252">Configure logging</span></span>

<span data-ttu-id="7245c-253">El registro se configura en el cliente .NET mediante el método `ConfigureLogging`.</span><span class="sxs-lookup"><span data-stu-id="7245c-253">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="7245c-254">Los proveedores de registro y los filtros se pueden registrar de la misma manera que en el servidor.</span><span class="sxs-lookup"><span data-stu-id="7245c-254">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="7245c-255">Consulte la documentación de [Inicio de sesión ASP.net Core](xref:fundamentals/logging/index) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="7245c-255">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="7245c-256">Para registrar los proveedores de registro, debe instalar los paquetes necesarios.</span><span class="sxs-lookup"><span data-stu-id="7245c-256">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="7245c-257">Consulte la sección [proveedores de registro integrados](xref:fundamentals/logging/index#built-in-logging-providers) de los documentos para obtener una lista completa.</span><span class="sxs-lookup"><span data-stu-id="7245c-257">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="7245c-258">Por ejemplo, para habilitar el registro de la consola, instale el paquete NuGet `Microsoft.Extensions.Logging.Console`.</span><span class="sxs-lookup"><span data-stu-id="7245c-258">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="7245c-259">Llame al método de extensión `AddConsole`:</span><span class="sxs-lookup"><span data-stu-id="7245c-259">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="7245c-260">En el cliente de JavaScript, existe un método `configureLogging` similar.</span><span class="sxs-lookup"><span data-stu-id="7245c-260">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="7245c-261">Proporcione un valor `LogLevel` que indique el nivel mínimo de mensajes de registro que se va a producir.</span><span class="sxs-lookup"><span data-stu-id="7245c-261">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="7245c-262">Los registros se escriben en la ventana de la consola del explorador.</span><span class="sxs-lookup"><span data-stu-id="7245c-262">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="7245c-263">En lugar de un valor `LogLevel`, también puede proporcionar un valor `string` que represente un nombre de nivel de registro.</span><span class="sxs-lookup"><span data-stu-id="7245c-263">Instead of a `LogLevel` value, you can also provide a `string` value representing a log level name.</span></span> <span data-ttu-id="7245c-264">Esto resulta útil al configurar el registro de SignalR en entornos en los que no tiene acceso a las constantes de `LogLevel`.</span><span class="sxs-lookup"><span data-stu-id="7245c-264">This is useful when configuring SignalR logging in environments where you don't have access to the `LogLevel` constants.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging("warn")
    .build();
```

<span data-ttu-id="7245c-265">En la tabla siguiente se enumeran los niveles de registro disponibles.</span><span class="sxs-lookup"><span data-stu-id="7245c-265">The following table lists the available log levels.</span></span> <span data-ttu-id="7245c-266">El valor que se proporciona a `configureLogging` establece el nivel de registro **mínimo** que se registrará.</span><span class="sxs-lookup"><span data-stu-id="7245c-266">The value you provide to `configureLogging` sets the **minimum** log level that will be logged.</span></span> <span data-ttu-id="7245c-267">Se registrarán los mensajes registrados en este nivel, **o los niveles que se enumeran en la tabla**.</span><span class="sxs-lookup"><span data-stu-id="7245c-267">Messages logged at this level, **or the levels listed after it in the table**, will be logged.</span></span>

| <span data-ttu-id="7245c-268">String</span><span class="sxs-lookup"><span data-stu-id="7245c-268">String</span></span>                      | <span data-ttu-id="7245c-269">LogLevel</span><span class="sxs-lookup"><span data-stu-id="7245c-269">LogLevel</span></span>               |
| --------------------------- | ---------------------- |
| `trace`                     | `LogLevel.Trace`       |
| `debug`                     | `LogLevel.Debug`       |
| <span data-ttu-id="7245c-270">`info` **o** `information`</span><span class="sxs-lookup"><span data-stu-id="7245c-270">`info` **or** `information`</span></span> | `LogLevel.Information` |
| <span data-ttu-id="7245c-271">`warn` **o** `warning`</span><span class="sxs-lookup"><span data-stu-id="7245c-271">`warn` **or** `warning`</span></span>     | `LogLevel.Warning`     |
| `error`                     | `LogLevel.Error`       |
| `critical`                  | `LogLevel.Critical`    |
| `none`                      | `LogLevel.None`        |

::: moniker-end

> [!NOTE]
> <span data-ttu-id="7245c-272">Para deshabilitar el registro por completo, especifique `signalR.LogLevel.None` en el método `configureLogging`.</span><span class="sxs-lookup"><span data-stu-id="7245c-272">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="7245c-273">Para obtener más información sobre el registro, consulte la [documentación de diagnóstico deSignalR](xref:signalr/diagnostics).</span><span class="sxs-lookup"><span data-stu-id="7245c-273">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="7245c-274">El cliente de Java de SignalR usa la biblioteca [SLF4J](https://www.slf4j.org/) para el registro.</span><span class="sxs-lookup"><span data-stu-id="7245c-274">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="7245c-275">Se trata de una API de registro de alto nivel que permite a los usuarios de la biblioteca elegir su propia implementación de registro específica mediante la incorporación de una dependencia de registro específica.</span><span class="sxs-lookup"><span data-stu-id="7245c-275">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="7245c-276">En el fragmento de código siguiente se muestra cómo usar `java.util.logging` con el cliente de Java SignalR.</span><span class="sxs-lookup"><span data-stu-id="7245c-276">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="7245c-277">Si no configura el registro en las dependencias, SLF4J carga un registrador de no operaciones predeterminado con el siguiente mensaje de ADVERTENCIA:</span><span class="sxs-lookup"><span data-stu-id="7245c-277">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="7245c-278">Esto se puede omitir sin ningún riesgo.</span><span class="sxs-lookup"><span data-stu-id="7245c-278">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="7245c-279">Configurar transportes permitidos</span><span class="sxs-lookup"><span data-stu-id="7245c-279">Configure allowed transports</span></span>

<span data-ttu-id="7245c-280">Los transportes utilizados por SignalR pueden configurarse en la llamada `WithUrl` (`withUrl` en JavaScript).</span><span class="sxs-lookup"><span data-stu-id="7245c-280">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="7245c-281">Una operación OR bit a bit de los valores de `HttpTransportType` se puede utilizar para restringir el cliente de modo que solo utilice los transportes especificados.</span><span class="sxs-lookup"><span data-stu-id="7245c-281">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="7245c-282">Todos los transportes están habilitados de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="7245c-282">All transports are enabled by default.</span></span>

<span data-ttu-id="7245c-283">Por ejemplo, para deshabilitar el transporte de eventos enviados por el servidor, pero permitir WebSockets y conexiones de sondeo largas:</span><span class="sxs-lookup"><span data-stu-id="7245c-283">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="7245c-284">En el cliente de JavaScript, los transportes se configuran estableciendo el campo `transport` en el objeto de opciones proporcionado para `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="7245c-284">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="7245c-285">En esta versión del cliente de Java, WebSockets es el único transporte disponible.</span><span class="sxs-lookup"><span data-stu-id="7245c-285">In this version of the Java client websockets is the only available transport.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-3.0"

<span data-ttu-id="7245c-286">En el cliente de Java, el transporte se selecciona con el método `withTransport` en el `HttpHubConnectionBuilder`.</span><span class="sxs-lookup"><span data-stu-id="7245c-286">In the Java client, the transport is selected with the `withTransport` method on the `HttpHubConnectionBuilder`.</span></span> <span data-ttu-id="7245c-287">De forma predeterminada, el cliente de Java usa el transporte de WebSockets.</span><span class="sxs-lookup"><span data-stu-id="7245c-287">The Java client defaults to using the WebSockets transport.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withTransport(TransportEnum.WEBSOCKETS)
    .build();
```

> [!NOTE]
> <span data-ttu-id="7245c-288">El cliente de Java SignalR no es compatible aún con la reserva de transporte.</span><span class="sxs-lookup"><span data-stu-id="7245c-288">The SignalR Java client doesn't support transport fallback yet.</span></span>

::: moniker-end

### <a name="configure-bearer-authentication"></a><span data-ttu-id="7245c-289">Configurar la autenticación de portador</span><span class="sxs-lookup"><span data-stu-id="7245c-289">Configure bearer authentication</span></span>

<span data-ttu-id="7245c-290">Para proporcionar datos de autenticación junto con solicitudes de SignalR, use la opción `AccessTokenProvider` (`accessTokenFactory` en JavaScript) para especificar una función que devuelva el token de acceso deseado.</span><span class="sxs-lookup"><span data-stu-id="7245c-290">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="7245c-291">En el cliente .NET, este token de acceso se pasa como un token de "autenticación de portador" de HTTP (mediante el encabezado `Authorization` con un tipo de `Bearer`).</span><span class="sxs-lookup"><span data-stu-id="7245c-291">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="7245c-292">En el cliente de JavaScript, el token de acceso se usa como un token de portador, **excepto** en algunos casos en los que las API del explorador restringen la capacidad de aplicar encabezados (específicamente, en los eventos enviados por el servidor y en las solicitudes de WebSockets).</span><span class="sxs-lookup"><span data-stu-id="7245c-292">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="7245c-293">En estos casos, el token de acceso se proporciona como un valor de cadena de consulta `access_token`.</span><span class="sxs-lookup"><span data-stu-id="7245c-293">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="7245c-294">En el cliente .NET, se puede especificar la opción `AccessTokenProvider` mediante el delegado Options en `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="7245c-294">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="7245c-295">En el cliente de JavaScript, el token de acceso se configura estableciendo el campo `accessTokenFactory` del objeto de opciones en `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="7245c-295">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

<span data-ttu-id="7245c-296">En el cliente de SignalR Java, puede configurar un token de portador que se usará para la autenticación proporcionando un generador de tokens de acceso a [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span><span class="sxs-lookup"><span data-stu-id="7245c-296">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="7245c-297">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) para [proporcionar una](https://github.com/ReactiveX/RxJava) [> de cadena de\<única](https://reactivex.io/documentation/single.html).</span><span class="sxs-lookup"><span data-stu-id="7245c-297">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="7245c-298">Con una llamada a [Single. defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), puede escribir lógica para generar tokens de acceso para el cliente.</span><span class="sxs-lookup"><span data-stu-id="7245c-298">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="7245c-299">Configurar las opciones de tiempo de espera y mantenimiento de conexión</span><span class="sxs-lookup"><span data-stu-id="7245c-299">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="7245c-300">Las opciones adicionales para configurar el comportamiento de tiempo de espera y mantenimiento de conexión están disponibles en el propio `HubConnection` objeto:</span><span class="sxs-lookup"><span data-stu-id="7245c-300">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>


::: moniker range=">= aspnetcore-2.2"

# <a name="nettabdotnet"></a>[<span data-ttu-id="7245c-301">.NET</span><span class="sxs-lookup"><span data-stu-id="7245c-301">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="7245c-302">Opción</span><span class="sxs-lookup"><span data-stu-id="7245c-302">Option</span></span> | <span data-ttu-id="7245c-303">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="7245c-303">Default value</span></span> | <span data-ttu-id="7245c-304">Descripción</span><span class="sxs-lookup"><span data-stu-id="7245c-304">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="7245c-305">30 segundos (30.000 milisegundos)</span><span class="sxs-lookup"><span data-stu-id="7245c-305">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="7245c-306">Tiempo de espera para la actividad del servidor.</span><span class="sxs-lookup"><span data-stu-id="7245c-306">Timeout for server activity.</span></span> <span data-ttu-id="7245c-307">Si el servidor no ha enviado un mensaje en este intervalo, el cliente considera que el servidor está desconectado y desencadena el evento `Closed` (`onclose` en JavaScript).</span><span class="sxs-lookup"><span data-stu-id="7245c-307">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="7245c-308">Este valor debe ser lo suficientemente grande como para que se envíe un mensaje ping desde el servidor **y** el cliente lo reciba dentro del intervalo de tiempo de espera.</span><span class="sxs-lookup"><span data-stu-id="7245c-308">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="7245c-309">El valor recomendado es un número al menos el doble del valor del `KeepAliveInterval` del servidor para permitir que lleguen los ping.</span><span class="sxs-lookup"><span data-stu-id="7245c-309">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="7245c-310">15 segundos</span><span class="sxs-lookup"><span data-stu-id="7245c-310">15 seconds</span></span> | <span data-ttu-id="7245c-311">Tiempo de espera para el protocolo de enlace inicial del servidor.</span><span class="sxs-lookup"><span data-stu-id="7245c-311">Timeout for initial server handshake.</span></span> <span data-ttu-id="7245c-312">Si el servidor no envía una respuesta de protocolo de enlace en este intervalo, el cliente cancela el protocolo de enlace y desencadena el evento `Closed` (`onclose` en JavaScript).</span><span class="sxs-lookup"><span data-stu-id="7245c-312">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="7245c-313">Se trata de una configuración avanzada que solo se debe modificar si se producen errores de tiempo de espera del Protocolo de enlace debido a una latencia de red grave.</span><span class="sxs-lookup"><span data-stu-id="7245c-313">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="7245c-314">Para obtener más información sobre el proceso de enlace, consulte la [especificación del Protocolo de concentrador deSignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="7245c-314">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="7245c-315">15 segundos</span><span class="sxs-lookup"><span data-stu-id="7245c-315">15 seconds</span></span> | <span data-ttu-id="7245c-316">Determina el intervalo en el que el cliente envía mensajes de ping.</span><span class="sxs-lookup"><span data-stu-id="7245c-316">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="7245c-317">Al enviar cualquier mensaje desde el cliente, se restablece el temporizador al inicio del intervalo.</span><span class="sxs-lookup"><span data-stu-id="7245c-317">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="7245c-318">Si el cliente no ha enviado un mensaje en el `ClientTimeoutInterval` establecido en el servidor, el servidor considera que el cliente está desconectado.</span><span class="sxs-lookup"><span data-stu-id="7245c-318">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

<span data-ttu-id="7245c-319">En el cliente de .NET, los valores de tiempo de espera se especifican como valores de `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="7245c-319">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="7245c-320">JavaScript</span><span class="sxs-lookup"><span data-stu-id="7245c-320">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="7245c-321">Opción</span><span class="sxs-lookup"><span data-stu-id="7245c-321">Option</span></span> | <span data-ttu-id="7245c-322">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="7245c-322">Default value</span></span> | <span data-ttu-id="7245c-323">Descripción</span><span class="sxs-lookup"><span data-stu-id="7245c-323">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="7245c-324">30 segundos (30.000 milisegundos)</span><span class="sxs-lookup"><span data-stu-id="7245c-324">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="7245c-325">Tiempo de espera para la actividad del servidor.</span><span class="sxs-lookup"><span data-stu-id="7245c-325">Timeout for server activity.</span></span> <span data-ttu-id="7245c-326">Si el servidor no ha enviado un mensaje en este intervalo, el cliente considera que el servidor está desconectado y desencadena el evento `onclose`.</span><span class="sxs-lookup"><span data-stu-id="7245c-326">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="7245c-327">Este valor debe ser lo suficientemente grande como para que se envíe un mensaje ping desde el servidor **y** el cliente lo reciba dentro del intervalo de tiempo de espera.</span><span class="sxs-lookup"><span data-stu-id="7245c-327">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="7245c-328">El valor recomendado es un número al menos el doble del valor del `KeepAliveInterval` del servidor para permitir que lleguen los ping.</span><span class="sxs-lookup"><span data-stu-id="7245c-328">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `keepAliveIntervalInMilliseconds` | <span data-ttu-id="7245c-329">15 segundos (15.000 milisegundos)</span><span class="sxs-lookup"><span data-stu-id="7245c-329">15 seconds (15,000 miliseconds)</span></span> | <span data-ttu-id="7245c-330">Determina el intervalo en el que el cliente envía mensajes de ping.</span><span class="sxs-lookup"><span data-stu-id="7245c-330">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="7245c-331">Al enviar cualquier mensaje desde el cliente, se restablece el temporizador al inicio del intervalo.</span><span class="sxs-lookup"><span data-stu-id="7245c-331">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="7245c-332">Si el cliente no ha enviado un mensaje en el `ClientTimeoutInterval` establecido en el servidor, el servidor considera que el cliente está desconectado.</span><span class="sxs-lookup"><span data-stu-id="7245c-332">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="7245c-333">Java</span><span class="sxs-lookup"><span data-stu-id="7245c-333">Java</span></span>](#tab/java)

| <span data-ttu-id="7245c-334">Opción</span><span class="sxs-lookup"><span data-stu-id="7245c-334">Option</span></span> | <span data-ttu-id="7245c-335">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="7245c-335">Default value</span></span> | <span data-ttu-id="7245c-336">Descripción</span><span class="sxs-lookup"><span data-stu-id="7245c-336">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="7245c-337">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="7245c-337">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="7245c-338">30 segundos (30.000 milisegundos)</span><span class="sxs-lookup"><span data-stu-id="7245c-338">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="7245c-339">Tiempo de espera para la actividad del servidor.</span><span class="sxs-lookup"><span data-stu-id="7245c-339">Timeout for server activity.</span></span> <span data-ttu-id="7245c-340">Si el servidor no ha enviado un mensaje en este intervalo, el cliente considera que el servidor está desconectado y desencadena el evento `onClose`.</span><span class="sxs-lookup"><span data-stu-id="7245c-340">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="7245c-341">Este valor debe ser lo suficientemente grande como para que se envíe un mensaje ping desde el servidor **y** el cliente lo reciba dentro del intervalo de tiempo de espera.</span><span class="sxs-lookup"><span data-stu-id="7245c-341">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="7245c-342">El valor recomendado es un número al menos el doble del valor del `KeepAliveInterval` del servidor para permitir que lleguen los ping.</span><span class="sxs-lookup"><span data-stu-id="7245c-342">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="7245c-343">15 segundos</span><span class="sxs-lookup"><span data-stu-id="7245c-343">15 seconds</span></span> | <span data-ttu-id="7245c-344">Tiempo de espera para el protocolo de enlace inicial del servidor.</span><span class="sxs-lookup"><span data-stu-id="7245c-344">Timeout for initial server handshake.</span></span> <span data-ttu-id="7245c-345">Si el servidor no envía una respuesta de protocolo de enlace en este intervalo, el cliente cancela el protocolo de enlace y desencadena el evento `onClose`.</span><span class="sxs-lookup"><span data-stu-id="7245c-345">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="7245c-346">Se trata de una configuración avanzada que solo se debe modificar si se producen errores de tiempo de espera del Protocolo de enlace debido a una latencia de red grave.</span><span class="sxs-lookup"><span data-stu-id="7245c-346">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="7245c-347">Para obtener más información sobre el proceso de enlace, consulte la [especificación del Protocolo de concentrador deSignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="7245c-347">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| <span data-ttu-id="7245c-348">`getKeepAliveInterval` / `setKeepAliveInterval`</span><span class="sxs-lookup"><span data-stu-id="7245c-348">`getKeepAliveInterval` / `setKeepAliveInterval`</span></span> | <span data-ttu-id="7245c-349">15 segundos (15.000 milisegundos)</span><span class="sxs-lookup"><span data-stu-id="7245c-349">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="7245c-350">Determina el intervalo en el que el cliente envía mensajes de ping.</span><span class="sxs-lookup"><span data-stu-id="7245c-350">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="7245c-351">Al enviar cualquier mensaje desde el cliente, se restablece el temporizador al inicio del intervalo.</span><span class="sxs-lookup"><span data-stu-id="7245c-351">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="7245c-352">Si el cliente no ha enviado un mensaje en el `ClientTimeoutInterval` establecido en el servidor, el servidor considera que el cliente está desconectado.</span><span class="sxs-lookup"><span data-stu-id="7245c-352">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="nettabdotnet"></a>[<span data-ttu-id="7245c-353">.NET</span><span class="sxs-lookup"><span data-stu-id="7245c-353">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="7245c-354">Opción</span><span class="sxs-lookup"><span data-stu-id="7245c-354">Option</span></span> | <span data-ttu-id="7245c-355">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="7245c-355">Default value</span></span> | <span data-ttu-id="7245c-356">Descripción</span><span class="sxs-lookup"><span data-stu-id="7245c-356">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="7245c-357">30 segundos (30.000 milisegundos)</span><span class="sxs-lookup"><span data-stu-id="7245c-357">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="7245c-358">Tiempo de espera para la actividad del servidor.</span><span class="sxs-lookup"><span data-stu-id="7245c-358">Timeout for server activity.</span></span> <span data-ttu-id="7245c-359">Si el servidor no ha enviado un mensaje en este intervalo, el cliente considera que el servidor está desconectado y desencadena el evento `Closed` (`onclose` en JavaScript).</span><span class="sxs-lookup"><span data-stu-id="7245c-359">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="7245c-360">Este valor debe ser lo suficientemente grande como para que se envíe un mensaje ping desde el servidor **y** el cliente lo reciba dentro del intervalo de tiempo de espera.</span><span class="sxs-lookup"><span data-stu-id="7245c-360">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="7245c-361">El valor recomendado es un número al menos el doble del valor del `KeepAliveInterval` del servidor para permitir que lleguen los ping.</span><span class="sxs-lookup"><span data-stu-id="7245c-361">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="7245c-362">15 segundos</span><span class="sxs-lookup"><span data-stu-id="7245c-362">15 seconds</span></span> | <span data-ttu-id="7245c-363">Tiempo de espera para el protocolo de enlace inicial del servidor.</span><span class="sxs-lookup"><span data-stu-id="7245c-363">Timeout for initial server handshake.</span></span> <span data-ttu-id="7245c-364">Si el servidor no envía una respuesta de protocolo de enlace en este intervalo, el cliente cancela el protocolo de enlace y desencadena el evento `Closed` (`onclose` en JavaScript).</span><span class="sxs-lookup"><span data-stu-id="7245c-364">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="7245c-365">Se trata de una configuración avanzada que solo se debe modificar si se producen errores de tiempo de espera del Protocolo de enlace debido a una latencia de red grave.</span><span class="sxs-lookup"><span data-stu-id="7245c-365">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="7245c-366">Para obtener más información sobre el proceso de enlace, consulte la [especificación del Protocolo de concentrador deSignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="7245c-366">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

<span data-ttu-id="7245c-367">En el cliente de .NET, los valores de tiempo de espera se especifican como valores de `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="7245c-367">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="7245c-368">JavaScript</span><span class="sxs-lookup"><span data-stu-id="7245c-368">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="7245c-369">Opción</span><span class="sxs-lookup"><span data-stu-id="7245c-369">Option</span></span> | <span data-ttu-id="7245c-370">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="7245c-370">Default value</span></span> | <span data-ttu-id="7245c-371">Descripción</span><span class="sxs-lookup"><span data-stu-id="7245c-371">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="7245c-372">30 segundos (30.000 milisegundos)</span><span class="sxs-lookup"><span data-stu-id="7245c-372">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="7245c-373">Tiempo de espera para la actividad del servidor.</span><span class="sxs-lookup"><span data-stu-id="7245c-373">Timeout for server activity.</span></span> <span data-ttu-id="7245c-374">Si el servidor no ha enviado un mensaje en este intervalo, el cliente considera que el servidor está desconectado y desencadena el evento `onclose`.</span><span class="sxs-lookup"><span data-stu-id="7245c-374">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="7245c-375">Este valor debe ser lo suficientemente grande como para que se envíe un mensaje ping desde el servidor **y** el cliente lo reciba dentro del intervalo de tiempo de espera.</span><span class="sxs-lookup"><span data-stu-id="7245c-375">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="7245c-376">El valor recomendado es un número al menos el doble del valor del `KeepAliveInterval` del servidor para permitir que lleguen los ping.</span><span class="sxs-lookup"><span data-stu-id="7245c-376">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="7245c-377">Java</span><span class="sxs-lookup"><span data-stu-id="7245c-377">Java</span></span>](#tab/java)

| <span data-ttu-id="7245c-378">Opción</span><span class="sxs-lookup"><span data-stu-id="7245c-378">Option</span></span> | <span data-ttu-id="7245c-379">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="7245c-379">Default value</span></span> | <span data-ttu-id="7245c-380">Descripción</span><span class="sxs-lookup"><span data-stu-id="7245c-380">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="7245c-381">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="7245c-381">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="7245c-382">30 segundos (30.000 milisegundos)</span><span class="sxs-lookup"><span data-stu-id="7245c-382">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="7245c-383">Tiempo de espera para la actividad del servidor.</span><span class="sxs-lookup"><span data-stu-id="7245c-383">Timeout for server activity.</span></span> <span data-ttu-id="7245c-384">Si el servidor no ha enviado un mensaje en este intervalo, el cliente considera que el servidor está desconectado y desencadena el evento `onClose`.</span><span class="sxs-lookup"><span data-stu-id="7245c-384">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="7245c-385">Este valor debe ser lo suficientemente grande como para que se envíe un mensaje ping desde el servidor **y** el cliente lo reciba dentro del intervalo de tiempo de espera.</span><span class="sxs-lookup"><span data-stu-id="7245c-385">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="7245c-386">El valor recomendado es un número al menos el doble del valor del `KeepAliveInterval` del servidor, para permitir que llegue el tiempo para que lleguen los ping.</span><span class="sxs-lookup"><span data-stu-id="7245c-386">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="7245c-387">15 segundos</span><span class="sxs-lookup"><span data-stu-id="7245c-387">15 seconds</span></span> | <span data-ttu-id="7245c-388">Tiempo de espera para el protocolo de enlace inicial del servidor.</span><span class="sxs-lookup"><span data-stu-id="7245c-388">Timeout for initial server handshake.</span></span> <span data-ttu-id="7245c-389">Si el servidor no envía una respuesta de protocolo de enlace en este intervalo, el cliente cancela el protocolo de enlace y desencadena el evento `onClose`.</span><span class="sxs-lookup"><span data-stu-id="7245c-389">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="7245c-390">Se trata de una configuración avanzada que solo se debe modificar si se producen errores de tiempo de espera del Protocolo de enlace debido a una latencia de red grave.</span><span class="sxs-lookup"><span data-stu-id="7245c-390">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="7245c-391">Para obtener más información sobre el proceso de enlace, consulte la [especificación del Protocolo de concentrador deSignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="7245c-391">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

::: moniker-end

---

### <a name="configure-additional-options"></a><span data-ttu-id="7245c-392">Configurar opciones adicionales</span><span class="sxs-lookup"><span data-stu-id="7245c-392">Configure additional options</span></span>

<span data-ttu-id="7245c-393">Se pueden configurar opciones adicionales en el método `WithUrl` (`withUrl` en JavaScript) en `HubConnectionBuilder` o en las diversas API de configuración de la `HttpHubConnectionBuilder` en el cliente de Java:</span><span class="sxs-lookup"><span data-stu-id="7245c-393">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="nettabdotnet"></a>[<span data-ttu-id="7245c-394">.NET</span><span class="sxs-lookup"><span data-stu-id="7245c-394">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="7245c-395">Opción de .NET</span><span class="sxs-lookup"><span data-stu-id="7245c-395">.NET Option</span></span> |  <span data-ttu-id="7245c-396">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="7245c-396">Default value</span></span> | <span data-ttu-id="7245c-397">Descripción</span><span class="sxs-lookup"><span data-stu-id="7245c-397">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="7245c-398">Función que devuelve una cadena que se proporciona como un token de autenticación de portador en solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="7245c-398">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="7245c-399">Establezca este valor en `true` para omitir el paso de negociación.</span><span class="sxs-lookup"><span data-stu-id="7245c-399">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="7245c-400">**Solo se admite cuando el transporte de WebSockets es el único transporte habilitado**.</span><span class="sxs-lookup"><span data-stu-id="7245c-400">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="7245c-401">Esta configuración no se puede habilitar cuando se usa el servicio Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="7245c-401">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="7245c-402">Empty</span><span class="sxs-lookup"><span data-stu-id="7245c-402">Empty</span></span> | <span data-ttu-id="7245c-403">Colección de certificados TLS que se enviarán a las solicitudes de autenticación.</span><span class="sxs-lookup"><span data-stu-id="7245c-403">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="7245c-404">Empty</span><span class="sxs-lookup"><span data-stu-id="7245c-404">Empty</span></span> | <span data-ttu-id="7245c-405">Colección de cookies HTTP que se enviarán con cada solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="7245c-405">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="7245c-406">Empty</span><span class="sxs-lookup"><span data-stu-id="7245c-406">Empty</span></span> | <span data-ttu-id="7245c-407">Credenciales que se van a enviar con cada solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="7245c-407">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="7245c-408">5 segundos</span><span class="sxs-lookup"><span data-stu-id="7245c-408">5 seconds</span></span> | <span data-ttu-id="7245c-409">Solo WebSockets.</span><span class="sxs-lookup"><span data-stu-id="7245c-409">WebSockets only.</span></span> <span data-ttu-id="7245c-410">Cantidad máxima de tiempo que el cliente espera después de cerrarse para que el servidor confirme la solicitud de cierre.</span><span class="sxs-lookup"><span data-stu-id="7245c-410">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="7245c-411">Si el servidor no reconoce el cierre dentro de este tiempo, el cliente se desconecta.</span><span class="sxs-lookup"><span data-stu-id="7245c-411">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="7245c-412">Empty</span><span class="sxs-lookup"><span data-stu-id="7245c-412">Empty</span></span> | <span data-ttu-id="7245c-413">Asignación de encabezados HTTP adicionales que se van a enviar con cada solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="7245c-413">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="7245c-414">Delegado que se puede utilizar para configurar o reemplazar el `HttpMessageHandler` utilizado para enviar solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="7245c-414">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="7245c-415">No se usa para las conexiones WebSocket.</span><span class="sxs-lookup"><span data-stu-id="7245c-415">Not used for WebSocket connections.</span></span> <span data-ttu-id="7245c-416">Este delegado debe devolver un valor distinto de NULL y recibe el valor predeterminado como parámetro.</span><span class="sxs-lookup"><span data-stu-id="7245c-416">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="7245c-417">Modifique la configuración de ese valor predeterminado y devuelva o devuelva una nueva instancia de `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="7245c-417">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="7245c-418">**Al reemplazar el controlador, asegúrese de copiar la configuración que desea conservar del controlador proporcionado; de lo contrario, las opciones configuradas (como cookies y encabezados) no se aplicarán al nuevo controlador.**</span><span class="sxs-lookup"><span data-stu-id="7245c-418">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="7245c-419">Proxy HTTP que se va a usar al enviar solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="7245c-419">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="7245c-420">Establezca este valor booleano para enviar las credenciales predeterminadas para las solicitudes HTTP y WebSockets.</span><span class="sxs-lookup"><span data-stu-id="7245c-420">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="7245c-421">Esto habilita el uso de la autenticación de Windows.</span><span class="sxs-lookup"><span data-stu-id="7245c-421">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="7245c-422">Delegado que se puede usar para configurar opciones adicionales de WebSocket.</span><span class="sxs-lookup"><span data-stu-id="7245c-422">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="7245c-423">Recibe una instancia de [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) que se puede usar para configurar las opciones.</span><span class="sxs-lookup"><span data-stu-id="7245c-423">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="7245c-424">JavaScript</span><span class="sxs-lookup"><span data-stu-id="7245c-424">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="7245c-425">Opción de JavaScript</span><span class="sxs-lookup"><span data-stu-id="7245c-425">JavaScript Option</span></span> | <span data-ttu-id="7245c-426">Default Value</span><span class="sxs-lookup"><span data-stu-id="7245c-426">Default Value</span></span> | <span data-ttu-id="7245c-427">Descripción</span><span class="sxs-lookup"><span data-stu-id="7245c-427">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="7245c-428">Función que devuelve una cadena que se proporciona como un token de autenticación de portador en solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="7245c-428">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="7245c-429">Establezca este valor en `true` para omitir el paso de negociación.</span><span class="sxs-lookup"><span data-stu-id="7245c-429">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="7245c-430">**Solo se admite cuando el transporte de WebSockets es el único transporte habilitado**.</span><span class="sxs-lookup"><span data-stu-id="7245c-430">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="7245c-431">Esta configuración no se puede habilitar cuando se usa el servicio Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="7245c-431">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="7245c-432">Java</span><span class="sxs-lookup"><span data-stu-id="7245c-432">Java</span></span>](#tab/java)

| <span data-ttu-id="7245c-433">Opción de Java</span><span class="sxs-lookup"><span data-stu-id="7245c-433">Java Option</span></span> | <span data-ttu-id="7245c-434">Default Value</span><span class="sxs-lookup"><span data-stu-id="7245c-434">Default Value</span></span> | <span data-ttu-id="7245c-435">Descripción</span><span class="sxs-lookup"><span data-stu-id="7245c-435">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="7245c-436">Función que devuelve una cadena que se proporciona como un token de autenticación de portador en solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="7245c-436">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="7245c-437">Establezca este valor en `true` para omitir el paso de negociación.</span><span class="sxs-lookup"><span data-stu-id="7245c-437">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="7245c-438">**Solo se admite cuando el transporte de WebSockets es el único transporte habilitado**.</span><span class="sxs-lookup"><span data-stu-id="7245c-438">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="7245c-439">Esta configuración no se puede habilitar cuando se usa el servicio Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="7245c-439">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="7245c-440">`withHeader` `withHeaders`</span><span class="sxs-lookup"><span data-stu-id="7245c-440">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="7245c-441">Empty</span><span class="sxs-lookup"><span data-stu-id="7245c-441">Empty</span></span> | <span data-ttu-id="7245c-442">Asignación de encabezados HTTP adicionales que se van a enviar con cada solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="7245c-442">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="7245c-443">En el cliente .NET, estas opciones se pueden modificar mediante el delegado de opciones proporcionado a `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="7245c-443">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="7245c-444">En el cliente de JavaScript, estas opciones se pueden proporcionar en un objeto de JavaScript proporcionado a `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="7245c-444">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="7245c-445">En el cliente de Java, estas opciones se pueden configurar con los métodos del `HttpHubConnectionBuilder` devueltos desde el `HubConnectionBuilder.create("HUB URL")`</span><span class="sxs-lookup"><span data-stu-id="7245c-445">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="7245c-446">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="7245c-446">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
