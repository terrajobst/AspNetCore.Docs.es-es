---
uid: identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity
title: "La cuenta de confirmación y recuperación de contraseña con la identidad de ASP.NET (C#) | Documentos de Microsoft"
author: HaoK
description: "Antes de realizar este tutorial que debe completar primero cree una aplicación web de ASP.NET MVC 5 segura con registro de restablecimiento de contraseña y de confirmación de correo electrónico. Este tutorial..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/26/2015
ms.topic: article
ms.assetid: 8d54180d-f826-4df7-b503-7debf5ed9fb3
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 548baaaa06980fb793c079b66b6edc34422eb579
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="account-confirmation-and-password-recovery-with-aspnet-identity-c"></a>Confirmación de cuenta y contraseña de recuperación con la identidad de ASP.NET (C#)
====================
por [Hao Kung](https://github.com/HaoK), [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://github.com/Rick-Anderson), [Suhas Joshi](https://github.com/suhasj)

> Antes de realizar este tutorial debe completar [crear una aplicación web de ASP.NET MVC 5 segura con registro de restablecimiento de contraseña y de confirmación de correo electrónico](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md). Este tutorial contiene información más detallada y le mostrará cómo configurar el correo electrónico de confirmación de la cuenta local y permitir a los usuarios restablecer su contraseña olvidada en ASP.NET Identity. En este artículo se escribió Rick Anderson ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)), Pranav Rastogi ([@rustd](https://twitter.com/rustd)), Hao Kung y Suhas Joshi. El ejemplo de NuGet escribió principalmente Hao Kung.


Una cuenta de usuario local requiere que el usuario crear una contraseña para la cuenta y contraseña se almacena (de forma segura) en la aplicación web. Identidad de ASP.NET también admite cuentas sociales, que no requieren que el usuario crear una contraseña para la aplicación. [Cuentas sociales](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) usar terceros (por ejemplo, Google, Twitter, Facebook o Microsoft) para autenticar a los usuarios. En este tema se trata lo siguiente:

- [Crear una aplicación de ASP.NET MVC](#createMvc) y explore las características de ASP.NET Identity.
- [Generar el ejemplo de identidad](#build)
- [Configurar la confirmación por correo electrónico](#email)

Los nuevos usuarios registran sus alias de correo electrónico, que crea una cuenta local.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image1.png)

Haga clic en el botón Registrar envía un correo electrónico de confirmación que contiene un símbolo (token) de validación a su dirección de correo electrónico.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image2.png)

El usuario se envía un correo electrónico con un token de confirmación de su cuenta.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image3.png)

Haga clic en el vínculo, se confirma la cuenta.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image4.png)

<a id="passwordReset"></a>

## <a name="password-recoveryreset"></a>Recuperación/restablecimiento de contraseña

Los usuarios locales que olviden su contraseña pueden tener un token de seguridad que se envía a su cuenta de correo electrónico, les permite restablecer su contraseña lo que.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image5.png)  
  
Pronto, el usuario recibirá un correo electrónico con un vínculo lo que les permite restablecer su contraseña.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image6.png)  
Haga clic en el vínculo lleva a la página de restablecimiento.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image7.png)  
  
Al hacer clic en el **restablecer** botón confirmará que se ha restablecido la contraseña.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image8.png)

<a id="createMvc"></a>

## <a name="create-an-aspnet-web-app"></a>Crear una aplicación Web ASP.NET

Empiece por instalar y ejecutar [Visual Studio Express 2013 para Web](https://go.microsoft.com/fwlink/?LinkId=299058) o [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Instalar Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) o superior.

> [!NOTE]
> Advertencia: Debe instalar Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) para completar este tutorial.


1. Cree un nuevo proyecto Web de ASP.NET y seleccione la plantilla MVC. Formularios Web Forms también admite la identidad de ASP.NET, por lo que podría seguir pasos similares en una aplicación de formularios web.
2. Deje la autenticación predeterminada como **cuentas de usuario individuales**.
3. Ejecutar la aplicación, haga clic en el **registrar** vincular y registrar un usuario. En este punto, es la única validación en el correo electrónico con el [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) atributo.
4. En el Explorador de servidores, vaya a **datos Connections\DefaultConnection\Tables\AspNetUsers**, haga clic en y seleccione **Abrir definición de tabla**.

    La siguiente imagen muestra la `AspNetUsers` esquema:

    ![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image9.png)
5. Haga clic con el botón secundario en el **AspNetUsers** de tabla y seleccione **mostrar datos de tabla**.  
  
    ![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image10.png)  
  
 En este momento, no se ha confirmado el correo electrónico.

El almacén de datos predeterminado para ASP.NET Identity es Entity Framework, pero puede configurarlo para usar otros almacenes de datos y para agregar campos adicionales. Vea [recursos adicionales](#addRes) sección al final de este tutorial.

El [clase de inicio OWIN](../../../aspnet/overview/owin-and-katana/owin-startup-class-detection.md) ( *Startup.cs* ) se llama cuando la aplicación se inicia y se invoca el `ConfigureAuth` método *aplicación\_Start\Startup.Auth.cs*, que configura la canalización OWIN e inicializa ASP.NET Identity. Examine el `ConfigureAuth` método. Cada `CreatePerOwinContext` llamada registra una devolución de llamada (guardado en el `OwinContext`) que se llamará una vez por solicitud para crear una instancia del tipo especificado. Puede establecer un punto de interrupción en el constructor y `Create` método de cada tipo (`ApplicationDbContext, ApplicationUserManager`) y compruebe que se llaman en cada solicitud. Una instancia de `ApplicationDbContext` y `ApplicationUserManager` se almacena en el contexto OWIN, que se puede acceder a lo largo de la aplicación. ASP.NET Identity se enlaza a la canalización OWIN a través de middleware de cookies. Para obtener más información, consulte [por administración de la duración de solicitud para la clase UserManager en ASP.NET Identity](https://blogs.msdn.com/b/webdev/archive/2014/02/12/per-request-lifetime-management-for-usermanager-class-in-asp-net-identity.aspx).

Cuando se cambia el perfil de seguridad, un nuevo sello de seguridad se generan y almacenan en la `SecurityStamp` campo de la *AspNetUsers* tabla. Tenga en cuenta que el `SecurityStamp` campo es diferente de la cookie de seguridad. La cookie de seguridad no se almacena en la `AspNetUsers` tabla (o cualquier otra estructura en la base de datos de identidad en). El token de cookie de seguridad es autofirmado mediante [DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx) y se crea con el `UserId, SecurityStamp` e información de tiempo de expiración.

El middleware de cookies, comprueba la cookie en cada solicitud. El `SecurityStampValidator` método en el `Startup` clase llega a la base de datos y comprueba el sello de seguridad de forma periódica, según se haya especificado con el `validateInterval`. Esto sólo ocurre cada 30 minutos (en nuestro ejemplo) a menos que cambie el perfil de seguridad. El intervalo de 30 minutos se ha elegido para reducir los viajes a la base de datos. Consulte mi [tutorial de autenticación en dos fases](index.md) para obtener más detalles.

Por los comentarios en el código, el `UseCookieAuthentication` método es compatible con la autenticación con cookies. La `SecurityStamp` código de campo y asociado proporciona un nivel adicional de seguridad a la aplicación, al cambiar la contraseña, se registrarán fuera del explorador que está conectado. El `SecurityStampValidator.OnValidateIdentity` método permite que la aplicación validar el token de seguridad cuando el usuario inicia sesión, que se usa cuando se cambia una contraseña o usa el inicio de sesión externo. Esto es necesario para asegurarse de que se invalidan los tokens (cookies) que se genera con la contraseña antigua. En el proyecto de ejemplo, si cambia la contraseña de los usuarios, a continuación, un nuevo símbolo (token) se genera para el usuario, se invalidan los tokens anteriores y `SecurityStamp` se actualiza el campo.

El sistema de identidades le permiten configurar la aplicación, cuando cambia el perfil de seguridad de los usuarios (por ejemplo, cuando el usuario cambie su contraseña o cambios asociado el inicio de sesión (por ejemplo, Facebook, Google, cuenta de Microsoft, etc.), el usuario ha iniciado de todos los instancias del explorador. Por ejemplo, la imagen siguiente muestra el [ejemplo de cierre de sesión único](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SingleSignOutSample/readme.txt) aplicación, lo que permite al usuario cerrar sesión en todas las instancias del explorador (en este caso, Internet Explorer, Firefox y Chrome) haciendo clic en un botón. Como alternativa, el ejemplo permite solo cerrar sesión en una instancia de explorador específico.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image11.png)

El [ejemplo de cierre de sesión único](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SingleSignOutSample/readme.txt) aplicación muestra cómo ASP.NET Identity permite volver a generar el token de seguridad. Esto es necesario para asegurarse de que se invalidan los tokens (cookies) que se genera con la contraseña antigua. Esta característica proporciona un nivel adicional de seguridad a la aplicación; al cambiar la contraseña, se conectará a donde ha iniciado sesión en esta aplicación.

El *aplicación\_Start\IdentityConfig.cs* archivo contiene la `ApplicationUserManager`, `EmailService` y `SmsService` clases. El `EmailService` y `SmsService` clases cada implementan la `IIdentityMessageService` de la interfaz, por lo que tiene los métodos comunes de cada clase para configurar correo electrónico y SMS. Aunque este tutorial solo muestra cómo agregar la notificación de correo electrónico a través de [SendGrid](http://sendgrid.com/), puede enviar correo electrónico mediante SMTP y otros mecanismos.

El `Startup` clase también contiene el modelo para agregar inicios de sesión sociales (Facebook, Twitter, etc.), vea el tutorial [aplicación de MVC 5 con Facebook, Twitter, LinkedIn y en el inicio de sesión de Google OAuth2](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) para obtener más información.

Examine el `ApplicationUserManager` (clase), que contiene la información de identidad de los usuarios y configura las siguientes características:

- Requisitos de seguridad de contraseña.
- Bloqueo de usuario (intentos y el tiempo).
- Autenticación en dos fases (2FA). Se cubren 2FA y SMS en el otro tutorial.
- Enlazar el correo electrónico y servicios de SMS. (Tratarán SMS en otro tutorial).

El `ApplicationUserManager` clase se deriva de la interfaz genérica `UserManager<ApplicationUser>` clase. `ApplicationUser`se deriva de [IdentityUser](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework.identityuser.aspx). `IdentityUser`se deriva de la interfaz genérica `IdentityUser` clase:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample1.cs)]

Las propiedades anteriores coincidan con las propiedades de la `AspNetUsers` tabla, que aparece anteriormente.

Argumentos genéricos en `IUser` le permiten derivar una clase con distintos tipos para la clave principal. Consulte la [ChangePK](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/ChangePK/readme.txt) ejemplo que muestra cómo cambiar la clave principal de string a int o GUID.

### <a name="applicationuser"></a>ApplicationUser

`ApplicationUser`(`public class ApplicationUserManager : UserManager<ApplicationUser>`) se define en *Models\IdentityModels.cs* como:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample2.cs?highlight=8-9)]

El resaltado código anterior genera un [ClaimsIdentity](https://msdn.microsoft.com/library/system.security.claims.claimsidentity.aspx). ASP.NET Identity y OWIN Cookie de autenticación basada en notificaciones, por lo tanto, el marco de trabajo requiere la aplicación para generar un `ClaimsIdentity` para el usuario. `ClaimsIdentity`contiene información acerca de todas las notificaciones para el usuario, como el nombre del usuario, la edad y cuáles son las funciones que pertenece el usuario. También puede agregar más notificaciones para el usuario en esta fase.

OWIN `AuthenticationManager.SignIn` método pasa en el `ClaimsIdentity` e inicia sesión en el usuario:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample3.cs?highlight=4-6)]

[Aplicación de MVC 5 con Facebook, Twitter, LinkedIn y en el inicio de sesión de Google OAuth2](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) muestra cómo puede agregar propiedades adicionales para la `ApplicationUser` clase.

## <a name="email-confirmation"></a>Confirmación por correo electrónico

Es una buena idea para confirmar el correo electrónico de un nuevo usuario registrar con para comprobar que no utiliza la suplantación otra persona (es decir, no ha registrado con el correo electrónico de otra persona). Supongamos que tenemos un foro de discusión, puede que desee evitar `"bob@example.com"` al registrar como `"joe@contoso.com"`. Sin confirmación por correo electrónico, `"joe@contoso.com"` pudo obtener el correo electrónico no deseado de la aplicación. Suponga que Roberto accidentalmente registrado como `"bib@example.com"` y no vio, no podrá usar la recuperación de contraseña porque la aplicación no tiene su correo electrónico correcto. Confirmación por correo electrónico proporciona sólo una protección limitada de robots y no proporciona protección de correo basura determinado, tienen muchos alias de correo electrónico de trabajo que puede usar para registrar. En el ejemplo siguiente, el usuario no podrá cambiar su contraseña hasta que se ha confirmado su cuenta (por ellas haciendo clic en un vínculo de confirmación recibido en la cuenta de correo electrónico que se registran con.) Puede aplicar este flujo de trabajo a otros escenarios, por ejemplo, al enviar un vínculo para confirmar y restablecer la contraseña en las nuevas cuentas creadas por el Administrador de envío de correo electrónico al usuario cuando ha cambiado su perfil y así sucesivamente. En general conveniente evitar que los nuevos usuarios se registren todos los datos del sitio web antes de que se han confirmado por correo electrónico, un mensaje de texto SMS u otro mecanismo. <a id="build"></a>

## <a name="building-a-more-complete-sample"></a>Al compilar un ejemplo más completo

En esta sección, deberá usar NuGet para descargar un ejemplo más completo, con que vamos a trabajar.

1. Crear un nuevo ***vacía*** proyecto Web de ASP.NET.
2. En la consola de administrador de paquetes, escriba lo siguiente los siguientes comandos: 

    [!code-console[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample4.cmd)]

 En este tutorial, usaremos [SendGrid](http://sendgrid.com/) para enviar correo electrónico. El `Identity.Samples` paquete instala el código que se va a trabajar.
3. Establecer el [proyecto para usar SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
4. Creación de la cuenta local de pruebas mediante la ejecución de la aplicación, haga clic en el **registrar** vincular y publicar el formulario de registro.
5. Haga clic en el vínculo de correo electrónico de demostración, que simula la confirmación por correo electrónico.
6. Eliminar el código de confirmación del vínculo de correo electrónico de demostración de la muestra (el `ViewBag.Link` código en el controlador de la cuenta. Consulte la `DisplayEmail` y `ForgotPasswordConfirmation` métodos de acción y vistas de razor).

> [!NOTE]
> Advertencia: Si cambia cualquiera de las opciones de seguridad en este ejemplo, aplicaciones de ensayo deberá someterse a una auditoría de seguridad que se llame explícitamente a los cambios realizados.


## <a name="examine-the-code-in-appstartidentityconfigcs"></a>Examine el código de aplicación\_Start\IdentityConfig.cs

El ejemplo muestra cómo crear una cuenta y agréguela a la *administración* rol. Debe reemplazar el correo electrónico en el ejemplo con el correo electrónico que se va a usar para la cuenta de administrador. La manera más fácil ahora para crear una cuenta de administrador está mediante programación en el `Seed` método. Esperamos que en el futuro una herramienta que le permitirá crear y administrar usuarios y roles. El código de ejemplo le permiten crear y administrar usuarios y roles, pero primero debe tener una cuenta de administradores para ejecutar los roles y las páginas de administración de usuario. En este ejemplo, se crea la cuenta de administrador cuando la base de datos es visible.

Cambiar la contraseña y cambie el nombre a una cuenta que puede recibir notificaciones por correo electrónico.

> [!WARNING]
> Seguridad - nunca almacenar los datos confidenciales en el código fuente.

Como se mencionó anteriormente, el `app.CreatePerOwinContext` llamada en la clase de inicio agrega las devoluciones de llamada para el `Create` método la aplicación DB contenidas, administrador y el rol de administrador de clases de usuario. Llamadas de canalización de OWIN la `Create` método en estas clases para cada solicitud y almacena el contexto para cada clase. El controlador account expone el Administrador de usuarios desde el contexto HTTP (que contiene el contexto OWIN):

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample5.cs)]

Cuando un usuario registra una cuenta local, el `HTTP Post Register` se llama al método:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample6.cs)]

El código anterior usa el modelo de datos para crear una nueva cuenta de usuario mediante el correo electrónico y una contraseña escritos. Si el alias de correo electrónico está en el almacén de datos, se produce un error en la creación de cuentas y vuelve a aparecer el formulario. El `GenerateEmailConfirmationTokenAsync` método crea un token de confirmación segura y lo almacena en el almacén de datos de identidades de ASP.NET. El [Url.Action](https://msdn.microsoft.com/library/dd505232(v=vs.118).aspx) método crea un vínculo que contiene el `UserId` y token de confirmación. Este vínculo, a continuación, se envían por correo electrónico al usuario, el usuario puede hacer clic en el vínculo de su aplicación de correo electrónico para confirmar su cuenta.

<a id="email"></a>

## <a name="set-up-email-confirmation"></a>Configurar la confirmación por correo electrónico

Vaya a la [página de registro Azure SendGrid](https://azure.microsoft.com/gallery/store/sendgrid/sendgrid-azure/) y registrar una cuenta de forma gratuita. Agregue código similar al siguiente para configurar SendGrid:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample7.cs?highlight=5)]

> [!NOTE]
> Con frecuencia, los clientes de correo electrónico acepten sólo los mensajes de texto (no HTML). Debe proporcionar el mensaje de texto y HTML. En el ejemplo anterior de SendGrid, esto se consigue con la `myMessage.Text` y `myMessage.Html` código mostrado anteriormente.


El código siguiente muestra cómo enviar correo electrónico utilizando el [MailMessage](https://msdn.microsoft.com/library/system.net.mail.mailmessage.aspx) clase where `message.Body` devuelve solo el vínculo.

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample8.cs)]

> [!WARNING]
> Seguridad - nunca almacenar los datos confidenciales en el código fuente. La cuenta y las credenciales se almacenan en el appSetting. En Azure, puede almacenar con seguridad estos valores en el  **[configurar](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)**  ficha en el portal de Azure. Vea [las prácticas recomendadas para implementar las contraseñas y otros datos confidenciales en ASP.NET y Azure](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).


Escriba las credenciales de SendGrid, ejecutar la aplicación, registrarse con un alias de correo electrónico puede haga clic en el vínculo de confirmación en el correo electrónico. Para ver cómo hacerlo con su [Outlook.com](http://outlook.com) cuentas de correo electrónico, consulte de John Atten [configuración SMTP de C# para el Host de SMTP de Outlook.Com](http://typecastexception.com/post/2013/12/20/C-SMTP-Configuration-for-OutlookCom-SMTP-Host.aspx) y su[ASP.NET 2.0 de identidad: configuración de la validación de la cuenta y la autorización de dos fases](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) envía.

Una vez que un usuario hace clic en el **registrar** botón se envía un correo electrónico de confirmación que contiene un token de validación a su dirección de correo electrónico.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image12.png)

El usuario se envía un correo electrónico con un token de confirmación de su cuenta.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image13.png)

## <a name="examine-the-code"></a>Examine el código

En el código siguiente se muestra el método `POST ForgotPassword`.

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample9.cs)]

El método produce un error en modo silencioso si no se ha confirmado el correo electrónico del usuario. Si se ha registrado un error para una dirección de correo electrónico no válida, los usuarios malintencionados pudieron utilizar esa información para buscar el identificador de usuario válido (alias de correo electrónico) a los ataques.

El código siguiente muestra el `ConfirmEmail` método en el controlador de la cuenta que se llama cuando el usuario hace clic en el vínculo de confirmación del correo electrónico enviado a estos:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample10.cs)]

Una vez que se ha utilizado un token de contraseña olvidada, se invalida. El cambio en el código siguiente en el `Create` (método) (en el *aplicación\_Start\IdentityConfig.cs* archivo) establece los tokens a punto de expirar en 3 horas.

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample11.cs?highlight=6-8)]

Con el código anterior, la contraseña olvidada y los tokens de confirmación de correo electrónico expirará en 3 horas. El valor predeterminado `TokenLifespan` es un día.

El código siguiente muestra el método de confirmación de correo electrónico:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample12.cs)]

 Para hacer que su aplicación más segura, ASP.NET Identity admite la autenticación en dos fases (2FA). Vea [identidad de ASP.NET 2.0: configurar la validación de la cuenta y la autorización de dos fases](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) por John Atten. Aunque puede establecer el bloqueo de cuenta en los errores de intento de contraseña de inicio de sesión, este enfoque hace susceptible a su inicio de sesión [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) bloqueos. Se recomienda que usar el bloqueo de cuenta con 2FA.  
<a id="addRes"></a>

## <a name="additional-resources"></a>Recursos adicionales

- [Información general sobre los proveedores de almacenamiento personalizado para ASP.NET Identity](../extensibility/overview-of-custom-storage-providers-for-aspnet-identity.md)
- [Aplicación de MVC 5 con Facebook, Twitter, LinkedIn y Google OAuth2 Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) también muestra cómo agregar información de perfil a la tabla de usuarios.
- [ASP.NET MVC e identidad 2.0: comprende los fundamentos](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) por John Atten.
- [Introducción a ASP.NET Identity](../getting-started/introduction-to-aspnet-identity.md)
- [Anuncio de RTM de ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) por Pranav Rastogi.
