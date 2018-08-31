---
title: Conservar notificaciones adicionales y los tokens de proveedores externos en ASP.NET Core
author: guardrex
description: Obtenga información sobre cómo establecer notificaciones adicionales y los tokens de proveedores externos.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/28/2018
uid: security/authentication/social/additional-claims
ms.openlocfilehash: 6386dd5f0bd901be01cf56081a6b9ca7f408f9f9
ms.sourcegitcommit: a669c4e3f42e387e214a354ac4143555602e6f66
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2018
ms.locfileid: "43336205"
---
# <a name="persist-additional-claims-and-tokens-from-external-providers-in-aspnet-core"></a>Conservar notificaciones adicionales y los tokens de proveedores externos en ASP.NET Core

Por [Luke Latham](https://github.com/guardrex)

Una aplicación ASP.NET Core puede establecer notificaciones adicionales y los tokens de proveedores de autenticación externos, como Facebook, Google, Microsoft y Twitter. Cada proveedor revela información distinta acerca de los usuarios en su plataforma, pero el patrón para recibir y transformar los datos de usuario en notificaciones adicionales es el mismo.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

## <a name="prerequisite"></a>Requisito previo

Decida qué proveedores de autenticación externos para admitir en la aplicación. Para cada proveedor, registrar la aplicación y obtener un identificador de cliente y secreto de cliente. Para obtener más información, consulta <xref:security/authentication/social/index>. El [aplicación de ejemplo](#sample-app-instructions) usa el [proveedor de autenticación de Google](xref:security/authentication/google-logins).

## <a name="authentication-provider-configuration"></a>Configuración del proveedor de autenticación

### <a name="set-the-client-id-and-client-secret"></a>Establece el identificador de cliente y el secreto de cliente

El proveedor de autenticación OAuth establece una relación de confianza con una aplicación mediante un identificador de cliente y secreto de cliente. Id. de cliente y los valores de secreto de cliente se crean para la aplicación mediante el proveedor de autenticación externo cuando la aplicación esté registrada con el proveedor. Cada proveedor externo que usa la aplicación debe configurarse de forma independiente con Id. de cliente y el secreto de cliente del proveedor. Para obtener más información, vea los temas de proveedor de autenticación externo que se aplican a su escenario:

* [Autenticación con Facebook](xref:security/authentication/facebook-logins)
* [Autenticación con Google](xref:security/authentication/google-logins)
* [Autenticación con Microsoft](xref:security/authentication/microsoft-logins)
* [Autenticación con Twitter](xref:security/authentication/twitter-logins)
* [Otros proveedores de autenticación](xref:security/authentication/otherlogins)
* [OpenIdConnect](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2)

La aplicación de ejemplo configura el proveedor de autenticación de Google con un identificador de cliente y secreto de cliente proporcionado por Google:

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=4,6)]

### <a name="establish-the-authentication-scope"></a>Establecer el ámbito de autenticación

Especifique la lista de permisos para recuperar el proveedor especificando el <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>. Ámbitos de autenticación para los proveedores externos comunes aparecen en la tabla siguiente.

| Proveedor  | Ámbito                                                            |
| --------- | ---------------------------------------------------------------- |
| Facebook  | `https://www.facebook.com/dialog/oauth`                          |
| Google    | `https://www.googleapis.com/auth/plus.login`                     |
| Microsoft | `https://login.microsoftonline.com/common/oauth2/v2.0/authorize` |
| Twitter   | `https://api.twitter.com/oauth/authenticate`                     |

La aplicación de ejemplo agrega Google `plus.login` ámbito de sesión de Google + en los permisos de solicitud:

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=7)]

### <a name="map-user-data-keys-and-create-claims"></a>Asignar claves de datos de usuario y crear notificaciones

En las opciones del proveedor, especifique un <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> para cada clave de datos de usuario del proveedor externo JSON para la identidad de aplicación leer en Inicio de sesión. Para obtener más información sobre los tipos de notificación, consulte <xref:System.Security.Claims.ClaimTypes>.

La aplicación de ejemplo crea un <xref:System.Security.Claims.ClaimTypes.Gender> notificación desde el `gender` clave en los datos de usuario de Google:

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=8)]

En <xref:Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync*>, un <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) ha iniciado sesión en la aplicación con <xref:Microsoft.AspNetCore.Identity.SignInManager`1.SignInAsync*>. Durante el inicio de sesión en proceso, el <xref:Microsoft.AspNetCore.Identity.UserManager`1> puede almacenar un `ApplicationUser` de notificación para los datos de usuario disponibles en el <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.

En la aplicación de ejemplo, `OnPostConfirmationAsync` (*Account/ExternalLogin.cshtml.cs*) establece un <xref:System.Security.Claims.ClaimTypes.Gender> de notificación para firmado en `ApplicationUser`:

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=30-31)]

### <a name="save-the-access-token"></a>Guarde el token de acceso

<xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> define si se deben almacenar los tokens de acceso y actualización en el <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> después de una autorización correcta. `SaveTokens` se establece en `false` de forma predeterminada para reducir el tamaño de la cookie de autenticación final.

La aplicación de ejemplo establece el valor de `SaveTokens` a `true` en <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=9)]

Cuando `OnPostConfirmationAsync` se ejecuta, almacenar el token de acceso ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) del proveedor externo en el `ApplicationUser`del `AuthenticationProperties`.

La aplicación de ejemplo guarda en el token de acceso:

* `OnPostConfirmationAsync` &ndash; Ejecuta de nuevo registro de usuario.
* `OnGetCallbackAsync` &ndash; Se ejecuta cuando inicia sesión un usuario registrado anteriormente en la aplicación.

*Account/ExternalLogin.cshtml.cs*:

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=34-35)]

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnGetCallbackAsync&highlight=31-32)]

### <a name="how-to-add-additional-custom-tokens"></a>Cómo agregar tokens personalizados adicionales

Para demostrar cómo agregar un token personalizado, que se almacena como parte de `SaveTokens`, la aplicación de ejemplo agrega un <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> con el actual <xref:System.DateTime> para un [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) de `TicketCreated`:

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=10-21)]

## <a name="sample-app-instructions"></a>Instrucciones de la aplicación de ejemplo

La aplicación de ejemplo muestra cómo:

* Obtenga el sexo del usuario de Google y almacena una notificación de género con el valor.
* Store el token de acceso de Google en el usuario `AuthenticationProperties`.

Para usar la aplicación de ejemplo:

1. Registrar la aplicación y obtener un identificador de cliente válido y el secreto de cliente para la autenticación de Google. Para obtener más información, consulta <xref:security/authentication/google-logins>.
1. Proporcione el identificador de cliente y secreto de cliente a la aplicación en el <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions> de `Startup.ConfigureServices`.
1. Ejecute la aplicación y solicite la página Mis notificaciones. Cuando el usuario no se ha iniciado sesión, la aplicación se redirige a Google. Inicie sesión con Google. Google redirige al usuario a la aplicación (`/Home/MyClaims`). El usuario se autentica y se carga la página Mis notificaciones. Está presente en la notificación de género **notificaciones de usuario** con el valor obtenido de Google. El token de acceso aparece en el **las propiedades de autenticación**.

```
User Claims

http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier
    b36a7b09-9135-4810-b7a5-78697ff23e99
http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name
    username@gmail.com
AspNet.Identity.SecurityStamp
    29G2TB881ATCUQFJSRFG1S0QJ0OOAWVT
http://schemas.xmlsoap.org/ws/2005/05/identity/claims/gender
    female
http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod
    Google

Authentication Properties

.Token.access_token
    bv42.Dgw...GQMv9ArLPs
.Token.token_type
    Bearer
.Token.expires_at
    2018-08-27T19:08:00.0000000+00:00
.Token.TicketCreated
    8/27/2018 6:08:00 PM
.TokenNames
    access_token;token_type;expires_at;TicketCreated
.issued
    Mon, 27 Aug 2018 18:08:05 GMT
.expires
    Mon, 10 Sep 2018 18:08:05 GMT
```
