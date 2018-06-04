---
title: Programa de instalación de Microsoft Account inicio de sesión externo con ASP.NET Core
author: rick-anderson
description: Este tutorial muestra la integración de autenticación de usuario de cuenta de Microsoft en una aplicación existente de ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 08/24/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/microsoft-logins
ms.openlocfilehash: a9bf7b49b1cfdfff65c639eed1e14c94c5432350
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2018
ms.locfileid: "34689027"
---
# <a name="microsoft-account-external-login-setup-with-aspnet-core"></a>Programa de instalación de Microsoft Account inicio de sesión externo con ASP.NET Core

Por [Valeriy Novytskyy](https://github.com/01binary) y [Rick Anderson](https://twitter.com/RickAndMSFT)

Este tutorial muestra cómo permitir a los usuarios iniciar sesión con su cuenta de Microsoft mediante un proyecto de ASP.NET Core 2.0 de ejemplo creado en el [página anterior](xref:security/authentication/social/index).

## <a name="create-the-app-in-microsoft-developer-portal"></a>Crear la aplicación en el Portal para desarrolladores de Microsoft

* Vaya a [ https://apps.dev.microsoft.com ](https://apps.dev.microsoft.com) y crear o iniciar sesión en una cuenta de Microsoft:

![Inicie sesión en el cuadro de diálogo](index/_static/MicrosoftDevLogin.png)

Si ya no tiene una cuenta de Microsoft, pulse  **[crear uno.](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=13&ct=1478151035&rver=6.7.6643.0&wp=SAPI_LONG&wreply=https%3a%2f%2fapps.dev.microsoft.com%2fLoginPostBack&id=293053&aadredir=1&contextid=D70D4F21246BAB50&bk=1478151036&uiflavor=web&uaid=f0c3de863a914c358b8dc01b1ff49e85&mkt=EN-US&lc=1033&lic=1)** Después de iniciar sesión se le redirigirá a **mis aplicaciones** página:

![Portal para desarrolladores de Microsoft abierta en Microsoft Edge](index/_static/MicrosoftDev.png)

* Pulse **agregar una aplicación** en la esquina superior derecha de las esquinas y escriba su **nombre de la aplicación** y **correo electrónico de contacto**:

![Cuadro de diálogo nuevo registro de la aplicación](index/_static/MicrosoftDevAppCreate.png)

* Para los fines de este tutorial, desactive el **el programa de instalación interactiva** casilla de verificación.

* Pulse **crear** para continuar la **registro** página. Proporcionar un **nombre** y anote el valor de la **Id. de aplicación**, que se utiliza como `ClientId` más adelante en el tutorial:

![Página de registro](index/_static/MicrosoftDevAppReg.png)

* Pulse **Agregar plataforma** en el **plataformas** sección y seleccione el **Web** plataforma:

![Agregar cuadro de diálogo de plataforma](index/_static/MicrosoftDevAppPlatform.png)

* En el nuevo **Web** plataforma sección, escriba la dirección URL de desarrollo con */signin-microsoft* anexan a la **redirigir direcciones URL** campo (por ejemplo: `https://localhost:44320/signin-microsoft`). El esquema de autenticación de Microsoft configurado más adelante en este tutorial controlará automáticamente las solicitudes en */signin-microsoft* ruta para implementar el flujo de OAuth:

![Sección de plataforma Web](index/_static/MicrosoftRedirectUri.png)

* Pulse **agregar dirección URL** para asegurarse de que se agregó la dirección URL.

* Rellene cualquier configuración de la aplicación si es necesario y pulse **guardar** en la parte inferior de la página para guardar los cambios en la configuración de la aplicación.

* Al implementar el sitio que necesite volver a visitar el **registro** página y establezca una nueva dirección URL pública.

## <a name="store-microsoft-application-id-and-password"></a>Almacenar identificador de la aplicación de Microsoft y la contraseña

* Tenga en cuenta el `Application Id` muestra en el **registro** página.

* Pulse **generar nueva contraseña** en el **aplicación secretos** sección. Se muestra un cuadro en el que puede copiar la contraseña de aplicación:

![Cuadro de diálogo nueva contraseña generada](index/_static/MicrosoftDevPassword.png)

Vincular valores confidenciales como Microsoft `Application ID` y `Password` a su configuración de aplicación con el [secreto Manager](xref:security/app-secrets). Para los fines de este tutorial, nombre de los tokens `Authentication:Microsoft:ApplicationId` y `Authentication:Microsoft:Password`.

## <a name="configure-microsoft-account-authentication"></a>Configurar la autenticación de cuenta de Microsoft

La plantilla de proyecto que se usan en este tutorial asegura de que [Microsoft.AspNetCore.Authentication.MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount) paquete ya está instalado.

* Para instalar este paquete con 2017 de Visual Studio, haga doble clic en el proyecto y seleccione **administrar paquetes de NuGet**.
* Para instalar con CLI de .NET Core, ejecute lo siguiente en el directorio del proyecto:

   `dotnet add package Microsoft.AspNetCore.Authentication.MicrosoftAccount`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

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

[!INCLUDE[](~/includes/chain-auth-providers.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Agregar el middleware de Microsoft Account en el `Configure` método *Startup.cs* archivo:

```csharp
app.UseMicrosoftAccountAuthentication(new MicrosoftAccountOptions()
{
    ClientId = Configuration["Authentication:Microsoft:ApplicationId"],
    ClientSecret = Configuration["Authentication:Microsoft:Password"]
});
```

---

Aunque la terminología utilizada en el Portal para desarrolladores de Microsoft nombres estos tokens `ApplicationId` y `Password`, se halle expuestos como `ClientId` y `ClientSecret` a la API de configuración.

Consulte la [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) referencia de API para obtener más información sobre las opciones de configuración compatible con autenticación de Microsoft Account. Esto se puede usar para solicitar información diferente sobre el usuario.

## <a name="sign-in-with-microsoft-account"></a>Inicie sesión con la cuenta de Microsoft

Ejecute la aplicación y haga clic en **sesión**. Aparece una opción para iniciar sesión con Microsoft:

![Registro de aplicación en la página Web: usuario no autenticado](index/_static/DoneMicrosoft.png)

Al hacer clic en Microsoft, se le redirigirá a Microsoft para la autenticación. Después de iniciar sesión con su Account de Microsoft (si no lo ha hecho) se le pedirá para permitir que la aplicación acceder a su información:

![Cuadro de diálogo de autenticación de Microsoft](index/_static/MicrosoftLogin.png)

Pulse **Sí** y se le redirigirá al sitio web donde puede establecer el correo electrónico.

Ahora que haya iniciado sesión con sus credenciales de Microsoft:

![Aplicación Web: usuario autenticado](index/_static/Done.png)

## <a name="troubleshooting"></a>Solución de problemas

* Si el proveedor de Microsoft Account le redirige a una página de error de inicio de sesión, tenga en cuenta el error título y descripción de la cadena parámetros de consulta justo después del `#` (hashtag) en el Uri.

  Aunque parezca que el mensaje de error indica un problema con la autenticación de Microsoft, la causa más común es la aplicación Uri no coincide con ninguno de los **URI de redireccionamiento** especificado para la **Web** plataforma .
* **ASP.NET Core solo 2.x:** identidad si no está configurado mediante una llamada a `services.AddIdentity` en `ConfigureServices`, intenta autenticar se producirá en *ArgumentException: se debe proporcionar la opción 'SignInScheme'*. La plantilla de proyecto que se usan en este tutorial se asegura de que esto se realiza.
* Si la base de datos de sitio no se ha creado mediante la aplicación de la migración inicial, obtendrá *error en una operación de base de datos al procesar la solicitud* error. Pulse **migraciones aplicar** para crear la base de datos y actualizar para continuar después del error.

## <a name="next-steps"></a>Pasos siguientes

* En este artículo se ha explicado cómo puede autenticar con Microsoft. Puede seguir un enfoque similar para autenticar con otros proveedores que se enumeran en la [página anterior](xref:security/authentication/social/index).

* Una vez que se publica un sitio web a la aplicación web de Azure, debe crear un nuevo `Password` en el Portal para desarrolladores de Microsoft.

* Establecer el `Authentication:Microsoft:ApplicationId` y `Authentication:Microsoft:Password` como configuración de la aplicación en el portal de Azure. El sistema de configuración está configurado para leer las claves de las variables de entorno.
