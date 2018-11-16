---
title: Configuración de inicio de sesión externo de Twitter con ASP.NET Core
author: rick-anderson
description: En este tutorial se muestra la integración de autenticación de usuario de la cuenta de Twitter en una aplicación de ASP.NET Core existente.
ms.author: riande
ms.custom: mvc
ms.date: 11/11/2018
uid: security/authentication/twitter-logins
ms.openlocfilehash: 43a5ea59d8853d297ae2c1ec3f4b1c0c14ec80c3
ms.sourcegitcommit: 09bcda59a58019fdf47b2db5259fe87acf19dd38
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2018
ms.locfileid: "51708431"
---
# <a name="twitter-external-login-setup-with-aspnet-core"></a>Configuración de inicio de sesión externo de Twitter con ASP.NET Core

Por [Valeriy Novytskyy](https://github.com/01binary) y [Rick Anderson](https://twitter.com/RickAndMSFT)

Este tutorial muestra cómo permitir a los usuarios [inicie sesión con su cuenta de Twitter](https://dev.twitter.com/web/sign-in/desktop-browser) con un proyecto de ASP.NET Core 2.0 de ejemplo creado en el [página anterior](xref:security/authentication/social/index).

## <a name="create-the-app-in-twitter"></a>Crear la aplicación en Twitter

* Vaya a [ https://apps.twitter.com/ ](https://apps.twitter.com/) e inicie sesión. Si no dispone de una cuenta de Twitter, use el **[Suscríbase ahora](https://twitter.com/signup)** vínculo para crear uno. Después de iniciar sesión, el **Application Management** se muestra la página:

  ![Administración de aplicaciones de Twitter abierto en Microsoft Edge](index/_static/TwitterAppManage.png)

* Pulse **crear nueva aplicación** y rellene la aplicación **nombre**, **descripción** públicas y **sitio Web** (Esto puede ser temporal hasta que el URI Registre el nombre de dominio):

  ![Crear una página de aplicación](index/_static/TwitterCreate.png)

* Escriba el URI de desarrollo con `/signin-twitter` anexado a la **URI de redirección de OAuth válido** campo (por ejemplo: `https://localhost:44320/signin-twitter`). El esquema de autenticación de Twitter configurado más adelante en este tutorial controlará automáticamente las solicitudes en `/signin-twitter` ruta para implementar el flujo de OAuth.

  > [!NOTE]
  > El segmento del URI `/signin-twitter` se establece como la devolución de llamada predeterminada del proveedor de autenticación de Twitter. Puede cambiar el URI de devolución de forma predeterminada al configurar el middleware de autenticación de Twitter a través de los heredados [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) propiedad de la [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) clase.

* Rellene el resto del formulario y pulse **crear aplicación de Twitter**. Se muestran los detalles de la nueva aplicación:

  ![Pestaña Detalles de la página de aplicación](index/_static/TwitterAppDetails.png)

* Al implementar el sitio, deberá volver a visitar la **Application Management** página y registrar un nuevo URI público.

## <a name="storing-twitter-consumerkey-and-consumersecret"></a>Almacenar Twitter ConsumerKey y ConsumerSecret

Vincular los valores confidenciales como Twitter `Consumer Key` y `Consumer Secret` para la configuración de aplicación mediante el [Secret Manager](xref:security/app-secrets). Para los fines de este tutorial, asigne el nombre de los tokens `Authentication:Twitter:ConsumerKey` y `Authentication:Twitter:ConsumerSecret`.

Estos tokens pueden encontrarse en el **claves y Tokens de acceso** ficha después de crear la nueva aplicación de Twitter:

![Pestaña de claves y Tokens de acceso](index/_static/TwitterKeys.png)

## <a name="configure-twitter-authentication"></a>Configurar la autenticación de Twitter

La plantilla de proyecto que se usa en este tutorial garantiza que [Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter) paquete ya está instalado.

* Para instalar este paquete con Visual Studio 2017, haga doble clic en el proyecto y seleccione **administrar paquetes de NuGet**.
* Para instalar con la CLI de .NET Core, ejecute lo siguiente en el directorio del proyecto:

  `dotnet add package Microsoft.AspNetCore.Authentication.Twitter`

::: moniker range=">= aspnetcore-2.0"

Agregue el servicio de Twitter en el `ConfigureServices` método *Startup.cs* archivo:

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddTwitter(twitterOptions =>
{
    twitterOptions.ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"];
    twitterOptions.ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Agregue el middleware de Twitter en el `Configure` método *Startup.cs* archivo:

```csharp
app.UseTwitterAuthentication(new TwitterOptions()
{
    ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"],
    ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"]
});
```

::: moniker-end

Consulte la [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) referencia de API para obtener más información sobre las opciones de configuración compatible con la autenticación de Twitter. Esto puede utilizarse para solicitar información diferente sobre el usuario.

## <a name="sign-in-with-twitter"></a>Inicie sesión con Twitter

Ejecute la aplicación y haga clic en **inicie sesión**. Aparece una opción para iniciar sesión con Twitter:

![Aplicación Web: usuario no autenticado](index/_static/DoneTwitter.png)

Al hacer clic en **Twitter** redirige a Twitter para la autenticación:

![Página de autenticación de Twitter](index/_static/TwitterLogin.png)

Después de escribir sus credenciales de Twitter, se le redirigirá al sitio web donde puede establecer su correo electrónico.

Ha iniciado sesión con sus credenciales de Twitter:

![Aplicación Web: usuario autenticado](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>Solución de problemas

* **ASP.NET Core 2.x solo:** identidad si no está configurado mediante una llamada a `services.AddIdentity` en `ConfigureServices`, intentando autenticarse producirá *ArgumentException: se debe proporcionar la opción 'SignInScheme'*. La plantilla de proyecto que se usa en este tutorial, se garantiza que esto se realiza.
* Si la base de datos de sitio no se ha creado aplicando a la migración inicial, obtendrá *error en una operación de base de datos al procesar la solicitud* error. Pulse **aplicar migraciones** para crear la base de datos y actualizar para continuar más allá del error.

## <a name="next-steps"></a>Pasos siguientes

* Este artículo, mostramos cómo puede autenticar con Twitter. Puede seguir un enfoque similar para autenticar con otros proveedores que se enumeran en la [página anterior](xref:security/authentication/social/index).

* Una vez que publique su sitio web a la aplicación web de Azure, debe restablecer el `ConsumerSecret` en el portal para desarrolladores de Twitter.

* Establecer el `Authentication:Twitter:ConsumerKey` y `Authentication:Twitter:ConsumerSecret` como configuración de la aplicación en Azure portal. El sistema de configuración está configurado para leer las claves de las variables de entorno.
