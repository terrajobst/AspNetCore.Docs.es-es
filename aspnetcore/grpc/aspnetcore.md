---
title: Servicios gRPC con ASP.NET Core
author: juntaoluo
description: Conozca los conceptos básicos al escribir servicios de gRPC con ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 08/07/2019
uid: grpc/aspnetcore
ms.openlocfilehash: 26f0d7610151460967b97665ed61deab1ef56d68
ms.sourcegitcommit: 2719c70cd15a430479ab4007ff3e197fbf5dfee0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/09/2019
ms.locfileid: "68862940"
---
# <a name="grpc-services-with-aspnet-core"></a>Servicios gRPC con ASP.NET Core

En este documento se muestra cómo empezar a trabajar con gRPC Services mediante ASP.NET Core.

## <a name="prerequisites"></a>Requisitos previos

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio para Mac](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="get-started-with-grpc-service-in-aspnet-core"></a>Introducción al servicio gRPC en ASP.NET Core

[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample)).

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Vea Introducción [a gRPC Services](xref:tutorials/grpc/grpc-start) para obtener instrucciones detalladas sobre cómo crear un proyecto de gRPC.

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Visual Studio para Mac](#tab/visual-studio-code+visual-studio-mac)

Ejecute `dotnet new grpc -o GrpcGreeter` desde la línea de comandos.

---

## <a name="add-grpc-services-to-an-aspnet-core-app"></a>Incorporación de gRPC Services a una aplicación ASP.NET Core

gRPC requiere el paquete [gRPC. AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore) .

### <a name="configure-grpc"></a>Configuración de gRPC

gRPC se habilita con el `AddGrpc` método:

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=7)]

Cada servicio gRPC se agrega a la canalización de enrutamiento `MapGrpcService` a través del método:

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=24)]

ASP.NET Core los middleware y las características comparten la canalización de enrutamiento, por lo que se puede configurar una aplicación para que atienda a los controladores de solicitud adicionales. Los controladores de solicitud adicionales, como los controladores MVC, funcionan en paralelo con los servicios gRPC configurados.

## <a name="integration-with-aspnet-core-apis"></a>Integración con API de ASP.NET Core

los servicios de gRPC tienen acceso completo a las características de ASP.NET Core, como la [inserción](xref:fundamentals/dependency-injection) de dependencias (di) y el [registro](xref:fundamentals/logging/index). Por ejemplo, la implementación del servicio puede resolver un servicio del registrador a partir del contenedor de DI mediante el constructor:

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

De forma predeterminada, la implementación del servicio gRPC puede resolver otros servicios de DI con cualquier duración (singleton, ámbito o transitorio).

### <a name="resolve-httpcontext-in-grpc-methods"></a>Resolver HttpContext en métodos gRPC

La API gRPC proporciona acceso a algunos datos de mensajes HTTP/2, como el método, el host, el encabezado y los finalizadores. El acceso se realiza `ServerCallContext` a través del argumento que se pasa a cada método gRPC:

[!code-csharp[](~/grpc/aspnetcore/sample/GrcpService/GreeterService.cs?highlight=3-4&name=snippet)]

`ServerCallContext`no proporciona acceso completo a `HttpContext` en todas las API de ASP.net. El `GetHttpContext` método de extensión proporciona acceso completo `HttpContext` a que representa el mensaje http/2 subyacente en las API de ASP.net:

[!code-csharp[](~/grpc/aspnetcore/sample/GrcpService/GreeterService2.cs?highlight=6-7&name=snippet)]

## <a name="grpc-and-aspnet-core-on-macos"></a>gRPC y ASP.NET Core en macOS

Kestrel no admite HTTP/2 con [seguridad de la capa de transporte (TLS)](https://tools.ietf.org/html/rfc5246) en MacOS. La plantilla ASP.NET Core gRPC y los ejemplos usan TLS de forma predeterminada. Verá el siguiente mensaje de error al intentar iniciar el servidor de gRPC:

> No se puede enlazar a https://localhost:5001 en la interfaz de bucle invertido IPv4: ' HTTP/2 a través de TLS no se admite en macOS debido a la falta de compatibilidad con ALPN. '.

Para solucionar este problema, configure Kestrel y el cliente de gRPC para que use HTTP/2 **sin** TLS. Esto solo debe realizarse durante el desarrollo. Si no se usa TLS, se enviarán mensajes gRPC sin cifrado.

Kestrel debe configurar un punto de conexión HTTP/2 sin `Program.cs`TLS en:

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.ConfigureKestrel(options =>
            {
                // Setup a HTTP/2 endpoint without TLS.
                options.ListenLocalhost(5000, o => o.Protocols = HttpProtocols.Http2);
            });
            webBuilder.UseStartup<Startup>();
        });
```

El cliente gRPC debe establecer el `System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport` `true` modificador en y `http` usarlo en la dirección del servidor:

```csharp
// This switch must be set before creating the HttpClient.
AppContext.SetSwitch("System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport", true);

var httpClient = new HttpClient();
// The port number(5000) must match the port of the gRPC server.
httpClient.BaseAddress = new Uri("http://localhost:5000");
var client = GrpcClient.Create<Greeter.GreeterClient>(httpClient);
```

> [!WARNING]
> HTTP/2 sin TLS solo se debe usar durante el desarrollo de aplicaciones. Las aplicaciones de producción siempre deben usar la seguridad de transporte. Para obtener más información, vea [consideraciones de seguridad en gRPC para ASP.net Core](xref:grpc/security#transport-security).

## <a name="additional-resources"></a>Recursos adicionales

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
