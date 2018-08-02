---
title: Acceso a HttpContext en ASP.NET Core
author: coderandhiker
description: Obtenga información sobre cómo acceder a HttpContext en ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 07/27/2018
uid: fundamentals/httpcontext
ms.openlocfilehash: ee185cd30af51fa6ee9a4d23ea60a56ec1b76c8d
ms.sourcegitcommit: 506a199274e9fe5fb4070b273ba94f29f14cb619
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/28/2018
ms.locfileid: "39332293"
---
# <a name="access-httpcontext-in-aspnet-core"></a>Acceso a HttpContext en ASP.NET Core

Las aplicaciones ASP.NET Core acceden a `HttpContext` a través de la interfaz [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) y de su implementación predeterminada [HttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.httpcontextaccessor). Solo es necesario utilizar `IHttpContextAccessor` si necesita acceder al valor `HttpContext` dentro de un servicio.

::: moniker range=">= aspnetcore-2.0"

## <a name="use-httpcontext-from-razor-pages"></a>Uso de HttpContext desde Razor Pages

[PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) de Razor Pages expone la propiedad [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext):

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

## <a name="use-httpcontext-from-a-razor-view"></a>Uso de HttpContext desde una vista de Razor

Las vistas de Razor exponen el valor `HttpContext` directamente mediante una propiedad [RazorPage.Context](/dotnet/api/microsoft.aspnetcore.mvc.razor.razorpage.context#Microsoft_AspNetCore_Mvc_Razor_RazorPage_Context) de la vista. En el ejemplo siguiente se recupera el nombre de usuario actual de una aplicación de Intranet mediante la Autenticación de Windows:

```cshtml
@{
    var username = Context.User.Identity.Name;
}
```

## <a name="use-httpcontext-from-a-controller"></a>Uso de HttpContext desde un controlador

Los controladores exponen la propiedad [ControllerBase.HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.httpcontext):

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

## <a name="use-httpcontext-from-middleware"></a>Uso de HttpContext desde el software intermedio

Cuando se trabaja con componentes de software intermedio personalizado, `HttpContext` se pasa al método `Invoke` o `InvokeAsync` y es accesible cuando el software intermedio está configurado:

```csharp
public class MyCustomMiddleware
{
    public Task InvokeAsync(HttpContext context)
    {
        // Middleware initialization optionally using HttpContext
    }
}
```

## <a name="use-httpcontext-from-custom-components"></a>Uso de HttpContext desde componentes personalizados

Para los componentes de otro marco y componentes personalizados que requieren acceso a `HttpContext`, el enfoque recomendado es registrar una dependencia mediante el contenedor integrado de [inserción de dependencias](xref:fundamentals/dependency-injection). El contenedor de inserción de dependencias proporciona `IHttpContextAccessor` a cualquier clase que la declare como una dependencia en sus constructores.

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

En el ejemplo anterior:

* `UserRepository` declara su dependencia de `IHttpContextAccessor`.
* La dependencia se proporciona cuando la inserción de dependencias resuelve la cadena de dependencias y crea una instancia de `UserRepository`.

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
