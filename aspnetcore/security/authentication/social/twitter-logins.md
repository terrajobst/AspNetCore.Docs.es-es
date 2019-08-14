---
title: Configuración de inicio de sesión externo de Twitter con ASP.NET Core
author: rick-anderson
description: En este tutorial se muestra la integración de la autenticación de usuarios de cuentas de Twitter en una aplicación ASP.NET Core existente.
ms.author: riande
ms.custom: mvc
ms.date: 05/11/2019
uid: security/authentication/twitter-logins
ms.openlocfilehash: 6b6fa3e50f602a92fec9112ac3ba43583de33a70
ms.sourcegitcommit: 89fcc6cb3e12790dca2b8b62f86609bed6335be9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2019
ms.locfileid: "68994279"
---
# <a name="twitter-external-sign-in-setup-with-aspnet-core"></a>Configuración de inicio de sesión externo de Twitter con ASP.NET Core

Por [Valeriy Novytskyy](https://github.com/01binary) y [Rick Anderson](https://twitter.com/RickAndMSFT)

Este ejemplo muestra cómo permitir a los usuarios [iniciar sesión con su cuenta de Twitter](https://dev.twitter.com/web/sign-in/desktop-browser) mediante un proyecto de ejemplo ASP.net Core 2,2 creado en la [página anterior](xref:security/authentication/social/index).

## <a name="create-the-app-in-twitter"></a>Creación de la aplicación en Twitter

* Vaya a [ https://apps.twitter.com/ ](https://apps.twitter.com/) e inicie sesión. Si aún no tiene una cuenta de Twitter, use el vínculo **[Regístrese ahora](https://twitter.com/signup)** para crear una.

* Pulse en **crear nueva aplicación** y rellene el **nombre**de la aplicación, la **Descripción** y el URI del **sitio web** público (esto puede ser temporal hasta que registre el nombre de dominio):

* Escriba el URI de desarrollo `/signin-twitter` con anexado en el campo URI de redirección de **OAuth válido** ( `https://webapp128.azurewebsites.net/signin-twitter`por ejemplo:). El esquema de autenticación de Twitter configurado más adelante en este ejemplo administrará `/signin-twitter` automáticamente las solicitudes de la ruta para implementar el flujo de OAuth.

  > [!NOTE]
  > El segmento `/signin-twitter` URI se establece como la devolución de llamada predeterminada del proveedor de autenticación de Twitter. Puede cambiar el URI de devolución de llamada predeterminado mientras configura el middleware de autenticación de Twitter a través de la propiedad heredada [RemoteAuthenticationOptions. CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) de la clase [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) .

* Rellene el resto del formulario y pulse en **crear la aplicación de Twitter**. Se muestran los nuevos detalles de la aplicación:

## <a name="storing-twitter-consumer-api-key-and-secret"></a>Almacenar la clave y el secreto de API de consumidor de Twitter

Ejecute los siguientes comandos para almacenar `ClientId` y `ClientSecret` usar el administrador de [secretos](xref:security/app-secrets)de forma segura:

```console
dotnet user-secrets set Authentication:Twitter:ConsumerAPIKey <Key>
dotnet user-secrets set Authentication:Twitter:ConsumerSecret <Secret>
```

Vincule configuraciones confidenciales como `Consumer Key` Twitter `Consumer Secret` y a la configuración de la aplicación mediante el [Administrador de secretos](xref:security/app-secrets). Para los fines de este ejemplo, asigne un nombre `Authentication:Twitter:ConsumerKey` a los tokens y. `Authentication:Twitter:ConsumerSecret`

Estos tokens se pueden encontrar en la pestaña **claves y tokens de acceso** después de crear una nueva aplicación de Twitter:

## <a name="configure-twitter-authentication"></a>Configuración de la autenticación de Twitter

Agregue el servicio Twitter en el `ConfigureServices` método en el archivo *Startup.CS* :

[!code-csharp[](~/security/authentication/social/social-code/StartupTwitter.cs?name=snippet&highlight=10-14)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

Consulte la referencia de la API de [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) para más información sobre las opciones de configuración admitidas por la autenticación de Twitter. Esto puede utilizarse para solicitar información diferente sobre el usuario.

## <a name="sign-in-with-twitter"></a>Inicio de sesión con Twitter

Ejecute la aplicación y seleccione **iniciar sesión**. Aparece una opción para iniciar sesión con Twitter:

Al hacer clic en **Twitter** , se redirige a Twitter para la autenticación:

Después de escribir sus credenciales de Twitter, se le redirigirá al sitio web donde puede establecer el correo electrónico.

Ya ha iniciado sesión con sus credenciales de Twitter:

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>solución de problemas

* **Solo ASP.NET Core 2. x:** Si la identidad no se configura `services.AddIdentity` mediante `ConfigureServices`una llamada a en, si se intenta realizar *la autenticación, se producirá una excepción ArgumentException: Se debe proporcionar*la opción ' SignInScheme '. La plantilla de proyecto utilizada en este ejemplo garantiza que esto se realiza.
* Si la base de datos de sitio no se ha creado aplicando a la migración inicial, obtendrá *error en una operación de base de datos al procesar la solicitud* error. Pulse **aplicar migraciones** para crear la base de datos y actualizar para continuar más allá del error.

## <a name="next-steps"></a>Pasos siguientes

* En este artículo se ha mostrado cómo puede realizar la autenticación con Twitter. Puede seguir un enfoque similar para autenticar con otros proveedores que se enumeran en la [página anterior](xref:security/authentication/social/index).

* Una vez que publique el sitio web en la aplicación Web de Azure, debe `ConsumerSecret` restablecer el en el portal para desarrolladores de Twitter.

* Establecer el `Authentication:Twitter:ConsumerKey` y `Authentication:Twitter:ConsumerSecret` como configuración de la aplicación en Azure portal. El sistema de configuración está configurado para leer las claves de las variables de entorno.
