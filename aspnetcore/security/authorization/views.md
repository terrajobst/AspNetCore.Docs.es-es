---
title: "Autorización basada en la vista"
author: rick-anderson
description: 
keywords: "Núcleo de ASP.NET,"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 24ce40d8-9b83-4bae-9d4c-a66350fcc8f8
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/views
ms.openlocfilehash: 3b7fa6025d766da80ba92ee27af20bf9bfe0dcf4
ms.sourcegitcommit: 74e22e08e3b08cb576e5184d16f4af5656c13c0c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/25/2017
---
# <a name="view-based-authorization"></a>Autorización basada en la vista

<a name=security-authorization-views></a>

A menudo un desarrollador desea mostrar, ocultar o que modifique una interfaz de usuario en función de la identidad del usuario actual. Puede acceder al servicio de autorización en las vistas MVC a través de [inyección de dependencia](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection). Para insertar el servicio de autorización en un uso de vista Razor el `@inject` directiva, por ejemplo `@inject IAuthorizationService AuthorizationService` (requiere `@using Microsoft.AspNetCore.Authorization`). Si desea que el servicio de autorización en cada vista, a continuación, coloque el `@inject` la directiva en el `_ViewImports.cshtml` un archivo en el `Views` directory. Para obtener más información sobre la inyección de dependencia en las vistas de consulta [inyección de dependencia en las vistas](../../mvc/views/dependency-injection.md).

Una vez que se ha insertado el servicio de autorización utilice mediante una llamada a la `AuthorizeAsync` método en exactamente la misma manera que se protegerían durante [autorización basada en recursos](resourcebased.md#security-authorization-resource-based-imperative).

```csharp
@if (await AuthorizationService.AuthorizeAsync(User, "PolicyName"))
   {
       <p>This paragraph is displayed because you fulfilled PolicyName.</p>
   }
   ```

En algunos casos, el recurso será el modelo de vista y se puede llamar a `AuthorizeAsync` en exactamente la misma manera que se protegerían durante [autorización basada en recursos](resourcebased.md#security-authorization-resource-based-imperative);

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
   @if ((await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit)).Succeeded)
   {
       <p><a class="btn btn-default" role="button"
           href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
   }
   ```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
   @if (await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit))
   {
       <p><a class="btn btn-default" role="button"
           href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
   }
   ```
---

Aquí puede ver que el modelo se pasa como la autorización de recursos debe tener en cuenta.

>[!WARNING]
>No confíe en Mostrar u ocultar partes de la interfaz de usuario como el único método de autorización. Ocultar una interfaz de usuario elemento no significa que un usuario no puede tener acceso a él. También debe autorizar al usuario dentro del código de controlador.
