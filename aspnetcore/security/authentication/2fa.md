---
title: Autenticación en dos fases con SMS en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo configurar la autenticación en dos fases (2FA) con una aplicación ASP.NET Core.
monikerRange: < aspnetcore-2.0
ms.author: riande
ms.date: 09/22/2018
uid: security/authentication/2fa
ms.openlocfilehash: 19cc4b5326e8359afd47dd75aca3d661c3f92a30
ms.sourcegitcommit: 9bdba90b2c97a4016188434657194b2d7027d6e3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2018
ms.locfileid: "47402125"
---
# <a name="two-factor-authentication-with-sms-in-aspnet-core"></a>Autenticación en dos fases con SMS en ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [suizo Devs](https://github.com/Swiss-Devs)

>[!WARNING]
> Dos aplicaciones de autenticador de autenticación (2FA) de factor, con una duración definida por única vez contraseña algoritmo (TOTP), son el enfoque para 2FA recomendado en el sector. 2FA uso TOTP es preferible a SMS 2FA. Para obtener más información, consulte [generación habilitar código QR para las aplicaciones de authenticator TOTP en ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) para ASP.NET Core 2.0 y versiones posteriores.

Este tutorial muestra cómo configurar la autenticación en dos fases (2FA) mediante SMS. Se proporcionan instrucciones para [twilio](https://www.twilio.com/) y [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), pero puede usar cualquier otro proveedor SMS. Se recomienda realizar [confirmación de la cuenta y contraseña de recuperación](xref:security/authentication/accconfirm) antes de comenzar este tutorial.

Ver el [ejemplo completado](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA). [Cómo descargar](xref:tutorials/index#how-to-download-a-sample).

## <a name="create-a-new-aspnet-core-project"></a>Crear un nuevo proyecto de ASP.NET Core

Crear una nueva aplicación web de ASP.NET Core denominada `Web2FA` con cuentas de usuario individuales. Siga las instrucciones de [exigir SSL en una aplicación ASP.NET Core](xref:security/enforcing-ssl) para configurar y requieren SSL.

### <a name="create-an-sms-account"></a>Crear una cuenta SMS

Crear una cuenta SMS, por ejemplo, desde [twilio](https://www.twilio.com/) o [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/). Registre las credenciales de autenticación (twilio: accountSid y authToken para ASPSMS: Userkey y la contraseña).

#### <a name="figuring-out-sms-provider-credentials"></a>Averiguar las credenciales del proveedor de SMS

**Twilio:** en la ficha Panel de la cuenta de Twilio, copie el **SID de cuenta** y **Auth token**.

**ASPSMS:** desde la configuración de su cuenta, vaya a **Userkey** y cópielo junto con su **contraseña**.

Más adelante, almacenaremos estos valores con la herramienta Administrador de secretos en el conjunto de claves `SMSAccountIdentification` y `SMSAccountPassword`.

#### <a name="specifying-senderid--originator"></a>Especifica el Id. de remitente / originador

**Twilio:** desde la ficha números, copie su Twilio **número de teléfono**.

**ASPSMS:** en el menú de los originadores desbloquear, desbloquear uno o varios de los originadores o elija un originador alfanumérico (no admitido todas las redes).

Más adelante se almacenará este valor con la herramienta secret manager dentro de la clave `SMSAccountFrom`.


### <a name="provide-credentials-for-the-sms-service"></a>Proporcione las credenciales para el servicio SMS

Vamos a usar el [patrón de opciones](xref:fundamentals/configuration/options) para tener acceso a la configuración de cuenta y clave de usuario.

   * Cree una clase para capturar la clave segura de SMS. Para este ejemplo, el `SMSoptions` se crea una clase en el *Services/SMSoptions.cs* archivo.

[!code-csharp[](2fa/sample/Web2FA/Services/SMSoptions.cs)]

Establecer el `SMSAccountIdentification`, `SMSAccountPassword` y `SMSAccountFrom` con el [herramienta secret manager](xref:security/app-secrets). Por ejemplo:

```none
C:/Web2FA/src/WebApp1>dotnet user-secrets set SMSAccountIdentification 12345
info: Successfully saved SMSAccountIdentification = 12345 to the secret store.
```
* Agregue el paquete NuGet del proveedor de SMS. Desde el Administrador de consola paquetes (PMC) ejecute:

**Twilio:**
`Install-Package Twilio`

**ASPSMS:**
`Install-Package ASPSMS`


* Agregue código en el *Services/MessageServices.cs* archivo para habilitar SMS. Utilice el Twilio o la sección ASPSMS:


**Twilio:** [!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]

**ASPSMS:** [!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]

### <a name="configure-startup-to-use-smsoptions"></a>Configurar el inicio de usar `SMSoptions`

Agregar `SMSoptions` al contenedor de servicios en la `ConfigureServices` método en el *Startup.cs*:

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet1&highlight=4)]

### <a name="enable-two-factor-authentication"></a>Habilitar la autenticación en dos fases

Abra el *Views/Manage/Index.cshtml* archivo de vista de Razor y quite el comentario caracteres (de modo que no hay ningún marcado commnted out).

## <a name="log-in-with-two-factor-authentication"></a>Inicie sesión con autenticación en dos fases

* Ejecute la aplicación y registrar un nuevo usuario

![Vista de registro de aplicación Web abierta en Microsoft Edge](2fa/_static/login2fa1.png)

* Puntee en el nombre de usuario, activa la `Index` métodos de acción de controlador de administración. A continuación, puntee en el número de teléfono **agregar** vínculo.

![Administrar la vista](2fa/_static/login2fa2.png)

* Agregar un número de teléfono que recibirá el código de verificación y pulse **enviar código de verificación**.

![Agregar página de número de teléfono](2fa/_static/login2fa3.png)

* Obtendrá un mensaje de texto con el código de verificación. Escríbala y pulse **enviar**

![Compruebe la página de número de teléfono](2fa/_static/login2fa4.png)

Si no recibe un mensaje de texto, consulte la página de registro de twilio.

* La vista de administración se muestra que el número de teléfono se agregó correctamente.

![Administrar la vista](2fa/_static/login2fa5.png)

* Pulse **habilitar** para habilitar la autenticación en dos fases.

![Administrar la vista](2fa/_static/login2fa6.png)

### <a name="test-two-factor-authentication"></a>Autenticación de dos fases de prueba

* Cierre la sesión.

* Inicia sesión.

* La cuenta de usuario habilitó la autenticación en dos fases, por lo que debe proporcionar el segundo factor de autenticación. En este tutorial se ha habilitado la verificación por teléfono. Las plantillas integradas permiten configurar el correo electrónico como segundo factor. Puede configurar factores adicionales de segundo para la autenticación como códigos QR. Pulse **enviar**.

![Enviar la vista de código de verificación](2fa/_static/login2fa7.png)

* Escriba el código que se obtiene en el mensaje SMS.

* Al hacer clic en el **recordar este explorador** casilla eximirán de la necesidad de usar 2FA para iniciar sesión cuando se usa el mismo dispositivo y el explorador. Habilitar 2FA y haga clic en **recordar este explorador** le proporcionará 2FA fuerte protección contra usuarios malintencionados que intenta acceder a su cuenta, siempre que no tienen acceso a su dispositivo. Puede hacerlo en cualquier dispositivo privada que se usan con frecuencia. Estableciendo **recordar este explorador**, obtendrá la seguridad añadida de 2FA desde dispositivos corrientemente no usamos y obtener la comodidad de no tener que pasar por 2FA en sus propios dispositivos.

![Comprobar la vista](2fa/_static/login2fa8.png)

## <a name="account-lockout-for-protecting-against-brute-force-attacks"></a>Bloqueo de cuenta para protegerse contra los ataques por fuerza bruta

Se recomienda el bloqueo de cuenta con 2FA. Una vez que un usuario inicia sesión a través de una cuenta local o social, se almacena cada intento incorrecto en 2FA. Si se alcanza los intentos de acceso con error máximo, el usuario está bloqueado (valor predeterminado: bloqueo de 5 minutos después de 5 intentos de acceso). Una autenticación correcta restablece el número de intentos de acceso erróneos y restablece el reloj. El máximo intentos de acceso y se puede establecer el tiempo de bloqueo con [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) y [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan). El siguiente ejemplo configura el bloqueo de cuentas durante 10 minutos tras 10 intentos de acceso:

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet2&highlight=13-17)]

Confirme que [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync) establece `lockoutOnFailure` a `true`:

```csharp
var result = await _signInManager.PasswordSignInAsync(
                 Input.Email, Input.Password, Input.RememberMe, lockoutOnFailure: true);
```
