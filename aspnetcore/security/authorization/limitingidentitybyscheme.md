---
title: Autorizar con un esquema específico en ASP.NET Core
author: rick-anderson
description: En este artículo se explica cómo limitar la identidad de un esquema específico cuando se trabaja con varios métodos de autenticación.
ms.author: riande
ms.date: 10/22/2018
uid: security/authorization/limitingidentitybyscheme
ms.openlocfilehash: 778bb61f472ab2e76f85da5999d3c79238188f19
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/27/2019
ms.locfileid: "64897342"
---
# <a name="authorize-with-a-specific-scheme-in-aspnet-core"></a>Autorizar con un esquema específico en ASP.NET Core

En algunos escenarios, como aplicaciones de página única (SPA), es habitual usar varios métodos de autenticación. Por ejemplo, la aplicación puede usar la autenticación basada en cookies para iniciar sesión y autenticación de portador JWT para las solicitudes de JavaScript. En algunos casos, la aplicación puede tener varias instancias de un controlador de autenticación. Por ejemplo, dos controladores de la cookie donde uno contiene una identidad básica y el otro se crea cuando se ha desencadenado una autenticación multifactor (MFA). MFA se puede desencadenar porque el usuario solicitó una operación que requiere una seguridad adicional.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Un esquema de autenticación se llama cuando el servicio de autenticación se configura durante la autenticación. Por ejemplo:

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

En el código anterior, se han agregado dos controladores de autenticación: una para las cookies y otra de portador.

>[!NOTE]
>Especificar el esquema predeterminado de resultados en el `HttpContext.User` propiedad se establezca en esa identidad. Si no se desea ese comportamiento, puede deshabilitarlo mediante la invocación de la forma sin parámetros de `AddAuthentication`.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Se denominan esquemas de autenticación cuando el middleware de autenticación se configuran durante la autenticación. Por ejemplo:

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

En el código anterior, se han agregado dos middleware de autenticación: una para las cookies y otra de portador.

>[!NOTE]
>Especificar el esquema predeterminado de resultados en el `HttpContext.User` propiedad se establezca en esa identidad. Si no se desea ese comportamiento, puede deshabilitarla estableciendo el `AuthenticationOptions.AutomaticAuthenticate` propiedad `false`.

---

## <a name="selecting-the-scheme-with-the-authorize-attribute"></a>Seleccionar el esquema con el atributo Authorize

En el momento de la autorización, la aplicación indica el controlador que se usará. Seleccione el controlador con el que va a autorizar la aplicación pasando una lista delimitada por comas de esquemas de autenticación `[Authorize]`. El `[Authorize]` atributo especifica el esquema de autenticación o combinaciones que puede utilizar independientemente de si se ha configurado un valor predeterminado. Por ejemplo:

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

En el ejemplo anterior, los controladores de la cookie y el portador ejecutan y tienen la oportunidad de crear y agregar una identidad para el usuario actual. Al especificar un único esquema, se ejecuta el controlador correspondiente.

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

En el código anterior, se ejecuta solo el controlador con el esquema de "Bearer". Se omiten las identidades basada en cookies.

## <a name="selecting-the-scheme-with-policies"></a>Seleccionar la combinación de directivas

Si desea especificar los esquemas deseados en [directiva](xref:security/authorization/policies), puede establecer el `AuthenticationSchemes` al agregar la directiva de colección:

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

En el ejemplo anterior, la directiva de "Over18" solo se ejecuta con la identidad del creado por el controlador "Bearer". Usar la directiva estableciendo el `[Authorize]` del atributo `Policy` propiedad:

```csharp
[Authorize(Policy = "Over18")]
public class RegistrationController : Controller
```

::: moniker range=">= aspnetcore-2.0"

## <a name="use-multiple-authentication-schemes"></a>Usar varios esquemas de autenticación

Algunas aplicaciones que necesite admitir varios tipos de autenticación. Por ejemplo, la aplicación puede autenticar a los usuarios de Azure Active Directory y de una base de datos de los usuarios. Otro ejemplo es una aplicación que autentica a los usuarios de Active Directory Federation Services y Azure Active Directory B2C. En este caso, la aplicación debe aceptar un token de portador JWT desde varios emisores.

Agregue todos los esquemas de autenticación que desea Aceptar. Por ejemplo, el siguiente código en `Startup.ConfigureServices` agrega dos esquemas de autenticación de portador JWT con distintos emisores:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Code omitted for brevity

    services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
        .AddJwtBearer(options =>
        {
            options.Audience = "https://localhost:5000/";
            options.Authority = "https://localhost:5000/identity/";
        })
        .AddJwtBearer("AzureAD", options =>
        {
            options.Audience = "https://localhost:5000/";
            options.Authority = "https://login.microsoftonline.com/eb971100-6f99-4bdc-8611-1bc8edd7f436/";
        });
}
```

> [!NOTE]
> Solo una autenticación de portador JWT se registra con el esquema de autenticación predeterminado `JwtBearerDefaults.AuthenticationScheme`. Autenticación adicional que se tiene que estar registrada con un esquema de autenticación único.

El siguiente paso es actualizar la directiva de autorización de forma predeterminada para aceptar ambos esquemas de autenticación. Por ejemplo:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Code omitted for brevity

    services.AddAuthorization(options =>
    {
        var defaultAuthorizationPolicyBuilder = new AuthorizationPolicyBuilder(
            JwtBearerDefaults.AuthenticationScheme,
            "AzureAD");
        defaultAuthorizationPolicyBuilder = 
            defaultAuthorizationPolicyBuilder.RequireAuthenticatedUser();
        options.DefaultPolicy = defaultAuthorizationPolicyBuilder.Build();
    });
}
```

Como se invalida la directiva de autorización de forma predeterminada, es posible usar el `[Authorize]` atributo en los controladores. El controlador, a continuación, acepta las solicitudes con el token JWT emitido por el emisor de primer o segundo.

::: moniker-end
