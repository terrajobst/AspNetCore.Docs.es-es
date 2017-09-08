---
title: "Confirmación de la cuenta y la recuperación de contraseña en el núcleo de ASP.NET"
author: rick-anderson
description: "Muestra cómo compilar una aplicación de ASP.NET Core con el restablecimiento de confirmación y la contraseña de correo electrónico."
keywords: "Núcleo de ASP.NET, restablecimiento de contraseña, confirmación por correo electrónico, seguridad"
ms.author: riande
manager: wpickett
ms.date: 07/19/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/accconfirm
ms.openlocfilehash: aaed75c78a99e59954add959a76a2fd68ea5f3fc
ms.sourcegitcommit: f2fb0b45284e4f8c4a9c422bec790aede7c1f0ac
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/17/2017
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a><span data-ttu-id="18318-104">Confirmación de la cuenta y la recuperación de contraseña en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="18318-104">Account confirmation and password recovery in ASP.NET Core</span></span>

<span data-ttu-id="18318-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="18318-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="18318-106">Este tutorial muestra cómo compilar una aplicación de ASP.NET Core con el restablecimiento de confirmación y la contraseña de correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="18318-106">This tutorial shows you how to build an ASP.NET Core app with email confirmation and password reset.</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="18318-107">Cree un nuevo proyecto de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="18318-107">Create a New ASP.NET Core Project</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="18318-108">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="18318-108">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="18318-109">Este paso se aplica a Visual Studio en Windows.</span><span class="sxs-lookup"><span data-stu-id="18318-109">This step applies to Visual Studio on Windows.</span></span> <span data-ttu-id="18318-110">Vea la sección siguiente para obtener instrucciones de CLI.</span><span class="sxs-lookup"><span data-stu-id="18318-110">See the next section for CLI instructions.</span></span>

<span data-ttu-id="18318-111">El tutorial requiere Visual Studio 2017 Preview 2 o posterior.</span><span class="sxs-lookup"><span data-stu-id="18318-111">The tutorial requires Visual Studio 2017 Preview 2 or later.</span></span>

* <span data-ttu-id="18318-112">En Visual Studio, cree un nuevo proyecto de aplicación Web.</span><span class="sxs-lookup"><span data-stu-id="18318-112">In Visual Studio, create a New Web Application Project.</span></span>
* <span data-ttu-id="18318-113">Seleccione **principales de ASP.NET 2.0**.</span><span class="sxs-lookup"><span data-stu-id="18318-113">Select **ASP.NET Core 2.0**.</span></span> <span data-ttu-id="18318-114">La siguiente imagen muestra **.NET Core** seleccionada, pero puede seleccionar **.NET Framework**.</span><span class="sxs-lookup"><span data-stu-id="18318-114">The following image show **.NET Core** selected, but you can select **.NET Framework**.</span></span>
* <span data-ttu-id="18318-115">Seleccione **Cambiar autenticación** y establezca en **cuentas de usuario individuales**.</span><span class="sxs-lookup"><span data-stu-id="18318-115">Select **Change Authentication** and set to **Individual User Accounts**.</span></span>
* <span data-ttu-id="18318-116">Mantenga el valor predeterminado **en la aplicación de cuentas de usuario de almacén**.</span><span class="sxs-lookup"><span data-stu-id="18318-116">Keep the default **Store user accounts in-app**.</span></span>

![Cuadro de diálogo proyecto nuevo que muestra "Radio de cuentas de usuario individuales" seleccionado](accconfirm/_static/2.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="18318-118">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="18318-118">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="18318-119">El tutorial requiere Visual Studio 2017 o una versión posterior.</span><span class="sxs-lookup"><span data-stu-id="18318-119">The tutorial requires Visual Studio 2017 or later.</span></span>

* <span data-ttu-id="18318-120">En Visual Studio, cree un nuevo proyecto de aplicación Web.</span><span class="sxs-lookup"><span data-stu-id="18318-120">In Visual Studio, create a New Web Application Project.</span></span>
* <span data-ttu-id="18318-121">Seleccione **Cambiar autenticación** y establezca en **cuentas de usuario individuales**.</span><span class="sxs-lookup"><span data-stu-id="18318-121">Select **Change Authentication** and set to **Individual User Accounts**.</span></span>

![Cuadro de diálogo proyecto nuevo que muestra "Radio de cuentas de usuario individuales" seleccionado](accconfirm/_static/indiv.png)

---

### <a name="net-core-cli-project-creation-for-macos-and-linux"></a><span data-ttu-id="18318-123">Creación del proyecto de CLI de núcleo de .NET para macOS y Linux</span><span class="sxs-lookup"><span data-stu-id="18318-123">.NET Core CLI project creation for macOS and Linux</span></span>

<span data-ttu-id="18318-124">Si usa la CLI o SQLite, ejecute lo siguiente en una ventana de comandos:</span><span class="sxs-lookup"><span data-stu-id="18318-124">If you're using the CLI or SQLite, run the following in a command window:</span></span>

```console
dotnet new mvc --auth Individual
```

* <span data-ttu-id="18318-125">`--auth Individual`Especifica la plantilla de cuentas de usuario individuales.</span><span class="sxs-lookup"><span data-stu-id="18318-125">`--auth Individual` specifies the Individual User Accounts template.</span></span>
* <span data-ttu-id="18318-126">En Windows, agregue el `-uld` opción.</span><span class="sxs-lookup"><span data-stu-id="18318-126">On Windows, add the `-uld` option.</span></span> <span data-ttu-id="18318-127">El `-uld` opción crea una cadena de conexión de LocalDB en lugar de una base de datos de SQLite.</span><span class="sxs-lookup"><span data-stu-id="18318-127">The `-uld` option creates a LocalDB connection string rather than a SQLite DB.</span></span>
* <span data-ttu-id="18318-128">Ejecute `new mvc --help` para obtener ayuda sobre este comando.</span><span class="sxs-lookup"><span data-stu-id="18318-128">Run `new mvc --help` to get help on this command.</span></span>

## <a name="test-new-user-registration"></a><span data-ttu-id="18318-129">Probar el nuevo registro de usuario</span><span class="sxs-lookup"><span data-stu-id="18318-129">Test new user registration</span></span>

<span data-ttu-id="18318-130">Ejecutar la aplicación, seleccione la **registrar** vincular y registrar un usuario.</span><span class="sxs-lookup"><span data-stu-id="18318-130">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="18318-131">Siga las instrucciones para ejecutar migraciones de Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="18318-131">Follow the instructions to run Entity Framework Core migrations.</span></span> <span data-ttu-id="18318-132">En este punto, es la única validación en el correo electrónico con el [[EmailAddress]](http://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) atributo.</span><span class="sxs-lookup"><span data-stu-id="18318-132">At this  point, the only validation on the email is with the [[EmailAddress]](http://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) attribute.</span></span> <span data-ttu-id="18318-133">Una vez enviado el registro, se registran en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="18318-133">After you submit the registration, you are logged into the app.</span></span> <span data-ttu-id="18318-134">Más adelante en el tutorial, esto cambiaremos para que nuevos usuarios no pueden iniciar sesión hasta que se ha validado el correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="18318-134">Later in the tutorial, we'll change this so new users cannot log in until their email has been validated.</span></span>

## <a name="view-the-identity-database"></a><span data-ttu-id="18318-135">Vista de la base de datos de identidad</span><span class="sxs-lookup"><span data-stu-id="18318-135">View the Identity database</span></span>

# <a name="sql-servertabsql-server"></a>[<span data-ttu-id="18318-136">SQL Server</span><span class="sxs-lookup"><span data-stu-id="18318-136">SQL Server</span></span>](#tab/sql-server)

* <span data-ttu-id="18318-137">Desde el **vista** menú, seleccione **Explorador de objetos de SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="18318-137">From the **View** menu, select **SQL Server Object Explorer** (SSOX).</span></span> 
* <span data-ttu-id="18318-138">Vaya a **(localdb) MSSQLLocalDB (SQL Server 13)**.</span><span class="sxs-lookup"><span data-stu-id="18318-138">Navigate to **(localdb)MSSQLLocalDB(SQL Server 13)**.</span></span> <span data-ttu-id="18318-139">Haga doble clic en **dbo. AspNetUsers** > **ver datos**:</span><span class="sxs-lookup"><span data-stu-id="18318-139">Right-click on **dbo.AspNetUsers** > **View Data**:</span></span>

![Menú contextual en la tabla de AspNetUsers en el Explorador de objetos de SQL Server](accconfirm/_static/ssox.png)

<span data-ttu-id="18318-141">Tenga en cuenta el `EmailConfirmed` campo es `False`.</span><span class="sxs-lookup"><span data-stu-id="18318-141">Note the `EmailConfirmed` field is `False`.</span></span>

<span data-ttu-id="18318-142">Puede volver a usar este correo electrónico en el paso siguiente cuando la aplicación envía un correo electrónico de confirmación.</span><span class="sxs-lookup"><span data-stu-id="18318-142">You might want to use this email again in the next step when the app sends a confirmation email.</span></span> <span data-ttu-id="18318-143">Haga doble clic en la fila y seleccione **eliminar**.</span><span class="sxs-lookup"><span data-stu-id="18318-143">Right-click on the row and select **Delete**.</span></span> <span data-ttu-id="18318-144">Eliminar el correo electrónico alias ahora le resultará más fácil en los pasos siguientes.</span><span class="sxs-lookup"><span data-stu-id="18318-144">Deleting the email alias now will make it easier in the following steps.</span></span>

# <a name="sqlitetabsqlite"></a>[<span data-ttu-id="18318-145">SQLite</span><span class="sxs-lookup"><span data-stu-id="18318-145">SQLite</span></span>](#tab/sqlite)

<span data-ttu-id="18318-146">Vea [trabajar con código en un proyecto de MVC de ASP.NET Core](xref:tutorials/first-mvc-app-xplat/working-with-sql) para obtener instrucciones sobre cómo ver la base de datos de SQLite.</span><span class="sxs-lookup"><span data-stu-id="18318-146">See [Working with SQLite in an ASP.NET Core MVC project](xref:tutorials/first-mvc-app-xplat/working-with-sql) for instructions on how to view the SQLite DB.</span></span> 

---

## <a name="require-ssl-and-setup-iis-express-for-ssl"></a><span data-ttu-id="18318-147">Requerir SSL y la instalación de IIS Express para SSL</span><span class="sxs-lookup"><span data-stu-id="18318-147">Require SSL and setup IIS Express for SSL</span></span>

<span data-ttu-id="18318-148">Vea [exigir SSL](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="18318-148">See [Enforcing SSL](xref:security/enforcing-ssl).</span></span>

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a><span data-ttu-id="18318-149">Requerir confirmación por correo electrónico</span><span class="sxs-lookup"><span data-stu-id="18318-149">Require email confirmation</span></span>

<span data-ttu-id="18318-150">Es una práctica recomendada para confirmar el correo electrónico de un nuevo registro de usuario para comprobar que no utiliza la suplantación otra persona (es decir, no ha registrado con el correo electrónico de otra persona).</span><span class="sxs-lookup"><span data-stu-id="18318-150">It's a best practice to confirm the email of a new user registration to verify they are not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="18318-151">Imagine que tuviera un foro de discusión, y desea evitar "yli@example.com"al registrar como"nolivetto@contoso.com."</span><span class="sxs-lookup"><span data-stu-id="18318-151">Suppose you had a discussion forum, and you wanted to prevent "yli@example.com" from registering as "nolivetto@contoso.com."</span></span> <span data-ttu-id="18318-152">Sin confirmación por correo electrónico, "nolivetto@contoso.com" pudo obtener el correo electrónico no deseado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="18318-152">Without email confirmation, "nolivetto@contoso.com" could get unwanted email from your app.</span></span> <span data-ttu-id="18318-153">Suponga que el usuario registrado accidentalmente como "ylo@example.com" y no se vio el error ortográfico de "yli", no podrán usar la recuperación de contraseñas porque la aplicación no tiene su correo electrónico correcto.</span><span class="sxs-lookup"><span data-stu-id="18318-153">Suppose the user accidentally registered as "ylo@example.com" and hadn't noticed the misspelling of "yli," they wouldn't be able to use password recovery because the app doesn't have their correct email.</span></span> <span data-ttu-id="18318-154">Confirmación por correo electrónico proporciona sólo una protección limitada de robots y no proporciona protección de correo basura determinado que tienen muchos alias de correo electrónico de trabajo que puede usar para registrar.</span><span class="sxs-lookup"><span data-stu-id="18318-154">Email confirmation provides only limited protection from bots and doesn't provide protection from determined spammers who have many working email aliases they can use to register.</span></span>

<span data-ttu-id="18318-155">En general conveniente evitar que los nuevos usuarios se registren todos los datos del sitio web antes de que tengan un correo electrónico confirmado.</span><span class="sxs-lookup"><span data-stu-id="18318-155">You generally want to prevent new users from posting any data to your web site before they have a confirmed email.</span></span> 

<span data-ttu-id="18318-156">Actualización `ConfigureServices` para requerir un correo electrónico confirmadas:</span><span class="sxs-lookup"><span data-stu-id="18318-156">Update `ConfigureServices` to require a confirmed email:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="18318-157">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="18318-157">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="18318-158">[!code-csharp[Main](accconfirm/sample/WebPW/Startup.cs?name=snippet1&highlight=6-9)]</span><span class="sxs-lookup"><span data-stu-id="18318-158">[!code-csharp[Main](accconfirm/sample/WebPW/Startup.cs?name=snippet1&highlight=6-9)]</span></span>


# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="18318-159">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="18318-159">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="18318-160">[!code-csharp[Main](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=13-16)]</span><span class="sxs-lookup"><span data-stu-id="18318-160">[!code-csharp[Main](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=13-16)]</span></span>

---

 
```csharp
config.SignIn.RequireConfirmedEmail = true;
```
<span data-ttu-id="18318-161">La línea anterior impide que los usuarios registrados que se registran en hasta que se ha confirmado el correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="18318-161">The preceding line prevents registered users from being logged in until their email is confirmed.</span></span> <span data-ttu-id="18318-162">Sin embargo, esa línea impedir que los usuarios nuevos que se registran en después de que se registren.</span><span class="sxs-lookup"><span data-stu-id="18318-162">However, that line does not prevent new users from being logged in after they register.</span></span> <span data-ttu-id="18318-163">El código predeterminado se registra en un usuario después de que se registren.</span><span class="sxs-lookup"><span data-stu-id="18318-163">The default code logs in a user after they register.</span></span> <span data-ttu-id="18318-164">Una vez que cierra la sesión, no podrá volver a iniciar sesión hasta que se registran.</span><span class="sxs-lookup"><span data-stu-id="18318-164">Once they log out, they won't be able to log in again until they register.</span></span> <span data-ttu-id="18318-165">Más adelante en el tutorial, cambiaremos el usuario de código por lo que recién registrado son **no** iniciado sesión.</span><span class="sxs-lookup"><span data-stu-id="18318-165">Later in the tutorial we'll change the code so newly registered user are **not** logged in.</span></span>

### <a name="configure-email-provider"></a><span data-ttu-id="18318-166">Configurar el proveedor de correo electrónico</span><span class="sxs-lookup"><span data-stu-id="18318-166">Configure email provider</span></span>

<span data-ttu-id="18318-167">En este tutorial, se usa SendGrid para enviar correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="18318-167">In this tutorial, SendGrid is used to send email.</span></span> <span data-ttu-id="18318-168">Necesita una cuenta de SendGrid y la clave para enviar correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="18318-168">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="18318-169">Puede usar otros proveedores de correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="18318-169">You can use other email providers.</span></span> <span data-ttu-id="18318-170">ASP.NET Core 2.x incluye `System.Net.Mail`, que le permite enviar correo electrónico desde la aplicación.</span><span class="sxs-lookup"><span data-stu-id="18318-170">ASP.NET Core 2.x includes `System.Net.Mail`, which allows you to send email from your app.</span></span> <span data-ttu-id="18318-171">Se recomienda que usar SendGrid u otro servicio de correo electrónico para enviar correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="18318-171">We recommend you use SendGrid or another email service to send email.</span></span>

<span data-ttu-id="18318-172">El [patrón opciones](xref:fundamentals/configuration#options-config-objects) se usa para acceder a la configuración de cuenta y clave de usuario.</span><span class="sxs-lookup"><span data-stu-id="18318-172">The [Options pattern](xref:fundamentals/configuration#options-config-objects) is used to access the user account and key settings.</span></span> <span data-ttu-id="18318-173">Para obtener más información, consulte [configuración](xref:fundamentals/configuration#fundamentals-configuration).</span><span class="sxs-lookup"><span data-stu-id="18318-173">For more information, see [configuration](xref:fundamentals/configuration#fundamentals-configuration).</span></span>

<span data-ttu-id="18318-174">Cree una clase para obtener la clave de proteger el correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="18318-174">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="18318-175">En este ejemplo, el `AuthMessageSenderOptions` se crea una clase en el *Services/AuthMessageSenderOptions.cs* archivo.</span><span class="sxs-lookup"><span data-stu-id="18318-175">For this sample, the `AuthMessageSenderOptions` class is created in the *Services/AuthMessageSenderOptions.cs* file.</span></span>

<span data-ttu-id="18318-176">[!code-csharp[Main](accconfirm/sample/WebApp1/Services/AuthMessageSenderOptions.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="18318-176">[!code-csharp[Main](accconfirm/sample/WebApp1/Services/AuthMessageSenderOptions.cs?name=snippet1)]</span></span>

<span data-ttu-id="18318-177">Establecer el `SendGridUser` y `SendGridKey` con el [herramienta Administrador de secreto](../app-secrets.md).</span><span class="sxs-lookup"><span data-stu-id="18318-177">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](../app-secrets.md).</span></span> <span data-ttu-id="18318-178">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="18318-178">For example:</span></span>

```none
C:\WebAppl\src\WebApp1>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="18318-179">En Windows, el Administrador de secreto almacena los pares de claves/valor en un *secrets.json* archivo en el directorio %APPDATA%/Microsoft/UserSecrets/ < WebAppName userSecretsId >.</span><span class="sxs-lookup"><span data-stu-id="18318-179">On Windows, Secret Manager stores your keys/value pairs in a *secrets.json* file in the %APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId> directory.</span></span>

<span data-ttu-id="18318-180">El contenido de la *secrets.json* archivos no están cifrados.</span><span class="sxs-lookup"><span data-stu-id="18318-180">The contents of the *secrets.json* file are not encrypted.</span></span> <span data-ttu-id="18318-181">El *secrets.json* archivo se muestra a continuación (el `SendGridKey` se ha quitado el valor.)</span><span class="sxs-lookup"><span data-stu-id="18318-181">The *secrets.json* file is shown below (the `SendGridKey` value has been removed.)</span></span>

  ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="configure-startup-to-use-authmessagesenderoptions"></a><span data-ttu-id="18318-182">Configurar el inicio para usar AuthMessageSenderOptions</span><span class="sxs-lookup"><span data-stu-id="18318-182">Configure startup to use AuthMessageSenderOptions</span></span>

<span data-ttu-id="18318-183">Agregar `AuthMessageSenderOptions` al contenedor de servicios al final de la `ConfigureServices` método en el *Startup.cs* archivo:</span><span class="sxs-lookup"><span data-stu-id="18318-183">Add `AuthMessageSenderOptions` to the service container at the end of the `ConfigureServices` method in the *Startup.cs* file:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="18318-184">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="18318-184">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="18318-185">[!code-csharp[Main](accconfirm/sample/WebPW/Startup.cs?name=snippet1&highlight=18)]</span><span class="sxs-lookup"><span data-stu-id="18318-185">[!code-csharp[Main](accconfirm/sample/WebPW/Startup.cs?name=snippet1&highlight=18)]</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="18318-186">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="18318-186">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
<span data-ttu-id="18318-187">[!code-csharp[Main](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=26)]</span><span class="sxs-lookup"><span data-stu-id="18318-187">[!code-csharp[Main](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=26)]</span></span>

---

### <a name="configure-the-authmessagesender-class"></a><span data-ttu-id="18318-188">Configurar la clase de AuthMessageSender</span><span class="sxs-lookup"><span data-stu-id="18318-188">Configure the AuthMessageSender class</span></span>

<span data-ttu-id="18318-189">Este tutorial muestra cómo agregar notificaciones de correo electrónico a través de [SendGrid](https://sendgrid.com/), pero puede enviar correo electrónico mediante SMTP y otros mecanismos.</span><span class="sxs-lookup"><span data-stu-id="18318-189">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

* <span data-ttu-id="18318-190">Instalar el `SendGrid` paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="18318-190">Install the `SendGrid` NuGet package.</span></span> <span data-ttu-id="18318-191">Introduzca la siguiente información desde la consola de administrador de paquetes, el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="18318-191">From the Package Manager Console,  enter the following the following command:</span></span>

  `Install-Package SendGrid`

* <span data-ttu-id="18318-192">Vea [empiece de forma gratuita con SendGrid](https://sendgrid.com/free/) para registrar una cuenta gratuita de SendGrid.</span><span class="sxs-lookup"><span data-stu-id="18318-192">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>

#### <a name="configure-sendgrid"></a><span data-ttu-id="18318-193">Configurar SendGrid</span><span class="sxs-lookup"><span data-stu-id="18318-193">Configure SendGrid</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="18318-194">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="18318-194">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

* <span data-ttu-id="18318-195">Agregue código en *Services/EmailSender.cs* similar al siguiente para configurar SendGrid:</span><span class="sxs-lookup"><span data-stu-id="18318-195">Add code in *Services/EmailSender.cs* similar to the following to configure SendGrid:</span></span>

<span data-ttu-id="18318-196">[!code-csharp[Main](accconfirm/sample/WebPW/Services/EmailSender.cs)]</span><span class="sxs-lookup"><span data-stu-id="18318-196">[!code-csharp[Main](accconfirm/sample/WebPW/Services/EmailSender.cs)]</span></span>


# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="18318-197">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="18318-197">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
* <span data-ttu-id="18318-198">Agregue código en *Services/MessageServices.cs* similar al siguiente para configurar SendGrid:</span><span class="sxs-lookup"><span data-stu-id="18318-198">Add code in *Services/MessageServices.cs* similar to the following to configure SendGrid:</span></span>

<span data-ttu-id="18318-199">[!code-csharp[Main](accconfirm/sample/WebApp1/Services/MessageServices.cs)]</span><span class="sxs-lookup"><span data-stu-id="18318-199">[!code-csharp[Main](accconfirm/sample/WebApp1/Services/MessageServices.cs)]</span></span>

---

## <a name="enable-account-confirmation-and-password-recovery"></a><span data-ttu-id="18318-200">Habilitar la recuperación de confirmación y la contraseña de cuenta</span><span class="sxs-lookup"><span data-stu-id="18318-200">Enable account confirmation and password recovery</span></span>

<span data-ttu-id="18318-201">La plantilla tiene el código para la recuperación de confirmación y la contraseña de cuenta.</span><span class="sxs-lookup"><span data-stu-id="18318-201">The template has the code for account confirmation and password recovery.</span></span> <span data-ttu-id="18318-202">Buscar el `[HttpPost] Register` método en el *AccountController.cs* archivo.</span><span class="sxs-lookup"><span data-stu-id="18318-202">Find the `[HttpPost] Register` method in the  *AccountController.cs* file.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="18318-203">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="18318-203">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="18318-204">Impedir que los usuarios recién registrados que se iniciará automáticamente la sesión como comentario la línea siguiente:</span><span class="sxs-lookup"><span data-stu-id="18318-204">Prevent newly registered users from being automatically logged on by commenting out the following line:</span></span>

```csharp 
await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="18318-205">El método completo se muestra con la línea cambiada resaltada:</span><span class="sxs-lookup"><span data-stu-id="18318-205">The complete method is shown with the changed line highlighted:</span></span>

<span data-ttu-id="18318-206">[!code-csharp[Main](accconfirm/sample/WebPW/Controllers/AccountController.cs?highlight=19&name=snippet_Register)]</span><span class="sxs-lookup"><span data-stu-id="18318-206">[!code-csharp[Main](accconfirm/sample/WebPW/Controllers/AccountController.cs?highlight=19&name=snippet_Register)]</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="18318-207">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="18318-207">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="18318-208">Quite los comentarios del código para habilitar la confirmación de la cuenta.</span><span class="sxs-lookup"><span data-stu-id="18318-208">Uncomment the code to enable account confirmation.</span></span>

<span data-ttu-id="18318-209">[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=16-25&name=snippet_Register)]</span><span class="sxs-lookup"><span data-stu-id="18318-209">[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=16-25&name=snippet_Register)]</span></span>

<span data-ttu-id="18318-210">Nota: Se estamos también impide que un usuario recién registrados que se iniciará automáticamente la sesión como comentario la línea siguiente:</span><span class="sxs-lookup"><span data-stu-id="18318-210">Note: We're also preventing a newly-registered user from being automatically logged on by commenting out the following line:</span></span>

```csharp 
//await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="18318-211">Habilitar la recuperación de contraseña por el comentario de código en el `ForgotPassword` acción en el *Controllers/AccountController.cs* archivo.</span><span class="sxs-lookup"><span data-stu-id="18318-211">Enable password recovery by uncommenting the code in the `ForgotPassword` action in the *Controllers/AccountController.cs* file.</span></span>

<span data-ttu-id="18318-212">[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=17-23&name=snippet_ForgotPassword)]</span><span class="sxs-lookup"><span data-stu-id="18318-212">[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=17-23&name=snippet_ForgotPassword)]</span></span>

<span data-ttu-id="18318-213">Quite el elemento de formulario en *Views/Account/ForgotPassword.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="18318-213">Uncomment the form element in *Views/Account/ForgotPassword.cshtml*.</span></span> <span data-ttu-id="18318-214">Desea quitar el `<p> For more information on how to enable reset password ... </p>` elemento que contiene un vínculo a este artículo.</span><span class="sxs-lookup"><span data-stu-id="18318-214">You might want to remove the `<p> For more information on how to enable reset password ... </p>` element which contains a link to this article.</span></span>

<span data-ttu-id="18318-215">[!code-html[Main](accconfirm/sample/WebApp1/Views/Account/ForgotPassword.cshtml?highlight=7-10,12,28)]</span><span class="sxs-lookup"><span data-stu-id="18318-215">[!code-html[Main](accconfirm/sample/WebApp1/Views/Account/ForgotPassword.cshtml?highlight=7-10,12,28)]</span></span>

---

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="18318-216">Registrar, confirma el correo electrónico y restablecer contraseña</span><span class="sxs-lookup"><span data-stu-id="18318-216">Register, confirm email, and reset password</span></span>

<span data-ttu-id="18318-217">Ejecutar la aplicación web y probar la confirmación de la cuenta y el flujo de recuperación de contraseña.</span><span class="sxs-lookup"><span data-stu-id="18318-217">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="18318-218">Ejecutar la aplicación y registrar un nuevo usuario</span><span class="sxs-lookup"><span data-stu-id="18318-218">Run the app and register a new user</span></span>

 ![Vista de cuenta registrar la aplicación Web](accconfirm/_static/loginaccconfirm1.png)

* <span data-ttu-id="18318-220">Compruebe su correo electrónico para el vínculo de confirmación de cuenta.</span><span class="sxs-lookup"><span data-stu-id="18318-220">Check your email for the account confirmation link.</span></span> <span data-ttu-id="18318-221">Vea [depurar correo electrónico](#debug) si no recibe el correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="18318-221">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="18318-222">Haga clic en el vínculo para confirmar tu correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="18318-222">Click the link to confirm your email.</span></span>
* <span data-ttu-id="18318-223">Inicie sesión con su correo electrónico y contraseña.</span><span class="sxs-lookup"><span data-stu-id="18318-223">Log in with your email and password.</span></span>
* <span data-ttu-id="18318-224">Cierre la sesión.</span><span class="sxs-lookup"><span data-stu-id="18318-224">Log off.</span></span>

### <a name="view-the-manage-page"></a><span data-ttu-id="18318-225">Ver la página de administración</span><span class="sxs-lookup"><span data-stu-id="18318-225">View the manage page</span></span>

<span data-ttu-id="18318-226">Seleccione el nombre de usuario en el explorador: ![ventana del explorador con el nombre de usuario](accconfirm/_static/un.png)</span><span class="sxs-lookup"><span data-stu-id="18318-226">Select your user name in the browser: ![browser window with user name](accconfirm/_static/un.png)</span></span>

<span data-ttu-id="18318-227">Debe expandir la barra de navegación para ver el nombre de usuario.</span><span class="sxs-lookup"><span data-stu-id="18318-227">You might need to expand the navbar to see user name.</span></span>

![barra de navegación](accconfirm/_static/x.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="18318-229">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="18318-229">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="18318-230">Se muestra la página de administración con el **perfil** pestaña seleccionada.</span><span class="sxs-lookup"><span data-stu-id="18318-230">The manage page is displayed with the **Profile** tab selected.</span></span> <span data-ttu-id="18318-231">El **correo electrónico** muestra una casilla de verificación que indica el correo electrónico se ha confirmado.</span><span class="sxs-lookup"><span data-stu-id="18318-231">The **Email** shows a check box indicating the email has been confirmed.</span></span> 

![página de administración](accconfirm/_static/rick2.png)


# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="18318-233">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="18318-233">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="18318-234">Hablaremos acerca de esta página más adelante en el tutorial.</span><span class="sxs-lookup"><span data-stu-id="18318-234">We'll talk about this page later in the tutorial.</span></span>
<span data-ttu-id="18318-235">![página de administración](accconfirm/_static/rick2.png)</span><span class="sxs-lookup"><span data-stu-id="18318-235">![manage page](accconfirm/_static/rick2.png)</span></span>

---

### <a name="test-password-reset"></a><span data-ttu-id="18318-236">Restablecimiento de contraseña de prueba</span><span class="sxs-lookup"><span data-stu-id="18318-236">Test password reset</span></span>

* <span data-ttu-id="18318-237">Si ha iniciado sesión, seleccione **Logout**.</span><span class="sxs-lookup"><span data-stu-id="18318-237">If you're logged in, select **Logout**.</span></span>  
* <span data-ttu-id="18318-238">Seleccione el **sesión** de vínculo y seleccione el **¿olvidó su contraseña?** vínculo.</span><span class="sxs-lookup"><span data-stu-id="18318-238">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="18318-239">Escriba el correo electrónico que usa para registrar la cuenta.</span><span class="sxs-lookup"><span data-stu-id="18318-239">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="18318-240">Se enviará un correo electrónico con un vínculo para restablecer su contraseña.</span><span class="sxs-lookup"><span data-stu-id="18318-240">An email with a link to reset your password will be sent.</span></span> <span data-ttu-id="18318-241">Compruebe su correo electrónico y haga clic en el vínculo para restablecer la contraseña.</span><span class="sxs-lookup"><span data-stu-id="18318-241">Check your email and click the link to reset your password.</span></span>  <span data-ttu-id="18318-242">Después de que la contraseña se restableció correctamente, puede iniciar sesión con su correo electrónico y una contraseña nueva.</span><span class="sxs-lookup"><span data-stu-id="18318-242">After your password has been successfully reset, you can login with your email and new password.</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="18318-243">Depurar el correo electrónico</span><span class="sxs-lookup"><span data-stu-id="18318-243">Debug email</span></span>

<span data-ttu-id="18318-244">Si no se puede obtener el trabajo de correo electrónico:</span><span class="sxs-lookup"><span data-stu-id="18318-244">If you can't get email working:</span></span>

* <span data-ttu-id="18318-245">Revise el [actividad de correo electrónico](https://sendgrid.com/docs/User_Guide/email_activity.html) página.</span><span class="sxs-lookup"><span data-stu-id="18318-245">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="18318-246">Compruebe la carpeta de correo basura.</span><span class="sxs-lookup"><span data-stu-id="18318-246">Check your spam folder.</span></span>
* <span data-ttu-id="18318-247">Pruebe otro alias de correo electrónico en un proveedor de correo electrónico diferente (Microsoft, Yahoo, Gmail, etcetera.)</span><span class="sxs-lookup"><span data-stu-id="18318-247">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="18318-248">Crear un [aplicación de consola para enviar correo electrónico](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).</span><span class="sxs-lookup"><span data-stu-id="18318-248">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).</span></span>
* <span data-ttu-id="18318-249">Vuelva a enviar a las cuentas de correo electrónico diferente.</span><span class="sxs-lookup"><span data-stu-id="18318-249">Try sending to different email accounts.</span></span>

<span data-ttu-id="18318-250">**Nota:** un procedimiento recomendado es no utilizar los secretos de producción de prueba y desarrollo.</span><span class="sxs-lookup"><span data-stu-id="18318-250">**Note:** A security best practice is to not use production secrets in test and development.</span></span> <span data-ttu-id="18318-251">Si publica la aplicación en Azure, puede establecer los secretos de SendGrid como configuración de la aplicación en el portal de la aplicación Web de Azure.</span><span class="sxs-lookup"><span data-stu-id="18318-251">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="18318-252">El sistema de configuración es el programa de instalación para leer las claves de las variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="18318-252">The configuration system is setup to read keys from environment variables.</span></span>

## <a name="prevent-login-at-registration"></a><span data-ttu-id="18318-253">Evitar el inicio de sesión en el registro</span><span class="sxs-lookup"><span data-stu-id="18318-253">Prevent login at registration</span></span>

<span data-ttu-id="18318-254">Con las plantillas de la actuales, cuando un usuario se haya completado el formulario de registro, se registran en (autenticado).</span><span class="sxs-lookup"><span data-stu-id="18318-254">With the current templates, once a user completes the registration form, they are logged in (authenticated).</span></span> <span data-ttu-id="18318-255">Por lo general desea confirmar su correo electrónico antes de registrarlos.</span><span class="sxs-lookup"><span data-stu-id="18318-255">You generally want to confirm their email before logging them in.</span></span> <span data-ttu-id="18318-256">En la sección siguiente, se modificará el código para requerir los nuevos usuarios tienen un correo electrónico confirmado antes de registrarse.</span><span class="sxs-lookup"><span data-stu-id="18318-256">In the section below, we will modify the code to require new users have a confirmed email before they are logged in.</span></span> <span data-ttu-id="18318-257">Actualización de la `[HttpPost] Login` acción en el *AccountController.cs* archivo con los siguientes cambios resaltados.</span><span class="sxs-lookup"><span data-stu-id="18318-257">Update the `[HttpPost] Login` action in the *AccountController.cs* file with the following highlighted changes.</span></span>

<span data-ttu-id="18318-258">[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=11-21&name=snippet_Login)]</span><span class="sxs-lookup"><span data-stu-id="18318-258">[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=11-21&name=snippet_Login)]</span></span>

<span data-ttu-id="18318-259">**Nota:** un procedimiento recomendado es no utilizar los secretos de producción de prueba y desarrollo.</span><span class="sxs-lookup"><span data-stu-id="18318-259">**Note:** A security best practice is to not use production secrets in test and development.</span></span> <span data-ttu-id="18318-260">Si publica la aplicación en Azure, puede establecer los secretos de SendGrid como configuración de la aplicación en el portal de la aplicación Web de Azure.</span><span class="sxs-lookup"><span data-stu-id="18318-260">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="18318-261">El sistema de configuración es el programa de instalación para leer las claves de las variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="18318-261">The configuration system is setup to read keys from environment variables.</span></span>


## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="18318-262">Combinar las cuentas de inicio de sesión locales y redes sociales</span><span class="sxs-lookup"><span data-stu-id="18318-262">Combine social and local login accounts</span></span>

<span data-ttu-id="18318-263">Nota: Esta sección solo se aplica a ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="18318-263">Note: This section applies only to ASP.NET Core 1.x.</span></span> <span data-ttu-id="18318-264">Para ASP.NET Core 2.x, consulte [esto](https://github.com/aspnet/Docs/issues/3753) problema.</span><span class="sxs-lookup"><span data-stu-id="18318-264">For ASP.NET Core 2.x, see [this](https://github.com/aspnet/Docs/issues/3753) issue.</span></span>

<span data-ttu-id="18318-265">Para completar esta sección, primero debe habilitar a un proveedor de autenticación externo.</span><span class="sxs-lookup"><span data-stu-id="18318-265">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="18318-266">Vea [habilitar la autenticación con Facebook, Google y otros proveedores externos](social/index.md).</span><span class="sxs-lookup"><span data-stu-id="18318-266">See [Enabling authentication using Facebook, Google and other external providers](social/index.md).</span></span>

<span data-ttu-id="18318-267">Puede combinar cuentas locales y redes sociales, haga clic en el vínculo de correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="18318-267">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="18318-268">En la siguiente secuencia, "RickAndMSFT@gmail.com" en primer lugar se crea como un inicio de sesión local; sin embargo, puede crear la cuenta como un inicio de sesión social primero y luego agregar un inicio de sesión local.</span><span class="sxs-lookup"><span data-stu-id="18318-268">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Aplicación Web: RickAndMSFT@gmail.com usuario autenticado](accconfirm/_static/rick.png)

<span data-ttu-id="18318-270">Haga clic en el **administrar** vínculo.</span><span class="sxs-lookup"><span data-stu-id="18318-270">Click on the **Manage** link.</span></span> <span data-ttu-id="18318-271">Tenga en cuenta el externo 0 (inicios de sesión sociales) asociados con esta cuenta.</span><span class="sxs-lookup"><span data-stu-id="18318-271">Note the 0 external (social logins) associated with this account.</span></span>

![Administrar la vista](accconfirm/_static/manage.png)

<span data-ttu-id="18318-273">Haga clic en el vínculo a otro servicio de inicio de sesión y Aceptar las solicitudes de aplicación.</span><span class="sxs-lookup"><span data-stu-id="18318-273">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="18318-274">En la imagen siguiente, Facebook es el proveedor de autenticación externos:</span><span class="sxs-lookup"><span data-stu-id="18318-274">In the image below, Facebook is the external authentication provider:</span></span>

![Administrar la vista de inicios de sesión externos enumerar Facebook](accconfirm/_static/fb.png)

<span data-ttu-id="18318-276">Se han combinado las dos cuentas.</span><span class="sxs-lookup"><span data-stu-id="18318-276">The two accounts have been combined.</span></span> <span data-ttu-id="18318-277">Podrá iniciar sesión con cualquiera de estas cuentas.</span><span class="sxs-lookup"><span data-stu-id="18318-277">You will be able to log on with either account.</span></span> <span data-ttu-id="18318-278">Puede que los usuarios agreguen cuentas locales en caso de que su registro sociales en servicio de autenticación está inactivo o, más probable es que ha perdido el acceso a su cuenta sociales.</span><span class="sxs-lookup"><span data-stu-id="18318-278">You might want your users to add local accounts in case their social log in authentication service is down, or more likely they have lost access to their social account.</span></span>
