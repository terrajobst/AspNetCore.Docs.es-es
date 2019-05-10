---
title: Twitter inicio de sesión de instalación externo con ASP.NET Core
author: rick-anderson
description: En este tutorial se muestra la integración de autenticación de usuario de la cuenta de Twitter en una aplicación de ASP.NET Core existente.
ms.author: riande
ms.custom: mvc
ms.date: 5/11/2019
uid: security/authentication/twitter-logins
ms.openlocfilehash: 486d58b600ca5326a0728de40bb386fbb9440f67
ms.sourcegitcommit: 3376f224b47a89acf329b2d2f9260046a372f924
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/10/2019
ms.locfileid: "65516883"
---
# <a name="twitter-external-sign-in-setup-with-aspnet-core"></a>Twitter inicio de sesión de instalación externo con ASP.NET Core

Por [Valeriy Novytskyy](https://github.com/01binary) y [Rick Anderson](https://twitter.com/RickAndMSFT)

Este ejemplo muestra cómo habilitar usuarios para [inicie sesión con su cuenta de Twitter](https://dev.twitter.com/web/sign-in/desktop-browser) usando un proyecto de ASP.NET Core 2.2 de ejemplo creado en el [página anterior](xref:security/authentication/social/index).

## <a name="create-the-app-in-twitter"></a>Crear la aplicación en Twitter

* Vaya a [ https://apps.twitter.com/ ](https://apps.twitter.com/) e inicie sesión. Si no dispone de una cuenta de Twitter, use el **[Suscríbase ahora](https://twitter.com/signup)** vínculo para crear uno.

* Pulse **crear nueva aplicación** y rellene la aplicación **nombre**, **descripción** públicas y **sitio Web** (Esto puede ser temporal hasta que el URI Registre el nombre de dominio):

* Escriba el URI de desarrollo con `/signin-twitter` anexado a la **URI de redirección de OAuth válido** campo (por ejemplo: `https://webapp128.azurewebsites.net/signin-twitter`). El esquema de autenticación de Twitter configurado más adelante en este ejemplo controlará automáticamente las solicitudes en `/signin-twitter` ruta para implementar el flujo de OAuth.

  > [!NOTE]
  > El segmento del URI `/signin-twitter` se establece como la devolución de llamada predeterminada del proveedor de autenticación de Twitter. Puede cambiar el URI de devolución de forma predeterminada al configurar el middleware de autenticación de Twitter a través de los heredados [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) propiedad de la [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) clase.

* Rellene el resto del formulario y pulse **crear aplicación de Twitter**. Se muestran los detalles de la nueva aplicación:

## <a name="storing-twitter-consumer-api-key-and-secret"></a>Almacenar la clave de API de consumidor de Twitter y el secreto

Ejecute los comandos siguientes para almacenar de forma segura `ClientId` y `ClientSecret` mediante [Secret Manager](xref:security/app-secrets):

```console
dotnet user-secrets set Authentication:Twitter:ConsumerAPIKey <Key>
dotnet user-secrets set Authentication:Twitter:ConsumerAPISecret <Secret>
```

Vincular los valores confidenciales como Twitter `Consumer Key` y `Consumer Secret` para la configuración de aplicación mediante el [Secret Manager](xref:security/app-secrets). Para los fines de este ejemplo, asigne el nombre de los tokens `Authentication:Twitter:ConsumerKey` y `Authentication:Twitter:ConsumerSecret`.

Estos tokens pueden encontrarse en el **claves y Tokens de acceso** ficha después de crear una nueva aplicación de Twitter:

## <a name="configure-twitter-authentication"></a>Configurar la autenticación de Twitter

Agregue el servicio de Twitter en el `ConfigureServices` método *Startup.cs* archivo:

[!code-csharp[](~/security/authentication/social/social-code/StartupTwitter.cs?name=snippet&highlight=10-14)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

Consulte la [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) referencia de API para obtener más información sobre las opciones de configuración compatible con la autenticación de Twitter. Esto puede utilizarse para solicitar información diferente sobre el usuario.

## <a name="sign-in-with-twitter"></a>Inicie sesión con Twitter

Ejecute la aplicación y seleccione **inicie sesión**. Aparece una opción para iniciar sesión con Twitter:

Al hacer clic en **Twitter** redirige a Twitter para la autenticación:

Después de escribir sus credenciales de Twitter, se le redirigirá al sitio web donde puede establecer su correo electrónico.

Ha iniciado sesión con sus credenciales de Twitter:

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>Solución de problemas

* **ASP.NET Core 2.x solo:** Si la identidad no está configurada mediante una llamada a `services.AddIdentity` en `ConfigureServices`, intentando autenticarse producirá *ArgumentException: Se debe proporcionar la opción 'SignInScheme'*. La plantilla de proyecto utilizada en este ejemplo garantiza que esto se realiza.
* Si la base de datos de sitio no se ha creado aplicando a la migración inicial, obtendrá *error en una operación de base de datos al procesar la solicitud* error. Pulse **aplicar migraciones** para crear la base de datos y actualizar para continuar más allá del error.

## <a name="next-steps"></a>Pasos siguientes

* Este artículo, mostramos cómo puede autenticar con Twitter. Puede seguir un enfoque similar para autenticar con otros proveedores que se enumeran en la [página anterior](xref:security/authentication/social/index).

* Una vez que publique su sitio web a la aplicación web de Azure, debe restablecer el `ConsumerSecret` en el portal para desarrolladores de Twitter.

* Establecer el `Authentication:Twitter:ConsumerKey` y `Authentication:Twitter:ConsumerSecret` como configuración de la aplicación en Azure portal. El sistema de configuración está configurado para leer las claves de las variables de entorno.