---
title: Activación de middleware basada en Factory en ASP.NET Core
author: guardrex
description: Aprenda a usar middleware fuertemente tipado con la implementación de una activación basada en Factory en ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/22/2019
uid: fundamentals/middleware/extensibility
ms.openlocfilehash: 17018d2dd20ed7b26bd0aa1095fa720a73f77261
ms.sourcegitcommit: d34b2627a69bc8940b76a949de830335db9701d3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/23/2019
ms.locfileid: "71186946"
---
# <a name="factory-based-middleware-activation-in-aspnet-core"></a><span data-ttu-id="71224-103">Activación de middleware basada en Factory en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="71224-103">Factory-based middleware activation in ASP.NET Core</span></span>

<span data-ttu-id="71224-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="71224-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="71224-105"><xref:Microsoft.AspNetCore.Http.IMiddlewareFactory>/<xref:Microsoft.AspNetCore.Http.IMiddleware> es un punto de extensibilidad para la activación de [middleware](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="71224-105"><xref:Microsoft.AspNetCore.Http.IMiddlewareFactory>/<xref:Microsoft.AspNetCore.Http.IMiddleware> is an extensibility point for [middleware](xref:fundamentals/middleware/index) activation.</span></span>

<span data-ttu-id="71224-106">Los métodos de extensión <xref:Microsoft.AspNetCore.Builder.UseMiddlewareExtensions.UseMiddleware*> comprueban si un tipo registrado de middleware implementa <xref:Microsoft.AspNetCore.Http.IMiddleware>.</span><span class="sxs-lookup"><span data-stu-id="71224-106"><xref:Microsoft.AspNetCore.Builder.UseMiddlewareExtensions.UseMiddleware*> extension methods check if a middleware's registered type implements <xref:Microsoft.AspNetCore.Http.IMiddleware>.</span></span> <span data-ttu-id="71224-107">Si es así, la instancia <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> registrada en el contenedor se usa para resolver la implementación <xref:Microsoft.AspNetCore.Http.IMiddleware> en lugar de usar la lógica de activación de middleware basado en convenciones.</span><span class="sxs-lookup"><span data-stu-id="71224-107">If it does, the <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> instance registered in the container is used to resolve the <xref:Microsoft.AspNetCore.Http.IMiddleware> implementation instead of using the convention-based middleware activation logic.</span></span> <span data-ttu-id="71224-108">El middleware se registra como un [servicio con ámbito o transitorio](xref:fundamentals/dependency-injection#service-lifetimes) en el contenedor de servicios de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="71224-108">The middleware is registered as a [scoped or transient service](xref:fundamentals/dependency-injection#service-lifetimes) in the app's service container.</span></span>

<span data-ttu-id="71224-109">Ventajas:</span><span class="sxs-lookup"><span data-stu-id="71224-109">Benefits:</span></span>

* <span data-ttu-id="71224-110">Activación por solicitud de cliente (inyección de servicios con ámbito)</span><span class="sxs-lookup"><span data-stu-id="71224-110">Activation per client request (injection of scoped services)</span></span>
* <span data-ttu-id="71224-111">Tipado fuerte de middleware</span><span class="sxs-lookup"><span data-stu-id="71224-111">Strong typing of middleware</span></span>

<span data-ttu-id="71224-112"><xref:Microsoft.AspNetCore.Http.IMiddleware> se activa por solicitud de cliente (conexión), por lo que los servicios con ámbito se pueden insertar en el constructor del middleware.</span><span class="sxs-lookup"><span data-stu-id="71224-112"><xref:Microsoft.AspNetCore.Http.IMiddleware> is activated per client request (connection), so scoped services can be injected into the middleware's constructor.</span></span>

<span data-ttu-id="71224-113">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="71224-113">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="imiddleware"></a><span data-ttu-id="71224-114">IMiddleware</span><span class="sxs-lookup"><span data-stu-id="71224-114">IMiddleware</span></span>

<span data-ttu-id="71224-115"><xref:Microsoft.AspNetCore.Http.IMiddleware> define el middleware para la canalización de solicitudes de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="71224-115"><xref:Microsoft.AspNetCore.Http.IMiddleware> defines middleware for the app's request pipeline.</span></span> <span data-ttu-id="71224-116">El método [InvokeAsync(HttpContext, RequestDelegate)](xref:Microsoft.AspNetCore.Http.IMiddleware.InvokeAsync*) controla las solicitudes y devuelve una <xref:System.Threading.Tasks.Task> que representa la ejecución del middleware.</span><span class="sxs-lookup"><span data-stu-id="71224-116">The [InvokeAsync(HttpContext, RequestDelegate)](xref:Microsoft.AspNetCore.Http.IMiddleware.InvokeAsync*) method handles requests and returns a <xref:System.Threading.Tasks.Task> that represents the execution of the middleware.</span></span>

<span data-ttu-id="71224-117">Middleware activado por convención:</span><span class="sxs-lookup"><span data-stu-id="71224-117">Middleware activated by convention:</span></span>

[!code-csharp[](extensibility/samples/3.x/MiddlewareExtensibilitySample/Middleware/ConventionalMiddleware.cs?name=snippet1)]

<span data-ttu-id="71224-118">Middleware activado por <xref:Microsoft.AspNetCore.Http.MiddlewareFactory>:</span><span class="sxs-lookup"><span data-stu-id="71224-118">Middleware activated by <xref:Microsoft.AspNetCore.Http.MiddlewareFactory>:</span></span>

[!code-csharp[](extensibility/samples/3.x/MiddlewareExtensibilitySample/Middleware/FactoryActivatedMiddleware.cs?name=snippet1)]

<span data-ttu-id="71224-119">Las extensiones se crean para los middlewares:</span><span class="sxs-lookup"><span data-stu-id="71224-119">Extensions are created for the middlewares:</span></span>

[!code-csharp[](extensibility/samples/3.x/MiddlewareExtensibilitySample/Middleware/MiddlewareExtensions.cs?name=snippet1)]

<span data-ttu-id="71224-120">No se pueden pasar objetos al middleware activado por Factory con <xref:Microsoft.AspNetCore.Builder.UseMiddlewareExtensions.UseMiddleware*>:</span><span class="sxs-lookup"><span data-stu-id="71224-120">It isn't possible to pass objects to the factory-activated middleware with <xref:Microsoft.AspNetCore.Builder.UseMiddlewareExtensions.UseMiddleware*>:</span></span>

```csharp
public static IApplicationBuilder UseFactoryActivatedMiddleware(
    this IApplicationBuilder builder, bool option)
{
    // Passing 'option' as an argument throws a NotSupportedException at runtime.
    return builder.UseMiddleware<FactoryActivatedMiddleware>(option);
}
```

<span data-ttu-id="71224-121">El middleware activado por Factory se agrega al contenedor integrado en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="71224-121">The factory-activated middleware is added to the built-in container in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](extensibility/samples/3.x/MiddlewareExtensibilitySample/Startup.cs?name=snippet1&highlight=6)]

<span data-ttu-id="71224-122">Ambos middlewares se registran en la canalización de procesamiento de solicitudes en `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="71224-122">Both middlewares are registered in the request processing pipeline in `Startup.Configure`:</span></span>

[!code-csharp[](extensibility/samples/3.x/MiddlewareExtensibilitySample/Startup.cs?name=snippet2&highlight=12-13)]

## <a name="imiddlewarefactory"></a><span data-ttu-id="71224-123">IMiddlewareFactory</span><span class="sxs-lookup"><span data-stu-id="71224-123">IMiddlewareFactory</span></span>

<span data-ttu-id="71224-124"><xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> proporciona métodos para crear middleware.</span><span class="sxs-lookup"><span data-stu-id="71224-124"><xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> provides methods to create middleware.</span></span> <span data-ttu-id="71224-125">La implementación de Middleware Factory se registra en el contenedor como un servicio con ámbito.</span><span class="sxs-lookup"><span data-stu-id="71224-125">The middleware factory implementation is registered in the container as a scoped service.</span></span>

<span data-ttu-id="71224-126">La implementación <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> predeterminada, <xref:Microsoft.AspNetCore.Http.MiddlewareFactory>, se encuentra en el paquete [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/).</span><span class="sxs-lookup"><span data-stu-id="71224-126">The default <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> implementation, <xref:Microsoft.AspNetCore.Http.MiddlewareFactory>, is found in the [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) package.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="71224-127"><xref:Microsoft.AspNetCore.Http.IMiddlewareFactory>/<xref:Microsoft.AspNetCore.Http.IMiddleware> es un punto de extensibilidad para la activación de [middleware](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="71224-127"><xref:Microsoft.AspNetCore.Http.IMiddlewareFactory>/<xref:Microsoft.AspNetCore.Http.IMiddleware> is an extensibility point for [middleware](xref:fundamentals/middleware/index) activation.</span></span>

<span data-ttu-id="71224-128">Los métodos de extensión <xref:Microsoft.AspNetCore.Builder.UseMiddlewareExtensions.UseMiddleware*> comprueban si un tipo registrado de middleware implementa <xref:Microsoft.AspNetCore.Http.IMiddleware>.</span><span class="sxs-lookup"><span data-stu-id="71224-128"><xref:Microsoft.AspNetCore.Builder.UseMiddlewareExtensions.UseMiddleware*> extension methods check if a middleware's registered type implements <xref:Microsoft.AspNetCore.Http.IMiddleware>.</span></span> <span data-ttu-id="71224-129">Si es así, la instancia <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> registrada en el contenedor se usa para resolver la implementación <xref:Microsoft.AspNetCore.Http.IMiddleware> en lugar de usar la lógica de activación de middleware basado en convenciones.</span><span class="sxs-lookup"><span data-stu-id="71224-129">If it does, the <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> instance registered in the container is used to resolve the <xref:Microsoft.AspNetCore.Http.IMiddleware> implementation instead of using the convention-based middleware activation logic.</span></span> <span data-ttu-id="71224-130">El middleware se registra como un [servicio con ámbito o transitorio](xref:fundamentals/dependency-injection#service-lifetimes) en el contenedor de servicios de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="71224-130">The middleware is registered as a [scoped or transient service](xref:fundamentals/dependency-injection#service-lifetimes) in the app's service container.</span></span>

<span data-ttu-id="71224-131">Ventajas:</span><span class="sxs-lookup"><span data-stu-id="71224-131">Benefits:</span></span>

* <span data-ttu-id="71224-132">Activación por solicitud de cliente (inyección de servicios con ámbito)</span><span class="sxs-lookup"><span data-stu-id="71224-132">Activation per client request (injection of scoped services)</span></span>
* <span data-ttu-id="71224-133">Tipado fuerte de middleware</span><span class="sxs-lookup"><span data-stu-id="71224-133">Strong typing of middleware</span></span>

<span data-ttu-id="71224-134"><xref:Microsoft.AspNetCore.Http.IMiddleware> se activa por solicitud de cliente (conexión), por lo que los servicios con ámbito se pueden insertar en el constructor del middleware.</span><span class="sxs-lookup"><span data-stu-id="71224-134"><xref:Microsoft.AspNetCore.Http.IMiddleware> is activated per client request (connection), so scoped services can be injected into the middleware's constructor.</span></span>

<span data-ttu-id="71224-135">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="71224-135">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="imiddleware"></a><span data-ttu-id="71224-136">IMiddleware</span><span class="sxs-lookup"><span data-stu-id="71224-136">IMiddleware</span></span>

<span data-ttu-id="71224-137"><xref:Microsoft.AspNetCore.Http.IMiddleware> define el middleware para la canalización de solicitudes de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="71224-137"><xref:Microsoft.AspNetCore.Http.IMiddleware> defines middleware for the app's request pipeline.</span></span> <span data-ttu-id="71224-138">El método [InvokeAsync(HttpContext, RequestDelegate)](xref:Microsoft.AspNetCore.Http.IMiddleware.InvokeAsync*) controla las solicitudes y devuelve una <xref:System.Threading.Tasks.Task> que representa la ejecución del middleware.</span><span class="sxs-lookup"><span data-stu-id="71224-138">The [InvokeAsync(HttpContext, RequestDelegate)](xref:Microsoft.AspNetCore.Http.IMiddleware.InvokeAsync*) method handles requests and returns a <xref:System.Threading.Tasks.Task> that represents the execution of the middleware.</span></span>

<span data-ttu-id="71224-139">Middleware activado por convención:</span><span class="sxs-lookup"><span data-stu-id="71224-139">Middleware activated by convention:</span></span>

[!code-csharp[](extensibility/samples/2.x/MiddlewareExtensibilitySample/Middleware/ConventionalMiddleware.cs?name=snippet1)]

<span data-ttu-id="71224-140">Middleware activado por <xref:Microsoft.AspNetCore.Http.MiddlewareFactory>:</span><span class="sxs-lookup"><span data-stu-id="71224-140">Middleware activated by <xref:Microsoft.AspNetCore.Http.MiddlewareFactory>:</span></span>

[!code-csharp[](extensibility/samples/2.x/MiddlewareExtensibilitySample/Middleware/FactoryActivatedMiddleware.cs?name=snippet1)]

<span data-ttu-id="71224-141">Las extensiones se crean para los middlewares:</span><span class="sxs-lookup"><span data-stu-id="71224-141">Extensions are created for the middlewares:</span></span>

[!code-csharp[](extensibility/samples/2.x/MiddlewareExtensibilitySample/Middleware/MiddlewareExtensions.cs?name=snippet1)]

<span data-ttu-id="71224-142">No se pueden pasar objetos al middleware activado por Factory con <xref:Microsoft.AspNetCore.Builder.UseMiddlewareExtensions.UseMiddleware*>:</span><span class="sxs-lookup"><span data-stu-id="71224-142">It isn't possible to pass objects to the factory-activated middleware with <xref:Microsoft.AspNetCore.Builder.UseMiddlewareExtensions.UseMiddleware*>:</span></span>

```csharp
public static IApplicationBuilder UseFactoryActivatedMiddleware(
    this IApplicationBuilder builder, bool option)
{
    // Passing 'option' as an argument throws a NotSupportedException at runtime.
    return builder.UseMiddleware<FactoryActivatedMiddleware>(option);
}
```

<span data-ttu-id="71224-143">El middleware activado por Factory se agrega al contenedor integrado en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="71224-143">The factory-activated middleware is added to the built-in container in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](extensibility/samples/2.x/MiddlewareExtensibilitySample/Startup.cs?name=snippet1&highlight=6)]

<span data-ttu-id="71224-144">Ambos middlewares se registran en la canalización de procesamiento de solicitudes en `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="71224-144">Both middlewares are registered in the request processing pipeline in `Startup.Configure`:</span></span>

[!code-csharp[](extensibility/samples/2.x/MiddlewareExtensibilitySample/Startup.cs?name=snippet2&highlight=13-14)]

## <a name="imiddlewarefactory"></a><span data-ttu-id="71224-145">IMiddlewareFactory</span><span class="sxs-lookup"><span data-stu-id="71224-145">IMiddlewareFactory</span></span>

<span data-ttu-id="71224-146"><xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> proporciona métodos para crear middleware.</span><span class="sxs-lookup"><span data-stu-id="71224-146"><xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> provides methods to create middleware.</span></span> <span data-ttu-id="71224-147">La implementación de Middleware Factory se registra en el contenedor como un servicio con ámbito.</span><span class="sxs-lookup"><span data-stu-id="71224-147">The middleware factory implementation is registered in the container as a scoped service.</span></span>

<span data-ttu-id="71224-148">La implementación <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> predeterminada, <xref:Microsoft.AspNetCore.Http.MiddlewareFactory>, se encuentra en el paquete [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/).</span><span class="sxs-lookup"><span data-stu-id="71224-148">The default <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> implementation, <xref:Microsoft.AspNetCore.Http.MiddlewareFactory>, is found in the [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) package.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="71224-149">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="71224-149">Additional resources</span></span>

* <xref:fundamentals/middleware/index>
* <xref:fundamentals/middleware/extensibility-third-party-container>
