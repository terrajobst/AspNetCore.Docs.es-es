---
uid: web-forms/overview/security/create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset
title: "Crear una aplicación de formularios Web Forms de ASP.NET segura con el registro de usuario, enviar por correo electrónico de confirmación y restablecimiento de contraseña (C#) | Documentos de Microsoft"
author: Erikre
description: "Este tutorial muestra cómo compilar una aplicación de formularios Web Forms de ASP.NET con el registro de usuario, confirmación por correo electrónico y contraseña restablecer mediante el miembro de la identidad de ASP.NET..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/02/2014
ms.topic: article
ms.assetid: 0a8d6044-5fab-4213-82d6-5618d5601358
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/security/create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset
msc.type: authoredcontent
ms.openlocfilehash: b6f3821a8022daa26f5efcc009ab3e6283a76a19
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset-c"></a>Crear una aplicación de formularios Web Forms de ASP.NET segura con el registro de usuario, enviar por correo electrónico de confirmación y restablecimiento de contraseña (C#)
====================
Por [Erik Reitan](https://github.com/Erikre)

> Este tutorial muestra cómo compilar una aplicación de formularios Web Forms de ASP.NET con el registro de usuario, confirmación por correo electrónico y con el sistema de pertenencia de ASP.NET Identity de restablecimiento de contraseña. Este tutorial se basa en de Rick Anderson [tutorial MVC](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md).


## <a name="introduction"></a>Introducción

Este tutorial le guiará por los pasos necesarios para crear una aplicación de formularios Web Forms de ASP.NET con Visual Studio y ASP.NET 4.5 para crear una aplicación de formularios Web Forms segura con el registro de usuario, el restablecimiento de contraseña y de confirmación de correo electrónico.

### <a name="tutorial-tasks-and-information"></a>Tareas del tutorial e información:

- [Crear un sitio Web de ASP.NET aplicación de formularios](#createWebForms)
- [Enlazar SendGrid](#SG)
- [Requerir confirmación por correo electrónico antes de inicio de sesión](#require)
- [Restablecimiento y recuperación de contraseñas](#reset)
- [Vínculo de confirmación de correo electrónico de reenvío](#rsend)
- [Solución de problemas de la aplicación](#dbg)
- [Recursos adicionales](#addRes)

<a id="createWebForms"></a>
## <a name="create-an-aspnet-web-forms-app"></a>Crear una aplicación de formularios Web de ASP.NET

Empiece por instalar y ejecutar [Visual Studio Express 2013 para Web](https://go.microsoft.com/fwlink/?LinkId=299058) o [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Instalar [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) o versiones posteriores también.

> [!NOTE]
> Advertencia: Debe instalar [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) o superior para completar este tutorial.


1. Cree un nuevo proyecto (**archivo**  - &gt; **nuevo proyecto**) y seleccione el **aplicación Web ASP.NET** plantilla y la versión más reciente de .NET Framework versión de la **nuevo proyecto** cuadro de diálogo.
2. Desde el **nuevo proyecto ASP.NET** cuadro de diálogo, seleccione la **formularios Web Forms** plantilla. Deje la autenticación predeterminada como **cuentas de usuario individuales**. Si desea hospedar la aplicación en Azure, deje el **Host en la nube** casilla activada.   
 A continuación, haga clic en **Aceptar** para crear el nuevo proyecto.  
    ![Cuadro de diálogo nuevo proyecto de ASP.NET](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image1.png)
3. Habilitar capa de Sockets seguros (SSL) para el proyecto. Siga los pasos disponibles en el **habilitar SSL para el proyecto** sección de la [Introducción a la serie de tutoriales de formularios Web Forms](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms).
4. Ejecutar la aplicación, haga clic en el **registrar** vincular y registrar un nuevo usuario. En este momento, la validación sola en el correo electrónico se basa en el [[EmailAddress]](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) atributo para asegurarse de que la dirección de correo electrónico está bien formada. Se modificará el código para agregar la confirmación por correo electrónico. Cierre la ventana del explorador.
5. En **Explorador de servidores** de Visual Studio (**vista**  - &gt; **Explorador de servidores**), vaya a **Connections\ de datos DefaultConnection\Tables\AspNetUsers**, haga clic en y seleccione **Abrir definición de tabla**. 

    La siguiente imagen muestra la `AspNetUsers` esquema de tabla:

    ![Esquema de la tabla de AspNetUsers](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image2.png)
6. En **Explorador de servidores**, haga doble clic en el **AspNetUsers** de tabla y seleccione **mostrar datos de tabla**.  
  
    ![Datos de la tabla de AspNetUsers](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image3.png)  
 En este momento, no se ha confirmado el correo electrónico para el usuario registrado.
7. Haga clic en la fila y seleccione Eliminar para eliminar el usuario. Podrá agregar este correo electrónico nuevo en el paso siguiente y envíe un mensaje de confirmación a la dirección de correo electrónico.

## <a name="email-confirmation"></a>Confirmación por correo electrónico

Es una práctica recomendada para confirmar el correo electrónico durante el registro de un nuevo usuario para comprobar que no utiliza la suplantación otra persona (es decir, no ha registrado con el correo electrónico de otra persona). Supongamos que tenemos un foro de discusión, puede que desee evitar `"bob@cpandl.com"` al registrar como `"joe@contoso.com"`. Sin confirmación por correo electrónico, `"joe@contoso.com"` pudo obtener el correo electrónico no deseado de la aplicación. Suponga que Roberto accidentalmente registrado como `"bib@cpandl.com"` y no vio, no podrá usar la recuperación de contraseñas porque la aplicación no tiene su correo electrónico correcto. Confirmación por correo electrónico proporciona sólo una protección limitada de robots y no proporciona protección de correo basura determinado.

En general conveniente evitar que los nuevos usuarios se registren todos los datos del sitio Web antes de que se han confirmado por cualquier correo electrónico, un mensaje de texto SMS u otro mecanismo. En las secciones siguientes, se habilitará la confirmación por correo electrónico y modificar el código para evitar que los usuarios recién registrados el registro hasta que se ha confirmado el correo electrónico. En este tutorial, usará el servicio de correo electrónico SendGrid.

<a id="SG"></a>
## <a name="hook-up-sendgrid"></a>Enlazar SendGrid

Aunque este tutorial solo muestra cómo agregar la notificación de correo electrónico a través de [SendGrid](http://sendgrid.com/), puede enviar correo electrónico mediante SMTP y otros mecanismos (vea [recursos adicionales](#addRes)).

1. En Visual Studio, abra el **Package Manager Console** (**herramientas**  - &gt; **Administrador de paquetes de NuGet**  - &gt; **Package Manager Console**) y escriba el comando siguiente:  
    `Install-Package SendGrid`
2. Vaya a la [página de suscripción de Azure SendGrid](https://azure.microsoft.com/en-us/gallery/store/sendgrid/sendgrid-azure/) y se registra de forma gratuita cuenta de SendGrid. Puede también registrarse para obtener una cuenta gratuita de SendGrid directamente en [sitio de SendGrid](http://www.sendgrid.com).
3. De **el Explorador de soluciones** abrir la *IdentityConfig.cs* un archivo en el *aplicación\_iniciar* carpeta y agregue el código siguiente aparecen resaltado en amarillo en el `EmailService` clase para configurar **SendGrid**:

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample1.cs?highlight=3,5,8-37)]
4. Además, agregue el siguiente `using` instrucciones al principio de la *IdentityConfig.cs* archivo: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample2.cs?highlight=1-4)]
5. Para simplificar este ejemplo, va a almacenar los valores de cuenta de servicio de correo electrónico en el `appSettings` sección de la *web.config* archivo. Agregue el siguiente XML aparecen resaltado en amarillo en el *web.config* archivo en la raíz del proyecto:

    [!code-xml[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample3.xml?highlight=2-5)]

    > [!WARNING]
    > Seguridad - nunca almacenar los datos confidenciales en el código fuente. En este ejemplo, la cuenta y las credenciales se almacenan en la **appSetting** sección de la *Web.config* archivo. En Azure, puede almacenar con seguridad estos valores en el  **[configurar](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)**  ficha en el portal de Azure. Para obtener información relacionada, vea tema de Rick Anderson [las prácticas recomendadas para implementar las contraseñas y otros datos confidenciales en ASP.NET y Azure](https://go.microsoft.com/fwlink/?LinkId=513141).
6. Agregue los valores de servicio de correo electrónico para reflejar los valores de autenticación de SendGrid (nombre de usuario y contraseña) para que pueda correcta envían correo electrónico desde la aplicación. Asegúrese de usar el nombre de la cuenta de SendGrid en lugar de la dirección de correo electrónico que proporcionó SendGrid.

### <a name="enable-email-confirmation"></a>Habilitar la confirmación por correo electrónico

 Para habilitar la confirmación por correo electrónico, deberá modificar el código de registro mediante los pasos siguientes.  
 

1. En el *cuenta* carpeta, abra el *Register.aspx.cs* código subyacente y actualizar el `CreateUser_Click` método para permitir a los siguientes cambios resaltados: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample4.cs?highlight=9-11)]
2. En **el Explorador de soluciones**, haga clic en *Default.aspx* y seleccione **establecer como página principal**.
3. Ejecute la aplicación presionando **F5.** Cuando se muestre la página, haga clic en el **registrar** vínculo para mostrar la página de registro.
4. Escriba su correo electrónico y contraseña, a continuación, haga clic en el **registrar** botón para enviar un mensaje de correo electrónico a través de SendGrid.  
 El estado actual de su proyecto y de código le permitirá al usuario iniciar sesión una vez que complete el formulario de registro, incluso si aún no lo ha confirmado que su cuenta.
5. Compruebe su cuenta de correo electrónico y haga clic en el vínculo para confirmar su correo electrónico.  
 Una vez que se envía el formulario de registro, se grabará en.  
    ![Sitio Web de ejemplo - iniciado sesión](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image4.png)

<a id="require"></a>
## <a name="require-email-confirmation-before-log-in"></a>Requerir confirmación por correo electrónico antes de inicio de sesión

Aunque se haya confirmado la cuenta de correo electrónico, en este momento no necesitaría hacer clic en el vínculo situado en el correo electrónico de comprobación sea totalmente con sesión iniciada. En la sección siguiente, modificará el código que requieren nuevos usuarios para que tenga un correo electrónico confirmado antes de que se registran en (autenticado).

1. En **el Explorador de soluciones** de Visual Studio, actualizar el `CreateUser_Click` evento en el *Register.aspx.cs* código subyacente contenida en el *cuentas* carpeta con el cambios resaltados siguientes: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample5.cs?highlight=13-14,17-21)]
2. Actualización de la `LogIn` método en el *Login.aspx.cs* con los siguientes cambios resaltados de código subyacente: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample6.cs?highlight=9-19,45-46)]

### <a name="run-the-application"></a>Ejecutar la aplicación

 Ahora que ha implementado el código para comprobar si se ha confirmado la dirección de correo electrónico de un usuario, puede comprobar la funcionalidad tanto la **registrar** y **inicio de sesión** páginas. 

1. Elimine las cuentas en el **AspNetUsers** tabla que contenga el alias de correo electrónico que se va a probar.
2. Ejecutar la aplicación (**F5**) y compruebe que no se puede registrar como un usuario hasta que se haya confirmado la dirección de correo electrónico.
3. Antes de confirmar la nueva cuenta a través del correo electrónico que acabamos de enviar, intenta iniciar sesión con la nueva cuenta.  
 Verá que no es posible iniciar sesión y que debe tener una cuenta de correo electrónico confirmado.
4. Una vez que confirme la dirección de correo electrónico, inicie sesión en la aplicación.

<a id="reset"></a>
## <a name="password-recovery-and-reset"></a>Restablecimiento y recuperación de contraseñas

1. En Visual Studio, quite los caracteres de comentario de la `Forgot` método en el *Forgot.aspx.cs* código subyacente contenida en el *cuenta* carpeta, por lo que el método aparece como sigue: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample7.cs?highlight=16-18)]
2. Abra la *Login.aspx* página. Reemplace el marcado cerca del final de la **loginForm** sección como se indica a continuación: 

    [!code-aspx[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample8.aspx?highlight=52-53)]
3. Abra la *Login.aspx.cs* código subyacente y elimine la línea siguiente de código aparecen resaltada en amarillo de la `Page_Load` controlador de eventos: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample9.cs?highlight=5)]
4. Ejecute la aplicación presionando **F5.** Cuando se muestre la página, haga clic en el **sesión** vínculo.
5. Haga clic en el **¿olvidó su contraseña?** vínculo para mostrar la **contraseña olvidada** página.
6. Escriba su dirección de correo electrónico y haga clic en el **enviar** botón para enviar un correo electrónico a la dirección que le permitirá restablecer su contraseña.   
 Compruebe su cuenta de correo electrónico y haga clic en el vínculo para mostrar la **restablecer contraseña** página.
7. En el **restablecer contraseña** página, escriba su correo electrónico, la contraseña y la contraseña confirmada. A continuación, presione la **restablecer** botón.  
 Cuando se restableció correctamente la contraseña, el **cambiar contraseña** se mostrará la página. Ahora puede iniciar sesión con la nueva contraseña.

<a id="rsend"></a>
## <a name="resend-email-confirmation-link"></a>Vínculo de confirmación de correo electrónico de reenvío

Una vez que un usuario crea una nueva cuenta local, reciben un correo electrónico un vínculo de confirmación que son necesarios para usar antes de que puedan iniciar sesión. Si el usuario accidentalmente elimina el correo electrónico de confirmación o nunca llega el correo electrónico, necesitará el vínculo de confirmación que se envíen de nuevo. Los cambios de código siguientes muestran cómo habilitarlo.

1. En Visual Studio, abra el **Login.aspx.cs** código subyacente y agregue el siguiente controlador de eventos después de la `LogIn` controlador de eventos:   

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample10.cs)]
2. Modificar el `LogIn` controlador de eventos en el *Login.aspx.cs* código subyacente, cambie el código que se resalta en amarillo, como se indica a continuación: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample11.cs?highlight=15-17)]
3. Actualización de la *Login.aspx* página agregando el código que se resalta en amarillo, como se indica a continuación: 

    [!code-aspx[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample12.aspx?highlight=45-46)]
4. Elimine las cuentas en el **AspNetUsers** tabla que contenga el alias de correo electrónico que se va a probar.
5. Ejecutar la aplicación (**F5**) y registrar su dirección de correo electrónico.
6. Antes de confirmar la nueva cuenta a través del correo electrónico que acabamos de enviar, intenta iniciar sesión con la nueva cuenta.  
 Verá que no es posible iniciar sesión y que debe tener una cuenta de correo electrónico confirmado. Además, ahora puede reenviar un mensaje de confirmación a su cuenta de correo electrónico.
7. Escriba su dirección de correo electrónico y contraseña, a continuación, presione la **Enviar confirmación** botón.
8. Una vez que confirme la dirección de correo electrónico basada en el mensaje de correo electrónico enviados recientemente, inicie sesión en la aplicación.

<a id="dbg"></a>
## <a name="troubleshooting-the-app"></a>Solución de problemas de la aplicación

Si no recibe un correo electrónico con el vínculo para comprobar las credenciales:

- Compruebe la carpeta correo no deseado o spam.
- Inicie sesión en la cuenta de SendGrid y haga clic en el [vínculo de actividad de correo electrónico](https://sendgrid.com/logs/index).
- Asegúrese de que usa el nombre de cuenta de usuario de SendGrid como un *Web.config* valor en lugar de su dirección de correo electrónico de la cuenta de SendGrid.

<a id="addRes"></a>
## <a name="additional-resources"></a>Recursos adicionales

- [Vínculos a la identidad de ASP.NET de los recursos recomendados](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Confirmación de cuenta y contraseña de recuperación con la identidad de ASP.NET](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [Serie de tutoriales de formularios Web Forms ASP.NET: agregar un proveedor de OAuth 2.0](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#OAuthWebForms)
- [Implementar una aplicación de formularios Web de ASP.NET segura con pertenencia, OAuth y base de datos SQL en el servicio de aplicaciones de Azure](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)
- [Serie de tutoriales de formularios Web Forms ASP.NET - habilitar SSL para el proyecto](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms)
