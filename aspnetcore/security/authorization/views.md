---
title: Autorización basada en la vista en ASP.NET Core MVC
author: rick-anderson
description: Este documento muestra cómo insertar y usar el servicio de autorización dentro de una vista de Razor de ASP.NET Core.
ms.author: riande
ms.date: 10/30/2017
uid: security/authorization/views
ms.openlocfilehash: e497c41d4dca29fed8733f18cf727804e3f06d8c
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342541"
---
# <a name="view-based-authorization-in-aspnet-core-mvc"></a>Autorización basada en la vista en ASP.NET Core MVC

A menudo, un programador desea mostrar, ocultar o modificar una interfaz de usuario según la identidad del usuario actual. Solo puede acceder al servicio de autorización en las vistas MVC a través de [inserción de dependencias](xref:fundamentals/dependency-injection). Para insertar el servicio de autorización en una vista de Razor, use el `@inject` directiva:

```cshtml
@using Microsoft.AspNetCore.Authorization
@inject IAuthorizationService AuthorizationService
```

Si desea que el servicio de autorización en cada vista, coloque el `@inject` la directiva en el *_ViewImports.cshtml* archivos de la *vistas* directory. Para más información, vea [Dependency injection into views](xref:mvc/views/dependency-injection) (Inserción de dependencias en vistas).

Usar el servicio de autorización insertado para invocar `AuthorizeAsync` exactamente del mismo modo que se comprobaría si durante [autorización basada en recursos](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, "PolicyName")).Succeeded)
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, "PolicyName"))
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

---

En algunos casos, el recurso será el modelo de vista. Invocar `AuthorizeAsync` exactamente del mismo modo que se comprobaría si durante [autorización basada en recursos](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit)).Succeeded)
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit))
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

---

En el código anterior, el modelo se pasa como un recurso de que la evaluación de directivas debe tomar en consideración.

> [!WARNING]
> No confíe en la visibilidad de alternancia de elementos de interfaz de usuario de la aplicación como la comprobación de autorización única. Ocultar un elemento de interfaz de usuario puede impedir completamente el acceso a su acción de controlador asociado. Por ejemplo, considere el botón en el fragmento de código anterior. Un usuario puede invocar la `Edit` dirección URL del método de acción si conozca el recurso relativo es */Document/Edit/1*. Por este motivo, la `Edit` método de acción debe realizar su propia comprobación de autorización.
