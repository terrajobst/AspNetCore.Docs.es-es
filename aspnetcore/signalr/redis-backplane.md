---
title: Redis backplane de escalado horizontal de SignalR de ASP.NET Core
author: bradygaster
description: Obtenga información sobre cómo configurar un backplane de Redis para habilitar el escalado horizontal de una aplicación ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/28/2018
uid: signalr/redis-backplane
ms.openlocfilehash: c02d8cd5fb3b6edbb21be4889da2e880099b731b
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837447"
---
# <a name="set-up-a-redis-backplane-for-aspnet-core-signalr-scale-out"></a>Configurar un backplane de Redis de escalado horizontal de SignalR de ASP.NET Core

Por [Andrew Stanton-Nurse](https://twitter.com/anurse), [Brady Gaster](https://twitter.com/bradygaster), y [Tom Dykstra](https://github.com/tdykstra),

En este artículo se explica los aspectos de SignalR específicos de la configuración de un [Redis](https://redis.io/) servidor que se usará para el escalado horizontal de una aplicación ASP.NET Core SignalR.

## <a name="set-up-a-redis-backplane"></a>Configurar un backplane de Redis

* Implementar un servidor de Redis.

  > [!IMPORTANT] 
  > Para su uso en producción, se recomienda un backplane de Redis solo cuando se ejecuta en el mismo centro de datos que la aplicación de SignalR. En caso contrario, latencia de red disminuye el rendimiento. Si se ejecuta la aplicación de SignalR en la nube de Azure, se recomienda Azure SignalR Service en lugar de un backplane de Redis. Puede usar el servicio Azure Redis Cache para el desarrollo y entornos de prueba.

  Para obtener más información, vea los siguientes recursos:

  * <xref:signalr/scale>
  * [Documentación de Redis](https://redis.io/)
  * [Documentación de Azure Redis Cache](https://docs.microsoft.com/en-us/azure/redis-cache/)

::: moniker range="= aspnetcore-2.1"

* En la aplicación de SignalR, instale el `Microsoft.AspNetCore.SignalR.Redis` paquete NuGet. (También hay un `Microsoft.AspNetCore.SignalR.StackExchangeRedis` empaquetar, pero que uno es para ASP.NET Core 2.2 y versiones posteriores.)

* En el `Startup.ConfigureServices` método, llame a `AddRedis` después `AddSignalR`:

  ```csharp
  services.AddSignalR().AddRedis("<your_Redis_connection_string>");
  ```

* Configure las opciones según sea necesario:
 
  Se puede establecer la mayoría de las opciones en la cadena de conexión o en el [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) objeto. Las opciones especificadas en `ConfigurationOptions` invalidará la establece en la cadena de conexión.

  El ejemplo siguiente muestra cómo establecer las opciones el `ConfigurationOptions` objeto. Este ejemplo agrega un prefijo de canal para que varias aplicaciones pueden compartir la misma instancia de Redis, como se explica en el paso siguiente.

  ```csharp
  services.AddSignalR()
    .AddRedis(connectionString, options => {
        options.Configuration.ChannelPrefix = "MyApp";
    });
  ```

  En el código anterior, `options.Configuration` se inicializa con todo lo que se especificó en la cadena de conexión.

::: moniker-end

::: moniker range="> aspnetcore-2.1"

* En la aplicación de SignalR, instale uno de los siguientes paquetes NuGet:

  * `Microsoft.AspNetCore.SignalR.StackExchangeRedis` -Depende de StackExchange.Redis 2.X.X. Este es el paquete recomendado para ASP.NET Core 2.2 y versiones posteriores.
  * `Microsoft.AspNetCore.SignalR.Redis` -Depende 1.X.X StackExchange.Redis. Este paquete no estará disponible en ASP.NET Core 3.0.

* En el `Startup.ConfigureServices` método, llame a `AddStackExchangeRedis` después `AddSignalR`:

  ```csharp
  services.AddSignalR().AddStackExchangeRedis("<your_Redis_connection_string>");
  ```

* Configure las opciones según sea necesario:
 
  Se puede establecer la mayoría de las opciones en la cadena de conexión o en el [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) objeto. Las opciones especificadas en `ConfigurationOptions` invalidará la establece en la cadena de conexión.

  El ejemplo siguiente muestra cómo establecer las opciones el `ConfigurationOptions` objeto. Este ejemplo agrega un prefijo de canal para que varias aplicaciones pueden compartir la misma instancia de Redis, como se explica en el paso siguiente.

  ```csharp
  services.AddSignalR()
    .AddStackExchangeRedis(connectionString, options => {
        options.Configuration.ChannelPrefix = "MyApp";
    });
  ```

  En el código anterior, `options.Configuration` se inicializa con todo lo que se especificó en la cadena de conexión.

  Para obtener información acerca de las opciones de Redis, consulte el [documentación Redis StackExchange](https://stackexchange.github.io/StackExchange.Redis/Configuration.html).

::: moniker-end

* Si usa un servidor de Redis para varias aplicaciones de SignalR, use un prefijo de canal diferentes para cada aplicación de SignalR.

  Establecer un prefijo de canal aísla una aplicación de SignalR de otras personas que utilizan los prefijos de canal diferente. Si no asigna prefijos diferentes, un mensaje enviado desde una aplicación a todos sus propios clientes irá a todos los clientes de todas las aplicaciones que usan el servidor de Redis como backplane.

* Configure su servidor granja equilibrio de carga software para sesiones permanentes. Estos son algunos ejemplos de documentación sobre cómo hacerlo:

  * [IIS](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing)
  * [HAProxy](https://www.haproxy.com/blog/load-balancing-affinity-persistence-sticky-sessions-what-you-need-to-know/)
  * [Nginx](https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/#sticky)
  * [pfSense](https://www.netgate.com/docs/pfsense/loadbalancing/inbound-load-balancing.html#sticky-connections)

## <a name="redis-server-errors"></a>Errores del servidor de Redis

Cuando un servidor de Redis deja de funcionar, SignalR genera excepciones que indican que no se entregarán los mensajes. Algunos mensajes de excepción típico:

* *Mensaje de error de escritura*
* *No se pudo invocar el método de concentrador 'NombreMétodo'*
* *Error de conexión con Redis*

SignalR no almacena en búfer los mensajes que les envíe cuando el servidor vuelve a activarse. Se pierden los mensajes enviados mientras el servidor Redis está inactivo.

SignalR vuelve a conectarse automáticamente cuando el servidor de Redis esté disponible de nuevo.

### <a name="custom-behavior-for-connection-failures"></a>Comportamiento personalizado para los errores de conexión

Este es un ejemplo que muestra cómo controlar eventos de error de conexión de Redis.

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

## <a name="clustering"></a>Agrupación en clústeres

Agrupación en clústeres es un método para lograr alta disponibilidad mediante el uso de varios servidores de Redis. Oficialmente no se admite la agrupación en clústeres, pero es posible que funcione.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información, vea los siguientes recursos:

* <xref:signalr/scale>
* [Documentación de Redis](https://redis.io/documentation)
* [Documentación de Redis StackExchange](https://stackexchange.github.io/StackExchange.Redis/)
* [Documentación de Azure Redis Cache](https://docs.microsoft.com/en-us/azure/redis-cache/)
