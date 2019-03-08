---
title: Autorización basada en directivas en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo crear y usar controladores de la directiva de autorización para exigir los requisitos de autorización en una aplicación ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 11/21/2017
uid: security/authorization/policies
ms.openlocfilehash: c2bc626b2dd341dda878a151def6b405884357d7
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665407"
---
# <a name="policy-based-authorization-in-aspnet-core"></a><span data-ttu-id="67554-103">Autorización basada en directivas en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="67554-103">Policy-based authorization in ASP.NET Core</span></span>

<span data-ttu-id="67554-104">Interiormente, [autorización basada en roles](xref:security/authorization/roles) y [autorización basada en notificaciones](xref:security/authorization/claims) usan un requisito, un controlador de requisito y una directiva configurada previamente.</span><span class="sxs-lookup"><span data-stu-id="67554-104">Underneath the covers, [role-based authorization](xref:security/authorization/roles) and [claims-based authorization](xref:security/authorization/claims) use a requirement, a requirement handler, and a pre-configured policy.</span></span> <span data-ttu-id="67554-105">Estos bloques de creación se admite la expresión de evaluaciones de autorización en el código.</span><span class="sxs-lookup"><span data-stu-id="67554-105">These building blocks support the expression of authorization evaluations in code.</span></span> <span data-ttu-id="67554-106">El resultado es una estructura de autorización completas, reutilizables y apta para las pruebas.</span><span class="sxs-lookup"><span data-stu-id="67554-106">The result is a richer, reusable, testable authorization structure.</span></span>

<span data-ttu-id="67554-107">Una directiva de autorización consta de uno o más requisitos.</span><span class="sxs-lookup"><span data-stu-id="67554-107">An authorization policy consists of one or more requirements.</span></span> <span data-ttu-id="67554-108">Está registrado como parte de la configuración del servicio de autorización, en el `Startup.ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="67554-108">It's registered as part of the authorization service configuration, in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63,72)]

<span data-ttu-id="67554-109">En el ejemplo anterior, se crea una directiva de "AtLeast21".</span><span class="sxs-lookup"><span data-stu-id="67554-109">In the preceding example, an "AtLeast21" policy is created.</span></span> <span data-ttu-id="67554-110">Tiene un requisito único&mdash;de una antigüedad mínima, que se suministra como parámetro al requisito.</span><span class="sxs-lookup"><span data-stu-id="67554-110">It has a single requirement&mdash;that of a minimum age, which is supplied as a parameter to the requirement.</span></span>

<span data-ttu-id="67554-111">Las directivas se aplican mediante el uso de la `[Authorize]` atributo con el nombre de la directiva.</span><span class="sxs-lookup"><span data-stu-id="67554-111">Policies are applied by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="67554-112">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="67554-112">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="requirements"></a><span data-ttu-id="67554-113">Requisitos</span><span class="sxs-lookup"><span data-stu-id="67554-113">Requirements</span></span>

<span data-ttu-id="67554-114">Un requisito de autorización es una colección de parámetros de datos que puede usar una directiva para evaluar la entidad de seguridad del usuario actual.</span><span class="sxs-lookup"><span data-stu-id="67554-114">An authorization requirement is a collection of data parameters that a policy can use to evaluate the current user principal.</span></span> <span data-ttu-id="67554-115">En nuestra directiva de "AtLeast21", el requisito es un único parámetro&mdash;la antigüedad mínima.</span><span class="sxs-lookup"><span data-stu-id="67554-115">In our "AtLeast21" policy, the requirement is a single parameter&mdash;the minimum age.</span></span> <span data-ttu-id="67554-116">Implementa un requisito [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), que es una interfaz de marcador vacío.</span><span class="sxs-lookup"><span data-stu-id="67554-116">A requirement implements [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), which is an empty marker interface.</span></span> <span data-ttu-id="67554-117">Un requisito de antigüedad mínima con parámetros podría implementarse como sigue:</span><span class="sxs-lookup"><span data-stu-id="67554-117">A parameterized minimum age requirement could be implemented as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

<span data-ttu-id="67554-118">Si una directiva de autorización contiene varios requisitos de autorización, deben pasar todos los requisitos para la evaluación de directivas se realice correctamente.</span><span class="sxs-lookup"><span data-stu-id="67554-118">If an authorization policy contains multiple authorization requirements, all requirements must pass in order for the policy evaluation to succeed.</span></span> <span data-ttu-id="67554-119">En otras palabras, se tratan varios requisitos de autorización que se agrega a una sola directiva de autorización en un **AND** base.</span><span class="sxs-lookup"><span data-stu-id="67554-119">In other words, multiple authorization requirements added to a single authorization policy are treated on an **AND** basis.</span></span>

> [!NOTE]
> <span data-ttu-id="67554-120">Un requisito no debe tener datos o propiedades.</span><span class="sxs-lookup"><span data-stu-id="67554-120">A requirement doesn't need to have data or properties.</span></span>

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a><span data-ttu-id="67554-121">Controladores de autorización</span><span class="sxs-lookup"><span data-stu-id="67554-121">Authorization handlers</span></span>

<span data-ttu-id="67554-122">Un controlador de autorización es responsable de la evaluación de propiedades de un requisito.</span><span class="sxs-lookup"><span data-stu-id="67554-122">An authorization handler is responsible for the evaluation of a requirement's properties.</span></span> <span data-ttu-id="67554-123">El controlador de autorización evalúa los requisitos en relación con proporcionado [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) para determinar si se permite el acceso.</span><span class="sxs-lookup"><span data-stu-id="67554-123">The authorization handler evaluates the requirements against a provided [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) to determine if access is allowed.</span></span>

<span data-ttu-id="67554-124">Puede tener un requisito [varios controladores](#security-authorization-policies-based-multiple-handlers).</span><span class="sxs-lookup"><span data-stu-id="67554-124">A requirement can have [multiple handlers](#security-authorization-policies-based-multiple-handlers).</span></span> <span data-ttu-id="67554-125">Puede heredar un controlador [AuthorizationHandler\<TRequirement >](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), donde `TRequirement` es el requisito para que lo administre.</span><span class="sxs-lookup"><span data-stu-id="67554-125">A handler may inherit [AuthorizationHandler\<TRequirement>](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), where `TRequirement` is the requirement to be handled.</span></span> <span data-ttu-id="67554-126">Como alternativa, puede implementar un controlador [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) para administrar más de un tipo de requisito.</span><span class="sxs-lookup"><span data-stu-id="67554-126">Alternatively, a handler may implement [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) to handle more than one type of requirement.</span></span>

### <a name="use-a-handler-for-one-requirement"></a><span data-ttu-id="67554-127">Usar un controlador para uno de los requisitos</span><span class="sxs-lookup"><span data-stu-id="67554-127">Use a handler for one requirement</span></span>

<a name="security-authorization-handler-example"></a>

<span data-ttu-id="67554-128">El siguiente es un ejemplo de una relación uno a uno en el que un controlador de antigüedad mínima utiliza un requisito único:</span><span class="sxs-lookup"><span data-stu-id="67554-128">The following is an example of a one-to-one relationship in which a minimum age handler utilizes a single requirement:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

<span data-ttu-id="67554-129">El código anterior determina si la entidad de usuario actual tiene una fecha de nacimiento de notificación de que se ha emitido por un emisor conocido y de confianza.</span><span class="sxs-lookup"><span data-stu-id="67554-129">The preceding code determines if the current user principal has a date of birth claim which has been issued by a known and trusted Issuer.</span></span> <span data-ttu-id="67554-130">No se puede realizar la autorización cuando falta la notificación, en cuyo caso se devuelve una tarea completada.</span><span class="sxs-lookup"><span data-stu-id="67554-130">Authorization can't occur when the claim is missing, in which case a completed task is returned.</span></span> <span data-ttu-id="67554-131">Cuando hay una notificación, se calcula la edad del usuario.</span><span class="sxs-lookup"><span data-stu-id="67554-131">When a claim is present, the user's age is calculated.</span></span> <span data-ttu-id="67554-132">Si el usuario cumple la antigüedad mínima definida por el requisito, la autorización se considere correcta.</span><span class="sxs-lookup"><span data-stu-id="67554-132">If the user meets the minimum age defined by the requirement, authorization is deemed successful.</span></span> <span data-ttu-id="67554-133">Cuando se realiza correctamente, la autorización `context.Succeed` se invoca con el requisito satisfecho como su único parámetro.</span><span class="sxs-lookup"><span data-stu-id="67554-133">When authorization is successful, `context.Succeed` is invoked with the satisfied requirement as its sole parameter.</span></span>

### <a name="use-a-handler-for-multiple-requirements"></a><span data-ttu-id="67554-134">Usar un controlador para varios requisitos</span><span class="sxs-lookup"><span data-stu-id="67554-134">Use a handler for multiple requirements</span></span>

<span data-ttu-id="67554-135">El siguiente es un ejemplo de una relación uno a varios en el que un controlador de permiso puede controlar los tres tipos diferentes de los requisitos:</span><span class="sxs-lookup"><span data-stu-id="67554-135">The following is an example of a one-to-many relationship in which a permission handler can handle three different types of requirements:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/PermissionHandler.cs?name=snippet_PermissionHandlerClass)]

<span data-ttu-id="67554-136">El código anterior recorre [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;una propiedad que contiene los requisitos no marcada como correcta.</span><span class="sxs-lookup"><span data-stu-id="67554-136">The preceding code traverses [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;a property containing requirements not marked as successful.</span></span> <span data-ttu-id="67554-137">Para un `ReadPermission` requisito, el usuario debe ser un propietario o un patrocinador para acceder al recurso solicitado.</span><span class="sxs-lookup"><span data-stu-id="67554-137">For a `ReadPermission` requirement, the user must be either an owner or a sponsor to access the requested resource.</span></span> <span data-ttu-id="67554-138">En el caso de un `EditPermission` o `DeletePermission` requisito quien debe ser un propietario para acceder al recurso solicitado.</span><span class="sxs-lookup"><span data-stu-id="67554-138">In the case of an `EditPermission` or `DeletePermission` requirement, he or she must be an owner to access the requested resource.</span></span>

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a><span data-ttu-id="67554-139">Registro del controlador</span><span class="sxs-lookup"><span data-stu-id="67554-139">Handler registration</span></span>

<span data-ttu-id="67554-140">Los controladores se registran en la colección de servicios durante la configuración.</span><span class="sxs-lookup"><span data-stu-id="67554-140">Handlers are registered in the services collection during configuration.</span></span> <span data-ttu-id="67554-141">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="67554-141">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63-65,72)]

<span data-ttu-id="67554-142">El código anterior registra `MinimumAgeHandler` como un singleton invocando `services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();`.</span><span class="sxs-lookup"><span data-stu-id="67554-142">The preceding code registers `MinimumAgeHandler` as a singleton by invoking `services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();`.</span></span> <span data-ttu-id="67554-143">Los controladores pueden registrarse mediante cualquiera de los integrados [duraciones de servicio](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="67554-143">Handlers can be registered using any of the built-in [service lifetimes](xref:fundamentals/dependency-injection#service-lifetimes).</span></span>

## <a name="what-should-a-handler-return"></a><span data-ttu-id="67554-144">¿Qué debe devolver un controlador?</span><span class="sxs-lookup"><span data-stu-id="67554-144">What should a handler return?</span></span>

<span data-ttu-id="67554-145">Tenga en cuenta que el `Handle` método en el [ejemplo controlador](#security-authorization-handler-example) no devuelve ningún valor.</span><span class="sxs-lookup"><span data-stu-id="67554-145">Note that the `Handle` method in the [handler example](#security-authorization-handler-example) returns no value.</span></span> <span data-ttu-id="67554-146">¿Cómo es un estado de éxito o error indicado?</span><span class="sxs-lookup"><span data-stu-id="67554-146">How is a status of either success or failure indicated?</span></span>

* <span data-ttu-id="67554-147">Un controlador indica éxito mediante una llamada a `context.Succeed(IAuthorizationRequirement requirement)`, pasando el requisito de que se ha validado correctamente.</span><span class="sxs-lookup"><span data-stu-id="67554-147">A handler indicates success by calling `context.Succeed(IAuthorizationRequirement requirement)`, passing the requirement that has been successfully validated.</span></span>

* <span data-ttu-id="67554-148">Un controlador no tiene que controlar los errores por lo general, ya que otros controladores para el mismo requisito pueden ser correcto.</span><span class="sxs-lookup"><span data-stu-id="67554-148">A handler doesn't need to handle failures generally, as other handlers for the same requirement may succeed.</span></span>

* <span data-ttu-id="67554-149">Para garantizar el error, incluso si otros controladores de requisitos se realice correctamente, llame a `context.Fail`.</span><span class="sxs-lookup"><span data-stu-id="67554-149">To guarantee failure, even if other requirement handlers succeed, call `context.Fail`.</span></span>

<span data-ttu-id="67554-150">Cuando se establece en `false`, [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) propiedad (disponible en ASP.NET Core 1.1 y versiones posterior) provoca un cortocircuito en la ejecución de controladores cuando `context.Fail` se llama.</span><span class="sxs-lookup"><span data-stu-id="67554-150">When set to `false`, the [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) property (available in ASP.NET Core 1.1 and later) short-circuits the execution of handlers when `context.Fail` is called.</span></span> <span data-ttu-id="67554-151">`InvokeHandlersAfterFailure` el valor predeterminado es `true`, en cuyo caso se llama a todos los controladores.</span><span class="sxs-lookup"><span data-stu-id="67554-151">`InvokeHandlersAfterFailure` defaults to `true`, in which case all handlers are called.</span></span> <span data-ttu-id="67554-152">Esto permite que los requisitos producir efectos secundarios, como el registro, que siempre se produce incluso si `context.Fail` se ha llamado en otro controlador.</span><span class="sxs-lookup"><span data-stu-id="67554-152">This allows requirements to produce side effects, such as logging, which always take place even if `context.Fail` has been called in another handler.</span></span>

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a><span data-ttu-id="67554-153">¿Por qué querría varios controladores para un requisito?</span><span class="sxs-lookup"><span data-stu-id="67554-153">Why would I want multiple handlers for a requirement?</span></span>

<span data-ttu-id="67554-154">En los casos donde desee que la evaluación en un **o** base, implementar varios controladores para un requisito único.</span><span class="sxs-lookup"><span data-stu-id="67554-154">In cases where you want evaluation to be on an **OR** basis, implement multiple handlers for a single requirement.</span></span> <span data-ttu-id="67554-155">Por ejemplo, Microsoft tiene puertas que abre sólo con las tarjetas de clave.</span><span class="sxs-lookup"><span data-stu-id="67554-155">For example, Microsoft has doors which only open with key cards.</span></span> <span data-ttu-id="67554-156">Si deja la tarjeta de claves en casa, la recepcionista imprime un adhesivo temporal y abre la puerta.</span><span class="sxs-lookup"><span data-stu-id="67554-156">If you leave your key card at home, the receptionist prints a temporary sticker and opens the door for you.</span></span> <span data-ttu-id="67554-157">En este escenario, tendría un requisito único, *BuildingEntry*, pero varios controladores, cada uno de ellos examinando un requisito único.</span><span class="sxs-lookup"><span data-stu-id="67554-157">In this scenario, you'd have a single requirement, *BuildingEntry*, but multiple handlers, each one examining a single requirement.</span></span>

<span data-ttu-id="67554-158">*BuildingEntryRequirement.cs*</span><span class="sxs-lookup"><span data-stu-id="67554-158">*BuildingEntryRequirement.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

<span data-ttu-id="67554-159">*BadgeEntryHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="67554-159">*BadgeEntryHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

<span data-ttu-id="67554-160">*TemporaryStickerHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="67554-160">*TemporaryStickerHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

<span data-ttu-id="67554-161">Asegúrese de que ambos controladores estén [registrado](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span><span class="sxs-lookup"><span data-stu-id="67554-161">Ensure that both handlers are [registered](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span></span> <span data-ttu-id="67554-162">Si cualquier controlador se ejecuta correctamente cuando una directiva se evalúa como el `BuildingEntryRequirement`, la evaluación de directivas se realiza correctamente.</span><span class="sxs-lookup"><span data-stu-id="67554-162">If either handler succeeds when a policy evaluates the `BuildingEntryRequirement`, the policy evaluation succeeds.</span></span>

## <a name="using-a-func-to-fulfill-a-policy"></a><span data-ttu-id="67554-163">Uso de func para satisfacer una directiva</span><span class="sxs-lookup"><span data-stu-id="67554-163">Using a func to fulfill a policy</span></span>

<span data-ttu-id="67554-164">Puede haber situaciones en que cumplir una directiva es sencilla expresar en código.</span><span class="sxs-lookup"><span data-stu-id="67554-164">There may be situations in which fulfilling a policy is simple to express in code.</span></span> <span data-ttu-id="67554-165">Es posible proporcionar un `Func<AuthorizationHandlerContext, bool>` al configurar la directiva con el `RequireAssertion` el generador de directiva.</span><span class="sxs-lookup"><span data-stu-id="67554-165">It's possible to supply a `Func<AuthorizationHandlerContext, bool>` when configuring your policy with the `RequireAssertion` policy builder.</span></span>

<span data-ttu-id="67554-166">Por ejemplo, el anterior `BadgeEntryHandler` podría volver a escribirse como sigue:</span><span class="sxs-lookup"><span data-stu-id="67554-166">For example, the previous `BadgeEntryHandler` could be rewritten as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=52-53,57-63)]

## <a name="accessing-mvc-request-context-in-handlers"></a><span data-ttu-id="67554-167">Acceso al contexto de solicitud MVC en controladores</span><span class="sxs-lookup"><span data-stu-id="67554-167">Accessing MVC request context in handlers</span></span>

<span data-ttu-id="67554-168">El `HandleRequirementAsync` método se implementa en un controlador de autorización tiene dos parámetros: un `AuthorizationHandlerContext` y `TRequirement` está controlando.</span><span class="sxs-lookup"><span data-stu-id="67554-168">The `HandleRequirementAsync` method you implement in an authorization handler has two parameters: an `AuthorizationHandlerContext` and the `TRequirement` you are handling.</span></span> <span data-ttu-id="67554-169">Marcos de trabajo como MVC o Jabbr son gratuitas agregar cualquier objeto a la `Resource` propiedad en el `AuthorizationHandlerContext` para pasar información adicional.</span><span class="sxs-lookup"><span data-stu-id="67554-169">Frameworks such as MVC or Jabbr are free to add any object to the `Resource` property on the `AuthorizationHandlerContext` to pass extra information.</span></span>

<span data-ttu-id="67554-170">Por ejemplo, MVC pasa una instancia de [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) en el `Resource` propiedad.</span><span class="sxs-lookup"><span data-stu-id="67554-170">For example, MVC passes an instance of [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) in the `Resource` property.</span></span> <span data-ttu-id="67554-171">Esta propiedad proporciona acceso a `HttpContext`, `RouteData`y todo lo contrario, proporcionada por MVC y páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="67554-171">This property provides access to `HttpContext`, `RouteData`, and everything else provided by MVC and Razor Pages.</span></span>

<span data-ttu-id="67554-172">El uso de la `Resource` propiedad es el marco específico.</span><span class="sxs-lookup"><span data-stu-id="67554-172">The use of the `Resource` property is framework specific.</span></span> <span data-ttu-id="67554-173">Uso de la información la `Resource` propiedad limita sus directivas de autorización para marcos de trabajo determinados.</span><span class="sxs-lookup"><span data-stu-id="67554-173">Using information in the `Resource` property limits your authorization policies to particular frameworks.</span></span> <span data-ttu-id="67554-174">Primero debe convertir el `Resource` propiedad mediante la `is` palabra clave y, a continuación, confirme la conversión se realizó correctamente para asegurarse de que no se bloquee el código con un `InvalidCastException` cuando se ejecuta en otros marcos de trabajo:</span><span class="sxs-lookup"><span data-stu-id="67554-174">You should cast the `Resource` property using the `is` keyword, and then confirm the cast has succeeded to ensure your code doesn't crash with an `InvalidCastException` when run on other frameworks:</span></span>

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```
