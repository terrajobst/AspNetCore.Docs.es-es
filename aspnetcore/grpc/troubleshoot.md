---
title: Solución de problemas de gRPC en .NET Core
author: jamesnk
description: Solución de problemas de errores al usar gRPC en .NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 08/12/2019
uid: grpc/troubleshoot
ms.openlocfilehash: ad74bfa57d2dde316734d55d86075f463e78ee56
ms.sourcegitcommit: 476ea5ad86a680b7b017c6f32098acd3414c0f6c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/14/2019
ms.locfileid: "69029038"
---
# <a name="troubleshoot-grpc-on-net-core"></a><span data-ttu-id="fd4ba-103">Solución de problemas de gRPC en .NET Core</span><span class="sxs-lookup"><span data-stu-id="fd4ba-103">Troubleshoot gRPC on .NET Core</span></span>

<span data-ttu-id="fd4ba-104">Por [James Newton-King](https://twitter.com/jamesnk)</span><span class="sxs-lookup"><span data-stu-id="fd4ba-104">By [James Newton-King](https://twitter.com/jamesnk)</span></span>

## <a name="mismatch-between-client-and-service-ssltls-configuration"></a><span data-ttu-id="fd4ba-105">La configuración de SSL/TLS de cliente y servicio no coincide</span><span class="sxs-lookup"><span data-stu-id="fd4ba-105">Mismatch between client and service SSL/TLS configuration</span></span>

<span data-ttu-id="fd4ba-106">La plantilla gRPC y los ejemplos usan la seguridad de la [capa de transporte (TLS)](https://tools.ietf.org/html/rfc5246) para proteger los servicios de gRPC de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="fd4ba-106">The gRPC template and samples use [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246) to secure gRPC services by default.</span></span> <span data-ttu-id="fd4ba-107">los clientes de gRPC deben usar una conexión segura para llamar correctamente a los servicios de gRPC protegidos.</span><span class="sxs-lookup"><span data-stu-id="fd4ba-107">gRPC clients need to use a secure connection to call secured gRPC services successfully.</span></span>

<span data-ttu-id="fd4ba-108">Puede comprobar la ASP.NET Core servicio gRPC usa TLS en los registros escritos al iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="fd4ba-108">You can verify the ASP.NET Core gRPC service is using TLS in the logs written on app start.</span></span> <span data-ttu-id="fd4ba-109">El servicio escuchará en un punto de conexión HTTPS:</span><span class="sxs-lookup"><span data-stu-id="fd4ba-109">The service will be listening on an HTTPS endpoint:</span></span>

```
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: https://localhost:5001
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
```

<span data-ttu-id="fd4ba-110">El cliente de .net Core debe `https` usar en la dirección del servidor para realizar llamadas con una conexión segura:</span><span class="sxs-lookup"><span data-stu-id="fd4ba-110">The .NET Core client must use `https` in the server address to make calls with a secured connection:</span></span>

```csharp
static async Task Main(string[] args)
{
    var httpClient = new HttpClient();
    // The port number(5001) must match the port of the gRPC server.
    httpClient.BaseAddress = new Uri("https://localhost:5001");
    var client = GrpcClient.Create<Greeter.GreeterClient>(httpClient);
}
```

<span data-ttu-id="fd4ba-111">Todas las implementaciones de cliente de gRPC admiten TLS.</span><span class="sxs-lookup"><span data-stu-id="fd4ba-111">All gRPC client implementations support TLS.</span></span> <span data-ttu-id="fd4ba-112">los clientes de gRPC de otros idiomas normalmente requieren el canal `SslCredentials`configurado con.</span><span class="sxs-lookup"><span data-stu-id="fd4ba-112">gRPC clients from other languages typically require the channel configured with `SslCredentials`.</span></span> <span data-ttu-id="fd4ba-113">`SslCredentials`especifica el certificado que usará el cliente y debe usarse en lugar de credenciales no seguras.</span><span class="sxs-lookup"><span data-stu-id="fd4ba-113">`SslCredentials` specifies the certificate that the client will use, and it must be used instead of insecure credentials.</span></span> <span data-ttu-id="fd4ba-114">Para obtener ejemplos de configuración de las distintas implementaciones de cliente de gRPC para usar TLS, consulte [autenticación de gRPC](https://www.grpc.io/docs/guides/auth/).</span><span class="sxs-lookup"><span data-stu-id="fd4ba-114">For examples of configuring the different gRPC client implementations to use TLS, see [gRPC Authentication](https://www.grpc.io/docs/guides/auth/).</span></span>

## <a name="call-insecure-grpc-services-with-net-core-client"></a><span data-ttu-id="fd4ba-115">Llamada a servicios de gRPC inseguros con el cliente de .NET Core</span><span class="sxs-lookup"><span data-stu-id="fd4ba-115">Call insecure gRPC services with .NET Core client</span></span>

<span data-ttu-id="fd4ba-116">Se requiere configuración adicional para llamar a los servicios de gRPC inseguros con el cliente de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="fd4ba-116">Additional configuration is required to call insecure gRPC services with the .NET Core client.</span></span> <span data-ttu-id="fd4ba-117">El cliente gRPC debe establecer el `System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport` `true` modificador en y `http` usarlo en la dirección del servidor:</span><span class="sxs-lookup"><span data-stu-id="fd4ba-117">The gRPC client must set the `System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport` switch to `true` and use `http` in the server address:</span></span>

```csharp
// This switch must be set before creating the HttpClient.
AppContext.SetSwitch("System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport", true);

var httpClient = new HttpClient();
// The port number(5000) must match the port of the gRPC server.
httpClient.BaseAddress = new Uri("http://localhost:5000");
var client = GrpcClient.Create<Greeter.GreeterClient>(httpClient);
```

## <a name="unable-to-start-aspnet-core-grpc-app-on-macos"></a><span data-ttu-id="fd4ba-118">No se puede iniciar ASP.NET Core aplicación gRPC en macOS</span><span class="sxs-lookup"><span data-stu-id="fd4ba-118">Unable to start ASP.NET Core gRPC app on macOS</span></span>

<span data-ttu-id="fd4ba-119">Kestrel no admite HTTP/2 con TLS en macOS y versiones anteriores de Windows, como Windows 7.</span><span class="sxs-lookup"><span data-stu-id="fd4ba-119">Kestrel doesn't support HTTP/2 with TLS on macOS and older Windows versions such as Windows 7.</span></span> <span data-ttu-id="fd4ba-120">La plantilla ASP.NET Core gRPC y los ejemplos usan TLS de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="fd4ba-120">The ASP.NET Core gRPC template and samples use TLS by default.</span></span> <span data-ttu-id="fd4ba-121">Verá el siguiente mensaje de error al intentar iniciar el servidor de gRPC:</span><span class="sxs-lookup"><span data-stu-id="fd4ba-121">You'll see the following error message when you attempt to start the gRPC server:</span></span>

> <span data-ttu-id="fd4ba-122">No se puede enlazar a https://localhost:5001 en la interfaz de bucle invertido IPv4: ' HTTP/2 a través de TLS no se admite en macOS debido a la falta de compatibilidad con ALPN. '.</span><span class="sxs-lookup"><span data-stu-id="fd4ba-122">Unable to bind to https://localhost:5001 on the IPv4 loopback interface: 'HTTP/2 over TLS is not supported on macOS due to missing ALPN support.'.</span></span>

<span data-ttu-id="fd4ba-123">Para solucionar este problema, configure Kestrel y el cliente de gRPC para que use HTTP/2 *sin* TLS.</span><span class="sxs-lookup"><span data-stu-id="fd4ba-123">To work around this issue, configure Kestrel and the gRPC client to use HTTP/2 *without* TLS.</span></span> <span data-ttu-id="fd4ba-124">Esto solo debe realizarse durante el desarrollo.</span><span class="sxs-lookup"><span data-stu-id="fd4ba-124">You should only do this during development.</span></span> <span data-ttu-id="fd4ba-125">Si no se usa TLS, se enviarán mensajes gRPC sin cifrado.</span><span class="sxs-lookup"><span data-stu-id="fd4ba-125">Not using TLS will result in gRPC messages being sent without encryption.</span></span>

<span data-ttu-id="fd4ba-126">Kestrel debe configurar un punto de conexión HTTP/2 sin TLS en *Program.CS*:</span><span class="sxs-lookup"><span data-stu-id="fd4ba-126">Kestrel must configure an HTTP/2 endpoint without TLS in *Program.cs*:</span></span>

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

<span data-ttu-id="fd4ba-127">El cliente de gRPC también debe estar configurado para no usar TLS.</span><span class="sxs-lookup"><span data-stu-id="fd4ba-127">The gRPC client must also be configured to not use TLS.</span></span> <span data-ttu-id="fd4ba-128">Para obtener más información, consulte [Call unsecure gRPC Services with .net Core Client](#call-insecure-grpc-services-with-net-core-client).</span><span class="sxs-lookup"><span data-stu-id="fd4ba-128">For more information, see [Call insecure gRPC services with .NET Core client](#call-insecure-grpc-services-with-net-core-client).</span></span>

> [!WARNING]
> <span data-ttu-id="fd4ba-129">HTTP/2 sin TLS solo se debe usar durante el desarrollo de aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="fd4ba-129">HTTP/2 without TLS should only be used during app development.</span></span> <span data-ttu-id="fd4ba-130">Las aplicaciones de producción siempre deben usar la seguridad de transporte.</span><span class="sxs-lookup"><span data-stu-id="fd4ba-130">Production apps should always use transport security.</span></span> <span data-ttu-id="fd4ba-131">Para obtener más información, vea [consideraciones de seguridad en gRPC para ASP.net Core](xref:grpc/security#transport-security).</span><span class="sxs-lookup"><span data-stu-id="fd4ba-131">For more information, see [Security considerations in gRPC for ASP.NET Core](xref:grpc/security#transport-security).</span></span>

## <a name="grpc-c-assets-are-not-code-generated-from-proto-files"></a><span data-ttu-id="fd4ba-132">los C# recursos gRPC no se generan a partir de  *\*archivos. proto*</span><span class="sxs-lookup"><span data-stu-id="fd4ba-132">gRPC C# assets are not code generated from *\*.proto* files</span></span>

<span data-ttu-id="fd4ba-133">la generación de código gRPC de los clientes concretos y las clases base del servicio requiere que se haga referencia a los archivos y las herramientas de protobuf desde un proyecto.</span><span class="sxs-lookup"><span data-stu-id="fd4ba-133">gRPC code generation of concrete clients and service base classes requires protobuf files and tooling to be referenced from a project.</span></span> <span data-ttu-id="fd4ba-134">Debe incluir:</span><span class="sxs-lookup"><span data-stu-id="fd4ba-134">You must include:</span></span>

* <span data-ttu-id="fd4ba-135">archivos *. proto* que desea utilizar en el `<Protobuf>` grupo de elementos.</span><span class="sxs-lookup"><span data-stu-id="fd4ba-135">*.proto* files you want to use in the `<Protobuf>` item group.</span></span> <span data-ttu-id="fd4ba-136">El proyecto debe hacer referencia a [los archivos *. proto* importados](https://developers.google.com/protocol-buffers/docs/proto3#importing-definitions) .</span><span class="sxs-lookup"><span data-stu-id="fd4ba-136">[Imported *.proto* files](https://developers.google.com/protocol-buffers/docs/proto3#importing-definitions) must be referenced by the project.</span></span>
* <span data-ttu-id="fd4ba-137">Referencia de paquete al paquete de herramientas de gRPC [gRPC. Tools](https://www.nuget.org/packages/Grpc.Tools/).</span><span class="sxs-lookup"><span data-stu-id="fd4ba-137">Package reference to the gRPC tooling package [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/).</span></span>

<span data-ttu-id="fd4ba-138">Para obtener más información sobre la C# generación de recursos <xref:grpc/basics>gRPC, vea.</span><span class="sxs-lookup"><span data-stu-id="fd4ba-138">For more information on generating gRPC C# assets, see <xref:grpc/basics>.</span></span>

<span data-ttu-id="fd4ba-139">De forma predeterminada, `<Protobuf>` una referencia genera un cliente concreto y una clase base de servicio.</span><span class="sxs-lookup"><span data-stu-id="fd4ba-139">By default, a `<Protobuf>` reference generates a concrete client and a service base class.</span></span> <span data-ttu-id="fd4ba-140">El atributo del elemento `GrpcServices` de referencia se puede utilizar para C# limitar la generación de recursos.</span><span class="sxs-lookup"><span data-stu-id="fd4ba-140">The reference element's `GrpcServices` attribute can be used to limit C# asset generation.</span></span> <span data-ttu-id="fd4ba-141">Las `GrpcServices` opciones válidas son:</span><span class="sxs-lookup"><span data-stu-id="fd4ba-141">Valid `GrpcServices` options are:</span></span>

* <span data-ttu-id="fd4ba-142">`Both`(valor predeterminado si no está presente)</span><span class="sxs-lookup"><span data-stu-id="fd4ba-142">`Both` (default when not present)</span></span>
* `Server`
* `Client`
* `None`

<span data-ttu-id="fd4ba-143">Una ASP.NET Core aplicación web que hospeda gRPC Services solo necesita la clase base de servicio generada:</span><span class="sxs-lookup"><span data-stu-id="fd4ba-143">An ASP.NET Core web app hosting gRPC services only needs the service base class generated:</span></span>

```xml
<ItemGroup>
  <Protobuf Include="Protos\greet.proto" GrpcServices="Server" />
</ItemGroup>
```

<span data-ttu-id="fd4ba-144">Una aplicación cliente de gRPC que realiza llamadas a gRPC solo necesita el cliente concreto generado:</span><span class="sxs-lookup"><span data-stu-id="fd4ba-144">A gRPC client app making gRPC calls only needs the concrete client generated:</span></span>

```xml
<ItemGroup>
  <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
</ItemGroup>
```
