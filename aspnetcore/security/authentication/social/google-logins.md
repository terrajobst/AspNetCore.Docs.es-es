---
title: Configuración de inicio de sesión externo de Google en ASP.NET Core
author: rick-anderson
description: En este tutorial se muestra la integración de autenticación de usuario de la cuenta de Google en una aplicación de ASP.NET Core existente.
ms.author: riande
ms.custom: mvc
ms.date: 11/11/2018
uid: security/authentication/google-logins
ms.openlocfilehash: dfda83e1d7cf3c5ff8e31de20c15d468de5d15c0
ms.sourcegitcommit: 09bcda59a58019fdf47b2db5259fe87acf19dd38
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2018
ms.locfileid: "51708457"
---
# <a name="google-external-login-setup-in-aspnet-core"></a>Configuración de inicio de sesión externo de Google en ASP.NET Core

Por [Valeriy Novytskyy](https://github.com/01binary) y [Rick Anderson](https://twitter.com/RickAndMSFT)

Este tutorial muestra cómo permitir que los usuarios iniciar sesión con su cuenta de Google + mediante un proyecto de ASP.NET Core 2.0 de ejemplo creado en el [página anterior](xref:security/authentication/social/index). Comenzamos siguiendo el [pasos oficiales](https://developers.google.com/identity/sign-in/web/devconsole-project) para crear una nueva aplicación en la consola de API de Google.

## <a name="create-the-app-in-google-api-console"></a>Creación de la aplicación en la consola de API de Google

* Vaya a [ https://console.developers.google.com/projectselector/apis/library ](https://console.developers.google.com/projectselector/apis/library) e inicie sesión. Si no dispone de una cuenta de Google, use **más opciones** > **[crear cuenta](https://accounts.google.com/SignUpWithoutGmail?service=cloudconsole&continue=https%3A%2F%2Fconsole.developers.google.com%2Fprojectselector%2Fapis%2Flibrary&ltmpl=api)**  vínculo para crear uno:

![Consola de API de Google](index/_static/GoogleConsoleLogin.png)

* Se le redirigirá a **biblioteca API Manager** página:

![Página de la biblioteca del Administrador de API](index/_static/GoogleConsoleSwitchboard.png)

* Pulse **crear** y escriba su **nombre del proyecto**:

![Cuadro de diálogo Nuevo proyecto](index/_static/GoogleConsoleNewProj.png)

* Después de aceptar el cuadro de diálogo, se le redirigirá a la página de la biblioteca que le permite elegir las características de la nueva aplicación. Buscar **Google + API** en la lista y haga clic en el vínculo al agregar la característica de API:

![Página de la biblioteca del Administrador de API](index/_static/GoogleConsoleChooseApi.png)

* Se muestra la página de la API recién agregada. Pulse **habilitar** para agregar el inicio de sesión de Google + en función a la aplicación:

![Página Administrador de API de Google + API](index/_static/GoogleConsoleEnableApi.png)

* Después de habilitar la API, pulse **crear credenciales** para configurar los secretos:

![Página Administrador de API de Google + API](index/_static/GoogleConsoleGoCredentials.png)

* Elija:
  * **Google + API**
  * **Servidor Web (por ejemplo, node.js, Tomcat)**, y
  * **Datos de usuario**:

![Página credenciales de administrador de API: Descubra de qué tipo de credenciales necesita el panel](index/_static/GoogleConsoleChooseCred.png)

* Pulse **las credenciales que es necesario?** que le lleva al segundo paso de configuración de la aplicación, **crear un identificador de cliente de OAuth 2.0**:

![Página credenciales de administrador de API: crear un identificador de cliente de OAuth 2.0](index/_static/GoogleConsoleCreateClient.png)

* Dado que vamos a crear un proyecto de Google + con una sola característica (inicio de sesión), podemos escribir el mismo **nombre** para el identificador de cliente de OAuth 2.0 que se usan para el proyecto.

* Escriba el URI de desarrollo con `/signin-google` anexado a la **URI de redireccionamiento autorizado** campo (por ejemplo: `https://localhost:44320/signin-google`). La autenticación de Google configurada más adelante en este tutorial controlará automáticamente las solicitudes en `/signin-google` ruta para implementar el flujo de OAuth.

> [!NOTE]
> El segmento del URI `/signin-google` se establece como la devolución de llamada predeterminada del proveedor de autenticación de Google. Puede cambiar el URI de devolución de forma predeterminada al configurar el middleware de autenticación de Google a través de los heredados [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) propiedad de la [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) clase.

* Presione la tecla TAB para agregar la **URI de redireccionamiento autorizado** entrada.

* Pulse **crear Id. de cliente**, que le lleva al tercer paso, **configurar la pantalla de consentimiento de OAuth 2.0**:

![Página credenciales de administrador de API: configuración de la pantalla de consentimiento de OAuth 2.0](index/_static/GoogleConsoleAddCred.png)

* Escriba su orientados al público **dirección de correo electrónico** y **nombre de producto** que se muestra para la aplicación cuando Google + pide al usuario que inicie sesión en. Opciones adicionales están disponibles en **más opciones de personalización**.

* Pulse **continuar** para continuar con el último paso, **descargar credenciales**:

![Página credenciales de administrador de API: descargar credenciales](index/_static/GoogleConsoleFinish.png)

* Pulse **descargar** para guardar un archivo JSON con los secretos de aplicación, y **realiza** para completar la creación de la nueva aplicación.

* Al implementar el sitio, deberá volver a visitar la **Google Console** y registrar una nueva dirección url pública.

## <a name="store-google-clientid-and-clientsecret"></a>Store Google ClientID y ClientSecret

Vincular configuración confidencial, como Google `Client ID` y `Client Secret` para la configuración de aplicación mediante el [Secret Manager](xref:security/app-secrets). Para los fines de este tutorial, asigne el nombre de los tokens `Authentication:Google:ClientId` y `Authentication:Google:ClientSecret`.

Los valores para estos tokens pueden encontrarse en el archivo JSON descargado en el paso anterior en `web.client_id` y `web.client_secret`.

## <a name="configure-google-authentication"></a>Configurar la autenticación de Google

::: moniker range=">= aspnetcore-2.0"

Agregue el servicio de Google en el `ConfigureServices` método *Startup.cs* archivo:

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddGoogle(googleOptions =>
{
    googleOptions.ClientId = Configuration["Authentication:Google:ClientId"];
    googleOptions.ClientSecret = Configuration["Authentication:Google:ClientSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

La plantilla de proyecto que se usa en este tutorial garantiza que [Microsoft.AspNetCore.Authentication.Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google) está instalado el paquete.

* Para instalar este paquete con Visual Studio 2017, haga doble clic en el proyecto y seleccione **administrar paquetes de NuGet**.
* Para instalar con la CLI de .NET Core, ejecute lo siguiente en el directorio del proyecto:

`dotnet add package Microsoft.AspNetCore.Authentication.Google`

Agregue el middleware de Google en el `Configure` método *Startup.cs* archivo:

```csharp
app.UseGoogleAuthentication(new GoogleOptions()
{
    ClientId = Configuration["Authentication:Google:ClientId"],
    ClientSecret = Configuration["Authentication:Google:ClientSecret"]
});
```

::: moniker-end

Consulte la [GoogleOptions](/dotnet/api/microsoft.aspnetcore.builder.googleoptions) referencia de API para obtener más información sobre las opciones de configuración compatible con la autenticación de Google. Esto puede utilizarse para solicitar información diferente sobre el usuario.

## <a name="sign-in-with-google"></a>Inicie sesión con Google

Ejecute la aplicación y haga clic en **inicie sesión**. Aparece una opción para iniciar sesión con Google:

![Aplicación Web que se ejecutan en Microsoft Edge: usuario no autenticado](index/_static/DoneGoogle.png)

Al hacer clic en Google, se le redirigirá a Google para la autenticación:

![Cuadro de diálogo de autenticación de Google](index/_static/GoogleLogin.png)

Después de escribir sus credenciales de Google, a continuación, se le redirigirá al sitio web donde puede establecer su correo electrónico.

Ha iniciado sesión con sus credenciales de Google:

![Aplicación Web que se ejecutan en Microsoft Edge: usuario autenticado](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>Solución de problemas

* Si recibe un `403 (Forbidden)` página de error desde su propia aplicación al que se ejecute en modo de desarrollo (o interrumpir el depurador con el mismo error), asegúrese de que **Google + API** se ha habilitado en el **biblioteca del Administrador de la API** siguiendo los pasos enumerados [anteriores en esta página](#create-the-app-in-google-api-console). Si no funciona el inicio de sesión y no recibe algún error, cambie al modo de desarrollo para hacer más fácil de depurar el problema.
* **ASP.NET Core 2.x solo:** identidad si no está configurado mediante una llamada a `services.AddIdentity` en `ConfigureServices`, intentando autenticarse producirá *ArgumentException: se debe proporcionar la opción 'SignInScheme'*. La plantilla de proyecto que se usa en este tutorial, se garantiza que esto se realiza.
* Si la base de datos de sitio no se ha creado aplicando a la migración inicial, obtendrá *error en una operación de base de datos al procesar la solicitud* error. Pulse **aplicar migraciones** para crear la base de datos y actualizar para continuar más allá del error.

## <a name="next-steps"></a>Pasos siguientes

* Este artículo, mostramos cómo puede autenticar con Google. Puede seguir un enfoque similar para autenticar con otros proveedores que se enumeran en la [página anterior](xref:security/authentication/social/index).

* Una vez que publique su sitio web a la aplicación web de Azure, debe restablecer el `ClientSecret` en la consola de API de Google.

* Establecer el `Authentication:Google:ClientId` y `Authentication:Google:ClientSecret` como configuración de la aplicación en Azure portal. El sistema de configuración está configurado para leer las claves de las variables de entorno.
