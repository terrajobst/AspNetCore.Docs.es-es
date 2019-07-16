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
# <a name="share-authentication-cookies-among-aspnet-apps"></a>Compartir cookies de autenticación entre aplicaciones ASP.NET

Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Luke Latham](https://github.com/guardrex)

Los sitios Web a menudo constan de aplicaciones web individuales trabajan juntos. Para proporcionar una experiencia de inicio de sesión único (SSO), aplicaciones web dentro de un sitio deben compartir las cookies de autenticación. Para admitir este escenario, la pila de protección de datos permite compartir la autenticación con cookies Katana y vales de autenticación de cookies de ASP.NET Core.

En los ejemplos siguientes:

* El nombre de la cookie de autenticación se establece en un valor común de `.AspNet.SharedCookie`.
* El `AuthenticationType` está establecido en `Identity.Application` explícitamente o de forma predeterminada.
* Un nombre de aplicación común se utiliza para habilitar el sistema de protección de datos compartir las claves de protección de datos (`SharedCookieApp`).
* `Identity.Application` se utiliza como el esquema de autenticación. Se usa cualquier esquema, se debe usar de forma coherente *dentro y entre* las aplicaciones de la cookie compartido como la combinación predeterminada o si se establece explícitamente. El esquema se utiliza al cifrar y descifrar las cookies, por lo que se debe usar un esquema coherente entre aplicaciones.
* Un común [clave de protección de datos](xref:security/data-protection/implementation/key-management) se utiliza la ubicación de almacenamiento.
  * En las aplicaciones de ASP.NET Core, <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.PersistKeysToFileSystem*> se usa para establecer la ubicación de almacenamiento de claves.
  * En las aplicaciones de .NET Framework, el Middleware de Cookie de autenticación usa una implementación de <xref:Microsoft.AspNetCore.DataProtection.DataProtectionProvider>. `DataProtectionProvider` proporciona servicios de protección de datos para el cifrado y descifrado de datos de carga de cookie de autenticación. El `DataProtectionProvider` instancia está aislada del sistema de protección de datos utilizado por otras partes de la aplicación. [DataProtectionProvider.Create (System.IO.DirectoryInfo, acción\<IDataProtectionBuilder >)](xref:Microsoft.AspNetCore.DataProtection.DataProtectionProvider.Create*) acepta un <xref:System.IO.DirectoryInfo> para especificar la ubicación de almacenamiento de claves de protección de datos.
* `DataProtectionProvider` requiere el [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) paquete NuGet:
  * En las aplicaciones de ASP.NET Core 2.x, hacer referencia a la [Microsoft.AspNetCore.App metapaquete](xref:fundamentals/metapackage-app).
  * En las aplicaciones de .NET Framework, agregue una referencia de paquete a [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/).
* <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*> establece el nombre de aplicación comunes.

## <a name="share-authentication-cookies-among-aspnet-core-apps"></a>Compartir cookies de autenticación entre aplicaciones de ASP.NET Core

Cuando se usa ASP.NET Core Identity:

* Las claves de protección de datos y el nombre de la aplicación deben compartirse entre aplicaciones. Se proporciona una ubicación de almacenamiento de claves comunes para la <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.PersistKeysToFileSystem*> método en los ejemplos siguientes. Use <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*> para configurar un nombre de aplicación compartida común (`SharedCookieApp` en los ejemplos siguientes). Para obtener más información, consulta <xref:security/data-protection/configuration/overview>.
* Use el <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.ConfigureApplicationCookie*> método de extensión para configurar el servicio de protección de datos para las cookies.
* En el ejemplo siguiente, el tipo de autenticación se establece en `Identity.Application` de forma predeterminada.

En `Startup.ConfigureServices`:

```csharp
services.AddDataProtection()
    .PersistKeysToFileSystem({PATH TO COMMON KEY RING FOLDER})
    .SetApplicationName("SharedCookieApp");

services.ConfigureApplicationCookie(options => {
    options.Cookie.Name = ".AspNet.SharedCookie";
});
```

Al usar cookies directamente sin ASP.NET Core Identity, configurar protección de datos y la autenticación en `Startup.ConfigureServices`. En el ejemplo siguiente, el tipo de autenticación se establece en `Identity.Application`:

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

Al hospedar aplicaciones que comparten cookies entre subdominios, especifique un dominio común en el [Cookie.Domain](xref:Microsoft.AspNetCore.Http.CookieBuilder.Domain) propiedad. Compartir cookies entre aplicaciones en `contoso.com`, tales como `first_subdomain.contoso.com` y `second_subdomain.contoso.com`, especifique el `Cookie.Domain` como `.contoso.com`:

```csharp
options.Cookie.Domain = ".contoso.com";
```

## <a name="encrypt-data-protection-keys-at-rest"></a>Cifrar las claves de protección de datos en reposo

Para las implementaciones de producción, configure el `DataProtectionProvider` para cifrar las claves en reposo con DPAPI o un X509Certificate. Para obtener más información, consulta <xref:security/data-protection/implementation/key-encryption-at-rest>. En el ejemplo siguiente, se proporciona una huella digital del certificado para <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.ProtectKeysWithCertificate*>:

```csharp
services.AddDataProtection()
    .ProtectKeysWithCertificate("{CERTIFICATE THUMBPRINT}");
```

## <a name="share-authentication-cookies-between-aspnet-4x-and-aspnet-core-apps"></a>Compartir cookies de autenticación entre ASP.NET 4.x y aplicaciones de ASP.NET Core

Las aplicaciones ASP.NET 4.x que usan el software intermedio de Katana Cookie de autenticación pueden configurarse para generar las cookies de autenticación que son compatibles con el Middleware de autenticación de cookies de ASP.NET Core. Esto permite actualizar las aplicaciones individuales de un sitio gran tamaño en varios pasos al tiempo que proporciona una experiencia de inicio de sesión único fluida en todo el sitio.

Cuando una aplicación usa el Middleware de autenticación de cookies de Katana, llama a `UseCookieAuthentication` en el proyecto *Startup.Auth.cs* archivo. Proyectos de aplicación web de ASP.NET 4.x crean con Visual Studio 2013 y usan más adelante el Middleware de autenticación de cookies de Katana de forma predeterminada. Aunque `UseCookieAuthentication` está obsoleto y no compatibles para las aplicaciones de ASP.NET Core, una llamada a `UseCookieAuthentication` en ASP.NET 4.x app que usa el Middleware de autenticación de cookies de Katana es válido.

Una aplicación ASP.NET 4.x debe tener como destino .NET Framework 4.5.1 o posterior. En caso contrario, no instale los paquetes de NuGet necesarios.

Para compartir las cookies de autenticación entre una aplicación ASP.NET 4.x y una aplicación ASP.NET Core, configure la aplicación de ASP.NET Core, como se indica en la [compartir cookies de autenticación entre aplicaciones de ASP.NET Core](#share-authentication-cookies-among-aspnet-core-apps) sección y, después, configure la aplicación ASP.NET 4.x como se indica a continuación.

Confirme que los paquetes de la aplicación se actualizan a las versiones más recientes. Instalar el [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) paquete en cada aplicación ASP.NET 4.x.

Busque y modifique la llamada a `UseCookieAuthentication`:

* Cambiar el nombre de cookie para que coincida con el nombre utilizado por el Middleware de autenticación de cookies de ASP.NET Core (`.AspNet.SharedCookie` en el ejemplo).
* En el ejemplo siguiente, el tipo de autenticación se establece en `Identity.Application`.
* Proporcionar una instancia de un `DataProtectionProvider` inicializado en la ubicación de almacenamiento de claves de protección de datos comunes.
* Confirme que el nombre de la aplicación está configurado como el nombre de aplicación común usado por todas las aplicaciones que comparten cookies de autenticación (`SharedCookieApp` en el ejemplo).

Si no establece `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier` y `http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider`, establezca <xref:System.Web.Helpers.AntiForgeryConfig.UniqueClaimTypeIdentifier> para una notificación que distingue a los usuarios únicos.

*App_Start/Startup.auth.cs*:

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

Al generar una identidad de usuario, el tipo de autenticación (`Identity.Application`) debe coincidir con el tipo definido en `AuthenticationType` establecido con `UseCookieAuthentication` en *App_Start/Startup.Auth.cs*.

*Models/IdentityModels.cs*:

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

## <a name="use-a-common-user-database"></a>Usar una base de datos de usuario comunes

Cuando las aplicaciones usan la misma identidad de esquema (misma versión de identidad), confirme que el sistema de identidad para cada aplicación se señala a la misma base de datos de usuario. En caso contrario, el sistema de identidades genera errores en tiempo de ejecución cuando intenta hacer coincidir la información de la cookie de autenticación con la información de su base de datos.

Cuando el esquema de la identidad es diferente entre aplicaciones, normalmente porque las aplicaciones usan diferentes versiones de identidad, uso compartido de una base de datos común según la versión más reciente de identidad no es posible sin tener que volver a asignar y agregar columnas en esquemas de identidad de la otra aplicación. A menudo resulta más eficaz actualizar las otras aplicaciones para usar la versión más reciente de identidad para que las aplicaciones puede compartir una base de datos comunes.

## <a name="additional-resources"></a>Recursos adicionales

* <xref:host-and-deploy/web-farm>
