---
title: Migrar de autenticación e identidad a ASP.NET Core 2.0
author: scottaddie
description: En este artículo se describe los pasos más comunes para migrar la autenticación de ASP.NET Core 1.x y la identidad a ASP.NET Core 2.0.
ms.author: scaddie
ms.date: 06/13/2019
uid: migration/1x-to-2x/identity-2x
ms.openlocfilehash: 3e8bc75b87a85159c9668b52eea32bb7d700be6c
ms.sourcegitcommit: 516f166c5f7cec54edf3d9c71e6e2ba53fb3b0e5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/18/2019
ms.locfileid: "67196379"
---
# <a name="migrate-authentication-and-identity-to-aspnet-core-20"></a><span data-ttu-id="0dbbd-103">Migrar de autenticación e identidad a ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="0dbbd-103">Migrate authentication and Identity to ASP.NET Core 2.0</span></span>

<span data-ttu-id="0dbbd-104">Por [Scott Addie](https://github.com/scottaddie) y [Hao Kung](https://github.com/HaoK)</span><span class="sxs-lookup"><span data-stu-id="0dbbd-104">By [Scott Addie](https://github.com/scottaddie) and [Hao Kung](https://github.com/HaoK)</span></span>

<span data-ttu-id="0dbbd-105">ASP.NET Core 2.0 tiene un nuevo modelo para la autenticación y [identidad](xref:security/authentication/identity) que simplifica la configuración mediante el uso de servicios.</span><span class="sxs-lookup"><span data-stu-id="0dbbd-105">ASP.NET Core 2.0 has a new model for authentication and [Identity](xref:security/authentication/identity) that simplifies configuration by using services.</span></span> <span data-ttu-id="0dbbd-106">Las aplicaciones de ASP.NET Core 1.x que utilizan la autenticación o Identity se pueden actualizar para usar el nuevo modelo, tal como se describe a continuación.</span><span class="sxs-lookup"><span data-stu-id="0dbbd-106">ASP.NET Core 1.x applications that use authentication or Identity can be updated to use the new model as outlined below.</span></span>

## <a name="update-namespaces"></a><span data-ttu-id="0dbbd-107">Actualice los espacios de nombres</span><span class="sxs-lookup"><span data-stu-id="0dbbd-107">Update namespaces</span></span>

<span data-ttu-id="0dbbd-108">En la versión 1.x, las clases como `IdentityRole` y `IdentityUser` se encontraron en el `Microsoft.AspNetCore.Identity.EntityFrameworkCore` espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="0dbbd-108">In 1.x, classes such `IdentityRole` and `IdentityUser` were found in the `Microsoft.AspNetCore.Identity.EntityFrameworkCore` namespace.</span></span>

<span data-ttu-id="0dbbd-109">En 2.0, la <xref:Microsoft.AspNetCore.Identity> espacio de nombres se convirtió en el nuevo lugar para varias de estas clases.</span><span class="sxs-lookup"><span data-stu-id="0dbbd-109">In 2.0, the <xref:Microsoft.AspNetCore.Identity> namespace became the new home for several of such classes.</span></span> <span data-ttu-id="0dbbd-110">Con el código de identidad de forma predeterminada, las clases afectados incluyen `ApplicationUser` y `Startup`.</span><span class="sxs-lookup"><span data-stu-id="0dbbd-110">With the default Identity code, affected classes include `ApplicationUser` and `Startup`.</span></span> <span data-ttu-id="0dbbd-111">Ajustar su `using` instrucciones para resolver las referencias afectadas.</span><span class="sxs-lookup"><span data-stu-id="0dbbd-111">Adjust your `using` statements to resolve the affected references.</span></span>

<a name="auth-middleware"></a>

## <a name="authentication-middleware-and-services"></a><span data-ttu-id="0dbbd-112">Middleware de autenticación y servicios</span><span class="sxs-lookup"><span data-stu-id="0dbbd-112">Authentication Middleware and services</span></span>

<span data-ttu-id="0dbbd-113">En los proyectos de 1.x, la autenticación se configura a través de middleware.</span><span class="sxs-lookup"><span data-stu-id="0dbbd-113">In 1.x projects, authentication is configured via middleware.</span></span> <span data-ttu-id="0dbbd-114">Se invoca un método de middleware para cada esquema de autenticación que desea admitir.</span><span class="sxs-lookup"><span data-stu-id="0dbbd-114">A middleware method is invoked for each authentication scheme you want to support.</span></span>

<span data-ttu-id="0dbbd-115">El siguiente ejemplo de 1.x configura la autenticación de Facebook con identidad en *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="0dbbd-115">The following 1.x example configures Facebook authentication with Identity in *Startup.cs*:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext>();
}

public void Configure(IApplicationBuilder app, ILoggerFactory loggerfactory)
{
    app.UseIdentity();
    app.UseFacebookAuthentication(new FacebookOptions {
        AppId = Configuration["auth:facebook:appid"],
        AppSecret = Configuration["auth:facebook:appsecret"]
    });
}
```

<span data-ttu-id="0dbbd-116">En los 2.0 proyectos, la autenticación se configura a través de servicios.</span><span class="sxs-lookup"><span data-stu-id="0dbbd-116">In 2.0 projects, authentication is configured via services.</span></span> <span data-ttu-id="0dbbd-117">Cada esquema de autenticación está registrado en el `ConfigureServices` método *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="0dbbd-117">Each authentication scheme is registered in the `ConfigureServices` method of *Startup.cs*.</span></span> <span data-ttu-id="0dbbd-118">El `UseIdentity` método se reemplaza por `UseAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="0dbbd-118">The `UseIdentity` method is replaced with `UseAuthentication`.</span></span>

<span data-ttu-id="0dbbd-119">El siguiente ejemplo 2.0 configura la autenticación de Facebook con identidad en *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="0dbbd-119">The following 2.0 example configures Facebook authentication with Identity in *Startup.cs*:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext>();

    // If you want to tweak Identity cookies, they're no longer part of IdentityOptions.
    services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
    services.AddAuthentication()
            .AddFacebook(options =>
            {
                options.AppId = Configuration["auth:facebook:appid"];
                options.AppSecret = Configuration["auth:facebook:appsecret"];
            });
}

public void Configure(IApplicationBuilder app, ILoggerFactory loggerfactory) {
    app.UseAuthentication();
}
```

<span data-ttu-id="0dbbd-120">El `UseAuthentication` método agrega un componente de middleware de autenticación único, que es responsable de la autenticación automática y la administración de las solicitudes de autenticación remota.</span><span class="sxs-lookup"><span data-stu-id="0dbbd-120">The `UseAuthentication` method adds a single authentication middleware component, which is responsible for automatic authentication and the handling of remote authentication requests.</span></span> <span data-ttu-id="0dbbd-121">Reemplaza todos los componentes de middleware individuales con un componente de middleware única y común.</span><span class="sxs-lookup"><span data-stu-id="0dbbd-121">It replaces all of the individual middleware components with a single, common middleware component.</span></span>

<span data-ttu-id="0dbbd-122">A continuación se muestran 2.0 instrucciones de migración para cada esquema de autenticación principales.</span><span class="sxs-lookup"><span data-stu-id="0dbbd-122">Below are 2.0 migration instructions for each major authentication scheme.</span></span>

### <a name="cookie-based-authentication"></a><span data-ttu-id="0dbbd-123">Autenticación basada en cookies</span><span class="sxs-lookup"><span data-stu-id="0dbbd-123">Cookie-based authentication</span></span>

<span data-ttu-id="0dbbd-124">Seleccione una de las dos opciones siguientes y realice los cambios necesarios en *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="0dbbd-124">Select one of the two options below, and make the necessary changes in *Startup.cs*:</span></span>

1. <span data-ttu-id="0dbbd-125">Uso de cookies con identidad</span><span class="sxs-lookup"><span data-stu-id="0dbbd-125">Use cookies with Identity</span></span>
    - <span data-ttu-id="0dbbd-126">Reemplace `UseIdentity` con `UseAuthentication` en el `Configure` método:</span><span class="sxs-lookup"><span data-stu-id="0dbbd-126">Replace `UseIdentity` with `UseAuthentication` in the `Configure` method:</span></span>

        ```csharp
        app.UseAuthentication();
        ```

    - <span data-ttu-id="0dbbd-127">Invocar el `AddIdentity` método en el `ConfigureServices` método para agregar los servicios de autenticación de cookies.</span><span class="sxs-lookup"><span data-stu-id="0dbbd-127">Invoke the `AddIdentity` method in the `ConfigureServices` method to add the cookie authentication services.</span></span>
    - <span data-ttu-id="0dbbd-128">Si lo desea, invocar el `ConfigureApplicationCookie` o `ConfigureExternalCookie` método en el `ConfigureServices` método ajustar la configuración de la cookie de identidad.</span><span class="sxs-lookup"><span data-stu-id="0dbbd-128">Optionally, invoke the `ConfigureApplicationCookie` or `ConfigureExternalCookie` method in the `ConfigureServices` method to tweak the Identity cookie settings.</span></span>

        ```csharp
        services.AddIdentity<ApplicationUser, IdentityRole>()
                .AddEntityFrameworkStores<ApplicationDbContext>()
                .AddDefaultTokenProviders();

        services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
        ```

2. <span data-ttu-id="0dbbd-129">Usar cookies sin identidad</span><span class="sxs-lookup"><span data-stu-id="0dbbd-129">Use cookies without Identity</span></span>
    - <span data-ttu-id="0dbbd-130">Reemplace el `UseCookieAuthentication` llame al método el `Configure` método con `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="0dbbd-130">Replace the `UseCookieAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

        ```csharp
        app.UseAuthentication();
        ```

    - <span data-ttu-id="0dbbd-131">Invocar el `AddAuthentication` y `AddCookie` métodos en el `ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="0dbbd-131">Invoke the `AddAuthentication` and `AddCookie` methods in the `ConfigureServices` method:</span></span>

        ```csharp
        // If you don't want the cookie to be automatically authenticated and assigned to HttpContext.User,
        // remove the CookieAuthenticationDefaults.AuthenticationScheme parameter passed to AddAuthentication.
        services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
                .AddCookie(options =>
                {
                    options.LoginPath = "/Account/LogIn";
                    options.LogoutPath = "/Account/LogOff";
                });
        ```

### <a name="jwt-bearer-authentication"></a><span data-ttu-id="0dbbd-132">Autenticación de portador JWT</span><span class="sxs-lookup"><span data-stu-id="0dbbd-132">JWT Bearer Authentication</span></span>

<span data-ttu-id="0dbbd-133">Realice los siguientes cambios en *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="0dbbd-133">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="0dbbd-134">Reemplace el `UseJwtBearerAuthentication` llame al método el `Configure` método con `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="0dbbd-134">Replace the `UseJwtBearerAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="0dbbd-135">Invocar el `AddJwtBearer` método en el `ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="0dbbd-135">Invoke the `AddJwtBearer` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
            .AddJwtBearer(options =>
            {
                options.Audience = "http://localhost:5001/";
                options.Authority = "http://localhost:5000/";
            });
    ```

    <span data-ttu-id="0dbbd-136">Este fragmento de código no usa la identidad, por lo que se debe establecer el esquema predeterminado pasando `JwtBearerDefaults.AuthenticationScheme` a la `AddAuthentication` método.</span><span class="sxs-lookup"><span data-stu-id="0dbbd-136">This code snippet doesn't use Identity, so the default scheme should be set by passing `JwtBearerDefaults.AuthenticationScheme` to the `AddAuthentication` method.</span></span>

### <a name="openid-connect-oidc-authentication"></a><span data-ttu-id="0dbbd-137">Autenticación de OpenID Connect (OIDC)</span><span class="sxs-lookup"><span data-stu-id="0dbbd-137">OpenID Connect (OIDC) authentication</span></span>

<span data-ttu-id="0dbbd-138">Realice los siguientes cambios en *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="0dbbd-138">Make the following changes in *Startup.cs*:</span></span>

- <span data-ttu-id="0dbbd-139">Reemplace el `UseOpenIdConnectAuthentication` llame al método el `Configure` método con `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="0dbbd-139">Replace the `UseOpenIdConnectAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="0dbbd-140">Invocar el `AddOpenIdConnect` método en el `ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="0dbbd-140">Invoke the `AddOpenIdConnect` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication(options =>
    {
        options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
        options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
    })
    .AddCookie()
    .AddOpenIdConnect(options =>
    {
        options.Authority = Configuration["auth:oidc:authority"];
        options.ClientId = Configuration["auth:oidc:clientid"];
    });
    ```

- <span data-ttu-id="0dbbd-141">Reemplace el `PostLogoutRedirectUri` propiedad en el `OpenIdConnectOptions` acción con `SignedOutRedirectUri`:</span><span class="sxs-lookup"><span data-stu-id="0dbbd-141">Replace the `PostLogoutRedirectUri` property in the `OpenIdConnectOptions` action with `SignedOutRedirectUri`:</span></span>

    ```csharp
    .AddOpenIdConnect(options =>
    {
        options.SignedOutRedirectUri = "https://contoso.com";
    });
    ```
    
### <a name="facebook-authentication"></a><span data-ttu-id="0dbbd-142">Autenticación con Facebook</span><span class="sxs-lookup"><span data-stu-id="0dbbd-142">Facebook authentication</span></span>

<span data-ttu-id="0dbbd-143">Realice los siguientes cambios en *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="0dbbd-143">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="0dbbd-144">Reemplace el `UseFacebookAuthentication` llame al método el `Configure` método con `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="0dbbd-144">Replace the `UseFacebookAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="0dbbd-145">Invocar el `AddFacebook` método en el `ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="0dbbd-145">Invoke the `AddFacebook` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddFacebook(options =>
            {
                options.AppId = Configuration["auth:facebook:appid"];
                options.AppSecret = Configuration["auth:facebook:appsecret"];
            });
    ```

### <a name="google-authentication"></a><span data-ttu-id="0dbbd-146">Autenticación con Google</span><span class="sxs-lookup"><span data-stu-id="0dbbd-146">Google authentication</span></span>

<span data-ttu-id="0dbbd-147">Realice los siguientes cambios en *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="0dbbd-147">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="0dbbd-148">Reemplace el `UseGoogleAuthentication` llame al método el `Configure` método con `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="0dbbd-148">Replace the `UseGoogleAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="0dbbd-149">Invocar el `AddGoogle` método en el `ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="0dbbd-149">Invoke the `AddGoogle` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddGoogle(options =>
            {
                options.ClientId = Configuration["auth:google:clientid"];
                options.ClientSecret = Configuration["auth:google:clientsecret"];
            });
    ```

### <a name="microsoft-account-authentication"></a><span data-ttu-id="0dbbd-150">Autenticación de Microsoft Account</span><span class="sxs-lookup"><span data-stu-id="0dbbd-150">Microsoft Account authentication</span></span>

<span data-ttu-id="0dbbd-151">Realice los siguientes cambios en *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="0dbbd-151">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="0dbbd-152">Reemplace el `UseMicrosoftAccountAuthentication` llame al método el `Configure` método con `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="0dbbd-152">Replace the `UseMicrosoftAccountAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="0dbbd-153">Invocar el `AddMicrosoftAccount` método en el `ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="0dbbd-153">Invoke the `AddMicrosoftAccount` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddMicrosoftAccount(options =>
            {
                options.ClientId = Configuration["auth:microsoft:clientid"];
                options.ClientSecret = Configuration["auth:microsoft:clientsecret"];
            });
    ```

### <a name="twitter-authentication"></a><span data-ttu-id="0dbbd-154">Autenticación con Twitter</span><span class="sxs-lookup"><span data-stu-id="0dbbd-154">Twitter authentication</span></span>

<span data-ttu-id="0dbbd-155">Realice los siguientes cambios en *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="0dbbd-155">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="0dbbd-156">Reemplace el `UseTwitterAuthentication` llame al método el `Configure` método con `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="0dbbd-156">Replace the `UseTwitterAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="0dbbd-157">Invocar el `AddTwitter` método en el `ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="0dbbd-157">Invoke the `AddTwitter` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddTwitter(options =>
            {
                options.ConsumerKey = Configuration["auth:twitter:consumerkey"];
                options.ConsumerSecret = Configuration["auth:twitter:consumersecret"];
            });
    ```

### <a name="setting-default-authentication-schemes"></a><span data-ttu-id="0dbbd-158">Esquemas de autenticación predeterminado de configuración</span><span class="sxs-lookup"><span data-stu-id="0dbbd-158">Setting default authentication schemes</span></span>

<span data-ttu-id="0dbbd-159">En la versión 1.x, el `AutomaticAuthenticate` y `AutomaticChallenge` propiedades de la [AuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1) clase base se pensada para establecerse en un esquema de autenticación único.</span><span class="sxs-lookup"><span data-stu-id="0dbbd-159">In 1.x, the `AutomaticAuthenticate` and `AutomaticChallenge` properties of the [AuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1) base class were intended to be set on a single authentication scheme.</span></span> <span data-ttu-id="0dbbd-160">No había una buena manera para exigir esto.</span><span class="sxs-lookup"><span data-stu-id="0dbbd-160">There was no good way to enforce this.</span></span>

<span data-ttu-id="0dbbd-161">Se han quitado en 2.0, estas dos propiedades como propiedades en cada `AuthenticationOptions` instancia.</span><span class="sxs-lookup"><span data-stu-id="0dbbd-161">In 2.0, these two properties have been removed as properties on the individual `AuthenticationOptions` instance.</span></span> <span data-ttu-id="0dbbd-162">Se pueden configurar en el `AddAuthentication` llamada al método dentro de la `ConfigureServices` método *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="0dbbd-162">They can be configured in the `AddAuthentication` method call within the `ConfigureServices` method of *Startup.cs*:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme);
```

<span data-ttu-id="0dbbd-163">En el fragmento de código anterior, el esquema predeterminado se establece en `CookieAuthenticationDefaults.AuthenticationScheme` ("Cookies").</span><span class="sxs-lookup"><span data-stu-id="0dbbd-163">In the preceding code snippet, the default scheme is set to `CookieAuthenticationDefaults.AuthenticationScheme` ("Cookies").</span></span>

<span data-ttu-id="0dbbd-164">Como alternativa, use una versión sobrecargada de la `AddAuthentication` método para establecer más de una propiedad.</span><span class="sxs-lookup"><span data-stu-id="0dbbd-164">Alternatively, use an overloaded version of the `AddAuthentication` method to set more than one property.</span></span> <span data-ttu-id="0dbbd-165">En el siguiente ejemplo de método sobrecargado, el esquema predeterminado se establece en `CookieAuthenticationDefaults.AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="0dbbd-165">In the following overloaded method example, the default scheme is set to `CookieAuthenticationDefaults.AuthenticationScheme`.</span></span> <span data-ttu-id="0dbbd-166">También se puede especificar el esquema de autenticación dentro de la persona `[Authorize]` atributos o las directivas de autorización.</span><span class="sxs-lookup"><span data-stu-id="0dbbd-166">The authentication scheme may alternatively be specified within your individual `[Authorize]` attributes or authorization policies.</span></span>

```csharp
services.AddAuthentication(options =>
{
    options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
});
```

<span data-ttu-id="0dbbd-167">Definir un esquema predeterminado de 2.0 si se cumple una de las condiciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="0dbbd-167">Define a default scheme in 2.0 if one of the following conditions is true:</span></span>
- <span data-ttu-id="0dbbd-168">Desea que el usuario para iniciar sesión automáticamente</span><span class="sxs-lookup"><span data-stu-id="0dbbd-168">You want the user to be automatically signed in</span></span>
- <span data-ttu-id="0dbbd-169">Usa el `[Authorize]` las directivas de autorización o el atributo sin especificar esquemas</span><span class="sxs-lookup"><span data-stu-id="0dbbd-169">You use the `[Authorize]` attribute or authorization policies without specifying schemes</span></span>

<span data-ttu-id="0dbbd-170">Una excepción a esta regla es la `AddIdentity` método.</span><span class="sxs-lookup"><span data-stu-id="0dbbd-170">An exception to this rule is the `AddIdentity` method.</span></span> <span data-ttu-id="0dbbd-171">Este método agrega las cookies para usted y establece el valor predeterminado autenticarse y Desafíe a esquemas para la cookie de aplicación `IdentityConstants.ApplicationScheme`.</span><span class="sxs-lookup"><span data-stu-id="0dbbd-171">This method adds cookies for you and sets the default authenticate and challenge schemes to the application cookie `IdentityConstants.ApplicationScheme`.</span></span> <span data-ttu-id="0dbbd-172">Además, Establece el esquema predeterminado de inicio de sesión a la cookie externa `IdentityConstants.ExternalScheme`.</span><span class="sxs-lookup"><span data-stu-id="0dbbd-172">Additionally, it sets the default sign-in scheme to the external cookie `IdentityConstants.ExternalScheme`.</span></span>

<a name="obsolete-interface"></a>

## <a name="use-httpcontext-authentication-extensions"></a><span data-ttu-id="0dbbd-173">Usar las extensiones de autenticación de HttpContext</span><span class="sxs-lookup"><span data-stu-id="0dbbd-173">Use HttpContext authentication extensions</span></span>

<span data-ttu-id="0dbbd-174">El `IAuthenticationManager` interfaz es el punto de entrada principal en el sistema de autenticación 1.x.</span><span class="sxs-lookup"><span data-stu-id="0dbbd-174">The `IAuthenticationManager` interface is the main entry point into the 1.x authentication system.</span></span> <span data-ttu-id="0dbbd-175">Se ha reemplazado por un nuevo conjunto de `HttpContext` métodos de extensión en el `Microsoft.AspNetCore.Authentication` espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="0dbbd-175">It has been replaced with a new set of `HttpContext` extension methods in the `Microsoft.AspNetCore.Authentication` namespace.</span></span>

<span data-ttu-id="0dbbd-176">Por ejemplo, los proyectos de 1.x referencia un `Authentication` propiedad:</span><span class="sxs-lookup"><span data-stu-id="0dbbd-176">For example, 1.x projects reference an `Authentication` property:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<span data-ttu-id="0dbbd-177">En los 2.0 proyectos, importe el `Microsoft.AspNetCore.Authentication` espacio de nombres y elimine el `Authentication` referencias de propiedad:</span><span class="sxs-lookup"><span data-stu-id="0dbbd-177">In 2.0 projects, import the `Microsoft.AspNetCore.Authentication` namespace, and delete the `Authentication` property references:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="windows-auth-changes"></a>

## <a name="windows-authentication-httpsys--iisintegration"></a><span data-ttu-id="0dbbd-178">Autenticación de Windows (HTTP.sys / IISIntegration)</span><span class="sxs-lookup"><span data-stu-id="0dbbd-178">Windows Authentication (HTTP.sys / IISIntegration)</span></span>

<span data-ttu-id="0dbbd-179">Hay dos variaciones de autenticación de Windows:</span><span class="sxs-lookup"><span data-stu-id="0dbbd-179">There are two variations of Windows authentication:</span></span>
1. <span data-ttu-id="0dbbd-180">El host sólo permite que los usuarios autenticados</span><span class="sxs-lookup"><span data-stu-id="0dbbd-180">The host only allows authenticated users</span></span>
2. <span data-ttu-id="0dbbd-181">El host permite tanto anónimos y los usuarios autenticados</span><span class="sxs-lookup"><span data-stu-id="0dbbd-181">The host allows both anonymous and authenticated users</span></span>

<span data-ttu-id="0dbbd-182">La primera variación que se ha descrito anteriormente se ve afectada por los cambios de 2.0.</span><span class="sxs-lookup"><span data-stu-id="0dbbd-182">The first variation described above is unaffected by the 2.0 changes.</span></span>

<span data-ttu-id="0dbbd-183">La segunda variación que se ha descrito anteriormente se ve afectada por los cambios de 2.0.</span><span class="sxs-lookup"><span data-stu-id="0dbbd-183">The second variation described above is affected by the 2.0 changes.</span></span> <span data-ttu-id="0dbbd-184">Por ejemplo, es posible que se lo que permite a los usuarios anónimos en su aplicación en IIS o [HTTP.sys](xref:fundamentals/servers/httpsys) pero autorizando a los usuarios en el nivel de controlador de capas.</span><span class="sxs-lookup"><span data-stu-id="0dbbd-184">For example, you may be allowing anonymous users into your app at the IIS or [HTTP.sys](xref:fundamentals/servers/httpsys) layer but authorizing users at the Controller level.</span></span> <span data-ttu-id="0dbbd-185">En este escenario, establezca el esquema predeterminado en `IISDefaults.AuthenticationScheme` en el `Startup.ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="0dbbd-185">In this scenario, set the default scheme to `IISDefaults.AuthenticationScheme` in the `Startup.ConfigureServices` method:</span></span>

```csharp
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

<span data-ttu-id="0dbbd-186">No se pudo establecer el esquema predeterminado impide que la solicitud authorize al desafío de trabajo.</span><span class="sxs-lookup"><span data-stu-id="0dbbd-186">Failure to set the default scheme prevents the authorize request to challenge from working.</span></span>

<a name="identity-cookie-options"></a>

## <a name="identitycookieoptions-instances"></a><span data-ttu-id="0dbbd-187">Instancias de IdentityCookieOptions</span><span class="sxs-lookup"><span data-stu-id="0dbbd-187">IdentityCookieOptions instances</span></span>

<span data-ttu-id="0dbbd-188">Un efecto secundario de los 2.0 cambios es el cambio a denominado opciones en lugar de las instancias de las opciones de cookie.</span><span class="sxs-lookup"><span data-stu-id="0dbbd-188">A side effect of the 2.0 changes is the switch to using named options instead of cookie options instances.</span></span> <span data-ttu-id="0dbbd-189">Se quita la capacidad para personalizar los nombres de esquema de cookie de identidad.</span><span class="sxs-lookup"><span data-stu-id="0dbbd-189">The ability to customize the Identity cookie scheme names is removed.</span></span>

<span data-ttu-id="0dbbd-190">Por ejemplo, use los proyectos de 1.x [inserción del constructor](xref:mvc/controllers/dependency-injection#constructor-injection) para pasar un `IdentityCookieOptions` parámetro en *AccountController.cs* y *ManageController.cs*.</span><span class="sxs-lookup"><span data-stu-id="0dbbd-190">For example, 1.x projects use [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) to pass an `IdentityCookieOptions` parameter into *AccountController.cs* and *ManageController.cs*.</span></span> <span data-ttu-id="0dbbd-191">El esquema de autenticación externo cookie se obtiene acceso desde la instancia proporcionada:</span><span class="sxs-lookup"><span data-stu-id="0dbbd-191">The external cookie authentication scheme is accessed from the provided instance:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor&highlight=4,11)]

<span data-ttu-id="0dbbd-192">La inyección de constructor mencionados anteriormente se convierte en innecesaria en los 2.0 proyectos y el `_externalCookieScheme` se puede eliminar el campo:</span><span class="sxs-lookup"><span data-stu-id="0dbbd-192">The aforementioned constructor injection becomes unnecessary in 2.0 projects, and the `_externalCookieScheme` field can be deleted:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor)]

<span data-ttu-id="0dbbd-193">los proyectos de 1.x usa el `_externalCookieScheme` campo como sigue:</span><span class="sxs-lookup"><span data-stu-id="0dbbd-193">1.x projects used the `_externalCookieScheme` field as follows:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<span data-ttu-id="0dbbd-194">En los 2.0 proyectos, reemplace el código anterior con el siguiente.</span><span class="sxs-lookup"><span data-stu-id="0dbbd-194">In 2.0 projects, replace the preceding code with the following.</span></span> <span data-ttu-id="0dbbd-195">El `IdentityConstants.ExternalScheme` constante se puede usar directamente.</span><span class="sxs-lookup"><span data-stu-id="0dbbd-195">The `IdentityConstants.ExternalScheme` constant can be used directly.</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<span data-ttu-id="0dbbd-196">Resolver recién agregada `SignOutAsync` llamar importando el espacio de nombres siguiente:</span><span class="sxs-lookup"><span data-stu-id="0dbbd-196">Resolve the newly added `SignOutAsync` call by importing the following namespace:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationImport)]

<a name="navigation-properties"></a>

## <a name="add-identityuser-poco-navigation-properties"></a><span data-ttu-id="0dbbd-197">Agregar propiedades de navegación IdentityUser POCO</span><span class="sxs-lookup"><span data-stu-id="0dbbd-197">Add IdentityUser POCO navigation properties</span></span>

<span data-ttu-id="0dbbd-198">Las propiedades de la base de navegación de Entity Framework (EF) Core `IdentityUser` POCO (objeto CRL estándar) se han quitado.</span><span class="sxs-lookup"><span data-stu-id="0dbbd-198">The Entity Framework (EF) Core navigation properties of the base `IdentityUser` POCO (Plain Old CLR Object) have been removed.</span></span> <span data-ttu-id="0dbbd-199">Si el proyecto de 1.x usa estas propiedades, agregarlos manualmente al proyecto 2.0:</span><span class="sxs-lookup"><span data-stu-id="0dbbd-199">If your 1.x project used these properties, manually add them back to the 2.0 project:</span></span>

```csharp
/// <summary>
/// Navigation property for the roles this user belongs to.
/// </summary>
public virtual ICollection<IdentityUserRole<int>> Roles { get; } = new List<IdentityUserRole<int>>();

/// <summary>
/// Navigation property for the claims this user possesses.
/// </summary>
public virtual ICollection<IdentityUserClaim<int>> Claims { get; } = new List<IdentityUserClaim<int>>();

/// <summary>
/// Navigation property for this users login accounts.
/// </summary>
public virtual ICollection<IdentityUserLogin<int>> Logins { get; } = new List<IdentityUserLogin<int>>();
```

<span data-ttu-id="0dbbd-200">Para evitar duplicadas claves externas al ejecutar migraciones de EF Core, agregue lo siguiente a su `IdentityDbContext` clase `OnModelCreating` método (después de que el `base.OnModelCreating();` llamar):</span><span class="sxs-lookup"><span data-stu-id="0dbbd-200">To prevent duplicate foreign keys when running EF Core Migrations, add the following to your `IdentityDbContext` class' `OnModelCreating` method (after the `base.OnModelCreating();` call):</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder builder)
{
    base.OnModelCreating(builder);
    // Customize the ASP.NET Core Identity model and override the defaults if needed.
    // For example, you can rename the ASP.NET Core Identity table names and more.
    // Add your customizations after calling base.OnModelCreating(builder);

    builder.Entity<ApplicationUser>()
        .HasMany(e => e.Claims)
        .WithOne()
        .HasForeignKey(e => e.UserId)
        .IsRequired()
        .OnDelete(DeleteBehavior.Cascade);

    builder.Entity<ApplicationUser>()
        .HasMany(e => e.Logins)
        .WithOne()
        .HasForeignKey(e => e.UserId)
        .IsRequired()
        .OnDelete(DeleteBehavior.Cascade);

    builder.Entity<ApplicationUser>()
        .HasMany(e => e.Roles)
        .WithOne()
        .HasForeignKey(e => e.UserId)
        .IsRequired()
        .OnDelete(DeleteBehavior.Cascade);
}
```

<a name="synchronous-method-removal"></a>

## <a name="replace-getexternalauthenticationschemes"></a><span data-ttu-id="0dbbd-201">Reemplace GetExternalAuthenticationSchemes</span><span class="sxs-lookup"><span data-stu-id="0dbbd-201">Replace GetExternalAuthenticationSchemes</span></span>

<span data-ttu-id="0dbbd-202">El método sincrónico `GetExternalAuthenticationSchemes` quitó una versión asincrónica.</span><span class="sxs-lookup"><span data-stu-id="0dbbd-202">The synchronous method `GetExternalAuthenticationSchemes` was removed in favor of an asynchronous version.</span></span> <span data-ttu-id="0dbbd-203">los proyectos de 1.x tienen el siguiente código *Controllers/ManageController.cs*:</span><span class="sxs-lookup"><span data-stu-id="0dbbd-203">1.x projects have the following code in *Controllers/ManageController.cs*:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemes)]

<span data-ttu-id="0dbbd-204">Este método aparece en *Views/Account/Login.cshtml* demasiado:</span><span class="sxs-lookup"><span data-stu-id="0dbbd-204">This method appears in *Views/Account/Login.cshtml* too:</span></span>

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Account/Login.cshtml?name=snippet_GetExtAuthNSchemes&highlight=2)]

<span data-ttu-id="0dbbd-205">En los 2.0 proyectos, utilice el <xref:Microsoft.AspNetCore.Identity.SignInManager`1.GetExternalAuthenticationSchemesAsync*> método.</span><span class="sxs-lookup"><span data-stu-id="0dbbd-205">In 2.0 projects, use the <xref:Microsoft.AspNetCore.Identity.SignInManager`1.GetExternalAuthenticationSchemesAsync*> method.</span></span> <span data-ttu-id="0dbbd-206">El cambio en *ManageController.cs* es similar al código siguiente:</span><span class="sxs-lookup"><span data-stu-id="0dbbd-206">The change in *ManageController.cs* resembles the following code:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemesAsync)]

<span data-ttu-id="0dbbd-207">En *Login.cshtml*, `AuthenticationScheme` propiedad accede en la `foreach` bucle cambia a `Name`:</span><span class="sxs-lookup"><span data-stu-id="0dbbd-207">In *Login.cshtml*, the `AuthenticationScheme` property accessed in the `foreach` loop changes to `Name`:</span></span>

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Views/Account/Login.cshtml?name=snippet_GetExtAuthNSchemesAsync&highlight=2,19)]

<a name="property-change"></a>

## <a name="manageloginsviewmodel-property-change"></a><span data-ttu-id="0dbbd-208">Cambio de propiedad ManageLoginsViewModel</span><span class="sxs-lookup"><span data-stu-id="0dbbd-208">ManageLoginsViewModel property change</span></span>

<span data-ttu-id="0dbbd-209">Un `ManageLoginsViewModel` objeto se usa en el `ManageLogins` acción de *ManageController.cs*.</span><span class="sxs-lookup"><span data-stu-id="0dbbd-209">A `ManageLoginsViewModel` object is used in the `ManageLogins` action of *ManageController.cs*.</span></span> <span data-ttu-id="0dbbd-210">En de los proyectos de 1.x, el objeto `OtherLogins` propiedad de valor devuelto es de tipo `IList<AuthenticationDescription>`.</span><span class="sxs-lookup"><span data-stu-id="0dbbd-210">In 1.x projects, the object's `OtherLogins` property return type is `IList<AuthenticationDescription>`.</span></span> <span data-ttu-id="0dbbd-211">Este tipo de valor devuelto requiere una importación de `Microsoft.AspNetCore.Http.Authentication`:</span><span class="sxs-lookup"><span data-stu-id="0dbbd-211">This return type requires an import of `Microsoft.AspNetCore.Http.Authentication`:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<span data-ttu-id="0dbbd-212">En los 2.0 proyectos, se cambia el tipo de valor devuelto a `IList<AuthenticationScheme>`.</span><span class="sxs-lookup"><span data-stu-id="0dbbd-212">In 2.0 projects, the return type changes to `IList<AuthenticationScheme>`.</span></span> <span data-ttu-id="0dbbd-213">Este nuevo tipo de valor devuelto es necesario sustituir la `Microsoft.AspNetCore.Http.Authentication` importar con un `Microsoft.AspNetCore.Authentication` importar.</span><span class="sxs-lookup"><span data-stu-id="0dbbd-213">This new return type requires replacing the `Microsoft.AspNetCore.Http.Authentication` import with a `Microsoft.AspNetCore.Authentication` import.</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<a name="additional-resources"></a>

## <a name="additional-resources"></a><span data-ttu-id="0dbbd-214">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="0dbbd-214">Additional resources</span></span>

<span data-ttu-id="0dbbd-215">Para obtener más información, consulte el [discusión Auth 2.0](https://github.com/aspnet/Security/issues/1338) problema en GitHub.</span><span class="sxs-lookup"><span data-stu-id="0dbbd-215">For more information, see the [Discussion for Auth 2.0](https://github.com/aspnet/Security/issues/1338) issue on GitHub.</span></span>
