---
title: Configuración de ASP.NET Core SignalR
author: bradygaster
description: Obtenga información sobre cómo configurar aplicaciones de ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 09/06/2018
uid: signalr/configuration
ms.openlocfilehash: 06e86921c65297e93dcd8954ba4983d1577bb615
ms.sourcegitcommit: ca5f03210bedc61c6639a734ae5674bfe095dee8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2019
ms.locfileid: "55073158"
---
# <a name="aspnet-core-signalr-configuration"></a><span data-ttu-id="fbf45-103">Configuración de ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="fbf45-103">ASP.NET Core SignalR configuration</span></span>

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="fbf45-104">Opciones de serialización de JSON/MessagePack</span><span class="sxs-lookup"><span data-stu-id="fbf45-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="fbf45-105">SignalR de ASP.NET Core admite dos protocolos para la codificación de mensajes: [JSON](https://www.json.org/) y [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="fbf45-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="fbf45-106">Cada protocolo tiene opciones de configuración de serialización.</span><span class="sxs-lookup"><span data-stu-id="fbf45-106">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="fbf45-107">Serialización de JSON se puede configurar en el servidor mediante el [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) método de extensión, que se puede agregar después [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) en su `Startup.ConfigureServices` método.</span><span class="sxs-lookup"><span data-stu-id="fbf45-107">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="fbf45-108">El `AddJsonProtocol` método toma un delegado que recibe un `options` objeto.</span><span class="sxs-lookup"><span data-stu-id="fbf45-108">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="fbf45-109">El [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) propiedad en ese objeto es un JSON.NET `JsonSerializerSettings` objeto que puede usarse para configurar la serialización de argumentos y valores devueltos.</span><span class="sxs-lookup"><span data-stu-id="fbf45-109">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="fbf45-110">Consulte la [documentación JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm) para obtener más detalles.</span><span class="sxs-lookup"><span data-stu-id="fbf45-110">See the [JSON.NET Documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm) for more details.</span></span>

<span data-ttu-id="fbf45-111">Por ejemplo, para configurar el serializador para usar nombres de propiedad "PascalCase", en lugar de los nombres predeterminados "camelCase", use el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="fbf45-111">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code:</span></span>

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver = 
        new DefaultContractResolver();
    });
```

<span data-ttu-id="fbf45-112">En el cliente. NET, la misma `AddJsonProtocol` existe en el método de extensión [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="fbf45-112">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="fbf45-113">El `Microsoft.Extensions.DependencyInjection` se debe importar el espacio de nombres para resolver el método de extensión:</span><span class="sxs-lookup"><span data-stu-id="fbf45-113">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="fbf45-114">No es posible configurar la serialización de JSON en el cliente de JavaScript en este momento.</span><span class="sxs-lookup"><span data-stu-id="fbf45-114">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="fbf45-115">Opciones de serialización MessagePack</span><span class="sxs-lookup"><span data-stu-id="fbf45-115">MessagePack serialization options</span></span>

<span data-ttu-id="fbf45-116">MessagePack serialización se puede configurar si se proporciona un delegado para el [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) llamar.</span><span class="sxs-lookup"><span data-stu-id="fbf45-116">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="fbf45-117">Consulte [MessagePack en SignalR](xref:signalr/messagepackhubprotocol) para obtener más detalles.</span><span class="sxs-lookup"><span data-stu-id="fbf45-117">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="fbf45-118">No es posible configurar la serialización de MessagePack en el cliente de JavaScript en este momento.</span><span class="sxs-lookup"><span data-stu-id="fbf45-118">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="fbf45-119">Configurar opciones de servidor</span><span class="sxs-lookup"><span data-stu-id="fbf45-119">Configure server options</span></span>

<span data-ttu-id="fbf45-120">En la tabla siguiente se describe las opciones para configurar los concentradores de SignalR:</span><span class="sxs-lookup"><span data-stu-id="fbf45-120">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="fbf45-121">Opción</span><span class="sxs-lookup"><span data-stu-id="fbf45-121">Option</span></span> | <span data-ttu-id="fbf45-122">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="fbf45-122">Default Value</span></span> | <span data-ttu-id="fbf45-123">Descripción</span><span class="sxs-lookup"><span data-stu-id="fbf45-123">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="fbf45-124">30 segundos</span><span class="sxs-lookup"><span data-stu-id="fbf45-124">30 seconds</span></span> | <span data-ttu-id="fbf45-125">El servidor tendrá en cuenta el cliente desconectado si no ha recibido un mensaje (incluido keep-alive) en este intervalo.</span><span class="sxs-lookup"><span data-stu-id="fbf45-125">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="fbf45-126">Es posible que tarden más de este intervalo de tiempo de espera para el cliente realmente marcarse desconectado debido a su implementación.</span><span class="sxs-lookup"><span data-stu-id="fbf45-126">It is possible for it to take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="fbf45-127">El valor recomendado es doble el `KeepAliveInterval` valor.</span><span class="sxs-lookup"><span data-stu-id="fbf45-127">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="fbf45-128">15 segundos</span><span class="sxs-lookup"><span data-stu-id="fbf45-128">15 seconds</span></span> | <span data-ttu-id="fbf45-129">Si el cliente no envía un mensaje de protocolo de enlace inicial dentro de este intervalo de tiempo, se cierra la conexión.</span><span class="sxs-lookup"><span data-stu-id="fbf45-129">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="fbf45-130">Se trata de una opción avanzada que sólo debería modificarse si se producen errores de tiempo de espera del protocolo de enlace debido a la latencia de red graves.</span><span class="sxs-lookup"><span data-stu-id="fbf45-130">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="fbf45-131">Para obtener más detalles sobre el proceso de negociación, consulte el [especificación del protocolo SignalR Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="fbf45-131">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="fbf45-132">15 segundos</span><span class="sxs-lookup"><span data-stu-id="fbf45-132">15 seconds</span></span> | <span data-ttu-id="fbf45-133">Si el servidor no ha enviado un mensaje dentro de este intervalo, se envía automáticamente un mensaje ping para mantener abierta la conexión.</span><span class="sxs-lookup"><span data-stu-id="fbf45-133">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="fbf45-134">Al cambiar `KeepAliveInterval`, cambie el `ServerTimeout` / `serverTimeoutInMilliseconds` configuración del cliente.</span><span class="sxs-lookup"><span data-stu-id="fbf45-134">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="fbf45-135">Recomendado `ServerTimeout` / `serverTimeoutInMilliseconds` valor es doble el `KeepAliveInterval` valor.</span><span class="sxs-lookup"><span data-stu-id="fbf45-135">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="fbf45-136">Todos los protocolos instalados</span><span class="sxs-lookup"><span data-stu-id="fbf45-136">All installed protocols</span></span> | <span data-ttu-id="fbf45-137">Protocolos admitidos por este concentrador.</span><span class="sxs-lookup"><span data-stu-id="fbf45-137">Protocols supported by this hub.</span></span> <span data-ttu-id="fbf45-138">De forma predeterminada, se permiten todos los protocolos registrados en el servidor, pero se pueden quitar protocolos de esta lista para deshabilitar los protocolos específicos para los concentradores individuales.</span><span class="sxs-lookup"><span data-stu-id="fbf45-138">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="fbf45-139">Si `true`detallados se devuelven los mensajes de excepción a los clientes cuando se produce una excepción en un método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="fbf45-139">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="fbf45-140">El valor predeterminado es `false`, ya que estos mensajes de excepción pueden contener información confidencial.</span><span class="sxs-lookup"><span data-stu-id="fbf45-140">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

<span data-ttu-id="fbf45-141">Se pueden configurar opciones para todos los centros proporcionando un delegado de opciones para la `AddSignalR` llamar `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="fbf45-141">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="fbf45-142">Opciones para un único centro de invalidan las opciones globales de `AddSignalR` y pueden configurarse mediante [AddHubOptions\<T >](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):</span><span class="sxs-lookup"><span data-stu-id="fbf45-142">Options for a single hub override the global options provided in `AddSignalR` and can be configured using [AddHubOptions\<T>](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

<span data-ttu-id="fbf45-143">Use `HttpConnectionDispatcherOptions` para configurar opciones avanzadas relacionadas con transportes y administración de búfer de memoria.</span><span class="sxs-lookup"><span data-stu-id="fbf45-143">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="fbf45-144">Estas opciones se configuran pasando un delegado para [MapHub\<T >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub).</span><span class="sxs-lookup"><span data-stu-id="fbf45-144">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub).</span></span>

| <span data-ttu-id="fbf45-145">Opción</span><span class="sxs-lookup"><span data-stu-id="fbf45-145">Option</span></span> | <span data-ttu-id="fbf45-146">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="fbf45-146">Default Value</span></span> | <span data-ttu-id="fbf45-147">Descripción</span><span class="sxs-lookup"><span data-stu-id="fbf45-147">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="fbf45-148">32 KB</span><span class="sxs-lookup"><span data-stu-id="fbf45-148">32 KB</span></span> | <span data-ttu-id="fbf45-149">El número máximo de bytes recibidos del cliente que los búferes del servidor.</span><span class="sxs-lookup"><span data-stu-id="fbf45-149">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="fbf45-150">Al aumentar este valor permite que el servidor recibir los mensajes más grandes, pero puede repercutir negativamente en el consumo de memoria.</span><span class="sxs-lookup"><span data-stu-id="fbf45-150">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="fbf45-151">Recopilar automáticamente los datos de la `Authorize` atributos aplicados a la clase de concentrador.</span><span class="sxs-lookup"><span data-stu-id="fbf45-151">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="fbf45-152">Una lista de [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) los objetos utilizados para determinar si un cliente está autorizado para conectarse al concentrador.</span><span class="sxs-lookup"><span data-stu-id="fbf45-152">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="fbf45-153">32 KB</span><span class="sxs-lookup"><span data-stu-id="fbf45-153">32 KB</span></span> | <span data-ttu-id="fbf45-154">El número máximo de bytes enviados por la aplicación que los búferes del servidor.</span><span class="sxs-lookup"><span data-stu-id="fbf45-154">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="fbf45-155">Al aumentar este valor permite que el servidor enviar los mensajes más grandes, pero puede repercutir negativamente en el consumo de memoria.</span><span class="sxs-lookup"><span data-stu-id="fbf45-155">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="fbf45-156">Se habilitan todos los transportes.</span><span class="sxs-lookup"><span data-stu-id="fbf45-156">All Transports are enabled.</span></span> | <span data-ttu-id="fbf45-157">Una máscara de bits de `HttpTransportType` valores que pueden restringir los transportes que un cliente puede usar para conectarse.</span><span class="sxs-lookup"><span data-stu-id="fbf45-157">A bitmask of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="fbf45-158">Véalo a continuación.</span><span class="sxs-lookup"><span data-stu-id="fbf45-158">See below.</span></span> | <span data-ttu-id="fbf45-159">Opciones adicionales específicas para el transporte de sondeo largo.</span><span class="sxs-lookup"><span data-stu-id="fbf45-159">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="fbf45-160">Véalo a continuación.</span><span class="sxs-lookup"><span data-stu-id="fbf45-160">See below.</span></span> | <span data-ttu-id="fbf45-161">Opciones adicionales específicas para el transporte de WebSockets.</span><span class="sxs-lookup"><span data-stu-id="fbf45-161">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="fbf45-162">El transporte de sondeo largo tiene opciones adicionales que se pueden configurar mediante el `LongPolling` propiedad:</span><span class="sxs-lookup"><span data-stu-id="fbf45-162">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="fbf45-163">Opción</span><span class="sxs-lookup"><span data-stu-id="fbf45-163">Option</span></span> | <span data-ttu-id="fbf45-164">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="fbf45-164">Default Value</span></span> | <span data-ttu-id="fbf45-165">Descripción</span><span class="sxs-lookup"><span data-stu-id="fbf45-165">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="fbf45-166">90 segundos</span><span class="sxs-lookup"><span data-stu-id="fbf45-166">90 seconds</span></span> | <span data-ttu-id="fbf45-167">La cantidad máxima de tiempo que el servidor espera para que un mensaje para enviárselo al cliente antes de finalizar una solicitud de sondeo única.</span><span class="sxs-lookup"><span data-stu-id="fbf45-167">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="fbf45-168">Al disminuir este valor hace que el cliente emitir solicitudes de sondeo de nuevo con más frecuencia.</span><span class="sxs-lookup"><span data-stu-id="fbf45-168">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="fbf45-169">El transporte de WebSocket tiene opciones adicionales que se pueden configurar mediante el `WebSockets` propiedad:</span><span class="sxs-lookup"><span data-stu-id="fbf45-169">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="fbf45-170">Opción</span><span class="sxs-lookup"><span data-stu-id="fbf45-170">Option</span></span> | <span data-ttu-id="fbf45-171">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="fbf45-171">Default Value</span></span> | <span data-ttu-id="fbf45-172">Descripción</span><span class="sxs-lookup"><span data-stu-id="fbf45-172">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="fbf45-173">5 segundos</span><span class="sxs-lookup"><span data-stu-id="fbf45-173">5 seconds</span></span> | <span data-ttu-id="fbf45-174">Después de cerrar el servidor, si se produce un error en el cliente cerrar dentro de este intervalo de tiempo, se termina la conexión.</span><span class="sxs-lookup"><span data-stu-id="fbf45-174">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="fbf45-175">Un delegado que puede usarse para establecer el `Sec-WebSocket-Protocol` encabezado en un valor personalizado.</span><span class="sxs-lookup"><span data-stu-id="fbf45-175">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="fbf45-176">El delegado recibe los valores solicitados por el cliente como entrada y se espera que devuelva el valor deseado.</span><span class="sxs-lookup"><span data-stu-id="fbf45-176">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="fbf45-177">Configurar las opciones de cliente</span><span class="sxs-lookup"><span data-stu-id="fbf45-177">Configure client options</span></span>

<span data-ttu-id="fbf45-178">Se pueden configurar las opciones de cliente en el `HubConnectionBuilder` tipo (disponible en los clientes de .NET y JavaScript), así como en el `HubConnection` propio.</span><span class="sxs-lookup"><span data-stu-id="fbf45-178">Client options can be configured on the `HubConnectionBuilder` type (available in both .NET and JavaScript clients), as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="fbf45-179">Configurar el registro</span><span class="sxs-lookup"><span data-stu-id="fbf45-179">Configure logging</span></span>

<span data-ttu-id="fbf45-180">El registro está configurado en el cliente de .NET mediante el `ConfigureLogging` método.</span><span class="sxs-lookup"><span data-stu-id="fbf45-180">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="fbf45-181">Registro de proveedores y los filtros se puede registrar en la misma manera, tal como están en el servidor.</span><span class="sxs-lookup"><span data-stu-id="fbf45-181">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="fbf45-182">Consulte la [registro en ASP.NET Core](xref:fundamentals/logging/index) documentación para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="fbf45-182">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="fbf45-183">Con el fin de registrar los proveedores de registro, debe instalar los paquetes necesarios.</span><span class="sxs-lookup"><span data-stu-id="fbf45-183">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="fbf45-184">Consulte la [proveedores de registro integrados](xref:fundamentals/logging/index#built-in-logging-providers) sección de la documentación para obtener una lista completa.</span><span class="sxs-lookup"><span data-stu-id="fbf45-184">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="fbf45-185">Por ejemplo, para habilitar el registro de consola, instale el `Microsoft.Extensions.Logging.Console` paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="fbf45-185">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="fbf45-186">Llame a la `AddConsole` método de extensión:</span><span class="sxs-lookup"><span data-stu-id="fbf45-186">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="fbf45-187">En el cliente de JavaScript, un proceso similar `configureLogging` método existe.</span><span class="sxs-lookup"><span data-stu-id="fbf45-187">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="fbf45-188">Proporcione un `LogLevel` valor que indica el nivel mínimo de los mensajes de registro para generar.</span><span class="sxs-lookup"><span data-stu-id="fbf45-188">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="fbf45-189">Los registros se escriben en la ventana de consola del explorador.</span><span class="sxs-lookup"><span data-stu-id="fbf45-189">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> <span data-ttu-id="fbf45-190">Para deshabilitar el registro por completo, especifique `signalR.LogLevel.None` en el `configureLogging` método.</span><span class="sxs-lookup"><span data-stu-id="fbf45-190">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="fbf45-191">A continuación, se muestran los niveles de registro disponibles para el cliente de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="fbf45-191">Log levels available to the JavaScript client are listed below.</span></span> <span data-ttu-id="fbf45-192">Establecer el nivel de registro en uno de estos valores habilita el registro de mensajes en **o superior** ese nivel.</span><span class="sxs-lookup"><span data-stu-id="fbf45-192">Setting the log level to one of these values enables logging of messages at **or above** that level.</span></span>

| <span data-ttu-id="fbf45-193">Nivel</span><span class="sxs-lookup"><span data-stu-id="fbf45-193">Level</span></span> | <span data-ttu-id="fbf45-194">Descripción</span><span class="sxs-lookup"><span data-stu-id="fbf45-194">Description</span></span> |
| ----- | ----------- |
| `None` | <span data-ttu-id="fbf45-195">Se registra ningún mensaje.</span><span class="sxs-lookup"><span data-stu-id="fbf45-195">No messages are logged.</span></span> |
| `Critical` | <span data-ttu-id="fbf45-196">Mensajes que indican un error en toda la aplicación.</span><span class="sxs-lookup"><span data-stu-id="fbf45-196">Messages that indicate a failure in the entire app.</span></span> |
| `Error` | <span data-ttu-id="fbf45-197">Mensajes que indican un error en la operación actual.</span><span class="sxs-lookup"><span data-stu-id="fbf45-197">Messages that indicate a failure in the current operation.</span></span> |
| `Warning` | <span data-ttu-id="fbf45-198">Mensajes que indican un problema grave.</span><span class="sxs-lookup"><span data-stu-id="fbf45-198">Messages that indicate a non-fatal problem.</span></span> |
| `Information` | <span data-ttu-id="fbf45-199">Mensajes informativos.</span><span class="sxs-lookup"><span data-stu-id="fbf45-199">Informational messages.</span></span> |
| `Debug` | <span data-ttu-id="fbf45-200">Mensajes de diagnóstico útiles para depurar.</span><span class="sxs-lookup"><span data-stu-id="fbf45-200">Diagnostic messages useful for debugging.</span></span> |
| `Trace` | <span data-ttu-id="fbf45-201">Mensajes de diagnóstico muy detallados diseñados para diagnosticar problemas específicos.</span><span class="sxs-lookup"><span data-stu-id="fbf45-201">Very detailed diagnostic messages designed for diagnosing specific issues.</span></span> |

### <a name="configure-allowed-transports"></a><span data-ttu-id="fbf45-202">Configurar transportes permitidos</span><span class="sxs-lookup"><span data-stu-id="fbf45-202">Configure allowed transports</span></span>

<span data-ttu-id="fbf45-203">Los transportes utilizados por SignalR se pueden configurar en el `WithUrl` llamar (`withUrl` en JavaScript).</span><span class="sxs-lookup"><span data-stu-id="fbf45-203">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="fbf45-204">Una operación OR bit a bit de los valores de `HttpTransportType` puede usarse para restringir el cliente para que solo use los transportes especificados.</span><span class="sxs-lookup"><span data-stu-id="fbf45-204">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="fbf45-205">Todos los transportes están habilitados de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="fbf45-205">All transports are enabled by default.</span></span>

<span data-ttu-id="fbf45-206">Por ejemplo, para deshabilitar el transporte de los eventos, pero permitir conexiones WebSockets y Long Polling:</span><span class="sxs-lookup"><span data-stu-id="fbf45-206">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="fbf45-207">En el cliente de JavaScript, se configuran los transportes estableciendo el `transport` campo en el objeto de opciones proporcionado para `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="fbf45-207">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

### <a name="configure-bearer-authentication"></a><span data-ttu-id="fbf45-208">Configurar la autenticación de portador</span><span class="sxs-lookup"><span data-stu-id="fbf45-208">Configure bearer authentication</span></span>

<span data-ttu-id="fbf45-209">Para proporcionar datos de autenticación junto con las solicitudes de SignalR, use el `AccessTokenProvider` opción (`accessTokenFactory` en JavaScript) para especificar una función que devuelve el token de acceso deseado.</span><span class="sxs-lookup"><span data-stu-id="fbf45-209">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="fbf45-210">En el cliente. NET, este token de acceso se pasa como un HTTP "Autenticación del portador" token (mediante el `Authorization` encabezado con un tipo de `Bearer`).</span><span class="sxs-lookup"><span data-stu-id="fbf45-210">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="fbf45-211">En el cliente de JavaScript, se usa el token de acceso como un token de portador, **excepto** en algunos casos donde las API de explorador restringir la capacidad de aplicar los encabezados (específicamente, en las solicitudes de los eventos y WebSockets).</span><span class="sxs-lookup"><span data-stu-id="fbf45-211">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="fbf45-212">En estos casos, el token de acceso se proporciona como un valor de cadena de consulta `access_token`.</span><span class="sxs-lookup"><span data-stu-id="fbf45-212">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="fbf45-213">En el cliente. NET, el `AccessTokenProvider` opción puede especificarse utilizando el delegado de opciones en `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="fbf45-213">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="fbf45-214">En el cliente de JavaScript, el token de acceso se configura estableciendo el `accessTokenFactory` campo en el objeto de opciones de `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="fbf45-214">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="fbf45-215">Configurar el tiempo de espera y las opciones de mantenimiento</span><span class="sxs-lookup"><span data-stu-id="fbf45-215">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="fbf45-216">Opciones adicionales para configurar el tiempo de espera y el comportamiento de mantenimiento están disponibles en el `HubConnection` propio objeto:</span><span class="sxs-lookup"><span data-stu-id="fbf45-216">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

| <span data-ttu-id="fbf45-217">Opción de .NET</span><span class="sxs-lookup"><span data-stu-id="fbf45-217">.NET Option</span></span> | <span data-ttu-id="fbf45-218">Opción de JavaScript</span><span class="sxs-lookup"><span data-stu-id="fbf45-218">JavaScript Option</span></span> | <span data-ttu-id="fbf45-219">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="fbf45-219">Default Value</span></span> | <span data-ttu-id="fbf45-220">Descripción</span><span class="sxs-lookup"><span data-stu-id="fbf45-220">Description</span></span> |
| ----------- | ----------------- | ------------- | ----------- |
| `ServerTimeout` | `serverTimeoutInMilliseconds` | <span data-ttu-id="fbf45-221">30 segundos (30.000 milisegundos)</span><span class="sxs-lookup"><span data-stu-id="fbf45-221">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="fbf45-222">Tiempo de espera para la actividad del servidor.</span><span class="sxs-lookup"><span data-stu-id="fbf45-222">Timeout for server activity.</span></span> <span data-ttu-id="fbf45-223">Si el servidor no ha enviado un mensaje en este intervalo, el cliente considera que las ha desconectado el servidor y los desencadenadores la `Closed` eventos (`onclose` en JavaScript).</span><span class="sxs-lookup"><span data-stu-id="fbf45-223">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="fbf45-224">Este valor debe ser lo suficientemente grande como para un mensaje ping se envíe desde el servidor **y** recibidos por el cliente dentro del intervalo de tiempo de espera.</span><span class="sxs-lookup"><span data-stu-id="fbf45-224">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="fbf45-225">El valor recomendado es de un número al menos el doble del servidor `KeepAliveInterval` valor, para dejar tiempo para pings de llegada.</span><span class="sxs-lookup"><span data-stu-id="fbf45-225">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="fbf45-226">No se puede configurar</span><span class="sxs-lookup"><span data-stu-id="fbf45-226">Not configurable</span></span> | <span data-ttu-id="fbf45-227">15 segundos</span><span class="sxs-lookup"><span data-stu-id="fbf45-227">15 seconds</span></span> | <span data-ttu-id="fbf45-228">Tiempo de espera para el protocolo de enlace de servidor inicial.</span><span class="sxs-lookup"><span data-stu-id="fbf45-228">Timeout for initial server handshake.</span></span> <span data-ttu-id="fbf45-229">Si el servidor no envía una respuesta de protocolo de enlace en este intervalo, el cliente cancela el protocolo de enlace y los desencadenadores la `Closed` eventos (`onclose` en JavaScript).</span><span class="sxs-lookup"><span data-stu-id="fbf45-229">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="fbf45-230">Se trata de una opción avanzada que sólo debería modificarse si se producen errores de tiempo de espera del protocolo de enlace debido a la latencia de red graves.</span><span class="sxs-lookup"><span data-stu-id="fbf45-230">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="fbf45-231">Para obtener más detalles sobre el proceso de negociación, consulte el [especificación del protocolo SignalR Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="fbf45-231">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

<span data-ttu-id="fbf45-232">En el cliente. NET, se especifican los valores de tiempo de espera como `TimeSpan` valores.</span><span class="sxs-lookup"><span data-stu-id="fbf45-232">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span> <span data-ttu-id="fbf45-233">En el cliente de JavaScript, los valores de tiempo de espera se especifican como un número que indica la duración en milisegundos.</span><span class="sxs-lookup"><span data-stu-id="fbf45-233">In the JavaScript client, timeout values are specified as a number indicating the duration in milliseconds.</span></span>

### <a name="configure-additional-options"></a><span data-ttu-id="fbf45-234">Configurar opciones adicionales</span><span class="sxs-lookup"><span data-stu-id="fbf45-234">Configure additional options</span></span>

<span data-ttu-id="fbf45-235">Se pueden configurar opciones adicionales en el `WithUrl` (`withUrl` en JavaScript) método `HubConnectionBuilder`:</span><span class="sxs-lookup"><span data-stu-id="fbf45-235">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder`:</span></span>

| <span data-ttu-id="fbf45-236">Opción de .NET</span><span class="sxs-lookup"><span data-stu-id="fbf45-236">.NET Option</span></span> | <span data-ttu-id="fbf45-237">Opción de JavaScript</span><span class="sxs-lookup"><span data-stu-id="fbf45-237">JavaScript Option</span></span> | <span data-ttu-id="fbf45-238">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="fbf45-238">Default Value</span></span> | <span data-ttu-id="fbf45-239">Descripción</span><span class="sxs-lookup"><span data-stu-id="fbf45-239">Description</span></span> |
| ----------- | ----------------- | ------------- | ----------- |
| `AccessTokenProvider` | `accessTokenFactory` | `null` | <span data-ttu-id="fbf45-240">Una función que devuelve una cadena que se proporciona como un token de autenticación de portador en solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="fbf45-240">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `skipNegotiation` | `false` | <span data-ttu-id="fbf45-241">Establezca esta opción en `true` para omitir el paso de negociación.</span><span class="sxs-lookup"><span data-stu-id="fbf45-241">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="fbf45-242">**Solo se admite cuando el transporte de WebSockets es el único tipo de transporte habilitado**.</span><span class="sxs-lookup"><span data-stu-id="fbf45-242">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="fbf45-243">No se puede habilitar esta configuración al usar Azure SignalR Service.</span><span class="sxs-lookup"><span data-stu-id="fbf45-243">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="fbf45-244">No se puede configurar \*</span><span class="sxs-lookup"><span data-stu-id="fbf45-244">Not configurable \*</span></span> | <span data-ttu-id="fbf45-245">Empty</span><span class="sxs-lookup"><span data-stu-id="fbf45-245">Empty</span></span> | <span data-ttu-id="fbf45-246">Una colección de certificados TLS que se envían autenticar las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="fbf45-246">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="fbf45-247">No se puede configurar \*</span><span class="sxs-lookup"><span data-stu-id="fbf45-247">Not configurable \*</span></span> | <span data-ttu-id="fbf45-248">Empty</span><span class="sxs-lookup"><span data-stu-id="fbf45-248">Empty</span></span> | <span data-ttu-id="fbf45-249">Una colección de cookies HTTP para enviar con cada solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="fbf45-249">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="fbf45-250">No se puede configurar \*</span><span class="sxs-lookup"><span data-stu-id="fbf45-250">Not configurable \*</span></span> | <span data-ttu-id="fbf45-251">Empty</span><span class="sxs-lookup"><span data-stu-id="fbf45-251">Empty</span></span> | <span data-ttu-id="fbf45-252">Credenciales que se enviará con todas las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="fbf45-252">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="fbf45-253">No se puede configurar \*</span><span class="sxs-lookup"><span data-stu-id="fbf45-253">Not configurable \*</span></span> | <span data-ttu-id="fbf45-254">5 segundos</span><span class="sxs-lookup"><span data-stu-id="fbf45-254">5 seconds</span></span> | <span data-ttu-id="fbf45-255">Solo WebSockets.</span><span class="sxs-lookup"><span data-stu-id="fbf45-255">WebSockets only.</span></span> <span data-ttu-id="fbf45-256">La cantidad máxima de tiempo de espera a que el cliente después del cierre del servidor confirmar la solicitud de cierre.</span><span class="sxs-lookup"><span data-stu-id="fbf45-256">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="fbf45-257">Si el servidor no reconoció el cierre dentro de este tiempo, el cliente se desconecta.</span><span class="sxs-lookup"><span data-stu-id="fbf45-257">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="fbf45-258">No se puede configurar \*</span><span class="sxs-lookup"><span data-stu-id="fbf45-258">Not configurable \*</span></span> | <span data-ttu-id="fbf45-259">Empty</span><span class="sxs-lookup"><span data-stu-id="fbf45-259">Empty</span></span> | <span data-ttu-id="fbf45-260">Un diccionario de encabezados HTTP adicionales que se enviará con todas las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="fbf45-260">A dictionary of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | <span data-ttu-id="fbf45-261">No se puede configurar \*</span><span class="sxs-lookup"><span data-stu-id="fbf45-261">Not configurable \*</span></span> | `null` | <span data-ttu-id="fbf45-262">Un delegado que puede usarse para configurar o reemplazar el `HttpMessageHandler` utilizado para enviar solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="fbf45-262">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="fbf45-263">No se utiliza para las conexiones de WebSocket.</span><span class="sxs-lookup"><span data-stu-id="fbf45-263">Not used for WebSocket connections.</span></span> <span data-ttu-id="fbf45-264">Este delegado debe devolver un valor distinto de null, y recibe el valor predeterminado como un parámetro.</span><span class="sxs-lookup"><span data-stu-id="fbf45-264">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="fbf45-265">Modificar la configuración en ese valor predeterminado y devolverlo o devolver un nuevo `HttpMessageHandler` instancia.</span><span class="sxs-lookup"><span data-stu-id="fbf45-265">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="fbf45-266">**Cuando Asegúrese de reemplazar el controlador copiar la configuración que desee impedir que el controlador proporcionado, en caso contrario, las opciones configuradas (por ejemplo, Cookies y encabezados) no se aplicarán al nuevo controlador.**</span><span class="sxs-lookup"><span data-stu-id="fbf45-266">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | <span data-ttu-id="fbf45-267">No se puede configurar \*</span><span class="sxs-lookup"><span data-stu-id="fbf45-267">Not configurable \*</span></span> | `null` | <span data-ttu-id="fbf45-268">Un proxy HTTP que se utilizará al enviar solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="fbf45-268">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | <span data-ttu-id="fbf45-269">No se puede configurar \*</span><span class="sxs-lookup"><span data-stu-id="fbf45-269">Not configurable \*</span></span> | `false` | <span data-ttu-id="fbf45-270">Establezca este valor booleano para enviar las credenciales predeterminadas para las solicitudes HTTP y WebSockets.</span><span class="sxs-lookup"><span data-stu-id="fbf45-270">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="fbf45-271">Esto permite el uso de autenticación de Windows.</span><span class="sxs-lookup"><span data-stu-id="fbf45-271">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | <span data-ttu-id="fbf45-272">No se puede configurar \*</span><span class="sxs-lookup"><span data-stu-id="fbf45-272">Not configurable \*</span></span> | `null` | <span data-ttu-id="fbf45-273">Un delegado que puede usarse para configurar opciones adicionales de WebSocket.</span><span class="sxs-lookup"><span data-stu-id="fbf45-273">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="fbf45-274">Recibe una instancia de [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) que puede utilizarse para configurar las opciones.</span><span class="sxs-lookup"><span data-stu-id="fbf45-274">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

<span data-ttu-id="fbf45-275">Opciones marcadas con un asterisco (\*) no son configurables en el cliente de JavaScript, debido a limitaciones en el Explorador de API.</span><span class="sxs-lookup"><span data-stu-id="fbf45-275">Options marked with an asterisk (\*) aren't configurable in the JavaScript client, due to limitations in browser APIs.</span></span>

<span data-ttu-id="fbf45-276">En el cliente. NET, estas opciones se pueden modificar por el delegado de las opciones proporcionado para `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="fbf45-276">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="fbf45-277">En el cliente de JavaScript, estas opciones se pueden proporcionar en un objeto de JavaScript proporcionado a `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="fbf45-277">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

## <a name="additional-resources"></a><span data-ttu-id="fbf45-278">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="fbf45-278">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
