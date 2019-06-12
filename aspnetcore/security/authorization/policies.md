---
title: Autorización basada en directivas en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo crear y usar controladores de la directiva de autorización para exigir los requisitos de autorización en una aplicación ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 04/05/2019
uid: security/authorization/policies
ms.openlocfilehash: 67337c847ba71df3fe61250996ec944632ad5d57
ms.sourcegitcommit: 1bb3f3f1905b4e7d4ca1b314f2ce6ee5dd8be75f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/11/2019
ms.locfileid: "66837355"
---
# <a name="policy-based-authorization-in-aspnet-core"></a>Autorización basada en directivas en ASP.NET Core

Interiormente, [autorización basada en roles](xref:security/authorization/roles) y [autorización basada en notificaciones](xref:security/authorization/claims) usan un requisito, un controlador de requisito y una directiva configurada previamente. Estos bloques de creación se admite la expresión de evaluaciones de autorización en el código. El resultado es una estructura de autorización completas, reutilizables y apta para las pruebas.

Una directiva de autorización consta de uno o más requisitos. Está registrado como parte de la configuración del servicio de autorización, en el `Startup.ConfigureServices` método:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=32-33,48-53,61,66)]

En el ejemplo anterior, se crea una directiva de "AtLeast21". Tiene un requisito único&mdash;de una antigüedad mínima, que se suministra como parámetro al requisito.

## <a name="iauthorizationservice"></a>IAuthorizationService 

El servicio principal que determina si la autorización sea correcta es <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService>:

[!code-csharp[](policies/samples/stubs/copy_of_IAuthorizationService.cs?highlight=24-25,48-49&name=snippet)]

El código anterior resalta los dos métodos de la [IAuthorizationService](https://github.com/aspnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationService.cs).

<xref:Microsoft.AspNetCore.Authorization.IAuthorizationRequirement> es un servicio de marcador con ningún método y el mecanismo para realizar el seguimiento de si la autorización sea correcta.

Cada <xref:Microsoft.AspNetCore.Authorization.IAuthorizationHandler> es responsable de comprobar si se cumplen los requisitos:
<!--The following code is a copy/paste from 
https://github.com/aspnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationHandler.cs -->

```csharp
/// <summary>
/// Classes implementing this interface are able to make a decision if authorization
/// is allowed.
/// </summary>
public interface IAuthorizationHandler
{
    /// <summary>
    /// Makes a decision if authorization is allowed.
    /// </summary>
    /// <param name="context">The authorization information.</param>
    Task HandleAsync(AuthorizationHandlerContext context);
}
```

La <xref:Microsoft.AspNetCore.Authorization.AuthorizationHandlerContext> clase es lo que el controlador que se usa para marcar si se cumplen los requisitos:

```csharp
 context.Succeed(requirement)
```

El código siguiente muestra la simplificada (y anotado con comentarios) predeterminado de la implementación del servicio de autorización:

```csharp
public async Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user, 
             object resource, IEnumerable<IAuthorizationRequirement> requirements)
{
    // Create a tracking context from the authorization inputs.
    var authContext = _contextFactory.CreateContext(requirements, user, resource);

    // By default this returns an IEnumerable<IAuthorizationHandlers> from DI.
    var handlers = await _handlers.GetHandlersAsync(authContext);

    // Invoke all handlers.
    foreach (var handler in handlers)
    {
        await handler.HandleAsync(authContext);
    }

    // Check the context, by default success is when all requirements have been met.
    return _evaluator.Evaluate(authContext);
}
```

El código siguiente muestra una típica `ConfigureServices`:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add all of your handlers to DI.
    services.AddSingleton<IAuthorizationHandler, MyHandler1>();
    // MyHandler2, ...

    services.AddSingleton<IAuthorizationHandler, MyHandlerN>();

    // Configure your policies
    services.AddAuthorization(options =>
          options.AddPolicy("Something",
          policy => policy.RequireClaim("Permission", "CanViewPage", "CanViewAnything")));


    services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
}
```

Use <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService> o `[Authorize(Policy = "Something"]` para la autorización.

## <a name="applying-policies-to-mvc-controllers"></a>Aplicar directivas a los controladores MVC

Si está usando las páginas de Razor, consulte [aplicar directivas a las páginas de Razor](#applying-policies-to-razor-pages) en este documento.

Las directivas se aplican a los controladores mediante el uso de la `[Authorize]` atributo con el nombre de la directiva. Por ejemplo:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="applying-policies-to-razor-pages"></a>Aplicar directivas a las páginas de Razor

Las directivas se aplican a las páginas de Razor mediante el uso de la `[Authorize]` atributo con el nombre de la directiva. Por ejemplo:

[!code-csharp[](policies/samples/PoliciesAuthApp2/Pages/AlcoholPurchase.cshtml.cs?name=snippet_AlcoholPurchaseModelClass&highlight=4)]

Las directivas pueden aplicarse también a las páginas de Razor mediante el uso de un [convención autorización](xref:security/authorization/razor-pages-authorization).

## <a name="requirements"></a>Requisitos

Un requisito de autorización es una colección de parámetros de datos que puede usar una directiva para evaluar la entidad de seguridad del usuario actual. En nuestra directiva de "AtLeast21", el requisito es un único parámetro&mdash;la antigüedad mínima. Implementa un requisito [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), que es una interfaz de marcador vacío. Un requisito de antigüedad mínima con parámetros podría implementarse como sigue:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

Si una directiva de autorización contiene varios requisitos de autorización, deben pasar todos los requisitos para la evaluación de directivas se realice correctamente. En otras palabras, se tratan varios requisitos de autorización que se agrega a una sola directiva de autorización en un **AND** base.

> [!NOTE]
> Un requisito no debe tener datos o propiedades.

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a>Controladores de autorización

Un controlador de autorización es responsable de la evaluación de propiedades de un requisito. El controlador de autorización evalúa los requisitos en relación con proporcionado [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) para determinar si se permite el acceso.

Puede tener un requisito [varios controladores](#security-authorization-policies-based-multiple-handlers). Puede heredar un controlador [AuthorizationHandler\<TRequirement >](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), donde `TRequirement` es el requisito para que lo administre. Como alternativa, puede implementar un controlador [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) para administrar más de un tipo de requisito.

### <a name="use-a-handler-for-one-requirement"></a>Usar un controlador para uno de los requisitos

<a name="security-authorization-handler-example"></a>

El siguiente es un ejemplo de una relación uno a uno en el que un controlador de antigüedad mínima utiliza un requisito único:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

El código anterior determina si la entidad de usuario actual tiene una fecha de nacimiento de notificación de que se ha emitido por un emisor conocido y de confianza. No se puede realizar la autorización cuando falta la notificación, en cuyo caso se devuelve una tarea completada. Cuando hay una notificación, se calcula la edad del usuario. Si el usuario cumple la antigüedad mínima definida por el requisito, la autorización se considere correcta. Cuando se realiza correctamente, la autorización `context.Succeed` se invoca con el requisito satisfecho como su único parámetro.

### <a name="use-a-handler-for-multiple-requirements"></a>Usar un controlador para varios requisitos

El siguiente es un ejemplo de una relación uno a varios en el que un controlador de permiso puede controlar los tres tipos diferentes de los requisitos:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/PermissionHandler.cs?name=snippet_PermissionHandlerClass)]

El código anterior recorre [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;una propiedad que contiene los requisitos no marcada como correcta. Para un `ReadPermission` requisito, el usuario debe ser un propietario o un patrocinador para acceder al recurso solicitado. En el caso de un `EditPermission` o `DeletePermission` requisito quien debe ser un propietario para acceder al recurso solicitado.

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a>Registro del controlador

Los controladores se registran en la colección de servicios durante la configuración. Por ejemplo:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=32-33,48-53,61,62-63,66)]

El código anterior registra `MinimumAgeHandler` como un singleton invocando `services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();`. Los controladores pueden registrarse mediante cualquiera de los integrados [duraciones de servicio](xref:fundamentals/dependency-injection#service-lifetimes).

## <a name="what-should-a-handler-return"></a>¿Qué debe devolver un controlador?

Tenga en cuenta que el `Handle` método en el [ejemplo controlador](#security-authorization-handler-example) no devuelve ningún valor. ¿Cómo es un estado de éxito o error indicado?

* Un controlador indica éxito mediante una llamada a `context.Succeed(IAuthorizationRequirement requirement)`, pasando el requisito de que se ha validado correctamente.

* Un controlador no tiene que controlar los errores por lo general, ya que otros controladores para el mismo requisito pueden ser correcto.

* Para garantizar el error, incluso si otros controladores de requisitos se realice correctamente, llame a `context.Fail`.

Si llama un controlador `context.Succeed` o `context.Fail`, todavía se llama a todos los otros controladores. Esto permite que los requisitos producir efectos secundarios, como el registro, que tiene lugar incluso si otro controlador ha validado correctamente o bien no un requisito. Cuando se establece en `false`, [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) propiedad (disponible en ASP.NET Core 1.1 y versiones posterior) provoca un cortocircuito en la ejecución de controladores cuando `context.Fail` se llama. `InvokeHandlersAfterFailure` el valor predeterminado es `true`, en cuyo caso se llama a todos los controladores.

> [!NOTE]
> Incluso si se produce un error de autenticación, se denominan controladores de autorización.

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a>¿Por qué querría varios controladores para un requisito?

En los casos donde desee que la evaluación en un **o** base, implementar varios controladores para un requisito único. Por ejemplo, Microsoft tiene puertas que abre sólo con las tarjetas de clave. Si deja la tarjeta de claves en casa, la recepcionista imprime un adhesivo temporal y abre la puerta. En este escenario, tendría un requisito único, *BuildingEntry*, pero varios controladores, cada uno de ellos examinando un requisito único.

*BuildingEntryRequirement.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

*BadgeEntryHandler.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

*TemporaryStickerHandler.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

Asegúrese de que ambos controladores estén [registrado](xref:security/authorization/policies#security-authorization-policies-based-handler-registration). Si cualquier controlador se ejecuta correctamente cuando una directiva se evalúa como el `BuildingEntryRequirement`, la evaluación de directivas se realiza correctamente.

## <a name="using-a-func-to-fulfill-a-policy"></a>Uso de func para satisfacer una directiva

Puede haber situaciones en que cumplir una directiva es sencilla expresar en código. Es posible proporcionar un `Func<AuthorizationHandlerContext, bool>` al configurar la directiva con el `RequireAssertion` el generador de directiva.

Por ejemplo, el anterior `BadgeEntryHandler` podría volver a escribirse como sigue:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=50-51,55-61)]

## <a name="accessing-mvc-request-context-in-handlers"></a>Acceso al contexto de solicitud MVC en controladores

El `HandleRequirementAsync` método se implementa en un controlador de autorización tiene dos parámetros: un `AuthorizationHandlerContext` y `TRequirement` está controlando. Marcos de trabajo como MVC o Jabbr son gratuitas agregar cualquier objeto a la `Resource` propiedad en el `AuthorizationHandlerContext` para pasar información adicional.

Por ejemplo, MVC pasa una instancia de [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) en el `Resource` propiedad. Esta propiedad proporciona acceso a `HttpContext`, `RouteData`y todo lo contrario, proporcionada por MVC y páginas de Razor.

El uso de la `Resource` propiedad es el marco específico. Uso de la información la `Resource` propiedad limita sus directivas de autorización para marcos de trabajo determinados. Primero debe convertir el `Resource` propiedad mediante la `is` palabra clave y, a continuación, confirme la conversión se realizó correctamente para asegurarse de que no se bloquee el código con un `InvalidCastException` cuando se ejecuta en otros marcos de trabajo:

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```
