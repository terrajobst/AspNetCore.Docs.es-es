---
uid: mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
title: Crear una aplicación web de ASP.NET MVC 5 segura con inicio de sesión, enviar por correo electrónico de confirmación y restablecimiento de contraseña (C#) | Documentos de Microsoft
author: Rick-Anderson
description: Este tutorial muestra cómo compilar una aplicación web de ASP.NET MVC 5 con confirmación por correo electrónico y con el sistema de pertenencia de ASP.NET Identity de restablecimiento de contraseña. Ca...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/26/2015
ms.topic: article
ms.assetid: d4911cb3-1afb-4805-b860-10818c4b1280
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
msc.type: authoredcontent
ms.openlocfilehash: bfa5d52019be81374c7a544e255ab7ffb301fa7b
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/08/2018
ms.locfileid: "34452573"
---
<a name="create-a-secure-aspnet-mvc-5-web-app-with-log-in-email-confirmation-and-password-reset-c"></a>Crear una aplicación web de ASP.NET MVC 5 segura con inicio de sesión, enviar por correo electrónico de confirmación y restablecimiento de contraseña (C#)
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> Este tutorial muestra cómo compilar una aplicación web de ASP.NET MVC 5 con confirmación por correo electrónico y con el sistema de pertenencia de ASP.NET Identity de restablecimiento de contraseña. Puede descargar la aplicación completa [aquí](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952). La descarga contiene aplicaciones auxiliares de depuración que permiten probar confirmación por correo electrónico y SMS sin necesidad de preparar un correo electrónico o el proveedor de SMS.
> 
> Este tutorial se escribió por [Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).


<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a>Crear una aplicación de ASP.NET MVC

Empiece por instalar y ejecutar [Visual Studio Express 2013 para Web](https://go.microsoft.com/fwlink/?LinkId=299058) o [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Instalar [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) o superior.

> [!NOTE]
> Advertencia: Debe instalar [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) o superior para completar este tutorial.


1. Cree un nuevo proyecto Web de ASP.NET y seleccione la plantilla MVC. Formularios Web Forms también admite la identidad de ASP.NET, por lo que podría seguir pasos similares en una aplicación de formularios web.  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image1.png)
2. Deje la autenticación predeterminada como **cuentas de usuario individuales**. Si desea hospedar la aplicación en Azure, deje activada la casilla de verificación. Más adelante en el tutorial se implementará en Azure. También puede [abrir una cuenta de Azure de forma gratuita](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).
3. Establecer el [proyecto para usar SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
4. Ejecutar la aplicación, haga clic en el **registrar** vincular y registrar un usuario. En este punto, es la única validación en el correo electrónico con el [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) atributo.
5. En el Explorador de servidores, vaya a **datos Connections\DefaultConnection\Tables\AspNetUsers**, haga clic en y seleccione **Abrir definición de tabla**.

    La siguiente imagen muestra la `AspNetUsers` esquema:

    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image2.png)
6. Haga clic con el botón secundario en el **AspNetUsers** de tabla y seleccione **mostrar datos de tabla**.  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image3.png)  
 En este momento, no se ha confirmado el correo electrónico.
7. Haga clic en la fila y seleccione Eliminar. Podrá agregar este correo electrónico nuevo en el paso siguiente y enviar un correo electrónico de confirmación.

## <a name="email-confirmation"></a>Confirmación por correo electrónico

Es una práctica recomendada para confirmar el correo electrónico de un nuevo registro de usuario para comprobar que no utiliza la suplantación otra persona (es decir, no ha registrado con el correo electrónico de otra persona). Supongamos que tenemos un foro de discusión, puede que desee evitar `"bob@example.com"` al registrar como `"joe@contoso.com"`. Sin confirmación por correo electrónico, `"joe@contoso.com"` pudo obtener el correo electrónico no deseado de la aplicación. Suponga que Roberto accidentalmente registrado como `"bib@example.com"` y no vio, no podrá usar la recuperación de contraseña porque la aplicación no tiene su correo electrónico correcto. Confirmación por correo electrónico proporciona sólo una protección limitada de robots y no proporciona protección de correo basura determinado, tienen muchos alias de correo electrónico de trabajo que puede usar para registrar.

En general conveniente evitar que los nuevos usuarios se registren todos los datos del sitio web antes de que se han confirmado por correo electrónico, un mensaje de texto SMS u otro mecanismo. <a id="build"></a>En las secciones siguientes, se habilitará la confirmación por correo electrónico y modificar el código para evitar que los usuarios recién registrados el registro hasta que se ha confirmado el correo electrónico.

<a id="SG"></a>
## <a name="hook-up-sendgrid"></a>Enlazar SendGrid

Aunque este tutorial solo muestra cómo agregar la notificación de correo electrónico a través de [SendGrid](http://sendgrid.com/), puede enviar correo electrónico mediante SMTP y otros mecanismos (vea [recursos adicionales](#addRes)).

1. En la consola de administrador de paquetes, escriba el siguiente comando: 

    [!code-console[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample1.cmd)]
2. Vaya a la [página de registro Azure SendGrid](https://go.microsoft.com/fwlink/?linkid=271033&clcid=0x409) y registrar una cuenta gratuita de SendGrid. Configurar SendGrid agregando código similar al siguiente en *App_Start/IdentityConfig.cs*:

    [!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample2.cs?highlight=3,5)]

Debe agregar que incluye lo siguiente:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample3.cs)]

Para simplificar este ejemplo, se podrá almacenar la configuración de la aplicación en el *web.config* archivo:

[!code-xml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample4.xml)]

> [!WARNING]
> Seguridad - nunca almacenar los datos confidenciales en el código fuente. La cuenta y las credenciales se almacenan en el appSetting. En Azure, puede almacenar con seguridad estos valores en el **[configurar](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** ficha en el portal de Azure. Vea [las prácticas recomendadas para implementar las contraseñas y otros datos confidenciales en ASP.NET y Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).


### <a name="enable-email-confirmation-in-the-account-controller"></a>Habilitar la confirmación de correo electrónico en el controlador de cuenta

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample5.cs?highlight=16-21)]

Compruebe el *Views\Account\ConfirmEmail.cshtml* archivo tiene la sintaxis razor correcto. (El carácter en la primera @ línea es posible que falte. )

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample6.cshtml?highlight=1)]

Ejecute la aplicación y haga clic en el vínculo de registro. Una vez que se envía el formulario de registro, que haya iniciado sesión.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image4.png)

Compruebe su cuenta de correo electrónico y haga clic en el vínculo para confirmar su correo electrónico.

<a id="require"></a>
## <a name="require-email-confirmation-before-log-in"></a>Requerir confirmación por correo electrónico antes de inicio de sesión

Actualmente una vez que un usuario complete el formulario de registro, se registran en. Por lo general desea confirmar su correo electrónico antes de registrarlos. En la sección siguiente, se modificará el código para requerir que los usuarios nuevos para que tenga un correo electrónico confirmado antes de que se registran en (autenticado). Actualización de la `HttpPost Register` método con los siguientes cambios resaltados:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample7.cs?highlight=14-15,23-30)]

Marcando como comentario el `SignInAsync` método, el usuario no se firmará mediante el registro. El `TempData["ViewBagLink"] = callbackUrl;` línea se puede usar para [depurar la aplicación](#dbg) y probar el registro sin enviar correo electrónico. `ViewBag.Message` se usa para mostrar las instrucciones de confirmación. El [Descargar ejemplo](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952) contiene código para probar la confirmación por correo electrónico sin tener que configurar el correo electrónico y también puede utilizarse para depurar la aplicación.

Crear un `Views\Shared\Info.cshtml` de archivos y agregue el siguiente marcado de razor:

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample8.cshtml)]

Agregar el [atributo Authorize](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.118).aspx) a la `Contact` método de acción del controlador Home. Puede hacer clic en el **póngase en contacto con** vínculo para comprobar que los usuarios anónimos no tienen acceso y los usuarios autenticados tengan acceso.

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample9.cs?highlight=1)]

También debe actualizar el `HttpPost Login` método de acción:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample10.cs?highlight=13-22)]

Actualización de la *Views\Shared\Error.cshtml* vista para mostrar el mensaje de error:

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample11.cshtml?highlight=8-17)]

Elimine las cuentas en el **AspNetUsers** tabla que contenga el alias de correo electrónico que se va a probar. Ejecutar la aplicación y compruebe que no puede iniciar sesión hasta que haya confirmado la dirección de correo electrónico. Una vez que confirme la dirección de correo electrónico, haga clic en el **póngase en contacto con** vínculo.

<a id="reset"></a>
## <a name="password-recoveryreset"></a>Recuperación/restablecimiento de contraseña

Quite los caracteres de comentario de la `HttpPost ForgotPassword` método de acción en el controlador de cuenta:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample12.cs?highlight=17-20)]

Quite los caracteres de comentario de la `ForgotPassword` [ActionLink](https://msdn.microsoft.com/library/system.web.mvc.html.linkextensions.actionlink(v=vs.118).aspx) en el *Views\Account\Login.cshtml* archivo de vista razor:

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample13.cshtml?highlight=47-50)]

Ahora, la página de inicio de sesión mostrará un vínculo para restablecer la contraseña.

<a id="rsend"></a>
## <a name="resend-email-confirmation-link"></a>Vínculo de confirmación de correo electrónico de reenvío

Una vez que un usuario crea una nueva cuenta local, reciben un correo electrónico un vínculo de confirmación que son necesarios para usar antes de que puedan iniciar sesión. Si el usuario accidentalmente elimina el correo electrónico de confirmación o nunca llega el correo electrónico, necesitará el vínculo de confirmación que se envíen de nuevo. Los cambios de código siguientes muestran cómo habilitarlo.

Agregue el siguiente método auxiliar a la parte inferior de la *Controllers\AccountController.cs* archivo:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample14.cs)]

El método Register para utilizar la nueva aplicación auxiliar de actualización:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample15.cs?highlight=17)]

Actualizar el método de inicio de sesión para enviar la contraseña si no se ha confirmado la cuenta de usuario:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample16.cs?highlight=20)]

<a id="combine"></a>
## <a name="combine-social-and-local-login-accounts"></a>Combinar las cuentas de inicio de sesión locales y redes sociales

Puede combinar cuentas locales y redes sociales, haga clic en el vínculo de correo electrónico. En la siguiente secuencia **RickAndMSFT@gmail.com** en primer lugar se crea como un inicio de sesión local, pero puede crear la cuenta como un inicio de sesión social primero y luego agregar un inicio de sesión local.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image5.png)

Haga clic en el **administrar** vínculo. Tenga en cuenta el **inicios de sesión externos: 0** asociados con esta cuenta.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image6.png)

Haga clic en el vínculo en otro registro de servicio y Aceptar las solicitudes de aplicación. Se han combinado las dos cuentas, podrá iniciar sesión con cualquiera de estas cuentas. Puede que los usuarios agreguen cuentas locales en caso de que su registro sociales en servicio de autenticación está inactivo o, más probable es que ha perdido el acceso a su cuenta sociales.

En la siguiente imagen, Tom es un inicio de sesión social (que puede ver desde el **inicios de sesión externos: 1** se muestra en la página).

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image7.png)

Al hacer clic en **elegir una contraseña** le permite agregar un registro local en asociados con la misma cuenta.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image8.png)

## <a name="email-confirmation-in-more-depth"></a>Confirmación por correo electrónico de forma más minuciosa

Mi tutorial [confirmación de cuenta y contraseña de recuperación con ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) entra en este tema con más detalles.

<a id="dbg"></a>
## <a name="debugging-the-app"></a>Para depurar la aplicación

Si no recibe un correo electrónico que contiene el vínculo:

- Compruebe la carpeta correo no deseado o spam.
- Inicie sesión en la cuenta de SendGrid y haga clic en el [vínculo de actividad de correo electrónico](https://sendgrid.com/logs/index).

Para probar el vínculo de comprobación sin correo electrónico, descargue el [ejemplo completo](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952). Se mostrará el vínculo de confirmación y códigos de confirmación en la página.

<a id="addRes"></a>
## <a name="additional-resources"></a>Recursos adicionales

- [Vínculos a la identidad de ASP.NET de los recursos recomendados](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [La cuenta de confirmación y recuperación de contraseña con ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) explica con más detalle en la confirmación de contraseña de cuenta y la recuperación.
- [Aplicación de MVC 5 con Facebook, Twitter, LinkedIn y Google OAuth2 Sign-on](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) este tutorial muestra cómo escribir una aplicación de ASP.NET MVC 5 con autorización de Facebook y Google OAuth 2. También muestra cómo agregar datos adicionales a la base de datos de identidad.
- [Implementar una aplicación de MVC de ASP.NET segura con pertenencia, OAuth y base de datos SQL en Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Este tutorial agrega la implementación de Azure, cómo proteger su aplicación con roles, cómo usar la API de pertenencia para agregar usuarios y roles y características de seguridad adicionales.
- [Crear una aplicación de Google para OAuth 2 y conectar la aplicación al proyecto](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [Crear la aplicación de Facebook y conectar la aplicación al proyecto](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [Cómo configurar SSL en el proyecto](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
