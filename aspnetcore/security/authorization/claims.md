---
title: Autorización basada en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo agregar comprobaciones de notificaciones para la autorización en una aplicación ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/claims
ms.openlocfilehash: 6b60ae5515819b017ab577f655ed91ee4d8ed0dd
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65086154"
---
# <a name="claims-based-authorization-in-aspnet-core"></a>Autorización basada en ASP.NET Core

<a name="security-authorization-claims-based"></a>

Cuando se crea una identidad se pueden asignar una o más notificaciones emitidas por una entidad de confianza. Una notificación es un par nombre-valor que representa el asunto de qué es, no lo que el sujeto puede hacer. Por ejemplo, puede tener un permiso de conducir, emitido por una entidad de licencia de conducción local. De conducir su permiso tiene su fecha de nacimiento. En este caso sería el nombre de la notificación `DateOfBirth`, el valor de notificación sería su fecha de nacimiento, por ejemplo `8th June 1970` y el emisor sería la autoridad de licencia de conducción. Autorización basada en notificaciones, en su forma más simple, comprueba el valor de una notificación y permite el acceso a un recurso en función de ese valor. Por ejemplo, si desea que el proceso de autorización el acceso a un club nocturno puede ser:

El responsable de la seguridad de la puerta evaluaría el valor de la fecha de nacimiento notificación y si confía en el emisor (la entidad de licencia conducción) antes de conceder que acceso.

Una identidad puede contener varias notificaciones con varios valores y puede contener varias notificaciones del mismo tipo.

## <a name="adding-claims-checks"></a>Agregar comprobaciones de notificaciones

Notificación de comprobaciones de autorización basado en son declarativas: el desarrollador incrusta dentro de su código, con un controlador o una acción dentro de un controlador, especificar las notificaciones que debe poseer el usuario actual y, opcionalmente, debe contener el valor de la notificación para tener acceso a la recurso solicitado. Notificaciones de los requisitos son basada en directivas, el desarrollador debe crear y registrar una directiva de expresar los requisitos de notificaciones.

El tipo más sencillo de directiva Busca la presencia de una notificación de notificación y no comprueba el valor.

Primero deberá crear y registrar la directiva. Esta operación se realiza como parte de la configuración del servicio de autorización, que normalmente forma parte de `ConfigureServices()` en su *Startup.cs* archivo.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("EmployeeOnly", policy => policy.RequireClaim("EmployeeNumber"));
    });
}
```

En este caso el `EmployeeOnly` directiva comprueba la presencia de un `EmployeeNumber` de notificación de la identidad actual.

A continuación, aplicar la directiva con la `Policy` propiedad en el `AuthorizeAttribute` atributo para especificar el nombre de la directiva;

```csharp
[Authorize(Policy = "EmployeeOnly")]
public IActionResult VacationBalance()
{
    return View();
}
```

El `AuthorizeAttribute` atributo puede aplicarse a un controlador todo, en este caso solo las identidades de la directiva de coincidencia se permitirá acceso a cualquier acción en el controlador.

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class VacationController : Controller
{
    public ActionResult VacationBalance()
    {
    }
}
```

Si tiene un controlador que está protegido por la `AuthorizeAttribute` de atributo, pero desea permitir el acceso anónimo a acciones concretas que se aplica los `AllowAnonymousAttribute` atributo.

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class VacationController : Controller
{
    public ActionResult VacationBalance()
    {
    }

    [AllowAnonymous]
    public ActionResult VacationPolicy()
    {
    }
}
```

La mayoría de las notificaciones incluyen un valor. Puede especificar una lista de valores permitidos al crear la directiva. El ejemplo siguiente se realizaría correctamente sólo para los empleados cuyo número de empleado era 1, 2, 3, 4 o 5.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("Founders", policy =>
                          policy.RequireClaim("EmployeeNumber", "1", "2", "3", "4", "5"));
    });
}
```

### <a name="add-a-generic-claim-check"></a>Agregar una comprobación de solicitud genérico

Si el valor de notificación no es un valor único o una transformación es necesaria, utilice [RequireAssertion](/dotnet/api/microsoft.aspnetcore.authorization.authorizationpolicybuilder.requireassertion). Para obtener más información, consulte [mediante func para satisfacer una directiva](xref:security/authorization/policies#using-a-func-to-fulfill-a-policy).

## <a name="multiple-policy-evaluation"></a>Evaluación de directiva múltiples

Si varias directivas se aplican a un controlador o acción, todas las directivas deben pasar antes de que se concede acceso. Por ejemplo:

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class SalaryController : Controller
{
    public ActionResult Payslip()
    {
    }

    [Authorize(Policy = "HumanResources")]
    public ActionResult UpdateSalary()
    {
    }
}
```

En el ejemplo anterior cualquier identidad que cumple el `EmployeeOnly` directiva puede tener acceso a la `Payslip` acción como esa directiva se aplica en el controlador. Sin embargo para poder llamar a la `UpdateSalary` acción debe cumplir la identidad *ambos* el `EmployeeOnly` directiva y la `HumanResources` directiva.

Si desea que las directivas más complicadas, como llevar a cabo una fecha de nacimiento notificación, calcular una edad de él, a continuación, comprobar la edad es 21 o una versión anterior, deberá escribir [controladores de directiva personalizada](xref:security/authorization/policies).
