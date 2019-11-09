---
title: Autorizar con un esquema específico en ASP.NET Core
author: rick-anderson
description: En este artículo se explica cómo limitar la identidad a un esquema específico cuando se trabaja con varios métodos de autenticación.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 11/08/2019
uid: security/authorization/limitingidentitybyscheme
ms.openlocfilehash: 38da80519b9d5d097c24d38b5a37503174629fc4
ms.sourcegitcommit: 4818385c3cfe0805e15138a2c1785b62deeaab90
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/09/2019
ms.locfileid: "73896964"
---
# <a name="authorize-with-a-specific-scheme-in-aspnet-core"></a><span data-ttu-id="1694a-103">Autorizar con un esquema específico en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1694a-103">Authorize with a specific scheme in ASP.NET Core</span></span>

<span data-ttu-id="1694a-104">En algunos escenarios, como las aplicaciones de una sola página (Spa), es habitual usar varios métodos de autenticación.</span><span class="sxs-lookup"><span data-stu-id="1694a-104">In some scenarios, such as Single Page Applications (SPAs), it's common to use multiple authentication methods.</span></span> <span data-ttu-id="1694a-105">Por ejemplo, la aplicación puede usar la autenticación basada en cookies para iniciar sesión y la autenticación de portador de JWT para solicitudes de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="1694a-105">For example, the app may use cookie-based authentication to log in and JWT bearer authentication for JavaScript requests.</span></span> <span data-ttu-id="1694a-106">En algunos casos, la aplicación puede tener varias instancias de un controlador de autenticación.</span><span class="sxs-lookup"><span data-stu-id="1694a-106">In some cases, the app may have multiple instances of an authentication handler.</span></span> <span data-ttu-id="1694a-107">Por ejemplo, dos controladores de cookies donde uno contiene una identidad básica y uno se crea cuando se ha desencadenado una autenticación multifactor (MFA).</span><span class="sxs-lookup"><span data-stu-id="1694a-107">For example, two cookie handlers where one contains a basic identity and one is created when a multi-factor authentication (MFA) has been triggered.</span></span> <span data-ttu-id="1694a-108">MFA se puede desencadenar porque el usuario solicitó una operación que requiere seguridad adicional.</span><span class="sxs-lookup"><span data-stu-id="1694a-108">MFA may be triggered because the user requested an operation that requires extra security.</span></span>

<span data-ttu-id="1694a-109">Un esquema de autenticación se denomina cuando el servicio de autenticación se configura durante la autenticación.</span><span class="sxs-lookup"><span data-stu-id="1694a-109">An authentication scheme is named when the authentication service is configured during authentication.</span></span> <span data-ttu-id="1694a-110">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="1694a-110">For example:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Code omitted for brevity

    services.AddAuthentication()
        .AddCookie(options => {
            options.LoginPath = "/Account/Unauthorized/";
            options.AccessDeniedPath = "/Account/Forbidden/";
        })
        .AddJwtBearer(options => {
            options.Audience = "http://localhost:5001/";
            options.Authority = "http://localhost:5000/";
        });
```

<span data-ttu-id="1694a-111">En el código anterior, se han agregado dos controladores de autenticación: uno para las cookies y otro para el portador.</span><span class="sxs-lookup"><span data-stu-id="1694a-111">In the preceding code, two authentication handlers have been added: one for cookies and one for bearer.</span></span>

>[!NOTE]
><span data-ttu-id="1694a-112">Al especificar el esquema predeterminado, se establece la propiedad `HttpContext.User` en esa identidad.</span><span class="sxs-lookup"><span data-stu-id="1694a-112">Specifying the default scheme results in the `HttpContext.User` property being set to that identity.</span></span> <span data-ttu-id="1694a-113">Si no se desea ese comportamiento, deshabilítelo mediante la invocación de la forma sin parámetros de `AddAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="1694a-113">If that behavior isn't desired, disable it by invoking the parameterless form of `AddAuthentication`.</span></span>

## <a name="selecting-the-scheme-with-the-authorize-attribute"></a><span data-ttu-id="1694a-114">Seleccionar el esquema con el atributo Authorize</span><span class="sxs-lookup"><span data-stu-id="1694a-114">Selecting the scheme with the Authorize attribute</span></span>

<span data-ttu-id="1694a-115">En el momento de la autorización, la aplicación indica el controlador que se va a usar.</span><span class="sxs-lookup"><span data-stu-id="1694a-115">At the point of authorization, the app indicates the handler to be used.</span></span> <span data-ttu-id="1694a-116">Seleccione el controlador con el que se autorizará la aplicación pasando una lista delimitada por comas de esquemas de autenticación a `[Authorize]`.</span><span class="sxs-lookup"><span data-stu-id="1694a-116">Select the handler with which the app will authorize by passing a comma-delimited list of authentication schemes to `[Authorize]`.</span></span> <span data-ttu-id="1694a-117">El atributo `[Authorize]` especifica el esquema de autenticación o los esquemas que se van a usar independientemente de si se ha configurado un valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="1694a-117">The `[Authorize]` attribute specifies the authentication scheme or schemes to use regardless of whether a default is configured.</span></span> <span data-ttu-id="1694a-118">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="1694a-118">For example:</span></span>

```csharp
[Authorize(AuthenticationSchemes = AuthSchemes)]
public class MixedController : Controller
    // Requires the following imports:
    // using Microsoft.AspNetCore.Authentication.Cookies;
    // using Microsoft.AspNetCore.Authentication.JwtBearer;
    private const string AuthSchemes =
        CookieAuthenticationDefaults.AuthenticationScheme + "," +
        JwtBearerDefaults.AuthenticationScheme;
```

<span data-ttu-id="1694a-119">En el ejemplo anterior, los controladores de cookies y portadores se ejecutan y tienen la oportunidad de crear y anexar una identidad para el usuario actual.</span><span class="sxs-lookup"><span data-stu-id="1694a-119">In the preceding example, both the cookie and bearer handlers run and have a chance to create and append an identity for the current user.</span></span> <span data-ttu-id="1694a-120">Al especificar solo un esquema, se ejecuta el controlador correspondiente.</span><span class="sxs-lookup"><span data-stu-id="1694a-120">By specifying a single scheme only, the corresponding handler runs.</span></span>

```csharp
[Authorize(AuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

<span data-ttu-id="1694a-121">En el código anterior, solo se ejecuta el controlador con el esquema "portador".</span><span class="sxs-lookup"><span data-stu-id="1694a-121">In the preceding code, only the handler with the "Bearer" scheme runs.</span></span> <span data-ttu-id="1694a-122">Se omiten las identidades basadas en cookies.</span><span class="sxs-lookup"><span data-stu-id="1694a-122">Any cookie-based identities are ignored.</span></span>

## <a name="selecting-the-scheme-with-policies"></a><span data-ttu-id="1694a-123">Seleccionar el esquema con directivas</span><span class="sxs-lookup"><span data-stu-id="1694a-123">Selecting the scheme with policies</span></span>

<span data-ttu-id="1694a-124">Si prefiere especificar los esquemas deseados en la [Directiva](xref:security/authorization/policies), puede establecer la `AuthenticationSchemes` colección al agregar la Directiva:</span><span class="sxs-lookup"><span data-stu-id="1694a-124">If you prefer to specify the desired schemes in [policy](xref:security/authorization/policies), you can set the `AuthenticationSchemes` collection when adding your policy:</span></span>

```csharp
services.AddAuthorization(options =>
{
    options.AddPolicy("Over18", policy =>
    {
        policy.AuthenticationSchemes.Add(JwtBearerDefaults.AuthenticationScheme);
        policy.RequireAuthenticatedUser();
        policy.Requirements.Add(new MinimumAgeRequirement());
    });
});
```

<span data-ttu-id="1694a-125">En el ejemplo anterior, la Directiva "Over18" solo se ejecuta con la identidad creada por el controlador de "portador".</span><span class="sxs-lookup"><span data-stu-id="1694a-125">In the preceding example, the "Over18" policy only runs against the identity created by the "Bearer" handler.</span></span> <span data-ttu-id="1694a-126">Use la Directiva estableciendo la propiedad `Policy` del atributo `[Authorize]`:</span><span class="sxs-lookup"><span data-stu-id="1694a-126">Use the policy by setting the `[Authorize]` attribute's `Policy` property:</span></span>

```csharp
[Authorize(Policy = "Over18")]
public class RegistrationController : Controller
```

::: moniker range=">= aspnetcore-2.0"

## <a name="use-multiple-authentication-schemes"></a><span data-ttu-id="1694a-127">Usar varios esquemas de autenticación</span><span class="sxs-lookup"><span data-stu-id="1694a-127">Use multiple authentication schemes</span></span>

<span data-ttu-id="1694a-128">Es posible que algunas aplicaciones necesiten admitir varios tipos de autenticación.</span><span class="sxs-lookup"><span data-stu-id="1694a-128">Some apps may need to support multiple types of authentication.</span></span> <span data-ttu-id="1694a-129">Por ejemplo, es posible que la aplicación autentique a los usuarios de Azure Active Directory y de una base de datos de usuarios.</span><span class="sxs-lookup"><span data-stu-id="1694a-129">For example, your app might authenticate users from Azure Active Directory and from a users database.</span></span> <span data-ttu-id="1694a-130">Otro ejemplo es una aplicación que autentica a los usuarios de Servicios de federación de Active Directory (AD FS) y Azure Active Directory B2C.</span><span class="sxs-lookup"><span data-stu-id="1694a-130">Another example is an app that authenticates users from both Active Directory Federation Services and Azure Active Directory B2C.</span></span> <span data-ttu-id="1694a-131">En este caso, la aplicación debe aceptar un token de portador JWT de varios emisores.</span><span class="sxs-lookup"><span data-stu-id="1694a-131">In this case, the app should accept a JWT bearer token from several issuers.</span></span>

<span data-ttu-id="1694a-132">Agregue todos los esquemas de autenticación que desea aceptar.</span><span class="sxs-lookup"><span data-stu-id="1694a-132">Add all authentication schemes you'd like to accept.</span></span> <span data-ttu-id="1694a-133">Por ejemplo, el siguiente código de `Startup.ConfigureServices` agrega dos esquemas de autenticación de portador JWT con distintos emisores:</span><span class="sxs-lookup"><span data-stu-id="1694a-133">For example, the following code in `Startup.ConfigureServices` adds two JWT bearer authentication schemes with different issuers:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Code omitted for brevity

    services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
        .AddJwtBearer(options =>
        {
            options.Audience = "https://localhost:5000/";
            options.Authority = "https://localhost:5000/identity/";
        })
        .AddJwtBearer("AzureAD", options =>
        {
            options.Audience = "https://localhost:5000/";
            options.Authority = "https://login.microsoftonline.com/eb971100-6f99-4bdc-8611-1bc8edd7f436/";
        });
}
```

> [!NOTE]
> <span data-ttu-id="1694a-134">Solo se registra una autenticación de portador de JWT con el esquema de autenticación predeterminado `JwtBearerDefaults.AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="1694a-134">Only one JWT bearer authentication is registered with the default authentication scheme `JwtBearerDefaults.AuthenticationScheme`.</span></span> <span data-ttu-id="1694a-135">La autenticación adicional tiene que registrarse con un esquema de autenticación único.</span><span class="sxs-lookup"><span data-stu-id="1694a-135">Additional authentication has to be registered with a unique authentication scheme.</span></span>

<span data-ttu-id="1694a-136">El siguiente paso consiste en actualizar la Directiva de autorización predeterminada para aceptar ambos esquemas de autenticación.</span><span class="sxs-lookup"><span data-stu-id="1694a-136">The next step is to update the default authorization policy to accept both authentication schemes.</span></span> <span data-ttu-id="1694a-137">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="1694a-137">For example:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Code omitted for brevity

    services.AddAuthorization(options =>
    {
        var defaultAuthorizationPolicyBuilder = new AuthorizationPolicyBuilder(
            JwtBearerDefaults.AuthenticationScheme,
            "AzureAD");
        defaultAuthorizationPolicyBuilder = 
            defaultAuthorizationPolicyBuilder.RequireAuthenticatedUser();
        options.DefaultPolicy = defaultAuthorizationPolicyBuilder.Build();
    });
}
```

<span data-ttu-id="1694a-138">Como la Directiva de autorización predeterminada se invalida, es posible usar el atributo `[Authorize]` en los controladores.</span><span class="sxs-lookup"><span data-stu-id="1694a-138">As the default authorization policy is overridden, it's possible to use the `[Authorize]` attribute in controllers.</span></span> <span data-ttu-id="1694a-139">Después, el controlador acepta las solicitudes con JWT emitidos por el primer o el segundo emisor.</span><span class="sxs-lookup"><span data-stu-id="1694a-139">The controller then accepts requests with JWT issued by the first or second issuer.</span></span>

::: moniker-end
