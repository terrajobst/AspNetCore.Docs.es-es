---
title: Configuración de la identidad de ASP.NET Core
author: AdrienTorris
description: Comprenda ASP.NET Core valores predeterminados de identidad y aprenda a configurar las propiedades de identidad para usar valores personalizados.
ms.author: riande
ms.date: 02/11/2019
uid: security/authentication/identity-configuration
ms.openlocfilehash: 823182bed2cb953e07f9374d135868aeb2be9c60
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78652697"
---
# <a name="configure-aspnet-core-identity"></a>Configuración de la identidad de ASP.NET Core

ASP.NET Core identidad usa valores predeterminados para la configuración, como la Directiva de contraseñas, el bloqueo y la configuración de cookies. Esta configuración se puede invalidar en la clase `Startup`.

## <a name="identity-options"></a>Opciones de identidad

La clase [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) representa las opciones que se pueden utilizar para configurar el sistema de identidades. `IdentityOptions` debe establecerse **después** de llamar a `AddIdentity` o `AddDefaultIdentity`.

### <a name="claims-identity"></a>Identidad de notificaciones

[IdentityOptions. ClaimsIdentity](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.claimsidentity) especifica [ClaimsIdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions) con las propiedades que se muestran en la tabla siguiente.

| Propiedad | Descripción | Valor predeterminado |
| -------- | ----------- | :-----: |
| [RoleClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.roleclaimtype) | Obtiene o establece el tipo de demanda que se usa para una demanda de rol. | [Argumentos ClaimType. Role](/dotnet/api/system.security.claims.claimtypes.role) |
| [SecurityStampClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.securitystampclaimtype) | Obtiene o establece el tipo de notificaciones que se usa para la notificaciones de marca de seguridad. | `AspNet.Identity.SecurityStamp` |
| [UserIdClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.useridclaimtype) | Obtiene o establece el tipo de demanda que se usa para la demanda de identificador de usuario. | [Argumentos ClaimType. NameIdentifier](/dotnet/api/system.security.claims.claimtypes.nameidentifier) |
| [UserNameClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.usernameclaimtype) | Obtiene o establece el tipo de demanda que se usa para la demanda del nombre de usuario. | [ClaimTypes.Name](/dotnet/api/system.security.claims.claimtypes.name) |

### <a name="lockout"></a>Bloquea

El bloqueo se establece en el método [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync#Microsoft_AspNetCore_Identity_SignInManager_1_PasswordSignInAsync_System_String_System_String_System_Boolean_System_Boolean_) :

[!code-csharp[](identity-configuration/sample/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=9)]

El código anterior se basa en la plantilla de identidad de `Login`. 

Las opciones de bloqueo se establecen en `StartUp.ConfigureServices`:

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_lock)]

El código anterior establece [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) con valores predeterminados.

Una autenticación correcta restablece el número de intentos de acceso erróneos y restablece el reloj.

[IdentityOptions. bloqueos](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.lockout) especifica [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) con las propiedades que se muestran en la tabla.

| Propiedad | Descripción | Valor predeterminado |
| -------- | ----------- | :-----: |
| [AllowedForNewUsers](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.allowedfornewusers) | Determina si un nuevo usuario puede bloquearse. | `true` |
| [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan) | La cantidad de tiempo que un usuario se bloquea cuando se produce un bloqueo. | 5 minutos |
| [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) | El número de intentos de acceso incorrectos hasta que se bloquea un usuario, si está habilitado el bloqueo. | 5 |

### <a name="password"></a>Contraseña

De forma predeterminada, la identidad requiere que las contraseñas contengan caracteres en mayúsculas, caracteres en minúsculas, dígitos y caracteres no alfanuméricos. Las contraseñas deben tener al menos seis caracteres. [PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) se puede establecer en `Startup.ConfigureServices`.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_pw)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

::: moniker-end

[IdentityOptions. Password](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.password) especifica [PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) con las propiedades que se muestran en la tabla.

::: moniker range=">= aspnetcore-2.0"

| Propiedad | Descripción | Valor predeterminado |
| -------- | ----------- | :-----: |
| [RequireDigit](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredigit) | Requiere un número entre 0-9 en la contraseña. | `true` |
| [RequiredLength](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredlength) | La longitud mínima de la contraseña. | 6 |
| [RequireLowercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirelowercase) | Requiere un carácter en minúscula en la contraseña. | `true` |
| [RequireNonAlphanumeric](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirenonalphanumeric) | Requiere un carácter no alfanumérico en la contraseña. | `true` |
| [RequiredUniqueChars](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireduniquechars) | Solo se aplica a ASP.NET Core 2,0 o posterior.<br><br> Requiere el número de caracteres distintos en la contraseña. | 1 |
| [RequireUppercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireuppercase) | Requiere un carácter en mayúsculas en la contraseña. | `true` |

::: moniker-end

::: moniker range="< aspnetcore-2.0"

| Propiedad | Descripción | Valor predeterminado |
| -------- | ----------- | :-----: |
| [RequireDigit](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredigit) | Requiere un número entre 0-9 en la contraseña. | `true` |
| [RequiredLength](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredlength) | La longitud mínima de la contraseña. | 6 |
| [RequireLowercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirelowercase) | Requiere un carácter en minúscula en la contraseña. | `true` |
| [RequireNonAlphanumeric](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirenonalphanumeric) | Requiere un carácter no alfanumérico en la contraseña. | `true` |
| [RequireUppercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireuppercase) | Requiere un carácter en mayúsculas en la contraseña. | `true` |

::: moniker-end

### <a name="sign-in"></a>Inicio de sesión

En el código siguiente se establece `SignIn` valores (en valores predeterminados):

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_si)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)] 

::: moniker-end

[IdentityOptions. Signen](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.signin) especifica [SignInOptions](/dotnet/api/microsoft.aspnetcore.identity.signinoptions) con las propiedades que se muestran en la tabla.

| Propiedad | Descripción | Valor predeterminado |
| -------- | ----------- | :-----: |
| [RequireConfirmedEmail](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedemail) | Requiere un correo electrónico confirmado para iniciar sesión. | `false` |
| [RequireConfirmedPhoneNumber](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedphonenumber) | Requiere un número de teléfono confirmado para iniciar sesión. | `false` |

### <a name="tokens"></a>Tokens

[IdentityOptions. tokens](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.tokens) especifica [TokenOptions](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions) con las propiedades que se muestran en la tabla.

|                                                        Propiedad                                                         |                                                                                      Descripción                                                                                      |
|-------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     [AuthenticatorTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.authenticatortokenprovider)     |                                       Obtiene o establece la `AuthenticatorTokenProvider` utilizada para validar los inicios de sesión en dos fases con un autenticador.                                       |
|       [ChangeEmailTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changeemailtokenprovider)       |                                     Obtiene o establece la `ChangeEmailTokenProvider` utilizada para generar los tokens que se usan en los correos electrónicos de confirmación de cambio de correo electrónico.                                     |
| [ChangePhoneNumberTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changephonenumbertokenprovider) |                                      Obtiene o establece la `ChangePhoneNumberTokenProvider` utilizada para generar tokens que se usan al cambiar los números de teléfono.                                      |
| [EmailConfirmationTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.emailconfirmationtokenprovider) |                                             Obtiene o establece el proveedor de tokens que se usa para generar los tokens que se usan en los correos electrónicos de confirmación de la cuenta.                                              |
|     [PasswordResetTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.passwordresettokenprovider)     | Obtiene o establece el [> IUserTwoFactorTokenProvider\<TUser](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactortokenprovider-1) que se usa para generar los tokens que se usan en los correos electrónicos de restablecimiento de contraseña. |
|                    [ProviderMap](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.providermap)                    |                Se utiliza para construir un [proveedor de tokens de usuario](/dotnet/api/microsoft.aspnetcore.identity.tokenproviderdescriptor) con la clave utilizada como nombre del proveedor.                 |

### <a name="user"></a>Usuario

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_user)]

[IdentityOptions. User](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.user) especifica [UserOptions](/dotnet/api/microsoft.aspnetcore.identity.useroptions) con las propiedades que se muestran en la tabla.

| Propiedad | Descripción | Valor predeterminado |
| -------- | ----------- | :-----: |
| [AllowedUserNameCharacters](/dotnet/api/microsoft.aspnetcore.identity.useroptions.allowedusernamecharacters) | Caracteres permitidos en el nombre de usuario. | abcdefghijklmnopqrstuvwxyz<br>ABCDEFGHIJKLMNOPQRSTUVWXYZ<br>0123456789<br>-.\_@+ |
| [RequireUniqueEmail](/dotnet/api/microsoft.aspnetcore.identity.useroptions.requireuniqueemail) | Requiere que cada usuario tenga un correo electrónico único. | `false` |

### <a name="cookie-settings"></a>Configuración de cookies

Configure la cookie de la aplicación en `Startup.ConfigureServices`. Se debe llamar a [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_ConfigureApplicationCookie_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_Cookies_CookieAuthenticationOptions__) **después** de llamar a `AddIdentity` o `AddDefaultIdentity`.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_cookie)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

::: moniker-end

Para obtener más información, vea [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions).

## <a name="password-hasher-options"></a>Opciones de hash de contraseña

<xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions> obtiene y establece las opciones para el hash de contraseñas.

| Opción | Descripción |
| ------ | ----------- |
| <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.CompatibilityMode> | El modo de compatibilidad utilizado al aplicar el algoritmo hash a nuevas contraseñas. Su valor predeterminado es <xref:Microsoft.AspNetCore.Identity.PasswordHasherCompatibilityMode.IdentityV3>. El primer byte de una contraseña con hash, denominada *marcador de formato*, especifica la versión del algoritmo hash que se usa para aplicar un algoritmo hash a la contraseña. Al comprobar una contraseña en un hash, el método <xref:Microsoft.AspNetCore.Identity.PasswordHasher`1.VerifyHashedPassword*> selecciona el algoritmo correcto en función del primer byte. Un cliente puede autenticarse independientemente de la versión del algoritmo que se usó para aplicar un algoritmo hash a la contraseña. Establecer el modo de compatibilidad afecta a la operación hash de *nuevas contraseñas*. |
| <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.IterationCount> | Número de iteraciones utilizadas al aplicar un algoritmo hash a las contraseñas mediante PBKDF2. Este valor solo se utiliza cuando el <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.CompatibilityMode> está establecido en <xref:Microsoft.AspNetCore.Identity.PasswordHasherCompatibilityMode.IdentityV3>. El valor debe ser un entero positivo y su valor predeterminado es `10000`. |

En el ejemplo siguiente, el <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.IterationCount> se establece en `12000` en `Startup.ConfigureServices`:

```csharp
// using Microsoft.AspNetCore.Identity;

services.Configure<PasswordHasherOptions>(option =>
{
    option.IterationCount = 12000;
});
```
