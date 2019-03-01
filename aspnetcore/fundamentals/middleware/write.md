---
title: Escritura de middleware de ASP.NET Core personalizado
author: rick-anderson
description: Obtenga información sobre cómo escribir middleware de ASP.NET Core personalizado.
ms.author: riande
ms.custom: mvc
ms.date: 02/14/2019
uid: fundamentals/middleware/write
ms.openlocfilehash: 2c5577394a10370d92c8a83f9d806b63f3245c8b
ms.sourcegitcommit: b3894b65e313570e97a2ab78b8addd22f427cac8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/23/2019
ms.locfileid: "56744923"
---
# <a name="write-custom-aspnet-core-middleware"></a><span data-ttu-id="10247-103">Escritura de middleware de ASP.NET Core personalizado</span><span class="sxs-lookup"><span data-stu-id="10247-103">Write custom ASP.NET Core middleware</span></span>

<span data-ttu-id="10247-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="10247-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="10247-105">El software intermedio es un software que se ensambla en una canalización de una aplicación para controlar las solicitudes y las respuestas.</span><span class="sxs-lookup"><span data-stu-id="10247-105">Middleware is software that's assembled into an app pipeline to handle requests and responses.</span></span> <span data-ttu-id="10247-106">ASP.NET Core proporciona un completo conjunto de componentes de middleware integrados, pero en algunos escenarios es posible que quiera escribir middleware personalizado.</span><span class="sxs-lookup"><span data-stu-id="10247-106">ASP.NET Core provides a rich set of built-in middleware components, but in some scenarios you might want to write a custom middleware.</span></span>

## <a name="middleware-class"></a><span data-ttu-id="10247-107">Clase de middleware</span><span class="sxs-lookup"><span data-stu-id="10247-107">Middleware class</span></span>

<span data-ttu-id="10247-108">El middleware normalmente está encapsulado en una clase y se expone con un método de extensión.</span><span class="sxs-lookup"><span data-stu-id="10247-108">Middleware is generally encapsulated in a class and exposed with an extension method.</span></span> <span data-ttu-id="10247-109">Use el siguiente software intermedio a modo de ejemplo. En este se establece la referencia cultural de la solicitud actual a partir de la cadena de solicitud:</span><span class="sxs-lookup"><span data-stu-id="10247-109">Consider the following middleware, which sets the culture for the current request from a query string:</span></span>

[!code-csharp[](index/snapshot/Culture/StartupCulture.cs?name=snippet1)]

<span data-ttu-id="10247-110">El código de ejemplo anterior se usa para mostrar la creación de un componente de software intermedio.</span><span class="sxs-lookup"><span data-stu-id="10247-110">The preceding sample code is used to demonstrate creating a middleware component.</span></span> <span data-ttu-id="10247-111">Para obtener más información sobre la compatibilidad con la localización integrada de ASP.NET Core, vea <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="10247-111">For ASP.NET Core's built-in localization support, see <xref:fundamentals/localization>.</span></span>

<span data-ttu-id="10247-112">Puede probar el middleware si pasa la referencia cultural.</span><span class="sxs-lookup"><span data-stu-id="10247-112">You can test the middleware by passing in the culture.</span></span> <span data-ttu-id="10247-113">Por ejemplo: `http://localhost:7997/?culture=no`.</span><span class="sxs-lookup"><span data-stu-id="10247-113">For example, `http://localhost:7997/?culture=no`.</span></span>

<span data-ttu-id="10247-114">El código siguiente mueve el delegado de middleware a una clase:</span><span class="sxs-lookup"><span data-stu-id="10247-114">The following code moves the middleware delegate to a class:</span></span>

[!code-csharp[](index/snapshot/Culture/RequestCultureMiddleware.cs)]

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="10247-115">El nombre del software intermedio del método `Task` debe ser `Invoke`.</span><span class="sxs-lookup"><span data-stu-id="10247-115">The middleware `Task` method's name must be `Invoke`.</span></span> <span data-ttu-id="10247-116">En ASP.NET Core 2.0 o posterior, el nombre puede ser `Invoke` o `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="10247-116">In ASP.NET Core 2.0 or later, the name can be either `Invoke` or `InvokeAsync`.</span></span>

::: moniker-end

## <a name="middleware-extension-method"></a><span data-ttu-id="10247-117">Método de extensión de middleware</span><span class="sxs-lookup"><span data-stu-id="10247-117">Middleware extension method</span></span>

<span data-ttu-id="10247-118">El método de extensión siguiente expone el software intermedio mediante <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder>:</span><span class="sxs-lookup"><span data-stu-id="10247-118">The following extension method exposes the middleware through <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder>:</span></span>

[!code-csharp[](index/snapshot/Culture/RequestCultureMiddlewareExtensions.cs)]

<span data-ttu-id="10247-119">El código siguiente llama al middleware desde `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="10247-119">The following code calls the middleware from `Startup.Configure`:</span></span>

[!code-csharp[](index/snapshot/Culture/Startup.cs?name=snippet1&highlight=5)]

<span data-ttu-id="10247-120">El middleware debería seguir el [principio de dependencias explicitas](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies) mediante la exposición de sus dependencias en el constructor.</span><span class="sxs-lookup"><span data-stu-id="10247-120">Middleware should follow the [Explicit Dependencies Principle](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies) by exposing its dependencies in its constructor.</span></span> <span data-ttu-id="10247-121">El middleware se construye una vez por *duración de la aplicación*.</span><span class="sxs-lookup"><span data-stu-id="10247-121">Middleware is constructed once per *application lifetime*.</span></span> <span data-ttu-id="10247-122">Si necesita compartir servicios con software intermedio en una solicitud, vea la sección [Dependencias bajo solicitud](#per-request-dependencies).</span><span class="sxs-lookup"><span data-stu-id="10247-122">See the [Per-request dependencies](#per-request-dependencies) section if you need to share services with middleware within a request.</span></span>

<span data-ttu-id="10247-123">Los componentes de software intermedio pueden resolver sus dependencias de una [inserción de dependencias (DI)](xref:fundamentals/dependency-injection) mediante parámetros del constructor.</span><span class="sxs-lookup"><span data-stu-id="10247-123">Middleware components can resolve their dependencies from [dependency injection (DI)](xref:fundamentals/dependency-injection) through constructor parameters.</span></span> <span data-ttu-id="10247-124">[UseMiddleware&lt;T&gt;](/dotnet/api/microsoft.aspnetcore.builder.usemiddlewareextensions.usemiddleware#Microsoft_AspNetCore_Builder_UseMiddlewareExtensions_UseMiddleware_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Type_System_Object___) también puede aceptar parámetros adicionales directamente.</span><span class="sxs-lookup"><span data-stu-id="10247-124">[UseMiddleware&lt;T&gt;](/dotnet/api/microsoft.aspnetcore.builder.usemiddlewareextensions.usemiddleware#Microsoft_AspNetCore_Builder_UseMiddlewareExtensions_UseMiddleware_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Type_System_Object___) can also accept additional parameters directly.</span></span>

## <a name="per-request-dependencies"></a><span data-ttu-id="10247-125">Dependencias bajo solicitud</span><span class="sxs-lookup"><span data-stu-id="10247-125">Per-request dependencies</span></span>

<span data-ttu-id="10247-126">Dado que el software intermedio se construye al inicio de la aplicación y no bajo solicitud, los servicios de duración *con ámbito* que usan los constructores de software intermedio no se comparten con otros tipos insertados mediante dependencias durante cada solicitud.</span><span class="sxs-lookup"><span data-stu-id="10247-126">Because middleware is constructed at app startup, not per-request, *scoped* lifetime services used by middleware constructors aren't shared with other dependency-injected types during each request.</span></span> <span data-ttu-id="10247-127">Si debe compartir un servicio *con ámbito* entre su middleware y otros tipos, agregue esos servicios a la signatura del método `Invoke`.</span><span class="sxs-lookup"><span data-stu-id="10247-127">If you must share a *scoped* service between your middleware and other types, add these services to the `Invoke` method's signature.</span></span> <span data-ttu-id="10247-128">El método `Invoke` puede aceptar parámetros adicionales que la inserción de dependencias propaga:</span><span class="sxs-lookup"><span data-stu-id="10247-128">The `Invoke` method can accept additional parameters that are populated by DI:</span></span>

```csharp
public class CustomMiddleware
{
    private readonly RequestDelegate _next;

    public CustomMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    // IMyScopedService is injected into Invoke
    public async Task Invoke(HttpContext httpContext, IMyScopedService svc)
    {
        svc.MyProperty = 1000;
        await _next(httpContext);
    }
}
```

## <a name="additional-resources"></a><span data-ttu-id="10247-129">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="10247-129">Additional resources</span></span>

* <xref:fundamentals/middleware/index>
* <xref:migration/http-modules>
* <xref:fundamentals/startup>
* <xref:fundamentals/request-features>
* <xref:fundamentals/middleware/extensibility>
* <xref:fundamentals/middleware/extensibility-third-party-container>
