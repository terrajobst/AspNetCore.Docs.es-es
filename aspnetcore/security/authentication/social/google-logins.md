---
title: Configuración de inicio de sesión externo de Google en ASP.NET Core
author: rick-anderson
description: En este tutorial se muestra la integración de autenticación de usuario de la cuenta de Google en una aplicación de ASP.NET Core existente.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 06/19/2019
uid: security/authentication/google-logins
ms.openlocfilehash: e12d831d2e0a5c9acae5ea41fb4187ad4ca6b0ea
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082487"
---
# <a name="google-external-login-setup-in-aspnet-core"></a>Configuración de inicio de sesión externo de Google en ASP.NET Core

Por [Valeriy Novytskyy](https://github.com/01binary) y [Rick Anderson](https://twitter.com/RickAndMSFT)

Las [API heredadas de Google + se han cerrado a partir del 7 de marzo de 2019](https://developers.google.com/+/api-shutdown). El inicio de sesión de Google + y los desarrolladores se deben trasladar a un nuevo sistema de inicio de sesión de Google. Los paquetes ASP.NET Core 2,1 y 2,2 de la autenticación de Google se han actualizado para dar cabida a los cambios. Para obtener más información y mitigaciones temporales de ASP.NET Core, consulte [este problema de github](https://github.com/aspnet/AspNetCore/issues/6486). Este tutorial se ha actualizado con el nuevo proceso de configuración.

En este tutorial se muestra cómo permitir que los usuarios inicien sesión con su cuenta de Google mediante el proyecto ASP.NET Core 2,2 creado en la [página anterior](xref:security/authentication/social/index).

## <a name="create-a-google-api-console-project-and-client-id"></a>Creación de un proyecto y un identificador de cliente de la consola de API de Google

* Vaya a [integración de Google inicio de sesión en la aplicación web](https://developers.google.com/identity/sign-in/web/devconsole-project) y seleccione **configurar un proyecto**.
* En el cuadro de diálogo **configurar el cliente de OAuth** , seleccione **servidor Web**.
* En el cuadro de entrada de texto **URI de redireccionamiento autorizados** , establezca el URI de redirección. Por ejemplo, `https://localhost:5001/signin-google`
* Guarde el **identificador de cliente** y el **secreto de cliente**.
* Al implementar el sitio, registre la nueva dirección URL pública en la **consola de Google**.

## <a name="store-google-clientid-and-clientsecret"></a>Store Google ClientID y ClientSecret

Almacenar configuraciones confidenciales como Google `Client ID` y `Client Secret` con el administrador de [secretos](xref:security/app-secrets). Para los fines de este tutorial, asigne un nombre `Authentication:Google:ClientId` a los tokens y: `Authentication:Google:ClientSecret`

```dotnetcli
dotnet user-secrets set "Authentication:Google:ClientId" "X.apps.googleusercontent.com"
dotnet user-secrets set "Authentication:Google:ClientSecret" "<client secret>"
```

[!INCLUDE[](~/includes/environmentVarableColon.md)]

Puede administrar las credenciales de API y el uso en la [consola de API](https://console.developers.google.com/apis/dashboard).

## <a name="configure-google-authentication"></a>Configuración de la autenticación de Google

Agregue el servicio de Google `Startup.ConfigureServices`a:

[!code-csharp[](~/security/authentication/social/social-code/StartupGoogle.cs?name=snippet_ConfigureServices&highlight=10-18)]

[!INCLUDE [default settings configuration](includes/default-settings2-2.md)]

## <a name="sign-in-with-google"></a>Inicie sesión con Google

* Ejecute la aplicación y haga clic en **iniciar sesión**. Aparece una opción para iniciar sesión con Google.
* Haga clic en el botón **Google** , que redirige a Google para la autenticación.
* Después de escribir las credenciales de Google, se le redirigirá al sitio Web.

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

Consulte la <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions> referencia de la API para obtener más información sobre las opciones de configuración admitidas por la autenticación de Google. Esto puede utilizarse para solicitar información diferente sobre el usuario.

## <a name="change-the-default-callback-uri"></a>Cambiar el URI de devolución de llamada predeterminado

El segmento del URI `/signin-google` se establece como la devolución de llamada predeterminada del proveedor de autenticación de Google. Puede cambiar el URI de devolución de forma predeterminada al configurar el middleware de autenticación de Google a través de los heredados [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) propiedad de la [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) clase.

## <a name="troubleshooting"></a>Solución de problemas

* Si el inicio de sesión no funciona y no recibe ningún error, cambie al modo de desarrollo para que el problema sea más fácil de depurar.
* Si la identidad no se configura `services.AddIdentity` mediante `ConfigureServices`una llamada a en, se intentará *autenticar los resultados en ArgumentException: Se debe proporcionar*la opción ' SignInScheme '. La plantilla de proyecto que se usa en este tutorial, se garantiza que esto se realiza.
* Si la base de datos de sitio no se ha creado aplicando a la migración inicial, obtendrá *error en una operación de base de datos al procesar la solicitud* error. Seleccione **aplicar migraciones** para crear la base de datos y actualice la página para continuar después del error.

## <a name="next-steps"></a>Pasos siguientes

* Este artículo, mostramos cómo puede autenticar con Google. Puede seguir un enfoque similar para autenticar con otros proveedores que se enumeran en la [página anterior](xref:security/authentication/social/index).
* Una vez que publique la aplicación en Azure, restablezca `ClientSecret` en la consola de la API de Google.
* Establecer el `Authentication:Google:ClientId` y `Authentication:Google:ClientSecret` como configuración de la aplicación en Azure portal. El sistema de configuración está configurado para leer las claves de las variables de entorno.
