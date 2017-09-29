---
title: "Autorización personalizada basada en directivas"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: e422a1b2-dc4a-4bcc-b8d9-7ee62009b6a3
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/policies
ms.openlocfilehash: 5021b5d20f6d9b9a4d8889f25b5e41f2c9306f64
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/28/2017
---
# <a name="custom-policy-based-authorization"></a>Autorización personalizada basada en directivas

<a name=security-authorization-policies-based></a>

Interiormente el [autorización rol](roles.md#security-authorization-role-based) y [autorización de notificaciones](claims.md#security-authorization-claims-based) hacer uso de un requisito y un controlador para el requisito y una directiva configurada previamente. Estos bloques de creación permiten expresar las evaluaciones de autorización en el código, lo que permite una experiencia mejor, reutilizables y la estructura de autorización puede probar fácilmente.

Una directiva de autorización está formada por uno o varios requisitos y registrada al iniciar la aplicación como parte de la configuración de servicio de autorización, en `ConfigureServices` en el *Startup.cs* archivo.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("Over21",
                          policy => policy.Requirements.Add(new MinimumAgeRequirement(21)));
    });
}
```

Aquí puede ver que una directiva de "Edad superior a 21" se crea con un requisito único, que de una antigüedad mínima, que se pasa como parámetro al requisito.

Las directivas se aplican mediante el `Authorize` atributo especificando el nombre de la directiva, por ejemplo,

```csharp
[Authorize(Policy="Over21")]
public class AlcoholPurchaseRequirementsController : Controller
{
    public ActionResult Login()
    {
    }

    public ActionResult Logout()
    {
    }
}
```

## <a name="requirements"></a>Requisitos

Un requisito de autorización es una colección de parámetros de datos que puede usar una directiva para evaluar la entidad de seguridad del usuario actual. En nuestra directiva de antigüedad mínima el requisito que tenemos es un único parámetro, la antigüedad mínima. Debe implementar un requisito `IAuthorizationRequirement`. Se trata de una interfaz vacía, de marcador. Un requisito de antigüedad mínima con parámetros podría implementarse de la manera siguiente:

```csharp
public class MinimumAgeRequirement : IAuthorizationRequirement
{
    public int MinimumAge { get; private set; }
    
    public MinimumAgeRequirement(int minimumAge)
    {
        MinimumAge = minimumAge;
    }
}
```

No tiene un requisito que tienen datos o propiedades.

<a name=security-authorization-policies-based-authorization-handler></a>

## <a name="authorization-handlers"></a>Controladores de autorización

Un controlador de autorización es responsable de la evaluación de las propiedades de un requisito. El controlador de autorización debe evaluará con respecto a proporcionado `AuthorizationHandlerContext` para decidir si se permite la autorización. Puede tener un requisito [varios controladores](policies.md#security-authorization-policies-based-multiple-handlers). Los controladores deben heredar `AuthorizationHandler<T>` donde T es el requisito es capaz de abrir.

<a name=security-authorization-handler-example></a>

El controlador de antigüedad mínima podría ser similar al siguiente:

```csharp
public class MinimumAgeHandler : AuthorizationHandler<MinimumAgeRequirement>
{
    protected override Task HandleRequirementAsync(AuthorizationHandlerContext context, MinimumAgeRequirement requirement)
    {
        if (!context.User.HasClaim(c => c.Type == ClaimTypes.DateOfBirth &&
                                   c.Issuer == "http://contoso.com"))
        {
            // .NET 4.x -> return Task.FromResult(0);
            return Task.CompletedTask;
        }

        var dateOfBirth = Convert.ToDateTime(context.User.FindFirst(
            c => c.Type == ClaimTypes.DateOfBirth && c.Issuer == "http://contoso.com").Value);

        int calculatedAge = DateTime.Today.Year - dateOfBirth.Year;
        if (dateOfBirth > DateTime.Today.AddYears(-calculatedAge))
        {
            calculatedAge--;
        }

        if (calculatedAge >= requirement.MinimumAge)
        {
            context.Succeed(requirement);
        }
        return Task.CompletedTask;
    }
}
```

En el código anterior se busque primero para ver si la entidad de seguridad del usuario actual tiene una fecha de nacimiento que ha sido emitidos por un emisor que sabemos y confianza de notificaciones. Si falta la notificación se no autorizar por lo que se devuelven. Si tenemos una notificación, se pueda determinar la antigüedad del usuario es, y si cumple la antigüedad mínima que se pasa por el requisito, a continuación, autorización ha correcta. Una vez que la autorización es correcta llamamos `context.Succeed()` pasando el requisito de que ha tenido éxito como un parámetro.

<a name=security-authorization-policies-based-handler-registration></a>

Los controladores deben estar registrados en la colección de servicios durante la configuración, por ejemplo,

```csharp

public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("Over21",
                          policy => policy.Requirements.Add(new MinimumAgeRequirement(21)));
    });

    services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();
}
```

Cada controlador se agrega a la colección de servicios mediante `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();` pasar en la clase de controlador.

## <a name="what-should-a-handler-return"></a>¿Qué debe devolver un controlador?

Puede ver en nuestro [controlador (ejemplo)](policies.md#security-authorization-handler-example) que la `Handle()` método no tiene ningún valor devuelto, entonces, ¿cómo se indicaba correcto o erróneo?

* Un controlador indica éxito mediante una llamada a `context.Succeed(IAuthorizationRequirement requirement)`, pasando el requisito de que se ha validado correctamente.

* Un controlador no necesita controlar los errores por lo general, como otros controladores para el mismo requisito pueden realizarse correctamente.

* Para garantizar el error incluso si otros controladores para un requisito correctamente, llame a `context.Fail`.

Independientemente de lo que se llama a dentro de su controlador de todos los controladores para un requisito se llamará cuando una directiva exige el requisito. Esto permite requisitos tienen efectos secundarios, como el registro, que siempre se llevará a cabo aunque `context.Fail()` se ha llamado en otro controlador.

<a name=security-authorization-policies-based-multiple-handlers></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a>¿Por qué desearía varios controladores para un requisito?

En casos donde probablemente prefiera evaluación en un **o** base se implementan varios controladores para un requisito único. Por ejemplo, Microsoft tiene puertas que solo se abren con tarjetas de clave. Si deja la tarjeta de claves en casa la recepcionista imprime una etiqueta temporal y abre la puerta para usted. En este escenario tendría un requisito único, *EnterBuilding*, pero varios controladores, cada uno de ellos examinando un requisito único.

```csharp
public class EnterBuildingRequirement : IAuthorizationRequirement
{
}

public class BadgeEntryHandler : AuthorizationHandler<EnterBuildingRequirement>
{
    protected override Task HandleRequirementAsync(AuthorizationHandlerContext context, EnterBuildingRequirement requirement)
    {
        if (context.User.HasClaim(c => c.Type == ClaimTypes.BadgeId &&
                                       c.Issuer == "http://microsoftsecurity"))
        {
            context.Succeed(requirement);
        }
        return Task.CompletedTask;
    }
}

public class HasTemporaryStickerHandler : AuthorizationHandler<EnterBuildingRequirement>
{
    protected override Task HandleRequirementAsync(AuthorizationHandlerContext context, EnterBuildingRequirement requirement)
    {
        if (context.User.HasClaim(c => c.Type == ClaimTypes.TemporaryBadgeId &&
                                       c.Issuer == "https://microsoftsecurity"))
        {
            // We'd also check the expiration date on the sticker.
            context.Succeed(requirement);
        }
        return Task.CompletedTask;
    }
}
```

Ahora, suponiendo que ambos controladores son [registrado](xref:security/authorization/policies#security-authorization-policies-based-handler-registration) cuando una directiva se evalúa como el `EnterBuildingRequirement` si se realiza correctamente en cualquier controlador de la evaluación de directivas se realizará correctamente.

## <a name="using-a-func-to-fulfill-a-policy"></a>Usar un elemento func para cumplir una directiva

Puede haber ocasiones donde cumplir una directiva es sencillo expresar en el código. Es posible que solo tiene que proporcionar un `Func<AuthorizationHandlerContext, bool>` al configurar la directiva con el `RequireAssertion` el generador de directiva.

Por ejemplo anterior `BadgeEntryHandler` podría volver a escribir lo siguiente:

```csharp
services.AddAuthorization(options =>
    {
        options.AddPolicy("BadgeEntry",
                          policy => policy.RequireAssertion(context =>
                                  context.User.HasClaim(c =>
                                     (c.Type == ClaimTypes.BadgeId ||
                                      c.Type == ClaimTypes.TemporaryBadgeId)
                                      && c.Issuer == "https://microsoftsecurity"));
                          }));
    }
 }
```

## <a name="accessing-mvc-request-context-in-handlers"></a>Acceso al contexto de solicitud MVC en los controladores

El `Handle` método que debe implementar en un controlador de autorización tiene dos parámetros, un `AuthorizationContext` y `Requirement` está controlando. Marcos de trabajo como MVC o Jabbr pueden agregar cualquier objeto a la `Resource` propiedad en el `AuthorizationContext` para pasar información adicional.

Por ejemplo MVC pasa una instancia de `Microsoft.AspNetCore.Mvc.Filters.AuthorizationFilterContext` en la propiedad de recurso que se utiliza para tener acceso a HttpContext, RouteData y todo lo demás MVC proporciona.

El uso de la `Resource` propiedad es específicos de la plataforma. De manera indicada en el `Resource` propiedad limitará las directivas de autorización para marcos de trabajo determinados. Primero debe convertir el `Resource` propiedad mediante la `as` palabra clave y, a continuación, compruebe la conversión tiene éxito para asegurarse de que el código de no bloqueo con `InvalidCastExceptions` cuando se ejecuta en otros marcos de trabajo;

```csharp
if (context.Resource is Microsoft.AspNetCore.Mvc.Filters.AuthorizationFilterContext mvcContext)
{
    // Examine MVC specific things like routing data.
}
```
