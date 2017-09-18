---
title: "Migrar de autenticación e identidad al núcleo de ASP.NET 2.0"
author: scottaddie
description: "En este artículo se describe los pasos más comunes para migrar ASP.NET Core 1.x autenticación e identidad principal de ASP.NET 2.0."
keywords: "Autenticación de ASP.NET Core, identidad,"
ms.author: scaddie
manager: wpickett
ms.date: 08/02/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/1x-to-2x/identity-2x
ms.openlocfilehash: b4e67e7cfea3c01e3ca8c0d5df2a04e789749932
ms.sourcegitcommit: f8f6b5934bd071a349f5bc1e389365c52b1c00fa
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/14/2017
---
# <a name="migrating-authentication-and-identity-to-aspnet-core-20"></a><span data-ttu-id="21f44-104">Migrar de autenticación e identidad al núcleo de ASP.NET 2.0</span><span class="sxs-lookup"><span data-stu-id="21f44-104">Migrating Authentication and Identity to ASP.NET Core 2.0</span></span>

<span data-ttu-id="21f44-105">Por [Scott Addie](https://github.com/scottaddie) y [Hao Kung](https://github.com/HaoK)</span><span class="sxs-lookup"><span data-stu-id="21f44-105">By [Scott Addie](https://github.com/scottaddie) and [Hao Kung](https://github.com/HaoK)</span></span>

<span data-ttu-id="21f44-106">Núcleo de ASP.NET 2.0 tiene un nuevo modelo para la autenticación y [identidad](xref:security/authentication/identity) que simplifica la configuración mediante el uso de servicios.</span><span class="sxs-lookup"><span data-stu-id="21f44-106">ASP.NET Core 2.0 has a new model for authentication and [Identity](xref:security/authentication/identity) which simplifies configuration by using services.</span></span> <span data-ttu-id="21f44-107">Las aplicaciones de ASP.NET Core 1.x que utilice la autenticación o la identidad se pueden actualizar para usar el nuevo modelo tal como se describe a continuación.</span><span class="sxs-lookup"><span data-stu-id="21f44-107">ASP.NET Core 1.x applications that use authentication or Identity can be updated to use the new model as outlined below.</span></span>

<a name="auth-middleware"></a>

## <a name="authentication-middleware-and-services"></a><span data-ttu-id="21f44-108">Middleware de autenticación y servicios</span><span class="sxs-lookup"><span data-stu-id="21f44-108">Authentication Middleware and Services</span></span>
<span data-ttu-id="21f44-109">En los proyectos de 1.x, se configura la autenticación a través de middleware.</span><span class="sxs-lookup"><span data-stu-id="21f44-109">In 1.x projects, authentication is configured via middleware.</span></span> <span data-ttu-id="21f44-110">Se invoca un método de middleware para cada esquema de autenticación que desea admitir.</span><span class="sxs-lookup"><span data-stu-id="21f44-110">A middleware method is invoked for each authentication scheme you want to support.</span></span>

<span data-ttu-id="21f44-111">El siguiente ejemplo de 1.x configura la autenticación de Facebook con identidad en *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="21f44-111">The following 1.x example configures Facebook authentication with Identity in *Startup.cs*:</span></span>

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

<span data-ttu-id="21f44-112">En los 2.0 proyectos, se configura la autenticación a través de servicios.</span><span class="sxs-lookup"><span data-stu-id="21f44-112">In 2.0 projects, authentication is configured via services.</span></span> <span data-ttu-id="21f44-113">Cada esquema de autenticación está registrado en el `ConfigureServices` método *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="21f44-113">Each authentication scheme is registered in the `ConfigureServices` method of *Startup.cs*.</span></span> <span data-ttu-id="21f44-114">El `UseIdentity` método se sustituye por `UseAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="21f44-114">The `UseIdentity` method is replaced with `UseAuthentication`.</span></span>

<span data-ttu-id="21f44-115">El siguiente ejemplo 2.0 configura la autenticación de Facebook con identidad en *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="21f44-115">The following 2.0 example configures Facebook authentication with Identity in *Startup.cs*:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext>();

    // If you want to tweak Identity cookies, they're no longer part of IdentityOptions.
    services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
    services.AddAuthentication()
            .AddFacebook(options => {
                options.AppId = Configuration["auth:facebook:appid"];
                options.AppSecret = Configuration["auth:facebook:appsecret"];
            });
}

public void Configure(IApplicationBuilder app, ILoggerFactory loggerfactory) {
    app.UseAuthentication();
}
```

<span data-ttu-id="21f44-116">El `UseAuthentication` método agrega un componente de middleware de autenticación único que es responsable de la autenticación automática y el control de solicitudes de autenticación remota.</span><span class="sxs-lookup"><span data-stu-id="21f44-116">The `UseAuthentication` method adds a single authentication middleware component which is responsible for automatic authentication and the handling of remote authentication requests.</span></span> <span data-ttu-id="21f44-117">Reemplaza todos los componentes de middleware individuales con un componente de middleware única y común.</span><span class="sxs-lookup"><span data-stu-id="21f44-117">It replaces all of the individual middleware components with a single, common middleware component.</span></span>

<span data-ttu-id="21f44-118">A continuación se muestran 2.0 instrucciones de migración para cada esquema de autenticación principales.</span><span class="sxs-lookup"><span data-stu-id="21f44-118">Below are 2.0 migration instructions for each major authentication scheme.</span></span>

### <a name="cookie-based-authentication"></a><span data-ttu-id="21f44-119">Autenticación basada en cookies</span><span class="sxs-lookup"><span data-stu-id="21f44-119">Cookie-based Authentication</span></span>
<span data-ttu-id="21f44-120">Seleccione una de las dos opciones siguientes y realice los cambios necesarios en *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="21f44-120">Select one of the two options below, and make the necessary changes in *Startup.cs*:</span></span>

1. <span data-ttu-id="21f44-121">Usar cookies con identidad</span><span class="sxs-lookup"><span data-stu-id="21f44-121">Use cookies with Identity</span></span>
    - <span data-ttu-id="21f44-122">Reemplace `UseIdentity` con `UseAuthentication` en el `Configure` método:</span><span class="sxs-lookup"><span data-stu-id="21f44-122">Replace `UseIdentity` with `UseAuthentication` in the `Configure` method:</span></span>

        ```csharp
        app.UseAuthentication();
        ```

    - <span data-ttu-id="21f44-123">Invocar la `AddIdentity` método en el `ConfigureServices` método para agregar los servicios de autenticación de la cookie.</span><span class="sxs-lookup"><span data-stu-id="21f44-123">Invoke the `AddIdentity` method in the `ConfigureServices` method to add the cookie authentication services.</span></span>
    - <span data-ttu-id="21f44-124">Si lo desea, invocar el `ConfigureApplicationCookie` o `ConfigureExternalCookie` método en el `ConfigureServices` método retocar la configuración de cookies de identidad.</span><span class="sxs-lookup"><span data-stu-id="21f44-124">Optionally, invoke the `ConfigureApplicationCookie` or `ConfigureExternalCookie` method in the `ConfigureServices` method to tweak the Identity cookie settings.</span></span>

        ```csharp
        services.AddIdentity<ApplicationUser, IdentityRole>()
                .AddEntityFrameworkStores<ApplicationDbContext>()
                .AddDefaultTokenProviders();
    
        services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
        ```

2. <span data-ttu-id="21f44-125">Usar cookies sin identidad</span><span class="sxs-lookup"><span data-stu-id="21f44-125">Use cookies without Identity</span></span>
    - <span data-ttu-id="21f44-126">Reemplace el `UseCookieAuthentication` llamada al método el `Configure` método con `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="21f44-126">Replace the `UseCookieAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
  
        ```csharp
        app.UseAuthentication();
        ```
 
    - <span data-ttu-id="21f44-127">Invocar la `AddAuthentication` y `AddCookie` métodos en el `ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="21f44-127">Invoke the `AddAuthentication` and `AddCookie` methods in the `ConfigureServices` method:</span></span>

        ```csharp
        // If you don't want the cookie to be automatically authenticated and assigned to HttpContext.User, 
        // remove the CookieAuthenticationDefaults.AuthenticationScheme parameter passed to AddAuthentication.
        services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
                .AddCookie(options => {
                    options.LoginPath = "/Account/LogIn";
                    options.LogoutPath = "/Account/LogOff";
                });
        ```

### <a name="jwt-bearer-authentication"></a><span data-ttu-id="21f44-128">Autenticación de portador JWT</span><span class="sxs-lookup"><span data-stu-id="21f44-128">JWT Bearer Authentication</span></span>
<span data-ttu-id="21f44-129">Realice los cambios siguientes en *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="21f44-129">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="21f44-130">Reemplace el `UseJwtBearerAuthentication` llamada al método el `Configure` método con `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="21f44-130">Replace the `UseJwtBearerAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="21f44-131">Invocar la `AddJwtBearer` método en el `ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="21f44-131">Invoke the `AddJwtBearer` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
            .AddJwtBearer(options => {
                options.Audience = "http://localhost:5001/";
                options.Authority = "http://localhost:5000/";
            });
    ```

    <span data-ttu-id="21f44-132">Este fragmento de código no utiliza la identidad, por lo que el esquema predeterminado debe establecerse pasando `JwtBearerDefaults.AuthenticationScheme` a la `AddAuthentication` método.</span><span class="sxs-lookup"><span data-stu-id="21f44-132">This code snippet doesn't use Identity, so the default scheme should be set by passing `JwtBearerDefaults.AuthenticationScheme` to the `AddAuthentication` method.</span></span>

### <a name="openid-connect-oidc-authentication"></a><span data-ttu-id="21f44-133">OpenID Connect autenticación (OIDC)</span><span class="sxs-lookup"><span data-stu-id="21f44-133">OpenID Connect (OIDC) Authentication</span></span>
<span data-ttu-id="21f44-134">Realice los cambios siguientes en *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="21f44-134">Make the following changes in *Startup.cs*:</span></span>

- <span data-ttu-id="21f44-135">Reemplace el `UseOpenIdConnectAuthentication` llamada al método el `Configure` método con `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="21f44-135">Replace the `UseOpenIdConnectAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="21f44-136">Invocar la `AddOpenIdConnect` método en el `ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="21f44-136">Invoke the `AddOpenIdConnect` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication(options => {
        options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
        options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
    })
    .AddCookie()
    .AddOpenIdConnect(options => {
        options.Authority = Configuration["auth:oidc:authority"];
        options.ClientId = Configuration["auth:oidc:clientid"];
    });
    ```

### <a name="facebook-authentication"></a><span data-ttu-id="21f44-137">Autenticación de Facebook</span><span class="sxs-lookup"><span data-stu-id="21f44-137">Facebook Authentication</span></span>
<span data-ttu-id="21f44-138">Realice los cambios siguientes en *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="21f44-138">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="21f44-139">Reemplace el `UseFacebookAuthentication` llamada al método el `Configure` método con `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="21f44-139">Replace the `UseFacebookAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="21f44-140">Invocar la `AddFacebook` método en el `ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="21f44-140">Invoke the `AddFacebook` method in the `ConfigureServices` method:</span></span>
    
    ```csharp
    services.AddAuthentication()
            .AddFacebook(options => {
                options.AppId = Configuration["auth:facebook:appid"];
                options.AppSecret = Configuration["auth:facebook:appsecret"];
            });
    ```

### <a name="google-authentication"></a><span data-ttu-id="21f44-141">Autenticación de Google</span><span class="sxs-lookup"><span data-stu-id="21f44-141">Google Authentication</span></span>
<span data-ttu-id="21f44-142">Realice los cambios siguientes en *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="21f44-142">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="21f44-143">Reemplace el `UseGoogleAuthentication` llamada al método el `Configure` método con `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="21f44-143">Replace the `UseGoogleAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="21f44-144">Invocar la `AddGoogle` método en el `ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="21f44-144">Invoke the `AddGoogle` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddGoogle(options => {
                options.ClientId = Configuration["auth:google:clientid"];
                options.ClientSecret = Configuration["auth:google:clientsecret"];
            });    
    ```

### <a name="microsoft-account-authentication"></a><span data-ttu-id="21f44-145">Autenticación de cuentas de Microsoft</span><span class="sxs-lookup"><span data-stu-id="21f44-145">Microsoft Account Authentication</span></span>
<span data-ttu-id="21f44-146">Realice los cambios siguientes en *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="21f44-146">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="21f44-147">Reemplace el `UseMicrosoftAccountAuthentication` llamada al método el `Configure` método con `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="21f44-147">Replace the `UseMicrosoftAccountAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="21f44-148">Invocar la `AddMicrosoftAccount` método en el `ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="21f44-148">Invoke the `AddMicrosoftAccount` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddMicrosoftAccount(options => {
                options.ClientId = Configuration["auth:microsoft:clientid"];
                options.ClientSecret = Configuration["auth:microsoft:clientsecret"];
            });
    ``` 

### <a name="twitter-authentication"></a><span data-ttu-id="21f44-149">Autenticación de Twitter</span><span class="sxs-lookup"><span data-stu-id="21f44-149">Twitter Authentication</span></span>
<span data-ttu-id="21f44-150">Realice los cambios siguientes en *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="21f44-150">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="21f44-151">Reemplace el `UseTwitterAuthentication` llamada al método el `Configure` método con `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="21f44-151">Replace the `UseTwitterAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="21f44-152">Invocar la `AddTwitter` método en el `ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="21f44-152">Invoke the `AddTwitter` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddTwitter(options => {
                options.ConsumerKey = Configuration["auth:twitter:consumerkey"];
                options.ConsumerSecret = Configuration["auth:twitter:consumersecret"];
            });
    ```

### <a name="setting-default-authentication-schemes"></a><span data-ttu-id="21f44-153">Esquemas de autenticación predeterminado de configuración</span><span class="sxs-lookup"><span data-stu-id="21f44-153">Setting Default Authentication Schemes</span></span>
<span data-ttu-id="21f44-154">En 1.x, la `AutomaticAuthenticate` y `AutomaticChallenge` propiedades estaban previstas para establecerse en un esquema de autenticación único.</span><span class="sxs-lookup"><span data-stu-id="21f44-154">In 1.x, the `AutomaticAuthenticate` and `AutomaticChallenge` properties were intended to be set on a single authentication scheme.</span></span> <span data-ttu-id="21f44-155">No había forma de buena para exigir esto.</span><span class="sxs-lookup"><span data-stu-id="21f44-155">There was no good way to enforce this.</span></span>

<span data-ttu-id="21f44-156">En 2.0, se han quitado estas dos propiedades como marcas en el individuo `AuthenticationOptions` instancia y han entrado en la base de [AuthenticationOptions](/aspnet/core/api/microsoft.aspnetcore.builder.authenticationoptions) clase.</span><span class="sxs-lookup"><span data-stu-id="21f44-156">In 2.0, these two properties have been removed as flags on the individual `AuthenticationOptions` instance and have moved into the base [AuthenticationOptions](/aspnet/core/api/microsoft.aspnetcore.builder.authenticationoptions) class.</span></span> <span data-ttu-id="21f44-157">Las propiedades se pueden configurar en el `AddAuthentication` llamada al método dentro de la `ConfigureServices` método *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="21f44-157">The properties can be configured in the `AddAuthentication` method call within the `ConfigureServices` method of *Startup.cs*:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme);
```

<span data-ttu-id="21f44-158">Como alternativa, use una versión sobrecargada de la `AddAuthentication` método para establecer más de una propiedad.</span><span class="sxs-lookup"><span data-stu-id="21f44-158">Alternatively, use an overloaded version of the `AddAuthentication` method to set more than one property.</span></span> <span data-ttu-id="21f44-159">En el siguiente ejemplo de método sobrecargado, el esquema predeterminado se establece en `CookieAuthenticationDefaults.AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="21f44-159">In the following overloaded method example, the default scheme is set to `CookieAuthenticationDefaults.AuthenticationScheme`.</span></span> <span data-ttu-id="21f44-160">O bien se puede especificar el esquema de autenticación dentro de la persona `[Authorize]` atributos o las directivas de autorización.</span><span class="sxs-lookup"><span data-stu-id="21f44-160">The authentication scheme may alternatively be specified within your individual `[Authorize]` attributes or authorization policies.</span></span>

```csharp
services.AddAuthentication(options => {
    options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
});
```

<span data-ttu-id="21f44-161">Definir un esquema predeterminado en 2.0 si se cumple alguna de las condiciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="21f44-161">Define a default scheme in 2.0 if one of the following conditions is true:</span></span>
- <span data-ttu-id="21f44-162">Desea que el usuario para iniciar sesión automáticamente en</span><span class="sxs-lookup"><span data-stu-id="21f44-162">You want the user to be automatically signed in</span></span>
- <span data-ttu-id="21f44-163">Usa el `[Authorize]` las directivas de autorización o de atributo sin especificar esquemas</span><span class="sxs-lookup"><span data-stu-id="21f44-163">You use the `[Authorize]` attribute or authorization policies without specifying schemes</span></span>

<span data-ttu-id="21f44-164">Una excepción a esta regla es la `AddIdentity` método.</span><span class="sxs-lookup"><span data-stu-id="21f44-164">An exception to this rule is the `AddIdentity` method.</span></span> <span data-ttu-id="21f44-165">Este método agrega cookies a usted y establece el valor predeterminado autenticarse y desafío esquemas a la cookie de aplicación `IdentityConstants.ApplicationScheme`.</span><span class="sxs-lookup"><span data-stu-id="21f44-165">This method adds cookies for you and sets the default authenticate and challenge schemes to the application cookie `IdentityConstants.ApplicationScheme`.</span></span> <span data-ttu-id="21f44-166">Además, Establece el esquema predeterminado de inicio de sesión en la cookie externa `IdentityConstants.ExternalScheme`.</span><span class="sxs-lookup"><span data-stu-id="21f44-166">Additionally, it sets the default sign-in scheme to the external cookie `IdentityConstants.ExternalScheme`.</span></span>

<a name="obsolete-interface"></a>

## <a name="use-httpcontext-authentication-extensions"></a><span data-ttu-id="21f44-167">Usar las extensiones de autenticación de HttpContext</span><span class="sxs-lookup"><span data-stu-id="21f44-167">Use HttpContext Authentication Extensions</span></span>
<span data-ttu-id="21f44-168">La `IAuthenticationManager` interfaz es el punto de entrada principal en el sistema de autenticación 1.x.</span><span class="sxs-lookup"><span data-stu-id="21f44-168">The `IAuthenticationManager` interface is the main entry point into the 1.x authentication system.</span></span> <span data-ttu-id="21f44-169">Se ha reemplazado por un nuevo conjunto de `HttpContext` métodos de extensión en la `Microsoft.AspNetCore.Authentication` espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="21f44-169">It has been replaced with a new set of `HttpContext` extension methods in the `Microsoft.AspNetCore.Authentication` namespace.</span></span>

<span data-ttu-id="21f44-170">Por ejemplo, 1.x proyectos referencia un `Authentication` propiedad:</span><span class="sxs-lookup"><span data-stu-id="21f44-170">For example, 1.x projects reference an `Authentication` property:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<span data-ttu-id="21f44-171">En los 2.0 proyectos, importar la `Microsoft.AspNetCore.Authentication` espacio de nombres y eliminar el `Authentication` referencias de propiedad:</span><span class="sxs-lookup"><span data-stu-id="21f44-171">In 2.0 projects, import the `Microsoft.AspNetCore.Authentication` namespace, and delete the `Authentication` property references:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="windows-auth-changes"></a>

## <a name="windows-authentication-httpsys--iisintegration"></a><span data-ttu-id="21f44-172">Autenticación de Windows (HTTP.sys / IISIntegration)</span><span class="sxs-lookup"><span data-stu-id="21f44-172">Windows Authentication (HTTP.sys / IISIntegration)</span></span>
<span data-ttu-id="21f44-173">Hay dos variaciones de autenticación de Windows:</span><span class="sxs-lookup"><span data-stu-id="21f44-173">There are two variations of Windows authentication:</span></span>
1. <span data-ttu-id="21f44-174">El host sólo permite que los usuarios autenticados</span><span class="sxs-lookup"><span data-stu-id="21f44-174">The host only allows authenticated users</span></span>
2. <span data-ttu-id="21f44-175">El host permite que ambas anónimo y los usuarios autenticados</span><span class="sxs-lookup"><span data-stu-id="21f44-175">The host allows both anonymous and authenticated users</span></span>

<span data-ttu-id="21f44-176">No se ve afectada por los cambios de 2.0 la primera variación que se ha descrito anteriormente.</span><span class="sxs-lookup"><span data-stu-id="21f44-176">The first variation described above is unaffected by the 2.0 changes.</span></span>

<span data-ttu-id="21f44-177">La segunda variación que se ha descrito anteriormente se ve afectada por los 2.0 cambios.</span><span class="sxs-lookup"><span data-stu-id="21f44-177">The second variation described above is affected by the 2.0 changes.</span></span> <span data-ttu-id="21f44-178">Por ejemplo, puede permitir a los usuarios anónimos en su aplicación en IIS o [HTTP.sys](xref:fundamentals/servers/weblistener) pero autorizando a los usuarios en el nivel de controlador de las capas.</span><span class="sxs-lookup"><span data-stu-id="21f44-178">As an example, you may be allowing anonymous users into your application at the IIS or [HTTP.sys](xref:fundamentals/servers/weblistener) layer but authorizing users at the Controller level.</span></span> <span data-ttu-id="21f44-179">En este escenario, establezca el esquema predeterminado `IISDefaults.AuthenticationScheme` en el `ConfigureServices` método *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="21f44-179">In this scenario, set the default scheme to `IISDefaults.AuthenticationScheme` in the `ConfigureServices` method of *Startup.cs*:</span></span>

```csharp
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

<span data-ttu-id="21f44-180">Para establecer el esquema predeterminado en consecuencia, se impiden la solicitud de autorización para su realización del trabajo.</span><span class="sxs-lookup"><span data-stu-id="21f44-180">Failure to set the default scheme accordingly prevents the authorize request to challenge from working.</span></span>

<a name="identity-cookie-options"></a>

## <a name="identitycookieoptions-instances"></a><span data-ttu-id="21f44-181">Instancias de IdentityCookieOptions</span><span class="sxs-lookup"><span data-stu-id="21f44-181">IdentityCookieOptions Instances</span></span>
<span data-ttu-id="21f44-182">Un efecto secundario de los 2.0 cambios es el conmutador que se va a usar con el nombre de opciones en lugar de instancias de opciones de cookie.</span><span class="sxs-lookup"><span data-stu-id="21f44-182">A side effect of the 2.0 changes is the switch to using named options instead of cookie options instances.</span></span> <span data-ttu-id="21f44-183">Se quita la capacidad de personalizar los nombres de esquema de cookie de identidad.</span><span class="sxs-lookup"><span data-stu-id="21f44-183">The ability to customize the Identity cookie scheme names is removed.</span></span>

<span data-ttu-id="21f44-184">Por ejemplo, los proyectos utilizan 1.x [inyección de constructor](xref:mvc/controllers/dependency-injection#constructor-injection) para pasar un `IdentityCookieOptions` parámetros en *AccountController.cs*.</span><span class="sxs-lookup"><span data-stu-id="21f44-184">For example, 1.x projects use [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) to pass an `IdentityCookieOptions` parameter into *AccountController.cs*.</span></span> <span data-ttu-id="21f44-185">El esquema de autenticación de cookie externo se tiene acceso desde la instancia proporcionada:</span><span class="sxs-lookup"><span data-stu-id="21f44-185">The external cookie authentication scheme is accessed from the provided instance:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor&highlight=4,11)]

<span data-ttu-id="21f44-186">La inyección de constructor mencionado anteriormente, se convierte en innecesaria en los proyectos de 2.0 y el `_externalCookieScheme` campo se puede eliminar:</span><span class="sxs-lookup"><span data-stu-id="21f44-186">The aforementioned constructor injection becomes unnecessary in 2.0 projects, and the `_externalCookieScheme` field can be deleted:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor)]

<span data-ttu-id="21f44-187">El `IdentityConstants.ExternalScheme` constante se puede usar directamente:</span><span class="sxs-lookup"><span data-stu-id="21f44-187">The `IdentityConstants.ExternalScheme` constant can be used directly:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="navigation-properties"></a>

## <a name="add-identityuser-poco-navigation-properties"></a><span data-ttu-id="21f44-188">Agregar propiedades de navegación de POCO IdentityUser</span><span class="sxs-lookup"><span data-stu-id="21f44-188">Add IdentityUser POCO Navigation Properties</span></span>
<span data-ttu-id="21f44-189">Las propiedades de navegación de núcleo de Entity Framework (EF) de la base de `IdentityUser` POCO (objeto CLR antiguos sin formato) se han quitado.</span><span class="sxs-lookup"><span data-stu-id="21f44-189">The Entity Framework (EF) Core navigation properties of the base `IdentityUser` POCO (Plain Old CLR Object) have been removed.</span></span> <span data-ttu-id="21f44-190">Si el proyecto 1.x utiliza estas propiedades, agregarlos manualmente al proyecto 2.0:</span><span class="sxs-lookup"><span data-stu-id="21f44-190">If your 1.x project used these properties, manually add them back to the 2.0 project:</span></span>

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

<span data-ttu-id="21f44-191">Para evitar que las claves externas duplicadas al ejecutar migraciones de núcleo de EF, agregue lo siguiente a su `IdentityDbContext` clase `OnModelCreating` método (después de la `base.OnModelCreating();` llamar a):</span><span class="sxs-lookup"><span data-stu-id="21f44-191">To prevent duplicate foreign keys when running EF Core Migrations, add the following to your `IdentityDbContext` class' `OnModelCreating` method (after the `base.OnModelCreating();` call):</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder builder)
{
    base.OnModelCreating(builder);
    // Customize the ASP.NET Identity model and override the defaults if needed.
    // For example, you can rename the ASP.NET Identity table names and more.
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

## <a name="replace-getexternalauthenticationschemes"></a><span data-ttu-id="21f44-192">Reemplazar GetExternalAuthenticationSchemes</span><span class="sxs-lookup"><span data-stu-id="21f44-192">Replace GetExternalAuthenticationSchemes</span></span>
<span data-ttu-id="21f44-193">El método sincrónico `GetExternalAuthenticationSchemes` se quitó en favor de una versión asincrónica.</span><span class="sxs-lookup"><span data-stu-id="21f44-193">The synchronous method `GetExternalAuthenticationSchemes` was removed in favor of an asynchronous version.</span></span> <span data-ttu-id="21f44-194">1.x proyectos tienen el siguiente código *ManageController.cs*:</span><span class="sxs-lookup"><span data-stu-id="21f44-194">1.x projects have the following code in *ManageController.cs*:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemes)]

<span data-ttu-id="21f44-195">Este método aparece en *Login.cshtml* demasiado:</span><span class="sxs-lookup"><span data-stu-id="21f44-195">This method appears in *Login.cshtml* too:</span></span>

[!code-cshtml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Account/Login.cshtml?range=62,75-84)]

<span data-ttu-id="21f44-196">En los 2.0 proyectos, utilice la `GetExternalAuthenticationSchemesAsync` método:</span><span class="sxs-lookup"><span data-stu-id="21f44-196">In 2.0 projects, use the `GetExternalAuthenticationSchemesAsync` method:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemesAsync)]

<span data-ttu-id="21f44-197">En *Login.cshtml*, `AuthenticationScheme` propiedad accede en la `foreach` bucle cambia a `Name`:</span><span class="sxs-lookup"><span data-stu-id="21f44-197">In *Login.cshtml*, the `AuthenticationScheme` property accessed in the `foreach` loop changes to `Name`:</span></span>

[!code-cshtml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Views/Account/Login.cshtml?range=62,75-84)]

<a name="property-change"></a>

## <a name="manageloginsviewmodel-property-change"></a><span data-ttu-id="21f44-198">Cambio de propiedad ManageLoginsViewModel</span><span class="sxs-lookup"><span data-stu-id="21f44-198">ManageLoginsViewModel Property Change</span></span>
<span data-ttu-id="21f44-199">A `ManageLoginsViewModel` objeto se usa en la `ManageLogins` acción de *ManageController.cs*.</span><span class="sxs-lookup"><span data-stu-id="21f44-199">A `ManageLoginsViewModel` object is used in the `ManageLogins` action of *ManageController.cs*.</span></span> <span data-ttu-id="21f44-200">En 1.x proyectos, el objeto `OtherLogins` propiedad de valor devuelto es de tipo `IList<AuthenticationDescription>`.</span><span class="sxs-lookup"><span data-stu-id="21f44-200">In 1.x projects, the object's `OtherLogins` property return type is `IList<AuthenticationDescription>`.</span></span> <span data-ttu-id="21f44-201">Este tipo de valor devuelto requiere una importación de `Microsoft.AspNetCore.Http.Authentication`:</span><span class="sxs-lookup"><span data-stu-id="21f44-201">This return type requires an import of `Microsoft.AspNetCore.Http.Authentication`:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<span data-ttu-id="21f44-202">En los 2.0 proyectos, se cambia el tipo de valor devuelto a `IList<AuthenticationScheme>`.</span><span class="sxs-lookup"><span data-stu-id="21f44-202">In 2.0 projects, the return type changes to `IList<AuthenticationScheme>`.</span></span> <span data-ttu-id="21f44-203">Este nuevo tipo de valor devuelto es necesario sustituir la `Microsoft.AspNetCore.Http.Authentication` importar con un `Microsoft.AspNetCore.Authentication` importar.</span><span class="sxs-lookup"><span data-stu-id="21f44-203">This new return type requires replacing the `Microsoft.AspNetCore.Http.Authentication` import with a `Microsoft.AspNetCore.Authentication` import.</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<a name="additional-resources"></a>

## <a name="additional-resources"></a><span data-ttu-id="21f44-204">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="21f44-204">Additional Resources</span></span>
<span data-ttu-id="21f44-205">Para obtener más detalles y explicación, consulte el [discusión para autenticación 2.0](https://github.com/aspnet/Security/issues/1338) problema en GitHub.</span><span class="sxs-lookup"><span data-stu-id="21f44-205">For additional details and discussion, see the [Discussion for Auth 2.0](https://github.com/aspnet/Security/issues/1338) issue on GitHub.</span></span>
