---
title: Autorización basada en directivas en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo crear y usar controladores de la directiva de autorización para exigir los requisitos de autorización en una aplicación ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 04/05/2019
uid: security/authorization/policies
ms.openlocfilehash: 67337c847ba71df3fe61250996ec944632ad5d57
ms.sourcegitcommit: 1bb3f3f1905b4e7d4ca1b314f2ce6ee5dd8be75f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/11/2019
ms.locfileid: "66837355"
---
# <a name="policy-based-authorization-in-aspnet-core"></a><span data-ttu-id="0bb9c-103">Autorización basada en directivas en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0bb9c-103">Policy-based authorization in ASP.NET Core</span></span>

<span data-ttu-id="0bb9c-104">Interiormente, [autorización basada en roles](xref:security/authorization/roles) y [autorización basada en notificaciones](xref:security/authorization/claims) usan un requisito, un controlador de requisito y una directiva configurada previamente.</span><span class="sxs-lookup"><span data-stu-id="0bb9c-104">Underneath the covers, [role-based authorization](xref:security/authorization/roles) and [claims-based authorization](xref:security/authorization/claims) use a requirement, a requirement handler, and a pre-configured policy.</span></span> <span data-ttu-id="0bb9c-105">Estos bloques de creación se admite la expresión de evaluaciones de autorización en el código.</span><span class="sxs-lookup"><span data-stu-id="0bb9c-105">These building blocks support the expression of authorization evaluations in code.</span></span> <span data-ttu-id="0bb9c-106">El resultado es una estructura de autorización completas, reutilizables y apta para las pruebas.</span><span class="sxs-lookup"><span data-stu-id="0bb9c-106">The result is a richer, reusable, testable authorization structure.</span></span>

<span data-ttu-id="0bb9c-107">Una directiva de autorización consta de uno o más requisitos.</span><span class="sxs-lookup"><span data-stu-id="0bb9c-107">An authorization policy consists of one or more requirements.</span></span> <span data-ttu-id="0bb9c-108">Está registrado como parte de la configuración del servicio de autorización, en el `Startup.ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="0bb9c-108">It's registered as part of the authorization service configuration, in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=32-33,48-53,61,66)]

<span data-ttu-id="0bb9c-109">En el ejemplo anterior, se crea una directiva de "AtLeast21".</span><span class="sxs-lookup"><span data-stu-id="0bb9c-109">In the preceding example, an "AtLeast21" policy is created.</span></span> <span data-ttu-id="0bb9c-110">Tiene un requisito único&mdash;de una antigüedad mínima, que se suministra como parámetro al requisito.</span><span class="sxs-lookup"><span data-stu-id="0bb9c-110">It has a single requirement&mdash;that of a minimum age, which is supplied as a parameter to the requirement.</span></span>

## <a name="iauthorizationservice"></a><span data-ttu-id="0bb9c-111">IAuthorizationService</span><span class="sxs-lookup"><span data-stu-id="0bb9c-111">IAuthorizationService</span></span> 

<span data-ttu-id="0bb9c-112">El servicio principal que determina si la autorización sea correcta es <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService>:</span><span class="sxs-lookup"><span data-stu-id="0bb9c-112">The primary service that determines if authorization is successful is <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService>:</span></span>

[!code-csharp[](policies/samples/stubs/copy_of_IAuthorizationService.cs?highlight=24-25,48-49&name=snippet)]

<span data-ttu-id="0bb9c-113">El código anterior resalta los dos métodos de la [IAuthorizationService](https://github.com/aspnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationService.cs).</span><span class="sxs-lookup"><span data-stu-id="0bb9c-113">The preceding code highlights the two methods of the [IAuthorizationService](https://github.com/aspnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationService.cs).</span></span>

<span data-ttu-id="0bb9c-114"><xref:Microsoft.AspNetCore.Authorization.IAuthorizationRequirement> es un servicio de marcador con ningún método y el mecanismo para realizar el seguimiento de si la autorización sea correcta.</span><span class="sxs-lookup"><span data-stu-id="0bb9c-114"><xref:Microsoft.AspNetCore.Authorization.IAuthorizationRequirement> is a marker service with no methods, and the mechanism for tracking whether authorization is successful.</span></span>

<span data-ttu-id="0bb9c-115">Cada <xref:Microsoft.AspNetCore.Authorization.IAuthorizationHandler> es responsable de comprobar si se cumplen los requisitos:</span><span class="sxs-lookup"><span data-stu-id="0bb9c-115">Each <xref:Microsoft.AspNetCore.Authorization.IAuthorizationHandler> is responsible for checking if requirements are met:</span></span>
<!--The following code is a copy/paste from 
https://github.com/aspnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationHandler.cs -->

```csharp
/// <summary>
/// Classes implementing this interface are able to make a decision if authorization
/// is allowed.
/// </summary>
public interface IAuthorizationHandler
{
    /// <summary>
    /// Makes a decision if authorization is allowed.
    /// </summary>
    /// <param name="context">The authorization information.</param>
    Task HandleAsync(AuthorizationHandlerContext context);
}
```

<span data-ttu-id="0bb9c-116">La <xref:Microsoft.AspNetCore.Authorization.AuthorizationHandlerContext> clase es lo que el controlador que se usa para marcar si se cumplen los requisitos:</span><span class="sxs-lookup"><span data-stu-id="0bb9c-116">The <xref:Microsoft.AspNetCore.Authorization.AuthorizationHandlerContext> class is what the handler uses to mark whether requirements have been met:</span></span>

```csharp
 context.Succeed(requirement)
```

<span data-ttu-id="0bb9c-117">El código siguiente muestra la simplificada (y anotado con comentarios) predeterminado de la implementación del servicio de autorización:</span><span class="sxs-lookup"><span data-stu-id="0bb9c-117">The following code shows the simplified (and annotated with comments) default implementation of the authorization service:</span></span>

```csharp
public async Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user, 
             object resource, IEnumerable<IAuthorizationRequirement> requirements)
{
    // Create a tracking context from the authorization inputs.
    var authContext = _contextFactory.CreateContext(requirements, user, resource);

    // By default this returns an IEnumerable<IAuthorizationHandlers> from DI.
    var handlers = await _handlers.GetHandlersAsync(authContext);

    // Invoke all handlers.
    foreach (var handler in handlers)
    {
        await handler.HandleAsync(authContext);
    }

    // Check the context, by default success is when all requirements have been met.
    return _evaluator.Evaluate(authContext);
}
```

<span data-ttu-id="0bb9c-118">El código siguiente muestra una típica `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="0bb9c-118">The following code shows a typical `ConfigureServices`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add all of your handlers to DI.
    services.AddSingleton<IAuthorizationHandler, MyHandler1>();
    // MyHandler2, ...

    services.AddSingleton<IAuthorizationHandler, MyHandlerN>();

    // Configure your policies
    services.AddAuthorization(options =>
          options.AddPolicy("Something",
          policy => policy.RequireClaim("Permission", "CanViewPage", "CanViewAnything")));


    services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
}
```

<span data-ttu-id="0bb9c-119">Use <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService> o `[Authorize(Policy = "Something"]` para la autorización.</span><span class="sxs-lookup"><span data-stu-id="0bb9c-119">Use <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService> or `[Authorize(Policy = "Something"]` for authorization.</span></span>

## <a name="applying-policies-to-mvc-controllers"></a><span data-ttu-id="0bb9c-120">Aplicar directivas a los controladores MVC</span><span class="sxs-lookup"><span data-stu-id="0bb9c-120">Applying policies to MVC controllers</span></span>

<span data-ttu-id="0bb9c-121">Si está usando las páginas de Razor, consulte [aplicar directivas a las páginas de Razor](#applying-policies-to-razor-pages) en este documento.</span><span class="sxs-lookup"><span data-stu-id="0bb9c-121">If you're using Razor Pages, see [Applying policies to Razor Pages](#applying-policies-to-razor-pages) in this document.</span></span>

<span data-ttu-id="0bb9c-122">Las directivas se aplican a los controladores mediante el uso de la `[Authorize]` atributo con el nombre de la directiva.</span><span class="sxs-lookup"><span data-stu-id="0bb9c-122">Policies are applied to controllers by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="0bb9c-123">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="0bb9c-123">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="applying-policies-to-razor-pages"></a><span data-ttu-id="0bb9c-124">Aplicar directivas a las páginas de Razor</span><span class="sxs-lookup"><span data-stu-id="0bb9c-124">Applying policies to Razor Pages</span></span>

<span data-ttu-id="0bb9c-125">Las directivas se aplican a las páginas de Razor mediante el uso de la `[Authorize]` atributo con el nombre de la directiva.</span><span class="sxs-lookup"><span data-stu-id="0bb9c-125">Policies are applied to Razor Pages by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="0bb9c-126">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="0bb9c-126">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp2/Pages/AlcoholPurchase.cshtml.cs?name=snippet_AlcoholPurchaseModelClass&highlight=4)]

<span data-ttu-id="0bb9c-127">Las directivas pueden aplicarse también a las páginas de Razor mediante el uso de un [convención autorización](xref:security/authorization/razor-pages-authorization).</span><span class="sxs-lookup"><span data-stu-id="0bb9c-127">Policies can also be applied to Razor Pages by using an [authorization convention](xref:security/authorization/razor-pages-authorization).</span></span>

## <a name="requirements"></a><span data-ttu-id="0bb9c-128">Requisitos</span><span class="sxs-lookup"><span data-stu-id="0bb9c-128">Requirements</span></span>

<span data-ttu-id="0bb9c-129">Un requisito de autorización es una colección de parámetros de datos que puede usar una directiva para evaluar la entidad de seguridad del usuario actual.</span><span class="sxs-lookup"><span data-stu-id="0bb9c-129">An authorization requirement is a collection of data parameters that a policy can use to evaluate the current user principal.</span></span> <span data-ttu-id="0bb9c-130">En nuestra directiva de "AtLeast21", el requisito es un único parámetro&mdash;la antigüedad mínima.</span><span class="sxs-lookup"><span data-stu-id="0bb9c-130">In our "AtLeast21" policy, the requirement is a single parameter&mdash;the minimum age.</span></span> <span data-ttu-id="0bb9c-131">Implementa un requisito [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), que es una interfaz de marcador vacío.</span><span class="sxs-lookup"><span data-stu-id="0bb9c-131">A requirement implements [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), which is an empty marker interface.</span></span> <span data-ttu-id="0bb9c-132">Un requisito de antigüedad mínima con parámetros podría implementarse como sigue:</span><span class="sxs-lookup"><span data-stu-id="0bb9c-132">A parameterized minimum age requirement could be implemented as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

<span data-ttu-id="0bb9c-133">Si una directiva de autorización contiene varios requisitos de autorización, deben pasar todos los requisitos para la evaluación de directivas se realice correctamente.</span><span class="sxs-lookup"><span data-stu-id="0bb9c-133">If an authorization policy contains multiple authorization requirements, all requirements must pass in order for the policy evaluation to succeed.</span></span> <span data-ttu-id="0bb9c-134">En otras palabras, se tratan varios requisitos de autorización que se agrega a una sola directiva de autorización en un **AND** base.</span><span class="sxs-lookup"><span data-stu-id="0bb9c-134">In other words, multiple authorization requirements added to a single authorization policy are treated on an **AND** basis.</span></span>

> [!NOTE]
> <span data-ttu-id="0bb9c-135">Un requisito no debe tener datos o propiedades.</span><span class="sxs-lookup"><span data-stu-id="0bb9c-135">A requirement doesn't need to have data or properties.</span></span>

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a><span data-ttu-id="0bb9c-136">Controladores de autorización</span><span class="sxs-lookup"><span data-stu-id="0bb9c-136">Authorization handlers</span></span>

<span data-ttu-id="0bb9c-137">Un controlador de autorización es responsable de la evaluación de propiedades de un requisito.</span><span class="sxs-lookup"><span data-stu-id="0bb9c-137">An authorization handler is responsible for the evaluation of a requirement's properties.</span></span> <span data-ttu-id="0bb9c-138">El controlador de autorización evalúa los requisitos en relación con proporcionado [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) para determinar si se permite el acceso.</span><span class="sxs-lookup"><span data-stu-id="0bb9c-138">The authorization handler evaluates the requirements against a provided [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) to determine if access is allowed.</span></span>

<span data-ttu-id="0bb9c-139">Puede tener un requisito [varios controladores](#security-authorization-policies-based-multiple-handlers).</span><span class="sxs-lookup"><span data-stu-id="0bb9c-139">A requirement can have [multiple handlers](#security-authorization-policies-based-multiple-handlers).</span></span> <span data-ttu-id="0bb9c-140">Puede heredar un controlador [AuthorizationHandler\<TRequirement >](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), donde `TRequirement` es el requisito para que lo administre.</span><span class="sxs-lookup"><span data-stu-id="0bb9c-140">A handler may inherit [AuthorizationHandler\<TRequirement>](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), where `TRequirement` is the requirement to be handled.</span></span> <span data-ttu-id="0bb9c-141">Como alternativa, puede implementar un controlador [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) para administrar más de un tipo de requisito.</span><span class="sxs-lookup"><span data-stu-id="0bb9c-141">Alternatively, a handler may implement [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) to handle more than one type of requirement.</span></span>

### <a name="use-a-handler-for-one-requirement"></a><span data-ttu-id="0bb9c-142">Usar un controlador para uno de los requisitos</span><span class="sxs-lookup"><span data-stu-id="0bb9c-142">Use a handler for one requirement</span></span>

<a name="security-authorization-handler-example"></a>

<span data-ttu-id="0bb9c-143">El siguiente es un ejemplo de una relación uno a uno en el que un controlador de antigüedad mínima utiliza un requisito único:</span><span class="sxs-lookup"><span data-stu-id="0bb9c-143">The following is an example of a one-to-one relationship in which a minimum age handler utilizes a single requirement:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

<span data-ttu-id="0bb9c-144">El código anterior determina si la entidad de usuario actual tiene una fecha de nacimiento de notificación de que se ha emitido por un emisor conocido y de confianza.</span><span class="sxs-lookup"><span data-stu-id="0bb9c-144">The preceding code determines if the current user principal has a date of birth claim which has been issued by a known and trusted Issuer.</span></span> <span data-ttu-id="0bb9c-145">No se puede realizar la autorización cuando falta la notificación, en cuyo caso se devuelve una tarea completada.</span><span class="sxs-lookup"><span data-stu-id="0bb9c-145">Authorization can't occur when the claim is missing, in which case a completed task is returned.</span></span> <span data-ttu-id="0bb9c-146">Cuando hay una notificación, se calcula la edad del usuario.</span><span class="sxs-lookup"><span data-stu-id="0bb9c-146">When a claim is present, the user's age is calculated.</span></span> <span data-ttu-id="0bb9c-147">Si el usuario cumple la antigüedad mínima definida por el requisito, la autorización se considere correcta.</span><span class="sxs-lookup"><span data-stu-id="0bb9c-147">If the user meets the minimum age defined by the requirement, authorization is deemed successful.</span></span> <span data-ttu-id="0bb9c-148">Cuando se realiza correctamente, la autorización `context.Succeed` se invoca con el requisito satisfecho como su único parámetro.</span><span class="sxs-lookup"><span data-stu-id="0bb9c-148">When authorization is successful, `context.Succeed` is invoked with the satisfied requirement as its sole parameter.</span></span>

### <a name="use-a-handler-for-multiple-requirements"></a><span data-ttu-id="0bb9c-149">Usar un controlador para varios requisitos</span><span class="sxs-lookup"><span data-stu-id="0bb9c-149">Use a handler for multiple requirements</span></span>

<span data-ttu-id="0bb9c-150">El siguiente es un ejemplo de una relación uno a varios en el que un controlador de permiso puede controlar los tres tipos diferentes de los requisitos:</span><span class="sxs-lookup"><span data-stu-id="0bb9c-150">The following is an example of a one-to-many relationship in which a permission handler can handle three different types of requirements:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/PermissionHandler.cs?name=snippet_PermissionHandlerClass)]

<span data-ttu-id="0bb9c-151">El código anterior recorre [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;una propiedad que contiene los requisitos no marcada como correcta.</span><span class="sxs-lookup"><span data-stu-id="0bb9c-151">The preceding code traverses [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;a property containing requirements not marked as successful.</span></span> <span data-ttu-id="0bb9c-152">Para un `ReadPermission` requisito, el usuario debe ser un propietario o un patrocinador para acceder al recurso solicitado.</span><span class="sxs-lookup"><span data-stu-id="0bb9c-152">For a `ReadPermission` requirement, the user must be either an owner or a sponsor to access the requested resource.</span></span> <span data-ttu-id="0bb9c-153">En el caso de un `EditPermission` o `DeletePermission` requisito quien debe ser un propietario para acceder al recurso solicitado.</span><span class="sxs-lookup"><span data-stu-id="0bb9c-153">In the case of an `EditPermission` or `DeletePermission` requirement, he or she must be an owner to access the requested resource.</span></span>

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a><span data-ttu-id="0bb9c-154">Registro del controlador</span><span class="sxs-lookup"><span data-stu-id="0bb9c-154">Handler registration</span></span>

<span data-ttu-id="0bb9c-155">Los controladores se registran en la colección de servicios durante la configuración.</span><span class="sxs-lookup"><span data-stu-id="0bb9c-155">Handlers are registered in the services collection during configuration.</span></span> <span data-ttu-id="0bb9c-156">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="0bb9c-156">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=32-33,48-53,61,62-63,66)]

<span data-ttu-id="0bb9c-157">El código anterior registra `MinimumAgeHandler` como un singleton invocando `services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();`.</span><span class="sxs-lookup"><span data-stu-id="0bb9c-157">The preceding code registers `MinimumAgeHandler` as a singleton by invoking `services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();`.</span></span> <span data-ttu-id="0bb9c-158">Los controladores pueden registrarse mediante cualquiera de los integrados [duraciones de servicio](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="0bb9c-158">Handlers can be registered using any of the built-in [service lifetimes](xref:fundamentals/dependency-injection#service-lifetimes).</span></span>

## <a name="what-should-a-handler-return"></a><span data-ttu-id="0bb9c-159">¿Qué debe devolver un controlador?</span><span class="sxs-lookup"><span data-stu-id="0bb9c-159">What should a handler return?</span></span>

<span data-ttu-id="0bb9c-160">Tenga en cuenta que el `Handle` método en el [ejemplo controlador](#security-authorization-handler-example) no devuelve ningún valor.</span><span class="sxs-lookup"><span data-stu-id="0bb9c-160">Note that the `Handle` method in the [handler example](#security-authorization-handler-example) returns no value.</span></span> <span data-ttu-id="0bb9c-161">¿Cómo es un estado de éxito o error indicado?</span><span class="sxs-lookup"><span data-stu-id="0bb9c-161">How is a status of either success or failure indicated?</span></span>

* <span data-ttu-id="0bb9c-162">Un controlador indica éxito mediante una llamada a `context.Succeed(IAuthorizationRequirement requirement)`, pasando el requisito de que se ha validado correctamente.</span><span class="sxs-lookup"><span data-stu-id="0bb9c-162">A handler indicates success by calling `context.Succeed(IAuthorizationRequirement requirement)`, passing the requirement that has been successfully validated.</span></span>

* <span data-ttu-id="0bb9c-163">Un controlador no tiene que controlar los errores por lo general, ya que otros controladores para el mismo requisito pueden ser correcto.</span><span class="sxs-lookup"><span data-stu-id="0bb9c-163">A handler doesn't need to handle failures generally, as other handlers for the same requirement may succeed.</span></span>

* <span data-ttu-id="0bb9c-164">Para garantizar el error, incluso si otros controladores de requisitos se realice correctamente, llame a `context.Fail`.</span><span class="sxs-lookup"><span data-stu-id="0bb9c-164">To guarantee failure, even if other requirement handlers succeed, call `context.Fail`.</span></span>

<span data-ttu-id="0bb9c-165">Si llama un controlador `context.Succeed` o `context.Fail`, todavía se llama a todos los otros controladores.</span><span class="sxs-lookup"><span data-stu-id="0bb9c-165">If a handler calls `context.Succeed` or `context.Fail`, all other handlers are still called.</span></span> <span data-ttu-id="0bb9c-166">Esto permite que los requisitos producir efectos secundarios, como el registro, que tiene lugar incluso si otro controlador ha validado correctamente o bien no un requisito.</span><span class="sxs-lookup"><span data-stu-id="0bb9c-166">This allows requirements to produce side effects, such as logging, which takes place even if another handler has successfully validated or failed a requirement.</span></span> <span data-ttu-id="0bb9c-167">Cuando se establece en `false`, [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) propiedad (disponible en ASP.NET Core 1.1 y versiones posterior) provoca un cortocircuito en la ejecución de controladores cuando `context.Fail` se llama.</span><span class="sxs-lookup"><span data-stu-id="0bb9c-167">When set to `false`, the [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) property (available in ASP.NET Core 1.1 and later) short-circuits the execution of handlers when `context.Fail` is called.</span></span> <span data-ttu-id="0bb9c-168">`InvokeHandlersAfterFailure` el valor predeterminado es `true`, en cuyo caso se llama a todos los controladores.</span><span class="sxs-lookup"><span data-stu-id="0bb9c-168">`InvokeHandlersAfterFailure` defaults to `true`, in which case all handlers are called.</span></span>

> [!NOTE]
> <span data-ttu-id="0bb9c-169">Incluso si se produce un error de autenticación, se denominan controladores de autorización.</span><span class="sxs-lookup"><span data-stu-id="0bb9c-169">Authorization handlers are called even if authentication fails.</span></span>

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a><span data-ttu-id="0bb9c-170">¿Por qué querría varios controladores para un requisito?</span><span class="sxs-lookup"><span data-stu-id="0bb9c-170">Why would I want multiple handlers for a requirement?</span></span>

<span data-ttu-id="0bb9c-171">En los casos donde desee que la evaluación en un **o** base, implementar varios controladores para un requisito único.</span><span class="sxs-lookup"><span data-stu-id="0bb9c-171">In cases where you want evaluation to be on an **OR** basis, implement multiple handlers for a single requirement.</span></span> <span data-ttu-id="0bb9c-172">Por ejemplo, Microsoft tiene puertas que abre sólo con las tarjetas de clave.</span><span class="sxs-lookup"><span data-stu-id="0bb9c-172">For example, Microsoft has doors which only open with key cards.</span></span> <span data-ttu-id="0bb9c-173">Si deja la tarjeta de claves en casa, la recepcionista imprime un adhesivo temporal y abre la puerta.</span><span class="sxs-lookup"><span data-stu-id="0bb9c-173">If you leave your key card at home, the receptionist prints a temporary sticker and opens the door for you.</span></span> <span data-ttu-id="0bb9c-174">En este escenario, tendría un requisito único, *BuildingEntry*, pero varios controladores, cada uno de ellos examinando un requisito único.</span><span class="sxs-lookup"><span data-stu-id="0bb9c-174">In this scenario, you'd have a single requirement, *BuildingEntry*, but multiple handlers, each one examining a single requirement.</span></span>

<span data-ttu-id="0bb9c-175">*BuildingEntryRequirement.cs*</span><span class="sxs-lookup"><span data-stu-id="0bb9c-175">*BuildingEntryRequirement.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

<span data-ttu-id="0bb9c-176">*BadgeEntryHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="0bb9c-176">*BadgeEntryHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

<span data-ttu-id="0bb9c-177">*TemporaryStickerHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="0bb9c-177">*TemporaryStickerHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

<span data-ttu-id="0bb9c-178">Asegúrese de que ambos controladores estén [registrado](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span><span class="sxs-lookup"><span data-stu-id="0bb9c-178">Ensure that both handlers are [registered](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span></span> <span data-ttu-id="0bb9c-179">Si cualquier controlador se ejecuta correctamente cuando una directiva se evalúa como el `BuildingEntryRequirement`, la evaluación de directivas se realiza correctamente.</span><span class="sxs-lookup"><span data-stu-id="0bb9c-179">If either handler succeeds when a policy evaluates the `BuildingEntryRequirement`, the policy evaluation succeeds.</span></span>

## <a name="using-a-func-to-fulfill-a-policy"></a><span data-ttu-id="0bb9c-180">Uso de func para satisfacer una directiva</span><span class="sxs-lookup"><span data-stu-id="0bb9c-180">Using a func to fulfill a policy</span></span>

<span data-ttu-id="0bb9c-181">Puede haber situaciones en que cumplir una directiva es sencilla expresar en código.</span><span class="sxs-lookup"><span data-stu-id="0bb9c-181">There may be situations in which fulfilling a policy is simple to express in code.</span></span> <span data-ttu-id="0bb9c-182">Es posible proporcionar un `Func<AuthorizationHandlerContext, bool>` al configurar la directiva con el `RequireAssertion` el generador de directiva.</span><span class="sxs-lookup"><span data-stu-id="0bb9c-182">It's possible to supply a `Func<AuthorizationHandlerContext, bool>` when configuring your policy with the `RequireAssertion` policy builder.</span></span>

<span data-ttu-id="0bb9c-183">Por ejemplo, el anterior `BadgeEntryHandler` podría volver a escribirse como sigue:</span><span class="sxs-lookup"><span data-stu-id="0bb9c-183">For example, the previous `BadgeEntryHandler` could be rewritten as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=50-51,55-61)]

## <a name="accessing-mvc-request-context-in-handlers"></a><span data-ttu-id="0bb9c-184">Acceso al contexto de solicitud MVC en controladores</span><span class="sxs-lookup"><span data-stu-id="0bb9c-184">Accessing MVC request context in handlers</span></span>

<span data-ttu-id="0bb9c-185">El `HandleRequirementAsync` método se implementa en un controlador de autorización tiene dos parámetros: un `AuthorizationHandlerContext` y `TRequirement` está controlando.</span><span class="sxs-lookup"><span data-stu-id="0bb9c-185">The `HandleRequirementAsync` method you implement in an authorization handler has two parameters: an `AuthorizationHandlerContext` and the `TRequirement` you are handling.</span></span> <span data-ttu-id="0bb9c-186">Marcos de trabajo como MVC o Jabbr son gratuitas agregar cualquier objeto a la `Resource` propiedad en el `AuthorizationHandlerContext` para pasar información adicional.</span><span class="sxs-lookup"><span data-stu-id="0bb9c-186">Frameworks such as MVC or Jabbr are free to add any object to the `Resource` property on the `AuthorizationHandlerContext` to pass extra information.</span></span>

<span data-ttu-id="0bb9c-187">Por ejemplo, MVC pasa una instancia de [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) en el `Resource` propiedad.</span><span class="sxs-lookup"><span data-stu-id="0bb9c-187">For example, MVC passes an instance of [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) in the `Resource` property.</span></span> <span data-ttu-id="0bb9c-188">Esta propiedad proporciona acceso a `HttpContext`, `RouteData`y todo lo contrario, proporcionada por MVC y páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="0bb9c-188">This property provides access to `HttpContext`, `RouteData`, and everything else provided by MVC and Razor Pages.</span></span>

<span data-ttu-id="0bb9c-189">El uso de la `Resource` propiedad es el marco específico.</span><span class="sxs-lookup"><span data-stu-id="0bb9c-189">The use of the `Resource` property is framework specific.</span></span> <span data-ttu-id="0bb9c-190">Uso de la información la `Resource` propiedad limita sus directivas de autorización para marcos de trabajo determinados.</span><span class="sxs-lookup"><span data-stu-id="0bb9c-190">Using information in the `Resource` property limits your authorization policies to particular frameworks.</span></span> <span data-ttu-id="0bb9c-191">Primero debe convertir el `Resource` propiedad mediante la `is` palabra clave y, a continuación, confirme la conversión se realizó correctamente para asegurarse de que no se bloquee el código con un `InvalidCastException` cuando se ejecuta en otros marcos de trabajo:</span><span class="sxs-lookup"><span data-stu-id="0bb9c-191">You should cast the `Resource` property using the `is` keyword, and then confirm the cast has succeeded to ensure your code doesn't crash with an `InvalidCastException` when run on other frameworks:</span></span>

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```
