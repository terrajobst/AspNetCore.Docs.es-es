---
title: "Autorización basada en la vista de MVC de ASP.NET Core"
author: rick-anderson
description: "Este documento muestra cómo insertar y utilizar el servicio de autorización dentro de una vista de ASP.NET Core Razor."
ms.author: riande
manager: wpickett
ms.date: 10/30/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/views
ms.openlocfilehash: 8f87ca90b77be1efd75688e8203cb57b1a3360ad
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2018
---
# <a name="view-based-authorization"></a>Autorización basada en la vista

A menudo, un programador desea mostrar, ocultar o modifique una interfaz de usuario en función de la identidad del usuario actual. Puede acceder al servicio de autorización en las vistas MVC a través de [inyección de dependencia](xref:fundamentals/dependency-injection#fundamentals-dependency-injection). Para insertar el servicio de autorización en una vista Razor, use la `@inject` directiva:

```cshtml
@using Microsoft.AspNetCore.Authorization
@inject IAuthorizationService AuthorizationService
```

Si desea que el servicio de autorización en cada vista, coloque el `@inject` la directiva en el *_ViewImports.cshtml* archivos de la *vistas* directory. Para obtener más información, consulte [inyección de dependencia en las vistas](xref:mvc/views/dependency-injection).

Usar el servicio de autorización insertado para invocar `AuthorizeAsync` exactamente del mismo modo que se protegerían durante [autorización basada en recursos](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):

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

En algunos casos, el recurso será el modelo de vista. Invocar `AuthorizeAsync` exactamente del mismo modo que se protegerían durante [autorización basada en recursos](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):

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

En el código anterior, el modelo se pasa como un recurso que se debe realizar la evaluación de directivas en consideración.

> [!WARNING]
> No confíe en alternancia visibilidad de los elementos de interfaz de usuario de la aplicación como la comprobación de autorización única. Ocultar un elemento de interfaz de usuario puede impedir totalmente el acceso a su acción de controlador asociado. Por ejemplo, considere el botón en el fragmento de código anterior. Un usuario puede invocar la `Edit` dirección URL del método de acción si conozca el recurso relativo es */Document/Edit/1*. Por este motivo, la `Edit` método de acción debe realizar su propia comprobación de autorización.
