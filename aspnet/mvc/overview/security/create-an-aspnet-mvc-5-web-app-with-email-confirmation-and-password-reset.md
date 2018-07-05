---
uid: mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
title: Crear una aplicación web de ASP.NET MVC 5 segura con inicio de sesión, por correo electrónico de confirmación y restablecimiento de contraseña (C#) | Microsoft Docs
author: Rick-Anderson
description: Este tutorial muestra cómo compilar una aplicación web de ASP.NET MVC 5 con confirmación por correo electrónico y restablecimiento de contraseña mediante el sistema de pertenencia de ASP.NET Identity. Ca...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/26/2015
ms.topic: article
ms.assetid: d4911cb3-1afb-4805-b860-10818c4b1280
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
msc.type: authoredcontent
ms.openlocfilehash: 56c1a5c414fdcece8d827d1187144b4948d8eb93
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37387048"
---
<a name="create-a-secure-aspnet-mvc-5-web-app-with-log-in-email-confirmation-and-password-reset-c"></a>Crear una aplicación web de ASP.NET MVC 5 segura con inicio de sesión, por correo electrónico de confirmación y restablecimiento de contraseña (C#)
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> Este tutorial muestra cómo compilar una aplicación web de ASP.NET MVC 5 con confirmación por correo electrónico y restablecimiento de contraseña mediante el sistema de pertenencia de ASP.NET Identity. Puede descargar la aplicación completada [aquí](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952). La descarga contiene aplicaciones auxiliares de depuración que le permiten probar la confirmación por correo electrónico y SMS sin tener que configurar un correo electrónico o el proveedor de SMS.
> 
> En este tutorial se escribió por [Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).


<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a>Crear una aplicación ASP.NET MVC

Comience por instalar y ejecutar [Visual Studio Express 2013 para Web](https://go.microsoft.com/fwlink/?LinkId=299058) o [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Instalar [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) o superior.

> [!NOTE]
> Advertencia: Debe instalar [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) o superior para completar este tutorial.


1. Cree un nuevo proyecto Web ASP.NET y seleccione la plantilla MVC. Formularios Web Forms también es compatible con ASP.NET Identity, por lo que podría seguir pasos similares en una aplicación de formularios web.  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image1.png)
2. Deje la autenticación predeterminada como **cuentas de usuario individuales**. Si desea hospedar la aplicación en Azure, deje activada la casilla de verificación. Más adelante en el tutorial se implementará en Azure. También puede [abrir una cuenta de Azure de forma gratuita](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).
3. Establecer el [proyecto para usar SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
4. Ejecute la aplicación, haga clic en el **registrar** vincular y registrar un usuario. En este momento, es la única validación en el correo electrónico con el [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) atributo.
5. En el Explorador de servidores, vaya a **datos Connections\DefaultConnection\Tables\AspNetUsers**, haga clic y seleccione **Abrir definición de tabla**.

    La siguiente imagen muestra la `AspNetUsers` esquema:

    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image2.png)
6. Haga clic con el botón derecho en el **AspNetUsers** de tabla y seleccione **mostrar datos de tabla**.  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image3.png)  
 En este momento, no se ha confirmado el correo electrónico.
7. Haga clic en la fila y seleccione Eliminar. Se agregará este correo electrónico nuevo en el paso siguiente y enviar un correo electrónico de confirmación.

## <a name="email-confirmation"></a>Confirmación por correo electrónico

Es una práctica recomendada para confirmar el correo electrónico de un nuevo registro de usuario para comprobar que no están suplantando persona (es decir, se hayan registrado con el correo electrónico de otra persona). Supongamos que tenemos un foro de discusión, ¿puede evitar `"bob@example.com"` registrar como `"joe@contoso.com"`. Sin confirmación por correo electrónico, `"joe@contoso.com"` podría obtener correo electrónico no deseado de su aplicación. Supongamos que Bob accidentalmente registrado como `"bib@example.com"` y se había dado cuenta, no sería capaz de usar la recuperación de contraseña porque la aplicación no tiene su correo electrónico correcta. Confirmación por correo electrónico proporciona una protección limitada solo desde los bots y no proporciona protección de correo basura determinado, tienen muchos alias de correo electrónico de trabajo que pueden usar para registrar.

Por lo general desea impedir que todos los datos del sitio web de registro antes de que se han confirmado por correo electrónico, un mensaje de texto SMS u otro mecanismo de nuevos usuarios. <a id="build"></a>En las secciones siguientes, se habilitará la confirmación por correo electrónico y modifique el código para evitar que los usuarios recién registrados iniciar sesión hasta que se ha confirmado el correo electrónico.

<a id="SG"></a>
## <a name="hook-up-sendgrid"></a>Enlazar SendGrid

Aunque este tutorial solo muestra cómo agregar la notificación de correo electrónico a través de [SendGrid](http://sendgrid.com/), puede enviar correo electrónico mediante SMTP y otros mecanismos (consulte [recursos adicionales](#addRes)).

1. En la consola de administrador de paquetes, escriba el siguiente comando: 

    [!code-console[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample1.cmd)]
2. Vaya a la [SendGrid de Azure página de registro](https://go.microsoft.com/fwlink/?linkid=271033&clcid=0x409) y registrarse para obtener una cuenta gratuita de SendGrid. Configuración de SendGrid mediante la adición de código similar al siguiente en *App_Start/IdentityConfig.cs*:

    [!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample2.cs?highlight=3,5)]

Necesita agregar que incluye lo siguiente:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample3.cs)]

Para simplificar este ejemplo, se almacenará la configuración de la aplicación en el *web.config* archivo:

[!code-xml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample4.xml)]

> [!WARNING]
> Seguridad: nunca almacene de datos confidenciales en el código fuente. La cuenta y las credenciales se almacenan en la appSetting. En Azure, puede almacenar con seguridad estos valores en el **[configurar](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** ficha en el portal de Azure. Consulte [procedimientos recomendados para implementar contraseñas y otros datos confidenciales en ASP.NET y Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).


### <a name="enable-email-confirmation-in-the-account-controller"></a>Habilitar la confirmación de correo electrónico en el controlador de cuentas

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample5.cs?highlight=16-21)]

Compruebe el *Views\Account\ConfirmEmail.cshtml* archivo tiene una sintaxis de razor correcto. (El @ caracteres en la primera línea es posible que falten. )

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample6.cshtml?highlight=1)]

Ejecute la aplicación y haga clic en el vínculo de registro. Una vez que envíe el formulario de registro, se registran en.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image4.png)

Compruebe su cuenta de correo electrónico y haga clic en el vínculo para confirmar su correo electrónico.

<a id="require"></a>
## <a name="require-email-confirmation-before-log-in"></a>Requerir confirmación por correo electrónico antes del inicio de sesión

Actualmente una vez que un usuario completa el formulario de registro, inician sesión. Por lo general desea confirmar su correo electrónico antes de registrarlos. En la sección siguiente, se modificará el código para exigir que los usuarios nuevos tener un correo electrónico de confirmación antes de registrarse (autenticado). Actualización de la `HttpPost Register` método con los cambios resaltados siguientes:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample7.cs?highlight=14-15,23-30)]

Marcando como comentario el `SignInAsync` método, el usuario no iniciará sesión en el registro. El `TempData["ViewBagLink"] = callbackUrl;` línea se puede usar para [depurar la aplicación](#dbg) y probar el registro sin enviar correo electrónico. `ViewBag.Message` se usa para mostrar las instrucciones de confirmación. El [Descargar ejemplo](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952) contiene código para probar la confirmación por correo electrónico sin configurar correo electrónico y también puede utilizarse para depurar la aplicación.

Crear un `Views\Shared\Info.cshtml` archivo y agregue el siguiente marcado de razor:

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample8.cshtml)]

Agregar el [atributo Authorize](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.118).aspx) a la `Contact` método de acción del controlador Home. Puede hacer clic en el **póngase en contacto con** vínculo para comprobar los usuarios anónimos no tienen acceso y los usuarios autenticados tengan acceso.

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample9.cs?highlight=1)]

También debe actualizar el `HttpPost Login` método de acción:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample10.cs?highlight=13-22)]

Actualización de la *Views\Shared\Error.cshtml* vista para mostrar el mensaje de error:

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample11.cshtml?highlight=8-17)]

Elimine las cuentas en el **AspNetUsers** tabla que contenga el alias de correo electrónico que desea realizar pruebas con. Ejecute la aplicación y compruebe que no puede iniciar sesión hasta que haya confirmado la dirección de correo electrónico. Una vez que confirme su dirección de correo electrónico, haga clic en el **póngase en contacto con** vínculo.

<a id="reset"></a>
## <a name="password-recoveryreset"></a>Recuperación/restablecimiento de contraseña

Quite los caracteres de comentario de la `HttpPost ForgotPassword` método de acción del controlador de cuentas:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample12.cs?highlight=17-20)]

Quite los caracteres de comentario de la `ForgotPassword` [ActionLink](https://msdn.microsoft.com/library/system.web.mvc.html.linkextensions.actionlink(v=vs.118).aspx) en el *Views\Account\Login.cshtml* archivo de vista razor:

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample13.cshtml?highlight=47-50)]

El registro en la página mostrará un vínculo para restablecer la contraseña.

<a id="rsend"></a>
## <a name="resend-email-confirmation-link"></a>Vínculo de confirmación de correo electrónico de reenvío

Una vez que un usuario crea una nueva cuenta local, reciben un correo electrónico un vínculo de confirmación deben usar antes de que puedan iniciar sesión. Si el usuario que accidentalmente elimina el correo electrónico de confirmación o nunca llega el correo electrónico, deben volverá a enviar el vínculo de confirmación. Los cambios de código siguientes muestran cómo habilitarlo.

Agregue el siguiente método auxiliar a la parte inferior de la *controllers\accountcontroller* archivo:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample14.cs)]

Actualice el método Register para usar la nueva aplicación auxiliar:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample15.cs?highlight=17)]

Actualice el método de inicio de sesión para volver a enviar la contraseña si no se ha confirmado la cuenta de usuario:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample16.cs?highlight=20)]

<a id="combine"></a>
## <a name="combine-social-and-local-login-accounts"></a>Combinar las cuentas de inicio de sesión social y local

Puede combinar las cuentas locales y redes sociales, haga clic en el vínculo de correo electrónico. En la siguiente secuencia **RickAndMSFT@gmail.com** en primer lugar se crea como un inicio de sesión local, pero puede crear la cuenta como un inicio de sesión social primero y luego agregar un inicio de sesión local.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image5.png)

Haga clic en el **administrar** vínculo. Tenga en cuenta la **inicios de sesión externos: 0** asociado con esta cuenta.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image6.png)

Haga clic en el vínculo a otro registro en el servicio y acepte las solicitudes de aplicación. Se han combinado las dos cuentas, podrá iniciar sesión con las cuentas. Es posible que desee que los usuarios agreguen cuentas locales en caso de su registro en el servicio de autenticación social esté inactivo o que es más probable hayan perdido el acceso a su cuenta de redes sociales.

En la siguiente imagen, Tom es un inicio de sesión social (que puede ver desde el **inicios de sesión externos: 1** se muestra en la página).

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image7.png)

Al hacer clic en **elige una contraseña** le permite agregar un registro local en asociada con la misma cuenta.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image8.png)

## <a name="email-confirmation-in-more-depth"></a>Confirmación por correo electrónico con más detalle

Mi tutorial [confirmación de la cuenta y contraseña de recuperación con ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) entra en este tema con más detalles.

<a id="dbg"></a>
## <a name="debugging-the-app"></a>Depuración de la aplicación

Si no recibe un correo electrónico que contiene el vínculo:

- Compruebe la carpeta correo no deseado o spam.
- Inicie sesión en su cuenta de SendGrid y haga clic en el [vínculo de la actividad de correo electrónico](https://sendgrid.com/logs/index).

Para probar el vínculo de comprobación sin correo electrónico, descargue el [ejemplo completado](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952). En la página se mostrará el vínculo de confirmación y códigos de confirmación.

<a id="addRes"></a>
## <a name="additional-resources"></a>Recursos adicionales

- [Recursos recomendados de vínculos a ASP.NET Identity](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [La cuenta de confirmación y la recuperación de contraseña con ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) explica con más detalle en la confirmación de contraseña de recuperación y cuenta.
- [Aplicación de MVC 5 con Facebook, Twitter, LinkedIn y Google OAuth2 Sign-on](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) este tutorial muestra cómo escribir una aplicación ASP.NET MVC 5 con Facebook y Google OAuth 2 la autorización. También muestra cómo agregar datos adicionales a la base de datos de identidad.
- [Implementar una aplicación ASP.NET MVC segura con pertenencia, OAuth y SQL Database en Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Este tutorial agrega la implementación de Azure, cómo proteger su aplicación con los roles, cómo usar la API de pertenencia para agregar usuarios y roles y características de seguridad adicionales.
- [Creación de una aplicación de Google para OAuth 2 y conectarse al proyecto de la aplicación](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [Creación de la aplicación de Facebook y conectarse al proyecto de la aplicación](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [Configuración de SSL en el proyecto](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
