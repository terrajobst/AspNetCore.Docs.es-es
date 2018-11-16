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
# <a name="resource-based-authorization-in-aspnet-core"></a>Autorización basada en recursos en ASP.NET Core

Estrategia de autorización depende de los recursos que se obtiene acceso. Considere la posibilidad de un documento que tiene una propiedad del autor. Solo el autor tiene permiso para actualizar el documento. Por lo tanto, el documento se debe recuperar del almacén de datos antes de que comience la evaluación de autorización.

Evaluación de atributos se produce antes del enlace de datos y antes de la ejecución del controlador de página o acción que se carga el documento. Por estos motivos, la autorización declarativa con un `[Authorize]` atributo no es suficiente. En su lugar, puede invocar un método de autorización personalizado&mdash;un estilo que se conoce como *autorización imperativa*.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample)).

[Crear una aplicación ASP.NET Core con datos de usuario protegidos por autorización](xref:security/authorization/secure-data) contiene una aplicación de ejemplo que utiliza la autorización basada en recursos.

## <a name="use-imperative-authorization"></a>Usar la autorización imperativa

Autorización se implementa como un [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) de servicio y se registra en la colección de servicios dentro de la `Startup` clase. El servicio está disponible a través de [inserción de dependencias](xref:fundamentals/dependency-injection) a los controladores de página o acciones.

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Controllers/DocumentController.cs?name=snippet_IAuthServiceDI&highlight=6)]

`IAuthorizationService` tiene dos `AuthorizeAsync` sobrecargas del método: una acepta el recurso y el nombre de la directiva y la otra acepta el recurso y una lista de requisitos para evaluar.

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

En el ejemplo siguiente, se carga el recurso se proteja en personalizada `Document` objeto. Un `AuthorizeAsync` sobrecarga se invoca para determinar si el usuario actual está autorizado para editar el documento proporcionado. Una directiva de autorización personalizada "EditPolicy" se incluye en la decisión. Consulte [autorización basada en directivas de Custom](xref:security/authorization/policies) para obtener más información sobre la creación de directivas de autorización.

> [!NOTE]
> El código siguiente, los ejemplos se supone que la autenticación se ha ejecutado y establezca el `User` propiedad.

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/Edit.cshtml.cs?name=snippet_DocumentEditHandler)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentEditAction)]

::: moniker-end

## <a name="write-a-resource-based-handler"></a>Escribir un controlador de recursos

Escribir un controlador para autorización basada en recursos no es mucho más diferente que [escribir un controlador simple requisitos](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler). Cree una clase de requisito personalizado e implementar una clase de controlador de requisito. Para obtener más información sobre la creación de una clase de requisito, consulte [requisitos](xref:security/authorization/policies#requirements).

La clase de controlador especifica el requisito y el tipo de recurso. Por ejemplo, un controlador utilizando un `SameAuthorRequirement` y un `Document` recursos se indica a continuación:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

::: moniker-end

En el ejemplo anterior, imagine que `SameAuthorRequirement` es un caso especial de un elemento genérico más `SpecificAuthorRequirement` clase. El `SpecificAuthorRequirement` clase (no mostrado) contiene un `Name` propiedad que representa el nombre del autor. El `Name` propiedad puede establecerse en el usuario actual.

Registrar el requisito y el controlador en `Startup.ConfigureServices`:

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Startup.cs?name=snippet_ConfigureServicesSample&highlight=3-7,9)]

### <a name="operational-requirements"></a>Requisitos operativos

Si va a realizar decisiones basadas en los resultados de las operaciones CRUD (creación, lectura, actualización, eliminación), use el [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) clase auxiliar. Esta clase le permite escribir un único controlador en lugar de una clase individual para cada operación. Para ello, proporcione algunos nombres de operación:

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_OperationsClass)]

El controlador se implementa como sigue, con un `OperationAuthorizationRequirement` requisito y un `Document` recursos:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

::: moniker-end

El controlador anterior valida la operación con el recurso, la identidad del usuario y el requisito `Name` propiedad.

Para llamar a un controlador de recursos operativos, especifique la operación al invocar `AuthorizeAsync` en su controlador de página o la acción. El ejemplo siguiente determina si el usuario autenticado tiene permiso para visualizar el documento proporcionado.

> [!NOTE]
> El código siguiente, los ejemplos se supone que la autenticación se ha ejecutado y establezca el `User` propiedad.

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/View.cshtml.cs?name=snippet_DocumentViewHandler&highlight=10-11)]

Si la autorización se realiza correctamente, se devuelve la página para ver el documento. Si se produce un error de autorización, pero el usuario está autenticado, devolver `ForbidResult` informa a cualquier middleware de autenticación con errores de autorización. Un `ChallengeResult` se devuelve cuando se debe realizar la autenticación. Para los clientes de explorador interactivo, puede ser adecuado redirigir al usuario a una página de inicio de sesión.

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentViewAction&highlight=11-12)]

Si la autorización se realiza correctamente, se devuelve la vista del documento. Si se produce un error de autorización, devolver `ChallengeResult` informa a cualquier middleware de autenticación que error de autorización, y el software intermedio puede tomar la respuesta adecuada. Una respuesta adecuada podría devolver un código de estado 401 o 403. Para los clientes de explorador interactivo, podría significar redirigir al usuario a una página de inicio de sesión.

::: moniker-end
