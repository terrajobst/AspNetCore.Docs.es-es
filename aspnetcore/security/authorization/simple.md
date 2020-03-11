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
# <a name="simple-authorization-in-aspnet-core"></a>Autorización simple en ASP.NET Core

<a name="security-authorization-simple"></a>

La autorización en MVC se controla a través del atributo `AuthorizeAttribute` y sus diversos parámetros. En su forma más sencilla, aplicar el `AuthorizeAttribute` atributo a un controlador o una acción limita el acceso al controlador o la acción a cualquier usuario autenticado.

Por ejemplo, el código siguiente limita el acceso al `AccountController` a cualquier usuario autenticado.

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

Si desea aplicar la autorización a una acción en lugar del controlador, aplique el atributo `AuthorizeAttribute` a la propia acción:

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

Ahora solo los usuarios autenticados pueden tener acceso a la función `Logout`.

También puede usar el atributo `AllowAnonymous` para permitir el acceso de usuarios no autenticados a acciones individuales. Por ejemplo:

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

Esto permitiría solo a los usuarios autenticados en el `AccountController`, a excepción de la `Login` acción, que es accesible para todos, independientemente de su estado autenticado o no autenticado/anónimo.

> [!WARNING]
> `[AllowAnonymous]` omite todas las instrucciones de autorización. Si combina `[AllowAnonymous]` y cualquier atributo de `[Authorize]`, se omiten los atributos de `[Authorize]`. Por ejemplo, si se aplica `[AllowAnonymous]` en el nivel de controlador, se omiten los atributos de `[Authorize]` en el mismo controlador (o en cualquier acción que haya dentro de él).
