---
uid: mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
title: Aplicación de ASP.NET MVC 5 con SMS y correo electrónico de autenticación en dos fases | Microsoft Docs
author: Rick-Anderson
description: Este tutorial muestra cómo compilar una aplicación web de ASP.NET MVC 5 con autenticación en dos fases. Debe completar la aplicación web de crear una ASP.NET MVC 5 segura con...
ms.author: riande
ms.date: 08/20/2015
ms.assetid: f50a5cdb-c06a-46ed-aa14-fc5b049dc8dc
msc.legacyurl: /mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: 97b73c5ae18a528d33b44a4de4afc434ac46af48
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577605"
---
<a name="aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication"></a>Aplicación de ASP.NET MVC 5 con SMS y correo electrónico de autenticación en dos fases
====================
por [Rick Anderson]((https://twitter.com/RickAndMSFT))

> Este tutorial muestra cómo compilar una aplicación web de ASP.NET MVC 5 con autenticación en dos fases. Debe completar [crear una aplicación web de ASP.NET MVC 5 segura con inicio de sesión, restablecimiento de confirmación y la contraseña de correo electrónico](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) antes de continuar. Puede descargar la aplicación completada [aquí](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952). La descarga contiene aplicaciones auxiliares de depuración que le permiten probar la confirmación por correo electrónico y SMS sin tener que configurar un correo electrónico o el proveedor de SMS.
> 
> En este tutorial se escribió por [Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).


- [Crear una aplicación ASP.NET MVC](#createMvc)
- [Configuración de SMS para la autenticación en dos fases](#SMS)
- [Habilitar la autenticación en dos fases](#enable2)
- [Recursos adicionales](#addRes)

<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a>Crear una aplicación ASP.NET MVC

Comience por instalar y ejecutar [Visual Studio Express 2013 para Web](https://go.microsoft.com/fwlink/?LinkId=299058) o [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Instalar [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) o superior.

> [!NOTE]
> Advertencia: Debe completar [crear una aplicación web de ASP.NET MVC 5 segura con inicio de sesión, restablecimiento de confirmación y la contraseña de correo electrónico](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) antes de continuar. Debe instalar [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) o superior para completar este tutorial.


1. Cree un nuevo proyecto Web ASP.NET y seleccione la plantilla MVC. Formularios Web Forms también es compatible con ASP.NET Identity, por lo que podría seguir pasos similares en una aplicación de formularios web.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image1.png)
2. Deje la autenticación predeterminada como **cuentas de usuario individuales**. Si desea hospedar la aplicación en Azure, deje activada la casilla de verificación. Más adelante en el tutorial se implementará en Azure. También puede [abrir una cuenta de Azure de forma gratuita](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).
3. Establecer el [proyecto para usar SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).

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
  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image2.png)  
  
   Dirección:  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
   Espacio de nombres:  
    `ASPSMSX2`
3. **Averiguar las credenciales de usuario del proveedor de SMS**  
  
   Twilio:  
   Desde el **panel** pestaña de la cuenta de Twilio, copia el **SID de cuenta** y **Auth token**.  
  
   ASPSMS:  
   En la configuración de su cuenta, vaya a **Userkey** y cópielo junto con su autodefinido **contraseña**.  
  
   Más adelante, almacenaremos estos valores en el *web.config* archivo dentro de las claves `"SMSAccountIdentification"` y `"SMSAccountPassword"` .
4. **Especifica el Id. de remitente / originador**  
  
   Twilio:  
   Desde el **números** pestaña, copie el número de teléfono de Twilio.  
  
   ASPSMS:  
   Dentro de la **desbloquear originadores** menú, desbloquear uno o varios de los originadores o elija un originador alfanumérico (no admite todas las redes).  
  
   Más adelante se almacenará este valor en el *web.config* archivo dentro de la clave `"SMSAccountFrom"` .
5. **Transferir las credenciales del proveedor SMS en la aplicación**  
  
   Facilitar las credenciales y el número de teléfono del remitente a la aplicación. Para simplificar las cosas se almacenará estos valores en el *web.config* archivo. Cuando se implementa en Azure, podemos almacenar los valores de forma segura en el **configuración de la aplicación** sección en el sitio web de la pestaña de configuración. 

    [!code-xml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample1.xml?highlight=8-10)]

    > [!WARNING]
    > Seguridad: nunca almacene de datos confidenciales en el código fuente. La cuenta y las credenciales se agregan al código anterior para simplificar el ejemplo. Consulte [procedimientos recomendados para implementar contraseñas y otros datos confidenciales en ASP.NET y Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).
6. **Implementación de transferencia de datos al proveedor SMS**  
  
   Configurar la `SmsService` clase en el *aplicación\_Start\IdentityConfig.cs* archivo.  
  
   En función del proveedor SMS usado activar cualquiera el **Twilio** o **ASPSMS** sección: 

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample2.cs)]
7. Actualización de la *Views\Manage\Index.cshtml* vista de Razor: (Nota: simplemente no quite los comentarios en el código existente, use el código siguiente.)  

    [!code-cshtml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample3.cshtml?highlight=29-66)]
8. Compruebe el `EnableTwoFactorAuthentication` y `DisableTwoFactorAuthentication` métodos de acción en el `ManageController` tienen la[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.118).aspx) atributo:  

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample4.cs?highlight=3,16)]
9. Ejecute la aplicación e inicie sesión con la cuenta que registró anteriormente.
10. Haga clic en el identificador de usuario, que activa el `Index` los métodos de acción `Manage` controlador.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image3.png)
11. Haga clic en Agregar.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image4.png)
12. El `AddPhoneNumber` método de acción muestra un cuadro de diálogo para especificar un número de teléfono que pueda recibir mensajes SMS.

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample5.cs)]

    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image5.png)
13. En unos pocos segundos obtendrá un mensaje de texto con el código de verificación. Escríbala y presione **enviar**.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image6.png)
14. La vista de administración se muestra que el número de teléfono se ha agregado.

<a id="enable2"></a>
## <a name="enable-two-factor-authentication"></a>Habilitar la autenticación en dos fases

En la aplicación de la plantilla generada, deberá usar la interfaz de usuario para habilitar la autenticación en dos fases (2FA). Para habilitar 2FA, haga clic en el identificador de usuario (alias de correo electrónico) en la barra de navegación.

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image7.png)

Haga clic en Habilitar 2FA.

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image8.png)

Cierre de sesión, a continuación, vuelva a iniciar sesión. Si ha activado el correo electrónico (consulte mi [tutorial anterior](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)), puede seleccionar el SMS o correo electrónico para 2FA.

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image9.png)

Se muestra la página de códigos de comprobación donde puede escribir el código (de SMS o correo electrónico).

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image10.png)

Al hacer clic en el **recordar este explorador** casilla eximirán de la necesidad de usar 2FA para iniciar sesión cuando se usa el explorador y el dispositivo donde se ha activado la casilla. Siempre y cuando los usuarios malintencionados no pueden obtener acceso al dispositivo, lo que permite 2FA y haga clic en el **recordar este explorador** le proporcionará acceso cómodo de contraseña de un solo paso, mientras se conservan la protección segura 2FA para todos los accesos desde dispositivos que no son de confianza. Puede hacerlo en cualquier dispositivo privada que se usan con frecuencia.

Este tutorial proporciona una introducción rápida a habilitar 2FA en una nueva aplicación de ASP.NET MVC. Mi tutorial [autenticación en dos fases mediante SMS y correo electrónico con ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) entra en detalle en el código subyacente del ejemplo.

<a id="addRes"></a>
## <a name="additional-resources"></a>Recursos adicionales

- [Autenticación en dos fases mediante SMS y correo electrónico con ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) entra en detalle en la autenticación en dos fases
- [Recursos recomendados de vínculos a ASP.NET Identity](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [La cuenta de confirmación y la recuperación de contraseña con ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) explica con más detalle en la confirmación de contraseña de recuperación y cuenta.
- [Aplicación de MVC 5 con Facebook, Twitter, LinkedIn y Google OAuth2 Sign-on](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) este tutorial muestra cómo escribir una aplicación ASP.NET MVC 5 con Facebook y Google OAuth 2 la autorización. También muestra cómo agregar datos adicionales a la base de datos de identidad.
- [Implementar una aplicación ASP.NET MVC segura con pertenencia, OAuth y SQL Database en Azure Web](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Este tutorial agrega la implementación de Azure, cómo proteger su aplicación con los roles, cómo usar la API de pertenencia para agregar usuarios y roles y características de seguridad adicionales.
- [Creación de una aplicación de Google para OAuth 2 y conectarse al proyecto de la aplicación](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [Creación de la aplicación de Facebook y conectarse al proyecto de la aplicación](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [Configuración de SSL en el proyecto](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
