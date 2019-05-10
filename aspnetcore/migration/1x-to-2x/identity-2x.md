---
title: Migrar de autenticación e identidad a ASP.NET Core 2.0
author: scottaddie
description: En este artículo se describe los pasos más comunes para migrar la autenticación de ASP.NET Core 1.x y la identidad a ASP.NET Core 2.0.
ms.author: scaddie
ms.date: 12/18/2018
uid: migration/1x-to-2x/identity-2x
ms.openlocfilehash: d11d41c82236436096660a24df81a3df4da0fb8e
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/27/2019
ms.locfileid: "64892212"
---
# <a name="migrate-authentication-and-identity-to-aspnet-core-20"></a>Migrar de autenticación e identidad a ASP.NET Core 2.0

Por [Scott Addie](https://github.com/scottaddie) y [Hao Kung](https://github.com/HaoK)

ASP.NET Core 2.0 tiene un nuevo modelo para la autenticación y [identidad](xref:security/authentication/identity) que simplifica la configuración mediante el uso de servicios. Las aplicaciones de ASP.NET Core 1.x que utilizan la autenticación o Identity se pueden actualizar para usar el nuevo modelo, tal como se describe a continuación.

<a name="auth-middleware"></a>

## <a name="authentication-middleware-and-services"></a>Middleware de autenticación y servicios

En los proyectos de 1.x, la autenticación se configura a través de middleware. Se invoca un método de middleware para cada esquema de autenticación que desea admitir.

El siguiente ejemplo de 1.x configura la autenticación de Facebook con identidad en *Startup.cs*:

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

En los 2.0 proyectos, la autenticación se configura a través de servicios. Cada esquema de autenticación está registrado en el `ConfigureServices` método *Startup.cs*. El `UseIdentity` método se reemplaza por `UseAuthentication`.

El siguiente ejemplo 2.0 configura la autenticación de Facebook con identidad en *Startup.cs*:

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

El `UseAuthentication` método agrega un componente de middleware de autenticación único que es responsable de la autenticación automática y la administración de las solicitudes de autenticación remota. Reemplaza todos los componentes de middleware individuales con un componente de middleware única y común.

A continuación se muestran 2.0 instrucciones de migración para cada esquema de autenticación principales.

### <a name="cookie-based-authentication"></a>Autenticación basada en cookies

Seleccione una de las dos opciones siguientes y realice los cambios necesarios en *Startup.cs*:

1. Uso de cookies con identidad
    - Reemplace `UseIdentity` con `UseAuthentication` en el `Configure` método:

        ```csharp
        app.UseAuthentication();
        ```

    - Invocar el `AddIdentity` método en el `ConfigureServices` método para agregar los servicios de autenticación de cookies.
    - Si lo desea, invocar el `ConfigureApplicationCookie` o `ConfigureExternalCookie` método en el `ConfigureServices` método ajustar la configuración de la cookie de identidad.

        ```csharp
        services.AddIdentity<ApplicationUser, IdentityRole>()
                .AddEntityFrameworkStores<ApplicationDbContext>()
                .AddDefaultTokenProviders();

        services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
        ```

2. Usar cookies sin identidad
    - Reemplace el `UseCookieAuthentication` llame al método el `Configure` método con `UseAuthentication`:

        ```csharp
        app.UseAuthentication();
        ```

    - Invocar el `AddAuthentication` y `AddCookie` métodos en el `ConfigureServices` método:

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

### <a name="jwt-bearer-authentication"></a>Autenticación de portador JWT

Realice los siguientes cambios en *Startup.cs*:
- Reemplace el `UseJwtBearerAuthentication` llame al método el `Configure` método con `UseAuthentication`:

    ```csharp
    app.UseAuthentication();
    ```

- Invocar el `AddJwtBearer` método en el `ConfigureServices` método:

    ```csharp
    services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
            .AddJwtBearer(options =>
            {
                options.Audience = "http://localhost:5001/";
                options.Authority = "http://localhost:5000/";
            });
    ```

    Este fragmento de código no usa la identidad, por lo que se debe establecer el esquema predeterminado pasando `JwtBearerDefaults.AuthenticationScheme` a la `AddAuthentication` método.

### <a name="openid-connect-oidc-authentication"></a>Autenticación de OpenID Connect (OIDC)

Realice los siguientes cambios en *Startup.cs*:

- Reemplace el `UseOpenIdConnectAuthentication` llame al método el `Configure` método con `UseAuthentication`:

    ```csharp
    app.UseAuthentication();
    ```

- Invocar el `AddOpenIdConnect` método en el `ConfigureServices` método:

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

### <a name="facebook-authentication"></a>Autenticación con Facebook

Realice los siguientes cambios en *Startup.cs*:
- Reemplace el `UseFacebookAuthentication` llame al método el `Configure` método con `UseAuthentication`:

    ```csharp
    app.UseAuthentication();
    ```

- Invocar el `AddFacebook` método en el `ConfigureServices` método:

    ```csharp
    services.AddAuthentication()
            .AddFacebook(options =>
            {
                options.AppId = Configuration["auth:facebook:appid"];
                options.AppSecret = Configuration["auth:facebook:appsecret"];
            });
    ```

### <a name="google-authentication"></a>Autenticación con Google

Realice los siguientes cambios en *Startup.cs*:
- Reemplace el `UseGoogleAuthentication` llame al método el `Configure` método con `UseAuthentication`:

    ```csharp
    app.UseAuthentication();
    ```

- Invocar el `AddGoogle` método en el `ConfigureServices` método:

    ```csharp
    services.AddAuthentication()
            .AddGoogle(options =>
            {
                options.ClientId = Configuration["auth:google:clientid"];
                options.ClientSecret = Configuration["auth:google:clientsecret"];
            });
    ```

### <a name="microsoft-account-authentication"></a>Autenticación de Microsoft Account

Realice los siguientes cambios en *Startup.cs*:
- Reemplace el `UseMicrosoftAccountAuthentication` llame al método el `Configure` método con `UseAuthentication`:

    ```csharp
    app.UseAuthentication();
    ```

- Invocar el `AddMicrosoftAccount` método en el `ConfigureServices` método:

    ```csharp
    services.AddAuthentication()
            .AddMicrosoftAccount(options =>
            {
                options.ClientId = Configuration["auth:microsoft:clientid"];
                options.ClientSecret = Configuration["auth:microsoft:clientsecret"];
            });
    ```

### <a name="twitter-authentication"></a>Autenticación con Twitter

Realice los siguientes cambios en *Startup.cs*:
- Reemplace el `UseTwitterAuthentication` llame al método el `Configure` método con `UseAuthentication`:

    ```csharp
    app.UseAuthentication();
    ```

- Invocar el `AddTwitter` método en el `ConfigureServices` método:

    ```csharp
    services.AddAuthentication()
            .AddTwitter(options =>
            {
                options.ConsumerKey = Configuration["auth:twitter:consumerkey"];
                options.ConsumerSecret = Configuration["auth:twitter:consumersecret"];
            });
    ```

### <a name="setting-default-authentication-schemes"></a>Esquemas de autenticación predeterminado de configuración

En la versión 1.x, el `AutomaticAuthenticate` y `AutomaticChallenge` propiedades de la [AuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1) clase base se pensada para establecerse en un esquema de autenticación único. No había una buena manera para exigir esto.

Se han quitado en 2.0, estas dos propiedades como propiedades en cada `AuthenticationOptions` instancia. Se pueden configurar en el `AddAuthentication` llamada al método dentro de la `ConfigureServices` método *Startup.cs*:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme);
```

En el fragmento de código anterior, el esquema predeterminado se establece en `CookieAuthenticationDefaults.AuthenticationScheme` ("Cookies").

Como alternativa, use una versión sobrecargada de la `AddAuthentication` método para establecer más de una propiedad. En el siguiente ejemplo de método sobrecargado, el esquema predeterminado se establece en `CookieAuthenticationDefaults.AuthenticationScheme`. También se puede especificar el esquema de autenticación dentro de la persona `[Authorize]` atributos o las directivas de autorización.

```csharp
services.AddAuthentication(options =>
{
    options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
});
```

Definir un esquema predeterminado de 2.0 si se cumple una de las condiciones siguientes:
- Desea que el usuario para iniciar sesión automáticamente
- Usa el `[Authorize]` las directivas de autorización o el atributo sin especificar esquemas

Una excepción a esta regla es la `AddIdentity` método. Este método agrega las cookies para usted y establece el valor predeterminado autenticarse y Desafíe a esquemas para la cookie de aplicación `IdentityConstants.ApplicationScheme`. Además, Establece el esquema predeterminado de inicio de sesión a la cookie externa `IdentityConstants.ExternalScheme`.

<a name="obsolete-interface"></a>

## <a name="use-httpcontext-authentication-extensions"></a>Usar las extensiones de autenticación de HttpContext

El `IAuthenticationManager` interfaz es el punto de entrada principal en el sistema de autenticación 1.x. Se ha reemplazado por un nuevo conjunto de `HttpContext` métodos de extensión en el `Microsoft.AspNetCore.Authentication` espacio de nombres.

Por ejemplo, los proyectos de 1.x referencia un `Authentication` propiedad:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

En los 2.0 proyectos, importe el `Microsoft.AspNetCore.Authentication` espacio de nombres y elimine el `Authentication` referencias de propiedad:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="windows-auth-changes"></a>

## <a name="windows-authentication-httpsys--iisintegration"></a>Autenticación de Windows (HTTP.sys / IISIntegration)

Hay dos variaciones de autenticación de Windows:
1. El host sólo permite que los usuarios autenticados
2. El host permite tanto anónimos y los usuarios autenticados

La primera variación que se ha descrito anteriormente se ve afectada por los cambios de 2.0.

La segunda variación que se ha descrito anteriormente se ve afectada por los cambios de 2.0. Por ejemplo, puede permitir a los usuarios anónimos en su aplicación en IIS o [HTTP.sys](xref:fundamentals/servers/httpsys) pero autorizando a los usuarios en el nivel de controlador de capas. En este escenario, establezca el esquema predeterminado en `IISDefaults.AuthenticationScheme` en el `Startup.ConfigureServices` método:

```csharp
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

En consecuencia, no se pudo establecer el esquema predeterminado impide que la solicitud authorize al desafío de trabajo.

<a name="identity-cookie-options"></a>

## <a name="identitycookieoptions-instances"></a>Instancias de IdentityCookieOptions

Un efecto secundario de los 2.0 cambios es el cambio a denominado opciones en lugar de las instancias de las opciones de cookie. Se quita la capacidad para personalizar los nombres de esquema de cookie de identidad.

Por ejemplo, use los proyectos de 1.x [inserción del constructor](xref:mvc/controllers/dependency-injection#constructor-injection) para pasar un `IdentityCookieOptions` parámetro en *AccountController.cs*. El esquema de autenticación externo cookie se obtiene acceso desde la instancia proporcionada:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor&highlight=4,11)]

La inyección de constructor mencionados anteriormente se convierte en innecesaria en los 2.0 proyectos y el `_externalCookieScheme` se puede eliminar el campo:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor)]

El `IdentityConstants.ExternalScheme` constante se puede usar directamente:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="navigation-properties"></a>

## <a name="add-identityuser-poco-navigation-properties"></a>Agregar propiedades de navegación IdentityUser POCO

Las propiedades de la base de navegación de Entity Framework (EF) Core `IdentityUser` POCO (objeto CRL estándar) se han quitado. Si el proyecto de 1.x usa estas propiedades, agregarlos manualmente al proyecto 2.0:

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

Para evitar duplicadas claves externas al ejecutar migraciones de EF Core, agregue lo siguiente a su `IdentityDbContext` clase `OnModelCreating` método (después de que el `base.OnModelCreating();` llamar):

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

## <a name="replace-getexternalauthenticationschemes"></a>Reemplace GetExternalAuthenticationSchemes

El método sincrónico `GetExternalAuthenticationSchemes` quitó una versión asincrónica. los proyectos de 1.x tienen el siguiente código *ManageController.cs*:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemes)]

Este método aparece en *Login.cshtml* demasiado:

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Account/Login.cshtml?range=62,75-84)]

En los 2.0 proyectos, utilice el `GetExternalAuthenticationSchemesAsync` método:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemesAsync)]

En *Login.cshtml*, `AuthenticationScheme` propiedad accede en la `foreach` bucle cambia a `Name`:

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Views/Account/Login.cshtml?range=62,75-84)]

<a name="property-change"></a>

## <a name="manageloginsviewmodel-property-change"></a>Cambio de propiedad ManageLoginsViewModel

Un `ManageLoginsViewModel` objeto se usa en el `ManageLogins` acción de *ManageController.cs*. En de los proyectos de 1.x, el objeto `OtherLogins` propiedad de valor devuelto es de tipo `IList<AuthenticationDescription>`. Este tipo de valor devuelto requiere una importación de `Microsoft.AspNetCore.Http.Authentication`:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

En los 2.0 proyectos, se cambia el tipo de valor devuelto a `IList<AuthenticationScheme>`. Este nuevo tipo de valor devuelto es necesario sustituir la `Microsoft.AspNetCore.Http.Authentication` importar con un `Microsoft.AspNetCore.Authentication` importar.

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<a name="additional-resources"></a>

## <a name="additional-resources"></a>Recursos adicionales

Para obtener más detalles y explicación, consulte el [discusión Auth 2.0](https://github.com/aspnet/Security/issues/1338) problema en GitHub.
