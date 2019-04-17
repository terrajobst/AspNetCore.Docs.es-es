---
title: Proveedores de directivas de autorización personalizado en ASP.NET Core
author: mjrousos
description: Obtenga información sobre cómo usar un IAuthorizationPolicyProvider personalizado en una aplicación ASP.NET Core para generar dinámicamente las directivas de autorización.
ms.author: riande
ms.custom: mvc
ms.date: 04/15/2019
uid: security/authorization/iauthorizationpolicyprovider
ms.openlocfilehash: e17372bb0ec9091c385a70b1e907eaa3cff24003
ms.sourcegitcommit: 017b673b3c700d2976b77201d0ac30172e2abc87
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2019
ms.locfileid: "59614414"
---
# <a name="custom-authorization-policy-providers-using-iauthorizationpolicyprovider-in-aspnet-core"></a>Proveedores personalizados de directiva de autorización mediante IAuthorizationPolicyProvider en ASP.NET Core 

Por [Mike Rousos](https://github.com/mjrousos)

Normalmente, cuando se usa [autorización basada en directivas](xref:security/authorization/policies), las directivas se registran mediante una llamada a `AuthorizationOptions.AddPolicy` como parte de la configuración del servicio de autorización. En algunos escenarios, puede no ser posible (o deseable) para registrar todas las directivas de autorización de esta manera. En esos casos, puede utilizar una personalizada `IAuthorizationPolicyProvider` para controlar cómo se proporcionan las directivas de autorización.

Ejemplos de escenarios donde un personalizado [IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) puede resultar útil incluir:

* Usar un servicio externo para proporcionar evaluación de directivas.
* Con un intervalo amplio de directivas (para los números de sala diferente o edades, por ejemplo), por lo que no tiene sentido agregar cada directiva de autorización individuales con un `AuthorizationOptions.AddPolicy` llamar.
* Creación de directivas en tiempo de ejecución basándose en la información de origen de datos externo (por ejemplo, una base de datos) o determinar los requisitos de autorización de forma dinámica mediante otro mecanismo.

[Ver o descargar el código de ejemplo](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider) desde el [repositorio de AspNetCore GitHub](https://github.com/aspnet/AspNetCore). Descargue el archivo ZIP de repositorio aspnet/AspNetCore. Descomprima el archivo. Navegue hasta la *src/seguridad/samples/CustomPolicyProvider* carpeta del proyecto.

## <a name="customize-policy-retrieval"></a>Personalizar la recuperación de directiva

Las aplicaciones de ASP.NET Core usan una implementación de la `IAuthorizationPolicyProvider` interfaz para recuperar las directivas de autorización. De forma predeterminada, [DefaultAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) se registra y se utiliza. `DefaultAuthorizationPolicyProvider` Devuelve las directivas de la `AuthorizationOptions` proporcionado en un `IServiceCollection.AddAuthorization` llamar.

Puede personalizar este comportamiento mediante el registro de otro `IAuthorizationPolicyProvider` implementación en la aplicación [inserción de dependencias](xref:fundamentals/dependency-injection) contenedor. 

El `IAuthorizationPolicyProvider` interfaz contiene dos API:

* [GetPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) devuelve una directiva de autorización para un nombre concreto.
* [GetDefaultPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync) devuelve la directiva de autorización de forma predeterminada (la directiva utilizada para `[Authorize]` atributos sin una directiva especificada). 

Mediante la implementación de estas dos API, puede personalizar cómo se proporcionan las directivas de autorización.

## <a name="parameterized-authorize-attribute-example"></a>Parametrizar autorizar el ejemplo de atributo

Un escenario donde `IAuthorizationPolicyProvider` es útil es personalizado para permitir `[Authorize]` atributos cuyos requisitos dependen de un parámetro. Por ejemplo, en [autorización basada en directivas](xref:security/authorization/policies) documentación, un edades ("AtLeast21") se utiliza la directiva como un ejemplo. Si las acciones de controlador diferente en una aplicación deben estar disponibles para los usuarios de *diferentes* edades, podría ser útil tener muchas directivas diferentes edades. En vez de registrar todas las diferentes edades directivas que la aplicación necesitará en `AuthorizationOptions`, puede generar las directivas de forma dinámica con un personalizado `IAuthorizationPolicyProvider`. Para hacer con las directivas más fácil, puede anotar las acciones con el atributo de autorización personalizado como `[MinimumAgeAuthorize(20)]`.

## <a name="custom-authorization-attributes"></a>Atributos personalizados de autorización

Las directivas de autorización se identifican por sus nombres. Personalizado `MinimumAgeAuthorizeAttribute` descrito anteriormente, debe asignar argumentos en una cadena que puede usarse para recuperar la directiva de autorización correspondiente. Puede hacerlo mediante la derivación de `AuthorizeAttribute` y realizar el `Age` encapsulado propiedad el `AuthorizeAttribute.Policy` propiedad.

```csharp
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

Este tipo de atributo tiene un `Policy` cadena según el prefijo codificado de forma rígida (`"MinimumAge"`) y un entero pasa a través del constructor.

Se puede aplicar a acciones en la misma manera que otras `Authorize` atributos salvo que toma un entero como parámetro.

```csharp
[MinimumAgeAuthorize(10)]
public IActionResult RequiresMinimumAge10()
```

## <a name="custom-iauthorizationpolicyprovider"></a>IAuthorizationPolicyProvider personalizado

Personalizado `MinimumAgeAuthorizeAttribute` facilita a las directivas de autorización de solicitud para todas las edades mínima deseada. El siguiente problema para resolver es asegurarse de que las directivas de autorización están disponibles para todos esas diferentes edades. Aquí es donde un `IAuthorizationPolicyProvider` es útil.

Cuando se usa `MinimumAgeAuthorizationAttribute`, los nombres de directiva de autorización seguirá el patrón `"MinimumAge" + Age`, por lo que personalizado `IAuthorizationPolicyProvider` debe generar directivas de autorización por:

* La antigüedad del nombre de la directiva de análisis.
* Uso de `AuthorizationPolicyBuilder` para crear un nuevo `AuthorizationPolicy`
* Agregar requisitos a la directiva según la edad con `AuthorizationPolicyBuilder.AddRequirements`. En otros escenarios, puede usar `RequireClaim`, `RequireRole`, o `RequireUserName` en su lugar.

```csharp
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

Al usar custom `IAuthorizationPolicyProvider` implementaciones, tenga en cuenta que ASP.NET Core solo usa una instancia de `IAuthorizationPolicyProvider`. Si un proveedor personalizado no es capaz de proporcionar directivas de autorización para todos los nombres de directiva que se utilizará, debe recurrir a un proveedor de copia de seguridad. 

Por ejemplo, considere una aplicación que necesita directivas personalizadas de edad y la recuperación de directivas basado en roles más tradicional. Este tipo de aplicación podría usar un proveedor de directivas de autorización personalizado que:

* Intenta analizar los nombres de directiva. 
* Las llamadas a un proveedor de directivas diferentes (como `DefaultAuthorizationPolicyProvider`) si el nombre de la directiva no contiene una edad.

El ejemplo `IAuthorizationPolicyProvider` implementación mostrado anteriormente se puede actualizar para usar el `DefaultAuthorizationPolicyProvider` mediante la creación de un proveedor de directivas de reserva en su constructor (que se usará en caso de que el nombre de la directiva no coincide con su patrón esperado de 'MinimumAge' + edad).

```csharp
private DefaultAuthorizationPolicyProvider FallbackPolicyProvider { get; }

public MinimumAgePolicyProvider(IOptions<AuthorizationOptions> options)
{
    // ASP.NET Core only uses one authorization policy provider, so if the custom implementation
    // doesn't handle all policies it should fall back to an alternate provider.
    FallbackPolicyProvider = new DefaultAuthorizationPolicyProvider(options);
}
```

A continuación, la `GetPolicyAsync` método puede actualizarse para utilizar el `FallbackPolicyProvider` en lugar de devolver null:

```csharp
...
return FallbackPolicyProvider.GetPolicyAsync(policyName);
```

## <a name="default-policy"></a>Directiva predeterminada

Además de proporcionar directivas de autorización con nombre, un personalizado `IAuthorizationPolicyProvider` debe implementar `GetDefaultPolicyAsync` para proporcionar una directiva de autorización para `[Authorize]` atributos sin un nombre de directiva especificado.

En muchos casos, este atributo de autorización solo requiere un usuario autenticado, por lo que puede realizar la directiva necesaria con una llamada a `RequireAuthenticatedUser`:

```csharp
public Task<AuthorizationPolicy> GetDefaultPolicyAsync() => 
    Task.FromResult(new AuthorizationPolicyBuilder().RequireAuthenticatedUser().Build());
```

Como ocurre con todos los aspectos de un personalizado `IAuthorizationPolicyProvider`, puede personalizar esto, según sea necesario. En algunos casos, sería conveniente para recuperar la directiva predeterminada de una acción de reserva `IAuthorizationPolicyProvider`.

## <a name="required-policy"></a>Directiva requerida

Personalizada `IAuthorizationPolicyProvider` debe implementar `GetRequiredPolicyAsync` para, opcionalmente, proporcione una directiva que siempre es necesaria. Si `GetRequiredPolicyAsync` devuelve una directiva no nula, esa directiva se combina con cualquier otro (designado o predeterminado) directiva que se solicita.

Si no se necesita ninguna directiva necesaria, el proveedor puede devolver un valor nulo o aplazar el proveedor de reserva:

```csharp
public Task<AuthorizationPolicy> GetRequiredPolicyAsync() => 
    Task.FromResult<AuthorizationPolicy>(null);
```

## <a name="use-a-custom-iauthorizationpolicyprovider"></a>Usar un IAuthorizationPolicyProvider personalizado

Para usar directivas personalizadas de un `IAuthorizationPolicyProvider`, debe:

* Registrar adecuado `AuthorizationHandler` tipos con inserción de dependencias (se describe en [autorización basada en directivas](xref:security/authorization/policies#authorization-handlers)), al igual que con todos los escenarios de autorización basada en directivas.
* Registrar personalizado `IAuthorizationPolicyProvider` tipo de colección de servicios de inyección de dependencia de la aplicación (en `Startup.ConfigureServices`) para reemplazar el proveedor de directivas predeterminado.

```csharp
services.AddSingleton<IAuthorizationPolicyProvider, MinimumAgePolicyProvider>();
```

Un completo personalizado `IAuthorizationPolicyProvider` ejemplo está disponible en el [repositorio de GitHub de aspnet/AuthSamples](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider).
