---
title: Autenticar a los usuarios con WS-Federation en ASP.NET Core
author: chlowell
description: "Este tutorial muestra cómo utilizar WS-Federation en una aplicación de ASP.NET Core."
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 02/27/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/ws-federation
ms.openlocfilehash: 0532f866e9c58b2e45623f522f62438e15017e54
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2018
---
# <a name="authenticate-users-with-ws-federation-in-aspnet-core"></a>Autenticar a los usuarios con WS-Federation en ASP.NET Core

Este tutorial muestra cómo permitir a los usuarios iniciar sesión con un proveedor de autenticación de WS-Federation como Active Directory Federation Services (ADFS) o [Azure Active Directory](/azure/active-directory/) (AAD). Usa la aplicación de ejemplo básica de ASP.NET 2.0 se describe en [Facebook, Google y la autenticación de proveedor externo](xref:security/authentication/social/index).

Para las aplicaciones de ASP.NET Core 2.0, ofrece compatibilidad con WS-Federation [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation). Este componente se procede de [Microsoft.Owin.Security.WsFederation](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation) y comparte muchos de los mecanismos de ese componente. Sin embargo, los componentes se diferencian en un par de aspectos importantes.

De forma predeterminada, el middleware nueva:

* No permite que los inicios de sesión no solicitados. Esta característica del protocolo WS-Federation es vulnerable a ataques XSRF. Sin embargo, se puede habilitar con el `AllowUnsolicitedLogins` opción.
* No se comprueba cada formulario post para los mensajes de inicio de sesión. Solo se solicita a la `CallbackPath` se comprueban para componentes de inicio de sesión `CallbackPath` tiene como valor predeterminado `/signin-wsfed` pero puede cambiarse. Esta ruta de acceso se puede compartir con otros proveedores de autenticación habilitando el `SkipUnrecognizedRequests` opción.

## <a name="register-the-app-with-active-directory"></a>Registrar la aplicación con Active Directory

### <a name="active-directory-federation-services"></a>Servicios de federación de Active Directory

* Abra el servidor **entidad confiar en Asistente para agregar** desde la consola de administración de AD FS:

![Agregar usuario de confianza de asistente: bienvenida](ws-federation/_static/AdfsAddTrust.png)

* Para escribir manualmente los datos, elija:

![Agregar a Asistente para la relación de confianza para usuario autenticado: Seleccione el origen de datos](ws-federation/_static/AdfsSelectDataSource.png)

* Escriba un nombre para mostrar para el usuario autenticado. El nombre no es importante para la aplicación de ASP.NET Core.

* [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) no es compatible con el cifrado de tokens, por lo que no configure un certificado de cifrado de tokens:

![Agregar a Asistente para la relación de confianza para usuario autenticado: Configurar certificados](ws-federation/_static/AdfsConfigureCert.png)

* Habilitar la compatibilidad de protocolo WS-Federation Passive, utilizando la dirección URL de la aplicación. Compruebe que el puerto sea correcto para la aplicación:

![Agregar a Asistente para la relación de confianza para usuario autenticado: Configurar la dirección URL](ws-federation/_static/AdfsConfigureUrl.png)

> [!NOTE]
> Debe ser una dirección URL HTTPS. IIS Express puede proporcionar un certificado autofirmado al hospedar la aplicación durante el desarrollo. Kestrel requiere configuración manual de certificados. Consulte la [documentación Kestrel](xref:fundamentals/servers/kestrel) para obtener más detalles.

* Haga clic en **siguiente** a través del resto del asistente y **cerrar** al final.

* Identidad de núcleo ASP.NET requiere un **Id. de nombre** de notificación. Agregue uno de los **editar reglas de notificación** cuadro de diálogo:

![Editar reglas de notificaciones](ws-federation/_static/EditClaimRules.png)

* En el **transformar notificaciones Asistente para agregar reglas**, deje el valor predeterminado **enviar atributos LDAP como notificaciones** plantilla seleccionada y haga clic en **siguiente**. Agregar una asignación de regla el **nombre de cuenta SAM** atributo LDAP para la **Id. de nombre** notificación saliente:

![Agregar Asistente para reglas de notificación de transformación: Configurar la regla de notificación](ws-federation/_static/AddTransformClaimRule.png)

* Haga clic en **finalizar** > **Aceptar** en el **editar reglas de notificación** ventana.

### <a name="azure-active-directory"></a>Azure Active Directory

* Vaya a la hoja de los registros de aplicación del inquilino AAD. Haga clic en **nuevo registro de aplicación**:

![Azure Active Directory: Registros de aplicaciones](ws-federation/_static/AadNewAppRegistration.png)

* Escriba un nombre para el registro de aplicación. Esto no es importante para la aplicación de ASP.NET Core.
* Escriba la dirección URL de la aplicación de escucha en que la **dirección URL de inicio de sesión**:

![Azure Active Directory: Crear el registro de aplicación](ws-federation/_static/AadCreateAppRegistration.png)

* Haga clic en **extremos** y tenga en cuenta el **documento de metadatos de federación** dirección URL. Se trata el middleware de WS-Federation `MetadataAddress`:

![Azure Active Directory: puntos de conexión](ws-federation/_static/AadFederationMetadataDocument.png)

* Navegue hasta el nuevo registro de aplicación. Haga clic en **configuración** > **propiedades** y tome nota de la **App ID URI**. Se trata el middleware de WS-Federation `Wtrealm`:

![Azure Active Directory: Propiedades de registro de aplicación](ws-federation/_static/AadAppIdUri.png)

## <a name="add-ws-federation-as-an-external-login-provider-for-aspnet-core-identity"></a>Agregar WS-Federation como proveedor de inicio de sesión externo para ASP.NET Core Identity

* Agregue una dependencia en [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) al proyecto.
* Agregar WS-Federation para la `Configure` método *Startup.cs*:

    ```csharp
    services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

    services.AddAuthentication()
        .AddWsFederation(options =>
        {
            // MetadataAddress represents the Active Directory instance used to authenticate users.
            options.MetadataAddress = "https://<ADFS FQDN or AAD tenant>/FederationMetadata/2007-06/FederationMetadata.xml";

            // Wtrealm is the app's identifier in the Active Directory instance.
            // For ADFS, use the relying party's identifier, its WS-Federation Passive protocol URL:
            options.Wtrealm = "https://localhost:44307/";

            // For AAD, use the App ID URI from the app registration's Properties blade:
            options.Wtrealm = "https://wsfedsample.onmicrosoft.com/bf0e7e6d-056e-4e37-b9a6-2c36797b9f01";
        });

    services.AddMvc()
     // ...
    ```

[!INCLUDE[default settings configuration](social/includes/default-settings.md)]

### <a name="log-in-with-ws-federation"></a>Inicie sesión con WS-Federation

Vaya a la aplicación y haga clic en el **sesión** vínculo en el encabezado de navegación. Hay una opción para iniciar sesión con WsFederation: ![en la página de registro](ws-federation/_static/WsFederationButton.png)

Con AD FS como el proveedor, el botón se redirige a una página de inicio de sesión de AD FS: ![página de inicio de sesión de ADFS](ws-federation/_static/AdfsLoginPage.png)

Con Azure Active Directory como el proveedor, el botón se redirige a una página de inicio de sesión AAD: ![página de inicio de sesión AAD](ws-federation/_static/AadSignIn.png)

Un inicio de sesión correcto en un nuevo usuario redirige a la página de registro de usuario de la aplicación: ![página de registro](ws-federation/_static/Register.png)

## <a name="use-ws-federation-without-aspnet-core-identity"></a>Use WS-Federation sin identidad principal de ASP.NET

El middleware de WS-Federation se puede utilizar sin identidad. Por ejemplo:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddAuthentication(sharedOptions =>
    {
        sharedOptions.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
        sharedOptions.DefaultSignInScheme = CookieAuthenticationDefaults.AuthenticationScheme;
        sharedOptions.DefaultChallengeScheme = WsFederationDefaults.AuthenticationScheme;
    })
    .AddWsFederation(options =>
    {
        options.Wtrealm = Configuration["wsfed:realm"];
        options.MetadataAddress = Configuration["wsfed:metadata"];
    })
    .AddCookie();
}

public void Configure(IApplicationBuilder app)
{
    app.UseAuthentication();
        // …
}
```
