---
title: Simple de autorización en ASP.NET Core
author: rick-anderson
description: Obtenga información acerca de cómo usar el atributo Authorize para restringir el acceso a las acciones y controladores de ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/simple
ms.openlocfilehash: 3c5e9d5dfd65ded40c9828a666143c1868f5562f
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272070"
---
# <a name="simple-authorization-in-aspnet-core"></a><span data-ttu-id="4545e-103">Simple de autorización en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4545e-103">Simple authorization in ASP.NET Core</span></span>

<a name="security-authorization-simple"></a>

<span data-ttu-id="4545e-104">Autorización en MVC se controla mediante el `AuthorizeAttribute` atributo y sus parámetros distintos.</span><span class="sxs-lookup"><span data-stu-id="4545e-104">Authorization in MVC is controlled through the `AuthorizeAttribute` attribute and its various parameters.</span></span> <span data-ttu-id="4545e-105">En su forma más sencilla, aplicar el `AuthorizeAttribute` atributo a una acción o controlador se limita el acceso al controlador o acción a cualquier usuario autenticado.</span><span class="sxs-lookup"><span data-stu-id="4545e-105">At its simplest, applying the `AuthorizeAttribute` attribute to a controller or action limits access to the controller or action to any authenticated user.</span></span>

<span data-ttu-id="4545e-106">Por ejemplo, el siguiente código limita el acceso a la `AccountController` a cualquier usuario autenticado.</span><span class="sxs-lookup"><span data-stu-id="4545e-106">For example, the following code limits access to the `AccountController` to any authenticated user.</span></span>

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

<span data-ttu-id="4545e-107">Si desea aplicar la autorización a una acción en lugar de con el controlador, se aplican los `AuthorizeAttribute` atribuir a la propia acción:</span><span class="sxs-lookup"><span data-stu-id="4545e-107">If you want to apply authorization to an action rather than the controller, apply the `AuthorizeAttribute` attribute to the action itself:</span></span>

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

<span data-ttu-id="4545e-108">Ahora sólo los usuarios autenticados pueden tener acceso a la `Logout` (función).</span><span class="sxs-lookup"><span data-stu-id="4545e-108">Now only authenticated users can access the `Logout` function.</span></span>

<span data-ttu-id="4545e-109">También puede usar el `AllowAnonymous` atributo para permitir el acceso a los usuarios no autenticados para acciones individuales.</span><span class="sxs-lookup"><span data-stu-id="4545e-109">You can also use the `AllowAnonymous` attribute to allow access by non-authenticated users to individual actions.</span></span> <span data-ttu-id="4545e-110">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="4545e-110">For example:</span></span>

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

<span data-ttu-id="4545e-111">Esto le permitiría solo los usuarios autenticados para la `AccountController`, excepto para el `Login` acción, que es accesible para todos los usuarios, independientemente de su estado autenticado o no autenticado / anónimo.</span><span class="sxs-lookup"><span data-stu-id="4545e-111">This would allow only authenticated users to the `AccountController`, except for the `Login` action, which is accessible by everyone, regardless of their authenticated or unauthenticated / anonymous status.</span></span>

>[!WARNING]
> <span data-ttu-id="4545e-112">`[AllowAnonymous]` omite todas las instrucciones de autorización.</span><span class="sxs-lookup"><span data-stu-id="4545e-112">`[AllowAnonymous]` bypasses all authorization statements.</span></span> <span data-ttu-id="4545e-113">Si aplica la combinación `[AllowAnonymous]` y cualquier `[Authorize]` siempre se omitirá el atributo, a continuación, los atributos de autorizar.</span><span class="sxs-lookup"><span data-stu-id="4545e-113">If you apply combine `[AllowAnonymous]` and any `[Authorize]` attribute then the Authorize attributes will always be ignored.</span></span> <span data-ttu-id="4545e-114">Por ejemplo, si aplica `[AllowAnonymous]` en el controlador de nivel de cualquier `[Authorize]` se pasará por alto los atributos en el mismo controlador, o en cualquier acción en él.</span><span class="sxs-lookup"><span data-stu-id="4545e-114">For example if you apply `[AllowAnonymous]` at the controller level any `[Authorize]` attributes on the same controller, or on any action within it will be ignored.</span></span>
