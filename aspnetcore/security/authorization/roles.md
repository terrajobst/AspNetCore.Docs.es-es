---
title: Autorización basada en roles en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo restringir el acceso de acción y controlador de ASP.NET Core pasando roles para el atributo Authorize.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/roles
ms.openlocfilehash: 59753b90d3196b0bc16d4963f45b995f5108bc8b
ms.sourcegitcommit: d99a8554c91f626cf5e466911cf504dcbff0e02e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/31/2018
ms.locfileid: "39356680"
---
# <a name="role-based-authorization-in-aspnet-core"></a><span data-ttu-id="51c0f-103">Autorización basada en roles en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="51c0f-103">Role-based authorization in ASP.NET Core</span></span>

<a name="security-authorization-role-based"></a>

<span data-ttu-id="51c0f-104">Cuando se crea una identidad puede pertenecer a uno o varios roles.</span><span class="sxs-lookup"><span data-stu-id="51c0f-104">When an identity is created it may belong to one or more roles.</span></span> <span data-ttu-id="51c0f-105">Por ejemplo, mujer puede pertenecer a los roles de administrador y usuario mientras Scott solo puede pertenecer al rol de usuario.</span><span class="sxs-lookup"><span data-stu-id="51c0f-105">For example, Tracy may belong to the Administrator and User roles whilst Scott may only belong to the User role.</span></span> <span data-ttu-id="51c0f-106">Cómo se crean y administran estas funciones depende del almacén de respaldo del proceso de autorización.</span><span class="sxs-lookup"><span data-stu-id="51c0f-106">How these roles are created and managed depends on the backing store of the authorization process.</span></span> <span data-ttu-id="51c0f-107">Las funciones se exponen a los desarrolladores a través de la [IsInRole](/dotnet/api/system.security.principal.genericprincipal.isinrole) método en el [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal) clase.</span><span class="sxs-lookup"><span data-stu-id="51c0f-107">Roles are exposed to the developer through the [IsInRole](/dotnet/api/system.security.principal.genericprincipal.isinrole) method on the [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal) class.</span></span>

::: moniker range=">= aspnetcore-2.0"

> [!IMPORTANT]
> <span data-ttu-id="51c0f-108">Este tema **no** es válido con páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="51c0f-108">This topic does **not** apply to Razor Pages.</span></span> <span data-ttu-id="51c0f-109">Es compatible con las páginas de Razor [IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter) y [IAsyncPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter).</span><span class="sxs-lookup"><span data-stu-id="51c0f-109">Razor Pages supports [IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter) and [IAsyncPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter).</span></span> <span data-ttu-id="51c0f-110">Para más información, vea [Filter methods for Razor Pages](xref:razor-pages/filter) (Métodos de filtrado para páginas de Razor).</span><span class="sxs-lookup"><span data-stu-id="51c0f-110">For more information, see [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>

::: moniker-end

## <a name="adding-role-checks"></a><span data-ttu-id="51c0f-111">Agregar comprobaciones de la función</span><span class="sxs-lookup"><span data-stu-id="51c0f-111">Adding role checks</span></span>

<span data-ttu-id="51c0f-112">Comprobaciones de autorización basada en roles son declarativas&mdash;el desarrollador incrusta dentro de su código, con un controlador o una acción dentro de un controlador, especificar las funciones que el usuario actual debe ser miembro de tener acceso al recurso solicitado.</span><span class="sxs-lookup"><span data-stu-id="51c0f-112">Role-based authorization checks are declarative&mdash;the developer embeds them within their code, against a controller or an action within a controller, specifying roles which the current user must be a member of to access the requested resource.</span></span>

<span data-ttu-id="51c0f-113">Por ejemplo, el siguiente código limita el acceso a todas las acciones en el `AdministrationController` a los usuarios que son miembros de la `Administrator` rol:</span><span class="sxs-lookup"><span data-stu-id="51c0f-113">For example, the following code limits access to any actions on the `AdministrationController` to users who are a member of the `Administrator` role:</span></span>

```csharp
[Authorize(Roles = "Administrator")]
public class AdministrationController : Controller
{
}
```

<span data-ttu-id="51c0f-114">Puede especificar varios roles como una lista separada por comas:</span><span class="sxs-lookup"><span data-stu-id="51c0f-114">You can specify multiple roles as a comma separated list:</span></span>

```csharp
[Authorize(Roles = "HRManager,Finance")]
public class SalaryController : Controller
{
}
```

<span data-ttu-id="51c0f-115">Este controlador sería sólo puede tener acceso a los usuarios que son miembros de la `HRManager` rol o la `Finance` rol.</span><span class="sxs-lookup"><span data-stu-id="51c0f-115">This controller would be only accessible by users who are members of the `HRManager` role or the `Finance` role.</span></span>

<span data-ttu-id="51c0f-116">Si aplica varios atributos de un usuario que obtiene acceso debe ser miembro de todas las funciones especificadas; el ejemplo siguiente requiere que un usuario debe ser miembro de la `PowerUser` y `ControlPanelUser` rol.</span><span class="sxs-lookup"><span data-stu-id="51c0f-116">If you apply multiple attributes then an accessing user must be a member of all the roles specified; the following sample requires that a user must be a member of both the `PowerUser` and `ControlPanelUser` role.</span></span>

```csharp
[Authorize(Roles = "PowerUser")]
[Authorize(Roles = "ControlPanelUser")]
public class ControlPanelController : Controller
{
}
```

<span data-ttu-id="51c0f-117">Puede limitar aún más el acceso aplicando los atributos de autorización de rol adicionales en el nivel de acción:</span><span class="sxs-lookup"><span data-stu-id="51c0f-117">You can further limit access by applying additional role authorization attributes at the action level:</span></span>

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

<span data-ttu-id="51c0f-118">En los miembros de fragmento de código anterior de la `Administrator` rol o la `PowerUser` rol puede acceder al controlador y el `SetTime` acción, pero solo los miembros de la `Administrator` rol puede tener acceso el `ShutDown` acción.</span><span class="sxs-lookup"><span data-stu-id="51c0f-118">In the previous code snippet members of the `Administrator` role or the `PowerUser` role can access the controller and the `SetTime` action, but only members of the `Administrator` role can access the `ShutDown` action.</span></span>

<span data-ttu-id="51c0f-119">También puede bloquear un controlador, pero permitir el acceso anónimo, no autenticado a acciones individuales.</span><span class="sxs-lookup"><span data-stu-id="51c0f-119">You can also lock down a controller but allow anonymous, unauthenticated access to individual actions.</span></span>

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

## <a name="policy-based-role-checks"></a><span data-ttu-id="51c0f-120">Comprobaciones de la función basada en directivas</span><span class="sxs-lookup"><span data-stu-id="51c0f-120">Policy based role checks</span></span>

<span data-ttu-id="51c0f-121">Requisitos del rol también se pueden expresar utilizando la sintaxis de directiva nueva, donde el desarrollador registra una directiva en el inicio como parte de la configuración del servicio de autorización.</span><span class="sxs-lookup"><span data-stu-id="51c0f-121">Role requirements can also be expressed using the new Policy syntax, where a developer registers a policy at startup as part of the Authorization service configuration.</span></span> <span data-ttu-id="51c0f-122">Normalmente, esto ocurre en `ConfigureServices()` en su *Startup.cs* archivo.</span><span class="sxs-lookup"><span data-stu-id="51c0f-122">This normally occurs in `ConfigureServices()` in your *Startup.cs* file.</span></span>

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

<span data-ttu-id="51c0f-123">Las directivas se aplican mediante el `Policy` propiedad en el `AuthorizeAttribute` atributo:</span><span class="sxs-lookup"><span data-stu-id="51c0f-123">Policies are applied using the `Policy` property on the `AuthorizeAttribute` attribute:</span></span>

```csharp
[Authorize(Policy = "RequireAdministratorRole")]
public IActionResult Shutdown()
{
    return View();
}
```

<span data-ttu-id="51c0f-124">Si desea especificar varios roles permitidos en un requisito, puede especificarlas como parámetros a la `RequireRole` método:</span><span class="sxs-lookup"><span data-stu-id="51c0f-124">If you want to specify multiple allowed roles in a requirement then you can specify them as parameters to the `RequireRole` method:</span></span>

```csharp
options.AddPolicy("ElevatedRights", policy =>
                  policy.RequireRole("Administrator", "PowerUser", "BackupAdministrator"));
```

<span data-ttu-id="51c0f-125">En este ejemplo, autoriza a los usuarios que pertenecen a la `Administrator`, `PowerUser` o `BackupAdministrator` roles.</span><span class="sxs-lookup"><span data-stu-id="51c0f-125">This example authorizes users who belong to the `Administrator`, `PowerUser` or `BackupAdministrator` roles.</span></span>
