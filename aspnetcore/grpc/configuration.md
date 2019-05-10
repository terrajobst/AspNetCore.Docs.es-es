---
title: gRPC para la configuración de ASP.NET Core
author: jamesnk
description: Obtenga información sobre cómo configurar gRPC para aplicaciones ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 04/09/2019
uid: grpc/configuration
ms.openlocfilehash: 66dfb9ec136616f10c1b7aaad766e18813b87de4
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087346"
---
# <a name="grpc-for-aspnet-core-configuration"></a>gRPC para la configuración de ASP.NET Core

## <a name="configure-services-options"></a>Configurar las opciones de servicios

En la tabla siguiente se describe las opciones de configuración de servicios de gRPC:

| Opción | Valor predeterminado | Descripción |
| ------ | ------------- | ----------- |
| `SendMaxMessageSize` | `null` | El tamaño máximo del mensaje en bytes que se pueden enviar desde el servidor. Intentando enviar un mensaje que supera los resultados de tamaño máximo de mensaje configurado en una excepción. |
| `ReceiveMaxMessageSize` | 4 MB | El tamaño máximo del mensaje en bytes, que puede ser recibido por el servidor. Si el servidor recibe un mensaje que supera este límite, produce una excepción. Al aumentar este valor permite que el servidor recibir los mensajes más grandes, pero puede repercutir negativamente en el consumo de memoria. |
| `EnableDetailedErrors` | `false` | Si `true`detallados se devuelven los mensajes de excepción a los clientes cuando se produce una excepción en un método de servicio. De manera predeterminada, es `false`. Si se establece en `true` puede producir la pérdida de información confidencial. |
| `CompressionProviders` | gzip | Una colección de proveedores de compresión utilizado para comprimir y descomprimir los mensajes. Se pueden crear proveedores de compresión personalizado y agregados a la colección. El valor predeterminado configurado el proveedor admite **gzip** compresión. |
| `ResponseCompressionAlgorithm` | `null` | El algoritmo de compresión utilizado para comprimir los mensajes enviados desde el servidor. El algoritmo debe coincidir con un proveedor de compresión en `CompressionProviders`. Para que el algorthm comprimir una respuesta, el cliente debe indicar admite el algoritmo enviando el **grpc-codificación aceptada** encabezado. |
| `ResponseCompressionLevel` | `null` | El nivel de compresión para comprimir los mensajes enviados desde el servidor. |

Se pueden configurar opciones para todos los servicios al proporcionar un delegado de opciones para la `AddGrpc` llamar `Startup.ConfigureServices`.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc(options =>
    {
        options.EnableDetailedErrors = true;
    });
}
```

Opciones para un único servicio invalidan las opciones globales de `AddGrpc` y pueden configurarse mediante `AddServiceOptions<TService>`:

```csharp
services.AddGrpc().AddServiceOptions<MyService>(options =>
{
    options.ReceiveMaxMessageSize = 10 * 1024 * 1024; // 10 megabytes
});
```

## <a name="configure-kestrel-options"></a>Configurar las opciones de Kestrel

Servidor kestrel tiene opciones de configuración que afectan al comportamiento de gRPC para ASP.NET.

### <a name="request-body-data-rate-limit"></a>Límite de velocidad de datos de cuerpo de solicitud

De forma predeterminada, el servidor Kestrel impone un [velocidad de datos del cuerpo de solicitud mínimo](
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>). Para cliente de streaming y dúplex, las llamadas de transmisión por secuencias, no se puede satisfacer esta velocidad y la conexión es posible que se agotó el tiempo. El mínimo debe deshabilitarse el límite de velocidad de datos cuando el servicio gRPC incluye el cliente de transmisión por secuencias y dúplex, las llamadas de transmisión por secuencias el cuerpo de solicitud:

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateHostBuilder(args).Build().Run();
    }

    public static IHostBuilder CreateHostBuilder(string[] args) =>
         Host.CreateDefaultBuilder(args)
    .ConfigureWebHostDefaults(webBuilder =>
    {
        webBuilder.UseStartup<Startup>();
        webBuilder.ConfigureKestrel((context, options) =>
        {
            options.Limits.MinRequestBodyDataRate = null;
        });
    });
}
```

## <a name="additional-resources"></a>Recursos adicionales

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
