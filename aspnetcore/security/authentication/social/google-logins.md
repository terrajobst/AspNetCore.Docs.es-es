---
title: Configuración de inicio de sesión externo de Google en ASP.NET Core
author: rick-anderson
description: En este tutorial se muestra la integración de autenticación de usuario de la cuenta de Google en una aplicación de ASP.NET Core existente.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 03/19/2020
uid: security/authentication/google-logins
ms.openlocfilehash: a114d23c25201c9fe31ad0397efaf99fe98a312a
ms.sourcegitcommit: 9b6e7f421c243963d5e419bdcfc5c4bde71499aa
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/21/2020
ms.locfileid: "79989771"
---
# <a name="google-external-login-setup-in-aspnet-core"></a>Configuración de inicio de sesión externo de Google en ASP.NET Core

Por [Valeriy Novytskyy](https://github.com/01binary) y [Rick Anderson](https://twitter.com/RickAndMSFT)

En este tutorial se muestra cómo permitir que los usuarios inicien sesión con su cuenta de Google mediante el proyecto ASP.NET Core 3,0 creado en la [página anterior](xref:security/authentication/social/index).

## <a name="create-a-google-api-console-project-and-client-id"></a>Creación de un proyecto y un identificador de cliente de la consola de API de Google

* Instale [Microsoft. AspNetCore. Authentication. Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google).
* Vaya a [integración de Google inicio de sesión en la aplicación web](https://developers.google.com/identity/sign-in/web/devconsole-project) y seleccione **configurar un proyecto**.
* En el cuadro de diálogo **configurar el cliente de OAuth** , seleccione **servidor Web**.
* En el cuadro de entrada de texto **URI de redireccionamiento autorizados** , establezca el URI de redirección. Por ejemplo: `https://localhost:44312/signin-google`
* Guarde el **identificador de cliente** y el **secreto de cliente**.
* Al implementar el sitio, registre la nueva dirección URL pública en la **consola de Google**.

## <a name="store-the-google-client-id-and-secret"></a>Almacenar el identificador y el secreto de cliente de Google

Almacenar valores confidenciales como el ID. de cliente de Google y los valores de secreto con el [Administrador de secretos](xref:security/app-secrets). En este ejemplo, siga estos pasos:

1. Inicialice el proyecto para el almacenamiento de secretos según las instrucciones de [enable Secret Storage](xref:security/app-secrets#enable-secret-storage).
1. Almacene la configuración confidencial en el almacén secreto local con las claves secretas `Authentication:Google:ClientId` y `Authentication:Google:ClientSecret`:

    ```dotnetcli
    dotnet user-secrets set "Authentication:Google:ClientId" "<client-id>"
    dotnet user-secrets set "Authentication:Google:ClientSecret" "<client-secret>"
    ```

[!INCLUDE[](~/includes/environmentVarableColon.md)]

Puede administrar las credenciales de API y el uso en la [consola de API](https://console.developers.google.com/apis/dashboard).

## <a name="configure-google-authentication"></a>Configuración de la autenticación de Google

Agregue el servicio de Google a `Startup.ConfigureServices`:

[!code-csharp[](~/security/authentication/social/social-code/3.x/StartupGoogle3x.cs?highlight=11-19)]

[!INCLUDE [default settings configuration](includes/default-settings2-2.md)]

## <a name="sign-in-with-google"></a>Inicio de sesión con Google

* Ejecute la aplicación y haga clic en **iniciar sesión**. Aparece una opción para iniciar sesión con Google.
* Haga clic en el botón **Google** , que redirige a Google para la autenticación.
* Después de escribir las credenciales de Google, se le redirigirá al sitio Web.

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

Consulte la referencia de la API de <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions> para obtener más información sobre las opciones de configuración admitidas por la autenticación de Google. Esto puede utilizarse para solicitar información diferente sobre el usuario.

## <a name="change-the-default-callback-uri"></a>Cambiar el URI de devolución de llamada predeterminado

El `/signin-google` de segmento URI se establece como la devolución de llamada predeterminada del proveedor de autenticación de Google. Puede cambiar el URI de devolución de llamada predeterminado mientras configura el middleware de autenticación de Google a través de la propiedad heredada [RemoteAuthenticationOptions. CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) de la clase [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) .

## <a name="troubleshooting"></a>Solución de problemas

* Si el inicio de sesión no funciona y no recibe ningún error, cambie al modo de desarrollo para que el problema sea más fácil de depurar.
* Si la identidad no se configura mediante una llamada a `services.AddIdentity` en `ConfigureServices`, se intentará autenticar los resultados en *ArgumentException: se debe proporcionar la opción ' SignInScheme '* . La plantilla de proyecto que se usa en este tutorial, se garantiza que esto se realiza.
* Si la base de datos del sitio no se ha creado aplicando la migración inicial, se producirá *un error en la operación de base de datos al procesar el error de solicitud* . Seleccione **aplicar migraciones** para crear la base de datos y actualice la página para continuar después del error.

## <a name="next-steps"></a>Pasos siguientes

* Este artículo, mostramos cómo puede autenticar con Google. Puede seguir un enfoque similar para autenticarse con otros proveedores mostrados en la [página anterior](xref:security/authentication/social/index).
* Una vez que publique la aplicación en Azure, restablezca el `ClientSecret` en la consola de la API de Google.
* Establezca la `Authentication:Google:ClientId` y `Authentication:Google:ClientSecret` como configuración de la aplicación en el Azure Portal. El sistema de configuración está configurado para leer las claves de las variables de entorno.
