---
title: Autorización basada en recursos en ASP.NET Core
author: scottaddie
description: Obtenga información sobre cómo implementar la autorización basada en recursos en una aplicación ASP.NET Core cuando un atributo Authorize no es suficiente.
ms.author: scaddie
ms.custom: mvc
ms.date: 11/07/2017
uid: security/authorization/resourcebased
ms.openlocfilehash: 6a110a69c58d5e20a15198378510486daec3d452
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342294"
---
# <a name="resource-based-authorization-in-aspnet-core"></a><span data-ttu-id="b490a-103">Autorización basada en recursos en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b490a-103">Resource-based authorization in ASP.NET Core</span></span>

<span data-ttu-id="b490a-104">Estrategia de autorización depende de los recursos que se obtiene acceso.</span><span class="sxs-lookup"><span data-stu-id="b490a-104">Authorization strategy depends upon the resource being accessed.</span></span> <span data-ttu-id="b490a-105">Considere la posibilidad de un documento que tiene una propiedad del autor.</span><span class="sxs-lookup"><span data-stu-id="b490a-105">Consider a document which has an author property.</span></span> <span data-ttu-id="b490a-106">Solo el autor tiene permiso para actualizar el documento.</span><span class="sxs-lookup"><span data-stu-id="b490a-106">Only the author is allowed to update the document.</span></span> <span data-ttu-id="b490a-107">Por lo tanto, el documento se debe recuperar del almacén de datos antes de que comience la evaluación de autorización.</span><span class="sxs-lookup"><span data-stu-id="b490a-107">Consequently, the document must be retrieved from the data store before authorization evaluation can occur.</span></span>

<span data-ttu-id="b490a-108">Evaluación de atributos se produce antes del enlace de datos y antes de la ejecución del controlador de página o acción que se carga el documento.</span><span class="sxs-lookup"><span data-stu-id="b490a-108">Attribute evaluation occurs before data binding and before execution of the page handler or action which loads the document.</span></span> <span data-ttu-id="b490a-109">Por estos motivos, la autorización declarativa con un `[Authorize]` atributo no es suficiente.</span><span class="sxs-lookup"><span data-stu-id="b490a-109">For these reasons, declarative authorization with an `[Authorize]` attribute won't suffice.</span></span> <span data-ttu-id="b490a-110">En su lugar, puede invocar un método de autorización personalizado&mdash;un estilo que se conoce como autorización imperativa.</span><span class="sxs-lookup"><span data-stu-id="b490a-110">Instead, you can invoke a custom authorization method&mdash;a style known as imperative authorization.</span></span>

<span data-ttu-id="b490a-111">Use la [aplicaciones de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([descarga](xref:tutorials/index#how-to-download-a-sample)) para explorar las características descritas en este tema.</span><span class="sxs-lookup"><span data-stu-id="b490a-111">Use the [sample apps](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample)) to explore the features described in this topic.</span></span>

<span data-ttu-id="b490a-112">[Crear una aplicación ASP.NET Core con datos de usuario protegidos por autorización](xref:security/authorization/secure-data) contiene una aplicación de ejemplo que utiliza la autorización basada en recursos.</span><span class="sxs-lookup"><span data-stu-id="b490a-112">[Create an ASP.NET Core app with user data protected by authorization](xref:security/authorization/secure-data) contains a sample app that uses resource-based authorization.</span></span>

## <a name="use-imperative-authorization"></a><span data-ttu-id="b490a-113">Usar la autorización imperativa</span><span class="sxs-lookup"><span data-stu-id="b490a-113">Use imperative authorization</span></span>

<span data-ttu-id="b490a-114">Autorización se implementa como un [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) de servicio y se registra en la colección de servicios dentro de la `Startup` clase.</span><span class="sxs-lookup"><span data-stu-id="b490a-114">Authorization is implemented as an [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) service and is registered in the service collection within the `Startup` class.</span></span> <span data-ttu-id="b490a-115">El servicio está disponible a través de [inserción de dependencias](xref:fundamentals/dependency-injection) a los controladores de página o acciones.</span><span class="sxs-lookup"><span data-stu-id="b490a-115">The service is made available via [dependency injection](xref:fundamentals/dependency-injection) to page handlers or actions.</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Controllers/DocumentController.cs?name=snippet_IAuthServiceDI&highlight=6)]

<span data-ttu-id="b490a-116">`IAuthorizationService` tiene dos `AuthorizeAsync` sobrecargas del método: una acepta el recurso y el nombre de la directiva y la otra acepta el recurso y una lista de requisitos para evaluar.</span><span class="sxs-lookup"><span data-stu-id="b490a-116">`IAuthorizationService` has two `AuthorizeAsync` method overloads: one accepting the resource and the policy name and the other accepting the resource and a list of requirements to evaluate.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b490a-117">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b490a-117">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b490a-118">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b490a-118">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

<span data-ttu-id="b490a-119">En el ejemplo siguiente, se carga el recurso se proteja en personalizada `Document` objeto.</span><span class="sxs-lookup"><span data-stu-id="b490a-119">In the following example, the resource to be secured is loaded into a custom `Document` object.</span></span> <span data-ttu-id="b490a-120">Un `AuthorizeAsync` sobrecarga se invoca para determinar si el usuario actual está autorizado para editar el documento proporcionado.</span><span class="sxs-lookup"><span data-stu-id="b490a-120">An `AuthorizeAsync` overload is invoked to determine whether the current user is allowed to edit the provided document.</span></span> <span data-ttu-id="b490a-121">Una directiva de autorización personalizada "EditPolicy" se incluye en la decisión.</span><span class="sxs-lookup"><span data-stu-id="b490a-121">A custom "EditPolicy" authorization policy is factored into the decision.</span></span> <span data-ttu-id="b490a-122">Consulte [autorización basada en directivas de Custom](xref:security/authorization/policies) para obtener más información sobre la creación de directivas de autorización.</span><span class="sxs-lookup"><span data-stu-id="b490a-122">See [Custom policy-based authorization](xref:security/authorization/policies) for more on creating authorization policies.</span></span>

> [!NOTE]
> <span data-ttu-id="b490a-123">El código siguiente, los ejemplos se supone que la autenticación se ha ejecutado y establezca el `User` propiedad.</span><span class="sxs-lookup"><span data-stu-id="b490a-123">The following code samples assume authentication has run and set the `User` property.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b490a-124">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b490a-124">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/Edit.cshtml.cs?name=snippet_DocumentEditHandler)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b490a-125">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b490a-125">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentEditAction)]

---

## <a name="write-a-resource-based-handler"></a><span data-ttu-id="b490a-126">Escribir un controlador de recursos</span><span class="sxs-lookup"><span data-stu-id="b490a-126">Write a resource-based handler</span></span>

<span data-ttu-id="b490a-127">Escribir un controlador para autorización basada en recursos no es mucho más diferente que [escribir un controlador simple requisitos](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler).</span><span class="sxs-lookup"><span data-stu-id="b490a-127">Writing a handler for resource-based authorization isn't much different than [writing a plain requirements handler](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler).</span></span> <span data-ttu-id="b490a-128">Cree una clase de requisito personalizado e implementar una clase de controlador de requisito.</span><span class="sxs-lookup"><span data-stu-id="b490a-128">Create a custom requirement class, and implement a requirement handler class.</span></span> <span data-ttu-id="b490a-129">La clase de controlador especifica el requisito y el tipo de recurso.</span><span class="sxs-lookup"><span data-stu-id="b490a-129">The handler class specifies both the requirement and resource type.</span></span> <span data-ttu-id="b490a-130">Por ejemplo, un controlador utilizando un `SameAuthorRequirement` requisito y un `Document` recurso tiene el aspecto siguiente:</span><span class="sxs-lookup"><span data-stu-id="b490a-130">For example, a handler utilizing a `SameAuthorRequirement` requirement and a `Document` resource looks as follows:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b490a-131">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b490a-131">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b490a-132">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b490a-132">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

---

<span data-ttu-id="b490a-133">Registrar el requisito y el controlador en el `Startup.ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="b490a-133">Register the requirement and handler in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Startup.cs?name=snippet_ConfigureServicesSample&highlight=3-7,9)]

### <a name="operational-requirements"></a><span data-ttu-id="b490a-134">Requisitos operativos</span><span class="sxs-lookup"><span data-stu-id="b490a-134">Operational requirements</span></span>

<span data-ttu-id="b490a-135">Si va a realizar decisiones basadas en los resultados de las operaciones CRUD (creación, lectura, actualización, eliminación), use el [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) clase auxiliar.</span><span class="sxs-lookup"><span data-stu-id="b490a-135">If you're making decisions based on the outcomes of CRUD (Create, Read, Update, Delete) operations, use the [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) helper class.</span></span> <span data-ttu-id="b490a-136">Esta clase le permite escribir un único controlador en lugar de una clase individual para cada operación.</span><span class="sxs-lookup"><span data-stu-id="b490a-136">This class enables you to write a single handler instead of an individual class for each operation type.</span></span> <span data-ttu-id="b490a-137">Para ello, proporcione algunos nombres de operación:</span><span class="sxs-lookup"><span data-stu-id="b490a-137">To use it, provide some operation names:</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_OperationsClass)]

<span data-ttu-id="b490a-138">El controlador se implementa como sigue, con un `OperationAuthorizationRequirement` requisito y un `Document` recursos:</span><span class="sxs-lookup"><span data-stu-id="b490a-138">The handler is implemented as follows, using an `OperationAuthorizationRequirement` requirement and a `Document` resource:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b490a-139">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b490a-139">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b490a-140">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b490a-140">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

---

<span data-ttu-id="b490a-141">El controlador anterior valida la operación con el recurso, la identidad del usuario y el requisito `Name` propiedad.</span><span class="sxs-lookup"><span data-stu-id="b490a-141">The preceding handler validates the operation using the resource, the user's identity, and the requirement's `Name` property.</span></span>

<span data-ttu-id="b490a-142">Para llamar a un controlador de recursos operativos, especifique la operación al invocar `AuthorizeAsync` en su controlador de página o la acción.</span><span class="sxs-lookup"><span data-stu-id="b490a-142">To call an operational resource handler, specify the operation when invoking `AuthorizeAsync` in your page handler or action.</span></span> <span data-ttu-id="b490a-143">El ejemplo siguiente determina si el usuario autenticado tiene permiso para visualizar el documento proporcionado.</span><span class="sxs-lookup"><span data-stu-id="b490a-143">The following example determines whether the authenticated user is permitted to view the provided document.</span></span>

> [!NOTE]
> <span data-ttu-id="b490a-144">El código siguiente, los ejemplos se supone que la autenticación se ha ejecutado y establezca el `User` propiedad.</span><span class="sxs-lookup"><span data-stu-id="b490a-144">The following code samples assume authentication has run and set the `User` property.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b490a-145">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b490a-145">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/View.cshtml.cs?name=snippet_DocumentViewHandler&highlight=10-11)]

<span data-ttu-id="b490a-146">Si la autorización se realiza correctamente, se devuelve la página para ver el documento.</span><span class="sxs-lookup"><span data-stu-id="b490a-146">If authorization succeeds, the page for viewing the document is returned.</span></span> <span data-ttu-id="b490a-147">Si se produce un error de autorización, pero el usuario está autenticado, devolver `ForbidResult` informa a cualquier middleware de autenticación con errores de autorización.</span><span class="sxs-lookup"><span data-stu-id="b490a-147">If authorization fails but the user is authenticated, returning `ForbidResult` informs any authentication middleware that authorization failed.</span></span> <span data-ttu-id="b490a-148">Un `ChallengeResult` se devuelve cuando se debe realizar la autenticación.</span><span class="sxs-lookup"><span data-stu-id="b490a-148">A `ChallengeResult` is returned when authentication must be performed.</span></span> <span data-ttu-id="b490a-149">Para los clientes de explorador interactivo, puede ser adecuado redirigir al usuario a una página de inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="b490a-149">For interactive browser clients, it may be appropriate to redirect the user to a login page.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b490a-150">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b490a-150">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentViewAction&highlight=11-12)]

<span data-ttu-id="b490a-151">Si la autorización se realiza correctamente, se devuelve la vista del documento.</span><span class="sxs-lookup"><span data-stu-id="b490a-151">If authorization succeeds, the view for the document is returned.</span></span> <span data-ttu-id="b490a-152">Si se produce un error de autorización, devolver `ChallengeResult` informa a cualquier middleware de autenticación que error de autorización, y el software intermedio puede tomar la respuesta adecuada.</span><span class="sxs-lookup"><span data-stu-id="b490a-152">If authorization fails, returning `ChallengeResult` informs any authentication middleware that authorization failed, and the middleware can take the appropriate response.</span></span> <span data-ttu-id="b490a-153">Una respuesta adecuada podría devolver un código de estado 401 o 403.</span><span class="sxs-lookup"><span data-stu-id="b490a-153">An appropriate response could be returning a 401 or 403 status code.</span></span> <span data-ttu-id="b490a-154">Para los clientes de explorador interactivo, podría significar redirigir al usuario a una página de inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="b490a-154">For interactive browser clients, it could mean redirecting the user to a login page.</span></span>

---
