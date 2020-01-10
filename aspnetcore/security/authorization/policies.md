---
title: Autorización basada en directivas en ASP.NET Core
author: rick-anderson
description: Aprenda a crear y usar controladores de directivas de autorización para aplicar los requisitos de autorización en una aplicación ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 10/05/2019
uid: security/authorization/policies
ms.openlocfilehash: eeb5ddd63ef8177325b35e5a666aa5e9ab047057
ms.sourcegitcommit: 7dfe6cc8408ac6a4549c29ca57b0c67ec4baa8de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/09/2020
ms.locfileid: "75828963"
---
# <a name="policy-based-authorization-in-aspnet-core"></a><span data-ttu-id="19dcf-103">Autorización basada en directivas en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="19dcf-103">Policy-based authorization in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="19dcf-104">En segundo plano, la autorización basada en [roles](xref:security/authorization/roles) y la [autorización basada en notificaciones](xref:security/authorization/claims) usan un requisito, un controlador de requisitos y una directiva configurada previamente.</span><span class="sxs-lookup"><span data-stu-id="19dcf-104">Underneath the covers, [role-based authorization](xref:security/authorization/roles) and [claims-based authorization](xref:security/authorization/claims) use a requirement, a requirement handler, and a pre-configured policy.</span></span> <span data-ttu-id="19dcf-105">Estos bloques de creación admiten la expresión de evaluaciones de autorización en el código.</span><span class="sxs-lookup"><span data-stu-id="19dcf-105">These building blocks support the expression of authorization evaluations in code.</span></span> <span data-ttu-id="19dcf-106">El resultado es una estructura de autorización más enriquecida, reutilizable y comprobable.</span><span class="sxs-lookup"><span data-stu-id="19dcf-106">The result is a richer, reusable, testable authorization structure.</span></span>

<span data-ttu-id="19dcf-107">Una directiva de autorización se compone de uno o varios requisitos.</span><span class="sxs-lookup"><span data-stu-id="19dcf-107">An authorization policy consists of one or more requirements.</span></span> <span data-ttu-id="19dcf-108">Se registra como parte de la configuración del servicio de autorización, en el método `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="19dcf-108">It's registered as part of the authorization service configuration, in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](policies/samples/3.0PoliciesAuthApp1/Startup.cs?range=31-32,39-40,42-45, 53, 58)]

<span data-ttu-id="19dcf-109">En el ejemplo anterior, se crea una directiva "AtLeast21".</span><span class="sxs-lookup"><span data-stu-id="19dcf-109">In the preceding example, an "AtLeast21" policy is created.</span></span> <span data-ttu-id="19dcf-110">Tiene un único requisito&mdash;de una edad mínima, que se proporciona como un parámetro al requisito.</span><span class="sxs-lookup"><span data-stu-id="19dcf-110">It has a single requirement&mdash;that of a minimum age, which is supplied as a parameter to the requirement.</span></span>

## <a name="iauthorizationservice"></a><span data-ttu-id="19dcf-111">IAuthorizationService</span><span class="sxs-lookup"><span data-stu-id="19dcf-111">IAuthorizationService</span></span> 

<span data-ttu-id="19dcf-112">El servicio principal que determina si la autorización se realiza correctamente es <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService>:</span><span class="sxs-lookup"><span data-stu-id="19dcf-112">The primary service that determines if authorization is successful is <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService>:</span></span>

[!code-csharp[](policies/samples/stubs/copy_of_IAuthorizationService.cs?highlight=24-25,48-49&name=snippet)]

<span data-ttu-id="19dcf-113">En el código anterior se resaltan los dos métodos de [IAuthorizationService](https://github.com/dotnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationService.cs).</span><span class="sxs-lookup"><span data-stu-id="19dcf-113">The preceding code highlights the two methods of the [IAuthorizationService](https://github.com/dotnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationService.cs).</span></span>

<span data-ttu-id="19dcf-114"><xref:Microsoft.AspNetCore.Authorization.IAuthorizationRequirement> es un servicio de marcador sin métodos y el mecanismo para hacer un seguimiento de si la autorización se realiza correctamente.</span><span class="sxs-lookup"><span data-stu-id="19dcf-114"><xref:Microsoft.AspNetCore.Authorization.IAuthorizationRequirement> is a marker service with no methods, and the mechanism for tracking whether authorization is successful.</span></span>

<span data-ttu-id="19dcf-115">Cada <xref:Microsoft.AspNetCore.Authorization.IAuthorizationHandler> es responsable de comprobar si se cumplen los requisitos:</span><span class="sxs-lookup"><span data-stu-id="19dcf-115">Each <xref:Microsoft.AspNetCore.Authorization.IAuthorizationHandler> is responsible for checking if requirements are met:</span></span>
<!--The following code is a copy/paste from 
https://github.com/dotnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationHandler.cs -->

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

<span data-ttu-id="19dcf-116">La clase <xref:Microsoft.AspNetCore.Authorization.AuthorizationHandlerContext> es lo que el controlador utiliza para indicar si se han cumplido los requisitos:</span><span class="sxs-lookup"><span data-stu-id="19dcf-116">The <xref:Microsoft.AspNetCore.Authorization.AuthorizationHandlerContext> class is what the handler uses to mark whether requirements have been met:</span></span>

```csharp
 context.Succeed(requirement)
```

<span data-ttu-id="19dcf-117">En el código siguiente se muestra la implementación predeterminada simplificada (y anotada con comentarios) del servicio de autorización:</span><span class="sxs-lookup"><span data-stu-id="19dcf-117">The following code shows the simplified (and annotated with comments) default implementation of the authorization service:</span></span>

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

<span data-ttu-id="19dcf-118">En el código siguiente se muestra un `ConfigureServices`típico:</span><span class="sxs-lookup"><span data-stu-id="19dcf-118">The following code shows a typical `ConfigureServices`:</span></span>

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


    services.AddControllersWithViews();
    services.AddRazorPages();
}
```

<span data-ttu-id="19dcf-119">Use <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService> o `[Authorize(Policy = "Something")]` para la autorización.</span><span class="sxs-lookup"><span data-stu-id="19dcf-119">Use <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService> or `[Authorize(Policy = "Something")]` for authorization.</span></span>

## <a name="applying-policies-to-mvc-controllers"></a><span data-ttu-id="19dcf-120">Aplicar directivas a los controladores de MVC</span><span class="sxs-lookup"><span data-stu-id="19dcf-120">Applying policies to MVC controllers</span></span>

<span data-ttu-id="19dcf-121">Si utiliza Razor Pages, consulte [aplicar directivas a Razor pages](#applying-policies-to-razor-pages) en este documento.</span><span class="sxs-lookup"><span data-stu-id="19dcf-121">If you're using Razor Pages, see [Applying policies to Razor Pages](#applying-policies-to-razor-pages) in this document.</span></span>

<span data-ttu-id="19dcf-122">Las directivas se aplican a los controladores mediante el atributo `[Authorize]` con el nombre de la Directiva.</span><span class="sxs-lookup"><span data-stu-id="19dcf-122">Policies are applied to controllers by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="19dcf-123">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="19dcf-123">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="applying-policies-to-razor-pages"></a><span data-ttu-id="19dcf-124">Aplicar directivas a Razor Pages</span><span class="sxs-lookup"><span data-stu-id="19dcf-124">Applying policies to Razor Pages</span></span>

<span data-ttu-id="19dcf-125">Las directivas se aplican a Razor Pages mediante el atributo `[Authorize]` con el nombre de la Directiva.</span><span class="sxs-lookup"><span data-stu-id="19dcf-125">Policies are applied to Razor Pages by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="19dcf-126">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="19dcf-126">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp2/Pages/AlcoholPurchase.cshtml.cs?name=snippet_AlcoholPurchaseModelClass&highlight=4)]

<span data-ttu-id="19dcf-127">También se pueden aplicar directivas a Razor Pages mediante una [Convención de autorización](xref:security/authorization/razor-pages-authorization).</span><span class="sxs-lookup"><span data-stu-id="19dcf-127">Policies can also be applied to Razor Pages by using an [authorization convention](xref:security/authorization/razor-pages-authorization).</span></span>

## <a name="requirements"></a><span data-ttu-id="19dcf-128">Requisitos de</span><span class="sxs-lookup"><span data-stu-id="19dcf-128">Requirements</span></span>

<span data-ttu-id="19dcf-129">Un requisito de autorización es una colección de parámetros de datos que una Directiva puede usar para evaluar la entidad de seguridad de usuario actual.</span><span class="sxs-lookup"><span data-stu-id="19dcf-129">An authorization requirement is a collection of data parameters that a policy can use to evaluate the current user principal.</span></span> <span data-ttu-id="19dcf-130">En nuestra directiva "AtLeast21", el requisito es un parámetro único&mdash;la edad mínima.</span><span class="sxs-lookup"><span data-stu-id="19dcf-130">In our "AtLeast21" policy, the requirement is a single parameter&mdash;the minimum age.</span></span> <span data-ttu-id="19dcf-131">Un requisito implementa [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), que es una interfaz de marcador vacía.</span><span class="sxs-lookup"><span data-stu-id="19dcf-131">A requirement implements [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), which is an empty marker interface.</span></span> <span data-ttu-id="19dcf-132">Un requisito de edad mínima parametrizada podría implementarse de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="19dcf-132">A parameterized minimum age requirement could be implemented as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

<span data-ttu-id="19dcf-133">Si una directiva de autorización contiene varios requisitos de autorización, deben cumplirse todos los requisitos para que la evaluación de la Directiva se realice correctamente.</span><span class="sxs-lookup"><span data-stu-id="19dcf-133">If an authorization policy contains multiple authorization requirements, all requirements must pass in order for the policy evaluation to succeed.</span></span> <span data-ttu-id="19dcf-134">En otras palabras, los distintos requisitos de autorización que se agregan a una sola directiva de autorización se tratan de manera **y** .</span><span class="sxs-lookup"><span data-stu-id="19dcf-134">In other words, multiple authorization requirements added to a single authorization policy are treated on an **AND** basis.</span></span>

> [!NOTE]
> <span data-ttu-id="19dcf-135">No es necesario que un requisito tenga datos ni propiedades.</span><span class="sxs-lookup"><span data-stu-id="19dcf-135">A requirement doesn't need to have data or properties.</span></span>

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a><span data-ttu-id="19dcf-136">Controladores de autorización</span><span class="sxs-lookup"><span data-stu-id="19dcf-136">Authorization handlers</span></span>

<span data-ttu-id="19dcf-137">Un controlador de autorización es responsable de la evaluación de las propiedades de un requisito.</span><span class="sxs-lookup"><span data-stu-id="19dcf-137">An authorization handler is responsible for the evaluation of a requirement's properties.</span></span> <span data-ttu-id="19dcf-138">El controlador de autorización evalúa los requisitos con respecto a un [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) proporcionado para determinar si se permite el acceso.</span><span class="sxs-lookup"><span data-stu-id="19dcf-138">The authorization handler evaluates the requirements against a provided [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) to determine if access is allowed.</span></span>

<span data-ttu-id="19dcf-139">Un requisito puede tener [varios controladores](#security-authorization-policies-based-multiple-handlers).</span><span class="sxs-lookup"><span data-stu-id="19dcf-139">A requirement can have [multiple handlers](#security-authorization-policies-based-multiple-handlers).</span></span> <span data-ttu-id="19dcf-140">Un controlador puede heredar [AuthorizationHandler\<TRequirement >](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), donde `TRequirement` es el requisito que se debe controlar.</span><span class="sxs-lookup"><span data-stu-id="19dcf-140">A handler may inherit [AuthorizationHandler\<TRequirement>](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), where `TRequirement` is the requirement to be handled.</span></span> <span data-ttu-id="19dcf-141">Como alternativa, un controlador puede implementar [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) para administrar más de un tipo de requisito.</span><span class="sxs-lookup"><span data-stu-id="19dcf-141">Alternatively, a handler may implement [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) to handle more than one type of requirement.</span></span>

### <a name="use-a-handler-for-one-requirement"></a><span data-ttu-id="19dcf-142">Usar un controlador para un requisito</span><span class="sxs-lookup"><span data-stu-id="19dcf-142">Use a handler for one requirement</span></span>

<a name="security-authorization-handler-example"></a>

<span data-ttu-id="19dcf-143">El siguiente es un ejemplo de una relación uno a uno en la que un controlador de edad mínima emplea un único requisito:</span><span class="sxs-lookup"><span data-stu-id="19dcf-143">The following is an example of a one-to-one relationship in which a minimum age handler utilizes a single requirement:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

<span data-ttu-id="19dcf-144">El código anterior determina si la entidad de seguridad de usuario actual tiene una declaración de nacimiento de fecha que ha sido emitida por un emisor conocido y de confianza.</span><span class="sxs-lookup"><span data-stu-id="19dcf-144">The preceding code determines if the current user principal has a date of birth claim which has been issued by a known and trusted Issuer.</span></span> <span data-ttu-id="19dcf-145">No se puede realizar la autorización cuando falta la demanda, en cuyo caso se devuelve una tarea completada.</span><span class="sxs-lookup"><span data-stu-id="19dcf-145">Authorization can't occur when the claim is missing, in which case a completed task is returned.</span></span> <span data-ttu-id="19dcf-146">Cuando existe una demanda, se calcula la edad del usuario.</span><span class="sxs-lookup"><span data-stu-id="19dcf-146">When a claim is present, the user's age is calculated.</span></span> <span data-ttu-id="19dcf-147">Si el usuario cumple la edad mínima definida por el requisito, la autorización se considera correcta.</span><span class="sxs-lookup"><span data-stu-id="19dcf-147">If the user meets the minimum age defined by the requirement, authorization is deemed successful.</span></span> <span data-ttu-id="19dcf-148">Cuando la autorización se realiza correctamente, `context.Succeed` se invoca con el requisito satisfecho como su único parámetro.</span><span class="sxs-lookup"><span data-stu-id="19dcf-148">When authorization is successful, `context.Succeed` is invoked with the satisfied requirement as its sole parameter.</span></span>

### <a name="use-a-handler-for-multiple-requirements"></a><span data-ttu-id="19dcf-149">Usar un controlador para varios requisitos</span><span class="sxs-lookup"><span data-stu-id="19dcf-149">Use a handler for multiple requirements</span></span>

<span data-ttu-id="19dcf-150">El siguiente es un ejemplo de una relación de uno a varios en la que un controlador de permisos puede controlar tres tipos diferentes de requisitos:</span><span class="sxs-lookup"><span data-stu-id="19dcf-150">The following is an example of a one-to-many relationship in which a permission handler can handle three different types of requirements:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/PermissionHandler.cs?name=snippet_PermissionHandlerClass)]

<span data-ttu-id="19dcf-151">El código anterior atraviesa [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;una propiedad que contiene requisitos no marcados como correctos.</span><span class="sxs-lookup"><span data-stu-id="19dcf-151">The preceding code traverses [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;a property containing requirements not marked as successful.</span></span> <span data-ttu-id="19dcf-152">Para un requisito `ReadPermission`, el usuario debe ser un propietario o un patrocinador para tener acceso al recurso solicitado.</span><span class="sxs-lookup"><span data-stu-id="19dcf-152">For a `ReadPermission` requirement, the user must be either an owner or a sponsor to access the requested resource.</span></span> <span data-ttu-id="19dcf-153">En el caso de un requisito `EditPermission` o `DeletePermission`, debe ser un propietario para tener acceso al recurso solicitado.</span><span class="sxs-lookup"><span data-stu-id="19dcf-153">In the case of an `EditPermission` or `DeletePermission` requirement, he or she must be an owner to access the requested resource.</span></span>

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a><span data-ttu-id="19dcf-154">Registro del controlador</span><span class="sxs-lookup"><span data-stu-id="19dcf-154">Handler registration</span></span>

<span data-ttu-id="19dcf-155">Los controladores se registran en la colección de servicios durante la configuración.</span><span class="sxs-lookup"><span data-stu-id="19dcf-155">Handlers are registered in the services collection during configuration.</span></span> <span data-ttu-id="19dcf-156">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="19dcf-156">For example:</span></span>

[!code-csharp[](policies/samples/3.0PoliciesAuthApp1/Startup.cs?range=31-32,39-40,42-45, 53-55, 58)]

<span data-ttu-id="19dcf-157">El código anterior registra `MinimumAgeHandler` como singleton mediante la invocación de `services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();`.</span><span class="sxs-lookup"><span data-stu-id="19dcf-157">The preceding code registers `MinimumAgeHandler` as a singleton by invoking `services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();`.</span></span> <span data-ttu-id="19dcf-158">Los controladores se pueden registrar con cualquiera de las [duraciones de servicio](xref:fundamentals/dependency-injection#service-lifetimes)integradas.</span><span class="sxs-lookup"><span data-stu-id="19dcf-158">Handlers can be registered using any of the built-in [service lifetimes](xref:fundamentals/dependency-injection#service-lifetimes).</span></span>

## <a name="what-should-a-handler-return"></a><span data-ttu-id="19dcf-159">¿Qué debe devolver un controlador?</span><span class="sxs-lookup"><span data-stu-id="19dcf-159">What should a handler return?</span></span>

<span data-ttu-id="19dcf-160">Tenga en cuenta que el método `Handle` del [ejemplo de controlador](#security-authorization-handler-example) no devuelve ningún valor.</span><span class="sxs-lookup"><span data-stu-id="19dcf-160">Note that the `Handle` method in the [handler example](#security-authorization-handler-example) returns no value.</span></span> <span data-ttu-id="19dcf-161">¿Cómo se indica el estado correcto o incorrecto?</span><span class="sxs-lookup"><span data-stu-id="19dcf-161">How is a status of either success or failure indicated?</span></span>

* <span data-ttu-id="19dcf-162">Un controlador indica que la operación se realizó correctamente mediante una llamada a `context.Succeed(IAuthorizationRequirement requirement)`, pasando el requisito que se ha validado correctamente.</span><span class="sxs-lookup"><span data-stu-id="19dcf-162">A handler indicates success by calling `context.Succeed(IAuthorizationRequirement requirement)`, passing the requirement that has been successfully validated.</span></span>

* <span data-ttu-id="19dcf-163">Un controlador no necesita controlar los errores por lo general, ya que otros controladores para el mismo requisito pueden realizarse correctamente.</span><span class="sxs-lookup"><span data-stu-id="19dcf-163">A handler doesn't need to handle failures generally, as other handlers for the same requirement may succeed.</span></span>

* <span data-ttu-id="19dcf-164">Para garantizar un error, incluso si se realizan correctamente otros controladores de requisitos, llame a `context.Fail`.</span><span class="sxs-lookup"><span data-stu-id="19dcf-164">To guarantee failure, even if other requirement handlers succeed, call `context.Fail`.</span></span>

<span data-ttu-id="19dcf-165">Si un controlador llama a `context.Succeed` o `context.Fail`, todavía se llama a todos los demás controladores.</span><span class="sxs-lookup"><span data-stu-id="19dcf-165">If a handler calls `context.Succeed` or `context.Fail`, all other handlers are still called.</span></span> <span data-ttu-id="19dcf-166">Esto permite a los requisitos producir efectos secundarios, como el registro, que tiene lugar incluso si otro controlador ha validado o no un requisito correctamente.</span><span class="sxs-lookup"><span data-stu-id="19dcf-166">This allows requirements to produce side effects, such as logging, which takes place even if another handler has successfully validated or failed a requirement.</span></span> <span data-ttu-id="19dcf-167">Cuando se establece en `false`, la propiedad [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) (disponible en ASP.net Core 1,1 y versiones posteriores) cortocircuita la ejecución de los controladores cuando se llama a `context.Fail`.</span><span class="sxs-lookup"><span data-stu-id="19dcf-167">When set to `false`, the [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) property (available in ASP.NET Core 1.1 and later) short-circuits the execution of handlers when `context.Fail` is called.</span></span> <span data-ttu-id="19dcf-168">`InvokeHandlersAfterFailure` tiene como valor predeterminado `true`, en cuyo caso se llama a todos los controladores.</span><span class="sxs-lookup"><span data-stu-id="19dcf-168">`InvokeHandlersAfterFailure` defaults to `true`, in which case all handlers are called.</span></span>

> [!NOTE]
> <span data-ttu-id="19dcf-169">Se llama a los controladores de autorización incluso si se produce un error de autenticación.</span><span class="sxs-lookup"><span data-stu-id="19dcf-169">Authorization handlers are called even if authentication fails.</span></span>

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a><span data-ttu-id="19dcf-170">¿Por qué sería conveniente tener varios controladores para un requisito?</span><span class="sxs-lookup"><span data-stu-id="19dcf-170">Why would I want multiple handlers for a requirement?</span></span>

<span data-ttu-id="19dcf-171">En los casos en los que desea que la evaluación esté en una base de **o** , implemente varios controladores para un único requisito.</span><span class="sxs-lookup"><span data-stu-id="19dcf-171">In cases where you want evaluation to be on an **OR** basis, implement multiple handlers for a single requirement.</span></span> <span data-ttu-id="19dcf-172">Por ejemplo, Microsoft tiene puertas que solo se abren con tarjetas clave.</span><span class="sxs-lookup"><span data-stu-id="19dcf-172">For example, Microsoft has doors which only open with key cards.</span></span> <span data-ttu-id="19dcf-173">Si deja la tarjeta de la llave en casa, el recepcionista imprime una etiqueta temporal y abre la puerta.</span><span class="sxs-lookup"><span data-stu-id="19dcf-173">If you leave your key card at home, the receptionist prints a temporary sticker and opens the door for you.</span></span> <span data-ttu-id="19dcf-174">En este escenario, tendría un único requisito, *BuildingEntry*, pero varios controladores, cada uno examinando un único requisito.</span><span class="sxs-lookup"><span data-stu-id="19dcf-174">In this scenario, you'd have a single requirement, *BuildingEntry*, but multiple handlers, each one examining a single requirement.</span></span>

<span data-ttu-id="19dcf-175">*BuildingEntryRequirement.cs*</span><span class="sxs-lookup"><span data-stu-id="19dcf-175">*BuildingEntryRequirement.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

<span data-ttu-id="19dcf-176">*BadgeEntryHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="19dcf-176">*BadgeEntryHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

<span data-ttu-id="19dcf-177">*TemporaryStickerHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="19dcf-177">*TemporaryStickerHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

<span data-ttu-id="19dcf-178">Asegúrese de que ambos controladores están [registrados](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span><span class="sxs-lookup"><span data-stu-id="19dcf-178">Ensure that both handlers are [registered](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span></span> <span data-ttu-id="19dcf-179">Si alguno de los controladores se realiza correctamente cuando una directiva evalúa el `BuildingEntryRequirement`, la evaluación de la Directiva se realiza correctamente.</span><span class="sxs-lookup"><span data-stu-id="19dcf-179">If either handler succeeds when a policy evaluates the `BuildingEntryRequirement`, the policy evaluation succeeds.</span></span>

## <a name="using-a-func-to-fulfill-a-policy"></a><span data-ttu-id="19dcf-180">Usar un FUNC para cumplir una directiva</span><span class="sxs-lookup"><span data-stu-id="19dcf-180">Using a func to fulfill a policy</span></span>

<span data-ttu-id="19dcf-181">Puede haber situaciones en las que el cumplimiento de una directiva sea fácil de expresar en el código.</span><span class="sxs-lookup"><span data-stu-id="19dcf-181">There may be situations in which fulfilling a policy is simple to express in code.</span></span> <span data-ttu-id="19dcf-182">Es posible proporcionar un `Func<AuthorizationHandlerContext, bool>` al configurar la Directiva con el generador de directivas de `RequireAssertion`.</span><span class="sxs-lookup"><span data-stu-id="19dcf-182">It's possible to supply a `Func<AuthorizationHandlerContext, bool>` when configuring your policy with the `RequireAssertion` policy builder.</span></span>

<span data-ttu-id="19dcf-183">Por ejemplo, el `BadgeEntryHandler` anterior podría volver a escribirse de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="19dcf-183">For example, the previous `BadgeEntryHandler` could be rewritten as follows:</span></span>

[!code-csharp[](policies/samples/3.0PoliciesAuthApp1/Startup.cs?range=42-43,47-53)]

## <a name="accessing-mvc-request-context-in-handlers"></a><span data-ttu-id="19dcf-184">Acceso al contexto de solicitud MVC en los controladores</span><span class="sxs-lookup"><span data-stu-id="19dcf-184">Accessing MVC request context in handlers</span></span>

<span data-ttu-id="19dcf-185">El método `HandleRequirementAsync` que implementa en un controlador de autorización tiene dos parámetros: un `AuthorizationHandlerContext` y el `TRequirement` que está controlando.</span><span class="sxs-lookup"><span data-stu-id="19dcf-185">The `HandleRequirementAsync` method you implement in an authorization handler has two parameters: an `AuthorizationHandlerContext` and the `TRequirement` you are handling.</span></span> <span data-ttu-id="19dcf-186">Los marcos como MVC o Jabbr pueden agregar cualquier objeto a la propiedad `Resource` en el `AuthorizationHandlerContext` para pasar información adicional.</span><span class="sxs-lookup"><span data-stu-id="19dcf-186">Frameworks such as MVC or Jabbr are free to add any object to the `Resource` property on the `AuthorizationHandlerContext` to pass extra information.</span></span>

<span data-ttu-id="19dcf-187">Por ejemplo, MVC pasa una instancia de [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) en la propiedad `Resource`.</span><span class="sxs-lookup"><span data-stu-id="19dcf-187">For example, MVC passes an instance of [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) in the `Resource` property.</span></span> <span data-ttu-id="19dcf-188">Esta propiedad proporciona acceso a `HttpContext`, `RouteData`y todo lo demás proporcionado por MVC y Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="19dcf-188">This property provides access to `HttpContext`, `RouteData`, and everything else provided by MVC and Razor Pages.</span></span>

<span data-ttu-id="19dcf-189">El uso de la propiedad `Resource` es específico del marco de trabajo.</span><span class="sxs-lookup"><span data-stu-id="19dcf-189">The use of the `Resource` property is framework specific.</span></span> <span data-ttu-id="19dcf-190">El uso de la información en la propiedad `Resource` limita las directivas de autorización a marcos concretos.</span><span class="sxs-lookup"><span data-stu-id="19dcf-190">Using information in the `Resource` property limits your authorization policies to particular frameworks.</span></span> <span data-ttu-id="19dcf-191">Debe convertir la propiedad `Resource` mediante la palabra clave `is` y, a continuación, confirmar que la conversión se ha realizado correctamente para asegurarse de que el código no se bloquea con un `InvalidCastException` cuando se ejecuta en otros marcos de trabajo:</span><span class="sxs-lookup"><span data-stu-id="19dcf-191">You should cast the `Resource` property using the `is` keyword, and then confirm the cast has succeeded to ensure your code doesn't crash with an `InvalidCastException` when run on other frameworks:</span></span>

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```

::: moniker-end


::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="19dcf-192">En segundo plano, la autorización basada en [roles](xref:security/authorization/roles) y la [autorización basada en notificaciones](xref:security/authorization/claims) usan un requisito, un controlador de requisitos y una directiva configurada previamente.</span><span class="sxs-lookup"><span data-stu-id="19dcf-192">Underneath the covers, [role-based authorization](xref:security/authorization/roles) and [claims-based authorization](xref:security/authorization/claims) use a requirement, a requirement handler, and a pre-configured policy.</span></span> <span data-ttu-id="19dcf-193">Estos bloques de creación admiten la expresión de evaluaciones de autorización en el código.</span><span class="sxs-lookup"><span data-stu-id="19dcf-193">These building blocks support the expression of authorization evaluations in code.</span></span> <span data-ttu-id="19dcf-194">El resultado es una estructura de autorización más enriquecida, reutilizable y comprobable.</span><span class="sxs-lookup"><span data-stu-id="19dcf-194">The result is a richer, reusable, testable authorization structure.</span></span>

<span data-ttu-id="19dcf-195">Una directiva de autorización se compone de uno o varios requisitos.</span><span class="sxs-lookup"><span data-stu-id="19dcf-195">An authorization policy consists of one or more requirements.</span></span> <span data-ttu-id="19dcf-196">Se registra como parte de la configuración del servicio de autorización, en el método `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="19dcf-196">It's registered as part of the authorization service configuration, in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=32-33,48-53,61,66)]

<span data-ttu-id="19dcf-197">En el ejemplo anterior, se crea una directiva "AtLeast21".</span><span class="sxs-lookup"><span data-stu-id="19dcf-197">In the preceding example, an "AtLeast21" policy is created.</span></span> <span data-ttu-id="19dcf-198">Tiene un único requisito&mdash;de una edad mínima, que se proporciona como un parámetro al requisito.</span><span class="sxs-lookup"><span data-stu-id="19dcf-198">It has a single requirement&mdash;that of a minimum age, which is supplied as a parameter to the requirement.</span></span>

## <a name="iauthorizationservice"></a><span data-ttu-id="19dcf-199">IAuthorizationService</span><span class="sxs-lookup"><span data-stu-id="19dcf-199">IAuthorizationService</span></span> 

<span data-ttu-id="19dcf-200">El servicio principal que determina si la autorización se realiza correctamente es <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService>:</span><span class="sxs-lookup"><span data-stu-id="19dcf-200">The primary service that determines if authorization is successful is <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService>:</span></span>

[!code-csharp[](policies/samples/stubs/copy_of_IAuthorizationService.cs?highlight=24-25,48-49&name=snippet)]

<span data-ttu-id="19dcf-201">En el código anterior se resaltan los dos métodos de [IAuthorizationService](https://github.com/dotnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationService.cs).</span><span class="sxs-lookup"><span data-stu-id="19dcf-201">The preceding code highlights the two methods of the [IAuthorizationService](https://github.com/dotnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationService.cs).</span></span>

<span data-ttu-id="19dcf-202"><xref:Microsoft.AspNetCore.Authorization.IAuthorizationRequirement> es un servicio de marcador sin métodos y el mecanismo para hacer un seguimiento de si la autorización se realiza correctamente.</span><span class="sxs-lookup"><span data-stu-id="19dcf-202"><xref:Microsoft.AspNetCore.Authorization.IAuthorizationRequirement> is a marker service with no methods, and the mechanism for tracking whether authorization is successful.</span></span>

<span data-ttu-id="19dcf-203">Cada <xref:Microsoft.AspNetCore.Authorization.IAuthorizationHandler> es responsable de comprobar si se cumplen los requisitos:</span><span class="sxs-lookup"><span data-stu-id="19dcf-203">Each <xref:Microsoft.AspNetCore.Authorization.IAuthorizationHandler> is responsible for checking if requirements are met:</span></span>
<!--The following code is a copy/paste from 
https://github.com/dotnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationHandler.cs -->

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

<span data-ttu-id="19dcf-204">La clase <xref:Microsoft.AspNetCore.Authorization.AuthorizationHandlerContext> es lo que el controlador utiliza para indicar si se han cumplido los requisitos:</span><span class="sxs-lookup"><span data-stu-id="19dcf-204">The <xref:Microsoft.AspNetCore.Authorization.AuthorizationHandlerContext> class is what the handler uses to mark whether requirements have been met:</span></span>

```csharp
 context.Succeed(requirement)
```

<span data-ttu-id="19dcf-205">En el código siguiente se muestra la implementación predeterminada simplificada (y anotada con comentarios) del servicio de autorización:</span><span class="sxs-lookup"><span data-stu-id="19dcf-205">The following code shows the simplified (and annotated with comments) default implementation of the authorization service:</span></span>

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

<span data-ttu-id="19dcf-206">En el código siguiente se muestra un `ConfigureServices`típico:</span><span class="sxs-lookup"><span data-stu-id="19dcf-206">The following code shows a typical `ConfigureServices`:</span></span>

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

<span data-ttu-id="19dcf-207">Use <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService> o `[Authorize(Policy = "Something")]` para la autorización.</span><span class="sxs-lookup"><span data-stu-id="19dcf-207">Use <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService> or `[Authorize(Policy = "Something")]` for authorization.</span></span>

## <a name="applying-policies-to-mvc-controllers"></a><span data-ttu-id="19dcf-208">Aplicar directivas a los controladores de MVC</span><span class="sxs-lookup"><span data-stu-id="19dcf-208">Applying policies to MVC controllers</span></span>

<span data-ttu-id="19dcf-209">Si utiliza Razor Pages, consulte [aplicar directivas a Razor pages](#applying-policies-to-razor-pages) en este documento.</span><span class="sxs-lookup"><span data-stu-id="19dcf-209">If you're using Razor Pages, see [Applying policies to Razor Pages](#applying-policies-to-razor-pages) in this document.</span></span>

<span data-ttu-id="19dcf-210">Las directivas se aplican a los controladores mediante el atributo `[Authorize]` con el nombre de la Directiva.</span><span class="sxs-lookup"><span data-stu-id="19dcf-210">Policies are applied to controllers by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="19dcf-211">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="19dcf-211">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="applying-policies-to-razor-pages"></a><span data-ttu-id="19dcf-212">Aplicar directivas a Razor Pages</span><span class="sxs-lookup"><span data-stu-id="19dcf-212">Applying policies to Razor Pages</span></span>

<span data-ttu-id="19dcf-213">Las directivas se aplican a Razor Pages mediante el atributo `[Authorize]` con el nombre de la Directiva.</span><span class="sxs-lookup"><span data-stu-id="19dcf-213">Policies are applied to Razor Pages by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="19dcf-214">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="19dcf-214">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp2/Pages/AlcoholPurchase.cshtml.cs?name=snippet_AlcoholPurchaseModelClass&highlight=4)]

<span data-ttu-id="19dcf-215">También se pueden aplicar directivas a Razor Pages mediante una [Convención de autorización](xref:security/authorization/razor-pages-authorization).</span><span class="sxs-lookup"><span data-stu-id="19dcf-215">Policies can also be applied to Razor Pages by using an [authorization convention](xref:security/authorization/razor-pages-authorization).</span></span>

## <a name="requirements"></a><span data-ttu-id="19dcf-216">Requisitos de</span><span class="sxs-lookup"><span data-stu-id="19dcf-216">Requirements</span></span>

<span data-ttu-id="19dcf-217">Un requisito de autorización es una colección de parámetros de datos que una Directiva puede usar para evaluar la entidad de seguridad de usuario actual.</span><span class="sxs-lookup"><span data-stu-id="19dcf-217">An authorization requirement is a collection of data parameters that a policy can use to evaluate the current user principal.</span></span> <span data-ttu-id="19dcf-218">En nuestra directiva "AtLeast21", el requisito es un parámetro único&mdash;la edad mínima.</span><span class="sxs-lookup"><span data-stu-id="19dcf-218">In our "AtLeast21" policy, the requirement is a single parameter&mdash;the minimum age.</span></span> <span data-ttu-id="19dcf-219">Un requisito implementa [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), que es una interfaz de marcador vacía.</span><span class="sxs-lookup"><span data-stu-id="19dcf-219">A requirement implements [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), which is an empty marker interface.</span></span> <span data-ttu-id="19dcf-220">Un requisito de edad mínima parametrizada podría implementarse de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="19dcf-220">A parameterized minimum age requirement could be implemented as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

<span data-ttu-id="19dcf-221">Si una directiva de autorización contiene varios requisitos de autorización, deben cumplirse todos los requisitos para que la evaluación de la Directiva se realice correctamente.</span><span class="sxs-lookup"><span data-stu-id="19dcf-221">If an authorization policy contains multiple authorization requirements, all requirements must pass in order for the policy evaluation to succeed.</span></span> <span data-ttu-id="19dcf-222">En otras palabras, los distintos requisitos de autorización que se agregan a una sola directiva de autorización se tratan de manera **y** .</span><span class="sxs-lookup"><span data-stu-id="19dcf-222">In other words, multiple authorization requirements added to a single authorization policy are treated on an **AND** basis.</span></span>

> [!NOTE]
> <span data-ttu-id="19dcf-223">No es necesario que un requisito tenga datos ni propiedades.</span><span class="sxs-lookup"><span data-stu-id="19dcf-223">A requirement doesn't need to have data or properties.</span></span>

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a><span data-ttu-id="19dcf-224">Controladores de autorización</span><span class="sxs-lookup"><span data-stu-id="19dcf-224">Authorization handlers</span></span>

<span data-ttu-id="19dcf-225">Un controlador de autorización es responsable de la evaluación de las propiedades de un requisito.</span><span class="sxs-lookup"><span data-stu-id="19dcf-225">An authorization handler is responsible for the evaluation of a requirement's properties.</span></span> <span data-ttu-id="19dcf-226">El controlador de autorización evalúa los requisitos con respecto a un [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) proporcionado para determinar si se permite el acceso.</span><span class="sxs-lookup"><span data-stu-id="19dcf-226">The authorization handler evaluates the requirements against a provided [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) to determine if access is allowed.</span></span>

<span data-ttu-id="19dcf-227">Un requisito puede tener [varios controladores](#security-authorization-policies-based-multiple-handlers).</span><span class="sxs-lookup"><span data-stu-id="19dcf-227">A requirement can have [multiple handlers](#security-authorization-policies-based-multiple-handlers).</span></span> <span data-ttu-id="19dcf-228">Un controlador puede heredar [AuthorizationHandler\<TRequirement >](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), donde `TRequirement` es el requisito que se debe controlar.</span><span class="sxs-lookup"><span data-stu-id="19dcf-228">A handler may inherit [AuthorizationHandler\<TRequirement>](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), where `TRequirement` is the requirement to be handled.</span></span> <span data-ttu-id="19dcf-229">Como alternativa, un controlador puede implementar [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) para administrar más de un tipo de requisito.</span><span class="sxs-lookup"><span data-stu-id="19dcf-229">Alternatively, a handler may implement [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) to handle more than one type of requirement.</span></span>

### <a name="use-a-handler-for-one-requirement"></a><span data-ttu-id="19dcf-230">Usar un controlador para un requisito</span><span class="sxs-lookup"><span data-stu-id="19dcf-230">Use a handler for one requirement</span></span>

<a name="security-authorization-handler-example"></a>

<span data-ttu-id="19dcf-231">El siguiente es un ejemplo de una relación uno a uno en la que un controlador de edad mínima emplea un único requisito:</span><span class="sxs-lookup"><span data-stu-id="19dcf-231">The following is an example of a one-to-one relationship in which a minimum age handler utilizes a single requirement:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

<span data-ttu-id="19dcf-232">El código anterior determina si la entidad de seguridad de usuario actual tiene una declaración de nacimiento de fecha que ha sido emitida por un emisor conocido y de confianza.</span><span class="sxs-lookup"><span data-stu-id="19dcf-232">The preceding code determines if the current user principal has a date of birth claim which has been issued by a known and trusted Issuer.</span></span> <span data-ttu-id="19dcf-233">No se puede realizar la autorización cuando falta la demanda, en cuyo caso se devuelve una tarea completada.</span><span class="sxs-lookup"><span data-stu-id="19dcf-233">Authorization can't occur when the claim is missing, in which case a completed task is returned.</span></span> <span data-ttu-id="19dcf-234">Cuando existe una demanda, se calcula la edad del usuario.</span><span class="sxs-lookup"><span data-stu-id="19dcf-234">When a claim is present, the user's age is calculated.</span></span> <span data-ttu-id="19dcf-235">Si el usuario cumple la edad mínima definida por el requisito, la autorización se considera correcta.</span><span class="sxs-lookup"><span data-stu-id="19dcf-235">If the user meets the minimum age defined by the requirement, authorization is deemed successful.</span></span> <span data-ttu-id="19dcf-236">Cuando la autorización se realiza correctamente, `context.Succeed` se invoca con el requisito satisfecho como su único parámetro.</span><span class="sxs-lookup"><span data-stu-id="19dcf-236">When authorization is successful, `context.Succeed` is invoked with the satisfied requirement as its sole parameter.</span></span>

### <a name="use-a-handler-for-multiple-requirements"></a><span data-ttu-id="19dcf-237">Usar un controlador para varios requisitos</span><span class="sxs-lookup"><span data-stu-id="19dcf-237">Use a handler for multiple requirements</span></span>

<span data-ttu-id="19dcf-238">El siguiente es un ejemplo de una relación de uno a varios en la que un controlador de permisos puede controlar tres tipos diferentes de requisitos:</span><span class="sxs-lookup"><span data-stu-id="19dcf-238">The following is an example of a one-to-many relationship in which a permission handler can handle three different types of requirements:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/PermissionHandler.cs?name=snippet_PermissionHandlerClass)]

<span data-ttu-id="19dcf-239">El código anterior atraviesa [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;una propiedad que contiene requisitos no marcados como correctos.</span><span class="sxs-lookup"><span data-stu-id="19dcf-239">The preceding code traverses [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;a property containing requirements not marked as successful.</span></span> <span data-ttu-id="19dcf-240">Para un requisito `ReadPermission`, el usuario debe ser un propietario o un patrocinador para tener acceso al recurso solicitado.</span><span class="sxs-lookup"><span data-stu-id="19dcf-240">For a `ReadPermission` requirement, the user must be either an owner or a sponsor to access the requested resource.</span></span> <span data-ttu-id="19dcf-241">En el caso de un requisito `EditPermission` o `DeletePermission`, debe ser un propietario para tener acceso al recurso solicitado.</span><span class="sxs-lookup"><span data-stu-id="19dcf-241">In the case of an `EditPermission` or `DeletePermission` requirement, he or she must be an owner to access the requested resource.</span></span>

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a><span data-ttu-id="19dcf-242">Registro del controlador</span><span class="sxs-lookup"><span data-stu-id="19dcf-242">Handler registration</span></span>

<span data-ttu-id="19dcf-243">Los controladores se registran en la colección de servicios durante la configuración.</span><span class="sxs-lookup"><span data-stu-id="19dcf-243">Handlers are registered in the services collection during configuration.</span></span> <span data-ttu-id="19dcf-244">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="19dcf-244">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=32-33,48-53,61,62-63,66)]

<span data-ttu-id="19dcf-245">El código anterior registra `MinimumAgeHandler` como singleton mediante la invocación de `services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();`.</span><span class="sxs-lookup"><span data-stu-id="19dcf-245">The preceding code registers `MinimumAgeHandler` as a singleton by invoking `services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();`.</span></span> <span data-ttu-id="19dcf-246">Los controladores se pueden registrar con cualquiera de las [duraciones de servicio](xref:fundamentals/dependency-injection#service-lifetimes)integradas.</span><span class="sxs-lookup"><span data-stu-id="19dcf-246">Handlers can be registered using any of the built-in [service lifetimes](xref:fundamentals/dependency-injection#service-lifetimes).</span></span>

## <a name="what-should-a-handler-return"></a><span data-ttu-id="19dcf-247">¿Qué debe devolver un controlador?</span><span class="sxs-lookup"><span data-stu-id="19dcf-247">What should a handler return?</span></span>

<span data-ttu-id="19dcf-248">Tenga en cuenta que el método `Handle` del [ejemplo de controlador](#security-authorization-handler-example) no devuelve ningún valor.</span><span class="sxs-lookup"><span data-stu-id="19dcf-248">Note that the `Handle` method in the [handler example](#security-authorization-handler-example) returns no value.</span></span> <span data-ttu-id="19dcf-249">¿Cómo se indica el estado correcto o incorrecto?</span><span class="sxs-lookup"><span data-stu-id="19dcf-249">How is a status of either success or failure indicated?</span></span>

* <span data-ttu-id="19dcf-250">Un controlador indica que la operación se realizó correctamente mediante una llamada a `context.Succeed(IAuthorizationRequirement requirement)`, pasando el requisito que se ha validado correctamente.</span><span class="sxs-lookup"><span data-stu-id="19dcf-250">A handler indicates success by calling `context.Succeed(IAuthorizationRequirement requirement)`, passing the requirement that has been successfully validated.</span></span>

* <span data-ttu-id="19dcf-251">Un controlador no necesita controlar los errores por lo general, ya que otros controladores para el mismo requisito pueden realizarse correctamente.</span><span class="sxs-lookup"><span data-stu-id="19dcf-251">A handler doesn't need to handle failures generally, as other handlers for the same requirement may succeed.</span></span>

* <span data-ttu-id="19dcf-252">Para garantizar un error, incluso si se realizan correctamente otros controladores de requisitos, llame a `context.Fail`.</span><span class="sxs-lookup"><span data-stu-id="19dcf-252">To guarantee failure, even if other requirement handlers succeed, call `context.Fail`.</span></span>

<span data-ttu-id="19dcf-253">Si un controlador llama a `context.Succeed` o `context.Fail`, todavía se llama a todos los demás controladores.</span><span class="sxs-lookup"><span data-stu-id="19dcf-253">If a handler calls `context.Succeed` or `context.Fail`, all other handlers are still called.</span></span> <span data-ttu-id="19dcf-254">Esto permite a los requisitos producir efectos secundarios, como el registro, que tiene lugar incluso si otro controlador ha validado o no un requisito correctamente.</span><span class="sxs-lookup"><span data-stu-id="19dcf-254">This allows requirements to produce side effects, such as logging, which takes place even if another handler has successfully validated or failed a requirement.</span></span> <span data-ttu-id="19dcf-255">Cuando se establece en `false`, la propiedad [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) (disponible en ASP.net Core 1,1 y versiones posteriores) cortocircuita la ejecución de los controladores cuando se llama a `context.Fail`.</span><span class="sxs-lookup"><span data-stu-id="19dcf-255">When set to `false`, the [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) property (available in ASP.NET Core 1.1 and later) short-circuits the execution of handlers when `context.Fail` is called.</span></span> <span data-ttu-id="19dcf-256">`InvokeHandlersAfterFailure` tiene como valor predeterminado `true`, en cuyo caso se llama a todos los controladores.</span><span class="sxs-lookup"><span data-stu-id="19dcf-256">`InvokeHandlersAfterFailure` defaults to `true`, in which case all handlers are called.</span></span>

> [!NOTE]
> <span data-ttu-id="19dcf-257">Se llama a los controladores de autorización incluso si se produce un error de autenticación.</span><span class="sxs-lookup"><span data-stu-id="19dcf-257">Authorization handlers are called even if authentication fails.</span></span>

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a><span data-ttu-id="19dcf-258">¿Por qué sería conveniente tener varios controladores para un requisito?</span><span class="sxs-lookup"><span data-stu-id="19dcf-258">Why would I want multiple handlers for a requirement?</span></span>

<span data-ttu-id="19dcf-259">En los casos en los que desea que la evaluación esté en una base de **o** , implemente varios controladores para un único requisito.</span><span class="sxs-lookup"><span data-stu-id="19dcf-259">In cases where you want evaluation to be on an **OR** basis, implement multiple handlers for a single requirement.</span></span> <span data-ttu-id="19dcf-260">Por ejemplo, Microsoft tiene puertas que solo se abren con tarjetas clave.</span><span class="sxs-lookup"><span data-stu-id="19dcf-260">For example, Microsoft has doors which only open with key cards.</span></span> <span data-ttu-id="19dcf-261">Si deja la tarjeta de la llave en casa, el recepcionista imprime una etiqueta temporal y abre la puerta.</span><span class="sxs-lookup"><span data-stu-id="19dcf-261">If you leave your key card at home, the receptionist prints a temporary sticker and opens the door for you.</span></span> <span data-ttu-id="19dcf-262">En este escenario, tendría un único requisito, *BuildingEntry*, pero varios controladores, cada uno examinando un único requisito.</span><span class="sxs-lookup"><span data-stu-id="19dcf-262">In this scenario, you'd have a single requirement, *BuildingEntry*, but multiple handlers, each one examining a single requirement.</span></span>

<span data-ttu-id="19dcf-263">*BuildingEntryRequirement.cs*</span><span class="sxs-lookup"><span data-stu-id="19dcf-263">*BuildingEntryRequirement.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

<span data-ttu-id="19dcf-264">*BadgeEntryHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="19dcf-264">*BadgeEntryHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

<span data-ttu-id="19dcf-265">*TemporaryStickerHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="19dcf-265">*TemporaryStickerHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

<span data-ttu-id="19dcf-266">Asegúrese de que ambos controladores están [registrados](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span><span class="sxs-lookup"><span data-stu-id="19dcf-266">Ensure that both handlers are [registered](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span></span> <span data-ttu-id="19dcf-267">Si alguno de los controladores se realiza correctamente cuando una directiva evalúa el `BuildingEntryRequirement`, la evaluación de la Directiva se realiza correctamente.</span><span class="sxs-lookup"><span data-stu-id="19dcf-267">If either handler succeeds when a policy evaluates the `BuildingEntryRequirement`, the policy evaluation succeeds.</span></span>

## <a name="using-a-func-to-fulfill-a-policy"></a><span data-ttu-id="19dcf-268">Usar un FUNC para cumplir una directiva</span><span class="sxs-lookup"><span data-stu-id="19dcf-268">Using a func to fulfill a policy</span></span>

<span data-ttu-id="19dcf-269">Puede haber situaciones en las que el cumplimiento de una directiva sea fácil de expresar en el código.</span><span class="sxs-lookup"><span data-stu-id="19dcf-269">There may be situations in which fulfilling a policy is simple to express in code.</span></span> <span data-ttu-id="19dcf-270">Es posible proporcionar un `Func<AuthorizationHandlerContext, bool>` al configurar la Directiva con el generador de directivas de `RequireAssertion`.</span><span class="sxs-lookup"><span data-stu-id="19dcf-270">It's possible to supply a `Func<AuthorizationHandlerContext, bool>` when configuring your policy with the `RequireAssertion` policy builder.</span></span>

<span data-ttu-id="19dcf-271">Por ejemplo, el `BadgeEntryHandler` anterior podría volver a escribirse de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="19dcf-271">For example, the previous `BadgeEntryHandler` could be rewritten as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=50-51,55-61)]

## <a name="accessing-mvc-request-context-in-handlers"></a><span data-ttu-id="19dcf-272">Acceso al contexto de solicitud MVC en los controladores</span><span class="sxs-lookup"><span data-stu-id="19dcf-272">Accessing MVC request context in handlers</span></span>

<span data-ttu-id="19dcf-273">El método `HandleRequirementAsync` que implementa en un controlador de autorización tiene dos parámetros: un `AuthorizationHandlerContext` y el `TRequirement` que está controlando.</span><span class="sxs-lookup"><span data-stu-id="19dcf-273">The `HandleRequirementAsync` method you implement in an authorization handler has two parameters: an `AuthorizationHandlerContext` and the `TRequirement` you are handling.</span></span> <span data-ttu-id="19dcf-274">Los marcos como MVC o Jabbr pueden agregar cualquier objeto a la propiedad `Resource` en el `AuthorizationHandlerContext` para pasar información adicional.</span><span class="sxs-lookup"><span data-stu-id="19dcf-274">Frameworks such as MVC or Jabbr are free to add any object to the `Resource` property on the `AuthorizationHandlerContext` to pass extra information.</span></span>

<span data-ttu-id="19dcf-275">Por ejemplo, MVC pasa una instancia de [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) en la propiedad `Resource`.</span><span class="sxs-lookup"><span data-stu-id="19dcf-275">For example, MVC passes an instance of [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) in the `Resource` property.</span></span> <span data-ttu-id="19dcf-276">Esta propiedad proporciona acceso a `HttpContext`, `RouteData`y todo lo demás proporcionado por MVC y Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="19dcf-276">This property provides access to `HttpContext`, `RouteData`, and everything else provided by MVC and Razor Pages.</span></span>

<span data-ttu-id="19dcf-277">El uso de la propiedad `Resource` es específico del marco de trabajo.</span><span class="sxs-lookup"><span data-stu-id="19dcf-277">The use of the `Resource` property is framework specific.</span></span> <span data-ttu-id="19dcf-278">El uso de la información en la propiedad `Resource` limita las directivas de autorización a marcos concretos.</span><span class="sxs-lookup"><span data-stu-id="19dcf-278">Using information in the `Resource` property limits your authorization policies to particular frameworks.</span></span> <span data-ttu-id="19dcf-279">Debe convertir la propiedad `Resource` mediante la palabra clave `is` y, a continuación, confirmar que la conversión se ha realizado correctamente para asegurarse de que el código no se bloquea con un `InvalidCastException` cuando se ejecuta en otros marcos de trabajo:</span><span class="sxs-lookup"><span data-stu-id="19dcf-279">You should cast the `Resource` property using the `is` keyword, and then confirm the cast has succeeded to ensure your code doesn't crash with an `InvalidCastException` when run on other frameworks:</span></span>

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```

::: moniker-end