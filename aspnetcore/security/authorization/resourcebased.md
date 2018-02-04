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
ms.openlocfilehash: 723e371e0d0b4877f96898c68cd59b433fa97dc1
ms.sourcegitcommit: 7a87d66cf1d01febe6635c7306f2f679434901d1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/03/2018
---
# <a name="resource-based-authorization"></a>Autorización basada en recursos

Estrategia de autorización depende de los recursos que se obtiene acceso. Considere la posibilidad de un documento que tiene una propiedad de autor. Solo el autor tiene permiso para actualizar el documento. Por lo tanto, el documento se debe recuperar desde el almacén de datos antes de que puede producirse la evaluación de autorización.

Evaluación de atributo se produce antes del enlace de datos y antes de la ejecución del controlador de la página o acción que carga el documento. Por estos motivos, autorización declarativa con un `[Authorize]` atributo no son suficientes. En su lugar, puede invocar un método de autorización personalizado&mdash;un estilo que se conoce como autorización imperativa.

Use la [aplicaciones de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([cómo descargar](xref:tutorials/index#how-to-download-a-sample)) para explorar las características descritas en este tema.

[Crear una aplicación de ASP.NET Core con datos de usuario protegidos por autorización](xref:security/authorization/secure-data) contiene una aplicación de ejemplo que utiliza la autorización basada en recursos.

## <a name="use-imperative-authorization"></a>Utilizar la autorización imperativa

Autorización se implementa como un [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) de servicio y está registrado en la colección de servicio en la `Startup` clase. El servicio debe ponerse a disposición a través de [inyección de dependencia](xref:fundamentals/dependency-injection#fundamentals-dependency-injection) a controladores de página o acciones.

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Controllers/DocumentController.cs?name=snippet_IAuthServiceDI&highlight=6)]

`IAuthorizationService`tiene dos `AuthorizeAsync` sobrecargas del método: acepta un recurso y, a continuación, el nombre de directiva y el otro aceptando el recurso y una lista de requisitos para evaluar.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

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

En el ejemplo siguiente, se carga el recurso que se va a proteger en un personalizado `Document` objeto. Un `AuthorizeAsync` sobrecarga se invoca para determinar si se permite al usuario actual para modificar el documento proporcionado. Una directiva de autorización personalizada "EditPolicy" es tenerse en cuenta en la decisión. Vea [autorización basada en directivas de personalizado](xref:security/authorization/policies) para obtener más información acerca de cómo crear directivas de autorización.

> [!NOTE]
> El código siguiente ejemplos se supone la autenticación se ha ejecutado y establezca el `User` propiedad.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/Edit.cshtml.cs?name=snippet_DocumentEditHandler)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentEditAction)]

---

## <a name="write-a-resource-based-handler"></a>Escribir un controlador basada en recursos

Escribir un controlador para la autorización basada en recursos no es muy diferente de [escribir un controlador de requisitos sin formato](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler). Cree una clase de requisito personalizado e implementar una clase de controlador de requisito. La clase de controlador especifica el requisito y el tipo de recurso. Por ejemplo, un controlador utilizando un `SameAuthorRequirement` requisito y un `Document` recurso tiene el siguiente aspecto:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

---

Registrar el requisito y el controlador en el `Startup.ConfigureServices` método:

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Startup.cs?name=snippet_ConfigureServicesSample&highlight=3-7,9)]

### <a name="operational-requirements"></a>Requisitos operativos

Si está realizando decisiones basadas en los resultados de CRUD (**C**rear, **R**EA, **U**ctualizar, **d.**liminar) de las operaciones, utilice la [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) clase auxiliar. Esta clase permite escribir un único controlador en lugar de una clase individual para cada tipo de operación. Para usarla, proporcionar algunos nombres de operación:

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_OperationsClass)]

El controlador se implementa como sigue, mediante una `OperationAuthorizationRequirement` requisito y un `Document` recursos:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

---

El controlador anterior valida la operación con el recurso, la identidad del usuario y el requisito `Name` propiedad.

Para llamar a un controlador de recursos operativa, especifique la operación al invocar `AuthorizeAsync` en el controlador de la página o la acción. En el ejemplo siguiente se determina si se permite al usuario autenticado para ver el documento proporcionado.

> [!NOTE]
> El código siguiente ejemplos se supone la autenticación se ha ejecutado y establezca el `User` propiedad.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/View.cshtml.cs?name=snippet_DocumentViewHandler&highlight=10-11)]

Si la autorización se realiza correctamente, se devuelve la página para ver el documento. Si se produce un error de autorización, pero el usuario se autentica, devolver `ForbidResult` informa a cualquier middleware de autenticación error de autorización. Un `ChallengeResult` se devuelve cuando se debe realizar la autenticación. Para los clientes de explorador interactivo, puede ser adecuado redirigir al usuario a una página de inicio de sesión.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentViewAction&highlight=11-12)]

Si la autorización se realiza correctamente, se devuelve la vista del documento. Si se produce un error en la autorización, devolver `ChallengeResult` informa a cualquier middleware de autenticación que error de autorización, y el software intermedio puede tomar la respuesta adecuada. Una respuesta adecuada podría devolver un código de estado 401 o 403. Para los clientes de explorador interactivo, podría significar redirige al usuario a una página de inicio de sesión.

---
