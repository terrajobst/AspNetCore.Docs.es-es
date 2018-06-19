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
ms.locfileid: "34233448"
---
# <a name="custom-authorization-policy-providers-using-iauthorizationpolicyprovider-in-aspnet-core"></a><span data-ttu-id="dd398-103">Proveedores personalizados de directiva de autorización mediante IAuthorizationPolicyProvider en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="dd398-103">Custom Authorization Policy Providers using IAuthorizationPolicyProvider in ASP.NET Core</span></span> 

<span data-ttu-id="dd398-104">Por [Mike Rousos](https://github.com/mjrousos)</span><span class="sxs-lookup"><span data-stu-id="dd398-104">By [Mike Rousos](https://github.com/mjrousos)</span></span>

<span data-ttu-id="dd398-105">Normalmente cuando se usa [autorización basada en directivas](xref:security/authorization/policies), las directivas se registran mediante una llamada a `AuthorizationOptions.AddPolicy` como parte de la configuración del servicio de autorización.</span><span class="sxs-lookup"><span data-stu-id="dd398-105">Typically when using [policy-based authorization](xref:security/authorization/policies), policies are registered by calling `AuthorizationOptions.AddPolicy` as part of authorization service configuration.</span></span> <span data-ttu-id="dd398-106">En algunos casos, puede no ser posible (o deseable) para registrar todas las directivas de autorización de esta manera.</span><span class="sxs-lookup"><span data-stu-id="dd398-106">In some scenarios, it may not be possible (or desirable) to register all authorization policies in this way.</span></span> <span data-ttu-id="dd398-107">En esos casos, puede utilizar un personalizado `IAuthorizationPolicyProvider` para controlar cómo se proporcionan las directivas de autorización.</span><span class="sxs-lookup"><span data-stu-id="dd398-107">In those cases, you can use a custom `IAuthorizationPolicyProvider` to control how authorization policies are supplied.</span></span>

<span data-ttu-id="dd398-108">Algunos ejemplos de escenarios donde un personalizado [IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) puede ser útil incluir:</span><span class="sxs-lookup"><span data-stu-id="dd398-108">Examples of scenarios where a custom [IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) may be useful include:</span></span>

* <span data-ttu-id="dd398-109">Uso de un servicio externo para proporcionar la evaluación de la directiva.</span><span class="sxs-lookup"><span data-stu-id="dd398-109">Using an external service to provide policy evaluation.</span></span>
* <span data-ttu-id="dd398-110">Con un amplio rango de directivas (para los números de sala diferente o edades, por ejemplo), por lo que no tiene sentido agregar cada directiva de autorización individuales con un `AuthorizationOptions.AddPolicy` llamar.</span><span class="sxs-lookup"><span data-stu-id="dd398-110">Using a large range of policies (for different room numbers or ages, for example), so it doesn’t make sense to add each individual authorization policy with an `AuthorizationOptions.AddPolicy` call.</span></span>
* <span data-ttu-id="dd398-111">Creación de directivas en tiempo de ejecución basándose en la información de un origen de datos externo (por ejemplo, una base de datos) o determinar los requisitos de autorización dinámicamente mediante otro mecanismo.</span><span class="sxs-lookup"><span data-stu-id="dd398-111">Creating policies at runtime based on information in an external data source (like a database) or determining authorization requirements dynamically through another mechanism.</span></span>

## <a name="customizing-policy-retrieval"></a><span data-ttu-id="dd398-112">Personalizar la recuperación de directivas</span><span class="sxs-lookup"><span data-stu-id="dd398-112">Customizing policy retrieval</span></span>

<span data-ttu-id="dd398-113">Aplicaciones de ASP.NET Core utilizan una implementación de la `IAuthorizationPolicyProvider` interfaz para recuperar las directivas de autorización.</span><span class="sxs-lookup"><span data-stu-id="dd398-113">ASP.NET Core apps use an implementation of the `IAuthorizationPolicyProvider` interface to retrieve authorization policies.</span></span> <span data-ttu-id="dd398-114">De forma predeterminada, [DefaultAuthorizationPolicyProvider](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) está registrado y que usa.</span><span class="sxs-lookup"><span data-stu-id="dd398-114">By default, [DefaultAuthorizationPolicyProvider](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) is registered and used.</span></span> <span data-ttu-id="dd398-115">`DefaultAuthorizationPolicyProvider` Devuelve las directivas de la `AuthorizationOptions` proporcionado en una `IServiceCollection.AddAuthorization` llamar.</span><span class="sxs-lookup"><span data-stu-id="dd398-115">`DefaultAuthorizationPolicyProvider` returns policies from the `AuthorizationOptions` provided in an `IServiceCollection.AddAuthorization` call.</span></span>

<span data-ttu-id="dd398-116">Puede personalizar este comportamiento registrando una diferentes `IAuthorizationPolicyProvider` implementación en la aplicación [inyección de dependencia](xref:fundamentals/dependency-injection) contenedor.</span><span class="sxs-lookup"><span data-stu-id="dd398-116">You can customize this behavior by registering a different `IAuthorizationPolicyProvider` implementation in the app’s [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> 

<span data-ttu-id="dd398-117">El `IAuthorizationPolicyProvider` interfaz contiene dos API:</span><span class="sxs-lookup"><span data-stu-id="dd398-117">The `IAuthorizationPolicyProvider` interface contains two APIs:</span></span>

* <span data-ttu-id="dd398-118">[GetPolicyAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) devuelve una directiva de autorización para un nombre concreto.</span><span class="sxs-lookup"><span data-stu-id="dd398-118">[GetPolicyAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) returns an authorization policy for a given name.</span></span>
* <span data-ttu-id="dd398-119">[GetDefaultPolicyAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync?view=aspnetcore-2.0) devuelve la directiva de autorización de forma predeterminada (la directiva utilizada para `[Authorize]` atributos sin una directiva especificada).</span><span class="sxs-lookup"><span data-stu-id="dd398-119">[GetDefaultPolicyAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync?view=aspnetcore-2.0) returns the default authorization policy (the policy used for `[Authorize]` attributes without a policy specified).</span></span> 

<span data-ttu-id="dd398-120">Mediante la implementación de estas dos API, puede personalizar cómo se proporcionan las directivas de autorización.</span><span class="sxs-lookup"><span data-stu-id="dd398-120">By implementing these two APIs, you can customize how authorization policies are provided.</span></span>

## <a name="parameterized-authorize-attribute-example"></a><span data-ttu-id="dd398-121">Parámetros autorizar el ejemplo de atributo</span><span class="sxs-lookup"><span data-stu-id="dd398-121">Parameterized authorize attribute example</span></span>

<span data-ttu-id="dd398-122">Un escenario donde `IAuthorizationPolicyProvider` es útil es habilitar personalizado `[Authorize]` atributos cuyos requisitos dependen de un parámetro.</span><span class="sxs-lookup"><span data-stu-id="dd398-122">One scenario where `IAuthorizationPolicyProvider` is useful is enabling custom `[Authorize]` attributes whose requirements depend on a parameter.</span></span> <span data-ttu-id="dd398-123">Por ejemplo, en [autorización basada en directivas](xref:security/authorization/policies) documentación, un edades ("AtLeast21") se utiliza la directiva como un ejemplo.</span><span class="sxs-lookup"><span data-stu-id="dd398-123">For example, in [policy-based authorization](xref:security/authorization/policies) documentation, an age-based (“AtLeast21”) policy was used as a sample.</span></span> <span data-ttu-id="dd398-124">Si las acciones de controlador diferentes en una aplicación deben estar disponibles para los usuarios de *diferentes* edades, puede ser útil tener muchas directivas diferentes edades.</span><span class="sxs-lookup"><span data-stu-id="dd398-124">If different controller actions in an app should be made available to users of *different* ages, it might be useful to have many different age-based policies.</span></span> <span data-ttu-id="dd398-125">En vez de registrar todas las diferentes edades directivas que necesita la aplicación en `AuthorizationOptions`, puede generar las directivas de forma dinámica con un personalizado `IAuthorizationPolicyProvider`.</span><span class="sxs-lookup"><span data-stu-id="dd398-125">Instead of registering all the different age-based policies that the application will need in `AuthorizationOptions`, you can generate the policies dynamically with a custom `IAuthorizationPolicyProvider`.</span></span> <span data-ttu-id="dd398-126">Para hacer con las directivas más fácil, puede anotar acciones con el atributo de autorización personalizada como `[MinimumAgeAuthorize(20)]`.</span><span class="sxs-lookup"><span data-stu-id="dd398-126">To make using the policies easier, you can annotate actions with custom authorization attribute like `[MinimumAgeAuthorize(20)]`.</span></span>

## <a name="custom-authorization-attributes"></a><span data-ttu-id="dd398-127">Atributos de autorización personalizada</span><span class="sxs-lookup"><span data-stu-id="dd398-127">Custom Authorization Attributes</span></span>

<span data-ttu-id="dd398-128">Las directivas de autorización se identifican por sus nombres.</span><span class="sxs-lookup"><span data-stu-id="dd398-128">Authorization policies are identified by their names.</span></span> <span data-ttu-id="dd398-129">Personalizado `MinimumAgeAuthorizeAttribute` descritos anteriormente se necesita asignar argumentos en una cadena que puede usarse para recuperar la directiva de autorización correspondiente.</span><span class="sxs-lookup"><span data-stu-id="dd398-129">The custom `MinimumAgeAuthorizeAttribute` described previously needs to map arguments into a string that can be used to retrieve the corresponding authorization policy.</span></span> <span data-ttu-id="dd398-130">Puede hacerlo mediante la derivación de `AuthorizeAttribute` y realizar la `Age` encapsulado propiedad el `AuthorizeAttribute.Policy` propiedad.</span><span class="sxs-lookup"><span data-stu-id="dd398-130">You can do this by deriving from `AuthorizeAttribute` and making the `Age` property wrap the `AuthorizeAttribute.Policy` property.</span></span>

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

<span data-ttu-id="dd398-131">Este tipo de atributo tiene un `Policy` cadena según el prefijo codificado de forma rígida (`"MinimumAge"`) y un entero pasa mediante el constructor.</span><span class="sxs-lookup"><span data-stu-id="dd398-131">This attribute type has a `Policy` string based on the hard-coded prefix (`"MinimumAge"`) and an integer passed in via the constructor.</span></span>

<span data-ttu-id="dd398-132">Se puede aplicar a las acciones de la misma manera que otras `Authorize` atributos salvo por que utiliza un entero como un parámetro.</span><span class="sxs-lookup"><span data-stu-id="dd398-132">You can apply it to actions in the same way as other `Authorize` attributes except that it takes an integer as a parameter.</span></span>

```CSharp
[MinimumAgeAuthorize(10)]
public IActionResult RequiresMinimumAge10()
```

## <a name="custom-iauthorizationpolicyprovider"></a><span data-ttu-id="dd398-133">IAuthorizationPolicyProvider personalizada</span><span class="sxs-lookup"><span data-stu-id="dd398-133">Custom IAuthorizationPolicyProvider</span></span>

<span data-ttu-id="dd398-134">Personalizado `MinimumAgeAuthorizeAttribute` facilita la directivas de solicitud de autorización para cualquier antigüedad mínima deseado.</span><span class="sxs-lookup"><span data-stu-id="dd398-134">The custom `MinimumAgeAuthorizeAttribute` makes it easy to request authorization policies for any minimum age desired.</span></span> <span data-ttu-id="dd398-135">El siguiente problema al resolver es asegurarse de que las directivas de autorización están disponibles para todos los diferentes edades.</span><span class="sxs-lookup"><span data-stu-id="dd398-135">The next problem to solve is making sure that authorization policies are available for all of those different ages.</span></span> <span data-ttu-id="dd398-136">Aquí es donde un `IAuthorizationPolicyProvider` es útil.</span><span class="sxs-lookup"><span data-stu-id="dd398-136">This is where an `IAuthorizationPolicyProvider` is useful.</span></span>

<span data-ttu-id="dd398-137">Cuando se usa `MinimumAgeAuthorizationAttribute`, los nombres de directiva de autorización sigue el patrón de `"MinimumAge" + Age`, por lo que la opción de instalación `IAuthorizationPolicyProvider` debe generar directivas de autorización por:</span><span class="sxs-lookup"><span data-stu-id="dd398-137">When using `MinimumAgeAuthorizationAttribute`, the authorization policy names will follow the pattern `"MinimumAge" + Age`, so the custom `IAuthorizationPolicyProvider` should generate authorization policies by:</span></span>

* <span data-ttu-id="dd398-138">Analizar la edad de nombre de la directiva.</span><span class="sxs-lookup"><span data-stu-id="dd398-138">Parsing the age from the policy name.</span></span>
* <span data-ttu-id="dd398-139">Usar `AuthorizationPolicyBuiler` para crear un nuevo `AuthorizationPolicy`</span><span class="sxs-lookup"><span data-stu-id="dd398-139">Using `AuthorizationPolicyBuiler` to create a new `AuthorizationPolicy`</span></span>
* <span data-ttu-id="dd398-140">Agregar requisitos a la directiva en función de la antigüedad con `AuthorizationPolicyBuilder.AddRequirements`.</span><span class="sxs-lookup"><span data-stu-id="dd398-140">Adding requirements to the policy based on the age with `AuthorizationPolicyBuilder.AddRequirements`.</span></span> <span data-ttu-id="dd398-141">En otros escenarios, puede usar `RequireClaim`, `RequireRole`, o `RequireUserName` en su lugar.</span><span class="sxs-lookup"><span data-stu-id="dd398-141">In other scenarios, you might use `RequireClaim`, `RequireRole`, or `RequireUserName` instead.</span></span>

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

## <a name="multiple-authorization-policy-providers"></a><span data-ttu-id="dd398-142">Varios proveedores de directiva de autorización</span><span class="sxs-lookup"><span data-stu-id="dd398-142">Multiple authorization policy providers</span></span>

<span data-ttu-id="dd398-143">Cuando se usa personalizado `IAuthorizationPolicyProvider` implementaciones, tenga en cuenta que ASP.NET Core solo usa una instancia de `IAuthorizationPolicyProvider`.</span><span class="sxs-lookup"><span data-stu-id="dd398-143">When using custom `IAuthorizationPolicyProvider` implementations, keep in mind that ASP.NET Core only uses one instance of `IAuthorizationPolicyProvider`.</span></span> <span data-ttu-id="dd398-144">Si un proveedor personalizado no es capaz de proporcionar las directivas de autorización para todos los nombres de directiva, debe recurrir a un proveedor de copia de seguridad.</span><span class="sxs-lookup"><span data-stu-id="dd398-144">If a custom provider isn't able to provide authorization policies for all policy names, it should fall back to a backup provider.</span></span> <span data-ttu-id="dd398-145">Nombres de las directivas podrían incluir aquellos que proceden de una directiva predeterminada para `[Authorize]` atributos sin nombre.</span><span class="sxs-lookup"><span data-stu-id="dd398-145">Policy names might include those that come from a default policy for `[Authorize]` attributes without a name.</span></span>

<span data-ttu-id="dd398-146">Por ejemplo, considere la posibilidad de que una aplicación necesita directivas de antigüedad personalizado y más tradicional de recuperación de directiva basada en roles.</span><span class="sxs-lookup"><span data-stu-id="dd398-146">For example, consider an application needed both custom age policies and more traditional role-based policy retrieval.</span></span> <span data-ttu-id="dd398-147">Dicha aplicación podría usar un proveedor de directivas de autorización personalizada que:</span><span class="sxs-lookup"><span data-stu-id="dd398-147">Such an app could use a custom authorization policy provider that:</span></span>

* <span data-ttu-id="dd398-148">Intenta analizar los nombres de las directivas.</span><span class="sxs-lookup"><span data-stu-id="dd398-148">Attempts to parse policy names.</span></span> 
* <span data-ttu-id="dd398-149">Llama a un proveedor de directivas diferentes (como `DefaultAuthorizationPolicyProvider`) si el nombre de directiva no contiene una edad.</span><span class="sxs-lookup"><span data-stu-id="dd398-149">Calls into a different policy provider (like `DefaultAuthorizationPolicyProvider`) if the policy name doesn't contain an age.</span></span>

## <a name="default-policy"></a><span data-ttu-id="dd398-150">Directiva predeterminada</span><span class="sxs-lookup"><span data-stu-id="dd398-150">Default policy</span></span>

<span data-ttu-id="dd398-151">Además de proporcionar directivas de autorización con nombre personalizado `IAuthorizationPolicyProvider` debe implementar `GetDefaultPolicyAsync` para proporcionar una directiva de autorización para `[Authorize]` atributos sin un nombre de directiva especificado.</span><span class="sxs-lookup"><span data-stu-id="dd398-151">In addition to providing named authorization policies, a custom `IAuthorizationPolicyProvider` needs to implement `GetDefaultPolicyAsync` to provide an authorization policy for `[Authorize]` attributes without a policy name specified.</span></span>

<span data-ttu-id="dd398-152">En muchos casos, este atributo de autorización solo requiere un usuario autenticado, por lo que puede hacer que la directiva necesaria con una llamada a `RequireAuthenticatedUser`:</span><span class="sxs-lookup"><span data-stu-id="dd398-152">In many cases, this authorization attribute only requires an authenticated user, so you can make the necessary policy with a call to `RequireAuthenticatedUser`:</span></span>

```CSharp
public Task<AuthorizationPolicy> GetDefaultPolicyAsync() => 
    Task.FromResult(new AuthorizationPolicyBuilder().RequireAuthenticatedUser().Build());
```

<span data-ttu-id="dd398-153">Al igual que con todos los aspectos de un personalizado `IAuthorizationPolicyProvider`, puede personalizarlo, según sea necesario.</span><span class="sxs-lookup"><span data-stu-id="dd398-153">As with all aspects of a custom `IAuthorizationPolicyProvider`, you can customize this, as needed.</span></span> <span data-ttu-id="dd398-154">En algunos casos:</span><span class="sxs-lookup"><span data-stu-id="dd398-154">In some cases:</span></span>

* <span data-ttu-id="dd398-155">No puede usar directivas de autorización de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="dd398-155">Default authorization policies might not be used.</span></span>
* <span data-ttu-id="dd398-156">Recuperar la directiva predeterminada se puede delegar en una acción de reserva `IAuthorizationPolicyProvider`.</span><span class="sxs-lookup"><span data-stu-id="dd398-156">Retrieving the default policy can be delegated to a fallback `IAuthorizationPolicyProvider`.</span></span>

## <a name="using-a-custom-iauthorizationpolicyprovider"></a><span data-ttu-id="dd398-157">Usar un IAuthorizationPolicyProvider personalizado</span><span class="sxs-lookup"><span data-stu-id="dd398-157">Using a Custom IAuthorizationPolicyProvider</span></span>

<span data-ttu-id="dd398-158">Para usar directivas personalizadas de un `IAuthorizationPolicyProvider`, debe:</span><span class="sxs-lookup"><span data-stu-id="dd398-158">To use custom policies from an `IAuthorizationPolicyProvider`, you must:</span></span>

* <span data-ttu-id="dd398-159">Registrar la correspondiente `AuthorizationHandler` tipos con inserción de dependencias (se describe en [autorización basada en directivas](xref:security/authorization/policies#authorization-handlers)), al igual que con todos los escenarios de autorización basada en directivas.</span><span class="sxs-lookup"><span data-stu-id="dd398-159">Register the appropriate `AuthorizationHandler` types with dependency injection (described in [policy-based authorization](xref:security/authorization/policies#authorization-handlers)), as with all policy-based authorization scenarios.</span></span>
* <span data-ttu-id="dd398-160">Registrar personalizado `IAuthorizationPolicyProvider` tipo de colección de servicio de inyección de dependencia de la aplicación (en `Startup.ConfigureServices`) para reemplazar el proveedor de directivas predeterminado.</span><span class="sxs-lookup"><span data-stu-id="dd398-160">Register the custom `IAuthorizationPolicyProvider` type in the app's dependency injection service collection (in `Startup.ConfigureServices`) to replace the default policy provider.</span></span>

```CSharp
services.AddTransient<IAuthorizationPolicyProvider, MinimumAgePolicyProvider>();
```

<span data-ttu-id="dd398-161">Un personalizado completo `IAuthorizationPolicyProvider` ejemplo está disponible en la [repositorio de GitHub de aspnet/AuthSamples](https://github.com/aspnet/AuthSamples/tree/dev/samples/CustomPolicyProvider).</span><span class="sxs-lookup"><span data-stu-id="dd398-161">A complete custom `IAuthorizationPolicyProvider` sample is available in the [aspnet/AuthSamples GitHub repository](https://github.com/aspnet/AuthSamples/tree/dev/samples/CustomPolicyProvider).</span></span>
