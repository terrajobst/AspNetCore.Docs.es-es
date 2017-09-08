---
title: "Autorización basada en notificaciones"
author: rick-anderson
description: 
keywords: "Núcleo de ASP.NET,"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 737be5cd-3511-4f1c-b0ce-65403fb5eed3
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/claims
ms.openlocfilehash: fca75952429d48b02c2c4350b79e29a1957599dc
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/11/2017
---
# <a name="claims-based-authorization"></a>Autorización basada en notificaciones

<a name=security-authorization-claims-based></a>

Cuando se crea una identidad se pueden asignar una o más notificaciones emitidos por una entidad de confianza. Una notificación es par que representa el asunto de qué es el valor de nombre, puede hacerlo no qué el sujeto. Por ejemplo puede tener una licencia de controladores, emitido por una entidad de licencia de conducir local. De conducir su permiso tiene tu fecha de nacimiento. En este caso, el nombre de notificación sería `DateOfBirth`, el valor de notificación sería tu fecha de nacimiento, por ejemplo `8th June 1970` y el emisor sería la autoridad de licencia de conducir. Autorización basada en notificaciones, en su forma más sencilla, comprueba el valor de una notificación y permite el acceso a un recurso basado en ese valor. Por ejemplo, si desea que el proceso de autorización el acceso a un club nocturno puede ser:

El responsable de seguridad de la puerta evalúe el valor de la fecha de nacimiento notificación y si confía en el emisor (la entidad de licencia determinante) antes de conceder que acceso.

Una identidad puede contener varias notificaciones con varios valores y puede contener varias notificaciones del mismo tipo.

## <a name="adding-claims-checks"></a>Agregar controles de notificaciones

Notificación de comprobaciones de autorización basados en son declarativas, el desarrollador incrusta dentro de su código, con un controlador o una acción en un controlador de la especificación de notificaciones que debe poseer el usuario actual y, opcionalmente, debe contener el valor de la notificación para tener acceso a la recurso solicitado. Notificaciones requisitos son basada en directivas, el desarrollador debe crear y registrar una directiva de expresar los requisitos de notificaciones.

El tipo más sencillo de directiva Busca la presencia de una notificación de notificación y no comprueba el valor.

En primer lugar debe crear y registrar la directiva. Esto tiene lugar como parte de la configuración de servicio de autorización, que normalmente toma parte en `ConfigureServices()` en su *Startup.cs* archivo.

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

A continuación, aplique la directiva con la `Policy` propiedad en el `AuthorizeAttribute` atributo para especificar el nombre de la directiva;

```csharp
[Authorize(Policy = "EmployeeOnly")]
public IActionResult VacationBalance()
{
    return View();
}
```

El `AuthorizeAttribute` atributo sólo se puede aplicar a un controlador todo, en esta instancia de la directiva de coincidencia de identidades tendrán permiso para acceder a una acción en el controlador.

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class VacationController : Controller
{
    public ActionResult VacationBalance()
    {
    }
}
```

Si tiene un controlador que está protegido por el `AuthorizeAttribute` de atributo, pero desea permitir el acceso anónimo a acciones concretas que se aplica los `AllowAnonymousAttribute` atributo.

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

La mayoría de notificaciones vienen con un valor. Puede especificar una lista de valores permitidos al crear la directiva. En el ejemplo siguiente se realizaría correctamente solo para los empleados cuyo número de empleado era 1, 2, 3, 4 o 5.

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

## <a name="multiple-policy-evaluation"></a>Evaluación múltiple de directiva

Si se aplican varias directivas a una acción o controlador de todas las directivas deben pasar antes de que se concede el acceso. Por ejemplo:

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

En el ejemplo anterior cualquier identidad que cumple el `EmployeeOnly` directiva puede tener acceso a la `Payslip` acción como esa directiva se aplica en el controlador. Sin embargo para llamar a la `UpdateSalary` acción debe cumplir la identidad *ambos* el `EmployeeOnly` directiva y la `HumanResources` directiva.

Si desea que las directivas más complicadas, como llevar a cabo una fecha de nacimiento notificación, calcular una edad de ella, a continuación, comprobar la edad es 21 o anterior, a continuación, tiene que escribir [controladores de directiva personalizada](policies.md#security-authorization-policies-based).
