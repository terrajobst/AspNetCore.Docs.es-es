---
title: Redis backplane de escalado horizontal de SignalR de ASP.NET Core
author: tdykstra
description: Obtenga información sobre cómo configurar un backplane de Redis para habilitar el escalado horizontal de una aplicación ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/28/2018
uid: signalr/redis-backplane
ms.openlocfilehash: c8b09c0d482da344b54d167c0c9757167eaa6186
ms.sourcegitcommit: e9b99854b0a8021dafabee0db5e1338067f250a9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2018
ms.locfileid: "52452983"
---
# <a name="set-up-a-redis-backplane-for-aspnet-core-signalr-scale-out"></a><span data-ttu-id="be767-103">Configurar un backplane de Redis de escalado horizontal de SignalR de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="be767-103">Set up a Redis backplane for ASP.NET Core SignalR scale-out</span></span>

<span data-ttu-id="be767-104">Por [Andrew Stanton-Nurse](https://twitter.com/anurse), [Brady Gaster](https://twitter.com/bradygaster), y [Tom Dykstra](https://github.com/tdykstra),</span><span class="sxs-lookup"><span data-stu-id="be767-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse), [Brady Gaster](https://twitter.com/bradygaster), and [Tom Dykstra](https://github.com/tdykstra),</span></span>

<span data-ttu-id="be767-105">En este artículo se explica los aspectos de SignalR específicos de la configuración de un [Redis](https://redis.io/) servidor que se usará para el escalado horizontal de una aplicación ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="be767-105">This article explains SignalR-specific aspects of setting up a [Redis](https://redis.io/) server to use for scaling out an ASP.NET Core SignalR app.</span></span>

## <a name="set-up-a-redis-backplane"></a><span data-ttu-id="be767-106">Configurar un backplane de Redis</span><span class="sxs-lookup"><span data-stu-id="be767-106">Set up a Redis backplane</span></span>

* <span data-ttu-id="be767-107">Implementar un servidor de Redis.</span><span class="sxs-lookup"><span data-stu-id="be767-107">Deploy a Redis server.</span></span>

  <span data-ttu-id="be767-108">Para su uso en producción, se recomienda un backplane de Redis solo para la infraestructura local.</span><span class="sxs-lookup"><span data-stu-id="be767-108">For production use, a Redis backplane is recommended only for on-premises infrastructure.</span></span> <span data-ttu-id="be767-109">Para minimizar la latencia, debe ser el servidor de Redis en el mismo centro de datos que la aplicación de SignalR.</span><span class="sxs-lookup"><span data-stu-id="be767-109">To minimize latency, the Redis server should be in the same data center as the SignalR app.</span></span> <span data-ttu-id="be767-110">Si se ejecuta la aplicación de SignalR en la nube de Azure, se recomienda Azure SignalR Service en lugar de un backplane de Redis.</span><span class="sxs-lookup"><span data-stu-id="be767-110">If your SignalR app is running in the Azure cloud, we recommend Azure SignalR Service instead of a Redis backplane.</span></span> <span data-ttu-id="be767-111">Puede usar el servicio Azure Redis Cache para el desarrollo y entornos de prueba.</span><span class="sxs-lookup"><span data-stu-id="be767-111">You can use the Azure Redis Cache Service for development and test environments.</span></span> <span data-ttu-id="be767-112">Para obtener más información, vea los siguientes recursos:</span><span class="sxs-lookup"><span data-stu-id="be767-112">For more information, see the following resources:</span></span>

  * <xref:signalr/scale>
  * [<span data-ttu-id="be767-113">Documentación de Redis</span><span class="sxs-lookup"><span data-stu-id="be767-113">Redis documentation</span></span>](https://redis.io/)
  * [<span data-ttu-id="be767-114">Documentación de Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="be767-114">Azure Redis Cache documentation</span></span>](https://docs.microsoft.com/en-us/azure/redis-cache/)

::: moniker range="= aspnetcore-2.1"

* <span data-ttu-id="be767-115">En la aplicación de SignalR, instale el `Microsoft.AspNetCore.SignalR.Redis` paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="be767-115">In the SignalR app, install the `Microsoft.AspNetCore.SignalR.Redis` NuGet package.</span></span>

* <span data-ttu-id="be767-116">En el `Startup.ConfigureServices` método, llame a `AddRedis` después `AddSignalR`:</span><span class="sxs-lookup"><span data-stu-id="be767-116">In the `Startup.ConfigureServices` method, call `AddRedis` after `AddSignalR`:</span></span>

  ```csharp
  services.AddSignalR().AddRedis("<your_Redis_connection_string>");
  ```

* <span data-ttu-id="be767-117">Configure las opciones según sea necesario:</span><span class="sxs-lookup"><span data-stu-id="be767-117">Configure options as needed:</span></span>
 
  <span data-ttu-id="be767-118">Se puede establecer la mayoría de las opciones en la cadena de conexión o en el [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) objeto.</span><span class="sxs-lookup"><span data-stu-id="be767-118">Most options can be set in the connection string or in the [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) object.</span></span> <span data-ttu-id="be767-119">Las opciones especificadas en `ConfigurationOptions` invalidará la establece en la cadena de conexión.</span><span class="sxs-lookup"><span data-stu-id="be767-119">Options specified in `ConfigurationOptions` override the ones set in the connection string.</span></span>

  <span data-ttu-id="be767-120">El ejemplo siguiente muestra cómo establecer las opciones el `ConfigurationOptions` objeto.</span><span class="sxs-lookup"><span data-stu-id="be767-120">The following example shows how to set options in the `ConfigurationOptions` object.</span></span> <span data-ttu-id="be767-121">Este ejemplo agrega un prefijo de canal para que varias aplicaciones pueden compartir la misma instancia de Redis, como se explica en el paso siguiente.</span><span class="sxs-lookup"><span data-stu-id="be767-121">This example adds a channel prefix so that multiple apps can share the same Redis instance, as explained in the following step.</span></span>

  ```csharp
  services.AddSignalR()
    .AddRedis(connectionString, options => {
        options.Configuration.ChannelPrefix = "MyApp";
    });
  ```

  <span data-ttu-id="be767-122">En el código anterior, `options.Configuration` se inicializa con todo lo que se especificó en la cadena de conexión.</span><span class="sxs-lookup"><span data-stu-id="be767-122">In the preceding code, `options.Configuration` is initialized with whatever was specified in the connection string.</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.1"

* <span data-ttu-id="be767-123">En la aplicación de SignalR, instale el `Microsoft.AspNetCore.SignalR.StackExchangeRedis` paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="be767-123">In the SignalR app, install the `Microsoft.AspNetCore.SignalR.StackExchangeRedis` NuGet package.</span></span>

* <span data-ttu-id="be767-124">En el `Startup.ConfigureServices` método, llame a `AddStackExchangeRedis` después `AddSignalR`:</span><span class="sxs-lookup"><span data-stu-id="be767-124">In the `Startup.ConfigureServices` method, call `AddStackExchangeRedis` after `AddSignalR`:</span></span>

  ```csharp
  services.AddSignalR().AddStackExchangeRedis("<your_Redis_connection_string>");
  ```

* <span data-ttu-id="be767-125">Configure las opciones según sea necesario:</span><span class="sxs-lookup"><span data-stu-id="be767-125">Configure options as needed:</span></span>
 
  <span data-ttu-id="be767-126">Se puede establecer la mayoría de las opciones en la cadena de conexión o en el [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) objeto.</span><span class="sxs-lookup"><span data-stu-id="be767-126">Most options can be set in the connection string or in the [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) object.</span></span> <span data-ttu-id="be767-127">Las opciones especificadas en `ConfigurationOptions` invalidará la establece en la cadena de conexión.</span><span class="sxs-lookup"><span data-stu-id="be767-127">Options specified in `ConfigurationOptions` override the ones set in the connection string.</span></span>

  <span data-ttu-id="be767-128">El ejemplo siguiente muestra cómo establecer las opciones el `ConfigurationOptions` objeto.</span><span class="sxs-lookup"><span data-stu-id="be767-128">The following example shows how to set options in the `ConfigurationOptions` object.</span></span> <span data-ttu-id="be767-129">Este ejemplo agrega un prefijo de canal para que varias aplicaciones pueden compartir la misma instancia de Redis, como se explica en el paso siguiente.</span><span class="sxs-lookup"><span data-stu-id="be767-129">This example adds a channel prefix so that multiple apps can share the same Redis instance, as explained in the following step.</span></span>

  ```csharp
  services.AddSignalR()
    .AddStackExchangeRedis(connectionString, options => {
        options.Configuration.ChannelPrefix = "MyApp";
    });
  ```

  <span data-ttu-id="be767-130">En el código anterior, `options.Configuration` se inicializa con todo lo que se especificó en la cadena de conexión.</span><span class="sxs-lookup"><span data-stu-id="be767-130">In the preceding code, `options.Configuration` is initialized with whatever was specified in the connection string.</span></span>

  <span data-ttu-id="be767-131">Para obtener información acerca de las opciones de Redis, consulte el [documentación Redis StackExchange](https://stackexchange.github.io/StackExchange.Redis/Configuration.html).</span><span class="sxs-lookup"><span data-stu-id="be767-131">For information about Redis options, see the [StackExchange Redis documentation](https://stackexchange.github.io/StackExchange.Redis/Configuration.html).</span></span>

::: moniker-end

* <span data-ttu-id="be767-132">Si usa un servidor de Redis para varias aplicaciones de SignalR, use un prefijo de canal diferentes para cada aplicación de SignalR.</span><span class="sxs-lookup"><span data-stu-id="be767-132">If you're using one Redis server for multiple SignalR apps, use a different channel prefix for each SignalR app.</span></span>

  <span data-ttu-id="be767-133">Establecer un prefijo de canal aísla una aplicación de SignalR de otras personas que utilizan los prefijos de canal diferente.</span><span class="sxs-lookup"><span data-stu-id="be767-133">Setting a channel prefix isolates one SignalR app from others that use different channel prefixes.</span></span> <span data-ttu-id="be767-134">Si no asigna prefijos diferentes, un mensaje enviado desde una aplicación a todos sus propios clientes irá a todos los clientes de todas las aplicaciones que usan el servidor de Redis como backplane.</span><span class="sxs-lookup"><span data-stu-id="be767-134">If you don't assign different prefixes, a message sent from one app to all of its own clients will go to all clients of all apps that use the Redis server as a backplane.</span></span>

* <span data-ttu-id="be767-135">Configure su servidor granja equilibrio de carga software para sesiones permanentes.</span><span class="sxs-lookup"><span data-stu-id="be767-135">Configure your server farm load balancing software for sticky sessions.</span></span> <span data-ttu-id="be767-136">Estos son algunos ejemplos de documentación sobre cómo hacerlo:</span><span class="sxs-lookup"><span data-stu-id="be767-136">Here are some examples of documentation on how to do that:</span></span>

  * [<span data-ttu-id="be767-137">IIS</span><span class="sxs-lookup"><span data-stu-id="be767-137">IIS</span></span>](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing)
  * [<span data-ttu-id="be767-138">HAProxy</span><span class="sxs-lookup"><span data-stu-id="be767-138">HAProxy</span></span>](https://www.haproxy.com/blog/load-balancing-affinity-persistence-sticky-sessions-what-you-need-to-know/)
  * [<span data-ttu-id="be767-139">Nginx</span><span class="sxs-lookup"><span data-stu-id="be767-139">Nginx</span></span>](https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/#sticky)
  * [<span data-ttu-id="be767-140">pfSense</span><span class="sxs-lookup"><span data-stu-id="be767-140">pfSense</span></span>](https://www.netgate.com/docs/pfsense/loadbalancing/inbound-load-balancing.html#sticky-connections)

## <a name="redis-server-errors"></a><span data-ttu-id="be767-141">Errores del servidor de Redis</span><span class="sxs-lookup"><span data-stu-id="be767-141">Redis server errors</span></span>

<span data-ttu-id="be767-142">Cuando un servidor de Redis deja de funcionar, SignalR genera excepciones que indican que no se entregarán los mensajes.</span><span class="sxs-lookup"><span data-stu-id="be767-142">When a Redis server goes down, SignalR throws exceptions that indicate messages won't be delivered.</span></span> <span data-ttu-id="be767-143">Algunos mensajes de excepción típico:</span><span class="sxs-lookup"><span data-stu-id="be767-143">Some typical exception messages:</span></span>

* <span data-ttu-id="be767-144">*Mensaje de error de escritura*</span><span class="sxs-lookup"><span data-stu-id="be767-144">*Failed writing message*</span></span>
* <span data-ttu-id="be767-145">*No se pudo invocar el método de concentrador 'NombreMétodo'*</span><span class="sxs-lookup"><span data-stu-id="be767-145">*Failed to invoke hub method 'MethodName'*</span></span>
* <span data-ttu-id="be767-146">*Error de conexión con Redis*</span><span class="sxs-lookup"><span data-stu-id="be767-146">*Connection to Redis failed*</span></span>

<span data-ttu-id="be767-147">SignalR no almacena en búfer los mensajes que les envíe cuando el servidor vuelve a activarse.</span><span class="sxs-lookup"><span data-stu-id="be767-147">SignalR doesn't buffer messages to send them when the server comes back up.</span></span> <span data-ttu-id="be767-148">Se pierden los mensajes enviados mientras el servidor Redis está inactivo.</span><span class="sxs-lookup"><span data-stu-id="be767-148">Any messages sent while the Redis server is down are lost.</span></span>

<span data-ttu-id="be767-149">SignalR vuelve a conectarse automáticamente cuando el servidor de Redis esté disponible de nuevo.</span><span class="sxs-lookup"><span data-stu-id="be767-149">SignalR automatically reconnects when the Redis server is available again.</span></span>

### <a name="custom-behavior-for-connection-failures"></a><span data-ttu-id="be767-150">Comportamiento personalizado para los errores de conexión</span><span class="sxs-lookup"><span data-stu-id="be767-150">Custom behavior for connection failures</span></span>

<span data-ttu-id="be767-151">Este es un ejemplo que muestra cómo controlar eventos de error de conexión de Redis.</span><span class="sxs-lookup"><span data-stu-id="be767-151">Here's an example that shows how to handle Redis connection failure events.</span></span>

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

## <a name="clustering"></a><span data-ttu-id="be767-152">Agrupación en clústeres</span><span class="sxs-lookup"><span data-stu-id="be767-152">Clustering</span></span>

<span data-ttu-id="be767-153">Agrupación en clústeres es un método para lograr alta disponibilidad mediante el uso de varios servidores de Redis.</span><span class="sxs-lookup"><span data-stu-id="be767-153">Clustering is a method for achieving high availability by using multiple Redis servers.</span></span> <span data-ttu-id="be767-154">Oficialmente no se admite la agrupación en clústeres, pero es posible que funcione.</span><span class="sxs-lookup"><span data-stu-id="be767-154">Clustering isn't officially supported, but it might work.</span></span>

## <a name="next-steps"></a><span data-ttu-id="be767-155">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="be767-155">Next steps</span></span>

<span data-ttu-id="be767-156">Para obtener más información, vea los siguientes recursos:</span><span class="sxs-lookup"><span data-stu-id="be767-156">For more information, see the following resources:</span></span>

* <xref:signalr/scale>
* [<span data-ttu-id="be767-157">Documentación de Redis</span><span class="sxs-lookup"><span data-stu-id="be767-157">Redis documentation</span></span>](https://redis.io/documentation)
* [<span data-ttu-id="be767-158">Documentación de Redis StackExchange</span><span class="sxs-lookup"><span data-stu-id="be767-158">StackExchange Redis documentation</span></span>](https://stackexchange.github.io/StackExchange.Redis/)
* [<span data-ttu-id="be767-159">Documentación de Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="be767-159">Azure Redis Cache documentation</span></span>](https://docs.microsoft.com/en-us/azure/redis-cache/)
