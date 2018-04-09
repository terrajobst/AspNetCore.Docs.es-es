---
title: Compartir cookies entre aplicaciones con ASP.NET y ASP.NET Core
author: rick-anderson
description: Obtenga información acerca de cómo compartir las cookies de autenticación entre ASP.NET 4.x y las aplicaciones de ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/19/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/cookie-sharing
ms.openlocfilehash: 2c0f5de4ecedb796e85c08fc50d9697947a75a3f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
# <a name="share-cookies-among-apps-with-aspnet-and-aspnet-core"></a>Compartir cookies entre aplicaciones con ASP.NET y ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Luke Latham](https://github.com/guardrex)

Sitios Web a menudo constan de las aplicaciones web individuales que trabajan juntos. Para proporcionar una experiencia de inicio de sesión único (SSO), las aplicaciones web dentro de un sitio deben compartir las cookies de autenticación. Para admitir este escenario, la pila de protección de datos permite compartir la autenticación con cookies Katana y vales de autenticación de ASP.NET Core cookie.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

El ejemplo ilustra la cookie compartir entre tres aplicaciones que usan autenticación con cookies:

* Aplicación de las páginas de Razor de núcleo 2.0 de ASP.NET sin usar [ASP.NET Core Identity](xref:security/authentication/identity)
* Aplicación MVC de ASP.NET Core 2.0 con la identidad de ASP.NET Core
* Aplicación MVC de ASP.NET Framework 4.6.1 con la identidad de ASP.NET

En los ejemplos siguientes:

* El nombre de la cookie de autenticación se establece en un valor común de `.AspNet.SharedCookie`.
* El `AuthenticationType` está establecido en `Identity.Application` explícitamente o de forma predeterminada.
* Un nombre de aplicación común se utiliza para habilitar el sistema de protección de datos compartir las claves de protección de datos (`SharedCookieApp`).
* `Identity.Application` se utiliza como el esquema de autenticación. Se utiliza el esquema, se debe usar de forma coherente *dentro y entre* las aplicaciones de cookie compartido como el esquema predeterminado o si se establece explícitamente. El esquema se utiliza al cifrar y descifrar las cookies, por lo que se debe usar un esquema coherente entre aplicaciones.
* Común [clave de protección de datos](xref:security/data-protection/implementation/key-management) se utiliza la ubicación de almacenamiento. La aplicación de ejemplo utiliza una carpeta denominada *KeyRing* en la raíz de la solución para almacenar las claves de protección de datos.
* En las aplicaciones ASP.NET Core, [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) se usa para establecer la ubicación de almacenamiento de claves. [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) se usa para configurar un nombre de aplicación compartido común.
* En la aplicación de .NET Framework, el middleware de autenticación de cookie usa una implementación de [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider). `DataProtectionProvider` proporciona servicios de protección de datos para el cifrado y descifrado de datos de carga de cookie de autenticación. El `DataProtectionProvider` instancia está aislada del sistema de protección de datos utilizado por otras partes de la aplicación.
  * [DataProtectionProvider.Create (System.IO.DirectoryInfo, acción\<IDataProtectionBuilder >)](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider.create?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionProvider_Create_System_IO_DirectoryInfo_System_Action_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder__) acepta un [DirectoryInfo](/dotnet/api/system.io.directoryinfo) para especificar la ubicación de almacenamiento de claves de protección de datos. La aplicación de ejemplo proporciona la ruta de acceso de la *KeyRing* carpeta `DirectoryInfo`. [DataProtectionBuilderExtensions.SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionBuilderExtensions_SetApplicationName_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_) establece el nombre de aplicación común.
  * [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) requiere el [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) paquete NuGet. Para obtener este paquete de núcleo de ASP.NET 2.0 y aplicaciones más adelante, hacen referencia a la [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage. Cuando el destino es .NET Framework, agregue una referencia de paquete a `Microsoft.AspNetCore.DataProtection.Extensions`.

## <a name="share-authentication-cookies-among-aspnet-core-apps"></a>Compartir las cookies de autenticación entre aplicaciones de ASP.NET Core

Al utilizar la identidad de núcleo de ASP.NET:

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)
En el `ConfigureServices` método, use la [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie) método de extensión para configurar el servicio de protección de datos para las cookies.

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.Core/Startup.cs?name=snippet1)]

Las claves de protección de datos y el nombre de la aplicación deben compartirse entre aplicaciones. En las aplicaciones de ejemplo, `GetKeyRingDirInfo` devuelve la ubicación de almacenamiento de claves comunes para la [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) método. Use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) para configurar un nombre de aplicación compartido común (`SharedCookieApp` en el ejemplo). Para obtener más información, consulte [configurar la protección de datos](xref:security/data-protection/configuration/overview).

Consulte la *CookieAuthWithIdentity.Core* del proyecto en el [código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([cómo descargar](xref:tutorials/index#how-to-download-a-sample)).

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)
En el `Configure` método, use la [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) para configurar:

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

* * *
Al utilizar cookies directamente:

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)
[!code-csharp[](cookie-sharing/sample/CookieAuth.Core/Startup.cs?name=snippet1)]

Las claves de protección de datos y el nombre de la aplicación deben compartirse entre aplicaciones. En las aplicaciones de ejemplo, `GetKeyRingDirInfo` devuelve la ubicación de almacenamiento de claves comunes para la [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) método. Use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) para configurar un nombre de aplicación compartido común (`SharedCookieApp` en el ejemplo). Para obtener más información, consulte [configurar la protección de datos](xref:security/data-protection/configuration/overview). 

Consulte la *CookieAuth.Core* del proyecto en el [código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([cómo descargar](xref:tutorials/index#how-to-download-a-sample)).

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)
```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    DataProtectionProvider = 
        DataProtectionProvider.Create(
            new DirectoryInfo(@"PATH_TO_KEY_RING_FOLDER"))
});
```

* * *
## <a name="encrypting-data-protection-keys-at-rest"></a>Cifrado de claves de protección de datos en reposo

Para las implementaciones de producción, configurar el `DataProtectionProvider` para cifrar las claves en reposo con DPAPI o un X509Certificate. Vea [clave de cifrado de datos almacenados](xref:security/data-protection/implementation/key-encryption-at-rest) para obtener más información.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .ProtectKeysWithCertificate("thumbprint");
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

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

## <a name="sharing-authentication-cookies-between-aspnet-4x-and-aspnet-core-apps"></a>Uso compartido de las cookies de autenticación entre ASP.NET 4.x y las aplicaciones de ASP.NET Core

Las aplicaciones ASP.NET 4.x que usar el middleware de autenticación de cookie de Katana pueden configurarse para generar las cookies de autenticación que son compatibles con el middleware de autenticación de la cookie de ASP.NET Core. Esto permite actualizar aplicaciones individuales de un sitio grande por etapas al proporcionar una experiencia SSO sin problemas en todo el sitio.

> [!TIP]
> Cuando una aplicación usa middleware de autenticación de cookie de Katana, llama al método `UseCookieAuthentication` en el proyecto *Startup.Auth.cs* archivo. Proyectos de aplicación web de ASP.NET 4.x crean con Visual Studio 2013 y utilizan el middleware de autenticación de la cookie de Katana de forma predeterminada.

> [!NOTE]
> Una aplicación ASP.NET 4.x debe tener como destino .NET Framework 4.5.1 o posterior. En caso contrario, los paquetes de NuGet necesarios no puede instalar.

Para compartir las cookies de autenticación entre aplicaciones de ASP.NET Core y ASP.NET 4.x, configure la aplicación de ASP.NET Core tal y como se ha indicado anteriormente, a continuación, configure las aplicaciones ASP.NET 4.x siguiendo los pasos siguientes.

1. Instalar el paquete [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) en cada aplicación ASP.NET 4.x.

2. En *Startup.Auth.cs*, busque la llamada a `UseCookieAuthentication` y modifíquelo como se indica a continuación. Cambie el nombre de cookie para que coincida con el nombre utilizado por el middleware de autenticación de la cookie de ASP.NET Core. Proporcionar una instancia de un `DataProtectionProvider` inicializado para la ubicación de almacenamiento de claves de protección de datos comunes. Asegúrese de que el nombre de la aplicación se establece en el nombre de aplicación común utilizado por todas las aplicaciones que comparten las cookies, `SharedCookieApp` en la aplicación de ejemplo.

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)
[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/App_Start/Startup.Auth.cs?name=snippet1)]

Consulte la *CookieAuthWithIdentity.NETFramework* del proyecto en el [código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([cómo descargar](xref:tutorials/index#how-to-download-a-sample)).

Al generar una identidad de usuario, el tipo de autenticación debe coincidir con el tipo definido en `AuthenticationType` establecido con `UseCookieAuthentication`.

*Models/IdentityModels.cs*:

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/Models/IdentityModels.cs?name=snippet1)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)
Establecer el `CookieManager` sobre la interoperabilidad `ChunkingCookieManager` para el formato fragmentado no es compatible.

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    AuthenticationType = DefaultAuthenticationTypes.ApplicationCookie,
    CookieName = ".AspNetCore.Cookies",
    // CookieName = ".AspNetCore.ApplicationCookie", (if using ASP.NET Identity)
    // CookiePath = "...", (if necessary)
    // ...
    TicketDataFormat = new AspNetTicketDataFormat(
        new DataProtectorShim(
            DataProtectionProvider.Create(
                new DirectoryInfo(@"PATH_TO_KEY_RING_FOLDER"))
            .CreateProtector(
                "Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationMiddleware",
                "Cookies", 
                "v2"))),
    CookieManager = new ChunkingCookieManager()
});
```

* * *
## <a name="use-a-common-user-database"></a>Usar una base de datos de usuario común

Confirme que el sistema de identidad para cada aplicación apunta a la misma base de datos de usuario. En caso contrario, el sistema de identidades produce errores en tiempo de ejecución cuando intenta hacer coincidir la información de la cookie de autenticación con la información de su base de datos.
