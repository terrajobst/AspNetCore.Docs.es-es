---
uid: identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
title: "Autenticación en dos fases mediante SMS y correo electrónico con ASP.NET Identity | Documentos de Microsoft"
author: HaoK
description: "Este tutorial le mostrará cómo configurar la autenticación en dos fases (2FA) mediante correo electrónico y SMS. En este artículo se escribió Rick Anderson ( @RickAndMSFT ), por..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/15/2015
ms.topic: article
ms.assetid: 053e23c4-13c9-40fa-87cb-3e9b0823b31e
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: ecb1fc693063995a3a05a7af5db64554c9f595e2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="two-factor-authentication-using-sms-and-email-with-aspnet-identity"></a>Autenticación en dos fases mediante SMS y correo electrónico con la identidad de ASP.NET
====================
por [Hao Kung](https://github.com/HaoK), [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://github.com/Rick-Anderson), [Suhas Joshi](https://github.com/suhasj)

> Este tutorial le mostrará cómo configurar la autenticación en dos fases (2FA) mediante correo electrónico y SMS.
> 
> En este artículo se escribió Rick Anderson ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)), Pranav Rastogi ([@rustd](https://twitter.com/rustd)), Hao Kung y Suhas Joshi. El ejemplo de NuGet escribió principalmente Hao Kung.


En este tema se trata lo siguiente:

- [Generar el ejemplo de identidad](#build)
- [Configurar SMS para la autenticación en dos fases](#SMS)
- [Habilitar la autenticación en dos fases](#enable2)
- [Cómo registrar un proveedor de autenticación de dos factores](#reg)
- [Combinar las cuentas de inicio de sesión locales y redes sociales](#combine)
- [Bloqueo de la cuenta de ataques por fuerza bruta](#lock)

<a id="build"></a>

## <a name="building-the-identity-sample"></a>Generar el ejemplo de identidad

En esta sección, deberá usar NuGet para descargar un ejemplo con que vamos a trabajar. Empiece por instalar y ejecutar [Visual Studio Express 2013 para Web](https://go.microsoft.com/fwlink/?LinkId=299058) o [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Instalar Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) o superior.

> [!NOTE]
> Advertencia: Debe instalar Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) para completar este tutorial.


1. Crear un nuevo ***vacía*** proyecto Web de ASP.NET.
2. En la consola de administrador de paquetes, escriba lo siguiente los siguientes comandos:  
  
    `Install-Package SendGrid`  
    `Install-Package -Prerelease Microsoft.AspNet.Identity.Samples`  
  
 En este tutorial, usaremos [SendGrid](http://sendgrid.com/) para enviar correo electrónico y [Twilio](https://www.twilio.com/) o [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) de texto sms. El `Identity.Samples` paquete instala el código que se va a trabajar.
3. Establecer el [proyecto para usar SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
4. *Opcional*: siga las instrucciones que aparecen en mi [tutorial de confirmación de correo electrónico](account-confirmation-and-password-recovery-with-aspnet-identity.md) enlazar SendGrid y, a continuación, ejecutar la aplicación y registrar una cuenta de correo electrónico.
5. * Opcional: * eliminar el código de confirmación del vínculo de correo electrónico de demostración de la muestra (el `ViewBag.Link` código en el controlador de la cuenta. Consulte la `DisplayEmail` y `ForgotPasswordConfirmation` métodos de acción y vistas de razor).
6. * Opcional: * quitar el `ViewBag.Status` código desde los controladores de administración y la cuenta y de la *Views\Account\VerifyCode.cshtml* y *Views\Manage\VerifyPhoneNumber.cshtml* vistas razor. Como alternativa, puede mantener la `ViewBag.Status` presentación para probar cómo funciona esta aplicación localmente sin necesidad de enlazar y enviar correos electrónicos y mensajes SMS.

> [!NOTE]
> Advertencia: Si cambia cualquiera de las opciones de seguridad en este ejemplo, aplicaciones de ensayo deberá someterse a una auditoría de seguridad que se llame explícitamente a los cambios realizados.


<a id="SMS"></a>

## <a name="set-up-sms-for-two-factor-authentication"></a>Configurar SMS para la autenticación en dos fases

Este tutorial proporciona instrucciones para el uso de Twilio o ASPSMS, pero puede usar cualquier otro proveedor SMS.

1. **Crear una cuenta de usuario con un proveedor de SMS**  
  
 Crear un [Twilio](https://www.twilio.com/try-twilio) o un [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) cuenta.
2. **Instalación de paquetes adicionales o agregar referencias de servicio**  
  
 Twilio:  
 En la consola de administrador de paquetes, escriba el siguiente comando:  
    `Install-Package Twilio`  
  
 ASPSMS:  
 Es necesario agregar la siguiente referencia de servicio:  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image1.png)  
  
 Dirección:  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
 Espacio de nombres:  
    `ASPSMSX2`
3. **Pensar en las credenciales de usuario del proveedor de SMS**  
  
 Twilio:  
 Desde el **panel** ficha de la cuenta de Twilio, copia el **SID de cuenta** y **token de autenticación**.  
  
 ASPSMS:  
 Desde la configuración de su cuenta, vaya a **clave de usuario confidenciales** y copie junto con su propio definido **contraseña**.  
  
 Más adelante almacenaremos estos valores en las variables `SMSAccountIdentification` y `SMSAccountPassword` .
4. **Especifica el identificador del remitente / originador**  
  
 Twilio:  
 Desde el **números** ficha, copie el número de teléfono Twilio.  
  
 ASPSMS:  
 En el **desbloquear remitentes** menú, desbloquear uno o más remitentes o elija un originador alfanumérico (no admite todas las redes).  
  
 Se almacenará más adelante este valor en la variable `SMSAccountFrom` .
5. **Transferir las credenciales del proveedor SMS en la aplicación**  
  
 Disponer de las credenciales y el número de teléfono del remitente a la aplicación:

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample1.cs)]

    > [!WARNING]
    > Seguridad - nunca almacenar los datos confidenciales en el código fuente. La cuenta y las credenciales se agregan al código anterior para simplificar el ejemplo. Consulte de Jon Atten [MVC de ASP.NET: mantener privados configuración fuera del Control de código fuente](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).
6. **Implementación de transferencia de datos al proveedor SMS**  
  
 Configurar la `SmsService` clase en el *aplicación\_Start\IdentityConfig.cs* archivo.  
  
 En función del proveedor SMS usado activar cualquiera el **Twilio** o **ASPSMS** sección: 

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample2.cs)]
7. Ejecutar la aplicación e inicie sesión con la cuenta que se registraron anteriormente.
8. Haga clic en el identificador de usuario, que activa el `Index` método de acción en `Manage` controlador.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image2.png)
9. Haga clic en Agregar.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image3.png)
10. En unos segundos, obtendrá un mensaje de texto con el código de comprobación. Escriba y presione **enviar**.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image4.png)
11. La vista gestionar muestra que el número de teléfono se ha agregado.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image5.png)

### <a name="examine-the-code"></a>Examine el código

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample3.cs?highlight=2)]

El `Index` método de acción en `Manage` controlador establece el mensaje de estado en función de la acción anterior y se proporcionan vínculos para cambiar la contraseña local o agregar una cuenta local. El `Index` método también muestra el estado o la 2FA número, inicios de sesión externos, 2FA habilitado por teléfono y recuerde 2FA método para este explorador (se explica más adelante). Al hacer clic en el identificador del usuario (correo electrónico) en la barra de título no pasa un mensaje. Al hacer clic en el **número de teléfono: quitar** enlace pasadas `Message=RemovePhoneSuccess` como una cadena de consulta.  
  
`https://localhost:44300/Manage?Message=RemovePhoneSuccess`

[![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image6.png)]

El `AddPhoneNumber` método de acción muestra un cuadro de diálogo para especificar un número de teléfono que puede recibir mensajes SMS.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample4.cs)]

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image7.png)

Al hacer clic en el **enviar código de comprobación** botón devuelve el número de teléfono para HTTP POST `AddPhoneNumber` método de acción.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample5.cs?highlight=12)]

El `GenerateChangePhoneNumberTokenAsync` método genera el token de seguridad que se establecerá en el mensaje SMS. Si se ha configurado el servicio SMS, el token se envía como la cadena &quot;es el código de seguridad &lt;token&gt;&quot;. El `SmsService.SendAsync` método al que se llama de forma asincrónica, la aplicación se redirigirá a la `VerifyPhoneNumber` método de acción (que muestra el siguiente cuadro de diálogo), donde puede escribir el código de comprobación.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image8.png)

Una vez que escriba el código y haga clic en enviar, se registra el código para HTTP POST `VerifyPhoneNumber` método de acción.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample6.cs)]

El `ChangePhoneNumberAsync` método comprueba el código de seguridad expuesto. Si el código es correcto, el número de teléfono se agrega a la `PhoneNumber` campo de la `AspNetUsers` tabla. Si esa llamada se realiza correctamente, el `SignInAsync` se llama al método:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample7.cs)]

El `isPersistent` parámetro establece si la sesión de autenticación se conserva entre varias solicitudes.

Cuando se cambia el perfil de seguridad, un nuevo sello de seguridad se generan y almacenan en la `SecurityStamp` campo de la *AspNetUsers* tabla. Tenga en cuenta que el `SecurityStamp` campo es diferente de la cookie de seguridad. La cookie de seguridad no se almacena en la `AspNetUsers` tabla (o cualquier otra estructura en la base de datos de identidad en). El token de cookie de seguridad es autofirmado mediante [DPAPI](https://msdn.microsoft.com/en-us/library/system.security.cryptography.protecteddata.aspx) y se crea con el `UserId, SecurityStamp` e información de tiempo de expiración.

El middleware de cookies, comprueba la cookie en cada solicitud. El `SecurityStampValidator` método en el `Startup` clase llega a la base de datos y comprueba el sello de seguridad de forma periódica, según se haya especificado con el `validateInterval`. Esto sólo ocurre cada 30 minutos (en nuestro ejemplo) a menos que cambie el perfil de seguridad. El intervalo de 30 minutos se ha elegido para reducir los viajes a la base de datos.

El `SignInAsync` método debe llamarse cuando se realiza cualquier cambio en el perfil de seguridad. Cuando se cambia el perfil de seguridad, la base de datos es de actualizaciones el `SecurityStamp` campo y sin llamar a la `SignInAsync` se mantiene la sesión en el método *solo* hasta la próxima vez que la canalización OWIN llega a la base de datos (el `validateInterval`). Puede probar esto cambiando la `SignInAsync` método para devolver resultados inmediatamente y establecer la cookie `validateInterval` propiedad de 30 minutos en 5 segundos:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample8.cs?highlight=3)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample9.cs?highlight=20-21)]

Con los cambios de código anterior, puede cambiar el perfil de seguridad (por ejemplo, al cambiar el estado de **dos fases habilitado**) y se registra la en 5 segundos cuando la `SecurityStampValidator.OnValidateIdentity` método se produce un error. Quite la línea de retorno en la `SignInAsync` (método), cambiar otro perfil de seguridad y no se registrarán. El `SignInAsync` método genera una nueva cookie de seguridad.

<a id="enable2"></a>

## <a name="enable-two-factor-authentication"></a>Habilitar la autenticación en dos fases

En la aplicación de ejemplo, debe usar la interfaz de usuario para habilitar la autenticación en dos fases (2FA). Para habilitar 2FA, haga clic en el identificador de usuario (alias de correo electrónico) en la barra de navegación.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image9.png)  
Haga clic en Habilitar 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image10.png) Cierre sesión y, a continuación, volver a iniciar sesión. Si ha habilitado el correo electrónico (consulte mi [tutorial anterior](account-confirmation-and-password-recovery-with-aspnet-identity.md)), puede seleccionar el SMS o correo electrónico para 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image11.png) Se muestra la página de códigos comprobar donde puede escribir el código (de SMS o correo electrónico).![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image12.png) Al hacer clic en el **recordar este explorador** casilla de verificación se excluya de la necesidad de usar 2FA para iniciar sesión con ese equipo y el explorador. Habilitar 2FA y haciendo clic en el **recordar este explorador** le proporcionará protección segura 2FA de usuarios malintencionados que intenta acceder a su cuenta, siempre y cuando no tienen acceso a su equipo. Puede hacerlo en cualquier equipo privado que se usan con frecuencia. Estableciendo **recordar este explorador**, obtendrá la seguridad añadida de 2FA de equipos que no use con regularidad y obtener la comodidad de no tener que pasar por 2FA en sus propios equipos. 

<a id="reg"></a>
## <a name="how-to-register-a-two-factor-authentication-provider"></a>Cómo registrar un proveedor de autenticación de dos factores

Cuando se crea un nuevo proyecto MVC, el *IdentityConfig.cs* archivo contiene el código siguiente para registrar un proveedor de autenticación de dos factores:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample10.cs?highlight=22-35)]

## <a name="add-a-phone-number-for-2fa"></a>Agregar un número de teléfono para 2FA

El `AddPhoneNumber` método de acción en el `Manage` controlador genera un token de seguridad y la envía para el teléfono número que se ha proporcionado.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample11.cs)]

Después de enviar el token, redirige a la `VerifyPhoneNumber` método de acción, donde puede escribir el código para registrar SMS para 2FA. No se utiliza SMS 2FA hasta que haya comprobado el número de teléfono.

## <a name="enabling-2fa"></a>Habilitar 2FA

El `EnableTFA` método de acción permite 2FA:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample12.cs)]

Tenga en cuenta el `SignInAsync` debe llamarse porque habilitar 2FA es un cambio en el perfil de seguridad. Cuando 2FA está habilitada, el usuario deberá usar 2FA para iniciar sesión, utilizando los métodos de 2FA se hayan registrado (SMS y correo electrónico en el ejemplo).

Puede agregar más proveedores 2FA como generadores de código QR o puede escribir su propiedad (vea [usando Google Authenticator con ASP.NET Identity](http://www.beabigrockstar.com/blog/using-google-authenticator-asp-net-identity/)).

> [!NOTE]
> Los códigos de 2FA se generan mediante [algoritmo de contraseña de un solo uso basado en tiempo](http://en.wikipedia.org/wiki/Time-based_One-time_Password_Algorithm) y códigos son válidos durante seis minutos. Si tiene más de seis minutos para escribir el código, obtendrá un mensaje de error de código no válido.


<a id="combine"></a>

## <a name="combine-social-and-local-login-accounts"></a>Combinar las cuentas de inicio de sesión locales y redes sociales

Puede combinar cuentas locales y redes sociales, haga clic en el vínculo de correo electrónico. En la siguiente secuencia &quot; RickAndMSFT@gmail.com &quot; en primer lugar se crea como un inicio de sesión local, pero puede crear la cuenta como un inicio de sesión social primero y luego agregar un inicio de sesión local.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image13.png)

Haga clic en el **administrar** vínculo. Tenga en cuenta el externo 0 (inicios de sesión sociales) asociados con esta cuenta.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image14.png)

Haga clic en el vínculo en otro registro de servicio y Aceptar las solicitudes de aplicación. Se han combinado las dos cuentas, podrá iniciar sesión con cualquiera de estas cuentas. Puede que los usuarios agreguen cuentas locales en caso de que su registro sociales en servicio de autenticación está inactivo o, más probable es que ha perdido el acceso a su cuenta sociales.

En la siguiente imagen, Tom es un inicio de sesión social (que puede ver desde el **inicios de sesión externos: 1** se muestra en la página).

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image15.png)

Al hacer clic en **elegir una contraseña** le permite agregar un registro local en asociados con la misma cuenta.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image16.png)

<a id="lock"></a>

## <a name="account-lockout-from-brute-force-attacks"></a>Bloqueo de la cuenta de ataques por fuerza bruta

Puede proteger las cuentas en la aplicación frente a ataques de diccionario habilitando el bloqueo de usuario. El siguiente código en el `ApplicationUserManager Create` método configura de bloqueo:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample13.cs)]

El código anterior permite el bloqueo para la autenticación en dos fases solo. Si bien puede permitir bloqueo de inicios de sesión cambiando `shouldLockout` en true en la `Login` método de controlador de la cuenta, se recomienda no habilitar de bloqueo para inicios de sesión ya que hace que la cuenta sea susceptible a [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) inicio de sesión los ataques. En el código de ejemplo, el bloqueo está deshabilitado para la cuenta de administrador que se crea en el `ApplicationDbInitializer Seed` método:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample14.cs?highlight=19)]

## <a name="requiring-a-user-to-have-a-validated-email-account"></a>Obligar al usuario a tiene una cuenta de correo electrónico validado

El código siguiente requiere que un usuario tiene una cuenta de correo electrónico validada antes de poder iniciar sesión:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample15.cs?highlight=8-17)]

## <a name="how-signinmanager-checks-for-2fa-requirement"></a>¿Cómo SignInManager busca 2FA requisito

El inicio de sesión local tanto el registro sociales de comprobación para ver si 2FA está habilitado. Si está habilitado 2FA, la `SignInManager` devuelve el método de inicio de sesión `SignInStatus.RequiresVerification`, y el usuario será redirigido a la `SendCode` método de acción, donde tendrán que escribir el código para completar el inicio de sesión en secuencia. Si el usuario tiene RememberMe se establece en la cookie local del usuario, el `SignInManager` devolverá `SignInStatus.Success` y no tendrá que pasar por 2FA.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample16.cs?highlight=20-22)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample17.cs?highlight=10-11,17-18)]

El código siguiente muestra el `SendCode` método de acción. A [SelectListItem](https://msdn.microsoft.com/en-us/library/system.web.mvc.selectlistitem.aspx) se crea con todos los métodos de 2FA habilitados para el usuario. El [SelectListItem](https://msdn.microsoft.com/en-us/library/system.web.mvc.selectlistitem.aspx) se pasa a la [DropDownListFor](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.dropdownlist.aspx) aplicación auxiliar, lo que permite al usuario seleccionar el enfoque de 2FA (normalmente el correo electrónico y SMS).

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample18.cs)]

Una vez que el usuario envía el enfoque 2FA, la `HTTP POST SendCode` se llama el método de acción, el `SignInManager` envía el código de 2FA y el usuario se redirige a la `VerifyCode` método de acción en el que pueden especificar el código para completar el inicio de sesión.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample19.cs?highlight=3,13-14,18)]

## <a name="2fa-lockout"></a>Bloqueo de 2FA

Aunque puede establecer el bloqueo de cuenta en los errores de intento de contraseña de inicio de sesión, este enfoque hace susceptible a su inicio de sesión [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) bloqueos. Se recomienda que usar el bloqueo de cuenta con 2FA. Cuando el `ApplicationUserManager` está creado, el código de ejemplo establece bloqueo 2FA y `MaxFailedAccessAttemptsBeforeLockout` a cinco. Una vez que un usuario inicia sesión (a través de cuenta local o sociales), se almacena cada intento fallido en 2FA y si se alcanza el número máximo de intentos, el usuario se bloquea durante cinco minutos (puede establecer el bloqueo de hora con `DefaultAccountLockoutTimeSpan`).

<a id="addRes"></a>

## <a name="additional-resources"></a>Recursos adicionales

- [ASP.NET Identity recomienda recursos](../getting-started/aspnet-identity-recommended-resources.md) por lo que vincula la lista completa de identidad blogs, vídeos, tutoriales y muy bien.
- [Aplicación de MVC 5 con Facebook, Twitter, LinkedIn y Google OAuth2 Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) también muestra cómo agregar información de perfil a la tabla de usuarios.
- [ASP.NET MVC e identidad 2.0: comprende los fundamentos](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) por John Atten.
- [Confirmación de cuenta y contraseña de recuperación con la identidad de ASP.NET](account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [Introducción a la identidad de ASP.NET](../getting-started/introduction-to-aspnet-identity.md)
- [Anuncio de RTM de ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) por Pranav Rastogi.
- [Identidad de ASP.NET 2.0: Configurar la validación de la cuenta y la autorización de dos fases](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) por John Atten.
