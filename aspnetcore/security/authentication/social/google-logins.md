---
title: Programa de instalación de inicio de sesión externo de Google en ASP.NET Core
author: rick-anderson
description: Este tutorial muestra la integración de autenticación de usuario de cuenta de Google en una aplicación existente de ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 08/02/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/google-logins
ms.openlocfilehash: aba12a94a573db35eadaa6a38f2fcf074b7b64c2
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/03/2018
---
# <a name="google-external-login-setup-in-aspnet-core"></a>Programa de instalación de inicio de sesión externo de Google en ASP.NET Core

Por [Valeriy Novytskyy](https://github.com/01binary) y [Rick Anderson](https://twitter.com/RickAndMSFT)

Este tutorial muestra cómo permitir a los usuarios iniciar sesión con su cuenta de Google + usando un proyecto de ASP.NET Core 2.0 de ejemplo creado en el [página anterior](xref:security/authentication/social/index). Comenzamos siguiendo el [pasos oficiales](https://developers.google.com/identity/sign-in/web/devconsole-project) para crear una nueva aplicación de consola de API de Google.

## <a name="create-the-app-in-google-api-console"></a>Crear la aplicación de consola de API de Google

* Vaya a [ https://console.developers.google.com/projectselector/apis/library ](https://console.developers.google.com/projectselector/apis/library) e inicie sesión. Si ya no tiene una cuenta de Google, use **más opciones** > **[crear cuenta](https://accounts.google.com/SignUpWithoutGmail?service=cloudconsole&continue=https%3A%2F%2Fconsole.developers.google.com%2Fprojectselector%2Fapis%2Flibrary&ltmpl=api)**  vínculo para crear una:

![Consola de API de Google](index/_static/GoogleConsoleLogin.png)

* Se le redirigirá a **biblioteca API Manager** página:

![Página de la biblioteca de API de administrador](index/_static/GoogleConsoleSwitchboard.png)

* Pulse **crear** y escriba su **nombre del proyecto**:

![Cuadro de diálogo Nuevo proyecto](index/_static/GoogleConsoleNewProj.png)

* Después de aceptar el cuadro de diálogo, se le redirigirá a la página de la biblioteca que le permite elegir las características de la aplicación nuevo. Buscar **API de Google +** en la lista y haga clic en el vínculo para agregar la característica de API:

![Página de la biblioteca de API de administrador](index/_static/GoogleConsoleChooseApi.png)

* Se muestra la página de la API recién agregada. Pulse **habilitar** para agregar inicio de sesión de Google + en la característica a la aplicación:

![Página de Google + API del Administrador de API](index/_static/GoogleConsoleEnableApi.png)

* Después de habilitar la API, pulse **crear credenciales** para configurar los secretos:

![Página de Google + API del Administrador de API](index/_static/GoogleConsoleGoCredentials.png)

* Elija:
   * **API de Google +**
   * **Servidor Web (por ejemplo, node.js, Tomcat)**, y
   * **Datos de usuario**:

![Página credenciales de administrador de APIs: Obtenga más información sobre qué tipo de credenciales necesita el panel](index/_static/GoogleConsoleChooseCred.png)

* Pulse **las credenciales que es necesario?** lo que irá al segundo paso de configuración de la aplicación, **crear un identificador de cliente de OAuth 2.0**:

![Página credenciales de administrador de APIs: crear un identificador de cliente de OAuth 2.0](index/_static/GoogleConsoleCreateClient.png)

* Dado que vamos a crear un proyecto de Google + con una sola característica (inicio de sesión), podemos escribir el mismo **nombre** para el identificador de cliente de OAuth 2.0 que la que se utilizará para el proyecto.

* Escriba el URI de desarrollo con */signin-google* anexan a la **URI de redireccionamiento autorizados** campo (por ejemplo: `https://localhost:44320/signin-google`). La autenticación de Google configurada más adelante en este tutorial controlará automáticamente las solicitudes en */signin-google* ruta para implementar el flujo de OAuth.

* Presione la tecla TAB para agregar el **URI de redireccionamiento autorizados** entrada.

* Pulse **crear ID de cliente**, lo que irá con el tercer paso: **configurar la pantalla de consentimiento de OAuth 2.0**:

![Página credenciales de administrador de APIs: configurar la pantalla de consentimiento de OAuth 2.0](index/_static/GoogleConsoleAddCred.png)

* Escriba el acceso público **dirección de correo electrónico** y **nombre de producto** se muestra para la aplicación cuando Google + pide al usuario que inicie sesión en. Hay opciones adicionales disponibles en **más opciones de personalización**.

* Pulse **continuar** para continuar con el último paso, **descargar credenciales**:

![Página credenciales de administrador de APIs: descargar credenciales](index/_static/GoogleConsoleFinish.png)

* Pulse **descargar** para guardar un archivo JSON con secretos de aplicación, y **realiza** para completar la creación de la nueva aplicación.

* Al implementar el sitio que necesite volver a visitar el **consola de Google** y registrar una nueva dirección url pública.

## <a name="store-google-clientid-and-clientsecret"></a>Almacén Google ClientID y ClientSecret

Vincular valores confidenciales como Google `Client ID` y `Client Secret` a su configuración de aplicación con el [secreto Manager](xref:security/app-secrets). Para los fines de este tutorial, nombre de los tokens `Authentication:Google:ClientId` y `Authentication:Google:ClientSecret`.

Los valores para estos tokens se pueden encontrar en el archivo JSON que descargó en el paso anterior en `web.client_id` y `web.client_secret`.

## <a name="configure-google-authentication"></a>Configurar la autenticación de Google

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)
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

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)
La plantilla de proyecto que se usan en este tutorial asegura de que [Microsoft.AspNetCore.Authentication.Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google) paquete está instalado.

 * Para instalar este paquete con 2017 de Visual Studio, haga doble clic en el proyecto y seleccione **administrar paquetes de NuGet**.
 * Para instalar con CLI de .NET Core, ejecute lo siguiente en el directorio del proyecto:

   `dotnet add package Microsoft.AspNetCore.Authentication.Google`

Agregar el middleware de Google en el `Configure` método *Startup.cs* archivo:

```csharp
app.UseGoogleAuthentication(new GoogleOptions()
{
    ClientId = Configuration["Authentication:Google:ClientId"],
    ClientSecret = Configuration["Authentication:Google:ClientSecret"]
});
```

* * *
Consulte la [GoogleOptions](/dotnet/api/microsoft.aspnetcore.builder.googleoptions) referencia de API para obtener más información sobre las opciones de configuración compatible con autenticación de Google. Esto se puede usar para solicitar información diferente sobre el usuario.

## <a name="sign-in-with-google"></a>Inicie sesión con Google

Ejecute la aplicación y haga clic en **sesión**. Aparece una opción para iniciar sesión con Google:

![Aplicación Web que se ejecuta en Microsoft Edge: usuario no autenticado](index/_static/DoneGoogle.png)

Al hacer clic en Google, se le redirigirá a Google para la autenticación:

![Cuadro de diálogo de autenticación de Google](index/_static/GoogleLogin.png)

Después de escribir sus credenciales de Google, a continuación, se le redirigirá al sitio web donde puede establecer el correo electrónico.

Ahora que haya iniciado sesión con sus credenciales de Google:

![Aplicación Web que se ejecuta en Microsoft Edge: usuario autenticado](index/_static/Done.png)

## <a name="troubleshooting"></a>Solución de problemas

* Si recibe un `403 (Forbidden)` página de error de la propia aplicación cuando se ejecuta en modo de desarrollo (o interrupción en el depurador con el mismo error), asegúrese de que **API de Google +** se ha habilitado en el **biblioteca del Administrador de la API** siguiendo los pasos enumerados [anteriores en esta página](#create-the-app-in-google-api-console). Si el inicio de sesión no funciona y no reciben los errores, cambie al modo de desarrollo para que sea más fácil depurar el problema.
* **ASP.NET Core solo 2.x:** identidad si no está configurado mediante una llamada a `services.AddIdentity` en `ConfigureServices`, intenta autenticar se producirá en *ArgumentException: se debe proporcionar la opción 'SignInScheme'*. La plantilla de proyecto que se usan en este tutorial se asegura de que esto se realiza.
* Si la base de datos de sitio no se ha creado mediante la aplicación de la migración inicial, obtendrá *error en una operación de base de datos al procesar la solicitud* error. Pulse **migraciones aplicar** para crear la base de datos y actualizar para continuar después del error.

## <a name="next-steps"></a>Pasos siguientes

* En este artículo se ha explicado cómo puede autenticar con Google. Puede seguir un enfoque similar para autenticar con otros proveedores que se enumeran en la [página anterior](xref:security/authentication/social/index).

* Una vez que se publica un sitio web a la aplicación web de Azure, debe restablecer la `ClientSecret` en la consola de API de Google.

* Establecer el `Authentication:Google:ClientId` y `Authentication:Google:ClientSecret` como configuración de la aplicación en el portal de Azure. El sistema de configuración está configurado para leer las claves de las variables de entorno.
