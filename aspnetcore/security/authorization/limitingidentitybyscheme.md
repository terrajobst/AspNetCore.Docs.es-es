---
title: "Autorizar a un esquema específico - ASP.NET Core"
author: rick-anderson
description: "Este artículo explica cómo limitar la identidad a un esquema específico cuando se trabaja con varios métodos de autenticación."
ms.author: riande
manager: wpickett
ms.date: 10/12/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/limitingidentitybyscheme
ms.openlocfilehash: 099dba1a4235ef62ea298748645b99e2d6d12d44
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2018
---
# <a name="authorize-with-a-specific-scheme"></a>Autorizar con un esquema específico

En algunos escenarios, como aplicaciones de una única página (SPAs), es habitual usar varios métodos de autenticación. Por ejemplo, la aplicación puede usar la autenticación basada en cookies para iniciar sesión y autenticación de portador JWT para las solicitudes de JavaScript. En algunos casos, la aplicación puede tener varias instancias de un controlador de autenticación. Por ejemplo, dos controladores de la cookie donde uno contiene una identidad básica y el otro se crea cuando se ha desencadenado una autenticación multifactor (MFA). MFA se puede desencadenar porque el usuario solicitó una operación que requiere seguridad adicional.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Un esquema de autenticación se denomina cuando se configura el servicio de autenticación durante la autenticación. Por ejemplo:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Code omitted for brevity

    services.AddAuthentication()
        .AddCookie(options => {
            options.LoginPath = "/Account/Unauthorized/";
            options.AccessDeniedPath = "/Account/Forbidden/";
        })
        .AddJwtBearer(options => {
            options.Audience = "http://localhost:5001/";
            options.Authority = "http://localhost:5000/";
        });
```

En el código anterior, se han agregado dos controladores de autenticación: uno para las cookies y otro para portador.

>[!NOTE]
>Especificar el esquema predeterminado de resultados en el `HttpContext.User` propiedad que se establece para esa identidad. Si no se desea este comportamiento, puede deshabilitarlo mediante la invocación de la forma sin parámetros de `AddAuthentication`.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Se denominan esquemas de autenticación cuando middlewares de autenticación se configuran durante la autenticación. Por ejemplo:

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
{
    // Code omitted for brevity

    app.UseCookieAuthentication(new CookieAuthenticationOptions()
    {
        AuthenticationScheme = "Cookie",
        LoginPath = "/Account/Unauthorized/",
        AccessDeniedPath = "/Account/Forbidden/",
        AutomaticAuthenticate = false
    });
    
    app.UseJwtBearerAuthentication(new JwtBearerOptions()
    {
        AuthenticationScheme = "Bearer",
        AutomaticAuthenticate = false,
        Audience = "http://localhost:5001/",
        Authority = "http://localhost:5000/",
        RequireHttpsMetadata = false
    });
```

En el código anterior, se han agregado dos middlewares de autenticación: uno para las cookies y otro para portador.

>[!NOTE]
>Especificar el esquema predeterminado de resultados en el `HttpContext.User` propiedad que se establece para esa identidad. Si no se desea este comportamiento, puede deshabilitarla estableciendo la `AuthenticationOptions.AutomaticAuthenticate` propiedad `false`.

---

## <a name="selecting-the-scheme-with-the-authorize-attribute"></a>Seleccionar el esquema con el atributo Authorize

En el momento de la autorización, la aplicación indica el controlador que se usará. Seleccione el controlador con el que se autorizará la aplicación pasando una lista delimitada por comas de esquemas de autenticación `[Authorize]`. El `[Authorize]` atributo especifica el esquema de autenticación o los esquemas para usar independientemente de si se configura un valor predeterminado. Por ejemplo:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
[Authorize(AuthenticationSchemes = AuthSchemes)]
public class MixedController : Controller
    // Requires the following imports:
    // using Microsoft.AspNetCore.Authentication.Cookies;
    // using Microsoft.AspNetCore.Authentication.JwtBearer;
    private const string AuthSchemes =
        CookieAuthenticationDefaults.AuthenticationScheme + "," +
        JwtBearerDefaults.AuthenticationScheme;
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
[Authorize(ActiveAuthenticationSchemes = AuthSchemes)]
public class MixedController : Controller
    // Requires the following imports:
    // using Microsoft.AspNetCore.Authentication.Cookies;
    // using Microsoft.AspNetCore.Authentication.JwtBearer;
    private const string AuthSchemes =
        CookieAuthenticationDefaults.AuthenticationScheme + "," +
        JwtBearerDefaults.AuthenticationScheme;
```

---

En el ejemplo anterior, los controladores de la cookie y el portador ejecutan y tienen una oportunidad para crear y agregar una identidad para el usuario actual. Mediante la especificación de un único esquema, se ejecuta el controlador correspondiente.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
[Authorize(AuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
[Authorize(ActiveAuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

---

En el código anterior, se ejecuta sólo el controlador con el esquema "Portador". Se omite cualquier identidad basada en cookies.

## <a name="selecting-the-scheme-with-policies"></a>Seleccionar el esquema con directivas

Si desea especificar los esquemas deseados en [directiva](xref:security/authorization/policies), puede establecer el `AuthenticationSchemes` colección cuando se agrega la directiva:

```csharp
services.AddAuthorization(options =>
{
    options.AddPolicy("Over18", policy =>
    {
        policy.AuthenticationSchemes.Add(JwtBearerDefaults.AuthenticationScheme);
        policy.RequireAuthenticatedUser();
        policy.Requirements.Add(new MinimumAgeRequirement());
    });
});
```

En el ejemplo anterior, la directiva de "Over18" solo se ejecuta con la identidad creada por el controlador "Portador". Usar la directiva estableciendo la `[Authorize]` del atributo `Policy` propiedad:

```csharp
[Authorize(Policy = "Over18")]
public class RegistrationController : Controller
```
