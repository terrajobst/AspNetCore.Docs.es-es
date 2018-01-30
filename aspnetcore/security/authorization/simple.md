---
title: "Autorización simple"
author: rick-anderson
description: "Este documento explica cómo usar el atributo Authorize para restringir el acceso a las acciones y controladores de ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/simple
ms.openlocfilehash: 3299a8fcbd8d8e089d8d7f95e46551c102bcc054
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
# <a name="simple-authorization"></a>Autorización simple

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

También puede usar el `AllowAnonymousAttribute` atributo para permitir el acceso a los usuarios no autenticados para acciones individuales. Por ejemplo:

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

>[!WARNING]
> `[AllowAnonymous]`omite todas las instrucciones de autorización. Si aplica la combinación `[AllowAnonymous]` y cualquier `[Authorize]` siempre se omitirá el atributo, a continuación, los atributos de autorizar. Por ejemplo, si aplica `[AllowAnonymous]` en el controlador de nivel de cualquier `[Authorize]` se pasará por alto los atributos en el mismo controlador, o en cualquier acción en él.
