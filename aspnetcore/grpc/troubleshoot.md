---
title: Solución de problemas de gRPC en .NET Core
author: jamesnk
description: Solución de problemas de errores al usar gRPC en .NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 08/26/2019
uid: grpc/troubleshoot
ms.openlocfilehash: e0c12aac083bc2e13f66831e756f2a93b7ee76b0
ms.sourcegitcommit: 8b36f75b8931ae3f656e2a8e63572080adc78513
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/05/2019
ms.locfileid: "70310446"
---
# <a name="troubleshoot-grpc-on-net-core"></a>Solución de problemas de gRPC en .NET Core

Por [James Newton-King](https://twitter.com/jamesnk)

En este documento se describen los problemas que suelen surgir al desarrollar aplicaciones de gRPC en .NET.

## <a name="mismatch-between-client-and-service-ssltls-configuration"></a>La configuración de SSL/TLS de cliente y servicio no coincide

La plantilla gRPC y los ejemplos usan la seguridad de la [capa de transporte (TLS)](https://tools.ietf.org/html/rfc5246) para proteger los servicios de gRPC de forma predeterminada. los clientes de gRPC deben usar una conexión segura para llamar correctamente a los servicios de gRPC protegidos.

Puede comprobar la ASP.NET Core servicio gRPC usa TLS en los registros escritos al iniciar la aplicación. El servicio escuchará en un punto de conexión HTTPS:

```
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: https://localhost:5001
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
```

El cliente de .net Core debe `https` usar en la dirección del servidor para realizar llamadas con una conexión segura:

```csharp
static async Task Main(string[] args)
{
    // The port number(5001) must match the port of the gRPC server.
    var channel = GrpcChannel.ForAddress("https://localhost:5001");
    var client = new Greet.GreeterClient(channel);
}
```

Todas las implementaciones de cliente de gRPC admiten TLS. los clientes de gRPC de otros idiomas normalmente requieren el canal `SslCredentials`configurado con. `SslCredentials`especifica el certificado que usará el cliente y debe usarse en lugar de credenciales no seguras. Para obtener ejemplos de configuración de las distintas implementaciones de cliente de gRPC para usar TLS, consulte [autenticación de gRPC](https://www.grpc.io/docs/guides/auth/).

## <a name="call-a-grpc-service-with-an-untrustedinvalid-certificate"></a>Llamar a un servicio de gRPC con un certificado no confiable o no válido

El cliente gRPC de .NET requiere que el servicio tenga un certificado de confianza. El siguiente mensaje de error se devuelve al llamar a un servicio de gRPC sin un certificado de confianza:

> Excepción no controlada. System .net. http. HttpRequestException: No se pudo establecer la conexión SSL; vea la excepción interna.
> ---> System. Security. Authentication. AuthenticationException: El certificado remoto no es válido según el procedimiento de validación.

Puede ver este error si está probando la aplicación localmente y el certificado de desarrollo de HTTPS ASP.NET Core no es de confianza. Para instrucciones sobre cómo corregir este problema, consulte [Confiar en el certificado de desarrollo de ASP.NET Core HTTPS en Windows y macOS](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos).

Si está llamando a un servicio de gRPC en otro equipo y no puede confiar en el certificado, el cliente de gRPC se puede configurar para omitir el certificado no válido. En el código siguiente se usa [HttpClientHandler. ServerCertificateCustomValidationCallback](/dotnet/api/system.net.http.httpclienthandler.servercertificatecustomvalidationcallback) para permitir llamadas sin un certificado de confianza:

```csharp
var httpClientHandler = new HttpClientHandler();
// Return `true` to allow certificates that are untrusted/invalid
httpClientHandler.ServerCertificateCustomValidationCallback = (message, cert, chain, errors) => true;

var httpClient = new HttpClient(httpClientHandler);
httpClient.BaseAddress = new Uri("https://localhost:5001");
var client = GrpcClient.Create<Greeter.GreeterClient>(httpClient);
```

> [!WARNING]
> Los certificados que no son de confianza solo se deben usar durante el desarrollo de aplicaciones. Las aplicaciones de producción siempre deben usar certificados válidos.

## <a name="call-insecure-grpc-services-with-net-core-client"></a>Llamada a servicios de gRPC inseguros con el cliente de .NET Core

Se requiere configuración adicional para llamar a los servicios de gRPC inseguros con el cliente de .NET Core. El cliente gRPC debe establecer el `System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport` `true` modificador en y `http` usarlo en la dirección del servidor:

```csharp
// This switch must be set before creating the GrpcChannel/HttpClient.
AppContext.SetSwitch("System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport", true);

// The port number(5000) must match the port of the gRPC server.
var channel = GrpcChannel.ForAddress("https://localhost:5001");
var client = new Greet.GreeterClient(channel);
```

## <a name="unable-to-start-aspnet-core-grpc-app-on-macos"></a>No se puede iniciar ASP.NET Core aplicación gRPC en macOS

Kestrel no admite HTTP/2 con TLS en macOS y versiones anteriores de Windows, como Windows 7. La plantilla ASP.NET Core gRPC y los ejemplos usan TLS de forma predeterminada. Verá el siguiente mensaje de error al intentar iniciar el servidor de gRPC:

> No se puede enlazar a https://localhost:5001 en la interfaz de bucle invertido IPv4: ' HTTP/2 a través de TLS no se admite en macOS debido a la falta de compatibilidad con ALPN. '.

Para solucionar este problema, configure Kestrel y el cliente de gRPC para que use HTTP/2 *sin* TLS. Esto solo debe realizarse durante el desarrollo. Si no se usa TLS, se enviarán mensajes gRPC sin cifrado.

Kestrel debe configurar un punto de conexión HTTP/2 sin TLS en *Program.CS*:

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

Cuando un punto de conexión HTTP/2 se configura sin TLS, el [ListenOptions. Protocols](xref:fundamentals/servers/kestrel#listenoptionsprotocols) del punto de `HttpProtocols.Http2`conexión debe establecerse en. `HttpProtocols.Http1AndHttp2`no se puede usar porque se requiere TLS para negociar HTTP/2. Sin TLS, se produce un error en todas las conexiones con el punto de conexión de forma predeterminada a HTTP/1.1 y las llamadas a gRPC.

El cliente de gRPC también debe estar configurado para no usar TLS. Para obtener más información, consulte [Call unsecure gRPC Services with .net Core Client](#call-insecure-grpc-services-with-net-core-client).

> [!WARNING]
> HTTP/2 sin TLS solo se debe usar durante el desarrollo de aplicaciones. Las aplicaciones de producción siempre deben usar la seguridad de transporte. Para obtener más información, vea [consideraciones de seguridad en gRPC para ASP.net Core](xref:grpc/security#transport-security).

## <a name="grpc-c-assets-are-not-code-generated-from-proto-files"></a>los C# recursos gRPC no se generan a partir de  *\*archivos. proto*

la generación de código gRPC de los clientes concretos y las clases base del servicio requiere que se haga referencia a los archivos y las herramientas de protobuf desde un proyecto. Debe incluir:

* archivos *. proto* que desea utilizar en el `<Protobuf>` grupo de elementos. El proyecto debe hacer referencia a [los archivos *. proto* importados](https://developers.google.com/protocol-buffers/docs/proto3#importing-definitions) .
* Referencia de paquete al paquete de herramientas de gRPC [gRPC. Tools](https://www.nuget.org/packages/Grpc.Tools/).

Para obtener más información sobre la C# generación de recursos <xref:grpc/basics>gRPC, vea.

De forma predeterminada, `<Protobuf>` una referencia genera un cliente concreto y una clase base de servicio. El atributo del elemento `GrpcServices` de referencia se puede utilizar para C# limitar la generación de recursos. Las `GrpcServices` opciones válidas son:

* `Both`(valor predeterminado si no está presente)
* `Server`
* `Client`
* `None`

Una ASP.NET Core aplicación web que hospeda gRPC Services solo necesita la clase base de servicio generada:

```xml
<ItemGroup>
  <Protobuf Include="Protos\greet.proto" GrpcServices="Server" />
</ItemGroup>
```

Una aplicación cliente de gRPC que realiza llamadas a gRPC solo necesita el cliente concreto generado:

```xml
<ItemGroup>
  <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
</ItemGroup>
```
