---
title: "Autorización personalizada basada en directivas en ASP.NET Core"
author: rick-anderson
description: "Obtenga información acerca de cómo crear y usar controladores de directiva de autorización personalizada para exigir requisitos de autorización en una aplicación de ASP.NET Core."
ms.author: riande
ms.custom: mvc
manager: wpickett
ms.date: 11/21/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/policies
ms.openlocfilehash: 1067e97dd6e71021929aa3690b0c3f5bfc6c9724
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2018
---
# <a name="custom-policy-based-authorization"></a>Autorización personalizada basada en directivas

Interiormente, [autorización basada en roles](xref:security/authorization/roles) y [autorización basada en notificaciones](xref:security/authorization/claims) utilizan un requisito, un controlador de requisito y una directiva configurada previamente. Estos bloques de creación se admite la expresión de evaluaciones de autorización en el código. El resultado es una estructura de autorización más enriquecida, reutilizables, comprobable.

Una directiva de autorización está formada por uno o más requisitos. Se registra como parte de la configuración de servicio de autorización, en la `ConfigureServices` método de la `Startup` clase:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63,72)]

En el ejemplo anterior, se crea una directiva de "AtLeast21". Tiene un requisito único, que de una antigüedad mínima, que se proporciona como un parámetro al requisito.

Las directivas se aplican mediante el `[Authorize]` atributo con el nombre de la directiva. Por ejemplo:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="requirements"></a>Requisitos

Un requisito de autorización es una colección de parámetros de datos que puede usar una directiva para evaluar la entidad de seguridad del usuario actual. En nuestra directiva de "AtLeast21", el requisito es un parámetro único&mdash;la antigüedad mínima. Implementa un requisito `IAuthorizationRequirement`, que es una interfaz de marcador vacío. Un requisito de antigüedad mínima con parámetros podría implementarse como sigue:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

> [!NOTE]
> No tiene un requisito que tienen datos o propiedades.

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a>Controladores de autorización

Un controlador de autorización es responsable de la evaluación de propiedades de un requisito. El controlador de autorización evalúa los requisitos con proporcionado `AuthorizationHandlerContext` para determinar si se permite el acceso. Puede tener un requisito [varios controladores](#security-authorization-policies-based-multiple-handlers). Heredan de controladores `AuthorizationHandler<T>`, donde `T` es el requisito para procesarse.

<a name="security-authorization-handler-example"></a>

El controlador de antigüedad mínima podría ser similar al siguiente:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

El código anterior determina si la entidad de seguridad del usuario actual tiene una fecha de nacimiento de notificación de que se ha emitido por un emisor conocido y de confianza. No se puede realizar la autorización cuando falta la notificación, en cuyo caso se devuelve una tarea completa. Cuando está presente una notificación, se calcula la edad del usuario. Si el usuario cumple con la antigüedad mínima definida por el requisito, la autorización se considere correcta. Cuando se realiza correctamente, la autorización `context.Succeed` se invoca con el requisito satisfecho como un parámetro.

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a>Registro del controlador

Los controladores se registran en la colección de servicios durante la configuración. Por ejemplo:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63-65,72)]

Cada controlador se agrega a la colección de servicios mediante la invocación de `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();`.

## <a name="what-should-a-handler-return"></a>¿Qué debe devolver un controlador?

Tenga en cuenta que la `Handle` método en el [controlador (ejemplo)](#security-authorization-handler-example) no devuelve ningún valor. ¿Cómo es un estado de correcto o erróneo indicadas?

* Un controlador indica éxito mediante una llamada a `context.Succeed(IAuthorizationRequirement requirement)`, pasando el requisito de que se ha validado correctamente.

* Un controlador no necesita controlar los errores por lo general, como otros controladores para el mismo requisito pueden realizarse correctamente.

* Para garantizar el error, incluso si otros controladores de requisito correctamente, llame a `context.Fail`.

Independientemente de lo que se llama a dentro de su controlador, todos los controladores para un requisito se llamará cuando una directiva exige el requisito. Esto permite requisitos tienen efectos secundarios, como el registro, que siempre se llevará a cabo aunque `context.Fail()` se ha llamado en otro controlador.

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a>¿Por qué desearía varios controladores para un requisito?

En casos donde probablemente prefiera evaluación en un **o** base, implementar varios controladores para un requisito único. Por ejemplo, Microsoft tiene puertas que solo se abren con tarjetas de clave. Si deja la tarjeta de claves en casa, la recepcionista imprime una etiqueta temporal y abre la puerta para usted. En este escenario, tendría un requisito único, *BuildingEntry*, pero varios controladores, cada uno de ellos examinando un requisito único.

*BuildingEntryRequirement.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

*BadgeEntryHandler.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

*TemporaryStickerHandler.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

Asegúrese de que ambos controladores estén [registrado](xref:security/authorization/policies#security-authorization-policies-based-handler-registration). Si cualquier controlador se ejecuta correctamente cuando una directiva se evalúa como el `BuildingEntryRequirement`, la evaluación de directivas se realiza correctamente.

## <a name="using-a-func-to-fulfill-a-policy"></a>Usar un elemento func para cumplir una directiva

Puede haber situaciones en las que cumplir una directiva es simple expresar en el código. Es posible proporcionar un `Func<AuthorizationHandlerContext, bool>` al configurar la directiva con el `RequireAssertion` el generador de directiva.

Por ejemplo, el anterior `BadgeEntryHandler` podría volver a escribir como se indica a continuación:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=52-53,57-63)]

## <a name="accessing-mvc-request-context-in-handlers"></a>Acceso al contexto de solicitud MVC en los controladores

El `HandleRequirementAsync` método se implementa en un controlador de autorización tiene dos parámetros: una `AuthorizationHandlerContext` y `TRequirement` está controlando. Marcos de trabajo como MVC o Jabbr pueden agregar cualquier objeto a la `Resource` propiedad en el `AuthorizationHandlerContext` para pasar información adicional.

Por ejemplo, MVC pasa una instancia de [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) en el `Resource` propiedad. Esta propiedad proporciona acceso a `HttpContext`, `RouteData`y todo el contenido más proporcionó MVC y las páginas de Razor.

El uso de la `Resource` propiedad es específicos de la plataforma. De manera indicada en el `Resource` propiedad limita las directivas de autorización para marcos de trabajo determinados. Debe convertir el `Resource` propiedad mediante la `as` (palabra clave) y, a continuación, confirme la conversión se realizará correctamente para asegurarse de que el código de no bloqueo con un `InvalidCastException` cuando se ejecuta en otros marcos de trabajo:

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```
