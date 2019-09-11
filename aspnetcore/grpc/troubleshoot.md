---
title: Solución de problemas de gRPC en .NET Core
author: jamesnk
description: Solución de problemas de errores al usar gRPC en .NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 09/05/2019
uid: grpc/troubleshoot
ms.openlocfilehash: 33864ceb18304eb1d3413bcc9aebacd6eaffdbc6
ms.sourcegitcommit: e7c56e8da5419bbc20b437c2dd531dedf9b0dc6b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70878497"
---
# <a name="troubleshoot-grpc-on-net-core"></a><span data-ttu-id="638e0-103">Solución de problemas de gRPC en .NET Core</span><span class="sxs-lookup"><span data-stu-id="638e0-103">Troubleshoot gRPC on .NET Core</span></span>

<span data-ttu-id="638e0-104">Por [James Newton-King](https://twitter.com/jamesnk)</span><span class="sxs-lookup"><span data-stu-id="638e0-104">By [James Newton-King](https://twitter.com/jamesnk)</span></span>

<span data-ttu-id="638e0-105">En este documento se describen los problemas que suelen surgir al desarrollar aplicaciones de gRPC en .NET.</span><span class="sxs-lookup"><span data-stu-id="638e0-105">This document discusses commonly encountered problems when developing gRPC apps on .NET.</span></span>

## <a name="mismatch-between-client-and-service-ssltls-configuration"></a><span data-ttu-id="638e0-106">La configuración de SSL/TLS de cliente y servicio no coincide</span><span class="sxs-lookup"><span data-stu-id="638e0-106">Mismatch between client and service SSL/TLS configuration</span></span>

<span data-ttu-id="638e0-107">La plantilla gRPC y los ejemplos usan la seguridad de la [capa de transporte (TLS)](https://tools.ietf.org/html/rfc5246) para proteger los servicios de gRPC de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="638e0-107">The gRPC template and samples use [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246) to secure gRPC services by default.</span></span> <span data-ttu-id="638e0-108">los clientes de gRPC deben usar una conexión segura para llamar correctamente a los servicios de gRPC protegidos.</span><span class="sxs-lookup"><span data-stu-id="638e0-108">gRPC clients need to use a secure connection to call secured gRPC services successfully.</span></span>

<span data-ttu-id="638e0-109">Puede comprobar la ASP.NET Core servicio gRPC usa TLS en los registros escritos al iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="638e0-109">You can verify the ASP.NET Core gRPC service is using TLS in the logs written on app start.</span></span> <span data-ttu-id="638e0-110">El servicio escuchará en un punto de conexión HTTPS:</span><span class="sxs-lookup"><span data-stu-id="638e0-110">The service will be listening on an HTTPS endpoint:</span></span>

```
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: https://localhost:5001
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
```

<span data-ttu-id="638e0-111">El cliente de .net Core debe `https` usar en la dirección del servidor para realizar llamadas con una conexión segura:</span><span class="sxs-lookup"><span data-stu-id="638e0-111">The .NET Core client must use `https` in the server address to make calls with a secured connection:</span></span>

```csharp
static async Task Main(string[] args)
{
    // The port number(5001) must match the port of the gRPC server.
    var channel = GrpcChannel.ForAddress("https://localhost:5001");
    var client = new Greet.GreeterClient(channel);
}
```

<span data-ttu-id="638e0-112">Todas las implementaciones de cliente de gRPC admiten TLS.</span><span class="sxs-lookup"><span data-stu-id="638e0-112">All gRPC client implementations support TLS.</span></span> <span data-ttu-id="638e0-113">los clientes de gRPC de otros idiomas normalmente requieren el canal `SslCredentials`configurado con.</span><span class="sxs-lookup"><span data-stu-id="638e0-113">gRPC clients from other languages typically require the channel configured with `SslCredentials`.</span></span> <span data-ttu-id="638e0-114">`SslCredentials`especifica el certificado que usará el cliente y debe usarse en lugar de credenciales no seguras.</span><span class="sxs-lookup"><span data-stu-id="638e0-114">`SslCredentials` specifies the certificate that the client will use, and it must be used instead of insecure credentials.</span></span> <span data-ttu-id="638e0-115">Para obtener ejemplos de configuración de las distintas implementaciones de cliente de gRPC para usar TLS, consulte [autenticación de gRPC](https://www.grpc.io/docs/guides/auth/).</span><span class="sxs-lookup"><span data-stu-id="638e0-115">For examples of configuring the different gRPC client implementations to use TLS, see [gRPC Authentication](https://www.grpc.io/docs/guides/auth/).</span></span>

## <a name="call-a-grpc-service-with-an-untrustedinvalid-certificate"></a><span data-ttu-id="638e0-116">Llamar a un servicio de gRPC con un certificado no confiable o no válido</span><span class="sxs-lookup"><span data-stu-id="638e0-116">Call a gRPC service with an untrusted/invalid certificate</span></span>

<span data-ttu-id="638e0-117">El cliente gRPC de .NET requiere que el servicio tenga un certificado de confianza.</span><span class="sxs-lookup"><span data-stu-id="638e0-117">The .NET gRPC client requires the service to have a trusted certificate.</span></span> <span data-ttu-id="638e0-118">El siguiente mensaje de error se devuelve al llamar a un servicio de gRPC sin un certificado de confianza:</span><span class="sxs-lookup"><span data-stu-id="638e0-118">The following error message is returned when calling a gRPC service without a trusted certificate:</span></span>

> <span data-ttu-id="638e0-119">Excepción no controlada.</span><span class="sxs-lookup"><span data-stu-id="638e0-119">Unhandled exception.</span></span> <span data-ttu-id="638e0-120">System .net. http. HttpRequestException: No se pudo establecer la conexión SSL; vea la excepción interna.</span><span class="sxs-lookup"><span data-stu-id="638e0-120">System.Net.Http.HttpRequestException: The SSL connection could not be established, see inner exception.</span></span>
> <span data-ttu-id="638e0-121">---> System. Security. Authentication. AuthenticationException: El certificado remoto no es válido según el procedimiento de validación.</span><span class="sxs-lookup"><span data-stu-id="638e0-121">---> System.Security.Authentication.AuthenticationException: The remote certificate is invalid according to the validation procedure.</span></span>

<span data-ttu-id="638e0-122">Puede ver este error si está probando la aplicación localmente y el certificado de desarrollo de HTTPS ASP.NET Core no es de confianza.</span><span class="sxs-lookup"><span data-stu-id="638e0-122">You may see this error if you are testing your app locally and the ASP.NET Core HTTPS development certificate is not trusted.</span></span> <span data-ttu-id="638e0-123">Para instrucciones sobre cómo corregir este problema, consulte [Confiar en el certificado de desarrollo de ASP.NET Core HTTPS en Windows y macOS](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos).</span><span class="sxs-lookup"><span data-stu-id="638e0-123">For instructions to fix this issue, see [Trust the ASP.NET Core HTTPS development certificate on Windows and macOS](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos).</span></span>

<span data-ttu-id="638e0-124">Si está llamando a un servicio de gRPC en otro equipo y no puede confiar en el certificado, el cliente de gRPC se puede configurar para omitir el certificado no válido.</span><span class="sxs-lookup"><span data-stu-id="638e0-124">If you are calling a gRPC service on another machine and are unable to trust the certificate then the gRPC client can be configured to ignore the invalid certificate.</span></span> <span data-ttu-id="638e0-125">En el código siguiente se usa [HttpClientHandler. ServerCertificateCustomValidationCallback](/dotnet/api/system.net.http.httpclienthandler.servercertificatecustomvalidationcallback) para permitir llamadas sin un certificado de confianza:</span><span class="sxs-lookup"><span data-stu-id="638e0-125">The following code uses [HttpClientHandler.ServerCertificateCustomValidationCallback](/dotnet/api/system.net.http.httpclienthandler.servercertificatecustomvalidationcallback) to allow calls without a trusted certificate:</span></span>

```csharp
var httpClientHandler = new HttpClientHandler();
// Return `true` to allow certificates that are untrusted/invalid
httpClientHandler.ServerCertificateCustomValidationCallback = (message, cert, chain, errors) => true;
var httpClient = new HttpClient(httpClientHandler);

var channel = GrpcChannel.ForAddress("https://localhost:5001",
    new GrpcChannelOptions { HttpClient = httpClient });
var client = new Greet.GreeterClient(channel);
```

> [!WARNING]
> <span data-ttu-id="638e0-126">Los certificados que no son de confianza solo se deben usar durante el desarrollo de aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="638e0-126">Untrusted certificates should only be used during app development.</span></span> <span data-ttu-id="638e0-127">Las aplicaciones de producción siempre deben usar certificados válidos.</span><span class="sxs-lookup"><span data-stu-id="638e0-127">Production apps should always use valid certificates.</span></span>

## <a name="call-insecure-grpc-services-with-net-core-client"></a><span data-ttu-id="638e0-128">Llamada a servicios de gRPC inseguros con el cliente de .NET Core</span><span class="sxs-lookup"><span data-stu-id="638e0-128">Call insecure gRPC services with .NET Core client</span></span>

<span data-ttu-id="638e0-129">Se requiere configuración adicional para llamar a los servicios de gRPC inseguros con el cliente de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="638e0-129">Additional configuration is required to call insecure gRPC services with the .NET Core client.</span></span> <span data-ttu-id="638e0-130">El cliente gRPC debe establecer el `System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport` `true` modificador en y `http` usarlo en la dirección del servidor:</span><span class="sxs-lookup"><span data-stu-id="638e0-130">The gRPC client must set the `System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport` switch to `true` and use `http` in the server address:</span></span>

```csharp
// This switch must be set before creating the GrpcChannel/HttpClient.
AppContext.SetSwitch("System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport", true);

// The port number(5000) must match the port of the gRPC server.
var channel = GrpcChannel.ForAddress("http://localhost:5000");
var client = new Greet.GreeterClient(channel);
```

## <a name="unable-to-start-aspnet-core-grpc-app-on-macos"></a><span data-ttu-id="638e0-131">No se puede iniciar ASP.NET Core aplicación gRPC en macOS</span><span class="sxs-lookup"><span data-stu-id="638e0-131">Unable to start ASP.NET Core gRPC app on macOS</span></span>

<span data-ttu-id="638e0-132">Kestrel no admite HTTP/2 con TLS en macOS y versiones anteriores de Windows, como Windows 7.</span><span class="sxs-lookup"><span data-stu-id="638e0-132">Kestrel doesn't support HTTP/2 with TLS on macOS and older Windows versions such as Windows 7.</span></span> <span data-ttu-id="638e0-133">La plantilla ASP.NET Core gRPC y los ejemplos usan TLS de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="638e0-133">The ASP.NET Core gRPC template and samples use TLS by default.</span></span> <span data-ttu-id="638e0-134">Verá el siguiente mensaje de error al intentar iniciar el servidor de gRPC:</span><span class="sxs-lookup"><span data-stu-id="638e0-134">You'll see the following error message when you attempt to start the gRPC server:</span></span>

> <span data-ttu-id="638e0-135">No se puede enlazar a https://localhost:5001 en la interfaz de bucle invertido IPv4: ' HTTP/2 a través de TLS no se admite en macOS debido a la falta de compatibilidad con ALPN. '.</span><span class="sxs-lookup"><span data-stu-id="638e0-135">Unable to bind to https://localhost:5001 on the IPv4 loopback interface: 'HTTP/2 over TLS is not supported on macOS due to missing ALPN support.'.</span></span>

<span data-ttu-id="638e0-136">Para solucionar este problema, configure Kestrel y el cliente de gRPC para que use HTTP/2 *sin* TLS.</span><span class="sxs-lookup"><span data-stu-id="638e0-136">To work around this issue, configure Kestrel and the gRPC client to use HTTP/2 *without* TLS.</span></span> <span data-ttu-id="638e0-137">Esto solo debe realizarse durante el desarrollo.</span><span class="sxs-lookup"><span data-stu-id="638e0-137">You should only do this during development.</span></span> <span data-ttu-id="638e0-138">Si no se usa TLS, se enviarán mensajes gRPC sin cifrado.</span><span class="sxs-lookup"><span data-stu-id="638e0-138">Not using TLS will result in gRPC messages being sent without encryption.</span></span>

<span data-ttu-id="638e0-139">Kestrel debe configurar un punto de conexión HTTP/2 sin TLS en *Program.CS*:</span><span class="sxs-lookup"><span data-stu-id="638e0-139">Kestrel must configure an HTTP/2 endpoint without TLS in *Program.cs*:</span></span>

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

<span data-ttu-id="638e0-140">Cuando un punto de conexión HTTP/2 se configura sin TLS, el [ListenOptions. Protocols](xref:fundamentals/servers/kestrel#listenoptionsprotocols) del punto de `HttpProtocols.Http2`conexión debe establecerse en.</span><span class="sxs-lookup"><span data-stu-id="638e0-140">When an HTTP/2 endpoint is configured without TLS, the endpoint's [ListenOptions.Protocols](xref:fundamentals/servers/kestrel#listenoptionsprotocols) must be set to `HttpProtocols.Http2`.</span></span> <span data-ttu-id="638e0-141">`HttpProtocols.Http1AndHttp2`no se puede usar porque se requiere TLS para negociar HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="638e0-141">`HttpProtocols.Http1AndHttp2` can't be used because TLS is required to negotiate HTTP/2.</span></span> <span data-ttu-id="638e0-142">Sin TLS, se produce un error en todas las conexiones con el punto de conexión de forma predeterminada a HTTP/1.1 y las llamadas a gRPC.</span><span class="sxs-lookup"><span data-stu-id="638e0-142">Without TLS, all connections to the endpoint default to HTTP/1.1, and gRPC calls fail.</span></span>

<span data-ttu-id="638e0-143">El cliente de gRPC también debe estar configurado para no usar TLS.</span><span class="sxs-lookup"><span data-stu-id="638e0-143">The gRPC client must also be configured to not use TLS.</span></span> <span data-ttu-id="638e0-144">Para obtener más información, consulte [Call unsecure gRPC Services with .net Core Client](#call-insecure-grpc-services-with-net-core-client).</span><span class="sxs-lookup"><span data-stu-id="638e0-144">For more information, see [Call insecure gRPC services with .NET Core client](#call-insecure-grpc-services-with-net-core-client).</span></span>

> [!WARNING]
> <span data-ttu-id="638e0-145">HTTP/2 sin TLS solo se debe usar durante el desarrollo de aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="638e0-145">HTTP/2 without TLS should only be used during app development.</span></span> <span data-ttu-id="638e0-146">Las aplicaciones de producción siempre deben usar la seguridad de transporte.</span><span class="sxs-lookup"><span data-stu-id="638e0-146">Production apps should always use transport security.</span></span> <span data-ttu-id="638e0-147">Para obtener más información, vea [consideraciones de seguridad en gRPC para ASP.net Core](xref:grpc/security#transport-security).</span><span class="sxs-lookup"><span data-stu-id="638e0-147">For more information, see [Security considerations in gRPC for ASP.NET Core](xref:grpc/security#transport-security).</span></span>

## <a name="grpc-c-assets-are-not-code-generated-from-proto-files"></a><span data-ttu-id="638e0-148">los C# recursos gRPC no se generan a partir de  *\*archivos. proto*</span><span class="sxs-lookup"><span data-stu-id="638e0-148">gRPC C# assets are not code generated from *\*.proto* files</span></span>

<span data-ttu-id="638e0-149">la generación de código gRPC de los clientes concretos y las clases base del servicio requiere que se haga referencia a los archivos y las herramientas de protobuf desde un proyecto.</span><span class="sxs-lookup"><span data-stu-id="638e0-149">gRPC code generation of concrete clients and service base classes requires protobuf files and tooling to be referenced from a project.</span></span> <span data-ttu-id="638e0-150">Debe incluir:</span><span class="sxs-lookup"><span data-stu-id="638e0-150">You must include:</span></span>

* <span data-ttu-id="638e0-151">archivos *. proto* que desea utilizar en el `<Protobuf>` grupo de elementos.</span><span class="sxs-lookup"><span data-stu-id="638e0-151">*.proto* files you want to use in the `<Protobuf>` item group.</span></span> <span data-ttu-id="638e0-152">El proyecto debe hacer referencia a [los archivos *. proto* importados](https://developers.google.com/protocol-buffers/docs/proto3#importing-definitions) .</span><span class="sxs-lookup"><span data-stu-id="638e0-152">[Imported *.proto* files](https://developers.google.com/protocol-buffers/docs/proto3#importing-definitions) must be referenced by the project.</span></span>
* <span data-ttu-id="638e0-153">Referencia de paquete al paquete de herramientas de gRPC [gRPC. Tools](https://www.nuget.org/packages/Grpc.Tools/).</span><span class="sxs-lookup"><span data-stu-id="638e0-153">Package reference to the gRPC tooling package [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/).</span></span>

<span data-ttu-id="638e0-154">Para obtener más información sobre la C# generación de recursos <xref:grpc/basics>gRPC, vea.</span><span class="sxs-lookup"><span data-stu-id="638e0-154">For more information on generating gRPC C# assets, see <xref:grpc/basics>.</span></span>

<span data-ttu-id="638e0-155">De forma predeterminada, `<Protobuf>` una referencia genera un cliente concreto y una clase base de servicio.</span><span class="sxs-lookup"><span data-stu-id="638e0-155">By default, a `<Protobuf>` reference generates a concrete client and a service base class.</span></span> <span data-ttu-id="638e0-156">El atributo del elemento `GrpcServices` de referencia se puede utilizar para C# limitar la generación de recursos.</span><span class="sxs-lookup"><span data-stu-id="638e0-156">The reference element's `GrpcServices` attribute can be used to limit C# asset generation.</span></span> <span data-ttu-id="638e0-157">Las `GrpcServices` opciones válidas son:</span><span class="sxs-lookup"><span data-stu-id="638e0-157">Valid `GrpcServices` options are:</span></span>

* <span data-ttu-id="638e0-158">`Both`(valor predeterminado si no está presente)</span><span class="sxs-lookup"><span data-stu-id="638e0-158">`Both` (default when not present)</span></span>
* `Server`
* `Client`
* `None`

<span data-ttu-id="638e0-159">Una ASP.NET Core aplicación web que hospeda gRPC Services solo necesita la clase base de servicio generada:</span><span class="sxs-lookup"><span data-stu-id="638e0-159">An ASP.NET Core web app hosting gRPC services only needs the service base class generated:</span></span>

```xml
<ItemGroup>
  <Protobuf Include="Protos\greet.proto" GrpcServices="Server" />
</ItemGroup>
```

<span data-ttu-id="638e0-160">Una aplicación cliente de gRPC que realiza llamadas a gRPC solo necesita el cliente concreto generado:</span><span class="sxs-lookup"><span data-stu-id="638e0-160">A gRPC client app making gRPC calls only needs the concrete client generated:</span></span>

```xml
<ItemGroup>
  <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
</ItemGroup>
```
