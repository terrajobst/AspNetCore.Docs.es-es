---
title: Registro y diagnóstico en gRPC en .NET
author: jamesnk
description: Obtenga información sobre cómo recopilar diagnósticos de la aplicación de gRPC en .NET.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 09/23/2019
uid: grpc/diagnostics
ms.openlocfilehash: 7194e91b40a08c4a7ee619b8f207900af2683aa1
ms.sourcegitcommit: fae6f0e253f9d62d8f39de5884d2ba2b4b2a6050
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2019
ms.locfileid: "71250733"
---
# <a name="logging-and-diagnostics-in-grpc-on-net"></a><span data-ttu-id="b1cdb-103">Registro y diagnóstico en gRPC en .NET</span><span class="sxs-lookup"><span data-stu-id="b1cdb-103">Logging and diagnostics in gRPC on .NET</span></span>

<span data-ttu-id="b1cdb-104">Por [James Newton-King](https://twitter.com/jamesnk)</span><span class="sxs-lookup"><span data-stu-id="b1cdb-104">By [James Newton-King](https://twitter.com/jamesnk)</span></span>

<span data-ttu-id="b1cdb-105">En este artículo se proporcionan instrucciones para recopilar diagnósticos de la aplicación de gRPC para ayudar a solucionar problemas.</span><span class="sxs-lookup"><span data-stu-id="b1cdb-105">This article provides guidance for gathering diagnostics from your gRPC app to help troubleshoot issues.</span></span>

## <a name="grpc-services-logging"></a><span data-ttu-id="b1cdb-106">registro de gRPC Services</span><span class="sxs-lookup"><span data-stu-id="b1cdb-106">gRPC services logging</span></span>

> [!WARNING]
> <span data-ttu-id="b1cdb-107">Los registros del lado servidor pueden contener información confidencial de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b1cdb-107">Server-side logs may contain sensitive information from your app.</span></span> <span data-ttu-id="b1cdb-108">No publique **nunca** los registros sin procesar de las aplicaciones de producción en foros públicos como github.</span><span class="sxs-lookup"><span data-stu-id="b1cdb-108">**Never** post raw logs from production apps to public forums like GitHub.</span></span>

<span data-ttu-id="b1cdb-109">Dado que los servicios de gRPC se hospedan en ASP.NET Core, usa el sistema de registro de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b1cdb-109">Since gRPC services are hosted on ASP.NET Core, it uses the ASP.NET Core logging system.</span></span> <span data-ttu-id="b1cdb-110">En la configuración predeterminada, gRPC registra muy poca información, pero esto puede configurarse.</span><span class="sxs-lookup"><span data-stu-id="b1cdb-110">In the default configuration, gRPC logs very little information, but this can configured.</span></span> <span data-ttu-id="b1cdb-111">Consulte la documentación sobre el [registro de ASP.net Core](xref:fundamentals/logging/index#configuration) para obtener más información sobre la configuración del registro de ASP.net Core.</span><span class="sxs-lookup"><span data-stu-id="b1cdb-111">See the documentation on [ASP.NET Core logging](xref:fundamentals/logging/index#configuration) for details on configuring ASP.NET Core logging.</span></span>

<span data-ttu-id="b1cdb-112">gRPC agrega registros en la `Grpc` categoría.</span><span class="sxs-lookup"><span data-stu-id="b1cdb-112">gRPC adds logs under the `Grpc` category.</span></span> <span data-ttu-id="b1cdb-113">Para habilitar registros detallados de gRPC, `Grpc` configure los prefijos en el `Debug` nivel del archivo *appSettings. JSON* agregando los siguientes `LogLevel` elementos a la subsección de: `Logging`</span><span class="sxs-lookup"><span data-stu-id="b1cdb-113">To enable detailed logs from gRPC, configure the `Grpc` prefixes to the `Debug` level in your *appsettings.json* file by adding the following items to the `LogLevel` sub-section in `Logging`:</span></span>

[!code-json[](diagnostics/sample/logging-config.json?highlight=7)]

<span data-ttu-id="b1cdb-114">También puede configurarlo en *Startup.CS* con `ConfigureLogging`:</span><span class="sxs-lookup"><span data-stu-id="b1cdb-114">You can also configure this in *Startup.cs* with `ConfigureLogging`:</span></span>

[!code-csharp[](diagnostics/sample/logging-config-code.cs?highlight=5)]

<span data-ttu-id="b1cdb-115">Si no está usando la configuración basada en JSON, establezca el siguiente valor de configuración en el sistema de configuración:</span><span class="sxs-lookup"><span data-stu-id="b1cdb-115">If you aren't using JSON-based configuration, set the following configuration value in your configuration system:</span></span>

* `Logging:LogLevel:Grpc` = `Debug`

<span data-ttu-id="b1cdb-116">Consulte la documentación del sistema de configuración para determinar cómo especificar valores de configuración anidados.</span><span class="sxs-lookup"><span data-stu-id="b1cdb-116">Check the documentation for your configuration system to determine how to specify nested configuration values.</span></span> <span data-ttu-id="b1cdb-117">Por ejemplo, al usar variables de entorno, `_` se usan dos caracteres en lugar `:` de (por ejemplo, `Logging__LogLevel__Grpc`).</span><span class="sxs-lookup"><span data-stu-id="b1cdb-117">For example, when using environment variables, two `_` characters are used instead of the `:` (for example, `Logging__LogLevel__Grpc`).</span></span>

<span data-ttu-id="b1cdb-118">Se recomienda usar el `Debug` nivel al recopilar diagnósticos más detallados para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b1cdb-118">We recommend using the `Debug` level when gathering more detailed diagnostics for your app.</span></span> <span data-ttu-id="b1cdb-119">El `Trace` nivel genera diagnósticos de nivel muy bajo y rara vez es necesario para diagnosticar problemas en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b1cdb-119">The `Trace` level produces very low-level diagnostics and is rarely needed to diagnose issues in your app.</span></span>

### <a name="sample-logging-output"></a><span data-ttu-id="b1cdb-120">Salida de registro de ejemplo</span><span class="sxs-lookup"><span data-stu-id="b1cdb-120">Sample logging output</span></span>

<span data-ttu-id="b1cdb-121">Este es un ejemplo de la salida de la `Debug` consola en el nivel de un servicio de gRPC:</span><span class="sxs-lookup"><span data-stu-id="b1cdb-121">Here is an example of console output at the `Debug` level of a gRPC service:</span></span>

```console
info: Microsoft.AspNetCore.Hosting.Diagnostics[1]
      Request starting HTTP/2 POST https://localhost:5001/Greet.Greeter/SayHello application/grpc
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[0]
      Executing endpoint 'gRPC - /Greet.Greeter/SayHello'
dbug: Grpc.AspNetCore.Server.ServerCallHandler[1]
      Reading message.
info: GrpcService.GreeterService[0]
      Hello World
dbug: Grpc.AspNetCore.Server.ServerCallHandler[6]
      Sending message.
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[1]
      Executed endpoint 'gRPC - /Greet.Greeter/SayHello'
info: Microsoft.AspNetCore.Hosting.Diagnostics[2]
      Request finished in 1.4113ms 200 application/grpc
```

## <a name="access-server-side-logs"></a><span data-ttu-id="b1cdb-122">Acceder a los registros del lado servidor</span><span class="sxs-lookup"><span data-stu-id="b1cdb-122">Access server-side logs</span></span>

<span data-ttu-id="b1cdb-123">La forma de acceder a los registros del lado servidor depende del entorno en el que se está ejecutando.</span><span class="sxs-lookup"><span data-stu-id="b1cdb-123">How you access server-side logs depends on the environment in which you're running.</span></span>

### <a name="as-a-console-app"></a><span data-ttu-id="b1cdb-124">Como aplicación de consola</span><span class="sxs-lookup"><span data-stu-id="b1cdb-124">As a console app</span></span>

<span data-ttu-id="b1cdb-125">Si está ejecutando en una aplicación de consola, el [registrador de consola](xref:fundamentals/logging/index#console-provider) debe estar habilitado de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="b1cdb-125">If you're running in a console app, the [Console logger](xref:fundamentals/logging/index#console-provider) should be enabled by default.</span></span> <span data-ttu-id="b1cdb-126">los registros de gRPC aparecerán en la consola de.</span><span class="sxs-lookup"><span data-stu-id="b1cdb-126">gRPC logs will appear in the console.</span></span>

### <a name="other-environments"></a><span data-ttu-id="b1cdb-127">Otros entornos</span><span class="sxs-lookup"><span data-stu-id="b1cdb-127">Other environments</span></span>

<span data-ttu-id="b1cdb-128">Si la aplicación se implementa en otro entorno (por ejemplo, Docker, Kubernetes o servicio de Windows), consulte <xref:fundamentals/logging/index> para obtener más información sobre cómo configurar los proveedores de registro adecuados para el entorno.</span><span class="sxs-lookup"><span data-stu-id="b1cdb-128">If the app is deployed to another environment (for example, Docker, Kubernetes, or Windows Service), see <xref:fundamentals/logging/index> for more information on how to configure logging providers suitable for the environment.</span></span>

## <a name="grpc-client-logging"></a><span data-ttu-id="b1cdb-129">registro de cliente de gRPC</span><span class="sxs-lookup"><span data-stu-id="b1cdb-129">gRPC client logging</span></span>

> [!WARNING]
> <span data-ttu-id="b1cdb-130">Los registros del lado cliente pueden contener información confidencial de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b1cdb-130">Client-side logs may contain sensitive information from your app.</span></span> <span data-ttu-id="b1cdb-131">No publique **nunca** los registros sin procesar de las aplicaciones de producción en foros públicos como github.</span><span class="sxs-lookup"><span data-stu-id="b1cdb-131">**Never** post raw logs from production apps to public forums like GitHub.</span></span>

<span data-ttu-id="b1cdb-132">Para obtener los registros del cliente .net, puede establecer la `GrpcChannelOptions.LoggerFactory` propiedad cuando se crea el canal del cliente.</span><span class="sxs-lookup"><span data-stu-id="b1cdb-132">To get logs from the .NET client, you can set the `GrpcChannelOptions.LoggerFactory` property when the client's channel is created.</span></span> <span data-ttu-id="b1cdb-133">Si está llamando a un servicio de gRPC desde una aplicación ASP.NET Core, el generador del registrador se puede resolver desde la inserción de dependencias (DI):</span><span class="sxs-lookup"><span data-stu-id="b1cdb-133">If you are calling a gRPC service from an ASP.NET Core app then the logger factory can be resolved from dependency injection (DI):</span></span>

[!code-csharp[](diagnostics/sample/net-client-dependency-injection.cs?highlight=7,16)]

<span data-ttu-id="b1cdb-134">Una manera alternativa de habilitar el registro de cliente es usar el [generador de cliente de gRPC](xref:grpc/clientfactory) para crear el cliente.</span><span class="sxs-lookup"><span data-stu-id="b1cdb-134">An alternative way to enable client logging is to use the [gRPC client factory](xref:grpc/clientfactory) to create the client.</span></span> <span data-ttu-id="b1cdb-135">Un cliente de gRPC registrado con el generador de cliente y resuelto desde DI usará automáticamente el registro configurado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b1cdb-135">A gRPC client registered with the client factory and resolved from DI will automatically use the app's configured logging.</span></span>

<span data-ttu-id="b1cdb-136">Si la aplicación no usa di, puede crear una nueva `ILoggerFactory` instancia con [LoggerFactory. Create](xref:Microsoft.Extensions.Logging.LoggerFactory.Create*).</span><span class="sxs-lookup"><span data-stu-id="b1cdb-136">If your app isn't using DI then you can create a new `ILoggerFactory` instance with [LoggerFactory.Create](xref:Microsoft.Extensions.Logging.LoggerFactory.Create*).</span></span> <span data-ttu-id="b1cdb-137">Para tener acceso a este método, agregue el paquete [Microsoft. Extensions. Logging](https://www.nuget.org/packages/microsoft.extensions.logging/) a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b1cdb-137">To access this method add the [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/) package to your app.</span></span>

[!code-csharp[](diagnostics/sample/net-client-loggerfactory-create.cs?highlight=1,8)]

### <a name="sample-logging-output"></a><span data-ttu-id="b1cdb-138">Salida de registro de ejemplo</span><span class="sxs-lookup"><span data-stu-id="b1cdb-138">Sample logging output</span></span>

<span data-ttu-id="b1cdb-139">Este es un ejemplo de la salida de la `Debug` consola en el nivel de un cliente de gRPC:</span><span class="sxs-lookup"><span data-stu-id="b1cdb-139">Here is an example of console output at the `Debug` level of a gRPC client:</span></span>

```console
dbug: Grpc.Net.Client.Internal.GrpcCall[1]
      Starting gRPC call. Method type: 'Unary', URI: 'https://localhost:5001/Greet.Greeter/SayHello'.
dbug: Grpc.Net.Client.Internal.GrpcCall[6]
      Sending message.
dbug: Grpc.Net.Client.Internal.GrpcCall[1]
      Reading message.
dbug: Grpc.Net.Client.Internal.GrpcCall[4]
      Finished gRPC call.
```

## <a name="additional-resources"></a><span data-ttu-id="b1cdb-140">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="b1cdb-140">Additional resources</span></span>

* <xref:fundamentals/logging/index>
* <xref:grpc/configuration>
* <xref:grpc/clientfactory>
