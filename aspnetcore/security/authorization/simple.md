---
title: Autorización simple en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo usar el atributo Authorize para restringir el acceso a las acciones y los controladores de ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/simple
ms.openlocfilehash: 6409def0508b855d3d2a4a1f4d3a3d15bfe5dd32
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/27/2019
ms.locfileid: "64897682"
---
# <a name="simple-authorization-in-aspnet-core"></a><span data-ttu-id="259bf-103">Autorización simple en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="259bf-103">Simple authorization in ASP.NET Core</span></span>

<a name="security-authorization-simple"></a>

<span data-ttu-id="259bf-104">Autorización en MVC se controla mediante el `AuthorizeAttribute` atributo y sus parámetros distintos.</span><span class="sxs-lookup"><span data-stu-id="259bf-104">Authorization in MVC is controlled through the `AuthorizeAttribute` attribute and its various parameters.</span></span> <span data-ttu-id="259bf-105">En su forma más simple, aplicar el `AuthorizeAttribute` atributo a una acción o controlador se limita el acceso al controlador o acción a cualquier usuario autenticado.</span><span class="sxs-lookup"><span data-stu-id="259bf-105">At its simplest, applying the `AuthorizeAttribute` attribute to a controller or action limits access to the controller or action to any authenticated user.</span></span>

<span data-ttu-id="259bf-106">Por ejemplo, el siguiente código limita el acceso a la `AccountController` a cualquier usuario autenticado.</span><span class="sxs-lookup"><span data-stu-id="259bf-106">For example, the following code limits access to the `AccountController` to any authenticated user.</span></span>

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

<span data-ttu-id="259bf-107">Si desea aplicar la autorización a una acción en lugar de con el controlador, se aplican los `AuthorizeAttribute` atributo a la propia acción:</span><span class="sxs-lookup"><span data-stu-id="259bf-107">If you want to apply authorization to an action rather than the controller, apply the `AuthorizeAttribute` attribute to the action itself:</span></span>

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

<span data-ttu-id="259bf-108">Ahora solo los usuarios autenticados pueden tener acceso el `Logout` función.</span><span class="sxs-lookup"><span data-stu-id="259bf-108">Now only authenticated users can access the `Logout` function.</span></span>

<span data-ttu-id="259bf-109">También puede usar el `AllowAnonymous` atributo para permitir el acceso a los usuarios no autenticados a acciones individuales.</span><span class="sxs-lookup"><span data-stu-id="259bf-109">You can also use the `AllowAnonymous` attribute to allow access by non-authenticated users to individual actions.</span></span> <span data-ttu-id="259bf-110">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="259bf-110">For example:</span></span>

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

<span data-ttu-id="259bf-111">Esto permitiría que solo los usuarios autenticados para la `AccountController`, excepto para el `Login` acción, que es accesible para todo el mundo, independientemente de su estado autenticado o no autenticado o anónimo.</span><span class="sxs-lookup"><span data-stu-id="259bf-111">This would allow only authenticated users to the `AccountController`, except for the `Login` action, which is accessible by everyone, regardless of their authenticated or unauthenticated / anonymous status.</span></span>

> [!WARNING]
> <span data-ttu-id="259bf-112">`[AllowAnonymous]` omite todas las instrucciones de autorización.</span><span class="sxs-lookup"><span data-stu-id="259bf-112">`[AllowAnonymous]` bypasses all authorization statements.</span></span> <span data-ttu-id="259bf-113">Si combina `[AllowAnonymous]` y cualquier `[Authorize]` atributo, el `[Authorize]` se omiten los atributos.</span><span class="sxs-lookup"><span data-stu-id="259bf-113">If you combine `[AllowAnonymous]` and any `[Authorize]` attribute, the `[Authorize]` attributes are ignored.</span></span> <span data-ttu-id="259bf-114">Por ejemplo, si aplica `[AllowAnonymous]` en el nivel de controlador, cualquier `[Authorize]` se omiten los atributos en el mismo controlador (o en cualquier acción dentro de él).</span><span class="sxs-lookup"><span data-stu-id="259bf-114">For example if you apply `[AllowAnonymous]` at the controller level, any `[Authorize]` attributes on the same controller (or on any action within it) is ignored.</span></span>
