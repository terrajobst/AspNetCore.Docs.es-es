---
title: configuración de gRPC para .NET
author: jamesnk
description: Obtenga información sobre cómo configurar gRPC para aplicaciones .NET.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 09/05/2019
uid: grpc/configuration
ms.openlocfilehash: 3ef92f10d914ef9fa3e13a7bdd5c863bab297f57
ms.sourcegitcommit: 73e255e846e414821b8cc20ffa3aec946735cd4e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/03/2019
ms.locfileid: "71925204"
---
# <a name="grpc-for-net-configuration"></a>configuración de gRPC para .NET

## <a name="configure-services-options"></a>Configurar opciones de servicios

los servicios de gRPC se `AddGrpc` configuran con en *Startup.CS*. En la tabla siguiente se describen las opciones para configurar los servicios de gRPC:

| Opción | Valor predeterminado | Descripción |
| ------ | ------------- | ----------- |
| `MaxSendMessageSize` | `null` | Tamaño de mensaje máximo en bytes que se puede enviar desde el servidor. Al intentar enviar un mensaje que supera el tamaño máximo configurado del mensaje, se produce una excepción. |
| `MaxReceiveMessageSize` | 4 MB | Tamaño de mensaje máximo en bytes que puede recibir el servidor. Si el servidor recibe un mensaje que supera este límite, se produce una excepción. Aumentar este valor permite que el servidor reciba mensajes de mayor tamaño, pero puede afectar negativamente al consumo de memoria. |
| `EnableDetailedErrors` | `false` | Si `true`es, los mensajes de excepción detallados se devuelven a los clientes cuando se produce una excepción en un método de servicio. De manera predeterminada, es `false`. Si `EnableDetailedErrors` se `true` establece en, se puede perder información confidencial. |
| `CompressionProviders` | gzip | Colección de proveedores de compresión usados para comprimir y descomprimir mensajes. Los proveedores de compresión personalizados se pueden crear y agregar a la colección. Los proveedores configurados de forma predeterminada admiten la compresión **gzip** . |
| `ResponseCompressionAlgorithm` | `null` | Algoritmo de compresión que se usa para comprimir los mensajes enviados desde el servidor. El algoritmo debe coincidir con un proveedor `CompressionProviders`de compresión en. Para que el algoritmo comprima una respuesta, el cliente debe indicar que admite el algoritmo enviándolo en el encabezado **GRPC-Accept-Encoding** . |
| `ResponseCompressionLevel` | `null` | El nivel de compresión utilizado para comprimir los mensajes enviados desde el servidor. |

Las opciones se pueden configurar para todos los servicios proporcionando un delegado de opciones `AddGrpc` a la `Startup.ConfigureServices`llamada en:

[!code-csharp[](~/grpc/configuration/sample/GrcpService/Startup.cs?name=snippet)]

Las opciones de un servicio único invalidan las opciones `AddGrpc` globales proporcionadas en y `AddServiceOptions<TService>`se pueden configurar mediante:

[!code-csharp[](~/grpc/configuration/sample/GrcpService/Startup2.cs?name=snippet)]

## <a name="configure-client-options"></a>Configurar opciones de cliente

la configuración del cliente de gRPC `GrpcChannelOptions`está establecida en. En la tabla siguiente se describen las opciones para configurar los canales gRPC:

| Opción | Valor predeterminado | Descripción |
| ------ | ------------- | ----------- |
| `HttpClient` | Nueva instancia | Que `HttpClient` se usa para realizar llamadas a gRPC. Se puede establecer un cliente para configurar un personalizado `HttpClientHandler`o agregar controladores adicionales a la canalización http para llamadas gRPC. Si no `HttpClient` se especifica, se crea una `HttpClient` nueva instancia para el canal. Se eliminará automáticamente. |
| `DisposeHttpClient` | `false` | Si `true` `HttpClient` se especifica y, se eliminará la `HttpClient` `GrpcChannel` instancia de cuando se elimine. |
| `LoggerFactory` | `null` | Utilizado `LoggerFactory` por el cliente para registrar información acerca de las llamadas a gRPC. Una `LoggerFactory` instancia se puede resolver a partir de la inserción de `LoggerFactory.Create`dependencias o se puede crear con. Para obtener ejemplos de cómo configurar el <xref:grpc/diagnostics#grpc-client-logging>registro, vea. |
| `MaxSendMessageSize` | `null` | Tamaño de mensaje máximo en bytes que se puede enviar desde el cliente. Al intentar enviar un mensaje que supera el tamaño máximo configurado del mensaje, se produce una excepción. |
| `MaxReceiveMessageSize` | 4 MB | Tamaño de mensaje máximo en bytes que puede recibir el cliente. Si el cliente recibe un mensaje que supera este límite, produce una excepción. Aumentar este valor permite al cliente recibir mensajes de mayor tamaño, pero puede afectar negativamente al consumo de memoria. |
| `Credentials` | `null` | Instancia de `ChannelCredentials`. Las credenciales se usan para agregar metadatos de autenticación a llamadas gRPC. |
| `CompressionProviders` | gzip | Colección de proveedores de compresión usados para comprimir y descomprimir mensajes. Los proveedores de compresión personalizados se pueden crear y agregar a la colección. Los proveedores configurados de forma predeterminada admiten la compresión **gzip** . |

El código siguiente:

* Establece el tamaño máximo del mensaje de envío y recepción en el canal.
* Crea un cliente.

[!code-csharp[](~/grpc/configuration/sample/Program.cs?name=snippet&highlight=3-8)]

[!INCLUDE[](~/includes/gRPCazure.md)]

## <a name="additional-resources"></a>Recursos adicionales

* <xref:grpc/aspnetcore>
* <xref:grpc/client>
* <xref:grpc/diagnostics>
* <xref:tutorials/grpc/grpc-start>
