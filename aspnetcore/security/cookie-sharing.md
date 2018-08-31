---
title: Compartir cookies entre aplicaciones con ASP.NET y ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo compartir cookies de autenticación entre ASP.NET 4.x y aplicaciones de ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 01/19/2017
uid: security/cookie-sharing
ms.openlocfilehash: afb0405ea87a6239c3017ba0a59a22527a817feb
ms.sourcegitcommit: ecf2cd4e0613569025b28e12de3baa21d86d4258
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/30/2018
ms.locfileid: "43312287"
---
# <a name="share-cookies-among-apps-with-aspnet-and-aspnet-core"></a><span data-ttu-id="ad96e-103">Compartir cookies entre aplicaciones con ASP.NET y ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ad96e-103">Share cookies among apps with ASP.NET and ASP.NET Core</span></span>

<span data-ttu-id="ad96e-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="ad96e-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="ad96e-105">Los sitios Web a menudo constan de aplicaciones web individuales trabajan juntos.</span><span class="sxs-lookup"><span data-stu-id="ad96e-105">Websites often consist of individual web apps working together.</span></span> <span data-ttu-id="ad96e-106">Para proporcionar una experiencia de inicio de sesión único (SSO), aplicaciones web dentro de un sitio deben compartir las cookies de autenticación.</span><span class="sxs-lookup"><span data-stu-id="ad96e-106">To provide a single sign-on (SSO) experience, web apps within a site must share authentication cookies.</span></span> <span data-ttu-id="ad96e-107">Para admitir este escenario, la pila de protección de datos permite compartir la autenticación con cookies Katana y vales de autenticación de cookies de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ad96e-107">To support this scenario, the data protection stack allows sharing Katana cookie authentication and ASP.NET Core cookie authentication tickets.</span></span>

<span data-ttu-id="ad96e-108">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ad96e-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="ad96e-109">En el ejemplo muestra el uso compartido entre tres aplicaciones que usan la autenticación con cookies de cookie:</span><span class="sxs-lookup"><span data-stu-id="ad96e-109">The sample illustrates cookie sharing across three apps that use cookie authentication:</span></span>

* <span data-ttu-id="ad96e-110">Aplicación ASP.NET Core 2.0 Razor Pages sin usar [ASP.NET Core Identity](xref:security/authentication/identity)</span><span class="sxs-lookup"><span data-stu-id="ad96e-110">ASP.NET Core 2.0 Razor Pages app without using [ASP.NET Core Identity](xref:security/authentication/identity)</span></span>
* <span data-ttu-id="ad96e-111">Aplicación MVC de ASP.NET Core 2.0 con ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="ad96e-111">ASP.NET Core 2.0 MVC app with ASP.NET Core Identity</span></span>
* <span data-ttu-id="ad96e-112">Aplicación MVC de ASP.NET Framework 4.6.1 con ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="ad96e-112">ASP.NET Framework 4.6.1 MVC app with ASP.NET Identity</span></span>

<span data-ttu-id="ad96e-113">En los ejemplos siguientes:</span><span class="sxs-lookup"><span data-stu-id="ad96e-113">In the examples that follow:</span></span>

* <span data-ttu-id="ad96e-114">El nombre de la cookie de autenticación se establece en un valor común de `.AspNet.SharedCookie`.</span><span class="sxs-lookup"><span data-stu-id="ad96e-114">The authentication cookie name is set to a common value of `.AspNet.SharedCookie`.</span></span>
* <span data-ttu-id="ad96e-115">El `AuthenticationType` está establecido en `Identity.Application` explícitamente o de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="ad96e-115">The `AuthenticationType` is set to `Identity.Application` either explicitly or by default.</span></span>
* <span data-ttu-id="ad96e-116">Un nombre de aplicación común se utiliza para habilitar el sistema de protección de datos compartir las claves de protección de datos (`SharedCookieApp`).</span><span class="sxs-lookup"><span data-stu-id="ad96e-116">A common app name is used to enable the data protection system to share data protection keys (`SharedCookieApp`).</span></span>
* <span data-ttu-id="ad96e-117">`Identity.Application` se utiliza como el esquema de autenticación.</span><span class="sxs-lookup"><span data-stu-id="ad96e-117">`Identity.Application` is used as the authentication scheme.</span></span> <span data-ttu-id="ad96e-118">Se usa cualquier esquema, se debe usar de forma coherente *dentro y entre* las aplicaciones de la cookie compartido como la combinación predeterminada o si se establece explícitamente.</span><span class="sxs-lookup"><span data-stu-id="ad96e-118">Whatever scheme is used, it must be used consistently *within and across* the shared cookie apps either as the default scheme or by explicitly setting it.</span></span> <span data-ttu-id="ad96e-119">El esquema se utiliza al cifrar y descifrar las cookies, por lo que se debe usar un esquema coherente entre aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="ad96e-119">The scheme is used when encrypting and decrypting cookies, so a consistent scheme must be used across apps.</span></span>
* <span data-ttu-id="ad96e-120">Un común [clave de protección de datos](xref:security/data-protection/implementation/key-management) se utiliza la ubicación de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="ad96e-120">A common [data protection key](xref:security/data-protection/implementation/key-management) storage location is used.</span></span> <span data-ttu-id="ad96e-121">La aplicación de ejemplo usa una carpeta denominada *KeyRing* en la raíz de la solución para almacenar las claves de protección de datos.</span><span class="sxs-lookup"><span data-stu-id="ad96e-121">The sample app uses a folder named *KeyRing* at the root of the solution to hold the data protection keys.</span></span>
* <span data-ttu-id="ad96e-122">En las aplicaciones ASP.NET Core, [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) se usa para establecer la ubicación de almacenamiento de claves.</span><span class="sxs-lookup"><span data-stu-id="ad96e-122">In the ASP.NET Core apps, [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) is used to set the key storage location.</span></span> <span data-ttu-id="ad96e-123">[SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) se usa para configurar un nombre de aplicación compartida común.</span><span class="sxs-lookup"><span data-stu-id="ad96e-123">[SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) is used to configure a common shared app name.</span></span>
* <span data-ttu-id="ad96e-124">En la aplicación de .NET Framework, el middleware de cookie de autenticación usa una implementación de [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider).</span><span class="sxs-lookup"><span data-stu-id="ad96e-124">In the .NET Framework app, the cookie authentication middleware uses an implementation of [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider).</span></span> <span data-ttu-id="ad96e-125">`DataProtectionProvider` proporciona servicios de protección de datos para el cifrado y descifrado de datos de carga de cookie de autenticación.</span><span class="sxs-lookup"><span data-stu-id="ad96e-125">`DataProtectionProvider` provides data protection services for the encryption and decryption of authentication cookie payload data.</span></span> <span data-ttu-id="ad96e-126">El `DataProtectionProvider` instancia está aislada del sistema de protección de datos utilizado por otras partes de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ad96e-126">The `DataProtectionProvider` instance is isolated from the data protection system used by other parts of the app.</span></span>
  * <span data-ttu-id="ad96e-127">[DataProtectionProvider.Create (System.IO.DirectoryInfo, acción\<IDataProtectionBuilder >)](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider.create?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionProvider_Create_System_IO_DirectoryInfo_System_Action_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder__) acepta un [DirectoryInfo](/dotnet/api/system.io.directoryinfo) para especificar la ubicación de almacenamiento de claves de protección de datos.</span><span class="sxs-lookup"><span data-stu-id="ad96e-127">[DataProtectionProvider.Create(System.IO.DirectoryInfo, Action\<IDataProtectionBuilder>)](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider.create?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionProvider_Create_System_IO_DirectoryInfo_System_Action_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder__) accepts a [DirectoryInfo](/dotnet/api/system.io.directoryinfo) to specify the location for data protection key storage.</span></span> <span data-ttu-id="ad96e-128">La aplicación de ejemplo proporciona la ruta de acceso de la *KeyRing* carpeta `DirectoryInfo`.</span><span class="sxs-lookup"><span data-stu-id="ad96e-128">The sample app provides the path of the *KeyRing* folder to `DirectoryInfo`.</span></span> <span data-ttu-id="ad96e-129">[DataProtectionBuilderExtensions.SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionBuilderExtensions_SetApplicationName_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_) establece el nombre de aplicación comunes.</span><span class="sxs-lookup"><span data-stu-id="ad96e-129">[DataProtectionBuilderExtensions.SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionBuilderExtensions_SetApplicationName_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_) sets the common app name.</span></span>
  * <span data-ttu-id="ad96e-130">[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) requiere la [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="ad96e-130">[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) requires the [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) NuGet package.</span></span> <span data-ttu-id="ad96e-131">Para obtener este paquete de aplicaciones más adelante y ASP.NET Core 2.1, hacer referencia a la [Microsoft.AspNetCore.App metapaquete](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="ad96e-131">To obtain this package for ASP.NET Core 2.1 and later apps, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="ad96e-132">Cuando el destino es .NET Framework, agregue una referencia de paquete a `Microsoft.AspNetCore.DataProtection.Extensions`.</span><span class="sxs-lookup"><span data-stu-id="ad96e-132">When targeting the .NET Framework, add a package reference to `Microsoft.AspNetCore.DataProtection.Extensions`.</span></span>

## <a name="share-authentication-cookies-among-aspnet-core-apps"></a><span data-ttu-id="ad96e-133">Compartir cookies de autenticación entre aplicaciones de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ad96e-133">Share authentication cookies among ASP.NET Core apps</span></span>

<span data-ttu-id="ad96e-134">Cuando se usa ASP.NET Core Identity:</span><span class="sxs-lookup"><span data-stu-id="ad96e-134">When using ASP.NET Core Identity:</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="ad96e-135">En el `ConfigureServices` método, use el [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie) método de extensión para configurar el servicio de protección de datos para las cookies.</span><span class="sxs-lookup"><span data-stu-id="ad96e-135">In the `ConfigureServices` method, use the [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie) extension method to set up the data protection service for cookies.</span></span>

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.Core/Startup.cs?name=snippet1)]

<span data-ttu-id="ad96e-136">Las claves de protección de datos y el nombre de la aplicación deben compartirse entre aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="ad96e-136">Data protection keys and the app name must be shared among apps.</span></span> <span data-ttu-id="ad96e-137">En las aplicaciones de ejemplo, `GetKeyRingDirInfo` devuelve la ubicación de almacenamiento de claves comunes para la [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) método.</span><span class="sxs-lookup"><span data-stu-id="ad96e-137">In the sample apps, `GetKeyRingDirInfo` returns the common key storage location to the [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) method.</span></span> <span data-ttu-id="ad96e-138">Use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) para configurar un nombre de aplicación compartida común (`SharedCookieApp` en el ejemplo).</span><span class="sxs-lookup"><span data-stu-id="ad96e-138">Use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) to configure a common shared app name (`SharedCookieApp` in the sample).</span></span> <span data-ttu-id="ad96e-139">Para obtener más información, consulte [configurar la protección de datos](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="ad96e-139">For more information, see [Configuring Data Protection](xref:security/data-protection/configuration/overview).</span></span>

<span data-ttu-id="ad96e-140">Al hospedar aplicaciones que comparten cookies entre subdominios, especifique un dominio común en el [Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain) propiedad.</span><span class="sxs-lookup"><span data-stu-id="ad96e-140">When hosting apps that share cookies across subdomains, specify a common domain in the [Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain) property.</span></span> <span data-ttu-id="ad96e-141">Compartir cookies entre aplicaciones en `contoso.com`, tales como `first_subdomain.contoso.com` y `second_subdomain.contoso.com`, especifique el `Cookie.Domain` como `.contoso.com`:</span><span class="sxs-lookup"><span data-stu-id="ad96e-141">To share cookies across apps at `contoso.com`, such as `first_subdomain.contoso.com` and `second_subdomain.contoso.com`, specify the `Cookie.Domain` as `.contoso.com`:</span></span>

```csharp
options.Cookie.Domain = ".contoso.com";
```

<span data-ttu-id="ad96e-142">Consulte la *CookieAuthWithIdentity.Core* del proyecto en el [código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([descarga](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="ad96e-142">See the *CookieAuthWithIdentity.Core* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="ad96e-143">En el `Configure` método, use el [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) para configurar:</span><span class="sxs-lookup"><span data-stu-id="ad96e-143">In the `Configure` method, use the [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) to set up:</span></span>

* <span data-ttu-id="ad96e-144">El servicio de protección de datos para las cookies.</span><span class="sxs-lookup"><span data-stu-id="ad96e-144">The data protection service for cookies.</span></span>
* <span data-ttu-id="ad96e-145">El `AuthenticationScheme` para que coincida con ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="ad96e-145">The `AuthenticationScheme` to match ASP.NET 4.x.</span></span>

```csharp
app.AddIdentity<ApplicationUser, IdentityRole>(options =>
{
    options.Cookies.ApplicationCookie.AuthenticationScheme = 
        "ApplicationCookie";

    var protectionProvider = 
        DataProtectionProvider.Create(
            new DirectoryInfo(@"PATH_TO_KEY_RING_FOLDER"));

    options.Cookies.ApplicationCookie.DataProtectionProvider = 
        protectionProvider;

    options.Cookies.ApplicationCookie.TicketDataFormat = 
        new TicketDataFormat(protectionProvider.CreateProtector(
            "Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationMiddleware", 
            "Cookies", 
            "v2"));
});
```

::: moniker-end

<span data-ttu-id="ad96e-146">Cuando se usa directamente las cookies:</span><span class="sxs-lookup"><span data-stu-id="ad96e-146">When using cookies directly:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](cookie-sharing/sample/CookieAuth.Core/Startup.cs?name=snippet1)]

<span data-ttu-id="ad96e-147">Las claves de protección de datos y el nombre de la aplicación deben compartirse entre aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="ad96e-147">Data protection keys and the app name must be shared among apps.</span></span> <span data-ttu-id="ad96e-148">En las aplicaciones de ejemplo, `GetKeyRingDirInfo` devuelve la ubicación de almacenamiento de claves comunes para la [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) método.</span><span class="sxs-lookup"><span data-stu-id="ad96e-148">In the sample apps, `GetKeyRingDirInfo` returns the common key storage location to the [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) method.</span></span> <span data-ttu-id="ad96e-149">Use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) para configurar un nombre de aplicación compartida común (`SharedCookieApp` en el ejemplo).</span><span class="sxs-lookup"><span data-stu-id="ad96e-149">Use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) to configure a common shared app name (`SharedCookieApp` in the sample).</span></span> <span data-ttu-id="ad96e-150">Para obtener más información, consulte [configurar la protección de datos](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="ad96e-150">For more information, see [Configuring Data Protection](xref:security/data-protection/configuration/overview).</span></span>

<span data-ttu-id="ad96e-151">Al hospedar aplicaciones que comparten cookies entre subdominios, especifique un dominio común en el [Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain) propiedad.</span><span class="sxs-lookup"><span data-stu-id="ad96e-151">When hosting apps that share cookies across subdomains, specify a common domain in the [Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain) property.</span></span> <span data-ttu-id="ad96e-152">Compartir cookies entre aplicaciones en `contoso.com`, tales como `first_subdomain.contoso.com` y `second_subdomain.contoso.com`, especifique el `Cookie.Domain` como `.contoso.com`:</span><span class="sxs-lookup"><span data-stu-id="ad96e-152">To share cookies across apps at `contoso.com`, such as `first_subdomain.contoso.com` and `second_subdomain.contoso.com`, specify the `Cookie.Domain` as `.contoso.com`:</span></span>

```csharp
options.Cookie.Domain = ".contoso.com";
```

<span data-ttu-id="ad96e-153">Consulte la *CookieAuth.Core* del proyecto en el [código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([descarga](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="ad96e-153">See the *CookieAuth.Core* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"


```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    DataProtectionProvider = 
        DataProtectionProvider.Create(
            new DirectoryInfo(@"PATH_TO_KEY_RING_FOLDER"))
});
```

::: moniker-end

## <a name="encrypting-data-protection-keys-at-rest"></a><span data-ttu-id="ad96e-154">Cifrar las claves de protección de datos en reposo</span><span class="sxs-lookup"><span data-stu-id="ad96e-154">Encrypting data protection keys at rest</span></span>

<span data-ttu-id="ad96e-155">Para las implementaciones de producción, configure el `DataProtectionProvider` para cifrar las claves en reposo con DPAPI o un X509Certificate.</span><span class="sxs-lookup"><span data-stu-id="ad96e-155">For production deployments, configure the `DataProtectionProvider` to encrypt keys at rest with DPAPI or an X509Certificate.</span></span> <span data-ttu-id="ad96e-156">Consulte [clave de cifrado en reposo](xref:security/data-protection/implementation/key-encryption-at-rest) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="ad96e-156">See [Key Encryption At Rest](xref:security/data-protection/implementation/key-encryption-at-rest) for more information.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
services.AddDataProtection()
    .ProtectKeysWithCertificate("thumbprint");
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    DataProtectionProvider = DataProtectionProvider.Create(
        new DirectoryInfo(@"PATH_TO_KEY_RING"),
        configure =>
        {
            configure.ProtectKeysWithCertificate("thumbprint");
        })
});
```

::: moniker-end

## <a name="sharing-authentication-cookies-between-aspnet-4x-and-aspnet-core-apps"></a><span data-ttu-id="ad96e-157">Uso compartido de cookies de autenticación entre ASP.NET 4.x y aplicaciones de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ad96e-157">Sharing authentication cookies between ASP.NET 4.x and ASP.NET Core apps</span></span>

<span data-ttu-id="ad96e-158">Las aplicaciones ASP.NET 4.x que usar el middleware de autenticación de cookies de Katana pueden configurarse para generar las cookies de autenticación que son compatibles con el middleware de autenticación de cookies de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ad96e-158">ASP.NET 4.x apps which use Katana cookie authentication middleware can be configured to generate authentication cookies that are compatible with the ASP.NET Core cookie authentication middleware.</span></span> <span data-ttu-id="ad96e-159">Esto permite actualizar aplicaciones individuales de un sitio grande por etapas al tiempo que proporciona una experiencia de inicio de sesión único fluida en todo el sitio.</span><span class="sxs-lookup"><span data-stu-id="ad96e-159">This allows upgrading a large site's individual apps piecemeal while providing a smooth SSO experience across the site.</span></span>

<span data-ttu-id="ad96e-160">Cuando una aplicación usa el middleware de autenticación de cookies de Katana, llama a `UseCookieAuthentication` en el proyecto *Startup.Auth.cs* archivo.</span><span class="sxs-lookup"><span data-stu-id="ad96e-160">When an app uses Katana cookie authentication middleware, it calls `UseCookieAuthentication` in the project's *Startup.Auth.cs* file.</span></span> <span data-ttu-id="ad96e-161">Proyectos de aplicación web de ASP.NET 4.x crean con Visual Studio 2013 y usan más adelante el middleware de autenticación de cookies de Katana de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="ad96e-161">ASP.NET 4.x web app projects created with Visual Studio 2013 and later use the Katana cookie authentication middleware by default.</span></span> <span data-ttu-id="ad96e-162">Aunque `UseCookieAuthentication` está obsoleto y no compatibles para las aplicaciones de ASP.NET Core, una llamada a `UseCookieAuthentication` en una aplicación ASP.NET 4.x que usa Katana middleware de autenticación de la cookie es válida.</span><span class="sxs-lookup"><span data-stu-id="ad96e-162">Although `UseCookieAuthentication` is obsolete and unsupported for ASP.NET Core apps, calling `UseCookieAuthentication` in an ASP.NET 4.x app that uses Katana cookie authentication middleware is valid.</span></span>

<span data-ttu-id="ad96e-163">Una aplicación ASP.NET 4.x debe tener como destino .NET Framework 4.5.1 o posterior.</span><span class="sxs-lookup"><span data-stu-id="ad96e-163">An ASP.NET 4.x app must target .NET Framework 4.5.1 or higher.</span></span> <span data-ttu-id="ad96e-164">En caso contrario, no instale los paquetes de NuGet necesarios.</span><span class="sxs-lookup"><span data-stu-id="ad96e-164">Otherwise, the necessary NuGet packages fail to install.</span></span>

<span data-ttu-id="ad96e-165">Para compartir las cookies de autenticación entre una aplicación ASP.NET 4.x y una aplicación ASP.NET Core, configurar la aplicación de ASP.NET Core, como se indicó anteriormente, a continuación, configure la aplicación ASP.NET 4.x siguiendo estos pasos:</span><span class="sxs-lookup"><span data-stu-id="ad96e-165">To share authentication cookies between an ASP.NET 4.x app and an ASP.NET Core app, configure the ASP.NET Core app as stated above, then configure the ASP.NET 4.x app by following these steps:</span></span>

1. <span data-ttu-id="ad96e-166">Instale el paquete [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) en cada aplicación ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="ad96e-166">Install the package [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) into each ASP.NET 4.x app.</span></span>

2. <span data-ttu-id="ad96e-167">En *Startup.Auth.cs*, busque la llamada a `UseCookieAuthentication` y modifíquela como se indica a continuación.</span><span class="sxs-lookup"><span data-stu-id="ad96e-167">In *Startup.Auth.cs*, locate the call to `UseCookieAuthentication` and modify it as follows.</span></span> <span data-ttu-id="ad96e-168">Cambie el nombre de cookie para que coincida con el nombre utilizado por el middleware de autenticación de cookies de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ad96e-168">Change the cookie name to match the name used by the ASP.NET Core cookie authentication middleware.</span></span> <span data-ttu-id="ad96e-169">Proporcionar una instancia de un `DataProtectionProvider` inicializado en la ubicación de almacenamiento de claves de protección de datos comunes.</span><span class="sxs-lookup"><span data-stu-id="ad96e-169">Provide an instance of a `DataProtectionProvider` initialized to the common data protection key storage location.</span></span> <span data-ttu-id="ad96e-170">Asegúrese de que el nombre de la aplicación se establece en el nombre de aplicación común usado por todas las aplicaciones que comparten cookies, `SharedCookieApp` en la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="ad96e-170">Make sure that the app name is set to the common app name used by all apps that share cookies, `SharedCookieApp` in the sample app.</span></span>

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/App_Start/Startup.Auth.cs?name=snippet1)]

<span data-ttu-id="ad96e-171">Consulte la *CookieAuthWithIdentity.NETFramework* del proyecto en el [código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([descarga](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="ad96e-171">See the *CookieAuthWithIdentity.NETFramework* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="ad96e-172">Al generar una identidad de usuario, el tipo de autenticación debe coincidir con el tipo definido en `AuthenticationType` establecido con `UseCookieAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="ad96e-172">When generating a user identity, the authentication type must match the type defined in `AuthenticationType` set with `UseCookieAuthentication`.</span></span>

<span data-ttu-id="ad96e-173">*Models/IdentityModels.cs*:</span><span class="sxs-lookup"><span data-stu-id="ad96e-173">*Models/IdentityModels.cs*:</span></span>

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/Models/IdentityModels.cs?name=snippet1)]

## <a name="use-a-common-user-database"></a><span data-ttu-id="ad96e-174">Usar una base de datos de usuario comunes</span><span class="sxs-lookup"><span data-stu-id="ad96e-174">Use a common user database</span></span>

<span data-ttu-id="ad96e-175">Confirme que el sistema de identidad para cada aplicación se señala a la misma base de datos de usuario.</span><span class="sxs-lookup"><span data-stu-id="ad96e-175">Confirm that the identity system for each app is pointed at the same user database.</span></span> <span data-ttu-id="ad96e-176">En caso contrario, el sistema de identidades genera errores en tiempo de ejecución cuando intenta hacer coincidir la información de la cookie de autenticación con la información de su base de datos.</span><span class="sxs-lookup"><span data-stu-id="ad96e-176">Otherwise, the identity system produces failures at runtime when it attempts to match the information in the authentication cookie against the information in its database.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ad96e-177">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="ad96e-177">Additional resources</span></span>

<xref:host-and-deploy/web-farm>
