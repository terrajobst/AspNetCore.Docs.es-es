---
title: Compartir cookies entre aplicaciones con ASP.NET y ASP.NET Core
author: rick-anderson
description: Obtenga información acerca de cómo compartir las cookies de autenticación entre ASP.NET 4.x y las aplicaciones de ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 01/19/2017
uid: security/cookie-sharing
ms.openlocfilehash: 2636688fa50fb36a8cbd07549e8038474ffa30ca
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278471"
---
# <a name="share-cookies-among-apps-with-aspnet-and-aspnet-core"></a><span data-ttu-id="2e5f8-103">Compartir cookies entre aplicaciones con ASP.NET y ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2e5f8-103">Share cookies among apps with ASP.NET and ASP.NET Core</span></span>

<span data-ttu-id="2e5f8-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="2e5f8-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="2e5f8-105">Sitios Web a menudo constan de las aplicaciones web individuales que trabajan juntos.</span><span class="sxs-lookup"><span data-stu-id="2e5f8-105">Websites often consist of individual web apps working together.</span></span> <span data-ttu-id="2e5f8-106">Para proporcionar una experiencia de inicio de sesión único (SSO), las aplicaciones web dentro de un sitio deben compartir las cookies de autenticación.</span><span class="sxs-lookup"><span data-stu-id="2e5f8-106">To provide a single sign-on (SSO) experience, web apps within a site must share authentication cookies.</span></span> <span data-ttu-id="2e5f8-107">Para admitir este escenario, la pila de protección de datos permite compartir la autenticación con cookies Katana y vales de autenticación de ASP.NET Core cookie.</span><span class="sxs-lookup"><span data-stu-id="2e5f8-107">To support this scenario, the data protection stack allows sharing Katana cookie authentication and ASP.NET Core cookie authentication tickets.</span></span>

<span data-ttu-id="2e5f8-108">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2e5f8-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="2e5f8-109">El ejemplo ilustra la cookie compartir entre tres aplicaciones que usan autenticación con cookies:</span><span class="sxs-lookup"><span data-stu-id="2e5f8-109">The sample illustrates cookie sharing across three apps that use cookie authentication:</span></span>

* <span data-ttu-id="2e5f8-110">Aplicación de las páginas de Razor de núcleo 2.0 de ASP.NET sin usar [ASP.NET Core Identity](xref:security/authentication/identity)</span><span class="sxs-lookup"><span data-stu-id="2e5f8-110">ASP.NET Core 2.0 Razor Pages app without using [ASP.NET Core Identity](xref:security/authentication/identity)</span></span>
* <span data-ttu-id="2e5f8-111">Aplicación MVC de ASP.NET Core 2.0 con la identidad de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2e5f8-111">ASP.NET Core 2.0 MVC app with ASP.NET Core Identity</span></span>
* <span data-ttu-id="2e5f8-112">Aplicación MVC de ASP.NET Framework 4.6.1 con la identidad de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="2e5f8-112">ASP.NET Framework 4.6.1 MVC app with ASP.NET Identity</span></span>

<span data-ttu-id="2e5f8-113">En los ejemplos siguientes:</span><span class="sxs-lookup"><span data-stu-id="2e5f8-113">In the examples that follow:</span></span>

* <span data-ttu-id="2e5f8-114">El nombre de la cookie de autenticación se establece en un valor común de `.AspNet.SharedCookie`.</span><span class="sxs-lookup"><span data-stu-id="2e5f8-114">The authentication cookie name is set to a common value of `.AspNet.SharedCookie`.</span></span>
* <span data-ttu-id="2e5f8-115">El `AuthenticationType` está establecido en `Identity.Application` explícitamente o de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="2e5f8-115">The `AuthenticationType` is set to `Identity.Application` either explicitly or by default.</span></span>
* <span data-ttu-id="2e5f8-116">Un nombre de aplicación común se utiliza para habilitar el sistema de protección de datos compartir las claves de protección de datos (`SharedCookieApp`).</span><span class="sxs-lookup"><span data-stu-id="2e5f8-116">A common app name is used to enable the data protection system to share data protection keys (`SharedCookieApp`).</span></span>
* <span data-ttu-id="2e5f8-117">`Identity.Application` se utiliza como el esquema de autenticación.</span><span class="sxs-lookup"><span data-stu-id="2e5f8-117">`Identity.Application` is used as the authentication scheme.</span></span> <span data-ttu-id="2e5f8-118">Se utiliza el esquema, se debe usar de forma coherente *dentro y entre* las aplicaciones de cookie compartido como el esquema predeterminado o si se establece explícitamente.</span><span class="sxs-lookup"><span data-stu-id="2e5f8-118">Whatever scheme is used, it must be used consistently *within and across* the shared cookie apps either as the default scheme or by explicitly setting it.</span></span> <span data-ttu-id="2e5f8-119">El esquema se utiliza al cifrar y descifrar las cookies, por lo que se debe usar un esquema coherente entre aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="2e5f8-119">The scheme is used when encrypting and decrypting cookies, so a consistent scheme must be used across apps.</span></span>
* <span data-ttu-id="2e5f8-120">Común [clave de protección de datos](xref:security/data-protection/implementation/key-management) se utiliza la ubicación de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="2e5f8-120">A common [data protection key](xref:security/data-protection/implementation/key-management) storage location is used.</span></span> <span data-ttu-id="2e5f8-121">La aplicación de ejemplo utiliza una carpeta denominada *KeyRing* en la raíz de la solución para almacenar las claves de protección de datos.</span><span class="sxs-lookup"><span data-stu-id="2e5f8-121">The sample app uses a folder named *KeyRing* at the root of the solution to hold the data protection keys.</span></span>
* <span data-ttu-id="2e5f8-122">En las aplicaciones ASP.NET Core, [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) se usa para establecer la ubicación de almacenamiento de claves.</span><span class="sxs-lookup"><span data-stu-id="2e5f8-122">In the ASP.NET Core apps, [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) is used to set the key storage location.</span></span> <span data-ttu-id="2e5f8-123">[SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) se usa para configurar un nombre de aplicación compartido común.</span><span class="sxs-lookup"><span data-stu-id="2e5f8-123">[SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) is used to configure a common shared app name.</span></span>
* <span data-ttu-id="2e5f8-124">En la aplicación de .NET Framework, el middleware de autenticación de cookie usa una implementación de [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider).</span><span class="sxs-lookup"><span data-stu-id="2e5f8-124">In the .NET Framework app, the cookie authentication middleware uses an implementation of [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider).</span></span> <span data-ttu-id="2e5f8-125">`DataProtectionProvider` proporciona servicios de protección de datos para el cifrado y descifrado de datos de carga de cookie de autenticación.</span><span class="sxs-lookup"><span data-stu-id="2e5f8-125">`DataProtectionProvider` provides data protection services for the encryption and decryption of authentication cookie payload data.</span></span> <span data-ttu-id="2e5f8-126">El `DataProtectionProvider` instancia está aislada del sistema de protección de datos utilizado por otras partes de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2e5f8-126">The `DataProtectionProvider` instance is isolated from the data protection system used by other parts of the app.</span></span>
  * <span data-ttu-id="2e5f8-127">[DataProtectionProvider.Create (System.IO.DirectoryInfo, acción\<IDataProtectionBuilder >)](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider.create?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionProvider_Create_System_IO_DirectoryInfo_System_Action_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder__) acepta un [DirectoryInfo](/dotnet/api/system.io.directoryinfo) para especificar la ubicación de almacenamiento de claves de protección de datos.</span><span class="sxs-lookup"><span data-stu-id="2e5f8-127">[DataProtectionProvider.Create(System.IO.DirectoryInfo, Action\<IDataProtectionBuilder>)](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider.create?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionProvider_Create_System_IO_DirectoryInfo_System_Action_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder__) accepts a [DirectoryInfo](/dotnet/api/system.io.directoryinfo) to specify the location for data protection key storage.</span></span> <span data-ttu-id="2e5f8-128">La aplicación de ejemplo proporciona la ruta de acceso de la *KeyRing* carpeta `DirectoryInfo`.</span><span class="sxs-lookup"><span data-stu-id="2e5f8-128">The sample app provides the path of the *KeyRing* folder to `DirectoryInfo`.</span></span> <span data-ttu-id="2e5f8-129">[DataProtectionBuilderExtensions.SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionBuilderExtensions_SetApplicationName_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_) establece el nombre de aplicación común.</span><span class="sxs-lookup"><span data-stu-id="2e5f8-129">[DataProtectionBuilderExtensions.SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionBuilderExtensions_SetApplicationName_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_) sets the common app name.</span></span>
  * <span data-ttu-id="2e5f8-130">[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) requiere el [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="2e5f8-130">[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) requires the [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) NuGet package.</span></span> <span data-ttu-id="2e5f8-131">Para obtener este paquete de aplicaciones más adelante y ASP.NET Core 2.1, hacen referencia a la [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="2e5f8-131">To obtain this package for ASP.NET Core 2.1 and later apps, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="2e5f8-132">Cuando el destino es .NET Framework, agregue una referencia de paquete a `Microsoft.AspNetCore.DataProtection.Extensions`.</span><span class="sxs-lookup"><span data-stu-id="2e5f8-132">When targeting the .NET Framework, add a package reference to `Microsoft.AspNetCore.DataProtection.Extensions`.</span></span>

## <a name="share-authentication-cookies-among-aspnet-core-apps"></a><span data-ttu-id="2e5f8-133">Compartir las cookies de autenticación entre aplicaciones de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2e5f8-133">Share authentication cookies among ASP.NET Core apps</span></span>

<span data-ttu-id="2e5f8-134">Al utilizar la identidad de núcleo de ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="2e5f8-134">When using ASP.NET Core Identity:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2e5f8-135">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2e5f8-135">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="2e5f8-136">En el `ConfigureServices` método, use la [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie) método de extensión para configurar el servicio de protección de datos para las cookies.</span><span class="sxs-lookup"><span data-stu-id="2e5f8-136">In the `ConfigureServices` method, use the [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie) extension method to set up the data protection service for cookies.</span></span>

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.Core/Startup.cs?name=snippet1)]

<span data-ttu-id="2e5f8-137">Las claves de protección de datos y el nombre de la aplicación deben compartirse entre aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="2e5f8-137">Data protection keys and the app name must be shared among apps.</span></span> <span data-ttu-id="2e5f8-138">En las aplicaciones de ejemplo, `GetKeyRingDirInfo` devuelve la ubicación de almacenamiento de claves comunes para la [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) método.</span><span class="sxs-lookup"><span data-stu-id="2e5f8-138">In the sample apps, `GetKeyRingDirInfo` returns the common key storage location to the [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) method.</span></span> <span data-ttu-id="2e5f8-139">Use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) para configurar un nombre de aplicación compartido común (`SharedCookieApp` en el ejemplo).</span><span class="sxs-lookup"><span data-stu-id="2e5f8-139">Use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) to configure a common shared app name (`SharedCookieApp` in the sample).</span></span> <span data-ttu-id="2e5f8-140">Para obtener más información, consulte [configurar la protección de datos](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="2e5f8-140">For more information, see [Configuring Data Protection](xref:security/data-protection/configuration/overview).</span></span>

<span data-ttu-id="2e5f8-141">Consulte la *CookieAuthWithIdentity.Core* del proyecto en el [código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([cómo descargar](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="2e5f8-141">See the *CookieAuthWithIdentity.Core* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2e5f8-142">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2e5f8-142">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="2e5f8-143">En el `Configure` método, use la [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) para configurar:</span><span class="sxs-lookup"><span data-stu-id="2e5f8-143">In the `Configure` method, use the [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) to set up:</span></span>

* <span data-ttu-id="2e5f8-144">El servicio de protección de datos para las cookies.</span><span class="sxs-lookup"><span data-stu-id="2e5f8-144">The data protection service for cookies.</span></span>
* <span data-ttu-id="2e5f8-145">El `AuthenticationScheme` para que coincida con ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="2e5f8-145">The `AuthenticationScheme` to match ASP.NET 4.x.</span></span>

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

---

<span data-ttu-id="2e5f8-146">Al utilizar cookies directamente:</span><span class="sxs-lookup"><span data-stu-id="2e5f8-146">When using cookies directly:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2e5f8-147">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2e5f8-147">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](cookie-sharing/sample/CookieAuth.Core/Startup.cs?name=snippet1)]

<span data-ttu-id="2e5f8-148">Las claves de protección de datos y el nombre de la aplicación deben compartirse entre aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="2e5f8-148">Data protection keys and the app name must be shared among apps.</span></span> <span data-ttu-id="2e5f8-149">En las aplicaciones de ejemplo, `GetKeyRingDirInfo` devuelve la ubicación de almacenamiento de claves comunes para la [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) método.</span><span class="sxs-lookup"><span data-stu-id="2e5f8-149">In the sample apps, `GetKeyRingDirInfo` returns the common key storage location to the [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) method.</span></span> <span data-ttu-id="2e5f8-150">Use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) para configurar un nombre de aplicación compartido común (`SharedCookieApp` en el ejemplo).</span><span class="sxs-lookup"><span data-stu-id="2e5f8-150">Use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) to configure a common shared app name (`SharedCookieApp` in the sample).</span></span> <span data-ttu-id="2e5f8-151">Para obtener más información, consulte [configurar la protección de datos](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="2e5f8-151">For more information, see [Configuring Data Protection](xref:security/data-protection/configuration/overview).</span></span> 

<span data-ttu-id="2e5f8-152">Consulte la *CookieAuth.Core* del proyecto en el [código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([cómo descargar](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="2e5f8-152">See the *CookieAuth.Core* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2e5f8-153">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2e5f8-153">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    DataProtectionProvider = 
        DataProtectionProvider.Create(
            new DirectoryInfo(@"PATH_TO_KEY_RING_FOLDER"))
});
```

---

## <a name="encrypting-data-protection-keys-at-rest"></a><span data-ttu-id="2e5f8-154">Cifrado de claves de protección de datos en reposo</span><span class="sxs-lookup"><span data-stu-id="2e5f8-154">Encrypting data protection keys at rest</span></span>

<span data-ttu-id="2e5f8-155">Para las implementaciones de producción, configurar el `DataProtectionProvider` para cifrar las claves en reposo con DPAPI o un X509Certificate.</span><span class="sxs-lookup"><span data-stu-id="2e5f8-155">For production deployments, configure the `DataProtectionProvider` to encrypt keys at rest with DPAPI or an X509Certificate.</span></span> <span data-ttu-id="2e5f8-156">Vea [clave de cifrado de datos almacenados](xref:security/data-protection/implementation/key-encryption-at-rest) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="2e5f8-156">See [Key Encryption At Rest](xref:security/data-protection/implementation/key-encryption-at-rest) for more information.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2e5f8-157">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2e5f8-157">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .ProtectKeysWithCertificate("thumbprint");
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2e5f8-158">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2e5f8-158">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

---

## <a name="sharing-authentication-cookies-between-aspnet-4x-and-aspnet-core-apps"></a><span data-ttu-id="2e5f8-159">Uso compartido de las cookies de autenticación entre ASP.NET 4.x y las aplicaciones de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2e5f8-159">Sharing authentication cookies between ASP.NET 4.x and ASP.NET Core apps</span></span>

<span data-ttu-id="2e5f8-160">Las aplicaciones ASP.NET 4.x que usar el middleware de autenticación de cookie de Katana pueden configurarse para generar las cookies de autenticación que son compatibles con el middleware de autenticación de la cookie de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2e5f8-160">ASP.NET 4.x apps which use Katana cookie authentication middleware can be configured to generate authentication cookies that are compatible with the ASP.NET Core cookie authentication middleware.</span></span> <span data-ttu-id="2e5f8-161">Esto permite actualizar aplicaciones individuales de un sitio grande por etapas al proporcionar una experiencia SSO sin problemas en todo el sitio.</span><span class="sxs-lookup"><span data-stu-id="2e5f8-161">This allows upgrading a large site's individual apps piecemeal while providing a smooth SSO experience across the site.</span></span>

<span data-ttu-id="2e5f8-162">Cuando una aplicación usa middleware de autenticación de cookie de Katana, llama al método `UseCookieAuthentication` en el proyecto *Startup.Auth.cs* archivo.</span><span class="sxs-lookup"><span data-stu-id="2e5f8-162">When an app uses Katana cookie authentication middleware, it calls `UseCookieAuthentication` in the project's *Startup.Auth.cs* file.</span></span> <span data-ttu-id="2e5f8-163">Proyectos de aplicación web de ASP.NET 4.x crean con Visual Studio 2013 y utilizan el middleware de autenticación de la cookie de Katana de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="2e5f8-163">ASP.NET 4.x web app projects created with Visual Studio 2013 and later use the Katana cookie authentication middleware by default.</span></span> <span data-ttu-id="2e5f8-164">Aunque `UseCookieAuthentication` está obsoleto y no compatibles para las aplicaciones de ASP.NET Core, una llamada a `UseCookieAuthentication` en una aplicación ASP.NET 4.x que usa Katana middleware de autenticación de la cookie es válido.</span><span class="sxs-lookup"><span data-stu-id="2e5f8-164">Although `UseCookieAuthentication` is obsolete and unsupported for ASP.NET Core apps, calling `UseCookieAuthentication` in an ASP.NET 4.x app that uses Katana cookie authentication middleware is valid.</span></span>

<span data-ttu-id="2e5f8-165">Una aplicación ASP.NET 4.x debe tener como destino .NET Framework 4.5.1 o posterior.</span><span class="sxs-lookup"><span data-stu-id="2e5f8-165">An ASP.NET 4.x app must target .NET Framework 4.5.1 or higher.</span></span> <span data-ttu-id="2e5f8-166">En caso contrario, los paquetes de NuGet necesarios no puede instalar.</span><span class="sxs-lookup"><span data-stu-id="2e5f8-166">Otherwise, the necessary NuGet packages fail to install.</span></span>

<span data-ttu-id="2e5f8-167">Para compartir las cookies de autenticación entre una aplicación ASP.NET 4.x y una aplicación de ASP.NET Core, configure la aplicación de ASP.NET Core tal y como se ha indicado anteriormente, después de configurar la aplicación ASP.NET 4.x siguiendo estos pasos:</span><span class="sxs-lookup"><span data-stu-id="2e5f8-167">To share authentication cookies between an ASP.NET 4.x app and an ASP.NET Core app, configure the ASP.NET Core app as stated above, then configure the ASP.NET 4.x app by following these steps:</span></span>

1. <span data-ttu-id="2e5f8-168">Instalar el paquete [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) en cada aplicación ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="2e5f8-168">Install the package [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) into each ASP.NET 4.x app.</span></span>

2. <span data-ttu-id="2e5f8-169">En *Startup.Auth.cs*, busque la llamada a `UseCookieAuthentication` y modifíquelo como se indica a continuación.</span><span class="sxs-lookup"><span data-stu-id="2e5f8-169">In *Startup.Auth.cs*, locate the call to `UseCookieAuthentication` and modify it as follows.</span></span> <span data-ttu-id="2e5f8-170">Cambie el nombre de cookie para que coincida con el nombre utilizado por el middleware de autenticación de la cookie de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2e5f8-170">Change the cookie name to match the name used by the ASP.NET Core cookie authentication middleware.</span></span> <span data-ttu-id="2e5f8-171">Proporcionar una instancia de un `DataProtectionProvider` inicializado para la ubicación de almacenamiento de claves de protección de datos comunes.</span><span class="sxs-lookup"><span data-stu-id="2e5f8-171">Provide an instance of a `DataProtectionProvider` initialized to the common data protection key storage location.</span></span> <span data-ttu-id="2e5f8-172">Asegúrese de que el nombre de la aplicación se establece en el nombre de aplicación común utilizado por todas las aplicaciones que comparten las cookies, `SharedCookieApp` en la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="2e5f8-172">Make sure that the app name is set to the common app name used by all apps that share cookies, `SharedCookieApp` in the sample app.</span></span>

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/App_Start/Startup.Auth.cs?name=snippet1)]

<span data-ttu-id="2e5f8-173">Consulte la *CookieAuthWithIdentity.NETFramework* del proyecto en el [código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([cómo descargar](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="2e5f8-173">See the *CookieAuthWithIdentity.NETFramework* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="2e5f8-174">Al generar una identidad de usuario, el tipo de autenticación debe coincidir con el tipo definido en `AuthenticationType` establecido con `UseCookieAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="2e5f8-174">When generating a user identity, the authentication type must match the type defined in `AuthenticationType` set with `UseCookieAuthentication`.</span></span>

<span data-ttu-id="2e5f8-175">*Models/IdentityModels.cs*:</span><span class="sxs-lookup"><span data-stu-id="2e5f8-175">*Models/IdentityModels.cs*:</span></span>

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/Models/IdentityModels.cs?name=snippet1)]

## <a name="use-a-common-user-database"></a><span data-ttu-id="2e5f8-176">Usar una base de datos de usuario común</span><span class="sxs-lookup"><span data-stu-id="2e5f8-176">Use a common user database</span></span>

<span data-ttu-id="2e5f8-177">Confirme que el sistema de identidad para cada aplicación apunta a la misma base de datos de usuario.</span><span class="sxs-lookup"><span data-stu-id="2e5f8-177">Confirm that the identity system for each app is pointed at the same user database.</span></span> <span data-ttu-id="2e5f8-178">En caso contrario, el sistema de identidades produce errores en tiempo de ejecución cuando intenta hacer coincidir la información de la cookie de autenticación con la información de su base de datos.</span><span class="sxs-lookup"><span data-stu-id="2e5f8-178">Otherwise, the identity system produces failures at runtime when it attempts to match the information in the authentication cookie against the information in its database.</span></span>
