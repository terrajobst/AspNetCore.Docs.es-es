---
title: gRPC para la configuración de ASP.NET Core
author: jamesnk
description: Obtenga información sobre cómo configurar gRPC para aplicaciones ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 08/21/2019
uid: grpc/configuration
ms.openlocfilehash: 34eb598211c87fbb2c68ae5e041da50d02f543f7
ms.sourcegitcommit: 8b36f75b8931ae3f656e2a8e63572080adc78513
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/05/2019
ms.locfileid: "70310316"
---
# <a name="grpc-for-aspnet-core-configuration"></a>gRPC para la configuración de ASP.NET Core

## <a name="configure-services-options"></a>Configurar opciones de servicios

En la tabla siguiente se describen las opciones para configurar los servicios de gRPC:

| Opción | Valor predeterminado | DESCRIPCIÓN |
| ------ | ------------- | ----------- |
| `MaxSendMessageSize` | `null` | Tamaño de mensaje máximo en bytes que se puede enviar desde el servidor. Al intentar enviar un mensaje que supera el tamaño máximo configurado del mensaje, se produce una excepción. |
| `MaxReceiveMessageSize` | 4 MB | Tamaño de mensaje máximo en bytes que puede recibir el servidor. Si el servidor recibe un mensaje que supera este límite, se produce una excepción. Aumentar este valor permite que el servidor reciba mensajes de mayor tamaño, pero puede afectar negativamente al consumo de memoria. |
| `EnableDetailedErrors` | `false` | Si `true`es, los mensajes de excepción detallados se devuelven a los clientes cuando se produce una excepción en un método de servicio. El valor predeterminado es `false`. Si `EnableDetailedErrors` se `true` establece en, se puede perder información confidencial. |
| `CompressionProviders` | gzip, DEFLATE | Colección de proveedores de compresión usados para comprimir y descomprimir mensajes. Los proveedores de compresión personalizados se pueden crear y agregar a la colección. Los proveedores configurados de forma predeterminada admiten **gzip** y la compresión **DEFLATE** . |
| `ResponseCompressionAlgorithm` | `null` | Algoritmo de compresión que se usa para comprimir los mensajes enviados desde el servidor. El algoritmo debe coincidir con un proveedor `CompressionProviders`de compresión en. Para que el algoritmo comprima una respuesta, el cliente debe indicar que admite el algoritmo enviándolo en el encabezado **GRPC-Accept-Encoding** . |
| `ResponseCompressionLevel` | `null` | El nivel de compresión utilizado para comprimir los mensajes enviados desde el servidor. |

Las opciones se pueden configurar para todos los servicios proporcionando un delegado de opciones `AddGrpc` a la `Startup.ConfigureServices`llamada en:

[!code-csharp[](~/grpc/configuration/sample/GrcpService/Startup.cs?name=snippet)]

Las opciones de un servicio único invalidan las opciones `AddGrpc` globales proporcionadas en y `AddServiceOptions<TService>`se pueden configurar mediante:

[!code-csharp[](~/grpc/configuration/sample/GrcpService/Startup2.cs?name=snippet)]

## <a name="configure-client-options"></a>Configurar opciones de cliente

En la tabla siguiente se describen las opciones para configurar los canales gRPC:

| Opción | Valor predeterminado | DESCRIPCIÓN |
| ------ | ------------- | ----------- |
| `HttpClient` | Nueva instancia | Que `HttpClient` se usa para realizar llamadas a gRPC. Se puede establecer un cliente para configurar un personalizado `HttpClientHandler`o agregar controladores adicionales a la canalización http para llamadas gRPC. De forma predeterminada, `HttpClient` se crea una nueva instancia. |
| `MaxSendMessageSize` | `null` | Tamaño de mensaje máximo en bytes que se puede enviar desde el cliente. Al intentar enviar un mensaje que supera el tamaño máximo configurado del mensaje, se produce una excepción. |
| `MaxReceiveMessageSize` | 4 MB | Tamaño de mensaje máximo en bytes que puede recibir el cliente. Si el servidor recibe un mensaje que supera este límite, se produce una excepción. Aumentar este valor permite que el servidor reciba mensajes de mayor tamaño, pero puede afectar negativamente al consumo de memoria. |
| `TransportOptions` | `null` | Opciones de transporte configure el modo en que el canal llama al servicio gRPC. Actualmente, la única implementación `HttpClientTransport` es Options, que `HttpClient` permite especificar el usado por gRPC. |
| `Credentials` | `null` | Instancia de `ChannelCredentials`. Las credenciales se usan para agregar metadatos de autenticación a llamadas gRPC. |
| `CompressionProviders` | gzip, DEFLATE | Colección de proveedores de compresión usados para comprimir y descomprimir mensajes. Los proveedores de compresión personalizados se pueden crear y agregar a la colección. Los proveedores configurados de forma predeterminada admiten **gzip** y la compresión **DEFLATE** . |

El código siguiente:

* Establece el tamaño máximo del mensaje de envío y recepción en el canal.
* Crea un cliente.

[!code-csharp[](~/grpc/configuration/sample/Program.cs?name=snippet&highlight=3-8)]

## <a name="additional-resources"></a>Recursos adicionales

* <xref:grpc/aspnetcore>
* <xref:grpc/client>
* <xref:tutorials/grpc/grpc-start>
