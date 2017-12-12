---
title: "Migrar de autenticación e identidad al núcleo de ASP.NET 2.0"
author: scottaddie
description: "En este artículo se describe los pasos más comunes para migrar ASP.NET Core 1.x autenticación e identidad principal de ASP.NET 2.0."
keywords: "Autenticación de ASP.NET Core, identidad,"
ms.author: scaddie
manager: wpickett
ms.date: 10/26/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/1x-to-2x/identity-2x
ms.openlocfilehash: 1d8c75a21cd7110b3e414f0c600e9f05cbaeff45
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
# <a name="migrating-authentication-and-identity-to-aspnet-core-20"></a><span data-ttu-id="aa971-104">Migrar de autenticación e identidad al núcleo de ASP.NET 2.0</span><span class="sxs-lookup"><span data-stu-id="aa971-104">Migrating Authentication and Identity to ASP.NET Core 2.0</span></span>

<span data-ttu-id="aa971-105">Por [Scott Addie](https://github.com/scottaddie) y [Hao Kung](https://github.com/HaoK)</span><span class="sxs-lookup"><span data-stu-id="aa971-105">By [Scott Addie](https://github.com/scottaddie) and [Hao Kung](https://github.com/HaoK)</span></span>

<span data-ttu-id="aa971-106">Núcleo de ASP.NET 2.0 tiene un nuevo modelo para la autenticación y [identidad](xref:security/authentication/identity) que simplifica la configuración mediante el uso de servicios.</span><span class="sxs-lookup"><span data-stu-id="aa971-106">ASP.NET Core 2.0 has a new model for authentication and [Identity](xref:security/authentication/identity) which simplifies configuration by using services.</span></span> <span data-ttu-id="aa971-107">Las aplicaciones de ASP.NET Core 1.x que utilice la autenticación o la identidad se pueden actualizar para usar el nuevo modelo tal como se describe a continuación.</span><span class="sxs-lookup"><span data-stu-id="aa971-107">ASP.NET Core 1.x applications that use authentication or Identity can be updated to use the new model as outlined below.</span></span>

<a name="auth-middleware"></a>

## <a name="authentication-middleware-and-services"></a><span data-ttu-id="aa971-108">Middleware de autenticación y servicios</span><span class="sxs-lookup"><span data-stu-id="aa971-108">Authentication Middleware and Services</span></span>
<span data-ttu-id="aa971-109">En los proyectos de 1.x, se configura la autenticación a través de middleware.</span><span class="sxs-lookup"><span data-stu-id="aa971-109">In 1.x projects, authentication is configured via middleware.</span></span> <span data-ttu-id="aa971-110">Se invoca un método de middleware para cada esquema de autenticación que desea admitir.</span><span class="sxs-lookup"><span data-stu-id="aa971-110">A middleware method is invoked for each authentication scheme you want to support.</span></span>

<span data-ttu-id="aa971-111">El siguiente ejemplo de 1.x configura la autenticación de Facebook con identidad en *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="aa971-111">The following 1.x example configures Facebook authentication with Identity in *Startup.cs*:</span></span>

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

<span data-ttu-id="aa971-112">En los 2.0 proyectos, se configura la autenticación a través de servicios.</span><span class="sxs-lookup"><span data-stu-id="aa971-112">In 2.0 projects, authentication is configured via services.</span></span> <span data-ttu-id="aa971-113">Cada esquema de autenticación está registrado en el `ConfigureServices` método *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="aa971-113">Each authentication scheme is registered in the `ConfigureServices` method of *Startup.cs*.</span></span> <span data-ttu-id="aa971-114">El `UseIdentity` método se sustituye por `UseAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="aa971-114">The `UseIdentity` method is replaced with `UseAuthentication`.</span></span>

<span data-ttu-id="aa971-115">El siguiente ejemplo 2.0 configura la autenticación de Facebook con identidad en *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="aa971-115">The following 2.0 example configures Facebook authentication with Identity in *Startup.cs*:</span></span>

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

<span data-ttu-id="aa971-116">El `UseAuthentication` método agrega un componente de middleware de autenticación único que es responsable de la autenticación automática y el control de solicitudes de autenticación remota.</span><span class="sxs-lookup"><span data-stu-id="aa971-116">The `UseAuthentication` method adds a single authentication middleware component which is responsible for automatic authentication and the handling of remote authentication requests.</span></span> <span data-ttu-id="aa971-117">Reemplaza todos los componentes de middleware individuales con un componente de middleware única y común.</span><span class="sxs-lookup"><span data-stu-id="aa971-117">It replaces all of the individual middleware components with a single, common middleware component.</span></span>

<span data-ttu-id="aa971-118">A continuación se muestran 2.0 instrucciones de migración para cada esquema de autenticación principales.</span><span class="sxs-lookup"><span data-stu-id="aa971-118">Below are 2.0 migration instructions for each major authentication scheme.</span></span>

### <a name="cookie-based-authentication"></a><span data-ttu-id="aa971-119">Autenticación basada en cookies</span><span class="sxs-lookup"><span data-stu-id="aa971-119">Cookie-based Authentication</span></span>
<span data-ttu-id="aa971-120">Seleccione una de las dos opciones siguientes y realice los cambios necesarios en *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="aa971-120">Select one of the two options below, and make the necessary changes in *Startup.cs*:</span></span>

1. <span data-ttu-id="aa971-121">Usar cookies con identidad</span><span class="sxs-lookup"><span data-stu-id="aa971-121">Use cookies with Identity</span></span>
    - <span data-ttu-id="aa971-122">Reemplace `UseIdentity` con `UseAuthentication` en el `Configure` método:</span><span class="sxs-lookup"><span data-stu-id="aa971-122">Replace `UseIdentity` with `UseAuthentication` in the `Configure` method:</span></span>

        ```csharp
        app.UseAuthentication();
        ```

    - <span data-ttu-id="aa971-123">Invocar la `AddIdentity` método en el `ConfigureServices` método para agregar los servicios de autenticación de la cookie.</span><span class="sxs-lookup"><span data-stu-id="aa971-123">Invoke the `AddIdentity` method in the `ConfigureServices` method to add the cookie authentication services.</span></span>
    - <span data-ttu-id="aa971-124">Si lo desea, invocar el `ConfigureApplicationCookie` o `ConfigureExternalCookie` método en el `ConfigureServices` método retocar la configuración de cookies de identidad.</span><span class="sxs-lookup"><span data-stu-id="aa971-124">Optionally, invoke the `ConfigureApplicationCookie` or `ConfigureExternalCookie` method in the `ConfigureServices` method to tweak the Identity cookie settings.</span></span>

        ```csharp
        services.AddIdentity<ApplicationUser, IdentityRole>()
                .AddEntityFrameworkStores<ApplicationDbContext>()
                .AddDefaultTokenProviders();
    
        services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
        ```

2. <span data-ttu-id="aa971-125">Usar cookies sin identidad</span><span class="sxs-lookup"><span data-stu-id="aa971-125">Use cookies without Identity</span></span>
    - <span data-ttu-id="aa971-126">Reemplace el `UseCookieAuthentication` llamada al método el `Configure` método con `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="aa971-126">Replace the `UseCookieAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
  
        ```csharp
        app.UseAuthentication();
        ```
 
    - <span data-ttu-id="aa971-127">Invocar la `AddAuthentication` y `AddCookie` métodos en el `ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="aa971-127">Invoke the `AddAuthentication` and `AddCookie` methods in the `ConfigureServices` method:</span></span>

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

### <a name="jwt-bearer-authentication"></a><span data-ttu-id="aa971-128">Autenticación de portador JWT</span><span class="sxs-lookup"><span data-stu-id="aa971-128">JWT Bearer Authentication</span></span>
<span data-ttu-id="aa971-129">Realice los cambios siguientes en *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="aa971-129">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="aa971-130">Reemplace el `UseJwtBearerAuthentication` llamada al método el `Configure` método con `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="aa971-130">Replace the `UseJwtBearerAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="aa971-131">Invocar la `AddJwtBearer` método en el `ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="aa971-131">Invoke the `AddJwtBearer` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
            .AddJwtBearer(options => 
            {
                options.Audience = "http://localhost:5001/";
                options.Authority = "http://localhost:5000/";
            });
    ```

    <span data-ttu-id="aa971-132">Este fragmento de código no utiliza la identidad, por lo que el esquema predeterminado debe establecerse pasando `JwtBearerDefaults.AuthenticationScheme` a la `AddAuthentication` método.</span><span class="sxs-lookup"><span data-stu-id="aa971-132">This code snippet doesn't use Identity, so the default scheme should be set by passing `JwtBearerDefaults.AuthenticationScheme` to the `AddAuthentication` method.</span></span>

### <a name="openid-connect-oidc-authentication"></a><span data-ttu-id="aa971-133">OpenID Connect autenticación (OIDC)</span><span class="sxs-lookup"><span data-stu-id="aa971-133">OpenID Connect (OIDC) Authentication</span></span>
<span data-ttu-id="aa971-134">Realice los cambios siguientes en *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="aa971-134">Make the following changes in *Startup.cs*:</span></span>

- <span data-ttu-id="aa971-135">Reemplace el `UseOpenIdConnectAuthentication` llamada al método el `Configure` método con `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="aa971-135">Replace the `UseOpenIdConnectAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="aa971-136">Invocar la `AddOpenIdConnect` método en el `ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="aa971-136">Invoke the `AddOpenIdConnect` method in the `ConfigureServices` method:</span></span>

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

### <a name="facebook-authentication"></a><span data-ttu-id="aa971-137">Autenticación de Facebook</span><span class="sxs-lookup"><span data-stu-id="aa971-137">Facebook Authentication</span></span>
<span data-ttu-id="aa971-138">Realice los cambios siguientes en *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="aa971-138">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="aa971-139">Reemplace el `UseFacebookAuthentication` llamada al método el `Configure` método con `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="aa971-139">Replace the `UseFacebookAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="aa971-140">Invocar la `AddFacebook` método en el `ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="aa971-140">Invoke the `AddFacebook` method in the `ConfigureServices` method:</span></span>
    
    ```csharp
    services.AddAuthentication()
            .AddFacebook(options => 
            {
                options.AppId = Configuration["auth:facebook:appid"];
                options.AppSecret = Configuration["auth:facebook:appsecret"];
            });
    ```

### <a name="google-authentication"></a><span data-ttu-id="aa971-141">Autenticación de Google</span><span class="sxs-lookup"><span data-stu-id="aa971-141">Google Authentication</span></span>
<span data-ttu-id="aa971-142">Realice los cambios siguientes en *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="aa971-142">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="aa971-143">Reemplace el `UseGoogleAuthentication` llamada al método el `Configure` método con `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="aa971-143">Replace the `UseGoogleAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="aa971-144">Invocar la `AddGoogle` método en el `ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="aa971-144">Invoke the `AddGoogle` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddGoogle(options => 
            {
                options.ClientId = Configuration["auth:google:clientid"];
                options.ClientSecret = Configuration["auth:google:clientsecret"];
            });    
    ```

### <a name="microsoft-account-authentication"></a><span data-ttu-id="aa971-145">Autenticación de cuentas de Microsoft</span><span class="sxs-lookup"><span data-stu-id="aa971-145">Microsoft Account Authentication</span></span>
<span data-ttu-id="aa971-146">Realice los cambios siguientes en *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="aa971-146">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="aa971-147">Reemplace el `UseMicrosoftAccountAuthentication` llamada al método el `Configure` método con `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="aa971-147">Replace the `UseMicrosoftAccountAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="aa971-148">Invocar la `AddMicrosoftAccount` método en el `ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="aa971-148">Invoke the `AddMicrosoftAccount` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddMicrosoftAccount(options => 
            {
                options.ClientId = Configuration["auth:microsoft:clientid"];
                options.ClientSecret = Configuration["auth:microsoft:clientsecret"];
            });
    ``` 

### <a name="twitter-authentication"></a><span data-ttu-id="aa971-149">Autenticación de Twitter</span><span class="sxs-lookup"><span data-stu-id="aa971-149">Twitter Authentication</span></span>
<span data-ttu-id="aa971-150">Realice los cambios siguientes en *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="aa971-150">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="aa971-151">Reemplace el `UseTwitterAuthentication` llamada al método el `Configure` método con `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="aa971-151">Replace the `UseTwitterAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="aa971-152">Invocar la `AddTwitter` método en el `ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="aa971-152">Invoke the `AddTwitter` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddTwitter(options => 
            {
                options.ConsumerKey = Configuration["auth:twitter:consumerkey"];
                options.ConsumerSecret = Configuration["auth:twitter:consumersecret"];
            });
    ```

### <a name="setting-default-authentication-schemes"></a><span data-ttu-id="aa971-153">Esquemas de autenticación predeterminado de configuración</span><span class="sxs-lookup"><span data-stu-id="aa971-153">Setting Default Authentication Schemes</span></span>
<span data-ttu-id="aa971-154">En 1.x, la `AutomaticAuthenticate` y `AutomaticChallenge` propiedades de la [AuthenticationOptions](https://docs.microsoft.com/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1) clase base se pensada para establecerse en un esquema de autenticación único.</span><span class="sxs-lookup"><span data-stu-id="aa971-154">In 1.x, the `AutomaticAuthenticate` and `AutomaticChallenge` properties of the [AuthenticationOptions](https://docs.microsoft.com/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1) base class were intended to be set on a single authentication scheme.</span></span> <span data-ttu-id="aa971-155">No había forma de buena para exigir esto.</span><span class="sxs-lookup"><span data-stu-id="aa971-155">There was no good way to enforce this.</span></span>

<span data-ttu-id="aa971-156">En 2.0, se han quitado estas dos propiedades como propiedades en la persona `AuthenticationOptions` instancia.</span><span class="sxs-lookup"><span data-stu-id="aa971-156">In 2.0, these two properties have been removed as properties on the individual `AuthenticationOptions` instance.</span></span> <span data-ttu-id="aa971-157">Se pueden configurar en el `AddAuthentication` llamada al método dentro de la `ConfigureServices` método *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="aa971-157">They can be configured in the `AddAuthentication` method call within the `ConfigureServices` method of *Startup.cs*:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme);
```

<span data-ttu-id="aa971-158">En el fragmento de código anterior, el esquema predeterminado se establece en `CookieAuthenticationDefaults.AuthenticationScheme` ("Cookies").</span><span class="sxs-lookup"><span data-stu-id="aa971-158">In the preceding code snippet, the default scheme is set to `CookieAuthenticationDefaults.AuthenticationScheme` ("Cookies").</span></span>

<span data-ttu-id="aa971-159">Como alternativa, use una versión sobrecargada de la `AddAuthentication` método para establecer más de una propiedad.</span><span class="sxs-lookup"><span data-stu-id="aa971-159">Alternatively, use an overloaded version of the `AddAuthentication` method to set more than one property.</span></span> <span data-ttu-id="aa971-160">En el siguiente ejemplo de método sobrecargado, el esquema predeterminado se establece en `CookieAuthenticationDefaults.AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="aa971-160">In the following overloaded method example, the default scheme is set to `CookieAuthenticationDefaults.AuthenticationScheme`.</span></span> <span data-ttu-id="aa971-161">O bien se puede especificar el esquema de autenticación dentro de la persona `[Authorize]` atributos o las directivas de autorización.</span><span class="sxs-lookup"><span data-stu-id="aa971-161">The authentication scheme may alternatively be specified within your individual `[Authorize]` attributes or authorization policies.</span></span>

```csharp
services.AddAuthentication(options => 
{
    options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
});
```

<span data-ttu-id="aa971-162">Definir un esquema predeterminado en 2.0 si se cumple alguna de las condiciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="aa971-162">Define a default scheme in 2.0 if one of the following conditions is true:</span></span>
- <span data-ttu-id="aa971-163">Desea que el usuario para iniciar sesión automáticamente en</span><span class="sxs-lookup"><span data-stu-id="aa971-163">You want the user to be automatically signed in</span></span>
- <span data-ttu-id="aa971-164">Usa el `[Authorize]` las directivas de autorización o de atributo sin especificar esquemas</span><span class="sxs-lookup"><span data-stu-id="aa971-164">You use the `[Authorize]` attribute or authorization policies without specifying schemes</span></span>

<span data-ttu-id="aa971-165">Una excepción a esta regla es la `AddIdentity` método.</span><span class="sxs-lookup"><span data-stu-id="aa971-165">An exception to this rule is the `AddIdentity` method.</span></span> <span data-ttu-id="aa971-166">Este método agrega cookies a usted y establece el valor predeterminado autenticarse y desafío esquemas a la cookie de aplicación `IdentityConstants.ApplicationScheme`.</span><span class="sxs-lookup"><span data-stu-id="aa971-166">This method adds cookies for you and sets the default authenticate and challenge schemes to the application cookie `IdentityConstants.ApplicationScheme`.</span></span> <span data-ttu-id="aa971-167">Además, Establece el esquema predeterminado de inicio de sesión en la cookie externa `IdentityConstants.ExternalScheme`.</span><span class="sxs-lookup"><span data-stu-id="aa971-167">Additionally, it sets the default sign-in scheme to the external cookie `IdentityConstants.ExternalScheme`.</span></span>

<a name="obsolete-interface"></a>

## <a name="use-httpcontext-authentication-extensions"></a><span data-ttu-id="aa971-168">Usar las extensiones de autenticación de HttpContext</span><span class="sxs-lookup"><span data-stu-id="aa971-168">Use HttpContext Authentication Extensions</span></span>
<span data-ttu-id="aa971-169">La `IAuthenticationManager` interfaz es el punto de entrada principal en el sistema de autenticación 1.x.</span><span class="sxs-lookup"><span data-stu-id="aa971-169">The `IAuthenticationManager` interface is the main entry point into the 1.x authentication system.</span></span> <span data-ttu-id="aa971-170">Se ha reemplazado por un nuevo conjunto de `HttpContext` métodos de extensión en la `Microsoft.AspNetCore.Authentication` espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="aa971-170">It has been replaced with a new set of `HttpContext` extension methods in the `Microsoft.AspNetCore.Authentication` namespace.</span></span>

<span data-ttu-id="aa971-171">Por ejemplo, 1.x proyectos referencia un `Authentication` propiedad:</span><span class="sxs-lookup"><span data-stu-id="aa971-171">For example, 1.x projects reference an `Authentication` property:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<span data-ttu-id="aa971-172">En los 2.0 proyectos, importar la `Microsoft.AspNetCore.Authentication` espacio de nombres y eliminar el `Authentication` referencias de propiedad:</span><span class="sxs-lookup"><span data-stu-id="aa971-172">In 2.0 projects, import the `Microsoft.AspNetCore.Authentication` namespace, and delete the `Authentication` property references:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="windows-auth-changes"></a>

## <a name="windows-authentication-httpsys--iisintegration"></a><span data-ttu-id="aa971-173">Autenticación de Windows (HTTP.sys / IISIntegration)</span><span class="sxs-lookup"><span data-stu-id="aa971-173">Windows Authentication (HTTP.sys / IISIntegration)</span></span>
<span data-ttu-id="aa971-174">Hay dos variaciones de autenticación de Windows:</span><span class="sxs-lookup"><span data-stu-id="aa971-174">There are two variations of Windows authentication:</span></span>
1. <span data-ttu-id="aa971-175">El host sólo permite que los usuarios autenticados</span><span class="sxs-lookup"><span data-stu-id="aa971-175">The host only allows authenticated users</span></span>
2. <span data-ttu-id="aa971-176">El host permite que ambas anónimo y los usuarios autenticados</span><span class="sxs-lookup"><span data-stu-id="aa971-176">The host allows both anonymous and authenticated users</span></span>

<span data-ttu-id="aa971-177">No se ve afectada por los cambios de 2.0 la primera variación que se ha descrito anteriormente.</span><span class="sxs-lookup"><span data-stu-id="aa971-177">The first variation described above is unaffected by the 2.0 changes.</span></span>

<span data-ttu-id="aa971-178">La segunda variación que se ha descrito anteriormente se ve afectada por los 2.0 cambios.</span><span class="sxs-lookup"><span data-stu-id="aa971-178">The second variation described above is affected by the 2.0 changes.</span></span> <span data-ttu-id="aa971-179">Por ejemplo, puede permitir a los usuarios anónimos en su aplicación en IIS o [HTTP.sys](xref:fundamentals/servers/weblistener) pero autorizando a los usuarios en el nivel de controlador de las capas.</span><span class="sxs-lookup"><span data-stu-id="aa971-179">As an example, you may be allowing anonymous users into your application at the IIS or [HTTP.sys](xref:fundamentals/servers/weblistener) layer but authorizing users at the Controller level.</span></span> <span data-ttu-id="aa971-180">En este escenario, establezca el esquema predeterminado `IISDefaults.AuthenticationScheme` en el `ConfigureServices` método *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="aa971-180">In this scenario, set the default scheme to `IISDefaults.AuthenticationScheme` in the `ConfigureServices` method of *Startup.cs*:</span></span>

```csharp
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

<span data-ttu-id="aa971-181">Para establecer el esquema predeterminado en consecuencia, se impiden la solicitud de autorización para su realización del trabajo.</span><span class="sxs-lookup"><span data-stu-id="aa971-181">Failure to set the default scheme accordingly prevents the authorize request to challenge from working.</span></span>

<a name="identity-cookie-options"></a>

## <a name="identitycookieoptions-instances"></a><span data-ttu-id="aa971-182">Instancias de IdentityCookieOptions</span><span class="sxs-lookup"><span data-stu-id="aa971-182">IdentityCookieOptions Instances</span></span>
<span data-ttu-id="aa971-183">Un efecto secundario de los 2.0 cambios es el conmutador que se va a usar con el nombre de opciones en lugar de instancias de opciones de cookie.</span><span class="sxs-lookup"><span data-stu-id="aa971-183">A side effect of the 2.0 changes is the switch to using named options instead of cookie options instances.</span></span> <span data-ttu-id="aa971-184">Se quita la capacidad de personalizar los nombres de esquema de cookie de identidad.</span><span class="sxs-lookup"><span data-stu-id="aa971-184">The ability to customize the Identity cookie scheme names is removed.</span></span>

<span data-ttu-id="aa971-185">Por ejemplo, los proyectos utilizan 1.x [inyección de constructor](xref:mvc/controllers/dependency-injection#constructor-injection) para pasar un `IdentityCookieOptions` parámetros en *AccountController.cs*.</span><span class="sxs-lookup"><span data-stu-id="aa971-185">For example, 1.x projects use [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) to pass an `IdentityCookieOptions` parameter into *AccountController.cs*.</span></span> <span data-ttu-id="aa971-186">El esquema de autenticación de cookie externo se tiene acceso desde la instancia proporcionada:</span><span class="sxs-lookup"><span data-stu-id="aa971-186">The external cookie authentication scheme is accessed from the provided instance:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor&highlight=4,11)]

<span data-ttu-id="aa971-187">La inyección de constructor mencionado anteriormente, se convierte en innecesaria en los proyectos de 2.0 y el `_externalCookieScheme` campo se puede eliminar:</span><span class="sxs-lookup"><span data-stu-id="aa971-187">The aforementioned constructor injection becomes unnecessary in 2.0 projects, and the `_externalCookieScheme` field can be deleted:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor)]

<span data-ttu-id="aa971-188">El `IdentityConstants.ExternalScheme` constante se puede usar directamente:</span><span class="sxs-lookup"><span data-stu-id="aa971-188">The `IdentityConstants.ExternalScheme` constant can be used directly:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="navigation-properties"></a>

## <a name="add-identityuser-poco-navigation-properties"></a><span data-ttu-id="aa971-189">Agregar propiedades de navegación de POCO IdentityUser</span><span class="sxs-lookup"><span data-stu-id="aa971-189">Add IdentityUser POCO Navigation Properties</span></span>
<span data-ttu-id="aa971-190">Las propiedades de navegación de núcleo de Entity Framework (EF) de la base de `IdentityUser` POCO (objeto CLR antiguos sin formato) se han quitado.</span><span class="sxs-lookup"><span data-stu-id="aa971-190">The Entity Framework (EF) Core navigation properties of the base `IdentityUser` POCO (Plain Old CLR Object) have been removed.</span></span> <span data-ttu-id="aa971-191">Si el proyecto 1.x utiliza estas propiedades, agregarlos manualmente al proyecto 2.0:</span><span class="sxs-lookup"><span data-stu-id="aa971-191">If your 1.x project used these properties, manually add them back to the 2.0 project:</span></span>

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

<span data-ttu-id="aa971-192">Para evitar que las claves externas duplicadas al ejecutar migraciones de núcleo de EF, agregue lo siguiente a su `IdentityDbContext` clase `OnModelCreating` método (después de la `base.OnModelCreating();` llamar a):</span><span class="sxs-lookup"><span data-stu-id="aa971-192">To prevent duplicate foreign keys when running EF Core Migrations, add the following to your `IdentityDbContext` class' `OnModelCreating` method (after the `base.OnModelCreating();` call):</span></span>

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

## <a name="replace-getexternalauthenticationschemes"></a><span data-ttu-id="aa971-193">Reemplazar GetExternalAuthenticationSchemes</span><span class="sxs-lookup"><span data-stu-id="aa971-193">Replace GetExternalAuthenticationSchemes</span></span>
<span data-ttu-id="aa971-194">El método sincrónico `GetExternalAuthenticationSchemes` se quitó en favor de una versión asincrónica.</span><span class="sxs-lookup"><span data-stu-id="aa971-194">The synchronous method `GetExternalAuthenticationSchemes` was removed in favor of an asynchronous version.</span></span> <span data-ttu-id="aa971-195">1.x proyectos tienen el siguiente código *ManageController.cs*:</span><span class="sxs-lookup"><span data-stu-id="aa971-195">1.x projects have the following code in *ManageController.cs*:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemes)]

<span data-ttu-id="aa971-196">Este método aparece en *Login.cshtml* demasiado:</span><span class="sxs-lookup"><span data-stu-id="aa971-196">This method appears in *Login.cshtml* too:</span></span>

[!code-cshtml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Account/Login.cshtml?range=62,75-84)]

<span data-ttu-id="aa971-197">En los 2.0 proyectos, utilice la `GetExternalAuthenticationSchemesAsync` método:</span><span class="sxs-lookup"><span data-stu-id="aa971-197">In 2.0 projects, use the `GetExternalAuthenticationSchemesAsync` method:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemesAsync)]

<span data-ttu-id="aa971-198">En *Login.cshtml*, `AuthenticationScheme` propiedad accede en la `foreach` bucle cambia a `Name`:</span><span class="sxs-lookup"><span data-stu-id="aa971-198">In *Login.cshtml*, the `AuthenticationScheme` property accessed in the `foreach` loop changes to `Name`:</span></span>

[!code-cshtml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Views/Account/Login.cshtml?range=62,75-84)]

<a name="property-change"></a>

## <a name="manageloginsviewmodel-property-change"></a><span data-ttu-id="aa971-199">Cambio de propiedad ManageLoginsViewModel</span><span class="sxs-lookup"><span data-stu-id="aa971-199">ManageLoginsViewModel Property Change</span></span>
<span data-ttu-id="aa971-200">A `ManageLoginsViewModel` objeto se usa en la `ManageLogins` acción de *ManageController.cs*.</span><span class="sxs-lookup"><span data-stu-id="aa971-200">A `ManageLoginsViewModel` object is used in the `ManageLogins` action of *ManageController.cs*.</span></span> <span data-ttu-id="aa971-201">En 1.x proyectos, el objeto `OtherLogins` propiedad de valor devuelto es de tipo `IList<AuthenticationDescription>`.</span><span class="sxs-lookup"><span data-stu-id="aa971-201">In 1.x projects, the object's `OtherLogins` property return type is `IList<AuthenticationDescription>`.</span></span> <span data-ttu-id="aa971-202">Este tipo de valor devuelto requiere una importación de `Microsoft.AspNetCore.Http.Authentication`:</span><span class="sxs-lookup"><span data-stu-id="aa971-202">This return type requires an import of `Microsoft.AspNetCore.Http.Authentication`:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<span data-ttu-id="aa971-203">En los 2.0 proyectos, se cambia el tipo de valor devuelto a `IList<AuthenticationScheme>`.</span><span class="sxs-lookup"><span data-stu-id="aa971-203">In 2.0 projects, the return type changes to `IList<AuthenticationScheme>`.</span></span> <span data-ttu-id="aa971-204">Este nuevo tipo de valor devuelto es necesario sustituir la `Microsoft.AspNetCore.Http.Authentication` importar con un `Microsoft.AspNetCore.Authentication` importar.</span><span class="sxs-lookup"><span data-stu-id="aa971-204">This new return type requires replacing the `Microsoft.AspNetCore.Http.Authentication` import with a `Microsoft.AspNetCore.Authentication` import.</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<a name="additional-resources"></a>

## <a name="additional-resources"></a><span data-ttu-id="aa971-205">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="aa971-205">Additional Resources</span></span>
<span data-ttu-id="aa971-206">Para obtener más detalles y explicación, consulte el [discusión para autenticación 2.0](https://github.com/aspnet/Security/issues/1338) problema en GitHub.</span><span class="sxs-lookup"><span data-stu-id="aa971-206">For additional details and discussion, see the [Discussion for Auth 2.0](https://github.com/aspnet/Security/issues/1338) issue on GitHub.</span></span>
