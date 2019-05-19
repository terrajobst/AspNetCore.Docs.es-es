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
# <a name="persist-additional-claims-and-tokens-from-external-providers-in-aspnet-core"></a><span data-ttu-id="f135b-103">Conservar notificaciones adicionales y los tokens de proveedores externos en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f135b-103">Persist additional claims and tokens from external providers in ASP.NET Core</span></span>

<span data-ttu-id="f135b-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="f135b-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="f135b-105">Una aplicación ASP.NET Core puede establecer notificaciones adicionales y los tokens de proveedores de autenticación externos, como Facebook, Google, Microsoft y Twitter.</span><span class="sxs-lookup"><span data-stu-id="f135b-105">An ASP.NET Core app can establish additional claims and tokens from external authentication providers, such as Facebook, Google, Microsoft, and Twitter.</span></span> <span data-ttu-id="f135b-106">Cada proveedor revela información distinta acerca de los usuarios en su plataforma, pero el patrón para recibir y transformar los datos de usuario en notificaciones adicionales es el mismo.</span><span class="sxs-lookup"><span data-stu-id="f135b-106">Each provider reveals different information about users on its platform, but the pattern for receiving and transforming user data into additional claims is the same.</span></span>

<span data-ttu-id="f135b-107">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f135b-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f135b-108">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="f135b-108">Prerequisites</span></span>

<span data-ttu-id="f135b-109">Decida qué proveedores de autenticación externos para admitir en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f135b-109">Decide which external authentication providers to support in the app.</span></span> <span data-ttu-id="f135b-110">Para cada proveedor, registrar la aplicación y obtener un identificador de cliente y secreto de cliente.</span><span class="sxs-lookup"><span data-stu-id="f135b-110">For each provider, register the app and obtain a client ID and client secret.</span></span> <span data-ttu-id="f135b-111">Para obtener más información, consulta <xref:security/authentication/social/index>.</span><span class="sxs-lookup"><span data-stu-id="f135b-111">For more information, see <xref:security/authentication/social/index>.</span></span> <span data-ttu-id="f135b-112">La aplicación de ejemplo usa el [proveedor de autenticación de Google](xref:security/authentication/google-logins).</span><span class="sxs-lookup"><span data-stu-id="f135b-112">The sample app uses the [Google authentication provider](xref:security/authentication/google-logins).</span></span>

## <a name="set-the-client-id-and-client-secret"></a><span data-ttu-id="f135b-113">Establece el identificador de cliente y el secreto de cliente</span><span class="sxs-lookup"><span data-stu-id="f135b-113">Set the client ID and client secret</span></span>

<span data-ttu-id="f135b-114">El proveedor de autenticación OAuth establece una relación de confianza con una aplicación mediante un identificador de cliente y secreto de cliente.</span><span class="sxs-lookup"><span data-stu-id="f135b-114">The OAuth authentication provider establishes a trust relationship with an app using a client ID and client secret.</span></span> <span data-ttu-id="f135b-115">Id. de cliente y los valores de secreto de cliente se crean para la aplicación mediante el proveedor de autenticación externo cuando la aplicación esté registrada con el proveedor.</span><span class="sxs-lookup"><span data-stu-id="f135b-115">Client ID and client secret values are created for the app by the external authentication provider when the app is registered with the provider.</span></span> <span data-ttu-id="f135b-116">Cada proveedor externo que usa la aplicación debe configurarse de forma independiente con Id. de cliente y el secreto de cliente del proveedor.</span><span class="sxs-lookup"><span data-stu-id="f135b-116">Each external provider that the app uses must be configured independently with the provider's client ID and client secret.</span></span> <span data-ttu-id="f135b-117">Para obtener más información, vea los temas de proveedor de autenticación externo que se aplican a su escenario:</span><span class="sxs-lookup"><span data-stu-id="f135b-117">For more information, see the external authentication provider topics that apply to your scenario:</span></span>

* [<span data-ttu-id="f135b-118">Autenticación con Facebook</span><span class="sxs-lookup"><span data-stu-id="f135b-118">Facebook authentication</span></span>](xref:security/authentication/facebook-logins)
* [<span data-ttu-id="f135b-119">Autenticación con Google</span><span class="sxs-lookup"><span data-stu-id="f135b-119">Google authentication</span></span>](xref:security/authentication/google-logins)
* [<span data-ttu-id="f135b-120">Autenticación con Microsoft</span><span class="sxs-lookup"><span data-stu-id="f135b-120">Microsoft authentication</span></span>](xref:security/authentication/microsoft-logins)
* [<span data-ttu-id="f135b-121">Autenticación con Twitter</span><span class="sxs-lookup"><span data-stu-id="f135b-121">Twitter authentication</span></span>](xref:security/authentication/twitter-logins)
* [<span data-ttu-id="f135b-122">Otros proveedores de autenticación</span><span class="sxs-lookup"><span data-stu-id="f135b-122">Other authentication providers</span></span>](xref:security/authentication/otherlogins)
* [<span data-ttu-id="f135b-123">OpenIdConnect</span><span class="sxs-lookup"><span data-stu-id="f135b-123">OpenIdConnect</span></span>](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2)

<span data-ttu-id="f135b-124">La aplicación de ejemplo configura el proveedor de autenticación de Google con un identificador de cliente y secreto de cliente proporcionado por Google:</span><span class="sxs-lookup"><span data-stu-id="f135b-124">The sample app configures the Google authentication provider with a client ID and client secret provided by Google:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=4,9)]

## <a name="establish-the-authentication-scope"></a><span data-ttu-id="f135b-125">Establecer el ámbito de autenticación</span><span class="sxs-lookup"><span data-stu-id="f135b-125">Establish the authentication scope</span></span>

<span data-ttu-id="f135b-126">Especifique la lista de permisos para recuperar el proveedor especificando el <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>.</span><span class="sxs-lookup"><span data-stu-id="f135b-126">Specify the list of permissions to retrieve from the provider by specifying the <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>.</span></span> <span data-ttu-id="f135b-127">Ámbitos de autenticación para los proveedores externos comunes aparecen en la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="f135b-127">Authentication scopes for common external providers appear in the following table.</span></span>

| <span data-ttu-id="f135b-128">Proveedor</span><span class="sxs-lookup"><span data-stu-id="f135b-128">Provider</span></span>  | <span data-ttu-id="f135b-129">Ámbito</span><span class="sxs-lookup"><span data-stu-id="f135b-129">Scope</span></span>                                                            |
| --------- | ---------------------------------------------------------------- |
| <span data-ttu-id="f135b-130">Facebook</span><span class="sxs-lookup"><span data-stu-id="f135b-130">Facebook</span></span>  | `https://www.facebook.com/dialog/oauth`                          |
| <span data-ttu-id="f135b-131">Google</span><span class="sxs-lookup"><span data-stu-id="f135b-131">Google</span></span>    | `https://www.googleapis.com/auth/userinfo.profile`               |
| <span data-ttu-id="f135b-132">Microsoft</span><span class="sxs-lookup"><span data-stu-id="f135b-132">Microsoft</span></span> | `https://login.microsoftonline.com/common/oauth2/v2.0/authorize` |
| <span data-ttu-id="f135b-133">Twitter</span><span class="sxs-lookup"><span data-stu-id="f135b-133">Twitter</span></span>   | `https://api.twitter.com/oauth/authenticate`                     |

<span data-ttu-id="f135b-134">En de la aplicación de ejemplo, Google `userinfo.profile` ámbito se agrega automáticamente el marco de trabajo cuando <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> se llama en el <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="f135b-134">In the sample app, Google's `userinfo.profile` scope is automatically added by the framework when <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> is called on the <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder>.</span></span> <span data-ttu-id="f135b-135">Si la aplicación requiere ámbitos adicionales, agregue las opciones.</span><span class="sxs-lookup"><span data-stu-id="f135b-135">If the app requires additional scopes, add them to the options.</span></span> <span data-ttu-id="f135b-136">En el ejemplo siguiente, Google `https://www.googleapis.com/auth/user.birthday.read` ámbito se agrega con el fin de recuperar la fecha de nacimiento del usuario:</span><span class="sxs-lookup"><span data-stu-id="f135b-136">In the following example, the Google `https://www.googleapis.com/auth/user.birthday.read` scope is added in order to retrieve a user's birthday:</span></span>

```csharp
options.Scope.Add("https://www.googleapis.com/auth/user.birthday.read");
```

## <a name="map-user-data-keys-and-create-claims"></a><span data-ttu-id="f135b-137">Asignar claves de datos de usuario y crear notificaciones</span><span class="sxs-lookup"><span data-stu-id="f135b-137">Map user data keys and create claims</span></span>

<span data-ttu-id="f135b-138">En las opciones del proveedor, especifique un <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> o <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonSubKey*> para cada clave o subclave en datos de usuario del proveedor externo JSON para la identidad de aplicación leer en Inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="f135b-138">In the provider's options, specify a <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> or <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonSubKey*> for each key/subkey in the external provider's JSON user data for the app identity to read on sign in.</span></span> <span data-ttu-id="f135b-139">Para obtener más información sobre los tipos de notificación, consulte <xref:System.Security.Claims.ClaimTypes>.</span><span class="sxs-lookup"><span data-stu-id="f135b-139">For more information on claim types, see <xref:System.Security.Claims.ClaimTypes>.</span></span>

<span data-ttu-id="f135b-140">La aplicación de ejemplo crea la configuración regional (`urn:google:locale`) e imagen (`urn:google:picture`) notificaciones a partir de la `locale` y `picture` claves en los datos de usuario de Google:</span><span class="sxs-lookup"><span data-stu-id="f135b-140">The sample app creates locale (`urn:google:locale`) and picture (`urn:google:picture`) claims from the `locale` and `picture` keys in Google user data:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=13-14)]

<span data-ttu-id="f135b-141">En <xref:Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync*>, un <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) ha iniciado sesión en la aplicación con <xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*>.</span><span class="sxs-lookup"><span data-stu-id="f135b-141">In <xref:Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync*>, an <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) is signed into the app with <xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*>.</span></span> <span data-ttu-id="f135b-142">Durante el inicio de sesión en proceso, el <xref:Microsoft.AspNetCore.Identity.UserManager%601> puede almacenar un `ApplicationUser` notificaciones para los datos de usuario disponibles en el <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.</span><span class="sxs-lookup"><span data-stu-id="f135b-142">During the sign in process, the <xref:Microsoft.AspNetCore.Identity.UserManager%601> can store an `ApplicationUser` claims for user data available from the <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.</span></span>

<span data-ttu-id="f135b-143">En la aplicación de ejemplo, `OnPostConfirmationAsync` (*Account/ExternalLogin.cshtml.cs*) establece la configuración regional (`urn:google:locale`) e imagen (`urn:google:picture`) notificaciones para firmado en `ApplicationUser`, incluida una notificación para <xref:System.Security.Claims.ClaimTypes.GivenName> :</span><span class="sxs-lookup"><span data-stu-id="f135b-143">In the sample app, `OnPostConfirmationAsync` (*Account/ExternalLogin.cshtml.cs*) establishes the locale (`urn:google:locale`) and picture (`urn:google:picture`) claims for the signed in `ApplicationUser`, including a claim for <xref:System.Security.Claims.ClaimTypes.GivenName>:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=35-51)]

<span data-ttu-id="f135b-144">De forma predeterminada, las notificaciones de usuario se almacenan en la cookie de autenticación.</span><span class="sxs-lookup"><span data-stu-id="f135b-144">By default, a user's claims are stored in the authentication cookie.</span></span> <span data-ttu-id="f135b-145">Si la cookie de autenticación es demasiado grande, es posible que la aplicación genere un error porque:</span><span class="sxs-lookup"><span data-stu-id="f135b-145">If the authentication cookie is too large, it can cause the app to fail because:</span></span>

* <span data-ttu-id="f135b-146">El explorador detecta que el encabezado de cookie es demasiado largo.</span><span class="sxs-lookup"><span data-stu-id="f135b-146">The browser detects that the cookie header is too long.</span></span>
* <span data-ttu-id="f135b-147">El tamaño total de la solicitud es demasiado grande.</span><span class="sxs-lookup"><span data-stu-id="f135b-147">The overall size of the request is too large.</span></span>

<span data-ttu-id="f135b-148">Si es necesaria para procesar las solicitudes de usuario una gran cantidad de datos de usuario:</span><span class="sxs-lookup"><span data-stu-id="f135b-148">If a large amount of user data is required for processing user requests:</span></span>

* <span data-ttu-id="f135b-149">Limitar el número y tamaño de las notificaciones de usuario para que sólo lo que requiere la aplicación de procesamiento de solicitudes.</span><span class="sxs-lookup"><span data-stu-id="f135b-149">Limit the number and size of user claims for request processing to only what the app requires.</span></span>
* <span data-ttu-id="f135b-150">Usar una personalizada <xref:Microsoft.AspNetCore.Authentication.Cookies.ITicketStore> para el Middleware de autenticación de cookies <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.SessionStore> para almacenar la identidad a través de solicitudes.</span><span class="sxs-lookup"><span data-stu-id="f135b-150">Use a custom <xref:Microsoft.AspNetCore.Authentication.Cookies.ITicketStore> for the Cookie Authentication Middleware's <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.SessionStore> to store identity across requests.</span></span> <span data-ttu-id="f135b-151">Conservar grandes cantidades de información de identidad en el servidor al enviar sólo una pequeña identificador de clave de sesión al cliente.</span><span class="sxs-lookup"><span data-stu-id="f135b-151">Preserve large quantities of identity information on the server while only sending a small session identifier key to the client.</span></span>

## <a name="save-the-access-token"></a><span data-ttu-id="f135b-152">Guarde el token de acceso</span><span class="sxs-lookup"><span data-stu-id="f135b-152">Save the access token</span></span>

<span data-ttu-id="f135b-153"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> define si se deben almacenar los tokens de acceso y actualización en el <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> después de una autorización correcta.</span><span class="sxs-lookup"><span data-stu-id="f135b-153"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> defines whether access and refresh tokens should be stored in the <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> after a successful authorization.</span></span> <span data-ttu-id="f135b-154">`SaveTokens` se establece en `false` de forma predeterminada para reducir el tamaño de la cookie de autenticación final.</span><span class="sxs-lookup"><span data-stu-id="f135b-154">`SaveTokens` is set to `false` by default to reduce the size of the final authentication cookie.</span></span>

<span data-ttu-id="f135b-155">La aplicación de ejemplo establece el valor de `SaveTokens` a `true` en <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:</span><span class="sxs-lookup"><span data-stu-id="f135b-155">The sample app sets the value of `SaveTokens` to `true` in <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=15)]

<span data-ttu-id="f135b-156">Cuando `OnPostConfirmationAsync` se ejecuta, almacenar el token de acceso ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) del proveedor externo en el `ApplicationUser`del `AuthenticationProperties`.</span><span class="sxs-lookup"><span data-stu-id="f135b-156">When `OnPostConfirmationAsync` executes, store the access token ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) from the external provider in the `ApplicationUser`'s `AuthenticationProperties`.</span></span>

<span data-ttu-id="f135b-157">La aplicación de ejemplo guarda el token de acceso en `OnPostConfirmationAsync` (nuevo registro de usuario) y `OnGetCallbackAsync` (usuario registrado anteriormente) en *Account/ExternalLogin.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="f135b-157">The sample app saves the access token in `OnPostConfirmationAsync` (new user registration) and `OnGetCallbackAsync` (previously registered user) in *Account/ExternalLogin.cshtml.cs*:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=54-56)]

## <a name="how-to-add-additional-custom-tokens"></a><span data-ttu-id="f135b-158">Cómo agregar tokens personalizados adicionales</span><span class="sxs-lookup"><span data-stu-id="f135b-158">How to add additional custom tokens</span></span>

<span data-ttu-id="f135b-159">Para demostrar cómo agregar un token personalizado, que se almacena como parte de `SaveTokens`, la aplicación de ejemplo agrega un <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> con el actual <xref:System.DateTime> para un [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) de `TicketCreated`:</span><span class="sxs-lookup"><span data-stu-id="f135b-159">To demonstrate how to add a custom token, which is stored as part of `SaveTokens`, the sample app adds an <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> with the current <xref:System.DateTime> for an [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) of `TicketCreated`:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=17-28)]

## <a name="creating-and-adding-claims"></a><span data-ttu-id="f135b-160">Creación y la incorporación de notificaciones</span><span class="sxs-lookup"><span data-stu-id="f135b-160">Creating and adding claims</span></span>

<span data-ttu-id="f135b-161">El marco de trabajo proporciona acciones comunes y métodos de extensión para crear y agregar notificaciones a la colección.</span><span class="sxs-lookup"><span data-stu-id="f135b-161">The framework provides common actions and extension methods for creating and adding claims to the collection.</span></span> <span data-ttu-id="f135b-162">Para obtener más información, vea <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions> y <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionUniqueExtensions>.</span><span class="sxs-lookup"><span data-stu-id="f135b-162">For more information, see the <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions> and <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionUniqueExtensions>.</span></span>

<span data-ttu-id="f135b-163">Los usuarios pueden definir las acciones personalizadas mediante la derivación de <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction> e implementación abstracta <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run*> método.</span><span class="sxs-lookup"><span data-stu-id="f135b-163">Users can define custom actions by deriving from <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction> and implementing the abstract <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run*> method.</span></span>

<span data-ttu-id="f135b-164">Para obtener más información, consulta <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims>.</span><span class="sxs-lookup"><span data-stu-id="f135b-164">For more information, see <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims>.</span></span>

## <a name="removal-of-claim-actions-and-claims"></a><span data-ttu-id="f135b-165">Eliminación de las acciones de notificación y notificaciones</span><span class="sxs-lookup"><span data-stu-id="f135b-165">Removal of claim actions and claims</span></span>

<span data-ttu-id="f135b-166">[ClaimActionCollection.Remove(String)](xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection.Remove*) quita todas las notificaciones de acciones para el determinado <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> de la colección.</span><span class="sxs-lookup"><span data-stu-id="f135b-166">[ClaimActionCollection.Remove(String)](xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection.Remove*) removes all claim actions for the given <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> from the collection.</span></span> <span data-ttu-id="f135b-167">[ClaimActionCollectionMapExtensions.DeleteClaim (ClaimActionCollection, String)](xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*) elimina una notificación de la dada <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> de la identidad.</span><span class="sxs-lookup"><span data-stu-id="f135b-167">[ClaimActionCollectionMapExtensions.DeleteClaim(ClaimActionCollection, String)](xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*) deletes a claim of the given <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> from the identity.</span></span> <span data-ttu-id="f135b-168"><xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*> se usa principalmente con [OpenID Connect (OIDC)](/azure/active-directory/develop/v2-protocols-oidc) para quitar las notificaciones generadas por el protocolo.</span><span class="sxs-lookup"><span data-stu-id="f135b-168"><xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*> is primarily used with [OpenID Connect (OIDC)](/azure/active-directory/develop/v2-protocols-oidc) to remove protocol-generated claims.</span></span>

## <a name="sample-app-output"></a><span data-ttu-id="f135b-169">Salida de la aplicación de ejemplo</span><span class="sxs-lookup"><span data-stu-id="f135b-169">Sample app output</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="f135b-170">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="f135b-170">Additional resources</span></span>

* <span data-ttu-id="f135b-171">[aplicación SocialSample ingeniería de ASPNET/AspNetCore](https://github.com/aspnet/AspNetCore/tree/master/src/Security/Authentication/samples/SocialSample) &ndash; es la aplicación de ejemplo vinculado en el [del repositorio de GitHub de aspnet/AspNetCore](https://github.com/aspnet/AspNetCore) `master` rama ingeniería.</span><span class="sxs-lookup"><span data-stu-id="f135b-171">[aspnet/AspNetCore engineering SocialSample app](https://github.com/aspnet/AspNetCore/tree/master/src/Security/Authentication/samples/SocialSample) &ndash; The linked sample app is on the [aspnet/AspNetCore GitHub repo's](https://github.com/aspnet/AspNetCore) `master` engineering branch.</span></span> <span data-ttu-id="f135b-172">El `master` rama contiene código en desarrollo activo para la próxima versión de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f135b-172">The `master` branch contains code under active development for the next release of ASP.NET Core.</span></span> <span data-ttu-id="f135b-173">Para ver una versión de la aplicación de ejemplo para una versión comercial de ASP.NET Core, use el **rama** lista desplegable lista para seleccionar una rama de versión (por ejemplo `release/2.2`).</span><span class="sxs-lookup"><span data-stu-id="f135b-173">To see a version of the sample app for a released version of ASP.NET Core, use the **Branch** drop down list to select a release branch (for example `release/2.2`).</span></span>
