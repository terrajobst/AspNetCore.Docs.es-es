---
title: "Autorización basada en recursos en ASP.NET Core"
author: scottaddie
description: "Aprenda a implementar la autorización basada en recursos en una aplicación de ASP.NET Core cuando un atributo Authorize no son suficientes."
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 11/07/2017
ms.devlang: csharp
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/resourcebased
ms.openlocfilehash: 708f306da740870b106cbeeb96879480f8745439
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
# <a name="resource-based-authorization"></a><span data-ttu-id="e3cfa-103">Autorización basada en recursos</span><span class="sxs-lookup"><span data-stu-id="e3cfa-103">Resource-based authorization</span></span>

<span data-ttu-id="e3cfa-104">Por [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="e3cfa-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="e3cfa-105">Estrategia de autorización depende de los recursos que se obtiene acceso.</span><span class="sxs-lookup"><span data-stu-id="e3cfa-105">Authorization strategy depends upon the resource being accessed.</span></span> <span data-ttu-id="e3cfa-106">Considere la posibilidad de un documento que tiene una propiedad de autor.</span><span class="sxs-lookup"><span data-stu-id="e3cfa-106">Consider a document which has an author property.</span></span> <span data-ttu-id="e3cfa-107">Solo el autor tiene permiso para actualizar el documento.</span><span class="sxs-lookup"><span data-stu-id="e3cfa-107">Only the author is allowed to update the document.</span></span> <span data-ttu-id="e3cfa-108">Por lo tanto, el documento se debe recuperar desde el almacén de datos antes de que puede producirse la evaluación de autorización.</span><span class="sxs-lookup"><span data-stu-id="e3cfa-108">Consequently, the document must be retrieved from the data store before authorization evaluation can occur.</span></span>

<span data-ttu-id="e3cfa-109">Evaluación de atributo se produce antes del enlace de datos y antes de la ejecución del controlador de la página o acción que carga el documento.</span><span class="sxs-lookup"><span data-stu-id="e3cfa-109">Attribute evaluation occurs before data binding and before execution of the page handler or action which loads the document.</span></span> <span data-ttu-id="e3cfa-110">Por estos motivos, autorización declarativa con un `[Authorize]` atributo no son suficientes.</span><span class="sxs-lookup"><span data-stu-id="e3cfa-110">For these reasons, declarative authorization with an `[Authorize]` attribute won't suffice.</span></span> <span data-ttu-id="e3cfa-111">En su lugar, puede invocar un método de autorización personalizado&mdash;un estilo que se conoce como autorización imperativa.</span><span class="sxs-lookup"><span data-stu-id="e3cfa-111">Instead, you can invoke a custom authorization method&mdash;a style known as imperative authorization.</span></span>

<span data-ttu-id="e3cfa-112">Use la [aplicaciones de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([cómo descargar](xref:tutorials/index#how-to-download-a-sample)) para explorar las características descritas en este tema.</span><span class="sxs-lookup"><span data-stu-id="e3cfa-112">Use the [sample apps](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample)) to explore the features described in this topic.</span></span>

## <a name="use-imperative-authorization"></a><span data-ttu-id="e3cfa-113">Utilizar la autorización imperativa</span><span class="sxs-lookup"><span data-stu-id="e3cfa-113">Use imperative authorization</span></span>

<span data-ttu-id="e3cfa-114">Autorización se implementa como un [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) de servicio y está registrado en la colección de servicio en la `Startup` clase.</span><span class="sxs-lookup"><span data-stu-id="e3cfa-114">Authorization is implemented as an [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) service and is registered in the service collection within the `Startup` class.</span></span> <span data-ttu-id="e3cfa-115">El servicio debe ponerse a disposición a través de [inyección de dependencia](xref:fundamentals/dependency-injection#fundamentals-dependency-injection) a controladores de página o acciones.</span><span class="sxs-lookup"><span data-stu-id="e3cfa-115">The service is made available via [dependency injection](xref:fundamentals/dependency-injection#fundamentals-dependency-injection) to page handlers or actions.</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Controllers/DocumentController.cs?name=snippet_IAuthServiceDI&highlight=6)]

<span data-ttu-id="e3cfa-116">`IAuthorizationService`tiene dos `AuthorizeAsync` sobrecargas del método: acepta un recurso y, a continuación, el nombre de directiva y el otro aceptando el recurso y una lista de requisitos para evaluar.</span><span class="sxs-lookup"><span data-stu-id="e3cfa-116">`IAuthorizationService` has two `AuthorizeAsync` method overloads: one accepting the resource and the policy name and the other accepting the resource and a list of requirements to evaluate.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e3cfa-117">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e3cfa-117">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e3cfa-118">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e3cfa-118">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

---

<a name="security-authorization-resource-based-imperative"></a>

<span data-ttu-id="e3cfa-119">En el ejemplo siguiente, se carga el recurso que se va a proteger en un personalizado `Document` objeto.</span><span class="sxs-lookup"><span data-stu-id="e3cfa-119">In the following example, the resource to be secured is loaded into a custom `Document` object.</span></span> <span data-ttu-id="e3cfa-120">Un `AuthorizeAsync` sobrecarga se invoca para determinar si se permite al usuario actual para modificar el documento proporcionado.</span><span class="sxs-lookup"><span data-stu-id="e3cfa-120">An `AuthorizeAsync` overload is invoked to determine whether the current user is allowed to edit the provided document.</span></span> <span data-ttu-id="e3cfa-121">Una directiva de autorización personalizada "EditPolicy" es tenerse en cuenta en la decisión.</span><span class="sxs-lookup"><span data-stu-id="e3cfa-121">A custom "EditPolicy" authorization policy is factored into the decision.</span></span> <span data-ttu-id="e3cfa-122">Vea [autorización basada en directivas de personalizado](xref:security/authorization/policies) para obtener más información acerca de cómo crear directivas de autorización.</span><span class="sxs-lookup"><span data-stu-id="e3cfa-122">See [Custom policy-based authorization](xref:security/authorization/policies) for more on creating authorization policies.</span></span>

> [!NOTE]
> <span data-ttu-id="e3cfa-123">El código siguiente ejemplos se supone la autenticación se ha ejecutado y establezca el `User` propiedad.</span><span class="sxs-lookup"><span data-stu-id="e3cfa-123">The following code samples assume authentication has run and set the `User` property.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e3cfa-124">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e3cfa-124">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/Edit.cshtml.cs?name=snippet_DocumentEditHandler)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e3cfa-125">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e3cfa-125">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentEditAction)]

---

## <a name="write-a-resource-based-handler"></a><span data-ttu-id="e3cfa-126">Escribir un controlador basada en recursos</span><span class="sxs-lookup"><span data-stu-id="e3cfa-126">Write a resource-based handler</span></span>

<span data-ttu-id="e3cfa-127">Escribir un controlador para la autorización basada en recursos no es muy diferente de [escribir un controlador de requisitos sin formato](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler).</span><span class="sxs-lookup"><span data-stu-id="e3cfa-127">Writing a handler for resource-based authorization isn't much different than [writing a plain requirements handler](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler).</span></span> <span data-ttu-id="e3cfa-128">Cree una clase de requisito personalizado e implementar una clase de controlador de requisito.</span><span class="sxs-lookup"><span data-stu-id="e3cfa-128">Create a custom requirement class, and implement a requirement handler class.</span></span> <span data-ttu-id="e3cfa-129">La clase de controlador especifica el requisito y el tipo de recurso.</span><span class="sxs-lookup"><span data-stu-id="e3cfa-129">The handler class specifies both the requirement and resource type.</span></span> <span data-ttu-id="e3cfa-130">Por ejemplo, un controlador utilizando un `SameAuthorRequirement` requisito y un `Document` recurso tiene el siguiente aspecto:</span><span class="sxs-lookup"><span data-stu-id="e3cfa-130">For example, a handler utilizing a `SameAuthorRequirement` requirement and a `Document` resource looks as follows:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e3cfa-131">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e3cfa-131">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e3cfa-132">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e3cfa-132">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

---

<span data-ttu-id="e3cfa-133">Registrar el requisito y el controlador en el `Startup.ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="e3cfa-133">Register the requirement and handler in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Startup.cs?name=snippet_ConfigureServicesSample&highlight=3-7,9)]

### <a name="operational-requirements"></a><span data-ttu-id="e3cfa-134">Requisitos operativos</span><span class="sxs-lookup"><span data-stu-id="e3cfa-134">Operational requirements</span></span>

<span data-ttu-id="e3cfa-135">Si está realizando decisiones basadas en los resultados de CRUD (**C**rear, **R**EA, **U**ctualizar, **d.**liminar) de las operaciones, utilice la [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) clase auxiliar.</span><span class="sxs-lookup"><span data-stu-id="e3cfa-135">If you're making decisions based on the outcomes of CRUD (**C**reate, **R**ead, **U**pdate, **D**elete) operations, use the [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) helper class.</span></span> <span data-ttu-id="e3cfa-136">Esta clase permite escribir un único controlador en lugar de una clase individual para cada tipo de operación.</span><span class="sxs-lookup"><span data-stu-id="e3cfa-136">This class enables you to write a single handler instead of an individual class for each operation type.</span></span> <span data-ttu-id="e3cfa-137">Para usarla, proporcionar algunos nombres de operación:</span><span class="sxs-lookup"><span data-stu-id="e3cfa-137">To use it, provide some operation names:</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_OperationsClass)]

<span data-ttu-id="e3cfa-138">El controlador se implementa como sigue, mediante una `OperationAuthorizationRequirement` requisito y un `Document` recursos:</span><span class="sxs-lookup"><span data-stu-id="e3cfa-138">The handler is implemented as follows, using an `OperationAuthorizationRequirement` requirement and a `Document` resource:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e3cfa-139">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e3cfa-139">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e3cfa-140">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e3cfa-140">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

---

<span data-ttu-id="e3cfa-141">El controlador anterior valida la operación con el recurso, la identidad del usuario y el requisito `Name` propiedad.</span><span class="sxs-lookup"><span data-stu-id="e3cfa-141">The preceding handler validates the operation using the resource, the user's identity, and the requirement's `Name` property.</span></span>

<span data-ttu-id="e3cfa-142">Para llamar a un controlador de recursos operativa, especifique la operación al invocar `AuthorizeAsync` en el controlador de la página o la acción.</span><span class="sxs-lookup"><span data-stu-id="e3cfa-142">To call an operational resource handler, specify the operation when invoking `AuthorizeAsync` in your page handler or action.</span></span> <span data-ttu-id="e3cfa-143">En el ejemplo siguiente se determina si se permite al usuario autenticado para ver el documento proporcionado.</span><span class="sxs-lookup"><span data-stu-id="e3cfa-143">The following example determines whether the authenticated user is permitted to view the provided document.</span></span>

> [!NOTE]
> <span data-ttu-id="e3cfa-144">El código siguiente ejemplos se supone la autenticación se ha ejecutado y establezca el `User` propiedad.</span><span class="sxs-lookup"><span data-stu-id="e3cfa-144">The following code samples assume authentication has run and set the `User` property.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e3cfa-145">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e3cfa-145">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/View.cshtml.cs?name=snippet_DocumentViewHandler&highlight=10-11)]

<span data-ttu-id="e3cfa-146">Si la autorización se realiza correctamente, se devuelve la página para ver el documento.</span><span class="sxs-lookup"><span data-stu-id="e3cfa-146">If authorization succeeds, the page for viewing the document is returned.</span></span> <span data-ttu-id="e3cfa-147">Si se produce un error de autorización, pero el usuario se autentica, devolver `ForbidResult` informa a cualquier middleware de autenticación error de autorización.</span><span class="sxs-lookup"><span data-stu-id="e3cfa-147">If authorization fails but the user is authenticated, returning `ForbidResult` informs any authentication middleware that authorization failed.</span></span> <span data-ttu-id="e3cfa-148">Un `ChallengeResult` se devuelve cuando se debe realizar la autenticación.</span><span class="sxs-lookup"><span data-stu-id="e3cfa-148">A `ChallengeResult` is returned when authentication must be performed.</span></span> <span data-ttu-id="e3cfa-149">Para los clientes de explorador interactivo, puede ser adecuado redirigir al usuario a una página de inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="e3cfa-149">For interactive browser clients, it may be appropriate to redirect the user to a login page.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e3cfa-150">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e3cfa-150">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentViewAction&highlight=11-12)]

<span data-ttu-id="e3cfa-151">Si la autorización se realiza correctamente, se devuelve la vista del documento.</span><span class="sxs-lookup"><span data-stu-id="e3cfa-151">If authorization succeeds, the view for the document is returned.</span></span> <span data-ttu-id="e3cfa-152">Si se produce un error en la autorización, devolver `ChallengeResult` informa a cualquier middleware de autenticación que error de autorización, y el software intermedio puede tomar la respuesta adecuada.</span><span class="sxs-lookup"><span data-stu-id="e3cfa-152">If authorization fails, returning `ChallengeResult` informs any authentication middleware that authorization failed, and the middleware can take the appropriate response.</span></span> <span data-ttu-id="e3cfa-153">Una respuesta adecuada podría devolver un código de estado 401 o 403.</span><span class="sxs-lookup"><span data-stu-id="e3cfa-153">An appropriate response could be returning a 401 or 403 status code.</span></span> <span data-ttu-id="e3cfa-154">Para los clientes de explorador interactivo, podría significar redirige al usuario a una página de inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="e3cfa-154">For interactive browser clients, it could mean redirecting the user to a login page.</span></span>

---
