---
title: "Autorización basada en recursos"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 0902ba17-5304-4a12-a2d4-e0904569e988
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/resourcebased
ms.openlocfilehash: d3575619c53e77dadc293ea2bb7dc72501a8a1e3
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/13/2017
---
# <a name="resource-based-authorization"></a><span data-ttu-id="688ca-103">Autorización basada en recursos</span><span class="sxs-lookup"><span data-stu-id="688ca-103">Resource Based Authorization</span></span>

<a name="security-authorization-resource-based"></a>

<span data-ttu-id="688ca-104">A menudo autorización depende de los recursos que se obtiene acceso.</span><span class="sxs-lookup"><span data-stu-id="688ca-104">Often authorization depends upon the resource being accessed.</span></span> <span data-ttu-id="688ca-105">Por ejemplo, un documento puede tener una propiedad de autor.</span><span class="sxs-lookup"><span data-stu-id="688ca-105">For example, a document may have an author property.</span></span> <span data-ttu-id="688ca-106">Solo el autor del documento se podrá actualizarla, el recurso se debe cargar desde el repositorio de documentos para poder realizar una evaluación de autorización.</span><span class="sxs-lookup"><span data-stu-id="688ca-106">Only the document author would be allowed to update it, so the resource must be loaded from the document repository before an authorization evaluation can be made.</span></span> <span data-ttu-id="688ca-107">Esto no es posible con un atributo Authorize, como la evaluación de atributo tiene lugar antes del enlace de datos y antes de ejecutar su propio código para cargar un recurso dentro de una acción.</span><span class="sxs-lookup"><span data-stu-id="688ca-107">This cannot be done with an Authorize attribute, as attribute evaluation takes place before data binding and before your own code to load a resource runs inside an action.</span></span> <span data-ttu-id="688ca-108">En lugar de autorización declarativa, el método de atributo, debemos usar autorización imperativa, donde un desarrollador llama a una función de autorizar dentro de su propio código.</span><span class="sxs-lookup"><span data-stu-id="688ca-108">Instead of declarative authorization, the attribute method, we must use imperative authorization, where a developer calls an authorize function within their own code.</span></span>

## <a name="authorizing-within-your-code"></a><span data-ttu-id="688ca-109">Autorizar dentro del código</span><span class="sxs-lookup"><span data-stu-id="688ca-109">Authorizing within your code</span></span>

<span data-ttu-id="688ca-110">Autorización se implementa como un servicio, `IAuthorizationService`, registrado en la colección de servicio y de disponibilidad a través de [inyección de dependencia](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection) para tener acceso a los controladores.</span><span class="sxs-lookup"><span data-stu-id="688ca-110">Authorization is implemented as a service, `IAuthorizationService`, registered in the service collection and available via [dependency injection](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection) for Controllers to access.</span></span>

```csharp
public class DocumentController : Controller
{
    IAuthorizationService _authorizationService;

    public DocumentController(IAuthorizationService authorizationService)
    {
        _authorizationService = authorizationService;
    }
}
```

<span data-ttu-id="688ca-111">`IAuthorizationService`tiene dos métodos, uno en los que pasa el recurso y el nombre de directiva y el otro en los que pasa el recurso y una lista de requisitos para evaluar.</span><span class="sxs-lookup"><span data-stu-id="688ca-111">`IAuthorizationService` has two methods, one where you pass the resource and the policy name and the other where you pass the resource and a list of requirements to evaluate.</span></span>

```csharp
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

<a name="security-authorization-resource-based-imperative"></a>

<span data-ttu-id="688ca-112">Para llamar al servicio, cargar el recurso dentro de la acción, a continuación, llame a la `AuthorizeAsync` sobrecarga necesita.</span><span class="sxs-lookup"><span data-stu-id="688ca-112">To call the service, load your resource within your action then call the `AuthorizeAsync` overload you require.</span></span> <span data-ttu-id="688ca-113">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="688ca-113">For example:</span></span>

```csharp
public async Task<IActionResult> Edit(Guid documentId)
{
    Document document = documentRepository.Find(documentId);

    if (document == null)
    {
        return new HttpNotFoundResult();
    }

    if (await _authorizationService.AuthorizeAsync(User, document, "EditPolicy"))
    {
        return View(document);
    }
    else
    {
        return new ChallengeResult();
    }
}
```

## <a name="writing-a-resource-based-handler"></a><span data-ttu-id="688ca-114">Escribir un controlador basada en recursos</span><span class="sxs-lookup"><span data-stu-id="688ca-114">Writing a resource based handler</span></span>

<span data-ttu-id="688ca-115">Escribir un controlador para la autorización basada en recursos no es tan muy diferente a [escribir un controlador de requisitos sin formato](policies.md#security-authorization-policies-based-authorization-handler).</span><span class="sxs-lookup"><span data-stu-id="688ca-115">Writing a handler for resource based authorization is not that much different to [writing a plain requirements handler](policies.md#security-authorization-policies-based-authorization-handler).</span></span> <span data-ttu-id="688ca-116">Crear un requisito y, a continuación, implemente un controlador para el requisito, especificar los requisitos como antes y el tipo de recurso.</span><span class="sxs-lookup"><span data-stu-id="688ca-116">You create a requirement, and then implement a handler for the requirement, specifying the requirement as before and also the resource type.</span></span> <span data-ttu-id="688ca-117">Por ejemplo, un controlador que podría aceptar un recurso de documento sería como sigue:</span><span class="sxs-lookup"><span data-stu-id="688ca-117">For example, a handler which might accept a Document resource would look as follows:</span></span>

```csharp
public class DocumentAuthorizationHandler : AuthorizationHandler<MyRequirement, Document>
{
    protected override Task HandleRequirementAsync(AuthorizationHandlerContext context,
                                                MyRequirement requirement,
                                                Document resource)
    {
        // Validate the requirement against the resource and identity.

        return Task.CompletedTask;
    }
}
```

<span data-ttu-id="688ca-118">No olvide que también tenga que registrar el controlador en el `ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="688ca-118">Don't forget you also need to register your handler in the `ConfigureServices` method:</span></span>

```csharp
services.AddSingleton<IAuthorizationHandler, DocumentAuthorizationHandler>();
```

### <a name="operational-requirements"></a><span data-ttu-id="688ca-119">Requisitos operativos</span><span class="sxs-lookup"><span data-stu-id="688ca-119">Operational Requirements</span></span>

<span data-ttu-id="688ca-120">Si va a realizar decisiones basadas en operaciones como lectura, escritura, actualización y eliminación, puede usar el `OperationAuthorizationRequirement` clase en el `Microsoft.AspNetCore.Authorization.Infrastructure` espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="688ca-120">If you are making decisions based on operations such as read, write, update and delete, you can use the `OperationAuthorizationRequirement` class in the `Microsoft.AspNetCore.Authorization.Infrastructure` namespace.</span></span> <span data-ttu-id="688ca-121">Esta clase de requisito creada previamente permite escribir un único controlador que tiene un nombre de la operación con parámetros, en lugar de crear clases individuales para cada operación.</span><span class="sxs-lookup"><span data-stu-id="688ca-121">This prebuilt requirement class enables you to write a single handler which has a parameterized operation name, rather than create individual classes for each operation.</span></span> <span data-ttu-id="688ca-122">Para usarla, proporcionar algunos nombres de operación:</span><span class="sxs-lookup"><span data-stu-id="688ca-122">To use it, provide some operation names:</span></span>

```csharp
public static class Operations
{
    public static OperationAuthorizationRequirement Create =
        new OperationAuthorizationRequirement { Name = "Create" };
    public static OperationAuthorizationRequirement Read =
        new OperationAuthorizationRequirement   { Name = "Read" };
    public static OperationAuthorizationRequirement Update =
        new OperationAuthorizationRequirement { Name = "Update" };
    public static OperationAuthorizationRequirement Delete =
        new OperationAuthorizationRequirement { Name = "Delete" };
}
```

<span data-ttu-id="688ca-123">El controlador puede, a continuación, implementarse como sigue, utilizando un hipotético `Document` clase como el recurso:</span><span class="sxs-lookup"><span data-stu-id="688ca-123">Your handler could then be implemented as follows, using a hypothetical `Document` class as the resource:</span></span>

```csharp
public class DocumentAuthorizationHandler :
    AuthorizationHandler<OperationAuthorizationRequirement, Document>
{
    public override Task HandleRequirementAsync(AuthorizationHandlerContext context,
                                                OperationAuthorizationRequirement requirement,
                                                Document resource)
    {
        // Validate the operation using the resource, the identity and
        // the Name property value from the requirement.

        return Task.CompletedTask;
    }
}
```

<span data-ttu-id="688ca-124">Puede ver el controlador funciona en `OperationAuthorizationRequirement`.</span><span class="sxs-lookup"><span data-stu-id="688ca-124">You can see the handler works on `OperationAuthorizationRequirement`.</span></span> <span data-ttu-id="688ca-125">El código dentro del controlador debe asumir la propiedad Name del requisito proporcionado en cuenta al realizar sus evaluaciones.</span><span class="sxs-lookup"><span data-stu-id="688ca-125">The code inside the handler must take the Name property of the supplied requirement into account when making its evaluations.</span></span>

<span data-ttu-id="688ca-126">Para llamar a un controlador de recursos operativos es necesario especificar la operación cuando se llama a `AuthorizeAsync` en la acción.</span><span class="sxs-lookup"><span data-stu-id="688ca-126">To call an operational resource handler you need to specify the operation when calling `AuthorizeAsync` in your action.</span></span> <span data-ttu-id="688ca-127">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="688ca-127">For example:</span></span>

```csharp
if (await _authorizationService.AuthorizeAsync(User, document, Operations.Read))
{
    return View(document);
}
else
{
    return new ChallengeResult();
}
```

<span data-ttu-id="688ca-128">Este ejemplo se comprueba si el usuario es capaz de realizar la operación de lectura actual `document` instancia.</span><span class="sxs-lookup"><span data-stu-id="688ca-128">This example checks if the User is able to perform the Read operation for the current `document` instance.</span></span> <span data-ttu-id="688ca-129">Si la autorización se realiza correctamente, se devolverá la vista del documento.</span><span class="sxs-lookup"><span data-stu-id="688ca-129">If authorization succeeds the view for the document will be returned.</span></span> <span data-ttu-id="688ca-130">Si la autorización da error devolver `ChallengeResult` informará a cualquier autenticación ha fallado la autorización de middleware y el software intermedio puede tomar la respuesta adecuada, por ejemplo devuelve un código de estado 401 o 403 o redirige al usuario a una página de inicio de sesión para clientes de explorador interactivo.</span><span class="sxs-lookup"><span data-stu-id="688ca-130">If authorization fails returning `ChallengeResult` will inform any authentication middleware authorization has failed and the middleware can take the appropriate response, for example returning a 401 or 403 status code, or redirecting the user to a login page for interactive browser clients.</span></span>
