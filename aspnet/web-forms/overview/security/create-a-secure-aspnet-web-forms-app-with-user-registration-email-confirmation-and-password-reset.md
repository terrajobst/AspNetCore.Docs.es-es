---
uid: web-forms/overview/security/create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset
title: Crear una aplicación de formularios Web Forms ASP.NET segura con el registro de usuario, por correo electrónico de confirmación y restablecimiento de contraseña (C#) | Microsoft Docs
author: Erikre
description: Este tutorial muestra cómo compilar una aplicación de formularios Web Forms ASP.NET con registro de usuario, confirmación por correo electrónico y restablecimiento de contraseña mediante el miembro de la identidad de ASP.NET...
ms.author: riande
ms.date: 10/02/2014
ms.assetid: 0a8d6044-5fab-4213-82d6-5618d5601358
msc.legacyurl: /web-forms/overview/security/create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset
msc.type: authoredcontent
ms.openlocfilehash: acc13776840408756901e20589b9efacc83ff2a9
ms.sourcegitcommit: 2c7ffe349eabdccf2ed748dd303ffd0ba6e1cfe3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/26/2019
ms.locfileid: "56833688"
---
<a name="create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset-c"></a>Crear una aplicación segura de formularios Web Forms de ASP.NET con registro de usuario, confirmación por correo electrónico y restablecimiento de contraseña (C#)
====================
por [Erik Reitan](https://github.com/Erikre)

> Este tutorial muestra cómo compilar una aplicación de formularios Web Forms ASP.NET con registro de usuario, confirmación por correo electrónico y restablecimiento de contraseña mediante el sistema de pertenencia de ASP.NET Identity. Este tutorial se basa en de Rick Anderson [tutorial de MVC](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md).


## <a name="introduction"></a>Introducción

Este tutorial le guiará por los pasos necesarios para crear una aplicación de formularios Web Forms de ASP.NET con Visual Studio y ASP.NET 4.5 para crear una aplicación de formularios Web con el registro de usuario, correo electrónico de confirmación y restablecimiento de contraseña.

### <a name="tutorial-tasks-and-information"></a>Tareas del tutorial y obtener información:

- [Crear una ASP.NET Web Forms app](#createWebForms)
- [Enlazar SendGrid](#SG)
- [Requerir confirmación por correo electrónico antes del inicio de sesión](#require)
- [Restablecimiento y recuperación de contraseñas](#reset)
- [Vínculo de confirmación de correo electrónico de reenvío](#rsend)
- [Solución de problemas de la aplicación](#dbg)
- [Recursos adicionales](#addRes)

<a id="createWebForms"></a>
## <a name="create-an-aspnet-web-forms-app"></a>Crear una aplicación de ASP.NET Web Forms

Comience por instalar y ejecutar [Visual Studio Express 2013 para Web](https://go.microsoft.com/fwlink/?LinkId=299058) o [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Instalar [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) o versiones posteriores también.

> [!NOTE]
> Advertencia: Debe instalar [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) o superior para completar este tutorial.


1. Cree un nuevo proyecto (**archivo**  - &gt; **nuevo proyecto**) y seleccione el **aplicación Web ASP.NET** plantilla y la versión más reciente de .NET Framework versión de la **nuevo proyecto** cuadro de diálogo.
2. Desde el **nuevo proyecto ASP.NET** cuadro de diálogo, seleccione el **formularios Web Forms** plantilla. Deje la autenticación predeterminada como **cuentas de usuario individuales**. Si desea hospedar la aplicación en Azure, deje el **Host en la nube** activada la casilla de verificación.   
 A continuación, haga clic en **Aceptar** para crear el nuevo proyecto.  
    ![Cuadro de diálogo nuevo proyecto de ASP.NET](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image1.png)
3. Habilitar capa de Sockets seguros (SSL) para el proyecto. Siga los pasos descritos en la **habilitar SSL para el proyecto** sección de la [Introducción a serie de tutoriales de formularios Web Forms](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms).
4. Ejecute la aplicación, haga clic en el **registrar** vincular y registrar un nuevo usuario. En este momento, la única validación en el correo electrónico se basa en el [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) atributo para asegurarse de que la dirección de correo electrónico está bien formada. Modificará el código para agregar la confirmación por correo electrónico. Cierre la ventana del explorador.
5. En **Explorador de servidores** de Visual Studio (**vista**  - &gt; **Explorador de servidores**), vaya a **Connections\ de datos DefaultConnection\Tables\AspNetUsers**, haga clic y seleccione **Abrir definición de tabla**. 

    La siguiente imagen muestra la `AspNetUsers` esquema de tabla:

    ![Esquema de la tabla AspNetUsers](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image2.png)
6. En **Explorador de servidores**, haga doble clic en el **AspNetUsers** de tabla y seleccione **mostrar datos de tabla**.  
  
    ![Datos de la tabla AspNetUsers](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image3.png)  
 En este momento, no se ha confirmado el correo electrónico para el usuario registrado.
7. Haga clic en la fila y seleccione Eliminar para eliminar el usuario. Deberá agregar este correo electrónico nuevo en el paso siguiente y enviar un mensaje de confirmación a la dirección de correo electrónico.

## <a name="email-confirmation"></a>Confirmación por correo electrónico

Es una práctica recomendada para confirmar el correo electrónico durante el registro de un nuevo usuario para comprobar que no están suplantando persona (es decir, se hayan registrado con el correo electrónico de otra persona). Supongamos que tenemos un foro de discusión, ¿puede evitar `"bob@cpandl.com"` registrar como `"joe@contoso.com"`. Sin confirmación por correo electrónico, `"joe@contoso.com"` podría obtener correo electrónico no deseado de su aplicación. Supongamos que Bob accidentalmente registrado como `"bib@cpandl.com"` y se había dado cuenta, no sería capaz de usar la recuperación de contraseña porque la aplicación no tiene su correo electrónico correcta. Confirmación por correo electrónico proporciona una protección limitada solo desde los bots y no proporciona protección de correo basura determinado.

Por lo general desea impedir que todos los datos del sitio Web de registro antes de que se han confirmado por cualquier correo electrónico, un mensaje de texto SMS u otro mecanismo de nuevos usuarios. En las secciones siguientes, se habilitará la confirmación por correo electrónico y modifique el código para evitar que los usuarios recién registrados iniciar sesión hasta que se ha confirmado el correo electrónico. En este tutorial, usará el servicio de correo electrónico SendGrid.

<a id="SG"></a>
## <a name="hook-up-sendgrid"></a>Enlazar SendGrid

SendGrid ha cambiado su API desde que se escribió este tutorial. Para obtener instrucciones de SendGrid actuales, vea [SendGrid](http://sendgrid.com/) o [habilitar la recuperación de confirmación y la contraseña de cuenta](xref:security/authentication/accconfirm#enable-account-confirmation-and-password-recovery).

Aunque este tutorial solo muestra cómo agregar la notificación de correo electrónico a través de [SendGrid](http://sendgrid.com/), puede enviar correo electrónico mediante SMTP y otros mecanismos (consulte [recursos adicionales](#addRes)).

1. En Visual Studio, abra el **Package Manager Console** (**herramientas**  - &gt; **Administrador de paquetes de NuGet**  - &gt; **Package Manager Console**) y escriba el siguiente comando:  
    `Install-Package SendGrid`
2. Vaya a la [página de registro de SendGrid de Azure](https://azure.microsoft.com/gallery/store/sendgrid/sendgrid-azure/) y registrar de forma gratuita la cuenta de SendGrid. Puede también registrarse para obtener una cuenta gratuita de SendGrid directamente en [sitio de SendGrid](http://www.sendgrid.com).
3. Desde **el Explorador de soluciones** abrir el *IdentityConfig.cs* de archivos en el *aplicación\_iniciar* carpeta y agregue el siguiente código resaltado en amarillo en la `EmailService` clase para configurar **SendGrid**:

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample1.cs?highlight=3,5,8-37)]
4. Además, agregue el siguiente `using` instrucciones al principio de la *IdentityConfig.cs* archivo: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample2.cs?highlight=1-4)]
5. Para simplificar este ejemplo, va a almacenar los valores de cuenta de servicio de correo electrónico en el `appSettings` sección de la *web.config* archivo. Agregue el código XML siguiente resaltado en amarillo en la *web.config* archivo en la raíz del proyecto:

    [!code-xml[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample3.xml?highlight=2-5)]

    > [!WARNING]
    > Seguridad: nunca almacene de datos confidenciales en el código fuente. En este ejemplo, la cuenta y las credenciales se almacenan en el **appSetting** sección de la *Web.config* archivo. En Azure, puede almacenar con seguridad estos valores en el **[configurar](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** ficha en el portal de Azure. Para obtener información relacionada, vea tema de Rick Anderson [procedimientos recomendados para implementar contraseñas y otros datos confidenciales en ASP.NET y Azure](https://go.microsoft.com/fwlink/?LinkId=513141).
6. Agregue los valores de servicio de correo electrónico para reflejar los valores de autenticación de SendGrid (nombre de usuario y contraseña) para que pueda correcta enviar correo electrónico desde la aplicación. Asegúrese de utilizar el nombre de cuenta de SendGrid en lugar de la dirección de correo electrónico que proporcionó SendGrid.

### <a name="enable-email-confirmation"></a>Habilitar la confirmación de correo electrónico

 Para habilitar la confirmación por correo electrónico, deberá modificar el código de registro mediante los pasos siguientes.  
 

1. En el *cuenta* carpeta, abra el *Register.aspx.cs* código subyacente y actualizar el `CreateUser_Click` método para habilitar los cambios resaltados siguientes: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample4.cs?highlight=9-11)]
2. En **el Explorador de soluciones**, haga clic en *Default.aspx* y seleccione **establecer como página principal**.
3. Ejecute la aplicación presionando **F5.** Cuando se muestre la página, haga clic en el **registrar** vínculo para mostrar la página de registro.
4. Escriba su correo electrónico y contraseña, a continuación, haga clic en el **registrar** botón para enviar un mensaje de correo electrónico a través de SendGrid.  
   El estado actual de su proyecto y código permitirá al usuario iniciar sesión una vez que complete el formulario de registro, aunque aún no ha confirmado que su cuenta.
5. Compruebe su cuenta de correo electrónico y haga clic en el vínculo para confirmar su correo electrónico.  
   Una vez que envíe el formulario de registro, se registrará.  
    ![Sitio Web de ejemplo - iniciada](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image4.png)

<a id="require"></a>
## <a name="require-email-confirmation-before-log-in"></a>Requerir confirmación por correo electrónico antes del inicio de sesión

Aunque haya confirmado la cuenta de correo electrónico, en este momento no sería necesario hacer clic en el vínculo de contenido en el correo electrónico de comprobación se ha iniciado sesión completamente. En la siguiente sección, modificará el código que requieren nuevos usuarios para que tenga un correo electrónico de confirmación antes de registrarse (autenticado).

1. En **el Explorador de soluciones** de Visual Studio, actualice el `CreateUser_Click` eventos en el *Register.aspx.cs* código contenido en el *cuentas* carpeta con el cambios resaltados siguientes: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample5.cs?highlight=13-14,17-21)]
2. Actualización de la `LogIn` método en el *Login.aspx.cs* código subyacente con los cambios resaltados siguientes: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample6.cs?highlight=9-19,45-46)]

### <a name="run-the-application"></a>Ejecutar la aplicación

 Ahora que ha implementado el código para comprobar si se ha confirmado la dirección de correo electrónico de un usuario, puede comprobar la funcionalidad tanto en el **registrar** y **inicio de sesión** páginas. 

1. Elimine las cuentas en el **AspNetUsers** tabla que contenga el alias de correo electrónico que desea probar.
2. Ejecute la aplicación (**F5**) y compruebe que no se puede registrar como un usuario hasta que haya confirmado la dirección de correo electrónico.
3. Antes de confirmar la nueva cuenta a través del correo electrónico que acaba de enviar, intenta iniciar sesión con la nueva cuenta.  
 Verá que no es posible iniciar sesión y que debe tener una cuenta de correo electrónico confirmado.
4. Una vez que confirme su dirección de correo electrónico, inicie sesión en la aplicación.

<a id="reset"></a>
## <a name="password-recovery-and-reset"></a>Restablecimiento y recuperación de contraseñas

1. En Visual Studio, quite los caracteres de comentario de la `Forgot` método en el *Forgot.aspx.cs* código contenido en el *cuenta* carpeta, por lo que el método aparece como sigue: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample7.cs?highlight=16-18)]
2. Abra el *Login.aspx* página. Reemplace el marcado cerca del final de la **loginForm** sección tal como se muestra a continuación: 

    [!code-aspx[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample8.aspx?highlight=52-53)]
3. Abra el *Login.aspx.cs* código subyacente y elimine la siguiente línea de código resaltado en amarillo de la `Page_Load` controlador de eventos: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample9.cs?highlight=5)]
4. Ejecute la aplicación presionando **F5.** Cuando se muestre la página, haga clic en el **iniciarla** vínculo.
5. Haga clic en el **¿olvidó su contraseña?** vínculo para mostrar el **contraseña olvidada** página.
6. Escriba su dirección de correo electrónico y haga clic en el **enviar** botón para enviar un correo electrónico a la dirección que le permitirá restablecer su contraseña.   
   Compruebe su cuenta de correo electrónico y haga clic en el vínculo para mostrar el **restablecer contraseña** página.
7. En el **restablecer contraseña** , escriba su correo electrónico, la contraseña y la contraseña confirmada. A continuación, presione el **restablecer** botón.  
   Cuando se restableció correctamente su contraseña, el **cambiar contraseña** se mostrará la página. Ahora puede iniciar sesión con la nueva contraseña.

<a id="rsend"></a>
## <a name="resend-email-confirmation-link"></a>Vínculo de confirmación de correo electrónico de reenvío

Una vez que un usuario crea una nueva cuenta local, reciben un correo electrónico un vínculo de confirmación deben usar antes de que puedan iniciar sesión. Si el usuario que accidentalmente elimina el correo electrónico de confirmación o nunca llega el correo electrónico, deben volverá a enviar el vínculo de confirmación. Los cambios de código siguientes muestran cómo habilitarlo.

1. En Visual Studio, abra el **Login.aspx.cs** código subyacente y agregue el siguiente controlador de eventos después de la `LogIn` controlador de eventos:   

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample10.cs)]
2. Modificar el `LogIn` controlador de eventos en el *Login.aspx.cs* código cambiando el código resaltado en amarillo como sigue: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample11.cs?highlight=15-17)]
3. Actualización de la *Login.aspx* página agregando el código resaltado en amarillo como sigue: 

    [!code-aspx[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample12.aspx?highlight=45-46)]
4. Elimine las cuentas en el **AspNetUsers** tabla que contenga el alias de correo electrónico que desea probar.
5. Ejecute la aplicación (**F5**) y registrar su dirección de correo electrónico.
6. Antes de confirmar la nueva cuenta a través del correo electrónico que acaba de enviar, intenta iniciar sesión con la nueva cuenta.  
   Verá que no es posible iniciar sesión y que debe tener una cuenta de correo electrónico confirmado. Además, ahora puede reenviar un mensaje de confirmación a su cuenta de correo electrónico.
7. Escriba su dirección de correo electrónico y contraseña, a continuación, presione el **Enviar confirmación** botón.
8. Una vez que confirme su dirección de correo electrónico basada en el mensaje recién enviado por correo electrónico, inicie sesión en la aplicación.

<a id="dbg"></a>
## <a name="troubleshooting-the-app"></a>Solución de problemas de la aplicación

Si no recibe un correo electrónico que contiene el vínculo para comprobar las credenciales:

- Compruebe la carpeta correo no deseado o spam.
- Inicie sesión en su cuenta de SendGrid y haga clic en el [vínculo de la actividad de correo electrónico](https://sendgrid.com/logs/index).
- Asegúrese de que usa el nombre de cuenta de usuario de SendGrid como una *Web.config* valor en lugar de su dirección de correo electrónico de la cuenta de SendGrid.

<a id="addRes"></a>
## <a name="additional-resources"></a>Recursos adicionales

- [Recursos recomendados de vínculos a ASP.NET Identity](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Confirmación de la cuenta y contraseña de recuperación con ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [Serie de tutoriales de formularios Web Forms ASP.NET: agregar un proveedor de OAuth 2.0](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#OAuthWebForms)
- [Implementar una aplicación de formularios Web ASP.NET segura con pertenencia, OAuth y SQL Database en Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)
- [Serie de tutoriales de formularios Web Forms ASP.NET - habilitar SSL para el proyecto](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms)
