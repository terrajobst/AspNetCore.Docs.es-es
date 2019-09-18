---
title: Configuración de inicio de sesión externo de cuenta de Microsoft con ASP.NET Core
author: rick-anderson
description: En este ejemplo se muestra la integración de cuenta de Microsoft la autenticación de usuario en una aplicación de ASP.NET Core existente.
ms.author: riande
ms.custom: mvc
ms.date: 05/11/2019
uid: security/authentication/microsoft-logins
ms.openlocfilehash: 91ace293fd16cd180b3d5c183c637af6db1d08c3
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082341"
---
# <a name="microsoft-account-external-login-setup-with-aspnet-core"></a>Configuración de inicio de sesión externo de cuenta de Microsoft con ASP.NET Core

Por [Valeriy Novytskyy](https://github.com/01binary) y [Rick Anderson](https://twitter.com/RickAndMSFT)

Este ejemplo muestra cómo permitir a los usuarios iniciar sesión con sus cuenta de Microsoft mediante el proyecto ASP.NET Core 2,2 creado en la [página anterior](xref:security/authentication/social/index).

## <a name="create-the-app-in-microsoft-developer-portal"></a>Creación de la aplicación en el portal para desarrolladores de Microsoft

* Vaya a la página [de registros de aplicaciones de Azure portal](https://go.microsoft.com/fwlink/?linkid=2083908) y cree o inicie sesión en un cuenta de Microsoft:

Si no tiene un cuenta de Microsoft, seleccione **crear uno**. Después de iniciar sesión, se le redirigirá a la página **registros de aplicaciones** :

* Seleccionar **nuevo registro**
* Escriba un **nombre**.
* Seleccione una opción para los **tipos de cuenta compatibles**.  <!-- Accounts for any org work with MS domain accounts. Most folks probably want the last option, personal MS accounts -->
* En **URI de redirección**, escriba la dirección URL `/signin-microsoft` de desarrollo con anexado. Por ejemplo, `https://localhost:44389/signin-microsoft`. El esquema de autenticación de Microsoft configurado más adelante en este ejemplo administrará `/signin-microsoft` automáticamente las solicitudes en la ruta para implementar el flujo de OAuth.
* Seleccionar **registro**

### <a name="create-client-secret"></a>Crear secreto de cliente

* En el panel izquierdo, seleccione **certificados & secretos**.
* En **secretos de cliente**, seleccione **nuevo secreto de cliente** .

  * Agregue una descripción para el secreto de cliente.
  * Seleccione el botón **Agregar** .

* En **secretos de cliente**, copie el valor del secreto de cliente.

> [!NOTE]
> El segmento `/signin-microsoft` URI se establece como la devolución de llamada predeterminada del proveedor de autenticación de Microsoft. Puede cambiar el URI de devolución de llamada predeterminado mientras configura el middleware de autenticación de Microsoft a través de la propiedad [RemoteAuthenticationOptions. CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) heredada de la clase [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) .

## <a name="store-the-microsoft-client-id-and-client-secret"></a>Almacenar el ID. de cliente y el secreto de cliente de Microsoft

Ejecute los siguientes comandos para almacenar `ClientId` y `ClientSecret` usar el administrador de [secretos](xref:security/app-secrets)de forma segura:

```dotnetcli
dotnet user-secrets set Authentication:Microsoft:ClientId <Client-Id>
dotnet user-secrets set Authentication:Microsoft:ClientSecret <Client-Secret>
```

Vincule configuraciones confidenciales como `ClientId` Microsoft `ClientSecret` y a la configuración de la aplicación mediante el [Administrador de secretos](xref:security/app-secrets). Para los fines de este ejemplo, asigne un nombre `Authentication:Microsoft:ClientId` a los tokens y. `Authentication:Microsoft:ClientSecret`

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="configure-microsoft-account-authentication"></a>Configurar la autenticación de la cuenta Microsoft

Agregue el servicio de la cuenta `Startup.ConfigureServices`de Microsoft a:

[!code-csharp[](~/security/authentication/social/social-code/StartupMS.cs?name=snippet&highlight=10-14)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

Consulte la referencia de la API de [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) para obtener más información sobre las opciones de configuración admitidas por la autenticación de la cuenta de Microsoft. Esto puede utilizarse para solicitar información diferente sobre el usuario.

## <a name="sign-in-with-microsoft-account"></a>Iniciar sesión con Microsoft cuenta

Ejecute el y haga clic **en iniciar sesión**. Aparece una opción para iniciar sesión con Microsoft. Al hacer clic en Microsoft, se le redirigirá a Microsoft para la autenticación. Después de iniciar sesión con su cuenta de Microsoft (si aún no ha iniciado sesión), se le pedirá que permita que la aplicación acceda a su información:

Puntee en **sí** y se le redirigirá de nuevo al sitio web donde puede establecer el correo electrónico.

Ya ha iniciado sesión con sus credenciales de Microsoft:

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>Solución de problemas

* Si el proveedor de la cuenta de Microsoft le redirige a una página de error de inicio de sesión, tenga en cuenta los parámetros de cadena `#` de consulta título y descripción del error directamente después de (hashtag) en el URI.

  Aunque el mensaje de error parece indicar un problema con la autenticación de Microsoft, la causa más común es que el URI de la aplicación no coincida con ninguno de los **URI de redirección** especificados para la plataforma **Web** .
* Si la identidad no se configura `services.AddIdentity` mediante `ConfigureServices`una llamada a en, si se intenta realizar *la autenticación, se producirá una excepción ArgumentException: Se debe proporcionar*la opción ' SignInScheme '. La plantilla de proyecto utilizada en este ejemplo garantiza que esto se realiza.
* Si la base de datos de sitio no se ha creado aplicando a la migración inicial, obtendrá *error en una operación de base de datos al procesar la solicitud* error. Pulse **aplicar migraciones** para crear la base de datos y actualizar para continuar más allá del error.

## <a name="next-steps"></a>Pasos siguientes

* En este artículo se ha mostrado cómo puede autenticarse con Microsoft. Puede seguir un enfoque similar para autenticar con otros proveedores que se enumeran en la [página anterior](xref:security/authentication/social/index).

* Una vez que publique el sitio web en la aplicación Web de Azure, cree un nuevo secreto de cliente en el portal para desarrolladores de Microsoft.

* Establecer el `Authentication:Microsoft:ClientId` y `Authentication:Microsoft:ClientSecret` como configuración de la aplicación en Azure portal. El sistema de configuración está configurado para leer las claves de las variables de entorno.