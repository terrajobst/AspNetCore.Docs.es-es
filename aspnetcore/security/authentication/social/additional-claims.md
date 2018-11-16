---
title: Conservar notificaciones adicionales y los tokens de proveedores externos en ASP.NET Core
author: guardrex
description: Obtenga información sobre cómo establecer notificaciones adicionales y los tokens de proveedores externos.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 11/11/2018
uid: security/authentication/social/additional-claims
ms.openlocfilehash: 9a24ac138950ef2bedac48f506655d06520137cf
ms.sourcegitcommit: 09bcda59a58019fdf47b2db5259fe87acf19dd38
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2018
ms.locfileid: "51708366"
---
# <a name="persist-additional-claims-and-tokens-from-external-providers-in-aspnet-core"></a><span data-ttu-id="53a0c-103">Conservar notificaciones adicionales y los tokens de proveedores externos en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="53a0c-103">Persist additional claims and tokens from external providers in ASP.NET Core</span></span>

<span data-ttu-id="53a0c-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="53a0c-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="53a0c-105">Una aplicación ASP.NET Core puede establecer notificaciones adicionales y los tokens de proveedores de autenticación externos, como Facebook, Google, Microsoft y Twitter.</span><span class="sxs-lookup"><span data-stu-id="53a0c-105">An ASP.NET Core app can establish additional claims and tokens from external authentication providers, such as Facebook, Google, Microsoft, and Twitter.</span></span> <span data-ttu-id="53a0c-106">Cada proveedor revela información distinta acerca de los usuarios en su plataforma, pero el patrón para recibir y transformar los datos de usuario en notificaciones adicionales es el mismo.</span><span class="sxs-lookup"><span data-stu-id="53a0c-106">Each provider reveals different information about users on its platform, but the pattern for receiving and transforming user data into additional claims is the same.</span></span>

<span data-ttu-id="53a0c-107">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="53a0c-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="53a0c-108">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="53a0c-108">Prerequisites</span></span>

<span data-ttu-id="53a0c-109">Decida qué proveedores de autenticación externos para admitir en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="53a0c-109">Decide which external authentication providers to support in the app.</span></span> <span data-ttu-id="53a0c-110">Para cada proveedor, registrar la aplicación y obtener un identificador de cliente y secreto de cliente.</span><span class="sxs-lookup"><span data-stu-id="53a0c-110">For each provider, register the app and obtain a client ID and client secret.</span></span> <span data-ttu-id="53a0c-111">Para obtener más información, consulta <xref:security/authentication/social/index>.</span><span class="sxs-lookup"><span data-stu-id="53a0c-111">For more information, see <xref:security/authentication/social/index>.</span></span> <span data-ttu-id="53a0c-112">El [aplicación de ejemplo](#sample-app-instructions) usa el [proveedor de autenticación de Google](xref:security/authentication/google-logins).</span><span class="sxs-lookup"><span data-stu-id="53a0c-112">The [sample app](#sample-app-instructions) uses the [Google authentication provider](xref:security/authentication/google-logins).</span></span>

## <a name="set-the-client-id-and-client-secret"></a><span data-ttu-id="53a0c-113">Establece el identificador de cliente y el secreto de cliente</span><span class="sxs-lookup"><span data-stu-id="53a0c-113">Set the client ID and client secret</span></span>

<span data-ttu-id="53a0c-114">El proveedor de autenticación OAuth establece una relación de confianza con una aplicación mediante un identificador de cliente y secreto de cliente.</span><span class="sxs-lookup"><span data-stu-id="53a0c-114">The OAuth authentication provider establishes a trust relationship with an app using a client ID and client secret.</span></span> <span data-ttu-id="53a0c-115">Id. de cliente y los valores de secreto de cliente se crean para la aplicación mediante el proveedor de autenticación externo cuando la aplicación esté registrada con el proveedor.</span><span class="sxs-lookup"><span data-stu-id="53a0c-115">Client ID and client secret values are created for the app by the external authentication provider when the app is registered with the provider.</span></span> <span data-ttu-id="53a0c-116">Cada proveedor externo que usa la aplicación debe configurarse de forma independiente con Id. de cliente y el secreto de cliente del proveedor.</span><span class="sxs-lookup"><span data-stu-id="53a0c-116">Each external provider that the app uses must be configured independently with the provider's client ID and client secret.</span></span> <span data-ttu-id="53a0c-117">Para obtener más información, vea los temas de proveedor de autenticación externo que se aplican a su escenario:</span><span class="sxs-lookup"><span data-stu-id="53a0c-117">For more information, see the external authentication provider topics that apply to your scenario:</span></span>

* [<span data-ttu-id="53a0c-118">Autenticación con Facebook</span><span class="sxs-lookup"><span data-stu-id="53a0c-118">Facebook authentication</span></span>](xref:security/authentication/facebook-logins)
* [<span data-ttu-id="53a0c-119">Autenticación con Google</span><span class="sxs-lookup"><span data-stu-id="53a0c-119">Google authentication</span></span>](xref:security/authentication/google-logins)
* [<span data-ttu-id="53a0c-120">Autenticación con Microsoft</span><span class="sxs-lookup"><span data-stu-id="53a0c-120">Microsoft authentication</span></span>](xref:security/authentication/microsoft-logins)
* [<span data-ttu-id="53a0c-121">Autenticación con Twitter</span><span class="sxs-lookup"><span data-stu-id="53a0c-121">Twitter authentication</span></span>](xref:security/authentication/twitter-logins)
* [<span data-ttu-id="53a0c-122">Otros proveedores de autenticación</span><span class="sxs-lookup"><span data-stu-id="53a0c-122">Other authentication providers</span></span>](xref:security/authentication/otherlogins)
* [<span data-ttu-id="53a0c-123">OpenIdConnect</span><span class="sxs-lookup"><span data-stu-id="53a0c-123">OpenIdConnect</span></span>](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2)

<span data-ttu-id="53a0c-124">La aplicación de ejemplo configura el proveedor de autenticación de Google con un identificador de cliente y secreto de cliente proporcionado por Google:</span><span class="sxs-lookup"><span data-stu-id="53a0c-124">The sample app configures the Google authentication provider with a client ID and client secret provided by Google:</span></span>

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=4,6)]

## <a name="establish-the-authentication-scope"></a><span data-ttu-id="53a0c-125">Establecer el ámbito de autenticación</span><span class="sxs-lookup"><span data-stu-id="53a0c-125">Establish the authentication scope</span></span>

<span data-ttu-id="53a0c-126">Especifique la lista de permisos para recuperar el proveedor especificando el <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>.</span><span class="sxs-lookup"><span data-stu-id="53a0c-126">Specify the list of permissions to retrieve from the provider by specifying the <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>.</span></span> <span data-ttu-id="53a0c-127">Ámbitos de autenticación para los proveedores externos comunes aparecen en la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="53a0c-127">Authentication scopes for common external providers appear in the following table.</span></span>

| <span data-ttu-id="53a0c-128">Proveedor</span><span class="sxs-lookup"><span data-stu-id="53a0c-128">Provider</span></span>  | <span data-ttu-id="53a0c-129">Ámbito</span><span class="sxs-lookup"><span data-stu-id="53a0c-129">Scope</span></span>                                                            |
| --------- | ---------------------------------------------------------------- |
| <span data-ttu-id="53a0c-130">Facebook</span><span class="sxs-lookup"><span data-stu-id="53a0c-130">Facebook</span></span>  | `https://www.facebook.com/dialog/oauth`                          |
| <span data-ttu-id="53a0c-131">Google</span><span class="sxs-lookup"><span data-stu-id="53a0c-131">Google</span></span>    | `https://www.googleapis.com/auth/plus.login`                     |
| <span data-ttu-id="53a0c-132">Microsoft</span><span class="sxs-lookup"><span data-stu-id="53a0c-132">Microsoft</span></span> | `https://login.microsoftonline.com/common/oauth2/v2.0/authorize` |
| <span data-ttu-id="53a0c-133">Twitter</span><span class="sxs-lookup"><span data-stu-id="53a0c-133">Twitter</span></span>   | `https://api.twitter.com/oauth/authenticate`                     |

<span data-ttu-id="53a0c-134">La aplicación de ejemplo agrega Google `plus.login` ámbito de sesión de Google + en los permisos de solicitud:</span><span class="sxs-lookup"><span data-stu-id="53a0c-134">The sample app adds the Google `plus.login` scope to request Google+ sign in permissions:</span></span>

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=7)]

## <a name="map-user-data-keys-and-create-claims"></a><span data-ttu-id="53a0c-135">Asignar claves de datos de usuario y crear notificaciones</span><span class="sxs-lookup"><span data-stu-id="53a0c-135">Map user data keys and create claims</span></span>

<span data-ttu-id="53a0c-136">En las opciones del proveedor, especifique un <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> para cada clave de datos de usuario del proveedor externo JSON para la identidad de aplicación leer en Inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="53a0c-136">In the provider's options, specify a <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> for each key in the external provider's JSON user data for the app identity to read on sign in.</span></span> <span data-ttu-id="53a0c-137">Para obtener más información sobre los tipos de notificación, consulte <xref:System.Security.Claims.ClaimTypes>.</span><span class="sxs-lookup"><span data-stu-id="53a0c-137">For more information on claim types, see <xref:System.Security.Claims.ClaimTypes>.</span></span>

<span data-ttu-id="53a0c-138">La aplicación de ejemplo crea un <xref:System.Security.Claims.ClaimTypes.Gender> notificación desde el `gender` clave en los datos de usuario de Google:</span><span class="sxs-lookup"><span data-stu-id="53a0c-138">The sample app creates a <xref:System.Security.Claims.ClaimTypes.Gender> claim from the `gender` key in Google user data:</span></span>

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=8)]

<span data-ttu-id="53a0c-139">En <xref:Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync*>, un <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) ha iniciado sesión en la aplicación con <xref:Microsoft.AspNetCore.Identity.SignInManager`1.SignInAsync*>.</span><span class="sxs-lookup"><span data-stu-id="53a0c-139">In <xref:Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync*>, an <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) is signed into the app with <xref:Microsoft.AspNetCore.Identity.SignInManager`1.SignInAsync*>.</span></span> <span data-ttu-id="53a0c-140">Durante el inicio de sesión en proceso, el <xref:Microsoft.AspNetCore.Identity.UserManager`1> puede almacenar un `ApplicationUser` de notificación para los datos de usuario disponibles en el <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.</span><span class="sxs-lookup"><span data-stu-id="53a0c-140">During the sign in process, the <xref:Microsoft.AspNetCore.Identity.UserManager`1> can store an `ApplicationUser` claim for user data available from the <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.</span></span>

<span data-ttu-id="53a0c-141">En la aplicación de ejemplo, `OnPostConfirmationAsync` (*Account/ExternalLogin.cshtml.cs*) establece un <xref:System.Security.Claims.ClaimTypes.Gender> de notificación para firmado en `ApplicationUser`:</span><span class="sxs-lookup"><span data-stu-id="53a0c-141">In the sample app, `OnPostConfirmationAsync` (*Account/ExternalLogin.cshtml.cs*) establishes a <xref:System.Security.Claims.ClaimTypes.Gender> claim for the signed in `ApplicationUser`:</span></span>

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=30-31)]

## <a name="save-the-access-token"></a><span data-ttu-id="53a0c-142">Guarde el token de acceso</span><span class="sxs-lookup"><span data-stu-id="53a0c-142">Save the access token</span></span>

<span data-ttu-id="53a0c-143"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> define si se deben almacenar los tokens de acceso y actualización en el <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> después de una autorización correcta.</span><span class="sxs-lookup"><span data-stu-id="53a0c-143"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> defines whether access and refresh tokens should be stored in the <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> after a successful authorization.</span></span> <span data-ttu-id="53a0c-144">`SaveTokens` se establece en `false` de forma predeterminada para reducir el tamaño de la cookie de autenticación final.</span><span class="sxs-lookup"><span data-stu-id="53a0c-144">`SaveTokens` is set to `false` by default to reduce the size of the final authentication cookie.</span></span>

<span data-ttu-id="53a0c-145">La aplicación de ejemplo establece el valor de `SaveTokens` a `true` en <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:</span><span class="sxs-lookup"><span data-stu-id="53a0c-145">The sample app sets the value of `SaveTokens` to `true` in <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:</span></span>

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=9)]

<span data-ttu-id="53a0c-146">Cuando `OnPostConfirmationAsync` se ejecuta, almacenar el token de acceso ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) del proveedor externo en el `ApplicationUser`del `AuthenticationProperties`.</span><span class="sxs-lookup"><span data-stu-id="53a0c-146">When `OnPostConfirmationAsync` executes, store the access token ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) from the external provider in the `ApplicationUser`'s `AuthenticationProperties`.</span></span>

<span data-ttu-id="53a0c-147">La aplicación de ejemplo guarda en el token de acceso:</span><span class="sxs-lookup"><span data-stu-id="53a0c-147">The sample app saves the access token in:</span></span>

* <span data-ttu-id="53a0c-148">`OnPostConfirmationAsync` &ndash; Ejecuta de nuevo registro de usuario.</span><span class="sxs-lookup"><span data-stu-id="53a0c-148">`OnPostConfirmationAsync` &ndash; Executes for new user registration.</span></span>
* <span data-ttu-id="53a0c-149">`OnGetCallbackAsync` &ndash; Se ejecuta cuando inicia sesión un usuario registrado anteriormente en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="53a0c-149">`OnGetCallbackAsync` &ndash; Executes when a previously registered user signs into the app.</span></span>

<span data-ttu-id="53a0c-150">*Account/ExternalLogin.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="53a0c-150">*Account/ExternalLogin.cshtml.cs*:</span></span>

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=34-35)]

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnGetCallbackAsync&highlight=31-32)]

## <a name="how-to-add-additional-custom-tokens"></a><span data-ttu-id="53a0c-151">Cómo agregar tokens personalizados adicionales</span><span class="sxs-lookup"><span data-stu-id="53a0c-151">How to add additional custom tokens</span></span>

<span data-ttu-id="53a0c-152">Para demostrar cómo agregar un token personalizado, que se almacena como parte de `SaveTokens`, la aplicación de ejemplo agrega un <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> con el actual <xref:System.DateTime> para un [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) de `TicketCreated`:</span><span class="sxs-lookup"><span data-stu-id="53a0c-152">To demonstrate how to add a custom token, which is stored as part of `SaveTokens`, the sample app adds an <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> with the current <xref:System.DateTime> for an [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) of `TicketCreated`:</span></span>

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=10-21)]

## <a name="sample-app-instructions"></a><span data-ttu-id="53a0c-153">Instrucciones de la aplicación de ejemplo</span><span class="sxs-lookup"><span data-stu-id="53a0c-153">Sample app instructions</span></span>

<span data-ttu-id="53a0c-154">La aplicación de ejemplo muestra cómo:</span><span class="sxs-lookup"><span data-stu-id="53a0c-154">The sample app demonstrates how to:</span></span>

* <span data-ttu-id="53a0c-155">Obtenga el sexo del usuario de Google y almacena una notificación de género con el valor.</span><span class="sxs-lookup"><span data-stu-id="53a0c-155">Obtain the user's gender from Google and store a gender claim with the value.</span></span>
* <span data-ttu-id="53a0c-156">Store el token de acceso de Google en el usuario `AuthenticationProperties`.</span><span class="sxs-lookup"><span data-stu-id="53a0c-156">Store the Google access token in the user's `AuthenticationProperties`.</span></span>

<span data-ttu-id="53a0c-157">Para usar la aplicación de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="53a0c-157">To use the sample app:</span></span>

1. <span data-ttu-id="53a0c-158">Registrar la aplicación y obtener un identificador de cliente válido y el secreto de cliente para la autenticación de Google.</span><span class="sxs-lookup"><span data-stu-id="53a0c-158">Register the app and obtain a valid client ID and client secret for Google authentication.</span></span> <span data-ttu-id="53a0c-159">Para obtener más información, consulta <xref:security/authentication/google-logins>.</span><span class="sxs-lookup"><span data-stu-id="53a0c-159">For more information, see <xref:security/authentication/google-logins>.</span></span>
1. <span data-ttu-id="53a0c-160">Proporcione el identificador de cliente y secreto de cliente a la aplicación en el <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions> de `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="53a0c-160">Provide the client ID and client secret to the app in the <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions> of `Startup.ConfigureServices`.</span></span>
1. <span data-ttu-id="53a0c-161">Ejecute la aplicación y solicite la página Mis notificaciones.</span><span class="sxs-lookup"><span data-stu-id="53a0c-161">Run the app and request the My Claims page.</span></span> <span data-ttu-id="53a0c-162">Cuando el usuario no se ha iniciado sesión, la aplicación se redirige a Google.</span><span class="sxs-lookup"><span data-stu-id="53a0c-162">When the user isn't signed in, the app redirects to Google.</span></span> <span data-ttu-id="53a0c-163">Inicie sesión con Google.</span><span class="sxs-lookup"><span data-stu-id="53a0c-163">Sign in with Google.</span></span> <span data-ttu-id="53a0c-164">Google redirige al usuario a la aplicación (`/Home/MyClaims`).</span><span class="sxs-lookup"><span data-stu-id="53a0c-164">Google redirects the user back to the app (`/Home/MyClaims`).</span></span> <span data-ttu-id="53a0c-165">El usuario se autentica y se carga la página Mis notificaciones.</span><span class="sxs-lookup"><span data-stu-id="53a0c-165">The user is authenticated, and the My Claims page is loaded.</span></span> <span data-ttu-id="53a0c-166">Está presente en la notificación de género **notificaciones de usuario** con el valor obtenido de Google.</span><span class="sxs-lookup"><span data-stu-id="53a0c-166">The gender claim is present under **User Claims** with the value obtained from Google.</span></span> <span data-ttu-id="53a0c-167">El token de acceso aparece en el **las propiedades de autenticación**.</span><span class="sxs-lookup"><span data-stu-id="53a0c-167">The access token appears in the **Authentication Properties**.</span></span>

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

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]
