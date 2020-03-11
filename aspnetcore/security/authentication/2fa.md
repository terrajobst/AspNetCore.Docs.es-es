---
title: Autenticación en dos fases con SMS en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo configurar la autenticación en dos fases (2FA) con una aplicación ASP.NET Core.
monikerRange: < aspnetcore-2.0
ms.author: riande
ms.date: 09/22/2018
ms.custom: mvc, seodec18
uid: security/authentication/2fa
ms.openlocfilehash: 424d21e446de02b39daa7685205a00931361b326
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78652727"
---
# <a name="two-factor-authentication-with-sms-in-aspnet-core"></a>Autenticación en dos fases con SMS en ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Suiza-desarrolladores](https://github.com/Swiss-Devs)

>[!WARNING]
> Dos aplicaciones de autenticador de autenticación (2FA) de factor, con una duración definida por única vez contraseña algoritmo (TOTP), son el enfoque para 2FA recomendado en el sector. 2FA uso TOTP es preferible a SMS 2FA. Para obtener más información, consulte [habilitación de la generación de código QR para aplicaciones TOTP Authenticator en ASP.net Core](xref:security/authentication/identity-enable-qrcodes) para ASP.net Core 2,0 y versiones posteriores.

Este tutorial muestra cómo configurar la autenticación en dos fases (2FA) mediante SMS. Se proporcionan instrucciones para [Twilio](https://www.twilio.com/) y [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), pero puede usar cualquier otro proveedor de SMS. Se recomienda completar la [confirmación de la cuenta y la recuperación de la contraseña antes de](xref:security/authentication/accconfirm) iniciar este tutorial.

[Vea o descargue el código de ejemplo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA). [Cómo descargar](xref:index#how-to-download-a-sample).

## <a name="create-a-new-aspnet-core-project"></a>Crear un nuevo proyecto de ASP.NET Core

Cree una nueva aplicación Web de ASP.NET Core denominada `Web2FA` con cuentas de usuario individuales. Siga las instrucciones de <xref:security/enforcing-ssl> para configurar y requerir HTTPS.

### <a name="create-an-sms-account"></a>Crear una cuenta SMS

Cree una cuenta de SMS, por ejemplo, de [Twilio](https://www.twilio.com/) o [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/). Registre las credenciales de autenticación (twilio: accountSid y authToken para ASPSMS: Userkey y la contraseña).

#### <a name="figuring-out-sms-provider-credentials"></a>Averiguar las credenciales del proveedor de SMS

**Twilio**

En la pestaña panel de la cuenta de Twilio, copie el SID de la **cuenta** y el **token de autenticación**.

**ASPSMS:**

En la configuración de la cuenta, vaya a **Userkey** y cópiela junto con la **contraseña**.

Más adelante almacenaremos estos valores en con la herramienta de administrador de secretos dentro de las claves `SMSAccountIdentification` y `SMSAccountPassword`.

#### <a name="specifying-senderid--originator"></a>Especifica el Id. de remitente / originador

**Twilio:** En la pestaña números, copie el **número de teléfono**de Twilio.

**ASPSMS:** En el menú desbloquear orígenes, desbloquee uno o más originadores o elija un originador alfanumérico (no admitido por todas las redes).

Más adelante se almacenará este valor con la herramienta de administración de secretos en el `SMSAccountFrom`de claves.

### <a name="provide-credentials-for-the-sms-service"></a>Proporcione las credenciales para el servicio SMS

Usaremos el [patrón de opciones](xref:fundamentals/configuration/options) para tener acceso a la configuración de clave y cuenta de usuario.

* Cree una clase para capturar la clave segura de SMS. En este ejemplo, la clase `SMSoptions` se crea en el archivo *Services/SMSoptions. CS* .

[!code-csharp[](2fa/sample/Web2FA/Services/SMSoptions.cs)]

Establezca el `SMSAccountIdentification`, `SMSAccountPassword` y `SMSAccountFrom` con la [herramienta de administración de secretos](xref:security/app-secrets). Por ejemplo:

```none
C:/Web2FA/src/WebApp1>dotnet user-secrets set SMSAccountIdentification 12345
info: Successfully saved SMSAccountIdentification = 12345 to the secret store.
```

* Agregue el paquete NuGet del proveedor de SMS. Desde el Administrador de consola paquetes (PMC) ejecute:

**Twilio**

`Install-Package Twilio`

**ASPSMS:**

`Install-Package ASPSMS`

* Agregue código en el archivo *Services/MessageServices. CS* para habilitar SMS. Utilice el Twilio o la sección ASPSMS:

**Twilio**  
[!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]

**ASPSMS:**  
[!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]

### <a name="configure-startup-to-use-smsoptions"></a>Configurar el inicio para usar `SMSoptions`

Agregue `SMSoptions` al contenedor de servicios en el método `ConfigureServices` en *Startup.CS*:

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet1&highlight=4)]

### <a name="enable-two-factor-authentication"></a>Habilitar la autenticación en dos fases

Abra el archivo de vista de Razor *views/Manage/index. cshtml* y quite los caracteres de comentario (por lo que no hay marcado como comentario).

## <a name="log-in-with-two-factor-authentication"></a>Inicie sesión con autenticación en dos fases

* Ejecute la aplicación y registrar un nuevo usuario

![Vista de registro de aplicación Web abierta en Microsoft Edge](2fa/_static/login2fa1.png)

* Puntee en el nombre de usuario, que activa el método de acción `Index` del controlador de administración. A continuación, puntee en el vínculo número de teléfono para **Agregar** .

![Vista de administración: pulse el vínculo "Agregar"](2fa/_static/login2fa2.png)

* Agregue un número de teléfono que recibirá el código de verificación y pulse **Enviar código de verificación**.

![Agregar página de número de teléfono](2fa/_static/login2fa3.png)

* Obtendrá un mensaje de texto con el código de verificación. Escríbalo y pulse **Enviar** .

![Compruebe la página de número de teléfono](2fa/_static/login2fa4.png)

Si no recibe un mensaje de texto, consulte la página de registro de twilio.

* La vista de administración se muestra que el número de teléfono se agregó correctamente.

![Administrar vista - número de teléfono se agregó correctamente](2fa/_static/login2fa5.png)

* Pulse **Habilitar** para habilitar la autenticación en dos fases.

![Vista de administración: habilitar la autenticación en dos fases](2fa/_static/login2fa6.png)

### <a name="test-two-factor-authentication"></a>Autenticación de dos fases de prueba

* Cierre la sesión.

* Inicie sesión.

* La cuenta de usuario habilitó la autenticación en dos fases, por lo que debe proporcionar el segundo factor de autenticación. En este tutorial se ha habilitado la verificación por teléfono. Las plantillas integradas permiten configurar el correo electrónico como segundo factor. Puede configurar factores adicionales de segundo para la autenticación como códigos QR. Pulse **submit (enviar**).

![Enviar la vista de código de verificación](2fa/_static/login2fa7.png)

* Escriba el código que se obtiene en el mensaje SMS.

* Al hacer clic en la casilla **recordar este explorador** , no es necesario usar 2FA para iniciar sesión cuando se usa el mismo dispositivo y explorador. Al habilitar 2FA y hacer clic en **recordar, este explorador** le proporcionará protección de 2FA segura a los usuarios malintencionados que intentan acceder a su cuenta, siempre y cuando no tengan acceso a su dispositivo. Puede hacerlo en cualquier dispositivo privada que se usan con frecuencia. Al establecer la opción **recordar este explorador**, obtendrá la seguridad agregada de 2FA de los dispositivos que no use con regularidad y que le resulte más cómodo no tener que ir a través de 2FA en sus propios dispositivos.

![Comprobar la vista](2fa/_static/login2fa8.png)

## <a name="account-lockout-for-protecting-against-brute-force-attacks"></a>Bloqueo de cuenta para protegerse contra los ataques por fuerza bruta

Se recomienda el bloqueo de cuenta con 2FA. Una vez que un usuario inicia sesión a través de una cuenta local o social, se almacena cada intento incorrecto en 2FA. Si se alcanza los intentos de acceso con error máximo, el usuario está bloqueado (valor predeterminado: bloqueo de 5 minutos después de 5 intentos de acceso). Una autenticación correcta restablece el número de intentos de acceso erróneos y restablece el reloj. El tiempo máximo de bloqueo y los intentos de acceso incorrectos se pueden establecer con [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) y [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan). El siguiente ejemplo configura el bloqueo de cuentas durante 10 minutos tras 10 intentos de acceso:

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet2&highlight=13-17)]

Confirme que [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync) establece `lockoutOnFailure` en `true`:

```csharp
var result = await _signInManager.PasswordSignInAsync(
                 Input.Email, Input.Password, Input.RememberMe, lockoutOnFailure: true);
```
