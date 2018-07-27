---
title: Acceso a HttpContext en ASP.NET Core
author: coderandhiker
description: Obtenga información sobre cómo acceder a HttpContext en ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 07/20/2018
uid: fundamentals/httpcontext
ms.openlocfilehash: b1ff80943db1788b465accd51c70a3c3a3462d5c
ms.sourcegitcommit: a3675f9704e4e73ecc7cbbbf016a13d2a5c4d725
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2018
ms.locfileid: "39202715"
---
# <a name="access-httpcontext-in-aspnet-core"></a><span data-ttu-id="706b2-103">Acceso a HttpContext en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="706b2-103">Access HttpContext in ASP.NET Core</span></span>

<span data-ttu-id="706b2-104">Las aplicaciones ASP.NET Core acceden a `HttpContext` a través de la interfaz [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) y de su implementación predeterminada [HttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.httpcontextaccessor).</span><span class="sxs-lookup"><span data-stu-id="706b2-104">ASP.NET Core apps access the `HttpContext` through the [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) interface and its default implementation [HttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.httpcontextaccessor).</span></span>

::: moniker range=">= aspnetcore-2.0"

## <a name="use-httpcontext-from-razor-pages"></a><span data-ttu-id="706b2-105">Uso de HttpContext desde Razor Pages</span><span class="sxs-lookup"><span data-stu-id="706b2-105">Use HttpContext from Razor Pages</span></span>

<span data-ttu-id="706b2-106">[PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) de Razor Pages expone la propiedad [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext):</span><span class="sxs-lookup"><span data-stu-id="706b2-106">The Razor Pages [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) exposes the [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext) property:</span></span>

```csharp
public class AboutModel : PageModel
{
    public string Message { get; set; }

    public void OnGet()
    {
        Message = HttpContext.Request.PathBase;
    }
}
```

::: moniker-end

## <a name="use-httpcontext-from-a-controller"></a><span data-ttu-id="706b2-107">Uso de HttpContext desde un controlador</span><span class="sxs-lookup"><span data-stu-id="706b2-107">Use HttpContext from a controller</span></span>

<span data-ttu-id="706b2-108">Los controladores exponen la propiedad [ControllerBase.HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.httpcontext):</span><span class="sxs-lookup"><span data-stu-id="706b2-108">Controllers expose the [ControllerBase.HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.httpcontext) property:</span></span>

```csharp
public class HomeController : Controller
{
    public IActionResult About()
    {
        var pathBase = HttpContext.Request.PathBase;
        // Do something with the PathBase.

        return View();
    }
}
```

## <a name="use-httpcontext-from-middleware"></a><span data-ttu-id="706b2-109">Uso de HttpContext desde el software intermedio</span><span class="sxs-lookup"><span data-stu-id="706b2-109">Use HttpContext from middleware</span></span>

<span data-ttu-id="706b2-110">Cuando se trabaja con componentes de software intermedio personalizado, `HttpContext` se pasa al método `Invoke` o `InvokeAsync` y es accesible cuando el software intermedio está configurado:</span><span class="sxs-lookup"><span data-stu-id="706b2-110">When working with custom middleware components, `HttpContext` is passed into the `Invoke` or `InvokeAsync` method and can be accessed when the middleware is configured:</span></span>

```csharp
public class MyCustomMiddleware
{
    public Task InvokeAsync(HttpContext context)
    {
        // Middleware initialization optionally using HttpContext
    }
}
```

## <a name="use-httpcontext-from-custom-components"></a><span data-ttu-id="706b2-111">Uso de HttpContext desde componentes personalizados</span><span class="sxs-lookup"><span data-stu-id="706b2-111">Use HttpContext from custom components</span></span>

<span data-ttu-id="706b2-112">Para los componentes de otro marco y componentes personalizados que requieren acceso a `HttpContext`, el enfoque recomendado es registrar una dependencia mediante el contenedor integrado de [inserción de dependencias](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="706b2-112">For other framework and custom components that require access to `HttpContext`, the recommended approach is to register a dependency using the built-in [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="706b2-113">El contenedor de inserción de dependencias proporciona `IHttpContextAccessor` a cualquier clase que la declare como una dependencia en sus constructores.</span><span class="sxs-lookup"><span data-stu-id="706b2-113">The dependency injection container supplies the `IHttpContextAccessor` to any classes that declare it as a dependency in their constructors.</span></span>

::: moniker range=">= aspnetcore-2.1"

```csharp
public void ConfigureServices(IServiceCollection services)
{
     services.AddMvc()
         .SetCompatibilityVersion(CompatibilityVersion.Version_2_1);
     services.AddHttpContextAccessor();
     services.AddTransient<IUserRepository, UserRepository>();
}
```

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

```csharp
public void ConfigureServices(IServiceCollection services)
{
     services.AddMvc();
     services.AddSingleton<IHttpContextAccessor, HttpContextAccessor>();
     services.AddTransient<IUserRepository, UserRepository>();
}
```

::: moniker-end

<span data-ttu-id="706b2-114">En el ejemplo anterior:</span><span class="sxs-lookup"><span data-stu-id="706b2-114">In the preceding example:</span></span>

* <span data-ttu-id="706b2-115">`UserRepository` declara su dependencia de `IHttpContextAccessor`.</span><span class="sxs-lookup"><span data-stu-id="706b2-115">`UserRepository` declares its dependency on `IHttpContextAccessor`.</span></span>
* <span data-ttu-id="706b2-116">La dependencia se proporciona cuando la inserción de dependencias resuelve la cadena de dependencias y crea una instancia de `UserRepository`.</span><span class="sxs-lookup"><span data-stu-id="706b2-116">The dependency is supplied when dependency injection resolves the dependency chain and creates an instance of `UserRepository`.</span></span>

```csharp
public class UserRepository : IUserRepository
{
    private readonly IHttpContextAccessor _httpContextAccessor;

    public UserRepository(IHttpContextAccessor httpContextAccessor)
    {
        _httpContextAccessor = httpContextAccessor;
    }

    public void LogCurrentUser()
    {
        var username = _httpContextAccessor.HttpContext.User.Identity.Name;
        service.LogAccessRequest(username);
    }
}
```
