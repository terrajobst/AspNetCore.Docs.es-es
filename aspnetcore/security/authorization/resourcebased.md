---
title: "Autorización basada en recursos"
author: rick-anderson
description: 
keywords: "Núcleo de ASP.NET,"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 0902ba17-5304-4a12-a2d4-e0904569e988
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/resourcebased
ms.openlocfilehash: 2f799588ba4aca4664e1679e4c34657e7ca121fb
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/11/2017
---
# <a name="resource-based-authorization"></a>Autorización basada en recursos

<a name=security-authorization-resource-based></a>

A menudo autorización depende de los recursos que se obtiene acceso. Por ejemplo, un documento puede tener una propiedad de autor. Solo el autor del documento se podrá actualizarla, el recurso se debe cargar desde el repositorio de documentos para poder realizar una evaluación de autorización. Esto no es posible con un atributo Authorize, como la evaluación de atributo tiene lugar antes del enlace de datos y antes de ejecutar su propio código para cargar un recurso dentro de una acción. En lugar de autorización declarativa, el método de atributo, debemos usar autorización imperativa, donde un desarrollador llama a una función de autorizar dentro de su propio código.

## <a name="authorizing-within-your-code"></a>Autorizar dentro del código

Autorización se implementa como un servicio, `IAuthorizationService`, registrado en la colección de servicio y de disponibilidad a través de [inyección de dependencia](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection) para tener acceso a los controladores.

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

`IAuthorizationService`tiene dos métodos, uno en los que pasa el recurso y el nombre de directiva y el otro en los que pasa el recurso y una lista de requisitos para evaluar.

```csharp
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

<a name=security-authorization-resource-based-imperative></a>

Para llamar a la carga del servicio del recurso dentro de la acción, a continuación, llame a la `AuthorizeAsync` sobrecarga necesita. Por ejemplo

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

## <a name="writing-a-resource-based-handler"></a>Escribir un controlador basada en recursos

Escribir un controlador para la autorización basada en recursos no es tan muy diferente a [escribir un controlador de requisitos sin formato](policies.md#security-authorization-policies-based-authorization-handler). Crear un requisito y, a continuación, implemente un controlador para el requisito, especificar los requisitos como antes y el tipo de recurso. Por ejemplo, un controlador que podría aceptar un recurso de documento tendría el siguiente aspecto;

```csharp
public class DocumentAuthorizationHandler : AuthorizationHandler<MyRequirement, Document>
{
    public override Task HandleRequirementAsync(AuthorizationHandlerContext context,
                                                MyRequirement requirement,
                                                Document resource)
    {
        // Validate the requirement against the resource and identity.

        return Task.CompletedTask;
    }
}
```

No olvide que también tenga que registrar el controlador en el `ConfigureServices` método;

```csharp
services.AddSingleton<IAuthorizationHandler, DocumentAuthorizationHandler>();
```

### <a name="operational-requirements"></a>Requisitos operativos

Si va a realizar decisiones basadas en operaciones como lectura, escritura, actualización y eliminación, puede usar el `OperationAuthorizationRequirement` clase en el `Microsoft.AspNetCore.Authorization.Infrastructure` espacio de nombres. Esta clase de requisito creada previamente permite escribir un único controlador que tiene un nombre de la operación con parámetros, en lugar de crear clases individuales para cada operación. Para usar proporcionan algunos nombres de operación:

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

El controlador puede, a continuación, implementarse como sigue, utilizando un hipotético `Document` clase como el recurso;

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

Puede ver el controlador funciona en `OperationAuthorizationRequirement`. El código dentro del controlador debe asumir la propiedad Name del requisito proporcionado en cuenta al realizar sus evaluaciones.

Para llamar a un controlador de recursos operativos es necesario especificar la operación cuando se llama a `AuthorizeAsync` en la acción. Por ejemplo

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

Este ejemplo se comprueba si el usuario es capaz de realizar la operación de lectura actual `document` instancia. Si la autorización se realiza correctamente, se devolverá la vista del documento. Si la autorización da error devolver `ChallengeResult` informará a cualquier autenticación ha fallado la autorización de middleware y el software intermedio puede tomar la respuesta adecuada, por ejemplo devuelve un código de estado 401 o 403 o redirige al usuario a una página de inicio de sesión para clientes de explorador interactivo.
