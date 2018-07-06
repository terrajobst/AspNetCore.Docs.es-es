---
uid: identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
title: Autenticación en dos fases mediante SMS y correo electrónico con ASP.NET Identity | Microsoft Docs
author: HaoK
description: Este tutorial le mostrará cómo configurar la autenticación en dos fases (2FA) mediante SMS y correo electrónico. En este artículo se escribió por Rick Anderson ( @RickAndMSFT ), por...
ms.author: aspnetcontent
ms.date: 09/15/2015
ms.assetid: 053e23c4-13c9-40fa-87cb-3e9b0823b31e
msc.legacyurl: /identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: c1f4bd4a3f65d4b7fccd86214fd0ba45c891e390
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807627"
---
<a name="two-factor-authentication-using-sms-and-email-with-aspnet-identity"></a>Autenticación en dos fases mediante SMS y correo electrónico con ASP.NET Identity
====================
por [Hao Kung](https://github.com/HaoK), [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://github.com/Rick-Anderson), [Suhas Joshi](https://github.com/suhasj)

> Este tutorial le mostrará cómo configurar la autenticación en dos fases (2FA) mediante SMS y correo electrónico.
> 
> En este artículo se escribió por Rick Anderson ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)), Pranav Rastogi ([@rustd](https://twitter.com/rustd)), Hao Kung y Suhas Joshi. El ejemplo de NuGet se ha escrito principalmente por Hao Kung.


En este tema se trata los siguientes:

- [Compilando el ejemplo de identidad](#build)
- [Configuración de SMS para la autenticación en dos fases](#SMS)
- [Habilitar la autenticación en dos fases](#enable2)
- [Cómo registrar un proveedor de autenticación de dos fases](#reg)
- [Combinar las cuentas de inicio de sesión social y local](#combine)
- [Bloqueo de cuenta frente a ataques de fuerza bruta](#lock)

<a id="build"></a>

## <a name="building-the-identity-sample"></a>Compilando el ejemplo de identidad

En esta sección, usará NuGet para descargar un ejemplo que vamos a trabajar con. Comience por instalar y ejecutar [Visual Studio Express 2013 para Web](https://go.microsoft.com/fwlink/?LinkId=299058) o [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Instalar Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) o superior.

> [!NOTE]
> Advertencia: Debe instalar Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) para completar este tutorial.


1. Cree un nuevo ***vacía*** proyecto Web de ASP.NET.
2. En la consola de administrador de paquetes, escriba lo siguiente los siguientes comandos:  
  
    `Install-Package SendGrid`  
    `Install-Package -Prerelease Microsoft.AspNet.Identity.Samples`  
  
   En este tutorial, vamos a usar [SendGrid](http://sendgrid.com/) para enviar correo electrónico y [Twilio](https://www.twilio.com/) o [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) de texto sms. El `Identity.Samples` paquete instala el código que se trabajará con.
3. Establecer el [proyecto para usar SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
4. *Opcional*: siga las instrucciones de mi [tutorial de confirmación de correo electrónico](account-confirmation-and-password-recovery-with-aspnet-identity.md) para enlazar SendGrid y, a continuación, ejecute la aplicación y registrar una cuenta de correo electrónico.
5. * Opcional: * eliminar el código de confirmación del vínculo de correo electrónico de demostración de la muestra (el `ViewBag.Link` código en el controlador de cuentas. Consulte la `DisplayEmail` y `ForgotPasswordConfirmation` los métodos de acción y las vistas de razor).
6. <em>Opcional: * quite el `ViewBag.Status` código desde los controladores de administración y la cuenta y de la *Views\Account\VerifyCode.cshtml</em> y <em>Views\Manage\VerifyPhoneNumber.cshtml</em> las vistas de razor. Como alternativa, puede mantener el `ViewBag.Status` display para probar cómo funciona esta aplicación localmente sin tener que enlaza y enviar correo electrónico y mensajes SMS.

> [!NOTE]
> Advertencia: Si cambia la configuración de seguridad en este ejemplo, las aplicaciones de producción debe someterse a una auditoría de seguridad que se llama explícitamente a los cambios realizados.


<a id="SMS"></a>

## <a name="set-up-sms-for-two-factor-authentication"></a>Configuración de SMS para la autenticación en dos fases

Este tutorial proporciona instrucciones para el uso de Twilio o ASPSMS, pero puede usar cualquier otro proveedor SMS.

1. **Creación de una cuenta de usuario con un proveedor de SMS**  
  
   Crear un [Twilio](https://www.twilio.com/try-twilio) o un [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) cuenta.
2. **Instalar paquetes adicionales o agregar referencias de servicio**  
  
   Twilio:  
   En la consola de administrador de paquetes, escriba el siguiente comando:  
    `Install-Package Twilio`  
  
   ASPSMS:  
   Se debe agregar la referencia de servicio siguientes:  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image1.png)  
  
   Dirección:  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
   Espacio de nombres:  
    `ASPSMSX2`
3. **Averiguar las credenciales de usuario del proveedor de SMS**  
  
   Twilio:  
   Desde el **panel** pestaña de la cuenta de Twilio, copia el **SID de cuenta** y **Auth token**.  
  
   ASPSMS:  
   En la configuración de su cuenta, vaya a **Userkey** y cópielo junto con su autodefinido **contraseña**.  
  
   Más adelante, almacenaremos estos valores en las variables `SMSAccountIdentification` y `SMSAccountPassword` .
4. **Especifica el Id. de remitente / originador**  
  
   Twilio:  
   Desde el **números** pestaña, copie el número de teléfono de Twilio.  
  
   ASPSMS:  
   Dentro de la **desbloquear originadores** menú, desbloquear uno o varios de los originadores o elija un originador alfanumérico (no admite todas las redes).  
  
   Más adelante, almacenaremos este valor en la variable `SMSAccountFrom` .
5. **Transferir las credenciales del proveedor SMS en la aplicación**  
  
   Disponer de las credenciales y el número de teléfono del remitente a la aplicación:

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample1.cs)]

    > [!WARNING]
    > Seguridad: nunca almacene de datos confidenciales en el código fuente. La cuenta y las credenciales se agregan al código anterior para simplificar el ejemplo. Consulte de Jon Atten [ASP.NET MVC: mantener privados configuración fuera del Control de código fuente](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).
6. **Implementación de transferencia de datos al proveedor SMS**  
  
   Configurar la `SmsService` clase en el *aplicación\_Start\IdentityConfig.cs* archivo.  
  
   En función del proveedor SMS usado activar cualquiera el **Twilio** o **ASPSMS** sección: 

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample2.cs)]
7. Ejecute la aplicación e inicie sesión con la cuenta que registró anteriormente.
8. Haga clic en el identificador de usuario, que activa el `Index` los métodos de acción `Manage` controlador.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image2.png)
9. Haga clic en Agregar.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image3.png)
10. En unos pocos segundos obtendrá un mensaje de texto con el código de verificación. Escríbala y presione **enviar**.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image4.png)
11. La vista de administración se muestra que el número de teléfono se ha agregado.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image5.png)

### <a name="examine-the-code"></a>Examine el código

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample3.cs?highlight=2)]

El `Index` los métodos de acción `Manage` controlador establece el mensaje de estado en función de la acción anterior y se proporcionan vínculos para cambiar la contraseña local o agregar una cuenta local. El `Index` método también muestra el estado o su 2FA número, inicios de sesión externos, 2FA habilitado por teléfono y recuerde método 2FA para este explorador (se explica más adelante). Al hacer clic en el identificador de usuario (correo electrónico) en la barra de título no pasa un mensaje. Al hacer clic en el **número de teléfono: quitar** enlace pasadas `Message=RemovePhoneSuccess` como una cadena de consulta.  
  
`https://localhost:44300/Manage?Message=RemovePhoneSuccess`

[![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image6.png)]

El `AddPhoneNumber` método de acción muestra un cuadro de diálogo para especificar un número de teléfono que pueda recibir mensajes SMS.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample4.cs)]

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image7.png)

Al hacer clic en el **enviar código de verificación** botón devuelve el número de teléfono a la operación HTTP POST `AddPhoneNumber` método de acción.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample5.cs?highlight=12)]

El `GenerateChangePhoneNumberTokenAsync` método genera el token de seguridad que se establecerá en el mensaje SMS. Si se ha configurado el servicio SMS, el token se envía como la cadena &quot;es el código de seguridad &lt;token&gt;&quot;. El `SmsService.SendAsync` se llama al método en forma asincrónica y, después, la aplicación se redirige a la `VerifyPhoneNumber` método de acción (que muestra el siguiente cuadro de diálogo), donde puede escribir el código de verificación.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image8.png)

Una vez que escriba el código y haga clic en enviar, se registra el código en la operación HTTP POST `VerifyPhoneNumber` método de acción.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample6.cs)]

El `ChangePhoneNumberAsync` método comprueba el código de seguridad registrado. Si el código es correcto, el número de teléfono se agrega a la `PhoneNumber` campo de la `AspNetUsers` tabla. Si esa llamada se realiza correctamente, el `SignInAsync` se llama al método:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample7.cs)]

El `isPersistent` parámetro establece si la sesión de autenticación se conserva entre varias solicitudes.

Cuando se cambia su perfil de seguridad, se genera un nuevo sello de seguridad y se almacenan en el `SecurityStamp` campo de la *AspNetUsers* tabla. Tenga en cuenta que el `SecurityStamp` campo es diferente de la cookie de seguridad. La cookie de seguridad no se almacena en el `AspNetUsers` tabla (o en cualquier parte de la base de datos de identidad). El token de cookie de seguridad es autofirmado mediante [DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx) y se crea con el `UserId, SecurityStamp` e información de tiempo de expiración.

El middleware de cookies, comprueba la cookie en cada solicitud. El `SecurityStampValidator` método en el `Startup` clase llega a la base de datos y comprueba el sello de seguridad periódicamente, como se especifica en el `validateInterval`. Esto solo se produce cada 30 minutos (en nuestro ejemplo) a menos que cambie su perfil de seguridad. Se eligió el intervalo de 30 minutos para reducir los viajes a la base de datos.

El `SignInAsync` método debe llamarse cuando se realiza cualquier cambio en el perfil de seguridad. Cuando se cambia el perfil de seguridad, la base de datos es actualizaciones el `SecurityStamp` campo y sin llamar a la `SignInAsync` está registrada en el método *sólo* llega hasta la próxima vez que la canalización de OWIN a la base de datos (el `validateInterval`). Puede probar esto cambiando la `SignInAsync` método para devolver de inmediato y la configuración de la cookie `validateInterval` propiedad de 30 minutos a 5 segundos:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample8.cs?highlight=3)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample9.cs?highlight=20-21)]

Con los cambios de código anterior, puede cambiar su perfil de seguridad (por ejemplo, cambiando el estado de **dos factores habilitados**) y le cerrará en 5 segundos cuando la `SecurityStampValidator.OnValidateIdentity` método produce un error. Quitar la línea de retorno en la `SignInAsync` método, cambiar otro perfil de seguridad y no se ha iniciado. El `SignInAsync` método genera una nueva cookie de seguridad.

<a id="enable2"></a>

## <a name="enable-two-factor-authentication"></a>Habilitar la autenticación en dos fases

En la aplicación de ejemplo, deberá usar la interfaz de usuario para habilitar la autenticación en dos fases (2FA). Para habilitar 2FA, haga clic en el identificador de usuario (alias de correo electrónico) en la barra de navegación.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image9.png)  
Haga clic en Habilitar 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image10.png) Cerrar sesión y, a continuación, volver a iniciar sesión. Si ha activado el correo electrónico (consulte mi [tutorial anterior](account-confirmation-and-password-recovery-with-aspnet-identity.md)), puede seleccionar el SMS o correo electrónico para 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image11.png) Se muestra la página de códigos de comprobación donde puede escribir el código (de SMS o correo electrónico).![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image12.png) Al hacer clic en el **recordar este explorador** casilla eximirán de la necesidad de utilizar 2FA para iniciar sesión con ese equipo y el explorador. Habilitar 2FA y haga clic en el **recordar este explorador** le proporcionará 2FA fuerte protección contra usuarios malintencionados que intenta acceder a su cuenta, siempre que no tienen acceso a su equipo. Puede hacerlo en cualquier equipo privado que se usan con frecuencia. Estableciendo **recordar este explorador**, obtendrá la seguridad añadida de 2FA desde equipos corrientemente no usamos y obtener la comodidad de no tener que pasar por 2FA en sus propios equipos. 

<a id="reg"></a>
## <a name="how-to-register-a-two-factor-authentication-provider"></a>Cómo registrar un proveedor de autenticación de dos fases

Cuando se crea un nuevo proyecto MVC, el *IdentityConfig.cs* archivo contiene el código siguiente para registrar un proveedor de autenticación de dos factores:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample10.cs?highlight=22-35)]

## <a name="add-a-phone-number-for-2fa"></a>Agregar un número de teléfono para 2FA

El `AddPhoneNumber` método de acción en el `Manage` controlador genera un token de seguridad y envíos para el teléfono número que se han proporcionado.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample11.cs)]

Después de enviar el token, redirige a la `VerifyPhoneNumber` método de acción, donde puede escribir el código para el registro de SMS para 2FA. No se utiliza SMS 2FA hasta que haya comprobado el número de teléfono.

## <a name="enabling-2fa"></a>Habilitar 2FA

El `EnableTFA` 2FA permite que el método de acción:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample12.cs)]

Tenga en cuenta el `SignInAsync` debe llamarse porque enable 2FA es un cambio en el perfil de seguridad. Cuando se habilita 2FA, el usuario deberá usar 2FA para iniciar sesión, utilizando los enfoques 2FA registraron (SMS y correo electrónico en el ejemplo).

Puede agregar más proveedores 2FA, como los generadores de código QR o puede escribir la propia (consulte [usando Google Authenticator con ASP.NET Identity](http://www.beabigrockstar.com/blog/using-google-authenticator-asp-net-identity/)).

> [!NOTE]
> Los códigos 2FA se generan mediante [algoritmo de la contraseña de un solo uso basado en tiempo](http://en.wikipedia.org/wiki/Time-based_One-time_Password_Algorithm) y códigos son válidos durante seis minutos. Si tiene más de seis minutos para escribir el código, obtendrá un mensaje de error de código no válido.


<a id="combine"></a>

## <a name="combine-social-and-local-login-accounts"></a>Combinar las cuentas de inicio de sesión social y local

Puede combinar las cuentas locales y redes sociales, haga clic en el vínculo de correo electrónico. En la siguiente secuencia &quot; RickAndMSFT@gmail.com &quot; en primer lugar se crea como un inicio de sesión local, pero puede crear la cuenta como un inicio de sesión social primero y luego agregar un inicio de sesión local.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image13.png)

Haga clic en el **administrar** vínculo. Tenga en cuenta la externa 0 (inicios de sesión sociales) asociado con esta cuenta.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image14.png)

Haga clic en el vínculo a otro registro en el servicio y acepte las solicitudes de aplicación. Se han combinado las dos cuentas, podrá iniciar sesión con las cuentas. Es posible que desee que los usuarios agreguen cuentas locales en caso de su registro en el servicio de autenticación social esté inactivo o que es más probable hayan perdido el acceso a su cuenta de redes sociales.

En la siguiente imagen, Tom es un inicio de sesión social (que puede ver desde el **inicios de sesión externos: 1** se muestra en la página).

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image15.png)

Al hacer clic en **elige una contraseña** le permite agregar un registro local en asociada con la misma cuenta.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image16.png)

<a id="lock"></a>

## <a name="account-lockout-from-brute-force-attacks"></a>Bloqueo de cuenta frente a ataques de fuerza bruta

Puede proteger las cuentas en su aplicación frente a ataques de diccionario habilitando el bloqueo de usuario. El siguiente código en el `ApplicationUserManager Create` método configura de bloqueo:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample13.cs)]

El código anterior permite el bloqueo de solo autenticación en dos fases. Aunque puede permitir bloqueo de inicios de sesión cambiando `shouldLockout` en true en el `Login` método del controlador de cuenta, se recomienda no habilite de bloqueo para inicios de sesión porque hace susceptible a la cuenta [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) inicio de sesión los ataques. En el código de ejemplo, el bloqueo está deshabilitado para la cuenta de administrador creada en el `ApplicationDbInitializer Seed` método:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample14.cs?highlight=19)]

## <a name="requiring-a-user-to-have-a-validated-email-account"></a>Solicitar al usuario que tiene una cuenta de correo electrónico validada

El código siguiente requiere que un usuario tenga una cuenta de correo electrónico validada antes de poder iniciar:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample15.cs?highlight=8-17)]

## <a name="how-signinmanager-checks-for-2fa-requirement"></a>Cómo SignInManager busca requisito 2FA

Registro local en tanto el inicio de sesión social Compruebe si está habilitado 2FA. Si está habilitado 2FA, el `SignInManager` devuelve el método de inicio de sesión `SignInStatus.RequiresVerification`, y el usuario será redirigido a la `SendCode` método de acción, donde tendrán que escribir el código para completar el registro de la secuencia. Si el usuario tiene RememberMe se establece en la cookie a los usuarios locales, el `SignInManager` devolverá `SignInStatus.Success` y no tendrá que pasar por 2FA.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample16.cs?highlight=20-22)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample17.cs?highlight=10-11,17-18)]

El código siguiente muestra el `SendCode` método de acción. Un [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) se crea con todos los métodos de 2FA habilitados para el usuario. El [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) se pasa a la [DropDownListFor](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist.aspx) auxiliares, lo que permite al usuario seleccionar el enfoque 2FA (normalmente, el correo electrónico y SMS).

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample18.cs)]

Una vez que el usuario envía el enfoque 2FA, el `HTTP POST SendCode` se denomina método de acción, el `SignInManager` envía el código 2FA y el usuario se redirige a la `VerifyCode` donde puede escribir el código para completar el inicio de sesión en el método de acción.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample19.cs?highlight=3,13-14,18)]

## <a name="2fa-lockout"></a>Bloqueo de 2FA

Aunque puede establecer el bloqueo de cuenta en errores de intento de contraseña de inicio de sesión, ese enfoque hace que el inicio de sesión pueda sufrir [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) bloqueos. Se recomienda que usar el bloqueo de cuenta solo con 2FA. Cuando el `ApplicationUserManager` está creado, el código de ejemplo establece bloqueo 2FA y `MaxFailedAccessAttemptsBeforeLockout` a cinco. Una vez que un usuario inicia sesión (a través de una cuenta local o cuenta de redes sociales), se almacena cada intento incorrecto en 2FA y, si se alcanza el número máximo de intentos, el usuario se bloquea durante cinco minutos (puede establecer la hora con bloqueo `DefaultAccountLockoutTimeSpan`).

<a id="addRes"></a>

## <a name="additional-resources"></a>Recursos adicionales

- [Recursos recomendados de ASP.NET Identity](../getting-started/aspnet-identity-recommended-resources.md) por lo que vincula la lista completa de blogs, vídeos, tutoriales y great de identidad.
- [Aplicación de MVC 5 con Facebook, Twitter, LinkedIn y Google OAuth2 Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) también muestra cómo agregar información de perfil a la tabla de usuarios.
- [Identidad 2.0 y ASP.NET MVC: comprender los aspectos básicos](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) por John Atten.
- [Confirmación de la cuenta y contraseña de recuperación con ASP.NET Identity](account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [Introducción a ASP.NET Identity](../getting-started/introduction-to-aspnet-identity.md)
- [Anuncio de RTM de ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) de Pranav Rastogi.
- [Identidad de ASP.NET 2.0: Configuración de validación de la cuenta y la autorización de dos fases](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) por John Atten.
