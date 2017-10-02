---
title: "Mediante la autenticación de Cookie sin identidad principal de ASP.NET"
author: rick-anderson
description: "Obtener una explicación del uso de autenticación con cookies sin ASP.NET Core Identity"
keywords: "Núcleo de ASP.NET, las cookies"
ms.author: riande
manager: wpickett
ms.date: 08/14/2017
ms.topic: article
ms.assetid: 2bdcbf95-8d9d-4537-a4a0-a5ee439dcb62
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/cookie
ms.openlocfilehash: e5c53a7044edb56e065b2dc1536343fdaf9fb007
ms.sourcegitcommit: 7d8f4e3443a2989a64343f8fec83e6a4c4ed2f97
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/29/2017
---
# <a name="using-cookie-authentication-without-aspnet-core-identity"></a><span data-ttu-id="130bd-104">Mediante la autenticación de Cookie sin identidad principal de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="130bd-104">Using Cookie Authentication without ASP.NET Core Identity</span></span>

<a name="security-authentication-cookie-middleware"></a>

<span data-ttu-id="130bd-105">ASP.NET Core 1.x proporciona una cookie [middleware](../../fundamentals/middleware.md#fundamentals-middleware) que serializa una entidad de seguridad de usuario en una cookie cifrada y, a continuación, en solicitudes posteriores, valida la cookie, vuelve a crear la entidad de seguridad y lo asigna a la `HttpContext.User` propiedad .</span><span class="sxs-lookup"><span data-stu-id="130bd-105">ASP.NET Core 1.x provides cookie [middleware](../../fundamentals/middleware.md#fundamentals-middleware) which serializes a user principal into an encrypted cookie and then, on subsequent requests, validates the cookie, recreates the principal, and assigns it to the `HttpContext.User` property.</span></span> <span data-ttu-id="130bd-106">Si desea proporcionar sus propias pantallas de inicio de sesión y las bases de datos de usuario, puede usar el middleware de cookies como una característica independiente.</span><span class="sxs-lookup"><span data-stu-id="130bd-106">If you want to provide your own login screens and user databases, you can use the cookie middleware as a standalone feature.</span></span>

<span data-ttu-id="130bd-107">Un cambio importante en ASP.NET Core 2.x es que el middleware de cookies está ausente.</span><span class="sxs-lookup"><span data-stu-id="130bd-107">A major change in ASP.NET Core 2.x is that the cookie middleware is absent.</span></span> <span data-ttu-id="130bd-108">En su lugar, el `UseAuthentication` invocación del método en el `Configure` método *Startup.cs* agrega el AuthenticationMiddleware que establece el `HttpContext.User` propiedad.</span><span class="sxs-lookup"><span data-stu-id="130bd-108">Instead, the `UseAuthentication` method invocation in the `Configure` method of *Startup.cs* adds the AuthenticationMiddleware which sets the `HttpContext.User` property.</span></span>

<a name="security-authentication-cookie-middleware-configuring"></a>

## <a name="adding-and-configuring"></a><span data-ttu-id="130bd-109">Agregar y configurar</span><span class="sxs-lookup"><span data-stu-id="130bd-109">Adding and configuring</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="130bd-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="130bd-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="130bd-111">Complete los pasos siguientes:</span><span class="sxs-lookup"><span data-stu-id="130bd-111">Complete the following steps:</span></span>

- <span data-ttu-id="130bd-112">Si no usa el `Microsoft.AspNetCore.All` [metapackage](xref:fundamentals/metapackage), instale la versión 2.0 del `Microsoft.AspNetCore.Authentication.Cookies` paquete de NuGet en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="130bd-112">If not using the `Microsoft.AspNetCore.All` [metapackage](xref:fundamentals/metapackage), install version 2.0+ of the `Microsoft.AspNetCore.Authentication.Cookies` NuGet package in your project.</span></span>

- <span data-ttu-id="130bd-113">Invocar la `UseAuthentication` método en el `Configure` método de la *Startup.cs* archivo:</span><span class="sxs-lookup"><span data-stu-id="130bd-113">Invoke the `UseAuthentication` method in the `Configure` method of the *Startup.cs* file:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="130bd-114">Invocar la `AddAuthentication` y `AddCookie` métodos en el `ConfigureServices` método de la *Startup.cs* archivo:</span><span class="sxs-lookup"><span data-stu-id="130bd-114">Invoke the `AddAuthentication` and `AddCookie` methods in the `ConfigureServices` method of the *Startup.cs* file:</span></span>

    ```csharp
    services.AddAuthentication("MyCookieAuthenticationScheme")
            .AddCookie("MyCookieAuthenticationScheme", options => {
                options.AccessDeniedPath = "/Account/Forbidden/";
                options.LoginPath = "/Account/Unauthorized/";
            });
    ```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="130bd-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="130bd-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="130bd-116">Complete los pasos siguientes:</span><span class="sxs-lookup"><span data-stu-id="130bd-116">Complete the following steps:</span></span>

- <span data-ttu-id="130bd-117">Instalar el `Microsoft.AspNetCore.Authentication.Cookies` paquete de NuGet en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="130bd-117">Install the `Microsoft.AspNetCore.Authentication.Cookies` NuGet package in your project.</span></span> <span data-ttu-id="130bd-118">Este paquete contiene el middleware de cookies.</span><span class="sxs-lookup"><span data-stu-id="130bd-118">This package contains the cookie middleware.</span></span>

- <span data-ttu-id="130bd-119">Agregue las líneas siguientes a la `Configure` método en su *Startup.cs* archivo antes de la `app.UseMvc()` instrucción:</span><span class="sxs-lookup"><span data-stu-id="130bd-119">Add the following lines to the `Configure` method in your *Startup.cs* file before the `app.UseMvc()` statement:</span></span>

    ```csharp
    app.UseCookieAuthentication(new CookieAuthenticationOptions()
    {
        AccessDeniedPath = "/Account/Forbidden/",
        AuthenticationScheme = "MyCookieAuthenticationScheme",
        AutomaticAuthenticate = true,
        AutomaticChallenge = true,
        LoginPath = "/Account/Unauthorized/"
    });
    ```

---

<span data-ttu-id="130bd-120">Los fragmentos de código anteriores configurar algunas o todas las opciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="130bd-120">The code snippets above configure some or all of the following options:</span></span>

* <span data-ttu-id="130bd-121">`AccessDeniedPath`-Ésta es la ruta de acceso relativa a la que redirección las solicitudes cuando un usuario intenta obtener acceso a un recurso pero no pasa cualquier [directivas de autorización](xref:security/authorization/policies#security-authorization-policies-based) para ese recurso.</span><span class="sxs-lookup"><span data-stu-id="130bd-121">`AccessDeniedPath` - This is the relative path to which requests redirect when a user attempts to access a resource but does not pass any [authorization policies](xref:security/authorization/policies#security-authorization-policies-based) for that resource.</span></span>

* <span data-ttu-id="130bd-122">`AuthenticationScheme`-Éste es un valor por el que se conoce un esquema de autenticación de cookie en particular.</span><span class="sxs-lookup"><span data-stu-id="130bd-122">`AuthenticationScheme` - This is a value by which a particular cookie authentication scheme is known.</span></span> <span data-ttu-id="130bd-123">Esto es útil cuando hay varias instancias de autenticación con cookies y desea [limitar la autorización a una instancia](xref:security/authorization/limitingidentitybyscheme#security-authorization-limiting-by-scheme).</span><span class="sxs-lookup"><span data-stu-id="130bd-123">This is useful when there are multiple instances of cookie authentication and you want to [limit authorization to one instance](xref:security/authorization/limitingidentitybyscheme#security-authorization-limiting-by-scheme).</span></span>

* <span data-ttu-id="130bd-124">`AutomaticAuthenticate`-Esta marca solo es relevante para ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="130bd-124">`AutomaticAuthenticate` - This flag is relevant only for ASP.NET Core 1.x.</span></span> <span data-ttu-id="130bd-125">Indica que la autenticación con cookies debe ejecutarse en cada solicitud y que intentan validar y reconstruir cualquier entidad de seguridad serializado que creó.</span><span class="sxs-lookup"><span data-stu-id="130bd-125">It indicates that the cookie authentication should run on every request and attempt to validate and reconstruct any serialized principal it created.</span></span>

* <span data-ttu-id="130bd-126">`AutomaticChallenge`-Esta marca solo es relevante para ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="130bd-126">`AutomaticChallenge` - This flag is relevant only for ASP.NET Core 1.x.</span></span> <span data-ttu-id="130bd-127">Indica que la autenticación con cookies 1.x debe redirigir el explorador a la `LoginPath` o `AccessDeniedPath` cuando se produce un error en la autorización.</span><span class="sxs-lookup"><span data-stu-id="130bd-127">It indicates that the 1.x cookie authentication should redirect the browser to the `LoginPath` or `AccessDeniedPath` when authorization fails.</span></span>

* <span data-ttu-id="130bd-128">`LoginPath`-Ésta es la ruta de acceso relativa a la que redirección las solicitudes cuando un usuario intenta obtener acceso a un recurso pero no se ha autenticado.</span><span class="sxs-lookup"><span data-stu-id="130bd-128">`LoginPath` - This is the relative path to which requests redirect when a user attempts to access a resource but has not been authenticated.</span></span>

<span data-ttu-id="130bd-129">[Otras opciones](xref:security/authentication/cookie#security-authentication-cookie-options) incluyen la capacidad de establecer el emisor de notificaciones crea la autenticación con cookies, se quita el nombre de la cookie de la autenticación, el dominio de la cookie y diversas propiedades de seguridad en la cookie.</span><span class="sxs-lookup"><span data-stu-id="130bd-129">[Other options](xref:security/authentication/cookie#security-authentication-cookie-options) include the ability to set the issuer for any claims the cookie authentication creates, the name of the cookie the authentication drops, the domain for the cookie and various security properties on the cookie.</span></span> <span data-ttu-id="130bd-130">De forma predeterminada, la autenticación con cookies utiliza opciones de seguridad adecuadas para las cookies que cree, como:</span><span class="sxs-lookup"><span data-stu-id="130bd-130">By default, the cookie authentication uses appropriate security options for any cookies it creates, such as:</span></span>
- <span data-ttu-id="130bd-131">Si establece la marca HttpOnly para impedir el acceso de la cookie en JavaScript del lado cliente</span><span class="sxs-lookup"><span data-stu-id="130bd-131">Setting the HttpOnly flag to prevent cookie access in client-side JavaScript</span></span>
- <span data-ttu-id="130bd-132">Limita la cookie a HTTPS si una solicitud ha viajado a través de HTTPS</span><span class="sxs-lookup"><span data-stu-id="130bd-132">Limiting the cookie to HTTPS if a request has traveled over HTTPS</span></span>

<a name="security-authentication-cookie-middleware-creating-a-cookie"></a>

## <a name="creating-an-identity-cookie"></a><span data-ttu-id="130bd-133">Crear una cookie de identidad</span><span class="sxs-lookup"><span data-stu-id="130bd-133">Creating an Identity cookie</span></span>

<span data-ttu-id="130bd-134">Para crear una cookie que contiene la información de usuario, debe construir un [ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal) que contiene la información que se va a serializar en la cookie.</span><span class="sxs-lookup"><span data-stu-id="130bd-134">To create a cookie holding your user information, you must construct a [ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal) holding the information you wish to be serialized in the cookie.</span></span> <span data-ttu-id="130bd-135">Una vez que tenga una adecuado `ClaimsPrincipal` de objeto, llame a lo siguiente dentro del método de controlador:</span><span class="sxs-lookup"><span data-stu-id="130bd-135">Once you have a suitable `ClaimsPrincipal` object, call the following inside your controller method:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="130bd-136">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="130bd-136">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync("MyCookieAuthenticationScheme", principal);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="130bd-137">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="130bd-137">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync("MyCookieAuthenticationScheme", principal);
```

---

<span data-ttu-id="130bd-138">Esto crea una cookie cifrada y lo agrega a la respuesta actual.</span><span class="sxs-lookup"><span data-stu-id="130bd-138">This creates an encrypted cookie and adds it to the current response.</span></span> <span data-ttu-id="130bd-139">El `AuthenticationScheme` especificado durante la [configuración](xref:security/authentication/cookie#security-authentication-cookie-middleware-configuring) debe usarse al llamar a `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="130bd-139">The `AuthenticationScheme` specified during [configuration](xref:security/authentication/cookie#security-authentication-cookie-middleware-configuring) must be used when calling `SignInAsync`.</span></span>

<span data-ttu-id="130bd-140">Tras los bastidores, el cifrado usado es ASP.NET Core [protección de datos](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) sistema.</span><span class="sxs-lookup"><span data-stu-id="130bd-140">Under the covers, the encryption used is ASP.NET Core's [Data Protection](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) system.</span></span> <span data-ttu-id="130bd-141">Si hospedan en varios equipos, equilibrio de carga, o usar una granja de servidores web, a continuación, deberá [configurar la protección de datos](xref:security/data-protection/configuration/overview#data-protection-configuring) para usar el mismo anillo de clave y el identificador de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="130bd-141">If you are hosting on multiple machines, load balancing, or using a web farm, then you need to [configure data protection](xref:security/data-protection/configuration/overview#data-protection-configuring) to use the same key ring and application identifier.</span></span>

## <a name="signing-out"></a><span data-ttu-id="130bd-142">Cierre de sesión</span><span class="sxs-lookup"><span data-stu-id="130bd-142">Signing out</span></span>

<span data-ttu-id="130bd-143">Para cerrar la sesión del usuario actual y eliminar las cookies, llame a lo siguiente en el controlador:</span><span class="sxs-lookup"><span data-stu-id="130bd-143">To sign out the current user and delete their cookie, call the following inside your controller:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="130bd-144">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="130bd-144">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
await HttpContext.SignOutAsync("MyCookieAuthenticationScheme");
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="130bd-145">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="130bd-145">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignOutAsync("MyCookieAuthenticationScheme");
```

---

## <a name="reacting-to-back-end-changes"></a><span data-ttu-id="130bd-146">Reaccionar ante los cambios de back-end</span><span class="sxs-lookup"><span data-stu-id="130bd-146">Reacting to back-end changes</span></span>

>[!WARNING]
> <span data-ttu-id="130bd-147">Una vez creada una cookie principal, se convierte en el único origen de identidad.</span><span class="sxs-lookup"><span data-stu-id="130bd-147">Once a principal cookie has been created, it becomes the single source of identity.</span></span> <span data-ttu-id="130bd-148">Incluso si se deshabilita a un usuario en los sistemas back-end, la autenticación con cookies no tiene ningún conocimiento de este, y un usuario permanece ha iniciado sesión como su cookie es válida.</span><span class="sxs-lookup"><span data-stu-id="130bd-148">Even if you disable a user in your back-end systems, the cookie authentication has no knowledge of this, and a user stays logged in as long as their cookie is valid.</span></span>

<span data-ttu-id="130bd-149">La autenticación con cookies proporciona una serie de eventos en su clase de opción.</span><span class="sxs-lookup"><span data-stu-id="130bd-149">The cookie authentication provides a series of events in its option class.</span></span> <span data-ttu-id="130bd-150">El `ValidateAsync()` evento puede utilizarse para interceptar e invalidar la validación de la identidad de la cookie.</span><span class="sxs-lookup"><span data-stu-id="130bd-150">The `ValidateAsync()` event can be used to intercept and override validation of the cookie identity.</span></span>

<span data-ttu-id="130bd-151">Considere la posibilidad de una base de datos de usuario de back-end que puede tener una columna "LastChanged".</span><span class="sxs-lookup"><span data-stu-id="130bd-151">Consider a back-end user database that may have a "LastChanged" column.</span></span> <span data-ttu-id="130bd-152">Con el fin de invalidar una cookie cuando cambia la base de datos, debe en primer lugar, cuando [crear la cookie](xref:security/authentication/cookie#security-authentication-cookie-middleware-creating-a-cookie), agregar una notificación de "LastChanged" que contiene el valor actual.</span><span class="sxs-lookup"><span data-stu-id="130bd-152">In order to invalidate a cookie when the database changes, you should first, when [creating the cookie](xref:security/authentication/cookie#security-authentication-cookie-middleware-creating-a-cookie), add a "LastChanged" claim containing the current value.</span></span> <span data-ttu-id="130bd-153">Cuando se cambia la base de datos, debe actualizarse el valor de "LastChanged".</span><span class="sxs-lookup"><span data-stu-id="130bd-153">When the database changes, the "LastChanged" value should be updated.</span></span>

<span data-ttu-id="130bd-154">Para implementar una invalidación para el `ValidateAsync()` eventos, debe escribir un método con la siguiente firma:</span><span class="sxs-lookup"><span data-stu-id="130bd-154">To implement an override for the `ValidateAsync()` event, you must write a method with the following signature:</span></span>

```csharp
Task ValidateAsync(CookieValidatePrincipalContext context);
```

<span data-ttu-id="130bd-155">ASP.NET Core Identity implementa esta comprobación como parte de su `SecurityStampValidator`.</span><span class="sxs-lookup"><span data-stu-id="130bd-155">ASP.NET Core Identity implements this check as part of its `SecurityStampValidator`.</span></span> <span data-ttu-id="130bd-156">Un ejemplo es similar a lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="130bd-156">An example looks like the following:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="130bd-157">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="130bd-157">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
public static class LastChangedValidator
{
    public static async Task ValidateAsync(CookieValidatePrincipalContext context)
    {
        // Pull database from registered DI services.
        var userRepository = context.HttpContext.RequestServices.GetRequiredService<IUserRepository>();
        var userPrincipal = context.Principal;

        // Look for the last changed claim.
        string lastChanged;
        lastChanged = (from c in userPrincipal.Claims
                        where c.Type == "LastUpdated"
                        select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !userRepository.ValidateLastChanged(userPrincipal, lastChanged))
        {
            context.RejectPrincipal();
            await context.HttpContext.SignOutAsync("MyCookieAuthenticationScheme");
        }
    }
}
```

<span data-ttu-id="130bd-158">Esto podría, a continuación, conectarse durante el registro de servicio de cookie en el `ConfigureServices` método *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="130bd-158">This would then be wired up during cookie service registration in the `ConfigureServices` method of *Startup.cs*:</span></span>

```csharp
services.AddAuthentication("MyCookieAuthenticationScheme")
        .AddCookie("MyCookieAuthenticationScheme", options =>
        {
            options.Events = new CookieAuthenticationEvents
            {
                OnValidatePrincipal = LastChangedValidator.ValidateAsync
            };
        });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="130bd-159">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="130bd-159">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
public static class LastChangedValidator
{
    public static async Task ValidateAsync(CookieValidatePrincipalContext context)
    {
        // Pull database from registered DI services.
        var userRepository = context.HttpContext.RequestServices.GetRequiredService<IUserRepository>();
        var userPrincipal = context.Principal;

        // Look for the last changed claim.
        string lastChanged;
        lastChanged = (from c in userPrincipal.Claims
                        where c.Type == "LastUpdated"
                        select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !userRepository.ValidateLastChanged(userPrincipal, lastChanged))
        {
            context.RejectPrincipal();
            await context.HttpContext.Authentication.SignOutAsync("MyCookieAuthenticationScheme");
        }
    }
}
```

<span data-ttu-id="130bd-160">Esto, a continuación, podría estar conectado seguridad durante la configuración de autenticación de cookie en el `Configure` método *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="130bd-160">This would then be wired up during cookie authentication configuration in the `Configure` method of *Startup.cs*:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    Events = new CookieAuthenticationEvents
    {
        OnValidatePrincipal = LastChangedValidator.ValidateAsync
    }
});
```

---

<span data-ttu-id="130bd-161">Considere el ejemplo en el que se ha actualizado su nombre &mdash; una decisión que no afectan a la seguridad de ninguna manera.</span><span class="sxs-lookup"><span data-stu-id="130bd-161">Consider the example in which their name has been updated &mdash; a decision which doesn't affect security in any way.</span></span> <span data-ttu-id="130bd-162">Si desea actualizar la entidad de seguridad de usuario de manera no destructiva, puede llamar a `context.ReplacePrincipal()` y establezca el `context.ShouldRenew` propiedad `true`.</span><span class="sxs-lookup"><span data-stu-id="130bd-162">If you want to non-destructively update the user principal, you can call `context.ReplacePrincipal()` and set the `context.ShouldRenew` property to `true`.</span></span>

<a name="security-authentication-cookie-options"></a>

## <a name="controlling-cookie-options"></a><span data-ttu-id="130bd-163">Controlar opciones de cookie</span><span class="sxs-lookup"><span data-stu-id="130bd-163">Controlling cookie options</span></span>

<span data-ttu-id="130bd-164">El [CookieAuthenticationOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) clase viene con distintas opciones de configuración para ajustar las cookies que se está creadas.</span><span class="sxs-lookup"><span data-stu-id="130bd-164">The [CookieAuthenticationOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) class comes with various configuration options to fine-tune the cookies being created.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="130bd-165">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="130bd-165">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="130bd-166">ASP.NET Core 2.x unifica las API utilizadas para la configuración de cookies.</span><span class="sxs-lookup"><span data-stu-id="130bd-166">ASP.NET Core 2.x unifies the APIs used for configuring cookies.</span></span> <span data-ttu-id="130bd-167">1.x API se han marcado como obsoletas y una nueva `Cookie` propiedad de tipo `CookieBuilder` se ha introducido en el `CookieAuthenticationOptions` clase.</span><span class="sxs-lookup"><span data-stu-id="130bd-167">The 1.x APIs have been marked as obsolete, and a new `Cookie` property of type `CookieBuilder` has been introduced in the `CookieAuthenticationOptions` class.</span></span> <span data-ttu-id="130bd-168">Se recomienda encarecidamente que migre a la API de 2.x.</span><span class="sxs-lookup"><span data-stu-id="130bd-168">It's recommended that you migrate to the 2.x APIs.</span></span>

* <span data-ttu-id="130bd-169">`ClaimsIssuer`es el emisor que se usará para la [emisor](https://docs.microsoft.com/dotnet/api/system.security.claims.claim.issuer) propiedad en las notificaciones que se creó mediante la autenticación con cookies.</span><span class="sxs-lookup"><span data-stu-id="130bd-169">`ClaimsIssuer` is the issuer to be used for the [Issuer](https://docs.microsoft.com/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the cookie authentication.</span></span>

* <span data-ttu-id="130bd-170">`CookieBuilder.Domain`es el nombre de dominio al que se sirve la cookie.</span><span class="sxs-lookup"><span data-stu-id="130bd-170">`CookieBuilder.Domain` is the domain name to which the cookie is served.</span></span> <span data-ttu-id="130bd-171">De forma predeterminada, este es el nombre de host que se envió la solicitud al.</span><span class="sxs-lookup"><span data-stu-id="130bd-171">By default, this is the host name the request was sent to.</span></span> <span data-ttu-id="130bd-172">El explorador sólo sirve la cookie a un nombre de host coincidente.</span><span class="sxs-lookup"><span data-stu-id="130bd-172">The browser only serves the cookie to a matching host name.</span></span> <span data-ttu-id="130bd-173">Puede que desee ajustar esta opción para que las cookies disponibles para todos los hosts en el dominio.</span><span class="sxs-lookup"><span data-stu-id="130bd-173">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="130bd-174">Por ejemplo, establecer el dominio de cookies `.contoso.com` pone a disposición `contoso.com`, `www.contoso.com`, `staging.www.contoso.com`, etcetera.</span><span class="sxs-lookup"><span data-stu-id="130bd-174">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, `staging.www.contoso.com`, etc.</span></span>

* <span data-ttu-id="130bd-175">`CookieBuilder.HttpOnly`es una marca que indica si la cookie debe ser accesible sólo a los servidores.</span><span class="sxs-lookup"><span data-stu-id="130bd-175">`CookieBuilder.HttpOnly` is a flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="130bd-176">El valor predeterminado es `true`.</span><span class="sxs-lookup"><span data-stu-id="130bd-176">This defaults to `true`.</span></span> <span data-ttu-id="130bd-177">Si cambia este valor puede abrir la aplicación al robo de cookies debe la aplicación tenga un error de Scripting entre sitios.</span><span class="sxs-lookup"><span data-stu-id="130bd-177">Changing this value may open your application to cookie theft should your application have a Cross-Site Scripting bug.</span></span>

* <span data-ttu-id="130bd-178">`CookieBuilder.Path`puede utilizarse para aislar las aplicaciones que se ejecutan en el mismo nombre de host.</span><span class="sxs-lookup"><span data-stu-id="130bd-178">`CookieBuilder.Path` can be used to isolate applications running on the same host name.</span></span> <span data-ttu-id="130bd-179">Si tiene aplicaciones que se ejecutan `/app1` y desea limitar las cookies que se emite para sólo se enviará a esa aplicación, a continuación, debe establecer el `CookiePath` propiedad `/app1`.</span><span class="sxs-lookup"><span data-stu-id="130bd-179">If you have an app running in `/app1` and want to limit the cookies issued to just be sent to that application, then you should set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="130bd-180">Al hacerlo, la cookie solo está disponible para las solicitudes a `/app1` o cualquier aparecen debajo de él.</span><span class="sxs-lookup"><span data-stu-id="130bd-180">By doing so, the cookie is only available to requests to `/app1` or anything underneath it.</span></span>

* <span data-ttu-id="130bd-181">`CookieBuilder.SameSite`indica si el explorador debe permitir que la cookie que se adjuntará a las solicitudes del mismo sitio o entre sitios.</span><span class="sxs-lookup"><span data-stu-id="130bd-181">`CookieBuilder.SameSite` indicates whether the browser should allow the cookie to be attached to same-site or cross-site requests.</span></span> <span data-ttu-id="130bd-182">El valor predeterminado es `SameSiteMode.Lax`.</span><span class="sxs-lookup"><span data-stu-id="130bd-182">This defaults to `SameSiteMode.Lax`.</span></span>

* <span data-ttu-id="130bd-183">`CookieBuilder.SecurePolicy`es una marca que indica si la cookie creada debe limitarse a HTTPS, HTTP o HTTPS o el mismo protocolo que la solicitud.</span><span class="sxs-lookup"><span data-stu-id="130bd-183">`CookieBuilder.SecurePolicy` is a flag indicating if the cookie created should be limited to HTTPS, HTTP or HTTPS, or the same protocol as the request.</span></span> <span data-ttu-id="130bd-184">El valor predeterminado es `SameAsRequest`.</span><span class="sxs-lookup"><span data-stu-id="130bd-184">This defaults to `SameAsRequest`.</span></span>

* <span data-ttu-id="130bd-185">`ExpireTimeSpan`es el `TimeSpan` tras el cual expira la cookie.</span><span class="sxs-lookup"><span data-stu-id="130bd-185">`ExpireTimeSpan` is the `TimeSpan` after which the cookie expires.</span></span> <span data-ttu-id="130bd-186">Se agrega a la fecha y hora actuales para crear la fecha de expiración de la cookie.</span><span class="sxs-lookup"><span data-stu-id="130bd-186">It's added to the current date and time to create the expiry date for the cookie.</span></span>

* <span data-ttu-id="130bd-187">`SlidingExpiration`es una marca que indica si la fecha de expiración de la cookie restablece cuando haya más de la mitad de la `ExpireTimeSpan` ha transcurrido el intervalo.</span><span class="sxs-lookup"><span data-stu-id="130bd-187">`SlidingExpiration` is a flag indicating whether the cookie expiration date resets when more than half of the `ExpireTimeSpan` interval has passed.</span></span> <span data-ttu-id="130bd-188">La nueva fecha de expiración se mueve hacia delante como la fecha actual más el `ExpireTimespan`.</span><span class="sxs-lookup"><span data-stu-id="130bd-188">The new expiry date is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="130bd-189">Un [la hora de expiración absoluta](xref:security/authentication/cookie#security-authentication-absolute-expiry) puede establecerse mediante el `AuthenticationProperties` al llamar a la clase `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="130bd-189">An [absolute expiry time](xref:security/authentication/cookie#security-authentication-absolute-expiry) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="130bd-190">Una fecha de expiración absoluta puede mejorar la seguridad de la aplicación mediante la limitación de la cantidad de tiempo para el que la cookie de autenticación es válida.</span><span class="sxs-lookup"><span data-stu-id="130bd-190">An absolute expiry can improve the security of your application by limiting the amount of time for which the authentication cookie is valid.</span></span>

<span data-ttu-id="130bd-191">Un ejemplo del uso `CookieAuthenticationOptions` en el `ConfigureServices` método *Startup.cs* sigue:</span><span class="sxs-lookup"><span data-stu-id="130bd-191">An example of using `CookieAuthenticationOptions` in the `ConfigureServices` method of *Startup.cs* follows:</span></span>

```csharp
services.AddAuthentication("MyCookieAuthenticationScheme")
        .AddCookie("MyCookieAuthenticationScheme", options =>
        {
            options.Cookie.Name = "AuthCookie";
            options.Cookie.Domain = "contoso.com";
            options.Cookie.Path = "/";
            options.Cookie.HttpOnly = true;
            options.Cookie.SameSite = SameSiteMode.Lax;
            options.Cookie.SecurePolicy = CookieSecurePolicy.Always;
        });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="130bd-192">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="130bd-192">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* <span data-ttu-id="130bd-193">`ClaimsIssuer`es el emisor que se usará para la [emisor](https://docs.microsoft.com/dotnet/api/system.security.claims.claim.issuer) propiedad en las notificaciones que se creó el middleware.</span><span class="sxs-lookup"><span data-stu-id="130bd-193">`ClaimsIssuer` is the issuer to be used for the [Issuer](https://docs.microsoft.com/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the middleware.</span></span>

* <span data-ttu-id="130bd-194">`CookieDomain`es el nombre de dominio al que se sirve la cookie.</span><span class="sxs-lookup"><span data-stu-id="130bd-194">`CookieDomain` is the domain name to which the cookie is served.</span></span> <span data-ttu-id="130bd-195">De forma predeterminada, este es el nombre de host que se envió la solicitud al.</span><span class="sxs-lookup"><span data-stu-id="130bd-195">By default, this is the host name the request was sent to.</span></span> <span data-ttu-id="130bd-196">El explorador sólo sirve la cookie a un nombre de host coincidente.</span><span class="sxs-lookup"><span data-stu-id="130bd-196">The browser only serves the cookie to a matching host name.</span></span> <span data-ttu-id="130bd-197">Puede que desee ajustar esta opción para que las cookies disponibles para todos los hosts en el dominio.</span><span class="sxs-lookup"><span data-stu-id="130bd-197">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="130bd-198">Por ejemplo, establecer el dominio de cookies `.contoso.com` pone a disposición `contoso.com`, `www.contoso.com`, `staging.www.contoso.com`, etcetera.</span><span class="sxs-lookup"><span data-stu-id="130bd-198">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, `staging.www.contoso.com`, etc.</span></span>

* <span data-ttu-id="130bd-199">`CookieHttpOnly`es una marca que indica si la cookie debe ser accesible sólo a los servidores.</span><span class="sxs-lookup"><span data-stu-id="130bd-199">`CookieHttpOnly` is a flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="130bd-200">El valor predeterminado es `true`.</span><span class="sxs-lookup"><span data-stu-id="130bd-200">This defaults to `true`.</span></span> <span data-ttu-id="130bd-201">Si cambia este valor puede abrir la aplicación al robo de cookies debe la aplicación tenga un error de Scripting entre sitios.</span><span class="sxs-lookup"><span data-stu-id="130bd-201">Changing this value may open your application to cookie theft should your application have a Cross-Site Scripting bug.</span></span>

* <span data-ttu-id="130bd-202">`CookiePath`puede utilizarse para aislar las aplicaciones que se ejecutan en el mismo nombre de host.</span><span class="sxs-lookup"><span data-stu-id="130bd-202">`CookiePath` can be used to isolate applications running on the same host name.</span></span> <span data-ttu-id="130bd-203">Si tiene aplicaciones que se ejecutan `/app1` y desea limitar las cookies que se emite para sólo se enviará a esa aplicación, a continuación, debe establecer el `CookiePath` propiedad `/app1`.</span><span class="sxs-lookup"><span data-stu-id="130bd-203">If you have an app running in `/app1` and want to limit the cookies issued to just be sent to that application, then you should set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="130bd-204">Al hacerlo, la cookie solo está disponible para las solicitudes a `/app1` o cualquier aparecen debajo de él.</span><span class="sxs-lookup"><span data-stu-id="130bd-204">By doing so, the cookie is only available to requests to `/app1` or anything underneath it.</span></span>

* <span data-ttu-id="130bd-205">`CookieSecure`es una marca que indica si la cookie creada debe limitarse a HTTPS, HTTP o HTTPS o el mismo protocolo que la solicitud.</span><span class="sxs-lookup"><span data-stu-id="130bd-205">`CookieSecure` is a flag indicating if the cookie created should be limited to HTTPS, HTTP or HTTPS, or the same protocol as the request.</span></span> <span data-ttu-id="130bd-206">El valor predeterminado es `SameAsRequest`.</span><span class="sxs-lookup"><span data-stu-id="130bd-206">This defaults to `SameAsRequest`.</span></span>

* <span data-ttu-id="130bd-207">`ExpireTimeSpan`es el `TimeSpan` tras el cual expira la cookie.</span><span class="sxs-lookup"><span data-stu-id="130bd-207">`ExpireTimeSpan` is the `TimeSpan` after which the cookie expires.</span></span> <span data-ttu-id="130bd-208">Se agrega a la fecha y hora actuales para crear la fecha de expiración de la cookie.</span><span class="sxs-lookup"><span data-stu-id="130bd-208">It's added to the current date and time to create the expiry date for the cookie.</span></span>

* <span data-ttu-id="130bd-209">`SlidingExpiration`es una marca que indica si la fecha de expiración de la cookie restablece cuando haya más de la mitad de la `ExpireTimeSpan` ha transcurrido el intervalo.</span><span class="sxs-lookup"><span data-stu-id="130bd-209">`SlidingExpiration` is a flag indicating whether the cookie expiration date resets when more than half of the `ExpireTimeSpan` interval has passed.</span></span> <span data-ttu-id="130bd-210">La nueva fecha de expiración se mueve hacia delante como la fecha actual más el `ExpireTimespan`.</span><span class="sxs-lookup"><span data-stu-id="130bd-210">The new expiry date is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="130bd-211">Un [la hora de expiración absoluta](xref:security/authentication/cookie#security-authentication-absolute-expiry) puede establecerse mediante el `AuthenticationProperties` al llamar a la clase `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="130bd-211">An [absolute expiry time](xref:security/authentication/cookie#security-authentication-absolute-expiry) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="130bd-212">Una fecha de expiración absoluta puede mejorar la seguridad de la aplicación mediante la limitación de la cantidad de tiempo para el que la cookie de autenticación es válida.</span><span class="sxs-lookup"><span data-stu-id="130bd-212">An absolute expiry can improve the security of your application by limiting the amount of time for which the authentication cookie is valid.</span></span>

<span data-ttu-id="130bd-213">Un ejemplo del uso `CookieAuthenticationOptions` en el `Configure` método *Startup.cs* sigue:</span><span class="sxs-lookup"><span data-stu-id="130bd-213">An example of using `CookieAuthenticationOptions` in the `Configure` method of *Startup.cs* follows:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    CookieName = "AuthCookie",
    CookieDomain = "contoso.com",
    CookiePath = "/",
    CookieHttpOnly = true,
    CookieSecure = CookieSecurePolicy.Always
});
```

---

## <a name="persistent-cookies-and-absolute-expiry-times"></a><span data-ttu-id="130bd-214">Las cookies persistentes y las horas de expiración absoluta</span><span class="sxs-lookup"><span data-stu-id="130bd-214">Persistent cookies and absolute expiry times</span></span>

<span data-ttu-id="130bd-215">Puede que desee la expiración de la cookie se conservan entre sesiones del explorador y desea que una fecha de expiración absoluta para la identidad y el token de transporte.</span><span class="sxs-lookup"><span data-stu-id="130bd-215">You may want the cookie expiry to persist across browser sessions and want an absolute expiry to the identity and the cookie transporting it.</span></span> <span data-ttu-id="130bd-216">Esta persistencia solo debería habilitarse con el consentimiento del usuario explícita, a través de una casilla "Recordar mi cuenta" en Inicio de sesión o un mecanismo similar.</span><span class="sxs-lookup"><span data-stu-id="130bd-216">This persistence should only be enabled with explicit user consent, via a "Remember Me" checkbox on login or a similar mechanism.</span></span> <span data-ttu-id="130bd-217">Puede realizar estas acciones mediante el uso de la `AuthenticationProperties` parámetro en el `SignInAsync` llama al método cuando [en una identidad de firma y la creación de la cookie](xref:security/authentication/cookie#security-authentication-cookie-middleware-creating-a-cookie).</span><span class="sxs-lookup"><span data-stu-id="130bd-217">You can do these things by using the `AuthenticationProperties` parameter on the `SignInAsync` method called when [signing in an identity and creating the cookie](xref:security/authentication/cookie#security-authentication-cookie-middleware-creating-a-cookie).</span></span> <span data-ttu-id="130bd-218">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="130bd-218">For example:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="130bd-219">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="130bd-219">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    "MyCookieAuthenticationScheme",
    principal,
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

<span data-ttu-id="130bd-220">El `AuthenticationProperties` (clase), utilizado en el fragmento de código anterior, se encuentra en la `Microsoft.AspNetCore.Authentication` espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="130bd-220">The `AuthenticationProperties` class, used in the preceding code snippet, resides in the `Microsoft.AspNetCore.Authentication` namespace.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="130bd-221">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="130bd-221">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync(
    "MyCookieAuthenticationScheme",
    principal,
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

<span data-ttu-id="130bd-222">El `AuthenticationProperties` (clase), utilizado en el fragmento de código anterior, se encuentra en la `Microsoft.AspNetCore.Http.Authentication` espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="130bd-222">The `AuthenticationProperties` class, used in the preceding code snippet, resides in the `Microsoft.AspNetCore.Http.Authentication` namespace.</span></span>

---

<span data-ttu-id="130bd-223">El fragmento de código anterior crea una identidad y cookie correspondiente que sobrevive a través de los cierres de explorador.</span><span class="sxs-lookup"><span data-stu-id="130bd-223">The preceding code snippet creates an identity and corresponding cookie which survives through browser closures.</span></span> <span data-ttu-id="130bd-224">Cualquier configuración de expiración deslizante configurada anteriormente mediante [opciones de cookie](xref:security/authentication/cookie#security-authentication-cookie-options) se aplican aún.</span><span class="sxs-lookup"><span data-stu-id="130bd-224">Any sliding expiration settings previously configured via [cookie options](xref:security/authentication/cookie#security-authentication-cookie-options) are still honored.</span></span> <span data-ttu-id="130bd-225">Si la cookie expira mientras se cierra el explorador, el explorador borra una vez que se reinicie.</span><span class="sxs-lookup"><span data-stu-id="130bd-225">If the cookie expires whilst the browser is closed, the browser clears it once it is restarted.</span></span>

<a name="security-authentication-absolute-expiry"></a>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="130bd-226">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="130bd-226">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    "MyCookieAuthenticationScheme",
    principal,
    new AuthenticationProperties
    {
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="130bd-227">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="130bd-227">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync(
    "MyCookieAuthenticationScheme",
    principal,
    new AuthenticationProperties
    {
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

---

<span data-ttu-id="130bd-228">El fragmento de código anterior crea una identidad y cookie correspondiente que tiene una validez de 20 minutos.</span><span class="sxs-lookup"><span data-stu-id="130bd-228">The preceding code snippet creates an identity and corresponding cookie which lasts for 20 minutes.</span></span> <span data-ttu-id="130bd-229">Esto omite cualquier configuración de expiración deslizante configurada anteriormente mediante [opciones de cookie](xref:security/authentication/cookie#security-authentication-cookie-options).</span><span class="sxs-lookup"><span data-stu-id="130bd-229">This ignores any sliding expiration settings previously configured via [cookie options](xref:security/authentication/cookie#security-authentication-cookie-options).</span></span>

<span data-ttu-id="130bd-230">El `ExpiresUtc` y `IsPersistent` propiedades son mutuamente excluyentes.</span><span class="sxs-lookup"><span data-stu-id="130bd-230">The `ExpiresUtc` and `IsPersistent` properties are mutually exclusive.</span></span>
