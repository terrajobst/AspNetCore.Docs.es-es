---
title: Autenticación en dos fases con SMS en ASP.NET Core
author: rick-anderson
description: Obtenga información acerca de cómo configurar la autenticación en dos fases (2FA) con una aplicación de ASP.NET Core.
manager: wpickett
monikerRange: < aspnetcore-2.0
ms.author: riande
ms.date: 08/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/2fa
ms.openlocfilehash: 20f00c2307e140d81e716304c96a143340d934d0
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/08/2018
---
# <a name="two-factor-authentication-with-sms-in-aspnet-core"></a>Autenticación en dos fases con SMS en ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [desarrolladores suizo](https://github.com/Swiss-Devs)

Vea [generación habilitar código QR para las aplicaciones de autenticador de ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) para ASP.NET Core 2.0 y versiones posteriores.

Este tutorial muestra cómo configurar la autenticación en dos fases (2FA) con SMS. Se proporcionan instrucciones para [twilio](https://www.twilio.com/) y [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), pero puede usar cualquier otro proveedor SMS. Se recomienda realizar [confirmación de cuenta y contraseña de recuperación](xref:security/authentication/accconfirm) antes de iniciar este tutorial.

Ver el [ejemplo completo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA). [Cómo descargar](xref:tutorials/index#how-to-download-a-sample).

## <a name="create-a-new-aspnet-core-project"></a>Cree un nuevo proyecto de ASP.NET Core

Crear una nueva aplicación web de ASP.NET Core denominada `Web2FA` con cuentas de usuario individuales. Siga las instrucciones de [exigir SSL en una aplicación de ASP.NET Core](xref:security/enforcing-ssl) para configurar y requerir SSL.

### <a name="create-an-sms-account"></a>Crear una cuenta SMS

Crear una cuenta SMS, por ejemplo, de [twilio](https://www.twilio.com/) o [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/). Registre las credenciales de autenticación (para twilio: accountSid y authToken para ASPSMS: clave de usuario confidenciales y la contraseña).

#### <a name="figuring-out-sms-provider-credentials"></a>Pensar en las credenciales del proveedor de SMS

**Twilio:**  
En la ficha Panel de su cuenta de Twilio, copie la **SID de cuenta** y **token de autenticación**.

**ASPSMS:**  
Desde la configuración de su cuenta, vaya a **clave de usuario confidenciales** y cópielo junto con su **contraseña**.

Más adelante se almacenará estos valores con la herramienta Administrador de secreto en el conjunto de claves `SMSAccountIdentification` y `SMSAccountPassword`.

#### <a name="specifying-senderid--originator"></a>Especifica el identificador del remitente / originador

**Twilio:**  
En la ficha números, copie su Twilio **número de teléfono**. 

**ASPSMS:**  
En el menú de remitentes desbloquear, desbloquear uno o más remitentes o elija un originador alfanumérico (no admitido todas las redes). 

Más adelante se almacenará este valor con la herramienta Administrador de secreto en la clave `SMSAccountFrom`.


### <a name="provide-credentials-for-the-sms-service"></a>Proporcione las credenciales para el servicio SMS

Vamos a usar la [patrón opciones](xref:fundamentals/configuration/options) para tener acceso a la configuración de cuenta y clave de usuario. 

   * Cree una clase para capturar la clave SMS segura. En este ejemplo, el `SMSoptions` se crea una clase en el *Services/SMSoptions.cs* archivo.

[!code-csharp[](2fa/sample/Web2FA/Services/SMSoptions.cs)]

Establecer el `SMSAccountIdentification`, `SMSAccountPassword` y `SMSAccountFrom` con el [herramienta Administrador de secreto](xref:security/app-secrets). Por ejemplo:

```none
C:/Web2FA/src/WebApp1>dotnet user-secrets set SMSAccountIdentification 12345
info: Successfully saved SMSAccountIdentification = 12345 to the secret store.
```
* Agregue el paquete de NuGet del proveedor de SMS. Desde el paquete de administrador de consola (PMC) ejecutar:

**Twilio:**  
`Install-Package Twilio`

**ASPSMS:**  
`Install-Package ASPSMS`


* Agregue código en el *Services/MessageServices.cs* archivo para habilitar SMS. Utilice la Twilio o la sección ASPSMS:


**Twilio:**  
[!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]

**ASPSMS:**  
[!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]

### <a name="configure-startup-to-use-smsoptions"></a>Configurar el inicio de usar `SMSoptions`

Agregar `SMSoptions` al contenedor de servicios en la `ConfigureServices` método en el *Startup.cs*:

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet1&highlight=4)]

### <a name="enable-two-factor-authentication"></a>Habilitar la autenticación en dos fases

Abra la *Views/Manage/Index.cshtml* archivo de vista Razor y quite el comentario de caracteres (por lo que ningún tipo de marcado es almohadilla).

## <a name="log-in-with-two-factor-authentication"></a>Inicie sesión con la autenticación en dos fases

* Ejecutar la aplicación y registrar un nuevo usuario

![Vista de registro de aplicación Web abierta en Microsoft Edge](2fa/_static/login2fa1.png)

* Puntee en el nombre de usuario, activa la `Index` métodos de acción de controlador de administrar. A continuación, puntee en el número de teléfono **agregar** vínculo.

![Administrar la vista](2fa/_static/login2fa2.png)

* Agregar un número de teléfono que recibirá el código de comprobación y pulse **enviar código de comprobación**.

![Agregar página de número de teléfono](2fa/_static/login2fa3.png)

* Obtendrá un mensaje de texto con el código de comprobación. Escríbala y pulse **enviar**

![Compruebe la página de número de teléfono](2fa/_static/login2fa4.png)

Si no recibe un mensaje de texto, consulte la página de registro de twilio.

* La vista gestionar muestra que el número de teléfono se agregó correctamente.

![Administrar la vista](2fa/_static/login2fa5.png)

* Pulse **habilitar** para habilitar la autenticación en dos fases.

![Administrar la vista](2fa/_static/login2fa6.png)

### <a name="test-two-factor-authentication"></a>Autenticación de dos factores de prueba

* Cierre la sesión.

* Inicia sesión.

* La cuenta de usuario ha habilitado la autenticación en dos fases, por lo que tendrá que proporcionar el segundo factor de autenticación. En este tutorial se ha habilitado la verificación por teléfono. Las plantillas creadas en también le permiten configurar el correo electrónico como el segundo factor. Puede configurar factores de segundo adicionales para la autenticación como códigos QR. Pulse **enviar**.

![Enviar vista de código de comprobación](2fa/_static/login2fa7.png)

* Escriba el código que se obtienen en el mensaje SMS.

* Al hacer clic en el **recordar este explorador** casilla de verificación se excluya de la necesidad de usar 2FA para iniciar sesión cuando se usa el mismo dispositivo y el explorador. Habilitar 2FA y haciendo clic en **recordar este explorador** le proporcionará protección segura 2FA de usuarios malintencionados que intenta acceder a su cuenta, siempre y cuando no tienen acceso al dispositivo. Puede hacerlo en cualquier dispositivo privada que se usan con frecuencia. Estableciendo **recordar este explorador**, obtener la seguridad adicional de 2FA desde dispositivos que no use con regularidad y obtener la comodidad de no tener que pasar por 2FA en sus propios dispositivos.

![Comprobar la vista](2fa/_static/login2fa8.png)

## <a name="account-lockout-for-protecting-against-brute-force-attacks"></a>Bloqueo de cuenta para protegerse contra los ataques por fuerza bruta

Se recomienda el bloqueo de cuenta con 2FA. Una vez que un usuario inicia sesión a través de una cuenta local o sociales, se almacena cada intento fallido en 2FA. Si se alcanza los intentos de acceso erróneos máximo, el usuario está bloqueado (valor predeterminado: bloqueo de 5 minutos después de 5 intentos de acceso). Una autenticación correcta restablece el número de intentos de acceso erróneos y restablece el reloj. El valor máximo intentos de acceso y se puede establecer el tiempo de bloqueo con [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) y [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan). El siguiente ejemplo configura el bloqueo de cuenta de 10 minutos tras 10 intentos de acceso:

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet2&highlight=13-17)]

Confirme que [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync) establece `lockoutOnFailure` a `true`:

```csharp
var result = await _signInManager.PasswordSignInAsync(
                 Input.Email, Input.Password, Input.RememberMe, lockoutOnFailure: true);
```
