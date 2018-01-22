---
title: "Autorizar a un esquema específico - ASP.NET Core"
author: rick-anderson
description: "Este artículo explica cómo limitar la identidad a un esquema específico cuando se trabaja con varios métodos de autenticación."
ms.author: riande
manager: wpickett
ms.date: 10/12/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/limitingidentitybyscheme
ms.openlocfilehash: 099dba1a4235ef62ea298748645b99e2d6d12d44
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2018
---
# <a name="authorize-with-a-specific-scheme"></a><span data-ttu-id="5cab4-103">Autorizar con un esquema específico</span><span class="sxs-lookup"><span data-stu-id="5cab4-103">Authorize with a specific scheme</span></span>

<span data-ttu-id="5cab4-104">En algunos escenarios, como aplicaciones de una única página (SPAs), es habitual usar varios métodos de autenticación.</span><span class="sxs-lookup"><span data-stu-id="5cab4-104">In some scenarios, such as Single Page Applications (SPAs), it's common to use multiple authentication methods.</span></span> <span data-ttu-id="5cab4-105">Por ejemplo, la aplicación puede usar la autenticación basada en cookies para iniciar sesión y autenticación de portador JWT para las solicitudes de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="5cab4-105">For example, the app may use cookie-based authentication to log in and JWT bearer authentication for JavaScript requests.</span></span> <span data-ttu-id="5cab4-106">En algunos casos, la aplicación puede tener varias instancias de un controlador de autenticación.</span><span class="sxs-lookup"><span data-stu-id="5cab4-106">In some cases, the app may have multiple instances of an authentication handler.</span></span> <span data-ttu-id="5cab4-107">Por ejemplo, dos controladores de la cookie donde uno contiene una identidad básica y el otro se crea cuando se ha desencadenado una autenticación multifactor (MFA).</span><span class="sxs-lookup"><span data-stu-id="5cab4-107">For example, two cookie handlers where one contains a basic identity and one is created when a multi-factor authentication (MFA) has been triggered.</span></span> <span data-ttu-id="5cab4-108">MFA se puede desencadenar porque el usuario solicitó una operación que requiere seguridad adicional.</span><span class="sxs-lookup"><span data-stu-id="5cab4-108">MFA may be triggered because the user requested an operation that requires extra security.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5cab4-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5cab4-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="5cab4-110">Un esquema de autenticación se denomina cuando se configura el servicio de autenticación durante la autenticación.</span><span class="sxs-lookup"><span data-stu-id="5cab4-110">An authentication scheme is named when the authentication service is configured during authentication.</span></span> <span data-ttu-id="5cab4-111">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="5cab4-111">For example:</span></span>

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

<span data-ttu-id="5cab4-112">En el código anterior, se han agregado dos controladores de autenticación: uno para las cookies y otro para portador.</span><span class="sxs-lookup"><span data-stu-id="5cab4-112">In the preceding code, two authentication handlers have been added: one for cookies and one for bearer.</span></span>

>[!NOTE]
><span data-ttu-id="5cab4-113">Especificar el esquema predeterminado de resultados en el `HttpContext.User` propiedad que se establece para esa identidad.</span><span class="sxs-lookup"><span data-stu-id="5cab4-113">Specifying the default scheme results in the `HttpContext.User` property being set to that identity.</span></span> <span data-ttu-id="5cab4-114">Si no se desea este comportamiento, puede deshabilitarlo mediante la invocación de la forma sin parámetros de `AddAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="5cab4-114">If that behavior isn't desired, disable it by invoking the parameterless form of `AddAuthentication`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5cab4-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5cab4-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="5cab4-116">Se denominan esquemas de autenticación cuando middlewares de autenticación se configuran durante la autenticación.</span><span class="sxs-lookup"><span data-stu-id="5cab4-116">Authentication schemes are named when authentication middlewares are configured during authentication.</span></span> <span data-ttu-id="5cab4-117">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="5cab4-117">For example:</span></span>

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

<span data-ttu-id="5cab4-118">En el código anterior, se han agregado dos middlewares de autenticación: uno para las cookies y otro para portador.</span><span class="sxs-lookup"><span data-stu-id="5cab4-118">In the preceding code, two authentication middlewares have been added: one for cookies and one for bearer.</span></span>

>[!NOTE]
><span data-ttu-id="5cab4-119">Especificar el esquema predeterminado de resultados en el `HttpContext.User` propiedad que se establece para esa identidad.</span><span class="sxs-lookup"><span data-stu-id="5cab4-119">Specifying the default scheme results in the `HttpContext.User` property being set to that identity.</span></span> <span data-ttu-id="5cab4-120">Si no se desea este comportamiento, puede deshabilitarla estableciendo la `AuthenticationOptions.AutomaticAuthenticate` propiedad `false`.</span><span class="sxs-lookup"><span data-stu-id="5cab4-120">If that behavior isn't desired, disable it by setting the `AuthenticationOptions.AutomaticAuthenticate` property to `false`.</span></span>

---

## <a name="selecting-the-scheme-with-the-authorize-attribute"></a><span data-ttu-id="5cab4-121">Seleccionar el esquema con el atributo Authorize</span><span class="sxs-lookup"><span data-stu-id="5cab4-121">Selecting the scheme with the Authorize attribute</span></span>

<span data-ttu-id="5cab4-122">En el momento de la autorización, la aplicación indica el controlador que se usará.</span><span class="sxs-lookup"><span data-stu-id="5cab4-122">At the point of authorization, the app indicates the handler to be used.</span></span> <span data-ttu-id="5cab4-123">Seleccione el controlador con el que se autorizará la aplicación pasando una lista delimitada por comas de esquemas de autenticación `[Authorize]`.</span><span class="sxs-lookup"><span data-stu-id="5cab4-123">Select the handler with which the app will authorize by passing a comma-delimited list of authentication schemes to `[Authorize]`.</span></span> <span data-ttu-id="5cab4-124">El `[Authorize]` atributo especifica el esquema de autenticación o los esquemas para usar independientemente de si se configura un valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="5cab4-124">The `[Authorize]` attribute specifies the authentication scheme or schemes to use regardless of whether a default is configured.</span></span> <span data-ttu-id="5cab4-125">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="5cab4-125">For example:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5cab4-126">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5cab4-126">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5cab4-127">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5cab4-127">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

<span data-ttu-id="5cab4-128">En el ejemplo anterior, los controladores de la cookie y el portador ejecutan y tienen una oportunidad para crear y agregar una identidad para el usuario actual.</span><span class="sxs-lookup"><span data-stu-id="5cab4-128">In the preceding example, both the cookie and bearer handlers run and have a chance to create and append an identity for the current user.</span></span> <span data-ttu-id="5cab4-129">Mediante la especificación de un único esquema, se ejecuta el controlador correspondiente.</span><span class="sxs-lookup"><span data-stu-id="5cab4-129">By specifying a single scheme only, the corresponding handler runs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5cab4-130">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5cab4-130">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
[Authorize(AuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5cab4-131">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5cab4-131">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
[Authorize(ActiveAuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

---

<span data-ttu-id="5cab4-132">En el código anterior, se ejecuta sólo el controlador con el esquema "Portador".</span><span class="sxs-lookup"><span data-stu-id="5cab4-132">In the preceding code, only the handler with the "Bearer" scheme runs.</span></span> <span data-ttu-id="5cab4-133">Se omite cualquier identidad basada en cookies.</span><span class="sxs-lookup"><span data-stu-id="5cab4-133">Any cookie-based identities are ignored.</span></span>

## <a name="selecting-the-scheme-with-policies"></a><span data-ttu-id="5cab4-134">Seleccionar el esquema con directivas</span><span class="sxs-lookup"><span data-stu-id="5cab4-134">Selecting the scheme with policies</span></span>

<span data-ttu-id="5cab4-135">Si desea especificar los esquemas deseados en [directiva](xref:security/authorization/policies), puede establecer el `AuthenticationSchemes` colección cuando se agrega la directiva:</span><span class="sxs-lookup"><span data-stu-id="5cab4-135">If you prefer to specify the desired schemes in [policy](xref:security/authorization/policies), you can set the `AuthenticationSchemes` collection when adding your policy:</span></span>

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

<span data-ttu-id="5cab4-136">En el ejemplo anterior, la directiva de "Over18" solo se ejecuta con la identidad creada por el controlador "Portador".</span><span class="sxs-lookup"><span data-stu-id="5cab4-136">In the preceding example, the "Over18" policy only runs against the identity created by the "Bearer" handler.</span></span> <span data-ttu-id="5cab4-137">Usar la directiva estableciendo la `[Authorize]` del atributo `Policy` propiedad:</span><span class="sxs-lookup"><span data-stu-id="5cab4-137">Use the policy by setting the `[Authorize]` attribute's `Policy` property:</span></span>

```csharp
[Authorize(Policy = "Over18")]
public class RegistrationController : Controller
```
