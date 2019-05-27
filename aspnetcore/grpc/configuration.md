---
title: gRPC para la configuración de ASP.NET Core
author: jamesnk
description: Obtenga información sobre cómo configurar gRPC para aplicaciones ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 04/09/2019
uid: grpc/configuration
ms.openlocfilehash: 851c9ca1f7d62f6f368df66bb38eb4bbaf64bf32
ms.sourcegitcommit: 5d384db2fa9373a93b5d15e985fb34430e49ad7a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66041896"
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

## <a name="additional-resources"></a>Recursos adicionales

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
