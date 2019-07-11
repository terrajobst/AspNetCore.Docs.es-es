---
title: Configuración de inicio de sesión externo Account de Microsoft con ASP.NET Core
author: rick-anderson
description: En este ejemplo se muestra la integración de autenticación de usuario de la cuenta de Microsoft en una aplicación de ASP.NET Core existente.
ms.author: riande
ms.custom: mvc
ms.date: 05/11/2019
uid: security/authentication/microsoft-logins
ms.openlocfilehash: 2c690e5bd8465806d42091616917cfdd747ef8f0
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2019
ms.locfileid: "67815577"
---
# <a name="microsoft-account-external-login-setup-with-aspnet-core"></a>Configuración de inicio de sesión externo Account de Microsoft con ASP.NET Core

Por [Valeriy Novytskyy](https://github.com/01binary) y [Rick Anderson](https://twitter.com/RickAndMSFT)

Este ejemplo muestra cómo habilitar usuarios iniciar sesión con su cuenta de Microsoft con el proyecto de ASP.NET Core 2.2 creado en el [página anterior](xref:security/authentication/social/index).

## <a name="create-the-app-in-microsoft-developer-portal"></a>Crear la aplicación en el Portal para desarrolladores de Microsoft

* Navegue hasta la [Azure portal: registros de aplicaciones](https://go.microsoft.com/fwlink/?linkid=2083908) página y crear o iniciar sesión en una cuenta de Microsoft:

Si no tienes una cuenta de Microsoft, seleccione **crearla**. Después de iniciar sesión se le redirigirá a la **registros de aplicaciones** página:

* Seleccione **nuevo registro**
* Escriba un **nombre**.
* Seleccione una opción para **admite tipos de cuenta**.  <!-- Accounts for any org work with MS domain accounts. Most folks probably want the last option, personal MS accounts -->
* En **URI de redireccionamiento**, escriba la dirección URL de desarrollo con `/signin-microsoft` anexado. Por ejemplo, `https://localhost:44389/signin-microsoft`. El esquema de autenticación de Microsoft configurado más adelante en este ejemplo controlará automáticamente las solicitudes en `/signin-microsoft` ruta para implementar el flujo de OAuth.
* Seleccione **registrar**

### <a name="create-client-secret"></a>Crear el secreto de cliente

* En el panel izquierdo, seleccione **certificados y secretos**.
* En **los secretos de cliente**, seleccione **nuevo secreto de cliente**

  * Agregue una descripción para el secreto de cliente.
  * Seleccione el **agregar** botón.

* En **los secretos de cliente**, copie el valor del secreto de cliente.

> [!NOTE]
> El segmento del URI `/signin-microsoft` se establece como la devolución de llamada predeterminada del proveedor de autenticación de Microsoft. Puede cambiar el URI de devolución de forma predeterminada al configurar el middleware de autenticación de Microsoft a través de los heredados [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) propiedad de la [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) clase.

## <a name="store-the-microsoft-client-id-and-client-secret"></a>El secreto de cliente y el identificador de cliente de Microsoft Store

Ejecute los comandos siguientes para almacenar de forma segura `ClientId` y `ClientSecret` mediante [Secret Manager](xref:security/app-secrets):

```console
dotnet user-secrets set Authentication:Microsoft:ClientId <Client-Id>
dotnet user-secrets set Authentication:Microsoft:ClientSecret <Client-Secret>
```

Vincular configuración confidencial, como Microsoft `ClientId` y `ClientSecret` para la configuración de aplicación mediante el [Secret Manager](xref:security/app-secrets). Para los fines de este ejemplo, asigne el nombre de los tokens `Authentication:Microsoft:ClientId` y `Authentication:Microsoft:ClientSecret`.

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="configure-microsoft-account-authentication"></a>Configurar la autenticación de la cuenta de Microsoft

Agregar el servicio de Microsoft Account `Startup.ConfigureServices`:

[!code-csharp[](~/security/authentication/social/social-code/StartupMS.cs?name=snippet&highlight=10-14)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

Consulte la [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) referencia de API para obtener más información sobre las opciones de configuración compatible con la autenticación de Microsoft Account. Esto puede utilizarse para solicitar información diferente sobre el usuario.

## <a name="sign-in-with-microsoft-account"></a>Inicie sesión con la cuenta de Microsoft

Ejecute el y haga clic en **inicie sesión**. Aparece una opción para iniciar sesión en Microsoft. Al hacer clic en Microsoft, se le redirigirá a Microsoft para la autenticación. Después de iniciar sesión con su Account de Microsoft (si aún no ha iniciado sesión) se le indicará que permiten que la aplicación acceso a tu información:

Pulse **Sí** y se le redirigirá al sitio web donde puede establecer su correo electrónico.

Ha iniciado sesión con sus credenciales de Microsoft:

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>solución de problemas

* Si el proveedor de Microsoft Account le redirige a una página de error de inicio de sesión, tenga en cuenta el error título y descripción cadena parámetros de consulta justo después de la `#` (hashtag) en el Uri.

  Aunque parezca que el mensaje de error indica un problema con la autenticación de Microsoft, la causa más común es la aplicación del Uri no coincide con cualquiera de los **URI de redirección** especificado para el **Web** plataforma .
* Si la identidad no está configurada mediante una llamada a `services.AddIdentity` en `ConfigureServices`, intentando autenticarse producirá *ArgumentException: Se debe proporcionar la opción 'SignInScheme'* . La plantilla de proyecto utilizada en este ejemplo garantiza que esto se realiza.
* Si la base de datos de sitio no se ha creado aplicando a la migración inicial, obtendrá *error en una operación de base de datos al procesar la solicitud* error. Pulse **aplicar migraciones** para crear la base de datos y actualizar para continuar más allá del error.

## <a name="next-steps"></a>Pasos siguientes

* Este artículo, mostramos cómo puede autenticar con Microsoft. Puede seguir un enfoque similar para autenticar con otros proveedores que se enumeran en la [página anterior](xref:security/authentication/social/index).

* Una vez que publique su sitio web a la aplicación web de Azure, cree a un nuevo cliente secretos en el Portal para desarrolladores de Microsoft.

* Establecer el `Authentication:Microsoft:ClientId` y `Authentication:Microsoft:ClientSecret` como configuración de la aplicación en Azure portal. El sistema de configuración está configurado para leer las claves de las variables de entorno.