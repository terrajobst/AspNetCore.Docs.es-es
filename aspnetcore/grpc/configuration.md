---
title: gRPC para la configuración de ASP.NET Core
author: jamesnk
description: Obtenga información sobre cómo configurar gRPC para aplicaciones ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 5/30/2019
uid: grpc/configuration
ms.openlocfilehash: 1f8250dc9aa8b82da384ee28287011baa19dc11f
ms.sourcegitcommit: a1364109d11d414121a6337b611bee61d6e489e9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66491234"
---
# <a name="grpc-for-aspnet-core-configuration"></a>gRPC para la configuración de ASP.NET Core

## <a name="configure-services-options"></a>Configurar las opciones de servicios

En la tabla siguiente se describe las opciones de configuración de servicios de gRPC:

| Opción | Valor predeterminado | Descripción |
| ------ | ------------- | ----------- |
| `SendMaxMessageSize` | `null` | El tamaño máximo del mensaje en bytes que se pueden enviar desde el servidor. Intentando enviar un mensaje que supera los resultados de tamaño máximo de mensaje configurado en una excepción. |
| `ReceiveMaxMessageSize` | 4 MB | El tamaño máximo del mensaje en bytes, que puede ser recibido por el servidor. Si el servidor recibe un mensaje que supera este límite, produce una excepción. Al aumentar este valor permite que el servidor recibir los mensajes más grandes, pero puede repercutir negativamente en el consumo de memoria. |
| `EnableDetailedErrors` | `false` | Si `true`detallados se devuelven los mensajes de excepción a los clientes cuando se produce una excepción en un método de servicio. De manera predeterminada, es `false`. Establecer `EnableDetailedErrors` a `true` puede producir la pérdida de información confidencial. |
| `CompressionProviders` | gzip | Una colección de proveedores de compresión utilizado para comprimir y descomprimir los mensajes. Se pueden crear proveedores de compresión personalizado y agregados a la colección. El valor predeterminado configurado el proveedor admite **gzip** compresión. |
| `ResponseCompressionAlgorithm` | `null` | El algoritmo de compresión utilizado para comprimir los mensajes enviados desde el servidor. El algoritmo debe coincidir con un proveedor de compresión en `CompressionProviders`. Para que el algoritmo comprimir una respuesta, el cliente debe indicar admite el algoritmo enviando el **grpc-codificación aceptada** encabezado. |
| `ResponseCompressionLevel` | `null` | El nivel de compresión para comprimir los mensajes enviados desde el servidor. |

Se pueden configurar opciones para todos los servicios al proporcionar un delegado de opciones para la `AddGrpc` llamar `Startup.ConfigureServices`:

[!code-csharp[](~/grpc/configuration/sample/GrcpService/Startup.cs?name=snippet)]

Opciones para un único servicio invalidan las opciones globales de `AddGrpc` y pueden configurarse mediante `AddServiceOptions<TService>`:

[!code-csharp[](~/grpc/configuration/sample/GrcpService/Startup2.cs?name=snippet)]

## <a name="configure-client-options"></a>Configurar las opciones de cliente

El código siguiente establece el envío máximo de cliente y el tamaño de mensaje de recepción:

[!code-csharp[](~/grpc/configuration/sample/Program.cs?name=snippet&highlight=3-6)]

## <a name="additional-resources"></a>Recursos adicionales

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
