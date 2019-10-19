---
title: Autorización basada en recursos en ASP.NET Core
author: scottaddie
description: Obtenga información sobre cómo implementar la autorización basada en recursos en una aplicación ASP.NET Core cuando un atributo Authorize no sea suficiente.
ms.author: scaddie
ms.custom: mvc
ms.date: 11/15/2018
uid: security/authorization/resourcebased
ms.openlocfilehash: 835592521c714e270595e1448ae6e0aed1707b77
ms.sourcegitcommit: a166291c6708f5949c417874108332856b53b6a9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/18/2019
ms.locfileid: "72590003"
---
# <a name="resource-based-authorization-in-aspnet-core"></a><span data-ttu-id="9c240-103">Autorización basada en recursos en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9c240-103">Resource-based authorization in ASP.NET Core</span></span>

<span data-ttu-id="9c240-104">La estrategia de autorización depende del recurso al que se tiene acceso.</span><span class="sxs-lookup"><span data-stu-id="9c240-104">Authorization strategy depends upon the resource being accessed.</span></span> <span data-ttu-id="9c240-105">Considere un documento que tiene una propiedad Author.</span><span class="sxs-lookup"><span data-stu-id="9c240-105">Consider a document that has an author property.</span></span> <span data-ttu-id="9c240-106">Solo el autor puede actualizar el documento.</span><span class="sxs-lookup"><span data-stu-id="9c240-106">Only the author is allowed to update the document.</span></span> <span data-ttu-id="9c240-107">Por consiguiente, el documento se debe recuperar del almacén de datos antes de que se pueda realizar la evaluación de la autorización.</span><span class="sxs-lookup"><span data-stu-id="9c240-107">Consequently, the document must be retrieved from the data store before authorization evaluation can occur.</span></span>

<span data-ttu-id="9c240-108">La evaluación de atributos se produce antes del enlace de datos y antes de la ejecución del controlador de páginas o la acción que carga el documento.</span><span class="sxs-lookup"><span data-stu-id="9c240-108">Attribute evaluation occurs before data binding and before execution of the page handler or action that loads the document.</span></span> <span data-ttu-id="9c240-109">Por estos motivos, la autorización declarativa con un atributo `[Authorize]` no es suficiente.</span><span class="sxs-lookup"><span data-stu-id="9c240-109">For these reasons, declarative authorization with an `[Authorize]` attribute doesn't suffice.</span></span> <span data-ttu-id="9c240-110">En su lugar, puede invocar un método de autorización personalizado &mdash;a estilo conocido como *autorización imperativa*.</span><span class="sxs-lookup"><span data-stu-id="9c240-110">Instead, you can invoke a custom authorization method&mdash;a style known as *imperative authorization*.</span></span>

<span data-ttu-id="9c240-111">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="9c240-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="9c240-112">[Creación de una aplicación ASP.net Core con datos de usuario protegidos por la autorización](xref:security/authorization/secure-data) contiene una aplicación de ejemplo que usa la autorización basada en recursos.</span><span class="sxs-lookup"><span data-stu-id="9c240-112">[Create an ASP.NET Core app with user data protected by authorization](xref:security/authorization/secure-data) contains a sample app that uses resource-based authorization.</span></span>

## <a name="use-imperative-authorization"></a><span data-ttu-id="9c240-113">Usar autorización imperativa</span><span class="sxs-lookup"><span data-stu-id="9c240-113">Use imperative authorization</span></span>

<span data-ttu-id="9c240-114">La autorización se implementa como un servicio [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) y se registra en la colección de servicios dentro de la clase `Startup`.</span><span class="sxs-lookup"><span data-stu-id="9c240-114">Authorization is implemented as an [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) service and is registered in the service collection within the `Startup` class.</span></span> <span data-ttu-id="9c240-115">El servicio está disponible a través de la [inserción de dependencias](xref:fundamentals/dependency-injection) en los controladores o acciones de página.</span><span class="sxs-lookup"><span data-stu-id="9c240-115">The service is made available via [dependency injection](xref:fundamentals/dependency-injection) to page handlers or actions.</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Controllers/DocumentController.cs?name=snippet_IAuthServiceDI&highlight=6)]

<span data-ttu-id="9c240-116">`IAuthorizationService` tiene dos sobrecargas de método `AuthorizeAsync`: una acepta el recurso y el nombre de la Directiva, y la otra acepta el recurso y una lista de requisitos para evaluar.</span><span class="sxs-lookup"><span data-stu-id="9c240-116">`IAuthorizationService` has two `AuthorizeAsync` method overloads: one accepting the resource and the policy name and the other accepting the resource and a list of requirements to evaluate.</span></span>

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

<span data-ttu-id="9c240-117">En el ejemplo siguiente, el recurso que se va a proteger se carga en un objeto de `Document` personalizado.</span><span class="sxs-lookup"><span data-stu-id="9c240-117">In the following example, the resource to be secured is loaded into a custom `Document` object.</span></span> <span data-ttu-id="9c240-118">Se invoca una sobrecarga de `AuthorizeAsync` para determinar si el usuario actual tiene permiso para editar el documento proporcionado.</span><span class="sxs-lookup"><span data-stu-id="9c240-118">An `AuthorizeAsync` overload is invoked to determine whether the current user is allowed to edit the provided document.</span></span> <span data-ttu-id="9c240-119">Una directiva de autorización "EditPolicy" personalizada se factoriza en la decisión.</span><span class="sxs-lookup"><span data-stu-id="9c240-119">A custom "EditPolicy" authorization policy is factored into the decision.</span></span> <span data-ttu-id="9c240-120">Consulte [autorización personalizada basada en directivas](xref:security/authorization/policies) para obtener más información sobre la creación de directivas de autorización.</span><span class="sxs-lookup"><span data-stu-id="9c240-120">See [Custom policy-based authorization](xref:security/authorization/policies) for more on creating authorization policies.</span></span>

> [!NOTE]
> <span data-ttu-id="9c240-121">En los ejemplos de código siguientes se asume que la autenticación se ha ejecutado y se ha establecido la propiedad `User`.</span><span class="sxs-lookup"><span data-stu-id="9c240-121">The following code samples assume authentication has run and set the `User` property.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/Edit.cshtml.cs?name=snippet_DocumentEditHandler)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentEditAction)]

::: moniker-end

## <a name="write-a-resource-based-handler"></a><span data-ttu-id="9c240-122">Escribir un controlador basado en recursos</span><span class="sxs-lookup"><span data-stu-id="9c240-122">Write a resource-based handler</span></span>

<span data-ttu-id="9c240-123">Escribir un controlador para la autorización basada en recursos no es muy diferente que [escribir un controlador de requisitos sin formato](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler).</span><span class="sxs-lookup"><span data-stu-id="9c240-123">Writing a handler for resource-based authorization isn't much different than [writing a plain requirements handler](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler).</span></span> <span data-ttu-id="9c240-124">Cree una clase de requisito personalizada e implemente una clase de controlador de requisitos.</span><span class="sxs-lookup"><span data-stu-id="9c240-124">Create a custom requirement class, and implement a requirement handler class.</span></span> <span data-ttu-id="9c240-125">Para obtener más información sobre cómo crear una clase de requisito, consulte [requisitos](xref:security/authorization/policies#requirements).</span><span class="sxs-lookup"><span data-stu-id="9c240-125">For more information on creating a requirement class, see [Requirements](xref:security/authorization/policies#requirements).</span></span>

<span data-ttu-id="9c240-126">La clase de controlador especifica el requisito y el tipo de recurso.</span><span class="sxs-lookup"><span data-stu-id="9c240-126">The handler class specifies both the requirement and resource type.</span></span> <span data-ttu-id="9c240-127">Por ejemplo, un controlador que utiliza un `SameAuthorRequirement` y un recurso de `Document` sigue:</span><span class="sxs-lookup"><span data-stu-id="9c240-127">For example, a handler utilizing a `SameAuthorRequirement` and a `Document` resource follows:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

::: moniker-end

<span data-ttu-id="9c240-128">En el ejemplo anterior, Imagine que `SameAuthorRequirement` es un caso especial de una clase `SpecificAuthorRequirement` más genérica.</span><span class="sxs-lookup"><span data-stu-id="9c240-128">In the preceding example, imagine that `SameAuthorRequirement` is a special case of a more generic `SpecificAuthorRequirement` class.</span></span> <span data-ttu-id="9c240-129">La clase `SpecificAuthorRequirement` (no se muestra) contiene una propiedad `Name` que representa el nombre del autor.</span><span class="sxs-lookup"><span data-stu-id="9c240-129">The `SpecificAuthorRequirement` class (not shown) contains a `Name` property representing the name of the author.</span></span> <span data-ttu-id="9c240-130">La propiedad `Name` podría establecerse en el usuario actual.</span><span class="sxs-lookup"><span data-stu-id="9c240-130">The `Name` property could be set to the current user.</span></span>

<span data-ttu-id="9c240-131">Registre el requisito y el controlador en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="9c240-131">Register the requirement and handler in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Startup.cs?name=snippet_ConfigureServicesSample&highlight=3-7,9)]

### <a name="operational-requirements"></a><span data-ttu-id="9c240-132">Requisitos operativos</span><span class="sxs-lookup"><span data-stu-id="9c240-132">Operational requirements</span></span>

<span data-ttu-id="9c240-133">Si va a tomar decisiones basadas en los resultados de las operaciones CRUD (crear, leer, actualizar, eliminar), use la clase auxiliar [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) .</span><span class="sxs-lookup"><span data-stu-id="9c240-133">If you're making decisions based on the outcomes of CRUD (Create, Read, Update, Delete) operations, use the [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) helper class.</span></span> <span data-ttu-id="9c240-134">Esta clase le permite escribir un único controlador en lugar de una clase individual para cada tipo de operación.</span><span class="sxs-lookup"><span data-stu-id="9c240-134">This class enables you to write a single handler instead of an individual class for each operation type.</span></span> <span data-ttu-id="9c240-135">Para usarlo, proporcione algunos nombres de operación:</span><span class="sxs-lookup"><span data-stu-id="9c240-135">To use it, provide some operation names:</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_OperationsClass)]

<span data-ttu-id="9c240-136">El controlador se implementa como sigue, mediante un requisito `OperationAuthorizationRequirement` y un recurso `Document`:</span><span class="sxs-lookup"><span data-stu-id="9c240-136">The handler is implemented as follows, using an `OperationAuthorizationRequirement` requirement and a `Document` resource:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

::: moniker-end

<span data-ttu-id="9c240-137">El controlador anterior valida la operación mediante el recurso, la identidad del usuario y la propiedad `Name` del requisito.</span><span class="sxs-lookup"><span data-stu-id="9c240-137">The preceding handler validates the operation using the resource, the user's identity, and the requirement's `Name` property.</span></span>

## <a name="challenge-and-forbid-with-an-operational-resource-handler"></a><span data-ttu-id="9c240-138">Desafío y prohibido con un controlador de recursos operativos</span><span class="sxs-lookup"><span data-stu-id="9c240-138">Challenge and forbid with an operational resource handler</span></span>

<span data-ttu-id="9c240-139">En esta sección se muestra cómo se procesan los resultados de las acciones de desafío y de prohibido y cómo difieren desafío y prohibido.</span><span class="sxs-lookup"><span data-stu-id="9c240-139">This section shows how the challenge and forbid action results are processed and how challenge and forbid differ.</span></span>

<span data-ttu-id="9c240-140">Para llamar a un controlador de recursos operativo, especifique la operación al invocar `AuthorizeAsync` en el controlador de páginas o en la acción.</span><span class="sxs-lookup"><span data-stu-id="9c240-140">To call an operational resource handler, specify the operation when invoking `AuthorizeAsync` in your page handler or action.</span></span> <span data-ttu-id="9c240-141">En el ejemplo siguiente se determina si se permite al usuario autenticado ver el documento proporcionado.</span><span class="sxs-lookup"><span data-stu-id="9c240-141">The following example determines whether the authenticated user is permitted to view the provided document.</span></span>

> [!NOTE]
> <span data-ttu-id="9c240-142">En los ejemplos de código siguientes se asume que la autenticación se ha ejecutado y se ha establecido la propiedad `User`.</span><span class="sxs-lookup"><span data-stu-id="9c240-142">The following code samples assume authentication has run and set the `User` property.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/View.cshtml.cs?name=snippet_DocumentViewHandler&highlight=10-11)]

<span data-ttu-id="9c240-143">Si la autorización se realiza correctamente, se devuelve la página para ver el documento.</span><span class="sxs-lookup"><span data-stu-id="9c240-143">If authorization succeeds, the page for viewing the document is returned.</span></span> <span data-ttu-id="9c240-144">Si se produce un error en la autorización pero se autentica el usuario, la devolución del `ForbidResult` informa a cualquier middleware de autenticación de que se haya producido un error de autorización.</span><span class="sxs-lookup"><span data-stu-id="9c240-144">If authorization fails but the user is authenticated, returning `ForbidResult` informs any authentication middleware that authorization failed.</span></span> <span data-ttu-id="9c240-145">Se devuelve un `ChallengeResult` cuando se debe realizar la autenticación.</span><span class="sxs-lookup"><span data-stu-id="9c240-145">A `ChallengeResult` is returned when authentication must be performed.</span></span> <span data-ttu-id="9c240-146">En el caso de los clientes de explorador interactivos, puede ser adecuado redirigir al usuario a una página de inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="9c240-146">For interactive browser clients, it may be appropriate to redirect the user to a login page.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentViewAction&highlight=11-12)]

<span data-ttu-id="9c240-147">Si la autorización se realiza correctamente, se devuelve la vista para el documento.</span><span class="sxs-lookup"><span data-stu-id="9c240-147">If authorization succeeds, the view for the document is returned.</span></span> <span data-ttu-id="9c240-148">Si se produce un error en la autorización, la devolución de `ChallengeResult` informa a cualquier middleware de autenticación de que se ha producido un error de autorización y el middleware puede tomar la respuesta adecuada.</span><span class="sxs-lookup"><span data-stu-id="9c240-148">If authorization fails, returning `ChallengeResult` informs any authentication middleware that authorization failed, and the middleware can take the appropriate response.</span></span> <span data-ttu-id="9c240-149">Una respuesta adecuada podría devolver un código de estado 401 o 403.</span><span class="sxs-lookup"><span data-stu-id="9c240-149">An appropriate response could be returning a 401 or 403 status code.</span></span> <span data-ttu-id="9c240-150">En el caso de los clientes interactivos del explorador, podría significar redirigir al usuario a una página de inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="9c240-150">For interactive browser clients, it could mean redirecting the user to a login page.</span></span>

::: moniker-end
