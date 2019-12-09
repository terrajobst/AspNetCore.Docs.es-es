---
title: Configuración de inicio de sesión externo de Twitter con ASP.NET Core
author: rick-anderson
description: En este tutorial se muestra la integración de la autenticación de usuarios de cuentas de Twitter en una aplicación ASP.NET Core existente.
ms.author: riande
ms.custom: mvc
ms.date: 12/06/2019
monikerRange: '>= aspnetcore-3.0'
uid: security/authentication/twitter-logins
ms.openlocfilehash: 5d0695160d90d0c5d31b8e35bc6c4cc984829333
ms.sourcegitcommit: 851b921080fe8d719f54871770ccf6f78052584e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/09/2019
ms.locfileid: "74944218"
---
# <a name="twitter-external-sign-in-setup-with-aspnet-core"></a>Configuración de inicio de sesión externo de Twitter con ASP.NET Core

Por [Valeriy Novytskyy](https://github.com/01binary) y [Rick Anderson](https://twitter.com/RickAndMSFT)

Este ejemplo muestra cómo permitir a los usuarios [iniciar sesión con su cuenta de Twitter](https://dev.twitter.com/web/sign-in/desktop-browser) mediante un proyecto de ejemplo ASP.net Core 3,0 creado en la [página anterior](xref:security/authentication/social/index).

## <a name="create-the-app-in-twitter"></a>Creación de la aplicación en Twitter

* Agregue el paquete de NuGet [Microsoft. AspNetCore. Authentication. Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter/3.0.0) al proyecto.

* Vaya a [ https://apps.twitter.com/ ](https://apps.twitter.com/) e inicie sesión. Si aún no tiene una cuenta de Twitter, use el vínculo **[Regístrese ahora](https://twitter.com/signup)** para crear una.

* Seleccione **Crear una aplicación**. Rellene el **nombre**de la aplicación, la descripción de la **aplicación** y el URI del **sitio web** público (esto puede ser temporal hasta que registre el nombre de dominio):

* Escriba el URI de desarrollo con `/signin-twitter` anexado en el campo **URL de devolución de llamada** (por ejemplo: `https://webapp128.azurewebsites.net/signin-twitter`). El esquema de autenticación de Twitter configurado más adelante en este ejemplo administrará automáticamente las solicitudes en `/signin-twitter` ruta para implementar el flujo de OAuth.

  > [!NOTE]
  > El `/signin-twitter` de segmento URI se establece como la devolución de llamada predeterminada del proveedor de autenticación de Twitter. Puede cambiar el URI de devolución de llamada predeterminado mientras configura el middleware de autenticación de Twitter a través de la propiedad heredada [RemoteAuthenticationOptions. CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) de la clase [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) .

* Rellene el resto del formulario y seleccione **crear**. Se muestran los nuevos detalles de la aplicación:

## <a name="storing-twitter-consumer-api-key-and-secret"></a>Almacenar la clave y el secreto de API de consumidor de Twitter

Ejecute los siguientes comandos para almacenar de forma segura `ClientId` y `ClientSecret` mediante el [Administrador de secretos](xref:security/app-secrets):

```dotnetcli
dotnet user-secrets set Authentication:Twitter:ConsumerAPIKey <Key>
dotnet user-secrets set Authentication:Twitter:ConsumerSecret <Secret>
```

Vincule configuraciones confidenciales como Twitter `Consumer Key` y `Consumer Secret` a la configuración de la aplicación mediante el [Administrador de secretos](xref:security/app-secrets). Para los fines de este ejemplo, asigne a los tokens el nombre `Authentication:Twitter:ConsumerKey` y `Authentication:Twitter:ConsumerSecret`.

Estos tokens se pueden encontrar en la pestaña **claves y tokens de acceso** después de crear una nueva aplicación de Twitter:

## <a name="configure-twitter-authentication"></a>Configuración de la autenticación de Twitter

Agregue el servicio Twitter en el método `ConfigureServices` del archivo *Startup.CS* :

[!code-csharp[](~/security/authentication/social/social-code/3.x/StartupTwitter3x.cs?name=snippet&highlight=10-14)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

Consulte la referencia de la API de [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) para más información sobre las opciones de configuración admitidas por la autenticación de Twitter. Esto puede utilizarse para solicitar información diferente sobre el usuario.

## <a name="sign-in-with-twitter"></a>Inicio de sesión con Twitter

Ejecute la aplicación y seleccione **iniciar sesión**. Aparece una opción para iniciar sesión con Twitter:

Al hacer clic en **Twitter** , se redirige a Twitter para la autenticación:

Después de escribir sus credenciales de Twitter, se le redirigirá al sitio web donde puede establecer el correo electrónico.

Ya ha iniciado sesión con sus credenciales de Twitter:

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>Solucionar problemas

* **ASP.NET Core 2.x solo:** identidad si no está configurado mediante una llamada a `services.AddIdentity` en `ConfigureServices`, intentando autenticarse producirá *ArgumentException: se debe proporcionar la opción 'SignInScheme'* . La plantilla de proyecto utilizada en este ejemplo garantiza que esto se realiza.
* Si la base de datos de sitio no se ha creado aplicando a la migración inicial, obtendrá *error en una operación de base de datos al procesar la solicitud* error. Pulse **aplicar migraciones** para crear la base de datos y actualizar para continuar más allá del error.

## <a name="next-steps"></a>Pasos siguientes

* En este artículo se ha mostrado cómo puede realizar la autenticación con Twitter. Puede seguir un enfoque similar para autenticar con otros proveedores que se enumeran en la [página anterior](xref:security/authentication/social/index).

* Una vez que publique el sitio web en la aplicación Web de Azure, debe restablecer el `ConsumerSecret` en el portal para desarrolladores de Twitter.

* Establecer el `Authentication:Twitter:ConsumerKey` y `Authentication:Twitter:ConsumerSecret` como configuración de la aplicación en Azure portal. El sistema de configuración está configurado para leer las claves de las variables de entorno.
