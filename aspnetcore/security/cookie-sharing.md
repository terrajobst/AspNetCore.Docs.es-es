---
title: Compartir cookies entre aplicaciones con ASP.NET y ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo compartir cookies de autenticación entre ASP.NET 4.x y aplicaciones de ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 04/06/2019
uid: security/cookie-sharing
ms.openlocfilehash: 7a049ed8787808e228859afc051b8697a6261c21
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2019
ms.locfileid: "59068315"
---
# <a name="share-cookies-among-apps-with-aspnet-and-aspnet-core"></a>Compartir cookies entre aplicaciones con ASP.NET y ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Luke Latham](https://github.com/guardrex)

Los sitios Web a menudo constan de aplicaciones web individuales trabajan juntos. Para proporcionar una experiencia de inicio de sesión único (SSO), aplicaciones web dentro de un sitio deben compartir las cookies de autenticación. Para admitir este escenario, la pila de protección de datos permite compartir la autenticación con cookies Katana y vales de autenticación de cookies de ASP.NET Core.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([cómo descargarlo](xref:index#how-to-download-a-sample))

En el ejemplo muestra el uso compartido entre tres aplicaciones que usan la autenticación con cookies de cookie:

* Aplicación ASP.NET Core 2.0 Razor Pages sin usar [ASP.NET Core Identity](xref:security/authentication/identity)
* Aplicación MVC de ASP.NET Core 2.0 con ASP.NET Core Identity
* Aplicación MVC de ASP.NET Framework 4.6.1 con ASP.NET Identity

En los ejemplos siguientes:

* El nombre de la cookie de autenticación se establece en un valor común de `.AspNet.SharedCookie`.
* El `AuthenticationType` está establecido en `Identity.Application` explícitamente o de forma predeterminada.
* Un nombre de aplicación común se utiliza para habilitar el sistema de protección de datos compartir las claves de protección de datos (`SharedCookieApp`).
* `Identity.Application` se utiliza como el esquema de autenticación. Se usa cualquier esquema, se debe usar de forma coherente *dentro y entre* las aplicaciones de la cookie compartido como la combinación predeterminada o si se establece explícitamente. El esquema se utiliza al cifrar y descifrar las cookies, por lo que se debe usar un esquema coherente entre aplicaciones.
* Un común [clave de protección de datos](xref:security/data-protection/implementation/key-management) se utiliza la ubicación de almacenamiento. La aplicación de ejemplo usa una carpeta denominada *KeyRing* en la raíz de la solución para almacenar las claves de protección de datos.
* En las aplicaciones ASP.NET Core, [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) se usa para establecer la ubicación de almacenamiento de claves. [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) se usa para configurar un nombre de aplicación compartida común.
* En la aplicación de .NET Framework, el middleware de cookie de autenticación usa una implementación de [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider). `DataProtectionProvider` proporciona servicios de protección de datos para el cifrado y descifrado de datos de carga de cookie de autenticación. El `DataProtectionProvider` instancia está aislada del sistema de protección de datos utilizado por otras partes de la aplicación.
  * [DataProtectionProvider.Create (System.IO.DirectoryInfo, acción\<IDataProtectionBuilder >)](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider.create?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionProvider_Create_System_IO_DirectoryInfo_System_Action_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder__) acepta un [DirectoryInfo](/dotnet/api/system.io.directoryinfo) para especificar la ubicación de almacenamiento de claves de protección de datos. La aplicación de ejemplo proporciona la ruta de acceso de la *KeyRing* carpeta `DirectoryInfo`. [DataProtectionBuilderExtensions.SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionBuilderExtensions_SetApplicationName_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_) establece el nombre de aplicación comunes.
  * [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) requiere la [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) paquete NuGet. Para obtener este paquete de aplicaciones más adelante y ASP.NET Core 2.1, hacer referencia a la [Microsoft.AspNetCore.App metapaquete](xref:fundamentals/metapackage-app). Cuando el destino es .NET Framework, agregue una referencia de paquete a `Microsoft.AspNetCore.DataProtection.Extensions`.

## <a name="share-authentication-cookies-among-aspnet-core-apps"></a>Compartir cookies de autenticación entre aplicaciones de ASP.NET Core

Cuando se usa ASP.NET Core Identity:

::: moniker range=">= aspnetcore-2.0"

En el `ConfigureServices` método, use el [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie) método de extensión para configurar el servicio de protección de datos para las cookies.

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.Core/Startup.cs?name=snippet1)]

Las claves de protección de datos y el nombre de la aplicación deben compartirse entre aplicaciones. En las aplicaciones de ejemplo, `GetKeyRingDirInfo` devuelve la ubicación de almacenamiento de claves comunes para la [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) método. Use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) para configurar un nombre de aplicación compartida común (`SharedCookieApp` en el ejemplo). Para obtener más información, consulte [configurar la protección de datos](xref:security/data-protection/configuration/overview).

Al hospedar aplicaciones que comparten cookies entre subdominios, especifique un dominio común en el [Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain) propiedad. Compartir cookies entre aplicaciones en `contoso.com`, tales como `first_subdomain.contoso.com` y `second_subdomain.contoso.com`, especifique el `Cookie.Domain` como `.contoso.com`:

```csharp
options.Cookie.Domain = ".contoso.com";
```

Consulte la *CookieAuthWithIdentity.Core* del proyecto en el [código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([descarga](xref:index#how-to-download-a-sample)).

::: moniker-end

::: moniker range="< aspnetcore-2.0"

En el `Configure` método, use el [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) para configurar:

* El servicio de protección de datos para las cookies.
* El `AuthenticationScheme` para que coincida con ASP.NET 4.x.

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

Cuando se usa directamente las cookies:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](cookie-sharing/sample/CookieAuth.Core/Startup.cs?name=snippet1)]

Las claves de protección de datos y el nombre de la aplicación deben compartirse entre aplicaciones. En las aplicaciones de ejemplo, `GetKeyRingDirInfo` devuelve la ubicación de almacenamiento de claves comunes para la [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) método. Use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) para configurar un nombre de aplicación compartida común (`SharedCookieApp` en el ejemplo). Para obtener más información, consulte [configurar la protección de datos](xref:security/data-protection/configuration/overview).

Al hospedar aplicaciones que comparten cookies entre subdominios, especifique un dominio común en el [Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain) propiedad. Compartir cookies entre aplicaciones en `contoso.com`, tales como `first_subdomain.contoso.com` y `second_subdomain.contoso.com`, especifique el `Cookie.Domain` como `.contoso.com`:

```csharp
options.Cookie.Domain = ".contoso.com";
```

Consulte la *CookieAuth.Core* del proyecto en el [código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([descarga](xref:index#how-to-download-a-sample)).

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

## <a name="encrypting-data-protection-keys-at-rest"></a>Cifrar las claves de protección de datos en reposo

Para las implementaciones de producción, configure el `DataProtectionProvider` para cifrar las claves en reposo con DPAPI o un X509Certificate. Consulte [clave de cifrado en reposo](xref:security/data-protection/implementation/key-encryption-at-rest) para obtener más información.

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

## <a name="sharing-authentication-cookies-between-aspnet-4x-and-aspnet-core-apps"></a>Uso compartido de cookies de autenticación entre ASP.NET 4.x y aplicaciones de ASP.NET Core

Las aplicaciones ASP.NET 4.x que usar el middleware de autenticación de cookies de Katana pueden configurarse para generar las cookies de autenticación que son compatibles con el middleware de autenticación de cookies de ASP.NET Core. Esto permite actualizar aplicaciones individuales de un sitio grande por etapas al tiempo que proporciona una experiencia de inicio de sesión único fluida en todo el sitio.

Cuando una aplicación usa el middleware de autenticación de cookies de Katana, llama a `UseCookieAuthentication` en el proyecto *Startup.Auth.cs* archivo. Proyectos de aplicación web de ASP.NET 4.x crean con Visual Studio 2013 y usan más adelante el middleware de autenticación de cookies de Katana de forma predeterminada. Aunque `UseCookieAuthentication` está obsoleto y no compatibles para las aplicaciones de ASP.NET Core, una llamada a `UseCookieAuthentication` en una aplicación ASP.NET 4.x que usa Katana middleware de autenticación de la cookie es válida.

Una aplicación ASP.NET 4.x debe tener como destino .NET Framework 4.5.1 o posterior. En caso contrario, no instale los paquetes de NuGet necesarios.

Para compartir las cookies de autenticación entre una aplicación ASP.NET 4.x y una aplicación ASP.NET Core, configurar la aplicación de ASP.NET Core, como se indicó anteriormente, a continuación, configure la aplicación ASP.NET 4.x siguiendo estos pasos:

1. Instale el paquete [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) en cada aplicación ASP.NET 4.x.

2. En *Startup.Auth.cs*, busque la llamada a `UseCookieAuthentication` y modifíquela como se indica a continuación. Cambie el nombre de cookie para que coincida con el nombre utilizado por el middleware de autenticación de cookies de ASP.NET Core. Proporcionar una instancia de un `DataProtectionProvider` inicializado en la ubicación de almacenamiento de claves de protección de datos comunes. Asegúrese de que el nombre de la aplicación se establece en el nombre de aplicación común usado por todas las aplicaciones que comparten cookies, `SharedCookieApp` en la aplicación de ejemplo.

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/App_Start/Startup.Auth.cs?name=snippet1)]

Consulte la *CookieAuthWithIdentity.NETFramework* del proyecto en el [código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([descarga](xref:index#how-to-download-a-sample)).

Al generar una identidad de usuario, el tipo de autenticación debe coincidir con el tipo definido en `AuthenticationType` establecido con `UseCookieAuthentication`.

*Models/IdentityModels.cs*:

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/Models/IdentityModels.cs?name=snippet1)]

## <a name="use-a-common-user-database"></a>Usar una base de datos de usuario comunes

Confirme que el sistema de identidad para cada aplicación se señala a la misma base de datos de usuario. En caso contrario, el sistema de identidades genera errores en tiempo de ejecución cuando intenta hacer coincidir la información de la cookie de autenticación con la información de su base de datos.

## <a name="additional-resources"></a>Recursos adicionales

<xref:host-and-deploy/web-farm>
