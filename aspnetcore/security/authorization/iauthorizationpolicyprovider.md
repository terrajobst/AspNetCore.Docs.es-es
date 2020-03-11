---
title: Proveedores de directivas de autorización personalizados en ASP.NET Core
author: mjrousos
description: Obtenga información sobre cómo usar un IAuthorizationPolicyProvider personalizado en una aplicación ASP.NET Core para generar dinámicamente directivas de autorización.
ms.author: riande
ms.custom: mvc
ms.date: 11/14/2019
uid: security/authorization/iauthorizationpolicyprovider
ms.openlocfilehash: 9f0a0cd5337f7f8d2fc8a4b6902a63b98f6bd702
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78651755"
---
# <a name="custom-authorization-policy-providers-using-iauthorizationpolicyprovider-in-aspnet-core"></a><span data-ttu-id="ad44b-103">Proveedores de directivas de autorización personalizada mediante IAuthorizationPolicyProvider en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ad44b-103">Custom Authorization Policy Providers using IAuthorizationPolicyProvider in ASP.NET Core</span></span> 

<span data-ttu-id="ad44b-104">Por [Mike Rousos](https://github.com/mjrousos)</span><span class="sxs-lookup"><span data-stu-id="ad44b-104">By [Mike Rousos](https://github.com/mjrousos)</span></span>

<span data-ttu-id="ad44b-105">Normalmente, cuando se usa [la autorización basada en directivas](xref:security/authorization/policies), las directivas se registran llamando a `AuthorizationOptions.AddPolicy` como parte de la configuración del servicio de autorización.</span><span class="sxs-lookup"><span data-stu-id="ad44b-105">Typically when using [policy-based authorization](xref:security/authorization/policies), policies are registered by calling `AuthorizationOptions.AddPolicy` as part of authorization service configuration.</span></span> <span data-ttu-id="ad44b-106">En algunos escenarios, puede que no sea posible (o deseable) registrar todas las directivas de autorización de esta manera.</span><span class="sxs-lookup"><span data-stu-id="ad44b-106">In some scenarios, it may not be possible (or desirable) to register all authorization policies in this way.</span></span> <span data-ttu-id="ad44b-107">En esos casos, puede usar un `IAuthorizationPolicyProvider` personalizado para controlar cómo se proporcionan las directivas de autorización.</span><span class="sxs-lookup"><span data-stu-id="ad44b-107">In those cases, you can use a custom `IAuthorizationPolicyProvider` to control how authorization policies are supplied.</span></span>

<span data-ttu-id="ad44b-108">Algunos ejemplos de escenarios en los que un [IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) personalizado puede ser útil son:</span><span class="sxs-lookup"><span data-stu-id="ad44b-108">Examples of scenarios where a custom [IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) may be useful include:</span></span>

* <span data-ttu-id="ad44b-109">Uso de un servicio externo para proporcionar evaluación de directivas.</span><span class="sxs-lookup"><span data-stu-id="ad44b-109">Using an external service to provide policy evaluation.</span></span>
* <span data-ttu-id="ad44b-110">Usar una gran variedad de directivas (por ejemplo, para distintos números de sala o edades), por lo que no tiene sentido agregar cada directiva de autorización individual con una llamada `AuthorizationOptions.AddPolicy`.</span><span class="sxs-lookup"><span data-stu-id="ad44b-110">Using a large range of policies (for different room numbers or ages, for example), so it doesn't make sense to add each individual authorization policy with an `AuthorizationOptions.AddPolicy` call.</span></span>
* <span data-ttu-id="ad44b-111">Crear directivas en tiempo de ejecución en función de la información de un origen de datos externo (por ejemplo, una base de datos) o de determinar dinámicamente los requisitos de autorización mediante otro mecanismo.</span><span class="sxs-lookup"><span data-stu-id="ad44b-111">Creating policies at runtime based on information in an external data source (like a database) or determining authorization requirements dynamically through another mechanism.</span></span>

<span data-ttu-id="ad44b-112">[Vea o descargue el código de ejemplo](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider) del [repositorio de github de AspNetCore](https://github.com/dotnet/AspNetCore).</span><span class="sxs-lookup"><span data-stu-id="ad44b-112">[View or download sample code](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider) from the [AspNetCore GitHub repository](https://github.com/dotnet/AspNetCore).</span></span> <span data-ttu-id="ad44b-113">Descargue el archivo ZIP del repositorio dotnet/AspNetCore.</span><span class="sxs-lookup"><span data-stu-id="ad44b-113">Download the dotnet/AspNetCore repository ZIP file.</span></span> <span data-ttu-id="ad44b-114">Descomprima el archivo.</span><span class="sxs-lookup"><span data-stu-id="ad44b-114">Unzip the file.</span></span> <span data-ttu-id="ad44b-115">Navegue a la carpeta de proyecto *src/Security/samples/CustomPolicyProvider* .</span><span class="sxs-lookup"><span data-stu-id="ad44b-115">Navigate to the *src/Security/samples/CustomPolicyProvider* project folder.</span></span>

## <a name="customize-policy-retrieval"></a><span data-ttu-id="ad44b-116">Personalizar la recuperación de directivas</span><span class="sxs-lookup"><span data-stu-id="ad44b-116">Customize policy retrieval</span></span>

<span data-ttu-id="ad44b-117">ASP.NET Core aplicaciones usan una implementación de la interfaz de `IAuthorizationPolicyProvider` para recuperar directivas de autorización.</span><span class="sxs-lookup"><span data-stu-id="ad44b-117">ASP.NET Core apps use an implementation of the `IAuthorizationPolicyProvider` interface to retrieve authorization policies.</span></span> <span data-ttu-id="ad44b-118">De forma predeterminada, [DefaultAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) se registra y se usa.</span><span class="sxs-lookup"><span data-stu-id="ad44b-118">By default, [DefaultAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) is registered and used.</span></span> <span data-ttu-id="ad44b-119">`DefaultAuthorizationPolicyProvider` devuelve las directivas del `AuthorizationOptions` proporcionado en una llamada a `IServiceCollection.AddAuthorization`.</span><span class="sxs-lookup"><span data-stu-id="ad44b-119">`DefaultAuthorizationPolicyProvider` returns policies from the `AuthorizationOptions` provided in an `IServiceCollection.AddAuthorization` call.</span></span>

<span data-ttu-id="ad44b-120">Personalice este comportamiento registrando una implementación de `IAuthorizationPolicyProvider` diferente en el contenedor de [inserción de dependencias](xref:fundamentals/dependency-injection) de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ad44b-120">Customize this behavior by registering a different `IAuthorizationPolicyProvider` implementation in the app's [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> 

<span data-ttu-id="ad44b-121">La interfaz de `IAuthorizationPolicyProvider` contiene tres API:</span><span class="sxs-lookup"><span data-stu-id="ad44b-121">The `IAuthorizationPolicyProvider` interface contains three APIs:</span></span>

* <span data-ttu-id="ad44b-122">[GetPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) devuelve una directiva de autorización para un nombre determinado.</span><span class="sxs-lookup"><span data-stu-id="ad44b-122">[GetPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) returns an authorization policy for a given name.</span></span>
* <span data-ttu-id="ad44b-123">[GetDefaultPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync) devuelve la Directiva de autorización predeterminada (la directiva usada para `[Authorize]` atributos sin una directiva especificada).</span><span class="sxs-lookup"><span data-stu-id="ad44b-123">[GetDefaultPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync) returns the default authorization policy (the policy used for `[Authorize]` attributes without a policy specified).</span></span> 
* <span data-ttu-id="ad44b-124">[GetFallbackPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getfallbackpolicyasync) devuelve la Directiva de autorización de reserva (la Directiva que usa el middleware de autorización cuando no se especifica ninguna directiva).</span><span class="sxs-lookup"><span data-stu-id="ad44b-124">[GetFallbackPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getfallbackpolicyasync) returns the fallback authorization policy (the policy used by the Authorization Middleware when no policy is specified).</span></span> 

<span data-ttu-id="ad44b-125">Mediante la implementación de estas API, puede personalizar cómo se proporcionan las directivas de autorización.</span><span class="sxs-lookup"><span data-stu-id="ad44b-125">By implementing these APIs, you can customize how authorization policies are provided.</span></span>

## <a name="parameterized-authorize-attribute-example"></a><span data-ttu-id="ad44b-126">Ejemplo de atributo Authorize parametrizado</span><span class="sxs-lookup"><span data-stu-id="ad44b-126">Parameterized authorize attribute example</span></span>

<span data-ttu-id="ad44b-127">Un escenario en el que `IAuthorizationPolicyProvider` es útil es habilitar atributos de `[Authorize]` personalizados cuyos requisitos dependen de un parámetro.</span><span class="sxs-lookup"><span data-stu-id="ad44b-127">One scenario where `IAuthorizationPolicyProvider` is useful is enabling custom `[Authorize]` attributes whose requirements depend on a parameter.</span></span> <span data-ttu-id="ad44b-128">Por ejemplo, en la documentación de [autorización basada en directivas](xref:security/authorization/policies) , se usó una directiva basada en la edad ("AtLeast21") como ejemplo.</span><span class="sxs-lookup"><span data-stu-id="ad44b-128">For example, in [policy-based authorization](xref:security/authorization/policies) documentation, an age-based (“AtLeast21”) policy was used as a sample.</span></span> <span data-ttu-id="ad44b-129">Si hay diferentes acciones de controlador en una aplicación que deben estar disponibles para los usuarios de *diferentes* edades, podría ser útil tener muchas directivas diferentes basadas en la edad.</span><span class="sxs-lookup"><span data-stu-id="ad44b-129">If different controller actions in an app should be made available to users of *different* ages, it might be useful to have many different age-based policies.</span></span> <span data-ttu-id="ad44b-130">En lugar de registrar todas las directivas basadas en la edad diferentes que la aplicación necesitará en `AuthorizationOptions`, puede generar las directivas dinámicamente con un `IAuthorizationPolicyProvider`personalizado.</span><span class="sxs-lookup"><span data-stu-id="ad44b-130">Instead of registering all the different age-based policies that the application will need in `AuthorizationOptions`, you can generate the policies dynamically with a custom `IAuthorizationPolicyProvider`.</span></span> <span data-ttu-id="ad44b-131">Para facilitar el uso de las directivas, puede anotar acciones con el atributo de autorización personalizado, como `[MinimumAgeAuthorize(20)]`.</span><span class="sxs-lookup"><span data-stu-id="ad44b-131">To make using the policies easier, you can annotate actions with custom authorization attribute like `[MinimumAgeAuthorize(20)]`.</span></span>

## <a name="custom-authorization-attributes"></a><span data-ttu-id="ad44b-132">Atributos de autorización personalizados</span><span class="sxs-lookup"><span data-stu-id="ad44b-132">Custom Authorization attributes</span></span>

<span data-ttu-id="ad44b-133">Las directivas de autorización se identifican por sus nombres.</span><span class="sxs-lookup"><span data-stu-id="ad44b-133">Authorization policies are identified by their names.</span></span> <span data-ttu-id="ad44b-134">Los `MinimumAgeAuthorizeAttribute` personalizados descritos anteriormente deben asignar argumentos en una cadena que se puede usar para recuperar la Directiva de autorización correspondiente.</span><span class="sxs-lookup"><span data-stu-id="ad44b-134">The custom `MinimumAgeAuthorizeAttribute` described previously needs to map arguments into a string that can be used to retrieve the corresponding authorization policy.</span></span> <span data-ttu-id="ad44b-135">Para ello, puede derivar de `AuthorizeAttribute` y hacer que la propiedad `Age` ajuste la propiedad `AuthorizeAttribute.Policy`.</span><span class="sxs-lookup"><span data-stu-id="ad44b-135">You can do this by deriving from `AuthorizeAttribute` and making the `Age` property wrap the `AuthorizeAttribute.Policy` property.</span></span>

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

<span data-ttu-id="ad44b-136">Este tipo de atributo tiene una cadena de `Policy` basada en el prefijo codificado de forma rígida (`"MinimumAge"`) y un entero que se pasa a través del constructor.</span><span class="sxs-lookup"><span data-stu-id="ad44b-136">This attribute type has a `Policy` string based on the hard-coded prefix (`"MinimumAge"`) and an integer passed in via the constructor.</span></span>

<span data-ttu-id="ad44b-137">Puede aplicarlo a acciones de la misma manera que a otros atributos de `Authorize`, salvo que toma un entero como parámetro.</span><span class="sxs-lookup"><span data-stu-id="ad44b-137">You can apply it to actions in the same way as other `Authorize` attributes except that it takes an integer as a parameter.</span></span>

```csharp
[MinimumAgeAuthorize(10)]
public IActionResult RequiresMinimumAge10()
```

## <a name="custom-iauthorizationpolicyprovider"></a><span data-ttu-id="ad44b-138">IAuthorizationPolicyProvider personalizado</span><span class="sxs-lookup"><span data-stu-id="ad44b-138">Custom IAuthorizationPolicyProvider</span></span>

<span data-ttu-id="ad44b-139">El `MinimumAgeAuthorizeAttribute` personalizado facilita la solicitud de directivas de autorización para cualquier edad mínima deseada.</span><span class="sxs-lookup"><span data-stu-id="ad44b-139">The custom `MinimumAgeAuthorizeAttribute` makes it easy to request authorization policies for any minimum age desired.</span></span> <span data-ttu-id="ad44b-140">El siguiente problema es asegurarse de que las directivas de autorización están disponibles para todas esas edades.</span><span class="sxs-lookup"><span data-stu-id="ad44b-140">The next problem to solve is making sure that authorization policies are available for all of those different ages.</span></span> <span data-ttu-id="ad44b-141">Aquí es donde un `IAuthorizationPolicyProvider` es útil.</span><span class="sxs-lookup"><span data-stu-id="ad44b-141">This is where an `IAuthorizationPolicyProvider` is useful.</span></span>

<span data-ttu-id="ad44b-142">Al usar `MinimumAgeAuthorizationAttribute`, los nombres de las directivas de autorización seguirán el patrón `"MinimumAge" + Age`, por lo que el `IAuthorizationPolicyProvider` personalizado debe generar directivas de autorización mediante:</span><span class="sxs-lookup"><span data-stu-id="ad44b-142">When using `MinimumAgeAuthorizationAttribute`, the authorization policy names will follow the pattern `"MinimumAge" + Age`, so the custom `IAuthorizationPolicyProvider` should generate authorization policies by:</span></span>

* <span data-ttu-id="ad44b-143">Analizando la edad del nombre de la Directiva.</span><span class="sxs-lookup"><span data-stu-id="ad44b-143">Parsing the age from the policy name.</span></span>
* <span data-ttu-id="ad44b-144">Usar `AuthorizationPolicyBuilder` para crear una nueva `AuthorizationPolicy`</span><span class="sxs-lookup"><span data-stu-id="ad44b-144">Using `AuthorizationPolicyBuilder` to create a new `AuthorizationPolicy`</span></span>
* <span data-ttu-id="ad44b-145">En este y en los ejemplos siguientes se asumirá que el usuario se autentica a través de una cookie.</span><span class="sxs-lookup"><span data-stu-id="ad44b-145">In this and following examples it will be assumed that the user is authenticated via a cookie.</span></span> <span data-ttu-id="ad44b-146">El `AuthorizationPolicyBuilder` debe construirse con al menos un nombre de esquema de autorización o siempre se realiza correctamente.</span><span class="sxs-lookup"><span data-stu-id="ad44b-146">The `AuthorizationPolicyBuilder` should either be constructed with at least one authorization scheme name or always succeed.</span></span> <span data-ttu-id="ad44b-147">De lo contrario, no hay información sobre cómo proporcionar un desafío al usuario y se producirá una excepción.</span><span class="sxs-lookup"><span data-stu-id="ad44b-147">Otherwise there is no information on how to provide a challenge to the user and an exception will be thrown.</span></span>
* <span data-ttu-id="ad44b-148">Agregar requisitos a la Directiva en función de la edad con `AuthorizationPolicyBuilder.AddRequirements`.</span><span class="sxs-lookup"><span data-stu-id="ad44b-148">Adding requirements to the policy based on the age with `AuthorizationPolicyBuilder.AddRequirements`.</span></span> <span data-ttu-id="ad44b-149">En otros escenarios, puede usar `RequireClaim`, `RequireRole`o `RequireUserName` en su lugar.</span><span class="sxs-lookup"><span data-stu-id="ad44b-149">In other scenarios, you might use `RequireClaim`, `RequireRole`, or `RequireUserName` instead.</span></span>

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

## <a name="multiple-authorization-policy-providers"></a><span data-ttu-id="ad44b-150">Varios proveedores de directivas de autorización</span><span class="sxs-lookup"><span data-stu-id="ad44b-150">Multiple authorization policy providers</span></span>

<span data-ttu-id="ad44b-151">Al usar implementaciones de `IAuthorizationPolicyProvider` personalizadas, tenga en cuenta que ASP.NET Core solo utiliza una instancia de `IAuthorizationPolicyProvider`.</span><span class="sxs-lookup"><span data-stu-id="ad44b-151">When using custom `IAuthorizationPolicyProvider` implementations, keep in mind that ASP.NET Core only uses one instance of `IAuthorizationPolicyProvider`.</span></span> <span data-ttu-id="ad44b-152">Si un proveedor personalizado no puede proporcionar directivas de autorización para todos los nombres de directiva que se van a usar, debe diferir a un proveedor de copia de seguridad.</span><span class="sxs-lookup"><span data-stu-id="ad44b-152">If a custom provider isn't able to provide authorization policies for all policy names that will be used, it should defer to a backup provider.</span></span> 

<span data-ttu-id="ad44b-153">Por ejemplo, considere una aplicación que necesita directivas de edad personalizadas y una recuperación de directiva basada en roles más tradicional.</span><span class="sxs-lookup"><span data-stu-id="ad44b-153">For example, consider an application that needs both custom age policies and more traditional role-based policy retrieval.</span></span> <span data-ttu-id="ad44b-154">Este tipo de aplicación podría usar un proveedor de directivas de autorización personalizado que:</span><span class="sxs-lookup"><span data-stu-id="ad44b-154">Such an app could use a custom authorization policy provider that:</span></span>

* <span data-ttu-id="ad44b-155">Intenta analizar los nombres de directiva.</span><span class="sxs-lookup"><span data-stu-id="ad44b-155">Attempts to parse policy names.</span></span> 
* <span data-ttu-id="ad44b-156">Llama a un proveedor de directivas diferente (como `DefaultAuthorizationPolicyProvider`) si el nombre de la Directiva no contiene una edad.</span><span class="sxs-lookup"><span data-stu-id="ad44b-156">Calls into a different policy provider (like `DefaultAuthorizationPolicyProvider`) if the policy name doesn't contain an age.</span></span>

<span data-ttu-id="ad44b-157">La implementación de `IAuthorizationPolicyProvider` de ejemplo mostrada anteriormente se puede actualizar para utilizar el `DefaultAuthorizationPolicyProvider` mediante la creación de un proveedor de directivas de copia de seguridad en su constructor (que se usará en caso de que el nombre de la Directiva no coincida con el patrón esperado de ' minimal ' + Age).</span><span class="sxs-lookup"><span data-stu-id="ad44b-157">The example `IAuthorizationPolicyProvider` implementation shown above can be updated to use the `DefaultAuthorizationPolicyProvider` by creating a backup policy provider in its constructor (to be used in case the policy name doesn't match its expected pattern of 'MinimumAge' + age).</span></span>

```csharp
private DefaultAuthorizationPolicyProvider BackupPolicyProvider { get; }

public MinimumAgePolicyProvider(IOptions<AuthorizationOptions> options)
{
    // ASP.NET Core only uses one authorization policy provider, so if the custom implementation
    // doesn't handle all policies it should fall back to an alternate provider.
    BackupPolicyProvider = new DefaultAuthorizationPolicyProvider(options);
}
```

<span data-ttu-id="ad44b-158">A continuación, el método `GetPolicyAsync` se puede actualizar para usar el `BackupPolicyProvider` en lugar de devolver null:</span><span class="sxs-lookup"><span data-stu-id="ad44b-158">Then, the `GetPolicyAsync` method can be updated to use the `BackupPolicyProvider` instead of returning null:</span></span>

```csharp
...
return BackupPolicyProvider.GetPolicyAsync(policyName);
```

## <a name="default-policy"></a><span data-ttu-id="ad44b-159">Directiva predeterminada</span><span class="sxs-lookup"><span data-stu-id="ad44b-159">Default policy</span></span>

<span data-ttu-id="ad44b-160">Además de proporcionar directivas de autorización con nombre, una `IAuthorizationPolicyProvider` personalizada debe implementar `GetDefaultPolicyAsync` para proporcionar una directiva de autorización para `[Authorize]` atributos sin un nombre de directiva especificado.</span><span class="sxs-lookup"><span data-stu-id="ad44b-160">In addition to providing named authorization policies, a custom `IAuthorizationPolicyProvider` needs to implement `GetDefaultPolicyAsync` to provide an authorization policy for `[Authorize]` attributes without a policy name specified.</span></span>

<span data-ttu-id="ad44b-161">En muchos casos, este atributo de autorización solo requiere un usuario autenticado, por lo que puede crear la Directiva necesaria con una llamada a `RequireAuthenticatedUser`:</span><span class="sxs-lookup"><span data-stu-id="ad44b-161">In many cases, this authorization attribute only requires an authenticated user, so you can make the necessary policy with a call to `RequireAuthenticatedUser`:</span></span>

```csharp
public Task<AuthorizationPolicy> GetDefaultPolicyAsync() => 
    Task.FromResult(new AuthorizationPolicyBuilder(CookieAuthenticationDefaults.AuthenticationScheme).RequireAuthenticatedUser().Build());
```

<span data-ttu-id="ad44b-162">Como con todos los aspectos de un `IAuthorizationPolicyProvider`personalizado, puede personalizarlo según sea necesario.</span><span class="sxs-lookup"><span data-stu-id="ad44b-162">As with all aspects of a custom `IAuthorizationPolicyProvider`, you can customize this, as needed.</span></span> <span data-ttu-id="ad44b-163">En algunos casos, puede ser deseable recuperar la directiva predeterminada de un `IAuthorizationPolicyProvider`de reserva.</span><span class="sxs-lookup"><span data-stu-id="ad44b-163">In some cases, it may be desirable to retrieve the default policy from a fallback `IAuthorizationPolicyProvider`.</span></span>

## <a name="fallback-policy"></a><span data-ttu-id="ad44b-164">Directiva de reserva</span><span class="sxs-lookup"><span data-stu-id="ad44b-164">Fallback policy</span></span>

<span data-ttu-id="ad44b-165">Opcionalmente, un `IAuthorizationPolicyProvider` personalizado puede implementar `GetFallbackPolicyAsync` para proporcionar una directiva que se usa al [combinar directivas](/dotnet/api/microsoft.aspnetcore.authorization.authorizationpolicy.combine) y cuando no se especifican directivas.</span><span class="sxs-lookup"><span data-stu-id="ad44b-165">A custom `IAuthorizationPolicyProvider` can optionally implement `GetFallbackPolicyAsync` to provide a policy that's used when [combining policies](/dotnet/api/microsoft.aspnetcore.authorization.authorizationpolicy.combine) and when no policies are specified.</span></span> <span data-ttu-id="ad44b-166">Si `GetFallbackPolicyAsync` devuelve una directiva que no es null, el middleware de autorización usa la Directiva devuelta cuando no se especifican directivas para la solicitud.</span><span class="sxs-lookup"><span data-stu-id="ad44b-166">If `GetFallbackPolicyAsync` returns a non-null policy, the returned policy is used by the Authorization Middleware when no policies are specified for the request.</span></span>

<span data-ttu-id="ad44b-167">Si no se requiere ninguna directiva de reserva, el proveedor puede devolver `null` o aplazar el proveedor de reserva:</span><span class="sxs-lookup"><span data-stu-id="ad44b-167">If no fallback policy is required, the provider can return `null` or defer to the fallback provider:</span></span>

```csharp
public Task<AuthorizationPolicy> GetFallbackPolicyAsync() => 
    Task.FromResult<AuthorizationPolicy>(null);
```

## <a name="use-a-custom-iauthorizationpolicyprovider"></a><span data-ttu-id="ad44b-168">Usar un IAuthorizationPolicyProvider personalizado</span><span class="sxs-lookup"><span data-stu-id="ad44b-168">Use a custom IAuthorizationPolicyProvider</span></span>

<span data-ttu-id="ad44b-169">Para usar las directivas personalizadas de un `IAuthorizationPolicyProvider`, debe:</span><span class="sxs-lookup"><span data-stu-id="ad44b-169">To use custom policies from an `IAuthorizationPolicyProvider`, you must:</span></span>

* <span data-ttu-id="ad44b-170">Registre los tipos de `AuthorizationHandler` adecuados con la inserción de dependencias (descrita en [la autorización basada en directivas](xref:security/authorization/policies#authorization-handlers)), al igual que con todos los escenarios de autorización basados en directivas.</span><span class="sxs-lookup"><span data-stu-id="ad44b-170">Register the appropriate `AuthorizationHandler` types with dependency injection (described in [policy-based authorization](xref:security/authorization/policies#authorization-handlers)), as with all policy-based authorization scenarios.</span></span>
* <span data-ttu-id="ad44b-171">Registre el tipo de `IAuthorizationPolicyProvider` personalizado en la colección de servicios de inserción de dependencias de la aplicación (en `Startup.ConfigureServices`) para reemplazar el proveedor de directivas predeterminado.</span><span class="sxs-lookup"><span data-stu-id="ad44b-171">Register the custom `IAuthorizationPolicyProvider` type in the app's dependency injection service collection (in `Startup.ConfigureServices`) to replace the default policy provider.</span></span>

```csharp
services.AddSingleton<IAuthorizationPolicyProvider, MinimumAgePolicyProvider>();
```

<span data-ttu-id="ad44b-172">Hay disponible un ejemplo de `IAuthorizationPolicyProvider` personalizado completo en el [repositorio de github de ASPNET/AuthSamples](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider).</span><span class="sxs-lookup"><span data-stu-id="ad44b-172">A complete custom `IAuthorizationPolicyProvider` sample is available in the [aspnet/AuthSamples GitHub repository](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider).</span></span>
