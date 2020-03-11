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
ms.openlocfilehash: 0461fc6a212ba78111bc2054cca74951721c5820
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78652667"
---
# <a name="set-up-a-redis-backplane-for-aspnet-core-opno-locsignalr-scale-out"></a>Configuración de un backplane de Redis para el escalado horizontal de ASP.NET Core SignalR

Por [Andrew Stanton-enfermera](https://twitter.com/anurse), [Brady transgastor](https://twitter.com/bradygaster)y [Tom Dykstra](https://github.com/tdykstra),

En este artículo se explican los aspectos específicos de SignalRde la configuración de un servidor de [Redis](https://redis.io/) para usar para escalar horizontalmente una aplicación ASP.net Core SignalR.

## <a name="set-up-a-redis-backplane"></a>Configuración de un backplane de Redis

* Implementar un servidor de Redis.

  > [!IMPORTANT] 
  > Para su uso en producción, se recomienda un backplane de Redis solo cuando se ejecuta en el mismo centro de datos que la aplicación SignalR. De lo contrario, la latencia de red degrada el rendimiento. Si la aplicación de SignalR se ejecuta en la nube de Azure, se recomienda Azure SignalR Service en lugar de un backplane de Redis. Puede usar el servicio de Azure Redis Cache para entornos de desarrollo y pruebas.

  Para obtener más información, consulte los siguientes recursos:

  * <xref:signalr/scale>
  * [Documentación de Redis](https://redis.io/)
  * [Documentación de Azure Redis Cache](https://docs.microsoft.com/azure/redis-cache/)

::: moniker range="= aspnetcore-2.1"

* En la aplicación SignalR, instale el paquete NuGet `Microsoft.AspNetCore.SignalR.Redis`.
* En el método `Startup.ConfigureServices`, llame a `AddRedis` después `AddSignalR`:

  ```csharp
  services.AddSignalR().AddRedis("<your_Redis_connection_string>");
  ```

* Configure las opciones según sea necesario:
 
  La mayoría de las opciones se pueden establecer en la cadena de conexión o en el objeto [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) . Las opciones especificadas en `ConfigurationOptions` invalidan las definidas en la cadena de conexión.

  En el ejemplo siguiente se muestra cómo establecer opciones en el objeto `ConfigurationOptions`. En este ejemplo se agrega un prefijo de canal para que varias aplicaciones puedan compartir la misma instancia de Redis, tal y como se explica en el paso siguiente.

  ```csharp
  services.AddSignalR()
    .AddRedis(connectionString, options => {
        options.Configuration.ChannelPrefix = "MyApp";
    });
  ```

  En el código anterior, `options.Configuration` se inicializa con lo que se haya especificado en la cadena de conexión.

::: moniker-end

::: moniker range="= aspnetcore-2.2"

* En la aplicación SignalR, instale uno de los siguientes paquetes NuGet:

  * `Microsoft.AspNetCore.SignalR.StackExchangeRedis`: depende de StackExchange. Redis 2. X.X. Este es el paquete recomendado para ASP.NET Core 2,2 y versiones posteriores.
  * `Microsoft.AspNetCore.SignalR.Redis`: depende de StackExchange. Redis 1. X.X. Este paquete no está incluido en ASP.NET Core 3,0 y versiones posteriores.

* En el método `Startup.ConfigureServices`, llame a <xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisDependencyInjectionExtensions.AddStackExchangeRedis*>:

  ```csharp
  services.AddSignalR().AddStackExchangeRedis("<your_Redis_connection_string>");
  ```

 Al usar `Microsoft.AspNetCore.SignalR.Redis`, llame a <xref:Microsoft.Extensions.DependencyInjection.RedisDependencyInjectionExtensions.AddRedis*>.

* Configure las opciones según sea necesario:
 
  La mayoría de las opciones se pueden establecer en la cadena de conexión o en el objeto [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) . Las opciones especificadas en `ConfigurationOptions` invalidan las definidas en la cadena de conexión.

  En el ejemplo siguiente se muestra cómo establecer opciones en el objeto `ConfigurationOptions`. En este ejemplo se agrega un prefijo de canal para que varias aplicaciones puedan compartir la misma instancia de Redis, tal y como se explica en el paso siguiente.

  ```csharp
  services.AddSignalR()
    .AddStackExchangeRedis(connectionString, options => {
        options.Configuration.ChannelPrefix = "MyApp";
    });
  ```

 Al usar `Microsoft.AspNetCore.SignalR.Redis`, llame a <xref:Microsoft.Extensions.DependencyInjection.RedisDependencyInjectionExtensions.AddRedis*>.

  En el código anterior, `options.Configuration` se inicializa con lo que se haya especificado en la cadena de conexión.

  Para obtener información sobre las opciones de Redis, consulte la [documentación de StackExchange Redis](https://stackexchange.github.io/StackExchange.Redis/Configuration.html).

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

* En la aplicación SignalR, instale el siguiente paquete de NuGet:

  * `Microsoft.AspNetCore.SignalR.StackExchangeRedis`
  
* En el método `Startup.ConfigureServices`, llame a <xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisDependencyInjectionExtensions.AddStackExchangeRedis*>:

  ```csharp
  services.AddSignalR().AddStackExchangeRedis("<your_Redis_connection_string>");
  ```
  
* Configure las opciones según sea necesario:
 
  La mayoría de las opciones se pueden establecer en la cadena de conexión o en el objeto [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) . Las opciones especificadas en `ConfigurationOptions` invalidan las definidas en la cadena de conexión.

  En el ejemplo siguiente se muestra cómo establecer opciones en el objeto `ConfigurationOptions`. En este ejemplo se agrega un prefijo de canal para que varias aplicaciones puedan compartir la misma instancia de Redis, tal y como se explica en el paso siguiente.

  ```csharp
  services.AddSignalR()
    .AddStackExchangeRedis(connectionString, options => {
        options.Configuration.ChannelPrefix = "MyApp";
    });
  ```

  En el código anterior, `options.Configuration` se inicializa con lo que se haya especificado en la cadena de conexión.

  Para obtener información sobre las opciones de Redis, consulte la [documentación de StackExchange Redis](https://stackexchange.github.io/StackExchange.Redis/Configuration.html).

::: moniker-end

* Si usa un servidor de Redis para varias SignalR aplicaciones, use un prefijo de canal diferente para cada aplicación SignalR.

  La configuración de un prefijo de canal aísla una SignalR aplicación de otras que usan prefijos de canal diferentes. Si no asigna prefijos diferentes, un mensaje enviado desde una aplicación a todos sus clientes irá a todos los clientes de todas las aplicaciones que usan el servidor de Redis como un backplane.

* Configure el software de equilibrio de carga de la granja de servidores para sesiones permanentes. Estos son algunos ejemplos de documentación sobre cómo hacerlo:

  * [IIS](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing)
  * [HAProxy](https://www.haproxy.com/blog/load-balancing-affinity-persistence-sticky-sessions-what-you-need-to-know/)
  * [Nginx](https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/#sticky)
  * [pfSense](https://www.netgate.com/docs/pfsense/loadbalancing/inbound-load-balancing.html#sticky-connections)

## <a name="redis-server-errors"></a>Errores del servidor de Redis

Cuando un servidor de Redis deja de funcionar, SignalR produce excepciones que indican que los mensajes no se entregarán. Algunos mensajes de excepción típicos:

* *Error al escribir el mensaje*
* *No se pudo invocar el método de concentrador ' MethodName '*
* *Error en la conexión a Redis*

SignalR no almacena en búfer los mensajes para enviarlos cuando el servidor vuelve a realizar la copia de seguridad. Los mensajes enviados mientras el servidor de Redis está inactivo se pierden.

SignalR se vuelve a conectar automáticamente cuando vuelve a estar disponible el servidor de Redis.

### <a name="custom-behavior-for-connection-failures"></a>Comportamiento personalizado para errores de conexión

Este es un ejemplo que muestra cómo controlar los eventos de error de conexión de Redis.

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

## <a name="redis-clustering"></a>Agrupación en clústeres de Redis

La [agrupación en clústeres de Redis](https://redis.io/topics/cluster-spec) es un método para lograr alta disponibilidad mediante el uso de varios servidores de Redis. La agrupación en clústeres no se admite oficialmente, pero podría funcionar.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información, consulte los siguientes recursos:

* <xref:signalr/scale>
* [Documentación de Redis](https://redis.io/documentation)
* [Documentación de Redis StackExchange](https://stackexchange.github.io/StackExchange.Redis/)
* [Documentación de Azure Redis Cache](https://docs.microsoft.com/azure/redis-cache/)
