---
title: Backplane de Redis para el escalado horizontal de ASP.NET Core SignalR
author: bradygaster
description: Aprenda a configurar un backplane de Redis para habilitar el escalado horizontal de una aplicación de SignalR de ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/redis-backplane
ms.openlocfilehash: 379d46fcaabb8eb0d04e521a5ad698229f947b7c
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963917"
---
# <a name="set-up-a-redis-backplane-for-aspnet-core-opno-locsignalr-scale-out"></a><span data-ttu-id="3253a-103">Configuración de un backplane de Redis para el escalado horizontal de ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="3253a-103">Set up a Redis backplane for ASP.NET Core SignalR scale-out</span></span>

<span data-ttu-id="3253a-104">Por [Andrew Stanton-enfermera](https://twitter.com/anurse), [Brady transgastor](https://twitter.com/bradygaster)y [Tom Dykstra](https://github.com/tdykstra),</span><span class="sxs-lookup"><span data-stu-id="3253a-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse), [Brady Gaster](https://twitter.com/bradygaster), and [Tom Dykstra](https://github.com/tdykstra),</span></span>

<span data-ttu-id="3253a-105">En este artículo se explican los aspectos específicos de SignalRde la configuración de un servidor de [Redis](https://redis.io/) para usar para escalar horizontalmente una aplicación ASP.net Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="3253a-105">This article explains SignalR-specific aspects of setting up a [Redis](https://redis.io/) server to use for scaling out an ASP.NET Core SignalR app.</span></span>

## <a name="set-up-a-redis-backplane"></a><span data-ttu-id="3253a-106">Configuración de un backplane de Redis</span><span class="sxs-lookup"><span data-stu-id="3253a-106">Set up a Redis backplane</span></span>

* <span data-ttu-id="3253a-107">Implementar un servidor de Redis.</span><span class="sxs-lookup"><span data-stu-id="3253a-107">Deploy a Redis server.</span></span>

  > [!IMPORTANT] 
  > <span data-ttu-id="3253a-108">Para su uso en producción, se recomienda un backplane de Redis solo cuando se ejecuta en el mismo centro de datos que la aplicación SignalR.</span><span class="sxs-lookup"><span data-stu-id="3253a-108">For production use, a Redis backplane is recommended only when it runs in the same data center as the SignalR app.</span></span> <span data-ttu-id="3253a-109">De lo contrario, la latencia de red degrada el rendimiento.</span><span class="sxs-lookup"><span data-stu-id="3253a-109">Otherwise, network latency degrades performance.</span></span> <span data-ttu-id="3253a-110">Si la aplicación de SignalR se ejecuta en la nube de Azure, se recomienda Azure SignalR Service en lugar de un backplane de Redis.</span><span class="sxs-lookup"><span data-stu-id="3253a-110">If your SignalR app is running in the Azure cloud, we recommend Azure SignalR Service instead of a Redis backplane.</span></span> <span data-ttu-id="3253a-111">Puede usar el servicio de Azure Redis Cache para entornos de desarrollo y pruebas.</span><span class="sxs-lookup"><span data-stu-id="3253a-111">You can use the Azure Redis Cache Service for development and test environments.</span></span>

  <span data-ttu-id="3253a-112">Para obtener más información, vea los siguientes recursos:</span><span class="sxs-lookup"><span data-stu-id="3253a-112">For more information, see the following resources:</span></span>

  * <xref:signalr/scale>
  * [<span data-ttu-id="3253a-113">Documentación de Redis</span><span class="sxs-lookup"><span data-stu-id="3253a-113">Redis documentation</span></span>](https://redis.io/)
  * [<span data-ttu-id="3253a-114">Azure Redis Cache documentación</span><span class="sxs-lookup"><span data-stu-id="3253a-114">Azure Redis Cache documentation</span></span>](https://docs.microsoft.com/azure/redis-cache/)

::: moniker range="= aspnetcore-2.1"

* <span data-ttu-id="3253a-115">En la aplicación SignalR, instale el paquete NuGet `Microsoft.AspNetCore.SignalR.Redis`.</span><span class="sxs-lookup"><span data-stu-id="3253a-115">In the SignalR app, install the `Microsoft.AspNetCore.SignalR.Redis` NuGet package.</span></span> <span data-ttu-id="3253a-116">(También hay un paquete de `Microsoft.AspNetCore.SignalR.StackExchangeRedis`, pero el otro es para ASP.NET Core 2,2 y versiones posteriores).</span><span class="sxs-lookup"><span data-stu-id="3253a-116">(There is also a `Microsoft.AspNetCore.SignalR.StackExchangeRedis` package, but that one is for ASP.NET Core 2.2 and later.)</span></span>

* <span data-ttu-id="3253a-117">En el método `Startup.ConfigureServices`, llame a `AddRedis` después `AddSignalR`:</span><span class="sxs-lookup"><span data-stu-id="3253a-117">In the `Startup.ConfigureServices` method, call `AddRedis` after `AddSignalR`:</span></span>

  ```csharp
  services.AddSignalR().AddRedis("<your_Redis_connection_string>");
  ```

* <span data-ttu-id="3253a-118">Configure las opciones según sea necesario:</span><span class="sxs-lookup"><span data-stu-id="3253a-118">Configure options as needed:</span></span>
 
  <span data-ttu-id="3253a-119">La mayoría de las opciones se pueden establecer en la cadena de conexión o en el objeto [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) .</span><span class="sxs-lookup"><span data-stu-id="3253a-119">Most options can be set in the connection string or in the [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) object.</span></span> <span data-ttu-id="3253a-120">Las opciones especificadas en `ConfigurationOptions` invalidan las definidas en la cadena de conexión.</span><span class="sxs-lookup"><span data-stu-id="3253a-120">Options specified in `ConfigurationOptions` override the ones set in the connection string.</span></span>

  <span data-ttu-id="3253a-121">En el ejemplo siguiente se muestra cómo establecer opciones en el objeto `ConfigurationOptions`.</span><span class="sxs-lookup"><span data-stu-id="3253a-121">The following example shows how to set options in the `ConfigurationOptions` object.</span></span> <span data-ttu-id="3253a-122">En este ejemplo se agrega un prefijo de canal para que varias aplicaciones puedan compartir la misma instancia de Redis, tal y como se explica en el paso siguiente.</span><span class="sxs-lookup"><span data-stu-id="3253a-122">This example adds a channel prefix so that multiple apps can share the same Redis instance, as explained in the following step.</span></span>

  ```csharp
  services.AddSignalR()
    .AddRedis(connectionString, options => {
        options.Configuration.ChannelPrefix = "MyApp";
    });
  ```

  <span data-ttu-id="3253a-123">En el código anterior, `options.Configuration` se inicializa con lo que se haya especificado en la cadena de conexión.</span><span class="sxs-lookup"><span data-stu-id="3253a-123">In the preceding code, `options.Configuration` is initialized with whatever was specified in the connection string.</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.1"

* <span data-ttu-id="3253a-124">En la aplicación SignalR, instale uno de los siguientes paquetes NuGet:</span><span class="sxs-lookup"><span data-stu-id="3253a-124">In the SignalR app, install one of the following NuGet packages:</span></span>

  * <span data-ttu-id="3253a-125">`Microsoft.AspNetCore.SignalR.StackExchangeRedis`: depende de StackExchange. Redis 2. X.X.</span><span class="sxs-lookup"><span data-stu-id="3253a-125">`Microsoft.AspNetCore.SignalR.StackExchangeRedis` - Depends on StackExchange.Redis 2.X.X.</span></span> <span data-ttu-id="3253a-126">Este es el paquete recomendado para ASP.NET Core 2,2 y versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="3253a-126">This is the recommended package for ASP.NET Core 2.2 and later.</span></span>
  * <span data-ttu-id="3253a-127">`Microsoft.AspNetCore.SignalR.Redis`: depende de StackExchange. Redis 1. X.X.</span><span class="sxs-lookup"><span data-stu-id="3253a-127">`Microsoft.AspNetCore.SignalR.Redis` - Depends on StackExchange.Redis 1.X.X.</span></span> <span data-ttu-id="3253a-128">Este paquete no se enviará en ASP.NET Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="3253a-128">This package will not be shipping in ASP.NET Core 3.0.</span></span>

* <span data-ttu-id="3253a-129">En el método `Startup.ConfigureServices`, llame a `AddStackExchangeRedis` después `AddSignalR`:</span><span class="sxs-lookup"><span data-stu-id="3253a-129">In the `Startup.ConfigureServices` method, call `AddStackExchangeRedis` after `AddSignalR`:</span></span>

  ```csharp
  services.AddSignalR().AddStackExchangeRedis("<your_Redis_connection_string>");
  ```

* <span data-ttu-id="3253a-130">Configure las opciones según sea necesario:</span><span class="sxs-lookup"><span data-stu-id="3253a-130">Configure options as needed:</span></span>
 
  <span data-ttu-id="3253a-131">La mayoría de las opciones se pueden establecer en la cadena de conexión o en el objeto [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) .</span><span class="sxs-lookup"><span data-stu-id="3253a-131">Most options can be set in the connection string or in the [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) object.</span></span> <span data-ttu-id="3253a-132">Las opciones especificadas en `ConfigurationOptions` invalidan las definidas en la cadena de conexión.</span><span class="sxs-lookup"><span data-stu-id="3253a-132">Options specified in `ConfigurationOptions` override the ones set in the connection string.</span></span>

  <span data-ttu-id="3253a-133">En el ejemplo siguiente se muestra cómo establecer opciones en el objeto `ConfigurationOptions`.</span><span class="sxs-lookup"><span data-stu-id="3253a-133">The following example shows how to set options in the `ConfigurationOptions` object.</span></span> <span data-ttu-id="3253a-134">En este ejemplo se agrega un prefijo de canal para que varias aplicaciones puedan compartir la misma instancia de Redis, tal y como se explica en el paso siguiente.</span><span class="sxs-lookup"><span data-stu-id="3253a-134">This example adds a channel prefix so that multiple apps can share the same Redis instance, as explained in the following step.</span></span>

  ```csharp
  services.AddSignalR()
    .AddStackExchangeRedis(connectionString, options => {
        options.Configuration.ChannelPrefix = "MyApp";
    });
  ```

  <span data-ttu-id="3253a-135">En el código anterior, `options.Configuration` se inicializa con lo que se haya especificado en la cadena de conexión.</span><span class="sxs-lookup"><span data-stu-id="3253a-135">In the preceding code, `options.Configuration` is initialized with whatever was specified in the connection string.</span></span>

  <span data-ttu-id="3253a-136">Para obtener información sobre las opciones de Redis, consulte la [documentación de StackExchange Redis](https://stackexchange.github.io/StackExchange.Redis/Configuration.html).</span><span class="sxs-lookup"><span data-stu-id="3253a-136">For information about Redis options, see the [StackExchange Redis documentation](https://stackexchange.github.io/StackExchange.Redis/Configuration.html).</span></span>

::: moniker-end

* <span data-ttu-id="3253a-137">Si usa un servidor de Redis para varias SignalR aplicaciones, use un prefijo de canal diferente para cada aplicación SignalR.</span><span class="sxs-lookup"><span data-stu-id="3253a-137">If you're using one Redis server for multiple SignalR apps, use a different channel prefix for each SignalR app.</span></span>

  <span data-ttu-id="3253a-138">La configuración de un prefijo de canal aísla una SignalR aplicación de otras que usan prefijos de canal diferentes.</span><span class="sxs-lookup"><span data-stu-id="3253a-138">Setting a channel prefix isolates one SignalR app from others that use different channel prefixes.</span></span> <span data-ttu-id="3253a-139">Si no asigna prefijos diferentes, un mensaje enviado desde una aplicación a todos sus clientes irá a todos los clientes de todas las aplicaciones que usan el servidor de Redis como un backplane.</span><span class="sxs-lookup"><span data-stu-id="3253a-139">If you don't assign different prefixes, a message sent from one app to all of its own clients will go to all clients of all apps that use the Redis server as a backplane.</span></span>

* <span data-ttu-id="3253a-140">Configure el software de equilibrio de carga de la granja de servidores para sesiones permanentes.</span><span class="sxs-lookup"><span data-stu-id="3253a-140">Configure your server farm load balancing software for sticky sessions.</span></span> <span data-ttu-id="3253a-141">Estos son algunos ejemplos de documentación sobre cómo hacerlo:</span><span class="sxs-lookup"><span data-stu-id="3253a-141">Here are some examples of documentation on how to do that:</span></span>

  * [<span data-ttu-id="3253a-142">IIS</span><span class="sxs-lookup"><span data-stu-id="3253a-142">IIS</span></span>](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing)
  * [<span data-ttu-id="3253a-143">HAProxy</span><span class="sxs-lookup"><span data-stu-id="3253a-143">HAProxy</span></span>](https://www.haproxy.com/blog/load-balancing-affinity-persistence-sticky-sessions-what-you-need-to-know/)
  * [<span data-ttu-id="3253a-144">Nginx</span><span class="sxs-lookup"><span data-stu-id="3253a-144">Nginx</span></span>](https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/#sticky)
  * [<span data-ttu-id="3253a-145">pfSense</span><span class="sxs-lookup"><span data-stu-id="3253a-145">pfSense</span></span>](https://www.netgate.com/docs/pfsense/loadbalancing/inbound-load-balancing.html#sticky-connections)

## <a name="redis-server-errors"></a><span data-ttu-id="3253a-146">Errores del servidor de Redis</span><span class="sxs-lookup"><span data-stu-id="3253a-146">Redis server errors</span></span>

<span data-ttu-id="3253a-147">Cuando un servidor de Redis deja de funcionar, SignalR produce excepciones que indican que los mensajes no se entregarán.</span><span class="sxs-lookup"><span data-stu-id="3253a-147">When a Redis server goes down, SignalR throws exceptions that indicate messages won't be delivered.</span></span> <span data-ttu-id="3253a-148">Algunos mensajes de excepción típicos:</span><span class="sxs-lookup"><span data-stu-id="3253a-148">Some typical exception messages:</span></span>

* <span data-ttu-id="3253a-149">*Error al escribir el mensaje*</span><span class="sxs-lookup"><span data-stu-id="3253a-149">*Failed writing message*</span></span>
* <span data-ttu-id="3253a-150">*No se pudo invocar el método de concentrador ' MethodName '*</span><span class="sxs-lookup"><span data-stu-id="3253a-150">*Failed to invoke hub method 'MethodName'*</span></span>
* <span data-ttu-id="3253a-151">*Error en la conexión a Redis*</span><span class="sxs-lookup"><span data-stu-id="3253a-151">*Connection to Redis failed*</span></span>

SignalR<span data-ttu-id="3253a-152"> no almacena en búfer los mensajes para enviarlos cuando el servidor vuelve a realizar la copia de seguridad.</span><span class="sxs-lookup"><span data-stu-id="3253a-152"> doesn't buffer messages to send them when the server comes back up.</span></span> <span data-ttu-id="3253a-153">Los mensajes enviados mientras el servidor de Redis está inactivo se pierden.</span><span class="sxs-lookup"><span data-stu-id="3253a-153">Any messages sent while the Redis server is down are lost.</span></span>

SignalR<span data-ttu-id="3253a-154"> se vuelve a conectar automáticamente cuando vuelve a estar disponible el servidor de Redis.</span><span class="sxs-lookup"><span data-stu-id="3253a-154"> automatically reconnects when the Redis server is available again.</span></span>

### <a name="custom-behavior-for-connection-failures"></a><span data-ttu-id="3253a-155">Comportamiento personalizado para errores de conexión</span><span class="sxs-lookup"><span data-stu-id="3253a-155">Custom behavior for connection failures</span></span>

<span data-ttu-id="3253a-156">Este es un ejemplo que muestra cómo controlar los eventos de error de conexión de Redis.</span><span class="sxs-lookup"><span data-stu-id="3253a-156">Here's an example that shows how to handle Redis connection failure events.</span></span>

::: moniker range="= aspnetcore-2.1"

```csharp
services.AddSignalR()
        .AddRedis(o =>
        {
            o.ConnectionFactory = async writer =>
            {
                var config = new ConfigurationOptions
                {
                    AbortOnConnectFail = false
                };
                config.EndPoints.Add(IPAddress.Loopback, 0);
                config.SetDefaultPorts();
                var connection = await ConnectionMultiplexer.ConnectAsync(config, writer);
                connection.ConnectionFailed += (_, e) =>
                {
                    Console.WriteLine("Connection to Redis failed.");
                };

                if (!connection.IsConnected)
                {
                    Console.WriteLine("Did not connect to Redis.");
                }

                return connection;
            };
        });
```

::: moniker-end

::: moniker range="> aspnetcore-2.1"

```csharp
services.AddSignalR()
        .AddMessagePackProtocol()
        .AddStackExchangeRedis(o =>
        {
            o.ConnectionFactory = async writer =>
            {
                var config = new ConfigurationOptions
                {
                    AbortOnConnectFail = false
                };
                config.EndPoints.Add(IPAddress.Loopback, 0);
                config.SetDefaultPorts();
                var connection = await ConnectionMultiplexer.ConnectAsync(config, writer);
                connection.ConnectionFailed += (_, e) =>
                {
                    Console.WriteLine("Connection to Redis failed.");
                };

                if (!connection.IsConnected)
                {
                    Console.WriteLine("Did not connect to Redis.");
                }

                return connection;
            };
        });
```

::: moniker-end

## <a name="redis-clustering"></a><span data-ttu-id="3253a-157">Agrupación en clústeres de Redis</span><span class="sxs-lookup"><span data-stu-id="3253a-157">Redis Clustering</span></span>

<span data-ttu-id="3253a-158">La [agrupación en clústeres de Redis](https://redis.io/topics/cluster-spec) es un método para lograr alta disponibilidad mediante el uso de varios servidores de Redis.</span><span class="sxs-lookup"><span data-stu-id="3253a-158">[Redis Clustering](https://redis.io/topics/cluster-spec) is a method for achieving high availability by using multiple Redis servers.</span></span> <span data-ttu-id="3253a-159">La agrupación en clústeres no se admite oficialmente, pero podría funcionar.</span><span class="sxs-lookup"><span data-stu-id="3253a-159">Clustering isn't officially supported, but it might work.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3253a-160">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="3253a-160">Next steps</span></span>

<span data-ttu-id="3253a-161">Para obtener más información, vea los siguientes recursos:</span><span class="sxs-lookup"><span data-stu-id="3253a-161">For more information, see the following resources:</span></span>

* <xref:signalr/scale>
* [<span data-ttu-id="3253a-162">Documentación de Redis</span><span class="sxs-lookup"><span data-stu-id="3253a-162">Redis documentation</span></span>](https://redis.io/documentation)
* [<span data-ttu-id="3253a-163">Documentación de Redis StackExchange</span><span class="sxs-lookup"><span data-stu-id="3253a-163">StackExchange Redis documentation</span></span>](https://stackexchange.github.io/StackExchange.Redis/)
* [<span data-ttu-id="3253a-164">Azure Redis Cache documentación</span><span class="sxs-lookup"><span data-stu-id="3253a-164">Azure Redis Cache documentation</span></span>](https://docs.microsoft.com/azure/redis-cache/)
