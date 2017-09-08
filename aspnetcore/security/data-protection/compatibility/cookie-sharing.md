---
title: Uso compartido de las cookies entre aplicaciones
author: rick-anderson
description: 
keywords: "Núcleo de ASP.NET, ASP.NET, las cookies, interoperabilidad, cookie de uso compartido"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 9a7aae45-8e21-4c54-950c-3c29df6c1082
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/compatibility/cookie-sharing
ms.openlocfilehash: c02a00a7f1defd081c5490a9cbc9ad4bcc6ba747
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/11/2017
---
# <a name="sharing-cookies-between-applications"></a>Uso compartido de las cookies entre aplicaciones

Sitios Web normalmente consisten en muchas aplicaciones web individuales, todas funcionan juntos armonía. Si desea que un desarrollador de aplicaciones proporcionar una buena experiencia de inicio de sesión único, a menudo tendrán todas las aplicaciones web diferente dentro del sitio para compartir los vales de autenticación entre sí.

Para admitir este escenario, la pila de protección de datos permite compartir la autenticación con cookies Katana y vales de autenticación de ASP.NET Core cookie.

## <a name="sharing-authentication-cookies-between-applications"></a>Cookies de autenticación para compartir entre aplicaciones

Para compartir las cookies de autenticación entre dos aplicaciones diferentes que ASP.NET Core, configure cada aplicación que se debe compartir las cookies como se indica a continuación.

En su configurar los métodos que utilizan el CookieAuthenticationOptions para configurar el servicio de protección de datos para las cookies y el AuthenticationScheme para que coincida con ASP.NET 4.X.

Si usas identidad:

```csharp
app.AddIdentity<ApplicationUser, IdentityRole>(options =>
   {
       options.Cookies.ApplicationCookie.AuthenticationScheme = "ApplicationCookie";
       var protectionProvider = DataProtectionProvider.Create(new DirectoryInfo(@"c:\shared-auth-ticket-keys\"));
       options.Cookies.ApplicationCookie.DataProtectionProvider = protectionProvider;
       options.Cookies.ApplicationCookie.TicketDataFormat = new TicketDataFormat(protectionProvider.CreateProtector("Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationMiddleware", "Cookies", "v2"));
   });
   ```

Si usa cookies directamente:

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
   {
       DataProtectionProvider = DataProtectionProvider.Create(new DirectoryInfo(@"c:\shared-auth-ticket-keys\"))
   });
   ```
   
El `DataProtectionProvider` requiere el `Microsoft.AspNetCore.DataProtection.Extensions` paquete NuGet.

Cuando se utiliza de esta manera, DirectoryInfo debe apuntar a una ubicación de almacenamiento de claves que se reservan específicamente para las cookies de autenticación. El middleware de autenticación de la cookie utiliza la implementación proporcionada explícitamente de la DataProtectionProvider, que ahora está aislada del sistema de protección de datos utilizada por otras partes de la aplicación. Se omite el nombre de la aplicación (intencionadamente por lo tanto, puesto que está intentando obtener varias aplicaciones compartan cargas).

>[!WARNING]
>Puede configurar el DataProtectionProvider de modo que las claves se cifran en reposo, como en el ejemplo siguiente.
>
>
>  ```csharp
>  app.UseCookieAuthentication(new CookieAuthenticationOptions
>  {
>      DataProtectionProvider = DataProtectionProvider.Create(
>          new DirectoryInfo(@"c:\shared-auth-ticket-keys\"),
>          configure =>
>          {
>              configure.ProtectKeysWithCertificate("thumbprint");
>          })
>  });
>  ```

## <a name="sharing-authentication-cookies-between-aspnet-4x-and-aspnet-core-applications"></a>Uso compartido de las cookies de autenticación entre ASP.NET 4.x y las aplicaciones de ASP.NET Core

Las aplicaciones de ASP.NET 4.x que usar el middleware de autenticación de cookie de Katana pueden configurarse para generar las cookies de autenticación que son compatibles con el middleware de autenticación de la cookie de ASP.NET Core. Esto permite actualizar las aplicaciones individuales de un sitio grande por etapas mientras sigue proporcionando un inicio de sesión único sin problemas en la experiencia en todo el sitio.

>[!TIP]
> Puede indicar si la aplicación existente usa middleware de autenticación de cookie de Katana por la existencia de una llamada a UseCookieAuthentication en Startup.Auth.cs del proyecto. Proyectos de aplicación web de ASP.NET 4.x crean con Visual Studio 2013 y utilizan el middleware de autenticación de la cookie de Katana de forma predeterminada.

> [!NOTE]
> La aplicación de ASP.NET 4.x debe tener como destino .NET Framework 4.5.1 o posterior, en caso contrario, los paquetes de NuGet necesarios no se instala.

Para compartir las cookies de autenticación entre las aplicaciones de ASP.NET 4.x y las aplicaciones de ASP.NET Core, configure la aplicación de ASP.NET Core tal y como se ha indicado anteriormente, a continuación, configure las aplicaciones de ASP.NET 4.x siguiendo los pasos siguientes.

1.  Instale el paquete Microsoft.Owin.Security.Interop en cada una de las aplicaciones de ASP.NET 4.x.

2.   En Startup.Auth.cs, busque la llamada a UseCookieAuthentication, que normalmente tendrá un aspecto similar al siguiente.

    ```csharp
    app.UseCookieAuthentication(new CookieAuthenticationOptions
    {
        // ...
    });
    ```
    
3.  Modifique la llamada a UseCookieAuthentication como se indica a continuación, cambiar el CookieName para que coincida con el nombre utilizado por el middleware de autenticación de la cookie de ASP.NET Core, proporcionar una instancia de un DataProtectionProvider que se ha inicializado a una ubicación de almacenamiento de claves, y establece CookieManager en interoperabilidad ChunkingCookieManager de modo que el formato fragmentado no es compatible.

    ```csharp
    app.UseCookieAuthentication(new CookieAuthenticationOptions
       {
           AuthenticationType = DefaultAuthenticationTypes.ApplicationCookie,
           CookieName = ".AspNetCore.Cookies",
           // CookieName = ".AspNetCore.ApplicationCookie", (if you're using identity)
           // CookiePath = "...", (if necessary)
           // ...
           TicketDataFormat = new AspNetTicketDataFormat(
               new DataProtectorShim(
                   DataProtectionProvider.Create(new DirectoryInfo(@"c:\shared-auth-ticket-keys\"))
                   .CreateProtector("Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationMiddleware",
                   "Cookies", "v2"))),
           CookieManager = new ChunkingCookieManager()
       });
       ```
    DirectoryInfo debe apuntar a la misma ubicación de almacenamiento que apunta la aplicación de ASP.NET Core para y debe configurarse con la misma configuración.

ASP.NET 4.x y aplicaciones de ASP.NET Core ahora están configurados para compartir las cookies de autenticación.

> [!NOTE]
> Debe asegurarse de que el sistema de identidad para cada aplicación apunta a la misma base de datos de usuario. En caso contrario, el sistema de identidades producirá errores en tiempo de ejecución cuando intenta hacer coincidir la información de la cookie de autenticación con la información de su base de datos.
