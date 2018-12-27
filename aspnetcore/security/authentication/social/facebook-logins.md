---
title: Configuración de inicio de sesión externo de Facebook en ASP.NET Core
author: rick-anderson
description: En este tutorial se muestra la integración de autenticación de usuario de la cuenta de Facebook en una aplicación de ASP.NET Core existente.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 12/18/2018
uid: security/authentication/facebook-logins
ms.openlocfilehash: 66f895c7c8dcc00d991c0ea57535f2ed56431a77
ms.sourcegitcommit: 3e94d192b2ed9409fe72e3735e158b333354964c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/21/2018
ms.locfileid: "53735783"
---
# <a name="facebook-external-login-setup-in-aspnet-core"></a>Configuración de inicio de sesión externo de Facebook en ASP.NET Core

Por [Valeriy Novytskyy](https://github.com/01binary) y [Rick Anderson](https://twitter.com/RickAndMSFT)

Este tutorial muestra cómo permitir que los usuarios iniciar sesión con su cuenta de Facebook con un proyecto de ASP.NET Core 2.0 de ejemplo creado en el [página anterior](xref:security/authentication/social/index). Empezamos creando un App identificador Facebook siguiendo el [pasos oficiales](https://developers.facebook.com).

## <a name="create-the-app-in-facebook"></a>Crear la aplicación en Facebook

* Navegue hasta la [app de desarrolladores de Facebook](https://developers.facebook.com/apps/) página e inicie sesión. Si no dispone de una cuenta de Facebook, use el **suscribirse a Facebook** vínculo en la página de inicio de sesión para crear uno.

* Pulse el **agregar una nueva aplicación** botón en la esquina superior derecha para crear un nuevo identificador de aplicación.

   ![Facebook para el portal de desarrolladores abierta en Microsoft Edge](index/_static/FBMyApps.png)

* Rellene el formulario y pulse el **crear Id. de aplicación** botón.

  ![Crear un formulario nuevo identificador de aplicación](index/_static/FBNewAppId.png)

* En el **seleccionar un producto** página, haga clic en **Set Up** en el **inicio de sesión de Facebook** tarjeta.

  ![Página de instalación del producto](index/_static/FBProductSetup.png)

* El **Quickstart** se iniciará con **elija una plataforma** como la primera página. Omitir el asistente por ahora, haga clic en el **configuración** vínculo en el menú de la izquierda:

  ![Inicio rápido de omisión](index/_static/FBSkipQuickStart.png)

* Se le presentará la **configuración de cliente OAuth** página:

  ![Página de configuración de OAuth de cliente](index/_static/FBOAuthSetup.png)

* Escriba el URI de desarrollo con */signin-facebook* anexado a la **URI de redirección de OAuth válido** campo (por ejemplo: `https://localhost:44320/signin-facebook`). La autenticación de Facebook configurada más adelante en este tutorial controlará automáticamente las solicitudes en */signin-facebook* ruta para implementar el flujo de OAuth.

> [!NOTE]
> El URI */signin-facebook* se establece como la devolución de llamada predeterminada del proveedor de autenticación de Facebook. Puede cambiar el URI de devolución de forma predeterminada al configurar el middleware de autenticación de Facebook a través de los heredados [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) propiedad de la [FacebookOptions](/dotnet/api/microsoft.aspnetcore.authentication.facebook.facebookoptions) clase.

* Haga clic en **guardar cambios**.

* Haga clic en **configuración** > **básica** vínculo en el panel de navegación izquierdo.

  En esta página, tome nota de su `App ID` y su `App Secret`. En la siguiente sección, va a agregar tanto en la aplicación de ASP.NET Core:

* Al implementar el sitio tiene que volver a visitar la **inicio de sesión de Facebook** página de la instalación y registrar un nuevo URI público.

## <a name="store-facebook-app-id-and-app-secret"></a>Store Id. de aplicación de Facebook y secreto de la aplicación

Vincular configuración confidencial, como Facebook `App ID` y `App Secret` para la configuración de aplicación mediante el [Secret Manager](xref:security/app-secrets). Para los fines de este tutorial, asigne el nombre de los tokens `Authentication:Facebook:AppId` y `Authentication:Facebook:AppSecret`.

Ejecute los comandos siguientes para almacenar de forma segura `App ID` y `App Secret` con Secret Manager:

```console
dotnet user-secrets set Authentication:Facebook:AppId <app-id>
dotnet user-secrets set Authentication:Facebook:AppSecret <app-secret>
```

## <a name="configure-facebook-authentication"></a>Configurar la autenticación de Facebook

::: moniker range=">= aspnetcore-2.0"

Agregue el servicio de Facebook en la `ConfigureServices` método en el *Startup.cs* archivo:

```csharp
services.AddDefaultIdentity<IdentityUser>()
        .AddDefaultUI(UIFramework.Bootstrap4)
        .AddEntityFrameworkStores<ApplicationDbContext>();

services.AddAuthentication().AddFacebook(facebookOptions =>
{
    facebookOptions.AppId = Configuration["Authentication:Facebook:AppId"];
    facebookOptions.AppSecret = Configuration["Authentication:Facebook:AppSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Instalar el [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) paquete.

* Para instalar este paquete con Visual Studio 2017, haga doble clic en el proyecto y seleccione **administrar paquetes de NuGet**.
* Para instalar con la CLI de .NET Core, ejecute lo siguiente en el directorio del proyecto:

   `dotnet add package Microsoft.AspNetCore.Authentication.Facebook`

Agregue el middleware de Facebook en la `Configure` método *Startup.cs* archivo:

```csharp
app.UseFacebookAuthentication(new FacebookOptions()
{
    AppId = Configuration["Authentication:Facebook:AppId"],
    AppSecret = Configuration["Authentication:Facebook:AppSecret"]
});
```

::: moniker-end

Consulte la [FacebookOptions](/dotnet/api/microsoft.aspnetcore.builder.facebookoptions) referencia de API para obtener más información sobre las opciones de configuración compatible con la autenticación de Facebook. Las opciones de configuración pueden utilizarse para:

* Solicitar información diferente sobre el usuario.
* Agregue los argumentos de cadena de consulta para personalizar la experiencia de inicio de sesión.

## <a name="sign-in-with-facebook"></a>Inicie sesión con Facebook

Ejecute la aplicación y haga clic en **inicie sesión**. Verá una opción para iniciar sesión con Facebook.

![Aplicación Web: Usuario no autenticado](index/_static/DoneFacebook.png)

Al hacer clic en **Facebook**, se le redirigirá a Facebook para la autenticación:

![Página de autenticación de Facebook](index/_static/FBLogin.png)

Dirección pública de perfil y correo electrónico de solicitudes de autenticación de Facebook de forma predeterminada:

![Pantalla de consentimiento de página de autenticación de Facebook](index/_static/FBLoginDone.png)

Una vez que escriba sus credenciales de Facebook se redirigen a su sitio donde puede establecer su correo electrónico.

Ha iniciado sesión con sus credenciales de Facebook:

![Aplicación Web: Usuario autenticado](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>Solución de problemas

* **ASP.NET Core 2.x solo:** Si la identidad no está configurada mediante una llamada a `services.AddIdentity` en `ConfigureServices`, intentando autenticarse producirá *ArgumentException: Se debe proporcionar la opción 'SignInScheme'*. La plantilla de proyecto que se usa en este tutorial, se garantiza que esto se realiza.
* Si la base de datos de sitio no se ha creado aplicando a la migración inicial, obtendrá *error en una operación de base de datos al procesar la solicitud* error. Pulse **aplicar migraciones** para crear la base de datos y actualizar para continuar más allá del error.

## <a name="next-steps"></a>Pasos siguientes

* Agregar el [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) paquete NuGet al proyecto para escenarios avanzados de autenticación de Facebook. Este paquete no es necesario integrar la funcionalidad de inicio de sesión externo de Facebook con su aplicación. 

* Este artículo, mostramos cómo puede autenticar con Facebook. Puede seguir un enfoque similar para autenticar con otros proveedores que se enumeran en la [página anterior](xref:security/authentication/social/index).

* Una vez que publique su sitio web a la aplicación web de Azure, debe restablecer el `AppSecret` en el portal para desarrolladores de Facebook.

* Establecer el `Authentication:Facebook:AppId` y `Authentication:Facebook:AppSecret` como configuración de la aplicación en Azure portal. El sistema de configuración está configurado para leer las claves de las variables de entorno.
