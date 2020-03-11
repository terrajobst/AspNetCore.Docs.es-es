---
title: Autorización simple en ASP.NET Core
author: rick-anderson
description: Aprenda a usar el atributo Authorize para restringir el acceso a ASP.NET Core controladores y acciones.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/simple
ms.openlocfilehash: 6409def0508b855d3d2a4a1f4d3a3d15bfe5dd32
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78653561"
---
# <a name="simple-authorization-in-aspnet-core"></a><span data-ttu-id="50075-103">Autorización simple en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="50075-103">Simple authorization in ASP.NET Core</span></span>

<a name="security-authorization-simple"></a>

<span data-ttu-id="50075-104">La autorización en MVC se controla a través del atributo `AuthorizeAttribute` y sus diversos parámetros.</span><span class="sxs-lookup"><span data-stu-id="50075-104">Authorization in MVC is controlled through the `AuthorizeAttribute` attribute and its various parameters.</span></span> <span data-ttu-id="50075-105">En su forma más sencilla, aplicar el `AuthorizeAttribute` atributo a un controlador o una acción limita el acceso al controlador o la acción a cualquier usuario autenticado.</span><span class="sxs-lookup"><span data-stu-id="50075-105">At its simplest, applying the `AuthorizeAttribute` attribute to a controller or action limits access to the controller or action to any authenticated user.</span></span>

<span data-ttu-id="50075-106">Por ejemplo, el código siguiente limita el acceso al `AccountController` a cualquier usuario autenticado.</span><span class="sxs-lookup"><span data-stu-id="50075-106">For example, the following code limits access to the `AccountController` to any authenticated user.</span></span>

```csharp
[Authorize]
public class AccountController : Controller
{
    public ActionResult Login()
    {
    }

    public ActionResult Logout()
    {
    }
}
```

<span data-ttu-id="50075-107">Si desea aplicar la autorización a una acción en lugar del controlador, aplique el atributo `AuthorizeAttribute` a la propia acción:</span><span class="sxs-lookup"><span data-stu-id="50075-107">If you want to apply authorization to an action rather than the controller, apply the `AuthorizeAttribute` attribute to the action itself:</span></span>

```csharp
public class AccountController : Controller
{
   public ActionResult Login()
   {
   }

   [Authorize]
   public ActionResult Logout()
   {
   }
}
```

<span data-ttu-id="50075-108">Ahora solo los usuarios autenticados pueden tener acceso a la función `Logout`.</span><span class="sxs-lookup"><span data-stu-id="50075-108">Now only authenticated users can access the `Logout` function.</span></span>

<span data-ttu-id="50075-109">También puede usar el atributo `AllowAnonymous` para permitir el acceso de usuarios no autenticados a acciones individuales.</span><span class="sxs-lookup"><span data-stu-id="50075-109">You can also use the `AllowAnonymous` attribute to allow access by non-authenticated users to individual actions.</span></span> <span data-ttu-id="50075-110">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="50075-110">For example:</span></span>

```csharp
[Authorize]
public class AccountController : Controller
{
    [AllowAnonymous]
    public ActionResult Login()
    {
    }

    public ActionResult Logout()
    {
    }
}
```

<span data-ttu-id="50075-111">Esto permitiría solo a los usuarios autenticados en el `AccountController`, a excepción de la `Login` acción, que es accesible para todos, independientemente de su estado autenticado o no autenticado/anónimo.</span><span class="sxs-lookup"><span data-stu-id="50075-111">This would allow only authenticated users to the `AccountController`, except for the `Login` action, which is accessible by everyone, regardless of their authenticated or unauthenticated / anonymous status.</span></span>

> [!WARNING]
> <span data-ttu-id="50075-112">`[AllowAnonymous]` omite todas las instrucciones de autorización.</span><span class="sxs-lookup"><span data-stu-id="50075-112">`[AllowAnonymous]` bypasses all authorization statements.</span></span> <span data-ttu-id="50075-113">Si combina `[AllowAnonymous]` y cualquier atributo de `[Authorize]`, se omiten los atributos de `[Authorize]`.</span><span class="sxs-lookup"><span data-stu-id="50075-113">If you combine `[AllowAnonymous]` and any `[Authorize]` attribute, the `[Authorize]` attributes are ignored.</span></span> <span data-ttu-id="50075-114">Por ejemplo, si se aplica `[AllowAnonymous]` en el nivel de controlador, se omiten los atributos de `[Authorize]` en el mismo controlador (o en cualquier acción que haya dentro de él).</span><span class="sxs-lookup"><span data-stu-id="50075-114">For example if you apply `[AllowAnonymous]` at the controller level, any `[Authorize]` attributes on the same controller (or on any action within it) is ignored.</span></span>
