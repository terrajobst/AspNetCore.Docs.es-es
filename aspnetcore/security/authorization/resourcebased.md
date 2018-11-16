---
title: Autorización basada en recursos en ASP.NET Core
author: scottaddie
description: Obtenga información sobre cómo implementar la autorización basada en recursos en una aplicación ASP.NET Core cuando un atributo Authorize no es suficiente.
ms.author: scaddie
ms.custom: mvc
ms.date: 11/15/2018
uid: security/authorization/resourcebased
ms.openlocfilehash: 6a163caa26b277fbee6b9d61f8f1d16a60c75b03
ms.sourcegitcommit: d3392f688cfebc1f25616da7489664d69c6ee330
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/16/2018
ms.locfileid: "51818374"
---
# <a name="resource-based-authorization-in-aspnet-core"></a><span data-ttu-id="0a232-103">Autorización basada en recursos en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0a232-103">Resource-based authorization in ASP.NET Core</span></span>

<span data-ttu-id="0a232-104">Estrategia de autorización depende de los recursos que se obtiene acceso.</span><span class="sxs-lookup"><span data-stu-id="0a232-104">Authorization strategy depends upon the resource being accessed.</span></span> <span data-ttu-id="0a232-105">Considere la posibilidad de un documento que tiene una propiedad del autor.</span><span class="sxs-lookup"><span data-stu-id="0a232-105">Consider a document that has an author property.</span></span> <span data-ttu-id="0a232-106">Solo el autor tiene permiso para actualizar el documento.</span><span class="sxs-lookup"><span data-stu-id="0a232-106">Only the author is allowed to update the document.</span></span> <span data-ttu-id="0a232-107">Por lo tanto, el documento se debe recuperar del almacén de datos antes de que comience la evaluación de autorización.</span><span class="sxs-lookup"><span data-stu-id="0a232-107">Consequently, the document must be retrieved from the data store before authorization evaluation can occur.</span></span>

<span data-ttu-id="0a232-108">Evaluación de atributos se produce antes del enlace de datos y antes de la ejecución del controlador de página o acción que se carga el documento.</span><span class="sxs-lookup"><span data-stu-id="0a232-108">Attribute evaluation occurs before data binding and before execution of the page handler or action that loads the document.</span></span> <span data-ttu-id="0a232-109">Por estos motivos, la autorización declarativa con un `[Authorize]` atributo no es suficiente.</span><span class="sxs-lookup"><span data-stu-id="0a232-109">For these reasons, declarative authorization with an `[Authorize]` attribute doesn't suffice.</span></span> <span data-ttu-id="0a232-110">En su lugar, puede invocar un método de autorización personalizado&mdash;un estilo que se conoce como *autorización imperativa*.</span><span class="sxs-lookup"><span data-stu-id="0a232-110">Instead, you can invoke a custom authorization method&mdash;a style known as *imperative authorization*.</span></span>

<span data-ttu-id="0a232-111">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="0a232-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="0a232-112">[Crear una aplicación ASP.NET Core con datos de usuario protegidos por autorización](xref:security/authorization/secure-data) contiene una aplicación de ejemplo que utiliza la autorización basada en recursos.</span><span class="sxs-lookup"><span data-stu-id="0a232-112">[Create an ASP.NET Core app with user data protected by authorization](xref:security/authorization/secure-data) contains a sample app that uses resource-based authorization.</span></span>

## <a name="use-imperative-authorization"></a><span data-ttu-id="0a232-113">Usar la autorización imperativa</span><span class="sxs-lookup"><span data-stu-id="0a232-113">Use imperative authorization</span></span>

<span data-ttu-id="0a232-114">Autorización se implementa como un [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) de servicio y se registra en la colección de servicios dentro de la `Startup` clase.</span><span class="sxs-lookup"><span data-stu-id="0a232-114">Authorization is implemented as an [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) service and is registered in the service collection within the `Startup` class.</span></span> <span data-ttu-id="0a232-115">El servicio está disponible a través de [inserción de dependencias](xref:fundamentals/dependency-injection) a los controladores de página o acciones.</span><span class="sxs-lookup"><span data-stu-id="0a232-115">The service is made available via [dependency injection](xref:fundamentals/dependency-injection) to page handlers or actions.</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Controllers/DocumentController.cs?name=snippet_IAuthServiceDI&highlight=6)]

<span data-ttu-id="0a232-116">`IAuthorizationService` tiene dos `AuthorizeAsync` sobrecargas del método: una acepta el recurso y el nombre de la directiva y la otra acepta el recurso y una lista de requisitos para evaluar.</span><span class="sxs-lookup"><span data-stu-id="0a232-116">`IAuthorizationService` has two `AuthorizeAsync` method overloads: one accepting the resource and the policy name and the other accepting the resource and a list of requirements to evaluate.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

```csharp
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

::: moniker-end

<a name="security-authorization-resource-based-imperative"></a>

<span data-ttu-id="0a232-117">En el ejemplo siguiente, se carga el recurso se proteja en personalizada `Document` objeto.</span><span class="sxs-lookup"><span data-stu-id="0a232-117">In the following example, the resource to be secured is loaded into a custom `Document` object.</span></span> <span data-ttu-id="0a232-118">Un `AuthorizeAsync` sobrecarga se invoca para determinar si el usuario actual está autorizado para editar el documento proporcionado.</span><span class="sxs-lookup"><span data-stu-id="0a232-118">An `AuthorizeAsync` overload is invoked to determine whether the current user is allowed to edit the provided document.</span></span> <span data-ttu-id="0a232-119">Una directiva de autorización personalizada "EditPolicy" se incluye en la decisión.</span><span class="sxs-lookup"><span data-stu-id="0a232-119">A custom "EditPolicy" authorization policy is factored into the decision.</span></span> <span data-ttu-id="0a232-120">Consulte [autorización basada en directivas de Custom](xref:security/authorization/policies) para obtener más información sobre la creación de directivas de autorización.</span><span class="sxs-lookup"><span data-stu-id="0a232-120">See [Custom policy-based authorization](xref:security/authorization/policies) for more on creating authorization policies.</span></span>

> [!NOTE]
> <span data-ttu-id="0a232-121">El código siguiente, los ejemplos se supone que la autenticación se ha ejecutado y establezca el `User` propiedad.</span><span class="sxs-lookup"><span data-stu-id="0a232-121">The following code samples assume authentication has run and set the `User` property.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/Edit.cshtml.cs?name=snippet_DocumentEditHandler)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentEditAction)]

::: moniker-end

## <a name="write-a-resource-based-handler"></a><span data-ttu-id="0a232-122">Escribir un controlador de recursos</span><span class="sxs-lookup"><span data-stu-id="0a232-122">Write a resource-based handler</span></span>

<span data-ttu-id="0a232-123">Escribir un controlador para autorización basada en recursos no es mucho más diferente que [escribir un controlador simple requisitos](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler).</span><span class="sxs-lookup"><span data-stu-id="0a232-123">Writing a handler for resource-based authorization isn't much different than [writing a plain requirements handler](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler).</span></span> <span data-ttu-id="0a232-124">Cree una clase de requisito personalizado e implementar una clase de controlador de requisito.</span><span class="sxs-lookup"><span data-stu-id="0a232-124">Create a custom requirement class, and implement a requirement handler class.</span></span> <span data-ttu-id="0a232-125">Para obtener más información sobre la creación de una clase de requisito, consulte [requisitos](xref:security/authorization/policies#requirements).</span><span class="sxs-lookup"><span data-stu-id="0a232-125">For more information on creating a requirement class, see [Requirements](xref:security/authorization/policies#requirements).</span></span>

<span data-ttu-id="0a232-126">La clase de controlador especifica el requisito y el tipo de recurso.</span><span class="sxs-lookup"><span data-stu-id="0a232-126">The handler class specifies both the requirement and resource type.</span></span> <span data-ttu-id="0a232-127">Por ejemplo, un controlador utilizando un `SameAuthorRequirement` y un `Document` recursos se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="0a232-127">For example, a handler utilizing a `SameAuthorRequirement` and a `Document` resource follows:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

::: moniker-end

<span data-ttu-id="0a232-128">En el ejemplo anterior, imagine que `SameAuthorRequirement` es un caso especial de un elemento genérico más `SpecificAuthorRequirement` clase.</span><span class="sxs-lookup"><span data-stu-id="0a232-128">In the preceding example, imagine that `SameAuthorRequirement` is a special case of a more generic `SpecificAuthorRequirement` class.</span></span> <span data-ttu-id="0a232-129">El `SpecificAuthorRequirement` clase (no mostrado) contiene un `Name` propiedad que representa el nombre del autor.</span><span class="sxs-lookup"><span data-stu-id="0a232-129">The `SpecificAuthorRequirement` class (not shown) contains a `Name` property representing the name of the author.</span></span> <span data-ttu-id="0a232-130">El `Name` propiedad puede establecerse en el usuario actual.</span><span class="sxs-lookup"><span data-stu-id="0a232-130">The `Name` property could be set to the current user.</span></span>

<span data-ttu-id="0a232-131">Registrar el requisito y el controlador en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="0a232-131">Register the requirement and handler in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Startup.cs?name=snippet_ConfigureServicesSample&highlight=3-7,9)]

### <a name="operational-requirements"></a><span data-ttu-id="0a232-132">Requisitos operativos</span><span class="sxs-lookup"><span data-stu-id="0a232-132">Operational requirements</span></span>

<span data-ttu-id="0a232-133">Si va a realizar decisiones basadas en los resultados de las operaciones CRUD (creación, lectura, actualización, eliminación), use el [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) clase auxiliar.</span><span class="sxs-lookup"><span data-stu-id="0a232-133">If you're making decisions based on the outcomes of CRUD (Create, Read, Update, Delete) operations, use the [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) helper class.</span></span> <span data-ttu-id="0a232-134">Esta clase le permite escribir un único controlador en lugar de una clase individual para cada operación.</span><span class="sxs-lookup"><span data-stu-id="0a232-134">This class enables you to write a single handler instead of an individual class for each operation type.</span></span> <span data-ttu-id="0a232-135">Para ello, proporcione algunos nombres de operación:</span><span class="sxs-lookup"><span data-stu-id="0a232-135">To use it, provide some operation names:</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_OperationsClass)]

<span data-ttu-id="0a232-136">El controlador se implementa como sigue, con un `OperationAuthorizationRequirement` requisito y un `Document` recursos:</span><span class="sxs-lookup"><span data-stu-id="0a232-136">The handler is implemented as follows, using an `OperationAuthorizationRequirement` requirement and a `Document` resource:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

::: moniker-end

<span data-ttu-id="0a232-137">El controlador anterior valida la operación con el recurso, la identidad del usuario y el requisito `Name` propiedad.</span><span class="sxs-lookup"><span data-stu-id="0a232-137">The preceding handler validates the operation using the resource, the user's identity, and the requirement's `Name` property.</span></span>

<span data-ttu-id="0a232-138">Para llamar a un controlador de recursos operativos, especifique la operación al invocar `AuthorizeAsync` en su controlador de página o la acción.</span><span class="sxs-lookup"><span data-stu-id="0a232-138">To call an operational resource handler, specify the operation when invoking `AuthorizeAsync` in your page handler or action.</span></span> <span data-ttu-id="0a232-139">El ejemplo siguiente determina si el usuario autenticado tiene permiso para visualizar el documento proporcionado.</span><span class="sxs-lookup"><span data-stu-id="0a232-139">The following example determines whether the authenticated user is permitted to view the provided document.</span></span>

> [!NOTE]
> <span data-ttu-id="0a232-140">El código siguiente, los ejemplos se supone que la autenticación se ha ejecutado y establezca el `User` propiedad.</span><span class="sxs-lookup"><span data-stu-id="0a232-140">The following code samples assume authentication has run and set the `User` property.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/View.cshtml.cs?name=snippet_DocumentViewHandler&highlight=10-11)]

<span data-ttu-id="0a232-141">Si la autorización se realiza correctamente, se devuelve la página para ver el documento.</span><span class="sxs-lookup"><span data-stu-id="0a232-141">If authorization succeeds, the page for viewing the document is returned.</span></span> <span data-ttu-id="0a232-142">Si se produce un error de autorización, pero el usuario está autenticado, devolver `ForbidResult` informa a cualquier middleware de autenticación con errores de autorización.</span><span class="sxs-lookup"><span data-stu-id="0a232-142">If authorization fails but the user is authenticated, returning `ForbidResult` informs any authentication middleware that authorization failed.</span></span> <span data-ttu-id="0a232-143">Un `ChallengeResult` se devuelve cuando se debe realizar la autenticación.</span><span class="sxs-lookup"><span data-stu-id="0a232-143">A `ChallengeResult` is returned when authentication must be performed.</span></span> <span data-ttu-id="0a232-144">Para los clientes de explorador interactivo, puede ser adecuado redirigir al usuario a una página de inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="0a232-144">For interactive browser clients, it may be appropriate to redirect the user to a login page.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentViewAction&highlight=11-12)]

<span data-ttu-id="0a232-145">Si la autorización se realiza correctamente, se devuelve la vista del documento.</span><span class="sxs-lookup"><span data-stu-id="0a232-145">If authorization succeeds, the view for the document is returned.</span></span> <span data-ttu-id="0a232-146">Si se produce un error de autorización, devolver `ChallengeResult` informa a cualquier middleware de autenticación que error de autorización, y el software intermedio puede tomar la respuesta adecuada.</span><span class="sxs-lookup"><span data-stu-id="0a232-146">If authorization fails, returning `ChallengeResult` informs any authentication middleware that authorization failed, and the middleware can take the appropriate response.</span></span> <span data-ttu-id="0a232-147">Una respuesta adecuada podría devolver un código de estado 401 o 403.</span><span class="sxs-lookup"><span data-stu-id="0a232-147">An appropriate response could be returning a 401 or 403 status code.</span></span> <span data-ttu-id="0a232-148">Para los clientes de explorador interactivo, podría significar redirigir al usuario a una página de inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="0a232-148">For interactive browser clients, it could mean redirecting the user to a login page.</span></span>

::: moniker-end
