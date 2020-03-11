---
title: Compartir cookies de autenticación entre aplicaciones ASP.NET
author: rick-anderson
description: Aprenda a compartir cookies de autenticación entre ASP.NET 4. x y las aplicaciones ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/05/2019
uid: security/cookie-sharing
ms.openlocfilehash: 7e29be22717f0b97fc115ac036cc54e333bed4e2
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78651611"
---
# <a name="share-authentication-cookies-among-aspnet-apps"></a>Compartir cookies de autenticación entre aplicaciones ASP.NET

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

Los sitios web a menudo se componen de aplicaciones web individuales que trabajan juntas. Para proporcionar una experiencia de inicio de sesión único (SSO), las aplicaciones web dentro de un sitio deben compartir las cookies de autenticación. Para admitir este escenario, la pila de protección de datos permite compartir la autenticación de cookies de Katana y ASP.NET Core vales de autenticación de cookies.

En los ejemplos siguientes:

* El nombre de la cookie de autenticación se establece en un valor común de `.AspNet.SharedCookie`.
* La `AuthenticationType` se establece en `Identity.Application` explícitamente o de forma predeterminada.
* Se usa un nombre de aplicación común para permitir que el sistema de protección de datos comparta claves de protección de datos (`SharedCookieApp`).
* `Identity.Application` se utiliza como esquema de autenticación. Sea cual sea el esquema que se use, se debe usar de forma coherente *dentro y entre* las aplicaciones de cookies compartidas, ya sea como esquema predeterminado o mediante su configuración explícita. El esquema se usa al cifrar y descifrar las cookies, por lo que se debe usar un esquema coherente entre las aplicaciones.
* Se utiliza una ubicación de almacenamiento de [claves de protección de datos](xref:security/data-protection/implementation/key-management) común.
  * En ASP.NET Core aplicaciones, <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.PersistKeysToFileSystem*> se usa para establecer la ubicación de almacenamiento de la clave.
  * En .NET Framework aplicaciones, el middleware de autenticación de cookies usa una implementación de <xref:Microsoft.AspNetCore.DataProtection.DataProtectionProvider>. `DataProtectionProvider` proporciona servicios de protección de datos para el cifrado y descifrado de los datos de carga de la cookie de autenticación. La instancia de `DataProtectionProvider` está aislada del sistema de protección de datos usado por otras partes de la aplicación. [DataProtectionProvider. Create (System. IO. DirectoryInfo, Action\<IDataProtectionBuilder >)](xref:Microsoft.AspNetCore.DataProtection.DataProtectionProvider.Create*) acepta un <xref:System.IO.DirectoryInfo> para especificar la ubicación del almacenamiento de la clave de protección de datos.
* `DataProtectionProvider` requiere el paquete NuGet [Microsoft. AspNetCore. bioprotection. Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) :
  * En ASP.NET Core aplicaciones 2. x, haga referencia al [metapaquete Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app).
  * En .NET Framework aplicaciones, agregue una referencia de paquete a [Microsoft. AspNetCore. desproteccion. Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/).
* <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*> establece el nombre común de la aplicación.

## <a name="share-authentication-cookies-with-aspnet-core-identity"></a>Compartir cookies de autenticación con ASP.NET Core identidad

Al usar ASP.NET Core identidad:

* Las claves de protección de datos y el nombre de la aplicación se deben compartir entre las aplicaciones. Se proporciona una ubicación de almacenamiento de claves común al método <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.PersistKeysToFileSystem*> en los ejemplos siguientes. Use <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*> para configurar un nombre común de la aplicación compartida (`SharedCookieApp` en los ejemplos siguientes). Para más información, consulte <xref:security/data-protection/configuration/overview>.
* Use el método de extensión <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.ConfigureApplicationCookie*> para configurar el servicio de protección de datos para las cookies.
* El tipo de autenticación predeterminado es `Identity.Application`.

En `Startup.ConfigureServices`:

```csharp
services.AddDataProtection()
    .PersistKeysToFileSystem("{PATH TO COMMON KEY RING FOLDER}")
    .SetApplicationName("SharedCookieApp");

services.ConfigureApplicationCookie(options => {
    options.Cookie.Name = ".AspNet.SharedCookie";
});
```

## <a name="share-authentication-cookies-without-aspnet-core-identity"></a>Compartir cookies de autenticación sin ASP.NET Core identidad

Al usar cookies directamente sin ASP.NET Core identidad, configure la protección de datos y la autenticación en `Startup.ConfigureServices`. En el ejemplo siguiente, el tipo de autenticación se establece en `Identity.Application`:

```csharp
services.AddDataProtection()
    .PersistKeysToFileSystem("{PATH TO COMMON KEY RING FOLDER}")
    .SetApplicationName("SharedCookieApp");

services.AddAuthentication("Identity.Application")
    .AddCookie("Identity.Application", options =>
    {
        options.Cookie.Name = ".AspNet.SharedCookie";
    });
```

## <a name="share-cookies-across-different-base-paths"></a>Compartir cookies entre diferentes rutas de acceso base

Una cookie de autenticación utiliza [HttpRequest. PathBase](xref:Microsoft.AspNetCore.Http.HttpRequest.PathBase) como su [Cookie predeterminada. Path](xref:Microsoft.AspNetCore.Http.CookieBuilder.Path). Si la cookie de la aplicación se debe compartir entre diferentes rutas de acceso base, se debe invalidar `Path`:

```csharp
services.AddDataProtection()
    .PersistKeysToFileSystem("{PATH TO COMMON KEY RING FOLDER}")
    .SetApplicationName("SharedCookieApp");

services.ConfigureApplicationCookie(options => {
    options.Cookie.Name = ".AspNet.SharedCookie";
    options.Cookie.Path = "/";
});
```

## <a name="share-cookies-across-subdomains"></a>Compartir cookies entre subdominios

Cuando hospede aplicaciones que compartan cookies entre subdominios, especifique un dominio común en la propiedad [cookie. Domain](xref:Microsoft.AspNetCore.Http.CookieBuilder.Domain) . Para compartir cookies entre aplicaciones en `contoso.com`, como `first_subdomain.contoso.com` y `second_subdomain.contoso.com`, especifique el `Cookie.Domain` como `.contoso.com`:

```csharp
options.Cookie.Domain = ".contoso.com";
```

## <a name="encrypt-data-protection-keys-at-rest"></a>Cifrado de claves de protección de datos en reposo

En el caso de las implementaciones de producción, configure el `DataProtectionProvider` para cifrar las claves en reposo con DPAPI o con un certificado X509. Para más información, consulte <xref:security/data-protection/implementation/key-encryption-at-rest>. En el ejemplo siguiente, se proporciona una huella digital de certificado para <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.ProtectKeysWithCertificate*>:

```csharp
services.AddDataProtection()
    .ProtectKeysWithCertificate("{CERTIFICATE THUMBPRINT}");
```

## <a name="share-authentication-cookies-between-aspnet-4x-and-aspnet-core-apps"></a>Compartir cookies de autenticación entre ASP.NET 4. x y las aplicaciones ASP.NET Core

Las aplicaciones ASP.NET 4. x que usan el middleware de autenticación de cookies Katana se pueden configurar para generar cookies de autenticación que son compatibles con el middleware de autenticación de cookies ASP.NET Core. Esto permite actualizar las aplicaciones individuales de un sitio de gran tamaño en varios pasos, a la vez que proporciona una experiencia de SSO sin problemas en todo el sitio.

Cuando una aplicación usa el middleware de autenticación de cookies de Katana, llama a `UseCookieAuthentication` en el archivo *Startup.auth.CS* del proyecto. Los proyectos de aplicación Web de ASP.NET 4. x creados con Visual Studio 2013 y versiones posteriores usan el middleware de autenticación de cookies Katana de forma predeterminada. Aunque `UseCookieAuthentication` está obsoleto y no se admite para aplicaciones de ASP.NET Core, llamar a `UseCookieAuthentication` en una aplicación ASP.NET 4. x que usa el middleware de autenticación de cookies Katana es válido.

Una aplicación ASP.NET 4. x debe tener como destino .NET Framework 4.5.1 o posterior. De lo contrario, no se pueden instalar los paquetes de NuGet necesarios.

Para compartir las cookies de autenticación entre una aplicación ASP.NET 4. x y una aplicación ASP.NET Core, configure la aplicación de ASP.NET Core como se indica en la sección [compartir cookies de autenticación entre ASP.net Core aplicaciones](#share-authentication-cookies-with-aspnet-core-identity) y, a continuación, configure la aplicación ASP.net 4. x como se indica a continuación.

Confirme que los paquetes de la aplicación se actualizan a las versiones más recientes. Instale el paquete [Microsoft. Owin. Security. Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) en cada aplicación ASP.net 4. x.

Busque y modifique la llamada a `UseCookieAuthentication`:

* Cambie el nombre de la cookie para que coincida con el nombre usado por el middleware de autenticación de cookies ASP.NET Core (`.AspNet.SharedCookie` en el ejemplo).
* En el ejemplo siguiente, el tipo de autenticación se establece en `Identity.Application`.
* Proporcione una instancia de una `DataProtectionProvider` inicializada en la ubicación de almacenamiento de la clave Common Data Protection.
* Confirme que el nombre de la aplicación se establece en el nombre de aplicación común que usan todas las aplicaciones que comparten cookies de autenticación (`SharedCookieApp` en el ejemplo).

Si no se establece `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier` y `http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider`, establezca <xref:System.Web.Helpers.AntiForgeryConfig.UniqueClaimTypeIdentifier> en una demanda que distinga a usuarios únicos.

*App_Start/startup.auth.CS*:

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
            DataProtectionProvider.Create("{PATH TO COMMON KEY RING FOLDER}",
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

Al generar una identidad de usuario, el tipo de autenticación (`Identity.Application`) debe coincidir con el tipo definido en `AuthenticationType` establecido con `UseCookieAuthentication` en *App_Start/startup.auth.CS*.

*Modelos/IdentityModels. CS*:

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

## <a name="use-a-common-user-database"></a>Usar una base de datos de usuario común

Cuando las aplicaciones usen el mismo esquema de identidad (la misma versión de la identidad), confirme que el sistema de identidad de cada aplicación señala a la misma base de datos de usuario. De lo contrario, el sistema de identidad produce errores en tiempo de ejecución cuando intenta hacer coincidir la información de la cookie de autenticación con la información de su base de datos.

Cuando el esquema de identidad es diferente entre las aplicaciones, normalmente porque las aplicaciones usan versiones de identidad diferentes, compartir una base de datos común basada en la versión más reciente de la identidad no es posible sin volver a asignar y Agregar columnas en los esquemas de identidad de la aplicación. A menudo, es más eficaz actualizar las otras aplicaciones para que usen la versión más reciente de la identidad para que las aplicaciones puedan compartir una base de datos común.

## <a name="additional-resources"></a>Recursos adicionales

* <xref:host-and-deploy/web-farm>
