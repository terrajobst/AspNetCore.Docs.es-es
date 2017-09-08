---
title: "Limitación de identidad por esquema"
author: rick-anderson
description: 
keywords: "Núcleo de ASP.NET,"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: d3d6ca1b-b4b5-4bf7-898e-dcd90ec1bf8c
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/limitingidentitybyscheme
ms.openlocfilehash: 2483c441da317a5c29b611b3a4910eae3c01fd7a
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/11/2017
---
# <a name="limiting-identity-by-scheme"></a><span data-ttu-id="9d630-103">Limitación de identidad por esquema</span><span class="sxs-lookup"><span data-stu-id="9d630-103">Limiting identity by scheme</span></span>

<a name=security-authorization-limiting-by-scheme></a>

<span data-ttu-id="9d630-104">En algunos escenarios, como aplicaciones de una página solo es posible acabar con varios métodos de autenticación.</span><span class="sxs-lookup"><span data-stu-id="9d630-104">In some scenarios, such as Single Page Applications it is possible to end up with multiple authentication methods.</span></span> <span data-ttu-id="9d630-105">Por ejemplo, la aplicación puede usar la autenticación basada en cookies para iniciar sesión y autenticación del portador para las solicitudes de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="9d630-105">For example, your application may use cookie-based authentication to log in and bearer authentication for JavaScript requests.</span></span> <span data-ttu-id="9d630-106">En algunos casos puede tener varias instancias de un middleware de autenticación.</span><span class="sxs-lookup"><span data-stu-id="9d630-106">In some cases you may have multiple instances of an authentication middleware.</span></span> <span data-ttu-id="9d630-107">Por ejemplo, dos cookie middlewares donde uno contiene una identidad básica y se crea uno cuando se desencadene una autenticación multifactor porque el usuario solicitó una operación que requiere seguridad adicional.</span><span class="sxs-lookup"><span data-stu-id="9d630-107">For example, two cookie middlewares where one contains a basic identity and one is created when a multi-factor authentication has triggered because the user requested an operation that requires extra security.</span></span>

<span data-ttu-id="9d630-108">Se denominan esquemas de autenticación cuando middleware de autenticación se configura durante la autenticación, por ejemplo</span><span class="sxs-lookup"><span data-stu-id="9d630-108">Authentication schemes are named when authentication middleware is configured during authentication, for example</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions()
{
    AuthenticationScheme = "Cookie",
    LoginPath = new PathString("/Account/Unauthorized/"),
    AccessDeniedPath = new PathString("/Account/Forbidden/"),
    AutomaticAuthenticate = false
});

app.UseBearerAuthentication(options =>
{
    options.AuthenticationScheme = "Bearer";
    options.AutomaticAuthenticate = false;
});
```

<span data-ttu-id="9d630-109">En esta configuración se han agregado dos middlewares de autenticación, uno para las cookies y otro para portador.</span><span class="sxs-lookup"><span data-stu-id="9d630-109">In this configuration two authentication middlewares have been added, one for cookies and one for bearer.</span></span>

>[!NOTE]
><span data-ttu-id="9d630-110">Al agregar varias middleware de autenticación, que debe asegurarse de que ningún middleware está configurado para ejecutarse automáticamente.</span><span class="sxs-lookup"><span data-stu-id="9d630-110">When adding multiple authentication middleware you should ensure that no middleware is configured to run automatically.</span></span> <span data-ttu-id="9d630-111">Para ello, establezca el `AutomaticAuthenticate` opciones de propiedad en false.</span><span class="sxs-lookup"><span data-stu-id="9d630-111">You do this by setting the `AutomaticAuthenticate` options property to false.</span></span> <span data-ttu-id="9d630-112">Si se realizan correctamente este filtrado por esquema no funcionará.</span><span class="sxs-lookup"><span data-stu-id="9d630-112">If you fail to do this filtering by scheme will not work.</span></span>

## <a name="selecting-the-scheme-with-the-authorize-attribute"></a><span data-ttu-id="9d630-113">Seleccionar el esquema con el atributo Authorize</span><span class="sxs-lookup"><span data-stu-id="9d630-113">Selecting the scheme with the Authorize attribute</span></span>

<span data-ttu-id="9d630-114">Ningún middleware de autenticación está configurado para ejecutar automáticamente y crear una identidad que debe, en el momento de la autorización elegir el middleware que se usará.</span><span class="sxs-lookup"><span data-stu-id="9d630-114">As no authentication middleware is configured to automatically run and create an identity you must, at the point of authorization choose which middleware will be used.</span></span> <span data-ttu-id="9d630-115">Seleccione el middleware que desea autorizar con la forma más sencilla es usar el `ActiveAuthenticationSchemes` propiedad.</span><span class="sxs-lookup"><span data-stu-id="9d630-115">The simplest way to select the middleware you wish to authorize with is to use the `ActiveAuthenticationSchemes` property.</span></span> <span data-ttu-id="9d630-116">Esta propiedad acepta una lista delimitada por comas de los esquemas de autenticación a utilizar.</span><span class="sxs-lookup"><span data-stu-id="9d630-116">This property accepts a comma delimited list of Authentication Schemes to use.</span></span> <span data-ttu-id="9d630-117">Por ejemplo,</span><span class="sxs-lookup"><span data-stu-id="9d630-117">For example;</span></span>

```csharp
[Authorize(ActiveAuthenticationSchemes = "Cookie,Bearer")]
public class MixedController : Controller
```

<span data-ttu-id="9d630-118">En el ejemplo anterior, la cookie y el portador middlewares ejecutará y tiene una oportunidad para crear y agregar una identidad para el usuario actual.</span><span class="sxs-lookup"><span data-stu-id="9d630-118">In the example above both the cookie and bearer middlewares will run and have a chance to create and append an identity for the current user.</span></span> <span data-ttu-id="9d630-119">Mediante la especificación de un único esquema solo el middleware especificado se ejecutará;</span><span class="sxs-lookup"><span data-stu-id="9d630-119">By specifying a single scheme only the specified middleware will run;</span></span>

```csharp
[Authorize(ActiveAuthenticationSchemes = "Bearer")]
```

<span data-ttu-id="9d630-120">En este caso se ejecutaría solo del middleware con el esquema de portador, y se ignorará cualquier identidad basado en cookies.</span><span class="sxs-lookup"><span data-stu-id="9d630-120">In this case only the middleware with the Bearer scheme would run, and any cookie based identities would be ignored.</span></span>

## <a name="selecting-the-scheme-with-policies"></a><span data-ttu-id="9d630-121">Seleccionar el esquema con directivas</span><span class="sxs-lookup"><span data-stu-id="9d630-121">Selecting the scheme with policies</span></span>

<span data-ttu-id="9d630-122">Si desea especificar los esquemas deseados en [directiva](policies.md#security-authorization-policies-based) puede establecer el `AuthenticationSchemes` colección al agregar la directiva.</span><span class="sxs-lookup"><span data-stu-id="9d630-122">If you prefer to specify the desired schemes in [policy](policies.md#security-authorization-policies-based) you can set the `AuthenticationSchemes` collection when adding your policy.</span></span>

```csharp
options.AddPolicy("Over18", policy =>
{
    policy.AuthenticationSchemes.Add("Bearer");
    policy.RequireAuthenticatedUser();
    policy.Requirements.Add(new Over18Requirement());
});
```

<span data-ttu-id="9d630-123">En este ejemplo el Over18 directiva solo se ejecutará con la identidad creada por el `Bearer` middleware.</span><span class="sxs-lookup"><span data-stu-id="9d630-123">In this example the Over18 policy will only run against the identity created by the `Bearer` middleware.</span></span>
