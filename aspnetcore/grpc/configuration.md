---
title: gRPC para la configuración de ASP.NET Core
author: jamesnk
description: Obtenga información sobre cómo configurar gRPC para aplicaciones ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 05/30/2019
uid: grpc/configuration
ms.openlocfilehash: e269d701f45c0b852a9006107f0162cc5af2c38a
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2019
ms.locfileid: "67814925"
---
# <a name="grpc-for-aspnet-core-configuration"></a><span data-ttu-id="9f958-103">gRPC para la configuración de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9f958-103">gRPC for ASP.NET Core configuration</span></span>

## <a name="configure-services-options"></a><span data-ttu-id="9f958-104">Configurar las opciones de servicios</span><span class="sxs-lookup"><span data-stu-id="9f958-104">Configure services options</span></span>

<span data-ttu-id="9f958-105">En la tabla siguiente se describe las opciones de configuración de servicios de gRPC:</span><span class="sxs-lookup"><span data-stu-id="9f958-105">The following table describes options for configuring gRPC services:</span></span>

| <span data-ttu-id="9f958-106">Opción</span><span class="sxs-lookup"><span data-stu-id="9f958-106">Option</span></span> | <span data-ttu-id="9f958-107">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="9f958-107">Default Value</span></span> | <span data-ttu-id="9f958-108">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="9f958-108">Description</span></span> |
| ------ | ------------- | ----------- |
| `SendMaxMessageSize` | `null` | <span data-ttu-id="9f958-109">El tamaño máximo del mensaje en bytes que se pueden enviar desde el servidor.</span><span class="sxs-lookup"><span data-stu-id="9f958-109">The maximum message size in bytes that can be sent from the server.</span></span> <span data-ttu-id="9f958-110">Intentando enviar un mensaje que supera los resultados de tamaño máximo de mensaje configurado en una excepción.</span><span class="sxs-lookup"><span data-stu-id="9f958-110">Attempting to send a message that exceeds the configured maximum message size results in an exception.</span></span> |
| `ReceiveMaxMessageSize` | <span data-ttu-id="9f958-111">4 MB</span><span class="sxs-lookup"><span data-stu-id="9f958-111">4 MB</span></span> | <span data-ttu-id="9f958-112">El tamaño máximo del mensaje en bytes, que puede ser recibido por el servidor.</span><span class="sxs-lookup"><span data-stu-id="9f958-112">The maximum message size in bytes that can be received by the server.</span></span> <span data-ttu-id="9f958-113">Si el servidor recibe un mensaje que supera este límite, produce una excepción.</span><span class="sxs-lookup"><span data-stu-id="9f958-113">If the server receives a message that exceeds this limit, it throws an exception.</span></span> <span data-ttu-id="9f958-114">Al aumentar este valor permite que el servidor recibir los mensajes más grandes, pero puede repercutir negativamente en el consumo de memoria.</span><span class="sxs-lookup"><span data-stu-id="9f958-114">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="9f958-115">Si `true`detallados se devuelven los mensajes de excepción a los clientes cuando se produce una excepción en un método de servicio.</span><span class="sxs-lookup"><span data-stu-id="9f958-115">If `true`, detailed exception messages are returned to clients when an exception is thrown in a service method.</span></span> <span data-ttu-id="9f958-116">El valor predeterminado es `false`.</span><span class="sxs-lookup"><span data-stu-id="9f958-116">The default is `false`.</span></span> <span data-ttu-id="9f958-117">Establecer `EnableDetailedErrors` a `true` puede producir la pérdida de información confidencial.</span><span class="sxs-lookup"><span data-stu-id="9f958-117">Setting `EnableDetailedErrors` to `true` can leak sensitive information.</span></span> |
| `CompressionProviders` | <span data-ttu-id="9f958-118">gzip</span><span class="sxs-lookup"><span data-stu-id="9f958-118">gzip</span></span> | <span data-ttu-id="9f958-119">Una colección de proveedores de compresión utilizado para comprimir y descomprimir los mensajes.</span><span class="sxs-lookup"><span data-stu-id="9f958-119">A collection of compression providers used to compress and decompress messages.</span></span> <span data-ttu-id="9f958-120">Se pueden crear proveedores de compresión personalizado y agregados a la colección.</span><span class="sxs-lookup"><span data-stu-id="9f958-120">Custom compression providers can be created and added to the collection.</span></span> <span data-ttu-id="9f958-121">El valor predeterminado configurado el proveedor admite **gzip** compresión.</span><span class="sxs-lookup"><span data-stu-id="9f958-121">The default configured provider supports **gzip** compression.</span></span> |
| `ResponseCompressionAlgorithm` | `null` | <span data-ttu-id="9f958-122">El algoritmo de compresión utilizado para comprimir los mensajes enviados desde el servidor.</span><span class="sxs-lookup"><span data-stu-id="9f958-122">The compression algorithm used to compress messages sent from the server.</span></span> <span data-ttu-id="9f958-123">El algoritmo debe coincidir con un proveedor de compresión en `CompressionProviders`.</span><span class="sxs-lookup"><span data-stu-id="9f958-123">The algorithm must match a compression provider in `CompressionProviders`.</span></span> <span data-ttu-id="9f958-124">Para que el algoritmo comprimir una respuesta, el cliente debe indicar admite el algoritmo enviando el **grpc-codificación aceptada** encabezado.</span><span class="sxs-lookup"><span data-stu-id="9f958-124">For the algorithm to compress a response, the client must indicate it supports the algorithm by sending it in the **grpc-accept-encoding** header.</span></span> |
| `ResponseCompressionLevel` | `null` | <span data-ttu-id="9f958-125">El nivel de compresión para comprimir los mensajes enviados desde el servidor.</span><span class="sxs-lookup"><span data-stu-id="9f958-125">The compress level used to compress messages sent from the server.</span></span> |

<span data-ttu-id="9f958-126">Se pueden configurar opciones para todos los servicios al proporcionar un delegado de opciones para la `AddGrpc` llamar `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="9f958-126">Options can be configured for all services by providing an options delegate to the `AddGrpc` call in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](~/grpc/configuration/sample/GrcpService/Startup.cs?name=snippet)]

<span data-ttu-id="9f958-127">Opciones para un único servicio invalidan las opciones globales de `AddGrpc` y pueden configurarse mediante `AddServiceOptions<TService>`:</span><span class="sxs-lookup"><span data-stu-id="9f958-127">Options for a single service override the global options provided in `AddGrpc` and can be configured using `AddServiceOptions<TService>`:</span></span>

[!code-csharp[](~/grpc/configuration/sample/GrcpService/Startup2.cs?name=snippet)]

## <a name="configure-client-options"></a><span data-ttu-id="9f958-128">Configurar las opciones de cliente</span><span class="sxs-lookup"><span data-stu-id="9f958-128">Configure client options</span></span>

<span data-ttu-id="9f958-129">El código siguiente establece el envío máximo de cliente y el tamaño de mensaje de recepción:</span><span class="sxs-lookup"><span data-stu-id="9f958-129">The following code sets the client maximum send and receive message size:</span></span>

[!code-csharp[](~/grpc/configuration/sample/Program.cs?name=snippet&highlight=3-6)]

## <a name="additional-resources"></a><span data-ttu-id="9f958-130">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="9f958-130">Additional resources</span></span>

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
