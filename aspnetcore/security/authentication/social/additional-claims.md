---
title: Conservar notificaciones y tokens adicionales de proveedores externos en ASP.NET Core
author: guardrex
description: Obtenga información sobre cómo establecer notificaciones y tokens adicionales de proveedores externos.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/01/2019
uid: security/authentication/social/additional-claims
ms.openlocfilehash: cdf263df8d1aa17ea3820a16ecbd10abce9d683d
ms.sourcegitcommit: 73e255e846e414821b8cc20ffa3aec946735cd4e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/03/2019
ms.locfileid: "71925153"
---
# <a name="persist-additional-claims-and-tokens-from-external-providers-in-aspnet-core"></a><span data-ttu-id="3fa0f-103">Conservar notificaciones y tokens adicionales de proveedores externos en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3fa0f-103">Persist additional claims and tokens from external providers in ASP.NET Core</span></span>

<span data-ttu-id="3fa0f-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="3fa0f-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="3fa0f-105">Una aplicación ASP.NET Core puede establecer notificaciones y tokens adicionales de proveedores de autenticación externos, como Facebook, Google, Microsoft y Twitter.</span><span class="sxs-lookup"><span data-stu-id="3fa0f-105">An ASP.NET Core app can establish additional claims and tokens from external authentication providers, such as Facebook, Google, Microsoft, and Twitter.</span></span> <span data-ttu-id="3fa0f-106">Cada proveedor revela información diferente sobre los usuarios en su plataforma, pero el patrón para recibir y transformar los datos de usuario en notificaciones adicionales es el mismo.</span><span class="sxs-lookup"><span data-stu-id="3fa0f-106">Each provider reveals different information about users on its platform, but the pattern for receiving and transforming user data into additional claims is the same.</span></span>

<span data-ttu-id="3fa0f-107">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3fa0f-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3fa0f-108">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="3fa0f-108">Prerequisites</span></span>

<span data-ttu-id="3fa0f-109">Decida qué proveedores de autenticación externos admitir en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3fa0f-109">Decide which external authentication providers to support in the app.</span></span> <span data-ttu-id="3fa0f-110">Para cada proveedor, registre la aplicación y obtenga un identificador de cliente y un secreto de cliente.</span><span class="sxs-lookup"><span data-stu-id="3fa0f-110">For each provider, register the app and obtain a client ID and client secret.</span></span> <span data-ttu-id="3fa0f-111">Para obtener más información, consulta <xref:security/authentication/social/index>.</span><span class="sxs-lookup"><span data-stu-id="3fa0f-111">For more information, see <xref:security/authentication/social/index>.</span></span> <span data-ttu-id="3fa0f-112">La aplicación de ejemplo usa el [proveedor de autenticación de Google](xref:security/authentication/google-logins).</span><span class="sxs-lookup"><span data-stu-id="3fa0f-112">The sample app uses the [Google authentication provider](xref:security/authentication/google-logins).</span></span>

## <a name="set-the-client-id-and-client-secret"></a><span data-ttu-id="3fa0f-113">Establecimiento del identificador de cliente y el secreto de cliente</span><span class="sxs-lookup"><span data-stu-id="3fa0f-113">Set the client ID and client secret</span></span>

<span data-ttu-id="3fa0f-114">El proveedor de autenticación OAuth establece una relación de confianza con una aplicación que usa un identificador de cliente y un secreto de cliente.</span><span class="sxs-lookup"><span data-stu-id="3fa0f-114">The OAuth authentication provider establishes a trust relationship with an app using a client ID and client secret.</span></span> <span data-ttu-id="3fa0f-115">El proveedor de autenticación externo crea los valores de identificador de cliente y secreto de cliente para la aplicación cuando la aplicación se registra con el proveedor.</span><span class="sxs-lookup"><span data-stu-id="3fa0f-115">Client ID and client secret values are created for the app by the external authentication provider when the app is registered with the provider.</span></span> <span data-ttu-id="3fa0f-116">Cada proveedor externo que use la aplicación debe configurarse de forma independiente con el identificador de cliente y el secreto de cliente del proveedor.</span><span class="sxs-lookup"><span data-stu-id="3fa0f-116">Each external provider that the app uses must be configured independently with the provider's client ID and client secret.</span></span> <span data-ttu-id="3fa0f-117">Para obtener más información, consulte los temas de proveedores de autenticación externos que se aplican a su escenario:</span><span class="sxs-lookup"><span data-stu-id="3fa0f-117">For more information, see the external authentication provider topics that apply to your scenario:</span></span>

* [<span data-ttu-id="3fa0f-118">Autenticación con Facebook</span><span class="sxs-lookup"><span data-stu-id="3fa0f-118">Facebook authentication</span></span>](xref:security/authentication/facebook-logins)
* [<span data-ttu-id="3fa0f-119">Autenticación con Google</span><span class="sxs-lookup"><span data-stu-id="3fa0f-119">Google authentication</span></span>](xref:security/authentication/google-logins)
* [<span data-ttu-id="3fa0f-120">Autenticación con Microsoft</span><span class="sxs-lookup"><span data-stu-id="3fa0f-120">Microsoft authentication</span></span>](xref:security/authentication/microsoft-logins)
* [<span data-ttu-id="3fa0f-121">Autenticación con Twitter</span><span class="sxs-lookup"><span data-stu-id="3fa0f-121">Twitter authentication</span></span>](xref:security/authentication/twitter-logins)
* [<span data-ttu-id="3fa0f-122">Otros proveedores de autenticación</span><span class="sxs-lookup"><span data-stu-id="3fa0f-122">Other authentication providers</span></span>](xref:security/authentication/otherlogins)
* [<span data-ttu-id="3fa0f-123">OpenIdConnect</span><span class="sxs-lookup"><span data-stu-id="3fa0f-123">OpenIdConnect</span></span>](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2)

<span data-ttu-id="3fa0f-124">La aplicación de ejemplo configura el proveedor de autenticación de Google con un identificador de cliente y un secreto de cliente proporcionado por Google:</span><span class="sxs-lookup"><span data-stu-id="3fa0f-124">The sample app configures the Google authentication provider with a client ID and client secret provided by Google:</span></span>

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=4,9)]

## <a name="establish-the-authentication-scope"></a><span data-ttu-id="3fa0f-125">Establecer el ámbito de autenticación</span><span class="sxs-lookup"><span data-stu-id="3fa0f-125">Establish the authentication scope</span></span>

<span data-ttu-id="3fa0f-126">Especifique la lista de permisos que se van a recuperar del proveedor especificando el <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>.</span><span class="sxs-lookup"><span data-stu-id="3fa0f-126">Specify the list of permissions to retrieve from the provider by specifying the <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>.</span></span> <span data-ttu-id="3fa0f-127">Los ámbitos de autenticación para proveedores externos comunes aparecen en la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="3fa0f-127">Authentication scopes for common external providers appear in the following table.</span></span>

| <span data-ttu-id="3fa0f-128">Proveedor</span><span class="sxs-lookup"><span data-stu-id="3fa0f-128">Provider</span></span>  | <span data-ttu-id="3fa0f-129">Scope</span><span class="sxs-lookup"><span data-stu-id="3fa0f-129">Scope</span></span>                                                            |
| --------- | ---------------------------------------------------------------- |
| <span data-ttu-id="3fa0f-130">Facebook</span><span class="sxs-lookup"><span data-stu-id="3fa0f-130">Facebook</span></span>  | `https://www.facebook.com/dialog/oauth`                          |
| <span data-ttu-id="3fa0f-131">Google</span><span class="sxs-lookup"><span data-stu-id="3fa0f-131">Google</span></span>    | `https://www.googleapis.com/auth/userinfo.profile`               |
| <span data-ttu-id="3fa0f-132">Microsoft</span><span class="sxs-lookup"><span data-stu-id="3fa0f-132">Microsoft</span></span> | `https://login.microsoftonline.com/common/oauth2/v2.0/authorize` |
| <span data-ttu-id="3fa0f-133">Twitter</span><span class="sxs-lookup"><span data-stu-id="3fa0f-133">Twitter</span></span>   | `https://api.twitter.com/oauth/authenticate`                     |

<span data-ttu-id="3fa0f-134">En la aplicación de ejemplo, el marco de trabajo agrega automáticamente el ámbito `userinfo.profile` de Google cuando se llama a <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> en el @no__t 2.</span><span class="sxs-lookup"><span data-stu-id="3fa0f-134">In the sample app, Google's `userinfo.profile` scope is automatically added by the framework when <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> is called on the <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder>.</span></span> <span data-ttu-id="3fa0f-135">Si la aplicación requiere ámbitos adicionales, agréguelos a las opciones.</span><span class="sxs-lookup"><span data-stu-id="3fa0f-135">If the app requires additional scopes, add them to the options.</span></span> <span data-ttu-id="3fa0f-136">En el ejemplo siguiente, se agrega el ámbito de Google `https://www.googleapis.com/auth/user.birthday.read` para recuperar el cumpleaños de un usuario:</span><span class="sxs-lookup"><span data-stu-id="3fa0f-136">In the following example, the Google `https://www.googleapis.com/auth/user.birthday.read` scope is added in order to retrieve a user's birthday:</span></span>

```csharp
options.Scope.Add("https://www.googleapis.com/auth/user.birthday.read");
```

## <a name="map-user-data-keys-and-create-claims"></a><span data-ttu-id="3fa0f-137">Asignar claves de datos de usuario y crear notificaciones</span><span class="sxs-lookup"><span data-stu-id="3fa0f-137">Map user data keys and create claims</span></span>

<span data-ttu-id="3fa0f-138">En las opciones del proveedor, especifique un <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> o <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonSubKey*> para cada clave o subclave en los datos de usuario JSON del proveedor externo para que la identidad de la aplicación Lea el inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="3fa0f-138">In the provider's options, specify a <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> or <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonSubKey*> for each key/subkey in the external provider's JSON user data for the app identity to read on sign in.</span></span> <span data-ttu-id="3fa0f-139">Para obtener más información sobre los tipos de notificaciones, vea <xref:System.Security.Claims.ClaimTypes>.</span><span class="sxs-lookup"><span data-stu-id="3fa0f-139">For more information on claim types, see <xref:System.Security.Claims.ClaimTypes>.</span></span>

<span data-ttu-id="3fa0f-140">La aplicación de ejemplo crea notificaciones de configuración regional (`urn:google:locale`) e imagen (`urn:google:picture`) de las claves @no__t 2 y `picture` en los datos de usuario de Google:</span><span class="sxs-lookup"><span data-stu-id="3fa0f-140">The sample app creates locale (`urn:google:locale`) and picture (`urn:google:picture`) claims from the `locale` and `picture` keys in Google user data:</span></span>

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=13-14)]

<span data-ttu-id="3fa0f-141">En <xref:Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync*>, un <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) se inicia sesión en la aplicación con <xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*>.</span><span class="sxs-lookup"><span data-stu-id="3fa0f-141">In <xref:Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync*>, an <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) is signed into the app with <xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*>.</span></span> <span data-ttu-id="3fa0f-142">Durante el proceso de inicio de sesión, el <xref:Microsoft.AspNetCore.Identity.UserManager%601> puede almacenar notificaciones `ApplicationUser` para los datos de usuario disponibles en el @no__t 2.</span><span class="sxs-lookup"><span data-stu-id="3fa0f-142">During the sign in process, the <xref:Microsoft.AspNetCore.Identity.UserManager%601> can store an `ApplicationUser` claims for user data available from the <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.</span></span>

<span data-ttu-id="3fa0f-143">En la aplicación de ejemplo, `OnPostConfirmationAsync` (*account/ExternalLogin. cshtml. CS*) establece las notificaciones de configuración regional (`urn:google:locale`) e imagen (`urn:google:picture`) del `ApplicationUser` con sesión iniciada, incluida una notificación de <xref:System.Security.Claims.ClaimTypes.GivenName>:</span><span class="sxs-lookup"><span data-stu-id="3fa0f-143">In the sample app, `OnPostConfirmationAsync` (*Account/ExternalLogin.cshtml.cs*) establishes the locale (`urn:google:locale`) and picture (`urn:google:picture`) claims for the signed in `ApplicationUser`, including a claim for <xref:System.Security.Claims.ClaimTypes.GivenName>:</span></span>

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=35-51)]

<span data-ttu-id="3fa0f-144">De forma predeterminada, las notificaciones de un usuario se almacenan en la cookie de autenticación.</span><span class="sxs-lookup"><span data-stu-id="3fa0f-144">By default, a user's claims are stored in the authentication cookie.</span></span> <span data-ttu-id="3fa0f-145">Si la cookie de autenticación es demasiado grande, puede provocar un error en la aplicación porque:</span><span class="sxs-lookup"><span data-stu-id="3fa0f-145">If the authentication cookie is too large, it can cause the app to fail because:</span></span>

* <span data-ttu-id="3fa0f-146">El explorador detecta que el encabezado de la cookie es demasiado largo.</span><span class="sxs-lookup"><span data-stu-id="3fa0f-146">The browser detects that the cookie header is too long.</span></span>
* <span data-ttu-id="3fa0f-147">El tamaño total de la solicitud es demasiado grande.</span><span class="sxs-lookup"><span data-stu-id="3fa0f-147">The overall size of the request is too large.</span></span>

<span data-ttu-id="3fa0f-148">Si se requiere una gran cantidad de datos de usuario para el procesamiento de solicitudes de usuario:</span><span class="sxs-lookup"><span data-stu-id="3fa0f-148">If a large amount of user data is required for processing user requests:</span></span>

* <span data-ttu-id="3fa0f-149">Limite el número y el tamaño de las notificaciones de usuario para el procesamiento de solicitudes solo a lo que requiere la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3fa0f-149">Limit the number and size of user claims for request processing to only what the app requires.</span></span>
* <span data-ttu-id="3fa0f-150">Use un @no__t personalizado-0 para el middleware de autenticación de cookies <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.SessionStore> para almacenar la identidad entre las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="3fa0f-150">Use a custom <xref:Microsoft.AspNetCore.Authentication.Cookies.ITicketStore> for the Cookie Authentication Middleware's <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.SessionStore> to store identity across requests.</span></span> <span data-ttu-id="3fa0f-151">Conservar grandes cantidades de información de identidad en el servidor y enviar solo una pequeña clave de identificador de sesión al cliente.</span><span class="sxs-lookup"><span data-stu-id="3fa0f-151">Preserve large quantities of identity information on the server while only sending a small session identifier key to the client.</span></span>

## <a name="save-the-access-token"></a><span data-ttu-id="3fa0f-152">Guardar el token de acceso</span><span class="sxs-lookup"><span data-stu-id="3fa0f-152">Save the access token</span></span>

<span data-ttu-id="3fa0f-153"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> define si los tokens de acceso y de actualización deben almacenarse en el <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> después de una autorización correcta.</span><span class="sxs-lookup"><span data-stu-id="3fa0f-153"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> defines whether access and refresh tokens should be stored in the <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> after a successful authorization.</span></span> <span data-ttu-id="3fa0f-154">de forma predeterminada, `SaveTokens` se establece en `false` para reducir el tamaño de la cookie de autenticación final.</span><span class="sxs-lookup"><span data-stu-id="3fa0f-154">`SaveTokens` is set to `false` by default to reduce the size of the final authentication cookie.</span></span>

<span data-ttu-id="3fa0f-155">La aplicación de ejemplo establece el valor de `SaveTokens` en `true` en <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:</span><span class="sxs-lookup"><span data-stu-id="3fa0f-155">The sample app sets the value of `SaveTokens` to `true` in <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:</span></span>

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=15)]

<span data-ttu-id="3fa0f-156">Cuando se ejecuta `OnPostConfirmationAsync`, almacene el token de acceso ([ExternalLoginInfo. AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) del proveedor externo en `AuthenticationProperties` de `ApplicationUser`.</span><span class="sxs-lookup"><span data-stu-id="3fa0f-156">When `OnPostConfirmationAsync` executes, store the access token ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) from the external provider in the `ApplicationUser`'s `AuthenticationProperties`.</span></span>

<span data-ttu-id="3fa0f-157">La aplicación de ejemplo guarda el token de acceso en `OnPostConfirmationAsync` (nuevo registro de usuario) y `OnGetCallbackAsync` (usuario registrado previamente) en *account/ExternalLogin. cshtml. CS*:</span><span class="sxs-lookup"><span data-stu-id="3fa0f-157">The sample app saves the access token in `OnPostConfirmationAsync` (new user registration) and `OnGetCallbackAsync` (previously registered user) in *Account/ExternalLogin.cshtml.cs*:</span></span>

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=54-56)]

## <a name="how-to-add-additional-custom-tokens"></a><span data-ttu-id="3fa0f-158">Cómo agregar tokens personalizados adicionales</span><span class="sxs-lookup"><span data-stu-id="3fa0f-158">How to add additional custom tokens</span></span>

<span data-ttu-id="3fa0f-159">Para mostrar cómo agregar un token personalizado, que se almacena como parte de `SaveTokens`, la aplicación de ejemplo agrega un <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> con el @no__t actual-2 para un [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) de `TicketCreated`:</span><span class="sxs-lookup"><span data-stu-id="3fa0f-159">To demonstrate how to add a custom token, which is stored as part of `SaveTokens`, the sample app adds an <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> with the current <xref:System.DateTime> for an [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) of `TicketCreated`:</span></span>

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=17-30)]

## <a name="creating-and-adding-claims"></a><span data-ttu-id="3fa0f-160">Creación y adición de notificaciones</span><span class="sxs-lookup"><span data-stu-id="3fa0f-160">Creating and adding claims</span></span>

<span data-ttu-id="3fa0f-161">El marco de trabajo proporciona acciones comunes y métodos de extensión para crear y agregar notificaciones a la colección.</span><span class="sxs-lookup"><span data-stu-id="3fa0f-161">The framework provides common actions and extension methods for creating and adding claims to the collection.</span></span> <span data-ttu-id="3fa0f-162">Para obtener más información, vea <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions> y <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionUniqueExtensions>.</span><span class="sxs-lookup"><span data-stu-id="3fa0f-162">For more information, see the <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions> and <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionUniqueExtensions>.</span></span>

<span data-ttu-id="3fa0f-163">Los usuarios pueden definir acciones personalizadas derivando de <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction> e implementando el método Abstract <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run*>.</span><span class="sxs-lookup"><span data-stu-id="3fa0f-163">Users can define custom actions by deriving from <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction> and implementing the abstract <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run*> method.</span></span>

<span data-ttu-id="3fa0f-164">Para obtener más información, consulta <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims>.</span><span class="sxs-lookup"><span data-stu-id="3fa0f-164">For more information, see <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims>.</span></span>

## <a name="removal-of-claim-actions-and-claims"></a><span data-ttu-id="3fa0f-165">Eliminación de las notificaciones y las acciones de notificación</span><span class="sxs-lookup"><span data-stu-id="3fa0f-165">Removal of claim actions and claims</span></span>

<span data-ttu-id="3fa0f-166">[ClaimActionCollection. Remove (String)](xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection.Remove*) quita todas las acciones de notificaciones para el @no__t especificado-1 de la colección.</span><span class="sxs-lookup"><span data-stu-id="3fa0f-166">[ClaimActionCollection.Remove(String)](xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection.Remove*) removes all claim actions for the given <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> from the collection.</span></span> <span data-ttu-id="3fa0f-167">[ClaimActionCollectionMapExtensions. DeleteClaim (ClaimActionCollection, String)](xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*) elimina una demanda del <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> especificado de la identidad.</span><span class="sxs-lookup"><span data-stu-id="3fa0f-167">[ClaimActionCollectionMapExtensions.DeleteClaim(ClaimActionCollection, String)](xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*) deletes a claim of the given <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> from the identity.</span></span> <span data-ttu-id="3fa0f-168"><xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*> se usa principalmente con [OpenID Connect (OIDC)](/azure/active-directory/develop/v2-protocols-oidc) para quitar notificaciones generadas por el protocolo.</span><span class="sxs-lookup"><span data-stu-id="3fa0f-168"><xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*> is primarily used with [OpenID Connect (OIDC)](/azure/active-directory/develop/v2-protocols-oidc) to remove protocol-generated claims.</span></span>

## <a name="sample-app-output"></a><span data-ttu-id="3fa0f-169">Salida de la aplicación de ejemplo</span><span class="sxs-lookup"><span data-stu-id="3fa0f-169">Sample app output</span></span>

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

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="3fa0f-170">Una aplicación ASP.NET Core puede establecer notificaciones y tokens adicionales de proveedores de autenticación externos, como Facebook, Google, Microsoft y Twitter.</span><span class="sxs-lookup"><span data-stu-id="3fa0f-170">An ASP.NET Core app can establish additional claims and tokens from external authentication providers, such as Facebook, Google, Microsoft, and Twitter.</span></span> <span data-ttu-id="3fa0f-171">Cada proveedor revela información diferente sobre los usuarios en su plataforma, pero el patrón para recibir y transformar los datos de usuario en notificaciones adicionales es el mismo.</span><span class="sxs-lookup"><span data-stu-id="3fa0f-171">Each provider reveals different information about users on its platform, but the pattern for receiving and transforming user data into additional claims is the same.</span></span>

<span data-ttu-id="3fa0f-172">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3fa0f-172">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3fa0f-173">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="3fa0f-173">Prerequisites</span></span>

<span data-ttu-id="3fa0f-174">Decida qué proveedores de autenticación externos admitir en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3fa0f-174">Decide which external authentication providers to support in the app.</span></span> <span data-ttu-id="3fa0f-175">Para cada proveedor, registre la aplicación y obtenga un identificador de cliente y un secreto de cliente.</span><span class="sxs-lookup"><span data-stu-id="3fa0f-175">For each provider, register the app and obtain a client ID and client secret.</span></span> <span data-ttu-id="3fa0f-176">Para obtener más información, consulta <xref:security/authentication/social/index>.</span><span class="sxs-lookup"><span data-stu-id="3fa0f-176">For more information, see <xref:security/authentication/social/index>.</span></span> <span data-ttu-id="3fa0f-177">La aplicación de ejemplo usa el [proveedor de autenticación de Google](xref:security/authentication/google-logins).</span><span class="sxs-lookup"><span data-stu-id="3fa0f-177">The sample app uses the [Google authentication provider](xref:security/authentication/google-logins).</span></span>

## <a name="set-the-client-id-and-client-secret"></a><span data-ttu-id="3fa0f-178">Establecimiento del identificador de cliente y el secreto de cliente</span><span class="sxs-lookup"><span data-stu-id="3fa0f-178">Set the client ID and client secret</span></span>

<span data-ttu-id="3fa0f-179">El proveedor de autenticación OAuth establece una relación de confianza con una aplicación que usa un identificador de cliente y un secreto de cliente.</span><span class="sxs-lookup"><span data-stu-id="3fa0f-179">The OAuth authentication provider establishes a trust relationship with an app using a client ID and client secret.</span></span> <span data-ttu-id="3fa0f-180">El proveedor de autenticación externo crea los valores de identificador de cliente y secreto de cliente para la aplicación cuando la aplicación se registra con el proveedor.</span><span class="sxs-lookup"><span data-stu-id="3fa0f-180">Client ID and client secret values are created for the app by the external authentication provider when the app is registered with the provider.</span></span> <span data-ttu-id="3fa0f-181">Cada proveedor externo que use la aplicación debe configurarse de forma independiente con el identificador de cliente y el secreto de cliente del proveedor.</span><span class="sxs-lookup"><span data-stu-id="3fa0f-181">Each external provider that the app uses must be configured independently with the provider's client ID and client secret.</span></span> <span data-ttu-id="3fa0f-182">Para obtener más información, consulte los temas de proveedores de autenticación externos que se aplican a su escenario:</span><span class="sxs-lookup"><span data-stu-id="3fa0f-182">For more information, see the external authentication provider topics that apply to your scenario:</span></span>

* [<span data-ttu-id="3fa0f-183">Autenticación con Facebook</span><span class="sxs-lookup"><span data-stu-id="3fa0f-183">Facebook authentication</span></span>](xref:security/authentication/facebook-logins)
* [<span data-ttu-id="3fa0f-184">Autenticación con Google</span><span class="sxs-lookup"><span data-stu-id="3fa0f-184">Google authentication</span></span>](xref:security/authentication/google-logins)
* [<span data-ttu-id="3fa0f-185">Autenticación con Microsoft</span><span class="sxs-lookup"><span data-stu-id="3fa0f-185">Microsoft authentication</span></span>](xref:security/authentication/microsoft-logins)
* [<span data-ttu-id="3fa0f-186">Autenticación con Twitter</span><span class="sxs-lookup"><span data-stu-id="3fa0f-186">Twitter authentication</span></span>](xref:security/authentication/twitter-logins)
* [<span data-ttu-id="3fa0f-187">Otros proveedores de autenticación</span><span class="sxs-lookup"><span data-stu-id="3fa0f-187">Other authentication providers</span></span>](xref:security/authentication/otherlogins)
* [<span data-ttu-id="3fa0f-188">OpenIdConnect</span><span class="sxs-lookup"><span data-stu-id="3fa0f-188">OpenIdConnect</span></span>](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2)

<span data-ttu-id="3fa0f-189">La aplicación de ejemplo configura el proveedor de autenticación de Google con un identificador de cliente y un secreto de cliente proporcionado por Google:</span><span class="sxs-lookup"><span data-stu-id="3fa0f-189">The sample app configures the Google authentication provider with a client ID and client secret provided by Google:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=4,9)]

## <a name="establish-the-authentication-scope"></a><span data-ttu-id="3fa0f-190">Establecer el ámbito de autenticación</span><span class="sxs-lookup"><span data-stu-id="3fa0f-190">Establish the authentication scope</span></span>

<span data-ttu-id="3fa0f-191">Especifique la lista de permisos que se van a recuperar del proveedor especificando el <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>.</span><span class="sxs-lookup"><span data-stu-id="3fa0f-191">Specify the list of permissions to retrieve from the provider by specifying the <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>.</span></span> <span data-ttu-id="3fa0f-192">Los ámbitos de autenticación para proveedores externos comunes aparecen en la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="3fa0f-192">Authentication scopes for common external providers appear in the following table.</span></span>

| <span data-ttu-id="3fa0f-193">Proveedor</span><span class="sxs-lookup"><span data-stu-id="3fa0f-193">Provider</span></span>  | <span data-ttu-id="3fa0f-194">Scope</span><span class="sxs-lookup"><span data-stu-id="3fa0f-194">Scope</span></span>                                                            |
| --------- | ---------------------------------------------------------------- |
| <span data-ttu-id="3fa0f-195">Facebook</span><span class="sxs-lookup"><span data-stu-id="3fa0f-195">Facebook</span></span>  | `https://www.facebook.com/dialog/oauth`                          |
| <span data-ttu-id="3fa0f-196">Google</span><span class="sxs-lookup"><span data-stu-id="3fa0f-196">Google</span></span>    | `https://www.googleapis.com/auth/userinfo.profile`               |
| <span data-ttu-id="3fa0f-197">Microsoft</span><span class="sxs-lookup"><span data-stu-id="3fa0f-197">Microsoft</span></span> | `https://login.microsoftonline.com/common/oauth2/v2.0/authorize` |
| <span data-ttu-id="3fa0f-198">Twitter</span><span class="sxs-lookup"><span data-stu-id="3fa0f-198">Twitter</span></span>   | `https://api.twitter.com/oauth/authenticate`                     |

<span data-ttu-id="3fa0f-199">En la aplicación de ejemplo, el marco de trabajo agrega automáticamente el ámbito `userinfo.profile` de Google cuando se llama a <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> en el @no__t 2.</span><span class="sxs-lookup"><span data-stu-id="3fa0f-199">In the sample app, Google's `userinfo.profile` scope is automatically added by the framework when <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> is called on the <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder>.</span></span> <span data-ttu-id="3fa0f-200">Si la aplicación requiere ámbitos adicionales, agréguelos a las opciones.</span><span class="sxs-lookup"><span data-stu-id="3fa0f-200">If the app requires additional scopes, add them to the options.</span></span> <span data-ttu-id="3fa0f-201">En el ejemplo siguiente, se agrega el ámbito de Google `https://www.googleapis.com/auth/user.birthday.read` para recuperar el cumpleaños de un usuario:</span><span class="sxs-lookup"><span data-stu-id="3fa0f-201">In the following example, the Google `https://www.googleapis.com/auth/user.birthday.read` scope is added in order to retrieve a user's birthday:</span></span>

```csharp
options.Scope.Add("https://www.googleapis.com/auth/user.birthday.read");
```

## <a name="map-user-data-keys-and-create-claims"></a><span data-ttu-id="3fa0f-202">Asignar claves de datos de usuario y crear notificaciones</span><span class="sxs-lookup"><span data-stu-id="3fa0f-202">Map user data keys and create claims</span></span>

<span data-ttu-id="3fa0f-203">En las opciones del proveedor, especifique un <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> o <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonSubKey*> para cada clave o subclave en los datos de usuario JSON del proveedor externo para que la identidad de la aplicación Lea el inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="3fa0f-203">In the provider's options, specify a <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> or <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonSubKey*> for each key/subkey in the external provider's JSON user data for the app identity to read on sign in.</span></span> <span data-ttu-id="3fa0f-204">Para obtener más información sobre los tipos de notificaciones, vea <xref:System.Security.Claims.ClaimTypes>.</span><span class="sxs-lookup"><span data-stu-id="3fa0f-204">For more information on claim types, see <xref:System.Security.Claims.ClaimTypes>.</span></span>

<span data-ttu-id="3fa0f-205">La aplicación de ejemplo crea notificaciones de configuración regional (`urn:google:locale`) e imagen (`urn:google:picture`) de las claves @no__t 2 y `picture` en los datos de usuario de Google:</span><span class="sxs-lookup"><span data-stu-id="3fa0f-205">The sample app creates locale (`urn:google:locale`) and picture (`urn:google:picture`) claims from the `locale` and `picture` keys in Google user data:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=13-14)]

<span data-ttu-id="3fa0f-206">En <xref:Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync*>, un <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) se inicia sesión en la aplicación con <xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*>.</span><span class="sxs-lookup"><span data-stu-id="3fa0f-206">In <xref:Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync*>, an <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) is signed into the app with <xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*>.</span></span> <span data-ttu-id="3fa0f-207">Durante el proceso de inicio de sesión, el <xref:Microsoft.AspNetCore.Identity.UserManager%601> puede almacenar notificaciones `ApplicationUser` para los datos de usuario disponibles en el @no__t 2.</span><span class="sxs-lookup"><span data-stu-id="3fa0f-207">During the sign in process, the <xref:Microsoft.AspNetCore.Identity.UserManager%601> can store an `ApplicationUser` claims for user data available from the <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.</span></span>

<span data-ttu-id="3fa0f-208">En la aplicación de ejemplo, `OnPostConfirmationAsync` (*account/ExternalLogin. cshtml. CS*) establece las notificaciones de configuración regional (`urn:google:locale`) e imagen (`urn:google:picture`) del `ApplicationUser` con sesión iniciada, incluida una notificación de <xref:System.Security.Claims.ClaimTypes.GivenName>:</span><span class="sxs-lookup"><span data-stu-id="3fa0f-208">In the sample app, `OnPostConfirmationAsync` (*Account/ExternalLogin.cshtml.cs*) establishes the locale (`urn:google:locale`) and picture (`urn:google:picture`) claims for the signed in `ApplicationUser`, including a claim for <xref:System.Security.Claims.ClaimTypes.GivenName>:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=35-51)]

<span data-ttu-id="3fa0f-209">De forma predeterminada, las notificaciones de un usuario se almacenan en la cookie de autenticación.</span><span class="sxs-lookup"><span data-stu-id="3fa0f-209">By default, a user's claims are stored in the authentication cookie.</span></span> <span data-ttu-id="3fa0f-210">Si la cookie de autenticación es demasiado grande, puede provocar un error en la aplicación porque:</span><span class="sxs-lookup"><span data-stu-id="3fa0f-210">If the authentication cookie is too large, it can cause the app to fail because:</span></span>

* <span data-ttu-id="3fa0f-211">El explorador detecta que el encabezado de la cookie es demasiado largo.</span><span class="sxs-lookup"><span data-stu-id="3fa0f-211">The browser detects that the cookie header is too long.</span></span>
* <span data-ttu-id="3fa0f-212">El tamaño total de la solicitud es demasiado grande.</span><span class="sxs-lookup"><span data-stu-id="3fa0f-212">The overall size of the request is too large.</span></span>

<span data-ttu-id="3fa0f-213">Si se requiere una gran cantidad de datos de usuario para el procesamiento de solicitudes de usuario:</span><span class="sxs-lookup"><span data-stu-id="3fa0f-213">If a large amount of user data is required for processing user requests:</span></span>

* <span data-ttu-id="3fa0f-214">Limite el número y el tamaño de las notificaciones de usuario para el procesamiento de solicitudes solo a lo que requiere la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3fa0f-214">Limit the number and size of user claims for request processing to only what the app requires.</span></span>
* <span data-ttu-id="3fa0f-215">Use un @no__t personalizado-0 para el middleware de autenticación de cookies <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.SessionStore> para almacenar la identidad entre las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="3fa0f-215">Use a custom <xref:Microsoft.AspNetCore.Authentication.Cookies.ITicketStore> for the Cookie Authentication Middleware's <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.SessionStore> to store identity across requests.</span></span> <span data-ttu-id="3fa0f-216">Conservar grandes cantidades de información de identidad en el servidor y enviar solo una pequeña clave de identificador de sesión al cliente.</span><span class="sxs-lookup"><span data-stu-id="3fa0f-216">Preserve large quantities of identity information on the server while only sending a small session identifier key to the client.</span></span>

## <a name="save-the-access-token"></a><span data-ttu-id="3fa0f-217">Guardar el token de acceso</span><span class="sxs-lookup"><span data-stu-id="3fa0f-217">Save the access token</span></span>

<span data-ttu-id="3fa0f-218"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> define si los tokens de acceso y de actualización deben almacenarse en el <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> después de una autorización correcta.</span><span class="sxs-lookup"><span data-stu-id="3fa0f-218"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> defines whether access and refresh tokens should be stored in the <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> after a successful authorization.</span></span> <span data-ttu-id="3fa0f-219">de forma predeterminada, `SaveTokens` se establece en `false` para reducir el tamaño de la cookie de autenticación final.</span><span class="sxs-lookup"><span data-stu-id="3fa0f-219">`SaveTokens` is set to `false` by default to reduce the size of the final authentication cookie.</span></span>

<span data-ttu-id="3fa0f-220">La aplicación de ejemplo establece el valor de `SaveTokens` en `true` en <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:</span><span class="sxs-lookup"><span data-stu-id="3fa0f-220">The sample app sets the value of `SaveTokens` to `true` in <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=15)]

<span data-ttu-id="3fa0f-221">Cuando se ejecuta `OnPostConfirmationAsync`, almacene el token de acceso ([ExternalLoginInfo. AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) del proveedor externo en `AuthenticationProperties` de `ApplicationUser`.</span><span class="sxs-lookup"><span data-stu-id="3fa0f-221">When `OnPostConfirmationAsync` executes, store the access token ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) from the external provider in the `ApplicationUser`'s `AuthenticationProperties`.</span></span>

<span data-ttu-id="3fa0f-222">La aplicación de ejemplo guarda el token de acceso en `OnPostConfirmationAsync` (nuevo registro de usuario) y `OnGetCallbackAsync` (usuario registrado previamente) en *account/ExternalLogin. cshtml. CS*:</span><span class="sxs-lookup"><span data-stu-id="3fa0f-222">The sample app saves the access token in `OnPostConfirmationAsync` (new user registration) and `OnGetCallbackAsync` (previously registered user) in *Account/ExternalLogin.cshtml.cs*:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=54-56)]

## <a name="how-to-add-additional-custom-tokens"></a><span data-ttu-id="3fa0f-223">Cómo agregar tokens personalizados adicionales</span><span class="sxs-lookup"><span data-stu-id="3fa0f-223">How to add additional custom tokens</span></span>

<span data-ttu-id="3fa0f-224">Para mostrar cómo agregar un token personalizado, que se almacena como parte de `SaveTokens`, la aplicación de ejemplo agrega un <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> con el @no__t actual-2 para un [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) de `TicketCreated`:</span><span class="sxs-lookup"><span data-stu-id="3fa0f-224">To demonstrate how to add a custom token, which is stored as part of `SaveTokens`, the sample app adds an <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> with the current <xref:System.DateTime> for an [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) of `TicketCreated`:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=17-30)]

## <a name="creating-and-adding-claims"></a><span data-ttu-id="3fa0f-225">Creación y adición de notificaciones</span><span class="sxs-lookup"><span data-stu-id="3fa0f-225">Creating and adding claims</span></span>

<span data-ttu-id="3fa0f-226">El marco de trabajo proporciona acciones comunes y métodos de extensión para crear y agregar notificaciones a la colección.</span><span class="sxs-lookup"><span data-stu-id="3fa0f-226">The framework provides common actions and extension methods for creating and adding claims to the collection.</span></span> <span data-ttu-id="3fa0f-227">Para obtener más información, vea <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions> y <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionUniqueExtensions>.</span><span class="sxs-lookup"><span data-stu-id="3fa0f-227">For more information, see the <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions> and <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionUniqueExtensions>.</span></span>

<span data-ttu-id="3fa0f-228">Los usuarios pueden definir acciones personalizadas derivando de <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction> e implementando el método Abstract <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run*>.</span><span class="sxs-lookup"><span data-stu-id="3fa0f-228">Users can define custom actions by deriving from <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction> and implementing the abstract <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run*> method.</span></span>

<span data-ttu-id="3fa0f-229">Para obtener más información, consulta <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims>.</span><span class="sxs-lookup"><span data-stu-id="3fa0f-229">For more information, see <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims>.</span></span>

## <a name="removal-of-claim-actions-and-claims"></a><span data-ttu-id="3fa0f-230">Eliminación de las notificaciones y las acciones de notificación</span><span class="sxs-lookup"><span data-stu-id="3fa0f-230">Removal of claim actions and claims</span></span>

<span data-ttu-id="3fa0f-231">[ClaimActionCollection. Remove (String)](xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection.Remove*) quita todas las acciones de notificaciones para el @no__t especificado-1 de la colección.</span><span class="sxs-lookup"><span data-stu-id="3fa0f-231">[ClaimActionCollection.Remove(String)](xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection.Remove*) removes all claim actions for the given <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> from the collection.</span></span> <span data-ttu-id="3fa0f-232">[ClaimActionCollectionMapExtensions. DeleteClaim (ClaimActionCollection, String)](xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*) elimina una demanda del <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> especificado de la identidad.</span><span class="sxs-lookup"><span data-stu-id="3fa0f-232">[ClaimActionCollectionMapExtensions.DeleteClaim(ClaimActionCollection, String)](xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*) deletes a claim of the given <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> from the identity.</span></span> <span data-ttu-id="3fa0f-233"><xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*> se usa principalmente con [OpenID Connect (OIDC)](/azure/active-directory/develop/v2-protocols-oidc) para quitar notificaciones generadas por el protocolo.</span><span class="sxs-lookup"><span data-stu-id="3fa0f-233"><xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*> is primarily used with [OpenID Connect (OIDC)](/azure/active-directory/develop/v2-protocols-oidc) to remove protocol-generated claims.</span></span>

## <a name="sample-app-output"></a><span data-ttu-id="3fa0f-234">Salida de la aplicación de ejemplo</span><span class="sxs-lookup"><span data-stu-id="3fa0f-234">Sample app output</span></span>

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

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="3fa0f-235">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="3fa0f-235">Additional resources</span></span>

* <span data-ttu-id="3fa0f-236">[ASPNET/AspNetCore Engineering SocialSample app](https://github.com/aspnet/AspNetCore/tree/master/src/Security/Authentication/samples/SocialSample) &ndash; la aplicación de ejemplo vinculada se encuentra en la rama de ingeniería de @no__t 3 [del repositorio de github de ASPNET/AspNetCore](https://github.com/aspnet/AspNetCore) .</span><span class="sxs-lookup"><span data-stu-id="3fa0f-236">[aspnet/AspNetCore engineering SocialSample app](https://github.com/aspnet/AspNetCore/tree/master/src/Security/Authentication/samples/SocialSample) &ndash; The linked sample app is on the [aspnet/AspNetCore GitHub repo's](https://github.com/aspnet/AspNetCore) `master` engineering branch.</span></span> <span data-ttu-id="3fa0f-237">La rama `master` contiene código bajo desarrollo activo para la próxima versión de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3fa0f-237">The `master` branch contains code under active development for the next release of ASP.NET Core.</span></span> <span data-ttu-id="3fa0f-238">Para ver una versión de la aplicación de ejemplo de una versión de lanzamiento de ASP.NET Core, use la lista desplegable **rama** para seleccionar una rama de versión (por ejemplo `release/{X.Y}`).</span><span class="sxs-lookup"><span data-stu-id="3fa0f-238">To see a version of the sample app for a released version of ASP.NET Core, use the **Branch** drop down list to select a release branch (for example `release/{X.Y}`).</span></span>
