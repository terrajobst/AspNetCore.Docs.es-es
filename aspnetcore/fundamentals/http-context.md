---
title: Acceso a HttpContext en ASP.NET Core
author: coderandhiker
description: Obtenga información sobre cómo acceder a HttpContext en ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 07/27/2018
uid: fundamentals/httpcontext
ms.openlocfilehash: 446882297524af3cbaed3ba7f941935debf5e7f4
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/27/2019
ms.locfileid: "56899208"
---
# <a name="access-httpcontext-in-aspnet-core"></a><span data-ttu-id="13188-103">Acceso a HttpContext en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="13188-103">Access HttpContext in ASP.NET Core</span></span>

<span data-ttu-id="13188-104">Las aplicaciones ASP.NET Core acceden a `HttpContext` a través de la interfaz [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) y de su implementación predeterminada [HttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.httpcontextaccessor).</span><span class="sxs-lookup"><span data-stu-id="13188-104">ASP.NET Core apps access the `HttpContext` through the [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) interface and its default implementation [HttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.httpcontextaccessor).</span></span> <span data-ttu-id="13188-105">Solo es necesario utilizar `IHttpContextAccessor` si necesita acceder al valor `HttpContext` dentro de un servicio.</span><span class="sxs-lookup"><span data-stu-id="13188-105">It's only necessary to use `IHttpContextAccessor` when you need access to the `HttpContext` inside a service.</span></span>

::: moniker range=">= aspnetcore-2.0"

## <a name="use-httpcontext-from-razor-pages"></a><span data-ttu-id="13188-106">Uso de HttpContext desde Razor Pages</span><span class="sxs-lookup"><span data-stu-id="13188-106">Use HttpContext from Razor Pages</span></span>

<span data-ttu-id="13188-107">[PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) de Razor Pages expone la propiedad [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext):</span><span class="sxs-lookup"><span data-stu-id="13188-107">The Razor Pages [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) exposes the [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext) property:</span></span>

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

## <a name="use-httpcontext-from-a-razor-view"></a><span data-ttu-id="13188-108">Uso de HttpContext desde una vista de Razor</span><span class="sxs-lookup"><span data-stu-id="13188-108">Use HttpContext from a Razor view</span></span>

<span data-ttu-id="13188-109">Las vistas de Razor exponen el valor `HttpContext` directamente mediante una propiedad [RazorPage.Context](/dotnet/api/microsoft.aspnetcore.mvc.razor.razorpage.context#Microsoft_AspNetCore_Mvc_Razor_RazorPage_Context) de la vista.</span><span class="sxs-lookup"><span data-stu-id="13188-109">Razor views expose the `HttpContext` directly via a [RazorPage.Context](/dotnet/api/microsoft.aspnetcore.mvc.razor.razorpage.context#Microsoft_AspNetCore_Mvc_Razor_RazorPage_Context) property on the view.</span></span> <span data-ttu-id="13188-110">En el ejemplo siguiente se recupera el nombre de usuario actual de una aplicación de Intranet mediante la Autenticación de Windows:</span><span class="sxs-lookup"><span data-stu-id="13188-110">The following example retrieves the current username in an Intranet app using Windows Authentication:</span></span>

```cshtml
@{
    var username = Context.User.Identity.Name;
}
```

## <a name="use-httpcontext-from-a-controller"></a><span data-ttu-id="13188-111">Uso de HttpContext desde un controlador</span><span class="sxs-lookup"><span data-stu-id="13188-111">Use HttpContext from a controller</span></span>

<span data-ttu-id="13188-112">Los controladores exponen la propiedad [ControllerBase.HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.httpcontext):</span><span class="sxs-lookup"><span data-stu-id="13188-112">Controllers expose the [ControllerBase.HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.httpcontext) property:</span></span>

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

## <a name="use-httpcontext-from-middleware"></a><span data-ttu-id="13188-113">Uso de HttpContext desde el software intermedio</span><span class="sxs-lookup"><span data-stu-id="13188-113">Use HttpContext from middleware</span></span>

<span data-ttu-id="13188-114">Cuando se trabaja con componentes de software intermedio personalizado, `HttpContext` se pasa al método `Invoke` o `InvokeAsync` y es accesible cuando el software intermedio está configurado:</span><span class="sxs-lookup"><span data-stu-id="13188-114">When working with custom middleware components, `HttpContext` is passed into the `Invoke` or `InvokeAsync` method and can be accessed when the middleware is configured:</span></span>

```csharp
public class MyCustomMiddleware
{
    public Task InvokeAsync(HttpContext context)
    {
        // Middleware initialization optionally using HttpContext
    }
}
```

## <a name="use-httpcontext-from-custom-components"></a><span data-ttu-id="13188-115">Uso de HttpContext desde componentes personalizados</span><span class="sxs-lookup"><span data-stu-id="13188-115">Use HttpContext from custom components</span></span>

<span data-ttu-id="13188-116">Para los componentes de otro marco y componentes personalizados que requieren acceso a `HttpContext`, el enfoque recomendado es registrar una dependencia mediante el contenedor integrado de [inserción de dependencias](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="13188-116">For other framework and custom components that require access to `HttpContext`, the recommended approach is to register a dependency using the built-in [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="13188-117">El contenedor de inserción de dependencias proporciona `IHttpContextAccessor` a cualquier clase que la declare como una dependencia en sus constructores.</span><span class="sxs-lookup"><span data-stu-id="13188-117">The dependency injection container supplies the `IHttpContextAccessor` to any classes that declare it as a dependency in their constructors.</span></span>

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

<span data-ttu-id="13188-118">En el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="13188-118">In the following example:</span></span>

* <span data-ttu-id="13188-119">`UserRepository` declara su dependencia de `IHttpContextAccessor`.</span><span class="sxs-lookup"><span data-stu-id="13188-119">`UserRepository` declares its dependency on `IHttpContextAccessor`.</span></span>
* <span data-ttu-id="13188-120">La dependencia se proporciona cuando la inserción de dependencias resuelve la cadena de dependencias y crea una instancia de `UserRepository`.</span><span class="sxs-lookup"><span data-stu-id="13188-120">The dependency is supplied when dependency injection resolves the dependency chain and creates an instance of `UserRepository`.</span></span>

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

## <a name="httpcontext-access-from-a-background-thread"></a><span data-ttu-id="13188-121">Acceso de HttpContext desde un subproceso en segundo plano</span><span class="sxs-lookup"><span data-stu-id="13188-121">HttpContext access from a background thread</span></span>

<span data-ttu-id="13188-122">`HttpContext` no es seguro para subprocesos.</span><span class="sxs-lookup"><span data-stu-id="13188-122">`HttpContext` is not thread-safe.</span></span> <span data-ttu-id="13188-123">La lectura o escritura de propiedades de `HttpContext` fuera del procesamiento de una solicitud puede iniciar una excepción `NullReferenceException`.</span><span class="sxs-lookup"><span data-stu-id="13188-123">Reading or writing properties of the `HttpContext` outside of processing a request can result in a `NullReferenceException`.</span></span>

> [!NOTE]
> <span data-ttu-id="13188-124">El uso de `HttpContext` fuera del procesamiento de una solicitud a menudo da como resultado una excepción `NullReferenceException`.</span><span class="sxs-lookup"><span data-stu-id="13188-124">Using `HttpContext` outside of processing a request often results in a `NullReferenceException`.</span></span> <span data-ttu-id="13188-125">Si la aplicación inicia excepciones `NullReferenceException` esporádicas, revise los elementos del código que inician el procesamiento en segundo plano, o bien que lo continúan después de que se complete una solicitud.</span><span class="sxs-lookup"><span data-stu-id="13188-125">If your app generates sporadic `NullReferenceException`s , review parts of the code that start background processing, or that continue processing after a request completes.</span></span> <span data-ttu-id="13188-126">Busque errores como la definición de un método de controlador como `async void`.</span><span class="sxs-lookup"><span data-stu-id="13188-126">Look for a mistakes like defining a controller method as `async void`.</span></span>

<span data-ttu-id="13188-127">Para realizar el trabajo en segundo plano de forma segura con datos `HttpContext`:</span><span class="sxs-lookup"><span data-stu-id="13188-127">To safely perform background work with `HttpContext` data:</span></span>

* <span data-ttu-id="13188-128">Copie los datos necesarios durante el procesamiento de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="13188-128">Copy the required data during request processing.</span></span>
* <span data-ttu-id="13188-129">Pase los datos copiados a una tarea en segundo plano.</span><span class="sxs-lookup"><span data-stu-id="13188-129">Pass the copied data to a background task.</span></span>

<span data-ttu-id="13188-130">Para evitar código no seguro, no pase nunca `HttpContext` a un método que realice trabajos en segundo plano; en su lugar, pase los datos que necesite.</span><span class="sxs-lookup"><span data-stu-id="13188-130">To avoid unsafe code, never pass the `HttpContext` into a method that does background work - pass the data you need instead.</span></span>

```csharp
public class EmailController
{
    public ActionResult SendEmail(string email)
    {
        var correlationId = HttpContext.Request.Headers["x-correlation-id"].ToString();

        // Starts sending an email, but doesn't wait for it to complete
        _ = SendEmailCore(correlationId);
        return View();
    }

    private async Task SendEmailCore(string correlationId)
    {
        // send the email
    }
}

