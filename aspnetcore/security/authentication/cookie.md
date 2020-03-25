---
title: Usar la autenticación de cookies sin ASP.NET Core identidad
author: rick-anderson
description: Aprenda a usar la autenticación de cookies sin ASP.NET Core identidad.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 02/11/2020
uid: security/authentication/cookie
ms.openlocfilehash: b7c8b2cccb27dd6818330b17439675e41bfef013
ms.sourcegitcommit: 91dc1dd3d055b4c7d7298420927b3fd161067c64
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "80219212"
---
# <a name="use-cookie-authentication-without-aspnet-core-identity"></a>Usar la autenticación de cookies sin ASP.NET Core identidad

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range=">= aspnetcore-3.0"

ASP.NET Core Identity es un completo proveedor de autenticación completo para crear y mantener inicios de sesión. Sin embargo, se puede usar un proveedor de autenticación basado en cookies sin ASP.NET Core identidad. Para más información, consulte <xref:security/authentication/identity>.

[Vea o descargue el código de ejemplo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))

Para fines de demostración en la aplicación de ejemplo, la cuenta de usuario para el usuario hipotético, María Rodriguez, está codificada en la aplicación. Use la dirección de **correo electrónico** `maria.rodriguez@contoso.com` y cualquier contraseña para iniciar sesión en el usuario. El usuario se autentica en el método `AuthenticateUser` del archivo *pages/Account/login. cshtml. CS* . En un ejemplo real, el usuario se autenticaría en una base de datos.

## <a name="configuration"></a>Configuración

Si la aplicación no usa el [metapaquete Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app), cree una referencia de paquete en el archivo de proyecto para el paquete [Microsoft. AspNetCore. Authentication. cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) .

En el método `Startup.ConfigureServices`, cree los servicios de middleware de autenticación con los métodos <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> y <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>:

[!code-csharp[](cookie/samples/3.x/CookieSample/Startup.cs?name=snippet1)]

<xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme> pasa a `AddAuthentication` establece el esquema de autenticación predeterminado para la aplicación. `AuthenticationScheme` resulta útil cuando hay varias instancias de autenticación de cookies y se desea [autorizar con un esquema específico](xref:security/authorization/limitingidentitybyscheme). Al establecer el `AuthenticationScheme` en [CookieAuthenticationDefaults. AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) , se proporciona el valor "cookies" para el esquema. Puede proporcionar cualquier valor de cadena que distinga el esquema.

El esquema de autenticación de la aplicación es diferente del esquema de autenticación de cookies de la aplicación. Cuando no se proporciona un esquema de autenticación de cookies para <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, utiliza `CookieAuthenticationDefaults.AuthenticationScheme` ("cookies").

De forma predeterminada, la propiedad <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> de la cookie de autenticación está establecida en `true`. Las cookies de autenticación se permiten cuando un visitante del sitio no ha dado su consentimiento a la recopilación de datos. Para más información, consulte <xref:security/gdpr#essential-cookies>.

En `Startup.Configure`, llame a `UseAuthentication` y `UseAuthorization` para establecer la propiedad `HttpContext.User` y ejecutar el middleware de autorización para las solicitudes. Llame a los métodos `UseAuthentication` y `UseAuthorization` antes de llamar a `UseEndpoints`:

[!code-csharp[](cookie/samples/3.x/CookieSample/Startup.cs?name=snippet2)]

La clase <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions> se usa para configurar las opciones del proveedor de autenticación.

Establezca `CookieAuthenticationOptions` en la configuración del servicio para la autenticación en el método `Startup.ConfigureServices`:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

## <a name="cookie-policy-middleware"></a>Middleware de directiva de cookies

El [middleware de directiva de cookies](xref:Microsoft.AspNetCore.CookiePolicy.CookiePolicyMiddleware) habilita las funcionalidades de directiva de cookies. Agregar el middleware a la canalización de procesamiento de aplicaciones distingue el orden de&mdash;solo afecta a los componentes de nivel inferior registrados en la canalización.

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

Utilice <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions> proporcionado al middleware de la Directiva de cookies para controlar las características globales del procesamiento de cookies y enlazar los controladores de procesamiento de cookies cuando se anexan o eliminan cookies.

El valor de <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy> predeterminado es `SameSiteMode.Lax` para permitir la autenticación de OAuth2. Para aplicar de forma estricta una directiva del mismo sitio de `SameSiteMode.Strict`, establezca el `MinimumSameSitePolicy`. Aunque esta configuración interrumpe OAuth2 y otros esquemas de autenticación entre orígenes, eleva el nivel de seguridad de las cookies para otros tipos de aplicaciones que no se basan en el procesamiento de solicitudes entre orígenes.

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

La configuración de middleware de la Directiva de cookies para `MinimumSameSitePolicy` puede afectar a la configuración de `Cookie.SameSite` en `CookieAuthenticationOptions` configuración de acuerdo con la matriz siguiente.

| MinimumSameSitePolicy | Cookie. SameSite | Valor de cookie. SameSite resultante |
| --------------------- | --------------- | --------------------------------- |
| SameSiteMode.None     | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict |
| SameSiteMode.Lax      | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.Lax<br>SameSiteMode.Lax<br>SameSiteMode.Strict |
| SameSiteMode.Strict   | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.Strict<br>SameSiteMode.Strict<br>SameSiteMode.Strict |

## <a name="create-an-authentication-cookie"></a>Crear una cookie de autenticación

Para crear una cookie que contiene información de usuario, construya un <xref:System.Security.Claims.ClaimsPrincipal>. La información del usuario se serializa y se almacena en la cookie. 

Cree un <xref:System.Security.Claims.ClaimsIdentity> con los <xref:System.Security.Claims.Claim>s necesarios y llame a <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*> para iniciar sesión en el usuario:

[!code-csharp[](cookie/samples/3.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet1)]

[!INCLUDE[request localized comments](~/includes/code-comments-loc.md)]

`SignInAsync` crea una cookie cifrada y la agrega a la respuesta actual. Si no se especifica `AuthenticationScheme`, se usa el esquema predeterminado.

El sistema de [protección de datos](xref:security/data-protection/using-data-protection) de ASP.net Core se usa para el cifrado. En el caso de una aplicación hospedada en varios equipos, equilibrio de carga entre aplicaciones o uso de una granja de servidores Web, [Configure la protección de datos](xref:security/data-protection/configuration/overview) para que use el mismo anillo de claves e identificador de la aplicación.

## <a name="sign-out"></a>Cerrar sesión

Para cerrar la sesión del usuario actual y eliminar su cookie, llame a <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>:

[!code-csharp[](cookie/samples/3.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet2)]

Si `CookieAuthenticationDefaults.AuthenticationScheme` (o "cookies") no se utiliza como esquema (por ejemplo, "ContosoCookie"), proporcione el esquema que se usa al configurar el proveedor de autenticación. De lo contrario, se usa el esquema predeterminado.

## <a name="react-to-back-end-changes"></a>Reaccionar a los cambios de back-end

Una vez creada una cookie, ésta es la única fuente de identidad. Si una cuenta de usuario está deshabilitada en los sistemas de servidor:

* El sistema de autenticación de cookies de la aplicación continúa procesando solicitudes basadas en la cookie de autenticación.
* El usuario permanecerá conectado en la aplicación siempre y cuando la cookie de autenticación sea válida.

El evento <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents.ValidatePrincipal*> se puede usar para interceptar e invalidar la validación de la identidad de la cookie. La validación de la cookie en cada solicitud mitiga el riesgo de que los usuarios revocados accedan a la aplicación.

Un enfoque para la validación de cookies se basa en realizar un seguimiento de cuándo cambia la base de datos de usuario. Si no se ha cambiado la base de datos desde que se emitió la cookie del usuario, no es necesario volver a autenticar al usuario si la cookie sigue siendo válida. En la aplicación de ejemplo, la base de datos se implementa en `IUserRepository` y almacena un valor `LastChanged`. Cuando se actualiza un usuario en la base de datos, el valor de `LastChanged` se establece en la hora actual.

Para invalidar una cookie cuando la base de datos cambie según el valor de `LastChanged`, cree la cookie con una `LastChanged` claim que contenga el valor de `LastChanged` actual de la base de datos:

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

Para implementar una invalidación para el evento `ValidatePrincipal`, escriba un método con la siguiente firma en una clase que derive de <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents>:

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

Registre la instancia de eventos durante el registro del servicio de cookies en el método `Startup.ConfigureServices`. Proporcione un [registro de servicio de ámbito](xref:fundamentals/dependency-injection#service-lifetimes) para la clase `CustomCookieAuthenticationEvents`:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

Considere una situación en la que el nombre del usuario se actualice&mdash;una decisión que no afecte a la seguridad de ningún modo. Si desea actualizar la entidad de seguridad de usuario de un no destructivo, llame a `context.ReplacePrincipal` y establezca la propiedad `context.ShouldRenew` en `true`.

> [!WARNING]
> El enfoque descrito aquí se desencadena en cada solicitud. La validación de las cookies de autenticación para todos los usuarios de cada solicitud puede producir una gran penalización del rendimiento de la aplicación.

## <a name="persistent-cookies"></a>Cookies persistentes

Puede que desee que la cookie se conserve entre sesiones del explorador. Esta persistencia solo se debe habilitar con el consentimiento explícito del usuario con una casilla "Recordarme" al iniciar sesión o un mecanismo similar. 

El siguiente fragmento de código crea una identidad y la cookie correspondiente que sobrevive a través de los cierres del explorador. Se respetan los valores de expiración que se hayan configurado previamente. Si la cookie expira mientras se cierra el explorador, el explorador borra la cookie una vez que se reinicia.

Establezca <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.IsPersistent> en `true` en <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties>:

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

## <a name="absolute-cookie-expiration"></a>Expiración absoluta de cookies

Se puede establecer una hora de expiración absoluta con <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.ExpiresUtc>. Para crear una cookie persistente, también se debe establecer `IsPersistent`. De lo contrario, la cookie se crea con una duración basada en sesión y puede expirar antes o después del vale de autenticación que contiene. Cuando se establece `ExpiresUtc`, invalida el valor de la opción <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.ExpireTimeSpan> de <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions>, si se establece.

El siguiente fragmento de código crea una identidad y la cookie correspondiente que dura 20 minutos. Esto omite cualquier configuración de expiración variable previamente configurada.

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

::: moniker-end

::: moniker range="< aspnetcore-3.0"

ASP.NET Core Identity es un completo proveedor de autenticación completo para crear y mantener inicios de sesión. Sin embargo, se puede usar un proveedor de autenticación de autenticación basada en cookies sin ASP.NET Core identidad. Para más información, consulte <xref:security/authentication/identity>.

[Vea o descargue el código de ejemplo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))

Para fines de demostración en la aplicación de ejemplo, la cuenta de usuario para el usuario hipotético, María Rodriguez, está codificada en la aplicación. Use la dirección de **correo electrónico** `maria.rodriguez@contoso.com` y cualquier contraseña para iniciar sesión en el usuario. El usuario se autentica en el método `AuthenticateUser` del archivo *pages/Account/login. cshtml. CS* . En un ejemplo real, el usuario se autenticaría en una base de datos.

## <a name="configuration"></a>Configuración

Si la aplicación no usa el [metapaquete Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app), cree una referencia de paquete en el archivo de proyecto para el paquete [Microsoft. AspNetCore. Authentication. cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) .

En el método `Startup.ConfigureServices`, cree el servicio de middleware de autenticación con los métodos <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> y <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>:

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet1)]

<xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme> pasa a `AddAuthentication` establece el esquema de autenticación predeterminado para la aplicación. `AuthenticationScheme` resulta útil cuando hay varias instancias de autenticación de cookies y se desea [autorizar con un esquema específico](xref:security/authorization/limitingidentitybyscheme). Al establecer el `AuthenticationScheme` en [CookieAuthenticationDefaults. AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) , se proporciona el valor "cookies" para el esquema. Puede proporcionar cualquier valor de cadena que distinga el esquema.

El esquema de autenticación de la aplicación es diferente del esquema de autenticación de cookies de la aplicación. Cuando no se proporciona un esquema de autenticación de cookies para <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, utiliza `CookieAuthenticationDefaults.AuthenticationScheme` ("cookies").

De forma predeterminada, la propiedad <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> de la cookie de autenticación está establecida en `true`. Las cookies de autenticación se permiten cuando un visitante del sitio no ha dado su consentimiento a la recopilación de datos. Para más información, consulte <xref:security/gdpr#essential-cookies>.

En el método `Startup.Configure`, llame al método `UseAuthentication` para invocar el middleware de autenticación que establece la propiedad `HttpContext.User`. Llame al método `UseAuthentication` antes de llamar a `UseMvcWithDefaultRoute` o `UseMvc`:

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet2)]

La clase <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions> se usa para configurar las opciones del proveedor de autenticación.

Establezca `CookieAuthenticationOptions` en la configuración del servicio para la autenticación en el método `Startup.ConfigureServices`:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

## <a name="cookie-policy-middleware"></a>Middleware de directiva de cookies

El [middleware de directiva de cookies](xref:Microsoft.AspNetCore.CookiePolicy.CookiePolicyMiddleware) habilita las funcionalidades de directiva de cookies. Agregar el middleware a la canalización de procesamiento de aplicaciones distingue el orden de&mdash;solo afecta a los componentes de nivel inferior registrados en la canalización.

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

Utilice <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions> proporcionado al middleware de la Directiva de cookies para controlar las características globales del procesamiento de cookies y enlazar los controladores de procesamiento de cookies cuando se anexan o eliminan cookies.

El valor de <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy> predeterminado es `SameSiteMode.Lax` para permitir la autenticación de OAuth2. Para aplicar de forma estricta una directiva del mismo sitio de `SameSiteMode.Strict`, establezca el `MinimumSameSitePolicy`. Aunque esta configuración interrumpe OAuth2 y otros esquemas de autenticación entre orígenes, eleva el nivel de seguridad de las cookies para otros tipos de aplicaciones que no se basan en el procesamiento de solicitudes entre orígenes.

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

La configuración de middleware de la Directiva de cookies para `MinimumSameSitePolicy` puede afectar a la configuración de `Cookie.SameSite` en `CookieAuthenticationOptions` configuración de acuerdo con la matriz siguiente.

| MinimumSameSitePolicy | Cookie. SameSite | Valor de cookie. SameSite resultante |
| --------------------- | --------------- | --------------------------------- |
| SameSiteMode.None     | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict |
| SameSiteMode.Lax      | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.Lax<br>SameSiteMode.Lax<br>SameSiteMode.Strict |
| SameSiteMode.Strict   | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.Strict<br>SameSiteMode.Strict<br>SameSiteMode.Strict |

## <a name="create-an-authentication-cookie"></a>Crear una cookie de autenticación

Para crear una cookie que contiene información de usuario, construya un <xref:System.Security.Claims.ClaimsPrincipal>. La información del usuario se serializa y se almacena en la cookie. 

Cree un <xref:System.Security.Claims.ClaimsIdentity> con los <xref:System.Security.Claims.Claim>s necesarios y llame a <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*> para iniciar sesión en el usuario:

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet1)]

`SignInAsync` crea una cookie cifrada y la agrega a la respuesta actual. Si no se especifica `AuthenticationScheme`, se usa el esquema predeterminado.

El sistema de [protección de datos](xref:security/data-protection/using-data-protection) de ASP.net Core se usa para el cifrado. En el caso de una aplicación hospedada en varios equipos, equilibrio de carga entre aplicaciones o uso de una granja de servidores Web, [Configure la protección de datos](xref:security/data-protection/configuration/overview) para que use el mismo anillo de claves e identificador de la aplicación.

## <a name="sign-out"></a>Cerrar sesión

Para cerrar la sesión del usuario actual y eliminar su cookie, llame a <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>:

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet2)]

Si `CookieAuthenticationDefaults.AuthenticationScheme` (o "cookies") no se utiliza como esquema (por ejemplo, "ContosoCookie"), proporcione el esquema que se usa al configurar el proveedor de autenticación. De lo contrario, se usa el esquema predeterminado.

## <a name="react-to-back-end-changes"></a>Reaccionar a los cambios de back-end

Una vez creada una cookie, ésta es la única fuente de identidad. Si una cuenta de usuario está deshabilitada en los sistemas de servidor:

* El sistema de autenticación de cookies de la aplicación continúa procesando solicitudes basadas en la cookie de autenticación.
* El usuario permanecerá conectado en la aplicación siempre y cuando la cookie de autenticación sea válida.

El evento <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents.ValidatePrincipal*> se puede usar para interceptar e invalidar la validación de la identidad de la cookie. La validación de la cookie en cada solicitud mitiga el riesgo de que los usuarios revocados accedan a la aplicación.

Un enfoque para la validación de cookies se basa en realizar un seguimiento de cuándo cambia la base de datos de usuario. Si no se ha cambiado la base de datos desde que se emitió la cookie del usuario, no es necesario volver a autenticar al usuario si la cookie sigue siendo válida. En la aplicación de ejemplo, la base de datos se implementa en `IUserRepository` y almacena un valor `LastChanged`. Cuando se actualiza un usuario en la base de datos, el valor de `LastChanged` se establece en la hora actual.

Para invalidar una cookie cuando la base de datos cambie según el valor de `LastChanged`, cree la cookie con una `LastChanged` claim que contenga el valor de `LastChanged` actual de la base de datos:

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

Para implementar una invalidación para el evento `ValidatePrincipal`, escriba un método con la siguiente firma en una clase que derive de <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents>:

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

Registre la instancia de eventos durante el registro del servicio de cookies en el método `Startup.ConfigureServices`. Proporcione un [registro de servicio de ámbito](xref:fundamentals/dependency-injection#service-lifetimes) para la clase `CustomCookieAuthenticationEvents`:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

Considere una situación en la que el nombre del usuario se actualice&mdash;una decisión que no afecte a la seguridad de ningún modo. Si desea actualizar la entidad de seguridad de usuario de un no destructivo, llame a `context.ReplacePrincipal` y establezca la propiedad `context.ShouldRenew` en `true`.

> [!WARNING]
> El enfoque descrito aquí se desencadena en cada solicitud. La validación de las cookies de autenticación para todos los usuarios de cada solicitud puede producir una gran penalización del rendimiento de la aplicación.

## <a name="persistent-cookies"></a>Cookies persistentes

Puede que desee que la cookie se conserve entre sesiones del explorador. Esta persistencia solo se debe habilitar con el consentimiento explícito del usuario con una casilla "Recordarme" al iniciar sesión o un mecanismo similar. 

El siguiente fragmento de código crea una identidad y la cookie correspondiente que sobrevive a través de los cierres del explorador. Se respetan los valores de expiración que se hayan configurado previamente. Si la cookie expira mientras se cierra el explorador, el explorador borra la cookie una vez que se reinicia.

Establezca <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.IsPersistent> en `true` en <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties>:

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

## <a name="absolute-cookie-expiration"></a>Expiración absoluta de cookies

Se puede establecer una hora de expiración absoluta con <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.ExpiresUtc>. Para crear una cookie persistente, también se debe establecer `IsPersistent`. De lo contrario, la cookie se crea con una duración basada en sesión y puede expirar antes o después del vale de autenticación que contiene. Cuando se establece `ExpiresUtc`, invalida el valor de la opción <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions.ExpireTimeSpan> de <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions>, si se establece.

El siguiente fragmento de código crea una identidad y la cookie correspondiente que dura 20 minutos. Esto omite cualquier configuración de expiración variable previamente configurada.

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

::: moniker-end

## <a name="additional-resources"></a>Recursos adicionales

* <xref:security/authorization/limitingidentitybyscheme>
* <xref:security/authorization/claims>
* [Comprobaciones de roles basadas en directivas](xref:security/authorization/roles#policy-based-role-checks)
* <xref:host-and-deploy/web-farm>
