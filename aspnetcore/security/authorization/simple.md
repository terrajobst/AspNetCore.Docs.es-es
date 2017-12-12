---
title: "Autorización simple"
author: rick-anderson
description: "Este documento explica cómo usar el atributo Authorize para restringir el acceso a las acciones y controladores de ASP.NET Core."
keywords: "Núcleo de ASP.NET, autorización, AuthorizeAttribute"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 391bcaad-205f-43e4-badc-fa592d6f79f3
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/simple
ms.openlocfilehash: f2dad58ffa17259412077d31f512b561e79ac595
ms.sourcegitcommit: b38796ea3806bf39b89806adfa681b2a33762907
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/07/2017
---
# <a name="simple-authorization"></a><span data-ttu-id="091d4-104">Autorización simple</span><span class="sxs-lookup"><span data-stu-id="091d4-104">Simple Authorization</span></span>

<a name="security-authorization-simple"></a>

<span data-ttu-id="091d4-105">Autorización en MVC se controla mediante el `AuthorizeAttribute` atributo y sus parámetros distintos.</span><span class="sxs-lookup"><span data-stu-id="091d4-105">Authorization in MVC is controlled through the `AuthorizeAttribute` attribute and its various parameters.</span></span> <span data-ttu-id="091d4-106">En su forma más sencilla, aplicar el `AuthorizeAttribute` atributo a una acción o controlador se limita el acceso al controlador o acción a cualquier usuario autenticado.</span><span class="sxs-lookup"><span data-stu-id="091d4-106">At its simplest, applying the `AuthorizeAttribute` attribute to a controller or action limits access to the controller or action to any authenticated user.</span></span>

<span data-ttu-id="091d4-107">Por ejemplo, el siguiente código limita el acceso a la `AccountController` a cualquier usuario autenticado.</span><span class="sxs-lookup"><span data-stu-id="091d4-107">For example, the following code limits access to the `AccountController` to any authenticated user.</span></span>

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

<span data-ttu-id="091d4-108">Si desea aplicar la autorización a una acción en lugar de con el controlador, se aplican los `AuthorizeAttribute` atribuir a la propia acción:</span><span class="sxs-lookup"><span data-stu-id="091d4-108">If you want to apply authorization to an action rather than the controller, apply the `AuthorizeAttribute` attribute to the action itself:</span></span>

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

<span data-ttu-id="091d4-109">Ahora sólo los usuarios autenticados pueden tener acceso a la `Logout` (función).</span><span class="sxs-lookup"><span data-stu-id="091d4-109">Now only authenticated users can access the `Logout` function.</span></span>

<span data-ttu-id="091d4-110">También puede usar el `AllowAnonymousAttribute` atributo para permitir el acceso a los usuarios no autenticados para acciones individuales.</span><span class="sxs-lookup"><span data-stu-id="091d4-110">You can also use the `AllowAnonymousAttribute` attribute to allow access by non-authenticated users to individual actions.</span></span> <span data-ttu-id="091d4-111">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="091d4-111">For example:</span></span>

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

<span data-ttu-id="091d4-112">Esto le permitiría solo los usuarios autenticados para la `AccountController`, excepto para el `Login` acción, que es accesible para todos los usuarios, independientemente de su estado autenticado o no autenticado / anónimo.</span><span class="sxs-lookup"><span data-stu-id="091d4-112">This would allow only authenticated users to the `AccountController`, except for the `Login` action, which is accessible by everyone, regardless of their authenticated or unauthenticated / anonymous status.</span></span>

>[!WARNING]
> <span data-ttu-id="091d4-113">`[AllowAnonymous]`omite todas las instrucciones de autorización.</span><span class="sxs-lookup"><span data-stu-id="091d4-113">`[AllowAnonymous]` bypasses all authorization statements.</span></span> <span data-ttu-id="091d4-114">Si aplica la combinación `[AllowAnonymous]` y cualquier `[Authorize]` siempre se omitirá el atributo, a continuación, los atributos de autorizar.</span><span class="sxs-lookup"><span data-stu-id="091d4-114">If you apply combine `[AllowAnonymous]` and any `[Authorize]` attribute then the Authorize attributes will always be ignored.</span></span> <span data-ttu-id="091d4-115">Por ejemplo, si aplica `[AllowAnonymous]` en el controlador de nivel de cualquier `[Authorize]` se pasará por alto los atributos en el mismo controlador, o en cualquier acción en él.</span><span class="sxs-lookup"><span data-stu-id="091d4-115">For example if you apply `[AllowAnonymous]` at the controller level any `[Authorize]` attributes on the same controller, or on any action within it will be ignored.</span></span>
