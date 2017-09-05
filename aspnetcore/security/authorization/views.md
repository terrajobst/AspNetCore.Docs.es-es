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
# <a name="view-based-authorization"></a><span data-ttu-id="e0d7b-103">Autorización basada en la vista</span><span class="sxs-lookup"><span data-stu-id="e0d7b-103">View Based Authorization</span></span>

<a name=security-authorization-views></a>

<span data-ttu-id="e0d7b-104">A menudo un desarrollador desea mostrar, ocultar o que modifique una interfaz de usuario en función de la identidad del usuario actual.</span><span class="sxs-lookup"><span data-stu-id="e0d7b-104">Often a developer will want to show, hide or otherwise modify a UI based on the current user identity.</span></span> <span data-ttu-id="e0d7b-105">Puede acceder al servicio de autorización en las vistas MVC a través de [inyección de dependencia](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="e0d7b-105">You can access the authorization service within MVC views via [dependency injection](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection).</span></span> <span data-ttu-id="e0d7b-106">Para insertar el servicio de autorización en un uso de vista Razor el `@inject` directiva, por ejemplo `@inject IAuthorizationService AuthorizationService` (requiere `@using Microsoft.AspNetCore.Authorization`).</span><span class="sxs-lookup"><span data-stu-id="e0d7b-106">To inject the authorization service into a Razor view use the `@inject` directive, for example `@inject IAuthorizationService AuthorizationService` (requires `@using Microsoft.AspNetCore.Authorization`).</span></span> <span data-ttu-id="e0d7b-107">Si desea que el servicio de autorización en cada vista, a continuación, coloque el `@inject` la directiva en el `_ViewImports.cshtml` un archivo en el `Views` directory.</span><span class="sxs-lookup"><span data-stu-id="e0d7b-107">If you want the authorization service in every view then place the `@inject` directive into the `_ViewImports.cshtml` file in the `Views` directory.</span></span> <span data-ttu-id="e0d7b-108">Para obtener más información sobre la inyección de dependencia en las vistas de consulta [inyección de dependencia en las vistas](../../mvc/views/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="e0d7b-108">For more information on dependency injection into views see [Dependency injection into views](../../mvc/views/dependency-injection.md).</span></span>

<span data-ttu-id="e0d7b-109">Una vez que se ha insertado el servicio de autorización utilice mediante una llamada a la `AuthorizeAsync` método en exactamente la misma manera que se protegerían durante [autorización basada en recursos](resourcebased.md#security-authorization-resource-based-imperative).</span><span class="sxs-lookup"><span data-stu-id="e0d7b-109">Once you have injected the authorization service you use it by calling the `AuthorizeAsync` method in exactly the same way as you would check during [resource based authorization](resourcebased.md#security-authorization-resource-based-imperative).</span></span>

```csharp
@if (await AuthorizationService.AuthorizeAsync(User, "PolicyName"))
   {
       <p>This paragraph is displayed because you fulfilled PolicyName.</p>
   }
   ```

<span data-ttu-id="e0d7b-110">En algunos casos, el recurso será el modelo de vista y se puede llamar a `AuthorizeAsync` en exactamente la misma manera que se protegerían durante [autorización basada en recursos](resourcebased.md#security-authorization-resource-based-imperative);</span><span class="sxs-lookup"><span data-stu-id="e0d7b-110">In some cases the resource will be your view model, and you can call `AuthorizeAsync` in exactly the same way as you would check during [resource based authorization](resourcebased.md#security-authorization-resource-based-imperative);</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e0d7b-111">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e0d7b-111">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
   @if ((await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit)).Succeeded)
   {
       <p><a class="btn btn-default" role="button"
           href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
   }
   ```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e0d7b-112">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e0d7b-112">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
   @if (await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit))
   {
       <p><a class="btn btn-default" role="button"
           href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
   }
   ```
---

<span data-ttu-id="e0d7b-113">Aquí puede ver que el modelo se pasa como la autorización de recursos debe tener en cuenta.</span><span class="sxs-lookup"><span data-stu-id="e0d7b-113">Here you can see the model is passed as the resource authorization should take into consideration.</span></span>

>[!WARNING]
><span data-ttu-id="e0d7b-114">No confíe en Mostrar u ocultar partes de la interfaz de usuario como el único método de autorización.</span><span class="sxs-lookup"><span data-stu-id="e0d7b-114">Do not rely on showing or hiding parts of your UI as your only authorization method.</span></span> <span data-ttu-id="e0d7b-115">Ocultar una interfaz de usuario elemento no significa que un usuario no puede tener acceso a él.</span><span class="sxs-lookup"><span data-stu-id="e0d7b-115">Hiding a UI element does not mean a user cannot access it.</span></span> <span data-ttu-id="e0d7b-116">También debe autorizar al usuario dentro del código de controlador.</span><span class="sxs-lookup"><span data-stu-id="e0d7b-116">You must also authorize the user within your controller code.</span></span>
