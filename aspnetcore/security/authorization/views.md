---
title: Autorización basada en la vista de MVC de ASP.NET Core
author: rick-anderson
description: Este documento muestra cómo insertar y utilizar el servicio de autorización dentro de una vista de ASP.NET Core Razor.
ms.author: riande
ms.date: 10/30/2017
uid: security/authorization/views
ms.openlocfilehash: f25bab61afc93ff14bfd9c36d95a6d2e54b06dfb
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277830"
---
# <a name="view-based-authorization-in-aspnet-core-mvc"></a><span data-ttu-id="2c11a-103">Autorización basada en la vista de MVC de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2c11a-103">View-based authorization in ASP.NET Core MVC</span></span>

<span data-ttu-id="2c11a-104">A menudo, un programador desea mostrar, ocultar o modifique una interfaz de usuario en función de la identidad del usuario actual.</span><span class="sxs-lookup"><span data-stu-id="2c11a-104">A developer often wants to show, hide, or otherwise modify a UI based on the current user identity.</span></span> <span data-ttu-id="2c11a-105">Puede acceder al servicio de autorización en las vistas MVC a través de [inyección de dependencia](xref:fundamentals/dependency-injection#fundamentals-dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="2c11a-105">You can access the authorization service within MVC views via [dependency injection](xref:fundamentals/dependency-injection#fundamentals-dependency-injection).</span></span> <span data-ttu-id="2c11a-106">Para insertar el servicio de autorización en una vista Razor, use la `@inject` directiva:</span><span class="sxs-lookup"><span data-stu-id="2c11a-106">To inject the authorization service into a Razor view, use the `@inject` directive:</span></span>

```cshtml
@using Microsoft.AspNetCore.Authorization
@inject IAuthorizationService AuthorizationService
```

<span data-ttu-id="2c11a-107">Si desea que el servicio de autorización en cada vista, coloque el `@inject` la directiva en el *_ViewImports.cshtml* archivos de la *vistas* directory.</span><span class="sxs-lookup"><span data-stu-id="2c11a-107">If you want the authorization service in every view, place the `@inject` directive into the *_ViewImports.cshtml* file of the *Views* directory.</span></span> <span data-ttu-id="2c11a-108">Para más información, vea [Dependency injection into views](xref:mvc/views/dependency-injection) (Inserción de dependencias en vistas).</span><span class="sxs-lookup"><span data-stu-id="2c11a-108">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span>

<span data-ttu-id="2c11a-109">Usar el servicio de autorización insertado para invocar `AuthorizeAsync` exactamente del mismo modo que se protegerían durante [autorización basada en recursos](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span><span class="sxs-lookup"><span data-stu-id="2c11a-109">Use the injected authorization service to invoke `AuthorizeAsync` in exactly the same way you would check during [resource-based authorization](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2c11a-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2c11a-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, "PolicyName")).Succeeded)
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2c11a-111">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2c11a-111">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, "PolicyName"))
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

---

<span data-ttu-id="2c11a-112">En algunos casos, el recurso será el modelo de vista.</span><span class="sxs-lookup"><span data-stu-id="2c11a-112">In some cases, the resource will be your view model.</span></span> <span data-ttu-id="2c11a-113">Invocar `AuthorizeAsync` exactamente del mismo modo que se protegerían durante [autorización basada en recursos](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span><span class="sxs-lookup"><span data-stu-id="2c11a-113">Invoke `AuthorizeAsync` in exactly the same way you would check during [resource-based authorization](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2c11a-114">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2c11a-114">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit)).Succeeded)
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2c11a-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2c11a-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit))
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

---

<span data-ttu-id="2c11a-116">En el código anterior, el modelo se pasa como un recurso que se debe realizar la evaluación de directivas en consideración.</span><span class="sxs-lookup"><span data-stu-id="2c11a-116">In the preceding code, the model is passed as a resource the policy evaluation should take into consideration.</span></span>

> [!WARNING]
> <span data-ttu-id="2c11a-117">No confíe en alternancia visibilidad de los elementos de interfaz de usuario de la aplicación como la comprobación de autorización única.</span><span class="sxs-lookup"><span data-stu-id="2c11a-117">Don't rely on toggling visibility of your app's UI elements as the sole authorization check.</span></span> <span data-ttu-id="2c11a-118">Ocultar un elemento de interfaz de usuario puede impedir totalmente el acceso a su acción de controlador asociado.</span><span class="sxs-lookup"><span data-stu-id="2c11a-118">Hiding a UI element may not completely prevent access to its associated controller action.</span></span> <span data-ttu-id="2c11a-119">Por ejemplo, considere el botón en el fragmento de código anterior.</span><span class="sxs-lookup"><span data-stu-id="2c11a-119">For example, consider the button in the preceding code snippet.</span></span> <span data-ttu-id="2c11a-120">Un usuario puede invocar la `Edit` dirección URL del método de acción si conozca el recurso relativo es */Document/Edit/1*.</span><span class="sxs-lookup"><span data-stu-id="2c11a-120">A user can invoke the `Edit` action method if he or she knows the relative resource URL is */Document/Edit/1*.</span></span> <span data-ttu-id="2c11a-121">Por este motivo, la `Edit` método de acción debe realizar su propia comprobación de autorización.</span><span class="sxs-lookup"><span data-stu-id="2c11a-121">For this reason, the `Edit` action method should perform its own authorization check.</span></span>
