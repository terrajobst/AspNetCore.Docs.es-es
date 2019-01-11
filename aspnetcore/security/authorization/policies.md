---
title: Autorización basada en directivas en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo crear y usar controladores de la directiva de autorización para exigir los requisitos de autorización en una aplicación ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 11/21/2017
uid: security/authorization/policies
ms.openlocfilehash: 4e8a9ac6c0594f9bab67214aaa8cab9199cca29d
ms.sourcegitcommit: cec77d5ad8a0cedb1ecbec32834111492afd0cd2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/10/2019
ms.locfileid: "54207400"
---
# <a name="policy-based-authorization-in-aspnet-core"></a><span data-ttu-id="d44ba-103">Autorización basada en directivas en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d44ba-103">Policy-based authorization in ASP.NET Core</span></span>

<span data-ttu-id="d44ba-104">Interiormente, [autorización basada en roles](xref:security/authorization/roles) y [autorización basada en notificaciones](xref:security/authorization/claims) usan un requisito, un controlador de requisito y una directiva configurada previamente.</span><span class="sxs-lookup"><span data-stu-id="d44ba-104">Underneath the covers, [role-based authorization](xref:security/authorization/roles) and [claims-based authorization](xref:security/authorization/claims) use a requirement, a requirement handler, and a pre-configured policy.</span></span> <span data-ttu-id="d44ba-105">Estos bloques de creación se admite la expresión de evaluaciones de autorización en el código.</span><span class="sxs-lookup"><span data-stu-id="d44ba-105">These building blocks support the expression of authorization evaluations in code.</span></span> <span data-ttu-id="d44ba-106">El resultado es una estructura de autorización completas, reutilizables y apta para las pruebas.</span><span class="sxs-lookup"><span data-stu-id="d44ba-106">The result is a richer, reusable, testable authorization structure.</span></span>

<span data-ttu-id="d44ba-107">Una directiva de autorización consta de uno o más requisitos.</span><span class="sxs-lookup"><span data-stu-id="d44ba-107">An authorization policy consists of one or more requirements.</span></span> <span data-ttu-id="d44ba-108">Está registrado como parte de la configuración del servicio de autorización, en el `Startup.ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="d44ba-108">It's registered as part of the authorization service configuration, in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63,72)]

<span data-ttu-id="d44ba-109">En el ejemplo anterior, se crea una directiva de "AtLeast21".</span><span class="sxs-lookup"><span data-stu-id="d44ba-109">In the preceding example, an "AtLeast21" policy is created.</span></span> <span data-ttu-id="d44ba-110">Tiene un requisito único&mdash;de una antigüedad mínima, que se suministra como parámetro al requisito.</span><span class="sxs-lookup"><span data-stu-id="d44ba-110">It has a single requirement&mdash;that of a minimum age, which is supplied as a parameter to the requirement.</span></span>

<span data-ttu-id="d44ba-111">Las directivas se aplican mediante el uso de la `[Authorize]` atributo con el nombre de la directiva.</span><span class="sxs-lookup"><span data-stu-id="d44ba-111">Policies are applied by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="d44ba-112">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="d44ba-112">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="requirements"></a><span data-ttu-id="d44ba-113">Requisitos</span><span class="sxs-lookup"><span data-stu-id="d44ba-113">Requirements</span></span>

<span data-ttu-id="d44ba-114">Un requisito de autorización es una colección de parámetros de datos que puede usar una directiva para evaluar la entidad de seguridad del usuario actual.</span><span class="sxs-lookup"><span data-stu-id="d44ba-114">An authorization requirement is a collection of data parameters that a policy can use to evaluate the current user principal.</span></span> <span data-ttu-id="d44ba-115">En nuestra directiva de "AtLeast21", el requisito es un único parámetro&mdash;la antigüedad mínima.</span><span class="sxs-lookup"><span data-stu-id="d44ba-115">In our "AtLeast21" policy, the requirement is a single parameter&mdash;the minimum age.</span></span> <span data-ttu-id="d44ba-116">Implementa un requisito [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), que es una interfaz de marcador vacío.</span><span class="sxs-lookup"><span data-stu-id="d44ba-116">A requirement implements [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), which is an empty marker interface.</span></span> <span data-ttu-id="d44ba-117">Un requisito de antigüedad mínima con parámetros podría implementarse como sigue:</span><span class="sxs-lookup"><span data-stu-id="d44ba-117">A parameterized minimum age requirement could be implemented as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

> [!NOTE]
> <span data-ttu-id="d44ba-118">Un requisito no debe tener datos o propiedades.</span><span class="sxs-lookup"><span data-stu-id="d44ba-118">A requirement doesn't need to have data or properties.</span></span>

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a><span data-ttu-id="d44ba-119">Controladores de autorización</span><span class="sxs-lookup"><span data-stu-id="d44ba-119">Authorization handlers</span></span>

<span data-ttu-id="d44ba-120">Un controlador de autorización es responsable de la evaluación de propiedades de un requisito.</span><span class="sxs-lookup"><span data-stu-id="d44ba-120">An authorization handler is responsible for the evaluation of a requirement's properties.</span></span> <span data-ttu-id="d44ba-121">El controlador de autorización evalúa los requisitos en relación con proporcionado [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) para determinar si se permite el acceso.</span><span class="sxs-lookup"><span data-stu-id="d44ba-121">The authorization handler evaluates the requirements against a provided [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) to determine if access is allowed.</span></span>

<span data-ttu-id="d44ba-122">Puede tener un requisito [varios controladores](#security-authorization-policies-based-multiple-handlers).</span><span class="sxs-lookup"><span data-stu-id="d44ba-122">A requirement can have [multiple handlers](#security-authorization-policies-based-multiple-handlers).</span></span> <span data-ttu-id="d44ba-123">Puede heredar un controlador [AuthorizationHandler\<TRequirement >](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), donde `TRequirement` es el requisito para que lo administre.</span><span class="sxs-lookup"><span data-stu-id="d44ba-123">A handler may inherit [AuthorizationHandler\<TRequirement>](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), where `TRequirement` is the requirement to be handled.</span></span> <span data-ttu-id="d44ba-124">Como alternativa, puede implementar un controlador [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) para administrar más de un tipo de requisito.</span><span class="sxs-lookup"><span data-stu-id="d44ba-124">Alternatively, a handler may implement [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) to handle more than one type of requirement.</span></span>

### <a name="use-a-handler-for-one-requirement"></a><span data-ttu-id="d44ba-125">Usar un controlador para uno de los requisitos</span><span class="sxs-lookup"><span data-stu-id="d44ba-125">Use a handler for one requirement</span></span>

<a name="security-authorization-handler-example"></a>

<span data-ttu-id="d44ba-126">El siguiente es un ejemplo de una relación uno a uno en el que un controlador de antigüedad mínima utiliza un requisito único:</span><span class="sxs-lookup"><span data-stu-id="d44ba-126">The following is an example of a one-to-one relationship in which a minimum age handler utilizes a single requirement:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

<span data-ttu-id="d44ba-127">El código anterior determina si la entidad de usuario actual tiene una fecha de nacimiento de notificación de que se ha emitido por un emisor conocido y de confianza.</span><span class="sxs-lookup"><span data-stu-id="d44ba-127">The preceding code determines if the current user principal has a date of birth claim which has been issued by a known and trusted Issuer.</span></span> <span data-ttu-id="d44ba-128">No se puede realizar la autorización cuando falta la notificación, en cuyo caso se devuelve una tarea completada.</span><span class="sxs-lookup"><span data-stu-id="d44ba-128">Authorization can't occur when the claim is missing, in which case a completed task is returned.</span></span> <span data-ttu-id="d44ba-129">Cuando hay una notificación, se calcula la edad del usuario.</span><span class="sxs-lookup"><span data-stu-id="d44ba-129">When a claim is present, the user's age is calculated.</span></span> <span data-ttu-id="d44ba-130">Si el usuario cumple la antigüedad mínima definida por el requisito, la autorización se considere correcta.</span><span class="sxs-lookup"><span data-stu-id="d44ba-130">If the user meets the minimum age defined by the requirement, authorization is deemed successful.</span></span> <span data-ttu-id="d44ba-131">Cuando se realiza correctamente, la autorización `context.Succeed` se invoca con el requisito satisfecho como su único parámetro.</span><span class="sxs-lookup"><span data-stu-id="d44ba-131">When authorization is successful, `context.Succeed` is invoked with the satisfied requirement as its sole parameter.</span></span>

### <a name="use-a-handler-for-multiple-requirements"></a><span data-ttu-id="d44ba-132">Usar un controlador para varios requisitos</span><span class="sxs-lookup"><span data-stu-id="d44ba-132">Use a handler for multiple requirements</span></span>

<span data-ttu-id="d44ba-133">El siguiente es un ejemplo de una relación uno a varios en el que un controlador de permiso puede controlar los tres tipos diferentes de los requisitos:</span><span class="sxs-lookup"><span data-stu-id="d44ba-133">The following is an example of a one-to-many relationship in which a permission handler can handle three different types of requirements:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/PermissionHandler.cs?name=snippet_PermissionHandlerClass)]

<span data-ttu-id="d44ba-134">El código anterior recorre [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;una propiedad que contiene los requisitos no marcada como correcta.</span><span class="sxs-lookup"><span data-stu-id="d44ba-134">The preceding code traverses [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;a property containing requirements not marked as successful.</span></span> <span data-ttu-id="d44ba-135">Para un `ReadPermission` requisito, el usuario debe ser un propietario o un patrocinador para acceder al recurso solicitado.</span><span class="sxs-lookup"><span data-stu-id="d44ba-135">For a `ReadPermission` requirement, the user must be either an owner or a sponsor to access the requested resource.</span></span> <span data-ttu-id="d44ba-136">En el caso de un `EditPermission` o `DeletePermission` requisito quien debe ser un propietario para acceder al recurso solicitado.</span><span class="sxs-lookup"><span data-stu-id="d44ba-136">In the case of an `EditPermission` or `DeletePermission` requirement, he or she must be an owner to access the requested resource.</span></span>

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a><span data-ttu-id="d44ba-137">Registro del controlador</span><span class="sxs-lookup"><span data-stu-id="d44ba-137">Handler registration</span></span>

<span data-ttu-id="d44ba-138">Los controladores se registran en la colección de servicios durante la configuración.</span><span class="sxs-lookup"><span data-stu-id="d44ba-138">Handlers are registered in the services collection during configuration.</span></span> <span data-ttu-id="d44ba-139">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="d44ba-139">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63-65,72)]

<span data-ttu-id="d44ba-140">Cada controlador se agrega a la colección de servicios mediante la invocación `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();`.</span><span class="sxs-lookup"><span data-stu-id="d44ba-140">Each handler is added to the services collection by invoking `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();`.</span></span>

## <a name="what-should-a-handler-return"></a><span data-ttu-id="d44ba-141">¿Qué debe devolver un controlador?</span><span class="sxs-lookup"><span data-stu-id="d44ba-141">What should a handler return?</span></span>

<span data-ttu-id="d44ba-142">Tenga en cuenta que el `Handle` método en el [ejemplo controlador](#security-authorization-handler-example) no devuelve ningún valor.</span><span class="sxs-lookup"><span data-stu-id="d44ba-142">Note that the `Handle` method in the [handler example](#security-authorization-handler-example) returns no value.</span></span> <span data-ttu-id="d44ba-143">¿Cómo es un estado de éxito o error indicado?</span><span class="sxs-lookup"><span data-stu-id="d44ba-143">How is a status of either success or failure indicated?</span></span>

* <span data-ttu-id="d44ba-144">Un controlador indica éxito mediante una llamada a `context.Succeed(IAuthorizationRequirement requirement)`, pasando el requisito de que se ha validado correctamente.</span><span class="sxs-lookup"><span data-stu-id="d44ba-144">A handler indicates success by calling `context.Succeed(IAuthorizationRequirement requirement)`, passing the requirement that has been successfully validated.</span></span>

* <span data-ttu-id="d44ba-145">Un controlador no tiene que controlar los errores por lo general, ya que otros controladores para el mismo requisito pueden ser correcto.</span><span class="sxs-lookup"><span data-stu-id="d44ba-145">A handler doesn't need to handle failures generally, as other handlers for the same requirement may succeed.</span></span>

* <span data-ttu-id="d44ba-146">Para garantizar el error, incluso si otros controladores de requisitos se realice correctamente, llame a `context.Fail`.</span><span class="sxs-lookup"><span data-stu-id="d44ba-146">To guarantee failure, even if other requirement handlers succeed, call `context.Fail`.</span></span>

<span data-ttu-id="d44ba-147">Cuando se establece en `false`, [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) propiedad (disponible en ASP.NET Core 1.1 y versiones posterior) provoca un cortocircuito en la ejecución de controladores cuando `context.Fail` se llama.</span><span class="sxs-lookup"><span data-stu-id="d44ba-147">When set to `false`, the [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) property (available in ASP.NET Core 1.1 and later) short-circuits the execution of handlers when `context.Fail` is called.</span></span> <span data-ttu-id="d44ba-148">`InvokeHandlersAfterFailure` el valor predeterminado es `true`, en cuyo caso se llama a todos los controladores.</span><span class="sxs-lookup"><span data-stu-id="d44ba-148">`InvokeHandlersAfterFailure` defaults to `true`, in which case all handlers are called.</span></span> <span data-ttu-id="d44ba-149">Esto permite que los requisitos producir efectos secundarios, como el registro, que siempre se produce incluso si `context.Fail` se ha llamado en otro controlador.</span><span class="sxs-lookup"><span data-stu-id="d44ba-149">This allows requirements to produce side effects, such as logging, which always take place even if `context.Fail` has been called in another handler.</span></span>

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a><span data-ttu-id="d44ba-150">¿Por qué querría varios controladores para un requisito?</span><span class="sxs-lookup"><span data-stu-id="d44ba-150">Why would I want multiple handlers for a requirement?</span></span>

<span data-ttu-id="d44ba-151">En los casos donde desee que la evaluación en un **o** base, implementar varios controladores para un requisito único.</span><span class="sxs-lookup"><span data-stu-id="d44ba-151">In cases where you want evaluation to be on an **OR** basis, implement multiple handlers for a single requirement.</span></span> <span data-ttu-id="d44ba-152">Por ejemplo, Microsoft tiene puertas que abre sólo con las tarjetas de clave.</span><span class="sxs-lookup"><span data-stu-id="d44ba-152">For example, Microsoft has doors which only open with key cards.</span></span> <span data-ttu-id="d44ba-153">Si deja la tarjeta de claves en casa, la recepcionista imprime un adhesivo temporal y abre la puerta.</span><span class="sxs-lookup"><span data-stu-id="d44ba-153">If you leave your key card at home, the receptionist prints a temporary sticker and opens the door for you.</span></span> <span data-ttu-id="d44ba-154">En este escenario, tendría un requisito único, *BuildingEntry*, pero varios controladores, cada uno de ellos examinando un requisito único.</span><span class="sxs-lookup"><span data-stu-id="d44ba-154">In this scenario, you'd have a single requirement, *BuildingEntry*, but multiple handlers, each one examining a single requirement.</span></span>

<span data-ttu-id="d44ba-155">*BuildingEntryRequirement.cs*</span><span class="sxs-lookup"><span data-stu-id="d44ba-155">*BuildingEntryRequirement.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

<span data-ttu-id="d44ba-156">*BadgeEntryHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="d44ba-156">*BadgeEntryHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

<span data-ttu-id="d44ba-157">*TemporaryStickerHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="d44ba-157">*TemporaryStickerHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

<span data-ttu-id="d44ba-158">Asegúrese de que ambos controladores estén [registrado](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span><span class="sxs-lookup"><span data-stu-id="d44ba-158">Ensure that both handlers are [registered](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span></span> <span data-ttu-id="d44ba-159">Si cualquier controlador se ejecuta correctamente cuando una directiva se evalúa como el `BuildingEntryRequirement`, la evaluación de directivas se realiza correctamente.</span><span class="sxs-lookup"><span data-stu-id="d44ba-159">If either handler succeeds when a policy evaluates the `BuildingEntryRequirement`, the policy evaluation succeeds.</span></span>

## <a name="using-a-func-to-fulfill-a-policy"></a><span data-ttu-id="d44ba-160">Uso de func para satisfacer una directiva</span><span class="sxs-lookup"><span data-stu-id="d44ba-160">Using a func to fulfill a policy</span></span>

<span data-ttu-id="d44ba-161">Puede haber situaciones en que cumplir una directiva es sencilla expresar en código.</span><span class="sxs-lookup"><span data-stu-id="d44ba-161">There may be situations in which fulfilling a policy is simple to express in code.</span></span> <span data-ttu-id="d44ba-162">Es posible proporcionar un `Func<AuthorizationHandlerContext, bool>` al configurar la directiva con el `RequireAssertion` el generador de directiva.</span><span class="sxs-lookup"><span data-stu-id="d44ba-162">It's possible to supply a `Func<AuthorizationHandlerContext, bool>` when configuring your policy with the `RequireAssertion` policy builder.</span></span>

<span data-ttu-id="d44ba-163">Por ejemplo, el anterior `BadgeEntryHandler` podría volver a escribirse como sigue:</span><span class="sxs-lookup"><span data-stu-id="d44ba-163">For example, the previous `BadgeEntryHandler` could be rewritten as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=52-53,57-63)]

## <a name="accessing-mvc-request-context-in-handlers"></a><span data-ttu-id="d44ba-164">Acceso al contexto de solicitud MVC en controladores</span><span class="sxs-lookup"><span data-stu-id="d44ba-164">Accessing MVC request context in handlers</span></span>

<span data-ttu-id="d44ba-165">El `HandleRequirementAsync` método se implementa en un controlador de autorización tiene dos parámetros: un `AuthorizationHandlerContext` y `TRequirement` está controlando.</span><span class="sxs-lookup"><span data-stu-id="d44ba-165">The `HandleRequirementAsync` method you implement in an authorization handler has two parameters: an `AuthorizationHandlerContext` and the `TRequirement` you are handling.</span></span> <span data-ttu-id="d44ba-166">Marcos de trabajo como MVC o Jabbr son gratuitas agregar cualquier objeto a la `Resource` propiedad en el `AuthorizationHandlerContext` para pasar información adicional.</span><span class="sxs-lookup"><span data-stu-id="d44ba-166">Frameworks such as MVC or Jabbr are free to add any object to the `Resource` property on the `AuthorizationHandlerContext` to pass extra information.</span></span>

<span data-ttu-id="d44ba-167">Por ejemplo, MVC pasa una instancia de [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) en el `Resource` propiedad.</span><span class="sxs-lookup"><span data-stu-id="d44ba-167">For example, MVC passes an instance of [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) in the `Resource` property.</span></span> <span data-ttu-id="d44ba-168">Esta propiedad proporciona acceso a `HttpContext`, `RouteData`y todo lo contrario, proporcionada por MVC y páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="d44ba-168">This property provides access to `HttpContext`, `RouteData`, and everything else provided by MVC and Razor Pages.</span></span>

<span data-ttu-id="d44ba-169">El uso de la `Resource` propiedad es el marco específico.</span><span class="sxs-lookup"><span data-stu-id="d44ba-169">The use of the `Resource` property is framework specific.</span></span> <span data-ttu-id="d44ba-170">Uso de la información la `Resource` propiedad limita sus directivas de autorización para marcos de trabajo determinados.</span><span class="sxs-lookup"><span data-stu-id="d44ba-170">Using information in the `Resource` property limits your authorization policies to particular frameworks.</span></span> <span data-ttu-id="d44ba-171">Primero debe convertir el `Resource` propiedad mediante la `as` palabra clave y, a continuación, confirme la conversión se realizará correctamente para asegurarse de que no se bloquee el código con un `InvalidCastException` cuando se ejecuta en otros marcos de trabajo:</span><span class="sxs-lookup"><span data-stu-id="d44ba-171">You should cast the `Resource` property using the `as` keyword, and then confirm the cast has succeed to ensure your code doesn't crash with an `InvalidCastException` when run on other frameworks:</span></span>

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```
