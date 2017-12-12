---
uid: web-forms/overview/security/create-an-aspnet-web-forms-app-with-sms-two-factor-authentication
title: "Crear un sitio Web de ASP.NET aplicación de formularios con la autenticación de dos factores SMS (C#) | Documentos de Microsoft"
author: Erikre
description: "Este tutorial muestra cómo crear una aplicación de formularios Web Forms de ASP.NET con la autenticación en dos fases. Este tutorial se ha diseñado para complementar el tutorial titulado Cr..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/09/2014
ms.topic: article
ms.assetid: 716264ae-ab72-45de-bfc5-53a6237089cf
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/security/create-an-aspnet-web-forms-app-with-sms-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: 92ab5f2d7a9a29089f3d340849e229d015613509
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="create-an-aspnet-web-forms-app-with-sms-two-factor-authentication-c"></a>Crear un sitio Web de ASP.NET aplicación de formularios con la autenticación de dos factores SMS (C#)
====================
Por [Erik Reitan](https://github.com/Erikre)

[Descargar la aplicación de formularios Web de ASP.NET con correo electrónico y SMS autenticación en dos fases](https://code.msdn.microsoft.com/ASPNET-Web-Forms-App-with-5a0ff94e)

> Este tutorial muestra cómo crear una aplicación de formularios Web Forms de ASP.NET con la autenticación en dos fases. Este tutorial se ha diseñado para complementar el tutorial titulado [crear una aplicación de formularios Web Forms de ASP.NET segura con el registro de usuario, el restablecimiento de contraseña y de confirmación de correo electrónico](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md). Además, este tutorial se basa en de Rick Anderson [tutorial MVC](../../../mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).


## <a name="introduction"></a>Introducción

Este tutorial le guiará por los pasos necesarios para crear una aplicación de formularios Web Forms de ASP.NET que admite la autenticación en dos fases mediante Visual Studio. Autenticación en dos fases es un paso de autenticación de usuario adicional. Este paso adicional, genera un número único de identificación personal (NIP) durante el inicio de sesión. El PIN normalmente se envía al usuario como un correo electrónico o un mensaje SMS. El usuario de la aplicación, a continuación, entra en el PIN como medida de autenticación adicional al iniciar sesión.

### <a name="tutorial-tasks-and-information"></a>Tareas del tutorial e información:

- [Crear una aplicación de formularios Web de ASP.NET](#createWebForms)
- [La instalación de SMS y la autenticación en dos fases](#SMS)
- [Habilitar la autenticación en dos fases para el usuario registrado](#use2FA)
- [Recursos adicionales](#addRes)

<a id="createWebForms"></a>
## <a name="create-an-aspnet-web-forms-app"></a>Crear una aplicación de formularios Web de ASP.NET

Empiece por instalar y ejecutar [Visual Studio Express 2013 para Web](https://go.microsoft.com/fwlink/?LinkId=299058) o [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Instalar [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) o versiones posteriores también. Además, debe crear un [Twilio](https://www.twilio.com/try-twilio) cuenta, como se explica a continuación.

> [!NOTE]
> Importante: Debe instalar [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) o superior para completar este tutorial.


1. Cree un nuevo proyecto (**archivo**  - &gt; **nuevo proyecto**) y seleccione el **aplicación Web ASP.NET** plantilla junto con .NET Framework versión 4.5.2 desde el **nuevo proyecto** cuadro de diálogo.
2. Desde el **nuevo proyecto ASP.NET** cuadro de diálogo, seleccione la **formularios Web Forms** plantilla. Deje la autenticación predeterminada como **cuentas de usuario individuales**. A continuación, haga clic en **Aceptar** para crear el nuevo proyecto.  
    ![Cuadro de diálogo nuevo proyecto de ASP.NET](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image1.png)
3. Habilitar capa de Sockets seguros (SSL) para el proyecto. Siga los pasos disponibles en el **habilitar SSL para el proyecto** sección de la [Introducción a la serie de tutoriales de formularios Web Forms](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms).
4. En Visual Studio, abra el **Package Manager Console** (**herramientas**  - &gt; **Administrador de paquetes de NuGet**  - &gt; **Package Manager Console**) y escriba el comando siguiente:  
    `Install-Package Twilio`

<a id="SMS"></a>
## <a name="setup-sms-and-two-factor-authentication"></a>La instalación de SMS y la autenticación en dos fases

Este tutorial usa Twilio, pero se puede utilizar cualquier proveedor SMS.

1. Crear un [Twilio](https://www.twilio.com/try-twilio) cuenta.
2. Desde el **panel** ficha de la cuenta de Twilio, copia el **SID de cuenta** y **Token de autenticación.** Agregará ellos a la aplicación más tarde.
3. Desde el **números** ficha, copie su Twilio **número de teléfono** así.
4. Realizar la Twilio **SID de cuenta**, **Token de autenticación** y **número de teléfono** disponibles para la aplicación. Para no complicar las cosas almacenará estos valores en el *web.config* archivo. Cuando se implementa en Azure, puede almacenar los valores de forma segura en el **appSettings** pestaña de configuración de la sección en el sitio web. Además, al agregar el número de teléfono, usar solo números.   
 Tenga en cuenta que también puede agregar credenciales de SendGrid. SendGrid es un servicio de notificación de correo electrónico. Para obtener más información sobre cómo habilitar SendGrid, consulte la sección 'Enlace SendGrid' del tutorial titulada [crear una aplicación de Secure ASP.NET Web Forms con el registro de usuario, el restablecimiento de contraseña y de confirmación de correo electrónico.](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md)

    [!code-xml[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample1.xml?highlight=2,6-10)]

    > [!WARNING]
    > Seguridad - nunca almacenar los datos confidenciales en el código fuente. En este ejemplo, la cuenta y las credenciales se almacenan en la **appSettings** sección de la *Web.config* archivo. En Azure, puede almacenar con seguridad estos valores en el  **[configurar](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)**  ficha en el portal de Azure. Para obtener información relacionada, vea el tema de Rick Anderson [las prácticas recomendadas para implementar las contraseñas y otros datos confidenciales en ASP.NET y Azure](https://go.microsoft.com/fwlink/?LinkId=513141).
5. Configurar la `SmsService` clase en el *aplicación\_Start\IdentityConfig.cs* resaltada en amarillo cambios del archivo realizando lo siguiente: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample2.cs?highlight=5-17)]
6. Agregue el siguiente `using` instrucciones al principio de la *IdentityConfig.cs* archivo: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample3.cs?highlight=1-4)]
7. Actualización de la *Account/Manage.aspx* archivo mediante la eliminación de las líneas resaltadas en amarillo:  

    [!code-aspx[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample4.aspx?highlight=38,53,57-60,63,66,70,73)]
8. En el `Page_Load` controlador de la *Manage.aspx.cs* código subyacente, quite la línea de código resaltada en amarillo para que aparezca como sigue: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample5.cs?highlight=8)]
9. En el código subyacente de *cuenta*/*TwoFactorAuthenticationSignIn.aspx.cs*, actualizar la `Page_Load` controlador agregando el código siguiente se resaltan en amarillo: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample6.cs?highlight=3-4,13)]

 Si hace que el código anterior se cambia, no se restablecerán DropDownList "Providers" que contiene las opciones de autenticación para el primer valor. Esto le permitirá al usuario seleccionar correctamente todas las opciones para utilizar al autenticar, no solo la primera.
10. En **el Explorador de soluciones**, haga clic en *Default.aspx* y seleccione **establecer como página principal**.
11. Al probar la aplicación, primero compile la aplicación (**Ctrl**+**MAYÚS**+**B**) y, a continuación, ejecute la aplicación (**F5**) y Seleccionar una opción **registrar** para crear una nueva cuenta de usuario o seleccionar **sesión** si ya se ha registrado la cuenta de usuario.
12. Después haber iniciado (como el usuario), haga clic en el identificador de usuario (dirección de correo electrónico) en la barra de navegación para mostrar el **administrar cuenta** página (Manage.aspx).  
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image2.png)
13. Haga clic en **agregar** junto a **número de teléfono** en el **administrar cuenta** página.  
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image3.png)
14. Agregar el número de teléfono donde usted (como el usuario) le gustaría recibir mensajes SMS (mensajes de texto) y haga clic en el **enviar** botón.   
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image4.png)  
 En este punto, la aplicación usará las credenciales de la *Web.config* ponerse en contacto con Twilio. Se enviará un mensaje SMS (mensaje de texto) al teléfono asociado a la cuenta de usuario. Puede comprobar que se envió el mensaje de Twilio mediante la visualización del panel de Twilio.
15. En unos segundos, el teléfono asociado a la cuenta de usuario recibirá un mensaje de texto que contiene el código de comprobación. Escriba el código de comprobación y presione **enviar**.  
     ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image5.png)

<a id="use2FA"></a>
## <a name="enable-two-factor-authentication-for-a-registered-user"></a>Habilitar la autenticación en dos fases para un usuario registrado

En este momento, ha habilitado la autenticación en dos fases para la aplicación. Un usuario podría usar la autenticación en dos fases, simplemente puede cambiar su configuración mediante la interfaz de usuario. 

1. Como un usuario de la aplicación puede habilitar autenticación en dos fases para su cuenta específica, haga clic en el identificador de usuario (alias de correo electrónico) en la barra de navegación para mostrar el **administrar cuenta** página. A continuación, haga clic en el **habilitar** vínculo para habilitar la autenticación en dos fases para la cuenta.![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image6.png)
2. Cierre la sesión y, a continuación, volver a iniciar sesión. Si ha habilitado el correo electrónico, puede seleccionar SMS o correo electrónico para la autenticación en dos fases. Si no ha habilitado el correo electrónico, vea el tutorial titulado [crear una aplicación de Secure ASP.NET Web Forms con el registro de usuario, confirmación por correo electrónico y de restablecimiento de contraseña](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md).![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image7.png)
3. Se abrirá la página de autenticación en dos fases donde puede escribir el código (de SMS o correo electrónico).![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image8.png)  
 Al hacer clic en el **recordar este explorador** casilla de verificación se excluya de la necesidad de usar la autenticación en dos fases para iniciar sesión cuando se usa el explorador y el dispositivo que ha seleccionado la casilla. Siempre y cuando los usuarios malintencionados no pueden obtener acceso al dispositivo, habilitar la autenticación en dos fases y haciendo clic en el **recordar este explorador** le proporcionará acceso cómodo de contraseña de un solo paso, al tiempo que se mantiene seguros protección de autenticación en dos fases para todo el acceso desde dispositivos no son de confianza. Puede hacerlo en cualquier dispositivo privada que se usan con frecuencia.

<a id="addRes"></a>
## <a name="additional-resources"></a>Recursos adicionales

- [Autenticación en dos fases mediante SMS y correo electrónico con la identidad de ASP.NET](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md)
- [Vínculos a la identidad de ASP.NET de los recursos recomendados](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Implementar una aplicación de formularios Web de ASP.NET segura con pertenencia, OAuth y base de datos SQL en un sitio Web de Azure](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)
- [Serie de tutoriales de formularios Web Forms ASP.NET: agregar un proveedor de OAuth 2.0](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#OAuthWebForms)
- [Serie de tutoriales de formularios Web Forms ASP.NET - habilitar SSL para el proyecto](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms)
- [Confirmación de cuenta y contraseña de recuperación con la identidad de ASP.NET](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [Crear la aplicación de Facebook y conectar la aplicación al proyecto](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
