---
title: Confirmación de la cuenta y la recuperación de contraseña en ASP.NET Core
author: rick-anderson
description: Obtener información sobre cómo compilar una aplicación de ASP.NET Core con el restablecimiento de confirmación y la contraseña de correo electrónico.
manager: wpickett
ms.author: riande
ms.date: 2/11/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/accconfirm
ms.openlocfilehash: e0bca48fcaa9a29847fdda714698ed8562d30707
ms.sourcegitcommit: 9bc34b8269d2a150b844c3b8646dcb30278a95ea
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/12/2018
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a>Confirmación de la cuenta y la recuperación de contraseña en ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Joe Audette](https://twitter.com/joeaudette)

Este tutorial muestra cómo compilar una aplicación de ASP.NET Core con el restablecimiento de confirmación y la contraseña de correo electrónico. Este tutorial es **no** un tema de principio. Debe estar familiarizado con:

* [ASP.NET Core](xref:tutorials/first-mvc-app/start-mvc)
* [Autenticación](xref:security/authentication/index)
* [Confirmación de cuentas y recuperación de contraseñas](xref:security/authentication/accconfirm)
* [Entity Framework Core](xref:data/ef-mvc/intro)

Vea [este archivo PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) para las versiones 1.1 de MVC de ASP.NET Core y 2.x.

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [](~/includes/net-core-prereqs.md)]

## <a name="create-a-new-aspnet-core-project-with-the-net-core-cli"></a>Cree un nuevo proyecto de ASP.NET Core con la CLI de núcleo de .NET

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```console
dotnet new razor --auth Individual -o WebPWrecover
cd WebPWrecover
```

* `--auth Individual` Especifica la plantilla de proyecto de cuentas de usuario individuales.
* En Windows, agregue el `-uld` opción. Especifica que LocalDB debe usarse en lugar de SQLite.
* Ejecute `new mvc --help` para obtener ayuda sobre este comando.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Si usa la CLI o SQLite, ejecute lo siguiente en una ventana de comandos:

```console
dotnet new mvc --auth Individual
```

* `--auth Individual` Especifica la plantilla de proyecto de cuentas de usuario individuales.
* En Windows, agregue el `-uld` opción. Especifica que LocalDB debe usarse en lugar de SQLite.
* Ejecute `new mvc --help` para obtener ayuda sobre este comando.

---

Como alternativa, puede crear un nuevo proyecto de ASP.NET Core con Visual Studio:

* En Visual Studio, cree un nuevo **aplicación Web** proyecto.
* Seleccione **principales de ASP.NET 2.0**. **.NET core** está seleccionado en la siguiente imagen, pero puede seleccionar **.NET Framework**.
* Seleccione **Cambiar autenticación** y establezca en **cuentas de usuario individuales**.
* Mantenga el valor predeterminado **en la aplicación de cuentas de usuario de almacén**.

![Cuadro de diálogo proyecto nuevo que muestra "Radio de cuentas de usuario individuales" seleccionado](accconfirm/_static/2.png)

## <a name="test-new-user-registration"></a>Probar el nuevo registro de usuario

Ejecutar la aplicación, seleccione la **registrar** vincular y registrar un usuario. Siga las instrucciones para ejecutar migraciones de Entity Framework Core. En este punto, es la única validación en el correo electrónico con el [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) atributo. Después de enviar el registro, se registran en la aplicación. Más adelante en el tutorial, el código se actualiza para que los usuarios nuevos no se pueden iniciar sesión hasta que se ha validado el correo electrónico.

## <a name="view-the-identity-database"></a>Vista de la base de datos de identidad

Vea [trabajar con código en un proyecto de MVC de ASP.NET Core](xref:tutorials/first-mvc-app-xplat/working-with-sql) para obtener instrucciones sobre cómo ver la base de datos de SQLite.

Para Visual Studio:

* Desde el **vista** menú, seleccione **Explorador de objetos de SQL Server** (SSOX).
* Vaya a **(localdb) MSSQLLocalDB (SQL Server 13)**. Haga doble clic en **dbo. AspNetUsers** > **ver datos**:

![Menú contextual en la tabla de AspNetUsers en el Explorador de objetos de SQL Server](accconfirm/_static/ssox.png)

Tenga en cuenta la tabla `EmailConfirmed` campo es `False`.

Puede volver a usar este correo electrónico en el paso siguiente cuando la aplicación envía un correo electrónico de confirmación. Haga doble clic en la fila y seleccione **eliminar**. Eliminar el alias de correo electrónico facilita en los pasos siguientes.

---

## <a name="require-https"></a>Requerir HTTPS

Vea [requieren HTTPS](xref:security/enforcing-ssl).

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a>Requerir confirmación por correo electrónico

Es una práctica recomendada para confirmar el correo electrónico de un nuevo registro de usuario. Enviar por correo electrónico de confirmación le ayuda a comprobar que no están suplantando otra persona (es decir, no ha registrado con el correo electrónico de otra persona). Imagine que tuviera un foro de discusión, y desea evitar "yli@example.com"al registrar como"nolivetto@contoso.com." Sin confirmación por correo electrónico, "nolivetto@contoso.com" podría recibir correos electrónicos no deseados de la aplicación. Suponga que el usuario registrado accidentalmente como "ylo@example.com" y no se vio el error ortográfico de "yli". No podrían usar recuperación de contraseña porque la aplicación no tiene su correo electrónico correcto. Confirmación por correo electrónico proporciona sólo una protección limitada de robots. Confirmación por correo electrónico no proporciona protección de los usuarios malintencionados con varias cuentas de correo electrónico.

En general conveniente evitar que los nuevos usuarios se registren todos los datos del sitio web antes de que tengan un correo electrónico confirmado.

Actualización `ConfigureServices` para requerir un correo electrónico confirmadas:

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet1&highlight=12-17)]

`config.SignIn.RequireConfirmedEmail = true;` impide que los usuarios registrados el registro hasta que se ha confirmado el correo electrónico.

### <a name="configure-email-provider"></a>Configurar el proveedor de correo electrónico

En este tutorial, se usa SendGrid para enviar correo electrónico. Necesita una cuenta de SendGrid y la clave para enviar correo electrónico. Puede usar otros proveedores de correo electrónico. ASP.NET Core 2.x incluye `System.Net.Mail`, que le permite enviar correo electrónico desde la aplicación. Se recomienda que usar SendGrid u otro servicio de correo electrónico para enviar correo electrónico. SMTP es difícil proteger y configurado correctamente.

El [patrón opciones](xref:fundamentals/configuration/options) se usa para acceder a la configuración de cuenta y clave de usuario. Para obtener más información, consulte [configuración](xref:fundamentals/configuration/index).

Cree una clase para obtener la clave de proteger el correo electrónico. En este ejemplo, el `AuthMessageSenderOptions` se crea una clase en el *Services/AuthMessageSenderOptions.cs* archivo:

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/AuthMessageSenderOptions.cs?name=snippet1)]

Establecer el `SendGridUser` y `SendGridKey` con el [herramienta Administrador de secreto](xref:security/app-secrets). Por ejemplo:

```console
C:\WebAppl\src\WebApp1>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

En Windows, el Administrador de secreto almacena pares de claves/valor en un *secrets.json* un archivo en el `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.

El contenido de la *secrets.json* archivos no están cifrados. El *secrets.json* archivo se muestra a continuación (el `SendGridKey` se ha quitado el valor.)

 ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="configure-startup-to-use-authmessagesenderoptions"></a>Configurar el inicio para usar AuthMessageSenderOptions

Agregar `AuthMessageSenderOptions` al contenedor de servicios al final de la `ConfigureServices` método en el *Startup.cs* archivo:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet2&highlight=28)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=26)]

---

### <a name="configure-the-authmessagesender-class"></a>Configurar la clase de AuthMessageSender

Este tutorial muestra cómo agregar notificaciones de correo electrónico a través de [SendGrid](https://sendgrid.com/), pero puede enviar correo electrónico mediante SMTP y otros mecanismos.

Instalar el `SendGrid` paquete NuGet:

* Desde la línea de comandos:

    `dotnet add package SendGrid`

* Desde la consola de administrador de paquetes, escriba el siguiente comando:

  `Install-Package SendGrid`

Vea [empiece de forma gratuita con SendGrid](https://sendgrid.com/free/) para registrar una cuenta gratuita de SendGrid.

#### <a name="configure-sendgrid"></a>Configurar SendGrid

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

Para configurar SendGrid, agregue código similar al siguiente en *Services/EmailSender.cs*:

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/EmailSender.cs)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

* Agregue código en *Services/MessageServices.cs* similar al siguiente para configurar SendGrid:

[!code-csharp[](accconfirm/sample/WebApp1/Services/MessageServices.cs)]

---

## <a name="enable-account-confirmation-and-password-recovery"></a>Habilitar la recuperación de confirmación y la contraseña de cuenta

La plantilla tiene el código para la recuperación de confirmación y la contraseña de cuenta. Buscar el `OnPostAsync` método *Pages/Account/Register.cshtml.cs*.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

Impedir que los usuarios recién registrados que se iniciará automáticamente la sesión como comentario la línea siguiente:

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

El método completo se muestra con la línea cambiada resaltada:

[!code-csharp[](accconfirm/sample/WebPWrecover/Pages/Account/Register.cshtml.cs?highlight=16&name=snippet_Register)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Para habilitar la confirmación de la cuenta, quite el código siguiente:

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=16-25&name=snippet_Register)]

**Nota:** el código impide que un usuario recién registrado que se iniciará automáticamente la sesión como comentario la línea siguiente:

```csharp
//await _signInManager.SignInAsync(user, isPersistent: false);
```

Habilitar la recuperación de contraseña por el comentario de código en el `ForgotPassword` acción de *Controllers/AccountController.cs*:

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=17-23&name=snippet_ForgotPassword)]

Quite el elemento de formulario en *Views/Account/ForgotPassword.cshtml*. Desea quitar el `<p> For more information on how to enable reset password ... </p>` elemento, que contiene un vínculo a este artículo.

[!code-cshtml[](accconfirm/sample/WebApp1/Views/Account/ForgotPassword.cshtml?highlight=7-10,12,28)]

---

## <a name="register-confirm-email-and-reset-password"></a>Registrar, confirma el correo electrónico y restablecer contraseña

Ejecutar la aplicación web y probar la confirmación de la cuenta y el flujo de recuperación de contraseña.

* Ejecutar la aplicación y registrar un nuevo usuario

  ![Vista de cuenta registrar la aplicación Web](accconfirm/_static/loginaccconfirm1.png)

* Compruebe su correo electrónico para el vínculo de confirmación de cuenta. Vea [depurar correo electrónico](#debug) si no recibe el correo electrónico.
* Haga clic en el vínculo para confirmar tu correo electrónico.
* Inicie sesión con su correo electrónico y contraseña.
* Cierre la sesión.

### <a name="view-the-manage-page"></a>Ver la página de administración

Seleccione el nombre de usuario en el explorador: ![ventana del explorador con el nombre de usuario](accconfirm/_static/un.png)

Debe expandir la barra de navegación para ver el nombre de usuario.

![barra de navegación](accconfirm/_static/x.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Se muestra la página de administración con el **perfil** pestaña seleccionada. El **correo electrónico** muestra una casilla de verificación que indica el correo electrónico se ha confirmado.

![página de administración](accconfirm/_static/rick2.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Esto se menciona más adelante en el tutorial.
![página de administración](accconfirm/_static/rick2.png)

---

### <a name="test-password-reset"></a>Restablecimiento de contraseña de prueba

* Si ha iniciado sesión, seleccione **Logout**.
* Seleccione el **sesión** de vínculo y seleccione el **¿olvidó su contraseña?** vínculo.
* Escriba el correo electrónico que usa para registrar la cuenta.
* Se envía un correo electrónico con un vínculo para restablecer su contraseña. Compruebe su correo electrónico y haga clic en el vínculo para restablecer la contraseña. Después de que la contraseña se restableció correctamente, puede iniciar sesión con su correo electrónico y la contraseña nueva.

<a name="debug"></a>

### <a name="debug-email"></a>Depurar el correo electrónico

Si no se puede obtener el trabajo de correo electrónico:

* Crear un [aplicación de consola para enviar correo electrónico](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).
* Revise el [actividad de correo electrónico](https://sendgrid.com/docs/User_Guide/email_activity.html) página.
* Compruebe la carpeta de correo basura.
* Pruebe otro alias de correo electrónico en un proveedor de correo electrónico diferente (Microsoft, Yahoo, Gmail, etcetera.)
* Vuelva a enviar a las cuentas de correo electrónico diferente.

**Una práctica recomendada de seguridad** es **no** utilice secretos de producción en pruebas y desarrollo. Si publica la aplicación en Azure, puede establecer los secretos de SendGrid como configuración de la aplicación en el portal de la aplicación Web de Azure. El sistema de configuración está configurado para leer las claves de las variables de entorno.

## <a name="combine-social-and-local-login-accounts"></a>Combinar las cuentas de inicio de sesión locales y redes sociales

Para completar esta sección, primero debe habilitar a un proveedor de autenticación externo. Vea [Facebook, Google y la autenticación de proveedor externo](xref:security/authentication/social/index).

Puede combinar cuentas locales y redes sociales, haga clic en el vínculo de correo electrónico. En la siguiente secuencia, "RickAndMSFT@gmail.com" en primer lugar se crea como un inicio de sesión local; sin embargo, puede crear la cuenta como un inicio de sesión social primero y luego agregar un inicio de sesión local.

![Aplicación Web: RickAndMSFT@gmail.com usuario autenticado](accconfirm/_static/rick.png)

Haga clic en el **administrar** vínculo. Tenga en cuenta el externo 0 (inicios de sesión sociales) asociados con esta cuenta.

![Administrar la vista](accconfirm/_static/manage.png)

Haga clic en el vínculo a otro servicio de inicio de sesión y Aceptar las solicitudes de aplicación. En la siguiente imagen, Facebook es el proveedor de autenticación externos:

![Administrar la vista de inicios de sesión externos enumerar Facebook](accconfirm/_static/fb.png)

Se han combinado las dos cuentas. Es posible iniciar sesión con cualquiera de estas cuentas. Puede que los usuarios agreguen cuentas locales en caso de que su servicio de autenticación de inicio de sesión social está inactivo o, más probablemente ha perdido acceso a su cuenta sociales.

## <a name="enable-account-confirmation-after-a-site-has-users"></a>Habilitar la confirmación de la cuenta después de un sitio tiene usuarios

Habilitar confirmación de la cuenta en un sitio con usuarios bloquea todos los usuarios existentes. No tienen acceso a los usuarios existentes porque no se ha confirmado sus cuentas. Para evitar salir del bloqueo de usuario, use uno de los métodos siguientes:

* Actualizar la base de datos para marcar todos los usuarios existentes como está confirmado
* Confirme que los usuarios existentes. Por ejemplo, lote enviar correos electrónicos con vínculos de confirmación.
