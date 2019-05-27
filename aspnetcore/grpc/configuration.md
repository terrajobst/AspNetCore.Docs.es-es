---
title: gRPC para la configuración de ASP.NET Core
author: jamesnk
description: Obtenga información sobre cómo configurar gRPC para aplicaciones ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 04/09/2019
uid: grpc/configuration
ms.openlocfilehash: 851c9ca1f7d62f6f368df66bb38eb4bbaf64bf32
ms.sourcegitcommit: 5d384db2fa9373a93b5d15e985fb34430e49ad7a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66041896"
---
# <a name="grpc-for-aspnet-core-configuration"></a><span data-ttu-id="23d28-103">gRPC para la configuración de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="23d28-103">gRPC for ASP.NET Core configuration</span></span>

## <a name="configure-services-options"></a><span data-ttu-id="23d28-104">Configurar las opciones de servicios</span><span class="sxs-lookup"><span data-stu-id="23d28-104">Configure services options</span></span>

<span data-ttu-id="23d28-105">En la tabla siguiente se describe las opciones de configuración de servicios de gRPC:</span><span class="sxs-lookup"><span data-stu-id="23d28-105">The following table describes options for configuring gRPC services:</span></span>

| <span data-ttu-id="23d28-106">Opción</span><span class="sxs-lookup"><span data-stu-id="23d28-106">Option</span></span> | <span data-ttu-id="23d28-107">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="23d28-107">Default Value</span></span> | <span data-ttu-id="23d28-108">Descripción</span><span class="sxs-lookup"><span data-stu-id="23d28-108">Description</span></span> |
| ------ | ------------- | ----------- |
| `SendMaxMessageSize` | `null` | <span data-ttu-id="23d28-109">El tamaño máximo del mensaje en bytes que se pueden enviar desde el servidor.</span><span class="sxs-lookup"><span data-stu-id="23d28-109">The maximum message size in bytes that can be sent from the server.</span></span> <span data-ttu-id="23d28-110">Intentando enviar un mensaje que supera los resultados de tamaño máximo de mensaje configurado en una excepción.</span><span class="sxs-lookup"><span data-stu-id="23d28-110">Attempting to send a message that exceeds the configured maximum message size results in an exception.</span></span> |
| `ReceiveMaxMessageSize` | <span data-ttu-id="23d28-111">4 MB</span><span class="sxs-lookup"><span data-stu-id="23d28-111">4 MB</span></span> | <span data-ttu-id="23d28-112">El tamaño máximo del mensaje en bytes, que puede ser recibido por el servidor.</span><span class="sxs-lookup"><span data-stu-id="23d28-112">The maximum message size in bytes that can be received by the server.</span></span> <span data-ttu-id="23d28-113">Si el servidor recibe un mensaje que supera este límite, produce una excepción.</span><span class="sxs-lookup"><span data-stu-id="23d28-113">If the server receives a message that exceeds this limit, it throws an exception.</span></span> <span data-ttu-id="23d28-114">Al aumentar este valor permite que el servidor recibir los mensajes más grandes, pero puede repercutir negativamente en el consumo de memoria.</span><span class="sxs-lookup"><span data-stu-id="23d28-114">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="23d28-115">Si `true`detallados se devuelven los mensajes de excepción a los clientes cuando se produce una excepción en un método de servicio.</span><span class="sxs-lookup"><span data-stu-id="23d28-115">If `true`, detailed exception messages are returned to clients when an exception is thrown in a service method.</span></span> <span data-ttu-id="23d28-116">De manera predeterminada, es `false`.</span><span class="sxs-lookup"><span data-stu-id="23d28-116">The default is `false`.</span></span> <span data-ttu-id="23d28-117">Si se establece en `true` puede producir la pérdida de información confidencial.</span><span class="sxs-lookup"><span data-stu-id="23d28-117">Setting this to `true` can leak sensitive information.</span></span> |
| `CompressionProviders` | <span data-ttu-id="23d28-118">gzip</span><span class="sxs-lookup"><span data-stu-id="23d28-118">gzip</span></span> | <span data-ttu-id="23d28-119">Una colección de proveedores de compresión utilizado para comprimir y descomprimir los mensajes.</span><span class="sxs-lookup"><span data-stu-id="23d28-119">A collection of compression providers used to compress and decompress messages.</span></span> <span data-ttu-id="23d28-120">Se pueden crear proveedores de compresión personalizado y agregados a la colección.</span><span class="sxs-lookup"><span data-stu-id="23d28-120">Custom compression providers can be created and added to the collection.</span></span> <span data-ttu-id="23d28-121">El valor predeterminado configurado el proveedor admite **gzip** compresión.</span><span class="sxs-lookup"><span data-stu-id="23d28-121">The default configured provider supports **gzip** compression.</span></span> |
| `ResponseCompressionAlgorithm` | `null` | <span data-ttu-id="23d28-122">El algoritmo de compresión utilizado para comprimir los mensajes enviados desde el servidor.</span><span class="sxs-lookup"><span data-stu-id="23d28-122">The compression algorithm used to compress messages sent from the server.</span></span> <span data-ttu-id="23d28-123">El algoritmo debe coincidir con un proveedor de compresión en `CompressionProviders`.</span><span class="sxs-lookup"><span data-stu-id="23d28-123">The algorithm must match a compression provider in `CompressionProviders`.</span></span> <span data-ttu-id="23d28-124">Para que el algorthm comprimir una respuesta, el cliente debe indicar admite el algoritmo enviando el **grpc-codificación aceptada** encabezado.</span><span class="sxs-lookup"><span data-stu-id="23d28-124">For the algorthm to compress a response the client must indicate it supports the algorithm by sending it in the **grpc-accept-encoding** header.</span></span> |
| `ResponseCompressionLevel` | `null` | <span data-ttu-id="23d28-125">El nivel de compresión para comprimir los mensajes enviados desde el servidor.</span><span class="sxs-lookup"><span data-stu-id="23d28-125">The compress level used to compress messages sent from the server.</span></span> |

<span data-ttu-id="23d28-126">Se pueden configurar opciones para todos los servicios al proporcionar un delegado de opciones para la `AddGrpc` llamar `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="23d28-126">Options can be configured for all services by providing an options delegate to the `AddGrpc` call in `Startup.ConfigureServices`.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc(options =>
    {
        options.EnableDetailedErrors = true;
    });
}
```

<span data-ttu-id="23d28-127">Opciones para un único servicio invalidan las opciones globales de `AddGrpc` y pueden configurarse mediante `AddServiceOptions<TService>`:</span><span class="sxs-lookup"><span data-stu-id="23d28-127">Options for a single service override the global options provided in `AddGrpc` and can be configured using `AddServiceOptions<TService>`:</span></span>

```csharp
services.AddGrpc().AddServiceOptions<MyService>(options =>
{
    options.ReceiveMaxMessageSize = 10 * 1024 * 1024; // 10 megabytes
});
```

## <a name="additional-resources"></a><span data-ttu-id="23d28-128">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="23d28-128">Additional resources</span></span>

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
