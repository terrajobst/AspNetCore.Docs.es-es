---
title: Confirmación de la cuenta y la recuperación de contraseñas en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo compilar una aplicación de ASP.NET Core con el restablecimiento de confirmación y la contraseña de correo electrónico.
ms.author: riande
ms.date: 7/11/2018
uid: security/authentication/accconfirm
ms.openlocfilehash: 3ca6d014245bb2a9bc4b1c90285f47eec7cefe84
ms.sourcegitcommit: 028ad28c546de706ace98066c76774de33e4ad20
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/08/2018
ms.locfileid: "39655477"
---
::: moniker range="<= aspnetcore-2.0"

Consulte [este archivo PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) para el núcleo de ASP.NET 1.1 y versión 2.1.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a>Confirmación de la cuenta y la recuperación de contraseñas en ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Joe Audette](https://twitter.com/joeaudette)

Este tutorial muestra cómo crear una aplicación de ASP.NET Core con el restablecimiento de confirmación y la contraseña de correo electrónico. Este tutorial es **no** un tema de principio. Debe estar familiarizado con:

* [ASP.NET Core](xref:tutorials/razor-pages/razor-pages-start)
* [Autenticación](xref:security/authentication/index)
* [Entity Framework Core](xref:data/ef-mvc/intro)

<!-- see C:/Dropbox/wrk/Code/SendGridConsole/Program.cs -->

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [](~/includes/2.1-SDK.md)]

## <a name="create-a-web--app-and-scaffold-identity"></a>Crear una aplicación web y aplicar la técnica scaffolding identidad

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

* En Visual Studio, cree un nuevo **aplicación Web** proyecto denominado **WebPWrecover**.
* Seleccione **ASP.NET Core 2.1**.
* Mantenga el valor predeterminado **autenticación** establecido en **sin autenticación**. La autenticación se agrega en el paso siguiente.

En el paso siguiente:

* Establece la página de diseño en *~/Pages/Shared/_Layout.cshtml*
* Seleccione *cuenta/Register*
* Cree un nuevo **clase de contexto de datos**

# <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli)

```console
dotnet new webapp -o WebPWrecover
cd WebPWrecover
dotnet tool install -g dotnet-aspnet-codegenerator
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -fi Account.Register -dc WebPWrecover.Models.WebPWrecoverContext
dotnet ef migrations add CreateIdentitySchema
dotnet ef database drop -f
dotnet ef database update
dotnet build
```

Ejecute `dotnet aspnet-codegenerator identity --help` para obtener ayuda acerca de la herramienta de scaffolding.

------

Siga las instrucciones de [Habilitar autenticación](xref:security/authentication/scaffold-identity#useauthentication):

* Agregar `app.UseAuthentication();` a `Startup.Configure`
* Agregar `<partial name="_LoginPartial" />` para el archivo de diseño.

## <a name="test-new-user-registration"></a>Nuevo registro de usuario de prueba

Ejecute la aplicación, seleccione la **registrar** vincular y registrar un usuario. En este momento, es la única validación en el correo electrónico con el [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) atributo. Después de enviar el registro, se registran en la aplicación. Más adelante en el tutorial, se actualiza el código para que los nuevos usuarios no pueden iniciar sesión hasta que se valida su correo electrónico.

[!INCLUDE[](~/includes/view-identity-db.md)]

Tenga en cuenta la tabla `EmailConfirmed` campo es `False`.

Es posible que desee volver a usar este correo electrónico en el paso siguiente cuando la aplicación envía un correo electrónico de confirmación. Haga doble clic en la fila y seleccione **eliminar**. Eliminar el alias de correo electrónico resulta más fácil en los pasos siguientes.

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a>Requerir confirmación por correo electrónico

Es una práctica recomendada para confirmar el correo electrónico de un nuevo registro de usuario. Enviar por correo electrónico de confirmación le ayuda a comprobar que no están suplantando persona (es decir, se hayan registrado con el correo electrónico de otra persona). Supongamos que tuviera un foro de discusión y desea evitar "yli@example.com"al registrar como"nolivetto@contoso.com". Sin confirmación por correo electrónico, "nolivetto@contoso.com" podría recibir el correo no deseado de la aplicación. Supongamos que el usuario registrado accidentalmente como "ylo@example.com" y no se vio el error ortográfico de "yli". No podrán usar la recuperación de contraseña porque la aplicación no tiene su correo electrónico correcto. Confirmación por correo electrónico proporciona una protección limitada de bots. Confirmación por correo electrónico no proporciona protección contra usuarios malintencionados con varias cuentas de correo electrónico.

Por lo general desea impedir que todos los datos del sitio web de registro antes de que tengan un correo electrónico confirmado nuevos usuarios.

Actualización *Areas/Identity/IdentityHostingStartup.cs* para requerir un correo electrónico de confirmación:

[!code-csharp[](accconfirm/sample/WebPWrecover21/Areas/Identity/IdentityHostingStartup.cs?name=snippet1&highlight=10-13)]

`config.SignIn.RequireConfirmedEmail = true;` impide que los usuarios registrados iniciar sesión hasta que se confirme su correo electrónico.

### <a name="configure-email-provider"></a>Configurar el proveedor de correo electrónico

En este tutorial, [SendGrid](https://sendgrid.com) se usa para enviar correo electrónico. Necesita una cuenta de SendGrid y una clave para enviar correo electrónico. Puede usar otros proveedores de correo electrónico. ASP.NET Core 2.x incluye `System.Net.Mail`, lo que permite enviar correo electrónico desde la aplicación. Se recomienda que usar SendGrid u otro servicio de correo electrónico para enviar correo electrónico. SMTP es difícil proteger y configurado correctamente.

Cree una clase para recuperar la clave de protección del correo electrónico. Para este ejemplo, crear *Services/AuthMessageSenderOptions.cs*:

[!code-csharp[](accconfirm/sample/WebPWrecover21/Services/AuthMessageSenderOptions.cs?name=snippet1)]

#### <a name="configure-sendgrid-user-secrets"></a>Configurar secretos de usuario de SendGrid

Agregar un único `<UserSecretsId>` valor para el `<PropertyGroup>` elemento del archivo del proyecto:

[!code-xml[](accconfirm/sample/WebPWrecover21/WebPWrecover.csproj?highlight=5)]

Establecer el `SendGridUser` y `SendGridKey` con el [herramienta secret manager](xref:security/app-secrets). Por ejemplo:

```console
C:/WebAppl>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

En Windows, Secret Manager almacena los pares de claves/valor en un *secrets.json* de archivos en el `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.

El contenido de la *secrets.json* archivo no se cifran. El *secrets.json* archivo se muestra a continuación (el `SendGridKey` se ha quitado el valor.)

 ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```
 
Para obtener más información, consulte el [patrón de opciones](xref:fundamentals/configuration/options) y [configuración](xref:fundamentals/configuration/index).

### <a name="install-sendgrid"></a>Instalar SendGrid

Este tutorial muestra cómo agregar notificaciones de correo electrónico a través de [SendGrid](https://sendgrid.com/), pero puede enviar correo electrónico mediante SMTP y otros mecanismos.

Instalar el `SendGrid` paquete NuGet:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

Desde la consola de administrador de paquetes, escriba el siguiente comando:

``` PMC
Install-Package SendGrid
```

# <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli)

En la consola, escriba el siguiente comando:

```cli
dotnet add package SendGrid
```

------

Consulte [comience gratis con SendGrid](https://sendgrid.com/free/) para registrar una cuenta gratuita de SendGrid.
### <a name="implement-iemailsender"></a>Implementar IEmailSender

Para implementar `IEmailSender`, crear *Services/EmailSender.cs* con código similar al siguiente:

[!code-csharp[](accconfirm/sample/WebPWrecover21/Services/EmailSender.cs)]

### <a name="configure-startup-to-support-email"></a>Configurar el inicio para admitir correo electrónico

Agregue el código siguiente a la `ConfigureServices` método en el *Startup.cs* archivo:

* Agregar `EmailSender` como un servicio de singleton.
* Registrar el `AuthMessageSenderOptions` instancia de configuración.

[!code-csharp[](accconfirm/sample/WebPWrecover21/Startup.cs?name=snippet2&highlight=12-99)]

## <a name="enable-account-confirmation-and-password-recovery"></a>Habilitar la recuperación de confirmación y la contraseña de cuenta

La plantilla tiene el código para la recuperación de confirmación y la contraseña de cuenta. Buscar el `OnPostAsync` método *Areas/Identity/Pages/Account/Register.cshtml.cs*.

Impedir que los usuarios recién registrados que se ha iniciado sesión automáticamente al marcar como comentario la línea siguiente:

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

El método completo se muestra con la línea cambiada resaltada:

[!code-csharp[](accconfirm/sample/WebPWrecover21/Areas/Identity/Pages/Account/Register.cshtml.cs?highlight=22&name=snippet_Register)]

## <a name="register-confirm-email-and-reset-password"></a>Registrar, confirmar correo electrónico y restablecimiento de contraseña

Ejecutar la aplicación web y probar el flujo de recuperación de contraseña y confirmación de la cuenta.

* Ejecute la aplicación y registrar un nuevo usuario

  ![Vista de cuenta registrar la aplicación Web](accconfirm/_static/loginaccconfirm1.png)

* Compruebe su correo electrónico para el vínculo de confirmación de la cuenta. Consulte [depurar correo electrónico](#debug) si no recibe el correo electrónico.
* Haga clic en el vínculo para confirmar su correo electrónico.
* Inicie sesión con su correo electrónico y contraseña.
* Cierre la sesión.

### <a name="view-the-manage-page"></a>Ver la página de administración

Seleccione el nombre de usuario en el explorador: ![ventana del explorador con el nombre de usuario](accconfirm/_static/un.png)

Es posible que deba expandir la barra de navegación para ver el nombre de usuario.

![barra de navegación](accconfirm/_static/x.png)

Se muestra la página de administración con el **perfil** pestaña seleccionada. El **correo electrónico** muestra una casilla de verificación que indica el correo electrónico se ha confirmado.

### <a name="test-password-reset"></a>Restablecimiento de contraseña de la prueba

* Si ha iniciado sesión, seleccione **Logout**.
* Seleccione el **iniciarla** vínculo y seleccione el **¿olvidó su contraseña?** vínculo.
* Escriba el correo electrónico usado para registrar la cuenta.
* Se envía un correo electrónico con un vínculo para restablecer su contraseña. Compruebe su correo electrónico y haga clic en el vínculo para restablecer la contraseña. Después de que se ha restablecido correctamente su contraseña, puede iniciar sesión con su correo electrónico y la nueva contraseña.

<a name="debug"></a>

### <a name="debug-email"></a>Depurar el correo electrónico

Si no se puede obtener el trabajo de correo electrónico:

* Establecer un punto de interrupción en `EmailSender.Execute` comprobar `SendGridClient.SendEmailAsync` se llama.
* Crear un [aplicación de consola para enviar correo electrónico](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) mediante código similar a `EmailSender.Execute`.
* Revise el [actividad de correo electrónico](https://sendgrid.com/docs/User_Guide/email_activity.html) página.
* Compruebe la carpeta de correo basura.
* Pruebe otro alias de correo electrónico en un proveedor de correo electrónico diferentes (Microsoft, Yahoo, Gmail, etcetera.)
* Pruebe a enviar a las cuentas de correo electrónico diferente.

**Una práctica recomendada de seguridad** es **no** use secretos de producción en desarrollo y pruebas. Si publica la aplicación en Azure, puede establecer los secretos de SendGrid como configuración de la aplicación en el portal de Azure Web App. El sistema de configuración está configurado para leer las claves de las variables de entorno.

## <a name="combine-social-and-local-login-accounts"></a>Combinar las cuentas de inicio de sesión social y local

Para completar esta sección, primero debe habilitar a un proveedor de autenticación externo. Consulte [Facebook, Google y la autenticación de proveedor externo](xref:security/authentication/social/index).

Puede combinar las cuentas locales y redes sociales, haga clic en el vínculo de correo electrónico. En la siguiente secuencia, "RickAndMSFT@gmail.com" en primer lugar se crea como un inicio de sesión local; sin embargo, puede crear la cuenta como un inicio de sesión social en primer lugar y luego agregar un inicio de sesión local.

![Aplicación Web: RickAndMSFT@gmail.com usuario autenticado](accconfirm/_static/rick.png)

Haga clic en el **administrar** vínculo. Tenga en cuenta la externa 0 (inicios de sesión sociales) asociado con esta cuenta.

![Administrar la vista](accconfirm/_static/manage.png)

Haga clic en el vínculo a otro servicio de inicio de sesión y acepte las solicitudes de aplicación. En la siguiente imagen, Facebook es el proveedor de autenticación externos:

![Administrar la vista de inicios de sesión externos listado de Facebook](accconfirm/_static/fb.png)

Se han combinado las dos cuentas. Es posible iniciar sesión con las cuentas. Es posible que desee que los usuarios agreguen cuentas locales en caso de su servicio de autenticación de inicio de sesión social esté inactivo o que es más probable hayan perdido el acceso a su cuenta de redes sociales.

## <a name="enable-account-confirmation-after-a-site-has-users"></a>Habilitar la confirmación de cuenta después de un sitio tiene usuarios

Habilitar confirmación de la cuenta en un sitio con usuarios bloquea todos los usuarios existentes. Los usuarios existentes están bloqueados porque sus cuentas no confirmadas. Para evitar el bloqueo de usuario existente, use uno de los métodos siguientes:

* Actualizar la base de datos para marcar todos los usuarios existentes, como se ha confirmado.
* Confirme que los usuarios existentes. Por ejemplo, envío por lotes de mensajes de correo electrónico con vínculos de confirmación.

::: moniker-end
