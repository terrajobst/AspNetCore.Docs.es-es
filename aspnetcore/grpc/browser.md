---
title: gRPC en aplicaciones de explorador
author: jamesnk
description: Aprenda a configurar los servicios de gRPC en ASP.NET Core a los que se puede llamar desde aplicaciones del explorador mediante gRPC-Web.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 01/24/2020
uid: grpc/browser
ms.openlocfilehash: 6359c3b76b3cb1ba2b6d9f9a989f64cbf4c4379d
ms.sourcegitcommit: b5ceb0a46d0254cc3425578116e2290142eec0f0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/28/2020
ms.locfileid: "76830637"
---
# <a name="grpc-in-browser-apps"></a><span data-ttu-id="b1871-103">gRPC en aplicaciones de explorador</span><span class="sxs-lookup"><span data-stu-id="b1871-103">gRPC in browser apps</span></span>

<span data-ttu-id="b1871-104">Por [James Newton-King](https://twitter.com/jamesnk)</span><span class="sxs-lookup"><span data-stu-id="b1871-104">By [James Newton-King](https://twitter.com/jamesnk)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b1871-105">**gRPC: compatibilidad Web en .NET es experimental**</span><span class="sxs-lookup"><span data-stu-id="b1871-105">**gRPC-Web support in .NET is experimental**</span></span>
>
> <span data-ttu-id="b1871-106">gRPC-web para .NET es un proyecto experimental, no un producto confirmado.</span><span class="sxs-lookup"><span data-stu-id="b1871-106">gRPC-Web for .NET is an experimental project, not a committed product.</span></span> <span data-ttu-id="b1871-107">Queremos:</span><span class="sxs-lookup"><span data-stu-id="b1871-107">We want to:</span></span>
>
> * <span data-ttu-id="b1871-108">Pruebe que nuestro enfoque para implementar gRPC-web funciona.</span><span class="sxs-lookup"><span data-stu-id="b1871-108">Test that our approach to implementing gRPC-Web works.</span></span>
> * <span data-ttu-id="b1871-109">Obtenga comentarios sobre si este enfoque es útil para los desarrolladores de .NET en comparación con la manera tradicional de configurar gRPC-web a través de un proxy.</span><span class="sxs-lookup"><span data-stu-id="b1871-109">Get feedback on if this approach is useful to .NET developers compared to the traditional way of setting up gRPC-Web via a proxy.</span></span>
>
> <span data-ttu-id="b1871-110">Deje comentarios en [https://github.com/grpc/grpc-dotnet](https://github.com/grpc/grpc-dotnet) para asegurarse de que crea algo que los desarrolladores desean y son productivos con.</span><span class="sxs-lookup"><span data-stu-id="b1871-110">Please leave feedback at [https://github.com/grpc/grpc-dotnet](https://github.com/grpc/grpc-dotnet) to ensure we build something that developers like and are productive with.</span></span>

<span data-ttu-id="b1871-111">No es posible llamar a un servicio gRPC de HTTP/2 desde una aplicación basada en explorador.</span><span class="sxs-lookup"><span data-stu-id="b1871-111">It is not possible to call a HTTP/2 gRPC service from a browser-based app.</span></span> <span data-ttu-id="b1871-112">[gRPC-web](https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-WEB.md) es un protocolo que permite a las aplicaciones de JavaScript y increíbles del explorador llamar a gRPC Services.</span><span class="sxs-lookup"><span data-stu-id="b1871-112">[gRPC-Web](https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-WEB.md) is a protocol that allows browser JavaScript and Blazor apps to call gRPC services.</span></span> <span data-ttu-id="b1871-113">En este artículo se explica cómo usar gRPC-Web en .NET Core.</span><span class="sxs-lookup"><span data-stu-id="b1871-113">This article explains how to use gRPC-Web in .NET Core.</span></span>

## <a name="configure-grpc-web-in-aspnet-core"></a><span data-ttu-id="b1871-114">Configuración de gRPC-Web en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b1871-114">Configure gRPC-Web in ASP.NET Core</span></span>

<span data-ttu-id="b1871-115">gRPC Services hospedados en ASP.NET Core se pueden configurar para admitir gRPC-web junto con HTTP/2 gRPC.</span><span class="sxs-lookup"><span data-stu-id="b1871-115">gRPC services hosted in ASP.NET Core can be configured to support gRPC-Web alongside HTTP/2 gRPC.</span></span> <span data-ttu-id="b1871-116">gRPC-web no requiere ningún cambio en los servicios.</span><span class="sxs-lookup"><span data-stu-id="b1871-116">gRPC-Web does not require any changes to services.</span></span> <span data-ttu-id="b1871-117">La única modificación es la configuración de inicio.</span><span class="sxs-lookup"><span data-stu-id="b1871-117">The only modification is startup configuration.</span></span>

<span data-ttu-id="b1871-118">Para habilitar gRPC-web con un servicio de gRPC ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="b1871-118">To enable gRPC-Web with an ASP.NET Core gRPC service:</span></span>

* <span data-ttu-id="b1871-119">Agregue una referencia al paquete [GRPC. AspNetCore. Web](https://www.nuget.org/packages/Grpc.AspNetCore.Web) .</span><span class="sxs-lookup"><span data-stu-id="b1871-119">Add a reference to the [Grpc.AspNetCore.Web](https://www.nuget.org/packages/Grpc.AspNetCore.Web) package.</span></span>
* <span data-ttu-id="b1871-120">Configure la aplicación para que use gRPC-Web agregando `AddGrpcWeb` y `UseGrpcWeb` a *Startup.CS*:</span><span class="sxs-lookup"><span data-stu-id="b1871-120">Configure the app to use gRPC-Web by adding `AddGrpcWeb` and `UseGrpcWeb` to *Startup.cs*:</span></span>

[!code-csharp[](~/grpc/browser/sample/Startup.cs?name=snippet_1&highlight=3,10,14)]

<span data-ttu-id="b1871-121">El código anterior:</span><span class="sxs-lookup"><span data-stu-id="b1871-121">The preceding code:</span></span>

* <span data-ttu-id="b1871-122">Agrega el middleware gRPC-Web, `UseGrpcWeb`, después del enrutamiento y antes de los extremos.</span><span class="sxs-lookup"><span data-stu-id="b1871-122">Adds the gRPC-Web middleware, `UseGrpcWeb`, after routing and before endpoints.</span></span>
* <span data-ttu-id="b1871-123">Especifica que el método `endpoints.MapGrpcService<GreeterService>()` admite gRPC-web con `EnableGrpcWeb`.</span><span class="sxs-lookup"><span data-stu-id="b1871-123">Specifies the `endpoints.MapGrpcService<GreeterService>()` method supports gRPC-Web with `EnableGrpcWeb`.</span></span> 

<span data-ttu-id="b1871-124">Como alternativa, configure todos los servicios para admitir gRPC-Web agregando `services.AddGrpcWeb(o => o.GrpcWebEnabled = true);` a ConfigureServices.</span><span class="sxs-lookup"><span data-stu-id="b1871-124">Alternatively, configure all services to support gRPC-Web by adding `services.AddGrpcWeb(o => o.GrpcWebEnabled = true);` to ConfigureServices.</span></span>

[!code-csharp[](~/grpc/browser/sample/AllServicesSupportExample_Startup.cs?name=snippet_1&highlight=5,12,16)]

<span data-ttu-id="b1871-125">Es posible que sea necesario realizar alguna configuración adicional para llamar a gRPC-web desde el explorador, como la configuración de ASP.NET Core para que sea compatible con CORS.</span><span class="sxs-lookup"><span data-stu-id="b1871-125">Some additional configuration may be required to call gRPC-Web from the browser, such as configuring ASP.NET Core to support CORS.</span></span> <span data-ttu-id="b1871-126">Para obtener más información, consulte [compatibilidad con CORS](xref:security/cors).</span><span class="sxs-lookup"><span data-stu-id="b1871-126">For more information, see [support CORS](xref:security/cors).</span></span>

## <a name="call-grpc-web-from-the-browser"></a><span data-ttu-id="b1871-127">Llamar a gRPC-web desde el explorador</span><span class="sxs-lookup"><span data-stu-id="b1871-127">Call gRPC-Web from the browser</span></span>

<span data-ttu-id="b1871-128">Las aplicaciones de explorador pueden usar gRPC-web para llamar a los servicios de gRPC.</span><span class="sxs-lookup"><span data-stu-id="b1871-128">Browser apps can use gRPC-Web to call gRPC services.</span></span> <span data-ttu-id="b1871-129">Hay algunos requisitos y limitaciones al llamar a gRPC Services con gRPC-web desde el explorador:</span><span class="sxs-lookup"><span data-stu-id="b1871-129">There are some requirements and limitations when calling gRPC services with gRPC-Web from the browser:</span></span>

* <span data-ttu-id="b1871-130">El servidor debe estar configurado para admitir gRPC-Web.</span><span class="sxs-lookup"><span data-stu-id="b1871-130">The server must have been configured to support gRPC-Web.</span></span>
* <span data-ttu-id="b1871-131">No se admite la transmisión por secuencias de cliente y las llamadas de streaming bidireccional.</span><span class="sxs-lookup"><span data-stu-id="b1871-131">Client streaming and bidirectional streaming calls aren't supported.</span></span> <span data-ttu-id="b1871-132">Se admite la transmisión por secuencias del servidor.</span><span class="sxs-lookup"><span data-stu-id="b1871-132">Server streaming is supported.</span></span>
* <span data-ttu-id="b1871-133">La llamada a gRPC Services en un dominio diferente requiere que [CORS](xref:security/cors) esté configurado en el servidor.</span><span class="sxs-lookup"><span data-stu-id="b1871-133">Calling gRPC services on a different domain requires [CORS](xref:security/cors) to be configured on the server.</span></span>

### <a name="javascript-grpc-web-client"></a><span data-ttu-id="b1871-134">GRPC de JavaScript: cliente web</span><span class="sxs-lookup"><span data-stu-id="b1871-134">JavaScript gRPC-Web client</span></span>

<span data-ttu-id="b1871-135">Hay un cliente web gRPC de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="b1871-135">There is a JavaScript gRPC-Web client.</span></span> <span data-ttu-id="b1871-136">Para obtener instrucciones sobre cómo usar gRPC-web desde JavaScript, consulte [escritura de código de cliente de JavaScript con gRPC-web](https://github.com/grpc/grpc-web/tree/master/net/grpc/gateway/examples/helloworld#write-client-code).</span><span class="sxs-lookup"><span data-stu-id="b1871-136">For instructions on how to use gRPC-Web from JavaScript, see [write JavaScript client code with gRPC-Web](https://github.com/grpc/grpc-web/tree/master/net/grpc/gateway/examples/helloworld#write-client-code).</span></span>

### <a name="configure-grpc-web-with-the-net-grpc-client"></a><span data-ttu-id="b1871-137">Configuración de gRPC-web con el cliente de gRPC de .NET</span><span class="sxs-lookup"><span data-stu-id="b1871-137">Configure gRPC-Web with the .NET gRPC client</span></span>

<span data-ttu-id="b1871-138">El cliente gRPC de .NET puede configurarse para realizar llamadas gRPC-Web.</span><span class="sxs-lookup"><span data-stu-id="b1871-138">The .NET gRPC client can be configured to make gRPC-Web calls.</span></span> <span data-ttu-id="b1871-139">Esto resulta útil para las aplicaciones de [Webassembly increíbles](xref:blazor/index#blazor-webassembly) , que se hospedan en el explorador y tienen las mismas limitaciones de http del código JavaScript.</span><span class="sxs-lookup"><span data-stu-id="b1871-139">This is useful for [Blazor WebAssembly](xref:blazor/index#blazor-webassembly) apps, which are hosted in the browser and have the same HTTP limitations of JavaScript code.</span></span> <span data-ttu-id="b1871-140">Llamar a gRPC-web con un cliente .NET es [igual que http/2 gRPC](xref:grpc/client).</span><span class="sxs-lookup"><span data-stu-id="b1871-140">Calling gRPC-Web with a .NET client is [the same as HTTP/2 gRPC](xref:grpc/client).</span></span> <span data-ttu-id="b1871-141">La única modificación es cómo se crea el canal.</span><span class="sxs-lookup"><span data-stu-id="b1871-141">The only modification is how the channel is created.</span></span>

<span data-ttu-id="b1871-142">Para usar gRPC-Web:</span><span class="sxs-lookup"><span data-stu-id="b1871-142">To use gRPC-Web:</span></span>

* <span data-ttu-id="b1871-143">Agregue una referencia al paquete [GRPC .net. Client. Web](https://www.nuget.org/packages/Grpc.Net.Client.Web) .</span><span class="sxs-lookup"><span data-stu-id="b1871-143">Add a reference to the [Grpc.Net.Client.Web](https://www.nuget.org/packages/Grpc.Net.Client.Web) package.</span></span>
* <span data-ttu-id="b1871-144">Configure el canal para usar el `GrpcWebHandler`:</span><span class="sxs-lookup"><span data-stu-id="b1871-144">Configure the channel to use the `GrpcWebHandler`:</span></span>

[!code-csharp[](~/grpc/browser/sample/Handler.cs?name=snippet_1)]

<span data-ttu-id="b1871-145">El código anterior:</span><span class="sxs-lookup"><span data-stu-id="b1871-145">The preceding code:</span></span>

* <span data-ttu-id="b1871-146">Configura un canal para utilizar gRPC-Web.</span><span class="sxs-lookup"><span data-stu-id="b1871-146">Configures a channel to use gRPC-Web.</span></span>
* <span data-ttu-id="b1871-147">Crea un cliente y realiza una llamada mediante el canal.</span><span class="sxs-lookup"><span data-stu-id="b1871-147">Creates a client and makes a call using the channel.</span></span>

<span data-ttu-id="b1871-148">La `GrpcWebHandler` tiene las siguientes opciones de configuración cuando se crea:</span><span class="sxs-lookup"><span data-stu-id="b1871-148">The `GrpcWebHandler` has the following configuration options when created:</span></span>

* <span data-ttu-id="b1871-149">**InnerHandler**: el <xref:System.Net.Http.HttpMessageHandler> subyacente que realiza la llamada http, por ejemplo, `HttpClientHandler`.</span><span class="sxs-lookup"><span data-stu-id="b1871-149">**InnerHandler**: The underlying <xref:System.Net.Http.HttpMessageHandler> that makes the HTTP call, for example, `HttpClientHandler`.</span></span>
* <span data-ttu-id="b1871-150">**Mode**: `GrpcWebMode` enum.</span><span class="sxs-lookup"><span data-stu-id="b1871-150">**Mode**: `GrpcWebMode` enum.</span></span> <span data-ttu-id="b1871-151">`GrpcWebMode.GrpcWebText` configura el contenido para que esté codificado en Base64, lo que es necesario para admitir llamadas de transmisión por secuencias del servidor.</span><span class="sxs-lookup"><span data-stu-id="b1871-151">`GrpcWebMode.GrpcWebText` configures content to be base64 encoded, which is required to support server streaming calls.</span></span>
* <span data-ttu-id="b1871-152">**HttpVersion**: `Version`del protocolo http.</span><span class="sxs-lookup"><span data-stu-id="b1871-152">**HttpVersion**: HTTP protocol `Version`.</span></span> <span data-ttu-id="b1871-153">gRPC-web no requiere un protocolo específico y no especificará uno al efectuar una solicitud, a menos que se configure.</span><span class="sxs-lookup"><span data-stu-id="b1871-153">gRPC-Web doesn't require a specific protocol and won't specify one when making a request unless configured.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b1871-154">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="b1871-154">Additional resources</span></span>

* [<span data-ttu-id="b1871-155">gRPC para el proyecto de GitHub de clientes Web</span><span class="sxs-lookup"><span data-stu-id="b1871-155">gRPC for Web Clients GitHub project</span></span>](https://github.com/grpc/grpc-web)
* <xref:security/cors>
