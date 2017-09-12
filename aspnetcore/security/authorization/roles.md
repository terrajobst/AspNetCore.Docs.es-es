---
title: "Autorización basada en roles"
author: rick-anderson
description: 
keywords: "Núcleo de ASP.NET,"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 5e014da1-8bc0-409b-951a-88b92c661fdf
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/roles
ms.openlocfilehash: 1dc76f316b70d486febe386cc47cd1f843d8d8e3
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2017
---
# <a name="role-based-authorization"></a><span data-ttu-id="9e41e-103">Autorización basada en roles</span><span class="sxs-lookup"><span data-stu-id="9e41e-103">Role based Authorization</span></span>

<a name=security-authorization-role-based></a>

<span data-ttu-id="9e41e-104">Cuando se crea una identidad puede pertenecer a uno o más roles, por ejemplo Tracy puede pertenecer a los roles de administrador y usuario aunque Scott solo puede pertenecer al rol de usuario.</span><span class="sxs-lookup"><span data-stu-id="9e41e-104">When an identity is created it may belong to one or more roles, for example Tracy may belong to the Administrator and User roles whilst Scott may only belong to the user role.</span></span> <span data-ttu-id="9e41e-105">¿Cómo se crean y administran estas funciones depende del almacén de copias de seguridad del proceso de autorización.</span><span class="sxs-lookup"><span data-stu-id="9e41e-105">How these roles are created and managed depends on the backing store of the authorization process.</span></span> <span data-ttu-id="9e41e-106">Las funciones se exponen al programador a través de la [IsInRole](https://docs.microsoft.com/dotnet/api/system.security.principal.genericprincipal.isinrole) propiedad en el [ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal) clase.</span><span class="sxs-lookup"><span data-stu-id="9e41e-106">Roles are exposed to the developer through the [IsInRole](https://docs.microsoft.com/dotnet/api/system.security.principal.genericprincipal.isinrole) property on the [ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal) class.</span></span>

## <a name="adding-role-checks"></a><span data-ttu-id="9e41e-107">Agregar comprobaciones de la función</span><span class="sxs-lookup"><span data-stu-id="9e41e-107">Adding role checks</span></span>

<span data-ttu-id="9e41e-108">Las comprobaciones de autorización basado en roles son declarativas: el desarrollador incrusta dentro de su código, con un controlador o una acción en un controlador, especificar las funciones que el usuario actual debe ser un miembro de tener acceso al recurso solicitado.</span><span class="sxs-lookup"><span data-stu-id="9e41e-108">Role based authorization checks are declarative - the developer embeds them within their code, against a controller or an action within a controller, specifying roles which the current user must be a member of to access the requested resource.</span></span>

<span data-ttu-id="9e41e-109">Por ejemplo el código siguiente podría limitar el acceso a todas las acciones en el `AdministrationController` a los usuarios que forman parte de la `Administrator` grupo.</span><span class="sxs-lookup"><span data-stu-id="9e41e-109">For example the following code would limit access to any actions on the `AdministrationController` to users who are a member of the `Administrator` group.</span></span>

```csharp
[Authorize(Roles = "Administrator")]
public class AdministrationController : Controller
{
}
```

<span data-ttu-id="9e41e-110">Puede especificar varios roles como una lista separada por comas;</span><span class="sxs-lookup"><span data-stu-id="9e41e-110">You can specify multiple roles as a comma separated list;</span></span>

```csharp
[Authorize(Roles = "HRManager,Finance")]
public class SalaryController : Controller
{
}
```

<span data-ttu-id="9e41e-111">Este controlador sería solo sea accesible por usuarios que son miembros de la `HRManager` rol o la `Finance` rol.</span><span class="sxs-lookup"><span data-stu-id="9e41e-111">This controller would be only accessible by users who are members of the `HRManager` role or the `Finance` role.</span></span>

<span data-ttu-id="9e41e-112">Si aplica varios atributos de un usuario que obtiene acceso debe ser miembro de todos los roles especificados; el ejemplo siguiente requiere que un usuario debe ser miembro de la `PowerUser` y `ControlPanelUser` rol.</span><span class="sxs-lookup"><span data-stu-id="9e41e-112">If you apply multiple attributes then an accessing user must be a member of all the roles specified; the following sample requires that a user must be a member of both the `PowerUser` and `ControlPanelUser` role.</span></span>

```csharp
[Authorize(Roles = "PowerUser")]
[Authorize(Roles = "ControlPanelUser")]
public class ControlPanelController : Controller
{
}
```

<span data-ttu-id="9e41e-113">Puede limitar aún más el acceso aplicando los atributos de autorización de rol adicionales en el nivel de acción;</span><span class="sxs-lookup"><span data-stu-id="9e41e-113">You can further limit access by applying additional role authorization attributes at the action level;</span></span>

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

<span data-ttu-id="9e41e-114">En los miembros de fragmento de código anterior de la `Administrator` rol o la `PowerUser` rol puede tener acceso al controlador y el `SetTime` acción, pero solo los miembros de la `Administrator` rol puede tener acceso a la `ShutDown` acción.</span><span class="sxs-lookup"><span data-stu-id="9e41e-114">In the previous code snippet members of the `Administrator` role or the `PowerUser` role can access the controller and the `SetTime` action, but only members of the `Administrator` role can access the `ShutDown` action.</span></span>

<span data-ttu-id="9e41e-115">También puede bloquear un controlador pero permitir el acceso anónimo, no autenticado a acciones individuales.</span><span class="sxs-lookup"><span data-stu-id="9e41e-115">You can also lock down a controller but allow anonymous, unauthenticated access to individual actions.</span></span>

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

<a name=security-authorization-role-policy></a>

## <a name="policy-based-role-checks"></a><span data-ttu-id="9e41e-116">Comprobaciones de la función basada en directivas</span><span class="sxs-lookup"><span data-stu-id="9e41e-116">Policy based role checks</span></span>

<span data-ttu-id="9e41e-117">También se pueden expresar requisitos del rol utilizando la nueva sintaxis de directiva, donde un desarrollador registra una directiva durante el inicio como parte de la configuración de servicio de autorización.</span><span class="sxs-lookup"><span data-stu-id="9e41e-117">Role requirements can also be expressed using the new Policy syntax, where a developer registers a policy at startup as part of the Authorization service configuration.</span></span> <span data-ttu-id="9e41e-118">Normalmente, esto ocurre en `ConfigureServices()` en su *Startup.cs* archivo.</span><span class="sxs-lookup"><span data-stu-id="9e41e-118">This normally occurs in `ConfigureServices()` in your *Startup.cs* file.</span></span>

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

<span data-ttu-id="9e41e-119">Las directivas se aplican mediante el `Policy` propiedad en el `AuthorizeAttribute` atributo;</span><span class="sxs-lookup"><span data-stu-id="9e41e-119">Policies are applied using the `Policy` property on the `AuthorizeAttribute` attribute;</span></span>

```csharp
[Authorize(Policy = "RequireAdministratorRole")]
public IActionResult Shutdown()
{
    return View();
}
```

<span data-ttu-id="9e41e-120">Si desea especificar varios roles permitidos en un requisito, a continuación, puede especificar como parámetros a la `RequireRole` método;</span><span class="sxs-lookup"><span data-stu-id="9e41e-120">If you want to specify multiple allowed roles in a requirement then you can specify them as parameters to the `RequireRole` method;</span></span>

```csharp
options.AddPolicy("ElevatedRights", policy =>
                  policy.RequireRole("Administrator", "PowerUser", "BackupAdministrator"));
```

<span data-ttu-id="9e41e-121">En este ejemplo se autoriza a los usuarios que pertenecen a la `Administrator`, `PowerUser` o `BackupAdministrator` roles.</span><span class="sxs-lookup"><span data-stu-id="9e41e-121">This example authorizes users who belong to the `Administrator`, `PowerUser` or `BackupAdministrator` roles.</span></span>
