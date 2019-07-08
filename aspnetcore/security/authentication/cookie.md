---
title: Usar autenticación de cookies sin ASP.NET Core Identity
author: rick-anderson
description: Obtenga información sobre cómo usar la autenticación de cookies sin ASP.NET Core Identity.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 07/07/2019
uid: security/authentication/cookie
ms.openlocfilehash: bbba2e77f806e1ed30bb734763cdbaedc1471d62
ms.sourcegitcommit: 91cc1f07ef178ab709ea42f8b3a10399c970496e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2019
ms.locfileid: "67622742"
---
# <a name="use-cookie-authentication-without-aspnet-core-identity"></a>Usar autenticación de cookies sin ASP.NET Core Identity

Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Luke Latham](https://github.com/guardrex)

ASP.NET Core Identity es un proveedor de autenticación completo y completa para crear y mantener los inicios de sesión. Sin embargo, se puede usar un proveedor de autenticación de la autenticación basada en cookies sin ASP.NET Core Identity. Para obtener más información, consulta <xref:security/authentication/identity>.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))

Para fines de demostración en la aplicación de ejemplo, la cuenta de usuario para el usuario hipotética, María Rodriguez, está codificado en la aplicación. Use la **correo electrónico** nombre de usuario `maria.rodriguez@contoso.com` y cualquier contraseña para iniciar sesión el usuario. El usuario se autentica en el `AuthenticateUser` método en el *Pages/Account/Login.cshtml.cs* archivo. En un ejemplo del mundo real, el usuario podría autenticarse en una base de datos.

## <a name="configuration"></a>Configuración

Si la aplicación no usa el [Microsoft.AspNetCore.App metapaquete](xref:fundamentals/metapackage-app), cree una referencia de paquete en el archivo de proyecto para el [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) paquete.

En el `Startup.ConfigureServices` método, cree el servicio de Middleware de autenticación con el <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> y <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*> métodos:

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet1)]

<xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme> pasa al `AddAuthentication` establece el esquema de autenticación predeterminado para la aplicación. `AuthenticationScheme` es útil cuando hay varias instancias de la autenticación con cookies y desea [autorizar con un esquema específico](xref:security/authorization/limitingidentitybyscheme). Establecer el `AuthenticationScheme` a [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) proporciona un valor de "Cookies" para el esquema. Puede proporcionar cualquier valor de cadena que distingue el esquema.

Esquema de autenticación de la aplicación es diferente de esquema de autenticación de cookies de la aplicación. Cuando no se proporciona un esquema de autenticación de cookies para <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, usa `CookieAuthenticationDefaults.AuthenticationScheme` ("Cookies").

La cookie de autenticación <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> propiedad está establecida en `true` de forma predeterminada. Cuando un visitante del sitio no ha dado su consentimiento para la recopilación de datos, se permiten las cookies de autenticación. Para obtener más información, consulta <xref:security/gdpr#essential-cookies>.

En el `Startup.Configure` método, llame a la `UseAuthentication` método para invocar el Middleware de autenticación que establece el `HttpContext.User` propiedad. Llame a la `UseAuthentication` método antes de llamar a `UseMvcWithDefaultRoute` o `UseMvc`:

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet2)]

La <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions> clase se usa para configurar las opciones de proveedor de autenticación.

Establecer `CookieAuthenticationOptions` en la configuración del servicio para la autenticación en el `Startup.ConfigureServices` método:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

## <a name="cookie-policy-middleware"></a>Middleware de la directiva de cookies.

[Middleware de cookies directiva](xref:Microsoft.AspNetCore.CookiePolicy.CookiePolicyMiddleware) habilita capacidades de directiva de cookie. Agregar el middleware a la canalización de procesamiento de la aplicación es el orden confidencial&mdash;solo afecta a los componentes de nivel inferior registrados en la canalización.

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

Use <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions> proporcionado para el Middleware de la directiva de cookies para controlar características globales del procesamiento de la cookie y el enlace en los controladores de procesamiento de la cookie cuando se agrega o se eliminan las cookies.

El valor predeterminado <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy> es el valor `SameSiteMode.Lax` para permitir la autenticación de OAuth2. Estrictamente aplicar una directiva del mismo sitio de `SameSiteMode.Strict`, establezca el `MinimumSameSitePolicy`. Aunque esta configuración la interrumpe OAuth2 y otros esquemas de autenticación de origen cruzado, eleva el nivel de seguridad de la cookie para otros tipos de aplicaciones que no dependen de procesamiento de solicitudes entre orígenes.

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

La configuración de directiva Middleware de cookies para `MinimumSameSitePolicy` pueden afectar a la configuración de `Cookie.SameSite` en `CookieAuthenticationOptions` valores según la tabla siguiente.

| MinimumSameSitePolicy | Cookie.SameSite | Configuración de Cookie.SameSite resultante |
| --------------------- | --------------- | --------------------------------- |
| SameSiteMode.None     | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict |
| SameSiteMode.Lax      | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.Lax<br>SameSiteMode.Lax<br>SameSiteMode.Strict |
| SameSiteMode.Strict   | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.Strict<br>SameSiteMode.Strict<br>SameSiteMode.Strict |

## <a name="create-an-authentication-cookie"></a>Crear una cookie de autenticación

Para crear una cookie que contiene información de usuario, construir un <xref:System.Security.Claims.ClaimsPrincipal>. La información de usuario se serializa y se almacena en la cookie. 

Crear un <xref:System.Security.Claims.ClaimsIdentity> con las <xref:System.Security.Claims.Claim>s y llame a <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*> para iniciar sesión en el usuario:

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet1)]

`SignInAsync` crea una cookie cifrada y lo agrega a la respuesta actual. Si `AuthenticationScheme` no se especifica, se usa el esquema predeterminado.

ASP.NET Core [protección de datos](xref:security/data-protection/using-data-protection) system se utiliza para el cifrado. Para una aplicación hospedada en varios equipos, equilibrio de carga entre aplicaciones, o usar una granja de servidores web, [configurar la protección de datos](xref:security/data-protection/configuration/overview) para usar el mismo conjunto de claves y el identificador de aplicación.

## <a name="sign-out"></a>Cerrar sesión

Para cerrar la sesión del usuario actual y eliminar sus cookies, llame a <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>:

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet2)]

Si `CookieAuthenticationDefaults.AuthenticationScheme` (o "Cookies") no se usa como el esquema (por ejemplo, "ContosoCookie"), proporcione el esquema usado al configurar el proveedor de autenticación. En caso contrario, se usa el esquema predeterminado.

## <a name="react-to-back-end-changes"></a>Reaccione ante los cambios de back-end

Una vez que se crea una cookie, la cookie es el único origen de identidad. Si se deshabilita una cuenta de usuario en los sistemas back-end:

* Sistema de autenticación de cookies de la aplicación continúa procesando las solicitudes basadas en la cookie de autenticación.
* El usuario permanece conectado a la aplicación siempre y cuando la cookie de autenticación es válida.

El <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents.ValidatePrincipal*> evento puede utilizarse para interceptar y reemplazar validación de la identidad de la cookie. Validando la cookie en cada solicitud, reduce el riesgo de revocados a los usuarios acceden a la aplicación.

Un enfoque de validación de la cookie se basa en mantener un seguimiento de cuando cambia la base de datos de usuario. Si la base de datos no se ha modificado desde que se emitió la cookie del usuario, no hay ninguna necesidad de volver a autenticar al usuario si la cookie sigue siendo válida. En la aplicación de ejemplo, la base de datos se implementa en `IUserRepository` y almacena una `LastChanged` valor. Cuando un usuario se actualiza en la base de datos, el `LastChanged` valor se establece en la hora actual.

Con el fin de invalidar una cookie cuando los cambios de la base de datos según la `LastChanged` valor, cree la cookie con un `LastChanged` de notificación que contiene el actual `LastChanged` valor de la base de datos:

```csharp
var claims = new List<Claim>
{
    new Claim(ClaimTypes.Name, user.Email),
    new Claim("LastChanged", {Database Value})
};

var claimsIdentity = new ClaimsIdentity(
    claims, 
    CookieAuthenticationDefaults.AuthenticationScheme);

await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme, 
    new ClaimsPrincipal(claimsIdentity));
```

Para implementar una invalidación para el `ValidatePrincipal` eventos, escribir un método con la siguiente firma en una clase que derive de <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents>:

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

El siguiente es un ejemplo de implementación de `CookieAuthenticationEvents`:

```csharp
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Authentication;
using Microsoft.AspNetCore.Authentication.Cookies;

public class CustomCookieAuthenticationEvents : CookieAuthenticationEvents
{
    private readonly IUserRepository _userRepository;

    public CustomCookieAuthenticationEvents(IUserRepository userRepository)
    {
        // Get the database from registered DI services.
        _userRepository = userRepository;
    }

    public override async Task ValidatePrincipal(CookieValidatePrincipalContext context)
    {
        var userPrincipal = context.Principal;

        // Look for the LastChanged claim.
        var lastChanged = (from c in userPrincipal.Claims
                           where c.Type == "LastChanged"
                           select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !_userRepository.ValidateLastChanged(lastChanged))
        {
            context.RejectPrincipal();

            await context.HttpContext.SignOutAsync(
                CookieAuthenticationDefaults.AuthenticationScheme);
        }
    }
}
```

Registrar la instancia de eventos durante el registro del servicio de cookie en el `Startup.ConfigureServices` método. Proporcione un [con ámbito de registro del servicio](xref:fundamentals/dependency-injection#service-lifetimes) para su `CustomCookieAuthenticationEvents` clase:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

Considere una situación en la que se actualiza el nombre del usuario&mdash;una decisión que no afectan a la seguridad de cualquier manera. Si desea actualizar sin destruir datos de la entidad de seguridad de usuario, llame a `context.ReplacePrincipal` y establezca el `context.ShouldRenew` propiedad `true`.

> [!WARNING]
> El enfoque descrito aquí se desencadena en cada solicitud. Validando las cookies de autenticación para todos los usuarios en cada solicitud puede producir una reducción del rendimiento de la aplicación.

## <a name="persistent-cookies"></a>Cookies persistentes

Desea que la cookie para conservar entre sesiones del explorador. Esta persistencia solo debería habilitarse con el consentimiento del usuario explícita con una casilla de verificación "Recordar mi cuenta" en Inicio de sesión o un mecanismo similar. 

El fragmento de código siguiente crea una identidad y cookie correspondiente que sobrevive a través de los cierres del explorador. Se respeta cualquier configuración de expiración deslizante configurado previamente. Si la cookie expira mientras se cierra el explorador, el explorador borrará la cookie de una vez que se reinicie.

Establecer <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.IsPersistent> a `true` en <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties>:

```csharp
// using Microsoft.AspNetCore.Authentication;

await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

## <a name="absolute-cookie-expiration"></a>Expiración de cookie absoluta

Se puede establecer un tiempo de expiración absoluta con <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.ExpiresUtc>. Para crear una cookie persistente, `IsPersistent` también debe establecerse. En caso contrario, la cookie se crea con una duración basados en sesión y puede expirar antes o después el vale de autenticación que contiene. Cuando `ExpiresUtc` está establecido, reemplaza el valor de la <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions.ExpireTimeSpan> opción de <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions>, si establece.

El fragmento de código siguiente crea una identidad y cookie correspondiente que tiene una duración de 20 minutos. Esto omite cualquier configuración de expiración deslizante configurado previamente.

```csharp
// using Microsoft.AspNetCore.Authentication;

await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true,
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

## <a name="additional-resources"></a>Recursos adicionales

* <xref:security/authorization/limitingidentitybyscheme>
* <xref:security/authorization/claims>
* [Comprobaciones de la función basada en directivas](xref:security/authorization/roles#policy-based-role-checks)
* <xref:host-and-deploy/web-farm>
