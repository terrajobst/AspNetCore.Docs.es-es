---
title: Configuración de ASP.NET Core Signalr
author: bradygaster
description: Aprenda a configurar aplicaciones de ASP.NET Core Signalr.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 08/05/2019
uid: signalr/configuration
ms.openlocfilehash: 66f274fcda27392091de6b4be8c7221bc87b7585
ms.sourcegitcommit: c452e6af92e130413106c4863193f377cde4cd9c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/10/2019
ms.locfileid: "72246491"
---
# <a name="aspnet-core-signalr-configuration"></a><span data-ttu-id="6feb1-103">Configuración de ASP.NET Core Signalr</span><span class="sxs-lookup"><span data-stu-id="6feb1-103">ASP.NET Core SignalR configuration</span></span>

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="6feb1-104">Opciones de serialización de JSON/MessagePack</span><span class="sxs-lookup"><span data-stu-id="6feb1-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="6feb1-105">ASP.NET Core Signalr admite dos protocolos para codificar los mensajes: [JSON](https://www.json.org/) y [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="6feb1-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="6feb1-106">Cada protocolo tiene opciones de configuración de serialización.</span><span class="sxs-lookup"><span data-stu-id="6feb1-106">Each protocol has serialization configuration options.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="6feb1-107">La serialización de JSON se puede configurar en el servidor mediante el método de extensión [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) .</span><span class="sxs-lookup"><span data-stu-id="6feb1-107">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method.</span></span> <span data-ttu-id="6feb1-108">`AddJsonProtocol` se puede agregar después de [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) en @no__t 2.</span><span class="sxs-lookup"><span data-stu-id="6feb1-108">`AddJsonProtocol` can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="6feb1-109">El método `AddJsonProtocol` toma un delegado que recibe un objeto `options`.</span><span class="sxs-lookup"><span data-stu-id="6feb1-109">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="6feb1-110">La propiedad [PayloadSerializerOptions](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializeroptions) de ese objeto es un objeto `System.Text.Json` <xref:System.Text.Json.JsonSerializerOptions> que se puede usar para configurar la serialización de argumentos y valores devueltos.</span><span class="sxs-lookup"><span data-stu-id="6feb1-110">The [PayloadSerializerOptions](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializeroptions) property on that object is a `System.Text.Json` <xref:System.Text.Json.JsonSerializerOptions> object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="6feb1-111">Para obtener más información, vea la [documentación de System. Text. JSON](/dotnet/api/system.text.json).</span><span class="sxs-lookup"><span data-stu-id="6feb1-111">For more information, see the [System.Text.Json documentation](/dotnet/api/system.text.json).</span></span>

<span data-ttu-id="6feb1-112">Por ejemplo, para configurar el serializador para que no cambie el uso de mayúsculas y minúsculas de los nombres de propiedad, en lugar de los nombres "camelCase" predeterminados, use el código siguiente en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="6feb1-112">As an example, to configure the serializer to not change the casing of property names, instead of the default "camelCase" names, use the following code in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerOptions.PropertyNamingPolicy = null
    });
```

<span data-ttu-id="6feb1-113">En el cliente de .NET, el mismo método de extensión `AddJsonProtocol` existe en [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="6feb1-113">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="6feb1-114">El espacio de nombres `Microsoft.Extensions.DependencyInjection` se debe importar para resolver el método de extensión:</span><span class="sxs-lookup"><span data-stu-id="6feb1-114">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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

### <a name="switch-to-newtonsoftjson"></a><span data-ttu-id="6feb1-115">Cambiar a Newtonsoft. JSON</span><span class="sxs-lookup"><span data-stu-id="6feb1-115">Switch to Newtonsoft.Json</span></span>

<span data-ttu-id="6feb1-116">Si necesita características de `Newtonsoft.Json` que no se admiten en `System.Text.Json`, consulte [cambiar a Newtonsoft. JSON](xref:migration/22-to-30#switch-to-newtonsoftjson).</span><span class="sxs-lookup"><span data-stu-id="6feb1-116">If you need features of `Newtonsoft.Json` that aren't supported in `System.Text.Json`, See [Switch to Newtonsoft.Json](xref:migration/22-to-30#switch-to-newtonsoftjson).</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="6feb1-117">La serialización de JSON se puede configurar en el servidor mediante el método de extensión [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) , que se puede agregar después de [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) en el método `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="6feb1-117">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="6feb1-118">El método `AddJsonProtocol` toma un delegado que recibe un objeto `options`.</span><span class="sxs-lookup"><span data-stu-id="6feb1-118">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="6feb1-119">La propiedad [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) de ese objeto es un objeto JSON.net `JsonSerializerSettings` que se puede usar para configurar la serialización de argumentos y valores devueltos.</span><span class="sxs-lookup"><span data-stu-id="6feb1-119">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="6feb1-120">Para obtener más información, vea la [documentación de JSON.net](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span><span class="sxs-lookup"><span data-stu-id="6feb1-120">For more information, see the [JSON.NET documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span></span>
 
<span data-ttu-id="6feb1-121">Por ejemplo, para configurar el serializador de modo que use nombres de propiedad "PascalCase", en lugar de los nombres predeterminados de "camelCase", use el siguiente código en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="6feb1-121">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code in `Startup.ConfigureServices`:</span></span>
 
```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
            new DefaultContractResolver();
    });
```

<span data-ttu-id="6feb1-122">En el cliente de .NET, el mismo método de extensión `AddJsonProtocol` existe en [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="6feb1-122">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="6feb1-123">El espacio de nombres `Microsoft.Extensions.DependencyInjection` se debe importar para resolver el método de extensión:</span><span class="sxs-lookup"><span data-stu-id="6feb1-123">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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

::: moniker-end

> [!NOTE]
> <span data-ttu-id="6feb1-124">En este momento, no es posible configurar la serialización de JSON en el cliente de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="6feb1-124">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="6feb1-125">Opciones de serialización de MessagePack</span><span class="sxs-lookup"><span data-stu-id="6feb1-125">MessagePack serialization options</span></span>

<span data-ttu-id="6feb1-126">La serialización de MessagePack se puede configurar proporcionando un delegado a la llamada a [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) .</span><span class="sxs-lookup"><span data-stu-id="6feb1-126">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="6feb1-127">Consulte [MessagePack en signalr](xref:signalr/messagepackhubprotocol) para obtener más detalles.</span><span class="sxs-lookup"><span data-stu-id="6feb1-127">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="6feb1-128">En este momento, no es posible configurar la serialización de MessagePack en el cliente de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="6feb1-128">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="6feb1-129">Configurar opciones de servidor</span><span class="sxs-lookup"><span data-stu-id="6feb1-129">Configure server options</span></span>

<span data-ttu-id="6feb1-130">En la tabla siguiente se describen las opciones para configurar los concentradores de Signalr:</span><span class="sxs-lookup"><span data-stu-id="6feb1-130">The following table describes options for configuring SignalR hubs:</span></span>

::: moniker range=">= aspnetcore-3.0"

| <span data-ttu-id="6feb1-131">Opción</span><span class="sxs-lookup"><span data-stu-id="6feb1-131">Option</span></span> | <span data-ttu-id="6feb1-132">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="6feb1-132">Default Value</span></span> | <span data-ttu-id="6feb1-133">Descripción</span><span class="sxs-lookup"><span data-stu-id="6feb1-133">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="6feb1-134">30 segundos</span><span class="sxs-lookup"><span data-stu-id="6feb1-134">30 seconds</span></span> | <span data-ttu-id="6feb1-135">El servidor considerará que el cliente se ha desconectado Si no ha recibido un mensaje (incluido Keep-Alive) en este intervalo.</span><span class="sxs-lookup"><span data-stu-id="6feb1-135">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="6feb1-136">Podría tardar más de este intervalo de tiempo de espera para que el cliente se marque realmente como desconectado, debido a la manera en que se implementa.</span><span class="sxs-lookup"><span data-stu-id="6feb1-136">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="6feb1-137">El valor recomendado es el doble del valor `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="6feb1-137">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="6feb1-138">15 segundos</span><span class="sxs-lookup"><span data-stu-id="6feb1-138">15 seconds</span></span> | <span data-ttu-id="6feb1-139">Si el cliente no envía un mensaje de enlace inicial dentro de este intervalo de tiempo, la conexión se cierra.</span><span class="sxs-lookup"><span data-stu-id="6feb1-139">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="6feb1-140">Se trata de una configuración avanzada que solo se debe modificar si se producen errores de tiempo de espera del Protocolo de enlace debido a una latencia de red grave.</span><span class="sxs-lookup"><span data-stu-id="6feb1-140">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="6feb1-141">Para más información sobre el proceso de enlace, consulte la [especificación del protocolo signalr Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="6feb1-141">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="6feb1-142">15 segundos</span><span class="sxs-lookup"><span data-stu-id="6feb1-142">15 seconds</span></span> | <span data-ttu-id="6feb1-143">Si el servidor no ha enviado un mensaje dentro de este intervalo, se envía automáticamente un mensaje ping para mantener abierta la conexión.</span><span class="sxs-lookup"><span data-stu-id="6feb1-143">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="6feb1-144">Al cambiar `KeepAliveInterval`, cambie la configuración de `ServerTimeout` @ no__t-2 @ no__t-3 en el cliente.</span><span class="sxs-lookup"><span data-stu-id="6feb1-144">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="6feb1-145">El valor recomendado `ServerTimeout` @ no__t-1 @ no__t-2 es el doble del valor `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="6feb1-145">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="6feb1-146">Todos los protocolos instalados</span><span class="sxs-lookup"><span data-stu-id="6feb1-146">All installed protocols</span></span> | <span data-ttu-id="6feb1-147">Protocolos admitidos por este centro.</span><span class="sxs-lookup"><span data-stu-id="6feb1-147">Protocols supported by this hub.</span></span> <span data-ttu-id="6feb1-148">De forma predeterminada, todos los protocolos registrados en el servidor están permitidos, pero los protocolos se pueden quitar de esta lista para deshabilitar determinados protocolos para centros individuales.</span><span class="sxs-lookup"><span data-stu-id="6feb1-148">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="6feb1-149">Si `true`, los mensajes de excepción detallados se devuelven a los clientes cuando se produce una excepción en un método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="6feb1-149">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="6feb1-150">El valor predeterminado es `false`, ya que estos mensajes de excepción pueden contener información confidencial.</span><span class="sxs-lookup"><span data-stu-id="6feb1-150">The default is `false`, as these exception messages can contain sensitive information.</span></span> |
| `StreamBufferCapacity` | `10` | <span data-ttu-id="6feb1-151">Número máximo de elementos que se pueden almacenar en búfer para las secuencias de carga de cliente.</span><span class="sxs-lookup"><span data-stu-id="6feb1-151">The maximum number of items that can be buffered for client upload streams.</span></span> <span data-ttu-id="6feb1-152">Si se alcanza este límite, el procesamiento de las invocaciones se bloquea hasta que el servidor procesa los elementos de la secuencia.</span><span class="sxs-lookup"><span data-stu-id="6feb1-152">If this limit is reached, the processing of invocations is blocked until the server processes stream items.</span></span>|
| `MaximumReceiveMessageSize` | <span data-ttu-id="6feb1-153">32 KB</span><span class="sxs-lookup"><span data-stu-id="6feb1-153">32 KB</span></span> | <span data-ttu-id="6feb1-154">Tamaño máximo de un único mensaje de concentrador entrante.</span><span class="sxs-lookup"><span data-stu-id="6feb1-154">Maximum size of a single incoming hub message.</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.2"

| <span data-ttu-id="6feb1-155">Opción</span><span class="sxs-lookup"><span data-stu-id="6feb1-155">Option</span></span> | <span data-ttu-id="6feb1-156">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="6feb1-156">Default Value</span></span> | <span data-ttu-id="6feb1-157">Descripción</span><span class="sxs-lookup"><span data-stu-id="6feb1-157">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="6feb1-158">30 segundos</span><span class="sxs-lookup"><span data-stu-id="6feb1-158">30 seconds</span></span> | <span data-ttu-id="6feb1-159">El servidor considerará que el cliente se ha desconectado Si no ha recibido un mensaje (incluido Keep-Alive) en este intervalo.</span><span class="sxs-lookup"><span data-stu-id="6feb1-159">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="6feb1-160">Podría tardar más de este intervalo de tiempo de espera para que el cliente se marque realmente como desconectado, debido a la manera en que se implementa.</span><span class="sxs-lookup"><span data-stu-id="6feb1-160">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="6feb1-161">El valor recomendado es el doble del valor `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="6feb1-161">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="6feb1-162">15 segundos</span><span class="sxs-lookup"><span data-stu-id="6feb1-162">15 seconds</span></span> | <span data-ttu-id="6feb1-163">Si el cliente no envía un mensaje de enlace inicial dentro de este intervalo de tiempo, la conexión se cierra.</span><span class="sxs-lookup"><span data-stu-id="6feb1-163">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="6feb1-164">Se trata de una configuración avanzada que solo se debe modificar si se producen errores de tiempo de espera del Protocolo de enlace debido a una latencia de red grave.</span><span class="sxs-lookup"><span data-stu-id="6feb1-164">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="6feb1-165">Para más información sobre el proceso de enlace, consulte la [especificación del protocolo signalr Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="6feb1-165">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="6feb1-166">15 segundos</span><span class="sxs-lookup"><span data-stu-id="6feb1-166">15 seconds</span></span> | <span data-ttu-id="6feb1-167">Si el servidor no ha enviado un mensaje dentro de este intervalo, se envía automáticamente un mensaje ping para mantener abierta la conexión.</span><span class="sxs-lookup"><span data-stu-id="6feb1-167">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="6feb1-168">Al cambiar `KeepAliveInterval`, cambie la configuración de `ServerTimeout` @ no__t-2 @ no__t-3 en el cliente.</span><span class="sxs-lookup"><span data-stu-id="6feb1-168">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="6feb1-169">El valor recomendado `ServerTimeout` @ no__t-1 @ no__t-2 es el doble del valor `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="6feb1-169">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="6feb1-170">Todos los protocolos instalados</span><span class="sxs-lookup"><span data-stu-id="6feb1-170">All installed protocols</span></span> | <span data-ttu-id="6feb1-171">Protocolos admitidos por este centro.</span><span class="sxs-lookup"><span data-stu-id="6feb1-171">Protocols supported by this hub.</span></span> <span data-ttu-id="6feb1-172">De forma predeterminada, todos los protocolos registrados en el servidor están permitidos, pero los protocolos se pueden quitar de esta lista para deshabilitar determinados protocolos para centros individuales.</span><span class="sxs-lookup"><span data-stu-id="6feb1-172">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="6feb1-173">Si `true`, los mensajes de excepción detallados se devuelven a los clientes cuando se produce una excepción en un método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="6feb1-173">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="6feb1-174">El valor predeterminado es `false`, ya que estos mensajes de excepción pueden contener información confidencial.</span><span class="sxs-lookup"><span data-stu-id="6feb1-174">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| <span data-ttu-id="6feb1-175">Opción</span><span class="sxs-lookup"><span data-stu-id="6feb1-175">Option</span></span> | <span data-ttu-id="6feb1-176">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="6feb1-176">Default Value</span></span> | <span data-ttu-id="6feb1-177">Descripción</span><span class="sxs-lookup"><span data-stu-id="6feb1-177">Description</span></span> |
| ------ | ------------- | ----------- |
| `HandshakeTimeout` | <span data-ttu-id="6feb1-178">15 segundos</span><span class="sxs-lookup"><span data-stu-id="6feb1-178">15 seconds</span></span> | <span data-ttu-id="6feb1-179">Si el cliente no envía un mensaje de enlace inicial dentro de este intervalo de tiempo, la conexión se cierra.</span><span class="sxs-lookup"><span data-stu-id="6feb1-179">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="6feb1-180">Se trata de una configuración avanzada que solo se debe modificar si se producen errores de tiempo de espera del Protocolo de enlace debido a una latencia de red grave.</span><span class="sxs-lookup"><span data-stu-id="6feb1-180">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="6feb1-181">Para más información sobre el proceso de enlace, consulte la [especificación del protocolo signalr Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="6feb1-181">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="6feb1-182">15 segundos</span><span class="sxs-lookup"><span data-stu-id="6feb1-182">15 seconds</span></span> | <span data-ttu-id="6feb1-183">Si el servidor no ha enviado un mensaje dentro de este intervalo, se envía automáticamente un mensaje ping para mantener abierta la conexión.</span><span class="sxs-lookup"><span data-stu-id="6feb1-183">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="6feb1-184">Al cambiar `KeepAliveInterval`, cambie la configuración de `ServerTimeout` @ no__t-2 @ no__t-3 en el cliente.</span><span class="sxs-lookup"><span data-stu-id="6feb1-184">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="6feb1-185">El valor recomendado `ServerTimeout` @ no__t-1 @ no__t-2 es el doble del valor `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="6feb1-185">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="6feb1-186">Todos los protocolos instalados</span><span class="sxs-lookup"><span data-stu-id="6feb1-186">All installed protocols</span></span> | <span data-ttu-id="6feb1-187">Protocolos admitidos por este centro.</span><span class="sxs-lookup"><span data-stu-id="6feb1-187">Protocols supported by this hub.</span></span> <span data-ttu-id="6feb1-188">De forma predeterminada, todos los protocolos registrados en el servidor están permitidos, pero los protocolos se pueden quitar de esta lista para deshabilitar determinados protocolos para centros individuales.</span><span class="sxs-lookup"><span data-stu-id="6feb1-188">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="6feb1-189">Si `true`, los mensajes de excepción detallados se devuelven a los clientes cuando se produce una excepción en un método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="6feb1-189">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="6feb1-190">El valor predeterminado es `false`, ya que estos mensajes de excepción pueden contener información confidencial.</span><span class="sxs-lookup"><span data-stu-id="6feb1-190">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

::: moniker-end

<span data-ttu-id="6feb1-191">Las opciones se pueden configurar para todos los concentradores proporcionando un delegado de opciones a la llamada `AddSignalR` en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="6feb1-191">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="6feb1-192">Las opciones de un solo concentrador invalidan las opciones globales proporcionadas en `AddSignalR` y se pueden configurar mediante <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span><span class="sxs-lookup"><span data-stu-id="6feb1-192">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="6feb1-193">Opciones de configuración de HTTP avanzadas</span><span class="sxs-lookup"><span data-stu-id="6feb1-193">Advanced HTTP configuration options</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="6feb1-194">Use `HttpConnectionDispatcherOptions` para configurar las opciones avanzadas relacionadas con los transportes y la administración del búfer de memoria.</span><span class="sxs-lookup"><span data-stu-id="6feb1-194">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="6feb1-195">Estas opciones se configuran pasando un delegado a [MapHub @ no__t-1t >](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub) en `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="6feb1-195">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="6feb1-196">Use `HttpConnectionDispatcherOptions` para configurar las opciones avanzadas relacionadas con los transportes y la administración del búfer de memoria.</span><span class="sxs-lookup"><span data-stu-id="6feb1-196">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="6feb1-197">Estas opciones se configuran pasando un delegado a [MapHub @ no__t-1t >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) en `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="6feb1-197">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="6feb1-198">En la tabla siguiente se describen las opciones para configurar las opciones de HTTP avanzadas de ASP.NET Core Signalr:</span><span class="sxs-lookup"><span data-stu-id="6feb1-198">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

::: moniker range=">= aspnetcore-3.0"

| <span data-ttu-id="6feb1-199">Opción</span><span class="sxs-lookup"><span data-stu-id="6feb1-199">Option</span></span> | <span data-ttu-id="6feb1-200">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="6feb1-200">Default Value</span></span> | <span data-ttu-id="6feb1-201">Descripción</span><span class="sxs-lookup"><span data-stu-id="6feb1-201">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="6feb1-202">32 KB</span><span class="sxs-lookup"><span data-stu-id="6feb1-202">32 KB</span></span> | <span data-ttu-id="6feb1-203">Número máximo de bytes recibidos del cliente que el servidor almacena en búfer antes de aplicar la presión de reserva.</span><span class="sxs-lookup"><span data-stu-id="6feb1-203">The maximum number of bytes received from the client that the server buffers before applying backpressure.</span></span> <span data-ttu-id="6feb1-204">Aumentar este valor permite que el servidor reciba mensajes más grandes con más rapidez sin aplicar la presión, pero puede aumentar el consumo de memoria.</span><span class="sxs-lookup"><span data-stu-id="6feb1-204">Increasing this value allows the server to receive larger messages more quickly without applying backpressure, but can increase memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="6feb1-205">Los datos se recopilan automáticamente a partir de los atributos `Authorize` aplicados a la clase hub.</span><span class="sxs-lookup"><span data-stu-id="6feb1-205">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="6feb1-206">Una lista de objetos [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) que se usan para determinar si un cliente está autorizado para conectarse al centro.</span><span class="sxs-lookup"><span data-stu-id="6feb1-206">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="6feb1-207">32 KB</span><span class="sxs-lookup"><span data-stu-id="6feb1-207">32 KB</span></span> | <span data-ttu-id="6feb1-208">Número máximo de bytes enviados por la aplicación que el servidor almacena en búfer antes de observar la contrapresión.</span><span class="sxs-lookup"><span data-stu-id="6feb1-208">The maximum number of bytes sent by the app that the server buffers before observing backpressure.</span></span> <span data-ttu-id="6feb1-209">El aumento de este valor permite que el servidor almacene en búfer mensajes más grandes con más rapidez sin esperar la presión, pero puede aumentar el consumo de memoria.</span><span class="sxs-lookup"><span data-stu-id="6feb1-209">Increasing this value allows the server to buffer larger messages more quickly without awaiting backpressure, but can increase memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="6feb1-210">Todos los transportes están habilitados.</span><span class="sxs-lookup"><span data-stu-id="6feb1-210">All Transports are enabled.</span></span> | <span data-ttu-id="6feb1-211">Una enumeración de marcas de bits de valores `HttpTransportType` que pueden restringir los transportes que un cliente puede utilizar para conectarse.</span><span class="sxs-lookup"><span data-stu-id="6feb1-211">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="6feb1-212">Véalo a continuación.</span><span class="sxs-lookup"><span data-stu-id="6feb1-212">See below.</span></span> | <span data-ttu-id="6feb1-213">Opciones adicionales específicas del transporte de sondeo prolongado.</span><span class="sxs-lookup"><span data-stu-id="6feb1-213">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="6feb1-214">Véalo a continuación.</span><span class="sxs-lookup"><span data-stu-id="6feb1-214">See below.</span></span> | <span data-ttu-id="6feb1-215">Opciones adicionales específicas del transporte de WebSockets.</span><span class="sxs-lookup"><span data-stu-id="6feb1-215">Additional options specific to the WebSockets transport.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-3.0"

| <span data-ttu-id="6feb1-216">Opción</span><span class="sxs-lookup"><span data-stu-id="6feb1-216">Option</span></span> | <span data-ttu-id="6feb1-217">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="6feb1-217">Default Value</span></span> | <span data-ttu-id="6feb1-218">Descripción</span><span class="sxs-lookup"><span data-stu-id="6feb1-218">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="6feb1-219">32 KB</span><span class="sxs-lookup"><span data-stu-id="6feb1-219">32 KB</span></span> | <span data-ttu-id="6feb1-220">Número máximo de bytes recibidos del cliente que el servidor almacena en búfer.</span><span class="sxs-lookup"><span data-stu-id="6feb1-220">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="6feb1-221">Aumentar este valor permite que el servidor reciba mensajes de mayor tamaño, pero puede afectar negativamente al consumo de memoria.</span><span class="sxs-lookup"><span data-stu-id="6feb1-221">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="6feb1-222">Los datos se recopilan automáticamente a partir de los atributos `Authorize` aplicados a la clase hub.</span><span class="sxs-lookup"><span data-stu-id="6feb1-222">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="6feb1-223">Una lista de objetos [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) que se usan para determinar si un cliente está autorizado para conectarse al centro.</span><span class="sxs-lookup"><span data-stu-id="6feb1-223">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="6feb1-224">32 KB</span><span class="sxs-lookup"><span data-stu-id="6feb1-224">32 KB</span></span> | <span data-ttu-id="6feb1-225">Número máximo de bytes enviados por la aplicación que el servidor almacena en búfer.</span><span class="sxs-lookup"><span data-stu-id="6feb1-225">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="6feb1-226">Al aumentar este valor, el servidor puede enviar mensajes de mayor tamaño, pero puede afectar negativamente al consumo de memoria.</span><span class="sxs-lookup"><span data-stu-id="6feb1-226">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="6feb1-227">Todos los transportes están habilitados.</span><span class="sxs-lookup"><span data-stu-id="6feb1-227">All Transports are enabled.</span></span> | <span data-ttu-id="6feb1-228">Una enumeración de marcas de bits de valores `HttpTransportType` que pueden restringir los transportes que un cliente puede utilizar para conectarse.</span><span class="sxs-lookup"><span data-stu-id="6feb1-228">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="6feb1-229">Véalo a continuación.</span><span class="sxs-lookup"><span data-stu-id="6feb1-229">See below.</span></span> | <span data-ttu-id="6feb1-230">Opciones adicionales específicas del transporte de sondeo prolongado.</span><span class="sxs-lookup"><span data-stu-id="6feb1-230">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="6feb1-231">Véalo a continuación.</span><span class="sxs-lookup"><span data-stu-id="6feb1-231">See below.</span></span> | <span data-ttu-id="6feb1-232">Opciones adicionales específicas del transporte de WebSockets.</span><span class="sxs-lookup"><span data-stu-id="6feb1-232">Additional options specific to the WebSockets transport.</span></span> |

::: moniker-end

<span data-ttu-id="6feb1-233">El transporte de sondeo largo tiene opciones adicionales que se pueden configurar mediante la propiedad `LongPolling`:</span><span class="sxs-lookup"><span data-stu-id="6feb1-233">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="6feb1-234">Opción</span><span class="sxs-lookup"><span data-stu-id="6feb1-234">Option</span></span> | <span data-ttu-id="6feb1-235">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="6feb1-235">Default Value</span></span> | <span data-ttu-id="6feb1-236">Descripción</span><span class="sxs-lookup"><span data-stu-id="6feb1-236">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="6feb1-237">90 segundos</span><span class="sxs-lookup"><span data-stu-id="6feb1-237">90 seconds</span></span> | <span data-ttu-id="6feb1-238">Cantidad máxima de tiempo que el servidor espera a que se envíe un mensaje al cliente antes de finalizar una única solicitud de sondeo.</span><span class="sxs-lookup"><span data-stu-id="6feb1-238">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="6feb1-239">Al disminuir este valor, el cliente emite nuevas solicitudes de sondeo con mayor frecuencia.</span><span class="sxs-lookup"><span data-stu-id="6feb1-239">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="6feb1-240">El transporte de WebSocket tiene opciones adicionales que se pueden configurar mediante la propiedad `WebSockets`:</span><span class="sxs-lookup"><span data-stu-id="6feb1-240">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="6feb1-241">Opción</span><span class="sxs-lookup"><span data-stu-id="6feb1-241">Option</span></span> | <span data-ttu-id="6feb1-242">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="6feb1-242">Default Value</span></span> | <span data-ttu-id="6feb1-243">Descripción</span><span class="sxs-lookup"><span data-stu-id="6feb1-243">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="6feb1-244">5 segundos</span><span class="sxs-lookup"><span data-stu-id="6feb1-244">5 seconds</span></span> | <span data-ttu-id="6feb1-245">Una vez que se cierra el servidor, si el cliente no se cierra dentro de este intervalo de tiempo, se finaliza la conexión.</span><span class="sxs-lookup"><span data-stu-id="6feb1-245">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="6feb1-246">Delegado que se puede utilizar para establecer el encabezado `Sec-WebSocket-Protocol` en un valor personalizado.</span><span class="sxs-lookup"><span data-stu-id="6feb1-246">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="6feb1-247">El delegado recibe los valores solicitados por el cliente como entrada y se espera que devuelva el valor deseado.</span><span class="sxs-lookup"><span data-stu-id="6feb1-247">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="6feb1-248">Configurar opciones de cliente</span><span class="sxs-lookup"><span data-stu-id="6feb1-248">Configure client options</span></span>

<span data-ttu-id="6feb1-249">Las opciones de cliente se pueden configurar en el tipo `HubConnectionBuilder` (disponible en los clientes de .NET y JavaScript).</span><span class="sxs-lookup"><span data-stu-id="6feb1-249">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="6feb1-250">También está disponible en el cliente de Java, pero la subclase `HttpHubConnectionBuilder` es lo que contiene las opciones de configuración del generador, así como en el propio `HubConnection`.</span><span class="sxs-lookup"><span data-stu-id="6feb1-250">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="6feb1-251">Configurar el registro</span><span class="sxs-lookup"><span data-stu-id="6feb1-251">Configure logging</span></span>

<span data-ttu-id="6feb1-252">El registro se configura en el cliente .NET mediante el método `ConfigureLogging`.</span><span class="sxs-lookup"><span data-stu-id="6feb1-252">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="6feb1-253">Los proveedores de registro y los filtros se pueden registrar de la misma manera que en el servidor.</span><span class="sxs-lookup"><span data-stu-id="6feb1-253">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="6feb1-254">Consulte la documentación de [Inicio de sesión ASP.net Core](xref:fundamentals/logging/index) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="6feb1-254">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="6feb1-255">Para registrar los proveedores de registro, debe instalar los paquetes necesarios.</span><span class="sxs-lookup"><span data-stu-id="6feb1-255">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="6feb1-256">Consulte la sección [proveedores de registro integrados](xref:fundamentals/logging/index#built-in-logging-providers) de los documentos para obtener una lista completa.</span><span class="sxs-lookup"><span data-stu-id="6feb1-256">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="6feb1-257">Por ejemplo, para habilitar el registro de la consola, instale el paquete NuGet `Microsoft.Extensions.Logging.Console`.</span><span class="sxs-lookup"><span data-stu-id="6feb1-257">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="6feb1-258">Llame al método de extensión `AddConsole`:</span><span class="sxs-lookup"><span data-stu-id="6feb1-258">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="6feb1-259">En el cliente de JavaScript, existe un método `configureLogging` similar.</span><span class="sxs-lookup"><span data-stu-id="6feb1-259">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="6feb1-260">Proporcione un valor `LogLevel` que indique el nivel mínimo de mensajes de registro que se va a producir.</span><span class="sxs-lookup"><span data-stu-id="6feb1-260">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="6feb1-261">Los registros se escriben en la ventana de la consola del explorador.</span><span class="sxs-lookup"><span data-stu-id="6feb1-261">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="6feb1-262">En lugar de un valor `LogLevel`, también puede proporcionar un valor `string` que represente un nombre de nivel de registro.</span><span class="sxs-lookup"><span data-stu-id="6feb1-262">Instead of a `LogLevel` value, you can also provide a `string` value representing a log level name.</span></span> <span data-ttu-id="6feb1-263">Esto resulta útil al configurar el registro de Signalr en entornos en los que no tiene acceso a las constantes `LogLevel`.</span><span class="sxs-lookup"><span data-stu-id="6feb1-263">This is useful when configuring SignalR logging in environments where you don't have access to the `LogLevel` constants.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging("warn")
    .build();
```

<span data-ttu-id="6feb1-264">En la tabla siguiente se enumeran los niveles de registro disponibles.</span><span class="sxs-lookup"><span data-stu-id="6feb1-264">The following table lists the available log levels.</span></span> <span data-ttu-id="6feb1-265">El valor que se proporciona a `configureLogging` establece el nivel de registro **mínimo** que se registrará.</span><span class="sxs-lookup"><span data-stu-id="6feb1-265">The value you provide to `configureLogging` sets the **minimum** log level that will be logged.</span></span> <span data-ttu-id="6feb1-266">Se registrarán los mensajes registrados en este nivel, **o los niveles que se enumeran en la tabla**.</span><span class="sxs-lookup"><span data-stu-id="6feb1-266">Messages logged at this level, **or the levels listed after it in the table**, will be logged.</span></span>

| <span data-ttu-id="6feb1-267">String</span><span class="sxs-lookup"><span data-stu-id="6feb1-267">String</span></span>                      | <span data-ttu-id="6feb1-268">LogLevel</span><span class="sxs-lookup"><span data-stu-id="6feb1-268">LogLevel</span></span>               |
| --------------------------- | ---------------------- |
| `trace`                     | `LogLevel.Trace`       |
| `debug`                     | `LogLevel.Debug`       |
| <span data-ttu-id="6feb1-269">`info` **o** `information`</span><span class="sxs-lookup"><span data-stu-id="6feb1-269">`info` **or** `information`</span></span> | `LogLevel.Information` |
| <span data-ttu-id="6feb1-270">`warn` **o** `warning`</span><span class="sxs-lookup"><span data-stu-id="6feb1-270">`warn` **or** `warning`</span></span>     | `LogLevel.Warning`     |
| `error`                     | `LogLevel.Error`       |
| `critical`                  | `LogLevel.Critical`    |
| `none`                      | `LogLevel.None`        |

::: moniker-end

> [!NOTE]
> <span data-ttu-id="6feb1-271">Para deshabilitar el registro por completo, especifique `signalR.LogLevel.None` en el método `configureLogging`.</span><span class="sxs-lookup"><span data-stu-id="6feb1-271">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="6feb1-272">Para obtener más información sobre el registro, consulte la documentación sobre el [diagnóstico de signalr](xref:signalr/diagnostics).</span><span class="sxs-lookup"><span data-stu-id="6feb1-272">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="6feb1-273">El cliente de Signalr Java usa la biblioteca [SLF4J](https://www.slf4j.org/) para el registro.</span><span class="sxs-lookup"><span data-stu-id="6feb1-273">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="6feb1-274">Se trata de una API de registro de alto nivel que permite a los usuarios de la biblioteca elegir su propia implementación de registro específica mediante la incorporación de una dependencia de registro específica.</span><span class="sxs-lookup"><span data-stu-id="6feb1-274">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="6feb1-275">En el fragmento de código siguiente se muestra cómo usar `java.util.logging` con el cliente de Java de Signalr.</span><span class="sxs-lookup"><span data-stu-id="6feb1-275">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="6feb1-276">Si no configura el registro en las dependencias, SLF4J carga un registrador de no operaciones predeterminado con el siguiente mensaje de ADVERTENCIA:</span><span class="sxs-lookup"><span data-stu-id="6feb1-276">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="6feb1-277">Esto se puede omitir sin ningún riesgo.</span><span class="sxs-lookup"><span data-stu-id="6feb1-277">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="6feb1-278">Configurar transportes permitidos</span><span class="sxs-lookup"><span data-stu-id="6feb1-278">Configure allowed transports</span></span>

<span data-ttu-id="6feb1-279">Los transportes utilizados por Signalr se pueden configurar en la llamada `WithUrl` (`withUrl` en JavaScript).</span><span class="sxs-lookup"><span data-stu-id="6feb1-279">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="6feb1-280">Se puede usar una operación OR bit a bit de los valores de `HttpTransportType` para restringir el cliente de modo que solo utilice los transportes especificados.</span><span class="sxs-lookup"><span data-stu-id="6feb1-280">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="6feb1-281">Todos los transportes están habilitados de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="6feb1-281">All transports are enabled by default.</span></span>

<span data-ttu-id="6feb1-282">Por ejemplo, para deshabilitar el transporte de eventos enviados por el servidor, pero permitir WebSockets y conexiones de sondeo largas:</span><span class="sxs-lookup"><span data-stu-id="6feb1-282">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="6feb1-283">En el cliente de JavaScript, los transportes se configuran estableciendo el campo `transport` en el objeto de opciones proporcionado a `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="6feb1-283">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="6feb1-284">En esta versión del cliente de Java, WebSockets es el único transporte disponible.</span><span class="sxs-lookup"><span data-stu-id="6feb1-284">In this version of the Java client websockets is the only available transport.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-3.0"

<span data-ttu-id="6feb1-285">En el cliente de Java, el transporte se selecciona con el método `withTransport` en el `HttpHubConnectionBuilder`.</span><span class="sxs-lookup"><span data-stu-id="6feb1-285">In the Java client, the transport is selected with the `withTransport` method on the `HttpHubConnectionBuilder`.</span></span> <span data-ttu-id="6feb1-286">De forma predeterminada, el cliente de Java usa el transporte de WebSockets.</span><span class="sxs-lookup"><span data-stu-id="6feb1-286">The Java client defaults to using the WebSockets transport.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withTransport(TransportEnum.WEBSOCKETS)
    .build();
```

> [!NOTE]
> <span data-ttu-id="6feb1-287">El cliente de Signalr Java no admite todavía la reserva de transporte.</span><span class="sxs-lookup"><span data-stu-id="6feb1-287">The SignalR Java client doesn't support transport fallback yet.</span></span>

::: moniker-end

### <a name="configure-bearer-authentication"></a><span data-ttu-id="6feb1-288">Configurar la autenticación de portador</span><span class="sxs-lookup"><span data-stu-id="6feb1-288">Configure bearer authentication</span></span>

<span data-ttu-id="6feb1-289">Para proporcionar datos de autenticación junto con solicitudes de Signalr, use la opción `AccessTokenProvider` (`accessTokenFactory` en JavaScript) para especificar una función que devuelva el token de acceso deseado.</span><span class="sxs-lookup"><span data-stu-id="6feb1-289">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="6feb1-290">En el cliente de .NET, este token de acceso se pasa como un token de "autenticación de portador" de HTTP (mediante el encabezado `Authorization` con un tipo de `Bearer`).</span><span class="sxs-lookup"><span data-stu-id="6feb1-290">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="6feb1-291">En el cliente de JavaScript, el token de acceso se usa como un token de portador, **excepto** en algunos casos en los que las API del explorador restringen la capacidad de aplicar encabezados (específicamente, en los eventos enviados por el servidor y en las solicitudes de WebSockets).</span><span class="sxs-lookup"><span data-stu-id="6feb1-291">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="6feb1-292">En estos casos, el token de acceso se proporciona como un valor de cadena de consulta `access_token`.</span><span class="sxs-lookup"><span data-stu-id="6feb1-292">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="6feb1-293">En el cliente .NET, se puede especificar la opción `AccessTokenProvider` mediante el delegado Options en `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="6feb1-293">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="6feb1-294">En el cliente de JavaScript, el token de acceso se configura estableciendo el campo `accessTokenFactory` en el objeto de opciones de `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="6feb1-294">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

<span data-ttu-id="6feb1-295">En el cliente de Signalr Java, puede configurar un token de portador que se usará para la autenticación proporcionando un generador de tokens de acceso a [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span><span class="sxs-lookup"><span data-stu-id="6feb1-295">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="6feb1-296">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) para proporcionar un [RxJava](https://github.com/ReactiveX/RxJava) de una [única\<cadena>](https://reactivex.io/documentation/single.html).</span><span class="sxs-lookup"><span data-stu-id="6feb1-296">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="6feb1-297">Con una llamada a [Single. defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), puede escribir lógica para generar tokens de acceso para el cliente.</span><span class="sxs-lookup"><span data-stu-id="6feb1-297">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="6feb1-298">Configurar las opciones de tiempo de espera y mantenimiento de conexión</span><span class="sxs-lookup"><span data-stu-id="6feb1-298">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="6feb1-299">Las opciones adicionales para configurar el comportamiento de tiempo de espera y mantenimiento de conexión están disponibles en el propio objeto @no__t:</span><span class="sxs-lookup"><span data-stu-id="6feb1-299">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>


::: moniker range=">= aspnetcore-2.2"

# <a name="nettabdotnet"></a>[<span data-ttu-id="6feb1-300">.NET</span><span class="sxs-lookup"><span data-stu-id="6feb1-300">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="6feb1-301">Opción</span><span class="sxs-lookup"><span data-stu-id="6feb1-301">Option</span></span> | <span data-ttu-id="6feb1-302">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="6feb1-302">Default value</span></span> | <span data-ttu-id="6feb1-303">Descripción</span><span class="sxs-lookup"><span data-stu-id="6feb1-303">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="6feb1-304">30 segundos (30.000 milisegundos)</span><span class="sxs-lookup"><span data-stu-id="6feb1-304">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="6feb1-305">Tiempo de espera para la actividad del servidor.</span><span class="sxs-lookup"><span data-stu-id="6feb1-305">Timeout for server activity.</span></span> <span data-ttu-id="6feb1-306">Si el servidor no ha enviado un mensaje en este intervalo, el cliente considera que el servidor está desconectado y desencadena el evento `Closed` (`onclose` en JavaScript).</span><span class="sxs-lookup"><span data-stu-id="6feb1-306">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="6feb1-307">Este valor debe ser lo suficientemente grande como para que se envíe un mensaje ping desde el servidor **y** el cliente lo reciba dentro del intervalo de tiempo de espera.</span><span class="sxs-lookup"><span data-stu-id="6feb1-307">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="6feb1-308">El valor recomendado es un número al menos el doble del valor `KeepAliveInterval` del servidor para permitir que lleguen los ping.</span><span class="sxs-lookup"><span data-stu-id="6feb1-308">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="6feb1-309">15 segundos</span><span class="sxs-lookup"><span data-stu-id="6feb1-309">15 seconds</span></span> | <span data-ttu-id="6feb1-310">Tiempo de espera para el protocolo de enlace inicial del servidor.</span><span class="sxs-lookup"><span data-stu-id="6feb1-310">Timeout for initial server handshake.</span></span> <span data-ttu-id="6feb1-311">Si el servidor no envía una respuesta de protocolo de enlace en este intervalo, el cliente cancela el protocolo de enlace y desencadena el evento `Closed` (`onclose` en JavaScript).</span><span class="sxs-lookup"><span data-stu-id="6feb1-311">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="6feb1-312">Se trata de una configuración avanzada que solo se debe modificar si se producen errores de tiempo de espera del Protocolo de enlace debido a una latencia de red grave.</span><span class="sxs-lookup"><span data-stu-id="6feb1-312">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="6feb1-313">Para más información sobre el proceso de enlace, consulte la [especificación del protocolo signalr Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="6feb1-313">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="6feb1-314">15 segundos</span><span class="sxs-lookup"><span data-stu-id="6feb1-314">15 seconds</span></span> | <span data-ttu-id="6feb1-315">Determina el intervalo en el que el cliente envía mensajes de ping.</span><span class="sxs-lookup"><span data-stu-id="6feb1-315">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="6feb1-316">Al enviar cualquier mensaje desde el cliente, se restablece el temporizador al inicio del intervalo.</span><span class="sxs-lookup"><span data-stu-id="6feb1-316">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="6feb1-317">Si el cliente no ha enviado un mensaje en el `ClientTimeoutInterval` establecido en el servidor, el servidor considera que el cliente está desconectado.</span><span class="sxs-lookup"><span data-stu-id="6feb1-317">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

<span data-ttu-id="6feb1-318">En el cliente de .NET, los valores de tiempo de espera se especifican como valores `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="6feb1-318">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="6feb1-319">JavaScript</span><span class="sxs-lookup"><span data-stu-id="6feb1-319">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="6feb1-320">Opción</span><span class="sxs-lookup"><span data-stu-id="6feb1-320">Option</span></span> | <span data-ttu-id="6feb1-321">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="6feb1-321">Default value</span></span> | <span data-ttu-id="6feb1-322">Descripción</span><span class="sxs-lookup"><span data-stu-id="6feb1-322">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="6feb1-323">30 segundos (30.000 milisegundos)</span><span class="sxs-lookup"><span data-stu-id="6feb1-323">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="6feb1-324">Tiempo de espera para la actividad del servidor.</span><span class="sxs-lookup"><span data-stu-id="6feb1-324">Timeout for server activity.</span></span> <span data-ttu-id="6feb1-325">Si el servidor no ha enviado un mensaje en este intervalo, el cliente considera que el servidor está desconectado y desencadena el evento `onclose`.</span><span class="sxs-lookup"><span data-stu-id="6feb1-325">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="6feb1-326">Este valor debe ser lo suficientemente grande como para que se envíe un mensaje ping desde el servidor **y** el cliente lo reciba dentro del intervalo de tiempo de espera.</span><span class="sxs-lookup"><span data-stu-id="6feb1-326">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="6feb1-327">El valor recomendado es un número al menos el doble del valor `KeepAliveInterval` del servidor para permitir que lleguen los ping.</span><span class="sxs-lookup"><span data-stu-id="6feb1-327">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `keepAliveIntervalInMilliseconds` | <span data-ttu-id="6feb1-328">15 segundos (15.000 milisegundos)</span><span class="sxs-lookup"><span data-stu-id="6feb1-328">15 seconds (15,000 miliseconds)</span></span> | <span data-ttu-id="6feb1-329">Determina el intervalo en el que el cliente envía mensajes de ping.</span><span class="sxs-lookup"><span data-stu-id="6feb1-329">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="6feb1-330">Al enviar cualquier mensaje desde el cliente, se restablece el temporizador al inicio del intervalo.</span><span class="sxs-lookup"><span data-stu-id="6feb1-330">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="6feb1-331">Si el cliente no ha enviado un mensaje en el `ClientTimeoutInterval` establecido en el servidor, el servidor considera que el cliente está desconectado.</span><span class="sxs-lookup"><span data-stu-id="6feb1-331">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="6feb1-332">Java</span><span class="sxs-lookup"><span data-stu-id="6feb1-332">Java</span></span>](#tab/java)

| <span data-ttu-id="6feb1-333">Opción</span><span class="sxs-lookup"><span data-stu-id="6feb1-333">Option</span></span> | <span data-ttu-id="6feb1-334">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="6feb1-334">Default value</span></span> | <span data-ttu-id="6feb1-335">Descripción</span><span class="sxs-lookup"><span data-stu-id="6feb1-335">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="6feb1-336">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="6feb1-336">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="6feb1-337">30 segundos (30.000 milisegundos)</span><span class="sxs-lookup"><span data-stu-id="6feb1-337">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="6feb1-338">Tiempo de espera para la actividad del servidor.</span><span class="sxs-lookup"><span data-stu-id="6feb1-338">Timeout for server activity.</span></span> <span data-ttu-id="6feb1-339">Si el servidor no ha enviado un mensaje en este intervalo, el cliente considera que el servidor está desconectado y desencadena el evento `onClose`.</span><span class="sxs-lookup"><span data-stu-id="6feb1-339">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="6feb1-340">Este valor debe ser lo suficientemente grande como para que se envíe un mensaje ping desde el servidor **y** el cliente lo reciba dentro del intervalo de tiempo de espera.</span><span class="sxs-lookup"><span data-stu-id="6feb1-340">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="6feb1-341">El valor recomendado es un número al menos el doble del valor `KeepAliveInterval` del servidor para permitir que lleguen los ping.</span><span class="sxs-lookup"><span data-stu-id="6feb1-341">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="6feb1-342">15 segundos</span><span class="sxs-lookup"><span data-stu-id="6feb1-342">15 seconds</span></span> | <span data-ttu-id="6feb1-343">Tiempo de espera para el protocolo de enlace inicial del servidor.</span><span class="sxs-lookup"><span data-stu-id="6feb1-343">Timeout for initial server handshake.</span></span> <span data-ttu-id="6feb1-344">Si el servidor no envía una respuesta de protocolo de enlace en este intervalo, el cliente cancela el protocolo de enlace y desencadena el evento `onClose`.</span><span class="sxs-lookup"><span data-stu-id="6feb1-344">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="6feb1-345">Se trata de una configuración avanzada que solo se debe modificar si se producen errores de tiempo de espera del Protocolo de enlace debido a una latencia de red grave.</span><span class="sxs-lookup"><span data-stu-id="6feb1-345">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="6feb1-346">Para más información sobre el proceso de enlace, consulte la [especificación del protocolo signalr Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="6feb1-346">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| <span data-ttu-id="6feb1-347">`getKeepAliveInterval` / `setKeepAliveInterval`</span><span class="sxs-lookup"><span data-stu-id="6feb1-347">`getKeepAliveInterval` / `setKeepAliveInterval`</span></span> | <span data-ttu-id="6feb1-348">15 segundos (15.000 milisegundos)</span><span class="sxs-lookup"><span data-stu-id="6feb1-348">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="6feb1-349">Determina el intervalo en el que el cliente envía mensajes de ping.</span><span class="sxs-lookup"><span data-stu-id="6feb1-349">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="6feb1-350">Al enviar cualquier mensaje desde el cliente, se restablece el temporizador al inicio del intervalo.</span><span class="sxs-lookup"><span data-stu-id="6feb1-350">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="6feb1-351">Si el cliente no ha enviado un mensaje en el `ClientTimeoutInterval` establecido en el servidor, el servidor considera que el cliente está desconectado.</span><span class="sxs-lookup"><span data-stu-id="6feb1-351">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="nettabdotnet"></a>[<span data-ttu-id="6feb1-352">.NET</span><span class="sxs-lookup"><span data-stu-id="6feb1-352">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="6feb1-353">Opción</span><span class="sxs-lookup"><span data-stu-id="6feb1-353">Option</span></span> | <span data-ttu-id="6feb1-354">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="6feb1-354">Default value</span></span> | <span data-ttu-id="6feb1-355">Descripción</span><span class="sxs-lookup"><span data-stu-id="6feb1-355">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="6feb1-356">30 segundos (30.000 milisegundos)</span><span class="sxs-lookup"><span data-stu-id="6feb1-356">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="6feb1-357">Tiempo de espera para la actividad del servidor.</span><span class="sxs-lookup"><span data-stu-id="6feb1-357">Timeout for server activity.</span></span> <span data-ttu-id="6feb1-358">Si el servidor no ha enviado un mensaje en este intervalo, el cliente considera que el servidor está desconectado y desencadena el evento `Closed` (`onclose` en JavaScript).</span><span class="sxs-lookup"><span data-stu-id="6feb1-358">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="6feb1-359">Este valor debe ser lo suficientemente grande como para que se envíe un mensaje ping desde el servidor **y** el cliente lo reciba dentro del intervalo de tiempo de espera.</span><span class="sxs-lookup"><span data-stu-id="6feb1-359">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="6feb1-360">El valor recomendado es un número al menos el doble del valor `KeepAliveInterval` del servidor para permitir que lleguen los ping.</span><span class="sxs-lookup"><span data-stu-id="6feb1-360">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="6feb1-361">15 segundos</span><span class="sxs-lookup"><span data-stu-id="6feb1-361">15 seconds</span></span> | <span data-ttu-id="6feb1-362">Tiempo de espera para el protocolo de enlace inicial del servidor.</span><span class="sxs-lookup"><span data-stu-id="6feb1-362">Timeout for initial server handshake.</span></span> <span data-ttu-id="6feb1-363">Si el servidor no envía una respuesta de protocolo de enlace en este intervalo, el cliente cancela el protocolo de enlace y desencadena el evento `Closed` (`onclose` en JavaScript).</span><span class="sxs-lookup"><span data-stu-id="6feb1-363">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="6feb1-364">Se trata de una configuración avanzada que solo se debe modificar si se producen errores de tiempo de espera del Protocolo de enlace debido a una latencia de red grave.</span><span class="sxs-lookup"><span data-stu-id="6feb1-364">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="6feb1-365">Para más información sobre el proceso de enlace, consulte la [especificación del protocolo signalr Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="6feb1-365">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

<span data-ttu-id="6feb1-366">En el cliente de .NET, los valores de tiempo de espera se especifican como valores `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="6feb1-366">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="6feb1-367">JavaScript</span><span class="sxs-lookup"><span data-stu-id="6feb1-367">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="6feb1-368">Opción</span><span class="sxs-lookup"><span data-stu-id="6feb1-368">Option</span></span> | <span data-ttu-id="6feb1-369">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="6feb1-369">Default value</span></span> | <span data-ttu-id="6feb1-370">Descripción</span><span class="sxs-lookup"><span data-stu-id="6feb1-370">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="6feb1-371">30 segundos (30.000 milisegundos)</span><span class="sxs-lookup"><span data-stu-id="6feb1-371">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="6feb1-372">Tiempo de espera para la actividad del servidor.</span><span class="sxs-lookup"><span data-stu-id="6feb1-372">Timeout for server activity.</span></span> <span data-ttu-id="6feb1-373">Si el servidor no ha enviado un mensaje en este intervalo, el cliente considera que el servidor está desconectado y desencadena el evento `onclose`.</span><span class="sxs-lookup"><span data-stu-id="6feb1-373">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="6feb1-374">Este valor debe ser lo suficientemente grande como para que se envíe un mensaje ping desde el servidor **y** el cliente lo reciba dentro del intervalo de tiempo de espera.</span><span class="sxs-lookup"><span data-stu-id="6feb1-374">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="6feb1-375">El valor recomendado es un número al menos el doble del valor `KeepAliveInterval` del servidor para permitir que lleguen los ping.</span><span class="sxs-lookup"><span data-stu-id="6feb1-375">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="6feb1-376">Java</span><span class="sxs-lookup"><span data-stu-id="6feb1-376">Java</span></span>](#tab/java)

| <span data-ttu-id="6feb1-377">Opción</span><span class="sxs-lookup"><span data-stu-id="6feb1-377">Option</span></span> | <span data-ttu-id="6feb1-378">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="6feb1-378">Default value</span></span> | <span data-ttu-id="6feb1-379">Descripción</span><span class="sxs-lookup"><span data-stu-id="6feb1-379">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="6feb1-380">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="6feb1-380">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="6feb1-381">30 segundos (30.000 milisegundos)</span><span class="sxs-lookup"><span data-stu-id="6feb1-381">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="6feb1-382">Tiempo de espera para la actividad del servidor.</span><span class="sxs-lookup"><span data-stu-id="6feb1-382">Timeout for server activity.</span></span> <span data-ttu-id="6feb1-383">Si el servidor no ha enviado un mensaje en este intervalo, el cliente considera que el servidor está desconectado y desencadena el evento `onClose`.</span><span class="sxs-lookup"><span data-stu-id="6feb1-383">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="6feb1-384">Este valor debe ser lo suficientemente grande como para que se envíe un mensaje ping desde el servidor **y** el cliente lo reciba dentro del intervalo de tiempo de espera.</span><span class="sxs-lookup"><span data-stu-id="6feb1-384">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="6feb1-385">El valor recomendado es un número al menos el doble del valor `KeepAliveInterval` del servidor, para permitir que llegue el tiempo para que lleguen los ping.</span><span class="sxs-lookup"><span data-stu-id="6feb1-385">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="6feb1-386">15 segundos</span><span class="sxs-lookup"><span data-stu-id="6feb1-386">15 seconds</span></span> | <span data-ttu-id="6feb1-387">Tiempo de espera para el protocolo de enlace inicial del servidor.</span><span class="sxs-lookup"><span data-stu-id="6feb1-387">Timeout for initial server handshake.</span></span> <span data-ttu-id="6feb1-388">Si el servidor no envía una respuesta de protocolo de enlace en este intervalo, el cliente cancela el protocolo de enlace y desencadena el evento `onClose`.</span><span class="sxs-lookup"><span data-stu-id="6feb1-388">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="6feb1-389">Se trata de una configuración avanzada que solo se debe modificar si se producen errores de tiempo de espera del Protocolo de enlace debido a una latencia de red grave.</span><span class="sxs-lookup"><span data-stu-id="6feb1-389">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="6feb1-390">Para más información sobre el proceso de enlace, consulte la [especificación del protocolo signalr Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="6feb1-390">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

::: moniker-end

---

### <a name="configure-additional-options"></a><span data-ttu-id="6feb1-391">Configurar opciones adicionales</span><span class="sxs-lookup"><span data-stu-id="6feb1-391">Configure additional options</span></span>

<span data-ttu-id="6feb1-392">Se pueden configurar opciones adicionales en el método `WithUrl` (`withUrl` en JavaScript) en `HubConnectionBuilder` o en las diversas API de configuración del `HttpHubConnectionBuilder` en el cliente de Java:</span><span class="sxs-lookup"><span data-stu-id="6feb1-392">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="nettabdotnet"></a>[<span data-ttu-id="6feb1-393">.NET</span><span class="sxs-lookup"><span data-stu-id="6feb1-393">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="6feb1-394">Opción de .NET</span><span class="sxs-lookup"><span data-stu-id="6feb1-394">.NET Option</span></span> |  <span data-ttu-id="6feb1-395">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="6feb1-395">Default value</span></span> | <span data-ttu-id="6feb1-396">Descripción</span><span class="sxs-lookup"><span data-stu-id="6feb1-396">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="6feb1-397">Función que devuelve una cadena que se proporciona como un token de autenticación de portador en solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="6feb1-397">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="6feb1-398">Establezca este valor en `true` para omitir el paso de negociación.</span><span class="sxs-lookup"><span data-stu-id="6feb1-398">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="6feb1-399">**Solo se admite cuando el transporte de WebSockets es el único transporte habilitado**.</span><span class="sxs-lookup"><span data-stu-id="6feb1-399">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="6feb1-400">Esta configuración no se puede habilitar cuando se usa el servicio Azure Signalr.</span><span class="sxs-lookup"><span data-stu-id="6feb1-400">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="6feb1-401">Empty</span><span class="sxs-lookup"><span data-stu-id="6feb1-401">Empty</span></span> | <span data-ttu-id="6feb1-402">Colección de certificados TLS que se enviarán a las solicitudes de autenticación.</span><span class="sxs-lookup"><span data-stu-id="6feb1-402">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="6feb1-403">Empty</span><span class="sxs-lookup"><span data-stu-id="6feb1-403">Empty</span></span> | <span data-ttu-id="6feb1-404">Colección de cookies HTTP que se enviarán con cada solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="6feb1-404">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="6feb1-405">Empty</span><span class="sxs-lookup"><span data-stu-id="6feb1-405">Empty</span></span> | <span data-ttu-id="6feb1-406">Credenciales que se van a enviar con cada solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="6feb1-406">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="6feb1-407">5 segundos</span><span class="sxs-lookup"><span data-stu-id="6feb1-407">5 seconds</span></span> | <span data-ttu-id="6feb1-408">Solo WebSockets.</span><span class="sxs-lookup"><span data-stu-id="6feb1-408">WebSockets only.</span></span> <span data-ttu-id="6feb1-409">Cantidad máxima de tiempo que el cliente espera después de cerrarse para que el servidor confirme la solicitud de cierre.</span><span class="sxs-lookup"><span data-stu-id="6feb1-409">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="6feb1-410">Si el servidor no reconoce el cierre dentro de este tiempo, el cliente se desconecta.</span><span class="sxs-lookup"><span data-stu-id="6feb1-410">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="6feb1-411">Empty</span><span class="sxs-lookup"><span data-stu-id="6feb1-411">Empty</span></span> | <span data-ttu-id="6feb1-412">Asignación de encabezados HTTP adicionales que se van a enviar con cada solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="6feb1-412">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="6feb1-413">Delegado que se puede utilizar para configurar o reemplazar el `HttpMessageHandler` que se usa para enviar solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="6feb1-413">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="6feb1-414">No se usa para las conexiones WebSocket.</span><span class="sxs-lookup"><span data-stu-id="6feb1-414">Not used for WebSocket connections.</span></span> <span data-ttu-id="6feb1-415">Este delegado debe devolver un valor distinto de NULL y recibe el valor predeterminado como parámetro.</span><span class="sxs-lookup"><span data-stu-id="6feb1-415">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="6feb1-416">Modifique la configuración de ese valor predeterminado y devuelva o devuelva una nueva instancia de `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="6feb1-416">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="6feb1-417">**Al reemplazar el controlador, asegúrese de copiar la configuración que desea conservar del controlador proporcionado; de lo contrario, las opciones configuradas (como cookies y encabezados) no se aplicarán al nuevo controlador.**</span><span class="sxs-lookup"><span data-stu-id="6feb1-417">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="6feb1-418">Proxy HTTP que se va a usar al enviar solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="6feb1-418">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="6feb1-419">Establezca este valor booleano para enviar las credenciales predeterminadas para las solicitudes HTTP y WebSockets.</span><span class="sxs-lookup"><span data-stu-id="6feb1-419">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="6feb1-420">Esto habilita el uso de la autenticación de Windows.</span><span class="sxs-lookup"><span data-stu-id="6feb1-420">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="6feb1-421">Delegado que se puede usar para configurar opciones adicionales de WebSocket.</span><span class="sxs-lookup"><span data-stu-id="6feb1-421">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="6feb1-422">Recibe una instancia de [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) que se puede usar para configurar las opciones.</span><span class="sxs-lookup"><span data-stu-id="6feb1-422">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="6feb1-423">JavaScript</span><span class="sxs-lookup"><span data-stu-id="6feb1-423">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="6feb1-424">Opción de JavaScript</span><span class="sxs-lookup"><span data-stu-id="6feb1-424">JavaScript Option</span></span> | <span data-ttu-id="6feb1-425">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="6feb1-425">Default Value</span></span> | <span data-ttu-id="6feb1-426">Descripción</span><span class="sxs-lookup"><span data-stu-id="6feb1-426">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="6feb1-427">Función que devuelve una cadena que se proporciona como un token de autenticación de portador en solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="6feb1-427">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="6feb1-428">Establezca este valor en `true` para omitir el paso de negociación.</span><span class="sxs-lookup"><span data-stu-id="6feb1-428">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="6feb1-429">**Solo se admite cuando el transporte de WebSockets es el único transporte habilitado**.</span><span class="sxs-lookup"><span data-stu-id="6feb1-429">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="6feb1-430">Esta configuración no se puede habilitar cuando se usa el servicio Azure Signalr.</span><span class="sxs-lookup"><span data-stu-id="6feb1-430">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="6feb1-431">Java</span><span class="sxs-lookup"><span data-stu-id="6feb1-431">Java</span></span>](#tab/java)

| <span data-ttu-id="6feb1-432">Opción de Java</span><span class="sxs-lookup"><span data-stu-id="6feb1-432">Java Option</span></span> | <span data-ttu-id="6feb1-433">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="6feb1-433">Default Value</span></span> | <span data-ttu-id="6feb1-434">Descripción</span><span class="sxs-lookup"><span data-stu-id="6feb1-434">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="6feb1-435">Función que devuelve una cadena que se proporciona como un token de autenticación de portador en solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="6feb1-435">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="6feb1-436">Establezca este valor en `true` para omitir el paso de negociación.</span><span class="sxs-lookup"><span data-stu-id="6feb1-436">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="6feb1-437">**Solo se admite cuando el transporte de WebSockets es el único transporte habilitado**.</span><span class="sxs-lookup"><span data-stu-id="6feb1-437">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="6feb1-438">Esta configuración no se puede habilitar cuando se usa el servicio Azure Signalr.</span><span class="sxs-lookup"><span data-stu-id="6feb1-438">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="6feb1-439">`withHeader` `withHeaders`</span><span class="sxs-lookup"><span data-stu-id="6feb1-439">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="6feb1-440">Empty</span><span class="sxs-lookup"><span data-stu-id="6feb1-440">Empty</span></span> | <span data-ttu-id="6feb1-441">Asignación de encabezados HTTP adicionales que se van a enviar con cada solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="6feb1-441">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="6feb1-442">En el cliente .NET, estas opciones se pueden modificar mediante el delegado de opciones proporcionado a `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="6feb1-442">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="6feb1-443">En el cliente de JavaScript, estas opciones se pueden proporcionar en un objeto de JavaScript proporcionado a `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="6feb1-443">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="6feb1-444">En el cliente de Java, estas opciones se pueden configurar con los métodos del `HttpHubConnectionBuilder` devueltos por `HubConnectionBuilder.create("HUB URL")`</span><span class="sxs-lookup"><span data-stu-id="6feb1-444">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="6feb1-445">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="6feb1-445">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
