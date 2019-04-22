---
title: Configuración de inicio de sesión externo de Google en ASP.NET Core
author: rick-anderson
description: En este tutorial se muestra la integración de autenticación de usuario de la cuenta de Google en una aplicación de ASP.NET Core existente.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 1/11/2019
uid: security/authentication/google-logins
ms.openlocfilehash: 44c79b3279db7946b6d89a726bd3f5acfb5f51af
ms.sourcegitcommit: 78339e9891c8676db01a6e81e9cb0cdaa280162f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59705596"
---
# <a name="google-external-login-setup-in-aspnet-core"></a>Configuración de inicio de sesión externo de Google en ASP.NET Core

Por [Valeriy Novytskyy](https://github.com/01binary) y [Rick Anderson](https://twitter.com/RickAndMSFT)

[Google + API heredadas que se haya cerrado a partir del 7 de marzo de 2019](https://developers.google.com/+/api-shutdown). Google + iniciar sesión y los desarrolladores deben mover a un nuevo inicio de sesión de Google en el sistema. Los paquetes ASP.NET Core 2.1 y 2.2 para la autenticación de Google se ha actualizado para adaptarse a los cambios. Para obtener más información y mitigaciones temporales para ASP.NET Core, consulte [este problema de GitHub](https://github.com/aspnet/AspNetCore/issues/6486). En este tutorial se ha actualizado con el nuevo proceso de instalación.

Este tutorial muestra cómo habilitar usuarios iniciar sesión con su cuenta de Google con el proyecto de ASP.NET Core 2.2 creado en el [página anterior](xref:security/authentication/social/index).

## <a name="create-a-google-api-console-project-and-client-id"></a>Crear un identificador de cliente y el proyecto de consola de API de Google

* Vaya a [la integración de Google signo In de la aplicación web](https://developers.google.com/identity/sign-in/web/devconsole-project) y seleccione **configurar un proyecto**.
* En el **configuración del cliente de OAuth** cuadro de diálogo, seleccione **servidor Web**.
* En el **URI de redireccionamiento autorizado** cuadro de entrada de texto, establecer el URI de redireccionamiento. Por ejemplo, `https://localhost:5001/signin-google`.
* Guardar el **Id. de cliente** y **secreto de cliente**.
* Al implementar el sitio, registrar la nueva url pública desde el **Google Console**.

## <a name="store-google-clientid-and-clientsecret"></a>Store Google ClientID y ClientSecret

Store valores confidenciales, como Google `Client ID` y `Client Secret` con el [Secret Manager](xref:security/app-secrets). Para los fines de este tutorial, asigne el nombre de los tokens `Authentication:Google:ClientId` y `Authentication:Google:ClientSecret`:

```console
dotnet user-secrets set "Authentication:Google:ClientId" "X.apps.googleusercontent.com"
dotnet user-secrets set "Authentication:Google:ClientSecret" "<client secret>"
```

[!INCLUDE[](~/includes/environmentVarableColon.md)]

Puede administrar las credenciales de la API y el uso en el [consola de API](https://console.developers.google.com/apis/dashboard).

## <a name="configure-google-authentication"></a>Configurar la autenticación de Google

Agregar el servicio de Google `Startup.ConfigureServices`.

[!INCLUDE [default settings configuration](includes/default-settings2-2.md)]

## <a name="sign-in-with-google"></a>Inicie sesión con Google

* Ejecute la aplicación y haga clic en **inicie sesión**. Aparece una opción para iniciar sesión con Google.
* Haga clic en el **Google** botón, que redirige a Google para la autenticación.
* Después de escribir sus credenciales de Google, se le redirigirá al sitio web.

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

Consulte la [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) referencia de API para obtener más información sobre las opciones de configuración compatible con la autenticación de Google. Esto puede utilizarse para solicitar información diferente sobre el usuario.

## <a name="change-the-default-callback-uri"></a>Cambiar el URI de devolución de llamada predeterminada

El segmento del URI `/signin-google` se establece como la devolución de llamada predeterminada del proveedor de autenticación de Google. Puede cambiar el URI de devolución de forma predeterminada al configurar el middleware de autenticación de Google a través de los heredados [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) propiedad de la [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) clase.

## <a name="troubleshooting"></a>Solución de problemas

* Si el inicio de sesión no funciona y no recibe algún error, cambie al modo de desarrollo para hacer más fácil de depurar el problema.
* Si la identidad no está configurada mediante una llamada a `services.AddIdentity` en `ConfigureServices`, al intentar autenticar los resultados en *ArgumentException: Se debe proporcionar la opción 'SignInScheme'*. La plantilla de proyecto que se usa en este tutorial, se garantiza que esto se realiza.
* Si la base de datos de sitio no se ha creado aplicando a la migración inicial, obtendrá *error en una operación de base de datos al procesar la solicitud* error. Pulse **aplicar migraciones** para crear la base de datos y actualizar para continuar más allá del error.

## <a name="next-steps"></a>Pasos siguientes

* Este artículo, mostramos cómo puede autenticar con Google. Puede seguir un enfoque similar para autenticar con otros proveedores que se enumeran en la [página anterior](xref:security/authentication/social/index).
* Una vez que publique la aplicación en Azure, restablezca el `ClientSecret` en la consola de API de Google.
* Establecer el `Authentication:Google:ClientId` y `Authentication:Google:ClientSecret` como configuración de la aplicación en Azure portal. El sistema de configuración está configurado para leer las claves de las variables de entorno.
