---
title: Proveedores de directiva de autorización personalizada en ASP.NET Core
author: mjrousos
description: Obtenga información acerca de cómo usar un IAuthorizationPolicyProvider personalizado en una aplicación de ASP.NET Core para generar dinámicamente las directivas de autorización.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/02/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/iauthorizationpolicyprovider
ms.openlocfilehash: a5bad88b37d38b819b960b1eb27808d891268c01
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/17/2018
---
# <a name="custom-authorization-policy-providers-using-iauthorizationpolicyprovider-in-aspnet-core"></a>Proveedores personalizados de directiva de autorización mediante IAuthorizationPolicyProvider en ASP.NET Core 

Por [Mike Rousos](https://github.com/mjrousos)

Normalmente cuando se usa [autorización basada en directivas](xref:security/authorization/policies), las directivas se registran mediante una llamada a `AuthorizationOptions.AddPolicy` como parte de la configuración del servicio de autorización. En algunos casos, puede no ser posible (o deseable) para registrar todas las directivas de autorización de esta manera. En esos casos, puede utilizar un personalizado `IAuthorizationPolicyProvider` para controlar cómo se proporcionan las directivas de autorización.

Algunos ejemplos de escenarios donde un personalizado [IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) puede ser útil incluir:

* Uso de un servicio externo para proporcionar la evaluación de la directiva.
* Con un amplio rango de directivas (para los números de sala diferente o edades, por ejemplo), por lo que no tiene sentido agregar cada directiva de autorización individuales con un `AuthorizationOptions.AddPolicy` llamar.
* Creación de directivas en tiempo de ejecución basándose en la información de un origen de datos externo (por ejemplo, una base de datos) o determinar los requisitos de autorización dinámicamente mediante otro mecanismo.

## <a name="customizing-policy-retrieval"></a>Personalizar la recuperación de directivas

Aplicaciones de ASP.NET Core utilizan una implementación de la `IAuthorizationPolicyProvider` interfaz para recuperar las directivas de autorización. De forma predeterminada, [DefaultAuthorizationPolicyProvider](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) está registrado y que usa. `DefaultAuthorizationPolicyProvider` Devuelve las directivas de la `AuthorizationOptions` proporcionado en una `IServiceCollection.AddAuthorization` llamar.

Puede personalizar este comportamiento registrando una diferentes `IAuthorizationPolicyProvider` implementación en la aplicación [inyección de dependencia](xref:fundamentals/dependency-injection) contenedor. 

El `IAuthorizationPolicyProvider` interfaz contiene dos API:

* [GetPolicyAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) devuelve una directiva de autorización para un nombre concreto.
* [GetDefaultPolicyAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync?view=aspnetcore-2.0) devuelve la directiva de autorización de forma predeterminada (la directiva utilizada para `[Authorize]` atributos sin una directiva especificada). 

Mediante la implementación de estas dos API, puede personalizar cómo se proporcionan las directivas de autorización.

## <a name="parameterized-authorize-attribute-example"></a>Parámetros autorizar el ejemplo de atributo

Un escenario donde `IAuthorizationPolicyProvider` es útil es habilitar personalizado `[Authorize]` atributos cuyos requisitos dependen de un parámetro. Por ejemplo, en [autorización basada en directivas](xref:security/authorization/policies) documentación, un edades ("AtLeast21") se utiliza la directiva como un ejemplo. Si las acciones de controlador diferentes en una aplicación deben estar disponibles para los usuarios de *diferentes* edades, puede ser útil tener muchas directivas diferentes edades. En vez de registrar todas las diferentes edades directivas que necesita la aplicación en `AuthorizationOptions`, puede generar las directivas de forma dinámica con un personalizado `IAuthorizationPolicyProvider`. Para hacer con las directivas más fácil, puede anotar acciones con el atributo de autorización personalizada como `[MinimumAgeAuthorize(20)]`.

## <a name="custom-authorization-attributes"></a>Atributos de autorización personalizada

Las directivas de autorización se identifican por sus nombres. Personalizado `MinimumAgeAuthorizeAttribute` descritos anteriormente se necesita asignar argumentos en una cadena que puede usarse para recuperar la directiva de autorización correspondiente. Puede hacerlo mediante la derivación de `AuthorizeAttribute` y realizar la `Age` encapsulado propiedad el `AuthorizeAttribute.Policy` propiedad.

```CSharp
internal class MinimumAgeAuthorizeAttribute : AuthorizeAttribute
{
    const string POLICY_PREFIX = "MinimumAge";

    public MinimumAgeAuthorizeAttribute(int age) => Age = age;

    // Get or set the Age property by manipulating the underlying Policy property
    public int Age
    {
        get
        {
            if (int.TryParse(Policy.Substring(POLICY_PREFIX.Length), out var age))
            {
                return age;
            }
            return default(int);
        }
        set
        {
            Policy = $"{POLICY_PREFIX}{value.ToString()}";
        }
    }
}
```

Este tipo de atributo tiene un `Policy` cadena según el prefijo codificado de forma rígida (`"MinimumAge"`) y un entero pasa mediante el constructor.

Se puede aplicar a las acciones de la misma manera que otras `Authorize` atributos salvo por que utiliza un entero como un parámetro.

```CSharp
[MinimumAgeAuthorize(10)]
public IActionResult RequiresMinimumAge10()
```

## <a name="custom-iauthorizationpolicyprovider"></a>IAuthorizationPolicyProvider personalizada

Personalizado `MinimumAgeAuthorizeAttribute` facilita la directivas de solicitud de autorización para cualquier antigüedad mínima deseado. El siguiente problema al resolver es asegurarse de que las directivas de autorización están disponibles para todos los diferentes edades. Aquí es donde un `IAuthorizationPolicyProvider` es útil.

Cuando se usa `MinimumAgeAuthorizationAttribute`, los nombres de directiva de autorización sigue el patrón de `"MinimumAge" + Age`, por lo que la opción de instalación `IAuthorizationPolicyProvider` debe generar directivas de autorización por:

* Analizar la edad de nombre de la directiva.
* Usar `AuthorizationPolicyBuiler` para crear un nuevo `AuthorizationPolicy`
* Agregar requisitos a la directiva en función de la antigüedad con `AuthorizationPolicyBuilder.AddRequirements`. En otros escenarios, puede usar `RequireClaim`, `RequireRole`, o `RequireUserName` en su lugar.

```CSharp
internal class MinimumAgePolicyProvider : IAuthorizationPolicyProvider
{
    const string POLICY_PREFIX = "MinimumAge";

    // Policies are looked up by string name, so expect 'parameters' (like age)
    // to be embedded in the policy names. This is abstracted away from developers
    // by the more strongly-typed attributes derived from AuthorizeAttribute
    // (like [MinimumAgeAuthorize()] in this sample)
    public Task<AuthorizationPolicy> GetPolicyAsync(string policyName)
    {
        if (policyName.StartsWith(POLICY_PREFIX, StringComparison.OrdinalIgnoreCase) &&
            int.TryParse(policyName.Substring(POLICY_PREFIX.Length), out var age))
        {
            var policy = new AuthorizationPolicyBuilder();
            policy.AddRequirements(new MinimumAgeRequirement(age));
            return Task.FromResult(policy.Build());
        }

        return Task.FromResult<AuthorizationPolicy>(null);
    }
}
```

## <a name="multiple-authorization-policy-providers"></a>Varios proveedores de directiva de autorización

Cuando se usa personalizado `IAuthorizationPolicyProvider` implementaciones, tenga en cuenta que ASP.NET Core solo usa una instancia de `IAuthorizationPolicyProvider`. Si un proveedor personalizado no es capaz de proporcionar las directivas de autorización para todos los nombres de directiva, debe recurrir a un proveedor de copia de seguridad. Nombres de las directivas podrían incluir aquellos que proceden de una directiva predeterminada para `[Authorize]` atributos sin nombre.

Por ejemplo, considere la posibilidad de que una aplicación necesita directivas de antigüedad personalizado y más tradicional de recuperación de directiva basada en roles. Dicha aplicación podría usar un proveedor de directivas de autorización personalizada que:

* Intenta analizar los nombres de las directivas. 
* Llama a un proveedor de directivas diferentes (como `DefaultAuthorizationPolicyProvider`) si el nombre de directiva no contiene una edad.

## <a name="default-policy"></a>Directiva predeterminada

Además de proporcionar directivas de autorización con nombre personalizado `IAuthorizationPolicyProvider` debe implementar `GetDefaultPolicyAsync` para proporcionar una directiva de autorización para `[Authorize]` atributos sin un nombre de directiva especificado.

En muchos casos, este atributo de autorización solo requiere un usuario autenticado, por lo que puede hacer que la directiva necesaria con una llamada a `RequireAuthenticatedUser`:

```CSharp
public Task<AuthorizationPolicy> GetDefaultPolicyAsync() => 
    Task.FromResult(new AuthorizationPolicyBuilder().RequireAuthenticatedUser().Build());
```

Al igual que con todos los aspectos de un personalizado `IAuthorizationPolicyProvider`, puede personalizarlo, según sea necesario. En algunos casos:

* No puede usar directivas de autorización de forma predeterminada.
* Recuperar la directiva predeterminada se puede delegar en una acción de reserva `IAuthorizationPolicyProvider`.

## <a name="using-a-custom-iauthorizationpolicyprovider"></a>Usar un IAuthorizationPolicyProvider personalizado

Para usar directivas personalizadas de un `IAuthorizationPolicyProvider`, debe:

* Registrar la correspondiente `AuthorizationHandler` tipos con inserción de dependencias (se describe en [autorización basada en directivas](xref:security/authorization/policies#authorization-handlers)), al igual que con todos los escenarios de autorización basada en directivas.
* Registrar personalizado `IAuthorizationPolicyProvider` tipo de colección de servicio de inyección de dependencia de la aplicación (en `Startup.ConfigureServices`) para reemplazar el proveedor de directivas predeterminado.

```CSharp
services.AddTransient<IAuthorizationPolicyProvider, MinimumAgePolicyProvider>();
```

Un personalizado completo `IAuthorizationPolicyProvider` ejemplo está disponible en la [repositorio de GitHub de aspnet/AuthSamples](https://github.com/aspnet/AuthSamples/tree/dev/samples/CustomPolicyProvider).
