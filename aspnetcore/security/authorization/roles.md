---
title: Autorización basada en roles en ASP.NET Core
author: rick-anderson
description: Obtener información sobre cómo restringir el acceso de acción y controlador de ASP.NET Core pasando roles para el atributo Authorize.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/roles
ms.openlocfilehash: 0d39a457782061a57779bacb0d3a255be352bd2d
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276437"
---
# <a name="role-based-authorization-in-aspnet-core"></a>Autorización basada en roles en ASP.NET Core

<a name="security-authorization-role-based"></a>

Cuando se crea una identidad puede pertenecer a uno o más roles. Por ejemplo, Tracy puede pertenecer a los roles de administrador y usuario aunque Scott solo puede pertenecer al rol de usuario. ¿Cómo se crean y administran estas funciones depende del almacén de copias de seguridad del proceso de autorización. Las funciones se exponen al programador a través de la [IsInRole](/dotnet/api/system.security.principal.genericprincipal.isinrole) método en el [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal) clase.

## <a name="adding-role-checks"></a>Agregar comprobaciones de la función

Comprobaciones de autorización basada en roles son declarativas&mdash;el desarrollador incrusta dentro de su código, con un controlador o una acción en un controlador, especificar las funciones que el usuario actual debe ser un miembro de tener acceso al recurso solicitado.

Por ejemplo, el siguiente código limita el acceso a todas las acciones en el `AdministrationController` a los usuarios que forman parte de la `Administrator` rol:

```csharp
[Authorize(Roles = "Administrator")]
public class AdministrationController : Controller
{
}
```

Puede especificar varios roles como una lista separada por comas:

```csharp
[Authorize(Roles = "HRManager,Finance")]
public class SalaryController : Controller
{
}
```

Este controlador sería solo sea accesible por usuarios que son miembros de la `HRManager` rol o la `Finance` rol.

Si aplica varios atributos de un usuario que obtiene acceso debe ser miembro de todos los roles especificados; el ejemplo siguiente requiere que un usuario debe ser miembro de la `PowerUser` y `ControlPanelUser` rol.

```csharp
[Authorize(Roles = "PowerUser")]
[Authorize(Roles = "ControlPanelUser")]
public class ControlPanelController : Controller
{
}
```

Puede limitar aún más el acceso mediante la aplicación de atributos de autorización de rol adicionales en el nivel de acción:

```csharp
[Authorize(Roles = "Administrator, PowerUser")]
public class ControlPanelController : Controller
{
    public ActionResult SetTime()
    {
    }

    [Authorize(Roles = "Administrator")]
    public ActionResult ShutDown()
    {
    }
}
```

En los miembros de fragmento de código anterior de la `Administrator` rol o la `PowerUser` rol puede tener acceso al controlador y el `SetTime` acción, pero solo los miembros de la `Administrator` rol puede tener acceso a la `ShutDown` acción.

También puede bloquear un controlador pero permitir el acceso anónimo, no autenticado a acciones individuales.

```csharp
[Authorize]
public class ControlPanelController : Controller
{
    public ActionResult SetTime()
    {
    }

    [AllowAnonymous]
    public ActionResult Login()
    {
    }
}
```

<a name="security-authorization-role-policy"></a>

## <a name="policy-based-role-checks"></a>Comprobaciones de la función basada en directivas

También se pueden expresar requisitos del rol utilizando la nueva sintaxis de directiva, donde un desarrollador registra una directiva durante el inicio como parte de la configuración de servicio de autorización. Normalmente, esto ocurre en `ConfigureServices()` en su *Startup.cs* archivo.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("RequireAdministratorRole", policy => policy.RequireRole("Administrator"));
    });
}
```

Las directivas se aplican mediante el `Policy` propiedad en el `AuthorizeAttribute` atributo:

```csharp
[Authorize(Policy = "RequireAdministratorRole")]
public IActionResult Shutdown()
{
    return View();
}
```

Si desea especificar varios roles permitidos en un requisito, a continuación, puede especificar como parámetros a la `RequireRole` método:

```csharp
options.AddPolicy("ElevatedRights", policy =>
                  policy.RequireRole("Administrator", "PowerUser", "BackupAdministrator"));
```

En este ejemplo se autoriza a los usuarios que pertenecen a la `Administrator`, `PowerUser` o `BackupAdministrator` roles.
