---
title: "Autorizar a un esquema específico - ASP.NET Core"
author: rick-anderson
description: "Este artículo explica cómo limitar la identidad a un esquema específico cuando se trabaja con varios métodos de autenticación."
keywords: "Núcleo de ASP.NET, identidad, el esquema de autenticación"
ms.author: riande
manager: wpickett
ms.date: 10/12/2017
ms.topic: article
ms.assetid: d3d6ca1b-b4b5-4bf7-898e-dcd90ec1bf8c
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/limitingidentitybyscheme
ms.openlocfilehash: 8c9d068b88263d0c06b11a6b87416fb02885c475
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
# <a name="authorize-with-a-specific-scheme"></a><span data-ttu-id="8b463-104">Autorizar con un esquema específico</span><span class="sxs-lookup"><span data-stu-id="8b463-104">Authorize with a specific scheme</span></span>

<span data-ttu-id="8b463-105">En algunos escenarios, como aplicaciones de una única página (SPAs), es habitual usar varios métodos de autenticación.</span><span class="sxs-lookup"><span data-stu-id="8b463-105">In some scenarios, such as Single Page Applications (SPAs), it's common to use multiple authentication methods.</span></span> <span data-ttu-id="8b463-106">Por ejemplo, la aplicación puede usar la autenticación basada en cookies para iniciar sesión y autenticación de portador JWT para las solicitudes de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="8b463-106">For example, the app may use cookie-based authentication to log in and JWT bearer authentication for JavaScript requests.</span></span> <span data-ttu-id="8b463-107">En algunos casos, la aplicación puede tener varias instancias de un controlador de autenticación.</span><span class="sxs-lookup"><span data-stu-id="8b463-107">In some cases, the app may have multiple instances of an authentication handler.</span></span> <span data-ttu-id="8b463-108">Por ejemplo, dos controladores de la cookie donde uno contiene una identidad básica y el otro se crea cuando se ha desencadenado una autenticación multifactor (MFA).</span><span class="sxs-lookup"><span data-stu-id="8b463-108">For example, two cookie handlers where one contains a basic identity and one is created when a multi-factor authentication (MFA) has been triggered.</span></span> <span data-ttu-id="8b463-109">MFA se puede desencadenar porque el usuario solicitó una operación que requiere seguridad adicional.</span><span class="sxs-lookup"><span data-stu-id="8b463-109">MFA may be triggered because the user requested an operation that requires extra security.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8b463-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8b463-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="8b463-111">Un esquema de autenticación se denomina cuando se configura el servicio de autenticación durante la autenticación.</span><span class="sxs-lookup"><span data-stu-id="8b463-111">An authentication scheme is named when the authentication service is configured during authentication.</span></span> <span data-ttu-id="8b463-112">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="8b463-112">For example:</span></span>

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

<span data-ttu-id="8b463-113">En el código anterior, se han agregado dos controladores de autenticación: uno para las cookies y otro para portador.</span><span class="sxs-lookup"><span data-stu-id="8b463-113">In the preceding code, two authentication handlers have been added: one for cookies and one for bearer.</span></span>

>[!NOTE]
><span data-ttu-id="8b463-114">Especificar el esquema predeterminado de resultados en el `HttpContext.User` propiedad que se establece para esa identidad.</span><span class="sxs-lookup"><span data-stu-id="8b463-114">Specifying the default scheme results in the `HttpContext.User` property being set to that identity.</span></span> <span data-ttu-id="8b463-115">Si no se desea este comportamiento, puede deshabilitarlo mediante la invocación de la forma sin parámetros de `AddAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="8b463-115">If that behavior isn't desired, disable it by invoking the parameterless form of `AddAuthentication`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8b463-116">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8b463-116">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="8b463-117">Se denominan esquemas de autenticación cuando middlewares de autenticación se configuran durante la autenticación.</span><span class="sxs-lookup"><span data-stu-id="8b463-117">Authentication schemes are named when authentication middlewares are configured during authentication.</span></span> <span data-ttu-id="8b463-118">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="8b463-118">For example:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
{
    // Code omitted for brevity

    app.UseCookieAuthentication(new CookieAuthenticationOptions()
    {
        AuthenticationScheme = "Cookie",
        LoginPath = "/Account/Unauthorized/",
        AccessDeniedPath = "/Account/Forbidden/",
        AutomaticAuthenticate = false
    });
    
    app.UseJwtBearerAuthentication(new JwtBearerOptions()
    {
        AuthenticationScheme = "Bearer",
        AutomaticAuthenticate = false,
        Audience = "http://localhost:5001/",
        Authority = "http://localhost:5000/",
        RequireHttpsMetadata = false
    });
```

<span data-ttu-id="8b463-119">En el código anterior, se han agregado dos middlewares de autenticación: uno para las cookies y otro para portador.</span><span class="sxs-lookup"><span data-stu-id="8b463-119">In the preceding code, two authentication middlewares have been added: one for cookies and one for bearer.</span></span>

>[!NOTE]
><span data-ttu-id="8b463-120">Especificar el esquema predeterminado de resultados en el `HttpContext.User` propiedad que se establece para esa identidad.</span><span class="sxs-lookup"><span data-stu-id="8b463-120">Specifying the default scheme results in the `HttpContext.User` property being set to that identity.</span></span> <span data-ttu-id="8b463-121">Si no se desea este comportamiento, puede deshabilitarla estableciendo la `AuthenticationOptions.AutomaticAuthenticate` propiedad `false`.</span><span class="sxs-lookup"><span data-stu-id="8b463-121">If that behavior isn't desired, disable it by setting the `AuthenticationOptions.AutomaticAuthenticate` property to `false`.</span></span>

---

## <a name="selecting-the-scheme-with-the-authorize-attribute"></a><span data-ttu-id="8b463-122">Seleccionar el esquema con el atributo Authorize</span><span class="sxs-lookup"><span data-stu-id="8b463-122">Selecting the scheme with the Authorize attribute</span></span>

<span data-ttu-id="8b463-123">En el momento de la autorización, la aplicación indica el controlador que se usará.</span><span class="sxs-lookup"><span data-stu-id="8b463-123">At the point of authorization, the app indicates the handler to be used.</span></span> <span data-ttu-id="8b463-124">Seleccione el controlador con el que se autorizará la aplicación pasando una lista delimitada por comas de esquemas de autenticación `[Authorize]`.</span><span class="sxs-lookup"><span data-stu-id="8b463-124">Select the handler with which the app will authorize by passing a comma-delimited list of authentication schemes to `[Authorize]`.</span></span> <span data-ttu-id="8b463-125">El `[Authorize]` atributo especifica el esquema de autenticación o los esquemas para usar independientemente de si se configura un valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="8b463-125">The `[Authorize]` attribute specifies the authentication scheme or schemes to use regardless of whether a default is configured.</span></span> <span data-ttu-id="8b463-126">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="8b463-126">For example:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8b463-127">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8b463-127">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8b463-128">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8b463-128">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
[Authorize(ActiveAuthenticationSchemes = AuthSchemes)]
public class MixedController : Controller
    // Requires the following imports:
    // using Microsoft.AspNetCore.Authentication.Cookies;
    // using Microsoft.AspNetCore.Authentication.JwtBearer;
    private const string AuthSchemes =
        CookieAuthenticationDefaults.AuthenticationScheme + "," +
        JwtBearerDefaults.AuthenticationScheme;
```

---

<span data-ttu-id="8b463-129">En el ejemplo anterior, los controladores de la cookie y el portador ejecutan y tienen una oportunidad para crear y agregar una identidad para el usuario actual.</span><span class="sxs-lookup"><span data-stu-id="8b463-129">In the preceding example, both the cookie and bearer handlers run and have a chance to create and append an identity for the current user.</span></span> <span data-ttu-id="8b463-130">Mediante la especificación de un único esquema, se ejecuta el controlador correspondiente.</span><span class="sxs-lookup"><span data-stu-id="8b463-130">By specifying a single scheme only, the corresponding handler runs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8b463-131">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8b463-131">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
[Authorize(AuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8b463-132">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8b463-132">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
[Authorize(ActiveAuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

---

<span data-ttu-id="8b463-133">En el código anterior, se ejecuta sólo el controlador con el esquema "Portador".</span><span class="sxs-lookup"><span data-stu-id="8b463-133">In the preceding code, only the handler with the "Bearer" scheme runs.</span></span> <span data-ttu-id="8b463-134">Se omite cualquier identidad basada en cookies.</span><span class="sxs-lookup"><span data-stu-id="8b463-134">Any cookie-based identities are ignored.</span></span>

## <a name="selecting-the-scheme-with-policies"></a><span data-ttu-id="8b463-135">Seleccionar el esquema con directivas</span><span class="sxs-lookup"><span data-stu-id="8b463-135">Selecting the scheme with policies</span></span>

<span data-ttu-id="8b463-136">Si desea especificar los esquemas deseados en [directiva](xref:security/authorization/policies), puede establecer el `AuthenticationSchemes` colección cuando se agrega la directiva:</span><span class="sxs-lookup"><span data-stu-id="8b463-136">If you prefer to specify the desired schemes in [policy](xref:security/authorization/policies), you can set the `AuthenticationSchemes` collection when adding your policy:</span></span>

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

<span data-ttu-id="8b463-137">En el ejemplo anterior, la directiva de "Over18" solo se ejecuta con la identidad creada por el controlador "Portador".</span><span class="sxs-lookup"><span data-stu-id="8b463-137">In the preceding example, the "Over18" policy only runs against the identity created by the "Bearer" handler.</span></span> <span data-ttu-id="8b463-138">Usar la directiva estableciendo la `[Authorize]` del atributo `Policy` propiedad:</span><span class="sxs-lookup"><span data-stu-id="8b463-138">Use the policy by setting the `[Authorize]` attribute's `Policy` property:</span></span>

```csharp
[Authorize(Policy = "Over18")]
public class RegistrationController : Controller
```
