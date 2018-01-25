---
uid: mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
title: "Aplicación de ASP.NET MVC 5 con SMS y correo electrónico de autenticación en dos fases | Documentos de Microsoft"
author: Rick-Anderson
description: "Este tutorial muestra cómo compilar una aplicación web de ASP.NET MVC 5 con la autenticación en dos fases. Debe completar la aplicación web de crear un seguro ASP.NET MVC 5 con..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/20/2015
ms.topic: article
ms.assetid: f50a5cdb-c06a-46ed-aa14-fc5b049dc8dc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: d6bc92f3cbe6b61332e33e8a507b4516bf5c15a5
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication"></a>Aplicación de ASP.NET MVC 5 con SMS y correo electrónico de autenticación en dos fases
====================
Por [Rick Anderson](https://github.com/Rick-Anderson)

> Este tutorial muestra cómo compilar una aplicación web de ASP.NET MVC 5 con la autenticación en dos fases. Debe completar [crear una aplicación web de ASP.NET MVC 5 segura con registro de restablecimiento de contraseña y de confirmación de correo electrónico](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) antes de continuar. Puede descargar la aplicación completa [aquí](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952). La descarga contiene aplicaciones auxiliares de depuración que permiten probar confirmación por correo electrónico y SMS sin necesidad de preparar un correo electrónico o el proveedor de SMS.
> 
> Este tutorial se escribió por [Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).


- [Crear una aplicación de ASP.NET MVC](#createMvc)
- [Configurar SMS para la autenticación en dos fases](#SMS)
- [Habilitar la autenticación en dos fases](#enable2)
- [Recursos adicionales](#addRes)

<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a>Crear una aplicación de ASP.NET MVC

Empiece por instalar y ejecutar [Visual Studio Express 2013 para Web](https://go.microsoft.com/fwlink/?LinkId=299058) o [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Instalar [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) o superior.

> [!NOTE]
> Advertencia: Se deben realizar [crear una aplicación web de ASP.NET MVC 5 segura con registro de restablecimiento de contraseña y de confirmación de correo electrónico](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) antes de continuar. Debe instalar [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) o superior para completar este tutorial.


1. Cree un nuevo proyecto Web de ASP.NET y seleccione la plantilla MVC. Formularios Web Forms también admite la identidad de ASP.NET, por lo que podría seguir pasos similares en una aplicación de formularios web.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image1.png)
2. Deje la autenticación predeterminada como **cuentas de usuario individuales**. Si desea hospedar la aplicación en Azure, deje activada la casilla de verificación. Más adelante en el tutorial se implementará en Azure. También puede [abrir una cuenta de Azure de forma gratuita](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).
3. Establecer el [proyecto para usar SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).

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
  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image2.png)  
  
 Dirección:  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
 Espacio de nombres:  
    `ASPSMSX2`
3. **Pensar en las credenciales de usuario del proveedor de SMS**  
  
 Twilio:  
 Desde el **panel** ficha de la cuenta de Twilio, copia el **SID de cuenta** y **token de autenticación**.  
  
 ASPSMS:  
 Desde la configuración de su cuenta, vaya a **clave de usuario confidenciales** y copie junto con su propio definido **contraseña**.  
  
 Más adelante se almacenará estos valores en el *web.config* archivo dentro de las claves `"SMSAccountIdentification"` y `"SMSAccountPassword"` .
4. **Especifica el identificador del remitente / originador**  
  
 Twilio:  
 Desde el **números** ficha, copie el número de teléfono Twilio.  
  
 ASPSMS:  
 En el **desbloquear remitentes** menú, desbloquear uno o más remitentes o elija un originador alfanumérico (no admite todas las redes).  
  
 Más adelante se almacenará este valor en el *web.config* archivo dentro de la clave `"SMSAccountFrom"` .
5. **Transferir las credenciales del proveedor SMS en la aplicación**  
  
 Disponer de las credenciales y el número de teléfono del remitente a la aplicación. Para no complicar las cosas se almacenará estos valores en el *web.config* archivo. Cuando se implementa en Azure, podemos almacenar los valores de forma segura en el **configuración de la aplicación** pestaña de configuración de la sección en el sitio web. 

    [!code-xml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample1.xml?highlight=8-10)]

    > [!WARNING]
    > Seguridad - nunca almacenar los datos confidenciales en el código fuente. La cuenta y las credenciales se agregan al código anterior para simplificar el ejemplo. Vea [las prácticas recomendadas para implementar las contraseñas y otros datos confidenciales en ASP.NET y Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).
6. **Implementación de transferencia de datos al proveedor SMS**  
  
 Configurar la `SmsService` clase en el *aplicación\_Start\IdentityConfig.cs* archivo.  
  
 En función del proveedor SMS usado activar cualquiera el **Twilio** o **ASPSMS** sección: 

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample2.cs)]
7. Actualización de la *Views\Manage\Index.cshtml* vista Razor: (Nota: no basta con quitar los comentarios en el código existente, utilice el código siguiente.)  

    [!code-cshtml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample3.cshtml?highlight=29-66)]
8. Compruebe el `EnableTwoFactorAuthentication` y `DisableTwoFactorAuthentication` métodos de acción en el `ManageController` tienen la[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.118).aspx) atributo:  

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample4.cs?highlight=3,16)]
9. Ejecutar la aplicación e inicie sesión con la cuenta que se registraron anteriormente.
10. Haga clic en el identificador de usuario, que activa el `Index` método de acción en `Manage` controlador.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image3.png)
11. Haga clic en Agregar.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image4.png)
12. El `AddPhoneNumber` método de acción muestra un cuadro de diálogo para especificar un número de teléfono que puede recibir mensajes SMS.

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample5.cs)]

    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image5.png)
13. En unos segundos, obtendrá un mensaje de texto con el código de comprobación. Escriba y presione **enviar**.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image6.png)
14. La vista gestionar muestra que el número de teléfono se ha agregado.

<a id="enable2"></a>
## <a name="enable-two-factor-authentication"></a>Habilitar la autenticación en dos fases

En la aplicación de la plantilla genera, debe usar la interfaz de usuario para habilitar la autenticación en dos fases (2FA). Para habilitar 2FA, haga clic en el identificador de usuario (alias de correo electrónico) en la barra de navegación.

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image7.png)

Haga clic en Habilitar 2FA.

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image8.png)

Cierre de sesión, a continuación, vuelva a iniciar sesión. Si ha habilitado el correo electrónico (consulte mi [tutorial anterior](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)), puede seleccionar el SMS o correo electrónico para 2FA.

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image9.png)

Se muestra la página de códigos comprobar donde puede escribir el código (de SMS o correo electrónico).

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image10.png)

Al hacer clic en el **recordar este explorador** casilla de verificación se excluya de la necesidad de usar 2FA para iniciar sesión cuando se usa el explorador y el dispositivo que ha seleccionado la casilla. Siempre y cuando los usuarios malintencionados no pueden obtener acceso al dispositivo, lo que permite 2FA y haciendo clic en el **recordar este explorador** le proporcionará acceso cómodo de contraseña de un solo paso, mientras se conservan la protección segura 2FA para todos los accesos desde dispositivos no son de confianza. Puede hacerlo en cualquier dispositivo privada que se usan con frecuencia.

Este tutorial proporciona una introducción rápida a habilitar 2FA en una nueva aplicación de ASP.NET MVC. Mi tutorial [autenticación en dos fases mediante SMS y correo electrónico con ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) entra en detalle en el código subyacente del ejemplo.

<a id="addRes"></a>
## <a name="additional-resources"></a>Recursos adicionales

- [Autenticación en dos fases mediante SMS y correo electrónico con ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) entra en detalle en la autenticación en dos fases
- [Vínculos a la identidad de ASP.NET de los recursos recomendados](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [La cuenta de confirmación y recuperación de contraseña con ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) explica con más detalle en la confirmación de contraseña de cuenta y la recuperación.
- [Aplicación de MVC 5 con Facebook, Twitter, LinkedIn y Google OAuth2 Sign-on](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) este tutorial muestra cómo escribir una aplicación de ASP.NET MVC 5 con autorización de Facebook y Google OAuth 2. También muestra cómo agregar datos adicionales a la base de datos de identidad.
- [Implementar una aplicación de MVC de ASP.NET seguras con pertenencia, OAuth y base de datos SQL en Azure Web](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Este tutorial agrega la implementación de Azure, cómo proteger su aplicación con roles, cómo usar la API de pertenencia para agregar usuarios y roles y características de seguridad adicionales.
- [Crear una aplicación de Google para OAuth 2 y conectar la aplicación al proyecto](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [Crear la aplicación de Facebook y conectar la aplicación al proyecto](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [Cómo configurar SSL en el proyecto](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
