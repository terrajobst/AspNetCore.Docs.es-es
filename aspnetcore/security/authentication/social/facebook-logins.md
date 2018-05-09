---
title: Programa de instalación de inicio de sesión externo de Facebook en ASP.NET Core
author: rick-anderson
description: Este tutorial muestra la integración de autenticación de usuario de cuenta de Facebook en una aplicación existente de ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 08/01/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/facebook-logins
ms.openlocfilehash: 19e862ef01655b24ba4d323b8f5f012de1455424
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/08/2018
---
# <a name="facebook-external-login-setup-in-aspnet-core"></a>Programa de instalación de inicio de sesión externo de Facebook en ASP.NET Core

Por [Valeriy Novytskyy](https://github.com/01binary) y [Rick Anderson](https://twitter.com/RickAndMSFT)

Este tutorial muestra cómo permitir a los usuarios iniciar sesión con su cuenta de Facebook mediante un proyecto de ASP.NET Core 2.0 de ejemplo creado en el [página anterior](xref:security/authentication/social/index). Requiere la autenticación de Facebook el [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) paquete NuGet. Comenzamos creando un Facebook App ID siguiendo el [pasos oficiales](https://developers.facebook.com).

## <a name="create-the-app-in-facebook"></a>Crear la aplicación de Facebook

*  Navegue hasta la [aplicación de desarrolladores de Facebook](https://developers.facebook.com/apps/) página e inicie sesión. Si ya no tiene una cuenta de Facebook, use la **registrarse para Facebook** vínculo en la página de inicio de sesión para crear uno.

* Pulse la **agregar una nueva aplicación** botón en la esquina superior derecha para crear un nuevo identificador de aplicación.

   ![Facebook para portal de desarrolladores de abrir con Microsoft Edge](index/_static/FBMyApps.png)

* Rellene el formulario y pulse el **crear Id. de aplicación** botón.

   ![Crear un formulario nuevo Id. de aplicación](index/_static/FBNewAppId.png)

* En el **seleccionar un producto** página, haga clic en **Set Up** en el **inicio de sesión de Facebook** tarjeta.

   ![Página de instalación del producto](index/_static/FBProductSetup.png)

* El **inicio rápido** asistente se iniciará con **elegir una plataforma** como la primera página. Omitir el asistente por ahora, haga clic en el **configuración** vínculo en el menú de la izquierda:

   ![Inicio rápido de Skip](index/_static/FBSkipQuickStart.png)

* Se le presentará la **configuración de cliente OAuth** página:

![Página de configuración de OAuth de cliente](index/_static/FBOAuthSetup.png)

* Escriba el URI de desarrollo con */signin-facebook* anexan a la **válido URI de redireccionamiento de OAuth** campo (por ejemplo: `https://localhost:44320/signin-facebook`). La autenticación de Facebook configurada más adelante en este tutorial controlará automáticamente las solicitudes en */signin-facebook* ruta para implementar el flujo de OAuth.

* Haga clic en **guardar cambios**.

* Haga clic en el **panel** vínculo en el panel de navegación izquierdo. 

    En esta página, tome nota de su `App ID` y su `App Secret`. Tanto en la aplicación de ASP.NET Core va a agregar en la sección siguiente:

   ![Panel para desarrolladores de Facebook](index/_static/FBDashboard.png)

* Al implementar el sitio debe volver a visitar el **inicio de sesión de Facebook** página de la instalación y registrar un nuevo URI público.

## <a name="store-facebook-app-id-and-app-secret"></a>Almacenar identificador de la aplicación de Facebook y secreto de la aplicación

Vincular valores confidenciales como Facebook `App ID` y `App Secret` a su configuración de aplicación con el [secreto Manager](xref:security/app-secrets). Para los fines de este tutorial, nombre de los tokens `Authentication:Facebook:AppId` y `Authentication:Facebook:AppSecret`.

Ejecute los comandos siguientes para almacenar de forma segura `App ID` y `App Secret` con el Administrador de secreto:

```console
dotnet user-secrets set Authentication:Facebook:AppId <app-id>
dotnet user-secrets set Authentication:Facebook:AppSecret <app-secret>
```

## <a name="configure-facebook-authentication"></a>Configurar la autenticación de Facebook

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)
Agregue el servicio de Facebook en la `ConfigureServices` método en el *Startup.cs* archivo:

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddFacebook(facebookOptions =>
{
    facebookOptions.AppId = Configuration["Authentication:Facebook:AppId"];
    facebookOptions.AppSecret = Configuration["Authentication:Facebook:AppSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)
Instalar el [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) paquete.

* Para instalar este paquete con 2017 de Visual Studio, haga doble clic en el proyecto y seleccione **administrar paquetes de NuGet**.
* Para instalar con CLI de .NET Core, ejecute lo siguiente en el directorio del proyecto:

   `dotnet add package Microsoft.AspNetCore.Authentication.Facebook`

Agregar el middleware de Facebook en la `Configure` método *Startup.cs* archivo:

```csharp
app.UseFacebookAuthentication(new FacebookOptions()
{
    AppId = Configuration["Authentication:Facebook:AppId"],
    AppSecret = Configuration["Authentication:Facebook:AppSecret"]
});
```

* * *
Consulte la [FacebookOptions](/dotnet/api/microsoft.aspnetcore.builder.facebookoptions) referencia de API para obtener más información sobre las opciones de configuración compatible con la autenticación de Facebook. Opciones de configuración pueden utilizarse para:

* Solicitar información diferente sobre el usuario.
* Agregue los argumentos de cadena de consulta para personalizar la experiencia de inicio de sesión.

## <a name="sign-in-with-facebook"></a>Inicie sesión con Facebook

Ejecute la aplicación y haga clic en **sesión**. Verá una opción para iniciar sesión con Facebook.

![Aplicación Web: usuario no autenticado](index/_static/DoneFacebook.png)

Al hacer clic en **Facebook**, se le redirigirá a Facebook para la autenticación:

![Página de autenticación de Facebook](index/_static/FBLogin.png)

Las solicitudes de autenticación de Facebook dirección pública de perfil y el correo electrónico de forma predeterminada:

![Página de autenticación de Facebook](index/_static/FBLoginDone.png)

Una vez que escriba sus credenciales de Facebook que se le redirigirá al sitio donde puede establecer el correo electrónico.

Ahora que haya iniciado sesión con sus credenciales de Facebook:

![Aplicación Web: usuario autenticado](index/_static/Done.png)

## <a name="troubleshooting"></a>Solución de problemas

* **ASP.NET Core solo 2.x:** identidad si no está configurado mediante una llamada a `services.AddIdentity` en `ConfigureServices`, intenta autenticar se producirá en *ArgumentException: se debe proporcionar la opción 'SignInScheme'*. La plantilla de proyecto que se usan en este tutorial se asegura de que esto se realiza.
* Si la base de datos de sitio no se ha creado mediante la aplicación de la migración inicial, obtendrá *error en una operación de base de datos al procesar la solicitud* error. Pulse **migraciones aplicar** para crear la base de datos y actualizar para continuar después del error.

## <a name="next-steps"></a>Pasos siguientes

* En este artículo se ha explicado cómo puede autenticar con Facebook. Puede seguir un enfoque similar para autenticar con otros proveedores que se enumeran en la [página anterior](xref:security/authentication/social/index).

* Una vez que se publica un sitio web a la aplicación web de Azure, debe restablecer la `AppSecret` en el portal para desarrolladores de Facebook.

* Establecer el `Authentication:Facebook:AppId` y `Authentication:Facebook:AppSecret` como configuración de la aplicación en el portal de Azure. El sistema de configuración está configurado para leer las claves de las variables de entorno.
