---
title: "Autorización personalizada basada en directivas en ASP.NET Core"
author: rick-anderson
description: "Obtenga información acerca de cómo crear y usar controladores de directiva de autorización personalizada para exigir requisitos de autorización en una aplicación de ASP.NET Core."
keywords: "Núcleo de ASP.NET, la autorización, la directiva personalizada, la directiva de autorización"
ms.author: riande
ms.custom: mvc
manager: wpickett
ms.date: 11/21/2017
ms.topic: article
ms.assetid: e422a1b2-dc4a-4bcc-b8d9-7ee62009b6a3
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/policies
ms.openlocfilehash: 280dd72b75e39546061d8455931f597f50c829fe
ms.sourcegitcommit: f1436107b4c022b26f5235dddef103cec5aa6bff
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/15/2017
---
# <a name="custom-policy-based-authorization"></a><span data-ttu-id="9b378-104">Autorización personalizada basada en directivas</span><span class="sxs-lookup"><span data-stu-id="9b378-104">Custom policy-based authorization</span></span>

<span data-ttu-id="9b378-105">Interiormente, [autorización basada en roles](xref:security/authorization/roles) y [autorización basada en notificaciones](xref:security/authorization/claims) utilizan un requisito, un controlador de requisito y una directiva configurada previamente.</span><span class="sxs-lookup"><span data-stu-id="9b378-105">Underneath the covers, [role-based authorization](xref:security/authorization/roles) and [claims-based authorization](xref:security/authorization/claims) use a requirement, a requirement handler, and a pre-configured policy.</span></span> <span data-ttu-id="9b378-106">Estos bloques de creación se admite la expresión de evaluaciones de autorización en el código.</span><span class="sxs-lookup"><span data-stu-id="9b378-106">These building blocks support the expression of authorization evaluations in code.</span></span> <span data-ttu-id="9b378-107">El resultado es una estructura de autorización más enriquecida, reutilizables, comprobable.</span><span class="sxs-lookup"><span data-stu-id="9b378-107">The result is a richer, reusable, testable authorization structure.</span></span>

<span data-ttu-id="9b378-108">Una directiva de autorización está formada por uno o más requisitos.</span><span class="sxs-lookup"><span data-stu-id="9b378-108">An authorization policy consists of one or more requirements.</span></span> <span data-ttu-id="9b378-109">Se registra como parte de la configuración de servicio de autorización, en la `ConfigureServices` método de la `Startup` clase:</span><span class="sxs-lookup"><span data-stu-id="9b378-109">It's registered as part of the authorization service configuration, in the `ConfigureServices` method of the `Startup` class:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63,72)]

<span data-ttu-id="9b378-110">En el ejemplo anterior, se crea una directiva de "AtLeast21".</span><span class="sxs-lookup"><span data-stu-id="9b378-110">In the preceding example, an "AtLeast21" policy is created.</span></span> <span data-ttu-id="9b378-111">Tiene un requisito único, que de una antigüedad mínima, que se proporciona como un parámetro al requisito.</span><span class="sxs-lookup"><span data-stu-id="9b378-111">It has a single requirement, that of a minimum age, which is supplied as a parameter to the requirement.</span></span>

<span data-ttu-id="9b378-112">Las directivas se aplican mediante el `[Authorize]` atributo con el nombre de la directiva.</span><span class="sxs-lookup"><span data-stu-id="9b378-112">Policies are applied by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="9b378-113">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="9b378-113">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="requirements"></a><span data-ttu-id="9b378-114">Requisitos</span><span class="sxs-lookup"><span data-stu-id="9b378-114">Requirements</span></span>

<span data-ttu-id="9b378-115">Un requisito de autorización es una colección de parámetros de datos que puede usar una directiva para evaluar la entidad de seguridad del usuario actual.</span><span class="sxs-lookup"><span data-stu-id="9b378-115">An authorization requirement is a collection of data parameters that a policy can use to evaluate the current user principal.</span></span> <span data-ttu-id="9b378-116">En nuestra directiva de "AtLeast21", el requisito es un parámetro único&mdash;la antigüedad mínima.</span><span class="sxs-lookup"><span data-stu-id="9b378-116">In our "AtLeast21" policy, the requirement is a single parameter&mdash;the minimum age.</span></span> <span data-ttu-id="9b378-117">Implementa un requisito `IAuthorizationRequirement`, que es una interfaz de marcador vacío.</span><span class="sxs-lookup"><span data-stu-id="9b378-117">A requirement implements `IAuthorizationRequirement`, which is an empty marker interface.</span></span> <span data-ttu-id="9b378-118">Un requisito de antigüedad mínima con parámetros podría implementarse como sigue:</span><span class="sxs-lookup"><span data-stu-id="9b378-118">A parameterized minimum age requirement could be implemented as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

> [!NOTE]
> <span data-ttu-id="9b378-119">No tiene un requisito que tienen datos o propiedades.</span><span class="sxs-lookup"><span data-stu-id="9b378-119">A requirement doesn't need to have data or properties.</span></span>

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a><span data-ttu-id="9b378-120">Controladores de autorización</span><span class="sxs-lookup"><span data-stu-id="9b378-120">Authorization handlers</span></span>

<span data-ttu-id="9b378-121">Un controlador de autorización es responsable de la evaluación de propiedades de un requisito.</span><span class="sxs-lookup"><span data-stu-id="9b378-121">An authorization handler is responsible for the evaluation of a requirement's properties.</span></span> <span data-ttu-id="9b378-122">El controlador de autorización evalúa los requisitos con proporcionado `AuthorizationHandlerContext` para determinar si se permite el acceso.</span><span class="sxs-lookup"><span data-stu-id="9b378-122">The authorization handler evaluates the requirements against a provided `AuthorizationHandlerContext` to determine if access is allowed.</span></span> <span data-ttu-id="9b378-123">Puede tener un requisito [varios controladores](#security-authorization-policies-based-multiple-handlers).</span><span class="sxs-lookup"><span data-stu-id="9b378-123">A requirement can have [multiple handlers](#security-authorization-policies-based-multiple-handlers).</span></span> <span data-ttu-id="9b378-124">Heredan de controladores `AuthorizationHandler<T>`, donde `T` es el requisito para procesarse.</span><span class="sxs-lookup"><span data-stu-id="9b378-124">Handlers inherit `AuthorizationHandler<T>`, where `T` is the requirement to be handled.</span></span>

<a name="security-authorization-handler-example"></a>

<span data-ttu-id="9b378-125">El controlador de antigüedad mínima podría ser similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="9b378-125">The minimum age handler might look like this:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

<span data-ttu-id="9b378-126">El código anterior determina si la entidad de seguridad del usuario actual tiene una fecha de nacimiento de notificación de que se ha emitido por un emisor conocido y de confianza.</span><span class="sxs-lookup"><span data-stu-id="9b378-126">The preceding code determines if the current user principal has a date of birth claim which has been issued by a known and trusted Issuer.</span></span> <span data-ttu-id="9b378-127">No se puede realizar la autorización cuando falta la notificación, en cuyo caso se devuelve una tarea completa.</span><span class="sxs-lookup"><span data-stu-id="9b378-127">Authorization can't occur when the claim is missing, in which case a completed task is returned.</span></span> <span data-ttu-id="9b378-128">Cuando está presente una notificación, se calcula la edad del usuario.</span><span class="sxs-lookup"><span data-stu-id="9b378-128">When a claim is present, the user's age is calculated.</span></span> <span data-ttu-id="9b378-129">Si el usuario cumple con la antigüedad mínima definida por el requisito, la autorización se considere correcta.</span><span class="sxs-lookup"><span data-stu-id="9b378-129">If the user meets the minimum age defined by the requirement, authorization is deemed successful.</span></span> <span data-ttu-id="9b378-130">Cuando se realiza correctamente, la autorización `context.Succeed` se invoca con el requisito satisfecho como un parámetro.</span><span class="sxs-lookup"><span data-stu-id="9b378-130">When authorization is successful, `context.Succeed` is invoked with the satisfied requirement as a parameter.</span></span>

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a><span data-ttu-id="9b378-131">Registro del controlador</span><span class="sxs-lookup"><span data-stu-id="9b378-131">Handler registration</span></span>

<span data-ttu-id="9b378-132">Los controladores se registran en la colección de servicios durante la configuración.</span><span class="sxs-lookup"><span data-stu-id="9b378-132">Handlers are registered in the services collection during configuration.</span></span> <span data-ttu-id="9b378-133">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="9b378-133">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63-65,72)]

<span data-ttu-id="9b378-134">Cada controlador se agrega a la colección de servicios mediante la invocación de `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();`.</span><span class="sxs-lookup"><span data-stu-id="9b378-134">Each handler is added to the services collection by invoking `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();`.</span></span>

## <a name="what-should-a-handler-return"></a><span data-ttu-id="9b378-135">¿Qué debe devolver un controlador?</span><span class="sxs-lookup"><span data-stu-id="9b378-135">What should a handler return?</span></span>

<span data-ttu-id="9b378-136">Tenga en cuenta que la `Handle` método en el [controlador (ejemplo)](#security-authorization-handler-example) no devuelve ningún valor.</span><span class="sxs-lookup"><span data-stu-id="9b378-136">Note that the `Handle` method in the [handler example](#security-authorization-handler-example) returns no value.</span></span> <span data-ttu-id="9b378-137">¿Cómo es un estado de correcto o erróneo indicadas?</span><span class="sxs-lookup"><span data-stu-id="9b378-137">How is a status of either success or failure indicated?</span></span>

* <span data-ttu-id="9b378-138">Un controlador indica éxito mediante una llamada a `context.Succeed(IAuthorizationRequirement requirement)`, pasando el requisito de que se ha validado correctamente.</span><span class="sxs-lookup"><span data-stu-id="9b378-138">A handler indicates success by calling `context.Succeed(IAuthorizationRequirement requirement)`, passing the requirement that has been successfully validated.</span></span>

* <span data-ttu-id="9b378-139">Un controlador no necesita controlar los errores por lo general, como otros controladores para el mismo requisito pueden realizarse correctamente.</span><span class="sxs-lookup"><span data-stu-id="9b378-139">A handler does not need to handle failures generally, as other handlers for the same requirement may succeed.</span></span>

* <span data-ttu-id="9b378-140">Para garantizar el error, incluso si otros controladores de requisito correctamente, llame a `context.Fail`.</span><span class="sxs-lookup"><span data-stu-id="9b378-140">To guarantee failure, even if other requirement handlers succeed, call `context.Fail`.</span></span>

<span data-ttu-id="9b378-141">Independientemente de lo que se llama a dentro de su controlador, todos los controladores para un requisito se llamará cuando una directiva exige el requisito.</span><span class="sxs-lookup"><span data-stu-id="9b378-141">Regardless of what you call inside your handler, all handlers for a requirement will be called when a policy requires the requirement.</span></span> <span data-ttu-id="9b378-142">Esto permite requisitos tienen efectos secundarios, como el registro, que siempre se llevará a cabo aunque `context.Fail()` se ha llamado en otro controlador.</span><span class="sxs-lookup"><span data-stu-id="9b378-142">This allows requirements to have side effects, such as logging, which will always take place even if `context.Fail()` has been called in another handler.</span></span>

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a><span data-ttu-id="9b378-143">¿Por qué desearía varios controladores para un requisito?</span><span class="sxs-lookup"><span data-stu-id="9b378-143">Why would I want multiple handlers for a requirement?</span></span>

<span data-ttu-id="9b378-144">En casos donde probablemente prefiera evaluación en un **o** base, implementar varios controladores para un requisito único.</span><span class="sxs-lookup"><span data-stu-id="9b378-144">In cases where you want evaluation to be on an **OR** basis, implement multiple handlers for a single requirement.</span></span> <span data-ttu-id="9b378-145">Por ejemplo, Microsoft tiene puertas que solo se abren con tarjetas de clave.</span><span class="sxs-lookup"><span data-stu-id="9b378-145">For example, Microsoft has doors which only open with key cards.</span></span> <span data-ttu-id="9b378-146">Si deja la tarjeta de claves en casa, la recepcionista imprime una etiqueta temporal y abre la puerta para usted.</span><span class="sxs-lookup"><span data-stu-id="9b378-146">If you leave your key card at home, the receptionist prints a temporary sticker and opens the door for you.</span></span> <span data-ttu-id="9b378-147">En este escenario, tendría un requisito único, *BuildingEntry*, pero varios controladores, cada uno de ellos examinando un requisito único.</span><span class="sxs-lookup"><span data-stu-id="9b378-147">In this scenario, you'd have a single requirement, *BuildingEntry*, but multiple handlers, each one examining a single requirement.</span></span>

<span data-ttu-id="9b378-148">*BuildingEntryRequirement.cs*</span><span class="sxs-lookup"><span data-stu-id="9b378-148">*BuildingEntryRequirement.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

<span data-ttu-id="9b378-149">*BadgeEntryHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="9b378-149">*BadgeEntryHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

<span data-ttu-id="9b378-150">*TemporaryStickerHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="9b378-150">*TemporaryStickerHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

<span data-ttu-id="9b378-151">Asegúrese de que ambos controladores estén [registrado](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span><span class="sxs-lookup"><span data-stu-id="9b378-151">Ensure that both handlers are [registered](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span></span> <span data-ttu-id="9b378-152">Si cualquier controlador se ejecuta correctamente cuando una directiva se evalúa como el `BuildingEntryRequirement`, la evaluación de directivas se realiza correctamente.</span><span class="sxs-lookup"><span data-stu-id="9b378-152">If either handler succeeds when a policy evaluates the `BuildingEntryRequirement`, the policy evaluation succeeds.</span></span>

## <a name="using-a-func-to-fulfill-a-policy"></a><span data-ttu-id="9b378-153">Usar un elemento func para cumplir una directiva</span><span class="sxs-lookup"><span data-stu-id="9b378-153">Using a func to fulfill a policy</span></span>

<span data-ttu-id="9b378-154">Puede haber situaciones en las que cumplir una directiva es simple expresar en el código.</span><span class="sxs-lookup"><span data-stu-id="9b378-154">There may be situations in which fulfilling a policy is simple to express in code.</span></span> <span data-ttu-id="9b378-155">Es posible proporcionar un `Func<AuthorizationHandlerContext, bool>` al configurar la directiva con el `RequireAssertion` el generador de directiva.</span><span class="sxs-lookup"><span data-stu-id="9b378-155">It's possible to supply a `Func<AuthorizationHandlerContext, bool>` when configuring your policy with the `RequireAssertion` policy builder.</span></span>

<span data-ttu-id="9b378-156">Por ejemplo, el anterior `BadgeEntryHandler` podría volver a escribir como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="9b378-156">For example, the previous `BadgeEntryHandler` could be rewritten as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=52-53,57-63)]

## <a name="accessing-mvc-request-context-in-handlers"></a><span data-ttu-id="9b378-157">Acceso al contexto de solicitud MVC en los controladores</span><span class="sxs-lookup"><span data-stu-id="9b378-157">Accessing MVC request context in handlers</span></span>

<span data-ttu-id="9b378-158">El `HandleRequirementAsync` método se implementa en un controlador de autorización tiene dos parámetros: una `AuthorizationHandlerContext` y `TRequirement` está controlando.</span><span class="sxs-lookup"><span data-stu-id="9b378-158">The `HandleRequirementAsync` method you implement in an authorization handler has two parameters: an `AuthorizationHandlerContext` and the `TRequirement` you are handling.</span></span> <span data-ttu-id="9b378-159">Marcos de trabajo como MVC o Jabbr pueden agregar cualquier objeto a la `Resource` propiedad en el `AuthorizationHandlerContext` para pasar información adicional.</span><span class="sxs-lookup"><span data-stu-id="9b378-159">Frameworks such as MVC or Jabbr are free to add any object to the `Resource` property on the `AuthorizationHandlerContext` to pass extra information.</span></span>

<span data-ttu-id="9b378-160">Por ejemplo, MVC pasa una instancia de [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) en el `Resource` propiedad.</span><span class="sxs-lookup"><span data-stu-id="9b378-160">For example, MVC passes an instance of [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) in the `Resource` property.</span></span> <span data-ttu-id="9b378-161">Esta propiedad proporciona acceso a `HttpContext`, `RouteData`y todo el contenido más proporcionó MVC y las páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="9b378-161">This property provides access to `HttpContext`, `RouteData`, and everything else provided by MVC and Razor Pages.</span></span>

<span data-ttu-id="9b378-162">El uso de la `Resource` propiedad es específicos de la plataforma.</span><span class="sxs-lookup"><span data-stu-id="9b378-162">The use of the `Resource` property is framework specific.</span></span> <span data-ttu-id="9b378-163">De manera indicada en el `Resource` propiedad limita las directivas de autorización para marcos de trabajo determinados.</span><span class="sxs-lookup"><span data-stu-id="9b378-163">Using information in the `Resource` property limits your authorization policies to particular frameworks.</span></span> <span data-ttu-id="9b378-164">Debe convertir el `Resource` propiedad mediante la `as` (palabra clave) y, a continuación, confirme la conversión se realizará correctamente para asegurarse de que el código de no bloqueo con un `InvalidCastException` cuando se ejecuta en otros marcos de trabajo:</span><span class="sxs-lookup"><span data-stu-id="9b378-164">You should cast the `Resource` property using the `as` keyword, and then confirm the cast has succeed to ensure your code doesn't crash with an `InvalidCastException` when run on other frameworks:</span></span>

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```
