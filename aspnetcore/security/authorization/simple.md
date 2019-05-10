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
# <a name="simple-authorization-in-aspnet-core"></a>Autorización simple en ASP.NET Core

<a name="security-authorization-simple"></a>

Autorización en MVC se controla mediante el `AuthorizeAttribute` atributo y sus parámetros distintos. En su forma más simple, aplicar el `AuthorizeAttribute` atributo a una acción o controlador se limita el acceso al controlador o acción a cualquier usuario autenticado.

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

Si desea aplicar la autorización a una acción en lugar de con el controlador, se aplican los `AuthorizeAttribute` atributo a la propia acción:

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

Ahora solo los usuarios autenticados pueden tener acceso el `Logout` función.

También puede usar el `AllowAnonymous` atributo para permitir el acceso a los usuarios no autenticados a acciones individuales. Por ejemplo:

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

Esto permitiría que solo los usuarios autenticados para la `AccountController`, excepto para el `Login` acción, que es accesible para todo el mundo, independientemente de su estado autenticado o no autenticado o anónimo.

> [!WARNING]
> `[AllowAnonymous]` omite todas las instrucciones de autorización. Si combina `[AllowAnonymous]` y cualquier `[Authorize]` atributo, el `[Authorize]` se omiten los atributos. Por ejemplo, si aplica `[AllowAnonymous]` en el nivel de controlador, cualquier `[Authorize]` se omiten los atributos en el mismo controlador (o en cualquier acción dentro de él).
