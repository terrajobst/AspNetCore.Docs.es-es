---
title: "Autorización basada en notificaciones"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 737be5cd-3511-4f1c-b0ce-65403fb5eed3
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/claims
ms.openlocfilehash: 51d747c3c295164b6c0e00b5bafe63c565d588b4
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/13/2017
---
# <a name="claims-based-authorization"></a><span data-ttu-id="baa5d-103">Autorización basada en notificaciones</span><span class="sxs-lookup"><span data-stu-id="baa5d-103">Claims-Based Authorization</span></span>

<a name="security-authorization-claims-based"></a>

<span data-ttu-id="baa5d-104">Cuando se crea una identidad se pueden asignar una o más notificaciones emitidos por una entidad de confianza.</span><span class="sxs-lookup"><span data-stu-id="baa5d-104">When an identity is created it may be assigned one or more claims issued by a trusted party.</span></span> <span data-ttu-id="baa5d-105">Una notificación es par que representa el asunto de qué es el valor de nombre, puede hacerlo no qué el sujeto.</span><span class="sxs-lookup"><span data-stu-id="baa5d-105">A claim is name value pair that represents what the subject is, not what the subject can do.</span></span> <span data-ttu-id="baa5d-106">Por ejemplo puede tener una licencia de controladores, emitido por una entidad de licencia de conducir local.</span><span class="sxs-lookup"><span data-stu-id="baa5d-106">For example you may have a Drivers License, issued by a local driving license authority.</span></span> <span data-ttu-id="baa5d-107">De conducir su permiso tiene tu fecha de nacimiento.</span><span class="sxs-lookup"><span data-stu-id="baa5d-107">Your driver's license has your date of birth on it.</span></span> <span data-ttu-id="baa5d-108">En este caso, el nombre de notificación sería `DateOfBirth`, el valor de notificación sería tu fecha de nacimiento, por ejemplo `8th June 1970` y el emisor sería la autoridad de licencia de conducir.</span><span class="sxs-lookup"><span data-stu-id="baa5d-108">In this case the claim name would be `DateOfBirth`, the claim value would be your date of birth, for example `8th June 1970` and the issuer would be the driving license authority.</span></span> <span data-ttu-id="baa5d-109">Autorización basada en notificaciones, en su forma más sencilla, comprueba el valor de una notificación y permite el acceso a un recurso basado en ese valor.</span><span class="sxs-lookup"><span data-stu-id="baa5d-109">Claims based authorization, at its simplest, checks the value of a claim and allows access to a resource based upon that value.</span></span> <span data-ttu-id="baa5d-110">Por ejemplo, si desea que el proceso de autorización el acceso a un club nocturno puede ser:</span><span class="sxs-lookup"><span data-stu-id="baa5d-110">For example if you want access to a night club the authorization process might be:</span></span>

<span data-ttu-id="baa5d-111">El responsable de seguridad de la puerta evalúe el valor de la fecha de nacimiento notificación y si confía en el emisor (la entidad de licencia determinante) antes de conceder que acceso.</span><span class="sxs-lookup"><span data-stu-id="baa5d-111">The door security officer would evaluate the value of your date of birth claim and whether they trust the issuer (the driving license authority) before granting you access.</span></span>

<span data-ttu-id="baa5d-112">Una identidad puede contener varias notificaciones con varios valores y puede contener varias notificaciones del mismo tipo.</span><span class="sxs-lookup"><span data-stu-id="baa5d-112">An identity can contain multiple claims with multiple values and can contain multiple claims of the same type.</span></span>

## <a name="adding-claims-checks"></a><span data-ttu-id="baa5d-113">Agregar controles de notificaciones</span><span class="sxs-lookup"><span data-stu-id="baa5d-113">Adding claims checks</span></span>

<span data-ttu-id="baa5d-114">Notificación de comprobaciones de autorización basados en son declarativas, el desarrollador incrusta dentro de su código, con un controlador o una acción en un controlador de la especificación de notificaciones que debe poseer el usuario actual y, opcionalmente, debe contener el valor de la notificación para tener acceso a la recurso solicitado.</span><span class="sxs-lookup"><span data-stu-id="baa5d-114">Claim based authorization checks are declarative - the developer embeds them within their code, against a controller or an action within a controller, specifying claims which the current user must possess, and optionally the value the claim must hold to access the requested resource.</span></span> <span data-ttu-id="baa5d-115">Notificaciones requisitos son basada en directivas, el desarrollador debe crear y registrar una directiva de expresar los requisitos de notificaciones.</span><span class="sxs-lookup"><span data-stu-id="baa5d-115">Claims requirements are policy based, the developer must build and register a policy expressing the claims requirements.</span></span>

<span data-ttu-id="baa5d-116">El tipo más sencillo de directiva Busca la presencia de una notificación de notificación y no comprueba el valor.</span><span class="sxs-lookup"><span data-stu-id="baa5d-116">The simplest type of claim policy looks for the presence of a claim and does not check the value.</span></span>

<span data-ttu-id="baa5d-117">En primer lugar debe crear y registrar la directiva.</span><span class="sxs-lookup"><span data-stu-id="baa5d-117">First you need to build and register the policy.</span></span> <span data-ttu-id="baa5d-118">Esto tiene lugar como parte de la configuración de servicio de autorización, que normalmente toma parte en `ConfigureServices()` en su *Startup.cs* archivo.</span><span class="sxs-lookup"><span data-stu-id="baa5d-118">This takes place as part of the Authorization service configuration, which normally takes part in `ConfigureServices()` in your *Startup.cs* file.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("EmployeeOnly", policy => policy.RequireClaim("EmployeeNumber"));
    });
}
```

<span data-ttu-id="baa5d-119">En este caso el `EmployeeOnly` directiva comprueba la presencia de un `EmployeeNumber` de notificación de la identidad actual.</span><span class="sxs-lookup"><span data-stu-id="baa5d-119">In this case the `EmployeeOnly` policy checks for the presence of an `EmployeeNumber` claim on the current identity.</span></span>

<span data-ttu-id="baa5d-120">A continuación, aplique la directiva con la `Policy` propiedad en el `AuthorizeAttribute` atributo para especificar el nombre de la directiva;</span><span class="sxs-lookup"><span data-stu-id="baa5d-120">You then apply the policy using the `Policy` property on the `AuthorizeAttribute` attribute to specify the policy name;</span></span>

```csharp
[Authorize(Policy = "EmployeeOnly")]
public IActionResult VacationBalance()
{
    return View();
}
```

<span data-ttu-id="baa5d-121">El `AuthorizeAttribute` atributo sólo se puede aplicar a un controlador todo, en esta instancia de la directiva de coincidencia de identidades tendrán permiso para acceder a una acción en el controlador.</span><span class="sxs-lookup"><span data-stu-id="baa5d-121">The `AuthorizeAttribute` attribute can be applied to an entire controller, in this instance only identities matching the policy will be allowed access to any Action on the controller.</span></span>

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class VacationController : Controller
{
    public ActionResult VacationBalance()
    {
    }
}
```

<span data-ttu-id="baa5d-122">Si tiene un controlador que está protegido por el `AuthorizeAttribute` de atributo, pero desea permitir el acceso anónimo a acciones concretas que se aplica los `AllowAnonymousAttribute` atributo.</span><span class="sxs-lookup"><span data-stu-id="baa5d-122">If you have a controller that is protected by the `AuthorizeAttribute` attribute, but want to allow anonymous access to particular actions you apply the `AllowAnonymousAttribute` attribute.</span></span>

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class VacationController : Controller
{
    public ActionResult VacationBalance()
    {
    }

    [AllowAnonymous]
    public ActionResult VacationPolicy()
    {
    }
}
```

<span data-ttu-id="baa5d-123">La mayoría de notificaciones vienen con un valor.</span><span class="sxs-lookup"><span data-stu-id="baa5d-123">Most claims come with a value.</span></span> <span data-ttu-id="baa5d-124">Puede especificar una lista de valores permitidos al crear la directiva.</span><span class="sxs-lookup"><span data-stu-id="baa5d-124">You can specify a list of allowed values when creating the policy.</span></span> <span data-ttu-id="baa5d-125">En el ejemplo siguiente se realizaría correctamente solo para los empleados cuyo número de empleado era 1, 2, 3, 4 o 5.</span><span class="sxs-lookup"><span data-stu-id="baa5d-125">The following example would only succeed for employees whose employee number was 1, 2, 3, 4 or 5.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("Founders", policy =>
                          policy.RequireClaim("EmployeeNumber", "1", "2", "3", "4", "5"));
    });
}
```

## <a name="multiple-policy-evaluation"></a><span data-ttu-id="baa5d-126">Evaluación múltiple de directiva</span><span class="sxs-lookup"><span data-stu-id="baa5d-126">Multiple Policy Evaluation</span></span>

<span data-ttu-id="baa5d-127">Si se aplican varias directivas a una acción o controlador de todas las directivas deben pasar antes de que se concede el acceso.</span><span class="sxs-lookup"><span data-stu-id="baa5d-127">If you apply multiple policies to a controller or action then all policies must pass before access is granted.</span></span> <span data-ttu-id="baa5d-128">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="baa5d-128">For example:</span></span>

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class SalaryController : Controller
{
    public ActionResult Payslip()
    {
    }

    [Authorize(Policy = "HumanResources")]
    public ActionResult UpdateSalary()
    {
    }
}
```

<span data-ttu-id="baa5d-129">En el ejemplo anterior cualquier identidad que cumple el `EmployeeOnly` directiva puede tener acceso a la `Payslip` acción como esa directiva se aplica en el controlador.</span><span class="sxs-lookup"><span data-stu-id="baa5d-129">In the above example any identity which fulfills the `EmployeeOnly` policy can access the `Payslip` action as that policy is enforced on the controller.</span></span> <span data-ttu-id="baa5d-130">Sin embargo para llamar a la `UpdateSalary` acción debe cumplir la identidad *ambos* el `EmployeeOnly` directiva y la `HumanResources` directiva.</span><span class="sxs-lookup"><span data-stu-id="baa5d-130">However in order to call the `UpdateSalary` action the identity must fulfill *both* the `EmployeeOnly` policy and the `HumanResources` policy.</span></span>

<span data-ttu-id="baa5d-131">Si desea que las directivas más complicadas, como llevar a cabo una fecha de nacimiento notificación, calcular una edad de ella, a continuación, comprobar la edad es 21 o anterior, a continuación, tiene que escribir [controladores de directiva personalizada](policies.md#security-authorization-policies-based).</span><span class="sxs-lookup"><span data-stu-id="baa5d-131">If you want more complicated policies, such as taking a date of birth claim, calculating an age from it then checking the age is 21 or older then you need to write [custom policy handlers](policies.md#security-authorization-policies-based).</span></span>
