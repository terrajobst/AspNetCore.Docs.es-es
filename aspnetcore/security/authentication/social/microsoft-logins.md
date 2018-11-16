---
title: Configuración de inicio de sesión externo Account de Microsoft con ASP.NET Core
author: rick-anderson
description: En este tutorial se muestra la integración de autenticación de usuario de la cuenta de Microsoft en una aplicación de ASP.NET Core existente.
ms.author: riande
ms.custom: mvc
ms.date: 11/11/2018
uid: security/authentication/microsoft-logins
ms.openlocfilehash: 89969370cea66b7b6632f1b0be59e135767c831e
ms.sourcegitcommit: 09bcda59a58019fdf47b2db5259fe87acf19dd38
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2018
ms.locfileid: "51708405"
---
# <a name="microsoft-account-external-login-setup-with-aspnet-core"></a>Configuración de inicio de sesión externo Account de Microsoft con ASP.NET Core

Por [Valeriy Novytskyy](https://github.com/01binary) y [Rick Anderson](https://twitter.com/RickAndMSFT)

Este tutorial muestra cómo permitir que los usuarios iniciar sesión con su cuenta de Microsoft mediante un proyecto de ASP.NET Core 2.0 de ejemplo creado en el [página anterior](xref:security/authentication/social/index).

## <a name="create-the-app-in-microsoft-developer-portal"></a>Crear la aplicación en el Portal para desarrolladores de Microsoft

* Vaya a [ https://apps.dev.microsoft.com ](https://apps.dev.microsoft.com) y crear o iniciar sesión en una cuenta de Microsoft:

![Inicie sesión en el cuadro de diálogo](index/_static/MicrosoftDevLogin.png)

Si aún no tiene una cuenta de Microsoft, pulse  **[crear uno!](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=13&ct=1478151035&rver=6.7.6643.0&wp=SAPI_LONG&wreply=https%3a%2f%2fapps.dev.microsoft.com%2fLoginPostBack&id=293053&aadredir=1&contextid=D70D4F21246BAB50&bk=1478151036&uiflavor=web&uaid=f0c3de863a914c358b8dc01b1ff49e85&mkt=EN-US&lc=1033&lic=1)** Después de iniciar sesión se le redirigirá a **mis aplicaciones** página:

![Portal para desarrolladores de Microsoft abierto en Microsoft Edge](index/_static/MicrosoftDev.png)

* Pulse **agregar una aplicación** en la parte superior derecha esquina y escriba su **nombre de la aplicación** y **correo electrónico de contacto**:

![Cuadro de diálogo nuevo registro de la aplicación](index/_static/MicrosoftDevAppCreate.png)

* Para los fines de este tutorial, desactive la **configuración paso a paso** casilla de verificación.

* Pulse **crear** para continuar la **registro** página. Proporcionar un **nombre** y anote el valor de la **Id. de aplicación**, que usan como `ClientId` más adelante en el tutorial:

![Página de registro](index/_static/MicrosoftDevAppReg.png)

* Pulse **Agregar plataforma** en el **plataformas** sección y seleccione el **Web** plataforma:

![Agregar cuadro de diálogo plataforma](index/_static/MicrosoftDevAppPlatform.png)

* En el nuevo **Web** plataforma sección, escriba la dirección URL de desarrollo con `/signin-microsoft` anexado a la **URL de redirección** campo (por ejemplo: `https://localhost:44320/signin-microsoft`). El esquema de autenticación de Microsoft configurado más adelante en este tutorial controlará automáticamente las solicitudes en `/signin-microsoft` ruta para implementar el flujo de OAuth:

![Sección de plataforma Web](index/_static/MicrosoftRedirectUri.png)

> [!NOTE]
> El segmento del URI `/signin-microsoft` se establece como la devolución de llamada predeterminada del proveedor de autenticación de Microsoft. Puede cambiar el URI de devolución de forma predeterminada al configurar el middleware de autenticación de Microsoft a través de los heredados [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) propiedad de la [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) clase.

* Pulse **agregar dirección URL** para asegurarse de que se ha agregado la dirección URL.

* Complete otras configuraciones de aplicaciones si es necesario y pulse **guardar** en la parte inferior de la página para guardar los cambios en la configuración de la aplicación.

* Al implementar el sitio, deberá volver a visitar la **registro** página y establecer una nueva dirección URL pública.

## <a name="store-microsoft-application-id-and-password"></a>Store Id. de aplicación de Microsoft y la contraseña

* Tenga en cuenta la `Application Id` muestra en el **registro** página.

* Pulse **generar nueva contraseña** en el **secretos de aplicación** sección. Esto muestra un cuadro donde puede copiar la contraseña de aplicación:

![Cuadro de diálogo nueva contraseña generada](index/_static/MicrosoftDevPassword.png)

Vincular configuración confidencial, como Microsoft `Application ID` y `Password` para la configuración de aplicación mediante el [Secret Manager](xref:security/app-secrets). Para los fines de este tutorial, asigne el nombre de los tokens `Authentication:Microsoft:ApplicationId` y `Authentication:Microsoft:Password`.

## <a name="configure-microsoft-account-authentication"></a>Configurar la autenticación de la cuenta de Microsoft

La plantilla de proyecto que se usa en este tutorial garantiza que [Microsoft.AspNetCore.Authentication.MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount) paquete ya está instalado.

* Para instalar este paquete con Visual Studio 2017, haga doble clic en el proyecto y seleccione **administrar paquetes de NuGet**.
* Para instalar con la CLI de .NET Core, ejecute lo siguiente en el directorio del proyecto:

   `dotnet add package Microsoft.AspNetCore.Authentication.MicrosoftAccount`

::: moniker range=">= aspnetcore-2.0"

Agregue el servicio de Microsoft Account en el `ConfigureServices` método *Startup.cs* archivo:

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddMicrosoftAccount(microsoftOptions =>
{
    microsoftOptions.ClientId = Configuration["Authentication:Microsoft:ApplicationId"];
    microsoftOptions.ClientSecret = Configuration["Authentication:Microsoft:Password"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Agregue el middleware de Microsoft Account en el `Configure` método *Startup.cs* archivo:

```csharp
app.UseMicrosoftAccountAuthentication(new MicrosoftAccountOptions()
{
    ClientId = Configuration["Authentication:Microsoft:ApplicationId"],
    ClientSecret = Configuration["Authentication:Microsoft:Password"]
});
```

::: moniker-end

Aunque la terminología usada en el Portal para desarrolladores de Microsoft los nombres de estos tokens `ApplicationId` y `Password`, estas se exponen como `ClientId` y `ClientSecret` a la API de configuración.

Consulte la [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) referencia de API para obtener más información sobre las opciones de configuración compatible con la autenticación de Microsoft Account. Esto puede utilizarse para solicitar información diferente sobre el usuario.

## <a name="sign-in-with-microsoft-account"></a>Inicie sesión con la cuenta de Microsoft

Ejecute la aplicación y haga clic en **inicie sesión**. Aparece una opción para iniciar sesión en Microsoft:

![Registro de aplicación en la página Web: usuario no autenticado](index/_static/DoneMicrosoft.png)

Al hacer clic en Microsoft, se le redirigirá a Microsoft para la autenticación. Después de iniciar sesión con su Account de Microsoft (si aún no ha iniciado sesión) se le indicará que permiten que la aplicación acceso a tu información:

![Cuadro de diálogo de autenticación de Microsoft](index/_static/MicrosoftLogin.png)

Pulse **Sí** y se le redirigirá al sitio web donde puede establecer su correo electrónico.

Ha iniciado sesión con sus credenciales de Microsoft:

![Aplicación Web: usuario autenticado](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>Solución de problemas

* Si el proveedor de Microsoft Account le redirige a una página de error de inicio de sesión, tenga en cuenta el error título y descripción cadena parámetros de consulta justo después de la `#` (hashtag) en el Uri.

  Aunque parezca que el mensaje de error indica un problema con la autenticación de Microsoft, la causa más común es la aplicación del Uri no coincide con cualquiera de los **URI de redirección** especificado para el **Web** plataforma .
* **ASP.NET Core 2.x solo:** identidad si no está configurado mediante una llamada a `services.AddIdentity` en `ConfigureServices`, intentando autenticarse producirá *ArgumentException: se debe proporcionar la opción 'SignInScheme'*. La plantilla de proyecto que se usa en este tutorial, se garantiza que esto se realiza.
* Si la base de datos de sitio no se ha creado aplicando a la migración inicial, obtendrá *error en una operación de base de datos al procesar la solicitud* error. Pulse **aplicar migraciones** para crear la base de datos y actualizar para continuar más allá del error.

## <a name="next-steps"></a>Pasos siguientes

* Este artículo, mostramos cómo puede autenticar con Microsoft. Puede seguir un enfoque similar para autenticar con otros proveedores que se enumeran en la [página anterior](xref:security/authentication/social/index).

* Una vez que publique su sitio web a la aplicación web de Azure, debe crear un nuevo `Password` en el Portal para desarrolladores de Microsoft.

* Establecer el `Authentication:Microsoft:ApplicationId` y `Authentication:Microsoft:Password` como configuración de la aplicación en Azure portal. El sistema de configuración está configurado para leer las claves de las variables de entorno.
