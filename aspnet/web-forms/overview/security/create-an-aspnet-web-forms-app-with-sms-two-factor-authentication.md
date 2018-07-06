---
uid: web-forms/overview/security/create-an-aspnet-web-forms-app-with-sms-two-factor-authentication
title: Crear una ASP.NET Web Forms app con autenticación en dos fases SMS (C#) | Microsoft Docs
author: Erikre
description: Este tutorial muestra cómo compilar una aplicación de formularios Web Forms de ASP.NET con la autenticación en dos fases. En este tutorial se ha diseñado para complementar el tutorial titulado Cr...
ms.author: aspnetcontent
ms.date: 10/09/2014
ms.assetid: 716264ae-ab72-45de-bfc5-53a6237089cf
msc.legacyurl: /web-forms/overview/security/create-an-aspnet-web-forms-app-with-sms-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: 16045b116ca5c797e7840f2ee5944e5f2c6282eb
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803554"
---
<a name="create-an-aspnet-web-forms-app-with-sms-two-factor-authentication-c"></a>Crear una ASP.NET Web Forms app con autenticación en dos fases SMS (C#)
====================
por [Erik Reitan](https://github.com/Erikre)

[Descargue la aplicación de ASP.NET Web Forms con correo electrónico y SMS autenticación en dos fases](https://code.msdn.microsoft.com/ASPNET-Web-Forms-App-with-5a0ff94e)

> Este tutorial muestra cómo compilar una aplicación de formularios Web Forms de ASP.NET con la autenticación en dos fases. En este tutorial se ha diseñado para complementar el tutorial titulado [crear una aplicación de formularios Web Forms ASP.NET segura con el registro de usuario, correo electrónico de confirmación y restablecimiento de contraseña](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md). Además, este tutorial se basaba en de Rick Anderson [tutorial de MVC](../../../mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).


## <a name="introduction"></a>Introducción

Este tutorial le guiará por los pasos necesarios para crear una aplicación de formularios Web Forms de ASP.NET que admite la autenticación en dos fases mediante Visual Studio. Autenticación en dos fases es un paso de autenticación de usuario adicional. Este paso adicional, genera un número exclusivo de identificación personal (NIP) durante el inicio de sesión. Normalmente, el PIN se envía al usuario como un correo electrónico o mensaje SMS. El usuario de la aplicación, a continuación, escriba el PIN como medida de autenticación adicional al iniciar sesión.

### <a name="tutorial-tasks-and-information"></a>Tareas del tutorial y obtener información:

- [Crear una aplicación de ASP.NET Web Forms](#createWebForms)
- [Configuración de SMS y la autenticación en dos fases](#SMS)
- [Habilitar la autenticación en dos fases para el usuario registrado](#use2FA)
- [Recursos adicionales](#addRes)

<a id="createWebForms"></a>
## <a name="create-an-aspnet-web-forms-app"></a>Crear una aplicación de ASP.NET Web Forms

Comience por instalar y ejecutar [Visual Studio Express 2013 para Web](https://go.microsoft.com/fwlink/?LinkId=299058) o [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Instalar [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) o versiones posteriores también. Además, deberá crear un [Twilio](https://www.twilio.com/try-twilio) cuenta, como se explica más adelante.

> [!NOTE]
> Importante: Debe instalar [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) o superior para completar este tutorial.


1. Cree un nuevo proyecto (**archivo**  - &gt; **nuevo proyecto**) y seleccione el **aplicación Web ASP.NET** plantilla junto con .NET Framework la versión 4.5.2 desde el **nuevo proyecto** cuadro de diálogo.
2. Desde el **nuevo proyecto ASP.NET** cuadro de diálogo, seleccione el **formularios Web Forms** plantilla. Deje la autenticación predeterminada como **cuentas de usuario individuales**. A continuación, haga clic en **Aceptar** para crear el nuevo proyecto.  
    ![Cuadro de diálogo nuevo proyecto de ASP.NET](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image1.png)
3. Habilitar capa de Sockets seguros (SSL) para el proyecto. Siga los pasos descritos en la **habilitar SSL para el proyecto** sección de la [Introducción a serie de tutoriales de formularios Web Forms](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms).
4. En Visual Studio, abra el **Package Manager Console** (**herramientas**  - &gt; **Administrador de paquetes de NuGet**  - &gt; **Package Manager Console**) y escriba el siguiente comando:  
    `Install-Package Twilio`

<a id="SMS"></a>
## <a name="setup-sms-and-two-factor-authentication"></a>Configuración de SMS y la autenticación en dos fases

Este tutorial usa Twilio, pero puede usar cualquier proveedor de SMS.

1. Crear un [Twilio](https://www.twilio.com/try-twilio) cuenta.
2. Desde el **panel** pestaña de la cuenta de Twilio, copia el **SID de cuenta** y **Token de autenticación.** Se agregarlos a la aplicación más adelante.
3. Desde el **números** pestaña, copie su Twilio **número de teléfono** también.
4. Realizar el Twilio **SID de cuenta**, **Auth Token** y **número de teléfono** disponibles para la aplicación. Para simplificar las cosas almacenará estos valores en el *web.config* archivo. Cuando se implementa en Azure, puede almacenar los valores de forma segura en el **appSettings** sección en el sitio web de la pestaña de configuración. Además, al agregar el número de teléfono, usar solo números.   
   Tenga en cuenta que también puede agregar las credenciales de SendGrid. SendGrid es un servicio de notificación de correo electrónico. Para más información sobre cómo habilitar SendGrid, consulte la sección de 'Enlace SendGrid' del tutorial titulada [crear una aplicación de ASP.NET Web Forms seguro con el registro de usuario, correo electrónico de confirmación y restablecimiento de contraseña.](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md)

    [!code-xml[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample1.xml?highlight=2,6-10)]

    > [!WARNING]
    > Seguridad: nunca almacene de datos confidenciales en el código fuente. En este ejemplo, la cuenta y las credenciales se almacenan en el **appSettings** sección de la *Web.config* archivo. En Azure, puede almacenar con seguridad estos valores en el **[configurar](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** ficha en el portal de Azure. Para obtener información relacionada, vea el tema de Rick Anderson [procedimientos recomendados para implementar contraseñas y otros datos confidenciales en ASP.NET y Azure](https://go.microsoft.com/fwlink/?LinkId=513141).
5. Configurar la `SmsService` clase en el *aplicación\_Start\IdentityConfig.cs* señalado en amarillo cambios del archivo haciendo lo siguiente: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample2.cs?highlight=5-17)]
6. Agregue el siguiente `using` instrucciones al principio de la *IdentityConfig.cs* archivo: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample3.cs?highlight=1-4)]
7. Actualización de la *Account/Manage.aspx* archivo mediante la eliminación de las líneas resaltadas en amarillo:  

    [!code-aspx[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample4.aspx?highlight=38,53,57-60,63,66,70,73)]
8. En el `Page_Load` controlador de la *Manage.aspx.cs* código subyacente, quite la línea de código resaltada en amarillo para que aparezca como sigue: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample5.cs?highlight=8)]
9. En el código subyacente de *cuenta*/*TwoFactorAuthenticationSignIn.aspx.cs*, actualice el `Page_Load` controlador agregando el siguiente código resaltado en amarillo: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample6.cs?highlight=3-4,13)]

   Mediante la realización de cambiar el código anterior, no se restablecerá al primer valor de DropDownList "Providers" que contiene las opciones de autenticación. Esto permitirá al usuario seleccionar correctamente todas las opciones para usar la autenticación, no solo la primera.
10. En **el Explorador de soluciones**, haga clic en *Default.aspx* y seleccione **establecer como página principal**.
11. Al probar la aplicación, cree primero la aplicación (**Ctrl**+**MAYÚS**+**B**) y, a continuación, ejecute la aplicación (**F5**) y Seleccionar una opción **registrar** para crear una nueva cuenta de usuario o seleccionar **iniciarla** si ya se ha registrado la cuenta de usuario.
12. Una vez (como el usuario) ha iniciado sesión, haga clic en el identificador de usuario (dirección de correo electrónico) en la barra de navegación para mostrar el **administrar cuenta** página (Manage.aspx).  
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image2.png)
13. Haga clic en **agregar** junto a **número de teléfono** en el **administrar cuenta** página.  
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image3.png)
14. Agregar el número de teléfono donde (como el usuario) desea recibir mensajes SMS (mensajes de texto) y haga clic en el **enviar** botón.   
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image4.png)  
    En este momento, la aplicación usará las credenciales desde el *Web.config* ponerse en contacto con Twilio. Se enviará un mensaje SMS (mensaje de texto) para el teléfono asociado a la cuenta de usuario. Puede comprobar que se envió el mensaje de Twilio viendo el panel de Twilio.
15. En unos segundos, el teléfono asociado a la cuenta de usuario obtendrá un mensaje de texto que contiene el código de verificación. Escriba el código de verificación y presione **enviar**.  
     ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image5.png)

<a id="use2FA"></a>
## <a name="enable-two-factor-authentication-for-a-registered-user"></a>Habilitar la autenticación en dos fases para un usuario registrado

En este momento, ha habilitado la autenticación en dos fases para la aplicación. Para que un usuario usar la autenticación en dos fases, simplemente puede cambiar su configuración de la interfaz de usuario. 

1. Como un usuario de la aplicación puede habilitar autenticación en dos fases de su cuenta específica, haga clic en el identificador de usuario (alias de correo electrónico) en la barra de navegación para mostrar el **administrar cuenta** página. A continuación, haga clic en el **habilitar** vínculo para habilitar la autenticación en dos fases para la cuenta.![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image6.png)
2. Cerrar sesión y, a continuación, volver a iniciar sesión. Si ha habilitado el correo electrónico, puede seleccionar SMS o correo electrónico para la autenticación en dos fases. Si no ha habilitado el correo electrónico, consulte el tutorial titulado [crear una aplicación de ASP.NET Web Forms seguros con registro de usuario, confirmación por correo electrónico y restablecimiento de contraseña](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md).![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image7.png)
3. Se abrirá la página de autenticación en dos fases donde puede escribir el código (de SMS o correo electrónico).![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image8.png)  
 Al hacer clic en el **recordar este explorador** casilla eximirán de la necesidad de usar la autenticación en dos fases para iniciar sesión cuando se usa el explorador y el dispositivo donde se ha activado la casilla. Siempre y cuando los usuarios malintencionados no pueden obtener acceso al dispositivo, habilitar la autenticación en dos fases y haga clic en el **recordar este explorador** le proporcionará acceso cómodo de contraseña de un solo paso, mientras se conservan de seguros protección de autenticación en dos fases para todo el acceso desde dispositivos que no son de confianza. Puede hacerlo en cualquier dispositivo privada que se usan con frecuencia.

<a id="addRes"></a>
## <a name="additional-resources"></a>Recursos adicionales

- [Autenticación en dos fases mediante SMS y correo electrónico con ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md)
- [Recursos recomendados de vínculos a ASP.NET Identity](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Implementar una aplicación de formularios Web ASP.NET segura con pertenencia, OAuth y SQL Database en un sitio Web de Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)
- [Serie de tutoriales de formularios Web Forms ASP.NET: agregar un proveedor de OAuth 2.0](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#OAuthWebForms)
- [Serie de tutoriales de formularios Web Forms ASP.NET - habilitar SSL para el proyecto](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms)
- [Confirmación de la cuenta y contraseña de recuperación con ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [Creación de la aplicación de Facebook y conectarse al proyecto de la aplicación](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
