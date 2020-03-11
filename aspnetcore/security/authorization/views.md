---
title: Autorización basada en vistas en ASP.NET Core MVC
author: rick-anderson
description: En este documento se muestra cómo insertar y emplear el servicio de autorización dentro de una ASP.NET Core vista de Razor.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 11/08/2019
uid: security/authorization/views
ms.openlocfilehash: fc03da9eb98d36ffdda932ee5b16f327c2be9f83
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78653567"
---
# <a name="view-based-authorization-in-aspnet-core-mvc"></a>Autorización basada en vistas en ASP.NET Core MVC

A menudo, un desarrollador desea mostrar, ocultar o modificar de otra forma una interfaz de usuario basada en la identidad del usuario actual. Puede tener acceso al servicio de autorización en las vistas de MVC a través de la [inserción de dependencias](xref:fundamentals/dependency-injection). Para insertar el servicio de autorización en una vista de Razor, use la Directiva `@inject`:

```cshtml
@using Microsoft.AspNetCore.Authorization
@inject IAuthorizationService AuthorizationService
```

Si desea usar el servicio de autorización en cada vista, coloque la Directiva `@inject` en el archivo *_ViewImports. cshtml* del directorio *views* . Para más información, vea [Dependency injection into views](xref:mvc/views/dependency-injection) (Inserción de dependencias en vistas).

Use el servicio de autorización insertado para invocar `AuthorizeAsync` exactamente del mismo modo que lo haría durante la [autorización basada en recursos](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, "PolicyName")).Succeeded)
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

En algunos casos, el recurso será el modelo de vista. Invoque `AuthorizeAsync` exactamente del mismo modo que lo haría durante la [autorización basada en recursos](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit)).Succeeded)
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

En el código anterior, el modelo se pasa como un recurso que debería tener en cuenta la evaluación de la Directiva.

> [!WARNING]
> No se base en la alternancia de la visibilidad de los elementos de la interfaz de usuario de la aplicación como la única comprobación de autorización. Ocultar un elemento de la interfaz de usuario puede no impedir completamente el acceso a su acción de controlador asociada. Por ejemplo, considere el botón en el fragmento de código anterior. Un usuario puede invocar el método de acción `Edit` si sabe que la dirección URL relativa del recurso es */Document/Edit/1*. Por esta razón, el método de acción `Edit` debe realizar su propia comprobación de autorización.
