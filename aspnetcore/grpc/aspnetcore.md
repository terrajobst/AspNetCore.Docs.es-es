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
# <a name="grpc-services-with-aspnet-core"></a><span data-ttu-id="2ee11-103">Servicios gRPC con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2ee11-103">gRPC services with ASP.NET Core</span></span>

<span data-ttu-id="2ee11-104">En este documento se muestra cómo empezar a trabajar con gRPC Services mediante ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2ee11-104">This document shows how to get started with gRPC services using ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2ee11-105">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="2ee11-105">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2ee11-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2ee11-106">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2ee11-107">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2ee11-107">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="2ee11-108">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="2ee11-108">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="get-started-with-grpc-service-in-aspnet-core"></a><span data-ttu-id="2ee11-109">Introducción al servicio gRPC en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2ee11-109">Get started with gRPC service in ASP.NET Core</span></span>

<span data-ttu-id="2ee11-110">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="2ee11-110">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2ee11-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2ee11-111">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="2ee11-112">Vea Introducción [a gRPC Services](xref:tutorials/grpc/grpc-start) para obtener instrucciones detalladas sobre cómo crear un proyecto de gRPC.</span><span class="sxs-lookup"><span data-stu-id="2ee11-112">See [Get started with gRPC services](xref:tutorials/grpc/grpc-start) for detailed instructions on how to create a gRPC project.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="2ee11-113">Visual Studio Code/Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="2ee11-113">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="2ee11-114">Ejecute `dotnet new grpc -o GrpcGreeter` desde la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="2ee11-114">Run `dotnet new grpc -o GrpcGreeter` from the command line.</span></span>

---

## <a name="add-grpc-services-to-an-aspnet-core-app"></a><span data-ttu-id="2ee11-115">Incorporación de gRPC Services a una aplicación ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2ee11-115">Add gRPC services to an ASP.NET Core app</span></span>

<span data-ttu-id="2ee11-116">gRPC requiere el paquete [gRPC. AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore) .</span><span class="sxs-lookup"><span data-stu-id="2ee11-116">gRPC requires the [Grpc.AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore) package.</span></span>

### <a name="configure-grpc"></a><span data-ttu-id="2ee11-117">Configuración de gRPC</span><span class="sxs-lookup"><span data-stu-id="2ee11-117">Configure gRPC</span></span>

<span data-ttu-id="2ee11-118">gRPC se habilita con el `AddGrpc` método:</span><span class="sxs-lookup"><span data-stu-id="2ee11-118">gRPC is enabled with the `AddGrpc` method:</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=7)]

<span data-ttu-id="2ee11-119">Cada servicio gRPC se agrega a la canalización de enrutamiento `MapGrpcService` a través del método:</span><span class="sxs-lookup"><span data-stu-id="2ee11-119">Each gRPC service is added to the routing pipeline through the `MapGrpcService` method:</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=24)]

<span data-ttu-id="2ee11-120">ASP.NET Core los middleware y las características comparten la canalización de enrutamiento, por lo que se puede configurar una aplicación para que atienda a los controladores de solicitud adicionales.</span><span class="sxs-lookup"><span data-stu-id="2ee11-120">ASP.NET Core middlewares and features share the routing pipeline, therefore an app can be configured to serve additional request handlers.</span></span> <span data-ttu-id="2ee11-121">Los controladores de solicitud adicionales, como los controladores MVC, funcionan en paralelo con los servicios gRPC configurados.</span><span class="sxs-lookup"><span data-stu-id="2ee11-121">The additional request handlers, such as MVC controllers, work in parallel with the configured gRPC services.</span></span>

## <a name="integration-with-aspnet-core-apis"></a><span data-ttu-id="2ee11-122">Integración con API de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2ee11-122">Integration with ASP.NET Core APIs</span></span>

<span data-ttu-id="2ee11-123">los servicios de gRPC tienen acceso completo a las características de ASP.NET Core, como la [inserción](xref:fundamentals/dependency-injection) de dependencias (di) y el [registro](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="2ee11-123">gRPC services have full access to the ASP.NET Core features such as [Dependency Injection](xref:fundamentals/dependency-injection) (DI) and [Logging](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="2ee11-124">Por ejemplo, la implementación del servicio puede resolver un servicio del registrador a partir del contenedor de DI mediante el constructor:</span><span class="sxs-lookup"><span data-stu-id="2ee11-124">For example, the service implementation can resolve a logger service from the DI container via the constructor:</span></span>

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

<span data-ttu-id="2ee11-125">De forma predeterminada, la implementación del servicio gRPC puede resolver otros servicios de DI con cualquier duración (singleton, ámbito o transitorio).</span><span class="sxs-lookup"><span data-stu-id="2ee11-125">By default, the gRPC service implementation can resolve other DI services with any lifetime (Singleton, Scoped, or Transient).</span></span>

### <a name="resolve-httpcontext-in-grpc-methods"></a><span data-ttu-id="2ee11-126">Resolver HttpContext en métodos gRPC</span><span class="sxs-lookup"><span data-stu-id="2ee11-126">Resolve HttpContext in gRPC methods</span></span>

<span data-ttu-id="2ee11-127">La API gRPC proporciona acceso a algunos datos de mensajes HTTP/2, como el método, el host, el encabezado y los finalizadores.</span><span class="sxs-lookup"><span data-stu-id="2ee11-127">The gRPC API provides access to some HTTP/2 message data, such as the method, host, header, and trailers.</span></span> <span data-ttu-id="2ee11-128">El acceso se realiza `ServerCallContext` a través del argumento que se pasa a cada método gRPC:</span><span class="sxs-lookup"><span data-stu-id="2ee11-128">Access is through the `ServerCallContext` argument passed to each gRPC method:</span></span>

[!code-csharp[](~/grpc/aspnetcore/sample/GrcpService/GreeterService.cs?highlight=3-4&name=snippet)]

<span data-ttu-id="2ee11-129">`ServerCallContext`no proporciona acceso completo a `HttpContext` en todas las API de ASP.net.</span><span class="sxs-lookup"><span data-stu-id="2ee11-129">`ServerCallContext` does not provide full access to `HttpContext` in all ASP.NET APIs.</span></span> <span data-ttu-id="2ee11-130">El `GetHttpContext` método de extensión proporciona acceso completo `HttpContext` a que representa el mensaje http/2 subyacente en las API de ASP.net:</span><span class="sxs-lookup"><span data-stu-id="2ee11-130">The `GetHttpContext` extension method provides full access to the `HttpContext` representing the underlying HTTP/2 message in ASP.NET APIs:</span></span>

[!code-csharp[](~/grpc/aspnetcore/sample/GrcpService/GreeterService2.cs?highlight=6-7&name=snippet)]

## <a name="grpc-and-aspnet-core-on-macos"></a><span data-ttu-id="2ee11-131">gRPC y ASP.NET Core en macOS</span><span class="sxs-lookup"><span data-stu-id="2ee11-131">gRPC and ASP.NET Core on macOS</span></span>

<span data-ttu-id="2ee11-132">Kestrel no admite HTTP/2 con [seguridad de la capa de transporte (TLS)](https://tools.ietf.org/html/rfc5246) en MacOS.</span><span class="sxs-lookup"><span data-stu-id="2ee11-132">Kestrel doesn't support HTTP/2 with [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246) on macOS.</span></span> <span data-ttu-id="2ee11-133">La plantilla ASP.NET Core gRPC y los ejemplos usan TLS de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="2ee11-133">The ASP.NET Core gRPC template and samples use TLS by default.</span></span> <span data-ttu-id="2ee11-134">Verá el siguiente mensaje de error al intentar iniciar el servidor de gRPC:</span><span class="sxs-lookup"><span data-stu-id="2ee11-134">You'll see the following error message when you attempt to start the gRPC server:</span></span>

> <span data-ttu-id="2ee11-135">No se puede enlazar a https://localhost:5001 en la interfaz de bucle invertido IPv4: ' HTTP/2 a través de TLS no se admite en macOS debido a la falta de compatibilidad con ALPN. '.</span><span class="sxs-lookup"><span data-stu-id="2ee11-135">Unable to bind to https://localhost:5001 on the IPv4 loopback interface: 'HTTP/2 over TLS is not supported on macOS due to missing ALPN support.'.</span></span>

<span data-ttu-id="2ee11-136">Para solucionar este problema, configure Kestrel y el cliente de gRPC para que use HTTP/2 **sin** TLS.</span><span class="sxs-lookup"><span data-stu-id="2ee11-136">To work around this issue, configure Kestrel and the gRPC client to use HTTP/2 **without** TLS.</span></span> <span data-ttu-id="2ee11-137">Esto solo debe realizarse durante el desarrollo.</span><span class="sxs-lookup"><span data-stu-id="2ee11-137">You should only do this during development.</span></span> <span data-ttu-id="2ee11-138">Si no se usa TLS, se enviarán mensajes gRPC sin cifrado.</span><span class="sxs-lookup"><span data-stu-id="2ee11-138">Not using TLS will result in gRPC messages being sent without encryption.</span></span>

<span data-ttu-id="2ee11-139">Kestrel debe configurar un punto de conexión HTTP/2 sin `Program.cs`TLS en:</span><span class="sxs-lookup"><span data-stu-id="2ee11-139">Kestrel must configure a HTTP/2 endpoint without TLS in `Program.cs`:</span></span>

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

<span data-ttu-id="2ee11-140">El cliente gRPC debe establecer el `System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport` `true` modificador en y `http` usarlo en la dirección del servidor:</span><span class="sxs-lookup"><span data-stu-id="2ee11-140">The gRPC client must set the `System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport` switch to `true` and use `http` in the server address:</span></span>

```csharp
// This switch must be set before creating the HttpClient.
AppContext.SetSwitch("System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport", true);

var httpClient = new HttpClient();
// The port number(5000) must match the port of the gRPC server.
httpClient.BaseAddress = new Uri("http://localhost:5000");
var client = GrpcClient.Create<Greeter.GreeterClient>(httpClient);
```

> [!WARNING]
> <span data-ttu-id="2ee11-141">HTTP/2 sin TLS solo se debe usar durante el desarrollo de aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="2ee11-141">HTTP/2 without TLS should only be used during app development.</span></span> <span data-ttu-id="2ee11-142">Las aplicaciones de producción siempre deben usar la seguridad de transporte.</span><span class="sxs-lookup"><span data-stu-id="2ee11-142">Production applications should always use transport security.</span></span> <span data-ttu-id="2ee11-143">Para obtener más información, vea [consideraciones de seguridad en gRPC para ASP.net Core](xref:grpc/security#transport-security).</span><span class="sxs-lookup"><span data-stu-id="2ee11-143">For more information, see [Security considerations in gRPC for ASP.NET Core](xref:grpc/security#transport-security).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2ee11-144">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="2ee11-144">Additional resources</span></span>

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
