---
title: "Autorización basada en la vista de MVC de ASP.NET Core"
author: rick-anderson
description: "Este documento muestra cómo insertar y utilizar el servicio de autorización dentro de una vista de ASP.NET Core Razor."
manager: wpickett
ms.author: riande
ms.date: 10/30/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/views
ms.openlocfilehash: 22754d07882cd704309a4e1a28ad0bf6f69432ea
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
# <a name="view-based-authorization"></a><span data-ttu-id="3c8d6-103">Autorización basada en la vista</span><span class="sxs-lookup"><span data-stu-id="3c8d6-103">View-based authorization</span></span>

<span data-ttu-id="3c8d6-104">A menudo, un programador desea mostrar, ocultar o modifique una interfaz de usuario en función de la identidad del usuario actual.</span><span class="sxs-lookup"><span data-stu-id="3c8d6-104">A developer often wants to show, hide, or otherwise modify a UI based on the current user identity.</span></span> <span data-ttu-id="3c8d6-105">Puede acceder al servicio de autorización en las vistas MVC a través de [inyección de dependencia](xref:fundamentals/dependency-injection#fundamentals-dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="3c8d6-105">You can access the authorization service within MVC views via [dependency injection](xref:fundamentals/dependency-injection#fundamentals-dependency-injection).</span></span> <span data-ttu-id="3c8d6-106">Para insertar el servicio de autorización en una vista Razor, use la `@inject` directiva:</span><span class="sxs-lookup"><span data-stu-id="3c8d6-106">To inject the authorization service into a Razor view, use the `@inject` directive:</span></span>

```cshtml
@using Microsoft.AspNetCore.Authorization
@inject IAuthorizationService AuthorizationService
```

<span data-ttu-id="3c8d6-107">Si desea que el servicio de autorización en cada vista, coloque el `@inject` la directiva en el *_ViewImports.cshtml* archivos de la *vistas* directory.</span><span class="sxs-lookup"><span data-stu-id="3c8d6-107">If you want the authorization service in every view, place the `@inject` directive into the *_ViewImports.cshtml* file of the *Views* directory.</span></span> <span data-ttu-id="3c8d6-108">Para obtener más información, consulte [inyección de dependencia en las vistas](xref:mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="3c8d6-108">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span>

<span data-ttu-id="3c8d6-109">Usar el servicio de autorización insertado para invocar `AuthorizeAsync` exactamente del mismo modo que se protegerían durante [autorización basada en recursos](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span><span class="sxs-lookup"><span data-stu-id="3c8d6-109">Use the injected authorization service to invoke `AuthorizeAsync` in exactly the same way you would check during [resource-based authorization](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3c8d6-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3c8d6-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, "PolicyName")).Succeeded)
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3c8d6-111">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3c8d6-111">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, "PolicyName"))
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

---

<span data-ttu-id="3c8d6-112">En algunos casos, el recurso será el modelo de vista.</span><span class="sxs-lookup"><span data-stu-id="3c8d6-112">In some cases, the resource will be your view model.</span></span> <span data-ttu-id="3c8d6-113">Invocar `AuthorizeAsync` exactamente del mismo modo que se protegerían durante [autorización basada en recursos](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span><span class="sxs-lookup"><span data-stu-id="3c8d6-113">Invoke `AuthorizeAsync` in exactly the same way you would check during [resource-based authorization](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3c8d6-114">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3c8d6-114">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit)).Succeeded)
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3c8d6-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3c8d6-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit))
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

---

<span data-ttu-id="3c8d6-116">En el código anterior, el modelo se pasa como un recurso que se debe realizar la evaluación de directivas en consideración.</span><span class="sxs-lookup"><span data-stu-id="3c8d6-116">In the preceding code, the model is passed as a resource the policy evaluation should take into consideration.</span></span>

> [!WARNING]
> <span data-ttu-id="3c8d6-117">No confíe en alternancia visibilidad de los elementos de interfaz de usuario de la aplicación como la comprobación de autorización única.</span><span class="sxs-lookup"><span data-stu-id="3c8d6-117">Don't rely on toggling visibility of your app's UI elements as the sole authorization check.</span></span> <span data-ttu-id="3c8d6-118">Ocultar un elemento de interfaz de usuario puede impedir totalmente el acceso a su acción de controlador asociado.</span><span class="sxs-lookup"><span data-stu-id="3c8d6-118">Hiding a UI element may not completely prevent access to its associated controller action.</span></span> <span data-ttu-id="3c8d6-119">Por ejemplo, considere el botón en el fragmento de código anterior.</span><span class="sxs-lookup"><span data-stu-id="3c8d6-119">For example, consider the button in the preceding code snippet.</span></span> <span data-ttu-id="3c8d6-120">Un usuario puede invocar la `Edit` dirección URL del método de acción si conozca el recurso relativo es */Document/Edit/1*.</span><span class="sxs-lookup"><span data-stu-id="3c8d6-120">A user can invoke the `Edit` action method if he or she knows the relative resource URL is */Document/Edit/1*.</span></span> <span data-ttu-id="3c8d6-121">Por este motivo, la `Edit` método de acción debe realizar su propia comprobación de autorización.</span><span class="sxs-lookup"><span data-stu-id="3c8d6-121">For this reason, the `Edit` action method should perform its own authorization check.</span></span>
