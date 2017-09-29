---
title: "Autorización personalizada basada en directivas"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: e422a1b2-dc4a-4bcc-b8d9-7ee62009b6a3
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/policies
ms.openlocfilehash: 5021b5d20f6d9b9a4d8889f25b5e41f2c9306f64
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/28/2017
---
# <a name="custom-policy-based-authorization"></a><span data-ttu-id="07f85-103">Autorización personalizada basada en directivas</span><span class="sxs-lookup"><span data-stu-id="07f85-103">Custom Policy-Based Authorization</span></span>

<a name=security-authorization-policies-based></a>

<span data-ttu-id="07f85-104">Interiormente el [autorización rol](roles.md#security-authorization-role-based) y [autorización de notificaciones](claims.md#security-authorization-claims-based) hacer uso de un requisito y un controlador para el requisito y una directiva configurada previamente.</span><span class="sxs-lookup"><span data-stu-id="07f85-104">Underneath the covers the [role authorization](roles.md#security-authorization-role-based) and [claims authorization](claims.md#security-authorization-claims-based) make use of a requirement, a handler for the requirement and a pre-configured policy.</span></span> <span data-ttu-id="07f85-105">Estos bloques de creación permiten expresar las evaluaciones de autorización en el código, lo que permite una experiencia mejor, reutilizables y la estructura de autorización puede probar fácilmente.</span><span class="sxs-lookup"><span data-stu-id="07f85-105">These building blocks allow you to express authorization evaluations in code, allowing for a richer, reusable, and easily testable authorization structure.</span></span>

<span data-ttu-id="07f85-106">Una directiva de autorización está formada por uno o varios requisitos y registrada al iniciar la aplicación como parte de la configuración de servicio de autorización, en `ConfigureServices` en el *Startup.cs* archivo.</span><span class="sxs-lookup"><span data-stu-id="07f85-106">An authorization policy is made up of one or more requirements and registered at application startup as part of the Authorization service configuration, in `ConfigureServices` in the *Startup.cs* file.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("Over21",
                          policy => policy.Requirements.Add(new MinimumAgeRequirement(21)));
    });
}
```

<span data-ttu-id="07f85-107">Aquí puede ver que una directiva de "Edad superior a 21" se crea con un requisito único, que de una antigüedad mínima, que se pasa como parámetro al requisito.</span><span class="sxs-lookup"><span data-stu-id="07f85-107">Here you can see an "Over21" policy is created with a single requirement, that of a minimum age, which is passed as a parameter to the requirement.</span></span>

<span data-ttu-id="07f85-108">Las directivas se aplican mediante el `Authorize` atributo especificando el nombre de la directiva, por ejemplo,</span><span class="sxs-lookup"><span data-stu-id="07f85-108">Policies are applied using the `Authorize` attribute by specifying the policy name, for example;</span></span>

```csharp
[Authorize(Policy="Over21")]
public class AlcoholPurchaseRequirementsController : Controller
{
    public ActionResult Login()
    {
    }

    public ActionResult Logout()
    {
    }
}
```

## <a name="requirements"></a><span data-ttu-id="07f85-109">Requisitos</span><span class="sxs-lookup"><span data-stu-id="07f85-109">Requirements</span></span>

<span data-ttu-id="07f85-110">Un requisito de autorización es una colección de parámetros de datos que puede usar una directiva para evaluar la entidad de seguridad del usuario actual.</span><span class="sxs-lookup"><span data-stu-id="07f85-110">An authorization requirement is a collection of data parameters that a policy can use to evaluate the current user principal.</span></span> <span data-ttu-id="07f85-111">En nuestra directiva de antigüedad mínima el requisito que tenemos es un único parámetro, la antigüedad mínima.</span><span class="sxs-lookup"><span data-stu-id="07f85-111">In our Minimum Age policy the requirement we have is a single parameter, the minimum age.</span></span> <span data-ttu-id="07f85-112">Debe implementar un requisito `IAuthorizationRequirement`.</span><span class="sxs-lookup"><span data-stu-id="07f85-112">A requirement must implement `IAuthorizationRequirement`.</span></span> <span data-ttu-id="07f85-113">Se trata de una interfaz vacía, de marcador.</span><span class="sxs-lookup"><span data-stu-id="07f85-113">This is an empty, marker interface.</span></span> <span data-ttu-id="07f85-114">Un requisito de antigüedad mínima con parámetros podría implementarse de la manera siguiente:</span><span class="sxs-lookup"><span data-stu-id="07f85-114">A parameterized minimum age requirement might be implemented as follows;</span></span>

```csharp
public class MinimumAgeRequirement : IAuthorizationRequirement
{
    public int MinimumAge { get; private set; }
    
    public MinimumAgeRequirement(int minimumAge)
    {
        MinimumAge = minimumAge;
    }
}
```

<span data-ttu-id="07f85-115">No tiene un requisito que tienen datos o propiedades.</span><span class="sxs-lookup"><span data-stu-id="07f85-115">A requirement doesn't need to have data or properties.</span></span>

<a name=security-authorization-policies-based-authorization-handler></a>

## <a name="authorization-handlers"></a><span data-ttu-id="07f85-116">Controladores de autorización</span><span class="sxs-lookup"><span data-stu-id="07f85-116">Authorization Handlers</span></span>

<span data-ttu-id="07f85-117">Un controlador de autorización es responsable de la evaluación de las propiedades de un requisito.</span><span class="sxs-lookup"><span data-stu-id="07f85-117">An authorization handler is responsible for the evaluation of any properties of a requirement.</span></span> <span data-ttu-id="07f85-118">El controlador de autorización debe evaluará con respecto a proporcionado `AuthorizationHandlerContext` para decidir si se permite la autorización.</span><span class="sxs-lookup"><span data-stu-id="07f85-118">The  authorization handler must evaluate them against a provided `AuthorizationHandlerContext` to decide if authorization is allowed.</span></span> <span data-ttu-id="07f85-119">Puede tener un requisito [varios controladores](policies.md#security-authorization-policies-based-multiple-handlers).</span><span class="sxs-lookup"><span data-stu-id="07f85-119">A requirement can have [multiple handlers](policies.md#security-authorization-policies-based-multiple-handlers).</span></span> <span data-ttu-id="07f85-120">Los controladores deben heredar `AuthorizationHandler<T>` donde T es el requisito es capaz de abrir.</span><span class="sxs-lookup"><span data-stu-id="07f85-120">Handlers must inherit `AuthorizationHandler<T>` where T is the requirement it handles.</span></span>

<a name=security-authorization-handler-example></a>

<span data-ttu-id="07f85-121">El controlador de antigüedad mínima podría ser similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="07f85-121">The minimum age handler might look like this:</span></span>

```csharp
public class MinimumAgeHandler : AuthorizationHandler<MinimumAgeRequirement>
{
    protected override Task HandleRequirementAsync(AuthorizationHandlerContext context, MinimumAgeRequirement requirement)
    {
        if (!context.User.HasClaim(c => c.Type == ClaimTypes.DateOfBirth &&
                                   c.Issuer == "http://contoso.com"))
        {
            // .NET 4.x -> return Task.FromResult(0);
            return Task.CompletedTask;
        }

        var dateOfBirth = Convert.ToDateTime(context.User.FindFirst(
            c => c.Type == ClaimTypes.DateOfBirth && c.Issuer == "http://contoso.com").Value);

        int calculatedAge = DateTime.Today.Year - dateOfBirth.Year;
        if (dateOfBirth > DateTime.Today.AddYears(-calculatedAge))
        {
            calculatedAge--;
        }

        if (calculatedAge >= requirement.MinimumAge)
        {
            context.Succeed(requirement);
        }
        return Task.CompletedTask;
    }
}
```

<span data-ttu-id="07f85-122">En el código anterior se busque primero para ver si la entidad de seguridad del usuario actual tiene una fecha de nacimiento que ha sido emitidos por un emisor que sabemos y confianza de notificaciones.</span><span class="sxs-lookup"><span data-stu-id="07f85-122">In the code above we first look to see if the current user principal has a date of birth claim which has been issued by an Issuer we know and trust.</span></span> <span data-ttu-id="07f85-123">Si falta la notificación se no autorizar por lo que se devuelven.</span><span class="sxs-lookup"><span data-stu-id="07f85-123">If the claim is missing we can't authorize so we return.</span></span> <span data-ttu-id="07f85-124">Si tenemos una notificación, se pueda determinar la antigüedad del usuario es, y si cumple la antigüedad mínima que se pasa por el requisito, a continuación, autorización ha correcta.</span><span class="sxs-lookup"><span data-stu-id="07f85-124">If we have a claim, we figure out how old the user is, and if they meet the minimum age passed in by the requirement then authorization has been successful.</span></span> <span data-ttu-id="07f85-125">Una vez que la autorización es correcta llamamos `context.Succeed()` pasando el requisito de que ha tenido éxito como un parámetro.</span><span class="sxs-lookup"><span data-stu-id="07f85-125">Once authorization is successful we call `context.Succeed()` passing in the requirement that has been successful as a parameter.</span></span>

<a name=security-authorization-policies-based-handler-registration></a>

<span data-ttu-id="07f85-126">Los controladores deben estar registrados en la colección de servicios durante la configuración, por ejemplo,</span><span class="sxs-lookup"><span data-stu-id="07f85-126">Handlers must be registered in the services collection during configuration, for example;</span></span>

```csharp

public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("Over21",
                          policy => policy.Requirements.Add(new MinimumAgeRequirement(21)));
    });

    services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();
}
```

<span data-ttu-id="07f85-127">Cada controlador se agrega a la colección de servicios mediante `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();` pasar en la clase de controlador.</span><span class="sxs-lookup"><span data-stu-id="07f85-127">Each handler is added to the services collection by using `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();` passing in your handler class.</span></span>

## <a name="what-should-a-handler-return"></a><span data-ttu-id="07f85-128">¿Qué debe devolver un controlador?</span><span class="sxs-lookup"><span data-stu-id="07f85-128">What should a handler return?</span></span>

<span data-ttu-id="07f85-129">Puede ver en nuestro [controlador (ejemplo)](policies.md#security-authorization-handler-example) que la `Handle()` método no tiene ningún valor devuelto, entonces, ¿cómo se indicaba correcto o erróneo?</span><span class="sxs-lookup"><span data-stu-id="07f85-129">You can see in our [handler example](policies.md#security-authorization-handler-example) that the `Handle()` method has no return value, so how do we indicate success or failure?</span></span>

* <span data-ttu-id="07f85-130">Un controlador indica éxito mediante una llamada a `context.Succeed(IAuthorizationRequirement requirement)`, pasando el requisito de que se ha validado correctamente.</span><span class="sxs-lookup"><span data-stu-id="07f85-130">A handler indicates success by calling `context.Succeed(IAuthorizationRequirement requirement)`, passing the requirement that has been successfully validated.</span></span>

* <span data-ttu-id="07f85-131">Un controlador no necesita controlar los errores por lo general, como otros controladores para el mismo requisito pueden realizarse correctamente.</span><span class="sxs-lookup"><span data-stu-id="07f85-131">A handler does not need to handle failures generally, as other handlers for the same requirement may succeed.</span></span>

* <span data-ttu-id="07f85-132">Para garantizar el error incluso si otros controladores para un requisito correctamente, llame a `context.Fail`.</span><span class="sxs-lookup"><span data-stu-id="07f85-132">To guarantee failure even if other handlers for a requirement succeed, call `context.Fail`.</span></span>

<span data-ttu-id="07f85-133">Independientemente de lo que se llama a dentro de su controlador de todos los controladores para un requisito se llamará cuando una directiva exige el requisito.</span><span class="sxs-lookup"><span data-stu-id="07f85-133">Regardless of what you call inside your handler all handlers for a requirement will be called when a policy requires the requirement.</span></span> <span data-ttu-id="07f85-134">Esto permite requisitos tienen efectos secundarios, como el registro, que siempre se llevará a cabo aunque `context.Fail()` se ha llamado en otro controlador.</span><span class="sxs-lookup"><span data-stu-id="07f85-134">This allows requirements to have side effects, such as logging, which will always take place even if `context.Fail()` has been called in another handler.</span></span>

<a name=security-authorization-policies-based-multiple-handlers></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a><span data-ttu-id="07f85-135">¿Por qué desearía varios controladores para un requisito?</span><span class="sxs-lookup"><span data-stu-id="07f85-135">Why would I want multiple handlers for a requirement?</span></span>

<span data-ttu-id="07f85-136">En casos donde probablemente prefiera evaluación en un **o** base se implementan varios controladores para un requisito único.</span><span class="sxs-lookup"><span data-stu-id="07f85-136">In cases where you want evaluation to be on an **OR** basis you implement multiple handlers for a single requirement.</span></span> <span data-ttu-id="07f85-137">Por ejemplo, Microsoft tiene puertas que solo se abren con tarjetas de clave.</span><span class="sxs-lookup"><span data-stu-id="07f85-137">For example, Microsoft has doors which only open with key cards.</span></span> <span data-ttu-id="07f85-138">Si deja la tarjeta de claves en casa la recepcionista imprime una etiqueta temporal y abre la puerta para usted.</span><span class="sxs-lookup"><span data-stu-id="07f85-138">If you leave your key card at home the receptionist prints a temporary sticker and opens the door for you.</span></span> <span data-ttu-id="07f85-139">En este escenario tendría un requisito único, *EnterBuilding*, pero varios controladores, cada uno de ellos examinando un requisito único.</span><span class="sxs-lookup"><span data-stu-id="07f85-139">In this scenario you'd have a single requirement, *EnterBuilding*, but multiple handlers, each one examining a single requirement.</span></span>

```csharp
public class EnterBuildingRequirement : IAuthorizationRequirement
{
}

public class BadgeEntryHandler : AuthorizationHandler<EnterBuildingRequirement>
{
    protected override Task HandleRequirementAsync(AuthorizationHandlerContext context, EnterBuildingRequirement requirement)
    {
        if (context.User.HasClaim(c => c.Type == ClaimTypes.BadgeId &&
                                       c.Issuer == "http://microsoftsecurity"))
        {
            context.Succeed(requirement);
        }
        return Task.CompletedTask;
    }
}

public class HasTemporaryStickerHandler : AuthorizationHandler<EnterBuildingRequirement>
{
    protected override Task HandleRequirementAsync(AuthorizationHandlerContext context, EnterBuildingRequirement requirement)
    {
        if (context.User.HasClaim(c => c.Type == ClaimTypes.TemporaryBadgeId &&
                                       c.Issuer == "https://microsoftsecurity"))
        {
            // We'd also check the expiration date on the sticker.
            context.Succeed(requirement);
        }
        return Task.CompletedTask;
    }
}
```

<span data-ttu-id="07f85-140">Ahora, suponiendo que ambos controladores son [registrado](xref:security/authorization/policies#security-authorization-policies-based-handler-registration) cuando una directiva se evalúa como el `EnterBuildingRequirement` si se realiza correctamente en cualquier controlador de la evaluación de directivas se realizará correctamente.</span><span class="sxs-lookup"><span data-stu-id="07f85-140">Now, assuming both handlers are [registered](xref:security/authorization/policies#security-authorization-policies-based-handler-registration) when a policy evaluates the `EnterBuildingRequirement` if either handler succeeds the policy evaluation will succeed.</span></span>

## <a name="using-a-func-to-fulfill-a-policy"></a><span data-ttu-id="07f85-141">Usar un elemento func para cumplir una directiva</span><span class="sxs-lookup"><span data-stu-id="07f85-141">Using a func to fulfill a policy</span></span>

<span data-ttu-id="07f85-142">Puede haber ocasiones donde cumplir una directiva es sencillo expresar en el código.</span><span class="sxs-lookup"><span data-stu-id="07f85-142">There may be occasions where fulfilling a policy is simple to express in code.</span></span> <span data-ttu-id="07f85-143">Es posible que solo tiene que proporcionar un `Func<AuthorizationHandlerContext, bool>` al configurar la directiva con el `RequireAssertion` el generador de directiva.</span><span class="sxs-lookup"><span data-stu-id="07f85-143">It is possible to simply supply a `Func<AuthorizationHandlerContext, bool>` when configuring your policy with the `RequireAssertion` policy builder.</span></span>

<span data-ttu-id="07f85-144">Por ejemplo anterior `BadgeEntryHandler` podría volver a escribir lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="07f85-144">For example the previous `BadgeEntryHandler` could be rewritten as follows;</span></span>

```csharp
services.AddAuthorization(options =>
    {
        options.AddPolicy("BadgeEntry",
                          policy => policy.RequireAssertion(context =>
                                  context.User.HasClaim(c =>
                                     (c.Type == ClaimTypes.BadgeId ||
                                      c.Type == ClaimTypes.TemporaryBadgeId)
                                      && c.Issuer == "https://microsoftsecurity"));
                          }));
    }
 }
```

## <a name="accessing-mvc-request-context-in-handlers"></a><span data-ttu-id="07f85-145">Acceso al contexto de solicitud MVC en los controladores</span><span class="sxs-lookup"><span data-stu-id="07f85-145">Accessing MVC Request Context In Handlers</span></span>

<span data-ttu-id="07f85-146">El `Handle` método que debe implementar en un controlador de autorización tiene dos parámetros, un `AuthorizationContext` y `Requirement` está controlando.</span><span class="sxs-lookup"><span data-stu-id="07f85-146">The `Handle` method you must implement in an authorization handler has two parameters, an `AuthorizationContext` and the `Requirement` you are handling.</span></span> <span data-ttu-id="07f85-147">Marcos de trabajo como MVC o Jabbr pueden agregar cualquier objeto a la `Resource` propiedad en el `AuthorizationContext` para pasar información adicional.</span><span class="sxs-lookup"><span data-stu-id="07f85-147">Frameworks such as MVC or Jabbr are free to add any object to the `Resource` property on the `AuthorizationContext` to pass through extra information.</span></span>

<span data-ttu-id="07f85-148">Por ejemplo MVC pasa una instancia de `Microsoft.AspNetCore.Mvc.Filters.AuthorizationFilterContext` en la propiedad de recurso que se utiliza para tener acceso a HttpContext, RouteData y todo lo demás MVC proporciona.</span><span class="sxs-lookup"><span data-stu-id="07f85-148">For example MVC passes an instance of `Microsoft.AspNetCore.Mvc.Filters.AuthorizationFilterContext` in the resource property which is used to access HttpContext, RouteData and everything else MVC provides.</span></span>

<span data-ttu-id="07f85-149">El uso de la `Resource` propiedad es específicos de la plataforma.</span><span class="sxs-lookup"><span data-stu-id="07f85-149">The use of the `Resource` property is framework specific.</span></span> <span data-ttu-id="07f85-150">De manera indicada en el `Resource` propiedad limitará las directivas de autorización para marcos de trabajo determinados.</span><span class="sxs-lookup"><span data-stu-id="07f85-150">Using information in the `Resource` property will limit your authorization policies to particular frameworks.</span></span> <span data-ttu-id="07f85-151">Primero debe convertir el `Resource` propiedad mediante la `as` palabra clave y, a continuación, compruebe la conversión tiene éxito para asegurarse de que el código de no bloqueo con `InvalidCastExceptions` cuando se ejecuta en otros marcos de trabajo;</span><span class="sxs-lookup"><span data-stu-id="07f85-151">You should cast the `Resource` property using the `as` keyword, and then check the cast has succeed to ensure your code doesn't crash with `InvalidCastExceptions` when run on other frameworks;</span></span>

```csharp
if (context.Resource is Microsoft.AspNetCore.Mvc.Filters.AuthorizationFilterContext mvcContext)
{
    // Examine MVC specific things like routing data.
}
```
