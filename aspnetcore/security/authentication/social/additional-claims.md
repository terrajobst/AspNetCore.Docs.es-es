---
title: Conservar notificaciones adicionales y los tokens de proveedores externos en ASP.NET Core
author: guardrex
description: Obtenga información sobre cómo establecer notificaciones adicionales y los tokens de proveedores externos.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/14/2019
uid: security/authentication/social/additional-claims
ms.openlocfilehash: e18287e5a4171b3f7a6daa122111448b8447c1da
ms.sourcegitcommit: ccbb84ae307a5bc527441d3d509c20b5c1edde05
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2019
ms.locfileid: "65874847"
---
# <a name="persist-additional-claims-and-tokens-from-external-providers-in-aspnet-core"></a>Conservar notificaciones adicionales y los tokens de proveedores externos en ASP.NET Core

Por [Luke Latham](https://github.com/guardrex)

Una aplicación ASP.NET Core puede establecer notificaciones adicionales y los tokens de proveedores de autenticación externos, como Facebook, Google, Microsoft y Twitter. Cada proveedor revela información distinta acerca de los usuarios en su plataforma, pero el patrón para recibir y transformar los datos de usuario en notificaciones adicionales es el mismo.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Requisitos previos

Decida qué proveedores de autenticación externos para admitir en la aplicación. Para cada proveedor, registrar la aplicación y obtener un identificador de cliente y secreto de cliente. Para obtener más información, consulta <xref:security/authentication/social/index>. La aplicación de ejemplo usa el [proveedor de autenticación de Google](xref:security/authentication/google-logins).

## <a name="set-the-client-id-and-client-secret"></a>Establece el identificador de cliente y el secreto de cliente

El proveedor de autenticación OAuth establece una relación de confianza con una aplicación mediante un identificador de cliente y secreto de cliente. Id. de cliente y los valores de secreto de cliente se crean para la aplicación mediante el proveedor de autenticación externo cuando la aplicación esté registrada con el proveedor. Cada proveedor externo que usa la aplicación debe configurarse de forma independiente con Id. de cliente y el secreto de cliente del proveedor. Para obtener más información, vea los temas de proveedor de autenticación externo que se aplican a su escenario:

* [Autenticación con Facebook](xref:security/authentication/facebook-logins)
* [Autenticación con Google](xref:security/authentication/google-logins)
* [Autenticación con Microsoft](xref:security/authentication/microsoft-logins)
* [Autenticación con Twitter](xref:security/authentication/twitter-logins)
* [Otros proveedores de autenticación](xref:security/authentication/otherlogins)
* [OpenIdConnect](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2)

La aplicación de ejemplo configura el proveedor de autenticación de Google con un identificador de cliente y secreto de cliente proporcionado por Google:

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=4,9)]

## <a name="establish-the-authentication-scope"></a>Establecer el ámbito de autenticación

Especifique la lista de permisos para recuperar el proveedor especificando el <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>. Ámbitos de autenticación para los proveedores externos comunes aparecen en la tabla siguiente.

| Proveedor  | Ámbito                                                            |
| --------- | ---------------------------------------------------------------- |
| Facebook  | `https://www.facebook.com/dialog/oauth`                          |
| Google    | `https://www.googleapis.com/auth/userinfo.profile`               |
| Microsoft | `https://login.microsoftonline.com/common/oauth2/v2.0/authorize` |
| Twitter   | `https://api.twitter.com/oauth/authenticate`                     |

En de la aplicación de ejemplo, Google `userinfo.profile` ámbito se agrega automáticamente el marco de trabajo cuando <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> se llama en el <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder>. Si la aplicación requiere ámbitos adicionales, agregue las opciones. En el ejemplo siguiente, Google `https://www.googleapis.com/auth/user.birthday.read` ámbito se agrega con el fin de recuperar la fecha de nacimiento del usuario:

```csharp
options.Scope.Add("https://www.googleapis.com/auth/user.birthday.read");
```

## <a name="map-user-data-keys-and-create-claims"></a>Asignar claves de datos de usuario y crear notificaciones

En las opciones del proveedor, especifique un <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> o <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonSubKey*> para cada clave o subclave en datos de usuario del proveedor externo JSON para la identidad de aplicación leer en Inicio de sesión. Para obtener más información sobre los tipos de notificación, consulte <xref:System.Security.Claims.ClaimTypes>.

La aplicación de ejemplo crea la configuración regional (`urn:google:locale`) e imagen (`urn:google:picture`) notificaciones a partir de la `locale` y `picture` claves en los datos de usuario de Google:

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=13-14)]

En <xref:Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync*>, un <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) ha iniciado sesión en la aplicación con <xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*>. Durante el inicio de sesión en proceso, el <xref:Microsoft.AspNetCore.Identity.UserManager%601> puede almacenar un `ApplicationUser` notificaciones para los datos de usuario disponibles en el <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.

En la aplicación de ejemplo, `OnPostConfirmationAsync` (*Account/ExternalLogin.cshtml.cs*) establece la configuración regional (`urn:google:locale`) e imagen (`urn:google:picture`) notificaciones para firmado en `ApplicationUser`, incluida una notificación para <xref:System.Security.Claims.ClaimTypes.GivenName> :

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=35-51)]

De forma predeterminada, las notificaciones de usuario se almacenan en la cookie de autenticación. Si la cookie de autenticación es demasiado grande, es posible que la aplicación genere un error porque:

* El explorador detecta que el encabezado de cookie es demasiado largo.
* El tamaño total de la solicitud es demasiado grande.

Si es necesaria para procesar las solicitudes de usuario una gran cantidad de datos de usuario:

* Limitar el número y tamaño de las notificaciones de usuario para que sólo lo que requiere la aplicación de procesamiento de solicitudes.
* Usar una personalizada <xref:Microsoft.AspNetCore.Authentication.Cookies.ITicketStore> para el Middleware de autenticación de cookies <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.SessionStore> para almacenar la identidad a través de solicitudes. Conservar grandes cantidades de información de identidad en el servidor al enviar sólo una pequeña identificador de clave de sesión al cliente.

## <a name="save-the-access-token"></a>Guarde el token de acceso

<xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> define si se deben almacenar los tokens de acceso y actualización en el <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> después de una autorización correcta. `SaveTokens` se establece en `false` de forma predeterminada para reducir el tamaño de la cookie de autenticación final.

La aplicación de ejemplo establece el valor de `SaveTokens` a `true` en <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=15)]

Cuando `OnPostConfirmationAsync` se ejecuta, almacenar el token de acceso ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) del proveedor externo en el `ApplicationUser`del `AuthenticationProperties`.

La aplicación de ejemplo guarda el token de acceso en `OnPostConfirmationAsync` (nuevo registro de usuario) y `OnGetCallbackAsync` (usuario registrado anteriormente) en *Account/ExternalLogin.cshtml.cs*:

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=54-56)]

## <a name="how-to-add-additional-custom-tokens"></a>Cómo agregar tokens personalizados adicionales

Para demostrar cómo agregar un token personalizado, que se almacena como parte de `SaveTokens`, la aplicación de ejemplo agrega un <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> con el actual <xref:System.DateTime> para un [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) de `TicketCreated`:

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=17-28)]

## <a name="creating-and-adding-claims"></a>Creación y la incorporación de notificaciones

El marco de trabajo proporciona acciones comunes y métodos de extensión para crear y agregar notificaciones a la colección. Para obtener más información, vea <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions> y <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionUniqueExtensions>.

Los usuarios pueden definir las acciones personalizadas mediante la derivación de <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction> e implementación abstracta <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run*> método.

Para obtener más información, consulta <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims>.

## <a name="removal-of-claim-actions-and-claims"></a>Eliminación de las acciones de notificación y notificaciones

[ClaimActionCollection.Remove(String)](xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection.Remove*) quita todas las notificaciones de acciones para el determinado <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> de la colección. [ClaimActionCollectionMapExtensions.DeleteClaim (ClaimActionCollection, String)](xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*) elimina una notificación de la dada <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> de la identidad. <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*> se usa principalmente con [OpenID Connect (OIDC)](/azure/active-directory/develop/v2-protocols-oidc) para quitar las notificaciones generadas por el protocolo.

## <a name="sample-app-output"></a>Salida de la aplicación de ejemplo

```
User Claims

http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier
    9b342344f-7aab-43c2-1ac1-ba75912ca999
http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name
    someone@gmail.com
AspNet.Identity.SecurityStamp
    7D4312MOWRYYBFI1KXRPHGOSTBVWSFDE
http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname
    Judy
urn:google:locale
    en
urn:google:picture
    https://lh4.googleusercontent.com/-XXXXXX/XXXXXX/XXXXXX/XXXXXX/photo.jpg

Authentication Properties

.Token.access_token
    yc23.AlvoZqz56...1lxltXV7D-ZWP9
.Token.token_type
    Bearer
.Token.expires_at
    2019-04-11T22:14:51.0000000+00:00
.Token.TicketCreated
    4/11/2019 9:14:52 PM
.TokenNames
    access_token;token_type;expires_at;TicketCreated
.persistent
.issued
    Thu, 11 Apr 2019 20:51:06 GMT
.expires
    Thu, 25 Apr 2019 20:51:06 GMT

```

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="additional-resources"></a>Recursos adicionales

* [aplicación SocialSample ingeniería de ASPNET/AspNetCore](https://github.com/aspnet/AspNetCore/tree/master/src/Security/Authentication/samples/SocialSample) &ndash; es la aplicación de ejemplo vinculado en el [del repositorio de GitHub de aspnet/AspNetCore](https://github.com/aspnet/AspNetCore) `master` rama ingeniería. El `master` rama contiene código en desarrollo activo para la próxima versión de ASP.NET Core. Para ver una versión de la aplicación de ejemplo para una versión comercial de ASP.NET Core, use el **rama** lista desplegable lista para seleccionar una rama de versión (por ejemplo `release/2.2`).
