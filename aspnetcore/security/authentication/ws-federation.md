---
title: Autenticar a los usuarios con WS-Federation en ASP.NET Core
author: chlowell
description: Este tutorial muestra cómo utilizar WS-Federation en una aplicación ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 01/16/2019
uid: security/authentication/ws-federation
ms.openlocfilehash: 7967410686da0e59742b721c0154e143bf19ba01
ms.sourcegitcommit: 2c7ffe349eabdccf2ed748dd303ffd0ba6e1cfe3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/26/2019
ms.locfileid: "56833545"
---
# <a name="authenticate-users-with-ws-federation-in-aspnet-core"></a>Autenticar a los usuarios con WS-Federation en ASP.NET Core

Este tutorial muestra cómo habilitar usuarios iniciar sesión con un proveedor de autenticación de WS-Federation, como Active Directory Federation Services (ADFS) o [Azure Active Directory](/azure/active-directory/) (AAD). Usa la aplicación de ejemplo de ASP.NET Core 2.0 se describe en [Facebook, Google y la autenticación de proveedor externo](xref:security/authentication/social/index).

Para las aplicaciones de ASP.NET Core 2.0, proporciona compatibilidad con WS-Federation [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation). Este componente se porta desde [Microsoft.Owin.Security.WsFederation](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation) y comparte muchos de los mecanismos de dicho componente. Sin embargo, los componentes se diferencian en un par de formas importantes.

De forma predeterminada, el middleware nueva:

* No permitir inicios de sesión no solicitados. Esta característica del protocolo WS-Federation es vulnerable a ataques XSRF. Sin embargo, puede habilitarse con el `AllowUnsolicitedLogins` opción.
* No se comprueba cada formulario post para los mensajes de inicio de sesión. Solo las solicitudes a la `CallbackPath` se comprueba el inicio de sesión complementos. `CallbackPath` el valor predeterminado es `/signin-wsfed` , pero puede cambiarse a través de los heredados [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) propiedad de la [ WsFederationOptions](/dotnet/api/microsoft.aspnetcore.authentication.wsfederation.wsfederationoptions) clase. Esta ruta de acceso se puede compartir con otros proveedores de autenticación habilitando la [SkipUnrecognizedRequests](/dotnet/api/microsoft.aspnetcore.authentication.wsfederation.wsfederationoptions.skipunrecognizedrequests) opción.

## <a name="register-the-app-with-active-directory"></a>Registrar la aplicación con Active Directory

### <a name="active-directory-federation-services"></a>Servicios de federación de Active Directory

* Abra el servidor **para usuario autenticado entidad Asistente para agregar confianza** desde la consola de administración de AD FS:

![Agregar el Asistente para la relación de confianza para usuario autenticado: Pantalla de inicio](ws-federation/_static/AdfsAddTrust.png)

* Elija esta opción escribir manualmente los datos:

![Agregar el Asistente para la relación de confianza para usuario autenticado: Seleccionar origen de datos](ws-federation/_static/AdfsSelectDataSource.png)

* Escriba un nombre para mostrar para el usuario de confianza. El nombre no es importante para la aplicación de ASP.NET Core.

* [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) no es compatible con el cifrado de tokens, por lo que no configure un certificado de cifrado de tokens:

![Agregar el Asistente para la relación de confianza para usuario autenticado: Configurar certificado](ws-federation/_static/AdfsConfigureCert.png)

* Habilitar la compatibilidad de protocolo pasivo de WS-Federation, mediante la dirección URL de la aplicación. Compruebe que el puerto sea correcto para la aplicación:

![Agregar el Asistente para la relación de confianza para usuario autenticado: Configurar URL](ws-federation/_static/AdfsConfigureUrl.png)

> [!NOTE]
> Debe ser una dirección URL HTTPS. IIS Express se puede proporcionar un certificado autofirmado al hospedar la aplicación durante el desarrollo. Kestrel requiere configuración manual de certificados. Consulte la [documentación Kestrel](xref:fundamentals/servers/kestrel) para obtener más detalles.

* Haga clic en **siguiente** con el resto del asistente y **cerrar** al final.

* ASP.NET Core Identity requiere un **Id. de nombre** de notificación. Agregue uno de los **editar reglas de notificación** cuadro de diálogo:

![Editar reglas de notificación](ws-federation/_static/EditClaimRules.png)

* En el **transformar notificaciones Asistente para agregar regla**, deje el valor predeterminado **enviar atributos LDAP como notificaciones** plantilla seleccionada y haga clic en **siguiente**. Agregue una regla de asignación el **SAM-Account-Name** atributo LDAP para la **Id. de nombre** notificación saliente:

![Agregar el Asistente para reglas de notificación de transformación: Configurar regla de notificación](ws-federation/_static/AddTransformClaimRule.png)

* Haga clic en **finalizar** > **Aceptar** en el **editar reglas de notificación** ventana.

### <a name="azure-active-directory"></a>Azure Active Directory

* Vaya a la hoja de registros de aplicaciones del inquilino AAD. Haga clic en **nuevo registro de aplicaciones**:

![Azure Active Directory: Registros de aplicaciones](ws-federation/_static/AadNewAppRegistration.png)

* Escriba un nombre para el registro de aplicación. Esto no es importante para la aplicación de ASP.NET Core.
* Escriba la dirección URL de la aplicación escucha en el que el **dirección URL de inicio de sesión**:

![Azure Active Directory: Crear registro de aplicación](ws-federation/_static/AadCreateAppRegistration.png)

* Haga clic en **extremos** y tenga en cuenta la **documento de metadatos de federación** dirección URL. Se trata el middleware de WS-Federation `MetadataAddress`:

![Azure Active Directory: puntos de conexión](ws-federation/_static/AadFederationMetadataDocument.png)

* Navegue hasta el nuevo registro de aplicación. Haga clic en **configuración** > **propiedades** y tome nota de la **App ID URI**. Se trata el middleware de WS-Federation `Wtrealm`:

![Azure Active Directory: Propiedades de registro de aplicación](ws-federation/_static/AadAppIdUri.png)

## <a name="add-ws-federation-as-an-external-login-provider-for-aspnet-core-identity"></a>Adición de WS-Federation como proveedor de inicio de sesión externo para ASP.NET Core Identity

* Agregar una dependencia en [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) al proyecto.
* Agregue el WS-Federation a `Startup.ConfigureServices`:

    ```csharp
    services.AddIdentity<IdentityUser, IdentityRole>()
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

[!INCLUDE [default settings configuration](social/includes/default-settings.md)]

### <a name="log-in-with-ws-federation"></a>Inicie sesión con WS-Federation

Vaya a la aplicación y haga clic en el **iniciarla** vínculo en el encabezado de la barra de navegación. Hay una opción para iniciar sesión con WsFederation: ![Inicie sesión en la página](ws-federation/_static/WsFederationButton.png)

Con ADFS como proveedor, el botón se redirige a una página de inicio de sesión de AD FS: ![Página de inicio de sesión de ADFS](ws-federation/_static/AdfsLoginPage.png)

Con Azure Active Directory como proveedor, el botón se redirige a una página de inicio de sesión de AAD: ![Página de inicio de sesión de AAD](ws-federation/_static/AadSignIn.png)

Un inicio de sesión correcto para un nuevo usuario redirige a la página de registro de usuario de la aplicación: ![Página de registro](ws-federation/_static/Register.png)

## <a name="use-ws-federation-without-aspnet-core-identity"></a>Use WS-Federation sin ASP.NET Core Identity

El middleware de WS-Federation se puede usar sin la identidad. Por ejemplo:

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
