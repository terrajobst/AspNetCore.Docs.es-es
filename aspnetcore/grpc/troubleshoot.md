---
title: Solución de problemas de gRPC en .NET Core
author: jamesnk
description: Solución de problemas de errores al usar gRPC en .NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 10/16/2019
uid: grpc/troubleshoot
ms.openlocfilehash: c501cda14f3bac9297695ece59cbc4634e4b7895
ms.sourcegitcommit: e71b6a85b0e94a600af607107e298f932924c849
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2019
ms.locfileid: "72519054"
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

El cliente de .NET Core debe usar `https` en la dirección del servidor para realizar llamadas con una conexión segura:

```csharp
static async Task Main(string[] args)
{
    // The port number(5001) must match the port of the gRPC server.
    var channel = GrpcChannel.ForAddress("https://localhost:5001");
    var client = new Greet.GreeterClient(channel);
}
```

Todas las implementaciones de cliente de gRPC admiten TLS. los clientes de gRPC de otros idiomas normalmente requieren el canal configurado con `SslCredentials`. `SslCredentials` especifica el certificado que usará el cliente y debe usarse en lugar de credenciales no seguras. Para obtener ejemplos de configuración de las distintas implementaciones de cliente de gRPC para usar TLS, consulte [autenticación de gRPC](https://www.grpc.io/docs/guides/auth/).

## <a name="call-a-grpc-service-with-an-untrustedinvalid-certificate"></a>Llamar a un servicio de gRPC con un certificado no confiable o no válido

El cliente gRPC de .NET requiere que el servicio tenga un certificado de confianza. El siguiente mensaje de error se devuelve al llamar a un servicio de gRPC sin un certificado de confianza:

> Excepción no controlada. System .net. http. HttpRequestException: no se pudo establecer la conexión SSL; vea la excepción interna.
> ---> System. Security. Authentication. AuthenticationException: el certificado remoto no es válido según el procedimiento de validación.

Puede ver este error si está probando la aplicación localmente y el certificado de desarrollo de HTTPS ASP.NET Core no es de confianza. Para instrucciones sobre cómo corregir este problema, consulte [Confiar en el certificado de desarrollo de ASP.NET Core HTTPS en Windows y macOS](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos).

Si está llamando a un servicio de gRPC en otro equipo y no puede confiar en el certificado, el cliente de gRPC se puede configurar para omitir el certificado no válido. En el código siguiente se usa [HttpClientHandler. ServerCertificateCustomValidationCallback](/dotnet/api/system.net.http.httpclienthandler.servercertificatecustomvalidationcallback) para permitir llamadas sin un certificado de confianza:

```csharp
var httpClientHandler = new HttpClientHandler();
// Return `true` to allow certificates that are untrusted/invalid
httpClientHandler.ServerCertificateCustomValidationCallback = 
    HttpClientHandler.DangerousAcceptAnyServerCertificateValidator;
var httpClient = new HttpClient(httpClientHandler);

var channel = GrpcChannel.ForAddress("https://localhost:5001",
    new GrpcChannelOptions { HttpClient = httpClient });
var client = new Greet.GreeterClient(channel);
```

> [!WARNING]
> Los certificados que no son de confianza solo se deben usar durante el desarrollo de aplicaciones. Las aplicaciones de producción siempre deben usar certificados válidos.

## <a name="call-insecure-grpc-services-with-net-core-client"></a>Llamada a servicios de gRPC inseguros con el cliente de .NET Core

Se requiere configuración adicional para llamar a los servicios de gRPC inseguros con el cliente de .NET Core. El cliente gRPC debe establecer el modificador `System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport` en `true` y usar `http` en la dirección del servidor:

```csharp
// This switch must be set before creating the GrpcChannel/HttpClient.
AppContext.SetSwitch(
    "System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport", true);

// The port number(5000) must match the port of the gRPC server.
var channel = GrpcChannel.ForAddress("http://localhost:5000");
var client = new Greet.GreeterClient(channel);
```

## <a name="unable-to-start-aspnet-core-grpc-app-on-macos"></a>No se puede iniciar ASP.NET Core aplicación gRPC en macOS

Kestrel no admite HTTP/2 con TLS en macOS y versiones anteriores de Windows, como Windows 7. La plantilla ASP.NET Core gRPC y los ejemplos usan TLS de forma predeterminada. Verá el siguiente mensaje de error al intentar iniciar el servidor de gRPC:

> No se puede enlazar a https://localhost:5001 en la interfaz de bucle invertido IPv4: ' HTTP/2 a través de TLS no se admite en macOS debido a que falta compatibilidad con ALPN. '.

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
                options.ListenLocalhost(5000, o => o.Protocols = 
                    HttpProtocols.Http2);
            });
            webBuilder.UseStartup<Startup>();
        });
```

Cuando se configura un punto de conexión HTTP/2 sin TLS, los [protocolos ListenOptions. Protocol](xref:fundamentals/servers/kestrel#listenoptionsprotocols) del punto de conexión se deben establecer en `HttpProtocols.Http2`. no se puede usar `HttpProtocols.Http1AndHttp2` porque se requiere TLS para negociar HTTP/2. Sin TLS, se produce un error en todas las conexiones con el punto de conexión de forma predeterminada a HTTP/1.1 y las llamadas a gRPC.

El cliente de gRPC también debe estar configurado para no usar TLS. Para obtener más información, consulte [Call unsecure gRPC Services with .net Core Client](#call-insecure-grpc-services-with-net-core-client).

> [!WARNING]
> HTTP/2 sin TLS solo se debe usar durante el desarrollo de aplicaciones. Las aplicaciones de producción siempre deben usar la seguridad de transporte. Para obtener más información, vea [consideraciones de seguridad en gRPC para ASP.net Core](xref:grpc/security#transport-security).

## <a name="grpc-c-assets-are-not-code-generated-from-proto-files"></a>los C# recursos gRPC no se generan a partir de archivos. proto

la generación de código gRPC de los clientes concretos y las clases base del servicio requiere que se haga referencia a los archivos y las herramientas de protobuf desde un proyecto. Debe incluir:

* archivos *. proto* que quiere usar en el grupo de elementos `<Protobuf>`. El proyecto debe hacer referencia a [los archivos *. proto* importados](https://developers.google.com/protocol-buffers/docs/proto3#importing-definitions) .
* Referencia de paquete al paquete de herramientas de gRPC [gRPC. Tools](https://www.nuget.org/packages/Grpc.Tools/).

Para obtener más información sobre la C# generación de recursos gRPC, consulte <xref:grpc/basics>.

De forma predeterminada, una referencia `<Protobuf>` genera un cliente concreto y una clase base de servicio. El atributo `GrpcServices` del elemento de referencia se puede utilizar para C# limitar la generación de activos. Las opciones válidas de `GrpcServices` son:

* `Both` (valor predeterminado si no está presente)
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

## <a name="wpf-projects-unable-to-generate-grpc-c-assets-from-proto-files"></a>Los proyectos de WPF no pueden C# generar recursos gRPC a partir de archivos. proto

Los proyectos de WPF tienen un [problema conocido](https://github.com/dotnet/wpf/issues/810) que evita que la generación de código de gRPC funcione correctamente. Los tipos gRPC generados en un proyecto de WPF haciendo referencia a los archivos `Grpc.Tools` y *. proto* crearán errores de compilación cuando se usen:

> error CS0246: no se encontró el tipo o el nombre de espacio de nombres ' MyGrpcServices ' (¿falta una directiva using o una referencia de ensamblado?)

Para solucionar este problema, puede hacer lo siguiente:

1. Cree un nuevo proyecto de biblioteca de clases de .NET Core.
2. En el nuevo proyecto, agregue referencias para habilitar [ C# la generación de código desde *@no__t archivos -3. proto* ](xref:grpc/basics#generated-c-assets):
    * Agregue una referencia de paquete al paquete [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/).
    * Agregue archivos *\*.proto* al grupo de elementos `<Protobuf>`.
3. En la aplicación WPF, agregue una referencia al nuevo proyecto.

La aplicación WPF puede usar los tipos generados por gRPC del nuevo proyecto de biblioteca de clases.

[!INCLUDE[](~/includes/gRPCazure.md)]
