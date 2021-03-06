---
title: Multi-factor Authentication en ASP.NET Core
author: damienbod
description: Aprenda a configurar multi-factor Authentication (MFA) en una aplicación ASP.NET Core.
monikerRange: '>= aspnetcore-3.1'
ms.author: rick-anderson
ms.custom: mvc
ms.date: 03/17/2020
no-loc:
- Identity
uid: security/authentication/mfa
ms.openlocfilehash: 6220688d53f0718ca5be5f63dd5d9539d37e2391
ms.sourcegitcommit: d64ef143c64ee4fdade8f9ea0b753b16752c5998
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2020
ms.locfileid: "79520150"
---
# <a name="multi-factor-authentication-in-aspnet-core"></a>Multi-factor Authentication en ASP.NET Core

Por [Damien Bowden](https://github.com/damienbod)

Multi-factor Authentication (MFA) es un proceso en el que se solicita un usuario durante un evento de inicio de sesión para otras formas de identificación. Este mensaje puede indicar un código de un teléfono móvil, usar una clave FIDO2 o proporcionar un análisis de huellas digitales. Cuando se requiere una segunda forma de autenticación, se mejora la seguridad. Un atacante no obtiene ni duplica fácilmente el factor adicional.

En este artículo se tratan las siguientes áreas:

* ¿Qué es MFA y qué flujos de MFA se recomiendan?
* Configurar MFA para páginas de administración mediante ASP.NET Core Identity
* Envío del requisito de inicio de sesión de MFA al servidor OpenID Connect
* Forzar ASP.NET Core cliente de OpenID Connect para que requiera MFA

## <a name="mfa-2fa"></a>MFA, 2FA

MFA requiere al menos dos o más tipos de pruebas para una identidad como algo que conoce, algo que posee o validación biométrica para que el usuario se autentique.

La autenticación en dos fases (2FA) es como un subconjunto de MFA, pero la diferencia es que MFA puede requerir dos o más factores para demostrar la identidad.

### <a name="mfa-totp-time-based-one-time-password-algorithm"></a>TOTP de MFA (algoritmo de contraseña de un solo tiempo basado en el tiempo)

MFA con TOTP es una implementación compatible mediante Identityde ASP.NET Core. Se puede usar junto con cualquier aplicación de autenticador compatible, lo que incluye:

* Aplicación Microsoft Authenticator
* Aplicación de Google Authenticator

Vea el siguiente vínculo para obtener información detallada sobre la implementación:

[Habilitar la generación de código QR para las aplicaciones de TOTP Authenticator en ASP.NET Core](xref:security/authentication/identity-enable-qrcodes)

### <a name="mfa-fido2-or-passwordless"></a>FIDO2 de MFA o no tiene contraseña

FIDO2 es actualmente:

* La forma más segura de lograr MFA.
* El único flujo de MFA que protege contra ataques de suplantación de identidad (phishing).

En la actualidad, ASP.NET Core no es compatible directamente con FIDO2. FIDO2 se puede usar para MFA o flujos sin contraseña.

Azure Active Directory proporciona compatibilidad con FIDO2 y flujos con contraseña. Para obtener más información, consulte [Opciones de autenticación con contraseñas para Azure Active Directory](/azure/active-directory/authentication/concept-authentication-passwordless).

### <a name="mfa-sms"></a>SMS DE MFA

MFA con SMS aumenta la seguridad de forma masiva en comparación con la autenticación de contraseña (factor único). Sin embargo, ya no se recomienda usar SMS como segundo factor. Existen demasiados vectores de ataque conocidos para este tipo de implementación.

[Directrices para NIST](https://pages.nist.gov/800-63-3/sp800-63b.html)

## <a name="configure-mfa-for-administration-pages-using-aspnet-core-opno-locidentity"></a>Configurar MFA para páginas de administración mediante ASP.NET Core Identity

MFA podría verse obligado a los usuarios a acceder a páginas confidenciales dentro de una aplicación de Identity de ASP.NET Core. Esto puede ser útil para las aplicaciones en las que existen distintos niveles de acceso para las distintas identidades. Por ejemplo, es posible que los usuarios puedan ver los datos del perfil mediante un inicio de sesión de contraseña, pero es necesario que un administrador use MFA para tener acceso a las páginas administrativas.

### <a name="extend-the-login-with-an-mfa-claim"></a>Extensión del inicio de sesión con una notificaciones de MFA

El código de demostración se configura mediante ASP.NET Core con Identity y Razor Pages. El método `AddIdentity` se usa en lugar de `AddDefaultIdentity` uno, por lo que se puede usar una implementación de `IUserClaimsPrincipalFactory` para agregar notificaciones a la identidad después de un inicio de sesión correcto.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlite(
            Configuration.GetConnectionString("DefaultConnection")));
    
    services.AddIdentity<IdentityUser, IdentityRole>(
            options => options.SignIn.RequireConfirmedAccount = false)
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

    services.AddSingleton<IEmailSender, EmailSender>();
    services.AddScoped<IUserClaimsPrincipalFactory<IdentityUser>, 
        AdditionalUserClaimsPrincipalFactory>();

    services.AddAuthorization(options =>
        options.AddPolicy("TwoFactorEnabled",
            x => x.RequireClaim("amr", "mfa")));

    services.AddRazorPages();
}
```

La clase `AdditionalUserClaimsPrincipalFactory` agrega la notificación de `amr` a las notificaciones de usuario solo después de un inicio de sesión correcto. El valor de la demanda se lee de la base de datos. La demanda se agrega aquí porque el usuario solo debe tener acceso a la vista protegida más alta si la identidad ha iniciado sesión con MFA. Si la vista de base de datos se lee directamente en la base de datos en lugar de usar la notificaciones, es posible acceder a la vista sin MFA directamente después de activar la MFA.

```csharp
using Microsoft.AspNetCore.Identity;
using Microsoft.Extensions.Options;
using System.Collections.Generic;
using System.Security.Claims;
using System.Threading.Tasks;

namespace IdentityStandaloneMfa
{
    public class AdditionalUserClaimsPrincipalFactory : 
        UserClaimsPrincipalFactory<IdentityUser, IdentityRole>
    {
        public AdditionalUserClaimsPrincipalFactory( 
            UserManager<IdentityUser> userManager,
            RoleManager<IdentityRole> roleManager, 
            IOptions<IdentityOptions> optionsAccessor) 
            : base(userManager, roleManager, optionsAccessor)
        {
        }

        public async override Task<ClaimsPrincipal> CreateAsync(IdentityUser user)
        {
            var principal = await base.CreateAsync(user);
            var identity = (ClaimsIdentity)principal.Identity;

            var claims = new List<Claim>();

            if (user.TwoFactorEnabled)
            {
                claims.Add(new Claim("amr", "mfa"));
            }
            else
            {
                claims.Add(new Claim("amr", "pwd"));
            }

            identity.AddClaims(claims);
            return principal;
        }
    }
}
```

Dado que la configuración del servicio Identity ha cambiado en la clase `Startup`, es necesario actualizar los diseños de la Identity. Scaffolding el Identity páginas en la aplicación. Defina el diseño en el archivo *Identity/Account/Manage/_Layout. cshtml* .

```cshtml
@{
    Layout = "/Pages/Shared/_Layout.cshtml";
}
```

Asigne también el diseño de todas las páginas de administración de las páginas Identity:

```cshtml
@{
    Layout = "_Layout.cshtml";
}
```

### <a name="validate-the-mfa-requirement-in-the-administration-page"></a>Validar el requisito de MFA en la página de administración

La página de Razor de administración valida que el usuario ha iniciado sesión con MFA. En el método `OnGet`, la identidad se utiliza para tener acceso a las notificaciones del usuario. Se comprueba la `amr` notificaciones para el valor `mfa`. Si la identidad no tiene esta demanda o está `false`, la página redirige a la página habilitar MFA. Esto es posible porque el usuario ya ha iniciado sesión, pero sin MFA.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc;
using Microsoft.AspNetCore.Mvc.RazorPages;

namespace IdentityStandaloneMfa
{
    public class AdminModel : PageModel
    {
        public IActionResult OnGet()
        {
            var claimTwoFactorEnabled = 
                User.Claims.FirstOrDefault(t => t.Type == "amr");

            if (claimTwoFactorEnabled != null && 
                "mfa".Equals(claimTwoFactorEnabled.Value))
            {
                // You logged in with MFA, do the administrative stuff
            }
            else
            {
                return Redirect(
                    "/Identity/Account/Manage/TwoFactorAuthentication");
            }

            return Page();
        }
    }
}
```

### <a name="ui-logic-to-toggle-user-login-information"></a>Lógica de la interfaz de usuario para alternar la información de inicio de sesión

En el inicio se ha agregado una directiva de autorización. La Directiva requiere la `amr` claim con el valor `mfa`.

```csharp
services.AddAuthorization(options =>
    options.AddPolicy("TwoFactorEnabled",
        x => x.RequireClaim("amr", "mfa")));
```

Esta Directiva se puede usar en la vista de `_Layout` para mostrar u ocultar el menú de **Administración** con la ADVERTENCIA:

```cshtml
@using Microsoft.AspNetCore.Authorization
@using Microsoft.AspNetCore.Identity
@inject SignInManager<IdentityUser> SignInManager
@inject UserManager<IdentityUser> UserManager
@inject IAuthorizationService AuthorizationService
```

Si la identidad ha iniciado sesión con MFA, el menú de **Administración** se muestra sin la advertencia de la información sobre herramientas. Cuando el usuario ha iniciado sesión sin MFA, se muestra el menú **Administrador (no habilitado)** junto con la información sobre herramientas que informa al usuario (explicando la advertencia).

```cshtml
@if (SignInManager.IsSignedIn(User))
{
    @if ((AuthorizationService.AuthorizeAsync(User, "TwoFactorEnabled")).Result.Succeeded)
    {
        <li class="nav-item">
            <a class="nav-link text-dark" asp-area="" asp-page="/Admin">Admin</a>
        </li>
    }
    else
    {
        <li class="nav-item">
            <a class="nav-link text-dark" asp-area="" asp-page="/Admin" 
               id="tooltip-demo"  
               data-toggle="tooltip" 
               data-placement="bottom" 
               title="MFA is NOT enabled. This is required for the Admin Page. If you have activated MFA, then logout, login again.">
                Admin (Not Enabled)
            </a>
        </li>
    }
}
```

Si el usuario inicia sesión sin MFA, se muestra la ADVERTENCIA:

![Autenticación MFA de administrador](mfa/_static/identitystandalonemfa_01.png)

Al hacer clic en el vínculo de **Administrador** , se redirige al usuario a la vista de habilitación de MFA:

![El administrador activa la autenticación MFA](mfa/_static/identitystandalonemfa_02.png)

## <a name="send-mfa-sign-in-requirement-to-openid-connect-server"></a>Envío del requisito de inicio de sesión de MFA al servidor OpenID Connect 

El parámetro `acr_values` se puede usar para pasar el `mfa` valor necesario desde el cliente al servidor en una solicitud de autenticación.

> [!NOTE]
> El parámetro `acr_values` debe administrarse en el servidor de Open ID Connect para que funcione.

### <a name="openid-connect-aspnet-core-client"></a>Cliente de ASP.NET Core OpenID Connect

El ASP.NET Core Razor Pages aplicación cliente de Open ID Connect usa el método `AddOpenIdConnect` para iniciar sesión en el servidor de Open ID Connect. El parámetro `acr_values` se establece con el valor `mfa` y se envía con la solicitud de autenticación. El `OpenIdConnectEvents` se utiliza para agregar este.

Para obtener los valores de parámetro `acr_values` recomendados, consulte [valores de referencia del método de autenticación](https://tools.ietf.org/html/draft-ietf-oauth-amr-values-08).

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddAuthentication(options =>
    {
        options.DefaultScheme =
            CookieAuthenticationDefaults.AuthenticationScheme;
        options.DefaultChallengeScheme =
            OpenIdConnectDefaults.AuthenticationScheme;
    })
    .AddCookie()
    .AddOpenIdConnect(options =>
    {
        options.SignInScheme =
            CookieAuthenticationDefaults.AuthenticationScheme;
        options.Authority = "<OpenID Connect server URL>";
        options.RequireHttpsMetadata = true;
        options.ClientId = "<OpenID Connect client ID>";
        options.ClientSecret = "<>";
        // Code with PKCE can also be used here
        options.ResponseType = "code id_token";
        options.Scope.Add("profile");
        options.Scope.Add("offline_access");
        options.SaveTokens = true;
        options.Events = new OpenIdConnectEvents
        {
            OnRedirectToIdentityProvider = context =>
            {
                context.ProtocolMessage.SetParameter("acr_values", "mfa");
                return Task.FromResult(0);
            }
        };
    });
```

### <a name="example-openid-connect-identityserver-4-server-with-aspnet-core-opno-locidentity"></a>Ejemplo de OpenID Connect IdentityServer 4 Server con ASP.NET Core Identity

En el servidor de OpenID Connect, que se implementa mediante ASP.NET Core Identity con las vistas de MVC, se crea una nueva vista denominada *ErrorEnable2FA. cshtml* . La vista:

* Muestra si el Identity procede de una aplicación que requiere MFA, pero el usuario no lo activó en Identity.
* Informa al usuario y agrega un vínculo para activarlo.

```cshtml
@{
    ViewData["Title"] = "ErrorEnable2FA";
}

<h1>The client application requires you to have MFA enabled. Enable this, try login again.</h1>

<br />

You can enable MFA to login here:

<br />

<a asp-controller="Manage" asp-action="TwoFactorAuthentication">Enable MFA</a>
```

En el método `Login`, se usa el `_interaction` de implementación de la interfaz de `IIdentityServerInteractionService` para tener acceso a los parámetros de solicitud de Open ID Connect. Se tiene acceso al parámetro `acr_values` mediante la propiedad `AcrValues`. A medida que el cliente lo envió con `mfa` conjunto, esto se puede comprobar.

Si se requiere MFA y el usuario de ASP.NET Core Identity tiene habilitado MFA, el inicio de sesión continúa. Cuando el usuario no tiene MFA habilitado, el usuario se redirige a la vista personalizada *ErrorEnable2FA. cshtml*. A continuación, ASP.NET Core Identity inicia la sesión del usuario.

```csharp
//
// POST: /Account/Login
[HttpPost]
[AllowAnonymous]
[ValidateAntiForgeryToken]
public async Task<IActionResult> Login(LoginInputModel model)
{
    var returnUrl = model.ReturnUrl;
    var context = 
        await _interaction.GetAuthorizationContextAsync(returnUrl);
    var requires2Fa = 
        context?.AcrValues.Count(t => t.Contains("mfa")) >= 1;

    var user = await _userManager.FindByNameAsync(model.Email);
    if (user != null && !user.TwoFactorEnabled && requires2Fa)
    {
        return RedirectToAction(nameof(ErrorEnable2FA));
    }

    // code omitted for brevity
```

El método `ExternalLoginCallback` funciona como el inicio de sesión de Identity local. Se comprueba la propiedad `AcrValues` para el valor `mfa`. Si el valor `mfa` está presente, MFA se fuerza antes de que se complete el inicio de sesión (por ejemplo, redirigido a la vista de `ErrorEnable2FA`).

```csharp
//
// GET: /Account/ExternalLoginCallback
[HttpGet]
[AllowAnonymous]
public async Task<IActionResult> ExternalLoginCallback(
    string returnUrl = null,
    string remoteError = null)
{
    var context =
        await _interaction.GetAuthorizationContextAsync(returnUrl);
    var requires2Fa =
        context?.AcrValues.Count(t => t.Contains("mfa")) >= 1;

    if (remoteError != null)
    {
        ModelState.AddModelError(
            string.Empty,
            _sharedLocalizer["EXTERNAL_PROVIDER_ERROR", 
            remoteError]);
        return View(nameof(Login));
    }
    var info = await _signInManager.GetExternalLoginInfoAsync();

    if (info == null)
    {
        return RedirectToAction(nameof(Login));
    }

    var email = info.Principal.FindFirstValue(ClaimTypes.Email);

    if (!string.IsNullOrEmpty(email))
    {
        var user = await _userManager.FindByNameAsync(email);
        if (user != null && !user.TwoFactorEnabled && requires2Fa)
        {
            return RedirectToAction(nameof(ErrorEnable2FA));
        }
    }

    // Sign in the user with this external login provider if the user already has a login.
    var result = await _signInManager
        .ExternalLoginSignInAsync(
            info.LoginProvider, 
            info.ProviderKey, 
            isPersistent: 
            false);

    // code omitted for brevity
```

Si el usuario ya ha iniciado sesión, la aplicación cliente:

* Todavía valida la demanda de `amr`.
* Puede configurar MFA con un vínculo a la vista de Identity de ASP.NET Core.

![acr_values-1](mfa/_static/acr_values-1.png)

## <a name="force-aspnet-core-openid-connect-client-to-require-mfa"></a>Forzar ASP.NET Core cliente de OpenID Connect para que requiera MFA

Este ejemplo muestra cómo una aplicación de página de Razor ASP.NET Core, que usa OpenID Connect para iniciar sesión, puede requerir que los usuarios se autentiquen mediante MFA.

Para validar el requisito de MFA, se crea un requisito de `IAuthorizationRequirement`. Esto se agregará a las páginas con una directiva que requiera MFA.

```csharp
using Microsoft.AspNetCore.Authorization;
 
namespace AspNetCoreRequireMfaOidc
{
    public class RequireMfa : IAuthorizationRequirement{}
}
```

Se implementa una `AuthorizationHandler` que utilizará la `amr` notificaciones y comprobará el valor `mfa`. El `amr` se devuelve en el `id_token` de una autenticación correcta y puede tener muchos valores distintos según se define en la especificación de [los valores de referencia del método de autenticación](https://tools.ietf.org/html/draft-ietf-oauth-amr-values-08) .

El valor devuelto depende de cómo se autentique la identidad y en la implementación del servidor de Open ID Connect.

El `AuthorizationHandler` usa el requisito de `RequireMfa` y valida la solicitud de `amr`. El servidor OpenID Connect se puede implementar mediante IdentityServer4 con ASP.NET Core Identity. Cuando un usuario inicia sesión con TOTP, se devuelve la demanda de `amr` con un valor de MFA. Si usa una implementación de servidor OpenID Connect diferente o un tipo de MFA diferente, la `amr` demanda tendrá o puede tener un valor diferente. El código debe extenderse para que lo acepte también.

```csharp
using Microsoft.AspNetCore.Authorization;
using System;
using System.Linq;
using System.Threading.Tasks;

namespace AspNetCoreRequireMfaOidc
{
    public class RequireMfaHandler : AuthorizationHandler<RequireMfa>
    {
        protected override Task HandleRequirementAsync(
            AuthorizationHandlerContext context, 
            RequireMfa requirement)
        {
            if (context == null)
                throw new ArgumentNullException(nameof(context));
            if (requirement == null)
                throw new ArgumentNullException(nameof(requirement));

            var amrClaim =
                context.User.Claims.FirstOrDefault(t => t.Type == "amr");

            if (amrClaim != null && amrClaim.Value == Amr.Mfa)
            {
                context.Succeed(requirement);
            }

            return Task.CompletedTask;
        }
    }
}
```

En el método `Startup.ConfigureServices`, se usa el método `AddOpenIdConnect` como esquema de desafío predeterminado. El controlador de autorización, que se usa para comprobar la `amr` Claim, se agrega al contenedor inversion of control. A continuación, se crea una directiva que agrega el requisito `RequireMfa`.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.ConfigureApplicationCookie(options =>
        options.Cookie.SecurePolicy =
            CookieSecurePolicy.Always);

    services.AddSingleton<IAuthorizationHandler, RequireMfaHandler>();

    services.AddAuthentication(options =>
    {
        options.DefaultScheme =
            CookieAuthenticationDefaults.AuthenticationScheme;
        options.DefaultChallengeScheme =
            OpenIdConnectDefaults.AuthenticationScheme;
    })
    .AddCookie()
    .AddOpenIdConnect(options =>
    {
        options.SignInScheme =
            CookieAuthenticationDefaults.AuthenticationScheme;
        options.Authority = "https://localhost:44352";
        options.RequireHttpsMetadata = true;
        options.ClientId = "AspNetCoreRequireMfaOidc";
        options.ClientSecret = "AspNetCoreRequireMfaOidcSecret";
        options.ResponseType = "code id_token";
        options.Scope.Add("profile");
        options.Scope.Add("offline_access");
        options.SaveTokens = true;
    });

    services.AddAuthorization(options =>
    {
        options.AddPolicy("RequireMfa", policyIsAdminRequirement =>
        {
            policyIsAdminRequirement.Requirements.Add(new RequireMfa());
        });
    });

    services.AddRazorPages();
}
```

Esta Directiva se usa después en la página de Razor según sea necesario. También se puede Agregar la Directiva globalmente para toda la aplicación.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Authorization;
using Microsoft.AspNetCore.Mvc;
using Microsoft.AspNetCore.Mvc.RazorPages;
using Microsoft.Extensions.Logging;

namespace AspNetCoreRequireMfaOidc.Pages
{
    [Authorize(Policy= "RequireMfa")]
    public class IndexModel : PageModel
    {
        private readonly ILogger<IndexModel> _logger;

        public IndexModel(ILogger<IndexModel> logger)
        {
            _logger = logger;
        }

        public void OnGet()
        {
        }
    }
}
```

Si el usuario se autentica sin MFA, es probable que la demanda de `amr` tenga un valor `pwd`. La solicitud no estará autorizada para tener acceso a la página. Con los valores predeterminados, se redirigirá al usuario a la página *account/AccessDenied* . Este comportamiento se puede cambiar o puede implementar su propia lógica personalizada aquí. En este ejemplo, se agrega un vínculo para que el usuario válido pueda configurar MFA para su cuenta.

```cshtml
@page
@model AspNetCoreRequireMfaOidc.AccessDeniedModel
@{
    ViewData["Title"] = "AccessDenied";
    Layout = "~/Pages/Shared/_Layout.cshtml";
}

<h1>AccessDenied</h1>

You require MFA to login here

<a href="https://localhost:44352/Manage/TwoFactorAuthentication">Enable MFA</a>
```

Ahora solo los usuarios que se autentican con MFA pueden tener acceso a la página o el sitio Web. Si se usan diferentes tipos de MFA o si 2FA es correcto, la demanda `amr` tendrá valores diferentes y se debe procesar correctamente. Los diferentes servidores de Open ID Connect también devuelven valores diferentes para esta demanda y pueden no seguir la especificación de [los valores de referencia del método de autenticación](https://tools.ietf.org/html/draft-ietf-oauth-amr-values-08) .

Al iniciar sesión sin MFA (por ejemplo, usando simplemente una contraseña):

* El `amr` tiene el valor `pwd`:

    ![require_mfa_oidc_02. png](mfa/_static/require_mfa_oidc_02.png)

* Acceso denegado:

    ![require_mfa_oidc_03. png](mfa/_static/require_mfa_oidc_03.png)

También puede iniciar sesión con OTP con Identity:

![require_mfa_oidc_01. png](mfa/_static/require_mfa_oidc_01.png)

## <a name="additional-resources"></a>Recursos adicionales

* [Habilitar la generación de código QR para las aplicaciones de TOTP Authenticator en ASP.NET Core](xref:security/authentication/identity-enable-qrcodes)
* [Opciones de autenticación con contraseña para Azure Active Directory](/azure/active-directory/authentication/concept-authentication-passwordless)
* [FIDO2 biblioteca .NET para FIDO2/atestación de webauthn y aserción mediante .NET](https://github.com/abergs/fido2-net-lib)
* [Webauthn maravilla](https://github.com/herrjemand/awesome-webauthn)
