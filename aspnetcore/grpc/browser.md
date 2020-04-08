---
title: Uso de gRPC en aplicaciones de explorador
author: jamesnk
description: Obtenga información sobre cómo configurar servicios gRPC en ASP.NET Core a los que se puede llamar desde aplicaciones del explorador usando gRPC-Web.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 02/16/2020
uid: grpc/browser
ms.openlocfilehash: 3beeffc26ffd3c2dc85bfc22a46d97d5fd78d3d0
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "78649421"
---
# <a name="use-grpc-in-browser-apps"></a><span data-ttu-id="7c575-103">Uso de gRPC en aplicaciones de explorador</span><span class="sxs-lookup"><span data-stu-id="7c575-103">Use gRPC in browser apps</span></span>

<span data-ttu-id="7c575-104">Por [James Newton-King](https://twitter.com/jamesnk)</span><span class="sxs-lookup"><span data-stu-id="7c575-104">By [James Newton-King](https://twitter.com/jamesnk)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7c575-105">**La compatibilidad con gRPC-Web en .NET es experimental.**</span><span class="sxs-lookup"><span data-stu-id="7c575-105">**gRPC-Web support in .NET is experimental**</span></span>
>
> <span data-ttu-id="7c575-106">gRPC-Web para .NET es un proyecto experimental, no un producto confirmado.</span><span class="sxs-lookup"><span data-stu-id="7c575-106">gRPC-Web for .NET is an experimental project, not a committed product.</span></span> <span data-ttu-id="7c575-107">Queremos:</span><span class="sxs-lookup"><span data-stu-id="7c575-107">We want to:</span></span>
>
> * <span data-ttu-id="7c575-108">Comprobar que nuestro método para implementar gRPC-Web funciona</span><span class="sxs-lookup"><span data-stu-id="7c575-108">Test that our approach to implementing gRPC-Web works.</span></span>
> * <span data-ttu-id="7c575-109">Obtener comentarios sobre si este método es útil para los desarrolladores de .NET en comparación con la manera tradicional de configurar gRPC-Web a través de un proxy</span><span class="sxs-lookup"><span data-stu-id="7c575-109">Get feedback on if this approach is useful to .NET developers compared to the traditional way of setting up gRPC-Web via a proxy.</span></span>
>
> <span data-ttu-id="7c575-110">Deje sus comentarios en [https://github.com/grpc/grpc-dotnet](https://github.com/grpc/grpc-dotnet) para asegurarnos de que creamos algo que gusta a los desarrolladores y los hace productivos.</span><span class="sxs-lookup"><span data-stu-id="7c575-110">Please leave feedback at [https://github.com/grpc/grpc-dotnet](https://github.com/grpc/grpc-dotnet) to ensure we build something that developers like and are productive with.</span></span>

<span data-ttu-id="7c575-111">No se puede llamar a un servicio HTTP/2 gRPC desde una aplicación basada en el explorador.</span><span class="sxs-lookup"><span data-stu-id="7c575-111">It is not possible to call a HTTP/2 gRPC service from a browser-based app.</span></span> <span data-ttu-id="7c575-112">[gRPC-Web](https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-WEB.md) es un protocolo que permite a las aplicaciones de explorador de JavaScript y Blazor llamar a servicios gRPC.</span><span class="sxs-lookup"><span data-stu-id="7c575-112">[gRPC-Web](https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-WEB.md) is a protocol that allows browser JavaScript and Blazor apps to call gRPC services.</span></span> <span data-ttu-id="7c575-113">En este artículo se explica cómo usar gRPC-Web en .NET Core.</span><span class="sxs-lookup"><span data-stu-id="7c575-113">This article explains how to use gRPC-Web in .NET Core.</span></span>

## <a name="configure-grpc-web-in-aspnet-core"></a><span data-ttu-id="7c575-114">Configuración de gRPC-Web en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7c575-114">Configure gRPC-Web in ASP.NET Core</span></span>

<span data-ttu-id="7c575-115">Los servicios gRPC hospedados en ASP.NET Core se pueden configurar para admitir gRPC-Web junto con HTTP/2 gRPC.</span><span class="sxs-lookup"><span data-stu-id="7c575-115">gRPC services hosted in ASP.NET Core can be configured to support gRPC-Web alongside HTTP/2 gRPC.</span></span> <span data-ttu-id="7c575-116">gRPC-Web no requiere ningún cambio en los servicios.</span><span class="sxs-lookup"><span data-stu-id="7c575-116">gRPC-Web does not require any changes to services.</span></span> <span data-ttu-id="7c575-117">La única modificación es la configuración de inicio.</span><span class="sxs-lookup"><span data-stu-id="7c575-117">The only modification is startup configuration.</span></span>

<span data-ttu-id="7c575-118">Para habilitar gRPC-Web con un servicio gRPC de ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="7c575-118">To enable gRPC-Web with an ASP.NET Core gRPC service:</span></span>

* <span data-ttu-id="7c575-119">Agregue una referencia al paquete [Grpc.AspNetCore.Web](https://www.nuget.org/packages/Grpc.AspNetCore.Web).</span><span class="sxs-lookup"><span data-stu-id="7c575-119">Add a reference to the [Grpc.AspNetCore.Web](https://www.nuget.org/packages/Grpc.AspNetCore.Web) package.</span></span>
* <span data-ttu-id="7c575-120">Configure la aplicación para que use gRPC-Web, agregando para ello `AddGrpcWeb` y `UseGrpcWeb` a *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="7c575-120">Configure the app to use gRPC-Web by adding `AddGrpcWeb` and `UseGrpcWeb` to *Startup.cs*:</span></span>

[!code-csharp[](~/grpc/browser/sample/Startup.cs?name=snippet_1&highlight=10,14)]

<span data-ttu-id="7c575-121">El código anterior:</span><span class="sxs-lookup"><span data-stu-id="7c575-121">The preceding code:</span></span>

* <span data-ttu-id="7c575-122">Agrega el middleware de gRPC-Web, `UseGrpcWeb`, después del enrutamiento y antes de los puntos de conexión.</span><span class="sxs-lookup"><span data-stu-id="7c575-122">Adds the gRPC-Web middleware, `UseGrpcWeb`, after routing and before endpoints.</span></span>
* <span data-ttu-id="7c575-123">Especifica que el método `endpoints.MapGrpcService<GreeterService>()` admite gRPC-Web con `EnableGrpcWeb`.</span><span class="sxs-lookup"><span data-stu-id="7c575-123">Specifies the `endpoints.MapGrpcService<GreeterService>()` method supports gRPC-Web with `EnableGrpcWeb`.</span></span> 

<span data-ttu-id="7c575-124">Opcionalmente, configure todos los servicios para que admitan gRPC-Web agregando `services.AddGrpcWeb(o => o.GrpcWebEnabled = true);` a ConfigureServices.</span><span class="sxs-lookup"><span data-stu-id="7c575-124">Alternatively, configure all services to support gRPC-Web by adding `services.AddGrpcWeb(o => o.GrpcWebEnabled = true);` to ConfigureServices.</span></span>

[!code-csharp[](~/grpc/browser/sample/AllServicesSupportExample_Startup.cs?name=snippet_1&highlight=6,13)]

### <a name="grpc-web-and-cors"></a><span data-ttu-id="7c575-125">gRPC-Web y CORS</span><span class="sxs-lookup"><span data-stu-id="7c575-125">gRPC-Web and CORS</span></span>

<span data-ttu-id="7c575-126">La seguridad del explorador evita que una página web realice solicitudes a un dominio diferente del que atendió a dicha página web.</span><span class="sxs-lookup"><span data-stu-id="7c575-126">Browser security prevents a web page from making requests to a different domain than the one that served the web page.</span></span> <span data-ttu-id="7c575-127">Esta restricción se aplica a la hora de realizar llamadas de gRPC-Web con aplicaciones de explorador.</span><span class="sxs-lookup"><span data-stu-id="7c575-127">This restriction applies to making gRPC-Web calls with browser apps.</span></span> <span data-ttu-id="7c575-128">Por ejemplo, una aplicación de explorador atendida por `https://www.contoso.com` no puede llamar a los servicios gRPC-Web hospedados en `https://services.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="7c575-128">For example, a browser app served by `https://www.contoso.com` is blocked from calling gRPC-Web services hosted on `https://services.contoso.com`.</span></span> <span data-ttu-id="7c575-129">Para suavizar esta restricción, podemos recurrir al uso compartido de recursos entre orígenes (CORS).</span><span class="sxs-lookup"><span data-stu-id="7c575-129">Cross Origin Resource Sharing (CORS) can be used to relax this restriction.</span></span>

<span data-ttu-id="7c575-130">Para permitir que la aplicación de explorador haga llamadas de gRPC-Web entre orígenes, configure [CORS en ASP.NET Core](xref:security/cors).</span><span class="sxs-lookup"><span data-stu-id="7c575-130">To allow your browser app to make cross-origin gRPC-Web calls, set up [CORS in ASP.NET Core](xref:security/cors).</span></span> <span data-ttu-id="7c575-131">Use la compatibilidad de CORS integrada y exponga los encabezados específicos de gRPC con <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>.</span><span class="sxs-lookup"><span data-stu-id="7c575-131">Use the built-in CORS support, and expose gRPC-specific headers with <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>.</span></span>

[!code-csharp[](~/grpc/browser/sample/CORS_Startup.cs?name=snippet_1&highlight=5-11,19,24)]

<span data-ttu-id="7c575-132">El código anterior:</span><span class="sxs-lookup"><span data-stu-id="7c575-132">The preceding code:</span></span>

* <span data-ttu-id="7c575-133">Llama a `AddCors` para agregar los servicios CORS y configura una directiva CORS que expone los encabezados específicos de gRPC.</span><span class="sxs-lookup"><span data-stu-id="7c575-133">Calls `AddCors` to add CORS services and configures a CORS policy that exposes gRPC-specific headers.</span></span>
* <span data-ttu-id="7c575-134">Llama a `UseCors` para agregar el middleware de CORS después del enrutamiento y antes de los puntos de conexión.</span><span class="sxs-lookup"><span data-stu-id="7c575-134">Calls `UseCors` to add the CORS middleware after routing and before endpoints.</span></span>
* <span data-ttu-id="7c575-135">Especifica que el método `endpoints.MapGrpcService<GreeterService>()` admite CORS con `RequiresCors`.</span><span class="sxs-lookup"><span data-stu-id="7c575-135">Specifies the `endpoints.MapGrpcService<GreeterService>()` method supports CORS with `RequiresCors`.</span></span>

## <a name="call-grpc-web-from-the-browser"></a><span data-ttu-id="7c575-136">Llamada a gRPC-Web desde el explorador</span><span class="sxs-lookup"><span data-stu-id="7c575-136">Call gRPC-Web from the browser</span></span>

<span data-ttu-id="7c575-137">Las aplicaciones de explorador pueden usar gRPC-Web para llamar a servicios gRPC.</span><span class="sxs-lookup"><span data-stu-id="7c575-137">Browser apps can use gRPC-Web to call gRPC services.</span></span> <span data-ttu-id="7c575-138">Existen algunos requisitos y limitaciones a la hora de llamar a servicios gRPC con gRPC-Web desde el explorador:</span><span class="sxs-lookup"><span data-stu-id="7c575-138">There are some requirements and limitations when calling gRPC services with gRPC-Web from the browser:</span></span>

* <span data-ttu-id="7c575-139">El servidor debe estar configurado para admitir gRPC-Web.</span><span class="sxs-lookup"><span data-stu-id="7c575-139">The server must have been configured to support gRPC-Web.</span></span>
* <span data-ttu-id="7c575-140">No se pueden usar llamadas de streaming de cliente ni de streaming bidireccional.</span><span class="sxs-lookup"><span data-stu-id="7c575-140">Client streaming and bidirectional streaming calls aren't supported.</span></span> <span data-ttu-id="7c575-141">Sí se puede usar el streaming de servidor.</span><span class="sxs-lookup"><span data-stu-id="7c575-141">Server streaming is supported.</span></span>
* <span data-ttu-id="7c575-142">Para llamar a servicios gRPC en otro dominio, hay que configurar [CORS](xref:security/cors) en el servidor.</span><span class="sxs-lookup"><span data-stu-id="7c575-142">Calling gRPC services on a different domain requires [CORS](xref:security/cors) to be configured on the server.</span></span>

### <a name="javascript-grpc-web-client"></a><span data-ttu-id="7c575-143">Cliente gRPC-Web de JavaScript</span><span class="sxs-lookup"><span data-stu-id="7c575-143">JavaScript gRPC-Web client</span></span>

<span data-ttu-id="7c575-144">Existe un cliente gRPC-Web de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="7c575-144">There is a JavaScript gRPC-Web client.</span></span> <span data-ttu-id="7c575-145">Para obtener instrucciones sobre cómo usar gRPC-Web en JavaScript, vea [Escribir código de cliente de JavaScript con gRPC-Web](https://github.com/grpc/grpc-web/tree/master/net/grpc/gateway/examples/helloworld#write-client-code).</span><span class="sxs-lookup"><span data-stu-id="7c575-145">For instructions on how to use gRPC-Web from JavaScript, see [write JavaScript client code with gRPC-Web](https://github.com/grpc/grpc-web/tree/master/net/grpc/gateway/examples/helloworld#write-client-code).</span></span>

### <a name="configure-grpc-web-with-the-net-grpc-client"></a><span data-ttu-id="7c575-146">Configuración de gRPC-Web con el cliente gRPC de .NET</span><span class="sxs-lookup"><span data-stu-id="7c575-146">Configure gRPC-Web with the .NET gRPC client</span></span>

<span data-ttu-id="7c575-147">El cliente gRPC de .NET se puede configurar para realizar llamadas de gRPC-Web.</span><span class="sxs-lookup"><span data-stu-id="7c575-147">The .NET gRPC client can be configured to make gRPC-Web calls.</span></span> <span data-ttu-id="7c575-148">Esto resulta útil en las aplicaciones de [WebAssembly de Blazor](xref:blazor/index#blazor-webassembly), que se hospedan en el explorador y presentan las mismas limitaciones de HTTP que el código JavaScript.</span><span class="sxs-lookup"><span data-stu-id="7c575-148">This is useful for [Blazor WebAssembly](xref:blazor/index#blazor-webassembly) apps, which are hosted in the browser and have the same HTTP limitations of JavaScript code.</span></span> <span data-ttu-id="7c575-149">Llamar a gRPC-Web con un cliente .NET es [igual que HTTP/2 gRPC](xref:grpc/client).</span><span class="sxs-lookup"><span data-stu-id="7c575-149">Calling gRPC-Web with a .NET client is [the same as HTTP/2 gRPC](xref:grpc/client).</span></span> <span data-ttu-id="7c575-150">La única modificación es cómo se crea el canal.</span><span class="sxs-lookup"><span data-stu-id="7c575-150">The only modification is how the channel is created.</span></span>

<span data-ttu-id="7c575-151">Para usar gRPC-Web:</span><span class="sxs-lookup"><span data-stu-id="7c575-151">To use gRPC-Web:</span></span>

* <span data-ttu-id="7c575-152">Agregue una referencia al paquete [Grpc.Net.Client.Web](https://www.nuget.org/packages/Grpc.Net.Client.Web).</span><span class="sxs-lookup"><span data-stu-id="7c575-152">Add a reference to the [Grpc.Net.Client.Web](https://www.nuget.org/packages/Grpc.Net.Client.Web) package.</span></span>
* <span data-ttu-id="7c575-153">Asegúrese de que la referencia al paquete [Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client) es 2.27.0 o mayor.</span><span class="sxs-lookup"><span data-stu-id="7c575-153">Ensure the reference to [Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client) package is 2.27.0 or greater.</span></span>
* <span data-ttu-id="7c575-154">Configure el canal para que use `GrpcWebHandler`:</span><span class="sxs-lookup"><span data-stu-id="7c575-154">Configure the channel to use the `GrpcWebHandler`:</span></span>

[!code-csharp[](~/grpc/browser/sample/Handler.cs?name=snippet_1)]

<span data-ttu-id="7c575-155">El código anterior:</span><span class="sxs-lookup"><span data-stu-id="7c575-155">The preceding code:</span></span>

* <span data-ttu-id="7c575-156">Configura un canal para que use gRPC-Web.</span><span class="sxs-lookup"><span data-stu-id="7c575-156">Configures a channel to use gRPC-Web.</span></span>
* <span data-ttu-id="7c575-157">Crea un cliente y realiza una llamada usando el canal.</span><span class="sxs-lookup"><span data-stu-id="7c575-157">Creates a client and makes a call using the channel.</span></span>

<span data-ttu-id="7c575-158">`GrpcWebHandler` presenta las siguientes opciones de configuración cuando se crea:</span><span class="sxs-lookup"><span data-stu-id="7c575-158">The `GrpcWebHandler` has the following configuration options when created:</span></span>

* <span data-ttu-id="7c575-159">**InnerHandler**: elemento <xref:System.Net.Http.HttpMessageHandler> subyacente que realiza la solicitud HTTP de gRPC, por ejemplo, `HttpClientHandler`.</span><span class="sxs-lookup"><span data-stu-id="7c575-159">**InnerHandler**: The underlying <xref:System.Net.Http.HttpMessageHandler> that makes the gRPC HTTP request, for example, `HttpClientHandler`.</span></span>
* <span data-ttu-id="7c575-160">**Modo**: tipo de enumeración que especifica si el elemento `Content-Type` de la solicitud HTTP de gRPC es `application/grpc-web` o `application/grpc-web-text`.</span><span class="sxs-lookup"><span data-stu-id="7c575-160">**Mode**: An enumeration type that specifies whether the gRPC HTTP request request `Content-Type` is `application/grpc-web` or `application/grpc-web-text`.</span></span>
    * <span data-ttu-id="7c575-161">`GrpcWebMode.GrpcWeb` configura el contenido que se va a enviar sin codificación.</span><span class="sxs-lookup"><span data-stu-id="7c575-161">`GrpcWebMode.GrpcWeb` configures content to be sent without encoding.</span></span> <span data-ttu-id="7c575-162">Valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="7c575-162">Default value.</span></span>
    * <span data-ttu-id="7c575-163">`GrpcWebMode.GrpcWebText` configura el contenido para que tenga codificación Base64.</span><span class="sxs-lookup"><span data-stu-id="7c575-163">`GrpcWebMode.GrpcWebText` configures content to be base64 encoded.</span></span> <span data-ttu-id="7c575-164">Esto es necesario en las llamadas de streaming de servidor en los exploradores.</span><span class="sxs-lookup"><span data-stu-id="7c575-164">Required for server streaming calls in browsers.</span></span>
* <span data-ttu-id="7c575-165">**HttpVersion**: elemento `Version` del protocolo HTTP que se usa para establecer [HttpRequestMessage.Version](xref:System.Net.Http.HttpRequestMessage.Version) en la solicitud HTTP de gRPC subyacente.</span><span class="sxs-lookup"><span data-stu-id="7c575-165">**HttpVersion**: HTTP protocol `Version` used to set [HttpRequestMessage.Version](xref:System.Net.Http.HttpRequestMessage.Version) on the underlying gRPC HTTP request.</span></span> <span data-ttu-id="7c575-166">gRPC-Web no requiere que haya una versión específica y no invalida el valor predeterminado a menos que así se especifique.</span><span class="sxs-lookup"><span data-stu-id="7c575-166">gRPC-Web doesn't require a specific version and doesn't override the default unless specified.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7c575-167">Los clientes de gRPC generados tienen métodos sincrónicos y asincrónicos para llamar a métodos unarios.</span><span class="sxs-lookup"><span data-stu-id="7c575-167">Generated gRPC clients have sync and async methods for calling unary methods.</span></span> <span data-ttu-id="7c575-168">Así, `SayHello` es sincrónico y `SayHelloAsync`, asincrónico.</span><span class="sxs-lookup"><span data-stu-id="7c575-168">For example, `SayHello` is sync and `SayHelloAsync` is async.</span></span> <span data-ttu-id="7c575-169">La llamada a un método sincrónico en una aplicación de WebAssembly de Blazor hará que la aplicación deje de responder.</span><span class="sxs-lookup"><span data-stu-id="7c575-169">Calling a sync method in a Blazor WebAssembly app will cause the app to become unresponsive.</span></span> <span data-ttu-id="7c575-170">En WebAssembly de Blazor siempre se deben usar métodos asincrónicos.</span><span class="sxs-lookup"><span data-stu-id="7c575-170">Async methods must always be used in Blazor WebAssembly.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7c575-171">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="7c575-171">Additional resources</span></span>

* [<span data-ttu-id="7c575-172">Proyecto de GitHub de gRPC para clientes web</span><span class="sxs-lookup"><span data-stu-id="7c575-172">gRPC for Web Clients GitHub project</span></span>](https://github.com/grpc/grpc-web)
* <xref:security/cors>
