---
title: gRPC en aplicaciones de explorador
author: jamesnk
description: Aprenda a configurar los servicios de gRPC en ASP.NET Core a los que se puede llamar desde aplicaciones del explorador mediante gRPC-Web.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 01/24/2020
uid: grpc/browser
ms.openlocfilehash: 6359c3b76b3cb1ba2b6d9f9a989f64cbf4c4379d
ms.sourcegitcommit: b5ceb0a46d0254cc3425578116e2290142eec0f0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/28/2020
ms.locfileid: "76830637"
---
# <a name="grpc-in-browser-apps"></a>gRPC en aplicaciones de explorador

Por [James Newton-King](https://twitter.com/jamesnk)

> [!IMPORTANT]
> **gRPC: compatibilidad Web en .NET es experimental**
>
> gRPC-web para .NET es un proyecto experimental, no un producto confirmado. Queremos:
>
> * Pruebe que nuestro enfoque para implementar gRPC-web funciona.
> * Obtenga comentarios sobre si este enfoque es útil para los desarrolladores de .NET en comparación con la manera tradicional de configurar gRPC-web a través de un proxy.
>
> Deje comentarios en [https://github.com/grpc/grpc-dotnet](https://github.com/grpc/grpc-dotnet) para asegurarse de que crea algo que los desarrolladores desean y son productivos con.

No es posible llamar a un servicio gRPC de HTTP/2 desde una aplicación basada en explorador. [gRPC-web](https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-WEB.md) es un protocolo que permite a las aplicaciones de JavaScript y increíbles del explorador llamar a gRPC Services. En este artículo se explica cómo usar gRPC-Web en .NET Core.

## <a name="configure-grpc-web-in-aspnet-core"></a>Configuración de gRPC-Web en ASP.NET Core

gRPC Services hospedados en ASP.NET Core se pueden configurar para admitir gRPC-web junto con HTTP/2 gRPC. gRPC-web no requiere ningún cambio en los servicios. La única modificación es la configuración de inicio.

Para habilitar gRPC-web con un servicio de gRPC ASP.NET Core:

* Agregue una referencia al paquete [GRPC. AspNetCore. Web](https://www.nuget.org/packages/Grpc.AspNetCore.Web) .
* Configure la aplicación para que use gRPC-Web agregando `AddGrpcWeb` y `UseGrpcWeb` a *Startup.CS*:

[!code-csharp[](~/grpc/browser/sample/Startup.cs?name=snippet_1&highlight=3,10,14)]

El código anterior:

* Agrega el middleware gRPC-Web, `UseGrpcWeb`, después del enrutamiento y antes de los extremos.
* Especifica que el método `endpoints.MapGrpcService<GreeterService>()` admite gRPC-web con `EnableGrpcWeb`. 

Como alternativa, configure todos los servicios para admitir gRPC-Web agregando `services.AddGrpcWeb(o => o.GrpcWebEnabled = true);` a ConfigureServices.

[!code-csharp[](~/grpc/browser/sample/AllServicesSupportExample_Startup.cs?name=snippet_1&highlight=5,12,16)]

Es posible que sea necesario realizar alguna configuración adicional para llamar a gRPC-web desde el explorador, como la configuración de ASP.NET Core para que sea compatible con CORS. Para obtener más información, consulte [compatibilidad con CORS](xref:security/cors).

## <a name="call-grpc-web-from-the-browser"></a>Llamar a gRPC-web desde el explorador

Las aplicaciones de explorador pueden usar gRPC-web para llamar a los servicios de gRPC. Hay algunos requisitos y limitaciones al llamar a gRPC Services con gRPC-web desde el explorador:

* El servidor debe estar configurado para admitir gRPC-Web.
* No se admite la transmisión por secuencias de cliente y las llamadas de streaming bidireccional. Se admite la transmisión por secuencias del servidor.
* La llamada a gRPC Services en un dominio diferente requiere que [CORS](xref:security/cors) esté configurado en el servidor.

### <a name="javascript-grpc-web-client"></a>GRPC de JavaScript: cliente web

Hay un cliente web gRPC de JavaScript. Para obtener instrucciones sobre cómo usar gRPC-web desde JavaScript, consulte [escritura de código de cliente de JavaScript con gRPC-web](https://github.com/grpc/grpc-web/tree/master/net/grpc/gateway/examples/helloworld#write-client-code).

### <a name="configure-grpc-web-with-the-net-grpc-client"></a>Configuración de gRPC-web con el cliente de gRPC de .NET

El cliente gRPC de .NET puede configurarse para realizar llamadas gRPC-Web. Esto resulta útil para las aplicaciones de [Webassembly increíbles](xref:blazor/index#blazor-webassembly) , que se hospedan en el explorador y tienen las mismas limitaciones de http del código JavaScript. Llamar a gRPC-web con un cliente .NET es [igual que http/2 gRPC](xref:grpc/client). La única modificación es cómo se crea el canal.

Para usar gRPC-Web:

* Agregue una referencia al paquete [GRPC .net. Client. Web](https://www.nuget.org/packages/Grpc.Net.Client.Web) .
* Configure el canal para usar el `GrpcWebHandler`:

[!code-csharp[](~/grpc/browser/sample/Handler.cs?name=snippet_1)]

El código anterior:

* Configura un canal para utilizar gRPC-Web.
* Crea un cliente y realiza una llamada mediante el canal.

La `GrpcWebHandler` tiene las siguientes opciones de configuración cuando se crea:

* **InnerHandler**: el <xref:System.Net.Http.HttpMessageHandler> subyacente que realiza la llamada http, por ejemplo, `HttpClientHandler`.
* **Mode**: `GrpcWebMode` enum. `GrpcWebMode.GrpcWebText` configura el contenido para que esté codificado en Base64, lo que es necesario para admitir llamadas de transmisión por secuencias del servidor.
* **HttpVersion**: `Version`del protocolo http. gRPC-web no requiere un protocolo específico y no especificará uno al efectuar una solicitud, a menos que se configure.

## <a name="additional-resources"></a>Recursos adicionales

* [gRPC para el proyecto de GitHub de clientes Web](https://github.com/grpc/grpc-web)
* <xref:security/cors>
