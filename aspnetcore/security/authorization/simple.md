---
title: Simple de autorización en ASP.NET Core
author: rick-anderson
description: Obtenga información acerca de cómo usar el atributo Authorize para restringir el acceso a las acciones y controladores de ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/simple
ms.openlocfilehash: 6409def0508b855d3d2a4a1f4d3a3d15bfe5dd32
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961128"
---
# <a name="simple-authorization-in-aspnet-core"></a>Simple de autorización en ASP.NET Core

<a name="security-authorization-simple"></a>

Autorización en MVC se controla mediante el `AuthorizeAttribute` atributo y sus parámetros distintos. En su forma más sencilla, aplicar el `AuthorizeAttribute` atributo a una acción o controlador se limita el acceso al controlador o acción a cualquier usuario autenticado.

Por ejemplo, el siguiente código limita el acceso a la `AccountController` a cualquier usuario autenticado.

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

Si desea aplicar la autorización a una acción en lugar de con el controlador, se aplican los `AuthorizeAttribute` atribuir a la propia acción:

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

Ahora sólo los usuarios autenticados pueden tener acceso a la `Logout` (función).

También puede usar el `AllowAnonymous` atributo para permitir el acceso a los usuarios no autenticados para acciones individuales. Por ejemplo:

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

Esto le permitiría solo los usuarios autenticados para la `AccountController`, excepto para el `Login` acción, que es accesible para todos los usuarios, independientemente de su estado autenticado o no autenticado / anónimo.

> [!WARNING]
> `[AllowAnonymous]` omite todas las instrucciones de autorización. Si se combinan `[AllowAnonymous]` y cualquier `[Authorize]` atributo, el `[Authorize]` se omiten los atributos. Por ejemplo, si aplica `[AllowAnonymous]` en el nivel de controlador, cualquier `[Authorize]` se omiten los atributos en el mismo controlador (o en cualquier acción dentro de él).
