---
title: Autorización basada en roles en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo restringir el acceso de acción y controlador de ASP.NET Core pasando roles para el atributo Authorize.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/roles
ms.openlocfilehash: 0467ea82831bffe6882e584930c2fa1212a244c7
ms.sourcegitcommit: 6ba5fb1fd0b7f9a6a79085b0ef56206e462094b7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/14/2019
ms.locfileid: "56248100"
---
# <a name="role-based-authorization-in-aspnet-core"></a>Autorización basada en roles en ASP.NET Core

<a name="security-authorization-role-based"></a>

Cuando se crea una identidad puede pertenecer a uno o varios roles. Por ejemplo, mujer puede pertenecer a los roles de administrador y usuario mientras Scott solo puede pertenecer al rol de usuario. Cómo se crean y administran estas funciones depende del almacén de respaldo del proceso de autorización. Las funciones se exponen a los desarrolladores a través de la [IsInRole](/dotnet/api/system.security.principal.genericprincipal.isinrole) método en el [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal) clase.

::: moniker range=">= aspnetcore-2.0"

> [!IMPORTANT]
> Este tema **no** es válido con páginas de Razor. Es compatible con las páginas de Razor [IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter) y [IAsyncPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter). Para más información, vea [Filter methods for Razor Pages](xref:razor-pages/filter) (Métodos de filtrado para páginas de Razor).

::: moniker-end

## <a name="adding-role-checks"></a>Agregar comprobaciones de la función

Comprobaciones de autorización basada en roles son declarativas&mdash;el desarrollador incrusta dentro de su código, con un controlador o una acción dentro de un controlador, especificar las funciones que el usuario actual debe ser miembro de tener acceso al recurso solicitado.

Por ejemplo, el siguiente código limita el acceso a todas las acciones en el `AdministrationController` a los usuarios que son miembros de la `Administrator` rol:

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

Este controlador sería sólo puede tener acceso a los usuarios que son miembros de la `HRManager` rol o la `Finance` rol.

Si aplica varios atributos de un usuario que obtiene acceso debe ser miembro de todas las funciones especificadas; el ejemplo siguiente requiere que un usuario debe ser miembro de la `PowerUser` y `ControlPanelUser` rol.

```csharp
[Authorize(Roles = "PowerUser")]
[Authorize(Roles = "ControlPanelUser")]
public class ControlPanelController : Controller
{
}
```

Puede limitar aún más el acceso aplicando los atributos de autorización de rol adicionales en el nivel de acción:

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

En los miembros de fragmento de código anterior de la `Administrator` rol o la `PowerUser` rol puede acceder al controlador y el `SetTime` acción, pero solo los miembros de la `Administrator` rol puede tener acceso el `ShutDown` acción.

También puede bloquear un controlador, pero permitir el acceso anónimo, no autenticado a acciones individuales.

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

Requisitos del rol también se pueden expresar utilizando la sintaxis de directiva nueva, donde el desarrollador registra una directiva en el inicio como parte de la configuración del servicio de autorización. Normalmente, esto ocurre en `ConfigureServices()` en su *Startup.cs* archivo.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("RequireAdministratorRole",
             policy => policy.RequireRole("Administrator"));
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

Si desea especificar varios roles permitidos en un requisito, puede especificarlas como parámetros a la `RequireRole` método:

```csharp
options.AddPolicy("ElevatedRights", policy =>
                  policy.RequireRole("Administrator", "PowerUser", "BackupAdministrator"));
```

En este ejemplo, autoriza a los usuarios que pertenecen a la `Administrator`, `PowerUser` o `BackupAdministrator` roles.

### <a name="add-role-services-to-identity"></a>Agregar servicios de función a la identidad

Anexar [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) para agregar servicios de rol:

[!code-csharp[](roles/samples/Startup.cs?name=snippet&highlight=7)]