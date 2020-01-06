---
title: configuración de gRPC para .NET
author: jamesnk
description: Obtenga información sobre cómo configurar gRPC para aplicaciones .NET.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 09/05/2019
uid: grpc/configuration
ms.openlocfilehash: de478752f94713d7f3d55ed66e6410500c3a72e8
ms.sourcegitcommit: 2cb857f0de774df421e35289662ba92cfe56ffd1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2019
ms.locfileid: "75355729"
---
# <a name="grpc-for-net-configuration"></a>configuración de gRPC para .NET

## <a name="configure-services-options"></a>Configurar opciones de servicios

los servicios de gRPC se configuran con `AddGrpc` en *Startup.CS*. En la tabla siguiente se describen las opciones para configurar los servicios de gRPC:

| Opción | Valor predeterminado | Descripción |
| ------ | ------------- | ----------- |
| `MaxSendMessageSize` | `null` | Tamaño de mensaje máximo en bytes que se puede enviar desde el servidor. Al intentar enviar un mensaje que supera el tamaño máximo configurado del mensaje, se produce una excepción. |
| `MaxReceiveMessageSize` | 4 MB | Tamaño de mensaje máximo en bytes que puede recibir el servidor. Si el servidor recibe un mensaje que supera este límite, se produce una excepción. Aumentar este valor permite que el servidor reciba mensajes de mayor tamaño, pero puede afectar negativamente al consumo de memoria. |
| `EnableDetailedErrors` | `false` | Si `true`, los mensajes de excepción detallados se devuelven a los clientes cuando se produce una excepción en un método de servicio. De manera predeterminada, es `false`. Establecer `EnableDetailedErrors` en `true` puede perder información confidencial. |
| `CompressionProviders` | gzip | Colección de proveedores de compresión usados para comprimir y descomprimir mensajes. Los proveedores de compresión personalizados se pueden crear y agregar a la colección. Los proveedores configurados de forma predeterminada admiten la compresión **gzip** . |
| `ResponseCompressionAlgorithm` | `null` | Algoritmo de compresión que se usa para comprimir los mensajes enviados desde el servidor. El algoritmo debe coincidir con un proveedor de compresión en `CompressionProviders`. Para que el algoritmo comprima una respuesta, el cliente debe indicar que admite el algoritmo enviándolo en el encabezado **GRPC-Accept-Encoding** . |
| `ResponseCompressionLevel` | `null` | El nivel de compresión utilizado para comprimir los mensajes enviados desde el servidor. |
| `Interceptors` | Ninguno | Colección de interceptores que se ejecutan con cada llamada a gRPC. Los interceptores se ejecutan en el orden en que se registran. Los interceptores configurados globalmente se ejecutan antes que los interceptores configurados para un servicio único. Para obtener más información sobre los interceptores de gRPC, consulte [interceptores de gRPC frente a middleware](xref:grpc/migration#grpc-interceptors-vs-middleware). |

Las opciones se pueden configurar para todos los servicios proporcionando un delegado de opciones a la llamada `AddGrpc` en `Startup.ConfigureServices`:

[!code-csharp[](~/grpc/configuration/sample/GrcpService/Startup.cs?name=snippet)]

Las opciones de un servicio único invalidan las opciones globales proporcionadas en `AddGrpc` y se pueden configurar mediante `AddServiceOptions<TService>`:

[!code-csharp[](~/grpc/configuration/sample/GrcpService/Startup2.cs?name=snippet)]

## <a name="configure-client-options"></a>Configurar opciones de cliente

la configuración del cliente gRPC se establece en `GrpcChannelOptions`. En la tabla siguiente se describen las opciones para configurar los canales gRPC:

| Opción | Valor predeterminado | Descripción |
| ------ | ------------- | ----------- |
| `HttpClient` | Nueva instancia | `HttpClient` que se usa para realizar llamadas a gRPC. Se puede establecer un cliente para configurar un `HttpClientHandler`personalizado o agregar controladores adicionales a la canalización HTTP para llamadas gRPC. Si no se especifica ningún `HttpClient`, se creará una nueva instancia de `HttpClient` para el canal. Se eliminará automáticamente. |
| `DisposeHttpClient` | `false` | Si `true`y se especifica un `HttpClient`, se eliminará la instancia de `HttpClient` cuando se elimine el `GrpcChannel`. |
| `LoggerFactory` | `null` | `LoggerFactory` que usa el cliente para registrar información acerca de las llamadas a gRPC. Una instancia de `LoggerFactory` se puede resolver a partir de la inserción de dependencias o crearse mediante `LoggerFactory.Create`. Para obtener ejemplos de cómo configurar el registro, consulte <xref:grpc/diagnostics#grpc-client-logging>. |
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
