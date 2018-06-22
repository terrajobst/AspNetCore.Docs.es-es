---
title: Configurar la identidad de ASP.NET Core
author: AdrienTorris
description: Comprender los valores predeterminados de ASP.NET Core Identity y obtener información sobre cómo configurar las propiedades de identidad para usar valores personalizados.
ms.author: scaddie
ms.date: 03/06/2018
uid: security/authentication/identity-configuration
ms.openlocfilehash: 914e9b22ed52b560366fdff1f2430d3dd66454c3
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276269"
---
# <a name="configure-aspnet-core-identity"></a>Configurar la identidad de ASP.NET Core

Identidad de ASP.NET Core utiliza la configuración predeterminada para la configuración como directiva de contraseñas, el tiempo de bloqueo y la configuración de cookies. Esta configuración puede invalidarse en la aplicación `Startup` clase.

## <a name="identity-options"></a>Opciones de identidad

El [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) clase representa las opciones que pueden usarse para configurar el sistema de identidades.

### <a name="claims-identity"></a>Identidad basada en notificaciones

[IdentityOptions.ClaimsIdentity](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.claimsidentity) especifica la [ClaimsIdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions) con las propiedades mostradas en la tabla.

| Property | Descripción | Default |
| -------- | ----------- | :-----: |
| [roleClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.roleclaimtype) | Obtiene o establece el tipo de notificación utilizado para una notificación de rol. | [ClaimTypes.Role](/dotnet/api/system.security.claims.claimtypes.role) |
| [SecurityStampClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.securitystampclaimtype) | Obtiene o establece el tipo de notificación utilizado para la notificación de marca de seguridad. | `AspNet.Identity.SecurityStamp` |
| [UserIdClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.useridclaimtype) | Obtiene o establece el tipo de notificación utilizado para la notificación de identificador de usuario. | [ClaimTypes.NameIdentifier](/dotnet/api/system.security.claims.claimtypes.nameidentifier) |
| [UserNameClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.usernameclaimtype) | Obtiene o establece el tipo de notificación utilizado para la notificación de nombre de usuario. | [ClaimTypes.Name](/dotnet/api/system.security.claims.claimtypes.name) |

### <a name="lockout"></a>Bloqueo

Impide al usuario durante un período de tiempo después de un determinado número de intentos de acceso erróneos (valor predeterminado: bloqueo de 5 minutos después de 5 intentos de acceso). Una autenticación correcta restablece el número de intentos de acceso erróneos y restablece el reloj.

En el ejemplo siguiente se muestra los valores predeterminados:

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,39-42,50-52)]

Confirme que [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync) establece `lockoutOnFailure` a `true`:

```csharp
var result = await _signInManager.PasswordSignInAsync(
                 Input.Email, Input.Password, Input.RememberMe, lockoutOnFailure: true);
```

[IdentityOptions.Lockout](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.lockout) especifica la [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) con las propiedades mostradas en la tabla.

| Property | Descripción | Default |
| -------- | ----------- | :-----: |
| [AllowedForNewUsers](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.allowedfornewusers) | Determina si un usuario nuevo puede bloquearse. | `true` |
| [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan) | La cantidad de tiempo que un usuario está bloqueado cuando se produce un bloqueo. | 5 minutos |
| [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) | El número de intentos de acceso erróneos hasta que se bloquea un usuario, si está habilitado el bloqueo. | 5 |

### <a name="password"></a>Contraseña

De forma predeterminada, identidad requiere que las contraseñas contengan un carácter en mayúsculas, caracteres en minúsculas, un dígito y un carácter no alfanumérico. Las contraseñas deben tener al menos seis caracteres. [Opciones de contraseña](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) puede cambiarse en `Startup.ConfigureServices`.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

Núcleo de ASP.NET 2.0 agregado la [RequiredUniqueChars](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireduniquechars) propiedad. De lo contrario, las opciones son los mismos que ASP.NET Core 1.x.

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

---

[IdentityOptions.Password](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.password) especifica la [opciones de contraseña](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) con las propiedades mostradas en la tabla.

| Property | Descripción | Default |
| -------- | ----------- | :-----: |
| [RequireDigit](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredigit) | Requiere un número entre 0-9 en la contraseña. | `true` |
| [RequiredLength](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredlength) | La longitud mínima de la contraseña. | 6 |
| [RequiredUniqueChars](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireduniquechars) | Solo se aplica a ASP.NET Core 2.0 o posterior.<br><br> Requiere el número de caracteres distintos de la contraseña. | 1 |
| [RequireLowercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirelowercase) | Requiere un carácter en minúscula en la contraseña. | `true` |
| [RequireNonAlphanumeric](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirenonalphanumeric) | Requiere un carácter que no sean alfanuméricos en la contraseña. | `true` |
| [RequireUppercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireuppercase) | Requiere un carácter en mayúsculas en la contraseña. | `true` |

### <a name="sign-in"></a>inicio de sesión

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)]

[IdentityOptions.SignIn](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.signin) especifica la [SignInOptions](/dotnet/api/microsoft.aspnetcore.identity.signinoptions) con las propiedades mostradas en la tabla.

| Property | Descripción | Default |
| -------- | ----------- | :-----: |
| [RequireConfirmedEmail](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedemail) | Requiere un correo electrónico confirmado para iniciar sesión en. | `false` |
| [RequireConfirmedPhoneNumber](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedphonenumber) | Requiere un número de teléfono confirmada iniciar sesión en. | `false` |

### <a name="tokens"></a>tokens

[IdentityOptions.Tokens](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.tokens) especifica la [TokenOptions](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions) con las propiedades mostradas en la tabla.


|                                                        Property                                                         |                                                                                      Descripción                                                                                      |
|-------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     [AuthenticatorTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.authenticatortokenprovider)     |                                       Obtiene o establece el `AuthenticatorTokenProvider` utilizado para validar los inicios de sesión de dos fases con un autenticador.                                       |
|       [ChangeEmailTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changeemailtokenprovider)       |                                     Obtiene o establece el `ChangeEmailTokenProvider` usado para generar tokens que se usan en correo electrónico de confirmación del cambio de correo electrónico.                                     |
| [ChangePhoneNumberTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changephonenumbertokenprovider) |                                      Obtiene o establece el `ChangePhoneNumberTokenProvider` usado para generar tokens que se usan al cambiar los números de teléfono.                                      |
| [EmailConfirmationTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.emailconfirmationtokenprovider) |                                             Obtiene o establece el proveedor de tokens que se usa para generar tokens que se usan en los correos electrónicos de confirmación de cuenta.                                              |
|     [PasswordResetTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.passwordresettokenprovider)     | Obtiene o establece la [IUserTwoFactorTokenProvider<TUser> ](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactortokenprovider-1) usado para generar tokens que se usan en los correos electrónicos de restablecimiento de contraseña. |
|                    [ProviderMap](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.providermap)                    |                Utilizado para construir un [proveedor de tokens de usuario](/dotnet/api/microsoft.aspnetcore.identity.tokenproviderdescriptor) con la clave que se usa como el nombre del proveedor.                 |

### <a name="user"></a>Usuario

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,48-52)]

[IdentityOptions.User](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.user) especifica la [UserOptions](/dotnet/api/microsoft.aspnetcore.identity.useroptions) con las propiedades mostradas en la tabla.

| Property | Descripción | Default |
| -------- | ----------- | :-----: |
| [AllowedUserNameCharacters](/dotnet/api/microsoft.aspnetcore.identity.useroptions.allowedusernamecharacters) | Caracteres permitidos en el nombre de usuario. | abcdefghijklmnopqrstuvwxyz<br>ABCDEFGHIJKLMNOPQRSTUVWXYZ<br>0123456789<br>-._@+ |
| [RequireUniqueEmail](/dotnet/api/microsoft.aspnetcore.identity.useroptions.requireuniqueemail) | Requiere que cada usuario tiene un correo electrónico único. | `false` |

## <a name="cookie-settings"></a>Configuración de cookies

Configurar la cookie de la aplicación en `Startup.ConfigureServices`:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

---

[CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions) tiene las siguientes propiedades:

|                                                               Property                                                               |                                                                                                                                                           Descripción                                                                                                                                                            |
|--------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath)       |                                                                 Informa al controlador que debe cambiar en una salida <em>403 Prohibido</em> código de estado en un <em>redireccionamiento 302</em> en la ruta de acceso especificada.<br><br>El valor predeterminado es `/Account/AccessDenied`.                                                                  |
|             [AuthenticationScheme](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.authenticationscheme)              |                                                                                                                Solo se aplica a ASP.NET Core 1.x.<br><br> El nombre lógico para un esquema de autenticación concreto.                                                                                                                |
|            [AutomaticAuthenticate](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticauthenticate)             |                                                                       Solo se aplica a ASP.NET Core 1.x.<br><br> Cuando sea true, autenticación con cookies debe ejecutar en cada solicitud y se intentan validar y reconstruir cualquier entidad de seguridad serializado que creó.                                                                        |
|               [AutomaticChallenge](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticchallenge)                |                                              Solo se aplica a ASP.NET Core 1.x.<br><br> Si es true, el middleware de autenticación controla desafíos automática. Si false, el middleware de autenticación sólo altera las respuestas cuando indique explícitamente la `AuthenticationScheme`.                                               |
|               [ClaimsIssuer](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.claimsissuer)               |                                                             Obtiene o establece el emisor que se debe usar para las notificaciones que se crean (se hereda de [AuthenticationSchemeOptions](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions)).                                                             |
|                             [Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain)                              |                                                                                                                                             Dominio que se va a asociar con la cookie.                                                                                                                                             |
|                         [Cookie.Expiration](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.expiration)                          |                 Obtiene o establece la duración de la cookie HTTP (no la cookie de autenticación). Esta propiedad se reemplaza por [ExpireTimeSpan](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.expiretimespan). Que no debe usarse en el contexto de CookieAuthentication.                  |
|                           [Cookie.HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly)                            |                                                                                                               Indica si una cookie es accesible mediante script de cliente.<br><br>El valor predeterminado es `true`.                                                                                                                |
|                               [Cookie.Name](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name)                                |                                                                                                                            El nombre de la cookie.<br><br>El valor predeterminado es `.AspNetCore.Cookies`.                                                                                                                            |
|                               [Cookie.Path](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path)                                |                                                                                                                                                         La ruta de acceso de la cookie.                                                                                                                                                         |
|                           [Cookie.SameSite](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite)                            |                                                                                           El `SameSite` atributo de la cookie.<br><br>El valor predeterminado es [SameSiteMode.Lax](/dotnet/api/microsoft.aspnetcore.http.samesitemode).                                                                                            |
|                       [Cookie.SecurePolicy](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.securepolicy)                        |                                                   El [CookieSecurePolicy](/dotnet/api/microsoft.aspnetcore.http.cookiesecurepolicy) configuración.<br><br>El valor predeterminado es [CookieSecurePolicy.SameAsRequest](/dotnet/api/microsoft.aspnetcore.http.cookiesecurepolicy).                                                    |
|                  [CookieDomain](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiedomain)                   |                                                                                                                      Solo se aplica a ASP.NET Core 1.x.<br><br> El nombre de dominio donde se sirve la cookie.                                                                                                                       |
|                [CookieHttpOnly](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiehttponly)                 |                                                                                       Solo se aplica a ASP.NET Core 1.x.<br><br> Una marca que indica si la cookie debe ser accesible sólo a los servidores.<br><br>El valor predeterminado es `true`.                                                                                        |
|                    [CookiePath](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiepath)                     |                                                                                                                  Solo se aplica a ASP.NET Core 1.x.<br><br> Se utiliza para aislar las aplicaciones que se ejecutan en el mismo nombre de host.                                                                                                                   |
|                  [CookieSecure](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiesecure)                   | Solo se aplica a ASP.NET Core 1.x.<br><br> Una marca que indica si la cookie creada debe limitarse a HTTPS (`CookieSecurePolicy.Always`), HTTP o HTTPS (`CookieSecurePolicy.None`), o el mismo protocolo que la solicitud (`CookieSecurePolicy.SameAsRequest`).<br><br>El valor predeterminado es `CookieSecurePolicy.SameAsRequest`. |
|          [CookieManager](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.cookiemanager)          |                                                                                                                         El componente utilizado para obtener las cookies de la solicitud o para establecerlas en la respuesta.                                                                                                                          |
| [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider) |                                                                             Si se establece, el proveedor utiliza por la [CookieAuthenticationHandler](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationhandler) para la protección de datos.                                                                             |
|                      [Descripción](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.description)                       |                                                                                                Solo se aplica a ASP.NET Core 1.x.<br><br> Información adicional sobre el tipo de autenticación que debe ponerse a disposición de la aplicación.                                                                                                |
|                 [Eventos](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.events)                 |                                                                                                      El controlador llama a métodos en el proveedor que proporcionan al control de la aplicación en determinados puntos donde se está produciendo el procesamiento.                                                                                                       |
|                 [EventsType](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.eventstype)                 |                                                            Si se establece, el servicio escriba para obtener la `Events` instancia en lugar de la propiedad (se hereda de [AuthenticationSchemeOptions](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions)).                                                            |
|         [ExpireTimeSpan](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.expiretimespan)         |                                                                                      Controla cuánto tiempo el vale de autenticación que se almacena en la cookie se conserva válida desde el punto en que se crea.<br><br>El valor predeterminado es 14 días.                                                                                       |
|              [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath)              |                                                                                                       Cuando un usuario está autorizado, se le redirige a esta ruta de acceso al inicio de sesión.<br><br>El valor predeterminado es `/Account/Login`.                                                                                                       |
|             [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath)             |                                                                                                            Cuando un usuario se cerró la sesión, se le redirige a esta ruta de acceso.<br><br>El valor predeterminado es `/Account/Logout`.                                                                                                            |
|     [ReturnUrlParameter](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.returnurlparameter)     |                                              Determina el nombre del parámetro de cadena de consulta que se anexa el middleware cuando un <em>401 no autorizado</em> código de estado se cambia a un <em>redireccionamiento 302</em> en la ruta de acceso de inicio de sesión.<br><br>El valor predeterminado es `ReturnUrl`.                                              |
|           [SessionStore](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.sessionstore)           |                                                                                                                              Contenedor opcional que se va a almacenar la identidad de todas las solicitudes.                                                                                                                               |
|      [slidingExpiration](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.slidingexpiration)      |                                                                           Cuando sea true, se emite una cookie nueva con una nueva hora de expiración cuando la cookie actual está en más de medio a través de la ventana de expiración.<br><br>El valor predeterminado es `true`.                                                                           |
|       [TicketDataFormat](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.ticketdataformat)       |                                                                                                 El `TicketDataFormat` se usa para proteger y desproteger la identidad y otras propiedades que se almacenan en el valor de cookie.                                                                                                  |

