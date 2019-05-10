---
title: gRPC para la configuración de ASP.NET Core
author: jamesnk
description: Obtenga información sobre cómo configurar gRPC para aplicaciones ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 04/09/2019
uid: grpc/configuration
ms.openlocfilehash: 66dfb9ec136616f10c1b7aaad766e18813b87de4
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087346"
---
# <a name="grpc-for-aspnet-core-configuration"></a><span data-ttu-id="210c4-103">gRPC para la configuración de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="210c4-103">gRPC for ASP.NET Core configuration</span></span>

## <a name="configure-services-options"></a><span data-ttu-id="210c4-104">Configurar las opciones de servicios</span><span class="sxs-lookup"><span data-stu-id="210c4-104">Configure services options</span></span>

<span data-ttu-id="210c4-105">En la tabla siguiente se describe las opciones de configuración de servicios de gRPC:</span><span class="sxs-lookup"><span data-stu-id="210c4-105">The following table describes options for configuring gRPC services:</span></span>

| <span data-ttu-id="210c4-106">Opción</span><span class="sxs-lookup"><span data-stu-id="210c4-106">Option</span></span> | <span data-ttu-id="210c4-107">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="210c4-107">Default Value</span></span> | <span data-ttu-id="210c4-108">Descripción</span><span class="sxs-lookup"><span data-stu-id="210c4-108">Description</span></span> |
| ------ | ------------- | ----------- |
| `SendMaxMessageSize` | `null` | <span data-ttu-id="210c4-109">El tamaño máximo del mensaje en bytes que se pueden enviar desde el servidor.</span><span class="sxs-lookup"><span data-stu-id="210c4-109">The maximum message size in bytes that can be sent from the server.</span></span> <span data-ttu-id="210c4-110">Intentando enviar un mensaje que supera los resultados de tamaño máximo de mensaje configurado en una excepción.</span><span class="sxs-lookup"><span data-stu-id="210c4-110">Attempting to send a message that exceeds the configured maximum message size results in an exception.</span></span> |
| `ReceiveMaxMessageSize` | <span data-ttu-id="210c4-111">4 MB</span><span class="sxs-lookup"><span data-stu-id="210c4-111">4 MB</span></span> | <span data-ttu-id="210c4-112">El tamaño máximo del mensaje en bytes, que puede ser recibido por el servidor.</span><span class="sxs-lookup"><span data-stu-id="210c4-112">The maximum message size in bytes that can be received by the server.</span></span> <span data-ttu-id="210c4-113">Si el servidor recibe un mensaje que supera este límite, produce una excepción.</span><span class="sxs-lookup"><span data-stu-id="210c4-113">If the server receives a message that exceeds this limit, it throws an exception.</span></span> <span data-ttu-id="210c4-114">Al aumentar este valor permite que el servidor recibir los mensajes más grandes, pero puede repercutir negativamente en el consumo de memoria.</span><span class="sxs-lookup"><span data-stu-id="210c4-114">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="210c4-115">Si `true`detallados se devuelven los mensajes de excepción a los clientes cuando se produce una excepción en un método de servicio.</span><span class="sxs-lookup"><span data-stu-id="210c4-115">If `true`, detailed exception messages are returned to clients when an exception is thrown in a service method.</span></span> <span data-ttu-id="210c4-116">De manera predeterminada, es `false`.</span><span class="sxs-lookup"><span data-stu-id="210c4-116">The default is `false`.</span></span> <span data-ttu-id="210c4-117">Si se establece en `true` puede producir la pérdida de información confidencial.</span><span class="sxs-lookup"><span data-stu-id="210c4-117">Setting this to `true` can leak sensitive information.</span></span> |
| `CompressionProviders` | <span data-ttu-id="210c4-118">gzip</span><span class="sxs-lookup"><span data-stu-id="210c4-118">gzip</span></span> | <span data-ttu-id="210c4-119">Una colección de proveedores de compresión utilizado para comprimir y descomprimir los mensajes.</span><span class="sxs-lookup"><span data-stu-id="210c4-119">A collection of compression providers used to compress and decompress messages.</span></span> <span data-ttu-id="210c4-120">Se pueden crear proveedores de compresión personalizado y agregados a la colección.</span><span class="sxs-lookup"><span data-stu-id="210c4-120">Custom compression providers can be created and added to the collection.</span></span> <span data-ttu-id="210c4-121">El valor predeterminado configurado el proveedor admite **gzip** compresión.</span><span class="sxs-lookup"><span data-stu-id="210c4-121">The default configured provider supports **gzip** compression.</span></span> |
| `ResponseCompressionAlgorithm` | `null` | <span data-ttu-id="210c4-122">El algoritmo de compresión utilizado para comprimir los mensajes enviados desde el servidor.</span><span class="sxs-lookup"><span data-stu-id="210c4-122">The compression algorithm used to compress messages sent from the server.</span></span> <span data-ttu-id="210c4-123">El algoritmo debe coincidir con un proveedor de compresión en `CompressionProviders`.</span><span class="sxs-lookup"><span data-stu-id="210c4-123">The algorithm must match a compression provider in `CompressionProviders`.</span></span> <span data-ttu-id="210c4-124">Para que el algorthm comprimir una respuesta, el cliente debe indicar admite el algoritmo enviando el **grpc-codificación aceptada** encabezado.</span><span class="sxs-lookup"><span data-stu-id="210c4-124">For the algorthm to compress a response the client must indicate it supports the algorithm by sending it in the **grpc-accept-encoding** header.</span></span> |
| `ResponseCompressionLevel` | `null` | <span data-ttu-id="210c4-125">El nivel de compresión para comprimir los mensajes enviados desde el servidor.</span><span class="sxs-lookup"><span data-stu-id="210c4-125">The compress level used to compress messages sent from the server.</span></span> |

<span data-ttu-id="210c4-126">Se pueden configurar opciones para todos los servicios al proporcionar un delegado de opciones para la `AddGrpc` llamar `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="210c4-126">Options can be configured for all services by providing an options delegate to the `AddGrpc` call in `Startup.ConfigureServices`.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc(options =>
    {
        options.EnableDetailedErrors = true;
    });
}
```

<span data-ttu-id="210c4-127">Opciones para un único servicio invalidan las opciones globales de `AddGrpc` y pueden configurarse mediante `AddServiceOptions<TService>`:</span><span class="sxs-lookup"><span data-stu-id="210c4-127">Options for a single service override the global options provided in `AddGrpc` and can be configured using `AddServiceOptions<TService>`:</span></span>

```csharp
services.AddGrpc().AddServiceOptions<MyService>(options =>
{
    options.ReceiveMaxMessageSize = 10 * 1024 * 1024; // 10 megabytes
});
```

## <a name="configure-kestrel-options"></a><span data-ttu-id="210c4-128">Configurar las opciones de Kestrel</span><span class="sxs-lookup"><span data-stu-id="210c4-128">Configure Kestrel options</span></span>

<span data-ttu-id="210c4-129">Servidor kestrel tiene opciones de configuración que afectan al comportamiento de gRPC para ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="210c4-129">Kestrel server has configuration options that affect the behavior of gRPC for ASP.NET.</span></span>

### <a name="request-body-data-rate-limit"></a><span data-ttu-id="210c4-130">Límite de velocidad de datos de cuerpo de solicitud</span><span class="sxs-lookup"><span data-stu-id="210c4-130">Request body data rate limit</span></span>

<span data-ttu-id="210c4-131">De forma predeterminada, el servidor Kestrel impone un [velocidad de datos del cuerpo de solicitud mínimo](
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>).</span><span class="sxs-lookup"><span data-stu-id="210c4-131">By default, the Kestrel server imposes a [minimum request body data rate](
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>).</span></span> <span data-ttu-id="210c4-132">Para cliente de streaming y dúplex, las llamadas de transmisión por secuencias, no se puede satisfacer esta velocidad y la conexión es posible que se agotó el tiempo. El mínimo debe deshabilitarse el límite de velocidad de datos cuando el servicio gRPC incluye el cliente de transmisión por secuencias y dúplex, las llamadas de transmisión por secuencias el cuerpo de solicitud:</span><span class="sxs-lookup"><span data-stu-id="210c4-132">For client streaming and duplex streaming calls, this rate may not be satisfied and the connection may be timed out. The minimum request body data rate limit must be disabled when the gRPC service includes client streaming and duplex streaming calls:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateHostBuilder(args).Build().Run();
    }

    public static IHostBuilder CreateHostBuilder(string[] args) =>
         Host.CreateDefaultBuilder(args)
    .ConfigureWebHostDefaults(webBuilder =>
    {
        webBuilder.UseStartup<Startup>();
        webBuilder.ConfigureKestrel((context, options) =>
        {
            options.Limits.MinRequestBodyDataRate = null;
        });
    });
}
```

## <a name="additional-resources"></a><span data-ttu-id="210c4-133">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="210c4-133">Additional resources</span></span>

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
