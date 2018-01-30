---
title: "Confirmación de la cuenta y la recuperación de contraseña en el núcleo de ASP.NET"
author: rick-anderson
description: "Muestra cómo compilar una aplicación de ASP.NET Core con el restablecimiento de confirmación y la contraseña de correo electrónico."
manager: wpickett
ms.author: riande
ms.date: 12/1/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/accconfirm
ms.openlocfilehash: 459f1793b1f1f73792bb6537856cb739774c6261
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a>Confirmación de la cuenta y la recuperación de contraseña en ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Joe Audette](https://twitter.com/joeaudette) 

Este tutorial muestra cómo compilar una aplicación de ASP.NET Core con el restablecimiento de confirmación y la contraseña de correo electrónico.

## <a name="create-a-new-aspnet-core-project"></a>Crear un proyecto de ASP.NET Core

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Este paso se aplica a Visual Studio en Windows. Vea la sección siguiente para obtener instrucciones de CLI.

El tutorial requiere Visual Studio 2017 Preview 2 o posterior.

* En Visual Studio, cree un nuevo proyecto de aplicación Web.
* Seleccione **principales de ASP.NET 2.0**. La siguiente imagen muestra **.NET Core** seleccionada, pero puede seleccionar **.NET Framework**.
* Seleccione **Cambiar autenticación** y establezca en **cuentas de usuario individuales**.
* Mantenga el valor predeterminado **en la aplicación de cuentas de usuario de almacén**.

![Cuadro de diálogo proyecto nuevo que muestra "Radio de cuentas de usuario individuales" seleccionado](accconfirm/_static/2.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

El tutorial requiere Visual Studio 2017 o una versión posterior.

* En Visual Studio, cree un nuevo proyecto de aplicación Web.
* Seleccione **Cambiar autenticación** y establezca en **cuentas de usuario individuales**.

![Cuadro de diálogo proyecto nuevo que muestra "Radio de cuentas de usuario individuales" seleccionado](accconfirm/_static/indiv.png)

---

### <a name="net-core-cli-project-creation-for-macos-and-linux"></a>Creación del proyecto de CLI de núcleo de .NET para macOS y Linux

Si usa la CLI o SQLite, ejecute lo siguiente en una ventana de comandos:

```console
dotnet new mvc --auth Individual
```

* `--auth Individual`Especifica la plantilla de cuentas de usuario individuales.
* En Windows, agregue el `-uld` opción. El `-uld` opción crea una cadena de conexión de LocalDB en lugar de una base de datos de SQLite.
* Ejecute `new mvc --help` para obtener ayuda sobre este comando.

## <a name="test-new-user-registration"></a>Probar el nuevo registro de usuario

Ejecutar la aplicación, seleccione la **registrar** vincular y registrar un usuario. Siga las instrucciones para ejecutar migraciones de Entity Framework Core. En este punto, es la única validación en el correo electrónico con el [[EmailAddress]](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) atributo. Una vez enviado el registro, se registran en la aplicación. Más adelante en el tutorial, esto cambiaremos para que nuevos usuarios no pueden iniciar sesión hasta que se ha validado el correo electrónico.

## <a name="view-the-identity-database"></a>Vista de la base de datos de identidad

# <a name="sql-servertabsql-server"></a>[SQL Server](#tab/sql-server)

* Desde el **vista** menú, seleccione **Explorador de objetos de SQL Server** (SSOX). 
* Vaya a **(localdb) MSSQLLocalDB (SQL Server 13)**. Haga doble clic en **dbo. AspNetUsers** > **ver datos**:

![Menú contextual en la tabla de AspNetUsers en el Explorador de objetos de SQL Server](accconfirm/_static/ssox.png)

Tenga en cuenta el `EmailConfirmed` campo es `False`.

Puede volver a usar este correo electrónico en el paso siguiente cuando la aplicación envía un correo electrónico de confirmación. Haga doble clic en la fila y seleccione **eliminar**. Eliminar el correo electrónico alias ahora le resultará más fácil en los pasos siguientes.

# <a name="sqlitetabsqlite"></a>[SQLite](#tab/sqlite)

Vea [trabajar con código en un proyecto de MVC de ASP.NET Core](xref:tutorials/first-mvc-app-xplat/working-with-sql) para obtener instrucciones sobre cómo ver la base de datos de SQLite. 

---

## <a name="require-ssl-and-setup-iis-express-for-ssl"></a>Requerir SSL y la instalación de IIS Express para SSL

Vea [exigir SSL](xref:security/enforcing-ssl).

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a>Requerir confirmación por correo electrónico

Es una práctica recomendada para confirmar el correo electrónico de un nuevo registro de usuario para comprobar que no están suplantando otra persona (es decir, no ha registrado con el correo electrónico de otra persona). Imagine que tuviera un foro de discusión, y desea evitar "yli@example.com"al registrar como"nolivetto@contoso.com." Sin confirmación por correo electrónico, "nolivetto@contoso.com" pudo obtener el correo electrónico no deseado de la aplicación. Suponga que el usuario registrado accidentalmente como "ylo@example.com" y no se vio el error ortográfico de "yli", no podrán usar la recuperación de contraseñas porque la aplicación no tiene su correo electrónico correcto. Confirmación por correo electrónico proporciona sólo una protección limitada de robots y no proporciona protección de correo basura determinado que tienen muchos alias de correo electrónico de trabajo que puede usar para registrar.

En general conveniente evitar que los nuevos usuarios se registren todos los datos del sitio web antes de que tengan un correo electrónico confirmado. 

Actualización `ConfigureServices` para requerir un correo electrónico confirmadas:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](accconfirm/sample/WebPW/Startup.cs?name=snippet1&highlight=6-9)]


# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=13-16)]

---

 
```csharp
config.SignIn.RequireConfirmedEmail = true;
```
La línea anterior impide que los usuarios registrados que se registran en hasta que se ha confirmado el correo electrónico. Sin embargo, esa línea no impide que los usuarios nuevos que se registran en después de que se registren. El código predeterminado se registra en un usuario después de que se registren. Una vez que cierra la sesión, no podrá volver a iniciar sesión hasta que se registran. Más adelante en el tutorial, cambiaremos el usuario de código por lo que recién registrado son **no** iniciado sesión.

### <a name="configure-email-provider"></a>Configurar el proveedor de correo electrónico

En este tutorial, se usa SendGrid para enviar correo electrónico. Necesita una cuenta de SendGrid y la clave para enviar correo electrónico. Puede usar otros proveedores de correo electrónico. ASP.NET Core 2.x incluye `System.Net.Mail`, que le permite enviar correo electrónico desde la aplicación. Se recomienda que usar SendGrid u otro servicio de correo electrónico para enviar correo electrónico.

El [patrón opciones](xref:fundamentals/configuration/options) se usa para acceder a la configuración de cuenta y clave de usuario. Para obtener más información, consulte [configuración](xref:fundamentals/configuration/index).

Cree una clase para obtener la clave de proteger el correo electrónico. En este ejemplo, el `AuthMessageSenderOptions` se crea una clase en el *Services/AuthMessageSenderOptions.cs* archivo.

[!code-csharp[Main](accconfirm/sample/WebApp1/Services/AuthMessageSenderOptions.cs?name=snippet1)]

Establecer el `SendGridUser` y `SendGridKey` con el [herramienta Administrador de secreto](../app-secrets.md). Por ejemplo:

```none
C:\WebAppl\src\WebApp1>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

En Windows, el Administrador de secreto almacena los pares de claves/valor en un *secrets.json* archivo en el directorio %APPDATA%/Microsoft/UserSecrets/ < WebAppName userSecretsId >.

El contenido de la *secrets.json* archivos no están cifrados. El *secrets.json* archivo se muestra a continuación (el `SendGridKey` se ha quitado el valor.)

  ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="configure-startup-to-use-authmessagesenderoptions"></a>Configurar el inicio para usar AuthMessageSenderOptions

Agregar `AuthMessageSenderOptions` al contenedor de servicios al final de la `ConfigureServices` método en el *Startup.cs* archivo:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](accconfirm/sample/WebPW/Startup.cs?name=snippet1&highlight=18)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)
[!code-csharp[Main](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=26)]

---

### <a name="configure-the-authmessagesender-class"></a>Configurar la clase de AuthMessageSender

Este tutorial muestra cómo agregar notificaciones de correo electrónico a través de [SendGrid](https://sendgrid.com/), pero puede enviar correo electrónico mediante SMTP y otros mecanismos.

* Instalar el `SendGrid` paquete NuGet. Introduzca la siguiente información desde la consola de administrador de paquetes, el siguiente comando:

  `Install-Package SendGrid`

* Vea [empiece de forma gratuita con SendGrid](https://sendgrid.com/free/) para registrar una cuenta gratuita de SendGrid.

#### <a name="configure-sendgrid"></a>Configurar SendGrid

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

* Agregue código en *Services/EmailSender.cs* similar al siguiente para configurar SendGrid:

[!code-csharp[Main](accconfirm/sample/WebPW/Services/EmailSender.cs)]


# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)
* Agregue código en *Services/MessageServices.cs* similar al siguiente para configurar SendGrid:

[!code-csharp[Main](accconfirm/sample/WebApp1/Services/MessageServices.cs)]

---

## <a name="enable-account-confirmation-and-password-recovery"></a>Habilitar la recuperación de confirmación y la contraseña de cuenta

La plantilla tiene el código para la recuperación de confirmación y la contraseña de cuenta. Buscar el `[HttpPost] Register` método en el *AccountController.cs* archivo.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Impedir que los usuarios recién registrados que se iniciará automáticamente la sesión como comentario la línea siguiente:

```csharp 
await _signInManager.SignInAsync(user, isPersistent: false);
```

El método completo se muestra con la línea cambiada resaltada:

[!code-csharp[Main](accconfirm/sample/WebPW/Controllers/AccountController.cs?highlight=19&name=snippet_Register)]

Nota: El código anterior generará un error si implementa `IEmailSender` y enviar un correo electrónico de texto sin formato. Vea [este problema](https://github.com/aspnet/Home/issues/2152) para obtener más información y una solución alternativa.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Quite los comentarios del código para habilitar la confirmación de la cuenta.

[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=16-25&name=snippet_Register)]

Nota: Se estamos también impide que un usuario recién registrados que se iniciará automáticamente la sesión como comentario la línea siguiente:

```csharp 
//await _signInManager.SignInAsync(user, isPersistent: false);
```

Habilitar la recuperación de contraseña por el comentario de código en el `ForgotPassword` acción en el *Controllers/AccountController.cs* archivo.

[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=17-23&name=snippet_ForgotPassword)]

Quite el elemento de formulario en *Views/Account/ForgotPassword.cshtml*. Desea quitar el `<p> For more information on how to enable reset password ... </p>` elemento que contiene un vínculo a este artículo.

[!code-html[Main](accconfirm/sample/WebApp1/Views/Account/ForgotPassword.cshtml?highlight=7-10,12,28)]

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

![navbar](accconfirm/_static/x.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Se muestra la página de administración con el **perfil** pestaña seleccionada. El **correo electrónico** muestra una casilla de verificación que indica el correo electrónico se ha confirmado. 

![página de administración](accconfirm/_static/rick2.png)


# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Hablaremos acerca de esta página más adelante en el tutorial.
![página de administración](accconfirm/_static/rick2.png)

---

### <a name="test-password-reset"></a>Restablecimiento de contraseña de prueba

* Si ha iniciado sesión, seleccione **Logout**.  
* Seleccione el **sesión** de vínculo y seleccione el **¿olvidó su contraseña?** vínculo.
* Escriba el correo electrónico que usa para registrar la cuenta.
* Se enviará un correo electrónico con un vínculo para restablecer su contraseña. Compruebe su correo electrónico y haga clic en el vínculo para restablecer la contraseña.  Después de que la contraseña se restableció correctamente, puede iniciar sesión con su correo electrónico y una contraseña nueva.

<a name="debug"></a>

### <a name="debug-email"></a>Depurar el correo electrónico

Si no se puede obtener el trabajo de correo electrónico:

* Revise el [actividad de correo electrónico](https://sendgrid.com/docs/User_Guide/email_activity.html) página.
* Compruebe la carpeta de correo basura.
* Pruebe otro alias de correo electrónico en un proveedor de correo electrónico diferente (Microsoft, Yahoo, Gmail, etcetera.)
* Crear un [aplicación de consola para enviar correo electrónico](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).
* Vuelva a enviar a las cuentas de correo electrónico diferente.

**Nota:** un procedimiento recomendado es no utilizar los secretos de producción de prueba y desarrollo. Si publica la aplicación en Azure, puede establecer los secretos de SendGrid como configuración de la aplicación en el portal de la aplicación Web de Azure. El sistema de configuración es el programa de instalación para leer las claves de las variables de entorno.

## <a name="prevent-login-at-registration"></a>Evitar el inicio de sesión en el registro

Con las plantillas de la actuales, cuando un usuario se haya completado el formulario de registro, que han iniciado sesión (autenticado). Por lo general desea confirmar su correo electrónico antes de registrarlos. En la sección siguiente, se modificará el código para requerir los nuevos usuarios tienen un correo electrónico confirmado antes de que han iniciado sesión. Actualización de la `[HttpPost] Login` acción en el *AccountController.cs* archivo con los siguientes cambios resaltados.

[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=11-21&name=snippet_Login)]

**Nota:** un procedimiento recomendado es no utilizar los secretos de producción de prueba y desarrollo. Si publica la aplicación en Azure, puede establecer los secretos de SendGrid como configuración de la aplicación en el portal de la aplicación Web de Azure. El sistema de configuración es el programa de instalación para leer las claves de las variables de entorno.


## <a name="combine-social-and-local-login-accounts"></a>Combinar las cuentas de inicio de sesión locales y redes sociales

Nota: Esta sección solo se aplica a ASP.NET Core 1.x. Para ASP.NET Core 2.x, consulte [esto](https://github.com/aspnet/Docs/issues/3753) problema.

Para completar esta sección, primero debe habilitar a un proveedor de autenticación externo. Vea [habilitar la autenticación con Facebook, Google y otros proveedores externos](social/index.md).

Puede combinar cuentas locales y redes sociales, haga clic en el vínculo de correo electrónico. En la siguiente secuencia, "RickAndMSFT@gmail.com" en primer lugar se crea como un inicio de sesión local; sin embargo, puede crear la cuenta como un inicio de sesión social primero y luego agregar un inicio de sesión local.

![Aplicación Web: RickAndMSFT@gmail.com usuario autenticado](accconfirm/_static/rick.png)

Haga clic en el **administrar** vínculo. Tenga en cuenta el externo 0 (inicios de sesión sociales) asociados con esta cuenta.

![Administrar la vista](accconfirm/_static/manage.png)

Haga clic en el vínculo a otro servicio de inicio de sesión y Aceptar las solicitudes de aplicación. En la imagen siguiente, Facebook es el proveedor de autenticación externos:

![Administrar la vista de inicios de sesión externos enumerar Facebook](accconfirm/_static/fb.png)

Se han combinado las dos cuentas. Podrá iniciar sesión con cualquiera de estas cuentas. Puede que los usuarios agreguen cuentas locales en caso de que su registro sociales en servicio de autenticación está inactivo o, más probablemente ha perdido acceso a su cuenta sociales.
