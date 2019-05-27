---
title: Servicios gRPC con ASP.NET Core
author: juntaoluo
description: Obtenga información sobre los conceptos básicos al escribir servicios gRPC con ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 03/08/2019
uid: grpc/aspnetcore
ms.openlocfilehash: 1f019fac23982a95fa37d43099522f4b3e9d107a
ms.sourcegitcommit: 5d384db2fa9373a93b5d15e985fb34430e49ad7a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66039281"
---
# <a name="grpc-services-with-aspnet-core"></a>Servicios gRPC con ASP.NET Core

Este documento muestra cómo empezar a trabajar con servicios gRPC mediante ASP.NET Core.

[!INCLUDE[](~/includes/net-core-prereqs-all-3.0.md)]

## <a name="get-started-with-grpc-service-in-aspnet-core"></a>Introducción al servicio gRPC en ASP.NET Core

[!INCLUDE[View or download sample code](~/includes/grpc/download.md)]

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Consulte [empezar a trabajar con servicios gRPC](xref:tutorials/grpc/grpc-start) para obtener instrucciones detalladas sobre cómo crear un proyecto gRPC.

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Visual Studio para Mac](#tab/visual-studio-code+visual-studio-mac)

Ejecute `dotnet new grpc -o GrpcGreeter` desde la línea de comandos.

---

## <a name="add-grpc-services-to-an-aspnet-core-app"></a>Agregar servicios de gRPC a una aplicación ASP.NET Core

gRPC requiere los siguientes paquetes:

* [Grpc.AspNetCore.Server](https://www.nuget.org/packages/Grpc.AspNetCore.Server)
* [Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/) para protobuf las API del mensaje.
* [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/)

### <a name="configure-grpc"></a>Configurar gRPC

gRPC está habilitado con el `AddGrpc` método:

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeter/Startup.cs?name=snippet&highlight=5)]

Cada servicio gRPC se agrega a la canalización de enrutamiento a través de la `MapGrpcService` método:

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeter/Startup.cs?name=snippet&highlight=21)]

Características y middleware de ASP.NET Core comparten la canalización de enrutamiento, por lo tanto, una aplicación puede configurarse para dar servicio a los controladores de solicitudes adicionales. Los controladores de solicitud adicionales, como controladores de MVC, trabajaran en paralelo con los servicios configurados gRPC.

## <a name="integration-with-aspnet-core-apis"></a>Integración con API de ASP.NET Core

gRPC services tienen acceso completo a las características de ASP.NET Core como [inserción de dependencias](xref:fundamentals/dependency-injection) (DI) y [registro](xref:fundamentals/logging/index). Por ejemplo, la implementación del servicio puede resolver un servicio de registrador desde el contenedor de DI a través del constructor:

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

De forma predeterminada, la implementación del servicio gRPC puede resolver otros servicios de inserción de dependencias con cualquier duración (Singleton, en el ámbito o transitorio).

### <a name="resolve-httpcontext-in-grpc-methods"></a>Resolver HttpContext en gRPC métodos

La API gRPC proporciona acceso a algunos datos de mensaje HTTP/2, como el método, host, encabezado y finalizadores. Es el acceso a través de la `ServerCallContext` argumento pasado a cada método gRPC:

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeter/Services/GreeterService.cs?highlight=3-4&name=snippet)]

`ServerCallContext` no proporciona acceso completo a `HttpContext` en todas las APIs ASP.NET. El `GetHttpContext` método de extensión proporciona acceso completo a la `HttpContext` que representa el mensaje de HTTP/2 subyacente en ASP.NET APIs:

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeter/Services/GreeterService.cs?name=snippet1)]

## <a name="additional-resources"></a>Recursos adicionales

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
