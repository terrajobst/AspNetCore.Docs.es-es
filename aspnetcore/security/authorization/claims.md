---
title: Autorización basada en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo agregar comprobaciones de notificaciones para la autorización en una aplicación ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/claims
ms.openlocfilehash: 6b60ae5515819b017ab577f655ed91ee4d8ed0dd
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65086154"
---
# <a name="claims-based-authorization-in-aspnet-core"></a><span data-ttu-id="dacb8-103">Autorización basada en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="dacb8-103">Claims-based authorization in ASP.NET Core</span></span>

<a name="security-authorization-claims-based"></a>

<span data-ttu-id="dacb8-104">Cuando se crea una identidad se pueden asignar una o más notificaciones emitidas por una entidad de confianza.</span><span class="sxs-lookup"><span data-stu-id="dacb8-104">When an identity is created it may be assigned one or more claims issued by a trusted party.</span></span> <span data-ttu-id="dacb8-105">Una notificación es un par nombre-valor que representa el asunto de qué es, no lo que el sujeto puede hacer.</span><span class="sxs-lookup"><span data-stu-id="dacb8-105">A claim is a name value pair that represents what the subject is, not what the subject can do.</span></span> <span data-ttu-id="dacb8-106">Por ejemplo, puede tener un permiso de conducir, emitido por una entidad de licencia de conducción local.</span><span class="sxs-lookup"><span data-stu-id="dacb8-106">For example, you may have a driver's license, issued by a local driving license authority.</span></span> <span data-ttu-id="dacb8-107">De conducir su permiso tiene su fecha de nacimiento.</span><span class="sxs-lookup"><span data-stu-id="dacb8-107">Your driver's license has your date of birth on it.</span></span> <span data-ttu-id="dacb8-108">En este caso sería el nombre de la notificación `DateOfBirth`, el valor de notificación sería su fecha de nacimiento, por ejemplo `8th June 1970` y el emisor sería la autoridad de licencia de conducción.</span><span class="sxs-lookup"><span data-stu-id="dacb8-108">In this case the claim name would be `DateOfBirth`, the claim value would be your date of birth, for example `8th June 1970` and the issuer would be the driving license authority.</span></span> <span data-ttu-id="dacb8-109">Autorización basada en notificaciones, en su forma más simple, comprueba el valor de una notificación y permite el acceso a un recurso en función de ese valor.</span><span class="sxs-lookup"><span data-stu-id="dacb8-109">Claims based authorization, at its simplest, checks the value of a claim and allows access to a resource based upon that value.</span></span> <span data-ttu-id="dacb8-110">Por ejemplo, si desea que el proceso de autorización el acceso a un club nocturno puede ser:</span><span class="sxs-lookup"><span data-stu-id="dacb8-110">For example if you want access to a night club the authorization process might be:</span></span>

<span data-ttu-id="dacb8-111">El responsable de la seguridad de la puerta evaluaría el valor de la fecha de nacimiento notificación y si confía en el emisor (la entidad de licencia conducción) antes de conceder que acceso.</span><span class="sxs-lookup"><span data-stu-id="dacb8-111">The door security officer would evaluate the value of your date of birth claim and whether they trust the issuer (the driving license authority) before granting you access.</span></span>

<span data-ttu-id="dacb8-112">Una identidad puede contener varias notificaciones con varios valores y puede contener varias notificaciones del mismo tipo.</span><span class="sxs-lookup"><span data-stu-id="dacb8-112">An identity can contain multiple claims with multiple values and can contain multiple claims of the same type.</span></span>

## <a name="adding-claims-checks"></a><span data-ttu-id="dacb8-113">Agregar comprobaciones de notificaciones</span><span class="sxs-lookup"><span data-stu-id="dacb8-113">Adding claims checks</span></span>

<span data-ttu-id="dacb8-114">Notificación de comprobaciones de autorización basado en son declarativas: el desarrollador incrusta dentro de su código, con un controlador o una acción dentro de un controlador, especificar las notificaciones que debe poseer el usuario actual y, opcionalmente, debe contener el valor de la notificación para tener acceso a la recurso solicitado.</span><span class="sxs-lookup"><span data-stu-id="dacb8-114">Claim based authorization checks are declarative - the developer embeds them within their code, against a controller or an action within a controller, specifying claims which the current user must possess, and optionally the value the claim must hold to access the requested resource.</span></span> <span data-ttu-id="dacb8-115">Notificaciones de los requisitos son basada en directivas, el desarrollador debe crear y registrar una directiva de expresar los requisitos de notificaciones.</span><span class="sxs-lookup"><span data-stu-id="dacb8-115">Claims requirements are policy based, the developer must build and register a policy expressing the claims requirements.</span></span>

<span data-ttu-id="dacb8-116">El tipo más sencillo de directiva Busca la presencia de una notificación de notificación y no comprueba el valor.</span><span class="sxs-lookup"><span data-stu-id="dacb8-116">The simplest type of claim policy looks for the presence of a claim and doesn't check the value.</span></span>

<span data-ttu-id="dacb8-117">Primero deberá crear y registrar la directiva.</span><span class="sxs-lookup"><span data-stu-id="dacb8-117">First you need to build and register the policy.</span></span> <span data-ttu-id="dacb8-118">Esta operación se realiza como parte de la configuración del servicio de autorización, que normalmente forma parte de `ConfigureServices()` en su *Startup.cs* archivo.</span><span class="sxs-lookup"><span data-stu-id="dacb8-118">This takes place as part of the Authorization service configuration, which normally takes part in `ConfigureServices()` in your *Startup.cs* file.</span></span>

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

<span data-ttu-id="dacb8-119">En este caso el `EmployeeOnly` directiva comprueba la presencia de un `EmployeeNumber` de notificación de la identidad actual.</span><span class="sxs-lookup"><span data-stu-id="dacb8-119">In this case the `EmployeeOnly` policy checks for the presence of an `EmployeeNumber` claim on the current identity.</span></span>

<span data-ttu-id="dacb8-120">A continuación, aplicar la directiva con la `Policy` propiedad en el `AuthorizeAttribute` atributo para especificar el nombre de la directiva;</span><span class="sxs-lookup"><span data-stu-id="dacb8-120">You then apply the policy using the `Policy` property on the `AuthorizeAttribute` attribute to specify the policy name;</span></span>

```csharp
[Authorize(Policy = "EmployeeOnly")]
public IActionResult VacationBalance()
{
    return View();
}
```

<span data-ttu-id="dacb8-121">El `AuthorizeAttribute` atributo puede aplicarse a un controlador todo, en este caso solo las identidades de la directiva de coincidencia se permitirá acceso a cualquier acción en el controlador.</span><span class="sxs-lookup"><span data-stu-id="dacb8-121">The `AuthorizeAttribute` attribute can be applied to an entire controller, in this instance only identities matching the policy will be allowed access to any Action on the controller.</span></span>

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class VacationController : Controller
{
    public ActionResult VacationBalance()
    {
    }
}
```

<span data-ttu-id="dacb8-122">Si tiene un controlador que está protegido por la `AuthorizeAttribute` de atributo, pero desea permitir el acceso anónimo a acciones concretas que se aplica los `AllowAnonymousAttribute` atributo.</span><span class="sxs-lookup"><span data-stu-id="dacb8-122">If you have a controller that's protected by the `AuthorizeAttribute` attribute, but want to allow anonymous access to particular actions you apply the `AllowAnonymousAttribute` attribute.</span></span>

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

<span data-ttu-id="dacb8-123">La mayoría de las notificaciones incluyen un valor.</span><span class="sxs-lookup"><span data-stu-id="dacb8-123">Most claims come with a value.</span></span> <span data-ttu-id="dacb8-124">Puede especificar una lista de valores permitidos al crear la directiva.</span><span class="sxs-lookup"><span data-stu-id="dacb8-124">You can specify a list of allowed values when creating the policy.</span></span> <span data-ttu-id="dacb8-125">El ejemplo siguiente se realizaría correctamente sólo para los empleados cuyo número de empleado era 1, 2, 3, 4 o 5.</span><span class="sxs-lookup"><span data-stu-id="dacb8-125">The following example would only succeed for employees whose employee number was 1, 2, 3, 4 or 5.</span></span>

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

### <a name="add-a-generic-claim-check"></a><span data-ttu-id="dacb8-126">Agregar una comprobación de solicitud genérico</span><span class="sxs-lookup"><span data-stu-id="dacb8-126">Add a generic claim check</span></span>

<span data-ttu-id="dacb8-127">Si el valor de notificación no es un valor único o una transformación es necesaria, utilice [RequireAssertion](/dotnet/api/microsoft.aspnetcore.authorization.authorizationpolicybuilder.requireassertion).</span><span class="sxs-lookup"><span data-stu-id="dacb8-127">If the claim value isn't a single value or a transformation is required, use [RequireAssertion](/dotnet/api/microsoft.aspnetcore.authorization.authorizationpolicybuilder.requireassertion).</span></span> <span data-ttu-id="dacb8-128">Para obtener más información, consulte [mediante func para satisfacer una directiva](xref:security/authorization/policies#using-a-func-to-fulfill-a-policy).</span><span class="sxs-lookup"><span data-stu-id="dacb8-128">For more information, see [Using a func to fulfill a policy](xref:security/authorization/policies#using-a-func-to-fulfill-a-policy).</span></span>

## <a name="multiple-policy-evaluation"></a><span data-ttu-id="dacb8-129">Evaluación de directiva múltiples</span><span class="sxs-lookup"><span data-stu-id="dacb8-129">Multiple Policy Evaluation</span></span>

<span data-ttu-id="dacb8-130">Si varias directivas se aplican a un controlador o acción, todas las directivas deben pasar antes de que se concede acceso.</span><span class="sxs-lookup"><span data-stu-id="dacb8-130">If you apply multiple policies to a controller or action, then all policies must pass before access is granted.</span></span> <span data-ttu-id="dacb8-131">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="dacb8-131">For example:</span></span>

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

<span data-ttu-id="dacb8-132">En el ejemplo anterior cualquier identidad que cumple el `EmployeeOnly` directiva puede tener acceso a la `Payslip` acción como esa directiva se aplica en el controlador.</span><span class="sxs-lookup"><span data-stu-id="dacb8-132">In the above example any identity which fulfills the `EmployeeOnly` policy can access the `Payslip` action as that policy is enforced on the controller.</span></span> <span data-ttu-id="dacb8-133">Sin embargo para poder llamar a la `UpdateSalary` acción debe cumplir la identidad *ambos* el `EmployeeOnly` directiva y la `HumanResources` directiva.</span><span class="sxs-lookup"><span data-stu-id="dacb8-133">However in order to call the `UpdateSalary` action the identity must fulfill *both* the `EmployeeOnly` policy and the `HumanResources` policy.</span></span>

<span data-ttu-id="dacb8-134">Si desea que las directivas más complicadas, como llevar a cabo una fecha de nacimiento notificación, calcular una edad de él, a continuación, comprobar la edad es 21 o una versión anterior, deberá escribir [controladores de directiva personalizada](xref:security/authorization/policies).</span><span class="sxs-lookup"><span data-stu-id="dacb8-134">If you want more complicated policies, such as taking a date of birth claim, calculating an age from it then checking the age is 21 or older then you need to write [custom policy handlers](xref:security/authorization/policies).</span></span>
