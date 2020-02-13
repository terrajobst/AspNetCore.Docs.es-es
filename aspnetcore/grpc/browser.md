---
title: Uso de gRPC en aplicaciones de explorador
author: jamesnk
description: Aprenda a configurar los servicios de gRPC en ASP.NET Core a los que se puede llamar desde aplicaciones del explorador mediante gRPC-Web.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 02/10/2020
uid: grpc/browser
ms.openlocfilehash: 333fc8c4277bbac47042d4904c276e963186914a
ms.sourcegitcommit: 85564ee396c74c7651ac47dd45082f3f1803f7a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2020
ms.locfileid: "77172271"
---
# <a name="use-grpc-in-browser-apps"></a><span data-ttu-id="6be8c-103">Uso de gRPC en aplicaciones de explorador</span><span class="sxs-lookup"><span data-stu-id="6be8c-103">Use gRPC in browser apps</span></span>

<span data-ttu-id="6be8c-104">Por [James Newton-King](https://twitter.com/jamesnk)</span><span class="sxs-lookup"><span data-stu-id="6be8c-104">By [James Newton-King](https://twitter.com/jamesnk)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6be8c-105">**gRPC: compatibilidad Web en .NET es experimental**</span><span class="sxs-lookup"><span data-stu-id="6be8c-105">**gRPC-Web support in .NET is experimental**</span></span>
>
> <span data-ttu-id="6be8c-106">gRPC-web para .NET es un proyecto experimental, no un producto confirmado.</span><span class="sxs-lookup"><span data-stu-id="6be8c-106">gRPC-Web for .NET is an experimental project, not a committed product.</span></span> <span data-ttu-id="6be8c-107">Queremos:</span><span class="sxs-lookup"><span data-stu-id="6be8c-107">We want to:</span></span>
>
> * <span data-ttu-id="6be8c-108">Pruebe que nuestro enfoque para implementar gRPC-web funciona.</span><span class="sxs-lookup"><span data-stu-id="6be8c-108">Test that our approach to implementing gRPC-Web works.</span></span>
> * <span data-ttu-id="6be8c-109">Obtenga comentarios sobre si este enfoque es útil para los desarrolladores de .NET en comparación con la manera tradicional de configurar gRPC-web a través de un proxy.</span><span class="sxs-lookup"><span data-stu-id="6be8c-109">Get feedback on if this approach is useful to .NET developers compared to the traditional way of setting up gRPC-Web via a proxy.</span></span>
>
> <span data-ttu-id="6be8c-110">Deje comentarios en [https://github.com/grpc/grpc-dotnet](https://github.com/grpc/grpc-dotnet) para asegurarse de que crea algo que los desarrolladores desean y son productivos con.</span><span class="sxs-lookup"><span data-stu-id="6be8c-110">Please leave feedback at [https://github.com/grpc/grpc-dotnet](https://github.com/grpc/grpc-dotnet) to ensure we build something that developers like and are productive with.</span></span>

<span data-ttu-id="6be8c-111">No es posible llamar a un servicio gRPC de HTTP/2 desde una aplicación basada en explorador.</span><span class="sxs-lookup"><span data-stu-id="6be8c-111">It is not possible to call a HTTP/2 gRPC service from a browser-based app.</span></span> <span data-ttu-id="6be8c-112">[gRPC-web](https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-WEB.md) es un protocolo que permite a las aplicaciones de JavaScript y increíbles del explorador llamar a gRPC Services.</span><span class="sxs-lookup"><span data-stu-id="6be8c-112">[gRPC-Web](https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-WEB.md) is a protocol that allows browser JavaScript and Blazor apps to call gRPC services.</span></span> <span data-ttu-id="6be8c-113">En este artículo se explica cómo usar gRPC-Web en .NET Core.</span><span class="sxs-lookup"><span data-stu-id="6be8c-113">This article explains how to use gRPC-Web in .NET Core.</span></span>

## <a name="configure-grpc-web-in-aspnet-core"></a><span data-ttu-id="6be8c-114">Configuración de gRPC-Web en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6be8c-114">Configure gRPC-Web in ASP.NET Core</span></span>

<span data-ttu-id="6be8c-115">gRPC Services hospedados en ASP.NET Core se pueden configurar para admitir gRPC-web junto con HTTP/2 gRPC.</span><span class="sxs-lookup"><span data-stu-id="6be8c-115">gRPC services hosted in ASP.NET Core can be configured to support gRPC-Web alongside HTTP/2 gRPC.</span></span> <span data-ttu-id="6be8c-116">gRPC-web no requiere ningún cambio en los servicios.</span><span class="sxs-lookup"><span data-stu-id="6be8c-116">gRPC-Web does not require any changes to services.</span></span> <span data-ttu-id="6be8c-117">La única modificación es la configuración de inicio.</span><span class="sxs-lookup"><span data-stu-id="6be8c-117">The only modification is startup configuration.</span></span>

<span data-ttu-id="6be8c-118">Para habilitar gRPC-web con un servicio de gRPC ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="6be8c-118">To enable gRPC-Web with an ASP.NET Core gRPC service:</span></span>

* <span data-ttu-id="6be8c-119">Agregue una referencia al paquete [GRPC. AspNetCore. Web](https://www.nuget.org/packages/Grpc.AspNetCore.Web) .</span><span class="sxs-lookup"><span data-stu-id="6be8c-119">Add a reference to the [Grpc.AspNetCore.Web](https://www.nuget.org/packages/Grpc.AspNetCore.Web) package.</span></span>
* <span data-ttu-id="6be8c-120">Configure la aplicación para que use gRPC-Web agregando `AddGrpcWeb` y `UseGrpcWeb` a *Startup.CS*:</span><span class="sxs-lookup"><span data-stu-id="6be8c-120">Configure the app to use gRPC-Web by adding `AddGrpcWeb` and `UseGrpcWeb` to *Startup.cs*:</span></span>

[!code-csharp[](~/grpc/browser/sample/Startup.cs?name=snippet_1&highlight=10,14)]

<span data-ttu-id="6be8c-121">El código anterior:</span><span class="sxs-lookup"><span data-stu-id="6be8c-121">The preceding code:</span></span>

* <span data-ttu-id="6be8c-122">Agrega el middleware gRPC-Web, `UseGrpcWeb`, después del enrutamiento y antes de los extremos.</span><span class="sxs-lookup"><span data-stu-id="6be8c-122">Adds the gRPC-Web middleware, `UseGrpcWeb`, after routing and before endpoints.</span></span>
* <span data-ttu-id="6be8c-123">Especifica que el método `endpoints.MapGrpcService<GreeterService>()` admite gRPC-web con `EnableGrpcWeb`.</span><span class="sxs-lookup"><span data-stu-id="6be8c-123">Specifies the `endpoints.MapGrpcService<GreeterService>()` method supports gRPC-Web with `EnableGrpcWeb`.</span></span> 

<span data-ttu-id="6be8c-124">Como alternativa, configure todos los servicios para admitir gRPC-Web agregando `services.AddGrpcWeb(o => o.GrpcWebEnabled = true);` a ConfigureServices.</span><span class="sxs-lookup"><span data-stu-id="6be8c-124">Alternatively, configure all services to support gRPC-Web by adding `services.AddGrpcWeb(o => o.GrpcWebEnabled = true);` to ConfigureServices.</span></span>

[!code-csharp[](~/grpc/browser/sample/AllServicesSupportExample_Startup.cs?name=snippet_1&highlight=6,13)]

<span data-ttu-id="6be8c-125">Es posible que sea necesario realizar alguna configuración adicional para llamar a gRPC-web desde el explorador, como la configuración de ASP.NET Core para que sea compatible con CORS.</span><span class="sxs-lookup"><span data-stu-id="6be8c-125">Some additional configuration may be required to call gRPC-Web from the browser, such as configuring ASP.NET Core to support CORS.</span></span> <span data-ttu-id="6be8c-126">Para obtener más información, consulte [compatibilidad con CORS](xref:security/cors).</span><span class="sxs-lookup"><span data-stu-id="6be8c-126">For more information, see [support CORS](xref:security/cors).</span></span>

## <a name="call-grpc-web-from-the-browser"></a><span data-ttu-id="6be8c-127">Llamar a gRPC-web desde el explorador</span><span class="sxs-lookup"><span data-stu-id="6be8c-127">Call gRPC-Web from the browser</span></span>

<span data-ttu-id="6be8c-128">Las aplicaciones de explorador pueden usar gRPC-web para llamar a los servicios de gRPC.</span><span class="sxs-lookup"><span data-stu-id="6be8c-128">Browser apps can use gRPC-Web to call gRPC services.</span></span> <span data-ttu-id="6be8c-129">Hay algunos requisitos y limitaciones al llamar a gRPC Services con gRPC-web desde el explorador:</span><span class="sxs-lookup"><span data-stu-id="6be8c-129">There are some requirements and limitations when calling gRPC services with gRPC-Web from the browser:</span></span>

* <span data-ttu-id="6be8c-130">El servidor debe estar configurado para admitir gRPC-Web.</span><span class="sxs-lookup"><span data-stu-id="6be8c-130">The server must have been configured to support gRPC-Web.</span></span>
* <span data-ttu-id="6be8c-131">No se admite la transmisión por secuencias de cliente y las llamadas de streaming bidireccional.</span><span class="sxs-lookup"><span data-stu-id="6be8c-131">Client streaming and bidirectional streaming calls aren't supported.</span></span> <span data-ttu-id="6be8c-132">Se admite la transmisión por secuencias del servidor.</span><span class="sxs-lookup"><span data-stu-id="6be8c-132">Server streaming is supported.</span></span>
* <span data-ttu-id="6be8c-133">La llamada a gRPC Services en un dominio diferente requiere que [CORS](xref:security/cors) esté configurado en el servidor.</span><span class="sxs-lookup"><span data-stu-id="6be8c-133">Calling gRPC services on a different domain requires [CORS](xref:security/cors) to be configured on the server.</span></span>

### <a name="javascript-grpc-web-client"></a><span data-ttu-id="6be8c-134">GRPC de JavaScript: cliente web</span><span class="sxs-lookup"><span data-stu-id="6be8c-134">JavaScript gRPC-Web client</span></span>

<span data-ttu-id="6be8c-135">Hay un cliente web gRPC de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="6be8c-135">There is a JavaScript gRPC-Web client.</span></span> <span data-ttu-id="6be8c-136">Para obtener instrucciones sobre cómo usar gRPC-web desde JavaScript, consulte [escritura de código de cliente de JavaScript con gRPC-web](https://github.com/grpc/grpc-web/tree/master/net/grpc/gateway/examples/helloworld#write-client-code).</span><span class="sxs-lookup"><span data-stu-id="6be8c-136">For instructions on how to use gRPC-Web from JavaScript, see [write JavaScript client code with gRPC-Web](https://github.com/grpc/grpc-web/tree/master/net/grpc/gateway/examples/helloworld#write-client-code).</span></span>

### <a name="configure-grpc-web-with-the-net-grpc-client"></a><span data-ttu-id="6be8c-137">Configuración de gRPC-web con el cliente de gRPC de .NET</span><span class="sxs-lookup"><span data-stu-id="6be8c-137">Configure gRPC-Web with the .NET gRPC client</span></span>

<span data-ttu-id="6be8c-138">El cliente gRPC de .NET puede configurarse para realizar llamadas gRPC-Web.</span><span class="sxs-lookup"><span data-stu-id="6be8c-138">The .NET gRPC client can be configured to make gRPC-Web calls.</span></span> <span data-ttu-id="6be8c-139">Esto resulta útil para las aplicaciones de [Webassembly increíbles](xref:blazor/index#blazor-webassembly) , que se hospedan en el explorador y tienen las mismas limitaciones de http del código JavaScript.</span><span class="sxs-lookup"><span data-stu-id="6be8c-139">This is useful for [Blazor WebAssembly](xref:blazor/index#blazor-webassembly) apps, which are hosted in the browser and have the same HTTP limitations of JavaScript code.</span></span> <span data-ttu-id="6be8c-140">Llamar a gRPC-web con un cliente .NET es [igual que http/2 gRPC](xref:grpc/client).</span><span class="sxs-lookup"><span data-stu-id="6be8c-140">Calling gRPC-Web with a .NET client is [the same as HTTP/2 gRPC](xref:grpc/client).</span></span> <span data-ttu-id="6be8c-141">La única modificación es cómo se crea el canal.</span><span class="sxs-lookup"><span data-stu-id="6be8c-141">The only modification is how the channel is created.</span></span>

<span data-ttu-id="6be8c-142">Para usar gRPC-Web:</span><span class="sxs-lookup"><span data-stu-id="6be8c-142">To use gRPC-Web:</span></span>

* <span data-ttu-id="6be8c-143">Agregue una referencia al paquete [GRPC .net. Client. Web](https://www.nuget.org/packages/Grpc.Net.Client.Web) .</span><span class="sxs-lookup"><span data-stu-id="6be8c-143">Add a reference to the [Grpc.Net.Client.Web](https://www.nuget.org/packages/Grpc.Net.Client.Web) package.</span></span>
* <span data-ttu-id="6be8c-144">Asegúrese de que la referencia al paquete [GRPC .net. Client](https://www.nuget.org/packages/Grpc.Net.Client) es 2.27.0 o superior.</span><span class="sxs-lookup"><span data-stu-id="6be8c-144">Ensure the reference to [Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client) package is 2.27.0 or greater.</span></span>
* <span data-ttu-id="6be8c-145">Configure el canal para usar el `GrpcWebHandler`:</span><span class="sxs-lookup"><span data-stu-id="6be8c-145">Configure the channel to use the `GrpcWebHandler`:</span></span>

[!code-csharp[](~/grpc/browser/sample/Handler.cs?name=snippet_1)]

<span data-ttu-id="6be8c-146">El código anterior:</span><span class="sxs-lookup"><span data-stu-id="6be8c-146">The preceding code:</span></span>

* <span data-ttu-id="6be8c-147">Configura un canal para utilizar gRPC-Web.</span><span class="sxs-lookup"><span data-stu-id="6be8c-147">Configures a channel to use gRPC-Web.</span></span>
* <span data-ttu-id="6be8c-148">Crea un cliente y realiza una llamada mediante el canal.</span><span class="sxs-lookup"><span data-stu-id="6be8c-148">Creates a client and makes a call using the channel.</span></span>

<span data-ttu-id="6be8c-149">La `GrpcWebHandler` tiene las siguientes opciones de configuración cuando se crea:</span><span class="sxs-lookup"><span data-stu-id="6be8c-149">The `GrpcWebHandler` has the following configuration options when created:</span></span>

* <span data-ttu-id="6be8c-150">**InnerHandler**: el <xref:System.Net.Http.HttpMessageHandler> subyacente que realiza la solicitud HTTP de gRPC, por ejemplo, `HttpClientHandler`.</span><span class="sxs-lookup"><span data-stu-id="6be8c-150">**InnerHandler**: The underlying <xref:System.Net.Http.HttpMessageHandler> that makes the gRPC HTTP request, for example, `HttpClientHandler`.</span></span>
* <span data-ttu-id="6be8c-151">**Mode**: un tipo de enumeración que especifica si la solicitud de solicitud HTTP gRPC `Content-Type` es `application/grpc-web` o `application/grpc-web-text`.</span><span class="sxs-lookup"><span data-stu-id="6be8c-151">**Mode**: An enumeration type that specifies whether the gRPC HTTP request request `Content-Type` is `application/grpc-web` or `application/grpc-web-text`.</span></span>
    * <span data-ttu-id="6be8c-152">`GrpcWebMode.GrpcWeb` configura el contenido que se va a enviar sin codificación.</span><span class="sxs-lookup"><span data-stu-id="6be8c-152">`GrpcWebMode.GrpcWeb` configures content to be sent without encoding.</span></span> <span data-ttu-id="6be8c-153">Valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="6be8c-153">Default value.</span></span>
    * <span data-ttu-id="6be8c-154">`GrpcWebMode.GrpcWebText` configura el contenido para que esté codificado en Base64.</span><span class="sxs-lookup"><span data-stu-id="6be8c-154">`GrpcWebMode.GrpcWebText` configures content to be base64 encoded.</span></span> <span data-ttu-id="6be8c-155">Se requiere para las llamadas de transmisión por secuencias del servidor en los exploradores.</span><span class="sxs-lookup"><span data-stu-id="6be8c-155">Required for server streaming calls in browsers.</span></span>
* <span data-ttu-id="6be8c-156">**HttpVersion**: `Version` del protocolo http que se usa para establecer [HttpRequestMessage. version](xref:System.Net.Http.HttpRequestMessage.Version) en la solicitud gRPC http subyacente.</span><span class="sxs-lookup"><span data-stu-id="6be8c-156">**HttpVersion**: HTTP protocol `Version` used to set [HttpRequestMessage.Version](xref:System.Net.Http.HttpRequestMessage.Version) on the underlying gRPC HTTP request.</span></span> <span data-ttu-id="6be8c-157">gRPC-web no requiere una versión específica y no invalida el valor predeterminado a menos que se especifique.</span><span class="sxs-lookup"><span data-stu-id="6be8c-157">gRPC-Web doesn't require a specific version and doesn't override the default unless specified.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6be8c-158">Los clientes de gRPC generados tienen métodos de sincronización y asincrónicos para llamar a métodos unarios.</span><span class="sxs-lookup"><span data-stu-id="6be8c-158">Generated gRPC clients have sync and async methods for calling unary methods.</span></span> <span data-ttu-id="6be8c-159">Por ejemplo, `SayHello` es Sync y `SayHelloAsync` es Async.</span><span class="sxs-lookup"><span data-stu-id="6be8c-159">For example, `SayHello` is sync and `SayHelloAsync` is async.</span></span> <span data-ttu-id="6be8c-160">La llamada a un método de sincronización en una aplicación de webassembly más brillante hará que la aplicación deje de responder.</span><span class="sxs-lookup"><span data-stu-id="6be8c-160">Calling a sync method in a Blazor WebAssembly app will cause the app to become unresponsive.</span></span> <span data-ttu-id="6be8c-161">Los métodos asincrónicos se deben usar siempre en webassembly.</span><span class="sxs-lookup"><span data-stu-id="6be8c-161">Async methods must always be used in Blazor WebAssembly.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6be8c-162">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="6be8c-162">Additional resources</span></span>

* [<span data-ttu-id="6be8c-163">gRPC para el proyecto de GitHub de clientes Web</span><span class="sxs-lookup"><span data-stu-id="6be8c-163">gRPC for Web Clients GitHub project</span></span>](https://github.com/grpc/grpc-web)
* <xref:security/cors>
