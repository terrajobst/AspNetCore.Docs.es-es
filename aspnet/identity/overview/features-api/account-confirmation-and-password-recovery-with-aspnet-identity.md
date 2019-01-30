---
uid: identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity
title: La cuenta de confirmación y la recuperación de contraseña con ASP.NET Identity (C#) | Microsoft Docs
author: HaoK
description: Antes de realizar este tutorial que debe completar primero cree una aplicación web de ASP.NET MVC 5 segura con inicio de sesión, restablecimiento de confirmación y la contraseña de correo electrónico. En este tutorial...
ms.author: riande
ms.date: 01/23/2019
ms.assetid: 8d54180d-f826-4df7-b503-7debf5ed9fb3
msc.legacyurl: /identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 47dc2c1044a5964624ba2f8af4f174a2fd99d3e8
ms.sourcegitcommit: c47d7c131eebbcd8811e31edda210d64cf4b9d6b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2019
ms.locfileid: "55236411"
---
# <a name="account-confirmation-and-password-recovery-with-aspnet-identity-c"></a>Confirmación y la contraseña de recuperación con ASP.NET Identity de la cuenta (C#)

> Antes de realizar este tutorial debe completar primero [crear una aplicación web de ASP.NET MVC 5 segura con inicio de sesión, restablecimiento de confirmación y la contraseña de correo electrónico](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md). Este tutorial contiene información más detallada y le mostrará cómo configurar el correo electrónico de confirmación de la cuenta local y permitir a los usuarios restablecer su contraseña olvidada en ASP.NET Identity.

Una cuenta de usuario local requiere que el usuario crear una contraseña para la cuenta, y esa contraseña se almacena (de forma segura) en la aplicación web. ASP.NET Identity también es compatible con cuentas de redes sociales, que no requieren que el usuario crear una contraseña para la aplicación. [Cuentas de redes sociales](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) usar terceros (por ejemplo, Google, Twitter, Facebook o Microsoft) para autenticar a los usuarios. En este tema se trata los siguientes:

- [Crear una aplicación ASP.NET MVC](#createMvc) y explore las características de ASP.NET Identity.
- [Generar el ejemplo de identidad](#build)
- [Configuración de correo electrónico de confirmación](#email)

Los nuevos usuarios registran su alias de correo electrónico, que crea una cuenta local.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image1.png)

Seleccione el botón Registrar envía un correo electrónico de confirmación que contiene un token de validación a su dirección de correo electrónico.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image2.png)

El usuario se envía un correo electrónico con un token de confirmación de su cuenta.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image3.png)

Al seleccionar el vínculo, confirma la cuenta.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image4.png)

<a id="passwordReset"></a>

## <a name="password-recoveryreset"></a>Recuperación/restablecimiento de contraseña

Los usuarios locales que olvida su contraseña pueden tener un token de seguridad enviado a su cuenta de correo electrónico, lo que les permite restablecer su contraseña.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image5.png)  
  
El usuario en breve recibirá un correo electrónico con un vínculo que lo que les permite restablecer su contraseña.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image6.png)  
Al seleccionar el vínculo llevará a la página de restablecimiento.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image7.png)  
  
Seleccionar el **restablecer** botón confirmará que se ha restablecido la contraseña.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image8.png)

<a id="createMvc"></a>

## <a name="create-an-aspnet-web-app"></a>Crear una aplicación web ASP.NET

Comience por instalar y ejecutar [Visual Studio 2017](https://visualstudio.microsoft.com/).


1. Cree un nuevo proyecto Web ASP.NET y seleccione la plantilla MVC. Formularios Web Forms también admiten la identidad de ASP.NET, por lo que podría seguir pasos similares en una aplicación de formularios web.
2. Cambie a **cuentas de usuario individuales**.
3. Ejecute la aplicación, seleccione la **registrar** vincular y registrar un usuario. En este momento, es la única validación en el correo electrónico con el [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) atributo.
4. En el Explorador de servidores, vaya a **datos Connections\DefaultConnection\Tables\AspNetUsers**, haga clic en y seleccione **Abrir definición de tabla**.

    La siguiente imagen muestra la `AspNetUsers` esquema:

    ![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image9.png)
5. Haga doble clic en el **AspNetUsers** de tabla y seleccione **mostrar datos de tabla**.  
  
    ![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image10.png)  
  
   En este momento, no se ha confirmado el correo electrónico.

El almacén de datos predeterminado para ASP.NET Identity es Entity Framework, pero puede configurarlo para usar otros almacenes de datos y para agregar campos adicionales. Consulte [recursos adicionales](#addRes) sección al final de este tutorial.

El [clase de inicio OWIN](../../../aspnet/overview/owin-and-katana/owin-startup-class-detection.md) ( *Startup.cs* ) se llama cuando la aplicación se inicia y se invoca el `ConfigureAuth` método *aplicación\_Start\Startup.Auth.cs*, que configura la canalización de OWIN e inicializa ASP.NET Identity. Examine el método `ConfigureAuth`. Cada `CreatePerOwinContext` llamada registra una devolución de llamada (guardado en el `OwinContext`) que se llamará una vez por solicitud para crear una instancia del tipo especificado. Puede establecer un punto de interrupción en el constructor y `Create` el método de cada tipo (`ApplicationDbContext, ApplicationUserManager`) y compruebe que se les llama en cada solicitud. Una instancia de `ApplicationDbContext` y `ApplicationUserManager` se almacena en el contexto OWIN, que se puede acceder a lo largo de la aplicación. ASP.NET Identity se enlaza a la canalización de OWIN a través de middleware de cookies. Para obtener más información, consulte [por administración de la duración de solicitud para la clase UserManager en ASP.NET Identity](https://blogs.msdn.com/b/webdev/archive/2014/02/12/per-request-lifetime-management-for-usermanager-class-in-asp-net-identity.aspx).

Cuando se cambia su perfil de seguridad, se genera un nuevo sello de seguridad y se almacenan en el `SecurityStamp` campo de la *AspNetUsers* tabla. Tenga en cuenta que el `SecurityStamp` campo es diferente de la cookie de seguridad. La cookie de seguridad no se almacena en el `AspNetUsers` tabla (o en cualquier parte de la base de datos de identidad). El token de cookie de seguridad es autofirmado mediante [DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx) y se crea con el `UserId, SecurityStamp` e información de tiempo de expiración.

El middleware de cookies, comprueba la cookie en cada solicitud. El `SecurityStampValidator` método en el `Startup` clase llega a la base de datos y comprueba el sello de seguridad periódicamente, como se especifica en el `validateInterval`. Esto solo se produce cada 30 minutos (en nuestro ejemplo) a menos que cambie su perfil de seguridad. Se eligió el intervalo de 30 minutos para reducir los viajes a la base de datos. Consulte mi [tutorial de autenticación en dos fases](index.md) para obtener más detalles.

Por los comentarios en el código, el `UseCookieAuthentication` método admite la autenticación de cookies. El `SecurityStamp` asociado y campo código proporciona una capa adicional de seguridad a la aplicación, al cambiar la contraseña, se registrarán fuera del explorador que inició sesión. El `SecurityStampValidator.OnValidateIdentity` método permite que la aplicación validar el token de seguridad cuando el usuario inicia sesión, que se usa cuando se cambia una contraseña o usa el inicio de sesión externo. Esto es necesario para asegurarse de que los tokens (cookies) generados con la contraseña antigua se invalidan. En el proyecto de ejemplo, si cambia la contraseña del usuario, a continuación, un nuevo token se genera para el usuario, se invalidan los tokens anteriores y `SecurityStamp` se actualiza el campo.

El sistema de identidad le permiten configurar la aplicación, cuando cambia el perfil de seguridad de los usuarios (por ejemplo, cuando el usuario cambia su contraseña o los cambios asociados inicio de sesión (por ejemplo, Facebook, Google, cuenta de Microsoft, etc.), el usuario ha iniciado de todos los instancias del explorador. Por ejemplo, la imagen siguiente muestra el [cierre de sesión único ejemplo](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SingleSignOutSample/readme.txt) app, que permite al usuario cerrar la sesión de todas las instancias del explorador (en este caso, Internet Explorer, Firefox y Chrome) seleccionando un botón. Como alternativa, el ejemplo permite sólo cerrar sesión en una instancia del explorador específico.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image11.png)

El [cierre de sesión único ejemplo](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SingleSignOutSample/readme.txt) aplicación muestra cómo ASP.NET Identity le permite volver a generar el token de seguridad. Esto es necesario para asegurarse de que los tokens (cookies) generados con la contraseña antigua se invalidan. Esta característica proporciona una capa adicional de seguridad a su aplicación; al cambiar la contraseña, le cerrará donde haya iniciado sesión en esta aplicación.

El *aplicación\_Start\IdentityConfig.cs* archivo contiene la `ApplicationUserManager`, `EmailService` y `SmsService` clases. El `EmailService` y `SmsService` clases cada implementan el `IIdentityMessageService` interfaz, por lo que tiene métodos comunes en cada clase de configuración de correo electrónico y SMS. Aunque este tutorial solo muestra cómo agregar la notificación de correo electrónico a través de [SendGrid](http://sendgrid.com/), puede enviar correo electrónico mediante SMTP y otros mecanismos.

El `Startup` clase también contiene el modelo para agregar inicios de sesión sociales (Facebook, Twitter, etc.), consulte mi tutorial [aplicación MVC 5 con Facebook, Twitter, LinkedIn y Google OAuth2 Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) para obtener más información.

Examine el `ApplicationUserManager` (clase), que contiene la información de identidad de los usuarios y configura las siguientes características:

- Requisitos de seguridad de contraseña.
- Bloqueo de usuario (intentos y tiempo).
- Autenticación en dos fases (2FA). Me referiré 2FA y SMS en otro tutorial.
- Enlazar el correo electrónico y los servicios de SMS. (Abordaré SMS en otro tutorial).

El `ApplicationUserManager` clase se deriva el tipo genérico `UserManager<ApplicationUser>` clase. `ApplicationUser` se deriva de [IdentityUser](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework.identityuser.aspx). `IdentityUser` deriva el tipo genérico `IdentityUser` clase:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample1.cs)]

Las propiedades anteriores coinciden con las propiedades en el `AspNetUsers` tabla, se muestra arriba.

Argumentos genéricos en `IUser` permiten derivar una clase con distintos tipos para la clave principal. Consulte la [ChangePK](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/ChangePK/readme.txt) ejemplo que muestra cómo cambiar la clave principal de string a int o GUID.

### <a name="applicationuser"></a>ApplicationUser

`ApplicationUser` (`public class ApplicationUserManager : UserManager<ApplicationUser>`) se define en *Models\IdentityModels.cs* como:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample2.cs?highlight=8-9)]

El código resaltado anterior genera un [ClaimsIdentity](https://msdn.microsoft.com/library/system.security.claims.claimsidentity.aspx). ASP.NET Identity y autenticación de cookies de OWIN basada en notificaciones, por lo tanto, el marco de trabajo requiere la aplicación para generar un `ClaimsIdentity` para el usuario. `ClaimsIdentity` contiene información sobre todas las notificaciones del usuario, como el nombre del usuario, edad y de los roles que pertenece el usuario. También puede agregar más notificaciones para el usuario en esta fase.

OWIN `AuthenticationManager.SignIn` método pasa el `ClaimsIdentity` e inicia sesión en el usuario:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample3.cs?highlight=4-6)]

[Aplicación de MVC 5 con Facebook, Twitter, LinkedIn y Google OAuth2 Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) muestra cómo puede agregar propiedades adicionales para el `ApplicationUser` clase.

## <a name="email-confirmation"></a>Confirmación por correo electrónico

Es una buena idea para confirmar el correo electrónico de un nuevo usuario registrarlo para comprobar que no están suplantando persona (es decir, se hayan registrado con el correo electrónico de otra persona). Supongamos que tenemos un foro de discusión, ¿puede evitar `"bob@example.com"` registrar como `"joe@contoso.com"`. Sin confirmación por correo electrónico, `"joe@contoso.com"` podría obtener correo electrónico no deseado de su aplicación. Supongamos que Bob accidentalmente registrado como `"bib@example.com"` y se había dado cuenta, no sería capaz de usar la recuperación de contraseña porque la aplicación no tiene su correo electrónico correcta. Confirmación por correo electrónico proporciona una protección limitada solo desde los bots y no proporciona protección de correo basura determinado, tienen muchos alias de correo electrónico de trabajo que pueden usar para registrar. En el ejemplo siguiente, el usuario no podrá cambiar su contraseña hasta que se ha confirmado su cuenta (por ellos seleccionando un vínculo de confirmación recibido en la cuenta de correo electrónico que se registraron.) Puede aplicar este flujo de trabajo a otros escenarios, por ejemplo, al enviar un vínculo para confirmar y restablecer la contraseña en las cuentas nuevas creadas por el administrador, enviar al usuario un correo electrónico cuando ha cambiado su perfil y así sucesivamente. Por lo general desea impedir que todos los datos del sitio web de registro antes de que se han confirmado por correo electrónico, un mensaje de texto SMS u otro mecanismo de nuevos usuarios. <a id="build"></a>

## <a name="build-a-more-complete-sample"></a>Compilar un ejemplo más completo

En esta sección, usará NuGet para descargar un ejemplo más completo, con que vamos a trabajar.

1. Cree un nuevo ***vacía*** proyecto Web de ASP.NET.
2. En la consola de administrador de paquetes, escriba los siguientes comandos: 

    [!code-console[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample4.cmd)]

   En este tutorial, vamos a usar [SendGrid](http://sendgrid.com/) para enviar correo electrónico. El `Identity.Samples` paquete instala el código que se trabajará con.
3. Establecer el [proyecto para usar SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
4. Creación de cuenta local de pruebas mediante la ejecución de la aplicación, seleccionando la **registrar** vincular y publicar el formulario de registro.
5. Seleccione el vínculo de correo electrónico de demostración, lo que simula la confirmación por correo electrónico.
6. Eliminar el código de confirmación del vínculo de correo electrónico de demostración de la muestra (el `ViewBag.Link` código en el controlador de cuentas. Consulte la `DisplayEmail` y `ForgotPasswordConfirmation` los métodos de acción y las vistas de razor).

> [!WARNING]
> Si cambia la configuración de seguridad en este ejemplo, las aplicaciones de producción debe someterse a una auditoría de seguridad que se llame explícitamente a los cambios realizados.


## <a name="examine-the-code-in-appstartidentityconfigcs"></a>Examine el código de aplicación\_Start\IdentityConfig.cs

El ejemplo muestra cómo crear una cuenta y agréguela a la *Admin* rol. Debe reemplazar el correo electrónico en el ejemplo con el correo electrónico que se va a usar para la cuenta de administrador. Ahora para crear una cuenta de administrador, lo más sencillo es mediante programación en el `Seed` método. Esperamos que en el futuro una herramienta que le permitirá crear y administrar usuarios y roles. El código de ejemplo le permiten crear y administrar usuarios y roles, pero primero debe tener una cuenta de administradores para ejecutar los roles y las páginas de administración de usuario. En este ejemplo, se crea la cuenta de administrador cuando se inicializa la base de datos.

Cambiar la contraseña y cambie el nombre a una cuenta que puede recibir notificaciones por correo electrónico.

> [!WARNING]
> Seguridad: nunca almacene de datos confidenciales en el código fuente.

Como se mencionó anteriormente, el `app.CreatePerOwinContext` llamada de la clase startup agrega las devoluciones de llamada para el `Create` método la base de datos de aplicación contenidas, administrador y el rol de administrador de clases de usuario. Las llamadas de canalización de OWIN el `Create` método en estas clases para cada solicitud y almacena el contexto para cada clase. El controlador de cuentas expone el Administrador de usuarios desde el contexto HTTP (que contiene el contexto OWIN):

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample5.cs)]

Cuando un usuario registra una cuenta local, el `HTTP Post Register` se llama al método:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample6.cs)]

El código anterior usa los datos del modelo para crear una nueva cuenta de usuario mediante el correo electrónico y la contraseña. Si el alias de correo electrónico está en el almacén de datos, se produce un error en la creación de la cuenta y volverá a aparecer el formulario. El `GenerateEmailConfirmationTokenAsync` método crea un token de confirmación seguro y lo almacena en el almacén de datos de ASP.NET Identity. El [Url.Action](https://msdn.microsoft.com/library/dd505232(v=vs.118).aspx) método crea un vínculo que contiene el `UserId` y token de confirmación. Este vínculo, a continuación, se envía por correo electrónico al usuario, el usuario puede seleccionar el vínculo de su aplicación de correo electrónico para confirmar su cuenta.

<a id="email"></a>

## <a name="set-up-email-confirmation"></a>Configuración de correo electrónico de confirmación

Vaya a la [SendGrid de Azure página de registro](https://azure.microsoft.com/gallery/store/sendgrid/sendgrid-azure/) y registro de cuenta de forma gratuita. Agregue código similar al siguiente para configurar SendGrid:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample7.cs?highlight=5)]

> [!NOTE]
> Los clientes de correo electrónico con frecuencia acepten sólo los mensajes de texto (no HTML). Debe proporcionar el mensaje de texto y HTML. En el ejemplo anterior de SendGrid, esto se realiza con el `myMessage.Text` y `myMessage.Html` código mostrado anteriormente.


El código siguiente muestra cómo enviar correo electrónico con el [MailMessage](https://msdn.microsoft.com/library/system.net.mail.mailmessage.aspx) clase where `message.Body` devuelve solo el vínculo.

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample8.cs)]

> [!WARNING]
> Seguridad: nunca almacene de datos confidenciales en el código fuente. La cuenta y las credenciales se almacenan en la appSetting. En Azure, puede almacenar con seguridad estos valores en el **[configurar](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** ficha en el portal de Azure. Consulte [procedimientos recomendados para implementar contraseñas y otros datos confidenciales en ASP.NET y Azure](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).


Escriba sus credenciales de SendGrid, ejecute la aplicación, registrarse con un alias de correo electrónico puede seleccionar el vínculo de confirmación en el correo electrónico. Para ver cómo hacerlo con su [Outlook.com](http://outlook.com) cuentas de correo electrónico, consulte de John Atten [ C# configuración SMTP para el Host de SMTP Outlook.Com](http://typecastexception.com/post/2013/12/20/C-SMTP-Configuration-for-OutlookCom-SMTP-Host.aspx) y su[Identity de ASP.NET 2.0: Configuración de validación de la cuenta y la autorización de dos fases](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) publicaciones.

Una vez que un usuario selecciona el **registrar** botón se envía un correo electrónico de confirmación que contiene un token de validación a su dirección de correo electrónico.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image12.png)

El usuario se envía un correo electrónico con un token de confirmación de su cuenta.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image13.png)

## <a name="examine-the-code"></a>Examine el código

En el código siguiente se muestra el método `POST ForgotPassword`.

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample9.cs)]

El método produce un error silencioso si no se ha confirmado el correo electrónico del usuario. Si se ha registrado un error para una dirección de correo electrónico no válida, los usuarios malintencionados podrían usar esa información para encontrar el identificador de usuario válido (alias de correo electrónico) a los ataques.

El código siguiente muestra el `ConfirmEmail` método del controlador de cuentas que se llama cuando el usuario selecciona el vínculo de confirmación en el correo electrónico enviado a ellas:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample10.cs)]

Una vez que se ha utilizado un token de contraseña olvidada, se invalida. El cambio de código siguiente en el `Create` método (en el *aplicación\_Start\IdentityConfig.cs* archivo) establece los tokens que caducan en 3 horas.

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample11.cs?highlight=6-8)]

Con el código anterior, ha olvidado la contraseña y los tokens de confirmación de correo electrónico expirará en 3 horas. El valor predeterminado `TokenLifespan` es un día.

El código siguiente muestra el método de confirmación de correo electrónico:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample12.cs)]

 Para que la aplicación más segura, ASP.NET Identity admite autenticación en dos fases (2FA). Consulte [Identity de ASP.NET 2.0: Configuración de validación de la cuenta y la autorización de dos fases](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) por John Atten. Aunque puede establecer el bloqueo de cuenta en errores de intento de contraseña de inicio de sesión, ese enfoque hace que el inicio de sesión pueda sufrir [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) bloqueos. Se recomienda que usar el bloqueo de cuenta solo con 2FA.  
<a id="addRes"></a>

## <a name="additional-resources"></a>Recursos adicionales

- [Información general sobre los proveedores de almacenamiento personalizado para ASP.NET Identity](../extensibility/overview-of-custom-storage-providers-for-aspnet-identity.md)
- [Aplicación de MVC 5 con Facebook, Twitter, LinkedIn y Google OAuth2 Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) también muestra cómo agregar información de perfil a la tabla de usuarios.
- [ASP.NET MVC e identidad 2.0: Comprender los aspectos básicos](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) por John Atten.
- [Introducción a ASP.NET Identity](../getting-started/introduction-to-aspnet-identity.md)
- [Anuncio de RTM de ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) de Pranav Rastogi.
