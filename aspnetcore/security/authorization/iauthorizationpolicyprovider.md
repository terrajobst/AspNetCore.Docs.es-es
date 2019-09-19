---
title: Proveedores de directivas de autorización personalizados en ASP.NET Core
author: mjrousos
description: Obtenga información sobre cómo usar un IAuthorizationPolicyProvider personalizado en una aplicación ASP.NET Core para generar dinámicamente directivas de autorización.
ms.author: riande
ms.custom: mvc
ms.date: 04/15/2019
uid: security/authorization/iauthorizationpolicyprovider
ms.openlocfilehash: b11f7f94e1042e65d5f2ff8ab0fe9ea7838bebeb
ms.sourcegitcommit: b1e480e1736b0fe0e4d8dce4a4cf5c8e47fc2101
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/19/2019
ms.locfileid: "71108061"
---
# <a name="custom-authorization-policy-providers-using-iauthorizationpolicyprovider-in-aspnet-core"></a><span data-ttu-id="9e541-103">Proveedores de directivas de autorización personalizada mediante IAuthorizationPolicyProvider en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9e541-103">Custom Authorization Policy Providers using IAuthorizationPolicyProvider in ASP.NET Core</span></span> 

<span data-ttu-id="9e541-104">Por [Mike Rousos](https://github.com/mjrousos)</span><span class="sxs-lookup"><span data-stu-id="9e541-104">By [Mike Rousos](https://github.com/mjrousos)</span></span>

<span data-ttu-id="9e541-105">Normalmente, cuando se usa [la autorización basada en directivas](xref:security/authorization/policies), las directivas `AuthorizationOptions.AddPolicy` se registran llamando como parte de la configuración del servicio de autorización.</span><span class="sxs-lookup"><span data-stu-id="9e541-105">Typically when using [policy-based authorization](xref:security/authorization/policies), policies are registered by calling `AuthorizationOptions.AddPolicy` as part of authorization service configuration.</span></span> <span data-ttu-id="9e541-106">En algunos escenarios, puede que no sea posible (o deseable) registrar todas las directivas de autorización de esta manera.</span><span class="sxs-lookup"><span data-stu-id="9e541-106">In some scenarios, it may not be possible (or desirable) to register all authorization policies in this way.</span></span> <span data-ttu-id="9e541-107">En esos casos, puede usar un personalizado `IAuthorizationPolicyProvider` para controlar cómo se proporcionan las directivas de autorización.</span><span class="sxs-lookup"><span data-stu-id="9e541-107">In those cases, you can use a custom `IAuthorizationPolicyProvider` to control how authorization policies are supplied.</span></span>

<span data-ttu-id="9e541-108">Algunos ejemplos de escenarios en los que un [IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) personalizado puede ser útil son:</span><span class="sxs-lookup"><span data-stu-id="9e541-108">Examples of scenarios where a custom [IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) may be useful include:</span></span>

* <span data-ttu-id="9e541-109">Uso de un servicio externo para proporcionar evaluación de directivas.</span><span class="sxs-lookup"><span data-stu-id="9e541-109">Using an external service to provide policy evaluation.</span></span>
* <span data-ttu-id="9e541-110">Usar una gran variedad de directivas (por ejemplo, para diferentes números de sala o edades), por lo que no tiene sentido agregar cada directiva de autorización individual `AuthorizationOptions.AddPolicy` con una llamada.</span><span class="sxs-lookup"><span data-stu-id="9e541-110">Using a large range of policies (for different room numbers or ages, for example), so it doesn’t make sense to add each individual authorization policy with an `AuthorizationOptions.AddPolicy` call.</span></span>
* <span data-ttu-id="9e541-111">Crear directivas en tiempo de ejecución en función de la información de un origen de datos externo (por ejemplo, una base de datos) o de determinar dinámicamente los requisitos de autorización mediante otro mecanismo.</span><span class="sxs-lookup"><span data-stu-id="9e541-111">Creating policies at runtime based on information in an external data source (like a database) or determining authorization requirements dynamically through another mechanism.</span></span>

<span data-ttu-id="9e541-112">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider) del [repositorio de github de AspNetCore](https://github.com/aspnet/AspNetCore).</span><span class="sxs-lookup"><span data-stu-id="9e541-112">[View or download sample code](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider) from the [AspNetCore GitHub repository](https://github.com/aspnet/AspNetCore).</span></span> <span data-ttu-id="9e541-113">Descargue el archivo ZIP del repositorio ASPNET/AspNetCore.</span><span class="sxs-lookup"><span data-stu-id="9e541-113">Download the aspnet/AspNetCore repository ZIP file.</span></span> <span data-ttu-id="9e541-114">Descomprima el archivo.</span><span class="sxs-lookup"><span data-stu-id="9e541-114">Unzip the file.</span></span> <span data-ttu-id="9e541-115">Navegue a la carpeta de proyecto *src/Security/samples/CustomPolicyProvider* .</span><span class="sxs-lookup"><span data-stu-id="9e541-115">Navigate to the *src/Security/samples/CustomPolicyProvider* project folder.</span></span>

## <a name="customize-policy-retrieval"></a><span data-ttu-id="9e541-116">Personalizar la recuperación de directivas</span><span class="sxs-lookup"><span data-stu-id="9e541-116">Customize policy retrieval</span></span>

<span data-ttu-id="9e541-117">ASP.net Core aplicaciones usan una implementación de la `IAuthorizationPolicyProvider` interfaz para recuperar las directivas de autorización.</span><span class="sxs-lookup"><span data-stu-id="9e541-117">ASP.NET Core apps use an implementation of the `IAuthorizationPolicyProvider` interface to retrieve authorization policies.</span></span> <span data-ttu-id="9e541-118">De forma predeterminada, [DefaultAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) se registra y se usa.</span><span class="sxs-lookup"><span data-stu-id="9e541-118">By default, [DefaultAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) is registered and used.</span></span> <span data-ttu-id="9e541-119">`DefaultAuthorizationPolicyProvider`Devuelve las directivas del `AuthorizationOptions` proporcionado en una `IServiceCollection.AddAuthorization` llamada.</span><span class="sxs-lookup"><span data-stu-id="9e541-119">`DefaultAuthorizationPolicyProvider` returns policies from the `AuthorizationOptions` provided in an `IServiceCollection.AddAuthorization` call.</span></span>

<span data-ttu-id="9e541-120">Puede personalizar este comportamiento registrando una implementación diferente `IAuthorizationPolicyProvider` en el contenedor de inserción de [dependencias](xref:fundamentals/dependency-injection) de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="9e541-120">You can customize this behavior by registering a different `IAuthorizationPolicyProvider` implementation in the app’s [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> 

<span data-ttu-id="9e541-121">La `IAuthorizationPolicyProvider` interfaz contiene dos API:</span><span class="sxs-lookup"><span data-stu-id="9e541-121">The `IAuthorizationPolicyProvider` interface contains two APIs:</span></span>

* <span data-ttu-id="9e541-122">[GetPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) devuelve una directiva de autorización para un nombre determinado.</span><span class="sxs-lookup"><span data-stu-id="9e541-122">[GetPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) returns an authorization policy for a given name.</span></span>
* <span data-ttu-id="9e541-123">[GetDefaultPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync) devuelve la Directiva de autorización predeterminada (la Directiva que `[Authorize]` se usa para los atributos sin una directiva especificada).</span><span class="sxs-lookup"><span data-stu-id="9e541-123">[GetDefaultPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync) returns the default authorization policy (the policy used for `[Authorize]` attributes without a policy specified).</span></span> 

<span data-ttu-id="9e541-124">Mediante la implementación de estas dos API, puede personalizar cómo se proporcionan las directivas de autorización.</span><span class="sxs-lookup"><span data-stu-id="9e541-124">By implementing these two APIs, you can customize how authorization policies are provided.</span></span>

## <a name="parameterized-authorize-attribute-example"></a><span data-ttu-id="9e541-125">Ejemplo de atributo Authorize parametrizado</span><span class="sxs-lookup"><span data-stu-id="9e541-125">Parameterized authorize attribute example</span></span>

<span data-ttu-id="9e541-126">Un escenario en `IAuthorizationPolicyProvider` el que resulta útil es `[Authorize]` habilitar atributos personalizados cuyos requisitos dependen de un parámetro.</span><span class="sxs-lookup"><span data-stu-id="9e541-126">One scenario where `IAuthorizationPolicyProvider` is useful is enabling custom `[Authorize]` attributes whose requirements depend on a parameter.</span></span> <span data-ttu-id="9e541-127">Por ejemplo, en la documentación de [autorización basada en directivas](xref:security/authorization/policies) , se usó una directiva basada en la edad ("AtLeast21") como ejemplo.</span><span class="sxs-lookup"><span data-stu-id="9e541-127">For example, in [policy-based authorization](xref:security/authorization/policies) documentation, an age-based (“AtLeast21”) policy was used as a sample.</span></span> <span data-ttu-id="9e541-128">Si hay diferentes acciones de controlador en una aplicación que deben estar disponibles para los usuarios de *diferentes* edades, podría ser útil tener muchas directivas diferentes basadas en la edad.</span><span class="sxs-lookup"><span data-stu-id="9e541-128">If different controller actions in an app should be made available to users of *different* ages, it might be useful to have many different age-based policies.</span></span> <span data-ttu-id="9e541-129">En lugar de registrar todas las directivas basadas en la edad diferentes que necesitará la aplicación en `AuthorizationOptions`, puede generar las directivas dinámicamente con un personalizado. `IAuthorizationPolicyProvider`</span><span class="sxs-lookup"><span data-stu-id="9e541-129">Instead of registering all the different age-based policies that the application will need in `AuthorizationOptions`, you can generate the policies dynamically with a custom `IAuthorizationPolicyProvider`.</span></span> <span data-ttu-id="9e541-130">Para facilitar el uso de las directivas, puede anotar acciones con el atributo de autorización `[MinimumAgeAuthorize(20)]`personalizado como.</span><span class="sxs-lookup"><span data-stu-id="9e541-130">To make using the policies easier, you can annotate actions with custom authorization attribute like `[MinimumAgeAuthorize(20)]`.</span></span>

## <a name="custom-authorization-attributes"></a><span data-ttu-id="9e541-131">Atributos de autorización personalizados</span><span class="sxs-lookup"><span data-stu-id="9e541-131">Custom Authorization attributes</span></span>

<span data-ttu-id="9e541-132">Las directivas de autorización se identifican por sus nombres.</span><span class="sxs-lookup"><span data-stu-id="9e541-132">Authorization policies are identified by their names.</span></span> <span data-ttu-id="9e541-133">El personalizado `MinimumAgeAuthorizeAttribute` descrito previamente necesita asignar argumentos en una cadena que se puede usar para recuperar la Directiva de autorización correspondiente.</span><span class="sxs-lookup"><span data-stu-id="9e541-133">The custom `MinimumAgeAuthorizeAttribute` described previously needs to map arguments into a string that can be used to retrieve the corresponding authorization policy.</span></span> <span data-ttu-id="9e541-134">Para ello, puede derivar de `AuthorizeAttribute` y hacer que la `Age` propiedad ajuste la `AuthorizeAttribute.Policy` propiedad.</span><span class="sxs-lookup"><span data-stu-id="9e541-134">You can do this by deriving from `AuthorizeAttribute` and making the `Age` property wrap the `AuthorizeAttribute.Policy` property.</span></span>

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

<span data-ttu-id="9e541-135">Este tipo de atributo tiene `Policy` una cadena basada en el prefijo codificado de`"MinimumAge"`forma rígida () y un entero que se pasa a través del constructor.</span><span class="sxs-lookup"><span data-stu-id="9e541-135">This attribute type has a `Policy` string based on the hard-coded prefix (`"MinimumAge"`) and an integer passed in via the constructor.</span></span>

<span data-ttu-id="9e541-136">Puede aplicarlo a acciones de la misma manera que a otros `Authorize` atributos, salvo que toma un entero como parámetro.</span><span class="sxs-lookup"><span data-stu-id="9e541-136">You can apply it to actions in the same way as other `Authorize` attributes except that it takes an integer as a parameter.</span></span>

```csharp
[MinimumAgeAuthorize(10)]
public IActionResult RequiresMinimumAge10()
```

## <a name="custom-iauthorizationpolicyprovider"></a><span data-ttu-id="9e541-137">IAuthorizationPolicyProvider personalizado</span><span class="sxs-lookup"><span data-stu-id="9e541-137">Custom IAuthorizationPolicyProvider</span></span>

<span data-ttu-id="9e541-138">El personalizado `MinimumAgeAuthorizeAttribute` facilita la solicitud de directivas de autorización para cualquier edad mínima deseada.</span><span class="sxs-lookup"><span data-stu-id="9e541-138">The custom `MinimumAgeAuthorizeAttribute` makes it easy to request authorization policies for any minimum age desired.</span></span> <span data-ttu-id="9e541-139">El siguiente problema es asegurarse de que las directivas de autorización están disponibles para todas esas edades.</span><span class="sxs-lookup"><span data-stu-id="9e541-139">The next problem to solve is making sure that authorization policies are available for all of those different ages.</span></span> <span data-ttu-id="9e541-140">Aquí es donde `IAuthorizationPolicyProvider` es útil.</span><span class="sxs-lookup"><span data-stu-id="9e541-140">This is where an `IAuthorizationPolicyProvider` is useful.</span></span>

<span data-ttu-id="9e541-141">Al usar `MinimumAgeAuthorizationAttribute`, los nombres de las directivas de autorización seguirán el patrón `"MinimumAge" + Age`, `IAuthorizationPolicyProvider` por lo que el personalizado debe generar directivas de autorización mediante:</span><span class="sxs-lookup"><span data-stu-id="9e541-141">When using `MinimumAgeAuthorizationAttribute`, the authorization policy names will follow the pattern `"MinimumAge" + Age`, so the custom `IAuthorizationPolicyProvider` should generate authorization policies by:</span></span>

* <span data-ttu-id="9e541-142">Analizando la edad del nombre de la Directiva.</span><span class="sxs-lookup"><span data-stu-id="9e541-142">Parsing the age from the policy name.</span></span>
* <span data-ttu-id="9e541-143">Usar `AuthorizationPolicyBuilder` para crear un nuevo`AuthorizationPolicy`</span><span class="sxs-lookup"><span data-stu-id="9e541-143">Using `AuthorizationPolicyBuilder` to create a new `AuthorizationPolicy`</span></span>
* <span data-ttu-id="9e541-144">En este y en los ejemplos siguientes se asumirá que el usuario se autentica a través de una cookie.</span><span class="sxs-lookup"><span data-stu-id="9e541-144">In this and following examples it will be assumed that the user is authenticated via a cookie.</span></span> <span data-ttu-id="9e541-145">Se `AuthorizationPolicyBuilder` debe construir con al menos un nombre de esquema de autorización o siempre se realiza correctamente.</span><span class="sxs-lookup"><span data-stu-id="9e541-145">The `AuthorizationPolicyBuilder` should either be constructed with at least one authorization scheme name or always succeed.</span></span> <span data-ttu-id="9e541-146">De lo contrario, no hay información sobre cómo proporcionar un desafío al usuario y se producirá una excepción.</span><span class="sxs-lookup"><span data-stu-id="9e541-146">Otherwise there is no information on how to provide a challenge to the user and an exception will be thrown.</span></span>
* <span data-ttu-id="9e541-147">Agregar requisitos a la Directiva en función de la edad `AuthorizationPolicyBuilder.AddRequirements`con.</span><span class="sxs-lookup"><span data-stu-id="9e541-147">Adding requirements to the policy based on the age with `AuthorizationPolicyBuilder.AddRequirements`.</span></span> <span data-ttu-id="9e541-148">En otros escenarios, puede usar `RequireClaim`, `RequireRole`o `RequireUserName` en su lugar.</span><span class="sxs-lookup"><span data-stu-id="9e541-148">In other scenarios, you might use `RequireClaim`, `RequireRole`, or `RequireUserName` instead.</span></span>

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
            var policy = new AuthorizationPolicyBuilder(CookieAuthenticationDefaults.AuthenticationScheme);
            policy.AddRequirements(new MinimumAgeRequirement(age));
            return Task.FromResult(policy.Build());
        }

        return Task.FromResult<AuthorizationPolicy>(null);
    }
}
```

## <a name="multiple-authorization-policy-providers"></a><span data-ttu-id="9e541-149">Varios proveedores de directivas de autorización</span><span class="sxs-lookup"><span data-stu-id="9e541-149">Multiple authorization policy providers</span></span>

<span data-ttu-id="9e541-150">Al usar implementaciones personalizadas `IAuthorizationPolicyProvider` , tenga en cuenta que ASP.net Core solo utiliza una instancia de. `IAuthorizationPolicyProvider`</span><span class="sxs-lookup"><span data-stu-id="9e541-150">When using custom `IAuthorizationPolicyProvider` implementations, keep in mind that ASP.NET Core only uses one instance of `IAuthorizationPolicyProvider`.</span></span> <span data-ttu-id="9e541-151">Si un proveedor personalizado no puede proporcionar directivas de autorización para todos los nombres de directiva que se van a usar, debe revertir a un proveedor de copia de seguridad.</span><span class="sxs-lookup"><span data-stu-id="9e541-151">If a custom provider isn't able to provide authorization policies for all policy names that will be used, it should fall back to a backup provider.</span></span> 

<span data-ttu-id="9e541-152">Por ejemplo, considere una aplicación que necesita directivas de edad personalizadas y una recuperación de directiva basada en roles más tradicional.</span><span class="sxs-lookup"><span data-stu-id="9e541-152">For example, consider an application that needs both custom age policies and more traditional role-based policy retrieval.</span></span> <span data-ttu-id="9e541-153">Este tipo de aplicación podría usar un proveedor de directivas de autorización personalizado que:</span><span class="sxs-lookup"><span data-stu-id="9e541-153">Such an app could use a custom authorization policy provider that:</span></span>

* <span data-ttu-id="9e541-154">Intenta analizar los nombres de directiva.</span><span class="sxs-lookup"><span data-stu-id="9e541-154">Attempts to parse policy names.</span></span> 
* <span data-ttu-id="9e541-155">Llama a un proveedor de directivas diferente ( `DefaultAuthorizationPolicyProvider`como) si el nombre de la Directiva no contiene una edad.</span><span class="sxs-lookup"><span data-stu-id="9e541-155">Calls into a different policy provider (like `DefaultAuthorizationPolicyProvider`) if the policy name doesn't contain an age.</span></span>

<span data-ttu-id="9e541-156">La implementación `IAuthorizationPolicyProvider` de ejemplo mostrada anteriormente se puede actualizar para `DefaultAuthorizationPolicyProvider` usar mediante la creación de un proveedor de directivas de reserva en su constructor (para usarla en caso de que el nombre de la Directiva no coincida con el patrón esperado de ' minimal ' + Age).</span><span class="sxs-lookup"><span data-stu-id="9e541-156">The example `IAuthorizationPolicyProvider` implementation shown above can be updated to use the `DefaultAuthorizationPolicyProvider` by creating a fallback policy provider in its constructor (to be used in case the policy name doesn't match its expected pattern of 'MinimumAge' + age).</span></span>

```csharp
private DefaultAuthorizationPolicyProvider FallbackPolicyProvider { get; }

public MinimumAgePolicyProvider(IOptions<AuthorizationOptions> options)
{
    // ASP.NET Core only uses one authorization policy provider, so if the custom implementation
    // doesn't handle all policies it should fall back to an alternate provider.
    FallbackPolicyProvider = new DefaultAuthorizationPolicyProvider(options);
}
```

<span data-ttu-id="9e541-157">A continuación, `GetPolicyAsync` el método se puede actualizar para `FallbackPolicyProvider` utilizar en lugar de devolver null:</span><span class="sxs-lookup"><span data-stu-id="9e541-157">Then, the `GetPolicyAsync` method can be updated to use the `FallbackPolicyProvider` instead of returning null:</span></span>

```csharp
...
return FallbackPolicyProvider.GetPolicyAsync(policyName);
```

## <a name="default-policy"></a><span data-ttu-id="9e541-158">Directiva predeterminada</span><span class="sxs-lookup"><span data-stu-id="9e541-158">Default policy</span></span>

<span data-ttu-id="9e541-159">Además de proporcionar directivas de autorización con nombre, un `IAuthorizationPolicyProvider` personalizado debe implementar `GetDefaultPolicyAsync` para proporcionar una directiva de autorización `[Authorize]` para los atributos sin un nombre de directiva especificado.</span><span class="sxs-lookup"><span data-stu-id="9e541-159">In addition to providing named authorization policies, a custom `IAuthorizationPolicyProvider` needs to implement `GetDefaultPolicyAsync` to provide an authorization policy for `[Authorize]` attributes without a policy name specified.</span></span>

<span data-ttu-id="9e541-160">En muchos casos, este atributo de autorización solo requiere un usuario autenticado, por lo que puede crear la Directiva necesaria con una `RequireAuthenticatedUser`llamada a:</span><span class="sxs-lookup"><span data-stu-id="9e541-160">In many cases, this authorization attribute only requires an authenticated user, so you can make the necessary policy with a call to `RequireAuthenticatedUser`:</span></span>

```csharp
public Task<AuthorizationPolicy> GetDefaultPolicyAsync() => 
    Task.FromResult(new AuthorizationPolicyBuilder(CookieAuthenticationDefaults.AuthenticationScheme).RequireAuthenticatedUser().Build());
```

<span data-ttu-id="9e541-161">Como con todos los aspectos de un `IAuthorizationPolicyProvider`personalizado, puede personalizarlo según sea necesario.</span><span class="sxs-lookup"><span data-stu-id="9e541-161">As with all aspects of a custom `IAuthorizationPolicyProvider`, you can customize this, as needed.</span></span> <span data-ttu-id="9e541-162">En algunos casos, puede ser deseable recuperar la directiva predeterminada de una reserva `IAuthorizationPolicyProvider`.</span><span class="sxs-lookup"><span data-stu-id="9e541-162">In some cases, it may be desirable to retrieve the default policy from a fallback `IAuthorizationPolicyProvider`.</span></span>

## <a name="required-policy"></a><span data-ttu-id="9e541-163">Directiva necesaria</span><span class="sxs-lookup"><span data-stu-id="9e541-163">Required policy</span></span>

<span data-ttu-id="9e541-164">Un personalizado `IAuthorizationPolicyProvider` debe implementar `GetRequiredPolicyAsync` para, opcionalmente, proporcionar una directiva que siempre es necesaria.</span><span class="sxs-lookup"><span data-stu-id="9e541-164">A custom `IAuthorizationPolicyProvider` needs to implement `GetRequiredPolicyAsync` to, optionally, provide a policy that is always required.</span></span> <span data-ttu-id="9e541-165">Si `GetRequiredPolicyAsync` devuelve una directiva que no es null, esa Directiva se combinará con cualquier otra directiva (con nombre o predeterminada) que se solicite.</span><span class="sxs-lookup"><span data-stu-id="9e541-165">If `GetRequiredPolicyAsync` returns a non-null policy, that policy will be combined with any other (named or default) policy that is requested.</span></span>

<span data-ttu-id="9e541-166">Si no se necesita ninguna directiva necesaria, el proveedor solo puede devolver null o aplazar al proveedor de reserva:</span><span class="sxs-lookup"><span data-stu-id="9e541-166">If no required policy is needed, the provider can just return null or defer to the fallback provider:</span></span>

```csharp
public Task<AuthorizationPolicy> GetRequiredPolicyAsync() => 
    Task.FromResult<AuthorizationPolicy>(null);
```

## <a name="use-a-custom-iauthorizationpolicyprovider"></a><span data-ttu-id="9e541-167">Usar un IAuthorizationPolicyProvider personalizado</span><span class="sxs-lookup"><span data-stu-id="9e541-167">Use a custom IAuthorizationPolicyProvider</span></span>

<span data-ttu-id="9e541-168">Para usar las directivas personalizadas de `IAuthorizationPolicyProvider`un, debe:</span><span class="sxs-lookup"><span data-stu-id="9e541-168">To use custom policies from an `IAuthorizationPolicyProvider`, you must:</span></span>

* <span data-ttu-id="9e541-169">Registre los tipos `AuthorizationHandler` adecuados con la inserción de dependencias (descrita en [la autorización basada en directivas](xref:security/authorization/policies#authorization-handlers)), al igual que con todos los escenarios de autorización basados en directivas.</span><span class="sxs-lookup"><span data-stu-id="9e541-169">Register the appropriate `AuthorizationHandler` types with dependency injection (described in [policy-based authorization](xref:security/authorization/policies#authorization-handlers)), as with all policy-based authorization scenarios.</span></span>
* <span data-ttu-id="9e541-170">Registre el tipo `IAuthorizationPolicyProvider` personalizado en la colección de servicios de inserción de dependencias `Startup.ConfigureServices`de la aplicación (en) para reemplazar el proveedor de directivas predeterminado.</span><span class="sxs-lookup"><span data-stu-id="9e541-170">Register the custom `IAuthorizationPolicyProvider` type in the app's dependency injection service collection (in `Startup.ConfigureServices`) to replace the default policy provider.</span></span>

```csharp
services.AddSingleton<IAuthorizationPolicyProvider, MinimumAgePolicyProvider>();
```

<span data-ttu-id="9e541-171">Hay disponible un `IAuthorizationPolicyProvider` ejemplo personalizado completo en el [repositorio de github de ASPNET/AuthSamples](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider).</span><span class="sxs-lookup"><span data-stu-id="9e541-171">A complete custom `IAuthorizationPolicyProvider` sample is available in the [aspnet/AuthSamples GitHub repository](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider).</span></span>
