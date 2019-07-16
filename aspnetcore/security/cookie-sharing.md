---
title: Compartir cookies de autenticación entre aplicaciones ASP.NET
author: rick-anderson
description: Obtenga información sobre cómo compartir cookies de autenticación entre ASP.NET 4.x y aplicaciones de ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/15/2019
uid: security/cookie-sharing
ms.openlocfilehash: b2f906ac97fe79b2a66a5ab709bcbcb03ab8cc39
ms.sourcegitcommit: 1bf80f4acd62151ff8cce517f03f6fa891136409
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/15/2019
ms.locfileid: "68223922"
---
# <a name="share-authentication-cookies-among-aspnet-apps"></a><span data-ttu-id="6e045-103">Compartir cookies de autenticación entre aplicaciones ASP.NET</span><span class="sxs-lookup"><span data-stu-id="6e045-103">Share authentication cookies among ASP.NET apps</span></span>

<span data-ttu-id="6e045-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="6e045-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="6e045-105">Los sitios Web a menudo constan de aplicaciones web individuales trabajan juntos.</span><span class="sxs-lookup"><span data-stu-id="6e045-105">Websites often consist of individual web apps working together.</span></span> <span data-ttu-id="6e045-106">Para proporcionar una experiencia de inicio de sesión único (SSO), aplicaciones web dentro de un sitio deben compartir las cookies de autenticación.</span><span class="sxs-lookup"><span data-stu-id="6e045-106">To provide a single sign-on (SSO) experience, web apps within a site must share authentication cookies.</span></span> <span data-ttu-id="6e045-107">Para admitir este escenario, la pila de protección de datos permite compartir la autenticación con cookies Katana y vales de autenticación de cookies de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6e045-107">To support this scenario, the data protection stack allows sharing Katana cookie authentication and ASP.NET Core cookie authentication tickets.</span></span>

<span data-ttu-id="6e045-108">En los ejemplos siguientes:</span><span class="sxs-lookup"><span data-stu-id="6e045-108">In the examples that follow:</span></span>

* <span data-ttu-id="6e045-109">El nombre de la cookie de autenticación se establece en un valor común de `.AspNet.SharedCookie`.</span><span class="sxs-lookup"><span data-stu-id="6e045-109">The authentication cookie name is set to a common value of `.AspNet.SharedCookie`.</span></span>
* <span data-ttu-id="6e045-110">El `AuthenticationType` está establecido en `Identity.Application` explícitamente o de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="6e045-110">The `AuthenticationType` is set to `Identity.Application` either explicitly or by default.</span></span>
* <span data-ttu-id="6e045-111">Un nombre de aplicación común se utiliza para habilitar el sistema de protección de datos compartir las claves de protección de datos (`SharedCookieApp`).</span><span class="sxs-lookup"><span data-stu-id="6e045-111">A common app name is used to enable the data protection system to share data protection keys (`SharedCookieApp`).</span></span>
* <span data-ttu-id="6e045-112">`Identity.Application` se utiliza como el esquema de autenticación.</span><span class="sxs-lookup"><span data-stu-id="6e045-112">`Identity.Application` is used as the authentication scheme.</span></span> <span data-ttu-id="6e045-113">Se usa cualquier esquema, se debe usar de forma coherente *dentro y entre* las aplicaciones de la cookie compartido como la combinación predeterminada o si se establece explícitamente.</span><span class="sxs-lookup"><span data-stu-id="6e045-113">Whatever scheme is used, it must be used consistently *within and across* the shared cookie apps either as the default scheme or by explicitly setting it.</span></span> <span data-ttu-id="6e045-114">El esquema se utiliza al cifrar y descifrar las cookies, por lo que se debe usar un esquema coherente entre aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="6e045-114">The scheme is used when encrypting and decrypting cookies, so a consistent scheme must be used across apps.</span></span>
* <span data-ttu-id="6e045-115">Un común [clave de protección de datos](xref:security/data-protection/implementation/key-management) se utiliza la ubicación de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="6e045-115">A common [data protection key](xref:security/data-protection/implementation/key-management) storage location is used.</span></span>
  * <span data-ttu-id="6e045-116">En las aplicaciones de ASP.NET Core, <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.PersistKeysToFileSystem*> se usa para establecer la ubicación de almacenamiento de claves.</span><span class="sxs-lookup"><span data-stu-id="6e045-116">In ASP.NET Core apps, <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.PersistKeysToFileSystem*> is used to set the key storage location.</span></span>
  * <span data-ttu-id="6e045-117">En las aplicaciones de .NET Framework, el Middleware de Cookie de autenticación usa una implementación de <xref:Microsoft.AspNetCore.DataProtection.DataProtectionProvider>.</span><span class="sxs-lookup"><span data-stu-id="6e045-117">In .NET Framework apps, Cookie Authentication Middleware uses an implementation of <xref:Microsoft.AspNetCore.DataProtection.DataProtectionProvider>.</span></span> <span data-ttu-id="6e045-118">`DataProtectionProvider` proporciona servicios de protección de datos para el cifrado y descifrado de datos de carga de cookie de autenticación.</span><span class="sxs-lookup"><span data-stu-id="6e045-118">`DataProtectionProvider` provides data protection services for the encryption and decryption of authentication cookie payload data.</span></span> <span data-ttu-id="6e045-119">El `DataProtectionProvider` instancia está aislada del sistema de protección de datos utilizado por otras partes de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="6e045-119">The `DataProtectionProvider` instance is isolated from the data protection system used by other parts of the app.</span></span> <span data-ttu-id="6e045-120">[DataProtectionProvider.Create (System.IO.DirectoryInfo, acción\<IDataProtectionBuilder >)](xref:Microsoft.AspNetCore.DataProtection.DataProtectionProvider.Create*) acepta un <xref:System.IO.DirectoryInfo> para especificar la ubicación de almacenamiento de claves de protección de datos.</span><span class="sxs-lookup"><span data-stu-id="6e045-120">[DataProtectionProvider.Create(System.IO.DirectoryInfo, Action\<IDataProtectionBuilder>)](xref:Microsoft.AspNetCore.DataProtection.DataProtectionProvider.Create*) accepts a <xref:System.IO.DirectoryInfo> to specify the location for data protection key storage.</span></span>
* <span data-ttu-id="6e045-121">`DataProtectionProvider` requiere el [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) paquete NuGet:</span><span class="sxs-lookup"><span data-stu-id="6e045-121">`DataProtectionProvider` requires the [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) NuGet package:</span></span>
  * <span data-ttu-id="6e045-122">En las aplicaciones de ASP.NET Core 2.x, hacer referencia a la [Microsoft.AspNetCore.App metapaquete](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="6e045-122">In ASP.NET Core 2.x apps, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
  * <span data-ttu-id="6e045-123">En las aplicaciones de .NET Framework, agregue una referencia de paquete a [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/).</span><span class="sxs-lookup"><span data-stu-id="6e045-123">In .NET Framework apps, add a package reference to [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/).</span></span>
* <span data-ttu-id="6e045-124"><xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*> establece el nombre de aplicación comunes.</span><span class="sxs-lookup"><span data-stu-id="6e045-124"><xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*> sets the common app name.</span></span>

## <a name="share-authentication-cookies-among-aspnet-core-apps"></a><span data-ttu-id="6e045-125">Compartir cookies de autenticación entre aplicaciones de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6e045-125">Share authentication cookies among ASP.NET Core apps</span></span>

<span data-ttu-id="6e045-126">Cuando se usa ASP.NET Core Identity:</span><span class="sxs-lookup"><span data-stu-id="6e045-126">When using ASP.NET Core Identity:</span></span>

* <span data-ttu-id="6e045-127">Las claves de protección de datos y el nombre de la aplicación deben compartirse entre aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="6e045-127">Data protection keys and the app name must be shared among apps.</span></span> <span data-ttu-id="6e045-128">Se proporciona una ubicación de almacenamiento de claves comunes para la <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.PersistKeysToFileSystem*> método en los ejemplos siguientes.</span><span class="sxs-lookup"><span data-stu-id="6e045-128">A common key storage location is provided to the <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.PersistKeysToFileSystem*> method in the following examples.</span></span> <span data-ttu-id="6e045-129">Use <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*> para configurar un nombre de aplicación compartida común (`SharedCookieApp` en los ejemplos siguientes).</span><span class="sxs-lookup"><span data-stu-id="6e045-129">Use <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*> to configure a common shared app name (`SharedCookieApp` in the following examples).</span></span> <span data-ttu-id="6e045-130">Para obtener más información, consulta <xref:security/data-protection/configuration/overview>.</span><span class="sxs-lookup"><span data-stu-id="6e045-130">For more information, see <xref:security/data-protection/configuration/overview>.</span></span>
* <span data-ttu-id="6e045-131">Use el <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.ConfigureApplicationCookie*> método de extensión para configurar el servicio de protección de datos para las cookies.</span><span class="sxs-lookup"><span data-stu-id="6e045-131">Use the <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.ConfigureApplicationCookie*> extension method to set up the data protection service for cookies.</span></span>
* <span data-ttu-id="6e045-132">En el ejemplo siguiente, el tipo de autenticación se establece en `Identity.Application` de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="6e045-132">In the following example, the authentication type is set to `Identity.Application` by default.</span></span>

<span data-ttu-id="6e045-133">En `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="6e045-133">In `Startup.ConfigureServices`:</span></span>

```csharp
services.AddDataProtection()
    .PersistKeysToFileSystem({PATH TO COMMON KEY RING FOLDER})
    .SetApplicationName("SharedCookieApp");

services.ConfigureApplicationCookie(options => {
    options.Cookie.Name = ".AspNet.SharedCookie";
});
```

<span data-ttu-id="6e045-134">Al usar cookies directamente sin ASP.NET Core Identity, configurar protección de datos y la autenticación en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="6e045-134">When using cookies directly without ASP.NET Core Identity, configure data protection and authentication in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="6e045-135">En el ejemplo siguiente, el tipo de autenticación se establece en `Identity.Application`:</span><span class="sxs-lookup"><span data-stu-id="6e045-135">In the following example, the authentication type is set to `Identity.Application`:</span></span>

```csharp
services.AddDataProtection()
    .PersistKeysToFileSystem({PATH TO COMMON KEY RING FOLDER})
    .SetApplicationName("SharedCookieApp");

services.AddAuthentication("Identity.Application")
    .AddCookie("Identity.Application", options =>
    {
        options.Cookie.Name = ".AspNet.SharedCookie";
    });
```

<span data-ttu-id="6e045-136">Al hospedar aplicaciones que comparten cookies entre subdominios, especifique un dominio común en el [Cookie.Domain](xref:Microsoft.AspNetCore.Http.CookieBuilder.Domain) propiedad.</span><span class="sxs-lookup"><span data-stu-id="6e045-136">When hosting apps that share cookies across subdomains, specify a common domain in the [Cookie.Domain](xref:Microsoft.AspNetCore.Http.CookieBuilder.Domain) property.</span></span> <span data-ttu-id="6e045-137">Compartir cookies entre aplicaciones en `contoso.com`, tales como `first_subdomain.contoso.com` y `second_subdomain.contoso.com`, especifique el `Cookie.Domain` como `.contoso.com`:</span><span class="sxs-lookup"><span data-stu-id="6e045-137">To share cookies across apps at `contoso.com`, such as `first_subdomain.contoso.com` and `second_subdomain.contoso.com`, specify the `Cookie.Domain` as `.contoso.com`:</span></span>

```csharp
options.Cookie.Domain = ".contoso.com";
```

## <a name="encrypt-data-protection-keys-at-rest"></a><span data-ttu-id="6e045-138">Cifrar las claves de protección de datos en reposo</span><span class="sxs-lookup"><span data-stu-id="6e045-138">Encrypt data protection keys at rest</span></span>

<span data-ttu-id="6e045-139">Para las implementaciones de producción, configure el `DataProtectionProvider` para cifrar las claves en reposo con DPAPI o un X509Certificate.</span><span class="sxs-lookup"><span data-stu-id="6e045-139">For production deployments, configure the `DataProtectionProvider` to encrypt keys at rest with DPAPI or an X509Certificate.</span></span> <span data-ttu-id="6e045-140">Para obtener más información, consulta <xref:security/data-protection/implementation/key-encryption-at-rest>.</span><span class="sxs-lookup"><span data-stu-id="6e045-140">For more information, see <xref:security/data-protection/implementation/key-encryption-at-rest>.</span></span> <span data-ttu-id="6e045-141">En el ejemplo siguiente, se proporciona una huella digital del certificado para <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.ProtectKeysWithCertificate*>:</span><span class="sxs-lookup"><span data-stu-id="6e045-141">In the following example, a certificate thumbprint is provided to <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.ProtectKeysWithCertificate*>:</span></span>

```csharp
services.AddDataProtection()
    .ProtectKeysWithCertificate("{CERTIFICATE THUMBPRINT}");
```

## <a name="share-authentication-cookies-between-aspnet-4x-and-aspnet-core-apps"></a><span data-ttu-id="6e045-142">Compartir cookies de autenticación entre ASP.NET 4.x y aplicaciones de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6e045-142">Share authentication cookies between ASP.NET 4.x and ASP.NET Core apps</span></span>

<span data-ttu-id="6e045-143">Las aplicaciones ASP.NET 4.x que usan el software intermedio de Katana Cookie de autenticación pueden configurarse para generar las cookies de autenticación que son compatibles con el Middleware de autenticación de cookies de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6e045-143">ASP.NET 4.x apps that use Katana Cookie Authentication Middleware can be configured to generate authentication cookies that are compatible with the ASP.NET Core Cookie Authentication Middleware.</span></span> <span data-ttu-id="6e045-144">Esto permite actualizar las aplicaciones individuales de un sitio gran tamaño en varios pasos al tiempo que proporciona una experiencia de inicio de sesión único fluida en todo el sitio.</span><span class="sxs-lookup"><span data-stu-id="6e045-144">This allows upgrading a large site's individual apps in several steps while providing a smooth SSO experience across the site.</span></span>

<span data-ttu-id="6e045-145">Cuando una aplicación usa el Middleware de autenticación de cookies de Katana, llama a `UseCookieAuthentication` en el proyecto *Startup.Auth.cs* archivo.</span><span class="sxs-lookup"><span data-stu-id="6e045-145">When an app uses Katana Cookie Authentication Middleware, it calls `UseCookieAuthentication` in the project's *Startup.Auth.cs* file.</span></span> <span data-ttu-id="6e045-146">Proyectos de aplicación web de ASP.NET 4.x crean con Visual Studio 2013 y usan más adelante el Middleware de autenticación de cookies de Katana de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="6e045-146">ASP.NET 4.x web app projects created with Visual Studio 2013 and later use the Katana Cookie Authentication Middleware by default.</span></span> <span data-ttu-id="6e045-147">Aunque `UseCookieAuthentication` está obsoleto y no compatibles para las aplicaciones de ASP.NET Core, una llamada a `UseCookieAuthentication` en ASP.NET 4.x app que usa el Middleware de autenticación de cookies de Katana es válido.</span><span class="sxs-lookup"><span data-stu-id="6e045-147">Although `UseCookieAuthentication` is obsolete and unsupported for ASP.NET Core apps, calling `UseCookieAuthentication` in an ASP.NET 4.x app that uses Katana Cookie Authentication Middleware is valid.</span></span>

<span data-ttu-id="6e045-148">Una aplicación ASP.NET 4.x debe tener como destino .NET Framework 4.5.1 o posterior.</span><span class="sxs-lookup"><span data-stu-id="6e045-148">An ASP.NET 4.x app must target .NET Framework 4.5.1 or later.</span></span> <span data-ttu-id="6e045-149">En caso contrario, no instale los paquetes de NuGet necesarios.</span><span class="sxs-lookup"><span data-stu-id="6e045-149">Otherwise, the necessary NuGet packages fail to install.</span></span>

<span data-ttu-id="6e045-150">Para compartir las cookies de autenticación entre una aplicación ASP.NET 4.x y una aplicación ASP.NET Core, configure la aplicación de ASP.NET Core, como se indica en la [compartir cookies de autenticación entre aplicaciones de ASP.NET Core](#share-authentication-cookies-among-aspnet-core-apps) sección y, después, configure la aplicación ASP.NET 4.x como se indica a continuación.</span><span class="sxs-lookup"><span data-stu-id="6e045-150">To share authentication cookies between an ASP.NET 4.x app and an ASP.NET Core app, configure the ASP.NET Core app as stated in the [Share authentication cookies among ASP.NET Core apps](#share-authentication-cookies-among-aspnet-core-apps) section, then configure the ASP.NET 4.x app as follows.</span></span>

<span data-ttu-id="6e045-151">Confirme que los paquetes de la aplicación se actualizan a las versiones más recientes.</span><span class="sxs-lookup"><span data-stu-id="6e045-151">Confirm that the app's packages are updated to the latest releases.</span></span> <span data-ttu-id="6e045-152">Instalar el [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) paquete en cada aplicación ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="6e045-152">Install the [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) package into each ASP.NET 4.x app.</span></span>

<span data-ttu-id="6e045-153">Busque y modifique la llamada a `UseCookieAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="6e045-153">Locate and modify the call to `UseCookieAuthentication`:</span></span>

* <span data-ttu-id="6e045-154">Cambiar el nombre de cookie para que coincida con el nombre utilizado por el Middleware de autenticación de cookies de ASP.NET Core (`.AspNet.SharedCookie` en el ejemplo).</span><span class="sxs-lookup"><span data-stu-id="6e045-154">Change the cookie name to match the name used by the ASP.NET Core Cookie Authentication Middleware (`.AspNet.SharedCookie` in the example).</span></span>
* <span data-ttu-id="6e045-155">En el ejemplo siguiente, el tipo de autenticación se establece en `Identity.Application`.</span><span class="sxs-lookup"><span data-stu-id="6e045-155">In the following example, the authentication type is set to `Identity.Application`.</span></span>
* <span data-ttu-id="6e045-156">Proporcionar una instancia de un `DataProtectionProvider` inicializado en la ubicación de almacenamiento de claves de protección de datos comunes.</span><span class="sxs-lookup"><span data-stu-id="6e045-156">Provide an instance of a `DataProtectionProvider` initialized to the common data protection key storage location.</span></span>
* <span data-ttu-id="6e045-157">Confirme que el nombre de la aplicación está configurado como el nombre de aplicación común usado por todas las aplicaciones que comparten cookies de autenticación (`SharedCookieApp` en el ejemplo).</span><span class="sxs-lookup"><span data-stu-id="6e045-157">Confirm that the app name is set to the common app name used by all apps that share authentication cookies (`SharedCookieApp` in the example).</span></span>

<span data-ttu-id="6e045-158">Si no establece `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier` y `http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider`, establezca <xref:System.Web.Helpers.AntiForgeryConfig.UniqueClaimTypeIdentifier> para una notificación que distingue a los usuarios únicos.</span><span class="sxs-lookup"><span data-stu-id="6e045-158">If not setting `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier` and `http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider`, set <xref:System.Web.Helpers.AntiForgeryConfig.UniqueClaimTypeIdentifier> to a claim that distinguishes unique users.</span></span>

<span data-ttu-id="6e045-159">*App_Start/Startup.auth.cs*:</span><span class="sxs-lookup"><span data-stu-id="6e045-159">*App_Start/Startup.Auth.cs*:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    AuthenticationType = "Identity.Application",
    CookieName = ".AspNet.SharedCookie",
    LoginPath = new PathString("/Account/Login"),
    Provider = new CookieAuthenticationProvider
    {
        OnValidateIdentity =
            SecurityStampValidator
                .OnValidateIdentity<ApplicationUserManager, ApplicationUser>(
                    validateInterval: TimeSpan.FromMinutes(30),
                    regenerateIdentity: (manager, user) =>
                        user.GenerateUserIdentityAsync(manager))
    },
    TicketDataFormat = new AspNetTicketDataFormat(
        new DataProtectorShim(
            DataProtectionProvider.Create({PATH TO COMMON KEY RING FOLDER},
                (builder) => { builder.SetApplicationName("SharedCookieApp"); })
            .CreateProtector(
                "Microsoft.AspNetCore.Authentication.Cookies." +
                    "CookieAuthenticationMiddleware",
                "Identity.Application",
                "v2"))),
    CookieManager = new ChunkingCookieManager()
});

System.Web.Helpers.AntiForgeryConfig.UniqueClaimTypeIdentifier =
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name";
```

<span data-ttu-id="6e045-160">Al generar una identidad de usuario, el tipo de autenticación (`Identity.Application`) debe coincidir con el tipo definido en `AuthenticationType` establecido con `UseCookieAuthentication` en *App_Start/Startup.Auth.cs*.</span><span class="sxs-lookup"><span data-stu-id="6e045-160">When generating a user identity, the authentication type (`Identity.Application`) must match the type defined in `AuthenticationType` set with `UseCookieAuthentication` in *App_Start/Startup.Auth.cs*.</span></span>

<span data-ttu-id="6e045-161">*Models/IdentityModels.cs*:</span><span class="sxs-lookup"><span data-stu-id="6e045-161">*Models/IdentityModels.cs*:</span></span>

```csharp
public class ApplicationUser : IdentityUser
{
    public async Task<ClaimsIdentity> GenerateUserIdentityAsync(
        UserManager<ApplicationUser> manager)
    {
        // The authenticationType must match the one defined in 
        // CookieAuthenticationOptions.AuthenticationType
        var userIdentity = 
            await manager.CreateIdentityAsync(this, "Identity.Application");

        // Add custom user claims here

        return userIdentity;
    }
}
```

## <a name="use-a-common-user-database"></a><span data-ttu-id="6e045-162">Usar una base de datos de usuario comunes</span><span class="sxs-lookup"><span data-stu-id="6e045-162">Use a common user database</span></span>

<span data-ttu-id="6e045-163">Cuando las aplicaciones usan la misma identidad de esquema (misma versión de identidad), confirme que el sistema de identidad para cada aplicación se señala a la misma base de datos de usuario.</span><span class="sxs-lookup"><span data-stu-id="6e045-163">When apps use the same Identity schema (same version of Identity), confirm that the Identity system for each app is pointed at the same user database.</span></span> <span data-ttu-id="6e045-164">En caso contrario, el sistema de identidades genera errores en tiempo de ejecución cuando intenta hacer coincidir la información de la cookie de autenticación con la información de su base de datos.</span><span class="sxs-lookup"><span data-stu-id="6e045-164">Otherwise, the identity system produces failures at runtime when it attempts to match the information in the authentication cookie against the information in its database.</span></span>

<span data-ttu-id="6e045-165">Cuando el esquema de la identidad es diferente entre aplicaciones, normalmente porque las aplicaciones usan diferentes versiones de identidad, uso compartido de una base de datos común según la versión más reciente de identidad no es posible sin tener que volver a asignar y agregar columnas en esquemas de identidad de la otra aplicación.</span><span class="sxs-lookup"><span data-stu-id="6e045-165">When the Identity schema is different among apps, usually because apps are using different Identity versions, sharing a common database based on the latest version of Identity isn't possible without remapping and adding columns in other app's Identity schemas.</span></span> <span data-ttu-id="6e045-166">A menudo resulta más eficaz actualizar las otras aplicaciones para usar la versión más reciente de identidad para que las aplicaciones puede compartir una base de datos comunes.</span><span class="sxs-lookup"><span data-stu-id="6e045-166">It's often more efficient to upgrade the other apps to use the latest Identity version so that a common database can be shared by the apps.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6e045-167">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="6e045-167">Additional resources</span></span>

* <xref:host-and-deploy/web-farm>
